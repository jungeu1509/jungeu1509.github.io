---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 8-3강"
excerpt: "프록시와 연관관계 관리 - 영속성 전이(CASCADE)와 고아 객체"
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
date:  2021-11-18 01:40:00 +0900
last_modified_at: 2021-11-17 01:40:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 영속성 전이 CASCADE

특정 엔티티를 영속 상태로 만들 때 연관된 엔티티도 함께 영속상태로 만들고 싶을 때 사용한다.

예를 들어 부모 엔티티를 저장할 때 자식 엔티티도 함께 저장하고 싶을 때 사용

## 부모 엔티티 예시

```java
@Entity
public class Parent {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "parent")
    private List<Child> childList = new ArrayList<>();

    public void addChild(Child child) {
        childList.add(child);
        child.setParent(this);
    }

    //getter setter
}

```

```java
@Entity
public class Child {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "parent_id")
    private Parent parent;

    //getter setter
}
```

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {

            Child child1 = new Child();
            Child child2 = new Child();

            Parent parent = new Parent();
            parent.addChild(child1);
            parent.addChild(child2);


            em.persist(parent);
            em.persist(child1);
            em.persist(child2);

            transaction.commit();
        } catch (Exception e) {
            transaction.rollback();
            System.out.println("e = "+e);
        } finally {
            em.close();
        }
        emf.close();
    }
}
```

위 처럼 하면 persist를 3번 호출해야한다.

Parent를 넣으면 알아서 관리해줬으면 좋겠어... 그럴경우 아래처럼 cascade 속성을 추가하면 된다.

```java
@Entity
public class Parent {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL)
    private List<Child> childList = new ArrayList<>();

    public void addChild(Child child) {
        childList.add(child);
        child.setParent(this);
    }
}
```

위처럼 엔티티를 수정할 경우 persist를 parent만 하면 자동으로 child들도 된다.

연관관계 상관없이 하위 속성을 다 적용해준다고 생각해주면 된다.

# 2. CASCADE 주의사항

- 영속성 전이는 연관관계를 매핑하는 것과 아무 관련이 없음
- 엔티티를 영속화할 때 연관된 엔티티도 함께 영속화하는 편리함을 제공할 뿐이다.
- 일대다에 무조건 거는 것이 아니다. 부모가 하나의 자식들을 관리할때 편하다.
  - 게시판의 첨부파일경로 같은 것에서 쓰면 좋다.
  - 만약 파일을 여러경로(여러 엔티티)에서 관리한다고 하면 쓰면 안된다.
  - 즉, 완전히 종속될때는 좋지만 자식객체가 다른 엔티티랑 관계가 생기기 시작한다고 할때 쓰면 안된다.

## CASCADE 사용 조건

cascade를 사용할 때는 두개의 전제가 참이여야 한다.

1. 라이프사이클이 같을 때(거의 유사할 때)
2. 단일소유자일 때

# 3. CASCADE 종류

- ALL: 모두 적용
- PERSIST: 영속
- REMOVE: 삭제
- MERGE: 병합
- REFRESH: REFRESH
- DETACH: DETACH

# 4. 고아객체

부모 엔티티와 연관관계가 끊어졌을경우 자식이 쓸모 없을경우 자동 삭제하는 기능을 사용하고 싶다 할 때 사용한다.

orphanRemoval = true

```java
@Entity
public class Parent {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Child> childList = new ArrayList<>();

    public void addChild(Child child) {
        childList.add(child);
        child.setParent(this);
    }
}
```

테스트를 해보자

```java
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {
            Child child1 = new Child();
            Child child2 = new Child();

            Parent parent = new Parent();
            parent.addChild(child1);
            parent.addChild(child2);

            em.persist(parent);

            em.flush();
            em.clear();

            Parent findParent = em.find(Parent.class, parent.getId());
            findParent.getChildList().remove(0);

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

DB를 확인할 경우 컬렉션에서 빠지게 될 경우 데이터가 지워지게된다.

# 5. 고아객체 주의사항

- 참조가 제거된 엔티티는 다른 곳에서 참조하지 않는 고아 객체로 보고 삭제하는 기능
- 참조하는 곳이 하나일 때 사용해야함!
- **특정 엔티티가 개인소유할때만 사용**
- @OneToOne, @OneToMany만 가능
- 참고: 개념적으로 부모를 제거하면 자식은 고아가 된다. 따라서 고아객체 제거 기능을 활성화 하면, 부모를 제거할 때 자식도 함께 제거된다. 이것은 CascadeType.REMOVE처럼 동작한다.
  - 부모객체만 지우면 자식객체 모조리 다 지워진다

**특정 엔티티가 개인소유 할때만 사용하는걸 기억하자.**

# 6. 영속성 전이와 고아 객체의 생명 주기

CascadeType.ALL + orphanRemovel=true

위처럼 두개를 다 사용하면 어떻게 될까

- 스스로 생명주기를 관리하는 엔티티는 em.persist()로 영속화, em.remove()로 제거
- 두 옵션을 모두 활성화 하면 부모 엔티티를 통해서 자식의 생명 주기를 관리할 수 있음
- 도메인 주도 설계(DDD)의 Aggregate Root개념을 구현할 때 유용
  - 이런게 있구나 하고 넘어가자...


