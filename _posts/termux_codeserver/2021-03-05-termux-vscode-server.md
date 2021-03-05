---
layout: single
title:  "Android앱 termux로 서버화하여 다양한 기기에서 vscode 실행하기"
excerpt: "vscode 서버 실행하여 다양한 기기의 브라우저에서 실행방법"
header:
  teaser:
search:
permalink:
categories: 
  - termux
tags:
  - termux
  - android
  - vscode
  - linux
  - 갤럭시탭
date:  2021-03-05 16:05:00 +0800
last_modified_at: 2021-02-05 16:05:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "server"
---

# [ 시리즈 목록 ]

Android앱 termux에서 vscode 실행하기

1. [Android앱 termux에서 vscode 실행하기](/_posts/termux/2021-03-15-termux-vscode)

2. **Android앱 termux로 서버화하여 다양한 기기에서 vscode 실행하기(현재글)**


---

# [ *. 이전글에서]


안드로이드 앱 termux를 이용하여 사용하는 안드로이드 기기 브라우저를 이용해서 vscode를 실행시키는 것을 정리했습니다.

저는 Galaxy Tab +7을 이용하여 키보드와 마우스를 연결하여 코드 작성하고 깃과 연동하여 사용합니다.

그러나 집에서 굳이 데스크탑을 놔두고 갤럭시탭을 쓸 이유가 없기 때문에 데스크탑에서 사용 하는 방법을 정리해 봤습니다.

검색하며 여러 사이트들을 뒤지다가 아래 사이트에서 최종 참고했습니다. 

이전글에서 termux 설치 및 환경설정하는 것을 알려드렸으니 보고 오시길 바랍니다.

(이 글은 거의 아래 사이트 번역이라고 보시면 됩니다.)

여러분들 모두 쉽게 성공하시길 바랍니다. 

[참고 사이트:https://gist.github.com/ppoffice/b9e88c9fd1daf882bc0e7f31221dda01](https://gist.github.com/ppoffice/b9e88c9fd1daf882bc0e7f31221dda01){: .btn .btn--info}

---

# [ 1. 안드로이드 로컬환경 에서 실행 ]
{: .text-center}

홈 디렉토리로 이동합니다
```bash
cd ~
```

(리눅스 환경을 처음 쓰시는 분들을 위해...)

이제 vscode를 실행시킬건데 우리가 사용할 명령어는 code-server입니다.

```bash
code-server --auth none --disable-telemetry
```

위의 명령어를 이용하면 지금 사용하는 기기의 브라우저(크롬, 파이어폭스, 삼성인터넷 등)에서 vscode를 실행하실 수 있습니다.

멀티테스킹을 이용하여 브라우저를 실행하고

주소에 https://[localhost]:8080 을 입력해줍니다.

**note:** localhost는 127.0.0.1 일겁니다.
즉 https://127.0.0.1:8080 을 입력하시면 됩니다. 
{: .notice--success}

여기서 사용한 명령어들을 좀 보면

--auth 는 인증 방식을 선택하는 것인데 로컬에서 사용하므로 인증 안해도 이용을 가능하게 한 것입니다. none선택을 안하면 비밀번호를 입력해야 합니다.
(비밀번호는 [2. 동일 네트워크환경의 다른 기기에서 실행](# [ 2. 동일 네트워크환경의 다른 기기에서 실행 ])에서 설명하겠습니다.)

--disable-telemetry는 사용하는 기기를 모니터하는 기능을 끄겠다는 것입니다.

---

# [ 2. 동일 네트워크환경의 다른 기기에서 실행 ]
{: .text-center}


## HTTP 사용

저는 안드로이드가 아닌 컴퓨터에서 사용하고 싶었기 때문에 이 기능을 사용했습니다.

우선 동일 네트워크에 있어야 합니다.

```bash
code-server --bind-addr 0.0.0.0:8080 --disable-telemetry
```
--bind-addr 명령어를 이용하여 모든 네트워크 인터페이스에 HTTP 서비스를 노출합니다.(쉽게 말하면 외부기기에서 안드로이드의 와이파이등의 통신환경을 이용하여 기기의 코드서버로 접속할 수 있게 합니다.)

여기서 --auth 명령어를 사용하지 않은 것을 알 수 있습니다.

그렇다면 비밀번호를 입력해야 하는데 비밀번호는 

```bash
cat ~/.config/code-server/config.yaml
```

위의 명령어를 치면 아래 사진처럼 알려줄 겁니다.

![1번그림-비밀번호](/_posts/termux/img/Termux_1.jpg "vscode-server_pw")

**코드서버 실행하니 명령어를 안먹어요!** 두가지 방법이 있습니다. 1. 터미널을 하나 더 실행합니다. 2. ctrl + C 를 이용하여 우선 서버를 중지 시킵니다. 위의 code-server 명령맨뒤에 &을 실행하면 백그라운드에서 명령이 실행됩니다.
{: .notice--success}

 **백그라운드 실행시 주의!**백그라운드에서 명령이 실행되면 앱이 종료되지 않는한 계속 실행되고 있는 상태입니다. 꼭 앱을 종료하여 백그라운드 실행을 꺼주세요. [명령어로 백그라운드 실행을 종료하는 법 참고 블로그](https://valuefactory.tistory.com/164)
{: .notice--warning}

(사실 위의 네모들은 리눅스를 사용하는 분들이라면 껌이죠...?)

cat은 concatenate(연결하다)의 동의어인 catenate에서 유래되었다고 합니다. 

파일을 연결하고 표시해주는 명령어 입니다.

## HTTPS 사용

브라우저에서 Visual Studio Code의 클립 보드 및 기타 기능을 사용하려면 HTTPS를 사용하도록 설정해야 할 수도 있습니다. 

HTTPS를 사용하려면 openssl-tool을 설치하고 시작시 code-server가 인증서를 생성하도록합니다.

우선 openssl-tool 이란 패키지를 하나 설치해야 합니다.

```bash
pkg install openssl-tool
```

다 됐습니다.

아래처럼 --cert라는 명령어를 추가해 code-server가 인증서를 생성하도록합니다.

```bash
code-server --bind-addr 0.0.0.0:8080 --cert --disable-telemetry
```

서버가 실행 됐습니다.

다른 기기의 브라우저에서 접속하려면 안드로이드 기기의 IP주소를 알아야 합니다.

```bash
ifconfig
```
 위 명령어를 사용하면 아래 사진처럼 각 인터페이스별 IP주소를 알려줍니다.
