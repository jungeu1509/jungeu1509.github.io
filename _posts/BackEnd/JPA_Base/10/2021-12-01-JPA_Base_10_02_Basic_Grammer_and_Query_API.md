---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 10-2강"
excerpt: "객체지향 쿼리 언어1 - 기본 문법 - 기본문법과 쿼리"
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
date:  2021-12-01 23:00:00 +0900
last_modified_at: 2021-12-01 23:00:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. JPQL 소개

- JPQL은 객체지향 쿼리 언어다. 따라서 테이블을 대상으로 쿼리 하는 것이 아니라 엔티티 객체를 대상으로 쿼리한다.
- JPQL은 SQL을 추상화해서 특정데이터베이스 SQL에 의존하 지 않는다.
- JPQL은 결국 SQL로 변환된다.

# 2. JPQL 기본문법

- select m from Member as m where m.age > 18
- 엔티티와 속성은 대소문자 구분O (Member, age)
- JPQL 키워드는 대소문자 구분X (SELECT, FROM, where)
- 엔티티 이름 사용, 테이블 이름이 아님(Member) 
  - 그래서 보통 클래스 이름과 테이블 이름을 맞춰주는게 정신건강에 편하다. 다르면 클래스 이름을 바꾸는게... ㅋㅋㅋ
- **별칭은 필수(m)** (as는 생략가능)

```
select_문 :: = 
  select_절
  from_절 
  [where_절] 
  [groupby_절] 
  [having_절] 
  [orderby_절]

update_문 :: = update_절 [where_절] 
delete_문 :: = delete_절 [where_절]
```

## 집합과 정렬

기본적으로 아래는 다 된다!

```
select
  COUNT(m), //회원수
  SUM(m.age), //나이 합
  AVG(m.age), //평균 나이 
  MAX(m.age), //최대 나이 
  MIN(m.age) //최소 나이
from Member m
```

- GROUP BY, HAVING 
- ORDER BY

그대로 쓰면 된다!

## TypeQuery, Query

- TypeQuery: 반환 타입이 명확할 때 사용
- Query: 반환 타입이 명확하지 않을 때 사용

반환타입이 명확하면 아래처럼 쓸 수 있다.

```java
TypedQuery<Member> query =
  em.createQuery("SELECT m FROM Member m", Member.class);

```

명확하지 않으면 아래처럼 쓸 수 있다.

```java
Query query =
em.createQuery("SELECT m.username, m.age from Member m");
```

username은 string이고 age라서 타입정보가 다르거나 받을 수 없을때 사용한다.

## 결과 조회 API

- query.getResultList(): 결과가 하나 이상일 때, 리스트 반환 
  - 결과가 없으면 빈리스트 반환
- query.getSingleResult(): 결과가 정확히 하나, 단일 객체 반환
  - 결과가 없으면: javax.persistence.NoResultException
  - 둘 이상이면: javax.persistence.NonUniqueResultException

```java
TypedQuery<Member> query =
  em.createQuery("SELECT m FROM Member m", Member.class);
List<Member> resultList = query.getResultList();

for(Member member1 : resultList) {
  System.out.println("member1 = " + member1);
}
```

```java
TypedQuery<Member> query =
  em.createQuery("SELECT m FROM Member m WHERE m.id = 10", Member.class);
Member result = query.getSingleResult();

System.out.println("result = " + result);
```

결과가 없으면 exception을 반환한다...

Spring Data JPA에서는 null값을 반환하거나 Optional로 반환한다.

## 파라미터 바인딩 - 이름 기준, 위치 기준

```java
SELECT m FROM Member m where m.username=:username query.setParameter("username", usernameParam);
```

```java
SELECT m FROM Member m where m.username=?1 query.setParameter(1, usernameParam);
```

파라미터를 통해 조건을 얻을 수 있다.

근데 위치기준은 잘못쓰면 위치가 뒤틀어져(밀려)버릴 수 있기때문에 안쓰는 것을 추천한다.(enum타입을 db에서 안쓰는 것과 비슷한 원리)
