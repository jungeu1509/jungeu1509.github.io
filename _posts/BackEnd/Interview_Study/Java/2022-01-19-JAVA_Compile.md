---
layout: single
title:  "Java 컴파일 과정"
excerpt: "면접 준비 스터디"
header:
  teaser: 
search: true
permalink:
categories: 
  - Interview
tags:
  - Interview
  - Java
date:  2022-01-19 11:45:00 +0900
last_modified_at: 2022-01-19 11:45:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. JAVA의 중요 특징

![JAVA Program](/assets/images/posts/BackEnd/Interview/JAVA_Compile/01_JAVA_Program.png)

JAVA는 어떤 하드웨어, 운영체제에 상관없이 컴파일된 코드가 플랫폼 독립적이라는 점이 가장 큰 특징이다. 다시말해서 어느 플랫폼이든 작성한 소스를 변경할 필요없이 다 실행시킬 수 있다는 것이다.

위의 특징을 가능하게 만드는 것이 JVM(JAVA Virtual Machine)이다.
간단히 JVM을 말하면 컴파일된 코드(바이트코드)를 실행시켜주는 가상의 컴퓨터라고 생각하면 된다. 

JVM은 추후 다른 포스트로 더 정리할 것이다. (그 전까지 맨 밑의 참고자료들을 읽어보는 것을 추천합니다.)

# 2. 컴파일 과정

![JAVA Code Execute](/assets/images/posts/BackEnd/Interview/JAVA_Compile/02_JAVA_Code_Execute.png)

1. 자바 개발자들이 .java 파일을 생성
2. 자바 컴파일러가 자바 소스파일을 컴파일한다.
    - 이때 나오는 파일은 자바 바이트 코드(.class)파일로 아직 컴퓨터가 읽을 수 없는 JVM이 이해할 수 있는 코드이다.
    - 바이트 코드의 각 명령어는 1바이트 크기의 Opcode와 추가 피연산자로 이루어져 있다.
3. 컴파일된 바이크 코드를 JVM의 클래스로더(Class Loader)에게 전달한다.
    - 클래스로더 세부동작
    1. 로드 : 클래스 파일을 가져와서 JVM의 메모리에 로드
    2. 검증 : 자바 언어 명세(Java Language Specification) 및 JVM 명세에 명시된 대로 구성되어 있는지 검사
    3. 준비 : 클래스가 필요로 하는 메모리를 할당합니다.(필요한 메모리는 클래스에서 정의된 필드, 메서드, 인터페이스 등을 나타내는 데이터 구조들 등을 말한다.)
    4. 분석 : 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경(클래스의 특정 메모리 주소를 참조 관계로 구성한 것이 아닌 참조하는 대상의 이름만을 지칭한 것.)
    5. 초기화 : 클래스 변수들을 적절한 값으로 초기화 (static 필드들을 설정된 값으로 초기화 등)
4. 클래스 로더는 동적로딩(Dynamic Loading)을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역(Runtime Data area) 즉, JVM의 메모리에 올린다.
5. JVM의 실행엔진(Excution Engine)은 JVM메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행한다.
    - 실행 엔진은 두가지 방식으로 변경한다.
    1. 인터프리터
        - 바이트 코드를 하나씩 읽어서 실행
        - 한줄 한줄 실행은 빠르나 전체적인 실행 속도는 느리다.
  
    2. JIT 컴파일러 (Just-In-Time Compiler)
        - 인터프리터의 단점을 보완하기 위해 도입된 방식
        - 바이트 코드 전체를 컴파일하여 바이너리 코드로 변경 후 해당 메서드를 더이상 인터프리팅 하지 않고, 바이너리 코드로 직접 실행하는 방식
        - 전체적인 실행속도는 인터프리팅보다 빠르다.

![JAVA Compile](/assets/images/posts/BackEnd/Interview/JAVA_Compile/03_JAVA_Compile.png)

# 참고자료

- [Tech Interview(gyoogle.dev)](https://gyoogle.dev/blog/computer-language/Java/컴파일%20과정.html)
- [dalpang.e 티스토리](https://steady-snail.tistory.com/67)
- [알짜배기 프로그래머 티스토리](https://aljjabaegi.tistory.com/387)
- [마이구미의 HelloWorld 티스토리](https://mygumi.tistory.com/115)
- [swiftymind 티스토리](https://swiftymind.tistory.com/78)

