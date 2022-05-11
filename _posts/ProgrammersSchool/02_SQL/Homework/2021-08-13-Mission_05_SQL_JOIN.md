---
layout: single
title:  "SQL 고득점 Kit 풀기 - JOIN"
excerpt: "프로그래머스 코딩테스트 연습 SQL 고득점 Kit 풀기 - JOIN"
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

# 1. 없어진 기록 찾기

문제링크 : [없어진 기록 찾기](https://programmers.co.kr/learn/courses/30/lessons/59042)

## 1.1. 문제풀이

```sql
SELECT O.ANIMAL_ID, O.NAME
FROM ANIMAL_OUTS AS O
LEFT JOIN ANIMAL_INS AS I
    ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE I.ANIMAL_ID IS NULL;
```

## 1.2. 풀이

1. LEFT JOIN(혹은 RIGHT JOIN) 을 이용해 두개의 테이블을 이용할 수 있다.

---

# 2. 있었는데요 없었습니다

문제링크 : [있었는데요 없었습니다](https://programmers.co.kr/learn/courses/30/lessons/59043)

## 2.1. 문제풀이

```sql
SELECT O.ANIMAL_ID, O.NAME
FROM ANIMAL_INS AS I
LEFT JOIN ANIMAL_OUTS AS O
    ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE I.DATETIME > O.DATETIME
ORDER BY I.DATETIME;
```

---

# 3. 오랜 기간 보호한 동물(1)

문제링크 : [오랜 기간 보호한 동물(1)](https://programmers.co.kr/learn/courses/30/lessons/59044)

## 3.1. 문제풀이

```sql
SELECT I.NAME, I.DATETIME
FROM ANIMAL_INS AS I
LEFT JOIN ANIMAL_OUTS AS O
    ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE O.DATETIME IS NULL
ORDER BY I.DATETIME
LIMIT 3;
```

---

# 4. 보호소에서 중성화한 동물

문제링크 : [보호소에서 중성화한 동물](https://programmers.co.kr/learn/courses/30/lessons/59045)

## 4.1. 문제풀이

```sql
SELECT I.ANIMAL_ID, I.ANIMAL_TYPE, I.NAME
FROM ANIMAL_INS AS I
LEFT JOIN ANIMAL_OUTS AS O
    ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE
    NOT (
        I.SEX_UPON_INTAKE LIKE 'Spayed%' OR
        I.SEX_UPON_INTAKE LIKE 'Neutered%'
    ) AND
    (
        O.SEX_UPON_OUTCOME LIKE 'Spayed%' OR
        O.SEX_UPON_OUTCOME LIKE 'Neutered%' 
    )
ORDER BY I.ANIMAL_ID;
```
