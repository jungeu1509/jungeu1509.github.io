---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 15강"
excerpt: "회원 웹 기능 - 홈 화면 추가"
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
date:  2021-06-21 20:30:00 +0900
last_modified_at: 2021-06-21 20:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

<https://inf.run/hisy> 강의를 수강하고 작성하는 게시물입니다.

---

# 1. Home 컨트롤러 만들기

src - main - java - Hello.hellospring - controller

밑에 HomeController를 만든다.

```java
@Controller
public class HomeController {
    @GetMapping("/")
    public String home(){
        return "home";
    }
}
```

GetMapping에서 "/" 는 'root'(제일 메인 주소)를 의미한다.

# 2. Home html 만들기

src - main - resources - templates

밑에 home.html을 만든다.

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>

<div class="container">
    <div>
        <h1>Hello Spring</h1> 
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a> 
            <a href="/members">회원 목록</a>
        </p> 
    </div>
</div> <!-- /container -->

</body>
</html>
```
간단하게만 설명하면

Hello Spring이라는 제목을 맨 위에 넣고 회원 기능이라는 글씨를 바로 아래 넣는다.

회원 기능이라는 글씨 아래 회원가입 글씨에 하이퍼링크로 /members/new 로 링크, 회원목록 글씨에 하이퍼링크로 /members 로 링크되는 내용이다.

# 3. 빌드와 실행

빌드와 실행후 <http://localhost:8080> 접속해보자.

메인 화면으로 들어오게 되고

회원가입 링크는 /members/new 로, 회원목록 링크는 /members 로 링크된다.

# 3.1 우선순위

여기서 알게되는 점 중의 하나는 웹 페이지 우선순위이다.

이전에 만들었던 index.html 파일을 무시하고 home.html을 띄웠다.

localhost에서 요청이 오면 controller를 먼저 찾아본다. 링크된 홈 화면이 있을경우 링크된 곳으로 가지만 내용이 없으면 정적페이지인 index.html로 연결한다.

