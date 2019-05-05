---
title: "git 공부 정리 (1): 설치 및 초기설정"
excerpt: "버전관리 프로그램 git에 대해 공부한 기록 첫 번째"
categories:
  - tool
tags:
  - git
  - github
  - jekyll
last_modified_at: 2019-03-19T00:30
---

본 문서는 버전관리 프로그램 git에 대해 공부한 기록 (1)이다. git 설치 및 초기설정에 관련한 내용을 공부했다.

## 1. 머리말

Jekyll을 사용한 GitHub 블로그 만들기에 대한 자료를 조사하던 중, 버전 관리 프로그램인 git의 사용방법을 모르면 전혀 진행이 안된다는 사실을 알았다. 왜냐하면 설명 대부분이 Command Line Interface 상에서 git을 사용하는 내용을 담고있기 때문이다. 나는 완전히 초보인 입장에서 git에 대한 용어(index, commit, clone, pull, push 등)나 시스템도 잘 모르고 GitHub도 잘 모르니까 너무 힘들고 불편했다. 이런점에서 git에 대해 확실히 공부하고 넘어가기로 했다.

여러 참고 자료를 살펴 보았으나 나에게 가장 잘 맞는 자료는 생활코딩 사이트 egoing님의 `"지옥에서 온 Git"` 강의였다. 예제를 통해 중요 git 명령어를 설명해 줄 뿐 아니라 원리까지 정말 차근차근 좋은 목소리로 동영상 강의를 해주신다.

* [지옥에서 온 Git - 생활코딩](https://opentutorials.org/course/2708)

egoing님의 강의를 듣기 전에는 아래 블로그 내용들이 잘 이해가 안되고 어려웠으나, 강의를 다 듣고 난 후 매우 쉽게 읽여진다.

* [git 간편안내서](https://rogerdudler.github.io/git-guide/index.ko.html)

* [Github를 이용하는 전체 흐름 이해하기](https://blog.outsider.ne.kr/865)

* [초보자를 위한 깃 사용법](http://blog.hyeyoonjung.com/2017/06/01/how-to-use-git/)

* [A Visual Git Reference](http://marklodato.github.io/visual-git-guide/index-ko.html)

git에 대한 기본 명령어의 동작을 이해하고 사용법을 익히고 난 후, 실무적인 단계에서 가장 궁금한 것은 다음 두 가지다.

* 어떤 상황에서 commit을 하는가?
* 어떤 상황에서 branch를 하는가?

여러 자료들을 쭉 보고 몇가지 힌트를 얻었고 그 힌트를 바탕으로 내가 직접 해보면서 나만의 방식을 찾아야 하는 것 같다. 그렇게 하기 위해 앞으로 다음과 같은 것들을 해보기로 했다.

1. 우선 익힌 명령어들을 더 확실히 습득하기 위해, 나만의 방식으로 시뮬레이션 해 보자.
2. 다음 상황을 고려하여 개발 진행을 시뮬레이션 해 보자.
   * 로컬 저장소만 사용한 혼자 개발
   * 로컬 저장소 및 원격 저장소를 사용한 혼자 개발
   * 로컬 저장소 및 원격 저장소를 사용한 공동 개발
3. Github에서 유명 오픈소스 프로젝트의 git history를 살펴보고 commit 메세지와 branch 메세지를 살펴 보자.
4. git에 관한 서적을 정석으로 공부하자. 우선 봐야 할 책은 [Pro Git](https://git-scm.com/book/ko/v2) 이다.
5. egoing님의 강의 마지막에 소개해 준 gitflow를 잘 살펴보자.

커밋 메세지 규약에 대한 글도 읽으면 좋을 것 같다.
* [좋은 git 커밋 메세지를 작성하기 위한 7가지 약속](https://meetup.toast.com/posts/106)
* [Git 커밋 메시지 작성법](https://item4.github.io/2016-11-01/How-to-Write-a-Git-Commit-Message/)
* [Guide to better Git - 좋은 커밋 메시지 작성하기](https://blog.jonnung.com/git/2018/01/02/guide-to-better-git-commit-message/)
* [GIT: Some Commit best practices and How to undo your recent Commits](https://koukia.ca/git-some-commit-best-practices-and-how-to-undo-your-recent-commits-d13c9dc3144f)

이것도 발견한 사이트인데 나중에 읽어보자.
* [누구나 쉽게 이해할 수 있는 Git 입문](https://backlog.com/git-tutorial/kr/)

git에서 working directory, staging area, repository를 부르는 또다른 용어에 대해 정리

working directory|staging area|repository
-|-|-
working tree|index|tree
working copy|cache|history

저 세 영역의 의미를 명확히 이해하려면 git Pro 책의 아래 링크를 먼저 읽어봐야 겠다.

[Git 도구 - Reset 명확히 알고 가기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0)

HEAD, index, working directory를 완전히 이해하지 못하면 버전 되돌리기를 확실히 이해하지 못할 것이다. 신중하게 읽고 넘어가자.

## 2. Windows git 설치

https://git-scm.com/ 에서 `Git-2.21.0-64-bit.exe` 다운로드 받고 설치를 시작하였다. 설치과정은 아래와 같다. 

![](/assets/images/2019-03-19/01_install_git_1.bmp)

![](/assets/images/2019-03-19/01_install_git_2.bmp)

![](/assets/images/2019-03-19/01_install_git_3.bmp)

![](/assets/images/2019-03-19/01_install_git_4.bmp)

![](/assets/images/2019-03-19/01_install_git_5.bmp)

![](/assets/images/2019-03-19/01_install_git_6.bmp)

![](/assets/images/2019-03-19/01_install_git_7.bmp)

![](/assets/images/2019-03-19/01_install_git_8.bmp)

![](/assets/images/2019-03-19/01_install_git_9.bmp)

![](/assets/images/2019-03-19/01_install_git_10.bmp)

설치가 완료되면 설치 경로에 아래와 같이 파일이 생성되어 있다.

![](/assets/images/2019-03-19/01_install_git_11.bmp)

시작 메뉴에도 아래와 같이 등록이 되어 있다.

![](/assets/images/2019-03-19/01_install_git_12.bmp)

윈도우 환경 변수의 시스템 변수를 보면 `Adjusting your PATH environments` 에서 두번째 옵션을 선택해서 인지 `C:\Program Files\Git\cmd` 경로가 추가되어 있다. 따라서 윈도우 cmd 창에서 git 명령어를 쓸 수 있다. 그런데 보통 대부분의 사람들은 시작메뉴에 등록되어 있는 `Git Bash`를 사용하는 것 같다. 이것을 사용하면 PATH에 등록되어 있지 않은 명령어들도 사용할 수 있는 것 같다. 예를 들어 `C:\Program Files\Git\usr\bin\ssh-keygen.exe` 같은 것들도 사용할 수 있다.

![](/assets/images/2019-03-19/01_install_git_13.bmp)

`Git Bash`는 대부분의 리눅스 명령어를 쓸 수 있게 해주는 것 같고, 아래와 같이 PATH 환경변수를 확인해 보니 그 안에 있는 명령어를 다 쓸 수 있는 것 같다.

```bash
$ echo $PATH | sed "s/:/\n/g"
(...생략...)
/mingw64/bin
/usr/local/bin
/usr/bin
/bin
/mingw64/bin
/usr/bin
(...생략...)
```

윈도우 탐색기에서 우클릭하면 나오는 메뉴에 아래와 같이 `Git GUI Here`, `Git BASH Here`가 추가되어 있는데, 설치 단계의 Select Components에서 Windows Explorer integration이 체크되어 있어서 인 것 같다.

![](/assets/images/2019-03-19/01_install_git_14.bmp)

## 3. Git Help 옵션

우선 `git bash` 를 실행한 후, `git help`를 통해 git에 어떤 옵션이 있는지 확인해 보았다.

```bash
$ git help
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add        Add file contents to the index
   mv         Move or rename a file, a directory, or a symlink
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect     Use binary search to find the commit that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
   status     Show the working tree status

grow, mark and tweak your common history
   branch     List, create, or delete branches
   checkout   Switch branches or restore working tree files
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   merge      Join two or more development histories together
   rebase     Reapply commits on top of another base tip
   tag        Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch      Download objects and refs from another repository
   pull       Fetch from and integrate with another repository or a local branch
   push       Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
```

맨 아래 설명에 나와 있듯이 git의 모든 명령어는 `git help -a`를 입력하면 된다. 

## 4. Git: User name & email 설정

git을 사용하여 버전관리를 해 나갈 때 각각의 commit을 누가 했는지 user.name 과 user.email 로 구분하는 것 같다. 나는 등록하기 전에 어떤 정보가 있는지 확인해 보기 위해 `git config --list` 를 먼저 입력해 보았다. 그랬더니 아래와 같이 이미 user.name 과 user.email 이 등록되어 있었다.

```bash
$ git config --list
core.symlinks=false
core.autocrlf=true
core.fscache=true
color.diff=auto
color.status=auto
color.branch=auto
color.interactive=true
help.format=html
rebase.autosquash=true
http.sslbackend=openssl
http.sslcainfo=C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
credential.helper=manager
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
user.name=ranfort77
user.email=ranfort77@gmail.com
```

아마 windows git을 설치하기 전에 `GitHub Desktop`을 설치하고 GitHub에 로그인을 한 적이 있었는데 그것때문에 이미 config 파일에 name과 email이 등록되어 있었나보다. 따라서 나는 `"git config --global user.name 사용자명"`, `"git config --global user.email 이메일"` 명령어로 등록을 안해도 되는 것 같아서 그냥 이대로 나뒀다. git config 파일은 `"C:\Users\사용자명\\.gitconfig"` 인 것 같다. `.gitconfig` 파일 열어보니까 name과 email 정보가 적혀 있다.
