---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 5-3강"
excerpt: "양방향 연관관계와 연관관계의 주인 2 - 주의점, 정리"
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
date:  2021-11-03 21:30:00 +0800
last_modified_at: 2021-11-03 21:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 양방향 매핑시 자주하는 실수

## 연관관계의 주인에 값을 입력하지 않음

테이블은 FK를 이용하여 관리가 되기 때문에 연관관계 주인에 매핑되는 것이 중요하다.

```java
Team team = new Team(); 
team.setName("TeamA"); 
em.persist(team);

Member member = new Member(); 
member.setName("member1");

//역방향(주인이 아닌 방향)만 연관관계 설정 
team.getMembers().add(member);
em.persist(member);
```

이럴 경우 Member에 Team이 매핑되지 않으므로 테이블에 적용되지 않는다.

사실 JPA 입장에서는 주인에만 넣으면 되는 코드이지만 객체 관계를 고려하면 아래처럼 양쪽에 넣어주는게 완벽하다.(순수 코드로만 테스트할때도 양쪽으로 하는것이 편하다.)

```java
Team team = new Team(); 
team.setName("TeamA"); 
em.persist(team);

Member member = new Member(); 
member.setName("member1");

team.getMembers().add(member); 
//연관관계의 주인에 값 설정
member.setTeam(team); //**
em.persist(member);
```

# 실수를 없애기 위해 정하기!

- 순수 객체 상태를 고려해서 항상 양쪽에 값을 설정하자
- 연관관계 편의 메소드를 생성하자
- 양방향 매핑시에 무한 루프를 조심하자
  - 예: toString(), lombok, JSON 생성 라이브러리

## 연관관계 편의 메서드

```java
//Member.java
    public void setTeam(Team team) {
        this.team = team;
        team.getMembers().add(this);
    }
```

근데 로직이 들어가는 메서드 이름에서 set을 넣는건 위험부담이 좀 있다. 그래서 changeTeam같이 이름을 바꾸는 것을 추천한다.

사실 이렇게 넣으면서 null 체크도 하고 해야할 것이 많다.(이미 연결된 것을 뺀다거나 해야한다.)

근데 편의를 위해서 연관관계의 주인이 아닌곳에서도 추가할 수 있도록 메서드를 집어넣는다.

```java
//Team.java
    public void addMember(Member member) {
        member.setTeam(this);
        members.add(member);
    }
```

## 양방향 매핑시 무한루프 주의

toString(), lombok, JSON 생성 라이브러리

위와 같은것들을 사용할때 양쪽을 서로 참조해서 무한루프 상황을 조심해야 한다.

toString을 예로들면 각자 객체로 들어가 또 toString을 하므로 참조참조가 된다. 

실제로 실무에서 Controller에서 entity를 바로 반영을 할 경우 entity를 Json으로 바꿀때 타고타고 무한루프를 탈 수 있다.

추천하는 것은 Controller에서 entity를 API스펙으로 바로 바꾸지 마라. dto로 변환해서 반환하는 것을 추천한다.

# 양방향 매핑 정리

단방향 매핑만으로도 이미 연관관계 매핑은 완료된 것이다.

양방향 매핑은 반대방향으로 조회 기능이 추가된 것 뿐이다.

사실 설계시에는 양방향매핑은 고려할게 너무 많아진다. 그래서 대부분 단방향으로 매핑하는 것이 좋다.

단방향 매핑을 잘 한 후 나중에 양방향은 추가할 수 있다.(테이블에 영향을 주지 않는다.)

# 연관관계의 주인을 정하는 기준

- 비즈니스 로직을 기준으로 연관관계의 주인을 선택하면 안됨
- 연관관계의 주인은 외래 키의 위치를 기준으로 정해야함
 