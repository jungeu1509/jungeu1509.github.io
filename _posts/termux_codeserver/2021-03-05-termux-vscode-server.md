---
layout: single
title:  "Android앱 termux로 서버화하여 다양한 기기에서 vscode 실행하기"
excerpt: "vscode 서버 실행하여 다양한 기기의 브라우저에서 실행방법"
header:
  teaser: /assets/images/posts/termux/termux.jpg
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
date:  2021-03-05 16:05:00 +0900
last_modified_at: 2021-03-05 18:43:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "server"
---

# [ 시리즈 목록 ]
{: .text-center}

Android앱 termux에서 vscode 실행하기

1. [Android앱 termux에서 vscode 실행하기](https://jungeu1509.github.io/termux/termux-vscode-run/)

2. **Android앱 termux로 서버화하여 다양한 기기에서 vscode 실행하기(현재글)**


---

# [ *. 이전글에서]
{: .text-center}

안드로이드 앱 termux를 이용하여 사용하는 안드로이드 기기 브라우저를 이용해서 vscode를 실행시키는 것을 정리했습니다.

저는 Galaxy Tab +7을 이용하여 키보드와 마우스를 연결하여 코드 작성하고 깃과 연동하여 사용합니다.

그러나 집에서 굳이 데스크탑을 놔두고 갤럭시탭을 쓸 이유가 없기 때문에 데스크탑에서 사용 하는 방법을 정리해 봤습니다.

검색하며 여러 사이트들을 뒤지다가 아래 사이트에서 최종 참고했습니다. 

이전글에서 termux 설치 및 환경설정하는 것을 알려드렸으니 보고 오시길 바랍니다.

(이 글은 거의 아래 사이트 번역이라고 보시면 됩니다.)

여러분들 모두 쉽게 성공하시길 바랍니다. 

[참고 사이트 : https://gist.github.com/ppoffice/b9e88c9fd1daf882bc0e7f31221dda01](https://gist.github.com/ppoffice/b9e88c9fd1daf882bc0e7f31221dda01){: .btn .btn--info}

---

# [ 1. 안드로이드 로컬환경 에서 실행 ]

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

## 2.1. HTTP 사용

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

![1번그림-비밀번호](/assets/images/posts/termux/20210305/Termux_1.jpg "vscode-server_pw")

**코드서버 실행하니 명령어를 안먹어요!** 두가지 방법이 있습니다. 1. 터미널을 하나 더 실행합니다. 2. ctrl + C 를 이용하여 우선 서버를 중지 시킵니다. 위의 code-server 명령맨뒤에 &을 실행하면 백그라운드에서 명령이 실행됩니다.
{: .notice--success}

 **백그라운드 실행시 주의!**백그라운드에서 명령이 실행되면 앱이 종료되지 않는한 계속 실행되고 있는 상태입니다. 꼭 앱을 종료하여 백그라운드 실행을 꺼주세요. [명령어로 백그라운드 실행을 종료하는 법 참고 블로그](https://valuefactory.tistory.com/164)
{: .notice--warning}

(사실 위의 네모들은 리눅스를 사용하는 분들이라면 껌이죠...?)

cat은 concatenate(연결하다)의 동의어인 catenate에서 유래되었다고 합니다. 

파일을 연결하고 표시해주는 명령어 입니다.

## 2.2. HTTPS 사용

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

![2번그림-IP](/assets/images/posts/termux/20210305/Termux_2.jpg "vscode-server_ip")

lo 는 로컬 서버입니다.

그 외에 몇 가지가 있을텐데 inet뒤의 00.00.00.00 부분이 IP 입니다.

저는 이름이 통일되어 있어서 뭐가 뭔지 모르겠네요... 저와 비슷하게 나오는 분들은 두개를 아래부터 차례로 해보세요

같은 네트워크 내의 다른 기기 브라우저에서 저 ip를 입력하면 안드로이드 기기에서 실행되는 코드서버를 사용하실 수 있습니다. (비밀번호를 입력하셔야 합니다.)

즉, https://[IP]:8080 을 주소창에 입력하면 됩니다!

---

# [ 3. 글을 마치며 ]

갤럭시 탭을 사용하면서 참 놀랍다고 생각합니다.

안드로이드 기기가 서버가 될 수도 있고, 외장메모리(?)가 될 수도 있고, 휴대용 통신 기기가 될 수도 있고. 다양한 기능으로 활용할 수 있습니다.

요즘은 심지어 멀티테스킹이 너무 잘 되어서 여러작업들을 동시에 원활히 가능합니다.

화면도 휴대전화에 비해 큰 편이라 개발하기 너무 좋습니다.(물론 노트북에 비할 수는 없지만)

가벼운 한대의 기기로 유튜브를 보면서 폰 게임을 하면서 개발도 하는 대단한 물건이라고 생각합니다.(카톡도 중간중간 하고) 아주 강추합니다.

제 목표는 gui환경에서 깃을 이용하고 코드를 관리하는 환경을 이용하는 것인데 충분히 활용 가능하다 생각합니다.

개발자 분들 중에서 아이패드와 갤럭시탭을 고민하시는 분들은 이런 것도 참고하시면 좋을 것 같아요. 

(아 물론 고사양 노트북이 갑이지만요.)
