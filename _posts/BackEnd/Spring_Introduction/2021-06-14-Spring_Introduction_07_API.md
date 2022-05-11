---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 7강"
excerpt: "API"
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
date:  2021-06-14 21:00:00 +0900
last_modified_at: 2021-06-14 21:00:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/hisy 강의를 수강하고 작성하는 게시물입니다.

---
# 1. API

## 1.1 API 예제

```java
package Hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.stereotype.Repository;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController  {

    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "hello!!");
        return "hello";
    }

    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam(name = "name", required = false) String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }

    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
}

```

중간에 @ResponseBody의 의미는

http의 body부를 직접 넣어주겠다는 의미이다.

name 에 문자를 넣어주면 

뷰가 없고 문자가 그대로 내려간다.

http://localhost:8080/hello-string?name=spring

위의 주소로 들어가면 똑같은 결과가 보여지는것을 알 수 있지만

소스보기를 하면 html과 다르게 문자만 그대로 나와있는것을 알 수 있다.

## 1.2. API를 이용하여 데이터를 넘기는 방식 예제(json)

```java
package Hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.stereotype.Repository;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController  {

    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "hello!!");
        return "hello";
    }

    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam(name = "name", required = false) String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }

    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }

    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }

    static class Hello {
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
}

```
위의 내용을 추가하고 실행해보자.

http://localhost:8080/hello-api?name=spring

json방식의 데이터 구조가 나온다(키,값으로 이루어진 구조이다. - 키는 name 값은 spring이다.)

과거에는 xml방식으로 많이 쓰였다.(html이 xml구조로 되어있다.)

그러나 무겁고 괄호 열고 닫고 해야하므로 거의 json방식으로 통일되었다.

### *. 참고 : getter and setter

java bin 표준 방식이다.

라이브러리에서 쓰거나 객체 초기화하는 역할이고

마음대로 호출 불가능하게 하기 위한 역할이다.

property접근방식이라고도 한다.

## 1.3. @ResponseBody 대략적 사용원리

1. 웹 브라우저에서 주소를 던진다
2. 스프링 부트의 내장 톰캣서버에서 주소를 받고 스프링 으로 넘긴다.
3. 스프링은 컨테이너에 있는것을 확인한다.
4. @ResponseBody가 있는 것을 발견하면 HTTP BODY에 내용을 직접 반환한다.
   1. @ResponseBody가 없을경우 viewRespose에 그냥 던진다.(이전의 강의 및 게시물 참고)
   2. 문자일 경우 HttpMessegeConverter에 문자를 그대로 넘긴다 - StringConverter가 동작한다.
   3. 문자가 아닌 객체일 경우 json방식으로 데이터를 만들어서 httpMessageConverter에 던진다 - JsonConverter가 동작한다.(매우 간략하게 말한것임. 보통 MappingJackson2HttpMessageConverter라는 라이브러리를 사용한다.)
5. 웹 브라우저 및 서버에 데이터를 던진다

참고로 클라이언트의 HTTP Accept 헤더와 서버의 컨트롤러 반환 타입 정보들을 조합하여 HttpMessageConverter가 선택된다.(요즘은 거의 json을 쓰지만 컨버터만 쓰면 된다는 생각을 하면된다.)