---
layout: single
title:  "[스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술] 4강"
excerpt: "빌드하고 실행하기
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
date:  2021-06-09 21:00:00 +0800
last_modified_at: 2021-06-09 21:00:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 1. 빌드하기

맥 os 사용할경우(필자는 맥을 이용한다.)

터미널에서 저장 폴더로 이동 후 (cd명령어 이용)

```bash
./gradlew build 
```

위의 명령어를 치면 빌드가 된다(자동으로 라이브러리가 다운받아진다)

# 2. 스프링 실행하기

build 폴더가 생성되고 완료되면

build/libs로 들어간다

jar파일을 실행한다.

```bash
java -jar <만들어진 파일>
```

스프링이 실행된다

이 파일만 복사하여 다른 곳에서 복사하여 실행시키면 서버가 자동으로 실행된다.

(실행할때 인텔리제이랑 동시실행하면 나중에 실행한 곳에서 오류가 난다.)

# 3. 빌드 clean

```bash
./gradlew clean
```

위 명령어를 입력하면

build폴더가 사라지게 된다.

다시 빌드하면 깔끔하게 생성된다.