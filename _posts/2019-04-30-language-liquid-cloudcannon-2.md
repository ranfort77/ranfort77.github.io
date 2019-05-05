---
title: "Jekyll & Liquid 시작하기 (2)"
excerpt: "CloudCannon 의 liquid 튜토리얼 공부 두 번째"
categories:
  - language
tags:
  - jekyll
  - liquid
last_modified_at: 2019-05-01T23:28
---



[Jekyll & Liquid 시작하기 (1)]({{ site.url }}/language/language-liquid-cloudcannon-1)에 이은 두 번째 정리

주요 관련 사이트 링크:

* [jekyll](<https://jekyllrb-ko.github.io/>)
* [liquid](<http://shopify.github.io/liquid/>)
* [CloudCannon](<https://cloudcannon.com/>)
* [YAML](<https://yaml.org/start.html>)

## 6. includes

{% raw %}

### {% include filename %}

`<site root>/_includes/` 폴더를 만들고, 안에 `head.html`을 아래와 같이 입력한다.

```html
<head>
  <meta charset="UTF-8">
  <title>{{ page.title }}</title>
</head>
```

`_includes`에 있는 코드는 `{% include <filename> %}` 방식으로 어디서든 내용을 포함 시킬 수 있다. `<site root>/_layouts/default.html`을 아래와 같이 수정한다.

```html
<!DOCTYPE html>
<html>
{% include head.html %}
<body>
  {{ content }}
</body>
</html>
```

`<site root>/_layouts/page.html`을 원래대로 아래와 같이 작성한다.

```html
---
layout: default
---
<h2>start page layout</h2>
{{ content }}
```

마찬가지로 `<site root>/index.html`을 원래대로 아래와 같이 작성한다.

```html
---
layout: page
title: My Homepage
---
<h1>Introduction to layouts</h1>
```

build 결과 `<site root>/_site/index.html`는 다음과 같다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>My Homepage</title>
</head>
<body>
  <h2>start page layout</h2>
<h1>Introduction to layouts</h1>
</body>
</html>
```

### include에 parameters 전달

`<site root>/_includes/foo.html` 의 내용이 다음과 같고,

```html
<h3 style="color:{{ include.color }}">foo.html in includes</h3>
```

`<site root>/temp.html`의 내용이 아래와 같다고 하자.

```html
---
---
{% include foo.html color="red" %}
```

`build`후에 `<site root>/_site/temp.html`의 결과는 다음과 같다.

```html
<h3 style="color:red">foo.html in includes</h3>
```

{% endraw %}

## 7. Liquid 흐름제어문

{% raw %}

[CloudCannon 튜토리얼7](<https://learn.cloudcannon.com/jekyll/control-flow-statements-in-liquid/>) 에서는 다음과 같은 내용을 소개하고 있다.

* collection을 만드는 방법
* collection을 참조하는 방법(`site.`을 이용)
* collection과 `for`, `if` 등을 조합한 예제
* 비교 연산자 `!=`, `<` , `>=`
* 특정 문자열이 속해 있는가? `{% if "Chocolate" contains "Choco" %}`
* `unless`
* `case` & `when`

다른 것들은 이미 살펴보았거나 앞으로 다시 나올 것이기 때문에 여기서는 `unless`와 `when`의 기능만 살펴본다.

### unless

아래 파일은 `ex7-1.html`이다. `unless`는 "조건이 아니면"의 의미로 `if`와 반대이다. `{% unless item.score >= 90 %}`은 `{% if item.score < 90 %}` 과 같다. 닫는 문법이 `{% endunless %}` 임을 주의하라.

```html
---
obj:
  - name: abc
    score: 90
  - name: def
    score: 95
  - name: ghi
    score: 80
---
{% for item in page.obj %}
  {% unless item.score >= 90 %}
    <h2>name={{ item.name }}, score={{ item.score }}</h2>
  {% endunless %}
{% endfor %}
```

`build` 결과는 아래와 같다. (이제 부터 빈줄은 생략한다.)

```html
    <h2>name=ghi, score=80</h2>
```

### when

아래 파일은 `ex7-2.html`이다. 

```html
---
obj:
  - name: abc
    id: 1
  - name: def
    id: 2
  - name: ghi
    id: 3
  - name: jkl
    id: 2
---
{% for item in page.obj %}
  {% case item.id %}
    {% when 1 %}
       <h2>{{ item.name }} : apple</h2>
    {% when 2 %}
       <h2>{{ item.name }} : banana</h2>
    {% when 3 %}
       <h2>{{ item.name }} : melon</h2>
  {% endcase %}
{% endfor %}
```

`{% when 1 %}` 은 `{% if item.id == 1 %}` 과 동일한 쓰임이지만 좀 더 깔끔해 보인다. `build` 결과는 다음과 같다.

```html
       <h2>abc : apple</h2>
       <h2>def : banana</h2>
       <h2>ghi : melon</h2>
       <h2>jkl : banana</h2>
```

{% endraw %}

## 8. 문자열 필터 (String filters)

{% raw %}

이미 한번 나왔었던 필터에 대한 더 자세한 예시를 보여준다.  필터에 대한 더 많은 자료는 [Liquid](<https://shopify.github.io/liquid/>)를 참고할 것. 아래 표 데이터를 보면서 기능을 어떤게 있구나 정도로 넘어가고 나중에 필요할 때 다시 찾아보면 될 것 같다.

| 필터                                                         | 결과                         |
| :----------------------------------------------------------- | ---------------------------- |
| `{{ "apple pie" | replace: "apple", "banana" }}` | banana pie |
| `{{ "cupcake" | prepend: "chocolate " }}`                    | chocolate cupcake            |
| `{{ "lemon" | append: " cake" }}`                            | lemon cake                   |
| `{{ "i like cupcakes" | capitalize }}`                       | I like cupcakes              |
| `{{ "BakeryStore" | downcase }}`                             | bakerystore                  |
| `{{ "apple pie" | upcase }}`                                 | APPLE PIE                    |
| `{{ "muffin&cupcake?" | cgi_escape }}`                       | muffin%26cupcake%3F          |
| `{{ "<h1>Banana Split</h1>" | escape }}`                     | <h1>Banana Split</h1>        |
| `{{ "blueberry muffin.html" | slugify }}`                    | blueberry-muffin-html        |
| `{{ "<h1>Greentea cheesecake</h1>" | strip_html }}`          | Greentea cheesecake          |
| `{{ "**Sour dough** bread" | markdownify }}`                 | **Sour dough** bread         |
| `{{ "I really really like cupcakes" | remove_first: 'really' }}` | I really like cupcakes       |
| `{{ "I really really like cupcakes" | remove: 'really' }}`   | I like cupcakes              |
| `{{ "I really really like cupcakes" | replace_first: "really", "truly" }}` | I truly really like cupcakes |
| `{{ "I really really like cupcakes" | replace: "really", "truly" }}` | I truly truly like cupcakes  |
| `{{ "Carrot cake" | size }}`                                 | 11                           |
| `{{ "Peanut butter cheesecake" | number_of_words }}`         | 3                            |
| `{{ "Souffle" | slice: 0 }}`                                 | S                            |
| `{{ "Souffle" | slice: 1 }}`                                 | o                            |
| `{{ "Souffle" | slice: -2 }}`                                | l                            |
| `{{ "Souffle" | slice: 2,4 }}`                               | uffl                         |
| `{{ "apple,banana,carrot" | split:"," | jsonify }}`          | ["apple","banana","carrot"]  |
| `{{ "The freshest bread in San Francisco" | truncate: 15 }}` | The freshest...              |
| `{{ "Who ate all the cupcakes?" | truncatewords: 3 }}`       | Who ate all...               |

{% endraw %}

## 9. 반복 (Loop)

{% raw %}

이 tutorial에서는 loop 를 사용하는데 여러가지 기능들을 더 소개해 주고 있다.

### cycle

아래 파일은 `ex9-1.html`이다. `{% cycle <items> %}` 는 호출될 때 마다 항목을 순차적으로 하나씩 불러온다.

```html
---
arr:
  - a
  - b
  - c
  - d
---
{% for item in page.arr %}
    <h2>{{ item }} = {% cycle "x", "y", "z" %}</h2>
{% endfor %}
```

build 결과는 다음과 같다. (빈 줄은 생략)

```html
    <h2>a = x</h2>
    <h2>b = y</h2>
    <h2>c = z</h2>
    <h2>d = x</h2>
```

### forloop.index

아래 파일은 `ex9-2.html`이다. `forloop.index`를 사용하면 1부터 시작하는 loop index을 얻을 수 있다. 0부터 시작하는 index는 `forloop.index0`를 쓰면 된다.

```html
---
arr:
  - a
  - b
  - c
  - d
---
{% for item in page.arr %}
    <h2>{{ item }} {{ forloop.index }} {{ forloop.index0 }}</h2>
{% endfor %}
```

build 결과는 다음과 같다. (빈 줄은 생략)

```html
    <h2>a 1 0</h2>
    <h2>b 2 1</h2>
    <h2>c 3 2</h2>
    <h2>d 4 3</h2>
```

### forloop.first and forloop.last

아래 파일은 `ex9-3.html`이다. `forloop.first`, `forloop.last`는 루프의 첫번째와 마지막 인덱스를 얻는다.

```html
---
arr:
  - a
  - b
  - c
  - d
---
{% for item in page.arr %}
    {% if forloop.first or forloop.last %}
        <h2>{{ item }}</h2>
	{% endif %}
{% endfor %}
```

build 결과는 다음과 같다. (빈 줄은 생략)

```html
        <h2>a</h2>
        <h2>d</h2>
```

### forloop.rindex

아래 파일은 `ex9-4.html`이다. `forloop.rindex`는 현재 반복될 루프가 몇 번 남았는가를 알려준다. (응용이 궁금하다면 [9. Looping in Liquid](<https://learn.cloudcannon.com/jekyll/looping-in-liquid/>)참고)

```html
---
arr:
  - a
  - b
  - c
  - d
---
{% for item in page.arr %}
        <h2>{{ item }} {{ forloop.rindex }}</h2>
{% endfor %}
```

build 결과는 다음과 같다. (빈 줄은 생략)

```html
        <h2>a 4</h2>
        <h2>b 3</h2>
        <h2>c 2</h2>
        <h2>d 1</h2>
```

### forloop.length

아래 파일은 `ex9-5.html`이다. 루프 안에서 `forloop.length`는 루프의 크기를 넘겨준다. 당연하지만 루프 밖에서는 `forloop.length`가 안먹히고 그때는 `page.arr.size` 처럼 array의 크기를 이용한다.

```html
---
arr:
  - a
  - b
  - c
  - d
---
{{ page.arr.size }}
{% for item in page.arr %}
        <h2>{{ item }} {{ forloop.length }}</h2>
{% endfor %}
```

build 결과이다.

```html
4
        <h2>a 4</h2>
        <h2>b 4</h2>
        <h2>c 4</h2>
        <h2>d 4</h2>
```

### continue, break, reversed

아래 파일은 `ex9-6.html`이다.  루프 제어를 위한 `{% continue %}`, `{% break %},` `reversed` 등이 있다.

```html
---
arr:
  - a
  - b
  - c
  - d
---
{% for item in page.arr %}
    {% if forloop.index == 3 %}
        {% continue %}  
	{% endif %}
    <h2>{{ item }} {{ forloop.index }}</h2>
{% endfor %}

{% for item in page.arr %}
    {% if forloop.index == 3 %}
        {% break %}  
	{% endif %}
    <h2>{{ item }} {{ forloop.index }}</h2>
{% endfor %}

{% for item in page.arr reversed %}
    <h2>{{ item }} {{ forloop.index }}</h2>
{% endfor %}
```

build 결과이다. (빈 줄은 적절히 생략)

```html
    <h2>a 1</h2>   
    <h2>b 2</h2>
    <h2>d 4</h2>

    <h2>a 1</h2>
    <h2>b 2</h2>

    <h2>d 1</h2>
    <h2>c 2</h2>
    <h2>b 3</h2>
    <h2>a 4</h2>
```

### limit and offset

아래 파일은 `ex9-7.html`이다. `offset`은 루프 시작위치이고 `limit` 는 최대 반복횟수다. 주의점은 `offset` 시작은 0이라는 것이다.

```html
---
arr:
  - a
  - b
  - c
  - d
---
{% for item in page.arr limit: 2 offset: 1 %}
    <h2>{{ item }}</h2>
{% endfor %}
```

build 결과이다.

```html
    <h2>b</h2>
    <h2>c</h2>
```

### else

아래 파일은 `ex9-8.html`이다. YAML에서 빈 array을 만드는 방법은 `arr = []` 인 것 같다. 루프에서 빈 array를 처리할 때 `else`를 사용할 수도 있다.

```html
---
arr: []
---
{% for item in page.arr %}
    <h2>{{ item }}</h2>
{% else %}
    <h2>empty array</h2>
{% endfor %}
```

build 결과는 다음과 같다.

```html
    <h2>empty array</h2>
```

### range

[liquid 정식문서](<https://shopify.github.io/liquid/tags/iteration/>)를 보니 range도 정리할 필요가 있어 보인다.  입력한 범위 정수 array 비스무리 한 걸 만드나 보다.

```html
---
---
{% assign num = 3 %}
{% for i in (1..num) %}
    <h2>{{ i }}</h2>
{% endfor %}
```

build 결과는 다음과 같다.

```html
    <h2>1</h2>
    <h2>2</h2>
    <h2>3</h2>
```

{% endraw %}