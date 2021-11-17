---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 8-2강"
excerpt: "프록시와 연관관계 관리 - 즉시 로딩과 지연 로딩"
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
date:  2021-11-18 00:30:00 +0900
last_modified_at: 2021-11-17 00:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 지연로딩(LAZY)을 사용해서 조회

```java
@Entity
Public Class Member{
  @ManyToOne(fetch = FetchType.LAZY)
  @JoinColumn
  Team team;
}
```

위와같이 fetch에 LAZY 속성으로 코딩하고 객체를 조회하면 Member클래스만 DB에서 조회하고 Team 객체는 조회하지 않는다.

Team 객체는 프록시로 세팅되게 된다.

Team에서 실제 값을 사용하면 그 때 객체를 초기화한다.

# 2. 즉시로딩(EAGER)을 사용해서 조회

```java
@Entity
Public Class Member{
  @ManyToOne(fetch = FetchType.EAGER)
  @JoinColumn
  Team team;
}
```

위와 같이 fetch에 EAGER 속성으로 코딩하고 객체를 조회하면 Member 클래스를 조회할때 join해서 Team까지 가져온다.

# 3. 실무에서는 즉시로딩을 쓰면 안된다.

가급적 지연로딩만 사용해야한다.

1. 즉시 로딩을 사용하면 예상하지 못한 SQL이 나간다.
2. 즉시 로딩은 JPQL에서 N+1 문제를 일으킨다.
   
## N+1 문제 예제

```java
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {

            Team teamA = new Team();
            teamA.setName("TeamA");
            em.persist(teamA);

            Team teamB = new Team();
            teamB.setName("TeamB");
            em.persist(teamB);

            Member member1 = new Member();
            member1.setName("member1");
            member1.setTeam(teamA);
            em.persist(member1);

            Member member2 = new Member();
            member2.setName("member2");
            member2.setTeam(teamB);
            em.persist(member2);

            em.flush();
            em.clear();
            
            Member m = em.find(Member.class, member.getId());

            List<Member> members = em.createQuery("select m from Member m",
                Member.class).getResultList();

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

만약 find 부분을 주석처리하고 createQuery만 사용한다고 해도 sql문이 두번나간다.

처음에 sql문을 그대로 전송한다. 그러면 member 테이블의 데이터만 쭉 다 가져온다. 그 후 객체를 생성하려고 보니 Team이 EAGER 속성이므로 team을 쭉 가져오게된다. 

```SQL
select * from Member
select * from Team where TEAM_ID == MEMBER_ID
```

즉 첫 Member 조회 쿼리가 1번 나가고
Member의 데이터 수 만큼 쿼리가 N번 추가적으로 나간다.

쿼리가 N+1 번 나간다고해서 N+1 문제이다.

부하를 줄이기 위해 속성을 LAZY로 하면 Member조회 쿼리만 한번 나간다.

그래서 해결책은 모든 연관관계는 LAZY로 부여한다. 그 후 3가지 방법이 있다.

1. 패치조인
2. 엔티티그래프어노테이션
3. 배치사이즈

주로 패치조인을 사용한다.

### 3.1. fetch join
동적으로 내가 원하는 것을 선택해서 한번에 싹 가져오는 것이다.

앱에서 어떤 화면에서는 member만 필요하고 어떤 화면에서는 team만 필요할텐데

만약 두개 다 필요하면 join쿼리를 이용해서 가져오는 것이다.

나중에 수업시간에 나오겠지만 예시를 들어보면 아래와 같다.

```java
List<Member> members = em.createQuery("select m from Member m join fetch m.team",
                Member.class).getResultList();
```

모든 데이터가 채워져서 나온다.

## XToOne 에서의 기본설정

기본이 즉시로딩으로 되어있다. 그러므로 LAZY를 직접 입력해서 설정해야한다.
