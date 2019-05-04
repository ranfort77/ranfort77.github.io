---
title: "GitHub 블로그 만들기에 대한 초기 생각들"
excerpt: "아무것도 모를때 했던 생각들과 시도들을 남겨 둔다."
categories:
  - blog
tags:
  - jekyll
  - ruby
last_modified_at: 2019-04-12T12:00:00-05:00
---



`git` , `github`, `jekyll`에 대해 아무것도 모를 때 했던 생각들과 시도의 기록을 남겨둔다.

## 1. 복잡한 생각들

최근에 Python 관련 공부한 것들을 정리하기 위해 기존에 사용하던 네이버 카페와 구글 블로거를 사용했다. 그런데 여러모로 불편한 점이 많았다. 디자인, 가독성, 코드 입력, 카테고리 등등... 뭔가 조금씩 아쉽고 불편했다. 내가 사용하는 방법을 잘 몰라서 그런건지 아니면 한 3년정도 사용해봐야 잘 사용하게 되는건지 몰라도 주위에 물어볼 사람도 없고 여튼 답답하다.

관심있는 정보를 구글에서 검색해서 들어가면 블로깅 잘한다 싶은 사람들은 티스토리를 쓰는 것 같다. 그런 의미에서 한때 티스토리 사용법을 알아볼까 고민한 적도 있는데 뭔가 어려울 것 같기도 하고 확 땡기는 점도 없어서 접었다. 네이버나 구글은 기존에 계정이 있어서 괜찮았는데 요즘에는 머리가 굳어서 그런지 새로운 웹사이트 가입도 어찌나 귀찮은지... 여튼 그렇다. 블로깅을 하려면 왜이렇게 알아야 할게 많은지... 내 관심분야 공부하기도 바쁜데 블로깅 하려면 알아야 할게 많은 것 같아서 그런것 하나하나 알아나가다 보면 어느새 공부하려고 했던 것은 우선순위 맨 뒤에 가 있다.

요즘에는 .github.io 형태의 주소를 갖는 블로그가 종종 보인다. 흠... 이건 뭐지? 평소에 IT분야에 관심이 있거나 현재 IT를 현업으로 하고 있는 사람들은 이미 몇년전부터 많이들 사용했나보다. 구글에서 GitHub 블로그 만들기를 검색해 봤다. 여기저기 클릭 클릭하면서 살펴보니 이것도 여간 복잡한 게 아니다. Git? GitHub? Ruby? Jekyll? Mingw? MSYS? Markdown? 등등... 흠... 뭐냐 이거... 살짝씩 용어는 들어본 것도 있고 완전히 처음 들어보는 것도 있고. 뭔가 굉장히 복잡해 보인다. 어떤 블로그에서는 *"알아야 할게 많긴 한데 GitHub 블로그 만들면 매우 좋다"*라고 한다. 그거에 또 혹해서 한번 조금씩 알아나가 보기로 한다.

## 2. 초기 조사 진행

구글에서 "깃허브 블로그"를 검색한 후, 검색결과 순서대로 블로그 게시물을 쭉 보았다. 우선 하나하나 보면서 자세히 따라하진 않고 대충 훓어 보기만 했다. 이것저것 보니까 만드는 방법이 전부 조금씩 다르다. 그래선지 이런 생각이 든다.

*"흠. 쉬워보이진 않네. 뭔가 클릭 클릭하면서 만드는게 아니고 생소한 명령어 입력이랑 복잡한 파일 수정작업이 들어가네. 쉽다고 하는데 이런쪽에 익숙하지 않은 사람은 쉬운게 아닌거 같다. 그리고 처음 만들때는 따라는 하는데 나중가면 잊어버리겠다."*

이런쪽을 많이 접한 사람들은 배경지식과 경험지식이 많기 때문에 중요하지 않은거/중요한거 구분하면서 촥촥 진행하는데, 생소한 사람은 모든게 다 어렵다. 그래도 어쩌겠어. 일단 반드시 필요한 것부터 해보았다. GitHub 블로그니까 GitHub 계정부터 만들었다.

### GitHub 계정 만들기

프로그래머나 IT관련자에게 GitHub는 매우 유명한 웹사이트 인가 보다. 근데 개발자도 아니고 IT전공자도 아니고 혼자만 사용하는 간단한 스크립트 또는 프로그램을 작성하는 사람이라면 GitHub를 잘 모른다. 왜냐하면 소스코드 버전관리도 필요하지 않고 공동개발도 하지 않기 때문이다. 그리고 주위에 누군가 *"Git이라는게 있어. 한번써봐. 이런점에서 매우 좋아"* 라고 이야기 해주는 사람도 없으면 Git 이나 GitHub를 알 수가 없다.

전에 몇 번 소스코드 버전관리를 해볼까 라는 생각에 구글에서 검색해본 적이 있었다. 그때 용어만 알아둔게 `서브버전(SVN)`이랑 `깃(Git)`이다.

그렇군. Git은 버전관리 프로그램 인가보다. 그러면 GitHub는 뭔가? 위키에 검색해보니 *"분산 버전 관리 툴인 깃(Git)을 사용하는 프로젝트를 지원하는 웹호스팅 서비스"* 라고 나온다. *"그래. GitHub는 소스코드 버전관리를 웹에서 서비스 해주는 건가보다"* 하고 대충 이해하고 넘어갔다.

일단 가입부터 하기로 했다. 나는 원래 Windows7 64bit에서 익스플로러를 쓰는데, [GitHub](https://github.com/)는 익스플로러를 더 이상 지원 안한다고 하니 크롬으로 들어갔다. 아래 그림에서 우측상단에 Sign up으로 가입을 진행했다.

![](/assets/images/2019-03-17/01_sign_up.bmp)

Step 1은 username, Email address, Password를 입력하고, Step 2, Step 3를 차례로 진행하면 계정 생성이 완료된다.

![](/assets/images/2019-03-17/02_join_github.bmp)

가입이 완료되고 로그인(Sign in)하면 아래와 같은 초기화면이 뜬다. 우측 상단에 `종`도 있고 `+`도 있고 `아이콘` 같은게 있는데 클릭해보면 서브메뉴같은게 나온다. Repository(저장소)라는 단어가 자주 나오고, 뭔가 알수없는 용어가 많다. 일단은 나중에 차차 알아보기로 했다.

![](/assets/images/2019-03-17/03_ini_screen.bmp)

### 계속 조사 진행

GitHub 계정은 만들었다. 다시 구글에서 "깃허브 블로그"를 검색해서 여러 자료를 읽어보면 계속 "Jekyll"이라는 말이 나온다. 흠. Jekyll이 뭐지? [지킬 홈페이지](https://jekyllrb.com/)에 들어가보니

*"Transform your plain text into static websites and blogs."*

라고 되어 있다. 그렇군. plain text를 정적 웹사이트나 블로그 형태로 변환시켜주는 툴인가 보구만. 일단 그렇게는 알겠는데 직접 와닫지는 않는다. 툴을 사용해보고 어떤 결과가 나오는지 직접 해봐야 와닫는다. 근데 할게 많으니 요렇게만 알고 일단 넘어간다.

그리고 여러 블로그 내용을 살펴보니 Jekyll은 Ruby 기반이라서 Ruby를 설치해야 된다는 말이 나온다. 이건 또 무슨 소리지? Ruby가 뭔가? [Ruby wiki](https://ko.wikipedia.org/wiki/%EB%A3%A8%EB%B9%84_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4))로 검색해 본다. 이렇게 나온다.

*"루비는 마츠모토 유키히로가 개발한 동적 객체 지향 스크립트 프로그래밍 언어이다."*

그렇구만. 사실 Ruby는 python과 비슷한 스크립트 언어라고 몇번 들어보기는 했다. 그러면 이제 뭘 해야 하지? 일단 여러 블로그 중에 아래 블로그를 보았다.

[참고 1. Jekyll을 이용하여 github에 블로그 만들기](https://brunch.co.kr/@hee072794/39)

참고 1을 그대로 따라 해보기로 했다. 그런데 시작부터 막힌다. Jekyll를 설치하라는데 터미널에 명령어를 입력해야한다. Jekyll을 소개하는 많은 블로거들이 Mac을 사용하는 것 같아보인다. Mac에는 Linux와 비슷하게 터미널이 있고 sudo 같은 리눅스 명령어도 있고, Ruby도 기본으로 설치되어 있어서 gem 같은 명령어도 기본으로 사용할 수 있는 것으로 보인다. 근데 나는 Windows7 이 아닌가? 그래서 "윈도우 jekyll"로 검색해 보았다. 아래 블로그들을 살펴보았다.

[참고 2. 윈도우에서 지킬을 이용한 블로그 생성](https://soojae.github.io/blog/jekyll-install/)

참고 2는 jekyll을 설치하는 부분까지 그대로 따라할 수 있다. 그 이후 내용은 지킬을 사용해서 로컬 컴퓨터에 블로그를 생성하고 웹브라우저에서 접속해 보는 것 까지 예제가 있다.

[참고 3. 윈도우 사용자의 깃헙 페이지 블로그 설치기](http://enshahar.com/%EB%B8%94%EB%A1%9C%EA%B7%B8/jekyll/pixyll/2017/11/14/blog-transferring/)

참고 3은 jekyll을 설치하는 부분까지 동일하고, 그 이후 내용은 아직 어렵고 감이 안잡힌다.

[참고 4. 지킬을 이용한 깃허브 블로그 만들기](https://geeksvoyage.com/raspberry%20pi/jekyll-for-pi/)

참고 4는 리눅스 기반 설명이지만 Jekyll 설치가 완료되면 그 이후 내용은 매우 도움이 될 것 같다.

### 윈도우에 Ruby 설치

아래 링크로 들어가서 `"Ruby+Devkit 2.5.3-1 (x64)"`를 다운로드 받았다.

[RubyInstaller for Windows](https://rubyinstaller.org/downloads/)

더 최신버전인 2.6.1도 있었는데 우측 글에 2.5.X를 추천하니까 2.5.3을 받았다.

![](/assets/images/2019-03-17/04_install_ruby_1.bmp)

다운로드 받은 "rubyinstaller-devkit-2.5.3-1-x64.exe"를 더블 클릭하면 아래와 같은 설치화면이 뜬다. `Install`을 클릭한다.

![](/assets/images/2019-03-17/04_install_ruby_2.bmp)

그러면 아래 그림처럼 "MSYS2 development toolchain"이 체크되어 있는데 781MB 정도로 꾀 큰 파일이다. [MSYS](http://www.mingw.org/wiki/MSYS)가 무엇인지 검색해보니  

*"MSYS is a collection of GNU utilities such as bash, make, gawk and grep to allow building of applications and programs which depend on traditionally UNIX tools to be present."*  

리눅스에서 쓰던 여러 GNU 유틸리티 모음을 윈도우에서도 쓸 수 있도록 하는 건가 보다. 대부분 설치하는 것 같아서 설치했다.

![](/assets/images/2019-03-17/04_install_ruby_3.bmp)

아래와 같이 설치가 완료되는데 까지 약 3분정도가 걸린 것 같다. 그런데 아직 끝난게 아니라 MSYS2 setup이 남아있다. 아래와 같이 체크가 되어 있는 상태에서 Finish를 클릭했다.

![](/assets/images/2019-03-17/04_install_ruby_4.bmp)

그러면 아래와 같은 화면이 뜨는데 Enter를 누르면 1,2,3을 모두 설치하기 시작한다.

![](/assets/images/2019-03-17/04_install_ruby_5.bmp)

아래 화면처럼 설치과정이 진행되고 완료되기 까지 약 7분정도 걸린 것 같다. 

![](/assets/images/2019-03-17/04_install_ruby_6.bmp)

아래 화면은 설치가 완료된 것이고 다시 Enter를 누르면 cmd 화면이 닫히면서 설치가 끝난 것이다.

![](/assets/images/2019-03-17/04_install_ruby_7.bmp)

마지막으로 설치된 경로를 확인해 보니 "C:\Ruby25-x64" 에 아래와 같은 여러 파일들이 설치되어 있었다.

![](/assets/images/2019-03-17/04_install_ruby_8.bmp)

아래와 같이 윈도우 시작메뉴에도 설치된 것들이 등록되어 있었다.

![](/assets/images/2019-03-17/04_install_ruby_9.bmp)

"Start Command Prompt with Ruby"를 실행한 후, 아래 그림처럼 "where ruby"를 쳐보니 "ruby.exe"의 경로가 리턴되어 PATH가 잘 인식되어 있음을 알 수 있다. 

![](/assets/images/2019-03-17/04_install_ruby_10.bmp)

이것은 Ruby 설치 제일 처음에 "Add Ruby excutables to your PATH"가 체크되어 있어서 사용자 변수 Path에 "C:\Ruby25-x64\bin"가 기록되어 있기 때문이다. 확인해 보려면 바탕화면의 컴퓨터 아이콘 우클릭 -> 고급 탭의 환경 변수 -> 사용자 변수의 Path를 확인해 볼 것. 아무리 사소한 거라도 자꾸 확인하고 넘어가자.

### Jekyll 설치

Ruby를 설치했으니 이번엔 Jekyll을 설치할 차례다. Jekyll을 설치하려면 cmd 창에서 아래 명령어를 입력하라고 되어 있다.

```bash
$ gem install jekyll bundler
```

gem은 Ruby의 패키지 매니저라고 한다. "C:\Ruby25-x64\bin" 에 있기 때문에 cmd에서 곧바로 입력할 수 있다. install은 인수인것 같고 jekyll과 bundler는 필요한 패키지 인가보다. 일단 하라는 대로 진행하였다. 

![](/assets/images/2019-03-17/05_install_jekyll_1.bmp)

명령어를 입력하고 Enter를 누르면 아래와 같은 메세지가 나오면서 약 2분간 설치가 진행되었다. 

![](/assets/images/2019-03-17/05_install_jekyll_2.bmp)

설치가 완료되면 "C:\Ruby25-x64\bin"에 `"jekyll"`과 `"bundler"` 명령어가 생성됨을 확인할 수 있었다.

## 3. Jekyll을 이용해서 처음 블로그 만들어 보기

지금부터는 [참고 2](https://soojae.github.io/blog/jekyll-install/)와 [참고 4](https://geeksvoyage.com/raspberry%20pi/jekyll-for-pi/)의 Jekyll을 설치하고 난 후, 처음 블로그 생성을 해보는 것을 따라한 것이다.

먼저 아래와 같이 "cmd" 창에 `"jekyll new blog"`를 입력하라고 되어 있다. 이렇게 하면 현재경로인 "C:\Users\사용자명"에 blog 라는 이름으로 폴더를 만들고 거기에 블로그 생성에 필요한 파일들을 만드는 것 같다. 아래 그림과 같이 진행하였다.

![](/assets/images/2019-03-17/06_gen_first_blog.bmp)

위 그림처럼 cmd창에 `jekyll new blog`를 입력하면 여러 메세지가 출력되고, 약 1분 후에 "New jekyll site installed in C:/Users/사용자명/blog"를 출력한 후 끝이 난다.  

"C:/Users/사용자명/blog" 경로로 가보면 아래 그림과 같이 여러 파일들이 생성되어 있다.

![](/assets/images/2019-03-17/06_gen_first_blog_2.bmp)

위 파일들이 블로그를 구성하는 파일일건데 각각이 무슨 기능을 하는지 아직 잘 모른다. 차차 알아나가기로 하고 계속 따라해 본다.  

이번에는 지킬 서버를 시작하기 위해, **blog 폴더로 이동 후 `jekyll serve`를 입력**하라고 한다. 아래와 같이 진행하였다.

![](/assets/images/2019-03-17/06_gen_first_blog_3.bmp)

서버를 시작하니까, 아래 그림과 같이 blog 폴더에 없던 파일도 생겨났다.

![](/assets/images/2019-03-17/06_gen_first_blog_4.bmp)

이제 Server address인 127.0.0.1:4000 (또는 localhost:4000)를 크롬의 url 주소창에 입력하니까 아래와 같은 화면이 뜬다. 여기까지 일단 참고 블로그를 성공적으로 따라하긴 했다.

![](/assets/images/2019-03-17/06_gen_first_blog_5.bmp)

**여기까지 한것은 로컬컴퓨터에 뭔가 블로그의 뼈대가 되는 파일들을 Jekyll로 생성해 본 것 같고, 로컬서버를 띄운 다음 웹브라우저로 접속 테스트를 해 본 것 같다. 아직은 잘 모르겠으나 조금씩 감이 잡히고 있다.**  

cmd에서 `Jekyll --help`를 쳐보면 Jekyll의 `new`와 `serve`가 어떤 의미인지 나온다.

![](/assets/images/2019-03-17/07_jekyll_help.bmp)

## 4. 계속 조사 진행

여러 블로그를 살펴봤는데 나랑 제일 잘 맞는 블로그 자료는 [참고 4. Geek's Voyage](https://geeksvoyage.com/raspberry%20pi/jekyll-template/) 였다. 이 자료를 참고하여 그대로 따라해 보려고 했다. 그런데 따라 하려니까 `fork`, `master branch`, `pull`, `push` 같은 git 관련 용어가 계속 나오고, `git init` 같은 git 관련 명령어를 실행할 수 있는 환경이 만들어져야 따라할 수 있다. (2019년 3월 18일 까지의 내용)

오늘은 2019년 4월 12일이다. 약 한달전부터 Github 블로그 만들기를 알아보다가, 3월 19일쯤 부터 git에 대해 공부하기 시작했다. 이제는 Github 블로그 만들기 설명글을 읽을 때 git에 대한 명령어와 용어 등이 나오면 거부감이 생기지 않으니까 다시 Github 블로그 만들기 조사에 들어간다.

### minimal-mistakes 적용 시도

먼저 지킬테마 중 minimal-mistakes-4.16.0.zip 을 다운로드 하였다. 다운받은 경로는 `D:\[Projects]\proj17_git_and_github/` 이다. 압축을 풀고 "minimal-mistakes-4.16.0/" 폴더 안에서 `Git Bash`를 실행하여 참고자료에 있는 것 처럼 `bundle` 명령을 실행하였다.

```bash
$ bundle
[!] There was an error parsing `Gemfile`: There are no gemspecs at D:/[Projects]/proj17_git_and_github/minimal-mistakes-4.16.0. Bundler cannot continue.

 #  from D:/[Projects]/proj17_git_and_github/minimal-mistakes-4.16.0/Gemfile:2
 #  -------------------------------------------
 #  source "https://rubygems.org"
 gemspec #  source "https://rubygems.org"
 
 #  -------------------------------------------
```
뭔가 안된 것 같다. [참고 4. Geek's Voyage](https://geeksvoyage.com/raspberry%20pi/jekyll-template/) 의 설명과는 다른 결과가 나왔다. 혹시 경로 문제인가 싶어서, 경로를 "C:\Users\사용자명"로 압축파일들을 이동시키고 다시 `bundle` 명령어를 `Git Bash`로 실행해 보았다.

```bash
$ bundle
fatal: not a git repository (or any of the parent directories): .git
Fetching gem metadata from https://rubygems.org/..........
Fetching gem metadata from https://rubygems.org/.
Resolving dependencies...
(...생략...)
Using minimal-mistakes-jekyll 4.16.0 from source at `.`
Bundle complete! 3 Gemfile dependencies, 47 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
```

위와 같이 뭔가 출력되면서 잘못되지는 않은 것 같다. 그 다음 `bundle exec jekyll serve` 명령을 실행해 봤다.

```bash
$ bundle exec jekyll serve
fatal: not a git repository (or any of the parent directories): .git
Configuration file: C:/Users/ahs/minimal-mistakes-4.16.0/_config.yml
            Source: C:/Users/ahs/minimal-mistakes-4.16.0
       Destination: C:/Users/ahs/minimal-mistakes-4.16.0/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
  Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/main.scss':
                    Invalid CP949 character "\xE2" on line 54
jekyll 3.8.5 | Error:  Invalid CP949 character "\xE2" on line 54
```

그랬더니 CP949 어쩌구 에러가 발생했다. 찾아보니 윈도우에서 Jekyll을 쓸 때 발생하는 인코딩 에러라고 한다.[(Jekyll - Github page 오류 해결)](https://aisiunme.github.io/jekyll/2018/07/25/troubleshooting-in-jekyll-serve/) 해결하려면 콘솔에 `chcp 65001` 을 입력하라고 한다. 전에 Jekyll을 설치 할 때 [Jekyll 홈페이지](http://jekyllrb-ko.github.io/docs/windows/)에서 이 내용을 보기는 했으나 당시에는 무슨 소린지 몰라서 그냥 넘어갔었다. 어쨌든 `Git bash`에서 아래와 같이 명령을 입력했더니...

```bash
$ chcp 65001
bash: chcp: command not found
```

명령어를 못 찾는다. `Git bash`에는 Path 등록이 안되어 있나보다. 루비를 설치하면 `Start Command Prompt with Ruby`가 있으니가 거기서 `chcp 65001`를 해보았다. 그러면 "Active code page: 65001" 이라고 하면서 콘솔의 폰트가 바뀐다.

다시 minimal-mistakes-4.16.0/ 폴더로 가서 `bundle exec jekyll serve` 명령을 실행한다.

![](/assets/images/2019-03-17/08_bundle_exec_jekyll_serve.bmp)

위와 같이 서버가 시작된 것 같다. 그리고 크롬에서 "localhost:4000" 으로 접속하니까 아래와 같이 창이 뜬다.

![](/assets/images/2019-03-17/09_localhost4000.bmp)

### Github에 블로그 저장소 만들기, 그리고 첫 커밋

Github에 username.github.io 형식의 저장소를 만들었다.

![](/assets/images/2019-03-17/10_gen_github_repo.bmp)

그런 후, 참고 4에서 설명해 준대로 진행한다. 먼저 다운받은 테마 폴더의 이름을 Github 저장소 이름으로 변경하고(폴더명은 자기마음대로) 폴더에 들어간 다음, 저장소 초기화를 한다. 그리고 첫번째 커밋을 한다.

```bash
$ mv minimal-mistakes-4.16.0/ ranfort77.github.io
$ cd ranfort77.github.io/
$ git init
Initialized empty Git repository in C:/Users/ahs/ranfort77.github.io/.git/

$ git add .
(warning: LF will be replaced by CRLF: 각 파일마다 많이 뜬다.)

$ git commit -m "first commit"
[master (root-commit) 3091122] first commit
 719 files changed, 41741 insertions(+)
( create mode 100644: 각 파일마다 많이 뜬다.)
```

첫번째 커밋이 완료되면 원격저장소 주소를 등록해 준다.

```bash
$ git remote add origin https://github.com/ranfort77/ranfort77.github.io.git
$ git remote -v
origin  https://github.com/ranfort77/ranfort77.github.io.git (fetch)
origin  https://github.com/ranfort77/ranfort77.github.io.git (push)
```

`git push -u origin master` 명령으로 원격저장소로 push 한다.

```bash
$ git push -u origin master
Enumerating objects: 624, done.
Counting objects: 100% (624/624), done.
Delta compression using up to 8 threads
Compressing objects: 100% (609/609), done.
Writing objects: 100% (624/624), 15.12 MiB | 1.76 MiB/s, done.
Total 624 (delta 56), reused 0 (delta 0)
remote: Resolving deltas: 100% (56/56), done.
To https://github.com/ranfort77/ranfort77.github.io.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

이제 현재 로컬저장소가 원격저장소로 전부 업로드 됐다.

### 첫 블로그 글쓰기

참고 4에서와 같이 `\_posts` 폴더에 `YYYY-MM-DD-title.md` 형식으로 글을 작성한다. 처음에는 `_posts` 폴더가 없으니 만들자.

```bash
$ mkdir _posts
$ cd _posts/
$ cat 2019-04-12-start-blog.md
---
title: "블로그 시작하기"

---

github.io를 사용하여 블로그를 시작!!
```

커밋하고, 원격저장소에 업로드 한다.

```bash
$ git add .
warning: LF will be replaced by CRLF in _posts/2019-04-12-start-blog.md.
The file will have its original line endings in your working directory

$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   _posts/2019-04-12-start-blog.md

$ git commit -m "first post"
[master 136cf53] first post
 1 file changed, 6 insertions(+)
 create mode 100644 _posts/2019-04-12-start-blog.md

$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 386 bytes | 386.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/ranfort77/ranfort77.github.io.git
   3091122..136cf53  master -> master
```

일단 포스트에 한글이 있으니까 뭔가 문제가 발생하는 것 같다. 그래서 영어로 바뀐 후 push 한 다음에 확인해 봤는데 Github에는 블로그 포스트가 빨리 적용이 안되는 것 같다. Github 내부적으로 마크다운을 html로 바꾼다고 한다. 그것때문인지 약간 늦게 적용되는데 첫번째 포스트는 한 5분이상 기다리니까 적용이 되었다.

오늘은 여기까지 한다.

