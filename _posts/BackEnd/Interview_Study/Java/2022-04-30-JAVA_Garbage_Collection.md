---
layout: single
title:  "Java Garbage Collection"
excerpt: "면접 준비 스터디"
header:
  teaser: 
search: true
permalink:
categories: 
  - Interview
tags:
  - Interview
  - Java
date:  2022-04-30 13:25:00 +0900
last_modified_at: 2022-04-30 13:25:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. Garbage Collection(가비지컬렉션)이란?

프로그램을 개발하다보면 유효하지 않은 메모리 가비지(Garbage)가 발생한다.

C언어의 경우 메모리 관리를 위해 할당받은 메모리를 직접 해제해주어야한다. (메모리를 해제하지 않아 생기는 버그를 메모리 누수(Membery Leak) 이라고 한다.)
그러나 JAVA의 경우 JVM의 가비지 컬렉터가 불필요한 메모리를 알아서 정리해준다. 

즉, 가비지 컬렉션은 더 이상 사용되지 않는 메모리공간을 JVM이 알아서 회수해주는 동작을 의미한다.

아래 예시를 보자.

```java
class Person {
  String name;
  public Person(String name) {
    this.name = name;
  }
}

Person person = new Person("A"); // A 할당
person = new Person("B"); // A 가비지 발생
```

Person("A")가 처음에 person 변수에 할당 되었지만 후에 Person("B")로 재할당 되면서 Person("A")가 사용되지 않게된다. 즉 가비지가 된다.
이러한 메모리 누수를 방지하기위해 가비지컬렉션이 주기적으로 검사하여 메모리를 정리한다.

# 2. 가비지컬렉션의 동작을 알아보기 전에 알아야할 것들

## 2.1. stop-the-world

JVM이 가비지컬렉션을 실행하기 위해 애플리케이션의 실행을 멈추는 것을 의미한다. 가비지컬렉션 실행을 완료하고 종료하면 중단되었던 어플리케이션을 다시 실행시킨다. stop-the-world가 수 초에 이르기도 하므로 애플리케이션 수행에 문제가 될 수도 있다. Heap의 사이즈가 커지면서 애플리케이션의 지연(Suspend)이 두드러지게 되었다. 이를 위해 다양한 Garbage Collection알고리즘이 생겼다.(참고하기 좋은 게시물 [망나니개발자(MangKyu) 티스토리](https://mangkyu.tistory.com/119)) 

보통의 경우에 GC 튜닝은 stop-the-world의 시간을 줄이는 것을 의미한다.

## 2.2. Reachability

어떤 객체에 유효한 참조가 있으면 Reachable 없으면 unreachable 이라고한다.

보통 unreachable 객체를 가비지라고 간주한다.

java.lang.ref 패키지에서는 더 자세히 구분했는데 더 자세히 알고싶다면 다음 자료를 참고하면 좋다.

[NAVER D2 Hello world](https://d2.naver.com/helloworld/329631)

## 2.3. Mark & Sweep

카비지컬렉션은 스택의 모든 변수 또는 reachable 객체를 스캔하면서 각각 어떤 객체를 참고하고 있는지 탐색한다. 사용되고 있는 메모리를 식별하는데 이러한 과정을 Mark라고한다.
이후에 Mark되지 않은 객체들을 메모리에서 제거하는데 이러한 과정을 Sweep라고한다.

- Mark : 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
- Sweep : Mark 단계에서 사용되지 않는 것으로 식별된 메모리를 해체하는 작업

그러나 모든 메모리를 Mark & Sweep 할 경우 비효율적이므로 다른 방법이 필요했고 그 방법이 Weak generation hypothesis 이다. 

## 2.4. Weak generation hypothesis

가비지컬렉션(Garbage Collection 이하 GC)은 두가지 가설(가정)으로 만들어졌다.
1. 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.
2. 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.

이 가정의 장점을 위해 크게 두가지의 물리적 공간으로 나누었다. Young영역과 Old영역이다.

- Young Generation Area(Young 영역)
  - 새롭게 생성한 객체의 대부분이 위치하는(할당되는) 곳.
  - 대부분의 객체가 금방 접근 불가능 상태가 되므로 많은 객체가 생성되었다 사라진다.
  - 이 영역에서 객체가 사라질때 Minor GC가 발생한다고 부른다.
- Old Generation Area(Old 영역)
  - Young 영역에서 접근 불가능 상태가 되지 않아 살아남은 객체가 복사되어 위치한다. 
  - Young 영역보다 큰 공간에 할당되고 GC가 적게 발생한다.
  - 이 영역에서 객체가 사라질때 Majar GC(혹은 Full GC)가 발생한다고 부른다.

Old 영역이 Young 영역보다 크게 할당되는 이유는 Young 영역의 수명이 짧은 객체들은 큰 공간을 필요로 하지 않기 때문이다.
또한 큰 객체들은 Young영역이 아니라 바로 Old 영역에 할당된다.

초기에는 permanent Generation 영역(혹은 Method Area)도 존재했었지만 Java8부터 제거되었다. 객체나 억류(intern)된 문자열 정보를 저장하는 곳이다.  

# 3. 가비지컬렉션의 동작과정

**가비지컬렉션 동작과정의 큰 흐름은 아래와 같다.**
1. 객체가 새롭게 생성되면 Young 영역에 할당된다.
2. Young 영역에서 unreachable 상태인 메모리를 정리한다.
3. Young 영역에서 reachable 상태를 유지하며 살아남은 객체를 Old 영역으로 이동(Promotion)한다.

Young 영역과 Old 영역의 가비지 컬렉션 과정을 자세히 살펴보자.

## 3.1. Young 영역의 Minor GC

### 3.1.1. Young 영역의 구성

Young 영역은 3개의 영역으로 나뉜다. Eden영역 1개와 Survivor영역 2개이다.

### 3.1.2. Young 영역의 동작

1. 새로 생성된 객체가 Eden 영역에 할당된다.
2. Eden 영역이 꽉차게되면 Minor GC가 실행된다.
   - 사용되지 않은 객체의 메모리는 해제된다.
   - 살아남은 객체는 1개의 Survivor 영역으로 이동된다.
   - 초기상태가 아닌 이미 Survivor 영역에 데이터가 있는 상황이라면 데이터가 존재하는 Survivor 영역(From survivor 영역)으로 이동한다.
3. 1~2번 과정을 반복하다 Survivor 영역이 가득 차게되면 Minor GC를 실행하고 살아남은 객체를 다른 Survivor 영역(To Survivor 영역)으로 이동시키고 데이터를 비운다.(1개의 Survivor 영역은 반드시 빈 상태가 된다.)
4. 1~3번의 과정을 반복하여 계속 살아남은 객체(3.1.3. 참고)는 Old 영역으로 이동한다.

**요약하면 Eden 영역에 최초로 객체가 만들어지고, Survivor 영역을 통해서 Old 영역으로 오래 살아남은 객체가 이동한다.**

Survivor 영역 중 하나는 반드시 비어있는 상태로 남아있어야 한다.(둘다 비어있거나 둘다 사용중이면 비정상적인 상황)

### 3.1.3. Aged object

survivor 영역으로 옮겨진 객체는 age가 Object Header에 기록된다. survivor 영역으로 이동할 때마다 age가 커지게 된다. 일정 수준 이상의 age가 되면 객체를 Old 영역으로 옮긴다. 

## 3.2. Old 영역의 Major GC

Old 영역은 기본적으로 데이터가 가득 차면 GC를 실행한다.

Young 영역은 일반적으로 Old 영역보다 크기가 작기 때문에 GC가 보통 0.5초에서 1초 사이에 끝난다. 그러나 Old 영역은 크기도 크고 Young 영역을 참조할 수도 있기 때문에 일반적으로 시간이 더 오래걸린다.(10배 이상의 시간을 사용할 수도 있다.)

### 3.2.1. Card Table

Old 영역으로 이동한 객체가 Young 영역에 있는 객체를 참조할 수도 있다. 이 경우 Young 영역에 있는 객체만으로는 가비지 여부를 판단할 수 없다. 이때 Weak generation hypothesis에서 두번째 경우에서 적지만 발생했을때를 가정한다.

두 영역을 스캔하는 것을 막기위해 참조 여부를 512바이트 크기의 카드테이블(Card Table)에 기록한다. Minor CG를 진행할 때 카드 테이블에 있는 참조 정보만 확인해서 Young영역 으로의 참조가 있는 Old 영역의 객체만 확인해서 가비지 여부를 판단한다.
발생하는 횟수가 적다고 가정했기 때문에 카드테이블에 기록하는 행위는 부담스러워 지지 않을 것이라 가정한다. 

# 참고자료

- [oracle 공식문서](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
- [Tech Interview(gyoogle.dev)](https://gyoogle.dev/blog/computer-language/Java/Garbage%20Collection.html)
- [NAVER D2 Hello world](https://d2.naver.com/helloworld/1329)
- [NAVER D2 Hello world](https://d2.naver.com/helloworld/329631)
- [데이터 엔지니어링(hbase) 티스토리](https://hbase.tistory.com/209)
- [망나니개발자(MangKyu) 티스토리](https://mangkyu.tistory.com/118)