---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 6-3강"
excerpt: "다양한 연관관계 매핑 - 일대일 [1:1]"
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
date:  2021-11-08 23:20:00 +0900
last_modified_at: 2021-11-08 23:20:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 일대다 [1:1]

일대일관계는 반대도 일대일이다.

## 단방향인 경우

주 테이블이나 대상 테이블 중에 외래키 선택가능

주 테이블에 외래키 or 대상 테이블 중에 외래 키

외래 키에 DB UNIQUE 제약조건을 추가한다.

![One to One](/assets/images/posts/BackEnd/JPA/06/06_03_1_One_To_One.png)

다대일 단방향 매핑과 거의 유사하다.
 
아래처럼 locker를 추가한다고 생각해보자.

인당 사물함 한개씩만 배정하는 것이다.

![Mapping Locker](/assets/images/posts/BackEnd/JPA/06/06_03_2_Locker.png)

```java
@Entity
public class Locker {

    @Id @GeneratedValue
    private Long id;

    private String name;

    //양방향일경우
    //@OneToOne(mappedBy = "locker")
    //private Member member;
}
```

```java
// Member.java
    @OneToOne
    @JoinColumn(name = "LOCKER_ID")
    private Locker locker;
```

## 양방향인 경우

다대일 양방향 매핑과 동일하다. 외래키가 있는 곳이 연관관계의 주인이다.

반대편은 mappedBy를 적용해야한다.

## 주의사항

일대일에서 연관관계의 주인이 아닌 곳에서 외래키 관리는 불가능하다.

## 연관관계의 주인은 누가 가져가는 것이 좋을까...? 

### 1. 일대다 관계로의 변환

시간이 흘렀을때 일대다 관계가 될 가능성이 큰 부분을 생각하면된다.

장기적으로 봤을때 일대다에서 다 부분에 연관관계 주인을 부여하면된다.

### 2. 많이 조회하게될 위치

미래에도 1:1 관계일 것 같으면 어떻게 해야할까?

만약 Locker보다 Member를 더 많이 조회하게 될 것 같으면 이미 Member를 조회했을때 Locker의 정보도 갖고있게되므로 Member가 연관관계 주인인 것이 유리하다.

### 정리

- 주 테이블에 외래키
  - 객체지향 개발자 선호
  - JPA 매핑 편리
  - 장점 : 주 테이블만 조회해도 대상 테이블에 데이터가 있는지 확인 가능
  - 단점 : 값이 없으면 외래 키에 null 허용
-  대상 테이블에 외래키
   - 대상 테이블에 외래 키가 존재
   - 전통적인 데이터베이스 개발자 선호
   - 장점 : 주 테이블과 대상 테이블을 일대일에서 일대다 관계로 변경할 때 테이블 구조 유지 
   - 단점 : 프록시 기능의 한계로 지연 로딩으로 설정해도 항상 즉시 로딩됨(프록시는 뒤에서 설명)

강사님은 주 테이블에 외래키를 넣는것을 선호한다.