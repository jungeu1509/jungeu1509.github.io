---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 9-2강"
excerpt: "값 타입 - 임베디드 타입"
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
date:  2021-11-24 22:20:00 +0900
last_modified_at: 2021-11-24 22:20:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. Embedded type(복합값 타입)

번역이 애매해서 내장값 타입, 복합값 타입 등으로 말할 수도 있지만, 그냥 임베디드 타입이라고 한다.

새로운 값 타입을 직접 정의할 수 있다.

주로 기본 값 타입을 모아서 만들어서 복합 값 타입이라고도 한다.

int, String과 같은 '값 타입'이다.(엔티티가 아니라서 추적도 불가능하고 변경하면 끝난다.)

## 예시

회원 엔티티가 이름, 근무 시작일, 근무 종료일, 주소 도시, 주소 번지, 주소 우편번호를 가진다고 하자.

위를 추상하면

회원 엔티티가 이름, 근무 기간, 집 주소를 가진다라고 묶을 수 있는데 이것을 임베디드 타입이라고 한다.

![임베디드 타입 예시](/assets/images/posts/BackEnd/JPA/09/09_02_1_embedded_type_example.png)

# 2. 사용법

@Embeddable: 값 타입을 정의하는 곳에 표시 

@Embedded: 값 타입을 사용하는 곳에 표시

기본 생성자 필수로 선언한다.

# 3. 장점

- 재사용 가능
- 높은 응집도
- Period.isWork()처럼 해당 값 타입만 사용하는 의미 있는 메소드를 만들 수 있음
- 임베디드 타입을 포함한 모든 값 타입은, 값 타입을 소유한 엔티티의 생명주기를 의존함

# 4. 임베디드 타입과 테이블 매핑

임베디드 타입을 테이블과 매핑하면 다음과 같다.

![임베디드 타입 매핑](/assets/images/posts/BackEnd/JPA/09/09_02_1_embedded_type_example.png)

```java
@Embeddable
public class Period{
  private LocalDateTime startDate;
  private LocalDateTime endDate;
}
```

```java
@Entity
public Class Member {
  @Embedded
  private Period workPeriod;
}
```


임베디드 타입은 엔티티의 값일 뿐이다.

임베디드 타입을 사용하기 **전과 후에 매핑하는 테이블은 같다.**(제일 중요)

객체와 테이블을 아주 세밀하게(find-grained) 매핑하는 것이 가능해진다.

잘 설계한 ORM 어플리케이션은 매핑한 테이블의 수 보다 클래스의 수가 더 많다.

# 5. 속성 재정의 가능

한 엔티티에서 같은 값 타입을 사용가능하다.

@AttributeOverrides, @AttributeOverride를 사용해서 컬럼 속성을 재정의 할 수 있다.

```java
@Entity
public Class Member {
  @Embedded
  private Period workPeriod;

  @Embedded
  @AttributeOverrides({
    @AttributeOverride(name="startDate", column=@Column(name="vacation_start"))
    @AttributeOverride(name="endDate", column=@Column(name="vacation_end"))
  })
  private Period vacationPeriod;
}
```

# 6. 임베디드 타입의 null

임베디드 객체가 null일 경우 임베디드 타입에 해당하는 테이블의 값들이 모두 null처리된다.
