---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 2강"
excerpt: "라이브러리 살펴보기"
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
date:  2021-06-08 20:30:00 +0800
last_modified_at: 2021-06-08 20:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. 라이브러리

프로젝트의 폴더 목록에서

External Libraries를 들어가면 내가 받은 라이브러리들을 볼 수 있다.

(강의의 목록과 달라서 살짝 당황)

결론은 내가 처음 프로젝트 만들때 다운받겠다고 한 라이브러리 들을 설치하면

dependency를 확인하여 모두 다 자동으로 다운받아진다.

오른쪽 보일랑말랑한 세로로된 메뉴목록에서

Gradle을 선택하면 다운받은 목록을 볼 수 있다.

(강의의 목록과 내용이 또 다름)

# 2. 다운받은 라이브러리들

## 2.1 스프링부트 라이브러리

### 2.1.1 spring-boot-starter-web

- 톰캣(웹서버)
- 스프링 웹 MVC 

### 2.1.2 spring-boot-starter-thymeleaf

타임리프 템플릿 엔진(view)

### 2.1.3 spring boot-starter

- spring-boot
  - spring-core
- spring-boot-starter-logging
  - logback,slf4j

## 2.2 테스트 라이브러리

### 2.2.1 spring-boot-starter-test

- junit : 테스트 프레임워크
- mockito : 목 라이브러리
- assertj : 테스트코드를 좀더 편하게 작성을 도와주는 라이브러리
- spring-test : 스프링 통합 테스트 지원

# 3. 로그 간단 설명

로그라는 것으로 출력을 해야한다.

심각한 에러만 보거나 분류별로 로그를 모아 볼 수 있다.

(System.out.print을 잘 안쓴다.)

spring-boot-starter-loging라이브러리를 보면

logback과 slf4j 라이브러리 도 기본적으로 다운받은 것을 볼 수 있다.

(두가지로 검색해보면 로그를 더 자세히 알 수 있다.)

# 3. test

junit 5버전을 최근 많이 사용하는 추세이다. 테스트 프레임워크이다.

(mockito, assertj 테스트를 편하게 해주는 라이브러리)

spring-test 라이브러리는 스프링과 통합해서 테스트 할 수 있는 라이브러리

# 4. 정리

안써봤으면 크게크게만 이해하고 나중에 써보면서 알 수 있다.