---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 3-2강"
excerpt: "영속성 관리 - 내부 동작 방식 - 영속성 컨텍스트 2"
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
date:  2021-10-20 22:30:00 +0900
last_modified_at: 2021-10-20 22:30:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/2zDo 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 엔티티 조회, 1차 캐시

```java
//엔티티를 생성한 상태(비영속)
Member member = new Member();
member.setId(1L);
member.setName("회원1");
//엔티티를 영속
em.persist(member);
```

엔티티를 영속하는 순간

키(@Id)와 값을 1차캐시에 저장하게된다.

## 1.1. 1차 캐시에서 조회

조회를 할때 DB에서 찾는것이 아닌 1차 캐시에서 조회를 하게 된다.

1차 캐시에서 없으면 DB에서 조회한다. 

DB에 존재하면 1차캐시에 저장하고 반환한다.

사실 큰 차이가 없긴 한데.

transaction 이 종료되면 1차 캐시도 지워지므로 크게 차이는 없다.

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {
            Member member = new Member();
            member.setId(3L);
            member.setName("nameC");
            
            em.persist(member);

            em.find(Member.class, 3L);

            transaction.commit();
        } catch (Exception e) {
            transaction.rollback();
        } finally {
            em.close();
        }
        emf.close();
    }
}
```

위 코드를 실행하면 insert 쿼리는 실행되지만 select 쿼리는 실행되지 않는 것을 알 수 있다. 1차 캐시 때문이다.

만약 위처럼 실행을 안하고 현재 DB에 데이터는 있으므로 find만 했을때 select 쿼리가 실행된다. (두번 연속 find를 하면 한번만 쿼리가 실행되고 두번째는 실행되지 않는다.)

# 2. 영속 엔티티의 동일성 보장

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {

            Member member1 = em.find(Member.class, 3L);
            Member member2 = em.find(Member.class, 3L);

            System.out.println(member1 == member2);

            transaction.commit();
        } catch (Exception e) {
            transaction.rollback();
        } finally {
            em.close();
        }
        emf.close();
    }
}
```

로그에서 참이 출력될까 거짓이 출력될까.

정답은 참이다!

# 3. 엔티티 등록시 트랜잭션을 지원하는 쓰기 지연

트랜젝션 시작하고 persist를 통해 영속성 부여했을때 바로 쿼리문이 나가는 것이 아닌 쓰기 지연 SQL 저장소에 쿼리문을 쌓아놓는다.

commit 하는 순간 flush가 되면서 쿼리문을 다 내보낸다.

잘 이용한다면 많은 성능향상이 있을 수 있다!

# 4. 변경감지 (Drity Checking)

JPA는 데이터를 찾아오면 데이터를 객체처럼 사용할 수 있게 지원한다.

그래서 데이터를 찾아와서 객체로 저장하고 객체를 변경하면 JPA가 변경을 감지한 후 DB의 데이터를 바꿔준다.

## 4.1. 변경감지 

commit 하면 내부적으로 플러시를 행하게 된다.

엔티티와 스냅샷(값을 읽어왔을때의 최초의 상태를 따로 저장해둔것)을 비교하게된다.

다를경우 update 쿼리를 보낸다.

## 4.2. 그러면 값을 변경할때에는...?

persist없이 값만 딱 변경하는 것이 맞다!

값을 바꾸면 commit 되는 시점에 자동으로 바꿔준다.
