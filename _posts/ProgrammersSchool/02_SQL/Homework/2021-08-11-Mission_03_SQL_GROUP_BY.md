---
layout: single
title:  "SQL 고득점 Kit 풀기 - GROUP BY"
excerpt: "프로그래머스 코딩테스트 연습 SQL 고득점 Kit 풀기 - GROUP BY"
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

문제링크 : [입양 시각 구하기(2)](https://programmers.co.kr/learn/courses/30/lessons/59413)

## 4.1. 문제풀이 - 1

```sql
SET @count := -1;

SELECT 
    (@count := @count + 1) AS HOUR, 
    (
        SELECT COUNT(1) 
        FROM ANIMAL_OUTS 
        WHERE HOUR(DATETIME) = @count
    ) AS COUNT
FROM ANIMAL_OUTS
WHERE @count < 23;
```

## 4.2. 풀이 - 1

SET을 이용하여 변수를 선언한다.

### 4.2.1. SQL에서의 변수

변수는 세가지 종류가 존재한다.

#### 4.2.1.1. 사용자 정의 변수

```sql
SET @변수이름 = 초기값;
SET @변수이름 := 초기값;
SELECT @변수이름 := 초기값;
```
위 세가지 중에서 하나로 선언 및 초기화를 한다.

저장하는 값에 의해 자료형이 정해지며, Integer, Decimal, Float, Binary 그리고 문자열 타입만 취급한다.

초기화 하지 않을경우 String 타입으로 NULL값이 저장된다.


#### 4.2.1.2. 지역변수

DECLARE로 먼저 선언한 후에 사용하며, 지역변수로 사용하거나 스토어 프로시저의 매개변수로 사용될 수 있다.

지역변수의 범위는 변수가 선언되는 곳의 BEGIN ~ END 블록으로 제한된다.

```sql
BEGIN
DECLARE 변수이름 [, 변수이름] ... type [DEFAULT 초기값]
END;
```

#### 4.2.1.3. 시스템변수

MySQL은 기본적으로 선언된 시스템 변수들이 존재한다. 

시스템변수는 GLOBAL 또는 세션단위로 사용가능하다. GLOBAL

```sql
-- 모든 변수 확인
SHOW VARIABLES;

-- LIKE절의 조건을 포함한 변수 찾기
SHOW GLOBAL VARIABLES LIKE '%st%'
SHOW VARIABLES LIKE '%st%'

-- 특정 변수 확인
SELECT @@sort_buffer_size;

-- GLOBAL 시스템 변수 설정
SET GLOBAL max_connections = 1000;
SET @@GLOBAL.max_connections = 1000;

-- 세션 시스템 변수 설정
SET SESSION sql_mode = 'TRADITIONAL';
SET @@SESSION.sql_mode = 'TRADITIONAL';
SET @@sql_mode = 'TRADITIONAL';

```

### 4.2.2. 참고자료

- https://dev.mysql.com/doc/refman/8.0/en/declare-local-variable.html
- https://dev.mysql.com/doc/refman/8.0/en/using-system-variables.html
- https://velog.io/@inyong_pang/MySQL-MySQL-variables%EB%B3%80%EC%88%98-%EB%A7%8C%EB%93%A4%EC%96%B4%EC%84%9C-%EC%9D%91%EC%9A%A9%ED%95%98%EA%B8%B0getidPK
- https://suhyunsim.github.io/2021-08-10/TIL

## 4.3. 문제풀이 - 2

```sql
WITH RECURSIVE temp AS
(
    SELECT 0 AS HOUR
    UNION ALL
    SELECT HOUR + 1 FROM temp WHERE HOUR < 23
)

SELECT temp.HOUR, COUNT(HOUR(OUTS.DATETIME)) AS COUNT
FROM temp
LEFT JOIN ANIMAL_OUTS as OUTS on temp.HOUR = HOUR(OUTS.DATETIME)
GROUP BY temp.HOUR;
```

## 4.4. 풀이 - 2

WITH RECURSIVE는 재귀 쿼리이다. 부모코드나 자식코드가 존재하는 계층 구조를 이용할때 자주 사용한다. 메모리 상에 가상의 테이블을 저장한다.

temp라는 테이블 이름을 사용할 것이다.

여기서 UNION은 두 개의 SELECT 결과를 합칠 때 사용한다.

두번째 SELECT에서 temp를 다시 불러옴으로써 누적이 된다.

LEFT JOIN을 활용하여 temp와 OUTS를 동시에 비교했다.

### 4.4.1. 참고자료

- https://suhyunsim.github.io/2021-08-10/TIL
- https://dpdpwl.tistory.com/98

