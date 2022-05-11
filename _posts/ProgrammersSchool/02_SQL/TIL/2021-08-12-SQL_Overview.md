---
layout: single
title:  "SQL Overview"
excerpt: "SQL 첫걸음"
header:
  teaser: 
search: false
permalink:
categories: 
  - ProgrammersSchool_TIL
tags:
  - coding
  - mysql
  - sql
  - til
date: 2021-08-12 17:30:00 +0900
last_modified_at: 2021-08-12 17:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. SQL(Structured Query Language)

SQL은 '관계형 데이터베이스'를 관리하는 시스템을(RDBMS, Relational Database Management System) 정의, 조작, 제어할때 사용하는 언어이다.

- 관계형 데이터베이스
  - 행과 열을 가지는 표 형식 데이터를 저장하는 형태의 DB
  - Key와 Value의 관계를 나타낸다.
  - 장점
    - 데이터의 분류, 정렬, 탐색속도가 빠름
    - 높은 신뢰성, 데이터의 무결성
  - 단점
    - 기존 작성된 스키마 수정의 어려움
    - DB의 부하를 분석하기 어려움

## 1.1. SQL 명령의 종류

SQL 구문은 크게 3가지로 구분할 수 있다.

|속성|설명|주요명령어|
|---|---|---|
|DDL(Data Definition Language)|데이터베이스나 테이블 등을 생성, 삭제하거나 그 구조를 변경하기 위한 명령어 |CREATE, ALTER, DROP|
|DML(Data Manipulation Language)|데이터베이스에 저장된 데이터를 처리하거나 조회, 검색하기 위한 명령어|INSERT, UPDATE, DELETE, SELECT 등|
|DCL(Data Control Language)|데이터베이스에 저장된 데이터를 관리하기 위하여 데이터의 보안성 및 무결성 등을 제어하기 위한 명령어|GRANT, REVOKE 등|

# 2. MySQL

MySQL은 가장 널리 사용되고 있는 관계형 데이터베이스 관리 시스템(RDBMS: Relational DBMS)이다.

MySQL은 오픈소스이며 다중 사용자와 다중 스레드를 지원한다.

다양한 운영체제에서 실행 가능하며 여러 프로그래밍 언어를 위한 다양한 API를 제공한다.

크기가 큰 데이터 집합도 아주 빠르고 효과적으로 처리할 수 있다.

MySQL은 키워드와 구문, 문자열은 대소문자를 구분하지 않는다.(하지만 테이블 명과 필드의 이름은 대소문자를 구분한다.)

## 2.1. 주석

```sql
# 한 줄 주석
-- 한 줄 주석
/* 두 줄
    이상의
    주석 */
```

'#', '--', '/* */' 기호를 사용해서 주석을 할 수 있다.

('--' 기호를 사용하는 경우 기호 뒤에 한칸 공백을 주어야한다.)

# *. 참고자료

- http://tcpschool.com/mysql/DB
- https://gmlwjd9405.github.io/2019/04/25/db-sql-select.html
- https://www.minwookim.kr/how-to-learn-sql-1/
