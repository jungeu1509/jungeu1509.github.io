---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 11-1강"
excerpt: "객체지향 쿼리 언어2 - 중급 문법 - 경로 표현식"
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
date:  2022-01-17 10:33:00 +0900
last_modified_at: 2021-01-17 10:33:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 경로 표현식

- 경로 표현식 : 점(.)을 찍어 객체 그래프를 탐색하는 것

예시는 아래와 같다.

```sql
select m.username
from Member m
join m.team t
join m.orders o
where t.name = '팀A'
```

m.username 은 상태필드라고 한다.
상태필드로 객체 그래프를 탐색했다고 볼 수 있다.

join m.team 은 엔티티로 넘어간것인데 단일 값 연관필드라고 한다.

join m.orders 는 양방향 관계가 걸려있는데 컬렉션이다. 컬렉션 값 연관필드라고 한다.

3가지 필드에 따라 내부적으로 동작 방식이 달라지고 결과가 달라지므로 구분하여 이해햐하 한다.

# 2. 경로 표현식 용어 정리

## 2.1. 상태 필드(State field)

단순히 값을 저장하기 위한 필드

## 2.2. 연관 필드(Association field)

연관관계를 위한 필드

- 단일 값 연관 필드 : @ManyToOne, @OneToOne 대상이 엔티티(ex: m.team)
- 컬렉션 값 연관 필드 : @OneToMany, @ManyToMany 대상이 컬렉션(ex: m.orders)

# 3. 경로 표현식 특징

- 상태필드 : 경로 탐색의 끝, 탐색X(탐색이 되지 않는다.)
- 단일 값 연관 경로 : **묵시적 내부 조인(inner join) 발생**, 탐색 O
- 컬렉션 값 연관 경로 : 묵시적 내부 조인 발생, 탐색 X
  - FROM 절에서 명시적 조인을 통해 별칭을 얻으면 별칭을 통해 탐색 가능

실제로 운용할떄는 묵시적 내부조인 발생하게 작성하면 안된다.

성능에 문제가 될 수 있고 많은 DB를 써야할 수 있는데 찾기 어려울 수 있다.

외부 조인은 불가능하다. 외부 조인을 사용하고 싶다면 명시적 조인을 사용해야한다.

## 3.1. 명시적 조인, 묵시적 조인

- 명시적 조인 : join 키워드 직접 사용
```sql
select m from Member m join m.team t
```

- 묵시적 조인 : 경로 표현식에 의해 묵시적으로 SQL 조인 발생(내부 조인만 가능)
```sql
select m.team from Member m
```

## 3.2. 단일 값 연관 경로 탐색

JPQL을 아래처럼 작성하면 묵시적 내부 조인이 계속 발생하여 난감해진다.

- JPQL
```sql
select o.member from Order o
```

- SQL
```sql
select m.* 
from Orders o 
inner join Member m on o.member_id = m.id
```

## 3.3. 경로 표현식 예제

```sql
select o.member.team from Order o # 성공
select t.members from Team # 성공
select t.members.username from Team t # 실패
select m.username from Team t join t.members m # 성공
```

첫번째 문장은 심플해 보이지만 조인이 2번 발생한다. 어떤 sql이 발생할지 예상하기 어렵고 사용하기 애매하다.

두번째 문장은 컬렉션에서 끝났으므로 성공한다.

세번째 문장은 컬렉션에서 더 들어갈 수 없음므로 실패한다. t.members로 끝내거나 t.members.size 정도를 사용할 수 있다.

네번째 문장은 명시적 조인으로 members를 가져와서 members에서 다시 시작했으므로 성공한다.

## 3.4. 경로 탐색을 사용한 묵시적 조인 시 주의사항

- 항상 내부 조인
- 컬렉션은 경로 탐색의 끝, 명시적 조인을 통해 별칭을 얻어야함
- 경로 탐색은 주로 SELECT, WHERE 절에서 사용하지만 묵시 적 조인으로 인해 SQL의 FROM (JOIN) 절에 영향을 줌

### 3.4.1. 실무적 조언

- 가급적 묵시적 조인 대신에 명시적 조인 사용(아예 쓰지 말아야...)
- 조인은 SQL 튜닝에 중요 포인트
- 묵시적 조인은 조인이 일어나는 상황을 한눈에 파악하기 어려움
