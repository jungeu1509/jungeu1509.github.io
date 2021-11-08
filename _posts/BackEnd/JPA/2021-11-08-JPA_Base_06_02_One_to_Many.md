---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 6-2강"
excerpt: "다양한 연관관계 매핑 - 일대다 [1:N]"
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
date:  2021-11-08 22:50:00 +0900
last_modified_at: 2021-11-08 22:50:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 일대다 [1:N]

이 모델은 권장하지 않는데 만약 일이 연관관계의 주인이면 어떻게 될까?

## 단방향의 경우

테이블의 일대다 관계에서는 무조건 다(N) 쪽에 외래키가 존재한다.

객체와 테이블의 차이 때문에 반대편 테이블의 외래키를 관리하는 특이한 구조이다.

@JoinColumn을 꼭 사용해야한다. 그렇지 않으면 조인 테이블방식을 사용하게된다.(중간에 중간테이블이 하나 추가된다.)

아래처럼 작성하면된다.

```java
/* 수정 전
@OneToMany(mappedBy = "team")
private List<Member> members = new ArrayList<>();
*/

// 수정 후
@OneToMany
@JoinColumn(name="TEAM_ID")
private List<Member> members = new ArrayList<>();
```

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {
            Member member = new Member();
            member.setUsername("member1");

            em.persist(member);

            Team team = new Team();
            team.setName("TeamA");
            team.getMembers().add(member);
            // team은 저장이 되는것이 맞지만 바로 윗줄은 좀 애매하다. team테이블에 저장되는 것이 아닌 member테이블에서 저장하므로 member테이블에서 update 쿼리가 나간다.

            em.persist(team);

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

나중에 실무로 일하다보면 테이블도 많아지고 복잡해지기 때문에

Team을 수정했는데 Member테이블이 수정되기 때문에 유지보수 중에 혼란해질 수 있다.

그래서 일대다 단방향은 다대일에서 양방향 매핑으로 변경하여 사용하면 편해진다.

## 양방향의 경우

설명을 위해 억지성이 조금 있다. 이 매핑은 공식적으로 존재하지 않는다.

읽기 전용 필드를 사용해서 양방향처럼 사용하는 방법이다.

```java
@ManyToOne
@JoinColumn(name = "TEAM_ID", insertable = false, updatable = false)
private Team team;
```

다대일 양방향을 사용하는 것이 편하다.
