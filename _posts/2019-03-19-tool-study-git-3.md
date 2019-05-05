---
title: "git 공부 정리 (3): git 연습, 커밋 하기"
excerpt: "버전관리 프로그램 git에 대해 공부한 기록 세 번째"
categories:
  - tool
tags:
  - git
  - github
  - jekyll
last_modified_at: 2019-03-21T00:30
---


git을 사용하여 코드 관리 연습을 해 보았다.


## 1. 첫 번째 커밋: add 함수 구현

여기서는 다음과 같은 명령어를 연습했다.

명령어 | 내용
------ | ----
`git init` | 위치한 폴더를 git 초기화
`git status` | 현재 상태 확인
.gitignore file | git에서 무시할 파일들 정의
`git status --ignored` | 무시되는 파일과 함께 현재 상태 확인
`git add .` | 위치한 폴더의 모든 파일을 index에 추가 
`git rm --cached <file>` | 지정한 파일의 추적 해제
`git add <file>` | 지정한 파일을 index에 추가 
`git commit -m <"commit message">` | 지정한 커밋 메세지와 함께 커밋
`git log` | 커밋 로그를 보여준다.

### git init, git status

우선 `testrepo/` 라는 이름으로 연습 폴더를 만들고, repository를 초기화 한다.

```bash
$ pwd
/d/[Projects]/proj17_git_and_github

$ mkdir testrepo
$ cd testrepo/
$ git init
Initialized empty Git repository in D:/[Projects]/proj17_git_and_github/testrepo/.git/
```

working directory에 `mymath.py` 파일을 만들고 아래 코드를 입력한다.

```python
# filename: mymath.py
def add(a, b):
    return a + b
```

위 코드를 테스트하기 위해 working directory에 `test_mymath.py` 파일을 만들고 아래 코드를 입력한다.

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

테스트가 통과하는 것을 확인하고, 현재 git의 상태를 확인해 본다.

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        __pycache__/
        mymath.py
        test_mymath.py

nothing added to commit but untracked files present (use "git add" to track)
```

### .gitignore, git status --ignored, git add

`__pycache__/` 폴더는 추적을 무시하기 위해 `.gitignore` 파일을 만들고 아래와 같이 작성한다.

```bash
$ touch .gitignore
$ vi .gitignore
(vi로 __pycache__ 를 입력)

$ cat .gitignore
__pycache__
```

다시 현재 git의 상태를 확인하는데 무시되는 파일도 같이 확인해 본다.

```bash
$ git status --ignored
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore
        mymath.py
        test_mymath.py

Ignored files:
  (use "git add -f <file>..." to include in what will be committed)

        __pycache__/

nothing added to commit but untracked files present (use "git add" to track)
```

Untracked files을 index에 올리고 상태를 확인한다.

```bash
$ git add .
warning: LF will be replaced by CRLF in .gitignore.
The file will have its original line endings in your working directory

$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   .gitignore
        new file:   mymath.py
        new file:   test_mymath.py
```

### git rm --cached

`test_mymath.py`만 추적해제하고 싶다면 다음과 같이 하면 된다. 예를 들어 몇개를 제외하고 아주 많은 파일을 index에 올릴때 `git add .`으로 올리고 몇 개만 삭제하는 방식이 쓰일지도 모른다.

```bash
$ git rm --cached test_mymath.py
rm 'test_mymath.py'

$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   .gitignore
        new file:   mymath.py

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        test_mymath.py
```

### git add, git commit

다시 `test_mymath.py`를 index에 추가한다.

```bash
$ git add test_mymath.py
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   .gitignore
        new file:   mymath.py
        new file:   test_mymath.py
```

첫 번째 커밋을 한다. 커밋 메세지는 [좋은 커밋 메시지 작성하기](https://blog.jonnung.com/git/2018/01/02/guide-to-better-git-commit-message/)를 최대한 따라한다. 그리고 log를 확인한다.

```bash
$ git commit -m "Implement add function"
[master (root-commit) e6679f9] Implement add function
 3 files changed, 16 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 mymath.py
 create mode 100644 test_mymath.py

$ git log
commit e6679f93001017d749c947669f14d1ce8f20b1cc (HEAD -> master)
Author: ranfort77 <ranfort77@gmail.com>
Date:   Thu Apr 11 16:49:18 2019 +0900

    Implement add function

$ git status
On branch master
nothing to commit, working tree clean
```

## 2. 두 번째 커밋: sub 함수 구현

다음과 같은 명령어를 연습했다.

명령어 | 내용
------ | ----
`git diff` | working directory와 index의 내용 차이
`git diff --cached` | index와 HEAD의 내용 차이
`git diff HEAD` | working directory와 HEAD의 내용 차이
`git log -p` | 각 커밋 간의 내용 차이
`git diff <commit-id1>..<commit-id2>` | 지정한 두 커밋간의 내용 차이
`git log --oneline` | 커밋 로그를 한 줄로 보여준다.

이제 sub 함수를 구현하기 위해 `mymath.py` 에 아래 코드를 추가한다.

```python
def sub(a, b):
    return a - b
```

그리고 sub 함수를 테스트 하기 위해 `test_mymath.py`의 아래 테스트 코드를 추가한다.

```python    
    def test_sub(self):
        self.assertEqual(mymath.sub(1, 1), 0)
```

테스트가 통과하는 것을 확인한다. 이제 working tree에 있는 `mymath.py`와 `test_mymath.py`가 변경되었기 때문에 working tree의 상태를 확인해 보자.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   mymath.py
        modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")
```

### git diff

커밋을 하기 전에, 현재 index의 내용과 working tree의 내용 차이를 체크해 보자.

```bash
$ git diff
diff --git a/mymath.py b/mymath.py
index bd762b1..50819d0 100644
--- a/mymath.py
+++ b/mymath.py
@@ -1,3 +1,6 @@
 #filename: mymath.py
 def add(a, b):
     return a + b
+
+def sub(a, b):
+    return a - b
diff --git a/test_mymath.py b/test_mymath.py
index 03d7def..a023657 100644
--- a/test_mymath.py
+++ b/test_mymath.py
@@ -3,10 +3,13 @@ import unittest
 import mymath

 class TestFoo(unittest.TestCase):
-
+
     def test_add(self):
         self.assertEqual(mymath.add(1, 1), 2)
-
-
+
+    def test_sub(self):
+        self.assertEqual(mymath.sub(1, 1), 0)
+
+
 if __name__ == '__main__':
     unittest.main()
```

이제 modified된 파일 내용을 index에 올린다.

```bash
$ git add .
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   mymath.py
        modified:   test_mymath.py
```

### git diff --cached

이제 modified 파일들이 index에 있기 때문에 `git diff`는 아무 내용도 출력하지 않는다. 왜냐하면 working tree와 index의 내용이 동일하기 때문이다. 대신 index 내용과 HEAD의 내용을 비교하면 차이가 생긴다.

```bash
$ git diff

$ git diff --cached
diff --git a/mymath.py b/mymath.py
index bd762b1..50819d0 100644
--- a/mymath.py
+++ b/mymath.py
@@ -1,3 +1,6 @@
 #filename: mymath.py
 def add(a, b):
     return a + b
+
+def sub(a, b):
+    return a - b
diff --git a/test_mymath.py b/test_mymath.py
index 03d7def..a023657 100644
--- a/test_mymath.py
+++ b/test_mymath.py
@@ -3,10 +3,13 @@ import unittest
 import mymath

 class TestFoo(unittest.TestCase):
-
+
     def test_add(self):
         self.assertEqual(mymath.add(1, 1), 2)
-
-
+
+    def test_sub(self):
+        self.assertEqual(mymath.sub(1, 1), 0)
+
+
 if __name__ == '__main__':
     unittest.main()
```

### git diff HEAD

당연하지만 working tree와 마지막 커밋 사이의 차이도 위와 같다. 

```bash
$ git diff HEAD
diff --git a/mymath.py b/mymath.py
index bd762b1..50819d0 100644
--- a/mymath.py
+++ b/mymath.py
@@ -1,3 +1,6 @@
 #filename: mymath.py
 def add(a, b):
     return a + b
+
+def sub(a, b):
+    return a - b
diff --git a/test_mymath.py b/test_mymath.py
index 03d7def..a023657 100644
--- a/test_mymath.py
+++ b/test_mymath.py
@@ -3,10 +3,13 @@ import unittest
 import mymath

 class TestFoo(unittest.TestCase):
-
+
     def test_add(self):
         self.assertEqual(mymath.add(1, 1), 2)
-
-
+
+    def test_sub(self):
+        self.assertEqual(mymath.sub(1, 1), 0)
+
+
 if __name__ == '__main__':
     unittest.main()
```

이제 두번째 커밋을 하고 로그를 확인해 본다.

```bash
$ git commit -m "Implement sub function"
[master ea1c1cb] Implement sub function
 2 files changed, 9 insertions(+), 3 deletions(-)

$ git log
commit ea1c1cbfb665ba0cd175616cbb6bb62f63a71b70 (HEAD -> master)
Author: ranfort77 <ranfort77@gmail.com>
Date:   Thu Apr 11 17:05:22 2019 +0900

    Implement sub function

commit e6679f93001017d749c947669f14d1ce8f20b1cc
Author: ranfort77 <ranfort77@gmail.com>
Date:   Thu Apr 11 16:49:18 2019 +0900

    Implement add function
```

이제 working directory, index, HEAD 모두 내용이 동일하기 때문에, `git diff`, `git diff --cached`, `git diff HEAD` 명령어는 모두 아무 내용을 출력하지 않는다.

### git log -p

출력 되는 로그에서 커밋 간의 차이를 모두 출력하면서 로그를 보려면 `git log -p`를 한다.

```bash
$ git log -p
commit ea1c1cbfb665ba0cd175616cbb6bb62f63a71b70 (HEAD -> master)
Author: ranfort77 <ranfort77@gmail.com>
Date:   Thu Apr 11 17:05:22 2019 +0900
(... 생략 ...)
```

### git log --oneline, git diff commit1..commit2

특정 두 커밋간의 차이를 보려면 `git diff <commit-id1>..<commit-id2>`이다.

```bash
$ git log --oneline
ea1c1cb (HEAD -> master) Implement sub function
e6679f9 Implement add function

$ git diff e6679f9..ea1c1cb
diff --git a/mymath.py b/mymath.py
index bd762b1..50819d0 100644
--- a/mymath.py
+++ b/mymath.py
@@ -1,3 +1,6 @@
 #filename: mymath.py
 def add(a, b):
     return a + b
+
+def sub(a, b):
+    return a - b
diff --git a/test_mymath.py b/test_mymath.py
index 03d7def..a023657 100644
--- a/test_mymath.py
+++ b/test_mymath.py
@@ -3,10 +3,13 @@ import unittest
 import mymath

 class TestFoo(unittest.TestCase):
-
+
     def test_add(self):
         self.assertEqual(mymath.add(1, 1), 2)
-
-
+
+    def test_sub(self):
+        self.assertEqual(mymath.sub(1, 1), 0)
+
+
 if __name__ == '__main__':
     unittest.main()
```

## 3. 세번째, 네번째 커밋: mul, div 함수 구현

다음과 같은 명령어를 연습했다.

명령어 | 내용
------ | ----
`git commit -am <"commit message">` | 추적되는 파일을 index에 add하고, 지정한 커밋 메세지로 커밋
`git reset --hard HEAD~` | 현재 HEAD의 바로 전 커밋으로 <br/>HEAD, index, working directory에 복사
`git reflog` | HEAD가 가리키는 커밋이 바뀔때마다 기록된 커밋 로그
`git reset --hard <commit-id>` | 지정한 커밋의 내용으로 HEAD, index, working directory에 복사 

`mymath.py`에 mul 함수를 구현한다.

```python
def mul(a, b):
    return a*b
```

`test_mymath.py`에 mul 함수에 대한 테스트를 추가한다.

```python    
    def test_mul(self):
        self.assertEqual(mymath.mul(2, 2), 4)
```

### git commit -am "commit message"

테스트가 통과하면 상태를 확인하고, 세번째 커밋을 한 후, 로그를 확인한다. 그리고 상태를 확인한다.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   mymath.py
        modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -am "Implement mul function"
[master fb38dae] Implement mul function
 2 files changed, 6 insertions(+)

$ git log --oneline
fb38dae (HEAD -> master) Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function

$ git status
On branch master
nothing to commit, working tree clean
```

`mymath.py` 에 div 함수를 구현한다.

```python
def div(a, b):
    return a/b
```

`test_mymath.py`에 div 함수에 대한 테스트를 추가한다.

```python    
    def test_div(self):
        self.assertEqual(mymath.div(6, 2), 3)
```

또다시 테스트가 통과하면 상태를 확인하고, 네번째 커밋을 한 후, 로그를 확인한다. 그리고 상태를 확인한다.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   mymath.py
        modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -am "Implement div function"
[master d36d6ce] Implement div function
 2 files changed, 6 insertions(+)

$ git log --oneline
d36d6ce (HEAD -> master) Implement div function
fb38dae Implement mul function
ea1c1cb Implement sub function
e6679f9 Implement add function

$ git status
On branch master
nothing to commit, working tree clean
```

### git reset --hard HEAD~

마지막 커밋인 네번째 커밋이 마음에 안들어서 바로전 커밋인 세번째 커밋으로 돌아가고 싶다. HEAD, index, working directory까지 모두 mul 함수를 구현했던 세번째 커밋으로 돌아가보자. `git reset --hard HEAD~` 명령어를 사용하면 HEAD가 가리키는 커밋의 바로 앞으로 돌아간다. 여기서 HEAD~는 HEAD가 가리키는 커밋의 바로 앞 커밋을 가리키는 별칭? 같아 보인다. --hard 옵션은 HEAD~가 가리키는 커밋 내용으로 working directory, index, HEAD로 모두 바꾸라는 의미다. (--mixed, --soft 도 있다. 각 의미는 Git pro책을 참고하거나 `git reset -h`를 하면 나온다.)

```bash
$ git reset --hard HEAD~
HEAD is now at fb38dae Implement mul function

$ git log --oneline
fb38dae (HEAD -> master) Implement mul function
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

$ cat test_mymath.py
# filename: test_mymath.py
import unittest
import mymath

class TestFoo(unittest.TestCase):

    def test_add(self):
        self.assertEqual(mymath.add(1, 1), 2)

    def test_sub(self):
        self.assertEqual(mymath.sub(1, 1), 0)

    def test_mul(self):
        self.assertEqual(mymath.mul(2, 2), 4)


if __name__ == '__main__':
    unittest.main()
```

### git reflog, git reset --hard commit-id

HEAD, index, working directory의 내용이 모두 같고 세번째 커밋의 내용으로 돌아간 상태이다. 그러나 git에는 네번째 커밋의 정보가 남아있다. `git reflog` 명령어의 출력내용은 HEAD가 가리키는 커밋이 바뀔때마다 기록되는 로그이다. 네번째 커밋의 커밋id를 확인하고, 네번째로 다시 상태를 돌린다. working directory의 내용도 네번째 커밋의 상태로 돌아온 상태임을 확인한다.

```bash
$ git reflog
fb38dae (HEAD -> master) HEAD@{0}: reset: moving to HEAD~
d36d6ce HEAD@{1}: commit: Implement div function
fb38dae (HEAD -> master) HEAD@{2}: commit: Implement mul function
ea1c1cb HEAD@{3}: commit: Implement sub function
e6679f9 HEAD@{4}: reset: moving to HEAD~
ade36b1 HEAD@{5}: reset: moving to ade36b1
e6679f9 HEAD@{6}: reset: moving to HEAD~
ade36b1 HEAD@{7}: commit (amend): Implement sub function
7d2298a HEAD@{8}: commit: Implement sub function
e6679f9 HEAD@{9}: commit (initial): Implement add function

$ git reset --hard d36d6ce
HEAD is now at d36d6ce Implement div function

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

$ cat test_mymath.py
# filename: test_mymath.py
import unittest
import mymath

class TestFoo(unittest.TestCase):

    def test_add(self):
        self.assertEqual(mymath.add(1, 1), 2)

    def test_sub(self):
        self.assertEqual(mymath.sub(1, 1), 0)

    def test_mul(self):
        self.assertEqual(mymath.mul(2, 2), 4)

    def test_div(self):
        self.assertEqual(mymath.div(6, 2), 3)


if __name__ == '__main__':
    unittest.main()
```

