---
layout: single
title:  "SQL 고득점 Kit 풀기 - String, Date"
excerpt: "프로그래머스 코딩테스트 연습 SQL 고득점 Kit 풀기 - String, Date"
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
date: 2021-08-13 19:30:00 +0900
last_modified_at: 2021-08-13 19:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. 루시와 엘라 찾기

문제링크 : [루시와 엘라 찾기](https://programmers.co.kr/learn/courses/30/lessons/59046)

## 1.1. 문제풀이

```sql
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
WHERE 
    NAME LIKE 'Lucy' OR
    NAME LIKE 'Ella' OR
    NAME LIKE 'Pickle' OR
    NAME LIKE 'Rogan' OR
    NAME LIKE 'Sabrina' OR
    NAME LIKE 'Mitty';
```

## 1.2. 풀이
1. WHERE 조건문을 사용하여 데이터를 선택할 수 있다.
2. LIKE 를 사용하여 문자열을 비교할 수 있다.

---

# 2. 이름에 el이 들어가는 동물 찾기

문제링크 : [이름에 el이 들어가는 동물 찾기](https://programmers.co.kr/learn/courses/30/lessons/59047)

## 2.1. 문제풀이 

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE
    ANIMAL_TYPE LIKE 'Dog' AND
    (
        NAME LIKE '%el%' OR
        NAME LIKE '%El%'
    )
ORDER BY NAME;
```

## 2.2. 풀이
1. LIKE에서 %를 사용하여 무엇이든 와도 상관없다는 의미를 사용한다.

---

# 3. 중성화 여부 파악하기

문제링크 : [중성화 여부 파악하기](https://programmers.co.kr/learn/courses/30/lessons/59409)


## 3.1. 문제풀이

```sql
SELECT ANIMAL_ID, NAME, 
    CASE
        WHEN SEX_UPON_INTAKE LIKE '%Neutered%'
            OR SEX_UPON_INTAKE LIKE '%Spayed%' THEN 'O'
        ELSE 'X'
    END AS 중성화
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

## 3.2. 풀이
1. CASE 문을 이용하여 조건에 따라 출력을 바꾼다.

---

# 4. 오랜 기간 보호한 동물(2)

문제링크 : [오랜 기간 보호한 동물(2)](https://programmers.co.kr/learn/courses/30/lessons/59411)

## 4.1. 문제풀이

```sql
SELECT I.ANIMAL_ID, I.NAME
FROM ANIMAL_INS AS I
LEFT JOIN ANIMAL_OUTS AS O
    ON I.ANIMAL_ID = O.ANIMAL_ID
ORDER BY O.DATETIME - I.DATETIME DESC
LIMIT 2;
```

## 4.2. 풀이
1. 시간타입을 사칙연산 할 수 있다.

---

# 5. DATETIME에서 DATE로 형 변환

문제링크 : [DATETIME에서 DATE로 형 변환](https://programmers.co.kr/learn/courses/30/lessons/59414)'

## 5.1. 문제풀이

```sql
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME,'%Y-%m-%d') AS 날짜
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

## 5.2. 풀이
1. DATE_FORMAT 을 쓸 수 있다.
