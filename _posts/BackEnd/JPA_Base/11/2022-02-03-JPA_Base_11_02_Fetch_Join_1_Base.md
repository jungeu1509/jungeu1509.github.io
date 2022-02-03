---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 11-2강"
excerpt: "객체지향 쿼리 언어2 - 중급 문법 - 페치 조인 1 - 기본"
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
date:  2022-02-03 18:39:00 +0900
last_modified_at: 2022-02-03 18:39:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 페치 조인(Fetch join)

- SQL 조인 종류는 아니다.
- JPQL에서 성능 최적화를 위해 제공하는 기능이다.
- 연관된 엔티티나 컬렉션을 SQL 한 번에 함께 조회하는 기능이다.(두번 나갈것 같은 쿼리문을 한방쿼리로 풀어낼 수 있는 기능이다.)
- 명령어는 join fetch 명령어를 사용한다.

실무에서 엄청 많이 쓰임!!!

## 1.1. 엔티티 패치 조인 예시

회원을 조회하면서 연관된 팀도 함께 조회하고싶다.(SQL 한 번에)

그러나 SQL을 보면 회원 뿐만 아니라 팀도 함께 SELECT하게 되어있다.

이를 바꾼다면?

[JPQL]
```sql
select m from Member m join fetch m.team
```

[SQL]
```sql
select M.*, T.* FROM MEMBER M
INNER JOIN TEAM T ON M.TEAM_ID=T.ID
```

jpql에서는 select m만 넣었는데 변경된 sql문 에서는 m과 t가 모두 같이 나간다.

## 1.2. 패치 조인 사용 예시

### 1.2.1. 패치 조인을 사용하지 않았을때

패치조인을 사용하지 않고 쿼리문을 작성해보자.

```java
public class JpaMain {
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
            teamB.setName("teamB");
            em.persist(teamB);

            Member member1 = new Member();
            member1.setName("member1");
            member1.setTeam(teamA);
            em.persist(member1);

            Member member2 = new Member();
            member2.setName("member2");
            member2.setTeam(teamA);
            em.persist(member2);

            Member member3 = new Member();
            member3.setName("member3");
            member3.setTeam(teamB);
            em.persist(member3);

            em.flush();
            em.clear();

            String query = "select m From Member m";

            List<Member> result = em.createQuery(query, Member.class).getResultList();

            for(Member member : result) {
                System.out.println("member = " + member);
            }

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

위의 코드를 실행하면 아래처럼 출력이 된다.

```bash
Hibernate: 
    /* select
        m 
    From
        Member m */ select
            member0_.MEMBER_ID as member_i1_3_,
            member0_.city as city2_3_,
            member0_.street as street3_3_,
            member0_.zipcode as zipcode4_3_,
            member0_.USERNAME as username5_3_,
            member0_.TEAM_ID as team_id6_3_ 
        from
            member member0_
Hibernate: 
    select
        team0_.TEAM_ID as team_id1_5_0_,
        team0_.name as name2_5_0_ 
    from
        Team team0_ 
    where
        team0_.TEAM_ID=?
member = member1, TeamA
member = member2, TeamA
Hibernate: 
    select
        team0_.TEAM_ID as team_id1_5_0_,
        team0_.name as name2_5_0_ 
    from
        Team team0_ 
    where
        team0_.TEAM_ID=?
member = member3, teamB
```

위의 출력을 통해 실행 순서를 생각해보자.

처음 루프를 돌면서 select하고 teamA를 가져온다. 영속성 컨텍스트에 teamA를 저장하고 출력한다.

member2는 TeamA 소속이므로 이미 영속성 컨텍스트에 저장되어있는 것을 가져온다.

세번째 루프에서 member3은 TeamB 소속이므로 영속성 컨텍스트에 저장되어 있지 않다.

TeamB 가져오는 쿼리를 실행하여 영속성 컨텍스트에 teamB를 저장한다.

영속성 컨텍스트에 있는 member3의 데이터를 출력한다.

회원 100명을 조회했을때 최악의 경우 sql이 101번 나가게된다. 이 문제는 **N + 1** 이라고 불린다.

즉시로딩 지연로딩 상관없이 모든경우 발생한다.

그러면 패치조인을 사용한다고 해보자.

### 1.2.2. 패치 조인을 사용할 때

```java
String query = "select m From Member m join fetch m.team";
```

쿼리문만 위처럼 바꿔보자.

```bash
/* select
        m 
    From
        Member m 
    join
        fetch m.team */ select
            member0_.MEMBER_ID as member_i1_3_0_,
            team1_.TEAM_ID as team_id1_5_1_,
            member0_.city as city2_3_0_,
            member0_.street as street3_3_0_,
            member0_.zipcode as zipcode4_3_0_,
            member0_.USERNAME as username5_3_0_,
            member0_.TEAM_ID as team_id6_3_0_,
            team1_.name as name2_5_1_ 
        from
            member member0_ 
        inner join
            Team team1_ 
                on member0_.TEAM_ID=team1_.TEAM_ID
member = member1, TeamA
member = member2, TeamA
member = member3, teamB
```

위를 보면 select문 실행시 join을 이용해서 team을 다 갖고오게된다. 즉, 프록시 객체가 아니고 엔티티객체이다.

그래서 지연로딩으로 설정을 해도 지연로딩 없이 깔끔하게 쭉 출력된다.

출력문에서 inner join을 사용했는데 쿼리문을 아래처럼 하면 outer join이 된다.

```java
String query = "select m From Member m left join fetch m.team";
```

# 2. 컬렉션 패치 조인

반대로 일대다관계의 패치 조인을 예시를 통해 생각해보자.

[JPQL]
```sql
select t from Team t join fetch t.members where t.name = 'teamA'
```

[SQL]
```sql
select T.*, M.* FROM TEAM T
INNER JOIN MEMBER M ON T.ID=M.TEAM_ID
WHERE T.NAME = 'teamA'
```

## 2.1. 컬렉션 패치 조인 사용 예시

위의 예제에서 쿼리문과 출력문만 아래처럼 바꿔보자.

```java
String query = "select t From Team t join fetch t.members";

List<Team> result = em.createQuery(query, Team.class).getResultList();

for(Team team : result) {
    System.out.println("team = " + team + " | members = " + team.getMembers().size());
    for(Member member : team.getMembers()) {
        System.out.println("-> member = " + member);
    }
}
```

위 코드를 실행하면 결과가 아래처럼 출력된다.

```bash
Hibernate: 
    /* select
        t 
    From
        Team t 
    join
        fetch t.members */ select
            team0_.TEAM_ID as team_id1_5_0_,
            members1_.MEMBER_ID as member_i1_3_1_,
            team0_.name as name2_5_0_,
            members1_.city as city2_3_1_,
            members1_.street as street3_3_1_,
            members1_.zipcode as zipcode4_3_1_,
            members1_.USERNAME as username5_3_1_,
            members1_.TEAM_ID as team_id6_3_1_,
            members1_.TEAM_ID as team_id6_3_0__,
            members1_.MEMBER_ID as member_i1_3_0__ 
        from
            Team team0_ 
        inner join
            member members1_ 
                on team0_.TEAM_ID=members1_.TEAM_ID
team = hellojpa.Team@3a66e67e | members = 2
-> member = hellojpa.Member@117d32e
-> member = hellojpa.Member@2bc378f7
team = hellojpa.Team@3a66e67e | members = 2
-> member = hellojpa.Member@117d32e
-> member = hellojpa.Member@2bc378f7
team = hellojpa.Team@268cbb86 | members = 1
-> member = hellojpa.Member@10f7918f
```

DB의 입장에서 teamA를 가져오면 회원이 두명이므로 teamA 객체 자체가 2개의 항목으로 뻥튀기된다. (A팀을 2번 가져오는 셈이다. 앞으로 데이터 뻥튀기라고 부르겠다.)

멤버를 프록시 객체로 받아오면 하나의 팀으로 받아와 질 것인데 비효율 적이라고 생각할 수 있다.

그러나 실제로 출력된 객체의 주소값을 보게되면 사실 동일한 한곳을 가리키는 것을 알 수 있다.

즉, JPA입장에서는 조회한 컬렉션에서 두개의 결과가 같은 객체를 가리키게된다.

뒤에서 DB의 입장에서 2개가 나오는게 아니라 객체의 입장에서 줄어서 나오는 방법을 설명하겠다.

## 2.2. 주의

다대일 관계에서는 데이터가 늘어나지 않는다. 그냥 fetch join을 사용해도 된다.

# 3. 페치 조인과 DISTINCT

- SQL의 DISTINCT는 중복된 결과를 제거하는 명령
- JPQL의 DISTINCT 2가지 기능 제공
    - SQL에 DISTINCT를 추가
    - 애플리케이션에서 엔티티 중복 제거

SQL에 DISTINCT를 추가하지만 테이블 상에서 데이터가 다르므로 SQL 결과에서 중복제거를 실패하게된다. 위의 2.1. 챕터의 예시로 봤을때 팀은 같지만 join된 표에서 member가 다르기 때문에 중복이 아니라고 DB는 판단한다. 즉, 완전히 동일해야 DB입장에서 DISTINCT가 된다.

distict를 사용해도 동일한 결과가 출력이 되지만 JPA가 같은 식별자를 가진 엔티티를 제거하여 결과를 추려주게된다. 


## 3.1. DISTINCT 예시

distinct를 사용한 예시를 위해 직접 코드를 작성해보자.

```java
String query = "select distinct t From Team t join fetch t.members";

List<Team> result = em.createQuery(query, Team.class).getResultList();

for(Team team : result) {
    System.out.println("team = " + team + " | members = " + team.getMembers().size());
    for(Member member : team.getMembers()) {
        System.out.println("-> member = " + member);
    }
}
```

```bash
Hibernate: 
    /* select
        distinct t 
    From
        Team t 
    join
        fetch t.members */ select
            distinct team0_.TEAM_ID as team_id1_5_0_,
            members1_.MEMBER_ID as member_i1_3_1_,
            team0_.name as name2_5_0_,
            members1_.city as city2_3_1_,
            members1_.street as street3_3_1_,
            members1_.zipcode as zipcode4_3_1_,
            members1_.USERNAME as username5_3_1_,
            members1_.TEAM_ID as team_id6_3_1_,
            members1_.TEAM_ID as team_id6_3_0__,
            members1_.MEMBER_ID as member_i1_3_0__ 
        from
            Team team0_ 
        inner join
            member members1_ 
                on team0_.TEAM_ID=members1_.TEAM_ID
team = hellojpa.Team@4596f8f3 | members = 2
-> member = hellojpa.Member@2370ac7a
-> member = hellojpa.Member@10f7918f
team = hellojpa.Team@64d4f7c7 | members = 1
-> member = hellojpa.Member@54e02f6a
```

for문으로 리스트 전체를 돌았으나 자동으로 team이 줄여져있는것을 알 수 있다.

# 4. 일반 조인 vs 패치 조인 차이

일반 조인을 사용했을 때 team을 가져온다고 생각해보자. 연관된 엔티티를 함께 조회하지 않는다. 정확한 데이터를 가져오는 것이 아니기 때문에 데이터 뻥튀기현상이 발생한다.

그러나 페치조인을 할 경우 연관된 데이터를 함께 퍼 올리게 된다.

## 4.1. 일반조인

[JPQL]
```sql
select t 
from Team t join t.members m
where t.name = 'teamA'
```

[SQL]
```sql
select T.*
INNER JOIN MEMBER M ON T.ID=M.TEAM_ID
WHERE T.NAME = 'teamA'
```

- JPQL은 결과를 반환할 때 연관관계 고려하지 않는다.
- SELECT절에 지정된 데이터만 가져온다.
- 여기서는 팀 엔티티만 조회하고, 회원 엔티티는 조회하지 않는다.

## 4.2. 패치 조인

[JPQL]
```sql
select t 
from Team t join fetch t.members 
where t.name = 'teamA'
```

[SQL]
```sql
select T.*, M.* FROM TEAM T
INNER JOIN MEMBER M ON T.ID=M.TEAM_ID
WHERE T.NAME = 'teamA'
```

- 연관된 엔티티도 함께 조회(즉시 로딩)된다.
- 객체 그래프를 SQL 한번에 조회하는 개념이다.
