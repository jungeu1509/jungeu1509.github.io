---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 10-3강"
excerpt: "객체지향 쿼리 언어1 - 기본 문법 - 프로젝션"
header:
  teaser: 
search: false
permalink:
categories: 
  - BackEnd
tags:
  - java
  - backend
  - study
date:  2021-12-02 00:50:00 +0900
last_modified_at: 2021-12-02 00:50:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 프로젝션

- SELECT 절에 조회할 대상을 지정하는 것
- 프로젝션 대상: 엔티티, 임베디드 타입, 스칼라 타입(숫자, 문자등 기본 데이터 타입)
- SELECT m FROM Member m -> 엔티티 프로젝션 
- SELECT m.team FROM Member m -> 엔티티 프로젝션
- SELECT m.address FROM Member m -> 임베디드 타입 프로젝션
- SELECT m.username, m.age FROM Member m -> 스칼라 타입 프로젝션 
- DISTINCT로 중복 제거

## 1.1. 엔티티 프로젝션 1

```java
Member member = new Member();
member.setUsername("member1");
member.setAge(10);
em.persist(member);

em.flush();
em.clear();

List<Member> result = em.createQuery("select m from Member m", Member.class).getResultList();

Member findMember = result.get(0);
findMember.setAge(20);

tx.commit();
```

위를 실행했을때 update쿼리문이 나간다. getResultList를 사용하면 영속성 컨텍스트에 업데이트 된다는 소리이다.

## 1.2. 엔티티 프로젝션 2

```sql
SELECT m.team FROM Member m
```

위처럼 쿼리문을 할경우 join문이 발생한다.

위 쿼리문은 안쓰는 것이 좋고 이왕이면 명시적으로 하는것이 좋다.(위같은 쿼리를 묵시적 쿼리라고 한다.)

## 1.3. 임베디드 타입 프로젝션

```sql
SELECT m.address FROM Member m
```

임베디드타입으로 객체에서는 분리했지만 db에서는 합쳐져 있기 때문에 아무 상관없이 select 되어진다.

## 1.4. 스칼라 타입 프로젝션

```sql
SELECT m.username, m.age FROM Member m
```

```sql
SELECT DISTINCT m.username, m.age FROM Member m
```

위처럼 막 가져오는 것을 스칼라 타입 프로젝션이라고 한다.

## 1.5. 프로젝션 여러 값 조회

스칼라 타입의 예제처럼 여러개의 값을 가져올때는 3가지의 방법으로 가져올 수 있다.

### 1.5.1. Query 타입으로 조회

이전 강의에서 배웠던 Query 타입으로 조회를 한다.

예를 들어보면 아래와 같은데 내부는 Object타입으로 들어있다.

```java
Query query =
    em.createQuery("SELECT m.username, m.age from Member m");

List resultList =
    em.createQuery("SELECT m.username, m.age from Member m")
    .getResultList();    

Object o = resultList.get(0);
Object[] result = (Object[]) o;
System.out.println("username = "+ result[0]);
System.out.println("age = "+ result[1]);
```

### 1.5.2. Object[] 타입으로 조회

```java
List<Object[]> resultList =
    em.createQuery("SELECT m.username, m.age from Member m")
    .getResultList();    

Object[] result = resultList.get(0);
System.out.println("username = "+ result[0]);
System.out.println("age = "+ result[1]);
```

### 1.5.3. new 명령어 사용

제일 깔끔한 방법

- 단순 값을 DTO로 바로 조회 SELECT new jpabook.jpql.UserDTO(m.username, m.age) FROM Member m
- 패키지명을 포함한 전체 클래스명 입력
- 순서와 타입이 일치하는 생성자 필요

```sql
SELECT new jpabook.jpql.UserDTO(m.username, m.age) FROM Member m
```
