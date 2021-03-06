---
layout: single
title:  "GITHUB 블로그에 맨위로 버튼(GO TO TOP) 만들기"
header:
  teaser:
categories: 
  - Blog
tags:
  - GITHUB
  - Blog
date:  2021-02-03 10:05:00 +0800
last_modified_at: 2021-02-04 10:57:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "server"
---
---

깃허브 블로그에 맨위로 버튼(Go To Top) 만들기

---

# 0. 시작하기 전에

저는 html 지식이 없습니다. 

jekyll 테마를 사용하시는 분만 가능할 것 같습니다.

아래 사이트를 참고하여 만들었습니다.

- [참고 사이트 - Github 블로그 질문](https://github.com/mmistakes/minimal-mistakes/issues/1731)

- [참고 사이트 - How TO - Scroll Back To Top Button](https://www.w3schools.com/howto/howto_js_scroll_to_top.asp)

처음엔 그냥 따라해 보시고 위치, 모양, 디자인 등을 개성에 맞게 바꾸시길 바랍니다.

참고 사이트에서는 아이콘을 사용하는데 저는 더 직관적으로 TOP라는 문구를 사용합니다.

---

# 1. _layouts/default.html 수정

우선 아래와 같은 코드 부분을 찾습니다.

* 수정 전
``` html
    <div id="footer" class="page__footer">
      <footer>
```
## 1.1. 버튼 안에 글자 넣기

* 수정 후
``` html
    <div id="footer" class="page__footer">
    <aside class="go_to_top_butten">
    <a href="#site-nav"><button title="Go to top">Top</button></a>
    </aside>
      <footer>
```

\<aside class="go_to_top_butten"\> 부분은 클래스의 이름을 "go_to_top_butten" 으로 지정

\<a href="#site-nav"\> : 연결할 주소를 지정 - 원하는 기능이 사이트의 맨 위로 가는 것이면 바꾸면 안됩니다.

\<button title="Go to top"\> : 버튼의 이름 "Go to top"으로 지정

Top : 버튼에 넣을 글자

## 1.2. 버튼 안에 아이콘 넣기

만약 글자를 넣는게 아닌 아이콘을 넣고 싶다면 [아이콘 사이트](https://fontawesome.com/)에서 원하는 아이콘을 찾고 TOP 이라는 글자 대신 아래 처럼 넣으면 됩니다.

``` html
    <div id="footer" class="page__footer">
    <aside class="go_to_top_butten">
    <a href="#site-nav"><i class="fas fa-angle-double-up fa-2x"></i></a>
    </aside>
      <footer>
```
"fas fa-angle-double-up fa-2x" 이 부분에 원하는 아이콘의 이름을 넣으세요.

저는 어두운 테마의 블로그를 쓰고 있는데 아이콘 색이 잘 안보였습니다.

글씨는 사용하는 테마의 버튼 배경색과 글자색이 자동적용되는 것 같습니다.

아이콘 색은 scss 부분에서 설정해줘야 할 듯 합니다.

---

# 2. _sass/~/_buttons.scss 수정

_buttons.scss 에 넣어도 되고  _sidebar.scss에 넣어도 동작됩니다. 나중에 찾기 쉬운 혹은 수정하기 편한 곳에 넣어주세요.
{: .notice}

``` scss
.go_to_top_butten {
  position: fixed;
  bottom: 1.5em;
  right: 2em;
  z-index: 10;
  cursor: pointer;
}
```

위의 내용을 추가해 넣습니다.

저는 추후 수정하기 쉽도록 주석 바로 아래에 넣었어요. [제 블로그 참고](https://github.com/jungeu1509/jungeu1509.github.io/blob/main/_sass/minimal-mistakes/_buttons.scss)

아래는 설정하는 예시입니다. 색을 설정해야 하거나 그러면 아래 코드의 괄호 안의 항목들 중에 필요한 설정을 가져다 적절하게 바꿔보세요.

``` scss
.go_to_top_butten {
  display: none;
  position: fixed;
  bottom: 20px;
  right: 30px;
  z-index: 300;
  font-size: 18px;
  border: none;
  outline: none;
  background-color: red;
  color: white;
  cursor: pointer;
  width 400%;
  border-radius: 4px;
}
```

설정 테스트 해보는 사이트 입니다. [설정 테스트 해보는 사이트](https://www.w3schools.com/howto/tryit.asp?filename=tryhow_js_scroll_to_top)

저는 위 사이트에서도 테스트 해보고 적용한 후에

``` bash
jekyll serve
```

명령어를 입력해서 로컬 사이트에서 확인을 해봤습니다.

[로컬 사이트에서 작업 확인하는 방법](https://jamiekang.github.io/2017/04/28/working-jekyll-locally/)

---

# 다른 방법

[Vanilla Back To Top](https://github.com/vfeskov/vanilla-back-to-top)

위의 방법 이용하기

저는 사용해 보지 않았습니다.

위의 git 에서 /examples/images 에서 삽입되는 이미지를 보시고 마음에 들면 이 방법을 사용하는 것이 더 간편해 보입니다.
