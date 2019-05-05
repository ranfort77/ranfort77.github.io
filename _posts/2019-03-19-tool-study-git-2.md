---
title: "git 공부 정리 (2): git 명령어 모음"
excerpt: "버전관리 프로그램 git에 대해 공부한 기록 두 번째"
categories:
  - tool
tags:
  - git
  - github
  - jekyll
last_modified_at: 2019-03-20T00:30
---

git 명령어 중 내가 자주 사용할지도 모르는 명령어와 옵션의 조합을 정리했다. 나중에 읽어볼 명령어 모음이 있는 블로그 링크도 남겨둔다.

* [Git 명령어 정리](https://blog.outsider.ne.kr/572)

## 초기화 (init) 및 설정 (config)

- `git init` *or* `git init .`: 현재 폴더에 repository를 생성
- `git init <polder>`: 지정한 폴더에 repository를 생성
- `git config --list`: 현재 설정을 보여준다.
- `git config --global user.name <name>`: 사용자 설정
- `git config --global user.email <email>`: 사용자 이메일 설정

## 상태 (status)

- `git status`: 현재 저장소의 상태를 보여준다.
- `git status --ignored`: 현재 저장소 상태 뿐 아니라 .gitignore 파일에 정의된 무시되는 파일까지 보여준다.

## 무시 (.gitignore 파일)

* [.gitignore 정의](https://statkclee.github.io/git-novice-kr/06-ignore/): working tree에 .gitignore 파일을 만들고, 규칙에 따라 git이 무시할 파일들을 정의하면 된다.

## 추적 및 인덱스에 추가 (add)

- `git add .`: 현재 폴더에 있는 모든 파일을 추적(tracked)하며 인덱스에 파일을 내용을 올린다. 서브폴더에 있는 모든 폴더의 파일들 역시 재귀적으로 추적하고 인덱스에 내용을 올린다. 이미 추적되고 있는 파일은 그냥 인덱스에 내용을 올린다.
- `git add <file>`: 지정한 파일에 대해 위 내용을 실행
- `git add <polder>`: 지정한 폴더의 모든 파일들에 대해 내용을 실행

## 추적 해제 (rm --cached), 인덱스 내용 회귀 (reset HEAD)

- `git rm --cached -r .`: 현재 폴더에 모든 파일을 추적해제(untracked)하며 서브폴더에 있는 모든 폴더의 파일들 역시 재귀적으로 추적해제. 인덱스 내용에는 관여하지 않는다.
- `git rm --cached <file>`: 지정한 파일에 대해 추적해제
- `git rm --cached -r <polder>`: 지정한 폴더에 모든 파일을 추적해제하며 서브폴더에 있는 모든 폴더의 파일들 역시 재귀적으로 추적해제
- `git reset HEAD <file>`: 이 명령어는 working directory에서 파일을 수정하고 `git add <file>`를 한 후 `git status`를 하면 소개해준다. 지정한 파일에 대해 unstage를 하려면 사용하라고 나온다. 이 명령은 git Pro책의 [Git 도구 - Reset 명확히 알고 가기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0)에 "경로를 주고 Reset하기"에 자세한 내용이 나온다. 그리고 이 명령은 `git reset --mixed <현재 HEAD의 commit-id> <file>`를 간단히 쓴 것으로 정확히 `git add <file>`의 반대 동작이다.

## 커밋 (commit)

- `git commit`: 자동으로 에디터가 열리고 커밋 메세지를 작성한 후 저장하면 커밋이 된다.
- `git commit -a`: 추적되고 있는 파일(한번 add한) 모두를 add 한 후, 커밋
- `git commit -m <"message">`: 커밋과 함께 커밋 메세지를 입력
- `git commit -am <"message">`: 위 둘을 합친 것

## 커밋 로그 (git log)

- `git log`: 커밋의 히스토리를 보여준다.
- `git log -p`: 각 커밋의 파일 간의 차이를 보여준다.
- `git log --reverse`: 커밋을 역순으로 보여준다.
- `git shortlog`: 사용자별로 묶어서 로그를 간단히 보여준다.
- `git log --oneline`: 커밋 메세지의 한줄짜리 타이틀만 보여준다.

## 커밋 되돌리기 (reset, checkout, revert)

- `git reset --hard <commit-id>`: 지정한 커밋으로 working tree, index, HEAD를 모두 되돌린다.
- `git reset --hard HEAD~`: HEAD의 한단계 전 조상 커밋으로 working tree, index, HEAD를 모두 되돌린다.
- `git reset --hard ORIG_HEAD`: ORIG_HEAD가 가리키는 커밋의 내용으로  working tree, index, HEAD를 모두 되돌린다.
- `git reflog`: HEAD가 가리키는 커밋이 바뀔때마다 저장되는 이력을 보여준다.
- `git checkout <commit-id>`: 지정한 commit-id로 HEAD를 이동시킨다. 'detached HEAD' state가 된다.
- `git revert <commit-id>`: 지정한 커밋의 구현 코드를 지우는 커밋을 생성

## 커밋 변경 (commit --amend)

- `git commit --amend`: 바로전의 커밋 메세지를 수정하며 커밋id가 바뀐다. 이 명령어는 아래 두 명령어를 연속해서 사용한 것과 같다.  
`git reset --soft HEAD~`  
`git commit`  
따라서 `git commit --amend`를 하기전에 working tree의 내용을 변경한 후, 그 내용을 index에 올리고 `git commit --amend`를 하면 커밋의 메세지 뿐 아니라 내용도 변경이 된다.

## working tree 변화를 확인하는 몇 가지 방법 (diff)

- `git diff`: 아직 index되지 않은 working tree의 변화 출력. 아마 현재 index의 내용과 working tree의 차이를 출력하는 것 같다. 즉, working tree의 수정된 파일을 index에 올리면 `git diff`는 아무 내용도 출력하지 않는다.
- `git diff --cached`: 현재 index와 마지막 커밋 비교. "-a"옵션 없이 "git commit" 할때 커밋되는 내용을 보여준다.
- `git diff HEAD`: 현재 working tree의 변화와 마지막 커밋 비교. "git commit -a"할때 커밋되는 내용을 보여준다.
- `git diff <commit-id1>..<commit-id2>`: 두 커밋의 차이 비교

## git object 내용 확인 방법 (show)

- `git show <object-hash>`: 지정한 hash 객체의 내용을 보여준다. git object에는 commit, tree, blob가 있는 것 같다.

## 브랜치 확인 및 생성 (branch)

- `git branch`: 현재 존재하는 브랜치 확인
- `git branch <branch-name>`: 지정한 브랜치 생성

## 브랜치 이동 (checkout) 및 생성 (checkout -b)

- `git checkout <branch-name>`: 지정한 브랜치로 이동
- `git checkout -b <branch-name>`: 지정한 브랜치를 생성하고, 그 브랜치로 이동

## 브랜치 삭제 (branch -d or -D)

- `git branch -d <branch-name>`: 브랜치를 삭제. 그러나 병합하지 않으면 삭제가 안된다.
- `git branch -D <branch-name>`: 병합하지 않은 브랜치 강제 삭제

## 브랜치 로그 (log) 및 비교 (diff)

- `git log --branches --oneline`
- `git log --branches --decorate --graph`
- `git log --branches --decorate --graph --oneline`
- `git log --branches --decorate --graph --all`
- `git log <branch-name1>..<branch-name2>`
- `git log -p <branch-name1>..<branch-name2>`
- `git diff <branch1-name1>..<branch-name2>`

## 브랜치 병합 (merge)

- `git merge <branch-name>`: 현재 위치한 브랜치에 지정한 브랜치의 내용을 병합

## 리베이스 (rebase)

- `git rebase <branch-name>`: 현재 브랜치의 커밋을 지정한 브랜치로 rebase

## 임시 저장 (stash save) 및 복구 (stash apply)

- `git stash save`: 커밋되지 않은 working tree와 index의 내용을 저장
- `git stash apply`: 저장한 working tree와 index를 복구
- `git stash list`: stash 저장 이력을 보여준다.
- `git stash drop`: stash 저장 이력을 최신부터 하나씩 삭제
- `git stash pop`: apply와 drop이 한번에 된다.

## 태그 (tag)

- `git tag`: 현재 태그 확인
- `git tag -v <"tag">`: 지정한 태그의 내용 출력
- `git tag <"tag">`: 현재 HEAD가 가리키는 branch의 최신 커밋을 태그 (light-weight tag)
- `git tag <"tag"> <commit-id>`: 지정한 커밋을 태그
- `git tag master`: 마스터 브랜치를 태그
- `git tag -a <"tag"> -m <"message">`: annotated tag 생성
- `git tag -d <"tag">`: 태그 삭제
- `git checkout <"tag">`: 태그로 이동
- `git push --tags`: `--tags` 옵션을 안 넣으면 태그는 원격저장소로 업로드 되지 않는다.

## 원격 저장소 (remote, clone, push, pull, fetch)

- `git remote`: 원격 저장소 확인
- `git remote -v`: 원격 저장소 확인
- `git remote remove origin`: 원격 저장소 삭제

* `git clone <remote-repo-address> <polder>`: 저장소 히스토리 포함 복제

- `git push --set-upstream origin master`: master 브랜치를 업로드
- `git push -u origin master`: `--set-upstream` 옵션과 같다.
- `git push`: 원격 저장소 업로드

- `git pull`: `fetch and merge` 와 같다.
- `git fetch`
- `git merge`

## 별칭 (alias)

* `git config --global alias.<alias> "option1 option 2 ..."`: ex) git config --global alias.bl "log --branches --oneline --graph"

## 그 밖의 명령어

* `git checkout -- <file>`: 커밋이 완료된 후, working tree의 특정 파일을 조금 수정했는데, 그 수정한 것을 다 삭제하고 싶을 때(조금 위험. 신중히.)
  
* 백업 저장소 또는 원격 저장소 설정, 생성, 삭제
    - `git init --bare <polder-path>`: 백업용 로컬 저장소를 초기화. 커밋이 안됨.
    - `git remote add origin <polder-path>`: 지정한 폴더를 원격 저장소로 하고 origin이라는 별칭을 짓는다.
* 알아봐야 하는 것
    - `git blame`

