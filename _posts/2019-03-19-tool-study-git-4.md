---
title: "git 공부 정리 (4): git 연습, 브랜치 만들기"
excerpt: "버전관리 프로그램 git에 대해 공부한 기록 네 번째"
categories:
  - tool
tags:
  - git
  - github
  - jekyll
last_modified_at: 2019-03-22T00:30
---



[git 공부정리 (3): git 연습, 커밋하기]({{ site.url }}/tool/tool-study-git-3)에 이어서, git을 사용하여 코드 관리 연습을 계속 진행해 보았다.

## 1. branch 만들기

여기서는 다음과 같은 명령어를 연습했다.

명령어 | 내용
------ | ----
`git branch` | 브랜치 확인
`git branch <branch-name>` | 브랜치 생성
`git checkout <branch-name>` | 브랜치 이동
`git commit --amend` | 바로 전 커밋의 메세지 바꾸기
`git log --branches --oneline` | 모든 브랜치에 대한 한줄 커밋 로그 확인
`git log --branches --graph --oneline` | 모든 브랜치에 대한 연결된 한줄 커밋 로그 확인
`git log <branch1>..<branch2>` | branch1에는 없는 branch2의 커밋 로그를 보여준다.
`git log -p <branch1>..<branch2>` | branch1에는 없는 branch2의 커밋 로그를 보여주며 <br/>커밋간의 차이도 보여준다.
`git diff <branch1>..<branch2>` | branch1과 branch2의 최신 커밋간의 파일 내용을 차이를 보여준다.
`git merge <branch-name>` | 현재 위치한 branch에서 지정한 브랜치의 내용을 병합
`git config --global alias.<alias-name> "options ..."` | 지정한 options들을 alias-name으로 별칭을 만든다.
`git branch -d <branch-name>` | 지정한 브랜치 삭제

### git branch, git checkout

`develop` 라는 브랜치를 만들고, `develop` 브랜치에서 개발을 진행한다.

```bash
$ git branch
* master

$ git branch develop
$ git branch
  develop
* master

$ git checkout develop
Switched to branch 'develop'

$ git log --oneline
d36d6ce (HEAD -> develop, master) Implement div function
fb38dae Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function
```

`mymath.py`에 dot 함수를 구현한다.

```python
def dot(a, b):
    return sum(map(lambda x,y: x*y, a, b))
```

`test_mymath.py`에 dot 함수의 테스트를 구현한다.

```python    
    def test_dot(self):
        self.assertEqual(mymath.dot([1, 1, 1],[2, 2, 2]), 6)
```

테스트의 통과를 확인하고, 커밋한다.

```bash
$ git status
On branch develop
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   mymath.py
        modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -am "Implement dot function"
[develop c9d41fa] Implement dot function
 2 files changed, 6 insertions(+)

$ git log --oneline
c9d41fa (HEAD -> develop) Implement dot function
d36d6ce (master) Implement div function
fb38dae Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function
```

`mymath.py`에 cross 함수를 구현한다.

```python
def cross(a, b):
    return [a[1]*b[2] - a[2]*b[1],
            a[2]*b[0] - a[0]*b[2],
            a[0]*b[1] - a[1]*b[0]]
```

`test_mymath.py`에 cross 함수의 테스트를 구현한다.

```python    
    def test_cross(self):
        self.assertEqual(mymath.cross([2, 3, 4],[5, 6, 7]), [-3, 6, -3])
```

테스트의 통과를 확인하고, 커밋한다.

```bash
$ git status
On branch develop
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   mymath.py
        modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -am "implement corss fucntion"
[develop ca30e99] implement corss fucntion
 2 files changed, 8 insertions(+)

$ git log --oneline
ca30e99 (HEAD -> develop) implement corss fucntion
c9d41fa Implement dot function
d36d6ce (master) Implement div function
fb38dae Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function
```

### git commit --amend

커밋 하고 나니까 커밋 메세지의 잘못 입력했다. 대문자로 시작하지 못했고 "cross"를 "corss" 로 잘못 입력했다. 커밋메세지를 수정하려면 `git commit --amend` 를 사용하면 된다.

```bash
$ git commit --amend
(...vi가 열리면 commit 메세지를 수정할 것!...)
[develop 227c8e9] Implement cross fucntion
 Date: Thu Apr 11 17:20:13 2019 +0900
 2 files changed, 8 insertions(+)

$ git log --oneline
227c8e9 (HEAD -> develop) Implement cross fucntion
c9d41fa Implement dot function
d36d6ce (master) Implement div function
fb38dae Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function
```

커밋 메세지를 변경하면 커밋id가 바뀐다.("ca30e99"가 "227c8e9" 로 바뀌었다.) 왜냐하면 커밋메세지가 커밋 hash값을 생성하는데 포함되기 때문이다.  `git commit --amend`는 커밋메세지 변경 뿐 아니라 커밋할 내용을 빠뜨렸거나 다른 파일을 포함시키고 싶을때도 쓸 수 있다. working tree에서 변경할 것을 작성한 후, index에 add하고 `git commit --amend`를 하면 된다.

이제 master로 브랜치를 이동한다.

```bash
$ git checkout master
Switched to branch 'master'

$ git log --oneline
d36d6ce (HEAD -> master) Implement div function
fb38dae Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function

$ cat mymath.py
#filename: mymath.py
def add(a, b):
    return a + b

def sub(a, b):
    return a - b

def mul(a, b):
    return a*b

def div(a, b):
    return a/b
```

### git log --branches

master 브랜치는 네번째 커밋을 가리키고 있기 때문에 master로 checkout하면 working directory의 내용이 네번째 스냅샷으로 바뀐다. 현재 master 브랜치이기 때문에 `git log --oneline`은 master 브랜치의 로그만 보여주지만, `--branches` 옵션을 주면 모든 브랜치의 로그를 보여준다.

```bash
$ git branch
  develop
* master

$ git log --oneline
d36d6ce (HEAD -> master) Implement div function
fb38dae Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function

$ git log --branches --oneline
227c8e9 (develop) Implement cross fucntion
c9d41fa Implement dot function
d36d6ce (HEAD -> master) Implement div function
fb38dae Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function
```

이제 master 브랜치에서 `mymath.py`에 ave 함수를 구현한다.

```python
def ave(a):
    return sum(a)/len(a)
```

그 다음 ave 함수의 테스트를 `test_mymath.py`에 추가한다.

```python    
    def test_ave(self):
        self.assertEqual(mymath.ave([1, 2]), 1.5)
```

테스트가 통과하는 것을 확인한 후, 커밋을 한다.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   mymath.py
        modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -am "Implement ave function"
[master 9363da0] Implement ave function
 2 files changed, 6 insertions(+)

$ git log --oneline
9363da0 (HEAD -> master) Implement ave function
d36d6ce Implement div function
fb38dae Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function
```

마스터 브랜치의 커밋 로그를 볼 수 있다. 모든 브랜치의 커밋 로그를 보자.

```bash
$ git log --branches --oneline
9363da0 (HEAD -> master) Implement ave function
227c8e9 (develop) Implement cross fucntion
c9d41fa Implement dot function
d36d6ce Implement div function
fb38dae Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function
```

분기가 이루어 진것이 잘 보이지 않는다. 확인하려면 `--graph` 옵션을 추가해야 한다.

```bash
$ git log --branches --oneline --graph
* 9363da0 (HEAD -> master) Implement ave function
| * 227c8e9 (develop) Implement cross fucntion
| * c9d41fa Implement dot function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

master 브랜치에 커밋을 하나 더 추가한다. `mymath.py` 에 std 함수를 추가한다.

```python
import math

def std(a):
    return math.sqrt(sum((e - ave(a))**2 for e in a)/(len(a)-1))
```

역시 std 함수의 테스트를 `test_mymath.py`에 추가한다.

```python    
    def test_std(self):
        self.assertAlmostEqual(mymath.std([2, 4, 5]), 1.5275252)
```

테스트가 통과함을 확인하고 커밋을 하고 로그를 확인한다.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   mymath.py
        modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -am "Implement std function"
[master 84bc79f] Implement std function
 2 files changed, 8 insertions(+)

$ git log --branches --oneline --graph
* 84bc79f (HEAD -> master) Implement std function
* 9363da0 Implement ave function
| * 227c8e9 (develop) Implement cross fucntion
| * c9d41fa Implement dot function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

### git log branch1..branch2

이제 `master` 브랜치에서 `develop` 브랜치의 내용을 `merge` 할 것이다. 그러기 전에 두 브랜치의 차이를 확인해 보자. `git log <branch1>..<branch2>` 명령은 branch1에는 없지만 branch2에 있는 커밋을 확인 할 수 있다. `git log -p` 옵션으로 커밋간의 차이도 볼 수 있다.

```bash
$ git log master..develop --oneline
227c8e9 (develop) Implement cross fucntion
c9d41fa Implement dot function

$ git log develop..master --oneline
84bc79f (HEAD -> master) Implement std function
9363da0 Implement ave function
```

### git diff branch1..branch2

두 브랜치간의 차이를 보는 더 유용한 명령어는 `git diff <branch1>..<branch2>` 인 것 같다. 두 브랜치의 파일의 차이를 보여준다.

```bash
$ git diff master..develop
(...내용생략...)
```

### git merge

이제 master 브랜치에서 `develop` 브랜치를 `merge` 할 것이다. 그런데 뭔가 겁난다면 프로젝트 폴더 전체를 압축하여 다른데 복사해 두자.(아직초보니까) 아래와 같이 merge 명령을 실행하니까 `mymath.py`, `test_mymath.py` 모두 conflict가 발생했다.

```bash
$ git merge develop
Auto-merging test_mymath.py
CONFLICT (content): Merge conflict in test_mymath.py
Auto-merging mymath.py
CONFLICT (content): Merge conflict in mymath.py
Automatic merge failed; fix conflicts and then commit the result.
```

그리고 아래와 같이 상태를 확인해 보니 양쪽 브랜치에서 같은 파일을 수정해서 그런 것 같다.

```bash
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   mymath.py
        both modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")
```

`mymath.py`를 확인해 보니 아래와 같이 되어 있다. 동일 위치를 수정해서 conflict가 발생 한 것 같다.

```python
<<<<<<< HEAD
def ave(a):
    return sum(a)/len(a)

def std(a):
    return math.sqrt(sum((e - ave(a))**2 for e in a)/(len(a)-1))
=======
def dot(a, b):
    return sum(map(lambda x,y: x*y, a, b))

def cross(a, b):
    return [a[1]*b[2] - a[2]*b[1],
            a[2]*b[0] - a[0]*b[2],
            a[0]*b[1] - a[1]*b[0]]
>>>>>>> develop
```

`test_mymath.py`를 확인해 보니 역시 마찬가지 였다.

```python
<<<<<<< HEAD
    def test_ave(self):
        self.assertEqual(mymath.ave([1, 2]), 1.5)

    def test_std(self):
        self.assertAlmostEqual(mymath.std([2, 4, 5]), 1.5275252)
=======
    def test_dot(self):
        self.assertEqual(mymath.dot([1, 1, 1],[2, 2, 2]), 6)

    def test_cross(self):
        self.assertEqual(mymath.cross([2, 3, 4],[5, 6, 7]), [-3, 6, -3])
>>>>>>> develop
```

사실 merge를 처음 해보기 전에, git merge가 이정도 코드는 함수 구조상 복잡하지 않기 때문에 알아서 합쳐주지 않을까? 하는 기대가 있었다. 그런데 이렇게 conflict가 발생하는 것을 보고 git merge가 굉장히 보수적으로 merge를 한다는 생각이 들었다. 사실 프로그램 코드를 자동 merge한다는 것은 굉장히 위험한 일이다. 그래서 git이 이것을 conflict를 내는 것을 보고 오히려 안심이 됐다. 이런 중요한 단계에서는 프로그래머가 판단하도록 맡기는 것 같다. (그리고 수많은 종류의 프로그래밍 언어에 대해 git merge가 구문을 논리적으로 이해해서 자동 merge 해준다는 것도 말이 안되는 것 같다.)

어쨌든 conflict를 해결하기 위해 `mymath.py`를 아래와 같이 수정했다.

```python
def ave(a):
    return sum(a)/len(a)

def std(a):
    return math.sqrt(sum((e - ave(a))**2 for e in a)/(len(a)-1))

def dot(a, b):
    return sum(map(lambda x,y: x*y, a, b))

def cross(a, b):
    return [a[1]*b[2] - a[2]*b[1],
            a[2]*b[0] - a[0]*b[2],
            a[0]*b[1] - a[1]*b[0]]
```

마찬가지로 `test_mymath.py`를 아래와 같이 수정했다.

```python    
    def test_ave(self):
        self.assertEqual(mymath.ave([1, 2]), 1.5)

    def test_std(self):
        self.assertAlmostEqual(mymath.std([2, 4, 5]), 1.5275252)

    def test_dot(self):
        self.assertEqual(mymath.dot([1, 1, 1],[2, 2, 2]), 6)

    def test_cross(self):
        self.assertEqual(mymath.cross([2, 3, 4],[5, 6, 7]), [-3, 6, -3])
```

테스트가 통과하는 것을 확인한다. conflict를 해결했기 때문에 수정된 파일을 index에 올리고 커밋을 한다.

```bash
$ git commit -a
```

그러면 아래와 같은 머지 커밋 메세지 창이 뜬다.

```bash
Merge branch 'develop'

# Conflicts:
#       mymath.py
#       test_mymath.py
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#       .git/MERGE_HEAD
# and try again.


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# All conflicts fixed but you are still merging.
#
# Changes to be committed:
#       modified:   mymath.py
#       modified:   test_mymath.py
#
```

vi을 `:wq`로 저장하면 아래 메세지가 뜨고 merge가 완료된다.

```bash
[master a1b77c8] Merge branch 'develop'
```

아래와 같이 로그를 확인해 본다.

```bash
$ git log --branches --oneline --graph
*   a1b77c8 (HEAD -> master) Merge branch 'develop'
|\
| * 227c8e9 (develop) Implement cross fucntion
| * c9d41fa Implement dot function
* | 84bc79f Implement std function
* | 9363da0 Implement ave function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

### git config --global alias

긴 명령어인 `git log --branches --oneline --graph` 을 매번 치기 귀찮으니까 "bl"로 alias를 한다.

```bash
$ git config --global alias.bl "log --branches --oneline --graph"
$ git config --list
(...생략...)
alias.bl=log --branches --oneline --graph
(...생략...)

$ git bl
(...생략...)
```

### git branch -d 

이제 `develop` 브랜치는 필요없다고 치고 삭제한다.

```bash
$ git branch -d develop
Deleted branch develop (was 227c8e9).

ahs@ahs-PC MINGW64 /d/[Projects]/proj17_git_and_github/testrepo (master)
$ git bl
*   a1b77c8 (HEAD -> master) Merge branch 'develop'
|\
| * 227c8e9 Implement cross fucntion
| * c9d41fa Implement dot function
* | 84bc79f Implement std function
* | 9363da0 Implement ave function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

## 2. 삭제한 branch 복구

여기서는 ORIG_HEAD, HEAD~ 같은 커밋을 가리키는 별칭?과 다음 명령어를 알아보았다.

명령어 | 내용
------ | ----
`git checkout <branch-name> <commit-id>` | 지정한 커밋에 지정한 브랜치를 생성하고, 그 브랜치로 이동
`git show <object-hash>` | 지정한 hash의 내용을 보여준다.
`git show -s --oneline <object-hash>` | 지정한 hash의 내용을 한줄로 내용차이를 보여주는 output 없이 보여준다.

바로 전 단계에서 필요 없어져서 삭제한 `develop` 브랜치를 다시 복구 하고 싶다면 어떻게 해야할지 알아보았다. 브랜치를 새로 생성하는 명령어인 `git branch <branch-name>`와 생성된 브랜치로 이동하는 명령어인 `git checkout <branch-name>`을 한번에 하는 명령이 `git checkout -b <branch-name>` 이다. 그런데 `git checkout -b <branch-name> <commit-id>`을 하면 지정한 commit-id에 지정한 브랜치를 생성하는 것 같다. 즉, commit-id를 생략한 명령어인 `git checkout -b <branch-name>`는 현재 위치한 commit에 브랜치를 생성하고 그 브랜치로 이동하는 명령어인 것 같다. 이전에 삭제한 develop 브랜치가 커밋id "227c8e9"에 있었기 때문에 `git checkout -b develop 227c8e9` 를 하면 복구할 수 있다.(복구라기 보다는 새로 생성인것 같다.) 그러나 복구하기 전에 현재상태에서 reflog를 먼저 확인하자.

```bash
$ git reflog
a1b77c8 (HEAD -> master) HEAD@{0}: commit (merge): Merge branch 'develop'
84bc79f HEAD@{1}: commit: Implement std function
9363da0 HEAD@{2}: commit: Implement ave function
d36d6ce HEAD@{3}: checkout: moving from develop to master
227c8e9 HEAD@{4}: commit (amend): Implement cross fucntion
ca30e99 HEAD@{5}: commit: implement corss fucntion
c9d41fa HEAD@{6}: commit: Implement dot function
d36d6ce HEAD@{7}: checkout: moving from master to develop
d36d6ce HEAD@{8}: reset: moving to d36d6ce
fb38dae HEAD@{9}: reset: moving to HEAD~
d36d6ce HEAD@{10}: commit: Implement div function
fb38dae HEAD@{11}: commit: Implement mul function
ea1c1cb HEAD@{12}: commit: Implement sub function
e6679f9 HEAD@{13}: reset: moving to HEAD~
ade36b1 HEAD@{14}: reset: moving to ade36b1
e6679f9 HEAD@{15}: reset: moving to HEAD~
ade36b1 HEAD@{16}: commit (amend): Implement sub function
7d2298a HEAD@{17}: commit: Implement sub function
e6679f9 HEAD@{18}: commit (initial): Implement add function
```

위와 같이 `git reflog`를 하면 HEAD가 가리키는 커밋이 바뀔 때마다 reflog에 기록이 되는데 그 이력을 보여준다. 원래 develop 브랜치가 존재했으면 reflog에 develop 브랜치가 위치해 있었던 커밋에 develop이 표시가 되는데 지금은 삭제되 있는 상태라 표시가 되어 있지 않다. 그리고 각 commit-id 우측에 HEAD@{n}은 커밋을 가리키는 포인터? alias? 같은 것 같다.(정확한 명칭은 모르겠다.) git에는 HEAD@{n} 같은 커밋을 가리키는 미리정의된 포인터 같은게 많이 있는 것 같다. `git show <object-hash>` 명령어로 미리정의된 포인터 같은 것들의 의미를 확인 할 수 있는 것 같다. 예를들어 reflog의 HEAD@{n} 등이 가리키는 commit이 어떤 것인지 확인 할 수 있다.

```bash
$ git show -s --oneline HEAD@{0}
a1b77c8 (HEAD -> master) Merge branch 'develop'

$ git show -s --oneline HEAD@{1}
84bc79f Implement std function

$ git show -s --oneline HEAD@{2}
9363da0 Implement ave function
```

`git log --branches --oneline --graph`의 결과를 출력하면 다음과 같다.

```bash
$ git bl
*   a1b77c8 (HEAD -> master) Merge branch 'develop'
|\
| * 227c8e9 Implement cross fucntion
| * c9d41fa Implement dot function
* | 84bc79f Implement std function
* | 9363da0 Implement ave function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

미리 정의된 포인터 HEAD, master, ORIG_HEAD, HEAD~, HEAD~2, HEAD~3, HEAD^, HEAD^1, HEAD^2, HEAD~1^1, HEAD~1^2 등이 가리키는 커밋이 무엇인지 확인해 보자.

```bash
$ git show -s --oneline HEAD
a1b77c8 (HEAD -> master) Merge branch 'develop'

$ git show -s --oneline master
a1b77c8 (HEAD -> master) Merge branch 'develop'

$ git show -s --oneline ORIG_HEAD
84bc79f Implement std function

$ git show -s --oneline HEAD~
84bc79f Implement std function

$ git show -s --oneline HEAD~2
9363da0 Implement ave function

$ git show -s --oneline HEAD~3
d36d6ce Implement div function

$ git show -s --oneline HEAD^
84bc79f Implement std function

$ git show -s --oneline HEAD^1
84bc79f Implement std function

$ git show -s --oneline HEAD^2
227c8e9 Implement cross fucntion

$ git show -s --oneline HEAD~1^1
9363da0 Implement ave function
```

* ORIG_HEAD: 새로운 커밋이 만들어지거나 HEAD가 이동할 때, HEAD가 현재 커밋을 가리키고, 그 전에 HEAD가 가리킨 커밋을 ORIG_HEAD가 가리키는 것 같다.(확실하진 않다.)
* HEAD~: HEAD의 바로전 조상 커밋을 가리키는 것 같다. 현재는 master 브랜치가 HEAD라 ORIG_HEAD와 HEAD~가 같아보이는데 다른 브랜치로 HEAD를 이동시켰을 때 ORIG_HEAD와 HEAD~차이를 확인해 보자.
* HEAD~2: HEAD의 두단계 전 조상 커밋을 가리키는 것 같다.
* HEAD^: [브랜치 전환하기](https://backlog.com/git-tutorial/kr/stepup/stepup1_3.html) 에서는 캐럿 기호는 브랜치 병합이 여러개가 있을때 브랜치 병합을 지정할 수 있다고 한다. 여기서는 "4035f75"를 가리키고 있다.
* HEAD^2: 여기서는 다른 가지의 가장 최근 커밋을 가리키고 있다.
* HEAD~1^1: 첫번째 브랜치 병합의 한 커밋 전을 가리키고 있다.

이제 develop 브랜치를 "227c8e9"에 다시 만든다.

```bash
$ git checkout -b develop 227c8e9
Switched to a new branch 'develop'

$ git bl
*   a1b77c8 (master) Merge branch 'develop'
|\
| * 227c8e9 (HEAD -> develop) Implement cross fucntion
| * c9d41fa Implement dot function
* | 84bc79f Implement std function
* | 9363da0 Implement ave function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git reflog
227c8e9 (HEAD -> develop) HEAD@{0}: checkout: moving from master to develop
a1b77c8 (master) HEAD@{1}: commit (merge): Merge branch 'develop'
84bc79f HEAD@{2}: commit: Implement std function
9363da0 HEAD@{3}: commit: Implement ave function
d36d6ce HEAD@{4}: checkout: moving from develop to master
227c8e9 (HEAD -> develop) HEAD@{5}: commit (amend): Implement cross fucntion
ca30e99 HEAD@{6}: commit: implement corss fucntion
c9d41fa HEAD@{7}: commit: Implement dot function
d36d6ce HEAD@{8}: checkout: moving from master to develop
d36d6ce HEAD@{9}: reset: moving to d36d6ce
fb38dae HEAD@{10}: reset: moving to HEAD~
d36d6ce HEAD@{11}: commit: Implement div function
fb38dae HEAD@{12}: commit: Implement mul function
ea1c1cb HEAD@{13}: commit: Implement sub function
e6679f9 HEAD@{14}: reset: moving to HEAD~
ade36b1 HEAD@{15}: reset: moving to ade36b1
e6679f9 HEAD@{16}: reset: moving to HEAD~
ade36b1 HEAD@{17}: commit (amend): Implement sub function
7d2298a HEAD@{18}: commit: Implement sub function
e6679f9 HEAD@{19}: commit (initial): Implement add function
```

`develop` 브랜치를 다시 만들고 HEAD가 `develop` 브랜치를 가리키는 커밋에 있다. 이제 다시 여러 포인터가 가리키는 곳을 확인해 보자.

```bash
$ git show -s --oneline ORIG_HEAD
84bc79f Implement std function

$ git show -s --oneline HEAD~
c9d41fa Implement dot function

$ git show -s --oneline HEAD~1
c9d41fa Implement dot function

$ git show -s --oneline HEAD~2
d36d6ce Implement div function

$ git show -s --oneline HEAD~3
fb38dae Implement mul function

$ git show -s --oneline HEAD^
c9d41fa Implement dot function

$ git show -s --oneline HEAD^2
fatal: ambiguous argument 'HEAD^2': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
```

* ORIG_HEAD: HEAD의 위치가 바뀌었지만 ORIG_HEAD가 가리키는 커밋은 변함이 없다. 새로운 커밋이 만들어 진게 아니라서 그런가 추측한다. (확실하진 않다.)
* HEAD~: 현재 HEAD가 가리키는 develop 브랜치의 커밋의 한단계 전 조상 커밋을 가리키고 있다.
* HEAD~2: 현재 HEAD의 두단계 전 조상 커밋을 가리키고 있다.
* HEAD^: develop 브랜치는 브랜치 병합이 없었는데 현재 브랜치의 한단계 전 커밋을 가리키고 있다.
* HEAD^2: 브랜치 병합이 없어서 정의가 안되어 있는 것 같다.

현재 HEAD의 위치에 따라 상대적인 커밋을 가리키는 별칭들인 HEAD~, HEAD~2, HEAD^ 같은 걸 쓰기보다는 로그를 자세히 확인하면서 명확한 커밋을 지정해야겠다는 생각을 했다. 아직 초보인 입장에서 HEAD의 상대적인 위치를 지정하는 별칭을 쓰면 왠지 실수할 것 같다. 확실할때만 쓰자.

## 3. merge 되돌리기

현재 repository는 아래와 같은 상태이다. 

```bash
$ git bl
*   a1b77c8 (master) Merge branch 'develop'
|\
| * 227c8e9 (HEAD -> develop) Implement cross fucntion
| * c9d41fa Implement dot function
* | 84bc79f Implement std function
* | 9363da0 Implement ave function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

master 브랜치로 이동한 후, merge를 되돌리자. merge를 돌리려면 `git reset --hard 84bc79f` 라고 하면 되는데, 커밋id인 `84bc79f`는 HEAD가 master에 있을 때, HEAD~가 가리키고 있으므로 아래와 같이 하면 된다. 

```bash
$ git checkout master
Switched to branch 'master'

$ git bl
*   a1b77c8 (HEAD -> master) Merge branch 'develop'
|\
| * 227c8e9 (develop) Implement cross fucntion
| * c9d41fa Implement dot function
* | 84bc79f Implement std function
* | 9363da0 Implement ave function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git reset --hard HEAD~
HEAD is now at 84bc79f Implement std function

$ git bl
* 84bc79f (HEAD -> master) Implement std function
* 9363da0 Implement ave function
| * 227c8e9 (develop) Implement cross fucntion
| * c9d41fa Implement dot function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git diff master..develop
(...생략...)
```

이제 merge하기 전 상태로 돌아왔다.

