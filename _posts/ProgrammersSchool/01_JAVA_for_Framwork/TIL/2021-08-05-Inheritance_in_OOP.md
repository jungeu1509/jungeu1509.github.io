---
layout: single
title:  "Inheritance in OOP"
excerpt: "객체지향프로그래밍의 상속과 추상화"
header:
  teaser: 
search: false
permalink:
categories: 
  - ProgrammersSchool_TIL
tags:
  - coding
  - oop
  - java
  - til
  - inheritance
date:  2021-08-05 18:30:00 +0900
last_modified_at: 2021-08-05 18:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. 상속

객체지향프로그래밍에서 상속의경우 공통된 기능을 여러 객체에 전달하고 싶을때 사용하는 것이 아니다.

**추상화에서 구체화를 위해 사용한다.**

## 1.1. 추상의 사전적 의미

추상(抽象 - 뺄추/모양상) : 여러가지 사물이나 개념에서 공통되는 특성이나 속성 따위를 추출하여 파악하는 작용이다.

## 1.2. 구체의 사전적 의미

구체(具體 - 갖출구/몸체) : 사물이 직접 경험하거나 지각할 수 있도록 일정한 형태와 성질을 갖춤.


# 2. 추상화와 구체화

- 추상화된 객체 : 추상체
- 구체화된 객체 : 구상체

객체간의 관계에서 상위의 것이 하위보다 추상적이어야 한다.

추상화의 예제는 다음과 같다.

## 2.1. 의미적 추상화

각각 독립적으로 수행가능(둘의 관계는 없다) 하지만 의미만으로 봤을때 추상적일 경우

```java
// 의미적 추상체
class Login {
    void login() {}
}

class KakaoLogin extends Login {
    void login() {}
}
```

## 2.2. 추상기능을 가진 객체

정의만 있는 클래스를 override한다.

```java
// 추상기능을 가진 객체
abstract class Login {
    abstract void login();
}

class KakaoLogin extends Login {
    void kakao() {}
    @Override void login() {}
}
```

## 2.3. 객체 자체가 추상적

객체 자체가 추상적이고 override한다.

```java
interface Login {
    void login();
}

class KakaoLogin implements Login {
    void kakao() {}
    @Override void login() {}
}
```