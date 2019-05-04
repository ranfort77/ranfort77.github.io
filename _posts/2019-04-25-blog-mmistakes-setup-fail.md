---
title: "Jekyll 테마 minimal-mistakes 적용 실패한 기록"
excerpt: "jekyll 테마를 사용해서 블로그를 만들기 위해 꾀 많은 노력을 기울였으나 실패한 기록"
toc: true
toc_sticky: true
categories:
  - blog
tags:
  - jekyll
  - minimal-mistakes
last_modified_at: 2019-04-26T22:40
---

Github 블로그를 만들기 위해, Jekyll 테마인 minimal-mistakes를 적용한 기록이다. 이 방식으로는 만족스러운 테마를 못 만들었다. 그래도 기록은 남겨둔다.

## 1. 환경

* 운영체제: Windows 7 64bit
* Windows git: [Git-2.21.0-64-bit.exe](https://git-scm.com/downloads)
* Windows Ruby: [Ruby+Devkit 2.5.3-1 x64](https://rubyinstaller.org/downloads/)
* `gem install jekyll bundler`
  * jekyll 3.8.5
  * bundler version 2.0.1


## 2. 필요한 선행 학습

구글에서 "github 블로그"를 검색하면 많은 블로그 자료들이 검색된다. 검색되는 순서대로 살펴보면 "만들기 쉽다", "간단하다"라는 말이 많이 나오는데 이것은 대부분 IT 관련 전공자, 현업 종사자 또는 IT관련 사전지식이 많은 사람들에게만 해당되는 이야기이다. 모든 공부가 그렇지만 관련 용어에 익숙하지 않고 원리도 모르는 상태에서 뭔가를 따라하기만 하면 진행은 되지만 *"왜 이렇게 해야 하지?"*라는 생각 때문에 따라하는 내내 답답함을 느낀다. 또는 똑같이 따라했는데 중간중간 막히는 부분이 생긴다. 그런데 원리를 모르니까 왜 막혔는지 또는 어떻게 해결해야 하는지를 알 수 없게 된다. 나는 Linux 운영체제 하에서 python으로 간단한 script 프로그램을 작성해 본 경험만 있어서 그럴듯한 github 블로그를 만드는 과정이 매우 힘들었다. 테마를 적용하는데 까지 아래와 같은 것들에 대해 얕은 지식이라도 있어야 가능했다.

### Linux commands
github에 블로그 호스팅을 하려면 git 명령어를 CLI (Command Line Interface) 환경에서 입력 해야한다. 명령어 입력이 익숙하지 않은 사람은 불편함을 느끼게 되고 모르는 명령어가 나오면 머리가 매우 복잡할 것이다. 그리고 경로확인(`pwd`), 파일목록(`ls`), 폴더생성(`mkdir`), 디렉토리이동(`cd`), 빈파일생성(`touch`), 파일편집(`vi`) 등의 명령들이 설명에 나오거나 또는 간간히 사용된다. 따라서 자주 사용하는 간단한 linux 명령어는 알아두면 좋다.

### Markdown
블로그 포스트 작성 및 페이지 작성 등을 Markdown 문법으로 작성하게 된다. Markdown 문법을 연습하는 방법은 jupyter notebook을 사용하거나 다른 마크다운 에디터를 사용하면 될 것이다.

### git
`init`, `add`, `commit`, `push`, `pull` 같은 명령어를 CLI로 입력해야 하고 의미도 알아야 한다. 초보자 입장에서 git에 대해 매우 잘 설명해 주는 곳은 [생활코딩: 지옥에서 온 Git](https://opentutorials.org/course/2708)이다. 

### github
우선 github에 가입해야 한다. 그리고 github의 기능 중 하나인 `fork`의 의미를 알아 둘 필요가 있다. 

### Ruby
Ruby에 대해서는 몰라도 되지만, 우리가 사용할 jekyll이 ruby gem (모듈이나 library 같은)이기 때문에 관련 용어가 나올 수 있다. python에 대한 사전지식이 있으면 Ruby는 비교적 쉽게 접근할 수 있다. 초보자가 Ruby 문법을 간단히 익히는데에는 역시 [생활코딩: Python & Ruby](https://opentutorials.org/course/1750)를 보면 좋다.

### RubyGem
`gem`, `bundle` 같은 명령어를 계속 사용하게 된다.

### HTML + CSS
원하는 형태로 블로그를 만들거나 수정할 때 HTML tag와 CSS 관련 용어의 의미를 모르면 굉장히 답답하다. 초보자라면 역시 [생활코딩: WEB1 - HTML & Internet](https://www.opentutorials.org/course/3084), 그리고 [생활코딩: WEB2 - CSS](https://www.opentutorials.org/course/3086)을 추천한다.

### Liquid
원하는 형태로 블로그를 수정하기 위해 liquid 문법을 조금 알아야 한다. 그렇지 않으면 굉장히 답답하다. Liquid를 가장 적당한 난이도로 가르쳐 주는 곳은 [Learn Jekyll & CloudCannon](https://learn.cloudcannon.com/)이다. 매우 좋은 설명과 예제가 있다. 반드시 읽어볼 필요가 있다. 블로그 customizing을 잘하려면 liquid를 잘 알아야 한다.

### 그밖의 참고 문서

#### [Jekyll DOCS](https://jekyllrb-ko.github.io/docs/home/)
완전 초보자 입장에서 읽으면 무슨 소린지 전혀 이해되지 않는다. jekyll 사용이 익숙해 지고 어느정도 이해가 되면 하나씩 하나씩 이해가 된다.

#### [minimal-mistakes Quick-Start Guide](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
여기서 적용하고자 하는 테마의 설명서다. 그러나 초보자 입장에서 보면 이해가 잘 안된다. 그리고 jekyll 테마들 마다 테마를 만든 개발자들이 조금씩 사용방법이 다르다. 이 설명서 역시 jekyll에 대한 이해가 조금씩 올라감에 따라서 더 잘 읽히고 이해가 된다.


## 3. Gem 기반 방법

[Minimal Mistakes Quick-Start Guide](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)의 Installing the theme를 읽어보면 테마를 설치하는 방식은 아래와 같이 3가지가 있다.

* Gem 기반 방법
* Remote 테마 방법
* Fork 방법 (또는 zip 다운로드 방법)

여기서는 gem 기반 방법을 설명한다. gem 기반 방법은 minimal-mistakes 테마 파일들을 RubyGems이 설치되어 있는 폴더에 저장하는 방식이다. 윈도우의 경우 RubyGems의 설치경로는 `"C:\Ruby25-x64\lib\ruby\gems\2.5.0\gems"` 이다. 이 경로로 가보면 많은 RubyGems이 설치되어 있음을 알 수 있다. 여러 블로그에서 Fork 방법 (Zip 다운로드 방법)으로 설명해 준다. Fork 방법은 다운받은 폴더 자체에 테마관련 코드들이 다 들어있고 블로그를 내 마음대로 꾸미려면 테마관련 코드 파일들을 수정해야 한다. 즉, 원본을 조금씩 수정해야 한다. 그래서 이 방식은 경험이 많은 사람이 하면 좋은데 초보자인 입장에서 테마 코드 원본을 수정하면 안될 것 같다. gem 기반 방법은 테마 관련 주요 파일은 RubyGems 폴더에 담겨 있고 다른 폴더를 블로그 폴더로 해서 간접적으로 테마코드를 가져와서 쓰거나 재정의 하는 방식으로 쓸 수 있다. 그리고 이후에 테마가 업데이트 사항이 있을 때 `bundle update` 명령으로 쉽게 업데이트 할 수 있는 장점이 있다.     

### 테마 설치

`Git Bash` 터미널을 실행한다. 먼저 아래 명령을 입력하여 minimal-mistakes 테마가 설치되어 있는지 확인해 보자. 아래 명령은 minimal-mis로 시작하는 gem이 설치되어 있는지 자세히(`-d`) 보여주는 명령이다.

```bash
$ gem list -d minimal-mis
```

minimal-mistakes 테마가 설치되어 있지 않기 때문에 당연히 아무 내용도 출력되지 않는다. 그 다음 rubygem 관리 커뮤니티인 rubygems.org에 minimal-mistakes가 있는지 확인해 보자.

```bash
$ gem search minimal-mis
minimal-mistakes-jekyll (4.16.0)
```

그러면 `minimal-mistakes-jekyll (4.16.0)`가 출력된다. 이제 설치를 한다.

```bash
$ gem install minimal-mistakes-jekyll
Successfully installed minimal-mistakes-jekyll-4.16.0
Parsing documentation for minimal-mistakes-jekyll-4.16.0
Installing ri documentation for minimal-mistakes-jekyll-4.16.0
Done installing documentation for minimal-mistakes-jekyll after 4 seconds
1 gem installed
```

`Successfully installed ...` 라는 말이 뜨면 설치 성공이다. 설치되어 있는지 다시 확인해 본다.

```bash
$ gem list -d minimal-mis
minimal-mistakes-jekyll (4.16.0)
    Author: Michael Rose
    Homepage: https://github.com/mmistakes/minimal-mistakes
    License: MIT
    Installed at: C:/Ruby25-x64/lib/ruby/gems/2.5.0

    A flexible two-column Jekyll theme.
```

설치전과는 다르게 minimal-mistakes 테마의 개발자, 홈페이지, 라이선스, 설치 경로 등의 정보가 출력된다. RubyGems 폴더에 가보면 여러 테마관련 파일들이 있음을 알 수 있다.

```bash
$ pwd
/c/Ruby25-x64/lib/ruby/gems/2.5.0/gems/minimal-mistakes-jekyll-4.16.0

$ ls
CHANGELOG.md  README.md  _includes/  _sass/
LICENSE       _data/     _layouts/   assets/
```

위와 같이 터미널 명령이 아닌 윈도우 탐색기로 직접 확인해 볼 수 있다.


### site root, Gemfile

이제 블로그 관련 파일들이 저장되는 폴더를 생성한다. 여기서는 D드라이브에 "myblog"라는 폴더를 만들었다. 따라서 경로는 "D:\myblog"이다. 이제 저 경로를 `<site root>` 라고 부른다. `<site root>`에 `Gemfile`을 만들고, 안에 아래와 같이 적는다.

```bash
gem "minimal-mistakes-jekyll"
```

**주의:** 앞으로 모든 파일을 만들 때 그 파일의 encoding은 UTF-8이어야 한다. 윈도우에서는 특히 조심해야 한다. 본 포스트를 작성하는 시점에서는 EditPlus를 사용하여 UTF-8로 작성하고 있다. 또한 BOM 헤더가 없어야 한다. 윈도우 메모장으로 파일을 작성하거나 저장하거나 수정할 때 자기도 모르는 사이에 encoding이 ANSI로 되는 경우가 있다. 그리고 윈도우 메모장으로 UTF-8로 저장하더라도 BOM 헤더가 추가되는 경우가 있다. 이렇게 되면 jekyll이 site build를 할 때 encoding 에러가 나는 경우가 많다. 나중에 이런 경우가 생기면 만든 파일 encoding을 항상 확인해 보는게 좋다.
{: .notice--warning}

이제 아래와 같이 `<site root>`에서 `git bash` 터미널을 열고 `bundle` 명령을 실행한다.

```bash
$ pwd
/d/myblog

$ ls
Gemfile

$ bundle
Resolving dependencies...
Using concurrent-ruby 1.1.5
...
Bundle complete! 1 Gemfile dependency, 46 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.

$ ls
Gemfile  Gemfile.lock
```

그러면 터미널에 `minimal-mistakes` 테마 사용을 위한 의존성 gems에 대한 정보가 화면에 출력되고, `<site root>`에 `Gemfile.lock` 파일이 생성되어 안에 의존성 gems에 대한 기록된다. 이렇게 하면 이 테마를 사용할 때 필요한 gems이 일관되게 유지된다.

### Configuration

`<site root>`에 `_config.yml` 파일을 만든다. 그리고  [default \_config.yml](https://github.com/mmistakes/minimal-mistakes/blob/master/_config.yml) 링크로 들어가서 내용을 복사한 후 `_config.yml`에 붙여넣는다. 이것은 minimal-mistakes 테마에 필요한 대부분의 설정이 거의 다 적혀 있다. 몇 가지 중요 부분을 수정할 것이다. 모든 configuration의 의미는 [Configuration](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)을 참고한다. 여기서는 우선 필요한 것 위주로 설명한다.


#### Gem 기반 테마 사용
theme의 주석을 해제한다. 이것은 gem 기반 테마를 사용한다는 의미로 RubyGems 폴더에 설치된 minimal-mistakes 테마 파일들을 사용한다는 뜻이다.

```yaml
theme                  : "minimal-mistakes-jekyll"
```

#### Site settings
Site settings 부분은 아래와 같이 바꾼다. locale은 블로그에 주요 gui 버튼들이나 interface 들의 언어설정이다. title은 블로그 이름이다. name은 블로그 주인의 이름이다. description은 블로그 간단 소개이다. title, name, description 모두 한글이 허용된다.

```yaml
# Site settings
locale                   : "ko-KR"
title                    : "Quiet Air"
name                     : "Freesia Boy"
description              : "Memory Archive"
```

#### Site Author
이제 site author로 가서 아래와 같이 수정한다. name은 역시 자기 이름이고, avatar에는 사진을 입력할 수 있는데 보통 `<site root>`에 assets 폴더를 만들고 그 안에 images 폴더를 만든 후 사진을 저장해 둔다. 그 경로의 링크를 입력해 주면 되는데 아직 준비가 안된 경우 주석으로 남겨둔다. bio는 자기소개이다. 쓰고 싶은걸 쓰면 된다. location은 주소, email 등을 입력하면 된다.

```yaml
# Site Author
author:
  name             : "Freesia Boy"
  avatar           : # "/assets/images/bio-photo.jpg"
  bio              : "I am an ordinary person."
  location         : "Somewhere"
  email            : "aaa@bbb.ccc"
```

우선 기본적인 설정은 완료 되었다.


### 로컬 서버 실행

`bundle exec jekyll serve` 명령으로 `localhost:4000`에 블로그를 띄운다. 

```bash
$ bundle exec jekyll serve
Configuration file: D:/myblog/_config.yml
            Source: D:/myblog
       Destination: D:/myblog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
        Pagination: Pagination is enabled, but I couldn't find an index.html page to use as the pagination template. Skipping pagination.
  Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/main.scss':
                    Invalid CP949 character "\xE2" on line 54
jekyll 3.8.5 | Error:  Invalid CP949 character "\xE2" on line 54
```

위와 같이 에러가 발생하면서 진행이 중단된다. 이것은 윈도우 운영체제에서 발생하는 에러로 아래와 같이 `chcp 65001` 명령으로 코드페이지를 바꾸면 해결된다.

```terminal
$ /c/Windows/System32/chcp 65001
Active code page: 65001
```

다시 `bundle exec jekyll serve`를 실행한다.

```bash
$ bundle exec jekyll serve
Configuration file: D:/myblog/_config.yml
            Source: D:/myblog
       Destination: D:/myblog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
        Pagination: Pagination is enabled, but I couldn't find an index.html page to use as the pagination template. Skipping pagination.
                    done in 2.028 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'D:/myblog'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```

pagination 관련 skipping이 일어났지만, 블로그 첫 화면에 해당하는 index.html을 추가하면 pagination skipping은 해결된다.  

`<site root>`에 가보면 `.sass-cache`, `_site` 폴더가 생성되었음을 알 수 있다. `_site` 폴더가 실제 블로그 페이지에 해당하는 html 들과 필요한 resouce가 저장되는 곳이다. jekyll이 `<site root>`에 있는 .markdown, .md, .html 파일들을 build 하여 `_site` 폴더의 지정한 위치가 놓이게 한다.

우선 크롬에서 `localhost:4000`으로 접속해 본다. 그 다음 `index.html` 을 만들고 안에 내용은 다음과 같이 하자.

```yml
---
layout: home
author_profile: true
---
```

**주의:** pagination은 `<site root>`의 `index.html`으로만 작동하며 `index.md`로는 작동하지 않는다. 또는 `_pages` 폴더를 만들어서 `home.md` 또는 `home.html`를 만들어서 permalink: /를 해도 작동하지 않는다. (확인 요!!) 이것에 관련된 내용은 [페이지 나누기](https://jekyllrb-ko.github.io/docs/pagination/)를 참고하라.
{: .notice--warning}

몇가지 확인하고 넘어가야 한다. 우선 왜 우측 상단에 `Quick-Start Guide` 네비게이션이 생겼는가? 그리고 그것을 클릭하면 왜 `minimal-mistakes` 페이지로 링크 되는가?
pagination이 무엇인가?
네비게이션 설정? 디자인?
포스트 태그, 카테고리 방식으로 마크다운 작성

### 앞으로 조사 할일

* 풋터 작성
* _pages 폴더 include
* Archives 설정의 이해
* categories 페이지 만들기
* tag 페이지 만들기
* 포스트 페이지 만들기
* Collection 페이지 만들기
* 디폴트 설정
* 컬렉션
* 포스트의 Updated 정보 위로 올리기
* 포스트의 태그, 카테고리 정보 위로 올리기
* SHARE ON 정보 없애기
* 한글 폰트 설정 방법
* 화면 width 설정 방법
* MathJax와 inline math 가 안된다. 되게 하라.
* Post 날짜 표시; last_modified_at 은 포스트 안에 Update로 표시가 되는 상태이다.