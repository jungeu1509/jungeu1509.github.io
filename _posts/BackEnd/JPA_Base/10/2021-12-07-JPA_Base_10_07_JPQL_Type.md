---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 10-7강"
excerpt: "객체지향 쿼리 언어1 - 기본 문법 - JPQL 타입 표현과 기타식"
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
date:  2021-12-07 00:30:00 +0900
last_modified_at: 2021-12-07 00:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. JPQL 타입 표현

- 문자 : 'HELLO', 'She''s'
- 숫자 : 10L(Long), 10D(Double), 10F(Float)
- Boolean : TRUE, FALSE
- ENUM: jpabook.MemberType.Admin (패키지명 포함)
- 엔티티 타입: TYPE(m) = Member (상속 관계에서 사용)

ENUM 사용할때 주의해야 한다.

## 1.1. ENUM 타입 예시

```java

@Entity
public class Member{
  // 중략
  @Enumerated(EnumType.STRING)
  private MemberType type;
  // 후략
}

public enum MemberType {
  ADMIN, USER
}
```

위처럼 코드가 존재한다고 할때

```java
String query = "select m.username, 'HELLO', TRUE From Member m";
List<Object[]> result = em.createQuery(query)
    .getResultList();

for(Object[] objects : result) {
  System.out.println("objects = " + objects[0]);
  System.out.println("objects = " + objects[1]);
  System.out.println("objects = " + objects[2]);
}
```

위처럼 출력한다고 했을 때 아래처럼 출력된다.

```bash
objects = teamA 
objects = HELLO
objects = true
```

쿼리에서 true의 경우 대소문자 구분하지 않는다.

```java
String query = "select m.username, 'HELLO', TRUE From Member m " + 
              "where m.type = jpql.MemberType.ADMIN";
```

ADMIN 앞은 패키지 이름이고 위 처럼 쿼리를 넣으면 ADMIN 타입의 멤버만 조회하게된다.

아래처럼 사용할 수도 있다.

```java
String query = "select m.username, 'HELLO', TRUE From Member m " + 
              "where m.type = :userType";
List<Object[]> result = em.createQuery(query)
    .setParameter("userType", MemberType.ADMIN)
    .getResultList();
```

## 1.2. 엔티티 타입 예시

Item을 Boojk, Album, Movie로 분리했다고 했을때 아래처럼 조건을 취할 수 있다.

(어노테이션으로 DiscriminatorValue라고 설정해놨을 때 - 기억이 안나면 상속관계를 다시 보면 된다.)

```java
String query = "select i From Item i " + 
              "where type(i) = Book";
em.createQuery(query, Item.class)
    .getResultList();
```

# 2. JPQL 기타

- SQL과 문법이 같은 식
- EXISTS, IN
- AND, OR, NOT
- =, >, >=, <, <=, <> 
- BETWEEN, LIKE, IS NULL

사실 SQL과 대부분 문법이 같다.

## 2.1. BETWEEN 예시

```java
String query = "select m.username, 'HELLO', true FROM Member m " +
            "where m.age between 0 and 10";
```
