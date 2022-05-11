---
layout: single
title:  "SQL Insert, Update, Delete"
excerpt: "SQL 문법들 중 Insert, Update, Delete 설명"
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
date: 2021-08-13 16:30:00 +0900
last_modified_at: 2021-08-13 16:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. INSERT

지정한 테이블에 데이터 레이블을 추가하는 것이다.

기본설정은 아래와 같다

```sql
INSERT INTO tablename(lable1, lable2)
VALUES(value1, value2);
```

테이블의 속성에 VALUES에 적은 값을 집어넣는다.

lable은 적어도 안적어도 상관고 무조건 값을 다 채울 없으나 primary key는 꼭 들어가야 한다.

예시

```sql
CREATE TABLE vital (
user_id int not null,
vital_id int primary key,
date timestamp not null,
weight int not null
);

INSERT INTO vital -- 레이블 생략 가능
VALUES(100, 1, '2020-01-01', 75);

INSERT INTO vital(user_id, vital_id, date, weight) 
VALUES(999, 5, '2020-01-02', 10);
```

# 2. Delete

조건을 기반으로 테이블에서 레코드를 삭제 혹은 모든 레코드 삭제할 수 있다.

기본형은 아래와 같다

```sql
DELETE FROM tablename
[WHERE 조건식];
```

WHERE를 안쓰면 모든 레코드를 삭제할 수 있다.

## 2.1. TRUNCATE 명령어 와의 차이점

TRUNCATE 조건없이 모든 레코드 삭제한다. 속도가 빠른 대신 트랜잭션 사용시 롤백 불가능하다.

# 3. Update

조건을 기반으로 테이블에서 특정 레코드(들)의 필드 값을 수정한다.

기본형은 아래와 같다.

```sql
UPDATE tablename
SET lable = value
[WHERE 조건식];
```

아래는 UPDATE 사용 예시이다.

vital_id가 4인 레코드의 weight를 92로 변경하라는 명령어이다.

```sql
UPDATE vital
SET weight=92
WHERE vital_id = 4;
```



---

# *. 참고자료

- 아사이 아츠시 저 박준용 옮김, SQL첫걸음, 한빛미디어
- http://tcpschool.com/mysql/DB

