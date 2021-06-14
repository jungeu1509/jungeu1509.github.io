---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 6강"
excerpt: "MVC와 템플릿 엔진"
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
date:  2021-06-14 20:30:00 +0800
last_modified_at: 2021-06-14 20:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/hisy 강의를 수강하고 작성하는 게시물입니다.

---
# 1. MVC(Model View Controler)와 템플릿 엔진

모델1에서는 컨트롤러와 뷰를 분류하지 않고 뷰에서 모든걸 다했다.

요즘은 컨트롤러와 뷰의 역할을 분류 했다.

뷰는 화면을 그리는 역할 컨트롤러는 그 외의 내부 처리하는 역할을 다 한다.(모델에 담아 화면에 넘겨주는 역할)

## 1.1 컨트롤러 예제

전에 작업하던 controller파일의 HelloController.java를 아래와 같이 수정한다(내용 추가 한다.)

```java
package Hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.stereotype.Repository;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

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
}
```

return 을 위해 resource - templates 아래에

hello-template.html파일을 아래와 같이 만든다.

```html
<html xmlns:th="http://www.thymleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

꺾은 괄호 사이의 글자는 html에서 그냥 보여진다.

서버에서 ${name}에 내용이 들어가면 내용이 치환된다.

모델의 키값으로 가져온다.

http://localhost:8080/hello-mvc

위는 name변수를 안주는 방법(hello null이라고 나온다)

http://localhost:8080/hello-mvc?name=spring

위의 주소는 name을 주었다. 내용이 바뀌는 것을 확인하면 좋을것이다.(hello spring이라고 나온다.)

## 1.2. 대략적인 정리

1. 웹 브라우저에서 주소를 받으면 스프링 부트의 내장 톰켓 서버를 거쳐 스프링에게 던진다.
2. 스프잉은 helloController에 매핑이 되어있는것을 확인 후 호출해준다
3. return할때 값을 스프링에게 넘긴다
4. 스프링이 viewResolver를 동작시킨다(후에 강의 예정)
5. 변환한 HTML을 웹 브라우저에 넘긴다.


