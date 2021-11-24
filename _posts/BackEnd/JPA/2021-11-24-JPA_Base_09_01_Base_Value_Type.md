---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 9-1강"
excerpt: "값 타입 - 기본값 타입"
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
date:  2021-11-24 22:00:00 +0900
last_modified_at: 2021-11-24 22:00:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 값타입

단순히 값으로 사용하는 자바 기본 타입이나 객체를 값타입이라고 한다.

식별자가 없고 값만 있으므로 변경시 추적이 불가능하다.

예) 숫자 100을 200으로 변경하면 완전히 다른값으로 대체된다



- 기본값 타입
  - 자바 기본 타입(int, double)
  - 래퍼 클래스(Integer, Long)
  - String
- 임베디드 타입(embedded type, 복합 값 타입)
- 컬렉션 값 타입(collection value type)

임베디드나 컬렉션은 JPA에서 정의를 해서 사용해야한다.

임베디드의 예시는 x, y 좌표를 이용하여 커스텀하여 사용하는 것.

자바 컬렉션에다 기본값이나 임베디드를 넣는것을 컬렉션이라고 한다.

# 2. 기본값 타입

생명주기를 엔티티에 의존한다. - 회원을 삭제하면 이름, 나이도 삭제된다.

값타입은 공유하면 안된다. - 회원이름 변경할때 다른 회원의 이름도 함께 변경되면 안된다.

## 참고

참고로 자바의 기본타입은 절대 공유하지 않는다.

```java
int a = 10;
int b = a;

a = 20;
```

b는 10이지 20이 아니다.

이처럼 int, double 같은 기본타입은 절대 공유하지 않는다.

그러나 Integer같은 래퍼 클래스나 String 같은 특수한 클래스는 공유가능한 객체이지만 변경되지 않는다.

```java
SpecialValue a = new SpecialValue(10);
SpecialValue b = a;

a.setValue(20);
```

위 경우 a, b 둘다 20이 출력된다. b에 레퍼런스가 넘어가서 같은 값을 공유하기 때문이다. 값을 바꾸는 것 자체가 불가능하다.

자바에서는 안전하게 사이드 이펙트가 발생하지 않도록 묶어버린다.
