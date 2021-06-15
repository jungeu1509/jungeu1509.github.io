---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 9강"
excerpt: "회원 도메인과 리파지토리 만들기"
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
date:  2021-06-15 19:30:00 +0800
last_modified_at: 2021-06-15 19:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/hisy 강의를 수강하고 작성하는 게시물입니다.

---
# 1. 회원

src-main-java-Hello.hellospring 에다가 domain 패키지를 만든다.

domain패키지 안에 Member 라는 클래스를 만든다.

id와 name 변수를 만들고

두개 모두 getter setter를 만든다(cmd + N 누르고 나온 팝업에서 getter and setter를 선택한다. 그 후 나온 팝업에서 모든 변수를 선택해준다.)

다음과 같이 만들어진다.

```java
package Hello.hellospring.domain;

public class Member {
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

# 2. 리파지토리
domain과 같은 위치에 repository 패키지를 만든다.

회원 객체를 저장하는 저장소 역할이다

repository 패키지 밑에 MemberRepository 인터페이스(클래스를 생성하고 이름쓰는칸 밑의 Interface를 누른다)를 만든다.

아래처럼 4가지 리파지토리의 기능(저장, 이름찾기, 아이디찾기, 모든 목록반환)을 만든다

```java
package Hello.hellospring.repository;

import Hello.hellospring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepository {
    Member save(Member member);
    Optional<Member> findById(Long id);
    Optional<Member> findByName(String name);
    List<Member> findAll();
}
```

## 2.1 리파지토리 구현체
이제 구현체를 만든다.

리퍼지토리 패키지 밑에 MemoryMemberRepository를 클래스로 만든다.

클래스 이름 오른쪽에 implements MemberRepository 를 입력하고

맥의 option + Enter 를 하면 팝업스크롤이 뜨는데 implements methods를 누르고 모든항목이 선택된 상태에서 OK를 하면 자동으로 implements가 된다.

## 2.1.1 save

어딘가에 저장을 해야 하므로 Map을 써서 저장한다.
```java
private static Map<Long, Member>
```
위를 입력하고 ctrl+space나 option+Enter해서 import한다.

(실무에서 동시성 문제가 발생할 수 있기 때문에 공유되는 변수일때는 컨커르해시맵(?)을 써야하는데 예제이므로 단순하게 사용)

시퀀스를 만든다 (키값을 생성해주는 애) - 실무에서는 long  보다는 어텀렁(?)을 쓴다.

```java
package Hello.hellospring.repository;

import Hello.hellospring.domain.Member;

import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Optional;

public class MemoryMemberRepository implements MemberRepository{

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        return null;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.empty();
    }

    @Override
    public Optional<Member> findByName(String name) {
        return Optional.empty();
    }

    @Override
    public List<Member> findAll() {
        return null;
    }
}
```
지금까지 이것과 같다.

이제 save내부를 채워보자.

```java
    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }
```

## 2.1.2 findById
id를 검색해서 빼주면 되는데

null이 리턴될 수 있으므로 Optional으로 감싸준다.(요즘 스타일)

```java
    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }
```

감싸서 반환하면 클라이언트에서 후처리를 할 수 있다.

## 2.1.3 findByName

java8의 람다를 사용한다.

```java
    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }
```

filter 를 통해 받은 변수와 멤버 이름이 같은것을 검색한다.
findany는 모든 값을 검색하며 돌게한다.(하나 찾으면 끝남)
못찾으면 null

## 2.1.4 findAll

저장은 Map을 썼는데 반환은 List로 되어있다. 자바 실무에서 list많이 쓴다.

```java
    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }
```

# 3. 총 작성 코드

디렉토리 구조는 아래 사진과 같다.

<img src="/assets/images/posts/BackEnd/Spring_Introduction/09/directory_screenshot.png" title="내부 디렉토리 구조" alt="내부 디렉터리 구조"></img><br/>

작성한 총 레파지토리 패키지의 MemoryMemberRepository.java 코드는 아래와 같다.

```java
package Hello.hellospring.repository;

import Hello.hellospring.domain.Member;

import java.util.*;

public class MemoryMemberRepository implements MemberRepository{

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }
}
```

