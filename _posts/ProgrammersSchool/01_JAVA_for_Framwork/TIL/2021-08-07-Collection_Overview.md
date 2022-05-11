---
layout: single
title:  "Collection Overview"
excerpt: "컬렉션 개요"
header:
  teaser: 
search: false
permalink:
categories: 
  - ProgrammersSchool_TIL
tags:
  - coding
  - collection
  - java
  - til
date:  2021-08-07 23:30:00 +0900
last_modified_at: 2021-08-11 13:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. Collection

여러 데이터 묶음을 컬렉션이라고 한다.

컬렉션은 추상체이다.

List와 Set 등이 구현체이다.

종류만 나열하고 추후 하나씩 글을 작성하는 것이 좋을 것 같아 나중에 글을 작성하고 링크를 첨부하겠다.

## 1.1. List

List는 순서가 있는 자료를 관리하기 좋은 구조로 중복데이터를 허용한다.

ArrayList, Vector, LinkedList, Stack, Queue 등이 존재한다.

## 1.2. Set

Set은 순서가 없는 자료를 관리하기 좋은 구조로 중복데이터를 허용하지 않는다.

HashSet, TreeSet 등이 존재한다.

## 1.3. Map

Map은 자료를 쌍(Pair)으로 관리하는데 필요한 메서드가 정의되어있다. (Key - Value 쌍 / Key값은 중복되는 것을 비허용한다.)

검색용 자료구조로 많이 쓰인다.

HashMap, TreeMap 등이 존재한다.

# 2. Iterator

요소를 순회하는 수단을 제공한다.

List의 경우 for 문과 get을 이용하여 데이터를 순회하며 조회가 가능하지만 Set의 경우 불가능하다.

그래서 사용하는것이 Iterator이다. 여러 데이터의 묶음을 풀어서 하나씩 처리할 수 있는 수단을 제공한다.

## 2.1. next()

next()를 통하여 다음 데이터를 조회할 수 있다.

## 2.2. hashnext()

hashnext()는 다음게 있으면 true 반환하며 next() 메서드를 실행한다.

## 2.3. Iterator 예시

아래는 Iterator 사용예시이다.

```java
public class Main {
    public static void main(String[] args) {
        MyIterator<String> iter =
            new MyCollection<String>(Arrays.asList("A", "CA", "DSB", "ASDC", "ASDFE"))
                .iterator(); // Iterator 선언 및 

        while(iter.hasNext()) {
            String s = iter.next();
            int len = s.length();
            if (len % 2 == 0) continue;
            System.out.println(s);
        }
    }
}

public class MyCollection<T> {
    private List<T> list;

    public MyCollection(List<T> list) {
        this.list = list;
    }
    
    public MyIterator<T> iterator() {
        return new MyIterator<T>() {
            private int index = 0;

            @Override
            public boolean hasNext() {
                return index < list.size();
            }

            @Override
            public T next() {
                return list.get(index++);
            }
        };
    }
}

public interface MyIterator<T> {
    boolean hasNext();
    T next();
}
```

# 3. Stream

사실 java배울때 제일 먼저 실행해보는 Stream.in 혹은 Stream.out 도 Stream이다.

자료가 모여있는 배열이나 컬렉션 또는 특정 범위 안에있는 일련의 숫자를 처리하는 기능이다.

배열, 컬렉션 등의 자료를 일관성 있게 처리할 수 있다.

## 3.1. Stream 예시

배열을 선언하여 출력하는 가장 쉬운 예시이다.

```java
int[] arr = {1, 2, 3, 4, 5};
for(int i = 0; i < arr.length; i++) {
  System.out.println(arr[i]);
}
```

위 예시를 Stream을 생성하여 출력하면 아래와 같다.

```java
int[] arr = {1, 2, 3, 4, 5};
Arrays.stream(arr).forEach(n -> System.out.println(n));
```
Arrays.stream(arr) 부분이 stream 생성부분이고
.forEach(n -> System.out.println(n)) 부분이 요소를 하나씩 꺼내어 출력하는 기능이다.

## 3.2. 중간연산

filter(조건문) 조건중 참인것만 출력
map(항목이름) 클래스의 해당 항목만 출력

```java
public class Main {
    public static void main(String[] args) {
        Arrays.asList("A", "AB", "ABC", "ABCD", "ABCDE")
                .stream()
                .map(String::length)
                .filter(i -> i % 2 == 1)
                .forEach(System.out::println);
    }
}
```

```bash
# 출력
1
3
5
```

map을 이용해 length 항목을 추출하도록 했고

filter를 이용해 항목이 홀수인 것만 출력했다.

## 3.3. 스트림 기타 생성

```java
Stream.generate
Stream.iterate
```
위처럼 만들 수 있다.


# *. 참고자료

- 박은종, Do it! 자바 프로그래밍 입문, 이지스퍼블리싱
