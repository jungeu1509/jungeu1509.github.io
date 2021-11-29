---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 9-5강"
excerpt: "값 타입 - 값 타입 컬렉션"
header:
  teaser: 
search: False
permalink:
categories: 
  - BackEnd
tags:
  - java
  - backend
  - study
date:  2021-11-24 23:40:00 +0900
last_modified_at: 2021-11-24 23:40:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. DB에서 컬렉션

DB에서는 대부분 컬렉션을 지원하지 않기때문에 별도의 테이블로 분리하여 결국 1대다 관계로 형성한다.

값 타입을 하나 이상 저장할 때 사용한다.

- @ElementCollection, @CollectionTable 사용
- 데이터베이스는 컬렉션을 같은 테이블에 저장할 수 없다.
- 컬렉션을 저장하기 위한 별도의 테이블이 필요함

## 값 타입 저장 및 조회 예제

```java
@Entity
@Table(name="member")
public class Member {

    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @Column(name = "USERNAME")
    private String name;

    @Embedded
    private Address homeAddress;

    @ElementCollection
    @CollectionTable(name = "FAVORITE_FOOD", joinColumns =
        @JoinColumn(name = "MEMBER_ID") // FK를 위해 작성
    )
    @Column(name = "FOOD_NAME") // String 값 하나만 있고 직접 설정한게 아니기 때문에 예외적으로 이름변경 가능
    private Set<String> favoriteFoods = new HashSet<>();

    @ElementCollection
    @CollectionTable(name = "ADDRESS", joinColumns =
        @JoinColumn(name = "MEMBER_ID")
    ) // 속성명을 바꾸고 싶으면 AttributeOverride하면된다.
    private List<Address> addressHistory = new ArrayList<>();

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "TEAM_ID")
    private Team team;

    public void setTeam(Team team) {
        this.team = team;
        team.getMembers().add(this);
    }
}
```
```java
@Embeddable
public class Address {
    private String city;
    private String street;
    private String zipcode;

    public Address() {}

    public Address(String city, String street, String zipcode) {
        this.city = city;
        this.street = street;
        this.zipcode = zipcode;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        Address address = (Address) o;
        return Objects.equals(city, address.city) && Objects.equals(street,
            address.street) && Objects.equals(zipcode, address.zipcode);
    }

    @Override
    public int hashCode() {
        return Objects.hash(city, street, zipcode);
    }
}
```

위처럼 코드를 작성하고 아래 메인을 실행하면 잘 설정되어 db에 저장된 것을 알 수 있다.

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {

            Team team = new Team();
            team.setName("TeamA");
            em.persist(team);

            Member member = new Member();
            member.setName("member1");
            member.setTeam(team);
            em.persist(member);

            member.setHomeAddress(new Address("homeCity", "street", "10000"));
            
            member.getFavoriteFoods().add("chicken");
            member.getFavoriteFoods().add("pizza");
            member.getFavoriteFoods().add("hamburger");
            
            member.getAddressHistory().add(new Address("old1", "street","10000"));
            member.getAddressHistory().add(new Address("old2", "street","10000"));
            
            em.persist(member);

            transaction.commit();
        } catch (Exception e) {
            transaction.rollback();
            System.out.println("e = "+e);
        } finally {
            em.close();
        }
        emf.close();
    }
```

**collection도 다른 테이블인데도 불구하고 생명주기가 동일한 것을 알 수 있다.**

값 타입 컬렉션은 영속성 전에(Cascade) + 고아객체 제거기능을 필수로 가진다고 볼 수 있다.

컬렉션들은 지연로딩이다.

## 값 타입 수정

값 타입을 수정할때는 값 타입이 불변이므로 새로 넣어야한다.

```java
Address a = findMember.getHomeAddress();
findMember.setHomeAddress(new Address("newCity", a.getStreet(), a.getZipcode()));
```

컬렉션에 있는것을 수정할때는 아래와 같다.

```java
Member findmember = em.find(Member.class, member.getId());
            
findmember.getFavoriteFoods().remove("pizza");
findmember.getFavoriteFoods().add("pasta");
```

위와 같이 값을 찾아서 지운 후 추가를 해야한다.

컬렉션의 데이터는 아래와 같이 수정을 하게된다.

```java
Member findmember = em.find(Member.class, member.getId());

// hashCode() 와 equals()가 제대로 재정의되어야 의도대로 삭제한다.
findmember.getAddressHistory().remove(new Address("old1", "street","10000"));
findmember.getAddressHistory().add(new Address("new1", "street","10000"));
```

그런데 log를 통해 sql문을 보면 모든 데이터를 지우고 다시 다 생성하는 과정을 거치게 된다.

# 2. 값 타입 컬렉션의 제약사항

- 값 타입은 엔티티와 다르게 식별자 개념이 없다.
- 값은 변경하면 추적이 어렵다.
- 값 타입 컬렉션에 변경 사항이 발생하면, 주인 엔티티와 **연관된 모든 데이터를 삭제**하고, 값 타입 컬렉션에 있는 **현재 값을 모두 다시 저장**한다.
- 값 타입 컬렉션을 매핑하는 테이블은 모든 컬럼을 묶어서 기본 키를 구성해야 함: null 입력X, 중복 저장X

모든 데이터를 삭제하고 현재값을 다시 저장하므로 추적이 어렵다.

# 3. 값 타입 컬렉션 대안

실무에서는 상황에 따라 값 타입 컬렉션 대신 일대다 관계를 고려하는 것이 낫다.

일대다 관계를 위해 엔티티를 만들고, 여기에서 값 타입을 사용한다.

영속성 전이(Cascade) + 고아 객체 제거를 사용해서 값 타입 컬렉션 처럼 사용한다.


```java
@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
@JoinColumn(name = "memberId")
private List<AddressEntity> addresshistory = new ArrayList<>();
```

이처럼 하면 각 테이블에 ID가 생기므로 각각 수정할 수 있게된다.

실무에서 써야한다면 진짜 단순한거 할때만 사용하는것을 추천한다.

값을 변경하지 않을 것 같다 하더라도 대부분 엔티티로 만드는 것을 추천한다.

# 4. 정리

- 엔티티 타입의 특징
  - 식별자O
  - 생명주기 관리
  - 공유
- 값타입의특징
  - 식별자X
  - 생명 주기를 엔티티에 의존
  - 공유하지 않는 것이 안전(복사해서 사용)
  - 불변객체로 만드는 것이 안전
