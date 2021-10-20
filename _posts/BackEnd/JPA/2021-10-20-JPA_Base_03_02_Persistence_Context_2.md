---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 3-2강"
excerpt: "영속성 관리 - 내부 동작 방식 - 영속성 컨텍스트 2"
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
date:  2021-10-20 23:50:00 +0800
last_modified_at: 2021-10-20 23:50:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/2zDo 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 엔티티 조회, 1차 캐시

```java
//엔티티를 생성한 상태(비영속)
Member member = new Member();
member.setId(1L);
member.setName("회원1");
//엔티티를 영속
em.persist(member);
```

엔티티를 영속하는 순간

키(@Id)와 값을 1차캐시에 저장하게된다.

## 1.1. 1차 캐시에서 조회

조회를 할때 DB에서 찾는것이 아닌 1차 캐시에서 조회를 하게 된다.

1차 캐시에서 없으면 DB에서 조회한다. 

DB에 존재하면 1차캐시에 저장하고 반환한다.

사실 큰 차이가 없긴 한데.

transaction 이 종료되면 1차 캐시도 지워지므로 크게 차이는 없다.

