---
layout: single
title:  "SQL 고득점 Kit 풀기 - IS NULL"
excerpt: "프로그래머스 코딩테스트 연습 SQL 고득점 Kit 풀기 - IS NULL"
header:
  teaser: 
search: false
permalink:
categories: 
  - ProgrammersSchool_Homework
tags:
  - coding
  - sql
  - programmers_school
  - programmers
date: 2021-08-12 19:30:00 +0900
last_modified_at: 2021-08-12 19:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. 이름이 없는 동물의 아이디

문제링크 : [이름이 없는 동물의 아이디](https://programmers.co.kr/learn/courses/30/lessons/59039)

## 1.1. 문제풀이

```sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NULL;
```

## 1.2. 풀이
1. WHERE 조건문을 사용하여 데이터를 선택할 수 있다.
2. IS NULL을 사용하여 해당 속성의 값이 NULL인 것을 찾는다.

---

# 2. 이름이 있는 동물의 아이디

문제링크 : [이름이 있는 동물의 아이디](https://programmers.co.kr/learn/courses/30/lessons/59407)

## 2.1. 문제풀이

```sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NOT NULL;
```

## 2.2. 풀이
1. IS NOT NULL을 사용하여 해당 속성의 값이 NULL이 아닌 것을 찾는다.

---

# 3. NULL 처리하기

문제링크 : [NULL 처리하기](https://programmers.co.kr/learn/courses/30/lessons/59410)

## 3.1. 문제풀이

```sql
SELECT
    ANIMAL_TYPE,
    CASE
        WHEN NAME IS NULL THEN 'No name'
        ELSE NAME
    END NAME,
    SEX_UPON_INTAKE
FROM ANIMAL_INS;
```

## 3.2. 풀이
1. CASE 문을 이용해 흐름 제어할 수 있다. 표현식의 논리값에 따라 다른 값을 반환한다.

