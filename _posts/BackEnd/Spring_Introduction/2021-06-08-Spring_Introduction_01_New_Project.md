---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 1강"
excerpt: "프로젝트 생성"
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
date:  2021-06-08 19:30:00 +0900
last_modified_at: 2021-06-14 20:00:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/hisy 강의를 수강하고 작성하는 게시물입니다.

---
# 1. 설치

- java 11
- intelliJ

# 2. 프로젝트 생성

부트 스타터 사이트에서 프로젝트를 간편하게 생성한다.
- https://start.spring.io

Project -> Gradle Project 생성 (라이브러리를 땡겨오고 라이프사이클 관리 툴 요즘은 그래들로 많이 한다.)

Language -> Java

Spring Boot -> (SNAPSHOT)은 아직 베타같은 느낌 

Dependencies
spring web 선택
Thymeleaf(html템플릿 엔진 - 회사마다 다를 것이다.)

Project Metadata
Group -> 회사도메인이라고 생각
Artifact -> 프로젝트명이라 생각

작성 후 GENERATE 선택(다운로드 받아진다.)

폴더를 적당한 곳에 이동

# 3. IntelliJ

Open버튼 클릭

폴더를 이동하여

build.gradle을 Open한다.

프로젝트로 열기 버튼을 누르면 프로젝트가 열려진다

열려지는 순간 네트워크를 이용하여 라이브러리를 자동으로 다운받아진다.

# 4. 프로젝트 구조

.gradle  무시

.idea -> 인텔리제이 설정파일

gradle 

src -> 메인폴더와 테스트 폴더 존재

## 4.1 src의 메인
  
자바와 리소스 폴더 존재

자바 : 자바 소스 파일들

리소스폴더 : xml, 설정파일, html파일 등 (자바를 제외한 모든 파일들이 들어간다.)

## 4.2 src의 테스트 파일

테스트 코드와 관련된 소스가 테스트 폴더에 존재

## 4.3 build.gradle

start.spring.io에서 자동으로 만들어준 개발자 친화적 설정파일

버전설정 및 라이브러리 가져오는 명령이 적혀있다.

sourceCompatibility -> 자바 버전

repositories -> 공개된 사이트에서 받아오라고 명령 적어놓은 것

dependencies -> 라이브러리들 (thymeleaf, web 등 들어있다)

## 4.4 .gitignore

git에 자동 업로드 방지

# 5. Run

src - main - java- 맨 아패의 소스파일

자바의 메인문이 있다.

메인문 부분의 줄표시 부분에 초록색 삼각형을 이용하여 동작을 시키면

메인문이 실행된다.

실행되면 

Tomcat started on port(s): 8080 (http)

라는 문장이 중간에 뜰것이다. 그러면 실행이 된것이다.

아무 브라우저나 들어가서 주소창에

localhost:8080

이라고 입력하면 Whitelabel Error Page가 뜰것이다.

뜨면 실행 성공이다.(소스 내부에 아무것도 없기 때문에 에러 페이지가 뜬다.)

인텔리제이의 빨간 네모를 클릭하고 브라우저에서 새로고침을 하면

페이지를 열 수 없다고 화면이 바뀔 것이다.

# 6. 기타 환경설정

환경설정으로 들어가서

검색창에 gradle을 입력하면 몇개가 검색이 되는데

Build, Execution, Deployment - Build Tools - Gradle 창으로 들어간다.

제일 가운데 Build and run using을 Gradle에서 IntelliJ로 변경한다.

Run tests using 또한 IntelliJ로 변경한다.

그러면 그래들이 뜨고 한참있다 인텔리제이가 뜨는것이 아닌 바로 인텔리제이가 뜬다.