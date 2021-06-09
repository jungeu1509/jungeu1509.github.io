---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 3강"
excerpt: "Welcome Page 만들기"
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
date:  2021-06-09 20:30:00 +0800
last_modified_at: 2021-06-09 20:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. Welcom 파일생성하기

src - main - resources - static

우클릭하여 new file을 클릭후 index.html을 만든다

html은 프론트엔드 영역!

일단 복붙으로 꾸미는 단계는 넘어간다.

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html"; charset="UTF-8" />
</head>
<body>
Hello
<a href="/hello">hello</a>
</body>
</html>
```

run 한 후 브라우저에서 localhost:8080 주소로 이동하면

(이미 run중이라면 stop하고 다시 run한다.)

Hello글씨가 쓰여진 페이지를 볼 수 있다.

# 2. 필요한것 찾는 방법

https://spring.io 에 들어간다

projects - spring boot 클릭

LEARN을 클릭하고 버전에 맞는 Reference Doc. 에 들어가면 여러가지 레퍼런스들이 존재한다.(메뉴얼)

spring boot features 에 들어가서 index.html로 검색하면 welcome page에 대하여 나온다.

스프링의 범위가 매우 넓으니 메뉴얼에서 검색하는 능력을 길러야 한다.

스프링 공식 튜토리얼도 존재한다.

# 3. 템플릿 엔진

템플릿 엔진을 사용하면 파일을 정적형식(그대로 넘기는 형식)인 html을 더 화려하게 꾸밀 수 있다.

다운받았던 Thymeleaf를 사용할것이다.(3 버전에서 많이 향상됐다.)

http://thymeleaf.org

스프링 부트 메뉴얼에 가면 어떤 템플릿엔진을 사용할 수 있는지도 나와있다.

# 4. 컨트롤러

패키지 만들기

src-main-java-hello.hellospring

controller라는 패키지를 만들었다.

그 패키지 안에 HelloController를 만든다

```java
package Hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController  {

    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "hello!!");
        return "hello";
    }
}
```
src-main -resources - templates 

hello.html을 만든다

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
    <meta http-equiv="content-type" content="text/html; charset=UTP-8"
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
</body>
</html>

``` 

p 태그 뒤의 th는 타임리프의 준말이다

는 data 컨트롤러의 model.addAttribute의 데이터로 치환된다. (hello!! 라는 데이터로 치환된다.)

@GetMapping은 Get 메서드 Post 메서드 에서의 Get메서드로

사용자가 url로 hello라고 주면

서버에서는 hello를 Get하여 model이라는걸 스프링이 넣어준다.

return은 리소스의 템플릿 내부에 있는 html로 넘어간다.

(return hello 라고 치면 템플릿 폴더 내의 hello.html를 찾는다. - templates/hello.html)

컨트롤러에서 리턴 값으로 문자를 반환하면 viewResolver가 화면을 찾아서 처리한다.

resources:templates/ + (ViewName) + .html 을 찾는다.

> spring-boot-devtools라이브러리를 추가하면 html 파일을 컴파일만 하면 서버 재시작 없이 View파일 변경 가능하다.

