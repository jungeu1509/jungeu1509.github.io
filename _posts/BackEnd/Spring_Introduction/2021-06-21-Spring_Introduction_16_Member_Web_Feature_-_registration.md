---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 16강"
excerpt: "회원 웹 기능 - 등록"
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
date:  2021-06-21 21:30:00 +0800
last_modified_at: 2021-06-21 21:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

<https://inf.run/hisy> 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 회원 가입 서비스 매핑하기

src - main - java - Hello.hellospring - controller - MemberController.java

기존의 작성해놨던 기능 밑에 연결 코드를 추가한다.

```java
@Controller
public class MemberController {

    private MemberService memberService;

    @Autowired
    public void setMemberService(MemberService memberService) {
        this.memberService = memberService;
    }
    @GetMapping("/members/new")
    public String createForm(){
        return "members/creatMemberForm";
    }
}
```

/members/new 링크된 곳과 매핑을 해주고 반환하는 주소를 적어준다.

# 2. 회원가입 화면

같은것 끼리 모여있는것이 관리하기도 좋으므로 resources - templates 폴더 아래 members 폴더를 만들어 준다.

폴더 안에 반환하는 주소와 동일하게 html을 만들어준다.

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">

<body>
<div class="container">
  <form action="/members/new" method="post">
    <div class="form-group">
      <label for="name">이름</label>
      <input type="text" id="name" name="name" placeholder="이름을 입력하세요">
    </div>
    <button type="submit">등록</button>
  </form>
</div> <!-- /container -->

</body>
</html>
```

빌드를 해서 run을 하면 화면이 생긴것을 알 수 있다.

근데 등록을 해도 다음 화면으로 넘어가지 않는 껍데기 화면이다.

# 3. 회원등록 컨트롤러

controller 파일 아래에 MemberForm 객체를 만든다.

private String name; 선언하고 getter setter를 만든다.

name 이 같으므로 createMemberForm.html 의 name과 매칭될 것이다.

```java
package Hello.hellospring.controller;

public class MemberForm {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

MemberController 객체 소스로 이동하여 아래처럼 PostMapping을 추가한다.

```java
@Controller
public class MemberController {

    private MemberService memberService;

    @Autowired
    public void setMemberService(MemberService memberService) {
        this.memberService = memberService;
    }
    @GetMapping("/members/new")
    public String createForm(){
        return "members/creatMemberForm";
    }

    @PostMapping("/members/new")
    public String create(MemberForm form) {
        Member member = new Member();
        member.setName(form.getName());

        memberService.join(member);

        return "redirect:/";
    }
}
```

우리가 만들었던 memberService와 결합시켜주는 부분이다.

재빌드 및 실행하고 localhost에서 이름을 입력하고 등록하면 메인화면으로 돌아온다.

저장이 된게 맞지만 다른 컨트롤러를 조합해야 된다.

# 4. 원리 간단 설명

localhost에서 회원가입버튼을 누르면 GET방식을 (주소를 통해 접속) 통해서 /members/new에 접속하게 된다.

@GetMapping("/members/new") 가 써있는

MemberController의 createForm을 거치게 된다.

내용 아무것도 없이 반환만 하므로 members/createMemberForm으로 이동한다.

members/createMemberForm를 템플릿에서 찾는다. 뷰 리절버라는 애가 찾아 선택한다. 타임리프 템플릿 엔지니어를 랜더링 한다.

그 후 HTML을 뿌려진다.

createMemberForm에서 '<form>' 태그를 입력하는 것은 값을 입력할 수 있는 html 태그이다.

input 태그에서 id는 레이블을 연결해주는 것이고 name은 input데이터가 서버로 넘어갈때 키값이 된다.

만약 localhost에서 이름을 입력하고 등록버튼을 누르면 form태그의 action과 method를 확인한다. 

입력한 데이터가 /members/new에 post방식으로 넘어온다.

MemberController에서 @PostMapping한 이유가 이것이다.(@GetMapping은 조회할때 보통 사용한다.)

즉, 등록을 하면 createForm이 아닌 create가 선택된다.

MemberForm에 String 형식의 이름이 setName을 통해 들어온다.

만약 잘 들어왔는지 확인하려면 

```java
System.out.println("member = " + member.getName());
```

위 코드를 return 사이에 입력하고 localhost에서 이름 등록을 하면 등록된 이름을 IntelliJ 터미널에서 보여줄 것이다.