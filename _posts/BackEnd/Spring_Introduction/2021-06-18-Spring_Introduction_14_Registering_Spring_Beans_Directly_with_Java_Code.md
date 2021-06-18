---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 14강"
excerpt: "자바 코드로 직접 스프링 빈 등록하기"
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
date:  2021-06-18 21:00:00 +0800
last_modified_at: 2021-06-18 21:00:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

<https://inf.run/hisy> 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 직접 설정파일에 스프링빈에 등록

시작전에 실습을 위해 MemberService와 MemoryMemberRepository에 있는 애노테이션을 지운다.

직접 자바파일을 이용하여 스프링 빈을 등록을 해줄 것이다.

src-main-java-Hello.hellospring  아래에 SpringConfig 클래스를 만든다.

@Configuration을 위에 쓰고 클래스 내부에 @Bean을 입력해 넣는다.

아래와 같이 등록을 해준다.

```java
@Configuration
public class SpringConfig {
    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }
}
```

이때 Controller의 경우 어디에 넣어줄 수 있는 객체가 아니므로

@Controller와 @Autowired는 이전처럼 써줘야한다.

# 2. xml 방법

xml로 작성하는 방법도 있는데 요즘은 잘 사용하지 않으므로 생략

# 3. Dependency injection

## 3.1. 생성자 주입

컨트롤러를 봤을 때 지금 처럼 생성자를 통해 멤버 컨트롤러에 주입되는 것을 생성자 주입이라고 한다.

가장 추천하는 방법이다.

## 3.2. 필드 주입

필드주입은 좋은방법은 아니다

```java
//MemberController.java
@Controller
public class MemberController {

    @Autowired private MemberService memberService;

//    @Autowired
//    public MemberController(MemberService memberServices)
//    {
//        this.memberService = memberServices;
//    }
}
```

중간에 바꿀 수 있는 방법이 아예 없기 때문이다.

## 3.3. setter 주입

```java
//MemberController.java
@Controller
public class MemberController {

    private MemberService memberService;

    @Autowired
    public void setMemberService(MemberService memberService) {
        this.memberService = memberService;
    }

//    @Autowired
//    public MemberController(MemberService memberServices)
//    {
//        this.memberService = memberServices;
//    }
}
```
setter 는 cmd + n 을 눌러서 setter를 찾아 넣으면 자동으로 만들어준다.

이 방법의 단점은 누군가 MemberController를 호출할때 public으로 열려있어야한다.

## 3.4 추천 방법은 생성자 주입

한번 세팅되면 수정할 일이 없는데 public으로 선언되면 위험할 수 있다.

조립시점만 조립을 하고 변경을 못하도록 막기위해 생성자를 사용하는게 좋다.

즉, 의존관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장한다.

# 4. 직접 설정 vs 컴포넌트 스캔(애노테이션) 장단점

컴포넌트 스캔을 사용하면 편하다는 장점이 있지만 구조를 바꿔야 할 경우 많은 코드들을 변경해야하는 단점이 있다.

새로운 구조의 리파지토리만 넣는다고 할때 새로운 리파지토리 클래스를 넣고 기존파일 손댈 필요없이 Config 파일만 바꿔주면 깔끔하게 바꿀 수 있다.

