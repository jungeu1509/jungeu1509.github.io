---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 7-2강"
excerpt: "고급 매핑 - Mapped Superclass - 매핑 정보 상속"
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
date:  2021-11-11 00:15:00 +0900
last_modified_at: 2021-11-11 00:15:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. @MappedSuperclass

공통 매핑 정보가 필요할 때 사용(예시 : id, name)

```java
@MappedSuperclass
public abstract class BaseEntity{
  @Column(name="INSERT_MEMBER")
  private String createdBy;
  private LocalDateTime createdDate;
  @Column(name="UPDATE_MEMBER")
  private String lastModifiedBy;
  private LocalDateTime lastModifiedDate;
}
```
추후에 JPA 이벤트를 이용하여 자동으로 입력되도록 할 수도 있다.

추상클래스로 제작하는것을 권장한다!

- 상속관계 매핑X
- 엔티티X, 테이블과 매핑X
- 부모 클래스를 상속 받는 자식 클래스에 매핑 정보만 제공 
- 조회, 검색 불가(em.find(BaseEntity) 불가)
- 직접 생성해서 사용할 일이 없으므로 추상 클래스 권장

- 테이블과 관계 없고, 단순히 엔티티가 공통으로 사용하는 매핑 정보를 모으는 역할
- 주로 등록일, 수정일, 등록자, 수정자 같은 전체 엔티티에서 공통 으로 적용하는 정보를 모을 때 사용
- 참고: @Entity 클래스는 엔티티나 @MappedSuperclass로 지 정한 클래스만 상속 가능