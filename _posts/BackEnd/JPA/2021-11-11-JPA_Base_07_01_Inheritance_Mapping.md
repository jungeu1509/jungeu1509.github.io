---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 7-1강"
excerpt: "고급 매핑 - 상속관계 매핑"
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
date:  2021-11-11 00:10:00 +0900
last_modified_at: 2021-11-11 00:10:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 상속관계 매핑

객체에는 상속관계가 존재하지만 관계형db는 상속관계가 존재하지않는다.

슈퍼타입 서브타입 관계라는 모델링 기법이 객체의 상속과 유사하다.

그래서 객체의 상속 구조와 비슷하게 DB의 슈퍼타입 서브타입 관계를 매핑한것을 **상속관계 매핑**이라고 한다.(논리모델)

## 슈퍼타입 서브타입 논리모델을 물리모델로 구현하는 방법

1. 각각 테이블로 변환 : join 전략
   
   - DTYPE 이란 속성을 부여하여 어떤 테이블을 join할지 속성을 확인하여 불러오는 전략
  
2. 통합 테이블로 변환 : 단일 테이블 전략

   - 논리모델을 한테이블에 다 넣어버린다.

3. 서브타입 테이블로 변환 : 구현 클래스마다 테이블 전략 

   - 각각 테이블을 구현하여 부모 속성을 각 테이블이 다 갖고있는것이다.

## JPA에서 객체와 테이블의 매핑

기본적으로 extern을 사용하여 객체를 상속하면 SINGLE_TABLE 전략으로 매핑된다.(단일 테이블 전략)

다른 전략을 사용하는 방법은 아래와 같이 @Inheritance(strategy =) 어노테이션을 사용하면 된다.

```java

@Entity
@Inheritance(strategy = InheritanceType.JOINED)
public class Item {}
```

만약 부모 클래스에 @DiscriminatorColumn 어노테이션을 사용하면 테이블에 DTYPE 속성으로 자식 테이블의 이름이 부모 테이블에 저장된다.(name="" 을 이용하여 속성 이름을 바꿀 수 있다.) 실무에서는 대부분 넣어준다.

자식 클래스에 @DiscriminatorValue("A") 어노테이션을 붙이면 DTYPE에 자식클래스 이름 대신 A가 들어간다.

## 테이블 전략별 장단점

### JOINED
- 장점
  - 테이블 정규화
  - 외래 키 참조 무결성 제약조건 활용가능
  - 저장공간 효율화
- 단점
  - 조회시 조인을 많이사용, 성능저하
  - 조회 쿼리가 복잡함
  - 데이터 저장시 INSERT SQL 2번 호출

### SINGLE_TABLE
- 장점
  - 조인이 필요 없으므로 일반적으로 조회성능이 빠름
  - 조회 쿼리가 단순함
- 단점
  - 자식 엔티티가 매핑한 컬럼은 모두 null 허용
  - 단일테이블에 모든 것을 저장하므로 테이블이 커질 수 있다.
  - 상황에 따라서 조회성능이 오히려 느려질 수 있다.

### TABLE_PER_CLASS

**이 전략은 데이터베이스 설계자와 ORM 전문가 둘 다 추천X**

- 장점
  - 서브 타입을 명확하게 구분해서 처리할 때 효과적
  - not null 제약조건 사용 가능
- 단점
  - 여러 자식 테이블을 함께 조회할 때 성능이 느림(UNION SQL 필요)
  - 자식 테이블을 통합해서 쿼리하기 어려움

기본적으로 Join전략이 제일 기준이라고 생각하고 들어간다.

