---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 6-4강"
excerpt: "다양한 연관관계 매핑 - 다대다 [N:M]"
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
date:  2021-11-09 00:10:00 +0900
last_modified_at: 2021-11-09 00:10:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 다대다 [N:M]

객체는 컬렉션을 사용해서 객체 2개로 다대다 관계가 가능하다. 하지만 관계형 데이터베이스는 정규화된 테이블 2개로 다대다 관계를 표현할 수 없다. 그래서 연결 테이블을 추가해서 일대다, 다대일 관계로 풀어내야한다.

@ManyToMany를 사용한다.

@JoinTable로 연결 테이블 지정한다.

다대다 매핑은 단방향 혹은 양방향 모두 가능하다.

예시를 들어보자

![Many To Many](/assets/images/posts/BackEnd/JPA/06/06_04_1_Many_To_Many.png)

## 단방향

```java
// Member.java
@ManyToMany
@JoinTable(name = "MEMBER_PRODUCT")
private List<Product> products = new ArrayList<>();
```

```java
//Product.java
@Entity
public class Product{
    @Id @GeneratedValue
    private long id;

    private String name;
}
```

## 양방향

```java
//Product.java
@Entity
public class Product{
    @Id @GeneratedValue
    private long id;

    private String name;

    @ManyToMany(mappedBy = "products")
    private List<Member> members = new ArrayList<>();
}
```

## 실무에서는...?

편리해 보이지만 실무에서 사용하지 않는다.

연결 테이블이 단순히 연결만 하고 끝나지 않는다. 주문시간, 수량같은 여러 데이터가 들어올 수 있다.

### 다대다 한계 극복

- 연결 테이블용 엔티티 추가(연결 테이블을 엔티티로 승격한다.)
- @ManyToMany 를 @OneToMany와 @ManyToOne으로 변경해준다.

![다대다 한계 극복](/assets/images/posts/BackEnd/JPA/06/06_04_2_Delete_Many_To_Many.png)
