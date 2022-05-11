---
layout: single
title:  "Android앱 termux에서 vscode 실행하기"
excerpt: "vscode 서버 실행하여 브라우저에서 실행"
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
date:  2021-02-13 22:12:00 +0900
last_modified_at: 2021-03-05 18:40:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "server"
---

# [ 시리즈 목록 ]
{: .text-center}

Android앱 termux에서 vscode 실행하기

1. **Android앱 termux에서 vscode 실행하기(현재글)**

2. [Android앱 termux로 서버화하여 다양한 기기에서 vscode 실행하기](https://jungeu1509.github.io/termux/termux-vscode-server/)


---

# [ *. 시작하기 전에 ]
{: .text-center}

Termux는 안드로이드에서 리눅스 환경을 제공하는 강력한 app 입니다.

ssh등 다양한 용도로 사용 가능하기 때문에 개발자들이 사랑하는 앱입니다.

Termux는 커맨드로만 사용할 수 있기 때문에 <strike>어려운 vim 환경 대신</strike> UI를 볼 수 있는 직관적인 vscode 에디터를 사용하는 방법을 찾았습니다.

서버환경을 이용하여 브라우저로 Android에 접근하는 방법입니다.

코딩을 하며 깃을 자주 사용하는 저는 깃을 직관적으로 관리가 가능한 에디터가 너무 필요했기 때문에

앱으로 실행이 아닌 서버환경에서 프로그래밍을 해야함에도 불구하고 너무 필요했고 잘 쓰고 있습니다.

저는 Galaxy Tab +7을 보유하고 있는데 너무 해보고 싶어서 하루 꼬박 날려가며(거의 8시간...) 시도했습니다.

검색하며 여러 사이트들을 뒤지다가 아래 사이트에서 최종 참고했습니다. 

(거의 아래 사이트 번역이라고 보시면 됩니다.)

여러분들 모두 쉽게 성공하시길 바랍니다. 

[참고 사이트 : https://gist.github.com/ppoffice/b9e88c9fd1daf882bc0e7f31221dda01](https://gist.github.com/ppoffice/b9e88c9fd1daf882bc0e7f31221dda01){: .btn .btn--info}

---

# [ 0. 기본 설정 ]
{: .text-center}

안드로이드 앱 Termux를 앱스토어를 이용하여 설치후 실행합니다.

명령어 입력창이 나타나면 다음과 같이 입력합니다.

```bash
pkg update -y
```


중간중간 패키지 리스트를 업데이트 할거냐 라고 물어보는데 터미널에서 N을 추천하길래 저는 모두 N 을 눌렀습니다. 

<strike>Y를 누르면 어떻게 변하는지 잘 모르겠습니다. 댓글로 알려주세요</strike>

```bash
pkg install -y python nodejs yarn git vim-python ripgrep
```

위의 명령어로 필요한 패키지들을 미리 모두 설치하겠습니다.

설치가 완료되면 vscode 설치를 위한 기본 설정은 끝납니다.

---

# [ 1. code-server 설치 ]
{: .text-center}

## 1.1. yarn을 이용한 다운로드 

```bash
yarn global add code-server
```

위의 명령어를 이용하여 code-server를 설치해주면 됩니다.

설치하는데 시간이 생각보다 걸립니다. (저는 약 200초 정도 걸렸습니다.)

## 1.2. code-server를 실행하기 전 환경설정

바로 code-server를 실행하면 오류가 발생합니다.

node를 못 찾는 오류가 발생하는데요

환경설정 파일을 수정해주고 다시 빌드해줄겁니다.

```bash
cd ~/.config/yarn/global/node_modules/code-server/lib/vscode/node_modules/spdlog/
```
위의 명령어를 이용해 환경설정 파일 위치로 이동해줍니다.

```bash
vi binding.gyp
```

위의 명령어를 이용해 파일을 수정해줍니다.

**vim을 처음 사용하시는분을 위한 팁** : i 를 누르면 일반모드에서 입력모드로 바뀝니다.
{: .notice--success}

```
"libraries": [ "-latomic" ],
```
위의 문장을 추가해 줄겁니다. 이 명령어는 안드로이드에서 spdlog 컴파일 가능하도록 합니다.

위 문장을 적당한 위치에 추가해야된다고 합니다. 

아래와 같이 추가합니다.

```
{
  "targets": [{
    "target_name": "spdlog",
    "libraries": [ "-latomic" ],
    "sources": [
      "src/main.cc"
      "src/logger.cc"
    ],
  # 중략
}
```

저장하고 종료합니다.

**vim을 처음 사용하시는 분을 위한 팁** : ESC를 누르면 입력모드에서 일반모드로 바뀌고 :wq 명령어를 입력하면 저장후 종료됩니다.
{: .notice--success}

## 1.3. spdlog 설치

```bash
npm install
```

위의 명령어를 이용하여 다시 빌드합니다.

(선택사항)빌드가 잘됐는지 테스트를 위해 아래 명령어를 입력합니다.

```bash
npm test
```

## 1.4. ripgrep 파일 위치 맞춰주기

처음 ripgrep 파일을 다운받아 줬는데 프로그램이 이 파일을 찾을 수 있도록 링크해줘야 한다.

```bash
cd ~/.config/yarn/global/node_module/code-server/lib/vscode/node_modules/vscode-ripgrep/bin
```

위의 명령어를 이용해 위치를 이동한다.

```bash
ln -s $(which rg) .
```

위의 명령어를 이용해 rg 파일을 현재 위치에 링크해준다.

위의 명령어까지 실행하면 모두 완료 됐습니다.

고생하셨습니다.

---

# [ 2. 테스트 실행해보기 ]
{: .text-center}

```bash
cd ~
```
홈 디렉토리로 이동합니다(리눅스 환경 처음 쓰시는 분들을 위해...)

```bash
code-server --auth none --disable-telemetry
```

위의 명령어를 이용하면 지금 사용하는 기기에서 vscode를 실행하실 수 있습니다.

설치한 기기에서 멀티테스킹을 이용하여 브라우저를 열어줍니다. (크롬, 파이어폭스, 삼성인터넷 등)

주소에 https://[localhost]:8080 을 입력해줍니다.

**note:** localhost는 127.0.0.1 일겁니다.
즉 https://127.0.0.1:8080 을 입력하시면 됩니다. 
{: .notice--success}

---

# [ *. 글을 마치며 ]
{: .text-center}

![VscodeAtBrowser](/assets/images/posts/termux/20210213/vscode.jpg "VscodeAtBrowser")

위 이미지 처럼 vscode 환경이 잘 실행되나요?

컴퓨터에 설치하여 사용하는 것과 서버로 사용하는 것이 약간 차이가 있긴 합니다만

명령어만 입력하는 창보다 <strike>어려운 vim 대신</strike> **UI 환경**이 더 익숙한 분들이 많을겁니다.

다른 장점도 있습니다!

브라우저에서 실행하는 것이기 때문에 ctrl+s를 누르지 않아도 **자동저장**되고 좋습니다.

## *. 다음 게시물에서는?

또한 같은 네트워크 다른 기기의  브라우저에서 접속도 가능합니다.

다음 게시물에서 이 방법을 알려드리도록 하겠습니다.
