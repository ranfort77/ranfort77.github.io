---
title: "Code Snippet Manager: Github Gist와 Lepton"
excerpt: "Github Gist와 Lepton 사용법 정리"
categories:
  - tool
tags:
  - lepton
  - gist
  - github
last_modified_at: 2020-01-26T01:48:00-05:00
---




## 머리말

과거에 작성해 둔 작은 단위의 기능 구현 코드 또는 공부할 때 기록해 둔 코드를 찾지 못해 다시 작성하는 경우가 많았다. 이러한 문제를 해결하기 위해 코드 조각 (Code snippet) 관리 툴을 찾아보았다. 

[5 Code Snippet Managers That Will Change The Way You Write Code](https://dev.to/tomlangdon/5-code-snippet-managers-that-will-change-the-way-you-write-code-10ml) 에서 소개 해 준 5가지 중 무료이면서 Windows 운영체제에서 사용할 수 있는 후보들은 아래와 같다. 

* [3Cols](https://3cols.com/)
* [Snipit](https://snipit.io/)
* [GitHub Gist](https://gist.github.com/)

그리고 [What is the best code-snippets manager](https://www.slant.co/topics/7247/~code-snippets-manager) 에서 찾은 또 다른 후보들은 아래와 같다. 

* [Lepton](https://hackjutsu.com/Lepton/)
* [Boostnote](https://boostnote.io/)
* GitLab Snippets
* [Gisto](https://www.gistoapp.com/)



여러 문서들을 살펴보니 Code snippet manager를 선택하는 기준은 아래와 같은 것들이 있었다.

* 편집기의 기능이 좋은가? 언어에 따라 적절한 구문 강조 (Syntax highlighting) 기능이 있는가?
* 코드 조각의 분류와 찾기 기능 (Favorites, Tags 또는 Cartegories) 이 잘 되어 있는가? 그리고 빠르게 동작하는가?
* 인터페이스가 이쁜가?
* 여러 사람이 코드 조각을 공유 할 수 있는가?
* 자기가 쓰는 통합 개발 환경 (IDE) 과 연동 가능한가?
* 무료인가?
* 다중 Platform에서 사용 가능 한가?
* 백업 기능이 잘 되어 있는가? 또는 클라우드 백업이 가능한가? 여러 기기 사이에 동기화가 가능한가?
* 버전 관리 (Version control) 기능이 있는가?



위와 같은 기준을 유념하면서 첫 번째로 살펴볼 Code snippet manager는 Lepton이다. 그런데 Lepton은 GitHub Gist 기반이라고 한다. 그래서 먼저 GitHub Gist가 무엇인지 그리고 어떤 기능이 있는지 살펴보았다.



## GitHub Gist

Github gist는 간단한 코드 조각이나 메모 등을 남길 수 있다. gist를 작성하려면 Github에 로그인 한 후 우측 상단에 `+`를 클릭하고 `New gist`를 선택하면 된다.

![](/assets/images/2020-01-25-codesnipman/gist-01-new-gist.png)

아래와 같이 gist 작성 화면이 뜨면 설명, 확장자 포함 파일명, 그리고 코드나 메모를 작성하면 된다. 파일 확장자를 통해 자동으로 언어를 파악하여 구문 강조 (syntax highlight) 를 하는 것 같다. 

![](/assets/images/2020-01-25-codesnipman/gist-02-write-gist.png)

좌측 하단의 Add file 버튼을 통해 파일을 추가 할 수 있다. 최종적으로 우측 하단에 Create secret gist 또는 Create public gist 버튼을 선택해야 gist가 만들어 진다. secret gist는 설명에 나와 있듯이 검색 엔진에 노출되지 않을 뿐 URL을 통해 누구나 사용할 수 있다고 나온다. gist URL은 gist가 만들어 지면 볼 수 있고 블로그 포스트나 카페 글에 gist 내용을 삽입 (Embed) 할 때 주로 사용하는 것 같다. public gist는 검색 엔진에서 검색된다. 검색창 우측에 All gists를 클릭하면 Discover gists 로 들어가게 되는데 여기는 자신을 포함한 모든 사용자가 등록한 public gists를 볼 수 있다.

Create public gist를 클릭하여 아래 그림처럼 첫 번째 gist를 만들었다. 우측 Embed 버튼 옆 URL이 gist를 삽입할 수 있는 HTML 코드다. Embed 버튼을 클릭하면 다른 방식의 공유 URL도 볼 수 있다.

![](/assets/images/2020-01-25-codesnipman/gist-03-create-gist.png)

Edit 버튼을 누르면 gist를 수정할 수 있다. gist를 수정한 후 Update gist를 하면 아래 그림과 같이 Revisions 탭에서 현재까지의 수정 사항들을 볼 수 있다. **즉, gist 역시 버전관리가 된다.**

![](/assets/images/2020-01-25-codesnipman/gist-04-edit-gist.png)

Github gist는 markdown도 지원하기 때문에 메모를 markdown으로 작성할 수 있다.

![](/assets/images/2020-01-25-codesnipman/gist-05-markdown.png)

여기에 구체적으로 기록하지는 않았지만 여러 gist를 만들어 보니 gists 간에 분류 또는 검색을 위한 태그나 카테고리 기능은 보이지 않았고 gist들을 탐색하는 속도도 그렇게 만족스럽지는 않았다.

여기까지 GitHub gist에 대해서 간단히 살펴보았다. [5 Code Snippet Managers That Will Change The Way You Write Code](https://dev.to/tomlangdon/5-code-snippet-managers-that-will-change-the-way-you-write-code-10ml) 에도 장단점이 언급되어 있지만, 내가 느낀 GitHub gist의 장단점은 아래와 같다.

* 장점: 무료, 다중 플랫폼 지원 및 접근, 백업, 버전관리, 링크를 통한 간단 공유
* 단점: 편집 환경, 태그 및 카테고리를 통한 분류 및 찾기, 속도

다음부터는 Lepton이 Github gist의 단점을 어떻게 보완하는지에 초점을 맞추어 살펴 볼 것이다.



## Lepton 시작

Lepton의 아쉬운 부분은 참고할 만한 적당한 문서가 없다는 것이다. 설치는 매우 쉽지만 참고 문서는 [FAQ](https://github.com/hackjutsu/Lepton/wiki/FAQ) 또는 [Lepton README](https://github.com/hackjutsu/Lepton)를 살펴보았다.

[Lepton Download Page](https://github.com/hackjutsu/Lepton/releases) 에서 윈도우 버전 Lepton-Setup-1.8.2.exe를 다운받아 설치한다. Lepton을 실행하면 Github Login 화면이 뜨고 계정과 비밀번호를 입력한다. Lepton이 Github Gists에 읽기와 쓰기 접근 승인해 주는 창이 뜨고 Authorize hackjutsu 를 클릭한다.

그러면 아래 그림처럼 곧바로 Github Gists를 동기화하여 가져온다.

![](/assets/images/2020-01-25-codesnipman/lepton-01-start.png)

인터페이스의 각 부분의 의미는 한눈에 봐도 알 수 있다. 

1. Menu: 각 메뉴를 눌러보면 알겠지만 가장 중요한 메뉴는 `Gist`이다. 
2. Gist title: Formatted Description을 쓰지 않으면 Github에서 작성했던 description이 gist title로 사용되어 나열되는 것 같다. Formatted Description을 사용하면 각 gist에 대한 title과 tag를 작성할 수 있다.
3. LANGUAGES:  gists를 언어별로 구분하여 나열해 준다. 언어별 카테고리라고 보면 된다. gist 파일의 확장자를 참고하여 자동으로 분류해 주는 것 같다. 그림에는 없지만 LANGUAGES 밑에 PINNED, TAGS 도 있다.
4. 선택한 gist의 내용을 보여준다.
5. 선택한 gist를 편집, web에서 열기, Revision 보기, 삭제를 할 수 있다.



## Formatted Description

formatted description은 `[title] description #tag1 #tag2 #...` 방식으로 작성한다.

연습으로 작성해 본 3개의 gists description을 formatted description으로 수정해 보았다. (의미있는 tag를 달아야 하지만 여기서는 연습삼아 대충 달아본다.)

![](/assets/images/2020-01-25-codesnipman/lepton-02-edit-gist-1.png)

그러면 아래 그림과 같이 각각의 gist 정보가 더 구체적으로 기술되고 tag로 분류되어 TAGS에서 분류할 수 있다.

![](/assets/images/2020-01-25-codesnipman/lepton-02-edit-gist-2.png)



## Sync

Lepton의 메뉴 표시줄에 설정 (Preferences) 메뉴는 보이지 않는다. 전단락에서 formatted description을 설명하면서 lepton 안에서 description을 수정하였다. 수정한 후 Github gist를 확인해 보니 이미 변경 내용이 동기화 되어있었다. 즉 lepton에서 gist 내용을 변경하면 github에 바로바로 동기화가 이루어 지는 것 같다. (이때 당연히 gist revisions도 바뀐다)

반대로 github gist에서 새로운 gist를 작성하면 lepton은 곧바로 가져오지 않는다. (주기적으로 동기화 설정이 되어 있지 않는 한 lepton이 github gist가 변경되었는지 모르기 때문에 당연한 것 같다) lepton에서 Sync 버튼을 누르면 변경된 github gist를 가져온다. Lepton 을 재시작해도 동기화가 된다.



## Immersive mode

단축키 `Ctrl + i`는 immersive mode인데 선택한 gist가 Lepton 화면 전체로 넓어진다. 확실하진 않지만 선택한 gist를 살펴보기 더 쾌적하라고 있는 기능인 것 같다.



## starred

Sync 버튼 아래에 있는 `#starred` 버튼을 누르면 Star을 달아 둔 다른 사람의 gists가 모인 Github 페이지로 이동한다. GitHub Gist 검색이나 Discover gists에서 기억해 둘 만한 다른 사람의 gist에 Star를 달아두면 유용할 것이다.



## PINNED

LANGUAGES 아래에 있는 PINNED의 옆에 `+`를 누르면 아래와 같이 현재까지 사용한 모든 태그 목록이 뜬다.

![](/assets/images/2020-01-25-codesnipman/lepton-03-pinned.png)

나중에 Lepton에 있는 gists가 아주 많아지면 LANGUAGES와 TAGS가 매우 많아지기 때문에 자주 사용하는 태그에 핀을 꼽아 둔다는 의미로 사용하는 것 같다. 



## Search

단축키 `Shift + Space`를 누르면 검색창이 뜬다.  description, tags, file names 등을 검색할 수 있다. 역시 gists가 많아지면 원하는 gist를 찾기에 이용할 수 있다.



## Lepton 총평

Lepton은 Github Gist의 단점인 편집 환경, gists 분류 및 찾기, 속도 등을 만족스럽게 보완한다. 무엇보다 Github에 동기화가 매우 쉽고 편하기 때문에 백업 안정성, 접근성이 매우 편리하다. 아직 많은 gists를 적용하여 사용해 본 적이 없기 때문에 조심스럽지만, 이정도 기능만으로도 개인적인 code snippets을 관리하기에 충분할 것 같다.



