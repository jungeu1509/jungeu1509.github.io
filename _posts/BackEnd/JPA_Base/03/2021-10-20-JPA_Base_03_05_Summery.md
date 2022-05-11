---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 3-5강"
excerpt: "영속성 관리 - 내부 동작 방식 - 정리"
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
date:  2021-10-20 23:24:00 +0900
last_modified_at: 2021-10-20 23:24:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/2zDo 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 1차 캐시

고객이 10 명이 동시에 접근했을때 각각이 1차 캐시를 얻는다고 보면 된다.

성능적인 이점은 거의 없다고 봐야한다.