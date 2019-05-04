---
title: Getting started with Jekyll and liquid
last_modified_at: 2019-05-01T22:28
categories:
  - web
tags:
  - jekyll
  - liquid
  - web
toc: true
author_profile: false
---



본 내용은 [Learn Jekyll & CloudCannon](<https://learn.cloudcannon.com/>)의 Getting started with Jekyll (16 tutorial)의 내용을 공부하여 나의 방식대로 다시 정리한 것이다. 링크의 튜토리얼은 예제를 활용하여 liquid를 빠르게 이해할 수 있다. 또한 16가지 튜토리얼은 매우 좋은 예제들이 source code로 첨부되어 있어서 `jekyll serve` 명령으로 어떤 동작을 하는지 확인하면서 공부할 수 있다.  

이 포스트에서는 CloudCannon 튜토리얼의 소스코드를 답습하는게 아니라 liquid에 관련된 간단한 요약만을 정리한다. 나중에 사용법을 잊어버렸을 때 기억하기 위함이다.

주요 관련 사이트 링크:

* [jekyll](<https://jekyllrb-ko.github.io/>)
* [liquid](<http://shopify.github.io/liquid/>)
* [CloudCannon](<https://cloudcannon.com/>)
* [YAML](<https://yaml.org/start.html>)

[TOC]

##  1. Jekyll 구동하기

`_site/` 폴더에 build 및 `localhost:4000`으로 server 시작

```
$ jekyll serve
```

`_site/` 폴더에 build 만 하고 싶을 때

```
$ jekyll build
```

jekyll은 아래 표와 같은 build를 제어하는 여러 runtime flags를 가지고 있고 대부분은 `_config.yml` 파일에 넣어서 정의해 둘 수 있다. 더 자세한 설정 정보는 [configuration documentation](<https://jekyllrb.com/docs/configuration/>)을 참고하라.

| Setting                                                      | Flags                      |
| ------------------------------------------------------------ | -------------------------- |
| Site Source<br />jekyll이 읽어서 처리할 source를 경로로 지정. 디폴트는 현재폴더 | -s, --source DIR           |
| Site Destination<br />jekyll이 build한 결과 파일을 지정. 디폴트는 _site/ | -d, --destination DIR      |
| Safe<br />custom plugins을 사용하지 못하게 하고 symblic links를 무시 | --safe                     |
| Regeneration<br />파일이 수정되면 자동으로 site를 재생성     | -w, --[no-]watch           |
| Configuration<br />_config.yml 대신 지정한 파일을 설정파일로 사용.<br />나중에 지정한 파일이 앞에 지정한 파일보다 우선 | --config FILE1[,FILE2,...] |
| Drafts<br />draft 포스트를 처리하고 렌더링                   | --drafts                   |
| Environment<br />build에서 특정 환경값을 사용                | JEKYLL_ENV=production      |
| Future<br />Publish posts or collection documents with a future date. | --future                   |
| LSI<br />Produce an index for related posts.                 | --lsi                      |
| Limit Posts<br />Limit the number of posts to parse and publish. | --limit_posts NUM          |
| Force polling<br />Force watch to use polling.               | --force_polling            |
| Verbose output<br />Print verbose output.                    | -V, --verbose              |
| Silence Output<br />Silence the normal output from Jekyll during a build. | -q, --quiet                |
| Incremental build<br />site가 커서 build가 오래걸릴 경우에 사용하면 변경된 포스트와 페이지만 build하기 때문에 build 속도가 개선. 그러나 어떤 경우에 site 생성에 문제 발생 | -I, --incremental          |

## 2. 파일 구조

jekyll site에서 사용하는 파일 구조에 대해서 살펴 본다.

**_config.yml**

jekyll site 설정 파일이다. 주로 다음과 같은 목적을 담당한다.

* site global variables 설정
* collections 정의 또는 defaults 설정
* runtime variables 지정

**_drafts/**

live site에 게시하지 않는 포스트를 작성할 수 있다. 주로 작성 중인 포스트를 쓰는데 사용

**_includes/**

사이트의 여러 페이지를 만들때 자주 중복되는 코드를 _includes/ 폴더에 정의해 두고 include 해서 쓸 수 있다. 예를 들어 사이트의 header.html, footer.html 을 _includes 폴더에 정의해 두고 여러 페이지를 만들 때 include 할 수 있다.

**_layouts/**

어떤 포스트를 markdown으로 작성 할 때 그 포스트의 Front matter(머리말)에 layout을 지정하게 된다. layout은 포스트가 jekyll build에 의해 html로 변환되어 _site/ 폴더로 옴겨질 때 그 포스트의 contents(내용)를 감싸는 template에 해당한다. 특정 layout 에는 header, footer, navigation 등을 구현하는 코드들이 들어 있다.

**_posts/**

markdown으로 작성하는 포스트가 위치하는 곳

**_data/**

jekyll이 site를 build 할 때 이 폴더 안의 YAML, JSON, CSV 형식의 파일 데이터는  다른 여러 페이지에서 liquid 형식으로 참조할 수 있다.

**_site/**

jekyll이 build하는 모든 결과 파일들과 resouce 파일들이 위치하는 곳

**.jekyll-metadata**

jekyll이 incremental build에 사용하기 위해 어떤 파일이 변경되었는가 추적하는데 사용하는 파일

**Other Files/Folders**

site 폴더 내에 있는 Front matter를 가지는 모든 파일들은 jekyll이 적절히 build를 하여 _site/ 폴더에 html 형태로 복사한다.  Front matter가 없는 파일(예를 들어 CSS, JavaScript, 이미지, 또는 다른 여러 resouce 파일들)은 변경없이 똑같은 폴더트리를 유지한 채 _site/ 폴더로 복사한다.

## 3. Liquid 소개

[liquid](<http://shopify.github.io/liquid/>)는 사이트의 페이지 처리를 위해 jekyll이 내부적으로 사용하는 templating language이다. liquid를 사용하면 페이지를 만드는데 변수를 사용할 수 있고 페이지 안에서 if 같은 선택로직이나 loop 같은 반복문을 사용할 수 있다.

두 가지 방식의 liquid tag가 있다.

* 변수 출력: `{{ variable }}`
* 로직 구현: `{% if statement %}`

### 변수 출력

빈 폴더를 만든다. 이제 이 폴더를 `<site root>`라고 하자. `<site root>`에 `ex3-1.html`을 만들고 아래 내용을 작성한다. `---`로 감싸져 있는 부분은 front matter (머리말) 라고 한다. front matter는 YAML로 정의한다.

```html
---
title: Introduction to Liquid
---
<h1>{{ page.title }}</h1>
```

터미널을 이용하여 아래와 같이 `<site root>`에서 `jekyll build`를 하면 `_site/` 폴더가 생성되고, 그 안에 `ex3-1.html`이 생성되며 그 내용은 "`<h1>Introduction to Liquid</h1>`" 임을 알 수 있다.

```terminal
$ jekyll build
...(빌드 메세지 생략)...
$ cat  _site/ex3-1.html
<h1>Introduction to Liquid</h1>
```

jekyll은 `jekyll build` 에 의해 `<site root>` 안에 front matter를 가지는 모든 파일을 적절히 변환하여 `_site/` 폴더에 `<filename>.html` 을 만든다. 그 때 front matter에 정의된 `var: value`은 `page.var` 형식으로 접근할 수 있고 `{{ page.var }}` 형식으로 출력할 수 있다. 여기서 `page.`는 현재 페이지 안에 정의된 것을 가리킨다. (`_config.yml`에 정의하는 global 변수는 `site.`으로 참조한다.)

따라서 `<site root>/ex3-1.html`의 `{{ page.title }}`은 `build` 시에 `Introduction to Liquid`로 치환되어 static page `<site root>/_site/ex3-1.html`로 저장된다. 

이제 `<site root>/_site/`폴더를 삭제하고 `<site root>/ex3-1.html`를 `ex3-1.md`로 바꾸고 안에 내용을 아래와 같이 변경한다.

```markdown
---
title: Introduction to Liquid
---
# {{ page.title }}
```

`jekyll build`를 해보자. 그러면 아래와 같이 `_site/ex3-1.html`파일이 생성되고 `id`속성이 붙여진 것을 제외하면 이전 예제와 동일한 결과인 `.html` 파일이 생성된다. 

```terminal
$ jekyll build
$ cat _site/ex3-1.html
<h1 id="introduction-to-liquid">Introduction to Liquid</h1>
```

jekyll은 `<site root>`안에 있는 모든 front matter를 가지는 파일을 추적하고 markdown 파일은 markdown 변환기에 의해 `html`로 `build` 한다.

이번에는 `<site root>/ex3-1.md`의 내용에서 front matter를 지우고 아래와 같이 만들어 둔다.

```markdown
# {{ page.title }}
```

그러면 아래와 같이 `_site/`에는 `ex3-1.md`만 있고 그 내용은 `<site root>/ex3-1.md`과 동일하다.

```terminal
$ jekyll build
$ ls _site/
ex3-1.md

$ cat ex3-1.md
# {{ page.title }}
```

front matter가 없기 때문에 jekyll은 그냥 그대로 파일을 `_site/` 폴더로 복사한다.

### 필터 (Filters)

필터를 이용하면 변수의 값 출력을 조정할 수 있다. 필터는 파이프(`|`)를 이용한다. 아래와 같이 `<site root>/ex3-2.html` 를 작성한다. 

```html
---
title: Introduction to Liquid
---
<h1>{{ page.title }}</h1>
<h1>{{ page.title | upcase }}</h1>
<h1>{{ page.title | upcase | truncate: 8 }}</h1>
```

`build` 한 후 `<site root>/_site/ex3-2.html`은 아래와 같다.

```html
<h1>Introduction to Liquid</h1>
<h1>INTRODUCTION TO LIQUID</h1>
<h1>INTRO...</h1>
```

필터에 대한 더 많은 자료는 [Liquid](<https://shopify.github.io/liquid/>)를 참고할 것.

### 선택(if)

`if`를 사용하는 형식은 아래와 같다. `<site root>/ex3-3.html`

```html
---
title: Introduction to Liquid
format: 2
---
{% if page.format == 1 %}
  <h1>{{ page.title }}</h1>
{% elsif page.format == 2 %}
  <h1>{{ page.title | upcase }}</h1>
{% else %}
  <h1>{{ page.title | upcase | truncate: 8 }}</h1>
{% endif %}
```

`build` 결과 `<site root>/_site/ex3-3.html` 아래와 같다. 

```html

  <h1>INTRODUCTION TO LIQUID</h1>

```

**주의 1:** `page.format`의 data type에 대해 확실히 파악하고 넘어가자. string인가? int인가? 아마도 string 일것 같긴한데 만약 그렇다면 비교에서 string과 int를 비교한 것인가? 자동 conversion이 일어나서 비교했나? 나중에 알아 볼 것.

**주의 2:** 결과의 빈 줄에 대해서! 아마 jekyll은 `build` 이후에 해당되는 로직 라인 줄을 삭제하는게 아니라 빈줄(줄넘김)로 남겨 두는 것 같다. 나중에 알게된 건데 빈줄을 제거하는 방법도 있다. [Whitespace control](<http://shopify.github.io/liquid/basics/whitespace/>) 참고.

### 반복(for, array)

아래 코드에서 front matter의 `arr`은 array이며 그 항목은 "a", "b", "c" 이다. `for`를 이용해서 항목을 하나씩 꺼낼 수 있다.

```html
---
title: Introduction to Liquid
arr:
  - a
  - b
  - c
---
<h1>page.title</h1>
<ul>
{% for item in page.arr %}
  <li>{{ item }}</li>
{% endfor %}
</ul>
```

`build` 결과는 다음과 같다.

```html
<h1>page.title</h1>
<ul>

  <li>a</li>

  <li>b</li>

  <li>c</li>

</ul>
```

**주의 :** 역시 해당 되는 로직 라인이 3번 반복 되고 마지막 `endfor` 포함해서 네칸이 빈 줄로 남겨져 있는 것 같다.

## 4. 머리말 (Front matter)

여기 부분의 CloudCannon 내용은 이미 전 단계와 많이 겹치기 때문에 생략하고, object를 정의하는 것만 작성한다.

### Objects

array와 object를 잘 구분하자.

```html
---
fruit:
  - name: apple
    cost: $1
    color: red
  - name: banana
    cost: $2
    color: yellow
  - name: orange
    cost: $1.50
    color: orange
---
<ul>
{% for item in page.fruit %}
  <li>{{ item.name }}, {{ item.cost }}, {{ item.color }}</li>
{% endfor %}
</ul>
```

`build` 후 결과.

```html
<ul>

  <li>apple, $1, red</li>

  <li>banana, $2, yellow</li>

  <li>orange, $1.50, orange</li>

</ul>
```

## 5. 레이아웃 (layouts)

### layout, {{ content }}

`<site root>`에 `_layouts/` 폴더를 만들고 안에 `default.html` 파일을 만든 후 아래와 같이 입력한다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  {{ content }}
</body>
</html>
```

`<site root>/index.html` 을 만들고 아래와 같이 내용을 입력한다.

```html
---
layout: default
---
<h1>Introduction to layouts</h1>
```

`build` 한 후, `<site root>/_site/index.html`은 다음과 같다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <h1>Introduction to layouts</h1>
</body>
</html>
```

jekyll은 `<site root>/index.html`을 `build`할 때 front matter안의 특수한 변수인 layout의 값에 해당하는 layout을 `_layouts/` 폴더에서 찾고, front matter 아래쪽의 content(내용)을 layout의 `{{ content }}`에 치환한 후 `<site root>/_site/index.html` 로 위치시킨다.

### layout 상속

위와 같은 원리로 `_layout/` 폴더에 `page.html` 을 추가하고 내용을 다음과 같이 입력한다.

```html
---
layout: default
---
<h2>start page layout</h2>
{{ content }}
```

`<site root>/index.html`의 layout을 아래와 같이 `page`로 수정한다.

```html
---
layout: page
---
<h1>Introduction to layouts</h1>
```

`build` 한 후, `<site root>/_site/index.html`의 내용은 아래와 같다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <h2>start page layout</h2>
<h1>Introduction to layouts</h1>
</body>
</html>
```

`_layout/page.html`의 내용은 `_layout/default.html`의 `{{ content }}`로 주입되고, `<site root>/index.html`내용은 layout을 `page.html`로 했기 때문에 `page.html`의 `{{ content }}`로 주입된 것이다.

### page.title

`_layout/default.html`의 `<title>` 태그의 내용을 `{{ page.title }}`로 수정한다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>{{ page.title }}</title>
</head>
<body>
  {{ content }}
</body>
</html>
```

`<site root>/index.html`의 front matter에 아래와 같이 `title`을 추가해 준다.

```html
---
layout: page
title: My Homepage
---
<h1>Introduction to layouts</h1>
```

`build` 한 후, `<site root>/_site/index.html`의 내용은 아래와 같다.

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

### jekyll 인터프리터 작업순서

위와 같은 결과가 왜 나오는지 처음에 의아했는데 [jekyll 인터프리터 작업순서](<http://jekyllrb-ko.github.io/tutorials/orderofinterpretation/>)를 읽으면 어느정도 이해되는 것 같기도? 내가 이해한 대로 순서를 요약하면 다음과 같다.

1. 모든 파일을 훑으면서 `site`, `page`, `post`, `collection` 객체의 값을 채운다. 값을 먼저!
2. front matter가 있는 모든 파일의 liquid 서식 처리: `{%  %}`, `{{ variable }}` 같은.
3. front matter가 있는 markdown을 html 로 변환
4. layout 적용 및 `{{ content }}` 삽입
5. 디렉토리 구조에 맞게 각 생성 컨텐츠를 `_site/` 로 저장

위 인터프리터 작업순서에 따라 이전 단계의 `index.html`에서 일어난 일을 뇌내망상으로 생각해 본다.

1. jekyll은 `build`가 필요한 파일을 반복적으로 처리한다. 어느순간 `<site root>/index.html`을 선택한다.
2. `<site root>/index.html`의 `page.title` 값이 치환될 가능성이 있는 모든 파일에 대해서 훑고 순환추적하다가`_layout/default.html`의 `page.title` 값을 채운다. 그 밖에 모든 변수와 객체값도 채운다.
3. `{{ if }}`, `{{ for }}`, `{{ variable }}` 등의 liquid 서식 처리를 한다. 
4. layout을 적용하고 `{{ content }}`삽입을 처리한다.
5. `_site/index.html` 로 저장한다.

같은 방식으로  [jekyll 인터프리터 작업순서](<http://jekyllrb-ko.github.io/tutorials/orderofinterpretation/>)에 잘못된 설정으로 발생하는 문제점을 이해하도록 생각해보자.

### Jekyll 3 vs Jekyll 2

현재 사용하고 있는 jekyll version은 3.8.5 이다. jekyll 2와 jekyll 3에 대해 `_layout/` 안에 있는 layout 파일들의 front matter에 정의된 변수를 참조 할 때 방식이 다르다. 예를 들어, `_layout/page.html`의 front matter가 다음과 같다고 가정하자.

```html
---
layout: default
city: San Francisco
---
```

jekyll 2에서는 아래와 같이 `page.`으로 참조 했다고 한다.

```html
{{ page.city }}
```

jekyll 3에서는 `layout.`으로 참조한다고 한다.

```html
{{ layout.city }}
```

생각해 보니 jekyll2의 방식은 특정 page에 city 변수가 있을 때 충돌의 위험이 있을 것 같다.

아래와 같이 실제 테스트를 해 보았다. 우선 `_layout/page.html`을 아래와 같이 수정한다.

```html
---
layout: default
city: San Francisco
---
<h2>start page layout</h2>
{{ content }}
in page: page.city   -> {{ page.city }}
in page: layout.city -> {{ layout.city }}
```

그리고 `<site root>/index.html`을 아래와 같이 수정한다.

```html
---
layout: page
title: My Homepage
city: New York
---
<h1>Introduction to layouts</h1>
in index: page.city   -> {{ page.city }}
in index: layout.city -> {{ layout.city }}
```

`build` 후, `<site root>/_site/index.html`은 다음과 같다.

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
in index: page.city   -> New York
in index: layout.city -> San Francisco
in page: page.city   -> New York
in page: layout.city -> San Francisco
</body>
</html>
```

결과를 확인해 보니 생각한 대로 결과가 나왔다. `index.html`의 `build` 에서 자신이 사용한 `layout`의 `city` 변수값을 `layout.city` 잘 가져왔다. 

그런데 여기서 한 가지 의문이 더 생겼는데 `_layouts/page.html`의 layout은 `_layouts/default.html`인데 `_layouts/page.html` 안에서 사용한 `layout.city`는 `_layouts/default.html`의 front matter에 정의된 `city` 여야 하지 않을까? 즉, 정의가 안되어 있기 때문에 `in page: layout.city -> San Francisco`처럼 출력이 되는게 아니라 `in page: layout.city ->`처럼 아무 내용도 출력이 되지 말아야 하는 것 아닌가?

결과를 보니 이런 규칙이 아닌가 추측해 본다.

* `_layouts/`에 있는 layout 파일들 안에서 `layout.city`를 사용할 때 `layout.`은 자신을 가리킨다.
* 명시적으로 렌더링되어 `_site/` 안에 생성되는 `<site root>/index.html` 같은 파일에서 `layout.city`를 사용하면 `_layouts/` 폴더에 있는 layout을 가리킨다.

아니면 이런 규칙인가?

* `index.html` 처럼 build가 되어  `_site/` 에 위치하는 page 관점에서 볼 때,  인터프리터 작업순서 1.에서 처럼 값이 먼저 채워지고 따라서 이 시점에서 `index.html`에 관련된 `layout.<var>`, `page.<var>`는 항상 고정돼 있다. 그리고 나중에 layout 처리가 되니까 문제가 없을 것 같다.

두 번째 규칙이 조금 더 타당해 보이는데? 어쨌든 어떤 동작을 하는지 결과가 나오는지 잘 숙지하고 사용하자.

## 6. includes

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

## 7. Liquid 흐름제어문

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

## 8. 문자열 필터 (String filters)

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

## 9. 반복 (Loop)

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



## 10. 블로그 만드는 방법 소개

### 포스트 작성

우선 두 개의 연습 포스트를 작성해 보자. `<site root>`에 `_posts/` 폴더를 만든다. `_posts/` 폴더는 보통 markdown으로 작성하는 포스트가 위치하는 곳이다. 포스트 파일의 작성 규칙은 파일명이 반드시 `YYYY-MM-DD-title-name.md`가 되어야 한다.  일단 아래와 같이 `_posts/2018-04-01-markdown-format.md` 를 만들고 내용은 아래와 같이 작성한다. front matter는 비워둔다.

```markdown
---
---
## 마크다운 형식
마크다운의 파일명 형식은 YYYY-MM-DD-title-name.md 이다.
```

두 번째 포스트 `_posts/2018-04-02-about-liquid.md`의 내용은 아래와 같다. (markdown으로 작성한 각 포스트의 front matter에 `date`와 `title`이 정의되어 있으면 front matter 정보가 파일명의 `date`, `title`보다 우선한다.)

```markdown
---
title: liquid 란 무엇인가?
date: 2018-04-06 09:30
---
## liquid 란?
jekyll이 사용하는 templating language 이다.
```

### 포스트 목록 페이지 만들기

`_posts/` 폴더에 만든 포스트의 목록을 보여주는 `blog.html` 을 만들기 위해 먼저 연습삼아 만들어 두었던 `layout` 을 정리하자. 

우선 `_includes/head.html`의 내용은 아래와 같다.

```html
<head>
  <meta charset="UTF-8">
  <title>{{ page.title }}</title>
</head>
```

그리고 위 `_includes/head.html`을 include하여 사용하는 `_layouts/default.html`은 아래와 같다.

```html
<!DOCTYPE html>
<html>
{% include head.html %}
<body>
  {{ content }}
</body>
</html>
```

이제 `<site root>/index.html` 을 만들고 안에 내용을 아래와 같이 작성한다.

```html
---
layout: default
title: Blog Home
---
{% for post in site.posts %}
  <ul>
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  </ul>
{% endfor %}
```

이제 블로그 사이트를 열어 보자.  터미널에서 `jekyll serve`를 실행한다.

```terminal
$ jekyll serve
```

서버가 시작되면 크롬에서 `localhost:4000` 으로 접속해 본다. 아래와 같은 두 링크가 보인다. 

> - [liquid 란 무엇인가?]()
> - [Markdown Format]()

이것은 크롬에서 `_site/index.html` 페이지를 연 것에 해당한다. layout이 볼품 없지만 `_posts/` 에 작성해 두었던 두 포스트 링크가 뜬다. 클릭해서 들어가 본다. 개별 포스트 내용을 보여주는 페이지가 열리는데 그 페이지 주소를 확인해 보자. 두 포스트의 주소는 아래와 같다.

* `localhost:4000/2018/04/06/about-liquid.html>`
* `localhost:4000/2018/04/01/markdown-format.html`

위 두 html 파일은 `_site/YYYY/MM/DD/<filename>.html` 로 저장이 되어 있는데 jekyll이 build 시에 `_posts/<filename>.md`를 `html`로 변환하고 `_site/...` 에 가져다 놓은 것이다. 

다시 `index.html`로 돌아가서 소스를 확인해 보자. 각 포스트에 접근하는 방법은 `site.posts` 를 이용하면 된다는 것을 알 수 있고 그 각각의 `post`는 `url`, `title`, `date` 등의 값을 갖는다. 그렇게 `<a href="{{ post.url }}">...` 로 a 태그를 이용해서 링크를 만든 것이다.

### 포스트 목록 페이지 개선

`_layouts/page.html` 을 만들고, (기존에 있으면 삭제하고 만들 것) 아래와 같이 적는다.

```html
---
layout: default
---
<h2>{{ page.title }}</h2>
{{ content }}
```

그리고 `<site root>/index.html`을 아래와 같이 수정한다.

```html
---
layout: page
title: Blog Home
---
{% for post in site.posts %}
  <ul>
    <li>
	  <p><a href="{{ post.url }}">{{ post.title }}</a></p>
      <p>{{ post.date | date: '%B %d, %Y' }}</p>
	</li>
  </ul>
{% endfor %}
```

다시 `localhost:4000`으로 들어가 보면 아래와 같이 바뀌어 있다. (**알아둘점:** `jekyll serve`가 시작되면 global 설정 파일 `_config.yml`을 바꾸지 않는 이상 곧바로 바뀐 페이지가 모두 rebuild가 된다.)

> **Blog Home**
>
> * [liquid 란 무엇인가?](http://localhost:4000/2018/04/06/about-liquid.html)
>
> April 06, 2018
>
> * [Markdown Format](http://localhost:4000/2018/04/01/markdown-format.html)
> April 01, 2018

### 포스트 페이지 개선

이번에는 Blog Home에서 각 포스트를 보기위해 링크를 클릭했을 때 나오는 포스트 페이지의 layout을 만들어 본다. `_layouts/post.html` 파일을 만들고 아래와 같이 작성한다. 이 layout은 개개의 포스트에서 사용하는 layout이다.

```html
---
layout: default
---
<h2>Title: {{ page.title }}</h2>
<p>{{ page.date | date: '%B %d, %Y' }}</p>
<br>
{{ content }}
```

이제 `_posts/` 폴더의 모든 markdown 파일의 front matter에 아래 코드를 넣어준다.

```markdown
---
layout: post
---
```

그러면 이제 모든 포스트 링크로 들어가보면 아래와 같은 방식으로 보인다. 

> **Title: liquid 란 무엇인가?**
> April 06, 2018

> **liquid 란?**
> jekyll이 사용하는 templating language 이다.



HTML과 CSS를 이용해서 layout을 좀 더 개선하면 더 멋지게 페이지를 완성할 수 있을 것이다.

## 11. 콜렉션 (collections)

### 콜렉션 만들기

포스트는 날짜별로 글을 모아둔 것이다. jekyll은 기본적으로 `_posts/` 폴더에 있는 글을 포스트로 인식하고 `site.posts` 로 모든 포스트 변수에 접근할 수 있다.

날짜와 관계없는 문서모음(예를 들어 매뉴얼 챕터들이나 날짜와 무관한 각종 컨텐츠들)은 콜렉션을 사용하여 폴더에 모아둘 수 있다. `<site root>` 에 `_dogs/` 라는 폴더를 만든다. 그리고 그 안에 아래와 같은 세 markdown 문서를 만든다.

`_dogs/husky.md`

```markdown
---
title: 허스키
---
## 특징
허스키는 잘 생겼음.
```

`_dogs/maltese.md`

```markdown
---
title: 말티즈
---
## 특징
말티즈는 작고 귀여워.
```

`_dogs/retriever.md`

```markdown
---
title: 리트리버
---
## 특징
리트리버는 순둥이.
```

이제 jekyll에게 `<site root>/_dogs/` 폴더의 내용이 콜렉션이라는 것을 알려줘야 한다. 전역 설정 파일인 `<site root>/_config.yml` 파일을 만들고 내용을 아래와 같이 적는다.

```yaml
collections:
  dogs:
```

위와 같이 하면 `_dogs/` 폴더의 문서들은 콜렉션이 되며 `site.dogs` 방식으로 정보에 접근할 수 있다. 우선 build 를 해본다.

```terminal
$ jekyll build
```

빌드 후 `_site/` 폴더를 확인해보면 `_dogs/` 에 작성한 문서들의 빌드된 html 파일들은 보이지 않는다. 콜렉션은 `_posts/` 와는 다르게 명시적으로 지정하지 않는 이상 콜렉션 개개 문서를 빌드하지 않는다. 

### 콜렉션 접근 연습

`<site root>/index.html`을 아래와 같이 수정한다.

```html
---
layout: page
title: Blog Home
---
<h2>Posts</h2>
<hr>
{% for post in site.posts %}
  <ul>
    <li>
	  <p><a href="{{ post.url }}">{{ post.title }}</a></p>
      <p>{{ post.date | date: '%B %d, %Y' }}</p>
	</li>
  </ul>
{% endfor %}

<br>
<h2>Collections: dogs</h2>
<hr>
{% for dog in site.dogs %}
    <h2>Title: {{ dog.title }}</h2>
    <p>{{ dog.content }}</p>
	<hr>
{% endfor %}
```

`jekyll serve`를 한 후, 크롬에서 `localhost:4000`으로 접속해 보면 dogs 콜렉션의 문서들의 내용이 보인다. dogs 콜렉션의 각각의 문서들은 `title`, `content` 로 접근할 수 있다. dogs 콜렉션에 각 markdown의 front matter에 정의해 놓은 변수들도 같은 방식으로 접근할 수 있다.

### 콜렉션 문서 페이지 만들기

위에서 만든 dogs 콜렉션도 포스트 처럼 각각의 문서 링크를 클릭하여 내용을 보는 방식으로 바꿔 보자. 우선 포스트처럼 dogs 콜렉션의 각 문서가 개개의 html로 빌드되려면 아래와 같이 `_config.yml`을 수정해야 한다. 

```yaml
collections:
  dogs:
    output: true
```

(**주의:** 만약 `_config.yml`을 수정하고 그것을 반영하려면 `jekyll serve`를 다시 시작해야 한다.)

`jekyll serve`를 하기전에 `jekyll build`를 해보면 `_site/dogs/` 폴더에 dogs 콜렉션의 문서들이 html로 생성되어 있음을 알 수 있다.

`<site root>/index.html`을 아래와 같이 수정한다.

```html
---
layout: page
title: Blog Home
---
<h2>Posts</h2>
<hr>
{% for post in site.posts %}
  <ul>
    <li>
	  <p><a href="{{ post.url }}">{{ post.title }}</a></p>
      <p>{{ post.date | date: '%B %d, %Y' }}</p>
	</li>
  </ul>
{% endfor %}

<br>
<h2>Collections: dogs</h2>
<hr>
{% for dog in site.dogs %}
  <ul>
    <li>
	  <p><a href="{{ dog.url }}">{{ dog.title }}</a></p>
	</li>
  </ul>
{% endfor %}
```

`jekyll serve`를 실행하고 `localhost:4000`을 해보면 포스트와 유사하게 dogs 콜렉션 문서에 접근할 수 있게 된다. 이제 dogs 콜렉션의 각각의 문서의 레이아웃을 만든다. 

`_layouts/single.html`  (하고싶은 이름을 정하면 된다.)

```html
---
layout: default
---
<h2>Title: {{ page.title }}</h2>
<br>
{{ content }}
```

그리고 `_dogs/` 의 모든 마크다운 문서 front matter에 `layout: single` 를 추가해 준다. 그러면 콜렉션의 각각의 문서 레이아웃이 바뀐다.

```markdown
---
layout: single
title: 허스키
---
## 특징
허스키는 잘 생겼음.
```

## 12. 데이터 파일 (data files)

`<site root>/_data` 폴더를 만든다. jekyll은 `_data/` 폴더에 있는 `.json`, `.csv`, `.yml` 파일을 읽고 `site.data` 형식으로 데이터에 접근 할 수 있다. 테스트를 해 보자.

`_data/` 폴더에 `navigation.yml`을 만들고 아래와 같이 작성한다.

```yml
main:
  - title: "Posts"
    url: "posts url"
  - title: "Collections"
    url: "collection url"
```

`<site root>/ex12-1.html` 을 만들고 아래와 같이 작성한다.

```html
---
---
{% for item in site.data.navigation.main %}
    <p>{{ item.title }}</p>
	<p>{{ item.url }}</p>
{% endfor %}
```

build 결과는 다음과 같다.

```html
    <p>Posts</p>
	<p>posts url</p>

    <p>Collections</p>
	<p>collection url</p>
```

더 자세한 응용은 [12. Introduction to data files](<https://learn.cloudcannon.com/jekyll/introduction-to-data-files/>) 참고.

## 13. 고유 주소 (Permalinks)

포스트, 페이지, 콜렉션 등은 빌드될 때 `_site/` 폴더 아래에 어디에 위치할지 고유 주소에 의해 결정된다. 예를 들어 포스트는 디폴트로 `_site/YYYY/MM/DD/title.html` 이 고유주소이다.  페이지의 경우는 그 페이지가 위치한 그대로를 따른다. 즉 `<site root>/index.html` 은 `_site/index.html` 로 빌드된다. 만약 `<site root>/blog/index.html`이라는 페이지는 `_site/blog/index.html`로 빌드된다. 콜렉션의 경우는 `output: true` 인 경우 `_site/collectionname/title.html` 이 된다. 이러한 디폴트 주소를 바꾸려면 `permalink` 변수를 이용하면 된다. `permalink` 의 유용함을 확인해 보자.

`<site root>/index.html`을 아래와 같이 수정

```html
---
layout: page
title: Blog Home
---
<h2><a href=/posts/>Go Posts</a></h2>
<h2><a href=/collections/>Go Collections</h2>
```

`jekyll serve`를 하면 사이트 초기화면이 아래와 같은 방식이 된다.

> **Blog Home**
> [Go Posts]()
> [Go Collections]()

`Go Posts`를 클릭하면 포스트가 나열되는 페이지로 가고, `Go Collections`을 누르면 콜렉션이 나열되는 페이지로 가도록 만들 것이다.

우선 포스트가 나열되는 페이지를 만든다. 최종 링크주소가 `localhost:4000/posts/index.html`이 되게 하려면 `<site root>/posts/index.html` 을 만들어야 빌드했을 때 `_site/posts/index.html` 이 된다. 그런데 이렇게 하면 페이지를 만들때마다 내부 폴더가 계속 늘어나니까 불편하다. 이때 `permalink` 를 사용하면 된다.

`<site root>`에  `viewposts.html` 을 만들고 아래와 같이 작성한다.

```html
---
layout: page
title: Posts
permalink: /posts/
---
<hr>
{% for post in site.posts %}
  <ul>
    <li>
	  <p><a href="{{ post.url }}">{{ post.title }}</a></p>
      <p>{{ post.date | date: '%B %d, %Y' }}</p>
	</li>
  </ul>
{% endfor %}
```

이미 `jekyll serve`를 해 놓은 상태라면 `_site/posts/index.html` 이 생성되어 있을 것이다. 따라서 `Go Posts`를 클릭하면 포스트들이 나열되는 페이지로 가게 된다. 위 코드에서 `permalink: /posts/`가 없다면 `_site/viewposts.html` 이 생성되기 때문에 올바로 링크가 걸리지 않는다.

이제 같은 방식으로 콜렉션이 나열되는 페이지를 만든다. `<site root>`에 `viewcollections.html`을 만들고 아래와 같이 작성한다.

```html
---
layout: page
title: Collections
permalink: /collections/
---
<br>
<h2>dogs</h2>
<hr>
{% for dog in site.dogs %}
  <ul>
    <li>
	  <p><a href="{{ dog.url }}">{{ dog.title }}</a></p>
	</li>
  </ul>
{% endfor %}
```

위 페이지는 `permalink: /collections/`에 의해 `_site/collections/index.html` 로 빌드 된다.

그런데 이런 방식으로 하면 `<site root>`에 계속 페이지가 늘어나게 된다. 페이지가 많아지면 복잡하고 관리하기 어려워지기 때문에 이제부터 새로운 페이지를 만들 때 모두 `<site root>/_pages/` 폴더에 넣어서 관리한다고 하자. 우선 `<site root>/_pages/` 폴더를 만들고 `viewposts.html`, `viewcollections.html`을 모두 `_pages/`로 이동시킨다. 그러면 링크인 `Go Posts`와 `Go Collections`는 동작하지 않는다. 이유는 jekyll이 `_pages/` 폴더 안에 있는 페이지를 빌드 하지 않기 때문이다. jekyll은 jekyll이 특수하게 사용하는 `_posts/`, `_includes/`, `_layout/`, `_data/`와 collection으로 등록되어 있는 폴더를 제외하고, underbar(`_`)로 시작하는 모든 폴더의 내용을 빌드하는 않는 것 같다.(추측. 나중에 문서를 찾아볼 것) 따라서 `_pages/` 폴더 안의 페이지를 빌드하려면 `_config.yml`에 아래 내용을 추가한다.

```yaml
include:
  - _pages
```

`_config.yml`의 내용이 적용되려면 `jekyll serve`를 다시 시작해야 한다. 다시 시작하면 이제 `_site/` 를 확인하면 페이지들이 빌드되어 있음을 알 수 있다.



