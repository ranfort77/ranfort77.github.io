---
title: "내용정리: 생활코딩 WEB2 - CSS"
excerpt: "공부 내용을 기억하기 위한 기록"
categories:
  - language
tags:
  - css
  - jekyll
last_modified_at: 2019-04-20T20:32
---



jekyll 블로그를 만들기 위해 CSS도 조금 알아야 할 것 같아서 [생활코딩 WEB2 - CSS](<https://opentutorials.org/course/3086>) 을 공부하였고, 내용을 요약 정리해 둔다.

## 전체 내용 요약

CSS 관련 주용 용어

* 선택자 (selector)
* declaration
  * property
  * value



소개한 HTML TAGs

- font
- style
- div
- span
- link



소개한 HTML attributes

* class
- id



소개한 css property:value

- color:red
- text-decoration:none
- font-size:45px
- text-align:center
- border
- padding
- margin
- display
- width



그리드 관련 property:value
- display:grid
- grid-template-columns: 150px 1fr



반응형 디자인

- @media(max-width:800px)



## CSS 등장 이전의 상황

HTML에서 font tag를 사용해서 다른색을 사용할 수 있다. 

```html
<h1><a href="index.html"><font color="red">WEB</font></a></h1>
```

그러나 HTML안에 font 같은 tag는 design 요소로, 정보로써 직접 관련이 없다. 

## CSS 등장

특정 html tag 전체에 대해서 design 요소를 추가하기 위해 style tag안에 css 문법을 사용할 수 있다. web page의 design 요소만 빼 온 것이 css이다.

```html
<!--
HTML 주석 처리; 아래 코드의 a는 selector이고 color:red;는 declaration 이다. 
declaration에서 color는 property이고 red는 value이다.
-->
<head>
    <style>
        a {
            color:red;
        }
    </style>    
</head>

<body>
    <ol>
        <li><a href="1.html">WEB</a></li>
        <li><a href="2.html">HTML</a></li>
        <li><a href="3.html">CSS</a></li>
    </ol>
</body>
```

## 혁명적 변화

```html
<!-- 
a tag에 대한 css가 정의되어 있더라도, a tag안에 style attribute로 정의해 둔 것이 더 우선한다.
-->
<head>
    <style>
        a {
            color:red;
            text-decoration: none;
        }
    </style>    
</head>

<body>
	<a href="1.html" style="color:red; text-decoration:underline">CSS</a>    
</body>
```

## CSS 속성을 스스로 알아내는 방법

text 크기를 바꾸는 css 을 알고 싶으면 `css text size property` 같은 것을 검색 할 것.

```html
<!-- 
검색한 결과를 이용해서 아래와 같이 style을 구성한다.
-->
<style>
    h1 {
      font-size: 45px;
      text-align: center;
    }
</style>
```

## CSS 선택자를 스스로 알아내는 방법

[css selector 형식](<https://www.w3schools.com/cssref/css_selectors.asp>)

```html
<!--
선택자 우선 순위: id > class > tag
                id는 웹페이지 내에 딱 한번만 사용할 것을 권고
-->
<head>
    <style>
        /* id selector는 #을 이용, class selector는 .을 이용 */
        #active {
            color:red;
        }    
        .saw {
          color:gray;
        }
        a {
            color:black;
        }
    </style>    
</head>

<body>
    <!--
    class attribute의 값은 공백을 통해 여러 값을 지정할 수 있음을 알 수 있다.
    -->
	<a href="1.html" class="saw active" id="active">HTML</a>    
</body>
```

## 박스모델

* block elements, inline elements의 이해
* 박스모델: border, margin, padding, display, width 등의 속성을 이해

```html
<style>
    h1, a {
        border:5px solid red;
        padding:20px;  /* 테두리와 내용 사이의 간격 */
        margin;20px;   /* 테두리와 테두리 사이의 간격 */
        display:block; /* a tag는 default가 inline이지만 block으로 */
        width:100px;   /* 화면전체가 아니라 100px만큼 block */
    }
</style>
```

* 박스모델 써먹기

```html
<!-- 박스에 줄 긋는 방법 -->
<style>
    h1 {
        border-bottom:1px solid gray;
    }
    ol {
        border-right:1px solid gray;
        width:100px;
    }
</style>
```

## 그리드

* design 을 위한 tag `<div>`, `<span>`
* grid 를 위한 `display:grid;`, `grid-template-columns` 속성 이해
* grid를 사용할 수 있는지 확인: [Can I Use](<https://caniuse.com/#search=grid>)

```html
<html>
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style>
    #grid {
      border:5px solid pink;
      display:grid;
      grid-template-columns: 150px 1fr;
    }
    div {
      border:5px solid gray;
    }
  </style>
</head>
<body>
  <div id="grid">
    <div>NAVIGATION</div>
    <div>ARTICLE</div>
  </div>
</body>
</html>
```

## 반응형 디자인

* 미디어 쿼리, 반응형 디자인, 반응형 웹, responsive web design

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
    <style>
      div{
        border:10px solid green;
        font-size:60px;
      }
      @media(max-width:800px) {
        div{
          display:none;
        }
      }
    </style>
  </head>
  <body>
    <div>
      Responsive
    </div>
  </body>
</html>
```

## CSS 코드의 재사용

* `style.css` 파일에 css 코드를 입력하고 `link` tag로 style를 가져올 수 있다.

```html
<!doctype html>
<html>
<head>
    <title>WEB - CSS</title>
    <meta charset="utf-8">
    <link rel="stylesheet" href="style.css">
</head>
    <body>
    	<p>contents</p>
    </body>
</html>
```

## 수업을 마치며

