---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 11강"
excerpt: "회원 서비스 개발"
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
date:  2021-06-17 19:30:00 +0800
last_modified_at: 2021-06-17 19:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

<https://inf.run/hisy> 강의를 수강하고 작성하는 게시물입니다.

---

실제 비즈니스 서비스를 만드는 부분이다.

# 1. 회원가입 서비스

main - java - hello.hellospring 밑에 service 패키지를 만든다.

service 밑에 MemberService 라는 클래스를 만든다.

cmd + option + v 바로 return 받는 명령어이다.

회원가입 서비스를 먼저 만든다.

```java
package Hello.hellospring.service;

import Hello.hellospring.domain.Member;
import Hello.hellospring.repository.MemberRepository;
import Hello.hellospring.repository.MemoryMemberRepository;

import java.util.Optional;

public class MemberService {
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    /*
     * 회원가입
     */
    public Long join(Member member){
        //같은 이름이 있는 중복회원 X
        Optional<Member> result = memberRepository.findByName(member.getName());
        result.ifPresent(m -> {
            throw new IllegalStateException("already exist.");
        });

        memberRepository.save(member);
        return member.getId();
    }

}
```

result.ifPresent 를 사용해도 되지만 result.orElseGet 도 많이 사용한다.(바로 get은 잘 안쓴다.)

result로 return 을 받는 방법도 있지만 코드가 길어지기 때문에 아래처럼 바로 ifPresent를 쓸 수도 있다.

```java
package Hello.hellospring.service;

import Hello.hellospring.domain.Member;
import Hello.hellospring.repository.MemberRepository;
import Hello.hellospring.repository.MemoryMemberRepository;

import java.util.Optional;

public class MemberService {
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    /*
     * 회원가입
     */
    public Long join(Member member){
        //같은 이름이 있는 중복회원 X
        memberRepository.findByName(member.getName())
                .ifPresent(m -> {
                    throw new IllegalStateException("already exist.");
                });

        memberRepository.save(member);
        return member.getId();
    }

}
```

memberRepository.findByName() 부터 4줄을 블럭지정하고 ctrl + T 를 누르고 extract method 를 선택하면

메서드를 외부로 추출할 수 있다.

이름을 validateDuplicateMember 라고 지어주면 다음과 같이 된다.

```java
package Hello.hellospring.service;

import Hello.hellospring.domain.Member;
import Hello.hellospring.repository.MemberRepository;
import Hello.hellospring.repository.MemoryMemberRepository;

import java.util.Optional;

public class MemberService {
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    /*
     * 회원가입
     */
    public Long join(Member member){
        //같은 이름이 있는 중복회원 X
        validateDuplicateMember(member); // 중복 회원 검증

        memberRepository.save(member);
        return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                .ifPresent(m -> {
                    throw new IllegalStateException("already exist.");
                });
    }

}
```
# 2. 전체 맴버 조회 서비스
```java
    /**
     * 전체 회원 서비스 조회
     */
    private List<Member> findMembers() {
        return memberRepository.findAll();
    }
```

# 3. Id 조회

```java
    public  Optional<Member> findOne(Long memberId) {
        return memberRepository.findById(memberId);
    }
```

# 4. 총 코드

```java
package Hello.hellospring.service;

import Hello.hellospring.domain.Member;
import Hello.hellospring.repository.MemberRepository;
import Hello.hellospring.repository.MemoryMemberRepository;

import java.util.List;
import java.util.Optional;

public class MemberService {
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    /*
     * 회원가입
     */
    public Long join(Member member){
        //같은 이름이 있는 중복회원 X
        validateDuplicateMember(member); // 중복 회원 검증

        memberRepository.save(member);
        return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                .ifPresent(m -> {
                    throw new IllegalStateException("already exist.");
                });
    }

    /**
     * 전체 회원 서비스 조회
     */
    private List<Member> findMembers() {
        return memberRepository.findAll();
    }

    public  Optional<Member> findOne(Long memberId) {
        return memberRepository.findById(memberId);
    }

}
```