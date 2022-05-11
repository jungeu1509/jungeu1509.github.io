---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 10-9강"
excerpt: "객체지향 쿼리 언어1 - 기본 문법 - JPQL 기본함수"
header:
  teaser: 
search: false
permalink:
categories: 
  - BackEnd
tags:
  - java
  - backend
  - study
date:  2021-12-07 01:00:00 +0900
last_modified_at: 2021-12-07 01:00:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. JPQL 기본 함수

아래는 JPQL 기본 함수이므로 DB에 상관없이 쓸 수 있다.

- CONCAT : 문자 더하기
- SUBSTRING : 문자 범위 지정하여 잘라내기
- TRIM : 공백 제거
- LOWER, UPPER : 대소문자 변경
- LENGTH : 문자의 길이
- LOCATE : 문자에서 부분위치 찾기
- ABS, SQRT, MOD : 수학기능
- SIZE, INDEX(JPA 용도)

## 1.1. CONCAT

```sql
select concat('a', 'b') From Member m
```

근데 Hibernate는 문자를 제공할 때 아래처럼도 쓸 수 있다.

```sql
select 'a' || 'b' From Member m
```

## 1.2. SUBSTRING

```sql
select substring(m.username, 2, 3) From Member m
```

## 1.3. TRIM

LTRIM RTRIM 다 쓸 수 있다.

## 1.4. LOCATE

```sql
select locate('de', 'abcdef') From Member m
```

위처럼 실행하면 4를 반환한다.

## 1.5. SIZE

```sql
select size(t.members) From Team t
```

컬렉션의 크기를 반환해준다.

## 1.6. INDEX

일반적으로 쓸 수 있는 것은 아니고 @OrderColumn을 사용할때 (사용하는것을 추천하지 않는다) List 타입의 컬렉션에서의 값타입일때 옵션을 줘서 쓸 수 있다.

(값타입에서 잠깐 언급했다...)

컬렉션의 위치값을 구할때 쓸 수 있다.

안쓰는 것이 좋다.. 데이터가 빠지면 null로 나오고 그러므로 안쓰는 것이 좋다.

# 2. 사용자 정의 함수

JPQL 기본함수로 할 수 없는경우 사용자 정의 함수를 호출하면 된다.

Application을 개발하다보면 DB의 함수를 불러와야할 때가 있다. 그 함수를 불러올때 사용하는 방법이다.

하이버네이트는 사용전 방언에 추가해야 한다.

사용하는 DB 방언을 상속받고, 사용자 정의 함수를 등록한 다.

방언을 추가하면 아래처럼 쓸 수 있다.

group_concat 이라는 함수를 등록했다고 치고 dilect를 만들어 등록해야한다.

```java
public class MyH2Dialect extends H2Dialect {
  public MyH2Dialect() {
    registerFunction("group_concat", new StandardSQLFunction("group_concat", StandardBasicTypes.STRING));
  }
}
```

등록하는것을 외우는 것은 무리이고 실제 소스코드를 열어서 자세히 나와있는것을 참고하여 만들면된다.

그 후 설정(persistence.xml)에 방언 속성을 수정해준다.

```xml
<property name="hibernate.dialect" value="dialect.MyH2Dialect"/>
```

```sql
select function('group_concat', m.username) from Member m
```

group_concat은 H2가 갖고있는 것이긴 한데 출력된 결과를 한줄의 String으로 나타내주는 함수이다.

참고로 하이버네이트를 사용한다면 직관적으로 아래처럼도 쓸 수 있다.

```sql
select group_concat(m.username) from Member m
```

intelliJ에서는 오류가나올 수 있다.

# 3. 다행이도...?

Dialect에 보면 DB에 해당하는 대부분의 함수가 등록이 되어있다.

