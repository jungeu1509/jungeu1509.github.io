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
date:  2021-08-11 19:30:00 +0900
last_modified_at: 2021-08-12 19:30:00 +0900
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
1. SELECT를 사용하여 테이블의 원하는 속성만 불러온다.
2. GROUP BY를 사용하여 원하는 속성을 그룹지을 수 있다.
3. GROUP BY를 이용하여 만든 그룹을 COUNT를 이용하여 갯수를 셀 수 있다.
4. AS를 사용하여 별명을 지어줄 수 있다.

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
1. HAVING 을 사용해서 조건을 걸 수 있다. - HAVING절은 GROUP BY와 같이 사용한다. 집계함수를 가지고 조건비교를 할 때 사용한다.

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
1. WHERE 를 사용하여 조건에 만족하는 것만 출력한다.
2. 시간 관련 함수를 사용할 수 있다.

---

# 4. 입양 시각 구하기(2)

문제링크 : [입양 시각 구하기(2)]

## 4.1. 문제풀이 

## 4.2. 풀이