---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 9-4강"
excerpt: "값 타입 - 값 타입의 비교"
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
date:  2021-11-24 23:00:00 +0900
last_modified_at: 2021-11-24 23:00:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 값 타입의 비교

값 타입: 인스턴스가 달라도 그 안에 값이 같으면 같은 것으로 봐야한다.

java에서 비교를 코드를통해 확인해보자.

```java
int a = 10;
int b = 10;
```

위에서 a==b 는 true이다.

```java
Address a = new Address(“서울시”);
Address b = new Address(“서울시”);
```

위에서 a==b 는 false이다. 참조값을 비교하기 때문이다.

## 동일성 비교와 동등성 비교

동일성(identity) 비교 : 인스턴스의 참조 값을 비교. '==' 사용

동등성(equivalence) 비교 : 인스턴스의 값을 비교. 'equals()' 사용

값 타입은 a.equals(b)를 사용해서 동등성 비교를 해야한다.

값 타입의 equals() 메소드를 적절하게 재정의(주로 모든 필드를 사용한다)

재정의는 그때그때 달라지는데 아래처럼 할 수도 있다.

```java
@Embeddable
public class Address {
    private String city;
    
    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        Address address = (Address) o;
        return Objects.equals(city, address.city);
    }

    @Override
    public int hashCode() {
        return Objects.hash(city);
    }
}
```

주의할 점은 equals 구현할때 hashCode도 재정의해야한다.
