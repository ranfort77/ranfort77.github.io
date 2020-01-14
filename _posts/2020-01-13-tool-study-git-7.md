---
title: "git 공부 정리 (7): ssh-keygen"
excerpt: "버전관리 프로그램 git에 대해 공부한 기록"
categories:
  - tool
tags:
  - git
  - github
  - ssh
last_modified_at: 2020-01-14T14:21
---



## 머리말

`git push` 명령으로 github에 project를 동기화 할 때 매번 패스워드를 입력하지 않도록 github에 ssh public key를 등록하는 것과 접속을 테스트 하는 것 까지 살펴보았다.



## 참고

설정 과정은 Github의 Help 문서인 아래 링크를 그대로 따라하면 된다.

* [Connecting to GitHub with SSH](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)

그러나, 따라하더라도 내용은 알고 따라해야 한다. 위 문서를 읽으면 개인키 (private key) 와 공개키 (public key) 가 뭔지, passphrase가 뭔지, ssh-agent 설정은 왜 하는지 등의 궁금증이 생긴다. 

우선 아래 문서를 읽어보면 그냥 패스워드를 사용하는 것 보다 SSH key를 사용하는 것이 왜 더 안전한지, SSH key 인증 단계에 대한 설명, private key 보호를 위한 passphrase 에 대한 언급, 그리고 SSH key 만들기 및 public key 복사 등에 대해 알려준다. 

* [ssh 사용시 암호 대신 SSH key로 인증하기](https://arsviator.blogspot.com/2015/04/ssh-ssh-key.html)

그리고 아래 문서에서는 거의 비슷한 내용이지만, ssh-agent 를 왜 쓰는지에 대한 설명과 사용방법을 알려준다.

* [SSH Keygen을 이용한 키 생성 방법과 ssh-agent에 대한 간단 설명](https://devlog.jwgo.kr/2019/04/17/ssh-keygen-and-ssh-agent/)

* [ssh agent 이용해 private key 관리하기](https://yum3.tistory.com/69)

아래 문서도 읽으면 좋다.

* [SSH Key - 비밀번호 없이 로그인](https://opentutorials.org/module/432/3742)



다음 부터는 Github Help 문서의 내용을 실습해 볼 것이다.



## 실습 1. SSH key 생성

먼저 SSH key를 생성해 보자. 기존에 만들어 놓은 키가 없다면 홈디렉토리 (윈도우에서는 `C:\Users\[사용자명]` 이 홈디렉토리 역할을 한다) 에 `.ssh/` 폴더가 없을 것이다. 

우선 윈도우에서 git bash를 실행해서 터미널을 연다. 그리고 Github Help 문서에 있는대로 아래 명령어를 입력하면 된다.

```bash
$ ssh-keygen -t rsa -b 4096 -C "ranfort77@gmail.com"
```

그러면 아래와 같은 출력 결과가 나온다.

```bash
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/****/.ssh/id_rsa): <엔터>
Created directory '/c/Users/****/.ssh'.
Enter passphrase (empty for no passphrase): <엔터>
Enter same passphrase again: <엔터>
... 생략 ...
```

위 명령어에서 `-t`, `-b`, `-C` 은 옵션인데, `-t rsa`는 생성할 키 타입을 `rsa` 로 하고 `-b 4096`은 생성할 키의 bit 수를 `4096` 으로 `-C comment` 는 생성되는 공개키 파일 끝에 입력한 comment가 붙는데, 주로 이메일 주소를 적는 것 같고 comment를 다는 이유는 나중에 여러 개의 key 관리를 편하게 하기 위함이라고 한다. 위 옵션 없이 그냥 `ssh-keygen` 을 하면 디폴트 옵션이 적용되는데, `rsa` 방식, `2048` bit, 컴퓨터의 사용자이름@devicestring 형식으로 생성된다. (궁금하면 해 볼 것)

위 과정 중 키가 어디에 저장될지 묻는데 그냥 <엔터>를 입력하면 홈디렉토리에 `.ssh` 폴더가 생성되고 거기에 키 파일들이 생성된다. 그리고 passphrase를 입력하라는 메세지에서 역시 그냥 <엔터>를 누르고, 다시 한번 입력하라는 메세지에서도 그냥 <엔터>를 쳐서 일단은 passphrase를 사용하지 않도록 했다.

아래와 같이 `.ssh` 폴더 아래에 개인키 `id_rsa`, 공개키 `id_rsa.pub` 가 생성되었다.

```bash
$ cd .ssh/

$ ls
id_rsa  id_rsa.pub

$ cat id_rsa.pub
... 생략 ...

$ cat id_rsa
... 생략 ...
```



## 실습 2. Github 접속 실패 테스트

아직 Github에 공개키를 등록하지 않았기 때문에 접속 테스트를 했을 때 당연히 실패하지만, 성공했을 때와의 차이를 확인하기 위해 한번 해보고 넘어간다.

Github 접속 테스트는 아래 명령어를 입력해 보면 된다.

```bash
$ ssh -T git@github.com
```

그러면 아래와 같은 출력 결과가 나온다.

```bash
The authenticity of host 'github.com (***.***.***.***)' can't be established.
RSA key fingerprint is SHA256:...........
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
```

connecting을 계속할거냐고 물으면 yes 를 입력하면 된다. `Permission denied (publickey)` 메세지는 접속이 실패한 것이다.



## 실습 3. 공개키를 Github에 등록

우선 공개키 `id_rsa.pub` 의 내용을 `cat` 명령어로 출력하고, 내용을 복사한다.

```bash
$ cat id_rsa.pub
ssh-rsa ... 생략 ...
```

그 다음 아래와 같이 Github 에서 계정 프로필을 보는 곳에 "Settings" 를 클릭하자.

![](/assets/images/2020-01-14-sshkey/inseart_publickey_1.png)

그리고 아래 그림처럼 "SSH and GPG keys" 를 클릭하고 Title에 적당한 문자를 입력하고 Key에 복사한 공개키 값을 붙여 넣고 "Add SSH key" 를 클릭하면 Github 계정의 패스워드를 입력하면 된다.

![](/assets/images/2020-01-14-sshkey/inseart_publickey_2.png)



## 실습 4. Github 접속 성공 테스트

다시 한번 접속을 시도해 보면 아래와 같은 성공 메세지가 나온다.

```bash
$ ssh -T git@github.com
Hi ranfort77! You've successfully authenticated, but GitHub does not provide shell access.
```

지금부터는 `git push`를 할 때 매번 password 를 입력하지 않아도 된다.



## 실습 5. git push 테스트

정말 password를 입력하지 않아도 되는지 Github에 test repository를 하나 만들어서 테스트 해보자. 

우선 Github 메인화면에서 저장소 만들기에 대해 New를 클릭하여 아래 그림처럼 "Create a new repository" 화면으로 간다. 저장소명은 `test_gitpush` 로 만들었다. `Initialize this repository with a README` 를 체크해야 `git clone`으로 바로 컴퓨터로 저장소를 복제할 수 있는 것 같다.

![](/assets/images/2020-01-14-sshkey/test_gitpush_1.png)

저장소를 만들었으면 아래 그림처럼 `git clone`을 위한 HTTPS 주소를 복사한다. (SSH 주소를 복사해도 된다)

![](/assets/images/2020-01-14-sshkey/test_gitpush_2.png)

이제 터미널에서 (위치는 홈디렉토리에서) `git clone` 명령어로 저장소를 복제한다. 

```bash
$ git clone https://github.com/ranfort77/test_gitpush.git
Cloning into 'test_gitpush'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
```

그러면 `test_gitpush/` 폴더가 생성되어 있다. 저장소 안으로 들어가서 `ls -a`를 해보자.

```bash
$ cd test_gitpush

$ ls -a
./  ../  .git/  README.md
```

파일 두 개를 만들어 보자.

```bash
$ touch foo.txt bar.txt

$ ls -a
./  ../  .git/  bar.txt  foo.txt  README.md
```

저장소 상태를 확인한 후, 저장소 변경사항을 `git add`, `git commit`을 한다. 

```bash
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        bar.txt
        foo.txt

nothing added to commit but untracked files present (use "git add" to track)

$ git add .

$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   bar.txt
        new file:   foo.txt

$ git commit -m "Add foo.txt and bar.txt"
[master 81d3b5c] Add foo.txt and bar.txt
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 bar.txt
 create mode 100644 foo.txt
```

`git log` 명령으로 로컬 저장소의 master 의 커밋 이력과 원격 저장소의 master 커밋 이력이 다름을 확인하자.

```bash
$ git log
commit 81d3b5caa5d4caa6c498fd8356e4da606aab9845 (HEAD -> master)
Author: ranfort77 <ranfort77@gmail.com>
Date:   Tue Jan 14 02:03:06 2020 +0900

    Add foo.txt and bar.txt

commit c5ea1970695d9a05e5c2d045e43a1ecfe7e064c0 (origin/master, origin/HEAD)
Author: ranfort77 <48449384+ranfort77@users.noreply.github.com>
Date:   Tue Jan 14 01:56:54 2020 +0900

    Initial commit
```

이제 `git push` 테스트를 할 수 있다. 패스워드 없이 `push` 가 되는지 확인한다.

```bash
$ git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 282 bytes | 141.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/ranfort77/test_gitpush.git
   c5ea197..81d3b5c  master -> master
```

패스워드 없이 잘 된다. 다시 `git log`로 로컬저장소와 원격저장소의 커밋 위치가 동일한 것을 확인한다.

```bash
$ git log
commit 81d3b5caa5d4caa6c498fd8356e4da606aab9845 (HEAD -> master, origin/master, origin/HEAD)
Author: ranfort77 <ranfort77@gmail.com>
Date:   Tue Jan 14 02:03:06 2020 +0900

    Add foo.txt and bar.txt

commit c5ea1970695d9a05e5c2d045e43a1ecfe7e064c0
Author: ranfort77 <48449384+ranfort77@users.noreply.github.com>
Date:   Tue Jan 14 01:56:54 2020 +0900

    Initial commit
```

아래 그림처럼 Github 원격 저장소에 `foo.txt`, `bar.txt`가 올라갔음을 확인하자.

![](/assets/images/2020-01-14-sshkey/test_gitpush_3.png)



## 실습 6. passphrase 사용하기

참고 문서들에서도 언급되지만 개인키는 절대로 다른 사람에게 노출되면 안 된다. 특히 회사 또는 팀의 프로젝트의 관리자라면 개인키 보안에 더욱 더 신경을 써야 할 것이다. 여기서는 개인키가 사용될 때 password를 입력하도록 하는 passphrase 설정을 해 보자. 

실습 1에서 했던 것 처럼 SSH key를 새로 생성하면 중간에 passphrase를 입력할 수 있지만, 그러면 Github도 공개키를 다시 등록해야 하므로 그렇게 하지 말고 passphrase 변경 방법에 대해서 알아보자.  변경 방법은 [개인키 암호 변경](https://qastack.kr/server/50775/how-do-i-change-my-private-key-passphrase) 을 참고 하였다.

변경 하려면 개인키가 있는 `.ssh/` 폴더에서 아래 명령을 실행한다.

```bash
$ ssh-keygen -p -f id_rsa
```

`ssh-keygen --help`를 쳐서 keygen 의 헬프를 보면 알 수 있듯이 `-p` 옵션은 passprase를 변경한다는 의미이고, `-f` 옵션은 변경할 파일을 지정하는 옵션이다.

아래와 같이 개인키의 passphrase를 입력하고, 확인 입력까지 하면 된다.

```bash
Key has comment 'ranfort77@gmail.com'
Enter new passphrase (empty for no passphrase): <passphrase 입력>
Enter same passphrase again: <passphrase 입력>
Your identification has been saved with the new passphrase.
```

이제 github 접속 테스트를 한다.

```bash
$ ssh -T git@github.com
```

아래와 같이 passphrase를 입력하라는 문구가 나오고, 설정한 passphrase를 입력하면 접속에 성공한다.

```bash
Enter passphrase for key '/c/Users/****/.ssh/id_rsa': <설정한 passphrase 입력>
Hi ranfort77! You've successfully authenticated, but GitHub does not provide shell access.
```

참고 문서에서도 언급되어 있듯이 passphrase는 네크워크에 노출되지 않고 로컬컴퓨터에 저장된 개인키를 해석하는데 만 사용하기 때문에 해킹에 더욱 더 안전하다. 그리고 누군가가 자기 컴퓨터를 사용할 때 Github 내용을 변경하는 것도 막을 수도 있다.

반면 여기까지만 설정해 두면 또 다시 Github에 접속할 때마다 (`git push`를 할때마다) 아래와 같이 passphrase를 매번 입력해 줘야 하는 번거로움이 있다.

```bash
$ ssh -T git@github.com
Enter passphrase for key '/c/Users/ahs/.ssh/id_rsa': <설정한 passphrase 입력>
Hi ranfort77! You've successfully authenticated, but GitHub does not provide shell access.

$ ssh -T git@github.com
Enter passphrase for key '/c/Users/ahs/.ssh/id_rsa': <설정한 passphrase 입력>
Hi ranfort77! You've successfully authenticated, but GitHub does not provide shell access.
```



## 실습 7. SSH agent

passphrase를 매번 입력하는 것을 해결하기 위해 우선 ssh-agent 를 실행한다. 방법은 Github help 문서에 있는 `eval $(ssh-agent -s)`를 그대로 입력한다. (명령어의 의미는 아래 단락인 여담에서 고찰해 보기로 하고 우선 그냥 진행한다)

```bash
$ eval $(ssh-agent -s)
Agent pid 16792

$ ps
      PID    PPID    PGID     WINPID   TTY         UID    STIME COMMAND
    ... 생략 ...
    16792       1   16792      16792  ?         197609 13:21:17 /usr/bin/ssh-agent
```

위와 같이 명령어를 입력하면 ssh-agent의 process id가 출력되고 background 로 ssh-agent가 실행된다.

그리고 개인키를 ssh-agent에 추가한다. 

```bash
$ ssh-add ~/.ssh/id_rsa
Enter passphrase for /c/Users/****/.ssh/id_rsa: <passphrase 입력>
Identity added: /c/Users/****/.ssh/id_rsa (ranfort77@gmail.com)
```

그러면 지금부터는 passphrase를 ssh-agent가 관리하기 때문에 매번 passphrase를 입력하지 않아도 된다.

```bash
$ ssh -T git@github.com
Hi ranfort77! You've successfully authenticated, but GitHub does not provide shell access.

$ ssh -T git@github.com
Hi ranfort77! You've successfully authenticated, but GitHub does not provide shell access.
```

그러나 현재 실행 중인 터미널을 닫고 다시 터미널을 연 후 (나의 경우 윈도우10 Git Bash 터미널) Github 에 접속 테스트를 하면 다시 passphrase를 입력하라는 메세지가 나온다. 아무래도 ssh-agent가 터미널을 닫을 때 종료되는 것 같다.

위 문제에 대해서 Github Help 문서인 [Auto-launching ssh-agent on Git for Windows](https://help.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-git-for-windows) 에 해결 방법이 나온다. 우선 홈디렉토리에 (윈도우에서는 `C:\Users\[사용자명]`) `.profile` 파일을 만들고 아래 내용을 복사한 후 저장한다. (윈도우에서 `.bashrc`를 만들어서 사용하면 Warnning 메세지가 뜨고 `.bash_profile` 관련 메세지가 나온다. 자세한 설명은 생략한다. 그냥 `.profile`로 할 것)

```bash
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
```

위와 같이 설정해 두면 컴퓨터를 껐다가 켠 후, 터미널을 시작하면 아래와 같이 곧바로 passphrase를 입력하라는 메세지가 나오고 이때 한번만 입력해 놓으면 그 다음부터는 passphrase를 입력하지 않아도 된다. (터미널을 닫았다가 새로 열어도 입력하지 않아도 된다)

```bash
Enter passphrase for /c/Users/ahs/.ssh/id_rsa: <passphrase 입력>
Identity added: /c/Users/ahs/.ssh/id_rsa (ranfort77@gmail.com)

$ ssh -T git@github.com
Hi ranfort77! You've successfully authenticated, but GitHub does not provide shell access.
```



## 여담

위와 같이 이미 자동으로 ssh-agent가 실행되도록 설정해 두었기 때문에 더 이상 `eval $(ssh-agent -s)` 가 어떤 의미인지 파악하지 않아도 된다. 그러나 조사한게 있으니 나중에 잊어 버리지 않도록 여기에 기록해 둔다.

우선 `ssh-agent -s` 명령은 [ssh-agent man-pages](http://man7.org/linux/man-pages/man1/ssh-agent.1.html) 에서 알 수 있듯이 표준 출력에 ssh-agent의 Bourne shell 명령어를 생성하고 출력한다. 

그리고 bash에 익숙한 사용자라면 자주 사용하는 것 같은데 `$(command)` 는 안에 있는 command를 실행한 결과를 값으로 하는 것이다. 예를 들어 command를 실행한 결과를 var에 저장하려면 `var=$(command)` 를 사용한다. [What is the benefit of using $() instead of backticks in shell scripts?](https://stackoverflow.com/questions/9449778/what-is-the-benefit-of-using-instead-of-backticks-in-shell-scripts) 을 참고 할 것.

`eval` 명령은 뒤에 있는 인수 문자열을 명령문으로 해석하고 실행한다. 자세한 내용은 [eval](https://mug896.github.io/bash-shell/eval.html) 을 참고 할 것.

즉, `eval $(ssh-agent -s)` 는 `ss-agent -s` 명령이 출력하는 문자를 실행한다.