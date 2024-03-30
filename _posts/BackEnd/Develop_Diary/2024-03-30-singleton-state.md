---
layout: single
title:  "Singleton 패턴에서 변수는 공유된다."
excerpt: "개발 실수 모음"
header:
  teaser: 
search: false
permalink:
categories: 
  - Backend
tags:
  - Backend
date:  2024-03-30 15:03:00 +0900
last_modified_at: 2024-03-30 15:03:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# Singleton 패턴에서 변수는 공유된다.

## 제작 구조

나는 JAVA와 Spring을 이용하여 개발을 하고있다.
최근 조건에따라 특정한 값을 불러오는 로직을 제작했다. 

다만 확인할 조건이 많고 조건이 추가될 가능성도 높아 조건별로 각 Class로 생성했다.

Service 클래스(Service 어노테이션을 사용한 클래스)에서 체크한 상태를 저장하기 위한 변수 stateDto 라는 이름의 변수를 생성했다.

![Structure](/assets/images/posts/BackEnd/Develop_Diary/01_structure.png)

정리해보면 

1. Controller에서 api를 받으면 
2. Service를 통해 stateDto 변수가 생성되고 
3. A, B, C 클래스에서 각각 조건을 체크하고 
4. stateDto에 각 조건에 대한 결과가 저장된다.

## 문제가 되는 부분

내가 잘못한 부분은 stateDto를 저장한 위치가 잘못되었다.

stateDto를 클래스 변수로 선언한 것이다.

즉 아래와 같이 만들었다.

```java
@Service
public class ServiceClass {

    private StateDto stateDto = new StateDto();

    public void method() {
      AClass a_class = new AClass();
      stateDto.setAClass(a_class.execute());
    }
}
```

### 클래스 변수란?

```java
public class test { 
    int iv; // 인스턴스 변수 static int 
    cv; // 클래스 변수 
    void method() { 
	    int lv; // 지역 변수 
    } 
}
// 출처: https://itmining.tistory.com/20 [에반, 어른반:티스토리]
```

클래스 변수로 선언하면 각 api호출 마다 변수가 각각 생겨 다르게 동작할 것이라 생각했다.

그러나 제일 중요한 Spring의 특징을 망각한 것이다.

### Spring에서 Service는 Singleton Pattern을 사용한다

Spring에서는 컴포넌트 객체를 싱글톤 패턴으로 사용하기 때문에

컴포넌트 객체에서 클래스 변수를 사용한다면 새로 생성되는 것이 아닌 기존의 인스턴스를 사용하여 동작하게 된다.

즉, stateDto 변수가 static 하게 선언되어 여러 사용자가 동시에 api를 쏴서 동작시킨다면 여러 사용자의 데이터가 간섭이 되는 것이다.

## 수정

클래스 변수를 지역변수로 바꾸는 것으로 코드를 수정했다.

```java
@Service
public class ServiceClass {
    public void method() {
      private StateDto stateDto = new StateDto();
      AClass a_class = new AClass();
      stateDto.setAClass(a_class.execute());
    }
}
```

## 고찰

위의 예시는 매우 심플링한 예시이지만

조건의 조건의 조건도 많고 stateDto가 저장해야할 부분도 많았기 때문에 단순히 구현을 한다는 부분에 너무 몰입해있었던것 같다.

특히 디자인 패턴을 적용하여 개발해야한다는 생각에 바보같이 기본적인 내용도 망각하다니...

스프링에 대한 특징이나 java에 대한 특징을 다시 좀 더 공부해야 하겠다.

## 참고자료 

- https://itmining.tistory.com/20 [에반, 어른반:티스토리]
- https://velog.io/@jaeeunxo1/spring-singleton
[lacblueeun, 스프링 핵심원리 - 싱글톤패턴]
