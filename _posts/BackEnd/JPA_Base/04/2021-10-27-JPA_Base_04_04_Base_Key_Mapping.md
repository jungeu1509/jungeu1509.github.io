---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 4-4강"
excerpt: "엔티티 매핑 - 기본 키 매핑"
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
date:  2021-10-27 23:15:00 +0900
last_modified_at: 2021-10-27 23:15:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 기본 키 매핑 어노테이션

1. @Id
2. @GeneratedValue

```java
@Id @GeneratedValue(strategy = GenerationType.AUTO) 
private Long id;
```

# 2. 기본 키 매핑 방법

1. 직접 할당 : @Id만 사용
2. 자동생성 : @GeneratedValue

- IDENTITY: 데이터베이스에 위임, MYSQL
- SEQUENCE: 데이터베이스 시퀀스 오브젝트 사용, ORACLE
  - @SequenceGenerator 필요
- TABLE: 키 생성용 테이블 사용, 모든 DB에서 사용
  - @TableGenerator 필요
- AUTO: 방언에 따라 자동 지정, 기본값

## 2.1. IDENTITY 전략
데이터베이스 종류에 따라 자동으로 부여된다.

db에 넣을때 null값으로 넣으면 db에서 알아서 생성해준다.

그러고 사용할때 JPA에서 알아서 값을 읽어와서(반환값 확인 후) 넣어준다.

## 2.2. SEQUENCE 전략
숫자로 id를 선언해야한다. int나 Integer는 애매하다.

Long으로 보통 부여한다.(권장한다)

값을 순서대로 생성한다.

보통 1에서 시작해서 1씩 증가하도록 설정된다.

아래처럼 시작값이랑 증가하는 값을 설정할 수 있다.

```java
@Entity
@SequenceGenerator(
name = “MEMBER_SEQ_GENERATOR",
sequenceName = “MEMBER_SEQ", //매핑할 데이터베이스 시퀀스 이름
initialValue = 1,  // 시작값
allocationSize = 1) // 시퀀스 한번 호출에 증가하는 수
public class Member {
  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE,
  private Long id;
```

- initialValue : DDL 생성 시에만 사용됨, 시퀀스 DDL을 생성할 때 처음 1 시작하는 수를 지정한다.
- allocationSize : 시퀀스 한 번 호출에 증가하는 수(성능 최적화에 사용됨 데이터베이스 시퀀스 값이 하나씩 증가하도록 설정되어 있으면 이 값 을 반드시 1로 설정해야 한다. 기본값은 50이다.)

시퀀스 한 번에 여러개 보내는 경우도 있다. 그러나 애매하게 보내면 빈 공간이 생길 수 있으므로 1을 권장한다.

## 2.3. TABLE 전략

별도 테이블을 끌어다 사용해서 시퀀스를 흉내낸다.

다른 테이블이 락 걸릴 수도 있고 성능이 안좋아질 수 있다.

서비스를 운영할때는 부담스러운 전략

# 3. 권장하는 식별자 전략

null이 되면 안된다. 유일해야하고 변하면 안된다.

자연키(일상생활에서 사용하는 식별자)를 찾기는 어렵다. 그래서 대리키를 주로 사용한다.

주민등록번호도 기본키로 적절하지 않다.(갑자기 정책이 변한다거나 등)

그래서 권장하는 방법은 

Long형 + 대체키 + 키 생성전략 사용

예를 들면 Autoincrement, UUID, 랜덤값을 사용한다.
