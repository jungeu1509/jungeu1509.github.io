---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 5-1강"
excerpt: "연관관계 매핑 기초 - 단방향 연관관계"
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
date:  2021-10-30 21:00:00 +0800
last_modified_at: 2021-10-30 21:00:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 단방향 연관관계

## 매핑하기 전 객체지향적 문제 확인하기

우리는 아래의 사진처럼 객체지향 모델링을 하려 한다.

![객체 지향 모델링](/assets/images/posts/BackEnd/JPA/05/05_01_1_modelling.png)

우선 아래처럼 엔티티 두개를 저장해보자.

```java
@Table(name="member")
public class Member {
    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @Column(name = "USERNAME")
    private String name;

    @Column(name = "TEAM_ID")
    private Long teamId;
}

@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column
    private Long id;

    private String name;
}
```

그리고 아래 코드를 이용해 테이블이 생성되는지 확인해보자.

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {

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

```bash
create table member (
    MEMBER_ID bigint not null,
    USERNAME varchar(255),
    TEAM_ID bigint,
    primary key (MEMBER_ID)
)

create table Team (
    id bigint not null,
    name varchar(255),
    primary key (id)
)
```

테이블은 생성이 되었지만 각각 속성이 연결되지는 않았다. (외래키를 갖고 있지는 않는다.)

실제 데이터를 저장해보자. 아래 소스처럼 저장을 한다.

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
            member.setTeamId(team.getId()); // 이 부분이 객체지향스럽지 못한 느낌이다.
            em.persist(member);

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

위의 소스를 실행하면 아래와같이 데이터가 들어가게된다.

![매핑하기 전](/assets/images/posts/BackEnd/JPA/05/05_01_2_jpa_one_way_mapping.png "매핑하기 전")

이렇게 되면 조회할때도 문제가 있다.

```java
Member findMember = em.find(Member.class, member.getId());

Long findteamId = findMember.getTeamId();
Team findTeam = em.find(Team.class, findteamId);
```

조회할 때 가져온 객체의 데이터를 가져와 다시 조회하는 형태가 되기 때문이다.

객체지향스럽지 못하다.

## 단방향 연관관계 맺기

맨처음 사진의 객체 지향 모델링 처럼 연결을 하기 위해 아래처럼 Member 엔티티의 코드를 바꿔 작성한다.

```java
@Table(name="member")
public class Member {

    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @Column(name = "USERNAME")
    private String name;

//    @Column(name = "TEAM_ID")
//    private Long teamId;

    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
```

ManyToOne 은 다대일 매핑하는 어노테이션이고

JoinColumn 은 join 하려는 속성을 설정하는 어노테이션이다.

### 조회해보기

위처럼 설정하고 아래처럼 메인코드를 실행해보자.

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

            //만약 쿼리문이 날라가는걸 보고싶으면 아래 두 줄을 작성하면 된다.
            em.flush(); // 영속성 컨텍스트와 db의 싱크를 맞춰준다.
            em.clear(); // 영속성 컨텍스트 비워버리기!

            Member findMember = em.find(Member.class, member.getId());

            Team findTeam = findMember.getTeam();

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

위처럼 Member의 ID를 찾아서 다시 찾도록 실행시키는 것이 아닌 Member를 찾으면 Team이 같이 가져와지도록 할 수 있다.

### 수정해보기

그렇다면 수정할때는...?

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

            //만약 쿼리문이 날라가는걸 보고싶으면 아래 두 줄을 작성하면 된다.
            em.flush(); // 영속성 컨텍스트와 db의 싱크를 맞춰준다.
            em.clear(); // 영속성 컨텍스트 비워버리기!

            Member findMember = em.find(Member.class, member.getId());

            Team findTeam = findMember.getTeam();

            Team teamNew = new Team();
            teamNew.setName("TeamB");
            em.persist(teamNew);

            member.setTeam(teamNew);
            
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

위와같이 teamNew를 생성하여 member에 setTeam 해주면 자동으로 teamNew가 매핑된다.