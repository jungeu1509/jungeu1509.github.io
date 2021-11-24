---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 9-3강"
excerpt: "값 타입 - 값 타입과 불변 객체"
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
date:  2021-11-24 22:50:00 +0900
last_modified_at: 2021-11-24 22:50:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

값 타입은 복잡한 객체 세상을 조금이라도 단순화하려고 만든 개념이다. 따라서 값 타입은 단순하고 안전하게 다룰 수 있어야 한다.

# 1. 값 타입 공유 참조

여러 엔티티들에서 임베디드 타입의 같은 값 타입을 공유하면 위험하다.

회원1과 회원2가 A주소(임베디드타입의 값)를 공유하고 있을때 회원1이 이사를 해서 주소를 바꾸면 회원2도 바뀌어버릴 수 있다.

이런 버그들은 잡기 진짜 힘드므로 부작용이 발생할 수 있다.

그래서 대신 값 (인스턴스)를 복사해서 사용해야한다.

```java
Period period = new Period();

Period copyPeriod = new Period(period.getStartTime(), period.getEndTime());
```

## 객체 타입의 한계

- 항상 값을 복사해서 사용하면 공유 참조로 인해 발생하는 부작용 을 피할 수 있다.
- 문제는 임베디드 타입처럼 **직접 정의한 값 타입은 자바의 기본 타입이 아니라 객체 타입**이다.
- 자바 기본 타입에 값을 대입하면 값을 복사한다.
- **객체 타입은 참조 값을 직접 대입하는 것을 막을 방법이 없다.**
- **객체의 공유 참조는 피할 수 없다.**

# 2. 불변객체

- 객체 타입을 수정할 수 없게 만들면 부작용을 원천 차단
- 값 타입은 불변 객체(immutable object)로 설계해야함.
- 불번 객체 : 생성시점 이후 절대 값을 변경할 수 없는 객체
- 생성자로만 값을 설정하고 수정자(Setter)를 만들지 않으면 된다.
- Integer, String은 자바가 제공하는 대표적인 불변 객체이다.

제약을 만들어 큰 문제를 막을 수 있다.

그러면 만들어진걸 수정할때는...? 아예 새로 생성해서 다시 집어넣는다!(통으로 갈아낀다!)

