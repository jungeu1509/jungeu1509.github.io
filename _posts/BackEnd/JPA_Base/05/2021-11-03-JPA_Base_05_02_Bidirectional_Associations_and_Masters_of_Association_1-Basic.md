---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 5-2강"
excerpt: "양방향 연관관계와 연관관계의 주인 1 - 기본"
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
date:  2021-11-03 21:00:00 +0800
last_modified_at: 2021-11-03 21:00:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 양방향 연관관계

객체는 참고를 사용하고 테이블은 외래키를 이용하므로 연관관계 주인이 중요하는데 나중에 설명할 것이다.

단방향의 경우 Member 객체에서 Team 객체로는 갈 수 있지만(getTeam() 메서드) 반대로는 불가능했다.

그래서 레퍼런스를 만들어두면 양방향으로 왔다갔다 할 수 있기 때문에 만들어 둘 것이다.

사실 테이블의 연관관계는 변하는 것이 없다.(Join 하여 사용한다.)

## 양방향 객체 연관관계

Team class 에 List를 이용하여 레퍼런스를 넣어둔다.

```java
@OnetoMany(mappedBy = "team")
private List<Member> members = new ArrayList<>();
```

new ArrayList<>()는 초기화해주는 관례이다.

여기서 member와 team은 다대일 관계이지만, team에서 봤을때는 일대다 관계이므로 OnetoMany 어노테이션을 달아준다.

그리고 mappedBy는 Member의 객체에서 매핑되는 변수의 이름(member 클래스의 team 변수)을 쓰면된다.

Main에서 확인해보자

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

            em.flush();
            em.clear();

            Member findMember = em.find(Member.class, member.getId());
            List<Member> members = findMember.getTeam().getMembers();

            for (Member m : members) {
                System.out.println("m = "+m.getName());
            }

            transaction.commit();
        } catch (Exception e) {
            transaction.rollback();
        } finally {
            em.close();
        }
        emf.close();
    }
}
```

양방향이 무조건 좋은건 아니다... 나중에 알려주긴 할 것이다.

## 연관관계의 주인과 mappedBy

**제일 어려운 부분이다.**

객체와 테이블간에 연관관계를 맺는 차이를 이해해야 한다.

객체는 연관관계를 맺는 포인트가 두부분이다.

Member -> Team / Team -> Member

이렇게 레퍼런스를 이용한 단방향인 연관관계를 억지로 두개로 붙인것이다.

테이블에서는 FK(외래키)를 이용하여 한방향을 이용하여 연관관계를 확인할 수 있다.

### 딜레마

양방향 매핑을 하면서 객체는 두개의 관계로 관리하고 테이블은 하나의 관계로 관리를 하기때문에 딜레마가 생긴다.

Member를 바꾸고 싶거나 새로운 Team으로 들어가고 싶을때

Member의 Team을 바꿔야할지 Team의 members를 바꿔야할지 혼동이 온다.

그래서 규칙이 생긴다. 둘 중 하나로 외래키를 관리해야한다.

이게 바로 연관관계 주인이다.

### 연관관계 주인


- 객체의 두 관계중 하나를 연관관계의 주인으로 지정한다.
- 주인은 mappedBy 속성 사용X
- 연관관계의 주인만이 외래 키를 관리(등록, 수정) 
- 주인이 아닌쪽은 읽기만 가능
- 주인이 아니면 mappedBy 속성으로 주인으로 지정한다.

즉, mappedBy속성이 적힌쪽은 종속된 곳(주인이 아닌 곳) 이므로 그 곳에서는 조회만 가능하다.

예시에서는 Member에서 JoinColumn이 써있으므로 그 곳에서 Team을 관리해야한다.

**왜래키가 있는 곳에서 주인으로 하자!**

일대다에서 일인 부분을 mappedby를 부여하고 다 부분을 주인인 부분으로 매핑한다.

테이블에서 FK가 있는 부분이 다인 부분이므로 Member가 주인인 것이 관리가 편하다.

Team객체를 관리했는데 쿼리가 Member쿼리가 나가면 관리하는 입장에서 혼동이 오기 쉽기 때문이다.(성능이슈도 있을 수 있다. 뒤에 나옴)


