---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 4-3강"
excerpt: "엔티티 매핑 - 필드와 컬럼 매핑"
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
date:  2021-10-27 22:33:00 +0800
last_modified_at: 2021-10-27 22:33:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

[https://inf.run/2zDo](https://inf.run/2zDo) 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 실습 요구사항 추가

1. 회원은 일반 회원과 관리자로 구분해야 한다.
2. 회원가입일과 수정일이 있어야 한다.
3. 회원을 설명할 수 있는 필드가 있어야 한다. 이 필드는 길이 제한이 없다.

이 조건을 위해 코드를 아래처럼 수정하고 실행해본다.

```java
@Entity
public class Member {
    @Id
    private Long id;

    @Column(name = "name")
    private String username;

    private Integer age;

    @Enumerated(EnumType.STRING)
    private RoleType roleType;

    @Temporal(TemporalType.TIMESTAMP)
    private Date createdDate;

    @Temporal(TemporalType.TIMESTAMP)
    private Date lastModifiedDate;

    @Lob
    private String description;
}
```

```java
public enum RoleType {
  USER, ADMIN
}
```

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {
            transaction.commit();
        } catch (Exception e) {
            transaction.rollback();
        } finally {
            em.close();
        }
        emf.close();
    }
}
// 대부분 삭제
```

위처럼 수정하고 실행시키면 로그에서 쿼리를 아래처럼 보낸것을 볼 수 있다.

```sql
create table member (
    id bigint not null,
    age integer,
    createdDate timestamp,
    description clob,
    lastModifiedDate timestamp,
    roleType varchar(255),
    name varchar(255),
    primary key (id)
)
```

## 1.1. 매핑 어노테이션 정리

- @Column : 컬럼 매핑
- @Temporal : 날짜 타입 매핑
- @Enumerated : enum 타입 매핑
- @Lob : BLOB, CLOB(문자일 경우) 매핑 (큰 양의 데이터)
- @Transient : 특정 필드를 컬럼에 매핑하지 않음(매핑 무시)

## 1.2. @Column

- name : 필드와 매핑할 테이블의 컬럼 이름(기본값 : 객체의 필드 이름)
- insertable, updatable : 등록, 변경 가능 여부 (기본값 : true)
- nullable(DDL) : null 값의 허용 여부를 설정한다. false로 설정하면 DDL 생성 시에 not null 제약조건이 붙는다.
- unique(DDL) : @Table의 uniqueConstraints와 같지만 한 컬럼에 간단히 유니크 제 약조건을 걸 때 사용한다.
- columnDefinition(DDL) : 데이터베이스 컬럼 정보를 직접 줄 수 있다. ex) varchar(100) default ‘EMPTY' (기본값 : 필드의 자바 타입과 방언 정보를 사용)
- length(DDL) : 문자 길이 제약조건, String 타입에만 사용한다. (기본값 : 255)
- precision, scale(DDL) : BigDecimal 타입에서 사용한다(BigInteger도 사용할 수 있다). precision은 소수점을 포함한 전체 자 릿수를, scale은 소수의 자릿수 다. 참고로 double, float 타입에는 적용되지 않는다. 아주 큰 숫자나 정 밀한 소수를 다루어야 할 때만 사용한다. (기본값 : precision=19, scale=2)

실무에서 unique 는 잘 안쓴다. uk값이 랜덤하게 변경되므로 log를 본다거나 할때 알아보기가 힘들다. 그래서 엔티티 맨위에 @Table(uniqueConstraints = ) 형식으로 많이 준다.

## 1.3. @Enumerated

실무에서 enum타입을 쓰는것도 위험할 수 있다. enum 클래스에서 나열된 순서(숫자)로 DB에 들어가는데 enum class 에서 순서가 뒤바뀐다거나 해서 값이 뒤바뀌도 db에서는 그대로 존재하기 때문에 보통 그냥 String 타입으로 저장한다.

## 1.4. @Temporal

날짜 타입을 매핑할때 사용한다.

최신 하이버네이트를 사용할때는 LocalDate, LocalDateTime을 사용할 때는 생략 가능한다.

## 1.5. @Lob

데이터베이스 BLOB, CLOB 타입과 매핑

@Lob에는 지정할 수 있는 속성이 없다.

매핑하는 필드 타입이 문자면 CLOB 매핑, 나머지는 BLOB 매핑

- CLOB: String, char[], java.sql.CLOB 
- BLOB: byte[], java.sql. BLOB
  
## 1.6. @Transient

필드 매핑하지 않는다.

데이터베이스에 저장하거나, 조회하지 않는다.

주로 메모리상에서만 임시로 어떤 값을 보관하고 싶을 때 사용한다.

```java
@Transient
private Integer temp;
```
