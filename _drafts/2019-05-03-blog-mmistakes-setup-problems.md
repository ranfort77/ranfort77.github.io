---
title: "minimal-mistakes 설정 문제점 기록"
excerpt: "윈도우7에서 Jekyll theme minimal-mistakes 설정 문제점 기록"
toc: true
toc_sticky: true
categories:
  - blog
tags:
  - jekyll
  - minimal-mistakes
last_modified_at: 2019-05-03T23:30:00-05:00
---


본 문서는 윈도우 7 환경에서 jekyll theme 중 하나인 [Minimal-mistakes](<https://mmistakes.github.io/minimal-mistakes/>) 적용 과정에서 겪었던 문제에 대해서 기록한다. 이것을 기록하는 이유는 경험했던 문제를 나중에 분명히 잊어버릴 것이기 때문에 다시 고생하지 않기 위함이다.

인터넷에서 jekyll 블로그를 검색했을 때, 대부분의 사람들이 MAC 환경 또는 리눅스 환경(주로 Ubuntu)에서 만드는 것을 가정하고 설명한다. 대부분 현직개발자 이거나, 개발을 전공하고 있거나 또는 개발에 관심이 많은 사람들이기 때문에 터미널 환경이 익숙하고 거부감이 없으며 관련 배경지식과 경험이 많기 때문에 어떤 문제가 발생했을 때 문제의 원인을 금방 파악하고 해결한다. 

그러나 나같은 개발 경험이 없는 사람은 몇 가지 문제가 발생한다. MAC도 없고 Linux도 없기 때문에 윈도우 환경에서 jekyll site 구축 설명을 읽어가면서 따라하게 된다. 경험해 본 바 윈도우 환경에서 jekyll site 구축 시 다음과 같은 문제가 생길 수 있다.

* 인코딩 문제: 관련 키워드는 `CP949`, `UTF-8`, `chcp 65001`
* 문서 인코딩과 BOM 문제: `ANSI`, `UTF-8`
* 경로 문제: 블로그 폴더의 위치가 어디에 있는가?
* 테마 적용의 방식 문제: gem 기반? remote theme 기반? git clone 기반? zip 다운로드 기반?
* 한글 폰트 적용 문제: gem 기반 테마 설정 시 문제가 발생
* Timezone 설정 문제
* categories 한글 문제

## 환경

이 문서를 작성 할 때 환경은 다음과 같다.

* 운영체제: Windows7 64bit

* 터미널: git bash

* Jekyll version: 3.8.5

* bundler version: 2.0.1

## 배경 지식

Minimal-mistakes 테마를 사용한 jekyll 블로그를 만들기를 알아볼 때, 주로 두 문서를 참고하였다.

* [jekyll 한국어 홈페이지 Docs](<https://jekyllrb-ko.github.io/>)
* [Minimal-mistakes Guide](<https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/>)

그러나 완전 초보자가 저 두 문서를 읽어보면 무슨 소린지 하나도 못 알아 듣는다. 그 이유는 배경지식이 부족하기 때문이다. jekyll 블로그를 만들 때 조금씩 이라도 필요한 배경지식은 `git`, `github`, `YAML`, `markdown`, `HTML`, `CSS`,  `Ruby`, `Rubygem`, `bundler`, `linux commands` 등이다. 어떤 것은 조금은 다룰 줄 알아야 하고 어떤 것은 관련 용어 정도만 알고 있어도 된다.

구글에서 **"jekyll 블로그"** 또는 **"jekyll 테마 적용"** 등으로 검색하면 아주 많은 자료가 나온다. 빠르게 쓸만한 jekyll 블로그 만들어 쓰자고 하는 목적이면 이 자료들을 따라하면 된다. 대부분  배경지식이 많은 사람들이 쓴 글들이라서 초보자가 따라하기에는 생략된 내용이 많거나 끝까지 설명 안해주는 파편화된 자료가 많다.  반면 최근 매우 좋은 jekyll site 설명 블로그를 발견하였다.

* [취미로 코딩하는 개발자: 하우투](<https://devinlife.com/howto/>) 

위 블로그는 Minimal-mistakes 테마를 적용하는 과정을 설명하는 시리즈로 지금까지 발견한 블로그 중 가장 친절하고 완결성이 있다. 위 블로그에서는 `git clone` 을 이용해서 Minimal-mistakes 테마 적용법을 설명해 준다. 그런데 따라하다 보니 시작부터 문제가 발생한다.

## 상황 1. 경로 문제

현재 내 윈도우의 드라이브는 `C`와 `D`로 나눠져 있다. 대부분의 중요 데이터를 D드라이브에 저장하고 있고 기존에 관리하던 여러 파일들이 있기 때문에 블로그 폴더를 `D:\[Projects]\proj20_blog` 아래에 만들기로 했다. 경로 중에 `[Projects]` 에 `square braket`을 사용한 이유는 윈도우에서 이름정렬 기준으로 다른 영어나 한글로만 된 폴더보다 상단에 배치해 주기 때문이다.

### git clone

앞으로의 과정은 [취미로 코딩하는 개발자: 하우투](<https://devinlife.com/howto/>) 를 따라하는 것이다. `D:\[Projects]\proj20_blog\`에서 `git clone`으로 Minimal-mistakes 테마를 가져오기로 한다.

```bash
$ pwd
/d/[Projects]/proj20_blog

$ git clone https://github.com/mmistakes/minimal-mistakes.git
Cloning into 'minimal-mistakes'...
remote: Enumerating objects: 15724, done.
remote: Total 15724 (delta 0), reused 0 (delta 0), pack-reused 15724
Receiving objects: 100% (15724/15724), 41.62 MiB | 1.24 MiB/s, done.
Resolving deltas: 100% (9174/9174), done.
Checking out files: 100% (719/719), done.

$ cd minimal-mistakes
$ ls -a
./              LICENSE      docs/
../             README.md    index.html
.editorconfig   Rakefile     minimal-mistakes-jekyll.gemspec
.git/           _config.yml  package-lock.json
.gitattributes  _data/       package.json
.github/        _includes/   screenshot-layouts.png
.gitignore      _layouts/    screenshot.png
.travis.yml     _sass/       staticman.yml
CHANGELOG.md    assets/      test/
Gemfile         banner.js

$ pwd
/d/[Projects]/proj20_blog/minimal-mistakes
```

위와 같이 `minimal-mistakes/` 폴더가 생성되고 그 안에 테마 관련 파일들이 다운로드 된다.

보통 jekyll 블로그에 대한 여러 블로그를 살펴보면 `git clone`으로 설명해 주기도 하고, `zip`을 다운 받으라고 하기도 하고, 최신 개발 버전이 아닌 안정화되어 있는 `release zip`을 다운 받으라고 하기도 한다. 차이를 이해하려면 `git`을 알아야 한다. 그러나 여기서는 일단 설명대로 그냥 따라하자. 위에서 볼 수 있듯이 많은 파일과 폴더들이 있다. 저중에 필요없는 파일들이 있지만 역시 그냥 넘어가고 설명을 따라한다. 

### bundle

`bundle` 을 실행한다. 

```bash
$ bundle

[!] There was an error parsing `Gemfile`: There are no gemspecs at D:/[Projects]/proj20_blog/minimal-mistakes. Bundler cannot continue.

 #  from D:/[Projects]/proj20_blog/minimal-mistakes/Gemfile:2
 #  -------------------------------------------
 #  source "https://rubygems.org"
 >  gemspec #  source "https://rubygems.org"
 #  -------------------------------------------
 
```

그러나 , 위와 같이 경로에 `gemspec` 이 없다면서 진행이 안 된다. 아마 블로그 폴더에 있는 `minimal-mistakes-jekyll.gemspec`이 gemspec 파일인데 인식을 못하는 것 같다.

여기서 굉장히 고생했는데 알고보니 **원인은 경로에 포함된 square braket 때문이었다.** 보통 어떤 응용 프로그램들은 **한글 경로**가 포함된 곳에 설치하면 올바르게 동작하지 않는 경우가 있다. 그리고 리눅스를 자주 사용하거나 프로그래밍을 자주 하는 사람들은 폴더를 만들 때 변수명과 비슷한 방식으로 알파벳과 숫자, 밑줄의 조합만으로 짖는 경우가 많다. 그러나 윈도우에서는 폴더명을 지을 때 공백(space)이나 괄호, 대괄호 등도 쓸 수 있기 때문에 필요에 따라 자주 그렇게 사용했었다.

어쨌든 앞으로 경로명을 지을 때 다음 규칙을 지키도록 한다.

* 한글은 사용하지 않는다.
* 특수 문자를 사용하지 않는다.
* 알파벳, 숫자, 밑줄(`_`), 하이픈(`-`)만 사용한다.
* 변수명을 지을때는 반드시 알파벳으로 시작하지만 (python 같은 언어에서는 `_`로도 시작할 수 있지만), 폴더명의 경우 숫자로 시작해도 될 것이라고 본다. 그러나 되도록 주의는 기울인다.

저 규칙대로 아래와 같이 경로를 다시 만들고 진행해 보았다.

```bash
$ pwd
/d/projects/proj20_blog

$ git clone https://github.com/mmistakes/minimal-mistakes.git
Cloning into 'minimal-mistakes'...
remote: Enumerating objects: 15724, done.
remote: Total 15724 (delta 0), reused 0 (delta 0), pack-reused 15724
Receiving objects: 100% (15724/15724), 41.62 MiB | 1.19 MiB/s, done.
Resolving deltas: 100% (9174/9174), done.
Checking out files: 100% (719/719), done.

$ cd minimal-mistakes/
$ bundle
Fetching gem metadata from https://rubygems.org/..........
Fetching gem metadata from https://rubygems.org/.
(... 이하 생략 ...)

$ ls -a
./              Gemfile.lock  banner.js
../             LICENSE       docs/
.editorconfig   README.md     index.html
.git/           Rakefile      minimal-mistakes-jekyll.gemspec
.gitattributes  _config.yml   package-lock.json
.github/        _data/        package.json
.gitignore      _includes/    screenshot-layouts.png
.travis.yml     _layouts/     screenshot.png
CHANGELOG.md    _sass/        staticman.yml
Gemfile         assets/       test/

```

잘된다. `bundle` 이 완료되면 `Gemfile.lock`파일이 생성된다. 

~~그리고 clone을 하고 bundle을 하니까 gem library에 minimal mistakes가 설치되지 않았다~~

## 상황 2. 인코딩 문제 (코드페이지)

이제 로컬 서버를 시작하기 위해 아래와 같이 `bundle exec jekyll serve`를 한다.

```bash
$ bundle exec jekyll serve
Configuration file: D:/projects/proj20_blog/minimal-mistakes/_config.yml
            Source: D:/projects/proj20_blog/minimal-mistakes
       Destination: D:/projects/proj20_blog/minimal-mistakes/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
  Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/main.scss':
                    Invalid CP949 character "\xE2" on line 54
jekyll 3.8.5 | Error:  Invalid CP949 character "\xE2" on line 54

```

위와 같이 정상 작동하지 않고 Conversion error가 발생한다. 이 문제는 아래와 같이 입력하면 해결된다.

```bash
$ /c/Windows/System32/chcp 65001
Active code page: 65001
```

`c/Windows/System32/chcp` 명령어는 CHange Code Page의 약자로 `chcp 65001`은 UTF-8 코드 페이지로 바꾼다는 의미 같다. (엄밀한 원리는 모른다. [CHCP 참고링크](<https://ss64.com/nt/chcp.html>)) 이 문제는 jekyll 홈페이지 [윈도우즈에서의 Jekyll](<https://jekyllrb-ko.github.io/docs/windows/>)에 언급되어 있다. 

이제 다시 `bundle exec jekyll serve`를 하면 된다.

```bash
$ bundle exec jekyll serve
Configuration file: D:/projects/proj20_blog/minimal-mistakes/_config.yml
            Source: D:/projects/proj20_blog/minimal-mistakes
       Destination: D:/projects/proj20_blog/minimal-mistakes/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 2.02 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'D:/projects/proj20_blog/minimal-mistakes'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.

```

서버는 시작되고 크롬에서 `localhost:4000`으로 접속하면 jekyll 블로그 초기화면을 볼 수 있다. 이제 필요없는 파일은 삭제해도 된다. [Minimal Mistakes Quick-Start Guide](<https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/>) Remove the Unnecessary 를 참고. 그런데 `minimal-mistakes-jekyll.gemspec`이 없으면 다시 `bundle exec jekyll serve`를 실행하면 gemspec 에러가 나니까 일단 `minimal-mistakes-jekyll.gemspec`은 나둔다. 왜 이런지는 `bundle`에 대한 이해가 필요하다.



