---
layout: single
title:  "SQL 고득점 Kit 풀기 - SELECT"
excerpt: "프로그래머스 코딩테스트 연습 SQL 고득점 Kit 풀기 - SELECT"
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
date: 2021-08-11 19:14:00 +0900
last_modified_at: 2021-08-12 00:33:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. 모든 레코드 조회하기

문제링크 : [모든 레코드 조회하기](https://programmers.co.kr/learn/courses/30/lessons/59034)

## 1.1. 문제풀이

```sql
SELECT *
FROM ANIMAL_INS
ORDER BY ANIMAL_ID ASC;
```

## 1.2. 풀이
1. 모든 레코드 조회를 위해 SELECT * 문을 쓸 수 있는가.
2. 오름차순으로 데이터를 정렬할 수 있는가. 'ORDER BY 속성 ASC'
    - ASC : 오름차순

---

# 2. 역순 정렬하기

문제링크 : [역순 정렬하기](https://programmers.co.kr/learn/courses/30/lessons/59035)
## 2.1. 문제풀이
```sql
SELECT NAME, DATETIME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID DESC;
```

## 2.2. 풀이
1. SELECT문을 사용해 테이블의 원하는 속성만 불러올 수 있는가.
2. 내림차순으로 데이터를 정렬할 수 았는가. 'ORDER BY 속성 DESC'
    - DESC : 내림차순

---

# 3. 아픈 동물 찾기

문제링크 : [아픈 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59036)

## 3.1. 문제풀이

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION LIKE 'Sick'
ORDER BY ANIMAL_ID;
```

## 3.2. 풀이
1. WHERE를 사용하여 조건에 참인 데이터만 가져올 수 있는가.
2. 값을 찾는 조건을 작성할 수 있는가. 'LIKE'
     - LIKE : 같은 것 찾기

---

# 4. 어린 동물 찾기

문제링크 : [어린 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59037)

## 4.1. 문제풀이

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION NOT LIKE 'Aged'
ORDER BY ANIMAL_ID;
```

## 4.2. 풀이
1. WHERE를 사용하여 조건에 참인 데이터만 가져올 수 있는가.
2. 일정값을 제외한 조건을 작성할 수 있는가. (NOT을 쓸 수 있는가.)

---

# 5. 동물의 아이디와 이름

문제링크 : [동물의 아이디와 이름](https://programmers.co.kr/learn/courses/30/lessons/59403)

## 5.1. 문제풀이

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

## 5.2. 풀이
1. 테이블의 원하는 속성만 불러올 수 있는가.
2. 오름차순으로 데이터를 정렬할 수 있는가.

---

# 6. 여러 기준으로 정렬하기

문제링크 : [여러 기준으로 정렬하기](https://programmers.co.kr/learn/courses/30/lessons/59404)

## 6.1. 문제풀이

```sql
SELECT ANIMAL_ID, NAME, DATETIME
FROM ANIMAL_INS
ORDER BY NAME, DATETIME DESC;
```

## 6.2. 풀이
1. 여러 기준으로 정렬할 수 있는가.
2. 기준마다 오름차순, 내림차순을 사용할 수 있는가.

---

# 7. 상위 n개 레코드

문제링크 : [상위 n개 레코드](https://programmers.co.kr/learn/courses/30/lessons/59405)

## 7.1. 문제풀이

```sql
SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME
LIMIT 1;
```

## 7.2. 풀이
1. 제한된 개수만 출력하기 위해 'LIMIT n' 을 쓸 수 있는가.
