---
title: "Jekyll & Liquid 시작하기 (3)"
excerpt: "CloudCannon 의 liquid 튜토리얼 공부 세 번째"
categories:
  - language
tags:
  - jekyll
  - liquid
last_modified_at: 2019-05-02T01:50
---



[Jekyll & Liquid 시작하기 (1)]({{ site.url }}/language/language-liquid-cloudcannon-1)

[Jekyll & Liquid 시작하기 (2)]({{ site.url }}/language/language-liquid-cloudcannon-2) 에 이은 세 번째 정리

주요 관련 사이트 링크:

* [jekyll](<https://jekyllrb-ko.github.io/>)
* [liquid](<http://shopify.github.io/liquid/>)
* [CloudCannon](<https://cloudcannon.com/>)
* [YAML](<https://yaml.org/start.html>)

## 10. 블로그 만드는 방법 소개

{% raw %}

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

{% endraw %}

## 11. 콜렉션 (collections)

{% raw %}

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

{% endraw %}

## 12. 데이터 파일 (data files)

{% raw %}

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

{% endraw %}

## 13. 고유 주소 (Permalinks)

{% raw %}

포스트, 페이지, 콜렉션 등은 빌드될 때 `_site/` 폴더 아래에 어디에 위치할지 고유 주소에 의해 결정된다. 예를 들어 포스트는 디폴트로 `_site/YYYY/MM/DD/title.html` 이 고유주소이다.  페이지의 경우는 그 페이지가 위치한 그대로를 따른다. 즉 `<site root>/index.html` 은 `_site/index.html` 로 빌드된다. 만약 `<site root>/blog/index.html`이라는 페이지는 `_site/blog/index.html`로 빌드된다. 콜렉션의 경우는 `output: true` 인 경우 `_site/collectionname/title.html` 이 된다. 이러한 디폴트 주소를 바꾸려면 `permalink` 변수를 이용하면 된다. `permalink` 의 유용함을 확인해 보자.

### 페이지 고유 주소

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

우선 포스트가 나열되는 페이지를 만든다. 최종 링크주소가 `localhost:4000/posts/index.html`이 되게 하려면 `<site root>/posts/index.html` 을 만들어야 빌드했을 때 `_site/posts/index.html` 이 된다. 그런데 이렇게 하면 페이지를 만들때마다 내부 폴더가 계속 늘어나니까 관리하기 불편하다. 이때 `permalink` 를 사용하면 된다.

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

### 포스트 고유 주소

개별 포스트에도 `permalink`를 달 수 있지만, 그렇게 하는 것은 비효율적이다. `_config.yml`에 포스트의 `permalink`를 설정하면 모든 포스트에 대해 적용된다. 아무것도 설정하지 않으면 기본 `permalink` 형식은 `permalink: /:year/:month/:day/:title` 이다. 

* 주의 1: 끝에 `/`를 붙이면 `/:title/index.html`로 생성되고, 붙이지 않으면 `/:title.html` 이 생성된다.
* 주의 2: `:title` 은 각 포스트의 front matter에 적힌 title이 아니라 파일명의 title이다.

  `_config.yml`에 아래를 추가해 보자.

```yaml
permalink: /:categories/:title/
```

`jekyll serve`를 한 후, `_site/`를 확인해 보면 `_site/2018/...` 식으로 빌드되던 포스트들이 `_site/:title/index.html` 로 빌드됐음을 볼 수 있다. 이것은 `_posts/`의 포스트 문서들에 `categories`가 없기 때문이다. 

`_posts/` 폴더의 두 markdown 문서의 front matter에 아래 내용을 추가한다.

```yaml
categories:
  - web
```

그러면 포스트들은 모두  `_site/web/` 에 빌드된다.

### 콜렉션 고유 주소

콜렉션 역시 개별 콜렉션 문서에 `permalink`를 달 수 있지만, `_config.yml`에 해당 콜렉션에 대한 `permalink`를 정의하는게 더 효율적이다. 아무것도 설정하지 않으면 콜렉션의 기본 `permalink` 형식은 `permalink: /:collection/:path` 이다. 여기서 `:path`는 collection 폴더 안에서의 상대경로이다. 

dogs 콜렉션에 대한 permalink를 아래와 같이 지정해 보자.

```yaml
collections:
  dogs:
    output: true
    permalink: /pets/:path
```

그러면 dogs 콜렉션에 대한 빌드 파일들이 `_site/pets/` 폴더에 저장된다. 나중에 cats 콜렉션을 만들어서 같은 곳에 빌드되도록 할 수 있다.

{% endraw %}

## 14. 기존 사이트 유지보수

{% raw %}

여기 내용은 기존 HTML + CSS 로 만들어진 웹사이트를 jekyll로 전환해 보는 튜터리얼이다. 이 부분은 원문을 직접 보면서 따라해보면 매우 좋을 것 같다. [14. Convert a static site to Jekyll](<https://learn.cloudcannon.com/jekyll/converting-a-static-site-to-jekyll/>)

## 15. 네비게이션

이제 `index.html`을 수정하여 `Go Posts`, `Go Collections` 같은 페이지 네비게이션을 수정할 것이다.

먼저, `_data/navigation.yml`의 내용을 아래와 같이 바꾼다.

```yaml
main:
  - title: "Posts"
    url: "/posts/"
  - title: "Collections"
    url: "/collections/"
```

이제 `index.html`을 아래와 같이 수정한다.

```html
---
layout: page
title: Blog Home
---
{% for item in site.data.navigation.main %}
  <h2><a href="{{ item.url }}">Go {{ item.title }}</a></h2>
{% endfor %}
```

이제 `_data/navigation.yml` 에 형식에 맞춰서 항목 추가를 하면 저절로 네이게이션 링크가 추가된다.  더 자세한 사용법은 [Advanced navigation](<https://learn.cloudcannon.com/jekyll/advanced-navigation/>) 참고할 것.

{% endraw %}