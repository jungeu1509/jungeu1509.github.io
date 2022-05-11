---
layout: single
title:  "StringBuffer vs. StringBuilder"
excerpt: "StringBuffer와 StringBuilder 차이점"
header:
  teaser: 
search: false
permalink:
categories: 
  - ProgrammersSchool_Homework
tags:
  - coding
  - java
  - programmers_school
date:  2021-08-05 18:00:00 +0900
last_modified_at: 2021-08-05 18:11:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
# 1. String

String 클래스는 Constant라고 생각하면 된다. Constant Pool에 생성한다.

만약 String에 뭔가를 더하게 되면 새로운 메모리 영역을 가리키게 된다.(추후 이전 메모리 영역은 Garbage Collection에 의해 사라진다.)

문자열을 수정하는 시점에 새로운 String instance가 생성되게 된다.

```java
public class StringTest {
    public static void main(String[] args) {
        String out = "";
        for(int i = 0; i < 10 ; i++) {
            out += i; // 이곳을 주의!
        }
        System.out.println(out); //0123456789
    }
}
```

위의 예시에서 a라는 스트링 클래스에 더하게된다면

메모리 영역에는 쓰레기 값으로 "", "0", "01", "012", "0123", ...

매우 많은 값이 추가가 될 것이다.

그래서 문자열을 수정하고 싶을때는 StringBuffer 혹은 StringBuilder를 사용하게 된다.

즉, **String은 immutable(불변) / StringBuffer 혹은 StringBuilder 는 mutable(가변)하다.**

# 2. StringBuffer VS StringBuilder

둘은 동일한 API를 갖고있다.

그러나 가장 큰 차이점은 **동기화의 유무**이다.

StringBuffer의 경우 동기화 키워드를 지원하며 멀티쓰레드에서 안전하다.

StringBuilder는 동기화를 지원하지 않기 때문에 멀티쓰레드 환경에서는 사용하지 않는다.

그러나 단일 쓰레드에서 StringBuilder의 성능이 더 뛰어나다.

StringBuilder를 사용해서 위의 소스코드에 적용하면 다음과 같다.

```java
public class StringTest {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < 10 ; i++) {
            sb.append(i); // StringBuilder 사용
        }
        
        System.out.println(sb); //0123456789
    }
}
```

## 2.1. 사용환경

String  : 문자열 연산 적고, 멀티쓰레드 환경
StringBuffer : 문자열 연산 많고, 멀티쓰레드 환경 
StringBuilder : 문자열 연산 많고, 단일쓰레드 환경(쓰레드 간 동기화 고려하지 않아도 되는 경우)

# 3. 간단 사용법

StringBuffer와 StringBuilder의 API가 동일하다.

```java
public class StringTest {
  public static void main(String[] args){
    String s = "test string";
    StringBuilder sb = new StringBuilder(s); // StringBuffer를 사용하고 싶다면 이곳만 변경!

    System.out.println("문자열 String 변환 : " + sb.toString());  // test string
    System.out.println("문자열 추출 : " + sb.substring(0,4)); //test
    System.out.println("문자열 추가 : " + sb.insert(5,"nice ")); // test nice string
    System.out.println("문자열 삭제 : " + sb.delete(5,10)); // test string
    System.out.println("문자열 연결 : " + sb.append("builder")); //test stringbuilder
    System.out.println("문자열의 길이 : " + sb.length()); // 11
    System.out.println("용량의 크기 : " + sb.capacity()); // 27
    System.out.println("문자열 역순 변경 : " + sb.reverse()); // gnirts tset
    System.out.println("마지막 상태 : " + sb); // gnirts tset
  }
}
```

위 코드 실행시 결과는 아래와 같다

문자열 String 변환 : test string
문자열 추출 : test
문자열 추가 : test nice string
문자열 삭제 : test string
문자열 연결 : test stringbuilder
문자열의 길이 : 18
용량의 크기 : 27
문자열 역순 변경 : redliubgnirts tset
마지막 상태 : redliubgnirts tset

# 참고자료

https://ifuwanna.tistory.com/221
https://jeong-pro.tistory.com/85
https://coding-factory.tistory.com/546