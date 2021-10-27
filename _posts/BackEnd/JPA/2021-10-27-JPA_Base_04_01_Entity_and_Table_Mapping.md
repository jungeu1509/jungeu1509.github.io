---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 4-1강"
excerpt: "엔티티 매핑 - 객체와 테블 매핑"
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
date:  2021-10-27 13:30:00 +0800
last_modified_at: 2021-10-27 13:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/2zDo 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 엔티티 매핑 소개

- 객체와 테이블 매핑: @Entity, @Table
- 필드와 컬럼 매핑: @Column
- 기본 키 매핑: @Id
- 연관관계 매핑: @ManyToOne, @JoinColumn

# 2. Entity

@Entity가 붙은 클래스는 JPA가 관리하게되고 엔티티라고 한다.

JPA를 통해서 테이블과 매핑한 클래스는 @Entity 필수이다.

## 2.1. 주의사항

기본 생성자가 필수이다.(파라미터가 없는 생성자. public 혹은 protected 생성자여야한다.)

final, enum, interface, inner 클래스를 사용하면 안된다.

저장할 필드에 final을 사용하면 안된다.

## 2.2. 예시

```java
@Entity
@Getter
@Setter
@Table(name="member") // db에서 사용할 테이블 이름을 적을 수 있다.
public class Member {
    @Id
    private Long id;
    private String name;

    public Member() {} // 기본 생성자가 필수적으로 필요하다.

    public Member(Long id, String name) {
        this.id = id;
        this.name = name;
    }
}
```