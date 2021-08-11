---
layout: single
title:  "SQL 고득점 Kit 풀기 - GROUP BY"
excerpt: "프로그래머스 코딩테스트 연습 SQL 고득점 Kit 풀기 - GROUP BY"
header:
  teaser: 
search: False 
permalink:
categories: 
  - ProgrammersSchool_Homework
tags:
  - coding
  - sql
  - programmers_school
  - programmers
date:  2021-08-11 19:30:00 +0800
last_modified_at: 2021-08-11 19:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. 고양이와 개는 몇 마리 있을까

문제링크 : [고양이와 개는 몇 마리 있을까](https://programmers.co.kr/learn/courses/30/lessons/59040)

## 1.1. 문제풀이

```sql
SELECT 
    ANIMAL_TYPE,
    COUNT(1) AS count
FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE;
```

## 1.2. 풀이

---

# 2. 동명 동물 수 찾기

문제링크 : [동명 동물 수 찾기](https://programmers.co.kr/learn/courses/30/lessons/59041)

## 2.1. 문제풀이

```sql
SELECT
    NAME,
    COUNT(1) AS COUNT
FROM ANIMAL_INS
GROUP BY NAME
HAVING COUNT(NAME) > 1 
ORDER BY NAME;
```

## 2.2. 풀이

---

# 3. 입양 시각 구하기(1)

문제링크 : [입양 시각 구하기(1)](https://programmers.co.kr/learn/courses/30/lessons/59412)

## 3.1. 문제풀이

```sql
SELECT 
    HOUR(DATETIME) AS HOUR, 
    COUNT(1) AS COUNT
FROM ANIMAL_OUTS
WHERE HOUR(DATETIME) >= 9 AND HOUR(DATETIME) < 20
GROUP BY HOUR
ORDER BY HOUR;
```

## 3.2. 풀이

---

# 4. 입양 시각 구하기(2)

문제링크 : [입양 시각 구하기(2)]

## 4.1. 문제풀이 

## 4.2. 풀이