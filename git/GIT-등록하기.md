# Git 시작하기 - 20210115

### Git 설정

```git
$ git --version
$ git config --global user.email hksoftcorn.dev@gmail.com
$ git config --global user.name hksoftcorn
$ git config --global --list
```

### Git 시작하기

> 관리 폴더로 들어가기 -> 여기서 Git-bash 실행하기

```git
$ git init
$ git status
$ git add . 
```

### .gitignore 추가하기

> vscode에서 .gitignore 파일 생성하기

[gitignore.io](https://www.toptal.com/developers/gitignore) -> windows, macOS, VisualStudioCode, python

.gitignore 파일에 생성 결과 붙여넣기

```git
$ git commit -m "first commit"
$ git status
$ git log
```

### Github repository 생성하기

> main -> master(이전 버전)으로 변경합니다. `setting - repositories`

```git
$ git remote add origin https://github.com/hksoftcorn/practice.git
$ git remote -v
$ git push -u origin master
```

> repo 삭제하기 : 레포지토리 - setting - 맨아래 delete

> 로컬에 저장된 기존 GitHub 아이디 삭제 : 웹 자격 증명 관리 -> Windows 자격 증명

### README.md 작성

> README 파일을 작성합니다. -> 해당 디렉토리에 저장합니다.

```GIT
$ git status
$ git add .
$ git commit -m "add README.md"
$ git push
```

### Git Clone

> github에서 code -> HTTPS 복사

`git clone https://github.com/hksoftcorn/practice.git {폴더명}`

