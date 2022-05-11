---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 2-2강"
excerpt: "JPA 시작하기 - 애플리케이션 개발"
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
date:  2021-10-20 11:50:00 +0900
last_modified_at: 2021-10-20 11:50:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/2zDo 강의를 수강하고 작성하는 게시물입니다.

---

# 1. JPA 구동 방식

1. Persistence 라는 Class 에서 시작을 한다.

2. Persistence 설정정보를 META-INF/persistence.xml 에서 읽어온다.

3. EntityManagerFactory Class 를 만들어낸다.

4. EntityManagerFactory에서 필요할때마다 EntityManager 를 생성해낸다.

# 2. JPA 동작 확인 실습

src/main/java 밑에 hellojpa 패키지를 만들고 JpaMain 클래스를 만들고 main을 만든다.

아래와 같이 main을 채워준다.

```java
public class JpaMain {
    public static void main(String[] args) {
        //EntityManagerFactory를 우선 생성하는데, persistenceUnitName을 파라미터로 넣어야한다.
        // 이전강의의 설정에서 입력했었던(persistence.xml에서 입력했었던) hello를 입력해준다.
        EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("hello");
        EntityManager entityManager = emf.createEntityManager();
        //실제 코드가 작성될 부분
        entityManager.close();
        emf.close();
    }
}
```

실행되는지 확인하고 잘 된다면 지금까지 잘 작성한 것이다.

(더 진행하려면 DB를 설정하고 해야하므로 여기까지밖에 작성 못함.)

# 3. 객체와 테이블을 생성하고 매핑하기.

## 3.0. 테이블 생성

h2를 연결하여 테이블을 만들것이다.

h2 설치한 폴더로 들어가서 bin 폴더로 들어간다.

h2.sh를 아래 명령어로 실행시킨다.

```bash
./h2.sh
```

만약 권한을 줘야한다고 에러메시지가 뜨면 아래명령어로 권한을 부여한다.

```bash
chmod +x h2.sh
```

브라우저 창으로 h2 콘솔이 나오는데

JDBC URL 이 jdbc:h2:tcp://localhost/~/test 와 동일한지 확인한다.(persistence.xml 에서 입력한 url과 동일해야한다.)

연결을 하면 오른쪽에 쿼리문을 쓸 수 있는 칸이 있는데 아래와 같이 작성하고 실행버튼을 눌러 테이블을 생성한다.

```sql
create table Member (
    id bigint not null,
    name varchar(255),
    primary key(id)
);
```

## 3.2. 객체 생성

다시 인텔리제이로 돌아와서 객체를 생성한다.

src/main/java/hellojpa 같은 디렉토리에서

Member Class를 아래와 같이 생성한다.

```java
package hellojpa;

import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class Member {
    @Id
    private Long id;
    private String name;

    //Getter, Setter 를 생성해준다.
}
```

여기서 @Entity는 꼭 적용해줘야한다.

import 할때 비슷한 것들이 많이 나오면

javax.persistence로 되어있는것을 넣어준다.

Getter 와 Setter까지 입력을 완료했으면 JPA를 이용하여 연결할 객체까지 생성할 준비가 완료된것이다.

# 4. 객체생성 테스트하기.

main으로 돌아가서 EntityManager 생성된 부분과 close 전의 부분에서 생성을 할 것이다.

잠깐 설명을 하자면

EntityManagerFactory는 무조건 딱 하나만 만들어야 하고

트렌젝션 단위마다(디비를 넣기위해 connection을 얻고 쿼리를 생성하고 날리고 저장하는 과정마다) EntityManager를 만들어줘야한다.

## 4.1. 등록

이제 아래와 같이 맴버를 저장해보자.

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();

        Member member = new Member();

        member.setId(1L);
        member.setName("nameA");

        em.persist(member);
        transaction.commit();

        em.close();
        emf.close();
    }
}
```

EntityManager를 활용을 해주어야하는데 transaction을 활용한다.

transaction을 begin 하고 commit을 한다.

위처럼 소스를 입력하고 실행시키면 

```
Hibernate: 
    /* insert hellojpa.Member
        */ insert 
        into
            Member
            (name, id) 
        values
            (?, ?)
```
이 주석을 보면 쿼리문을 볼 수가 있다.

persistence.xml 에서 show_sql 옵션을 true 해서 쿼리문을 볼 수 있는 것이고

format_sql은 쿼리문을 이쁘게 포맷팅하여 보여준다.

use_sql_comments는 쿼리문이 왜 나오게 되었는지 설명해준다.

사실 위의 코드는 예외처리가 되지 않은 코드이다 예외처리를 하면 아래와 같다.

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {
            Member member = new Member();
            member.setId(1L);
            member.setName("nameA");

            em.persist(member);
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

정석 코드는 강의를 위해 위처럼 보여주지만 실제로는 이렇게 쓰지 않는다.

spring이 대부분 다 해주기 때문이다.

## 4.2. 조회

만약 조회를 한다면 다음과 같이 조회할 수 있다.

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {

            Member member = em.find(Member.class, 1L);
            System.out.println("findMember.id = " + member.getId());
            System.out.println("findMember.name = " + member.getName());

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

## 4.3. 삭제

조회한 것을 지우기만하면 된다.

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {
            Member member = em.find(Member.class, 1L);

            // 삭제하는 부분
            em.remove(member);

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

## 4.4. 수정

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {
            Member member = em.find(Member.class, 1L);

            member.setName("ByeA");

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

놀라운 것은 자바 객체를 수정만 하면 바꾸게 JPA 가 만들어져 있다. (업데이트 쿼리가 나간다.)

# 5. 주의사항

- 엔티티 매니저 팩토리는 하나만 생성해서 애플리케이션 전체에 서 공유
- 엔티티 매니저는 쓰레드간에 공유X (사용하고 버려야 한다).
- JPA의 모든 데이터 변경은 트랜잭션 안에서 실행

# 6. JPQL 소개

특정 조건을 만족하는 조회를 하고싶다면 JPQL을 사용해야한다.

다음 코드와 같이 쿼리문을 날려서 사용할 수 있다.

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {
            List<Member> result = em.createQuery("select m from Member as m", Member.class).getResultList();

            for(Member member: result) {
                System.out.println("member.name = " + member.getName());
            }

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

여기서 쿼리문을 사용할 때에는 테이블을 사용하는 느낌 보다는 객체를 사용한다고 생각한다.

로그문에서 쿼리를 사용한 부분을 보면

```
Hibernate: 
    /* select
        m 
    from
        Member as m */ select
            member0_.id as id1_0_,
            member0_.name as name2_0_ 
        from
            Member member0_
member.name = nameA
```

로그에서는 select 후 객체를 다 나열한다. 쿼리문에서 m 을 이용해서 멤버 엔티티를 선택했다고 보면 된다.


## 6.1. JPQL이 무슨 메리트가 있나...?

예를들어 페이징을 하고싶다고 해보자.
아래처럼 setFirstResult setMaxResults를 사용하면 된다.

```java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction transaction = em.getTransaction();
        transaction.begin();
        try {
            List<Member> result = em.createQuery("select m from Member as m", Member.class).setFirstResult(5).setMaxResults(10).getResultList();

            for(Member member: result) {
                System.out.println("member.name = " + member.getName());
            }

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

위 코드를 실행하면 로그에서 limit와 offset이 설정되는것을 볼 수 있다. (설정한 쿼리문마다 용어가 다르다.)


- JPA는 SQL을 추상화한 JPQL이라는 개체 지향 쿼리 언어 제공
- SQL과 문법 유사, SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN 지원
- JPQL은 엔티티 객체를 대상으로 쿼리 
- SQL은 데이터베이스 테이블을 대상으로 쿼리
- 테이블이 아닌 객체를 대상으로 검색하는 객체 지향 쿼리
- SQL을 추상화해서 특정 데이터베이스 SQL에 의존하지 않는다.
- JPQL을 한마디로 정의하면 객체 지향 SQL
- JPQL은 뒤에서 아주 자세히 다룰 것이다.