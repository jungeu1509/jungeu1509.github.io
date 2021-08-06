---
layout: single
title:  "Interface Overview"
excerpt: "인터페이스 개요"
header:
  teaser: 
search: False
permalink:
categories: 
  - ProgrammersSchool_TIL
tags:
  - coding
  - interface
  - java
  - til
  - inheritance
date:  2021-08-06 23:30:00 +0800
last_modified_at: 2021-08-06 23:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. Interface 인터페이스

클래스 혹은 프로그램이 제공하는 기능을 명시적으로 선언하는 역할

interface 키워드를 통해 선언 가능하다.

interface의 기능은 다음과 같다.

- 구현 강제
- 다형성 제공
  - 인터페이스만 교체해주면 쉽게 호환이 될 수 있다.
- 결합도를 낮춤(의존성 역전)
  - 결합도란 하나의 클래스가 다른 클래스와 얼마나 많이 연결되어있는지 나타내는 표현

인터페이스 에서 선언한 메서드는 컴파일러가 자동으로 추상메서드로 변경시킨다.
인터페이스 에서 선언한 변수의 경우 값이 변하지 않는 상수가 된다.

static 메서드를 사용할 수 있는데 함수 제공역할로 사용할 수 있다

## 1.1. 결합도를 낮추는 예시

사이트에서 사용자 로그인 서비스(이하 US)를 제공하려고 한다.

Naver로그인(이하 N)을 이용하거나 Kakao로그인(이하 K)을 이용할 수 있게 만들었다.

US가 K와 N을 의존한다.

![Interface 결합도 Before](/assets/images/posts/ProgrammersSchool/TIL/Interface_001.jpg "interface Dependency Injection Before")

US 와 K, N 사이의 의존성을 외부에 맡기면 여러 기능을 수행할 수 있다. 즉, 의존도를 낮춘다.(의존성을 주입받았다. Dependency Injection, DI)

Login이란 인터페이스를 삽입한다.

US 는 Login을 의존하고 K와 N은 Login을 상속받게된다.

![Interface 결합도 After](/assets/images/posts/ProgrammersSchool/TIL/Interface_002.jpg "interface Dependency Injection After")

# 2. Interface method

## 2.1. Default method (기본 메서드)

인터페이스에서 구현하지만, 이후 인터페이스를 구현한 클래스가 생성되면 그 클래스에서 사용할 기본 기능이다.

즉, 기본으로 제공되는 메서드를 의미한다.

인터페이스를 구현한 추상클래스 혹은 추상클래스를 상속받은 클래스에서는 코드를 구현할 필요는 없다.

그러나 하위클래스에서 override해서 구현해서 사용할 수 있다.

## 2.2. Static method (정적 메서드)

클래스의 생성과 무관하게 사용할수 있다. 정적 메서드를 사용할 때는 인터페이스 이름으로 직접 참조하여 사용한다.

인터페이스에서 정의된것으로 사용하는데 함수 제공자로 사용할 수 있다.

# 3. Functional Interface

추상메서드가 하나만 존재하는 인터페이스이다. SAM(Single Abstract Method)라고도 불린다.

(Default 혹은 Static 메서드는 존재하지 않거나 여러개 있어도 상관없다.)

@FunctionalInterface를 붙여준다

# 3.1. 기본 함수형 인터페이스

자바에서 기본적으로 제공하는 함수형 인터페이스들이 있다.

java.util.function 페키지에 정의되어 있다.

몇가지 예시는 아래와 같다.

- Runnable (인자를 받지 않고 리턴값도 없다)
- Supplier<T> (인자를 받지 않고 T타입 객체 리턴)
- Consumer<T> (T타입 객체를 인자로 받고 리턴값이 없다.)
- Function<T, R> (T타입 객체를 인자로 받고 R타입 객체 리턴)
- Predicate<T> (T타입 객체를 인자로 받고 boolean을 리턴)

# *. 참고자료

- 박은종, Do it! 자바 프로그래밍 입문, 이지스퍼블리싱
- https://codechacha.com/ko/java8-functional-interface/
- https://limkydev.tistory.com/197