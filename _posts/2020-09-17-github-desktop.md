---
title: "GitHub Desktop 사용법"
excerpt: "GitHub Desktop 사용 방법에 대한 정리"
categories:
  - tool
tags:
  - git
  - github
last_modified_at: 2020-09-20T11:04:00-05:00
---




## 머리말

[GitHub Desktop](https://desktop.github.com/) 사용법을 정리한다. 정리 방식은 GitHub Desktop의 GUI 기능을 설명하고, 각각의 기능들이 CLI (Command Line Interface) 에서 어떤 git 명령어에 해당하는지 살펴본다.

설명에 필요한 예제 코드와 서술 흐름은 아래 링크에 작성했던 git 연습에서 사용한 것과 동일하다.

* [git 공부 정리 (3): git 연습, 커밋하기](https://ranfort77.github.io/tool/tool-study-git-3/)
* [git 공부 정리 (4): git 연습, 브랜치 만들기](https://ranfort77.github.io/tool/tool-study-git-4/)
* [git 공부 정리 (5): git 연습, 리베이스, 체리픽 등](https://ranfort77.github.io/tool/tool-study-git-5/)
* [git 공부 정리 (6): 원격저장소 Github 사용해 보기](https://ranfort77.github.io/tool/tool-study-git-6/)




## 1. 설치 및 로그인

GitHub Desktop은 [desktop.github.com](https://desktop.github.com/)에서 Download for Windows (64bit)를 다운로드하고, 설치파일을 실행하면 별다른 과정없이 쉽게 설치할 수 있다. 이 글을 쓰는 시점에서 설치된 버전은 GitHub Desktop 2.5.5이다.  설치가 완료되면 아래 화면이 뜬다.

| ![](\assets\images\github-desktop\github-desktop-f01-01-intro.png) |
| :----------------------------------------------------------: |
|                   그림 1-1. GitHub 로그인                    |

Sign in to GitHub.com을 선택하여 GitHub에 로그인한다. 그러면 아래 그림처럼 Configure Git 창이 뜬다. Name과 Email을 입력하게 되는데, 이것은 commit 메세지에 적용되는 Author 정보이기 때문에 틀리지 않게 잘 지정한다. 아마 여기에 입력한 name과 email은 git의 설정파일인 `C:\Users\username\.gitconfig`에 기록될 것이다. 

| ![](\assets\images\github-desktop\github-desktop-f01-02-config.png) |
| :----------------------------------------------------------: |
|                   그림 1-2. Configure Git                    |

Continue를 하면 Make GitHub Desktop better! 창이 뜨는데, GitHub 개선을 위한 사용 통계를 제공할지 말지 동의한 후 Finish를 하여 설정을 끝낸다. 그러면 아래와 같이 Let's get started! 창이 뜬다. 


| ![](\assets\images\github-desktop\github-desktop-f01-03-getstarted.png) |
| :----------------------------------------------------------: |
|                 그림 1-3. Let's get started!                 |

오른쪽의 Your repositories는 GitHub에 있는 remote repositories를 의미한다. repository를 선택하면 아래에 Clone 버튼이 생기고 그것을 통해 Local repository로 clone을 만들 수 있다. 일단 여기서는 clone을 하지 않는다.

좌측에 `Create a tutorial repository...`는 GitHub Desktop 사용에 대한 간단한 tutorial을 진행할 수 있는데, 이후 섹션에서 이것을 살펴 볼 것이다. `Clone a repository from the Internet...`은 GitHub나 URL을 통해 remote repository에서 Local Clone을 만들 수 있다. 그림 1-3 오른쪽 repository clone과 같은 기능이며, 메뉴 표시줄의 `File > Clone repository...`를 선택한 것과 같다. `Create a New Repository on your hard drive...`는 말 그대로 Local repository를 새로 만드는 것인데, `File > New repository...`를 한 것과 같다. 마지막 `Add an Existing Repository from your hard drive...`는 이미 컴퓨터에 존재하고 있는 Local repository를 GitHub Desktop에 인식시키는 것이며, `File > Local Repository...`를 선택한 것과 같다.

설치 및 로그인, 그리고 Let's get started에 대한 설명은 여기까지다. Windows에서 GitHub Desktop의 설치와 설정 정보는 아래 경로에 저장된다. 깨끗하게 재설치하고 싶다면 GitHub Desktop 삭제 후 아래 폴더도 삭제하고 재설치 하면 된다.

* 설치 경로: `C:\Users\ran\AppData\Local\GitHubDesktop`
* 설정 파일 경로: `C:\Users\ran\AppData\Roaming\GitHub Desktop`



## 2. 설정 

아직 GitHub Desktop에 등록된 Local Repository가 하나도 없기 때문에 GitHub Desktop 상단에 위치한 메뉴 표시줄의 View, Repository, Branch 메뉴는 대부분 비활성화 되어 있다. Local Repository를 만들기 전에 먼저 기본 설정에 관해 설명한다. `File > Options...`를 선택하면 아래 그림과 같이 설정 창이 뜬다. 

| ![](\assets\images\github-desktop\github-desktop-f02-01-options.png) |
| :----------------------------------------------------------: |
|                      그림 2-1. Options                       |

Accounts에는 GitHub의 로그인/로그아웃을 할 수 있다. Integrations에는 External Editor와 Shell을 설정할 수 있는다. Repository 메뉴의 Open in command prompt, Open in Typora를 선택할 때 Integrations에 설정된 Editor와 Shell이 사용된다. 사전에 Typora가 설치되어 있다면 기본으로 설정되며, Shell은 기본으로 Command Prompt로 설정되어 있지만 나의 경우 Git Bash로 바꿨다. Git에서는 Name과 email을 재설정 할 수 있다. Appearance는 GitHub Desktop의 테마를 바꾼다. Advanced에서는 특정 커밋으로 이동하거나 브랜치를 바꿀 때 어떻게 동작할지 그리고 repository 삭제, 커밋 취소, repository 업로드를 할 때 대화상자를 띄울지에 대한 선택을 할 수 있다.



## 3. 인터페이스

Let's get started!에서 `Create a tutorial repository...` 를 통해 GitHub Desktop 사용법을 간단히 익힐 수 있다. `Create a tutorial repository...`를 클릭하고 Continue를 하면 `C:\Users\username\Documents\GitHub\desktop-tutorial`에 local repository가 생성되며, 곧바로 GitHub에 업로드 된다. 그리고 아래와 같은 화면이 나타난다. 

| ![](\assets\images\github-desktop\github-desktop-f03-01-interface.png) |
| :----------------------------------------------------------: |
|                  그림 3-1. 기본 인터페이스                   |

우측에 Get started 아래에 설명하는 대로 tutorial을 그대로 진행하면 된다. 단계 1인 text editor 설치는 이미 typora가 설치되어 있으면 완료된다. 나머지 단계는 branch 생성, 파일 수정, 커밋, GitHub 업로드, pull request 진행에 대한 간단한 실습이다. 

여기서는 GitHub Desktop의 기본 인터페이스에 대해서 살펴 보자. 우선 메뉴 표시줄 아래에 툴바가 있는데, Current repository, Current branch, Fetch origin 이라고 되어 있다. 

Current repository 아래에는 desktop-tutorial이라고 써져 있는데, 이것은 현재 선택된 repository가 desktop-tutorial이라는 의미이다. Current repository 툴 아래에 Changes와 History 버튼이 있다. 이것은 현재 repository의 상태와 로그를 보여주는 창이다. 즉, Changes는 working directory에 파일이 생성/삭제/수정되면 그 정보가 표시되는데 git 명령어로는 `git status`에 해당된다. History는 커밋 로그를 한 눈에 보여준다. 즉, History는 git 명령어로 `git log`와 관련된 것이라고 보면 된다. 현재는 Initial commit 밖에 없는 상태다. 좌측 아래에 보면 `Summary (required)`, `Description`이 있고, `Commit to master` 버튼이 비활성화 되어 있는 것을 볼 수 있다. 여기는 Changes에 변화가 생겼을 때 커밋 메세지를 입력하고 커밋을 하는 기능이다. 즉, git 명령어로 `git commit` 과 관련된 것이라고 보면 된다.

Current repository 툴을 눌러보면 아래 그림 3-2처럼 현재 등록된 local repositories 목록과 선택된 local repository를 볼 수 있다. 아직 desktop-tutorial 밖에 없어서 목록에 하나만 있다. Add 버튼을 누르면 Clone, Create, Add repository 기능이 있음을 알 수 있다. 그리고 repository에서 우클릭을 하면 그림 3-2처럼 context menu가 나온다. `Open in Git Bash`를 선택하면 git bash 터미널이 실행되고 디렉토리가 바로 repository에 위치한다. 이를 통해 미세한 git 명령어를 사용해야 할 때 이용할 수 있다. `Show in Explorer`는 repository 폴더를 윈도우 탐색기로 연다. `Open in Typora`는 Typora 편집기를 열고 repository 문서들을 수정할 수 있다. `Remove...`를 선택하면 repository 삭제 관련 창이 뜨는데, `Also move this repository to Recycle Bin`을 체크하면 GitHub Desktop에서 local repository 정보를 삭제하고, local repository 디렉토리를 모두 휴지통으로 이동시킨다. 반면 체크하지 않으면 GitHub Desktop에서 local repository 정보를 삭제하지만, local repository 디렉토리는 원래 있던 위치에 그대로 둔다. 따라서 `File > Add local repository...`를 통해 다시 GitHub Desktop으로 불러올 수 있다. GitHub Desktop에서 remove repository는 local repository 삭제에만 관여하고 GitHub에 있는 remote repository는 전혀 관여하지 않으니, GitHub에 최신 버전까지 완전히 동기화 되어 있다면 local repository 삭제에 걱정없이 언제든지 Clone을 통해 복구할 수 있다.

| ![](\assets\images\github-desktop\github-desktop-f03-02-interface.png) |
| :----------------------------------------------------------: |
|                 그림 3-2. Current repository                 |

이번에는 Current branch 툴을 살펴보자. Current branch 툴 아래에는 master라고 써져 있는데 현재 선택된 branch가 master branch 임을 의미한다. Current branch 툴을 눌러보면 아래 그림 3-3처럼 현재 desktop-tutorial repository에 어떤 branch가 있는지 목록이 나온다. 아직은 master branch만 있는 상태다. New branch 버튼을 통해 새로운 branch를 생성할 수 있다. 오른쪽에 Pull requests 버튼도 보이는데, (이것은 이 저장소에 pull requests가 들어오면 뭔가가 뜰 것이다.) 맨 아래에는 `Choose a branch to merge into master` 버튼도 보인다. 이것은 특정 branch를 master에 병합할 때 사용한다.

| ![](\assets\images\github-desktop\github-desktop-f03-03-interface.png) |
| :----------------------------------------------------------: |
|                   그림 3-3. Current branch                   |

마지막 Fetch origin 툴은 현재는 Fetch origin이라고 되어 있지만, repository에 변화가 생겨 커밋을 하면 local repository와 GitHub의 remote repository의 history가 달라지게 된다. 그러면 Fetch origin은 Publish branch 로 바뀌게 된다. Publish branch를 누르면 곧바로 GitHub에 repository가 push된다.



## 4. Commit

지금부터 좀 더 구체적인 예제를 통해 GitHub Desktop 사용법을 익혀볼 것이다. 본 섹션의 내용은 [git 공부 정리 (3): git 연습, 커밋하기](https://ranfort77.github.io/tool/tool-study-git-3/)를 GitHub Desktop에 적용한 것이다. Python으로 간단한 함수를 작성하면서 GitHub Desktop을 이용해서 커밋하는 방법과 커밋을 취소하는 방법에 대해 알아보자.



### Create a repository

우선 `testrepo` 라는 이름의 local repository를 만든다. File > New Repository... 를 선택하고 아래 그림처럼 내용을 입력한 후, Create repository를 클릭하면 repository가 생성된다. repository에는 설정한 대로 README.md, .gitignore, LICENSE 파일들도 같이 생성해 본다.

| ![](\assets\images\github-desktop\github-desktop-f04-01-newrepo.png) |
| :----------------------------------------------------------: |
|           그림 4-1. 새로운 local repository 만들기           |

위 동작은 아래 git 명령어를 입력한 것과 같다. 물론 명령어로 입력할 때는 README.md, .gitignore, LICENSE 파일들은 수동으로 생성해 줘야 한다.

```bash
$ cd D:\projects
$ mkdir testrepo
$ cd testrepo
$ git init
```



### First commit: add function

repository에 mymath.py 라는 이름으로 파일을 만들고, add 함수를 구현한다. 그리고 test_mymath.py 라는 이름으로 add 함수에 대한 테스트 코드를 구현한다. 그런 후 테스트 코드를 실행하여 테스트 통과를 확인한다. (나는 Python IDE로 anaconda spyder를 사용했다. 자기가 편한 것을 사용하면 된다.)

```python
# filename: mymath.py
def add(a, b):
    return a + b
```

```python
# filename: test_mymath.py
import unittest
import mymath

class TestFoo(unittest.TestCase):

    def test_add(self):
        self.assertEqual(mymath.add(1, 1), 2)


if __name__ == '__main__':
    unittest.main()
```

이제 GitHub Desktop의 Changes를 보면 새로 추가된 파일 (우측 녹색 `+` 아이콘이 New를 나타냄) 이 있음을 확인할 수 있다. `__pycache__/` 폴더가 Changes에 표시되지 않는 이유는 이미 .gitignore 파일에 정의되어 있기 때문이다. 

| ![](\assets\images\github-desktop\github-desktop-f04-02-changes.png) |
| :----------------------------------------------------------: |
|                      그림 4-2. Changes                       |

Changes 화면은 명령어 `git status`를 입력한 것과 같다. 뿐만 아니라 Changes에서 파일을 각각 선택 할 수 있다는 점에서 `git add`, `git rm --cached ` 등의 명령어를 복합적으로 사용한 것과 같은 효과이다. 즉, 모두 선택한 것은 `git add .`에 해당하며, test_mymath.py를 선택 해제하면 `git rm --cached test_mymath.py`를 한 것과 같다.

이제 첫 번째 커밋을 할 것이다. 커밋 도구는 GitHub Desktop 좌측 아래에 있다. 아래 그림처럼 입력하고 Commit to master를 클릭하여 커밋을 완료한다.

| ![](\assets\images\github-desktop\github-desktop-f04-03-commit.png) |
| :----------------------------------------------------------: |
|                   그림 4-3. 첫 번째 Commit                   |

커밋을 하면 Changes는 없어지고, 옆에 History에 변화가 생긴다. 변화는 아래 그림과 같다.

| ![](\assets\images\github-desktop\github-desktop-f04-04-history.png) |
| :----------------------------------------------------------: |
|                      그림 4-4. History                       |

History는 `git log`를 입력한 것과 같다. 커밋 이력을 볼 수 있으며 각 커밋의 내용과 커밋 ID 등을 확인할 수 있다. 



### Second commit: sub function

이번에는 mymath.py에 sub 함수를 추가하고, test_mymath.py에 test_sub 메소드를 추가한다. 그리고 테스트가 통과하는지 확인한다.

```python
def sub(a, b):
    return a - b
```

```python
    def test_sub(self):
        self.assertEqual(mymath.sub(1, 1), 0)
```

GitHub Desktop의 Changes는 두 파일이 수정 (주황색 ◙ 아이콘) 되었다는 것을 보여준다. Changes의 오른쪽에 코드는 파일이 어떻게 수정되었는지 차이를 보여주며, 이는 `git diff`에 해당한다. 이는 현재 work directory의 내용과 현재 index의 내용이 다름을 보여준다. (현재 index의 내용은 마지막 커밋의 내용과 같다) 

이제 두 번째 커밋을 한다. 앞에서 했는 것과 동일하게 좌측 아래에 커밋 도구를 이용한다. 아래 그림은 커밋 도구를 이용한 커밋 메시지 작성과 (왼쪽), 커밋 이후의 History (오른쪽) 을 보여준다.

| ![](\assets\images\github-desktop\github-desktop-f04-05-commit.png) |
| :----------------------------------------------------------: |
|              그림 4-5. 두 번째 Commit과 History              |



### Third, Fourth commits: mul, div functions

mymath.py에 mul 함수를 추가하고, test_mymath.py에 test_mul 메소드를 추가한다. 그리고 테스트가 통과하는지 확인한다.

```python
def mul(a, b):
    return a*b
```

```python
    def test_mul(self):
        self.assertEqual(mymath.mul(2, 2), 4)
```

아래 그림처럼 세 번째 커밋을 하고 History를 확인한다.

| ![](\assets\images\github-desktop\github-desktop-f04-06-commit.png) |
| :----------------------------------------------------------: |
|              그림 4-6. 세 번째 Commit과 History              |

mymath.py에 div 함수를 추가하고, test_mymath.py에 test_div 메소드를 추가한다. 그리고 테스트가 통과하는지 확인한다.

```python
def div(a, b):
    return a/b
```

```python
    def test_div(self):
        self.assertEqual(mymath.div(6, 2), 3)
```

아래 그림처럼 네 번째 커밋을 하고 History를 확인한다.

| ![](\assets\images\github-desktop\github-desktop-f04-07-commit.png) |
| :----------------------------------------------------------: |
|              그림 4-7. 네 번째 Commit과 History              |



### Undo

커밋 도구 제일 아래에 보면 Undo 버튼이 있다. Undo 버튼은 직전 커밋을 취소한다. 누를 때마다 한 단계씩 계속 취소한다. 단, 커밋만 취소가 되고 working directory의 파일 내용은 그대로 유지된다. 이 Undo 버튼은 git 명령어로는 아래에 해당된다.

```bash
$ git reset --mixed HEAD~
```

reset에 대해 확실히 알고 싶다면 [Reset 명확히 알고 가기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0)를 읽어 본다. 위 명령어는 HEAD를 직전 커밋으로 이동시키는데, `--mixed` 옵션에 의해 working directory의 내용은 변경하지 않는다. 다만 index 내용은 직전 커밋의 스냅샷으로 바꾼다. 이것을 확인하려면 Undo를 한 후 `git diff` 커맨드를 사용해 보면 된다. 또는 GitHub Desktop의 Changes만 봐도 알 수 있다.

이 Undo 버튼은 아래와 같은 상황에서 사용될 수 있다.

* 커밋을 했는데 커밋 메세지를 잘못 입력하여 바꾸고 싶을 때
* 코드를 좀 더 확장 또는 수정하고 커밋을 했어야 했는데, 모르고 그냥 커밋 했을 때
* 최근 몇 커밋을 하나의 커밋으로 통합하고 싶을 때



### Reflog

만약 커밋 취소 했다가 다시 복구 하고 싶다고 하자. 그런데 GitHub Desktop에는 commit redo 같은 기능은 없다. 이때는 터미널에서 직접 명령어를 입력해서 복구해야 한다. 

우선 동작 이력을 확인해야 한다. git bash 터미널을 열고 `git reflog`를 입력하면 아래와 같이 최근에 한 동작들에 대한 이력이 출력된다. 여기에는 커밋 이력도 포함되어 있다.

```bash
$ git reflog
c8b6430 (HEAD -> master) HEAD@{0}: reset: moving to c8b6430
a1d18c5 HEAD@{1}: reset: moving to a1d18c59
f9d9bf8 HEAD@{2}: reset: moving to f9d9bf80
05d730f HEAD@{3}: commit: Implement div function
f9d9bf8 HEAD@{4}: reset: moving to f9d9bf8
6d2228d HEAD@{5}: commit: Implement div function
f9d9bf8 HEAD@{6}: commit: Implement mul function
a1d18c5 HEAD@{7}: commit: Implement sub function
c8b6430 (HEAD -> master) HEAD@{8}: commit: Implement add function
f3599a7 HEAD@{9}: commit (initial): Initial commit
```

첫 번째 열의 `c8b6430` 같은 16진수 코드는 각각의 동작에 고유하게 부여되는 SHA1 hash 값의 앞 7자리다. 이것은 각 동작의 고유 ID로 쓰인다. 즉, div function을 구현한 커밋의 commit ID는 `6d2228d`인 것이다. 두 번째 열의 `HEAD@{n}`으로 나타낸 것은 현재 `HEAD`가 위치한 커밋에서 n 단계 전 임을 나타낸다. 이것은 특정 commit을 지시하는 별칭으로 사용할 수 있다. 즉, commitID가 필요한 어떤 명령어에 `6d2228d` 대신 `HEAD@{5}`로 지정할 수 있다. 물론 commit ID를 사용해도 된다. div 함수를 구현한 커밋으로 돌아가려면 아래 명령어를 입력하면 된다.

```bash
$ git reset --hard HEAD@{5}
```

git은 거의 모든 동작에 대해 이력을 저장하기 때문에 이를 이용해서 이전 상태를 쉽게 복구할 수 있다.



## 5. Branch

본 섹션의 내용은 [git 공부 정리 (4): git 연습, 브랜치 만들기](https://ranfort77.github.io/tool/tool-study-git-4/)를 GitHub Desktop에 적용한 것이다. GitHub Desktop을 이용해서 브랜치 생성, 이동, 병합, 삭제 방법에 대해 알아보자.



### Create a branch

메뉴에서 `Branch > New Branch ...`를 선택하거나, 아래 그림처럼 Current branch 툴의 Filter 박스에 새로운 브랜치 이름을 입력하고 Create new branch 버튼을 누르면 된다. 여기서는 develop 이라는 이름으로 새로운 브랜치를 만들었다.

| ![](\assets\images\github-desktop\github-desktop-f05-01-newbranch.png) |
| :----------------------------------------------------------: |
|                 그림 5-1. Create new branch                  |

새로운 develop branch를 만들면 곧바로 current branch는 develop이 된다. 이것은 터미널에서 아래 명령을 입력한 것과 같다. 

```bash
$ git branch develop
$ git checkout develop
```

또는 아래와 같이 한 명령으로도 할 수 있다.

```bash
$ git checkout -b develop
```



### Fifth, Sixth commits: dot, cross functions

**현재 develop 브랜치인 상태**에서 새로운 커밋을 해보자. 먼저 mymath.py에 dot 함수를 추가하고, test_mymath.py에 test_dot 메소드를 추가한다. 그리고 테스트가 통과하는지 확인한다.

```python
def dot(a, b):
    return sum(map(lambda x,y: x*y, a, b))
```

```python
    def test_dot(self):
        self.assertEqual(mymath.dot([1, 1, 1],[2, 2, 2]), 6)
```

Changes에서 파일이 수정됐음을 확인하고 Commit을 한다. Commit Message는 `Implement dot function`이다. 

이번에는 mymath.py에 cross 함수를 추가하고, test_mymath.py에 test_cross 메소드를 추가한다. 그리고 테스트가 통과하는지 확인한다.

```python
def cross(a, b):
    return [a[1]*b[2] - a[2]*b[1],
            a[2]*b[0] - a[0]*b[2],
            a[0]*b[1] - a[1]*b[0]]
```

```python
    def test_cross(self):
        self.assertEqual(mymath.cross([2, 3, 4],[5, 6, 7]), [-3, 6, -3])
```

Changes에서 파일이 수정됐음을 확인하고 Commit을 한다. Commit Message는 `Implement cross function`이다. 

현재까지 develop branch의 History는 add, sub, mul, div, dot, cross를 구현한 커밋임을 확인한다.



### Switching branch

이제 **master 브랜치로 이동**한다. GitHub Desktop에서 브랜치를 바꾸는 방법은 매우 간단하다. Current branch 툴을 클릭한 후, master를 선택하면 된다. 이 동작은 터미널에서 아래 명령어를 입력한 것과 같다. 

```bash
$ git checkout master
```

master 브랜치로 이동하면 mymath.py와 test_mymath.py가 네 번째 커밋인 div function 까지만 기록되어 있을 것이다. dot과 cross 구현 커밋은 develop 브랜치에 있기 때문이다. 



### Seventh, Eighth commits: ave, std functions

**현재 master 브랜치인 상태**에서 새로운 커밋을 해보자. 먼저 mymath.py에 ave 함수를 추가하고, test_mymath.py에 test_ave 메소드를 추가한다. 그리고 테스트가 통과하는지 확인한다.

```python
def ave(a):
    return sum(a)/len(a)
```

```python
    def test_ave(self):
        self.assertEqual(mymath.ave([1, 2]), 1.5)
```

Changes에서 파일이 수정됐음을 확인하고 Commit을 한다. Commit Message는 `Implement ave function`이다. 

이번에는 mymath.py에 std 함수를 추가하고, test_mymath.py에 test_std 메소드를 추가한다. 그리고 테스트가 통과하는지 확인한다.

```python
import math

def std(a):
    return math.sqrt(sum((e - ave(a))**2 for e in a)/(len(a)-1))
```

```python
    def test_std(self):
        self.assertAlmostEqual(mymath.std([2, 4, 5]), 1.5275252)
```

Changes에서 파일이 수정됐음을 확인하고 Commit을 한다. Commit Message는 `Implement std function`이다. 



### Compare to branch

이제 master 브랜치와 develop 브랜치는 서로 같은 커밋 이력을 가지고 있다. add, sub, mul, div 구현 커밋은 동일하다. 반면 develop 브랜치는 dot, cross 구현 커밋을 가지고 있고, master 브랜치는 ave, std 구현 커밋을 가지고 있다. 브랜치에 따라 history를 tree로 보려면 터미널에서 `git log --oneline --branches --graph`를 입력하면 된다. 결과는 아래와 같다.

```bash
$ git log --oneline --branches --graph
* 9d2d247 (HEAD -> master) Implement std function
* af2f781 Implement ave function
| * 7655b6b (develop) Implement cross function
| * d17b6f4 Implement dot function
|/
* 6d2228d Implement div function
* f9d9bf8 Implement mul function
* a1d18c5 Implement sub function
* c8b6430 Implement add function
* f3599a7 Initial commit
```

아쉽게도 GitHub Desktop에는 아직 history tree를 보는 GUI 기능이 없다. 이전 버전에서는 있었지만 새 버전이 나오면서 없어진 것 같다. ([Bring back the "Tree" to show history branches](https://github.com/desktop/desktop/issues/5468))

GitHub Desktop에는 두 브랜치의 history를 비교하는 기능은 있다. 두 브랜치를 비교하려면 아래 그림처럼 History 창의 Select branch to compare 박스를 이용하면 된다. 

| ![](\assets\images\github-desktop\github-desktop-f05-02-comparebranch.png) |
| :----------------------------------------------------------: |
|                    그림 5-2. branch 비교                     |

현재 위치한 master 브랜치에서 develop 브랜치를 선택하면 Behind와 Ahead가 활성화 된다. Behind는 develop브랜치에만 있는 커밋이 보이고, Ahead에는 master 브랜치에만 있는 커밋이 보인다. 이 기능은 아래 명령어를 입력한 것과 같다. 

```bash
$ git log master..develop  # Behind
$ git log develop..master  # Ahead
```

branch 간의 내용을 비교해 보는 명령어인 `git diff master..develop` 에 대한 기능 역시 아직 GitHub Desktop에 없는 것 같다. 



### Merge into current branch

이제 develop 브랜치의 커밋을 master 브랜치로 병합해 볼 것이다. 방법은 master 브랜치에 위치한 상태에서 메뉴의 `Branch > Merge into current branch ...` 를 선택하거나, Current branch 툴 가장 아래의 `Choose a branch to merge into master`를 클릭하면 된다. 그런데 병합 버튼 위에 보면 develop 브랜치를 master 브랜치로 병합하면 파일 두개가 충돌이 발생할 수 있다고 경고한다.

| ![](\assets\images\github-desktop\github-desktop-f05-03-merge.png) |
| :----------------------------------------------------------: |
|                       그림 5-3. Merge                        |

동일한 파일을 서로 다른 브랜치에서 수정한 상태에서, 두 브랜치를 병합하려 할 때 충돌 (conflict)이 발생할 수 있다. 그래도 Merge develop into master 버튼 누른다. 그러면 아래 그림과 같이 2개의 파일이 충돌이 발생했고 해결하라는 창이 뜬다.

| ![](\assets\images\github-desktop\github-desktop-f05-04-conflict.png) |
| :----------------------------------------------------------: |
|                     그림 5-4. Conflicts                      |

코드를 확인해 보면 충돌 지점에 표시가 되어 있다. 적절히 코드를 수정하여 충돌을 해결하면, 그림 5-4의 창이 아래 그림 5-5로 자동으로 변한다. (혹시 코드가 복잡해서 도저히 충돌을 해결하지 못하면 언제든지 Abort merge 버튼을 눌러서 merge를 중단할 수 있다) 이제 Commit merge를 눌러준다.

| ![](\assets\images\github-desktop\github-desktop-f05-05-resolve.png) |
| :----------------------------------------------------------: |
|                 그림 5-5. Resolve conflicts                  |

이제 master 브랜치의 history를 보면 merge commit이 생성되었음을 알 수있고, develop 브랜치에 있던 커밋들도 master 브랜치의 history에 모두 포함되어 있음을 알 수 있다. 현재 상태의 history tree를 보면 아래와 같다. 

```bash
$ git log --oneline --branches --graph
*   a1d6252 (HEAD -> master) Merge branch 'develop'
|\
| * 7655b6b (develop) Implement cross function
| * d17b6f4 Implement dot function
* | 9d2d247 Implement std function
* | af2f781 Implement ave function
|/
* 6d2228d Implement div function
* f9d9bf8 Implement mul function
* a1d18c5 Implement sub function
* c8b6430 Implement add function
* f3599a7 Initial commit
```



### Deleting branch

이제 develop 브랜치는 삭제해도 된다. GitHub Desktop에서 브랜치를 삭제하려면 우선 해당 브랜치로 체크아웃 해야한다. 그러면 메뉴의 `Branch > Delete...`가 활성화 되고, Delete를 눌러서 삭제하면 된다. 삭제한 후 GitHub Desktop에서 master 브랜치의 History를 확인하라. 그리고 터미널에서 history tree는 확인하면 아래와 같이 되어 있다.

```bash
$ git log --oneline --branches --graph
*   a1d6252 (HEAD -> master) Merge branch 'develop'
|\
| * 7655b6b Implement cross function
| * d17b6f4 Implement dot function
* | 9d2d247 Implement std function
* | af2f781 Implement ave function
|/
* 6d2228d Implement div function
* f9d9bf8 Implement mul function
* a1d18c5 Implement sub function
* c8b6430 Implement add function
* f3599a7 Initial commit
```

브랜치 삭제에 해당하는 명령어는 `git branch -d <branch-name>` 인데, 명령어로 할 때는 삭제되는 브랜치에 있으면 안 된다. 



### Restoring branch

Compare to branch 섹션에 있는 history tree 처럼, merge 되기 전으로 돌아가는 방법에 대해 설명한다. 이것은 GitHub Desktop에서는 할 수 없고, 터미널에서 명령어를 입력해야 한다. merge 전으로 돌아가려면 먼저 develop 브랜치를 복구해야 한다. 그런데 복구라기 보다는 다시 생성이라고 말하는게 더 적절하다. 아래 명령어를 이용한다.

```
$ git checkout -b develop <commit-id>
```

위 명령어는 develop 브랜치를 commit-id 위치에 생성하고, 곧바로 develop 브랜치로 체크아웃 하라는 명령어다. commit-id에는 develop 브랜치가 삭제 되기 전 위치해 있던 `7655b6b`를 입력하면 된다. 그러면 아래와 같이 develop 브랜치가 복구되었다. 

```bash
$ git log --oneline --branches --graph
*   a1d6252 (master) Merge branch 'develop'
|\
| * 7655b6b (HEAD -> develop) Implement cross function
| * d17b6f4 Implement dot function
* | 9d2d247 Implement std function
* | af2f781 Implement ave function
|/
* 6d2228d Implement div function
* f9d9bf8 Implement mul function
* a1d18c5 Implement sub function
* c8b6430 Implement add function
* f3599a7 Initial commit
```

이제 master 브랜치로 체크아웃 하고 master 브랜치의 위치를 `9d2d247`로 이동시키면 된다. 아래 명령어를 입력한다.

```bash
$ git checkout master
$ git reset --hard 9d2d247
```

그러면 아래와 같이 merge 하기 전 상태가 된다.

```bash
$ git log --oneline --branches --graph
* 9d2d247 (HEAD -> master) Implement std function
* af2f781 Implement ave function
| * 7655b6b (develop) Implement cross function
| * d17b6f4 Implement dot function
|/
* 6d2228d Implement div function
* f9d9bf8 Implement mul function
* a1d18c5 Implement sub function
* c8b6430 Implement add function
* f3599a7 Initial commit
```



## 6. Rebase

merge는 한 브랜치에 다른 브랜치를 병합하면서 새로운 merge commit을 만들며, 병합된 후에도 갈라졌다가 합쳐지는 commit tree 형태를 유지한다. 본 섹션에서는 merge 와는 다른 커밋 관리 방법인 rebase에 대해 살펴본다. 그리고 GitHub Desktop에 아직 구현되어 있지 않지만 cherry-pick, merge squash에 대해서도 간단히 언급한다. 이 내용은 [git 공부 정리 (5): git 연습, 리베이스, 체리픽 등](https://ranfort77.github.io/tool/tool-study-git-5/)을 GitHub Desktop으로 적용해 본 것이다.

rebase는 브랜치 B의 모든 커밋을 브랜치 A로 하나씩 이동시켜서, 커밋의 구조가 선형이 되도록 한다. 현재까지 예제로 사용한 master 브랜치와 develop 브랜치를 사용하여 rebase가 무엇인지 이해해 보자.

현재 repository는 아래와 같이 master 브랜치와 develop 브랜치로 나뉘어져 있는 상태다.

```bash
$ git log --oneline --branches --graph
* 9d2d247 (HEAD -> master) Implement std function
* af2f781 Implement ave function
| * 7655b6b (develop) Implement cross function
| * d17b6f4 Implement dot function
|/
* 6d2228d Implement div function
* f9d9bf8 Implement mul function
* a1d18c5 Implement sub function
* c8b6430 Implement add function
* f3599a7 Initial commit
```

develop 브랜치의 커밋들을 rebase하려면 먼저 develop 브랜치로 checkout 해야한다. 그런 후 메뉴 표시줄에서 `Branch > Rebase current branch...`를 선택하면 아래 그림과 같은 창이 또고, master를 선택하면 아래쪽에 Start rebase 버튼이 활성화된다. Start rebase 버튼 위에 텍스트는 rebase를 진행하면 "develop 브랜치의 2개의 커밋이 master 브랜치의 위쪽으로 이동하여 갱신된다"는 의미이다. 이제 Start rebase 버튼을 클릭한다.

| ![](\assets\images\github-desktop\github-desktop-f06-01-rebase.png) |
| :----------------------------------------------------------: |
|                       그림 6-1. rebase                       |

그러면 `Commit 1 of 2`라는 작은 메세지 창이 떳다가 금방 사라지고, 곧바로 아래 그림과 같이 mymath.py와 test_mymath.py에서 충돌이 발생했다고 알려준다. 두 개의 commit을 하나씩 순차적으로 rebase 하는 과정이라고 보면 된다. 따라서 conflict도 두 번 해결해야 한다. (모든 커밋이 충돌이 발생한다면 최대 rebase되는 commit 수 만큼 충돌을 해결해야 한다.)

| ![](\assets\images\github-desktop\github-desktop-f06-02-conflict.png) |
| :----------------------------------------------------------: |
|                      그림 6-2. conflict                      |

먼저 첫 번째 rebase되는 커밋인 dot 구현 관련 충돌을 해결한다. mymath.py 코드를 보면 아래와 같이 충돌이 일어난 코드를 표시해 주고 있다. 올바르게 코드를 수정한다. 마찬가지로 test_mymath.py도 올바로게 충돌을 해결해 준다. (코드가 복잡해서 도저히 충돌을 해결못하면 언제든지 Abort rebase 버튼을 눌러서 rebase 작업을 중단할 수 있다)

```python
<<<<<<< HEAD
def ave(a):
    return sum(a)/len(a)

def std(a):
    return math.sqrt(sum((e - ave(a))**2 for e in a)/(len(a)-1))
=======
def dot(a, b):
    return sum(map(lambda x,y: x*y, a, b))
>>>>>>> d17b6f4... Implement dot function
```

충돌이 없게 코드를 수정한 후 GitHub Desktop에 돌아오면, 아래 그림과 같이 충돌이 없다는 창이 뜬다. Continue rebase 버튼을 눌러서 rebase를 계속 진행한다.

| ![](\assets\images\github-desktop\github-desktop-f06-03-continue.png) |
| :----------------------------------------------------------: |
|                 그림 6-3. Resolve conflicts                  |

두 번째 rebase 되는 커밋인 cross 구현 관련 충돌은 아래 그림처럼 mymath.py 에서만 일어난다. 위에서 했던 방법대로 mymath.py 코드를 올바르게 수정/저장한다.

| ![](\assets\images\github-desktop\github-desktop-f06-04-continue.png) |
| :----------------------------------------------------------: |
|                      그림 6-4. conflict                      |

다시 GitHub Desktop에 돌아오면, 아래 그림과 같이 충돌이 없다는 창이 뜨며, Continue rebase를 클릭하면 모든 rebase가 완료된다.

| ![](\assets\images\github-desktop\github-desktop-f06-05-continue.png) |
| :----------------------------------------------------------: |
|                 그림 6-5. Resolve conflicts                  |

여기까지 진행하면 commit tree는 아래와 같다.

```bash
$ git log --oneline --branches --graph
* 6d0b818 (HEAD -> develop) Implement cross function
* 4483ed1 Implement dot function
* 9d2d247 (master) Implement std function
* af2f781 Implement ave function
* 6d2228d Implement div function
* f9d9bf8 Implement mul function
* a1d18c5 Implement sub function
* c8b6430 Implement add function
* f3599a7 Initial commit
```

merge와는 다르게 rebase는 develop 브랜치로 갈라졌던 커밋들을 master 브랜치에 하나씩 갖다 붙인 후, 그 끝에 develop 브랜치가 위치한다. 이렇게 함으로써 커밋 이력이 선형화되어 좀 더 깔끔해진다.

이제 할 일은 master 브랜치를 develop 브랜치가 있는 곳으로 이동시키는 것이다. 이에 해당하는 명령어는 아래와 같다.

```bash
$ git checkout master   # master 브랜치로 이동
$ git merge develop     # master 브랜치를 develop 브랜치로 Fast-forward
```

위 merge 명령은 단순히 master를 develop 쪽으로 옮기는 것으로 Fast-Forward (빨리 감기) 라고 부른다. 

위 작업은 GitHub Desktop에서도 할 수 있다. 우선 Current branch 툴을 사용해서 master로 체크아웃 한다. 그리고 메뉴의  `File > Merge into current branch...`를 클릭하여 아래 그림과 같이 develop 브랜치와 merge를 한다.

| ![](\assets\images\github-desktop\github-desktop-f06-06-merge.png) |
| :----------------------------------------------------------: |
|                    그림 6-6. Fast-forward                    |

이제 더 이상 develop 브랜치는 필요없으므로 삭제해도 된다. 삭제 방법은 Deleting branch 섹션에서 설명한 대로 하면 된다. 최종적으로 아래와 같은 commit tree가 된다.

```bash
$ git log --oneline --branches --graph
* 6d0b818 (HEAD -> master) Implement cross function
* 4483ed1 Implement dot function
* 9d2d247 Implement std function
* af2f781 Implement ave function
* 6d2228d Implement div function
* f9d9bf8 Implement mul function
* a1d18c5 Implement sub function
* c8b6430 Implement add function
* f3599a7 Initial commit
```

merge와 비교하여 rebase는 최종 commit tree가 깔끔하다는 장점이 있다. 반면 단점은 여러 커밋을 rebase 할 때 rebase하는 커밋마다 충돌이 많이 발생하면 하나하나의 충돌 해결이 귀찮다는 것이다.



### Cherry-pick

cherry-pick은 커밋 하나만 rebase 하는 명령이다. 아쉽게도 아직 GitHub Desktop에는 이 기능이 구현되어 있지 않은 것 같다. Cherry-pick에 대한 내용은 [여기 링크](https://ranfort77.github.io/tool/tool-study-git-5/#3-cherry-pick)를 참고하라.



### Merge squash

merge squash는 마치 merge와 rebase를 합한 것과 같은 기능이다. develop 브랜치의 dot과 cross 관련 두 커밋을 하나로 묶어서 커밋을 만들고, 그 커밋을 master로 rebase 한다. 아쉽게도 merge squash 역시 GitHub Desktop에 아직 구현되어 있지 않은 것 같다. merge squash에 대한 내용은 [여기 링크](https://ranfort77.github.io/tool/tool-study-git-5/#4-merge-squash)를 참고하라.



## 7. Revert

Revert는 특정 커밋에서 구현된 코드를 지우고, 지웠다는 이력에 대한 커밋을 남긴다. 지금까지 사용한 예제로 revert를 이해해 보자. 현재 commit 이력은 아래와 같다. GitHub Desktop의 History에서도 확인할 수 있다.

```bash
$ git log --oneline --branches --graph
* 5e32782 (HEAD -> master) Implement cross function
* d23f0e7 Implement dot function
* 9d2d247 Implement std function
* af2f781 Implement ave function
* 6d2228d Implement div function
* f9d9bf8 Implement mul function
* a1d18c5 Implement sub function
* c8b6430 Implement add function
* f3599a7 Initial commit
```

cross function 구현 관련 내용을 파일에서 삭제하고 싶다고 하자. 우선 아래 그림처럼 History에서 cross function 관련 커밋을 우클릭 한 후 `Revert this commit`을 선택한다.

| ![](\assets\images\github-desktop\github-desktop-f07-01-revert.png) |
| :----------------------------------------------------------: |
|                       그림 7-1. Revert                       |

그러면 Commit 메세지가 `Revert "Implement cross function"` 이라는 새로운 커밋이 생성된다. 그리고 mymath.py와 test_mymath.py를 확인해 보면 cross function 관련 구현이 사라져 있다. 마찬가지 방법으로 다른 커밋에도 적용할 수 있다. 

복구 하려면 commit undo를 하면 된다. 그런 후 파일의 내용을 되돌리려면 메뉴 표시줄의 `Branch > Discard all changes...`를 선택하면 뜨는 창에서 `Discard all changes`를 클릭하면 된다.



## 8. Stash all changes

브랜치 B에서 작업을 하는 중간에 브랜치 A로 체크아웃 해야 하는 경우가 생긴다. 그냥 체크아웃 하면 마지막 커밋부터 현재까지 작업한 내용을 잃어버리게 된다. 그렇다고 커밋을 하기에는 작업이 완료되지 않아 애매한 경우가 있다. 이 때 사용하는 것이 stash 기능이다. stash는 변한 내용을 저장하는 기능이다.

아래와 같이 master 브랜치와 develop 브랜치가 나뉘어져 있던 시점으로 다시 돌아가자.

```bash
$ git log --oneline --branches --graph
* 9d2d247 (HEAD -> master) Implement std function
* af2f781 Implement ave function
| * 7655b6b (develop) Implement cross function
| * d17b6f4 Implement dot function
|/
* 6d2228d Implement div function
* f9d9bf8 Implement mul function
* a1d18c5 Implement sub function
* c8b6430 Implement add function
* f3599a7 Initial commit
```

develop 브랜치로 체크아웃 한 후, mymath.py에 `# stash test` 라는 comment를 아무대나 입력한다. 그러면 GitHub Desktop의 Changes 창에 변한 부분이 보인다. 이제 메뉴에 `Branch > Stash all changes`를 클릭하면 아래 그림과 같이 Stashed Changes 라는 버튼이 생긴다. 

| ![](\assets\images\github-desktop\github-desktop-f08-01-stash.png) |
| :----------------------------------------------------------: |
|                       그림 8-1. Stash                        |

이제 master 브랜치로 갔다가 다시 develop 브랜치로 돌아온 후, 그림 8-1의 Stashed Changes를 누르고 Restore 버튼을 클릭하면 작업한 내용이 복구된다.



## 9. Push to GitHub

local repository를 remote repository인 GitHub로 업로드 하는 것은 GitHub Desktop에서 매우 간단히 할 수 있다. 툴 바에 Publish repository를 처음 클릭하면 아래와 같은 창이 뜬다.

| ![](\assets\images\github-desktop\github-desktop-f09-01-push.png) |
| :----------------------------------------------------------: |
|                 그림 9-1. Publish repository                 |

Name과 Description은 이미 입력이 되어 있고, Keep this code private이 체크되어 있으면 private repository로 업로드 되고, 해제하면 public으로 업로드 된다. Publish repository를 누르면 GitHub 저장소에 업로드 된다.

