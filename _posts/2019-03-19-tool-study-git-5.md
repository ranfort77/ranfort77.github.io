---
title: "git 공부 정리 (5): git 연습, 리베이스, 체리픽 등"
excerpt: "버전관리 프로그램 git에 대해 공부한 기록 다섯 번째"
categories:
  - tool
tags:
  - git
  - github
  - jekyll
last_modified_at: 2019-03-23T00:30
---

[git 공부정리 (4): git 연습, 브랜치 만들기]({{ site.url }}/tool/tool-study-git-4)에 이어서, git을 사용하여 코드 관리 연습을 계속 진행해 보았다.

## 1. rebase

여기서는 다음과 같은 명령어를 연습하였다.

명령어 | 내용
------ | ----
`git rebase <branch-name>` | 현재 위치한 브랜치의 커밋들을 지정한 브랜치로 rebase
`git rebase --continue` | rebase시 conflict가 발생하면, 해결 후 rebase 계속 진행
`git rebase --abort` | rebase시 conflict가 발생하면, rebase 중단

이제 develop 브랜치의 내용을 master 브랜치로 rebase 해 보겠다. rebase는 rebase 할 내용이 있는 develop 브랜치로 먼저 checkout을 한 후, develop의 내용을 master로 rebase 한다는 의미의 명령어인 `git rebase master` 라고 해야 한다.

```bash
$ git checkout develop
Switched to branch 'develop'

$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: Implement dot function
Using index info to reconstruct a base tree...
M       mymath.py
M       test_mymath.py
Falling back to patching base and 3-way merge...
Auto-merging test_mymath.py
CONFLICT (content): Merge conflict in test_mymath.py
Auto-merging mymath.py
CONFLICT (content): Merge conflict in mymath.py
error: Failed to merge in the changes.
hint: Use 'git am --show-current-patch' to see the failed patch
Patch failed at 0001 Implement dot function
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
```

결과는 `develop` 브랜치의 `c9d41fa` 커밋인 "Implement dot function"를 `master` 브랜치의 `84bc79f` 커밋인 "Implement std function"를 base로 하는 커밋으로 만들기를 시도하다가 conflict가 발생했다는 것이다.

conflict가 발생한 `mymath.py` 내용을 확인하고 아래와 같이 conflict를 해결한다.

```python
def ave(a):
    return sum(a)/len(a)

def std(a):
    return math.sqrt(sum((e - ave(a))**2 for e in a)/(len(a)-1))

def dot(a, b):
    return sum(map(lambda x,y: x*y, a, b))
```

마찬가지로 `test_mymath.py` 내용을 확인하고 아래와 같이 confilict를 해결한다.

```python    
    def test_ave(self):
        self.assertEqual(mymath.ave([1, 2]), 1.5)

    def test_std(self):
        self.assertAlmostEqual(mymath.std([2, 4, 5]), 1.5275252)

    def test_dot(self):
        self.assertEqual(mymath.dot([1, 1, 1],[2, 2, 2]), 6)
```

conflict를 해결했으면 테스트가 통과하는 것을 확인한다. rebase를 계속 진행하려면 conflict가 해결된 파일을 다시 index로 add 한 후, `git rebase --continue`를 한다.

```bash
$ git status
rebase in progress; onto 84bc79f
You are currently rebasing branch 'develop' on '84bc79f'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

        both modified:   mymath.py
        both modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")

$ git add .
$ git status
rebase in progress; onto 84bc79f
You are currently rebasing branch 'develop' on '84bc79f'.
  (all conflicts fixed: run "git rebase --continue")

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   mymath.py
        modified:   test_mymath.py

$ git rebase --continue
Applying: Implement dot function
Applying: Implement cross fucntion
```

conflict를 해결했기 때문에 첫번째 rebase의 결과는 잘 진행되며(Applying: Implement dot function), 두번째 rebase에서(Applying: Implement cross function) 는 conflict 없이 잘 진행된다. (혹시 conflict가 발생하면 위와 같은 동작을 반복하면 된다.)

rebase 작업은 잘 완료가 되었다. 아래왁 같이 커밋 이력은 매우 깨끗하게 한줄로 나열된다. 

```bash
$ git bl
* eb005f7 (HEAD -> develop) Implement cross fucntion
* c08399a Implement dot function
* 84bc79f (master) Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

이제 master 브랜치를 최신 커밋상태로 Fast-Forward(빨리감기) 하면 된다.

```bash
$ git checkout master
Switched to branch 'master'

$ git merge develop
Updating 84bc79f..eb005f7
Fast-forward
 mymath.py      | 8 ++++++++
 test_mymath.py | 6 ++++++
 2 files changed, 14 insertions(+)

$ git bl
* eb005f7 (HEAD -> master, develop) Implement cross fucntion
* c08399a Implement dot function
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

이제 `develop` 브랜치는 필요없으니까 삭제한다.

```bash
$ git branch -d develop
Deleted branch develop (was eb005f7).

$ git bl
* eb005f7 (HEAD -> master) Implement cross fucntion
* c08399a Implement dot function
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

rebase 중간에 conflict를 도저히 해결못하겠다 싶으면 `git rebase --abort` 로 rebase를 중단할 수 있다.

그리고 생활코딩 사이트의 egoing님의 [rebase 강의의 추가 의견](https://opentutorials.org/course/2708/15553)을 보면 rebase에 관한 팁이 있다.

"*강의 보았습니다만, 개인적으로는 merge보다 rebase를 선호하고, 오히려 rebase를 안전하게 느끼고 있습니다. 중간에 conflict를 수정하다 어려움을 만나도 언제든지 rebase --abort로 rebase 작업을 취소할 수 있고, 심지어 그것 조차도 부담이 된다면, rebase 전에 임시 branch를 백업 용도로 만들었다가 rebase 후에 삭제하는 방법을 사용하기도 합니다.
세번째 강의에서처럼 지속적으로 conflict가 발생하는 경우라면, 아예 master에 임시 branch를 만들고 커밋별로 cherry-pick을 하기도 합니다.
rebase와 cherry-pick으로 히스토리를 정리한 뒤 push하면 master 브랜치 관리자가 해당 branch를 확인후 fast-forward 방식만으로 master를 관리하기 쉽게 만들어준다는 장점이 있습니다.*"(안신열님의 의견)

언급한 cherry-pick에 대해서는 이후에 공부해 봐야겠다. 뭔지 아직 모른다.

아무래도 최종 커밋 이력을 보면 rebase가 좋아보인다. 한줄로 쭉 나열되고 마치 한사람이 전부 코드를 개발한것 마냥 이력이 정리될 것 같다. 그런데 rebase는 rebase를 하려는 브랜치의 커밋 개수만큼 마지막에 커밋을 새로 정리하는 거라 rebase 하려는 커밋이 많으면 rebase 시에 좀 귀찮기는 할 것 같다. 위 추가 의견에 있듯이 관리자가 편하다고 한다. 관리자를 안해봐서 아직은 어떤 면이 편한지 감이 잡히지는 않는다.

merge는 이력 모양새가 rebase만큼은 깨끗할것 같지는 않다. 그런데 merge를 하면 merge시에 병합되는 커밋하나만 생겨서 귀찮음이 덜한 것 같다.  

프로젝트에 따라서 브랜치 별로 merge를 할지 rebase를 할지 정책을 정한다고도 한다. 나중에 차차 알아보자.

## 2. 완료된 rebase 되돌리기

아래와 같이 rebase가 완료된 것을 rebase 되기 전으로 돌리려면 어떻게 할까 생각해 봤다.

```bash
$ git bl
* eb005f7 (HEAD -> master) Implement cross fucntion
* c08399a Implement dot function
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

우선 reflog를 확인해보면, rebase 되기 전의 develop 브랜치가 가리키던 커밋이 남아있다.

```bash
$ git reflog
eb005f7 (HEAD -> master) HEAD@{0}: merge develop: Fast-forward
84bc79f HEAD@{1}: checkout: moving from develop to master
eb005f7 (HEAD -> master) HEAD@{2}: rebase finished: returning to refs/heads/develop
eb005f7 (HEAD -> master) HEAD@{3}: rebase: Implement cross fucntion
c08399a HEAD@{4}: rebase: Implement dot function
84bc79f HEAD@{5}: rebase: checkout master
227c8e9 HEAD@{6}: checkout: moving from master to develop
84bc79f HEAD@{7}: reset: moving to HEAD~
a1b77c8 HEAD@{8}: checkout: moving from develop to master
227c8e9 HEAD@{9}: checkout: moving from master to develop
a1b77c8 HEAD@{10}: commit (merge): Merge branch 'develop'
84bc79f HEAD@{11}: commit: Implement std function
9363da0 HEAD@{12}: commit: Implement ave function
d36d6ce HEAD@{13}: checkout: moving from develop to master
227c8e9 HEAD@{14}: commit (amend): Implement cross fucntion
ca30e99 HEAD@{15}: commit: implement corss fucntion
c9d41fa HEAD@{16}: commit: Implement dot function
d36d6ce HEAD@{17}: checkout: moving from master to develop
d36d6ce HEAD@{18}: reset: moving to d36d6ce
fb38dae HEAD@{19}: reset: moving to HEAD~
d36d6ce HEAD@{20}: commit: Implement div function
fb38dae HEAD@{21}: commit: Implement mul function
ea1c1cb HEAD@{22}: commit: Implement sub function
e6679f9 HEAD@{23}: reset: moving to HEAD~
ade36b1 HEAD@{24}: reset: moving to ade36b1
e6679f9 HEAD@{25}: reset: moving to HEAD~
ade36b1 HEAD@{26}: commit (amend): Implement sub function
7d2298a HEAD@{27}: commit: Implement sub function
e6679f9 HEAD@{28}: commit (initial): Implement add function
```

`227c8e9 HEAD@{6}`이 rebase 되기 전에 master에서 develop으로 checkout한 시점인데, 그때 develop 브랜치가 가리키던 커밋이다. 저 커밋을 다시 develop 브랜치로 만들어 주면 될것 같다.

```bash
$ git checkout -b develop HEAD@{6}
Switched to a new branch 'develop'

$ git bl
* eb005f7 (master) Implement cross fucntion
* c08399a Implement dot function
* 84bc79f Implement std function
* 9363da0 Implement ave function
| * 227c8e9 (HEAD -> develop) Implement cross fucntion
| * c9d41fa Implement dot function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

git의 멋진 자료구조 덕분에 매우 쉽게 브랜치 분기가 다시 복구가 된다. 놀랍다. (자료구조의 원리는 egoing의 git 강의를 보면 이해가 쉬웠다.) 이제 master 브랜치의 rebase된 커밋만 되돌리면 rebase하기 전이랑 똑같아 진다.

```bash
$ git checkout master
Switched to branch 'master'

$ git reset --hard 84bc79f
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
```

## 3. cherry-pick

다음과 같은 명령어를 연습했다.

명령어 | 내용
------ | ----
`git cherry-pick <commit-id>` | 지정한 커밋을 rebase
`git cherry-pick --continue` | cherry-pick 계속 진행

현재 커밋 이력을 보면 HEAD가 master에 있다.

```bash
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
```

develop 브랜치의 두 커밋을 cherry-pick으로 "c9d41fa", "227c8e9" 순서로 master로 rebase 한다. cherry-pick을 하려면 우선 base가 위치한 브랜치에서 `git cherry-pick <commit-id>`를 해야 하는 것 같다. 현재 master 브랜치에 있으니 checkout은 안해도 된다. 먼저 "c9d41fa"를 진행한다.

```bash
$ git cherry-pick c9d41fa
error: could not apply c9d41fa... Implement dot function
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'
```

이전 rebase 처럼 conflict를 발생한다. conflict를 해결하고, 테스트가 통과하는 것을 확인하고 cherry-pick을 계속 진행한다.

```bash
$ git status
On branch master
You are currently cherry-picking commit c9d41fa.
  (fix conflicts and run "git cherry-pick --continue")
  (use "git cherry-pick --abort" to cancel the cherry-pick operation)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   mymath.py
        both modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")

$ git add .
$ git cherry-pick --continue
[master 2ae43dc] Implement dot function
 Date: Thu Apr 11 17:17:47 2019 +0900
 2 files changed, 6 insertions(+)

$ git bl
* 2ae43dc (HEAD -> master) Implement dot function
* 84bc79f Implement std function
* 9363da0 Implement ave function
| * 227c8e9 (develop) Implement cross fucntion
| * c9d41fa Implement dot function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

그 다음 `227c8e9` 도 cherry-pick을 진행한다.

```bash
$ git cherry-pick 227c8e9
[master 0687e2a] Implement cross fucntion
 Date: Thu Apr 11 17:20:13 2019 +0900
 2 files changed, 8 insertions(+)

$ git bl
* 0687e2a (HEAD -> master) Implement cross fucntion
* 2ae43dc Implement dot function
* 84bc79f Implement std function
* 9363da0 Implement ave function
| * 227c8e9 (develop) Implement cross fucntion
| * c9d41fa Implement dot function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

두 번째 커밋은 conflict 없이 진행이 완료된다. 이전과 마찬가지로 develop 브랜치를 삭제해 보자.

```bash
$ git branch -d develop
error: The branch 'develop' is not fully merged.
If you are sure you want to delete it, run 'git branch -D develop'.

$ git branch -D develop
Deleted branch develop (was 227c8e9).

$ git bl
* 0687e2a (HEAD -> master) Implement cross fucntion
* 2ae43dc Implement dot function
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

merge가 안된 브랜치는 -d 옵션으로 삭제가 안되기 때문에 -D 옵션으로 삭제한다. 그러면 최종적으로 rebase 한것과 동일해 진다.

cherry-pick을 중간에 취소하는 명령어는 rebase와 비슷하게 `git cherry-pick --abort` 이다.

## 4. merge --squash

다음과 같은 명령어를 연습했다.

명령어 | 내용
------ | ----
`git merge --squash <branch>` | 지정한 커밋을 squash 병합

이전에 했던 것 처럼 아래와 같이 다시 병합이 되기 전으로 되돌리자.

```bash
$ git checkout -b develop 227c8e9
Switched to a new branch 'develop'

$ git bl
* 0687e2a (master) Implement cross fucntion
* 2ae43dc Implement dot function
* 84bc79f Implement std function
* 9363da0 Implement ave function
| * 227c8e9 (HEAD -> develop) Implement cross fucntion
| * c9d41fa Implement dot function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git checkout master
Switched to branch 'master'

$ git bl
* 0687e2a (HEAD -> master) Implement cross fucntion
* 2ae43dc Implement dot function
* 84bc79f Implement std function
* 9363da0 Implement ave function
| * 227c8e9 (develop) Implement cross fucntion
| * c9d41fa Implement dot function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git reset --hard 84bc79f
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
```

develop 브랜치에 있는 두 최신 커밋인 `c9d41fa`, `227c8e9`를 병합 한 후, 그 내용을 master 브랜치로 커밋을 만드는게(병합한 것을 rebase 하는 것 같다.) `git merge --squash <branch>` 라고 한다. merge랑 rebase를 익혔는데 이것도 언젠가 쓸모가 있나보다. 우선은 어떤식으로 되는지 확인해 보자.

```bash
$ git branch
  develop
* master

$ git merge --squash develop
Auto-merging test_mymath.py
CONFLICT (content): Merge conflict in test_mymath.py
Auto-merging mymath.py
CONFLICT (content): Merge conflict in mymath.py
Squash commit -- not updating HEAD
Automatic merge failed; fix conflicts and then commit the result.
```

일단 master 브랜치에서 `git merge --squash develop`를 하면 conflict가 발생한다. conflict를 해결하고, 테스트가 통과하는 것을 확인한다.

```bash
$ git status
On branch master
Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

        both modified:   mymath.py
        both modified:   test_mymath.py

no changes added to commit (use "git add" and/or "git commit -a")

$ git add .
$ git commit -a

(... vi 창에서 이미 squash 커밋 메세지가 아래와 같이 입력되어 있고 ...)
(... :wq 로 저장하면 아래와 같이 진행된다.)

[master 3f08fd3] Squashed commit of the following:
 2 files changed, 14 insertions(+)

$ git bl
* 3f08fd3 (HEAD -> master) Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
| * 227c8e9 (develop) Implement cross fucntion
| * c9d41fa Implement dot function
|/
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

이제 `3f08fd3` 커밋에 `c9d41fa`, `227c8e9` 커밋의 코드 구현이 다 들어 있으니까 develop 브랜치는 필요없다. 삭제한다.

```bash
$ git branch -d develop
error: The branch 'develop' is not fully merged.
If you are sure you want to delete it, run 'git branch -D develop'.

$ git branch -D develop
Deleted branch develop (was 227c8e9).

$ git bl
* 3f08fd3 (HEAD -> master) Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

최종 결과를 보니까 `merge --squash`가 장점이 있는 것 같다. 이전에 merge와 rebase 비교에서 rebase는 커밋 이력을 일자로 만들어줘서 merge보다 깔끔하지만, 각 커밋만큼의 rebase된 커밋 개수가 생겨야 되서 conflict가 계속 날 경우 일일히 해결하기 귀찮았는데, squash merge는 rebase처럼 일자로 되면서 커밋이 하나만 생겨서 귀찮음이 덜 한 것 같다. 물론 rebase되는 각각의 커밋들의 개발 이력들이 매우 중요하고 남길 필요가 있다면 rebase를 쓰는게 좋을 것 같고, 그게 아니라 하나로 묶어도 되는 커밋들이면 squash merge도 좋은 것 같다.

## 5. revert

다음과 같은 명령어를 연습했다.

명령어 | 내용
------ | ----
`git revert <commit-id>` | 지정한 커밋의 구현 코드를 삭제한 커밋을 생성

git 관련 자료를 보다 보니까 나중에 여러명이서 사용하는 원격 저장소를 쓸 때, 원격저장소에 한번 커밋되면 reset으로 삭제하면 안된다고 한다. 그래서 그때는 revert를 하라고 한다. revert에 대해서 알아봤다. 현재 커밋의 상태는 아래와 같다.

```bash
$ git bl
* 3f08fd3 (HEAD -> master) Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function
```

위에서 가장 최신 커밋인 `3f08fd3`를 revert하면 어떤일이 일어나는지 살펴보자.

```bash
$ git revert 3f08fd3

(여기서 vi가 열리고, Revert "..." 하는 커밋 메세지가 미리 적혀있다.)
(:wq 로 저장한다.)

[master 547d9a8] Revert "Squashed commit of the following:"
 2 files changed, 14 deletions(-)

$ git bl
* 547d9a8 (HEAD -> master) Revert "Squashed commit of the following:"
* 3f08fd3 Squashed commit of the following:
* 84bc79f Implement std function
* 9363da0 Implement ave function
* d36d6ce Implement div function
* fb38dae Implement mul function
* ea1c1cb Implement sub function
* e6679f9 Implement add function

$ git log -p
(...생략...)
```

그렇군. **revert는 커밋을 삭제하는게 아니라 특정 커밋에서 구현된 코드를 삭제하는 커밋을 생성하는 명령어 같다.** `git log -p`로 확인해 보면 `3f08fd3`로 구현한 코드가 삭제되어 있다.  이렇게 되면 원격저장소에 있는 커밋이 없이진게 아니니까 문제가 없어 보인다.

