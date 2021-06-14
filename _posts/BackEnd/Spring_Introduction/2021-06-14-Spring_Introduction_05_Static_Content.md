---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 5강"
excerpt: "정적 컨텐츠"
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
date:  2021-06-14 20:00:00 +0800
last_modified_at: 2021-06-14 20:00:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. 스프링 웹 개발 기초

1. 정적 컨텐츠 : 파일을 그대로 웹 브라우저에 내려줌

2. MVC(Model View Controler)와 템플릿 엔진
  - jsp, php 템플릿 엔진 (서버에서 프로그래밍 해서 html로 바꿔주는)

3. API
   - json이라는 데이터 구조 포멧으로 클라이언트한테 데이터를 전달한다.


# 2. 정적컨텐츠

Static Contect 내용을 참조할 사이트

https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features

직접 찾아가는 방법
1. https://www.spring.io 에 접속한다
2. 메뉴 - Projects - Spring Boot - Learn - 해당 버전 의 Reference Doc. - Spring Boot Features
3. Developing Web Applications에서 내용을 참조한다.

버전에 따라 내용이 다르다.(당황)

## 2.1. html 추가하기

프로젝트의 src - main - resources - static 내부에 아무 html파일이나 만들어보자.

(hello-static.html파일을 만들었다.)

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>static content</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
정적 컨텐츠 입니다.
</body>
</html>
```

프로젝트를 빌드 및 실행 후 브라우저에서 

<localhost:8080/hello-static.html>

위 주소로 접속하면 작성한 화면을 볼 수 있다.

원하는 파일을 넣으면 정적파일을 그대로 보여준다(프로그래밍을 할 수는 없다.)

# 2.2. Static Content 대략적 원리

1. 주소를 브라우저에서 받아오면 내장 톰켓 서버에서 제일 먼저 받게 된다.
2. 톰켓 서버에서 스프링 컨테이너에 주소를 넘긴다.
3. 컨트롤러 쪽에서 html주소를 찾아본다.(컨트롤러가 우선순위를 갖는다.)
4. 컨트롤러에서 없을 경우 내부에 있는 리소스에서 찾는다.
5. 찾은 html파일을 반환한다.