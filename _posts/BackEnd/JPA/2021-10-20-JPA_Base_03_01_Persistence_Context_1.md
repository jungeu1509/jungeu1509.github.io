---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 3-1강"
excerpt: "영속성 관리 - 내부 동작 방식 - 영속성 컨텍스트 1"
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
date:  2021-10-20 17:30:00 +0800
last_modified_at: 2021-10-20 17:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/2zDo 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 영속성 컨텍스트란?

**"엔티티를 영구 저장하는 환경"**

```java
EntityManager.persist(entity);
```

위 코드처럼 저번강의에서 사용했었다.

엔티티를 영속성 컨텍스트에 저장한다고 생각하면된다.

## 1.1. 엔티티 매니저

엔티티매니저를 통해서 영속성 컨텍스트에 접근하는 것이다.

## 1.2. 엔티티의 생명주기

- 비영속 (new/transient) : 영속성 컨텍스트와 전혀 관계가 없는 새로운 상태
- 영속 (managed) : 영속성 컨텍스트에 관리되는 상태
- 준영속 (detached) : 영속성 컨텍스트에 저장되었다가 분리된 상태
- 삭제 (removed) : 삭제된 상태

### 1.2.1. 비영속

JPA와 전혀 관련없이 객체만 생성한 상태이다.

```java
Member member = new Member();
member.setId(1L);
member.setName("회원1");
```

### 1.2.2. 영속

```java
//객체를 생성한 상태(비영속)
Member member = new Member();
member.setId(1L);
member.setName("회원1");
EntityManager em = emf.createEntityManager();
em.getTransaction().begin();
//객체를 저장한 상태(영속)
em.persist(member);
```
위 코드를 실행하면 영속성 컨텍스트에 관리되는 상태가 된다.

**(주의!)** 영속성 상태가 된다고 하더라도 db에 쿼리가 날라간 것이 아니다! 

transaction을 커밋했을때 쿼리가 날라간다.
(강사님은 persist 앞뒤로 로그를 출력시켜 쿼리문이 나가는 순서를 설명해 주셨다 - persist 바로 뒤에서도 쿼리문이 나가지 않고 뒤 로그 출력 후 쿼리문이 출력됐다.)

### 1.2.3. 준영속

```java
em.detach(member);
```
위 코드를 이용해서 영속성 컨텍스트에서 분리한 상태

### 1.2.4. 삭제

```java
em.remove(member);
```

객체를 삭제한 상태

# 2. 영속성 컨텍스트의 이점

- 1차 캐시
- 동일성(identity) 보장
- 트랜잭션을 지원하는 쓰기 지연 (transactional write-behind)
- 변경 감지(Dirty Checking) 
- 지연 로딩(Lazy Loading)
