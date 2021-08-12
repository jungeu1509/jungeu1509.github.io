---
layout: single
title:  "SQL 고득점 Kit 풀기 - SUM, MAX, MIN"
excerpt: "프로그래머스 코딩테스트 연습 SQL 고득점 Kit 풀기 - SUM, MAX, MIN"
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
date: 2021-08-12 19:20:00 +0900
last_modified_at: 2021-08-12 19:20:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. 최댓값 구하기

문제링크 : [최댓값 구하기](https://programmers.co.kr/learn/courses/30/lessons/59415)

## 1.1. 문제풀이

```sql
SELECT MAX(DATETIME) AS 시간
FROM ANIMAL_INS;
```

## 1.2. 풀이
1. MAX를 사용하여 최댓값을 사용할 수 있다.

---

# 2. 최솟값 구하기

문제링크 : [최솟값 구하기](https://programmers.co.kr/learn/courses/30/lessons/59038)

## 2.1. 문제풀이

```sql
SELECT MIN(DATETIME) AS 시간
FROM ANIMAL_INS;
```

## 2.2. 풀이
1. MIN을 사용하여 최솟값을 사용할 수 있다.

---

# 3. 동물 수 구하기

문제링크 : [동물 수 구하기](https://programmers.co.kr/learn/courses/30/lessons/59406)

## 3.1. 문제풀이

```sql
SELECT COUNT(1) AS count
FROM ANIMAL_INS;
```

## 3.2. 풀이

1. COUNT를 사용하여 수를 셀 수 있다.

---

# 4. 중복 제거하기

문제링크 : [중복 제거하기](https://programmers.co.kr/learn/courses/30/lessons/59408)

## 4.1. 문제풀이

```sql
SELECT COUNT(DISTINCT NAME) AS count
FROM ANIMAL_INS;
```

## 4.2. 풀이
1. COUNT를 사용하여 수를 셀 수 있다.
2. DISTINCT를 사용하여 중복되지 않는값을 선택할 수 있다.
