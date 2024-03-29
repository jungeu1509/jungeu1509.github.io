---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 8강"
excerpt: "비즈니스 요구사항 정리"
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
date:  2021-06-15 19:30:00 +0900
last_modified_at: 2021-06-15 19:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/hisy 강의를 수강하고 작성하는 게시물입니다.

---
# 1. 비즈니스 요구사항 정리

- 데이터 : 회원ID, 이름
- 기능 : 회원등록, 조회
- 아직 데이터 저장소가 선정되지 않음(가상의 시나리오)

# 2. 일반적인 웹 애플리케이션 계층 구조

- 컨트롤러 : 웹 MVC의 컨트롤러 역할
    - API 만들때 역할
- 서비스 : 핵심 비즈니스 로직 구현
    - 회원이 중복가입이 안된다 등의 로직 비즈니스 도메인 객체를 갖고 핵심 비즈니스 로직이 동작하도록 구현한 객체
- 리포지토리 : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인 : 비즈니스 도메인 객체(회원, 주문, 쿠폰, 등등 ) 주로 데이터베이스에 저장하고 관리됨

# 3. 클래스 의존관계

아직 데이터 저장소가 선정되지 않아 *우선 인터페이스로 구현* 클래스로 변경할 수 있도록 설계
저장소는 다양한 저장소 고민중
개발을 진행하기 위해 초기 개발단계에서 구현체로 가벼운 메모리 기반의 데이터 저장소 사용

# 4. 다음 강에서

위의 대략적인 설명을 통해 구체적 코드를 작성할 것이다.