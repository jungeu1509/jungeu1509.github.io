---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 12강"
excerpt: "회원 서비스 테스트"
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
date:  2021-06-17 20:00:00 +0800
last_modified_at: 2021-06-17 20:00:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

<https://inf.run/hisy> 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 테스트 단축키

이전 테스트에서는

test폴더에 패키지를 만들고 해서 했었지만 간단하게 하는 방법이 있다.

클래스에 커서를 두고 cmd+ shift+ t 누르면 create new test 가 뜬다

라이브러리는 JUnit5를 선택하고

테스트를 진행할 메서드를 선택하고 ok를 누르면 넘어간다.

모두 선택하고 넘어간다.

자동으로 테스트코드 템플릿이 작성된것을 확인할 수 있다.

*참고 테스트는 실제코드에 포함되지 않으므로 함수명을 한글로 막적어도 상관없다.

# 1.1. 테스트 작성 tip

테스트는 보통 3가지 단계로 구분할 수 있다.

given / when / then

세가지 분류 주석을 적고 시작하면 편리하다.

# 2. join 테스트

```java
    MemberService memberService = new MemberService();

    @Test
    void join() {
        //given
        Member member = new Member();
        member.setName("hello");

        //when
        Long saveId = memberService.join(member);

        //then
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }
```
assertThat.isEqualTo는 static import 해서 사용했다.(option+enter 사용 : 10강 참고)

## 2.1. 예외처리 검증

회원가입 서비스는 잘 저장되는지도 중요하지만 중복 이름을 제거하는 것이 어찌보면 더 중요할 수 있다.

예외 부분을 테스트하는 검증메서드를 만들어보자.

```java

    @Test
    void 중복_회원_예외() {
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        //when
        memberService.join(member1);
        try {
            memberService.join(member2);
            fail();
        } catch (IllegalStateException e) {
            assertThat(e.getMessage()).isEqualTo("already exist.");
        }
    }
```

try catch 넣으면 코드가 깔끔해 보이지 않는다. 방법을 수정하여 아래와 같이도 쓸 수 있다.

```java
    @Test
    void 중복_회원_예외() {
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        //when
        memberService.join(member1);
        assertThrows(IllegalStateException.class, () -> memberService.join(member2));

    }
```

메시지를 반환받아 확인할 수도 있다. (cmd + option + v)

```java
    @Test
    void 중복_회원_예외() {
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        //when
        memberService.join(member1);
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));

        assertThat(e.getMessage()).isEqualTo("already exist.");
    }
```

## 2.2. 테스트 사이 clear 해주기

현재 member간 이름이 달라서 저장됐지만 다음 테스트에서 동일한 이름을 쓰면 오류가 나므로 clear 해줘야한다.

10강에서 해준것 처럼 해준다.

```java
    MemoryMemberRepository memberRepository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach() {
        memberRepository.clearStore();
    }
```

위의 문장을 추가해준다.(10강 참고)

ctrl + r 누르면 이전에 run했던것을 그대로 run 해준다.

## 2.3. 인스턴스 관리

MemoryMemberRepository() 는 MemberService에서 new로 생성을 했는데 MemberServiceTest 에서 다시 new를 이용해서 생성을 해줬다.

즉 2개를 갖고있는 것인데 테스트진행할때도 원래 갖고있던 인스턴스(리파지토리)로 테스트 하는게 옳다. (다른 db가 될 수 있다.)

같은 인스턴스(리파지토리)를 사용하도록 바꿔야 한다.

MemberService 클래스로 이동하여 new를 삭제하고 아래와 같이 바꿔준다.

cmd + N 을 눌러 constructor 팝업을 띄운다

MemberRepository를 선택하여 내부에서 직접 생성하는게 아닌 외부에서 넣어주게 바꿔준다.

```java
// MemberService.java
public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

```

test에서도 외부에서 넣어주게 바꿔준다.

new를 지우고 @BeforeEach를 이용하여 테스트 시작전에 넣어준다.

```java
// MemberServiceTest.java
class MemberServiceTest {
    MemberService memberService;
    MemoryMemberRepository memberRepository;

    @BeforeEach
    public void beforeEach() {
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }
```
