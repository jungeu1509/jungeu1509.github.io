---
layout: single
title:  "HTTP란 무엇인가?"
excerpt: "면접 준비 스터디"
header:
  teaser: 
search: true
permalink:
categories: 
  - Interview
tags:
  - Interview
  - Http
  - Network
date:  2022-02-23 13:25:00 +0900
last_modified_at: 2022-02-23 13:25:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# HTTP 란?

HTTP는 'HyperText Transfer Protocol'의 줄임말로 www상에서 사용하는 프로토콜이다. 즉, 인터넷에서 데이터를 주고받을 수 있는 프로토콜이다.

1990년 대 팀 버너스리가 월드와이드웹을 만들어서 하이퍼텍스트 문서들을 주고 받기 위한 규약으로 만든 것이 HTTP이다. 현재는 문서들 뿐만 아니라 이미지, 비디오, 음성 등 거의 모든 형식의 데이터를 전송하는데 사용 되고있다.

HTTP는 서버와 클라이언트 사이에 요청과 응답을 주고 받는 프로토콜로 우리가 흔히 웹브라우저 주소창에 입력하는 웹 주소인 URL을 통해 요청과 응답이 이루어진다.

실제 전송은 TCP를 통해 이루어 지며 포트는 80번을 이용한다.

# HTTP 동작

클라이언트 즉, 사용자가 브라우저를 통해서 어떠한 서비스를 url을 통하거나 다른 것을 통해서 요청(request)을 하면 서버에서는 해당 요청사항에 맞는 결과를 찾아서 사용자에게 응답(response)하는 형태로 동작한다.

- 요청 : client -> server
- 응답 : server -> client

HTML 문서만이 HTTP 통신을 위한 유일한 정보 문서는 아니다.

Plain text로 부터 JSON 데이터 및 XML과 같은 형태의 정보도 주고 받을 수 있으며, 보통은 클라이언트가 어떤 정보를 HTML 형태로 받고 싶은지, JSON 형태로 받고 싶은지 명시해주는 경우가 많다.

# HTTP 특징

HTTP 메시지는 HTTP 서버와 HTTP 클라이언트에 의해 해석이 된다.

TCP/IP를 이용하는 응용 프로토콜이다. (컴퓨터와 컴퓨터간에 데이터를 전송 할 수 있도록 하는 장치로 인터넷이라는 거대한 통신망을 통해 원하는 정보(데이터)를 주고 받는 기능을 이용하는 응용 프로토콜)

HTTP는 연결 상태를 유지하지 않는 비연결성 프로토콜이다. (이러한 단점을 해결하기 위해 Cookie와 Session이 등장하였다.)(연결상태를 유지하는 프로토콜은 FTP, Telnet이 있다.)

HTTP는 연결을 유지하지 않는 프로토콜이기 때문에 요청/응답 방식으로 동작한다.

# HTTP 구조

HTTP  메시지는 ASCII 로 인코딩된 텍스트로 되어 있다. 

기본적인 메시지 구조는 다음과 같다.

- 시작줄
- HTTP 헤더
- 공백
- 바디

HTTP 메시지는 기본적으로 클라이언트가 요청하고 서버가 응답하는 구조이기 때문에 메시지는 요청이냐 응답이냐에 따라 각 메시지의 구성 내용이 달라 진다.

## 요청 구조

- 시작줄 : HTTP 메서드( GET, POST, PUT ... ), 요청 URL, HTTP 버전
- HTTP 헤더 : request 헤더 ( Host, User-Agent, Accept ... ), general 헤더 ( Connection ... ), entity 헤더 ( Content-Type ... )
- 공백
- 바디 : 데이터 등

## 응답 구조

- 시작줄 : HTTP 버전, 상태 코드 ( 200, 404 ... ), 상태 텍스트 ( Not Found ... )
- HTTP 헤더 : response 헤더 ( Server, Set-Cookie, Age ... ), general 헤더 ( Connection ... ), entity 헤더 ( Content-Type ... )
- 공백
- 바디 : 데이터 등

# 참고자료

- [인생의 로그캣](https://noahlogs.tistory.com/34)
- [surim's develog - HTTP란 무엇인가?](https://velog.io/@surim014/HTTP란-무엇인가)
- [zerocho.com](https://www.zerocho.com/category/HTTP/post/5b344f3af94472001b17f2da)
- [토마's - HTTP란 무엇인가?](https://toma0912.tistory.com/69)
- [ROYDEST - HTTP란?](https://roydest.tistory.com/entry/HTTP%EB%9E%80)
