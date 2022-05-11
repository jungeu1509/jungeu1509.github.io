---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 10-1강"
excerpt: "객체지향 쿼리 언어1 - 기본 문법 - 소개"
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
date:  2021-11-29 23:00:00 +0900
last_modified_at: 2021-11-29 23:00:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. JPA는 다양한 쿼리 방법을 지원한다.

JPQL

JPA Criteria

QueryDSL

네이티브 SQL

JDBC API 직접 사용, MyBatis, SpringJdbcTemplate 함께 사용

실무에서는 대부분 JPQL을 사용하는데 아주 드물게 안될때는 JDBC API를 사용한다.

# 2. JPQL 소개

가장 단순한 조회 방법

  - EntityManager.find()
  - 객체 그래프 탐색 (a.getB().getC())

그런데 나이가 18살 이상인 회원을 모두 검색하고 싶다면?

JPA는 엔티티 객체 중심으로 개발하는데 문제는 검색쿼리이다.

검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색해야한다.

모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능하다.

애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL이 필요하다.

최소한의 필요한 데이터만 가져오려는 노력을 해야하기 때문이다. (DB성능 문제 등...)

**JPA**는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어 제공

SQL과 문법 유사, SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN 지원

즉 가장 큰 차이는 

- JPQL은 엔티티 객체를 대상으로 쿼리 
- SQL은 데이터베이스 테이블을 대상으로 쿼리



```java
//검색
String jpql = "select m From Member m where m.name like ‘%hello%'";

List<Member> result = em.createQuery(jpql, Member.class).getResultList();
```

문법이 sql이랑 거의 동일하다. 그러나 몇가지 차이가 있는데 엔티티 객체를 대상으로 하는 것이 그 중 하나이다.

위의 jpql 예제에서 m은 Member를 의미하고 member자체를 조회하는것을 의미한다.

실행된 SQL문은 아래와 같다.

```
select
    m.id as id,
    m.age as age,
    m.USERNAME as USERNAME,
    m.TEAM_ID as TEAM_ID
from
    Member m
where
    m.age>18
```


계속 강조를 하면 아래와 같다.

- 테이블이 아닌 객체를 대상으로 검색하는 객체 지향 쿼리
- SQL을 추상화해서 특정 데이터베이스 SQL에 의존X
- JPQL을 한마디로 정의하면 객체 지향 SQL

# Criteria 소개

jpql을 짤때 결국 넣는것은 String 문이다. String문을 직접 작성하다보면 띄어쓰기, 오타 등 오류가 많이 난다. 그래서 만들어진게 Criteria이다.

- 문자가 아닌 자바코드로 JPQL을 작성할 수 있음
- JPQL 빌더 역할
- JPA 공식 기능
- **단점: 너무 복잡하고 실용성이 없다.**
- Criteria 대신에 QueryDSL 사용 권장

```java
//Criteria 사용 준비
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery<Member> query = cb.createQuery(Member.class);

//루트 클래스 (조회를 시작할 클래스)
Root<Member> m = query.from(Member.class);

//쿼리 생성 
CriteriaQuery<Member> cq =  query.select(m).where(cb.equal(m.get("username"), “kim”));
List<Member> resultList = em.createQuery(cq).getResultList();
```

짜다보면 어렵기도 하다 ㅠㅠ...

그러나 장점은 잘못쓰면 컴파일 에러가난다. 동적으로 쿼리를 짜기 쉬워진다.

그러나 유지보수가 어렵고 가독성이 너무 안좋아 **실무에서 잘 안쓴다.**

# QueryDSL 소개

```java
  //JPQL
  //select m from Member m where m.age > 18
JPAFactoryQuery query = new JPAQueryFactory(em); 
QMember m = QMember.member;

List<Member> list = 
    query.selectFrom(m)
        .where(m.age.gt(18)) 
        .orderBy(m.name.desc()) 
        .fetch();
```

- 문자가 아닌 자바코드로 JPQL을 작성할 수 있음
- JPQL 빌더 역할
- 컴파일 시점에 문법 오류를 찾을 수 있음 
- 동적쿼리 작성 편리함
- 단순하고 쉬움(JPQL과 거의 1:1 이다.)
- 실무 사용 권장

# 네이티브 SQL 소개

- JPA가 제공하는 SQL을 직접 사용하는 기능
- JPQL로 해결할 수 없는 특정 데이터베이스에 의존적인 기능
- 예) 오라클 CONNECT BY, 특정 DB만 사용하는 SQL 힌트(꼭 네이티브 SQL이 아니더라도 Hibernate나 방언 세팅을 해서 동작하도록 설정할 수 있다.)

```java
String sql =
    “SELECT ID, AGE, TEAM_ID, NAME FROM MEMBER WHERE NAME = ‘kim’";

List<Member> resultList =
            em.createNativeQuery(sql, Member.class).getResultList();
```

강사님은 실무에서 잘 안쓰고 사용해야되면 스프링에서 제공하는 jdbcTemplate를 사용한다.


# JDBC 직접 사용, SpringJdbcTemplate 등

- JPA를 사용하면서 JDBC 커넥션을 직접 사용하거나, 스프링 JdbcTemplate, 마이바티스등을 함께 사용 가능
- 단 영속성 컨텍스트를 적절한 시점에 강제로 플러시 필요
  - 영속성 컨텍스트는 flush가 되어야 db에 데이터가 있다.
- 예) JPA를 우회해서 SQL을 실행하기 직전에 영속성 컨텍스트 수동 플러시

# 정리

강사님은 주로 JPQL과 QueryDSL로 해결한다. 진짜 안되는 5프로 정도는 JdbcTemplate로 사용한다.
