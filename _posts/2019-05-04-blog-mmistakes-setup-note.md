---
title: "minimal-mistakes-4.16.2.zip 설정 기록"
excerpt: "윈도우7에서 mmistakes-4.16.2-zip으로 블로그 설정 기록"
toc: true
toc_sticky: true
categories:
  - blog
tags:
  - jekyll
  - minimal-mistakes
last_modified_at: 2019-05-06T02:01:00-05:00
mathjax: true
---


진행한 운영체제는 윈도우7이고, 블로그 설정은 [취미로 코딩하는 개발자: 하우투](https://devinlife.com/howto/)를 보고 그대로 따라 진행하였다. 나중에 잊어버릴 나를 위하여 간단하게 설정한 기록을 남겨둔다.


## 1. 압축 해제 및 필요없는 파일 삭제

`minimal-mistakes-4.16.2.zip` 압축을 풀고, 폴더명을`<username>.github.io` 만든다.

아래 목록을 삭제한다. [minimal-mistakes Quick-Start Guide: Remove the Unnecessary](<https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/>) 에서는 10가지를 언급하였으나, 언급한 9가지를 삭제하였고 `minimal-mistakes-jekyll.gemspec`은 삭제하지 않았다. 그리고 그 외에 언급하지 않은 파일들(진하게 표시한 파일들) 6가지도 필요없을 것 같아서 삭제하였다.

* `.github/`
* `docs/`
* `test/`
* `.editorconfig`
* `.gitattributes`
* **`.travis.yml`**
* **`banner.js`**
* `CHANGELOG.md`
* **`package.json`**
* **`package-lock.json`**
* **`Rakefile`**
* `README.md`
* `screenshot.png`
* `screenshot-layouts.png`
* **`staticman.yml`**

결국 `<username>.github.io/`남아있는 파일 및 폴더는 다음과 같다.

* `_data/`
* `_includes/`
* `_layouts/`
* `_sass/`
* `assets/`
* `.gitignore`
* `_config.yml`
* `Gemfile`
* `index.html`
* `LICENSE`
* `minimal-mistakes-jekyll.gemspec`

## 2. localhost 서버 시작 및 첫 포스트

`git` 초기화 한다. 그러면 `.git/` 폴더가 생성된다.

```bash
$ git init
Initialized empty Git repository in D:/ranfort77.github.io/.git/
```

`bundle` 명령을 실행한다. 그러면 `Gemfile.lock`가 생성된다.

```bash
$ bundle
Fetching gem metadata from https://rubygems.org/..........
Fetching gem metadata from https://rubygems.org/.
Resolving dependencies...
(... 이하 생략 ...)
```

`bundle exec jekyll serve`로 `localhost:4000`에서 블로그 테마를 시작할 것인데 그전에 윈도우에서는 code page를 65001 해줘야 한다. 아니면 `jekyll 3.8.5 | Error:  Invalid CP949 character "\xE2" on line 54` 가 발생한다.

```bash
$ /c/Windows/System32/chcp 65001
Active code page: 65001

$ bundle exec jekyll serve
Configuration file: D:/ranfort77.github.io/_config.yml
            Source: D:/ranfort77.github.io
       Destination: D:/ranfort77.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 1.704 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'D:/ranfort77.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```

크롬에서 `localhost:4000` 으로 접속하고 블로그 기본 화면을 확인한다.

`_posts/` 폴더를 만들고, 안에 `2019-01-01-my-first-post.md`을 아래와 같이 작성한다.

```markdown
---
title: "나의 첫번째 포스트"
excerpt: "테스트 목적으로 작성한 첫번째 포스트"
toc: false
categories:
  - none
tags:
  - none
last_modified_at: 2019-01-02T09:15:00-05:00
---

## 목적

테스트 목적으로 작성한 첫번째 포스트
```

`localhost:4000`에서 새로고침하여 포스트가 보이는지 확인한다.

## 3. github 웹호스팅

깃허브에서 `<username>.github.io`라는 이름으로 새로운 repository를 만든다. 이 때 "Public"으로 만들고 README 파일은 만들지 않는다.

 git의 첫번째 커밋을 한다.

```bash
$ git add .
(... 생략 ...)

$ git commit -m "Make first blog frame"
(... 생략 ...)

$ git log
commit 5d73bec2bff204d5b5740783717a8c5a90d62004 (HEAD -> master)
Author: ranfort77 <ranfort77@gmail.com>
Date:   Sat May 4 03:53:36 2019 +0900

    Make first blog frame
```

remote repository를 연결하고 업로드한다.

```bash
$ git remote add origin https://github.com/ranfort77/ranfort77.github.io.git
$ git remote -v
origin  https://github.com/ranfort77/ranfort77.github.io.git (fetch)
origin  https://github.com/ranfort77/ranfort77.github.io.git (push)

$ git push -u origin master
Enumerating objects: 192, done.
Counting objects: 100% (192/192), done.
Delta compression using up to 8 threads
Compressing objects: 100% (181/181), done.
Writing objects: 100% (192/192), 244.14 KiB | 3.34 MiB/s, done.
Total 192 (delta 10), reused 0 (delta 0)
remote: Resolving deltas: 100% (10/10), done.
To https://github.com/ranfort77/ranfort77.github.io.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

크롬에서 `https://<username>.github.io`로 접속해서 웹호스팅이 되고 있음을 확인한다.

## 4. _config.yml 설정파일 수정

`_config.yml`에서 다른 것은 그대로 두고 아래 작성한 것만 수정하였다. teaser 이미지는 `/assets/images/teaser.jpg` 에 저장해 둬야 한다.

```yaml
title                    : "Quiet Air"
name                     : &name "코다마"
description              : &desc "기록노트"
url                      : &url "https://ranfort77.github.io"

repository               : &github "https://github.com/ranfort77"
teaser                   : &teaser "/assets/images/teaser.jpg"

og_image                 : *teaser 
# Site Author
author:
  name             : *name 
  avatar           : *teaser   
  bio              : "가만히 있기 좋아하는 사람"
  location         : "태양계 지구"  
  email            : "ranfort77@gmail.com"  
  links:
    - label: "Website"
      icon: "fas fa-fw fa-link"
      url: *url  
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: *github      
# Site Footer
footer:
  links:
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: *github
      
# Defaults
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: # true
      share: true
      related: true

  # _pages
  - scope:
      path: ""
      type: pages
    values:
      layout: single
      author_profile: true
      read_time: false
      comments: false
      share: true
      related: false      
```

설정이 완료되면 `bundle exec jekyll serve`를 한 후, `localhost:4000`에서 변화를 확인한다.

## 5. About, 404 페이지 만들기

`_pages/` 폴더를 만든다. 그리고 우선 `about.md` 파일을 만들고 내용을 아래와 같이 작성한다.

```markdown
---
title: "가만히 있기 좋아하는 사람의 블로그"
permalink: /about/
layout: single
---

## 여기에 이 블로그 설명에 대해서 적는다.
```

`404.md` 파일을 만들고 아래와 같이 작성한다.

```markdown
---
title: "Page Not Found"
excerpt: "Page not found. Your pixels are in another canvas."
permalink: /404.html
author_profile: false
---

요청하신 페이지를 찾을 수 없습니다.

<script>
  var GOOG_FIXURL_LANG = 'en';
  var GOOG_FIXURL_SITE = {% raw %}{{ site.url }}{% endraw %}
</script>
<script src="https://linkhelp.clients.google.com/tbproxy/lh/wm/fixurl.js">
</script>
```

## 6. 네비게이션 만들기

앞으로 카테고리, 태그, About 페이지를 만들 것이다. 그래서 네이게이션에 이 페이지가 연결되도록 한다. `_data/navigation.yml`을 아래와 같이 수정한다.

```yaml
# main links
main:
  - title: "카테고리"
    url: /categories/
  - title: "태그"
    url: /tags/
  - title: "About"
    url: /about/
```

## 7. 카테고리, 태그 페이지 만들기

모든 카테고리들을 볼 수 있는 카테고리 페이지를 만든다. `_pages/category-archive.md` 를 만들고 아래와 같이 작성한다.

```markdown
---
title: "Posts by Category"
layout: categories
permalink: /categories/
author_profile: true
---
```

모든 태그들을 볼 수 있는 태그 페이지를 만든다. `_pages/tag-archive.md`를 만들고 아래와 같이 작성한다.

```markdown
---
title: "Posts by Tag"
permalink: /tags/
layout: tags
author_profile: true
---
```

## 8. 한글 폰트 변경

한글 폰트 나눔 고딕을 적용한다. 

`assets/css/main.scss`에 다음을 추가한다.

```scss
@import url('https://fonts.googleapis.com/css?family=Nanum+Gothic');
```

`_sass/minimal-mistakes/_variables.scss` 안에 아래와 같이 `"Nanum Gothic"`을 추가한다.

```scss
/* system typefaces */
$serif: Georgia, Times, serif !default;
$sans-serif: -apple-system, BlinkMacSystemFont, "Nanum Gothic", "Roboto", "Segoe UI",
  "Helvetica Neue", "Lucida Grande", Arial, sans-serif !default;
```

## 9. 본문 사이즈 고정

`_sass/minimal-mistakes/_reset.scss` 파일을 아래와 같이 수정한다.

```scss
  @include breakpoint($medium) {
    font-size: 18px;
  }

  @include breakpoint($large) {
    font-size: 18px;
  }

  @include breakpoint($x-large) {
    font-size: 18px;
  }
```

## 10. 메뉴바 사이즈 조정

`_sass/minimal-mistakes/_masthead.scss`에서 아래와 같이 수정

```scss
    /* padding: 1em; */
    padding: 0.4em;
    /* font-family: $sans-serif-narrow; 밑에 추가 */
    /* devinlife : 메뉴바 텍스트 설정 */
    font-size: $type-size-4;
    font-weight: bold;
```

## 11. 포스트 페이지 수정

개개 포스트 페이지에 들어가면, 포스트 상단에 있는 읽은 시간을 없애고, update 시간을 배치하는 방법

 `_layout/single.html`에서 `<header>` 부분을 아래와 같이 수정

```html
{% raw %}
<!--
{% if page.read_time %}
    <p class="page__meta"><i class="far fa-clock" aria-hidden="true"></i>{% include read-time.html %}</p>
{% endif %}
-->
{% if page.last_modified_at %}
    <p class="page__date"><strong><i class="fas fa-fw fa-calendar-alt" aria-hidden="true"></i> {{ site.data.ui-text[site.locale].date_label | default: "Updated:" }}</strong> <time datetime="{{ page.last_modified_at | date: "%Y-%m-%d" }}">{{ page.last_modified_at | date: "%B %d, %Y" }}</time></p>
{% elsif page.date %}
    <p class="page__date"><strong><i class="fas fa-fw fa-calendar-alt" aria-hidden="true"></i> {{ site.data.ui-text[site.locale].date_label | default: "Updated:" }}</strong> <time datetime="{{ page.date | date_to_xmlschema }}">{{ page.date | date: "%B %d, %Y" }}</time></p>
{% endif %}
{% endraw %}
```

각 포스트들을 나열한 페이지에서 날짜를 보여주도록 하는 방법

`_includes/archive-single.html` 을 아래와 같이 수정하면 될 줄 알았는데 안되네..

```html
{% raw %}
<!--
{% if post.read_time %}
    <p class="page__meta"><i class="far fa-clock" aria-hidden="true"></i> {% include read-time.html %}</p>
{% endif %}
-->
{% if page.date %}
    <p class="page__date"><i class="fas fa-fw fa-calendar-alt" aria-hidden="true"></i> {{ site.data.ui-text[site.locale].date_label | default: "Date:" }}<time datetime="{{ page.date | date_to_xmlschema }}">{{ page.date | date: "%B %d, %Y" }}</time></p>
{% endif %}
{% endraw %}
```

검색해 보니 같은 질문 한 사람이 있었다.(<https://github.com/mmistakes/minimal-mistakes/issues/550>)

`_includes/archive-single.html`을 아래와 같이 수정

```html
{% raw %}
<p class="page__meta">
{% if post.date %}
    <i class="fa fa-fw fa-calendar" aria-hidden="true"></i> <time datetime="{{ post.date | date_to_xmlschema }}">Start: {{ post.date | date: "%B %d, %Y " }}</time>&emsp;
{% endif %}
{% if post.read_time %}
    <i class="fa fa-clock-o" aria-hidden="true"></i>&nbsp;{% include read-time.html %}
{% endif %}
</p>
{% endraw %}
```

## 12. mathjax

mathjax가 안 됐었는데, 해결방법은 [Jekyll theme 기반의 정적 블로그 만들기](https://github.com/YoungestSalon/TIL/blob/master/Jekyll%20theme%20%EA%B8%B0%EB%B0%98%EC%9D%98%20%EC%A0%95%EC%A0%81%20%EB%B8%94%EB%A1%9C%EA%B7%B8%20%EB%A7%8C%EB%93%A4%EA%B8%B0.md)에서 찾았다. `_includes/scripts.html`의 맨 아래에 다음 코드를 추가한다.

```html
{% raw %}
{% if page.mathjax %}
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
{% endif %}
{% endraw %}
```

그런 후 포스트를 쓸 때, `page.mathjax: true`를 입력한다.


$$
\sigma = \sqrt{ \frac{1}{N} \sum_{i=1}^N (x_i -\mu)^2}
$$


## 13. 여기까지 한 후, github 동기화

github 동기화 한 후, mathjax는 되는데 폰트적용은 안되고 반응형 폰트도 적용이 안된다. 그런데 5분이상 기다리니까 적용됐다. 생각보다 github 적용 시간이 오래 걸린다.

## 14. Disqus 계정 가입 및 댓글 기능 추가

그대로 따라하여 댓글 기능 추가 완료

## 15. 계속 추가할 계획

(...진행 중...)

