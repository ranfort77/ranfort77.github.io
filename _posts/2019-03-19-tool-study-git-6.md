---
title: "git 공부 정리 (6): 원격저장소 Github 사용해 보기"
excerpt: "버전관리 프로그램 git에 대해 공부한 기록 여섯 번째"
categories:
  - tool
tags:
  - git
  - github
  - jekyll
last_modified_at: 2019-03-24T00:30
---

원격저장소 github 사용법을 연습해 보았다.

## 1. push, pull

다음과 같은 명령어를 연습했다.

명령어 | 내용
------ | ----
`git remote add <remote-name> <remote-address>` | 원격저장소 등록
`git remote` | 원격저장소 확인
`git remote -v` | 원격저장소 확인
`git push -u <remote-name> <branch-name>` | 처음 원격저장소에 업로드할때
`git push` | 원격저장소에  커밋 이력 업로드
`git pull` | 원격저장소에서 커밋 이력 다운로드

현재 로컬저장소의 커밋 상태는 다음과 같다.

```bash
$ git bl
* 547d9a8 (HEAD -> master) Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

이제 현재까지 작업한 내용을 github에 올리는 연습을 해본다. 먼저 github에 가입한 후, 원격저장소를 아래와 같이 만들었다. 원격저장소의 이름은 로컬저장소의 이름과 같은 필요는 없다. 그리고 "Initialize this repository with a README"는 체크하지 않았다.

![](/assets/images/2019-03-19/04_create_repo.bmp)

"Create repository"를 클릭하면 아래와 같은 창이 뜨는데, 저장소 주소가 HTTPS와 SSH가 있는데 우선 HTTPS를 클릭해서 주소를 복사해 둔다.(주소 우측에 작은 아이콘 클릭)

![](/assets/images/2019-03-19/05_git_address.bmp)

`git remote add <remote-name> <remote-address>` 방식으로 원격저장소를 등록해 준다. 보통 메인 원격저장소를 origin으로 이름 짓는게 관례라고 한다. 여러 명령어로 원격저장소가 등록되었다는 것을 확인할 수 있다.

```bash
$ git remote add origin https://github.com/ranfort77/myrepo1.git
$ git remote
origin

$ git remote -v
origin  https://github.com/ranfort77/myrepo1.git (fetch)
origin  https://github.com/ranfort77/myrepo1.git (push)

$ git config --list
(...생략...)
remote.origin.url=https://github.com/ranfort77/myrepo1.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
```

이제 `git push` 명령어로 로컬저장소를 원격저장소로 업로드를 시도해 보았다.

```bash
$ git push
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master
```

위와 같이 업로드가 실패하는데 실패한 이유는 현재 브랜치인 master가 업스트림 브랜치가 없으니 업스트림으로 리모트를 설정하기 위해 `git push -u origin master` 명령을 하라는 말 같다. 하기전에 업스트림 브랜치에 대해서 검색해 봤는데 [Git Pro 3.5 Git 브랜치 - 리모트 브랜치](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%A6%AC%EB%AA%A8%ED%8A%B8-%EB%B8%8C%EB%9E%9C%EC%B9%98)에 자세히 나온다.

```bash
$ git push -u origin master
Enumerating objects: 30, done.
Counting objects: 100% (30/30), done.
Delta compression using up to 8 threads
Compressing objects: 100% (29/29), done.
Writing objects: 100% (30/30), 2.85 KiB | 584.00 KiB/s, done.
Total 30 (delta 13), reused 0 (delta 0)
remote: Resolving deltas: 100% (13/13), done.
To https://github.com/ranfort77/myrepo1.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

그렇게 하라는대로 명령어를 입력하니까 위와 같이 뭔가 된것 같은 결과가 나온다. Github 저장소를 확인해 보니까 아래와 같이 업로드가 되었다. 신기하다. 여태까지 했던 소스 파일이랑 커밋 이력 등 여러 정보들이 보기 쉽게 정리되어 나온다.

![](/assets/images/2019-03-19/06_view_repo.bmp)

아래와 같이 `git config --list`를 다시 확인했더니 전에는 없던 정보가 등록되어 있다.

```bash
$ git config --list
(...생략...)
branch.master.remote=origin
branch.master.merge=refs/heads/master
```

아까 업스트림 브랜치 관련 설정인가 보다. 그리고 현재 git 커밋 이력을 확인해 봤다.

```bash
$ git bl
* 547d9a8 (HEAD -> master, origin/master) Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

처음에는 없었던 "origin/master" 라는 브랜치가 표시되어 있는데 Git Pro 책에 보니 저게 "리모트 트래킹 브랜치" 인가 보다. 저 리모트 트래킹 브랜치는 원격저장소와 상호작용할 때만 움직이는 것 같다. 실험삼아 아무 의미 없는 커밋을 만들어 봤다.

```bash
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

$ touch foo.txt
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        foo.txt

nothing added to commit but untracked files present (use "git add" to track)

$ git add foo.txt
$ git commit -am "Add foo.txt"
[master 67532a4] Add foo.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 foo.txt

$ git bl
* 67532a4 (HEAD -> master) Add foo.txt
* 547d9a8 (origin/master) Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

이제 `git status` 명령어가 원격저장소의 동기화가 최신인지 여부도 가르쳐 준다. 역시 커밋 이력을 보니 master 브랜치가 최신 커밋을 가리키고 있고 리모트 트래킹 브랜치는 한 커밋 전에 있어서 `git status`가 `git push` 를 하라고 가르쳐 준다. 이제부터는 `git push` 만 하면 업로드 된다.

```bash
$ git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 333 bytes | 333.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/ranfort77/myrepo1.git
   547d9a8..67532a4  master -> master

$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

$ git bl
* 67532a4 (HEAD -> master, origin/master) Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

상태를 확인해 보니 리모트 트래킹 브랜치가 최신 커밋을 가리키고 있고, Github에도 foo.txt 파일이 추가됨을 확인할 수 있었다.

이전에 한번 업로드된 커밋을 reset으로 삭제하면 안된다고 했는데 테스트 해봤다.

```bash
$ git bl
* 67532a4 (HEAD -> master, origin/master) Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git reset --hard HEAD~
HEAD is now at 547d9a8 Revert "Squashed commit of the following:"

$ git bl --all
* 67532a4 (origin/master) Add foo.txt
* 547d9a8 (HEAD -> master) Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git push
To https://github.com/ranfort77/myrepo1.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/ranfort77/myrepo1.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

진짜 거부되었다. 여기서 다시 origin/master가 가리키는 `67532a4` 커밋으로 master 브랜치를 이동시키는 방법은 reset으로 브랜치 이동을 해도되고, origin/master 브랜치와 merge로 fast-forward를 해도 되지만, 위 출력결과에도 있는 것처럼 `git pull` 을 해서 fast-forward하는 방법도 있다.

```bash
$ git pull
Updating 547d9a8..67532a4
Fast-forward
 foo.txt | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 foo.txt

$ git bl --all
* 67532a4 (HEAD -> master, origin/master) Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

revert로 foo.txt를 지워버려야 겠다. 그리고 `git push`로 다시 github에 커밋을 업로드 한다.

```bash
$ git revert 67532a4
[master 36f6033] Revert "Add foo.txt"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 foo.txt

$ git bl
* 36f6033 (HEAD -> master) Revert "Add foo.txt"
* 67532a4 (origin/master) Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git push
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 247 bytes | 247.00 KiB/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/ranfort77/myrepo1.git
   67532a4..36f6033  master -> master

$ git bl
* 36f6033 (HEAD -> master, origin/master) Revert "Add foo.txt"
* 67532a4 Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ ls -a
./  ../  .git/  .gitignore  __pycache__/  mymath.py  test_mymath.py
```

## 2. clone, fetch

현재까지의 작업 폴더인 testrepo/는 회사였다고 가정한다. 회사에서 하던 작업을 집에서도 연속해서 한다고 가정하고 Github에 저장한 저장소를 집의 로컬저장소로 복제한다. 집의 로컬저장소는 testrepo_clone/ 라고 하자. 우선 testrepo_clone/ 이라는 폴더를 만들고 `git clone <remote-address> <directory>`으로 원격저장소를 로컬저장소로 가져온다.

```bash
$ mkdir testrepo_clone
$ cd testrepo_clone/
$ ls -a
./  ../

$ git clone https://github.com/ranfort77/myrepo1.git .
Cloning into '.'...
remote: Enumerating objects: 34, done.
remote: Counting objects: 100% (34/34), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 34 (delta 14), reused 33 (delta 13), pack-reused 0
Unpacking objects: 100% (34/34), done.

$ ls -a
./  ../  .git/  .gitignore  mymath.py  test_mymath.py

$ git config --list
(...생략...)
user.name=ranfort77
user.email=ranfort77@gmail.com
alias.bl=log --branches --oneline --graph
(...생략...)
remote.origin.url=https://github.com/ranfort77/myrepo1.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master

$ git bl
* 36f6033 (HEAD -> master, origin/master, origin/HEAD) Revert "Add foo.txt"
* 67532a4 Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

`git clone` 명령어로 원격저장소 내용을 로컬저장소 testrepo_clone/ 으로 가져왔고 커밋이력도 그대로 임을 확인하였다. clone을 하니까 업스트림 브랜치 관련 설정도 다 되어 있고 말그대로 모든 설정이 testrepo/ 와 동일하다. 한가지 다른 것은 가장 최신 커밋에 origin/HEAD 라는 표시가 있다는 것이다.

이제 가상의 커밋 두개를 testrepo_clone/ 저장소에서 추가하고, `git push`를 해 본다.

```bash
$ touch bar.txt
$ git add bar.txt
$ git commit -am "Add bar.txt"
[master 65721d2] Add bar.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 bar.txt

$ touch temp.txt
$ git add temp.txt
$ git commit -am "Add temp.txt"
[master 0cff5e7] Add temp.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 temp.txt

$ git bl
* 0cff5e7 (HEAD -> master) Add temp.txt
* 65721d2 Add bar.txt
* 36f6033 (origin/master, origin/HEAD) Revert "Add foo.txt"
* 67532a4 Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git push
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 509 bytes | 509.00 KiB/s, done.
Total 5 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/ranfort77/myrepo1.git
   36f6033..0cff5e7  master -> master

$ git bl
* 0cff5e7 (HEAD -> master, origin/master, origin/HEAD) Add temp.txt
* 65721d2 Add bar.txt
* 36f6033 Revert "Add foo.txt"
* 67532a4 Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

이제 다시 회사로 갔다고 치고 testrepo/ 디렉토리로 간다. 현재 원격저장소에는 testrepo_clone/ 에서 "0cff5e7"까지 push 했는데, 아직 testrepo/ 에는 아래와 같이 "36f6003" 까지만 있다.

```bash
$ cd ..
$ cd testrepo/
$ git bl
* 36f6033 (HEAD -> master, origin/master) Revert "Add foo.txt"
* 67532a4 Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

`git pull`을 하면 원격저장소에 있는 모든 커밋을 가져오는 동시에 (그러면 리모트 트래킹 브랜치 origin/master가 "0cff5e7"을 가리킬 것이다.), master 브랜치가 fast-forward로 origin/master와 merge 될 것이다. 이것을 나눠서 하는게 `git fetch`, `git merge <branch>` 인데 여기서는 나눠서 해볼 것이다. `fetch` 하기전에 우선 `fetch`할 꺼리가 있는지 확인해 봐야 겠다. `git fetch --dry-run`을 하면 새로운 커밋이 원격저장소에 있는지 확인할 수 있는 것 같다.

```bash
$ git fetch --dry-run
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 5 (delta 1), reused 5 (delta 1), pack-reused 0
Unpacking objects: 100% (5/5), done.
From https://github.com/ranfort77/myrepo1
   36f6033..0cff5e7  master     -> origin/master

$ git bl
* 36f6033 (HEAD -> master, origin/master) Revert "Add foo.txt"
* 67532a4 Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

`git fetch --dry-run`으로 새로운 커밋이 원격저장소에 있는것을 확인했고, 커밋 이력을 보니 아직 업데이트는 안된 상황이다. 이제 `git fetch`로 커밋을 가져온다.

```bash
$ git fetch
From https://github.com/ranfort77/myrepo1
   36f6033..0cff5e7  master     -> origin/master

$ git bl --all
* 0cff5e7 (origin/master) Add temp.txt
* 65721d2 Add bar.txt
* 36f6033 (HEAD -> master) Revert "Add foo.txt"
* 67532a4 Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git fetch --dry-run
```

`git fetch`로 원격저장소의 최신 커밋을 가져왔고(origin/master가 "0cff5e7"을 가리키고 있다.) 아직 master는 merge가 안된 상태이다. 더이상 원격저장소에서 가져올 커밋들이 없으니까 `git fetch --dry-run`을 해도 아무것도 출력되지 않는 것 같다. 이제 merge 한다.

```bash
$ git merge
Updating 36f6033..0cff5e7
Fast-forward
 bar.txt  | 0
 temp.txt | 0
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 bar.txt
 create mode 100644 temp.txt

$ git bl
* 0cff5e7 (HEAD -> master, origin/master) Add temp.txt
* 65721d2 Add bar.txt
* 36f6033 Revert "Add foo.txt"
* 67532a4 Add foo.txt
* 547d9a8 Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ ls -a
./  ../  .git/  .gitignore  __pycache__/  bar.txt  mymath.py  temp.txt  test_mymath.py
```

최종적으로 merge를 하니까 master 브랜치가 최신 커밋을 가리키고 있고, working directory의 내용도 testrepo_clone/와 동일해 졌다.
