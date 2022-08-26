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
date:  2021-02-03 10:05:00 +0900
last_modified_at: 2022-05-12 10:57:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "server"
---
---

깃허브 블로그에 맨위로 가는 버튼(Go To Top) 만들기

---

# 0. 시작하기 전에

**저는 프론트엔드(html, css 등) 지식이 없습니다.**

응용 가능할 것 같기도 하지만 저는 jekyll 테마를 사용했기 때문에 테마 적용한사람을 위한 방법입니다. (프론트를 잘 아시는분들은 껌일지도...?)

아래 사이트를 참고하여 만들었습니다.

- [참고 사이트 - Github 블로그 질문](https://github.com/mmistakes/minimal-mistakes/issues/1731)

- [참고 사이트 - How TO - Scroll Back To Top Button](https://www.w3schools.com/howto/howto_js_scroll_to_top.asp)

처음엔 그냥 따라해 보시고 위치, 모양, 디자인 등을 개성에 맞게 바꾸시길 바랍니다.

참고 사이트에서는 아이콘을 사용하는데 저는 더 직관적으로 'TOP'라는 문구를 사용합니다.

---

# 1. footer 찾기

우선 footer를 찾기위해 아래와 같은 코드 부분을 찾습니다.

```html
    <div id="footer" class="page__footer">
      <footer>
```

만약 이 부분에 아래와 같이 footer/custom.html 이라고 써있는 코드가 있다면 _includes/footer/custom.html 에 넣으시는게 제작자의 의도 같습니다.

```html
    <div id="footer" class="page__footer">
      <footer>
        {% include footer/custom.html %} 
        {% include_cached footer.html %}
      </footer>
    </div>
```

없어도 <footer> 부분만 찾으셨으면 상관없습니다! 

## 1.1. 버튼 안에 글자 넣기

1.1.1. 절과 1.1.2. 둘중 편한 위치에 넣으시면 됩니다. 다만 저는 나중에 수정과 클린코드를 위해 1.1.1. 방법을 더 추천합니다.

custom.html이 없을경우 1.1.2.를 따라하시면 됩니다.

### 1.1.1. _include/footer/custom.html 위치에 코드 넣기

이곳은 수정하지 않았다면 비어있는 곳입니다.

개발자라면 도화지를 만나 두근두근한 상태인데요(?)

아래와 같이 코드를 넣어주면 이쁘게 버튼이 생길겁니다.

* 수정 전
```html
<!-- start custom footer snippets -->
<!-- end custom footer snippets -->
```

* 수정 후
```html
<!-- start custom footer snippets -->
<aside class="go_to_top_butten">
    <a href="#site-nav"><button title="Go to top">TOP</button></a>
</aside>
<!-- end custom footer snippets -->
```

수정 전에있던게 주석이라 코드와 상관없는 줄이긴 하지만 나중에 페이지에서 알아보기 쉽게 주석 두줄사이에 버튼코드를 넣어줍니다.

### 1.1.2. _layouts/defalt.html에 코드 넣기

* 수정 전
  
```html
<div id="footer" class="page__footer">
  <footer>
```

* 수정 후
    
```html
<div id="footer" class="page__footer">
<aside class="go_to_top_butten">
    <a href="#site-nav"><button title="Go to top">TOP</button></a>
</aside>
  <footer>
```

### 1.1.3. 삽입한 코드 설명

\<aside class="go_to_top_butten"\> 부분은 클래스의 이름을 "go_to_top_butten" 으로 지정

\<a href="#site-nav"\> : 연결할 주소를 지정 - 원하는 기능이 사이트의 맨 위로 가는 것이면 바꾸면 안됩니다.

\<button title="Go to top"\> : 버튼의 이름 "Go to top"으로 지정

TOP : 버튼에 넣을 글자

버튼에 보여지는 글을 바꾸고 싶으시다면 TOP 부분을 변경하시면 됩니다. 아이콘을 넣고 싶으시면 1.2. 절을 보세요!

## 1.2. 버튼 안에 아이콘 넣기

만약 글자를 넣는게 아닌 아이콘을 넣고 싶다면 [아이콘 사이트](https://fontawesome.com/)에서 원하는 아이콘을 찾고 TOP 이라는 글자 대신 아래 처럼 넣으면 됩니다.

```html
<aside class="go_to_top_butten">
  <a href="#site-nav"><i class="fas fa-angle-double-up fa-2x"></i></a>
</aside>
```
"fas fa-angle-double-up fa-2x" 이 부분에 원하는 아이콘의 이름을 넣으세요.

저는 어두운 테마의 블로그를 쓰고 있는데 아이콘 색이 잘 안보였습니다.

글씨는 사용하는 테마의 버튼 배경색과 글자색이 자동적용되는 것 같습니다.

아이콘 색은 scss 부분에서 설정해줘야 할 듯 합니다.

---

# 2. 버튼 위치 등 수정

_sass/minimal-mistakes/_buttons.scss 파일로 들어갑니다.

_buttons.scss 에 넣어도 되고  _sidebar.scss에 넣어도 동작됩니다. 나중에 찾기 쉬운 혹은 수정하기 편한 곳에 넣어주세요.
{: .notice}

```scss
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

아래는 설정하는 예시입니다. 색을 설정해야 하거나 그러면 아래 코드의 괄호 안의 항목들 중에 필요한 설정을 가져다 적절하게 바꿔보세요. 해석 가능하다면 직관적이기 때문에 코드설명은 하지 않겠습니다.

```scss
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

```bash
jekyll serve
```

명령어를 입력해서 로컬 사이트에서 확인을 해봤습니다.~~(사실 바로 적용하는게 편하긴 하죠)~~

[로컬 사이트에서 작업 확인하는 방법](https://jamiekang.github.io/2017/04/28/working-jekyll-locally/)

# 3. 적용한 모습

![My Blog Homepage](/assets/images/posts/Blog/Go_To_Top/01_my_blog_homepage.png)

---

# 다른 방법

[Vanilla Back To Top](https://github.com/vfeskov/vanilla-back-to-top)

위의 방법 이용하기

저는 사용해 보지 않았습니다.

위의 git 에서 /examples/images 에서 삽입되는 이미지를 보시고 마음에 들면 이 방법을 사용하는 것이 더 간편해 보입니다.
