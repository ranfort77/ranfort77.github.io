---
title: Jekyll & Liquid 시작하기 (1)
last_modified_at: 2019-04-30T22:28
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
2. front matter가 있는 모든 파일의 liquid 서식 처리: `\{\%  \%\}`, `\{\{ variable \}\}` 같은.
3. front matter가 있는 markdown을 html 로 변환
4. layout 적용 및 `\{\{ content \}\}` 삽입
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

