---
title: "내용정리: 생활코딩 WEB1 - HTML & Internet"
excerpt: "내가 공부한 내용 잊어버리지 않게 기록해 둔다."
categories:
  - language
tags:
  - html
  - jekyll
last_modified_at: 2019-04-20T00:11
---



jekyll 블로그를 만들기 위해 HTML을 좀 알아야 할 것 같아서, [생활코딩 WEB1 - HTML & Internet](<https://opentutorials.org/course/3084>) 을 공부하였고, 잊어버리지 않게 중요한 것만 요약 정리한다. 

## 전체 내용 요약

HTML 관련 주용 용어

* tag
  * parent tag
  * child tag
* attribute
  * style



소개한 TAGs

* string 
* u 
* h1 
* p 
* br 
* span 
* img 
* ol 
* li 
* html 
* head 
* title 
* body 
* a 
* input 
* iframe



소개한 웹 사이트

* W3C
* http://info.cern.ch 

- [github](<https://github.com/>)
- https://www.bitballoon.com](https://www.bitballoon.com/)
- <http://neocities.org/>
- Amazon S3
- Google Cloud Storage
- Azure Blob
- DISQUS
- LiveRe
- [www.tawk.to](http://www.tawk.to/)
- 구글 애널리틱스



## 프로젝트 동기



## 기획



## 코딩과 HTML



## HTML 코딩 실습 환경 준비

[Atom](http://atom.io)

## 기본 문법 - 태그

```html
<strong>폰트 진하게</strong>
<u>밑줄</u>
```

## 혁명적인 변화

[W3C](<https://www.w3.org/>) : 웹 표준 국제 기구

```html
<h1>Heading 1</h1>
<h2>Heading 2</h2>
<h3>Heading 3</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6</h6>
```

## 통계에 기반한 학습

자주 사용하는 25개 tag  : <https://opentutorials.org/course/3084/18452>

```html
<p>단락</p>
<br>줄바꿈
```

## 줄바꿈

CSS 에 대한 아주 간단한 소개

```html
<p style="margin-top:45px;">
    contents ...<br>
</p>
```

## HTML이 중요한 이유

아래 둘 사이에 보이는 방식에는 차이가 없어 보이지만 검색엔진이 검색할 때는 차이가 있다.

```html
<h3>coding</h3>
<string><span style:font-size:22px;>coding</span></string>
```

## 최후의 문법 속성과 img

속성(attribute)

```html
<img src="imgage.jpg" width="100%">
```

## 부모 자식과 목록

Ordered List, Unordered List

```html
<ol>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ol>

<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>
```

## 문서의 구조와 슈퍼스타들

```html
<!doctype html>
<html>
    <head>
        <title>WEB1 - HTML</title>
        <meta charset="utf-8">
    </head>
    <body>
        
    </body>
</html>
```

## HTML 태그의 제왕

Anchor

```html
<a href="https://www.w3.org/TR/html5/" target="_blank" title="html specification">링크</a>
```

## 웹사이트 완성

## 원시웹

웹의 역사에 대한 소개 

최초의 웹페이지: <http://info.cern.ch>

## 인터넷을 여는 열쇠 : 서버와 클라이언트

## 웹호스팅 (github pages)

[github](<https://github.com/>)에 자신이 만든 웹페이지를 올릴 수 있고, 서비스 할 수 있음

Free static web hosting

- [https://www.bitballoon.com](https://www.bitballoon.com/)
- <http://neocities.org/>
- Amazon S3
- Google Cloud Storage
- Azure Blob

## 웹서버 소개

아파치 설치 관련 - skip

## 수업을 마치며 1

## 수업을 마치며 2

```html
<input type="checkbox">
```



## 수업을 마치며 3

front-end, back-end web 기술에 대한 종류 소개

## 부록 : 코드의 힘

## 부록 : 코드의 힘 - 동영상 삽입

```html
<iframe>
    동영상 삽입관련 태그
</iframe>
```

## 부록 : 코드의 힘 - 댓글 기능 추가

웹사이트 댓글 서비스

* DISQUS
* LiveRe

## 부록 : 코드의 힘 - 채팅 기능 추가

웹사이트 채팅 서비스

- [www.tawk.to](http://www.tawk.to/)

## 부록 : 코드의 힘 - 방문자 분석기

* 구글 애널리틱스