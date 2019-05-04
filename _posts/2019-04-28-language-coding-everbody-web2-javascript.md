---
title: "내용정리: 생활코딩 WEB2 - JavaScript"
excerpt: "javascript 공부 내용을 기억하기 위한 기록"
categories:
  - language
tags:
  - javascript
  - jekyll
last_modified_at: 2019-04-29T19:24
---


jekyll 블로그를 만들기 위해 javaScript도 조금 알아야 할 것 같아서 [생활코딩 WEB2 - JavaScript](<https://opentutorials.org/course/3085>) 를 공부하였고, 그 내용을 정리한다.

## 수업의 목적

### HTML: `<input>` 태그, `onclick` attribute 

```html
<!-- HTML -->
<input type="button" value="night" onclick="document.querySelector('body').style.backgroundColor='black'; document.querySelector('body').style.color='white'">
```

## HTML과 JavaScript의 만남 (`script` 태그)

### JavaScript: `document.write`

```html
<!-- HTML -->
<script>
    document.write(1 + 1)
</script>
```

## HTML과 JavaScript의 만남 2 (이벤트)

### JavaScript: 대화상자 `alert`

구글에 검색: [JavaScript Events](<https://www.w3schools.com/js/js_events.asp>)

```html
<!-- HTML -->
<input type="button" value="hi" onclick="alert('hi')">
<input type="text" onchange="alert('changed')">
<input type="text" onkeydown="alert('key down!')">
```

## HTML과 JavaScript의 만남 3 (콘솔)

### JavaScript: 크롬 우클릭>검사>콘솔 이용

특정 웹페이지에서 콘솔을 열면 그 웹페이지 내의 정보를 javaScript에서 이용할 수 있다.

## 데이터타입 - 문자열과 숫자

### JavaScript: `1+1`, `string.length`

```javascript
// javaScript
1 + 1
1 - 1
1 * 1
3 / 2
'Hello world'.length
'Hello world'.toUpperCase()
'Hello world'.indexOf('w')
"1" + "1"
```

## 변수와 대입 연산자

### JavaScript: `var x = 1`

```javascript
// javaScript
var x = 1
var s = 'hello'
```

## 웹브라우저 제어

다음 토픽부터는 CSS에 대한 중요한 문법과 JavaScript로 CSS가 적용되는 tag를 어떻게 선택할 것인가에 대해 알아보자.

## CSS 기초

### CSS: 태그의 `style` attribute 이용

```html
<!-- HTML -->
<h2 style="background-color:coral; color:powderblue">
    JavaScript
</h2>
```

### CSS: 태그의 `class` attribute와 `<style>` 태그 이용

```html
<html>
    <head>
        <style>
            .js {
                font-weight:bold;
                color:red;
            }
        </style>
    </head>
    
    <body>
        <span class="js">JavaScript</span>
    </body>
</html>
```

### CSS: 선택자 우선순위, id>class>tag

```html
<html>
    <head>
        <style>
            .js {
                color:red;
            }
            #first {
                color:green;
            }
            span {
                color:blue;
            }
        </style>
    </head>
    
    <body>
        <span id="first" class="js">JavaScript</span>
        <span class="js">CSS</span>
        <span>HTML</span>
    </body>
</html>
```

## 제어할 태그 선택하기

### JavaScript: `document.querySelector`

```html
<!-- HTML -->
<input type="button" value="night" onclick="document.querySelector('body').style.backgroundColor='black'; document.querySelector('body').style.color='white'">
```

## 프로그램, 프로그래밍, 프로그래머

## 조건문 예고

## 비교 연산자와 Boolean 데이터 타입

### JavaScript: `===`, `!===`, `<`

```html
<!-- HTML -->
<h2>JavaScript 비교연산자 테스트</h2>
<span>1===1:</span>
<script>document.write((1===1) + '<br>')</script>
<span>1!==1:</span>
<script>document.write((1!==1) + '<br>')</script>
<span>1&lt;1:</span>
<script>document.write((1<2) + '<br>')</script>
```

## 조건문

### JavaScript: `if`

```html
<!-- HTML -->
<script>
    if (true) {
        
    } else {
        
    }
</script>
```

## 조건문의 활용

```html
<!-- HTML -->
<input id="night_day" type="button" value="night" onclick="
    if(document.querySelector('#night_day').value === 'night'){
      document.querySelector('body').style.backgroundColor = 'black';
      document.querySelector('body').style.color = 'white';
      document.querySelector('#night_day').value = 'day';
    } else {
      document.querySelector('body').style.backgroundColor = 'white';
      document.querySelector('body').style.color = 'black';
      document.querySelector('#night_day').value = 'night';
    }
">
```

## 리팩토링(refactoring)

### JavaScript: `this`

```html
<!-- HTML -->
<input id="night_day" type="button" value="night" onclick="
    // this는 현재 tag를 가리킨다.
    var target = document.querySelector('body');
    if(this.value === 'night'){
      target.style.backgroundColor = 'black';
      target.style.color = 'white';
      this.value = 'day';
    } else {
      target.style.backgroundColor = 'white';
      target.style.color = 'black';
      this.value = 'night';
    }
">
```

## 반복문 예고

## 배열

### JavaScript: Array

알아볼 점: document 내에서 `<script>` 태그로 분리되어 있는 javaScript가 데이터 scope가 같은가?

```html
<!-- HTML -->
<body>
    <script>
      var arr = ['a', 'b'];
    </script>

    <script>
      document.write(arr[0]);
      document.write(arr[1]);
      document.push('c');
      document.push('d');
      document.write(arr.length)
    </script>
</body>
```

## 반복문

### JavaScript: `while`

```html
<!-- HTML -->
<ul>
    <script>
        var i = 0;
        while (i < 4) {
            document.write('<li>' + i + '</li>');
            i = i + 1;
        }
    </script>
</ul>
```

## 배열과 반복문

위 코드 내용과 거의 같다.

## 배열과 반복문의 활용

### JavaScript: `document.querySelectorAll`

`node list`에 대해서 알아볼 것

```javascript
// JavaScript: 크롬 개발도구 console에서 출력; 실행 유보: shift+Enter
alist = document.querySelectorAll('a');
console.log(alist[0]);
```

```javascript
// javaScript: 모든 <a> 태그의 style color를 바꾼다.
var alist = document.querySelectorAll('a');
var i = 0;
while(i < alist.length){
    alist[i].style.color = 'powderblue';
    i = i + 1;
}
```

## 함수예고

### JavaScript: `function`

```html
<html>
    <head>
        <script>
            function foo(self) {
                self.value = 'def';
            }
        </script>
    </head>
    <body>
        <input type='button' value='abc' onclick="foo(this);">
    </body>
</html>
```

## 함수

### JavaScript: parameter, argument, `return`

```html
<html>
    <head>
        <script>
            function add(val1, val2) {
                return val1 + val2;
            }
        </script>
    </head>
    <body>
        <input type='button' value='abc' onclick="
         document.write(add(1, 2));">
    </body>
</html>
```

## 함수의 활용

### JavaScript: `this`, `self` 이해

```html
<html>
    <head>
        <script>
            function changeBodyColor(self) {
                var target = document.querySelector('body');
                target.style.color = self.value;
				if (self.value === 'red') {
					self.value = 'black';
				} else {
					self.value = 'red';
				}
            }
        </script>
    </head>
    <body>
	    <h2>change body color</h2>
        <input type='button' value='red' onclick="
           changeBodyColor(this);
         ">
    </body>
</html>
```

## 객체예고

## 객체

### JavaScript: object 만들기, 데이터 참조, 할당

```javascript
// JavaScript
var arr = {'a':1, 'b':2, 'c':3};
typeof(arr);
arr['a'];
arr.a;
arr['d'] = 4;
arr.e = 5;
```

### JavaScript: `for`

```javascript
// JavaScript
var arr = {'a':1, 'b':2, 'c':3};
for (key in arr) {
    document.write(key + ' : ' + arr[key] + '<br>');
}
```

### JavaScript: object property, method, `this`

object는 함수도 담을 수 있는데, 메소드라고 한다.

```javascript
// JavaScript
var arr = {'a':1, 'b':2, 'c':3};
arr.writeProperties = function() {
    for (key in this) {
        if (typeof(this[key]) !== 'function') {
            document.write(key + ' : ' + this[key] + '<br>');
        }
    }
}
arr.writeProperties();
```

## 객체 활용

## 파일로 쪼개서 정리 정돈하기

### HTML: `<script>`태그의 `src` attribute

```html
<!-- HTML -->
<script src="source.js"></script>
```

## 라이브러리와 프레임워크

### JavaScript: jQuery (CDN)

```html
<!-- HTML -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.0/jquery.min.js"></script>
<script>$('a').css('color', 'red')</script>
```

## UI VS API

## 수업을 마치며

### document, DOM, window, ajax 등

아래 찾아 보기

* document 객체: tag 삭제, 자식 tag 추가 등
* DOM 객체: document 객체를 봐도 알 수 없다면.
* window 객체: 현재 열려있는 web 주소, 새창, web browser 화면 크기 변경.
* ajax: webpage를 reload 하지 않고 정보를 변경.
* cookie: webpage가 reload되어도 현재 상태를 유지. 사용자를 위한 개인화된 서비스
* offline web application: 인터넷이 끈겨도 동작하는 webpage
* webRTC: 화상통신 webapp
* speech: 사용자 음성인식, 음성전달
* webGL: 3차원 그래픽
* webVR: 가상현실





