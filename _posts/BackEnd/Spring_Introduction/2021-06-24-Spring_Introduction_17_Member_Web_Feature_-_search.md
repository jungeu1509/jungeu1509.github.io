---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 17강"
excerpt: "회원 웹 기능 - 조회"
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
date:  2021-06-24 20:30:00 +0900
last_modified_at: 2021-06-24 20:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

<https://inf.run/hisy> 강의를 수강하고 작성하는 게시물입니다.

---

# 1. MemberController 소스 내용 추가

```java
@GetMapping("/members")
    public  String list(Model model) {
        List<Member> members = memberService.findMembers();
        model.addAttribute("members", members);
        return "members/memberList";

    }

```

# 2. MemberList.html 추가

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>

<div class="container">
  <div>
    <table>
      <thead>
      <tr>
        <th>#</th>
        <th>이름</th>
      </tr>
      </thead>
      <tbody>
      <tr th:each="member : ${members}">
        <td th:text="${member.id}"></td>
        <td th:text="${member.name}"></td>
      </tr>
      </tbody>
    </table>
  </div>

</div> <!-- /container -->

</body>
</html>
```

여기서 '$'표시는 파라미터 표시이다. model 안의 값을 꺼내온다.

controller에서 model이라는 곳에 list로 모든 멤버들을 담아놨다.

tr은 타임리프의 문법으로로 무한루프를 돌게된다.

무한루프를 돌때마다 객체를 하나씩 꺼내서 돌게된다.

run 시키고 회원을 두명 등록시키면 회원목록 페이지에서 소스는 다음과 같이 보인다.

```html
<!DOCTYPE HTML>
<html>
<body>

<div class="container">
  <div>
    <table>
      <thead>
      <tr>
        <th>#</th>
        <th>이름</th>
      </tr>
      </thead>
      <tbody>
      <tr>
        <td>1</td>
        <td>spring1</td>
      </tr>
      <tr>
        <td>2</td>
        <td>spring2</td>
      </tr>
      </tbody>
    </table>
  </div>

</div> <!-- /container -->

</body>
</html>
```

무한루프가 2번 돌고 id와 name을 꺼내온것을 볼 수 있다.

getID, getName에 가서 가져온다.

# 3. 메모리

메모리 안에서 서버를 생성하기 때문에 stop하고 run을 하면 회원목록이 지워진다.

그렇기 때문에 DB가 필요하다.