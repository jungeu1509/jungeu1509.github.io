---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 10-8강"
excerpt: "객체지향 쿼리 언어1 - 기본 문법 - 조건식(CASE 등등)"
header:
  teaser: 
search: False
permalink:
categories: 
  - BackEnd
tags:
  - java
  - backend
  - study
date:  2021-12-07 00:45:00 +0900
last_modified_at: 2021-12-07 00:45:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 조건식 - 기본 CASE 식
```sql
select
    case when m.age <= 10 then '학생요금' 
        when m.age >= 60 then '경로요금'
        else '일반요금'
    end
from Member m
```

# 2. 조건식 - 단순 CASE 식
```sql
select
    case t.name
        when '팀A' then '인센티브110%' 
        when '팀B' then '인센티브120%'
        else '인센티브105%'
    end
from Team t
```

# 3. COALESCE

하나씩 조회해서 null이 아니면 반환

## 3.1. 예시

사용자 이름이 없으면 이름 없는 회원을 반환

```sql
select coalesce(m.username,'이름 없는 회원') from Member m
```

# 4. NULLIF

두 값이 같으면 null 반환, 다르면 첫번째 값 반환

## 4.1. 예시

사용자 이름이 ‘관리자’면 null을 반환하고 나머지는 본인의 이름을 반환

```sql
select NULLIF(m.username, '관리자') from Member m
```
