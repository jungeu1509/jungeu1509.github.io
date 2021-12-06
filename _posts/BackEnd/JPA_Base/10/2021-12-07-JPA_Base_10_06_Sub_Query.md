---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 10-6강"
excerpt: "객체지향 쿼리 언어1 - 기본 문법 - 서브쿼리"
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
date:  2021-12-07 00:10:00 +0900
last_modified_at: 2021-12-07 00:10:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 서브 쿼리

서브쿼리는 쿼리가 있는데 그 안에서 또 쿼리를 집어넣어 만들 수 있는 쿼리를 말한다.

## 1.1. 예시

- 나이가 평균보다 많은 회원 
  
```sql
select m 
from Member m
where m.age > (
  select avg(m2.age) 
  from Member m2
)
```

- 한 건이라도 주문한 고객
  
```sql
select m 
from Member m
where (
  select count(o) 
  from Order o 
  where m = o.member
) > 0
```

# 2. 서브 쿼리 지원 함수

- [NOT] EXISTS (subquery): 서브 쿼리에 결과가 존재하면 참 
  - {ALL | ANY | SOME} (subquery)
  - ALL 모두 만족하면 참
  - ANY, SOME: 같은 의미, 조건을 하나라도 만족하면 참
- [NOT] IN (subquery): 서브 쿼리의 결과 중 하나라도 같은 것이 있으면 참

## 2.1. 예제 

- 팀A 소속인 회원

```sql
select m 
from Member m
where exists (
  select t 
  from m.team t 
  where t.name = '팀A'
)
```

- 전체 상품 각각의 재고보다 주문량이 많은 주문들
  
```sql
select o 
from Order o 
where o.orderAmount > ALL (
  select p.stockAmount 
  from Product p
)
```

- 어떤 팀이든 팀에 소속된 회원 
  
```sql
select m 
from Member m 
where m.team = ANY (
  select t 
  from Team t
)
```

# 3. JPA 서브 쿼리 한계

- JPA는 WHERE, HAVING 절에서만 서브 쿼리 사용 가능
- SELECT 절도 가능(하이버네이트에서 지원)
- FROM 절의 서브 쿼리는 현재 JPQL에서 불가능
  - 조인으로 풀 수 있으면 풀어서 해결
  - 그래도 안되면 포기...

## 3.1. 불가능 예시

```java
String query = "select mm.age, mm.username" +
    "from (select m.age, m.username from Member m) as mm";
List<Member> result = em.createQuery(query, Member.class)
  .getResultList();
```

from 안에 서브쿼리는 불가능... 보통 join절로 풀어서 해결한다.

정 안되면 네이티브 SQL을 사용하거나, 다 가져와서 Application안에서 조작하는 식으로 사용하거나 쿼리를 두번 날린다.

