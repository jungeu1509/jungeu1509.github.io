---
layout: single
title:  "SQL SELECT, WHERE, GROUP BY, HAVING"
excerpt: "SQL 문법들 중 SELECT, WHERE, GROUP BY, HAVING 설명"
header:
  teaser: 
search: False
permalink:
categories: 
  - ProgrammersSchool_TIL
tags:
  - coding
  - mysql
  - sql
  - til
  - select
  - groupby
date: 2021-08-12 18:30:00 +0900
last_modified_at: 2021-08-12 18:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. SELECT

SELECT 명령문은 sql의 DML(Data Manipulation Language)이다.

기본 구조는 아래와 같다.

```sql
SELECT * FROM tablename;
```

FROM 뒤에는 사용할 테이블이름을 적고 SELECT 뒤에는 사용할 열(속성명)을 적는다.

명령문이 종료되면 ';'를 적어준다.

예시에서는 '*' 을 적었는데 모든열을 다 선택한다는 뜻이다.

특정 열을 선택할 때는 아래와 같다.

```sql
SELECT Name1, Name2 FROM tablename;
```

쉼표를 이용해 여러개를 불러올 수 있다.

## 1.2. AS를 이용한 별칭

AS 명령어를 이용하여 테이블의 속성을 별칭으로 지정할 수 있다. 

아래와 같이 작성 할 수 있다.

```sql
SELECT
  Name1 AS NewName1,
  Name2 AS NewName2
FROM tablename;
```
Name1을 NewName1로 칭하여 불러오고 Name2를 NewName2로 칭하여 불러온다.
위처럼 줄바꿈을 할 수 있다.

## 1.3. 중복값 제거

SELECT 뒤에 DISTINCT를 붙여주어 중복값을 제거할 수 있다.

```sql
SELECT DISTINCT Name
FROM tablename;
```

Name속성이 중복되는 결과는 출력하지 않고 한번만 선택된다.

## 1.4. ORDER BY

출력시 필드를 오름차순 혹은 내림차순으로 정렬할 수 있다.

```sql
SELECT *
FROM tablename
ORDER BY name ASC;
```

name 항목으로 오름차순 정렬해준다.

내림차순 정렬할때는 ASC 대신 DESC를 쓰면된다.

만약 선택한 속성이 null값일 경우 오름차순일땐 맨 앞에 내림차순일땐 맨 뒤에 위치하게된다.

여러 필드를 이용하여 정렬할 수 있다. 쉼표를 이용하여 나열하면 된다.

---

# 2. WHERE

WHERE는 검색 조건 지정한다. 기본 구조는 다음과 같다.

```sql
SELECT *
FROM tablename
WHERE 조건;
```

WHERE 다음에 조건식을 써주면 된다. 예시는 아래와 같다.

```sql
SELECT *
FROM Reservation
WHERE Name='Gildong Hong';
```

Reservation이라는 표에서 이름 속성이 'Gildong Hong'인 사람만 출력해준다.

## 2.1. 조건문의 연산자

MySQL은 대부분의 프로그래밍 언어에서 지원하는 기본적인 연산자를 모두 제공한다.

이러한 연산자를 사용하여 데이터를 추출하고 처리한다.

연산자의 종류만 간단히 적고 넘어가겠다.

### 2.1.1 산술 연산자

|산술 연산자|설명|
|---|---|
|+|왼쪽 피연산자에 오른쪽 피연산자를 더함.|
|-|왼쪽 피연산자에서 오른쪽 피연산자를 뺌.|
|*|왼쪽 피연산자에 오른쪽의 피연산자를 곱함.|
|/|왼쪽 피연산자를 오른쪽 피연산자로 나눔.|
|DIV|왼쪽 피연산자를 오른쪽 피연산자로 나눈 후, 소수 부분을 버림.|
|%또는 MOD|왼쪽 피연산자를 오른쪽 피연산자로 나눈 후, 그 나머지를 반환함.|

### 2.1.2. 대입 연산자

|산술 연산자|설명|
|---|---|
|=|왼쪽 피연산자에 오른쪽 피연산자를 대입함. (SET 문이나 UPDATE 문의 SET 절에서만 대입연산자로 사용됨)|
|:=|왼쪽 피연산자에 오른쪽 피연산자를 대입함.|

### 2.1.3. 비교 연산자

|비교 연산자|설명|
|--|---|
|=|왼쪽 피연산자와 오른쪽 피연산자가 같으면 참을 반환함.|
|!=, <>|왼쪽 피연산자와 오른쪽 피연산자가 같지 않으면 참을 반환함.|
|<|왼쪽 피연산자가 오른쪽 피연산자보다 작으면 참을 반환함.|
|<=|왼쪽 피연산자가 오른쪽 피연산자보다 작거나 같으면 참을 반환함.|
|>|왼쪽 피연산자가 오른쪽 피연산자보다 크면 참을 반환함.|
|>=|왼쪽 피연산자가 오른쪽 피연산자보다 크거나 같으면 참을 반환함.|
|<=>|양쪽의 피연산자가 모두 NULL이면 참을 반환하고, 하나의 피연산자만 NULL이면 거짓을 반환함.|
|IS|왼쪽 피연산자와 오른쪽 피연산자가 같으면 참을 반환함. (오른쪽 피연산자가 불리언 값인 TRUE, FALSE, UNKNOWN 값일 때 사용함)|
|IS NOT|왼쪽 피연산자와 오른쪽 피연산자가 같지 않으면 참을 반환함. (오른쪽 피연산자가 불리언 값인 TRUE, FALSE, UNKNOWN 값일 때 사용함)|
|IS NULL|피연산자의 값이 NULL이면 참을 반환함.|
|IS NOT NULL|피연산자의 값이 NULL이 아니면 참을 반환함.|
|BETWEEN min AND max|피연산자의 값이 min 값보다 크거나 같고, max 값보다 작거나 같으면 참을 반환함.|
|NOT BETWEEN min AND max|피연산자의 값이 min 값보다 작거나 max 크면 참을 반환함.|
|IN()|피연산자의 값이 인수로 전달받은 리스트에 존재하면 참을 반환함.|
|NOT IN()|피연산자의 값이 인수로 전달받은 리스트에 존재하지 않으면 참을 반환함.|

### 2.1.4. 논리 연산자


|논리 연산자|설명|
|---|---|
|AND|논리식이 모두 참이면 참을 반환함.|
|&&|논리식이 모두 참이면 참을 반환함.|
|OR|논리식 중에서 하나라도 참이면 참을 반환함.|
|&#124;&#124;|논리식 중에서 하나라도 참이면 참을 반환함.|
|XOR|논리식이 서로 다르면 참을 반환함.|
|NOT|논리식의 결과가 참이면 거짓을, 거짓이면 참을 반환함.|
|!|논리식의 결과가 참이면 거짓을, 거짓이면 참을 반환함.|

### 2.1.5. 비트 연산자

|비트 연산자|설명|
|---|---|
|&|대응되는 비트가 모두 1이면 1을 반환함. (AND 연산)|
|&#124;|대응되는 비트 중에서 하나라도 1이면 1을 반환함. (OR 연산)|
|^|대응되는 비트가 서로 다르면 1을 반환함. (XOR 연산)|
|~|비트를 1이면 0으로, 0이면 1로 반전시킴. (NOT 연산)|
|<<|지정한 수만큼 비트를 전부 왼쪽으로 이동시킴. (left shift 연산)|
|>>|부호를 유지하면서 지정한 수만큼 비트를 전부 오른쪽으로 이동시킴. (right shift 연산)|

---

# 3. GROUP BY

선택된 레코드의 집합을 필드의 값이나 표현식에 의해 그룹화한 결과 집합을 반환한다.

SELECT문에서 사용할 수 있다.

## 3.1. 그룹함수들

|함수명|설명|
|---|---|
|COUNT()|선택된 필드에서 특정 조건을 만족하는 레코드의 총 개수를 반환|
|MIN() / MAX()|선택된 필드에서 최대값 혹은 최소값을 반환|
|SUM()|선택된 필드에 저장된 값들의 총 합을 반환|
|AVG()|선택된 필드에 저장된 값들의 평균을 반환|

대부분의 그룹 함수는 NULL값을 제외하고 동작한 결과를 반환한다.

---

# 4. HAVING

HAVING은 SELECT 문의 WHERE처럼 GROUP BY 절에 의해 반환되는 결과 집합의 조건을 설정해준다.

기본 구성은 아래와 같다.

```sql
SELECT
  FieldName,
  GroupFunction(FieldName)
FROM
  tablename
[WHERE 조건]
GROUP BY FieldName
HAVING 조건;
```

---

# 5. 예시 

예약한 사람들의 데이터가 Reservation 테이블에 저장되어있다.

이름, 날짜, 나이를 출력하되 예약팀에서 제일 나이가 많은 사람들을 출력한다.

단, 팀에서 나이가 제일 많은 사람이 15살 넘는 사람만 출력한다.

```sql
SELECT Name, Date, MAX(Age) AS MaxAge
FROM Reservation
GROUP BY Address
HAVING MaxAge > 15;
```

---

# *. 참고자료

- http://tcpschool.com/mysql/DB
- https://gmlwjd9405.github.io/2019/04/25/db-sql-select.html
- https://www.minwookim.kr/how-to-learn-sql-1/

