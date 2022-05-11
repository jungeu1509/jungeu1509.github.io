---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 13강"
excerpt: "컴포넌트 스캔과 자동 의존관계 설정"
header:
  teaser: 
search: false
permalink:
categories: 
  - BackEnd
tags:
  - java
  - backend
  - study
date:  2021-06-18 20:20:00 +0900
last_modified_at: 2021-06-18 20:20:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

<https://inf.run/hisy> 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 스트링 빈을 등록하고 의존관계 설정하기

회원 컨트롤러가 회원서비스와 회원 리포지토리를 사용할 수있게 의존관계 만든다.

화면 의존관계를 화면에 출력하려고한다.

스트링빈을 등록하는 방법에는 2가지가 있다.

1. 컴포넌트 스캔과 자동 의존관계 설정
2. 자바 코드로 직접 스프링 빈 등록하기.

# 2. 컴포넌트 스캔과 자동 의존관계 설정

main - java- Hello.hellospring - controller 밑에 MemberController 클래스를 만든다.

클래스 이름 위에 @Controller 를 쓴다.

## 2.1. 애노테이션

스프링 컨테이너가 스프링창에 뜰때 스프링 컨테이너와 스프링통이 생긴다. 거기에 @Controller(컨트롤 에노테이션)가 있으면 맴버 컨트롤러 객체가 생성되고 스프링에 넣어두고, 스프링이 관리한다.

스프링 컨테이너에서 스프링 빈이 관리된다고 한다. 

@Component 애노테이션이 있으면 스프링 빈으로 자동 등록된다.

 - @Controller 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔 때문이다.

@Component를 포함하는 다음 애노테이션도 스프링 빈으로 자동 등록된다

- @Controller
- @Service
- @Repository

## 2.1.1. 애노테이션 주의사항.

아무데서나 에노테이션이 되는건 아니다.

HelloSpringApplication의 main을 따라가기 때문에 Hello.hellospring의 패키지안에서만 자동으로 등록된다.

스프링 빈으로 등록할 때 유일하게 기본으로 하나씩만등록된다(싱글톤으로 동작한다). 

설정으로 싱글톤이 아니게 할 수도 있지만 기본적으로 대부분 싱글톤으로 작성한다.

## 2.2. 각 애노테이션 설정하기

Controller의 경우 MemberController 클래스 위에 다음과 같이 작성한다

```java
@Controller
public class MemberController {
    // 내용은 뒤에 추가 예정
}
```

Service의 경우 MemberService 클래스 위에 작성한다.

```java
// MemberService.java
@Service
public class MemberService {
```

Repository의 경우 MemberRepository 클래스 위에 작성한다.

```java
// MemberRepository.java
@Repository
public class MemoryMemberRepository implements MemberRepository{
```

이 구조가 보통의 스프링 구조라고 생각하면된다.

컨트롤러 - 서비스 - 리파지토리 구조

# 3. 의존관계 생성

지금 빌드 및 실행을 하면 오류가 날것이다.

각 객체들을 스캔할 준비는 되었지만 서로간의 의존관계는 등록이 되지 않았다.

각 선언에서 new를 하면 서로 다른 객체들을 사용하게 되는 것이다.

즉, 잘못하면 repository 3개를 사용하는 꼴이 된다.

이것을 해결하기 위해 @Autowired를 작성해준다.

우선 MemberController 클래스에서 멤버 서비스를 연결해주기 위해 다음과 같이 작성한다.

```java
//MemberController.java
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberServices) {
        this.memberService = memberServices;
    }
}
```

MemberService 클래스에서는 이미 @Autowired를 작성했었다.

```java
// MemberService.java
@Service
public class MemberService {

    private final MemberRepository memberRepository;

    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

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

MemberRepository는 저장소가 되는 부분이므로 new를 해준다.

서로 3개가 의존관계가 된 것이다.

생성자에 @Autowired가 있으면 객체 생성 시점에서 스프링 컨테이너에서 해당 스프링빈을 찾아서 주입한다.

생성자가 1개씩 있다면 생략도 가능하다.
