# CLI 

> 2021.04.20.

[CS50 IDE](https://ide.cs50.io/) 하버드 컴퓨터공학 개론 수업에서 제공하는 통합 개발 환경입니다. Ubuntu 운영체제 위에서 구동됩니다. 여기서 IDE의 의미는 통합 개발 환경이라는 뜻입니다.

## 운영체제

- Window 계열
  - windows...
- Unix 계열
  - Unix : 유료형
    - `MacOS`, OSX
  - Linux : 무료형, 오픈 소스
    - Ubuntu, CentOS, 

> Unix 기초 구동 프로그램 shell 
>
> - sh : shell
> - `bash : bourne again shell`
> - zsh : z shell
>
> $ : command 준비완료!



## 기초 명령어

bash를 확인합니다.

```bash
$ which bash

/usr/bin/bash
```

### 1. 기본 입출력

hello world를 출력해봅시다!

```bash
$ echo 'hello world'
```

manual을 봐봅시다!

```bash
$ man mkdir
```

> 📌 Unix 이동 꿀팁
>
> ctrl + a : 맨 앞으로
>
> ctrl + e : 맨 끝으로
>
> ctrl + b : 한 칸 뒤로
>
> ctrl + f : 한 칸 앞으로
>
> ctrl + w : word 단위로 지우기



### 2. 기본 조작

`echo` -> hello world를 greeting.txt에 작성합니다. 덮어쓰기만 가능합니다. 'w' rewrite 기능입니다.

```bash
$ echo 'hello world' > greeting.txt
```

```bash
$ echo 'bye bye' > greeting.txt
```

 greeting.txt에 이어서 작성합니다. append 기능입니다. 

```bash
$ echo 'hello world' >> greeting.txt
```

`ls` 기능을 알아 봅시다. 옵션으로 `a` : 숨김파일 보이기, `l` : 상세 정보 보이기, `t` : 시간 순으로 보이기

```bash
$ touch not_hidden.txt .hidden.txt
$ ls -alt
```

`cp` 기능은 {원본}을 {주소}를 지정하여 {복사이름}까지 지정하여 복사합니다.

```bash
$ cp greetings.txt ./copy_greeting.py
$ cp original/original.txt copy/
```

`rm` 기능은 remove

```bash
$ rm -f original.txt greeting.txt
```

`mv` 기능은 move 또는 rename !!

```bash
$ mv original/README.md .
$ mv README.md hello.md
```

> why 줄여서 쓸까??
>
> - UNIX 운영체제에서 시간 분할 메모리 할당을 채택
> - 명령어 작성 시 끊겨서 보임
> - full name 작성시 답답해 죽음



### 3. 응용 조작

`curl`, `wget`

```bash
$ curl https://eduyu.github.io/files/sonnets.txt
$ curl https://eduyu.github.io/files/sonnets.txt >> sonnets.txt
```

`cat`, `head`, `tail`

```bash
$ cat sonnets.txt
$ head sonnets.txt
$ tail sonnets.txt
```

`wc` : 줄, 단어, 글자 수 정보 파악

```bash
$ wc sonnets.txt
$ head sonnets.txt | wc
```

```bash
$ ping google.com
$ ping google.com > google.log
$ tail -f google.log
```

`less` : q로 종료하는 이유! 

```bash
$ less sonnets.txt

# rose를 검색합니다.
/rose
n -> next
shift n -> previous
u, d -> up, down
q -> quit
```

`grep` : global regular expression and print

```bash
$ grep rose sonnets.txt
$ grep rose sonnets.txt | wc
$ grep -i rose sonnets.txt | wc
```

grep 활용 

```bash
$ pip list | grep -i 'django'
$ ps aux
$ ps aux | grep 'runserver'

# pid -> 프로세스 아이디 확인
# 프로세스 강제 종료
$ kill -9 2836
```

`find`

```bash
$ find . -name '*.txt'
$ find . -name '0*.py'
```

`꿀팁`

```bash
$ git add . && git commit -m 'hello' && git push origin master
$ mkdir -p my_app/templates/my_app
$ cd -
$ rm db.sqlite3 my_app/migrations/0*
```

```bash
$ python manage.py runserver

# 이전 명령어 탐색
ctrl + r
run
$ python manage.py runserver
```

`alias`

```bash
# .bashrc  -> bash run command
# .bash_profile
$ ls .bash*
$ cat .bashrc

echo -e "import sys; sys.stdin = open('input.txt', 'r')" >> sol$1.py
```

##### *편집기 : vim 쓰세요

















