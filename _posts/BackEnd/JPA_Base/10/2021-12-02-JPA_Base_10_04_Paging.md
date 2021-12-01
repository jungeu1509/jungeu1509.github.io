---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 10-4강"
excerpt: "객체지향 쿼리 언어1 - 기본 문법 - 페이징 API"
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

# 1. 페이징

JPA는 페이징을 다음 두 API로 추상화한다.

- setFirstResult(int startPosition) : 조회 시작 위치(0부터 시작)
- setMaxResults(int maxResult) : 조회할 데이터 수

```java
//페이징 쿼리
String jpql = "select m from Member m order by m.name desc"; 
List<Member> resultList = em.createQuery(jpql, Member.class)
    .setFirstResult(10) 
    .setMaxResults(20) 
    .getResultList();
```

## SQL문들의 방언

페이징이 문법마다 너무 다르고 복잡하다.

JPA에서는 알아서 다 만들어주어 편리하다.

(완전 아트!)

### 참고 : MYSQL 방언

```SQL
SELECT
  M.ID AS ID,
  M.AGE AS AGE,
  M.TEAM_ID AS TEAM_ID,
  M.NAME AS NAME
FROM
  MEMBER M
ORDER BY
  M.NAME DESC LIMIT ?, ?
```