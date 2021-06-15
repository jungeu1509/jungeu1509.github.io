---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 10강"
excerpt: "회원 리포지토리 테스트 케이스 작성"
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
date:  2021-06-15 20:30:00 +0800
last_modified_at: 2021-06-15 20:30:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

<https://inf.run/hisy> 강의를 수강하고 작성하는 게시물입니다.

---
# 1. 회원 리포지토리 테스트케이스 작성

(코드를 코드로 검증한다)

개발한 기능을 테스트 할때

1. main 메서드를 통해 실행해서 확인
2. 웹 애플리케이션의 컨트롤러를 통해 해당기능 실행
3. 자바의 JUnit이라는 프레임워크로 테스트 실행

JUnit이라는 프레임워크를 사용하면 테스트 코드를 통해 준비하고 실행하기 단순해지고 여러 테스트를 한번에 실행하기 쉬워진다.

# 2. 테스트 패키지 제작

src-test-java-Hello.hellospring 밑에 repository 패키지를 만든다.

보통 테스트는 메인의 구조와 같게 만든다.

다른점은 테스트 하려는 클래스를 만들고 Test글자를 붙인다.

즉 MemoryMemberRepositoryTest라는 클래스를 만든다.

클래스는 다른곳에서 쓰지 않을 것이기 때문에 public으로 안해도 된다.(지움)

# 3. save 테스트

인스턴스를 생성하고

@Test를 앞에 붙이면 org.junit.jupiter.api를 불러온다.

save를 테스트 해볼 것이므로 public void save()를 입력하면 실행 할 수 있다.

왼쪽의 초록 삼각형을 누르면 Run할 수 있다.

```java
package Hello.hellospring.repository;

import Hello.hellospring.domain.Member;
import org.junit.jupiter.api.Test;

class MemoryMemberRepositoryTest {

    MemberRepository repository = new MemoryMemberRepository();

    @Test
    public void save() {
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findById(member.getId()).get();

        System.out.println("result = " + (result == member)) ;
    }
}
```

member를 생성해서 spring이란 name으로 지정하여 레파지토리에 저장한다.

뜰어갔는지 검증하기 위해서 member의 이름이 레파지토리에 있는지 findById를 통해 검색하고 get을 통해 값을 던진다.

만약 잘 저장이 됐고 검색도 됐을경우 result와 member가 같을것이므로

result == member 는 True일 것이다. 그래서 True가 출력된다.

**참고** soutv 를 누르면 아래 문구가 자동으로 나온다.

```java 
System.out.println("result = " + result);
```

## 3.1 테스트 결과 출력

True False라는 문자로 결과를 확인하는 것에는 한계가 있으므로 다른 api를 사용할 것이다.

### 3.1.1. Assertions org.junit.jupiter.api

Assertions를 입력하면 org.junit.jupiter.api가 있다. 

```java
Assertions.assertEquals(result, member);
```
위 소스는 두개가 같으므로 그냥 빌드가 된다.

만약 member대신 null을 입력하면 error가 발생한다.

### 3.1.2. Assertions org.assertj.core.api

Assertions를 입력하면 org.assertj.core.api도 존재한다.

```java
Assertions.assertThat(member).isEqualTo(result);
```

위의 소스도 동일하게 동작한다.

Assertions에서 option+enter를 하면 static import할 수 있다.

소스 제일 위에 자동으로 입력이 되고

다음부터 Assertions라고 하면 바로 assertj의 Assertions를 쓸 수 있다.

# 4. findByName 테스트



```java
    @Test
    public void findByName(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        Member result = repository.findByName("spring1").get();

        Assertions.assertThat(result).isEqualTo(member1);

    }
```

shift+f6을 입력하면 단어에 자동으로 블럭지정이 되면서 같은 단락의 같은 단어를 동시에 바꿀 수 있다.

member2 입력할때 3줄을 복사해서 member2만 복사하면 된다.

결과 검증 라인을 아래처럼 바꾼다면 거짓으로 빌드가 안될것이다.

```java
Assertions.assertThat(result).isEqualTo(member2);
```

# 4.1. 여러개를 동시에 테스트

클래스 레벨에서 돌리거나

test- java - Hello.hellospring 을 우클릭하여 돌리면

단위 테스트가 가능하다.

# 5. findAll test

```java
    @Test
    public void findAll(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        List<Member> result = repository.findAll();

        Assertions.assertThat(result.size()).isEqualTo(2);

    }
```

결과 검증 라인을 아래처럼 바꾼다면 거짓으로 빌드가 안될것이다.

```java
Assertions.assertThat(result.size()).isEqualTo(3);
```

# 5.1. 전체테스트 오류

전체테스트를 하면 갑자기 에러가 날 것이다.

테스트 순서가 임의로 진행이 된다.

즉, findAll 테스트에서 멤버를 저장하고 findByName 테스트에서 멤버를 또 저장했으므로 멤버수가 2개보다 많아져서 오류가 난다.

그래서 테스트를 할때마다 레파지토리를 깔끔하게 클리어를 해주어야 한다.

MemoryMemberRepository만 테스트 하는 것이므로

테스트 클래스 맨 위에 MemberRepository 인터페이스로 선언된 repository를 MemoryMemberRepository 형식으로 변경해준다.

```java
    MemoryMemberRepository repository = new MemoryMemberRepository();
```

main - java- Hello.hellospring- repository - MemoryMemberRepository.java

로 이동해서 맨 아래에 clearStore() 라는 함수를 만들어준다.

```java
    public void clearStore() {
        store.clear();
    }
```

store.clear()는 저장된것을 다 지워준다.

다시 테스트 클래스로 돌아와서 아래 코드를 repository 선언 밑에 넣어준다. 

```java
    @AfterEach
    public void afterEach() {
        repository.clearStore();
    }
```

@AfterEach는 매 매서드 마다 동작하는 명령이다.

즉, 테스트가 끝날때마다 repository를 비워준다.

# 6. 테스트에 대하여 부연설명

내가 A를 만든다고 할 때 미리 검증 틀을 만들어 놓고 만들어지면 틀에 맞춰지나 확인을 해보는 역순으로도 가능하다.

이것을 테스트 주도 개발 TDD라고 한다.

나중에 테스트가 수백 수천개가 될때 자동화가 될 수도 있다.

대형 프로젝트가 될 수록 테스트 코드없이 프로젝트가 만들어 질 수 없다.

꼭 테스트에 관하여 깊이있게 공부해야한다.
