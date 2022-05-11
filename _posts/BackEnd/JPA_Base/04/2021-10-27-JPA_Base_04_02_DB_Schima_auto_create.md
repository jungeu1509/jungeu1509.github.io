---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 4-2강"
excerpt: "엔티티 매핑 - 데이터베이스 스키마 자동 생성"
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
date:  2021-10-27 13:30:00 +0900
last_modified_at: 2021-10-27 13:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/2zDo 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 데이터베이스 스키마 자동 생성

DDL을 애플리케이션 실행 시점에 자동 생성

테이블중심->객체중심

데이터베이스 방언을 활용해서 데이터베이스에 맞는 적절한 DDL 생성

이렇게 생성된 DDL은 개발 장비에서만 사용

생성된 DDL은 운영서버에서는 사용하지 않거나, 적절히 다듬은 후 사용

# 2. hibernate.hbm2ddl.auto 속성

- create : 기존테이블 삭제 후 다시 생성(DROP + CREATE)
- create-drop : create와 같으나 종료시점에 테이블 DROP
- update : 변경분만 DB에 반영된다. (운영DB에는 사용하면 안된다. 만약 속성을 지울경우 DB에는 적용되지 않는다.)
- validate : 엔티티와 테이블이 정상 매핑되었는지만 확인
- none : 사용하지 않음

# 3. 데이터베이스 스키마 사용시 주의할 점

1. 개발 초기 단계는 create 또는 update 사용을 추천한다.
2. 테스트 서버는 update 또는 validate 사용을 추천한다.
3. 스테이징과 운영 서버는 validate 또는 none 사용을 추천한다.

**운영 장비에는 절대 create, create-drop, update를 사용하면 안된다.**

데이터 날려먹기 딱 좋다...

이미 데이터가 쌓여있는 상황에서 update 등을 사용하면 alter 테이블쿼리가 발생해서 DB가 멈출수도 있고(lock) create나 create-drop을 사용하면 데이터가 날라갈 수도 있기 때문에 직접하거나 테스트에서 충분히 확인한 후 사용하는 것이 좋다. 

서비스는 5분만 정지되어도 난리가 나기때문에 신중해야한다.

# 4. DDL 생성 기능

@Column을 이용해서 속성을 부여할 수 있다.

테이블에서 속성이름을 주거나 길이를 지정하거나 유니크 제약조건을 추가할 수 있다.

```java
@Column(name = "name", nullable = false, length = 10)
// DB에서 속성을 name이라고 지정한다.
// null값을 부여할 수 없다.
// 길이는 10을 넘을 수 없다.
```

DDL 생성기능은 DDL을 자동 생성할 때만 사용되고 JPA의 실행 로직에는 영향을 주지 않는다.

