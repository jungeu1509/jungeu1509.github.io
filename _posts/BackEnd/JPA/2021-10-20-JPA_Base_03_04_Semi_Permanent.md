---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 3-4강"
excerpt: "영속성 관리 - 내부 동작 방식 - 준영속"
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
date:  2021-10-20 23:20:00 +0800
last_modified_at: 2021-10-20 23:20:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/2zDo 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 준영속 상태

새로 등록하거나 조회했을때 영속상태가 된다.

영속 상태의 엔티티가 영속성 컨텍스트에서 분리한 상태를 준영속 상태라 한다.

영속성 컨텍스트가 제공하는 기능을 사용하지 못한다.

직접 쓸일이 거의 없다.

# 2. 준영속 상태로 만드는 방법

- em.detach(entity) : 특정 엔티티만 준영속 상태로 전환 
- em.clear() : 영속성 컨텍스트를 완전히 초기화
- em.close() : 영속성 컨텍스트를 종료