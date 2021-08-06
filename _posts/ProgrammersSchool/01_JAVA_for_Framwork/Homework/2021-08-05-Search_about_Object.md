---
layout: single
title:  "Search about Object Class"
excerpt: "Object 클래스에 관하여"
header:
  teaser: 
search: False
permalink:
categories: 
  - ProgrammersSchool_Homework
tags:
  - coding
  - java
  - programmers_school
date:  2021-08-07 01:02:00 +0800
last_modified_at: 2021-08-07 01:02:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. Object 클래스
java로 코딩하면서 자연스럽게 사용한 String, Integer와 같은 클래스는 import하지 않고도 사용할 수 있었다. 왜 그럴까?

java.lang 패키지에 속해있기 때문인데 java.lang 패키지는 컴파일 할때 자동으로 추가되어 하위 클래스를 모두 사용할 수 있게된다.

Object클래스도 java.lang에 속해있다. 즉 java.lang.Object이다.

Object 클래스는 모든 자바 클래스의 최상위 클래스이다.(컴파일 시 extends Object가 자동으로 붙는다고 보면된다.)

String 클래스 역시 Object클래스를 상속받았다.(Object 메서드를 사용, 재정의, Object형으로 변환할 수도 있다. - 물론 모든 매서드를 재정의 할 수 있는 것은 아니다.)

# 2. 주로 사용되는 Object의 메서드

주로 사용되는 Object 메서드는 아래 표와 같다.

|메서드|설명|
|---|---|
|String toString()|객체를 문자열로 표현하며 반환. 재정의하여 객체에 대한 설명이나 특정 멤버 변수값을 반환한다.|
|boolean equals(Object obj)|두 인스턴스가 동일한지 여부를 반환한다. 재정의하여 논리적으로 동일한 인스턴스임을 정의할 수 있다.|
|int hashCode()|객체의 해시 코드 값을 반환|
|Object clone()|객체를 복제하여 동일한 멤버 변수 값을 가진 새로운 인스턴스를 생성한다.|
|Class getClass()|객체의 Class 클래스를 반환|
|void finalize()|인스턴스가 힙 메모리에서 제거될 때 가비지 컬렉터(GC)에 의해 호출되는 메서드. 네트워크 연결 해제, 열려있는 파일 스트림 해제 등 구현한다.|
|void wait()|멀티스레드 프로그램에서 사용하는 메서드. 스레드를 '기다리는 상태(non runnable)'로 만든다.|
|void notify()|wait() 메서드에 의해 기다리고 있는 스레드(non runnable 상태)를 실행 가능한 상태(runnable)로 가져온다.|

이들 중 위의 세개를 자세히 알아보자.

## 2.1. toString()

toString() 메서드는 인스턴스 정보를 문자열로 반환하는 메서드이다.

```java
Age age = new Age(20); // 구현 생략
System.out.println(age); 
```
위와 같은 출력문에 인스턴스를 바로 넣을경우 해당하는 값이 아닌 아래와 같이 인스턴스 정보값이 나온다.

```bash
Class.Name@hashcode()

# example : object.Age@16f65612
```

클래스이름@해시코드값 이 출력된다. 바로 인스턴스 정보값이다.

(단 String과 Integer 클래스의 경우 인스턴스 정보가 아닌 해당 값이 출력된다. - 클래스에서 이미 재정의 되어있다.)

제대로 출력하려면 아래와 같이 사용해야한다.

```java
Age age = new Age(20); // 구현 생략
System.out.println(age.toString());
```

출력이 정상적으로 데이터 값. 즉, 20이 나온다.

@Override 해서 재정의하여 사용할 수 있다.

## 2.2. equals()

두 인스턴스가 같은지 비교하여 boolean 값을 반환해준다. 

원래의 기능은 두 인스턴스의 주소를 비교하는 것이다. (주소가 같을경우 무조건 같은 값)

그러나 주소가 달라도 같은 값일 수 있다.

String과 Integer클래스에서는 equals가 재정의 되어 논리적으로 같은 인스턴스인지 확인하도록 구현되어있다.(메모리 주소가 다르더라도 같은 값인지)

@Override 해서 같은 인스턴스인지 재정의하여 사용할 수 있다.

## 2.3. hashCode()

정보를 저장하거나 검색할 때 사용하는 자료 구조이다.

정보를 어디에 저장할 것인지, 어디서 가져올 것인지 해시 함수를 이용하여 구현한다.

```java
Age age = new Age(20); // 구현 생략
System.out.println(age); 
```

toString 설명할때 썼던 위의 소스를 다시 가져와봤다. 위의 출력결과는  '클래스이름@해시코드'라고 했다.

출력된 해시코드 다시 말해서 참조 변수를 출력할때 나온 값은 자바 가상 머신이 힙 메모리에 저장한 '인스턴스의 주소 값'이다.

실제로 Object 클래스의 toString() 메서드의 원형을 보면 hashCode()가 사용되었다. 아래 코드는 Object 클래스의 toString() 메서드 원형이다.

```java
getClass().getName() + '@' + Integer.toHexString(hashCode())
```

즉 자바에서는 두 인스턴스가 같다면 hashCode() 메서드에서 반환하는 값이 같아야 한다.

equals() 메서드에서 인스턴스 주소가 같은 경우 외에 주소는 다르지만 논리적으로는 같은 값을 갖는 인스턴스도 존재한다고 하며 재정의 해야한다고 했다.

그러므로 equals를 재정의 했다면 hashCode()도 재정의 해주어야한다.

# *. 참고자료

- 박은종, Do it! 자바 프로그래밍 입문, 이지스퍼블리싱
- https://codingdog.tistory.com/entry/java-toString-%EB%A9%94%EC%84%9C%EB%93%9C-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EC%A0%95%EB%B3%B4%EB%A5%BC-%EC%B6%9C%EB%A0%A5%ED%95%9C%EB%8B%A4
- 
