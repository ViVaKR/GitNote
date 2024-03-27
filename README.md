<!-- markdownlint-disable MD000 -->

# Git

## Github 회원가입 : [Github](https://github.com "Github")

1. 첫 저장소 생성 (Create new repository)
2. 생성한 저장소(Repository) 주소 복사 (`Copy` Created Repository URL: HTTPS, SSH or CLI)
3. 내 컴퓨터에서 가져올 폴더 루트로 이동 : 클론하면 해당 저장소(Repository)가 서브 디렉토리로 자동 생성됨
4. 생성한 저장소 가져오기 (Clone): $ git clone `<Repo URL Paste>`

## Git 설치 : [Git Download](https://git-scm.com "Git")

## Git 최초 환경설정

```bash
    git config --global user.name "<이름>"
    git config --global user.email "<이메일주소>"

    # true : 개행문자가 혼재할 경우 안전을 위해 이후의 처리를 중단함
    # warn : 개행문자가 혼재할 경우 경고를 출력하고 처리는 계속 진행함
    # false :  core.autocrlf 설정에 의해 변환  수행
    git config --global core.safecrlf false

    git config --global core.editor "code --wait"
    git config --global core.editor "mvim -f"  # using mac vim
    git config --global color.ui true
    git config --global --list # 전역설정 확인
    git add -h # Help

    # remove config global
    git config --global --unset core.excludesfile

```

## Git 저장소 생성

```bash
    mkdir domo-git      # 작업폴더 생성
    cd demo-git
    echo "# Demo Repo" >> README.md # README File 만들기
    git init .          # 초기화 .git 로컬 저장소 생성
    git add README.md   # or `$ git add .` 스태이징 에리어에 추적할 파일 추가
    git status          # git status -s (짤막하게 확인하기, --short)
    git diff            # 수정했지만 아직 staged 상태가 아닌 파일을 배교해 보기
    git diff --staged   # 저장소에 커밋한 것과 staging area 에 있는 것을 비교
    git rm file-name -f   # 실제 파일도 삭제됨
    git rm file-name --cached file-name  # staging area 에서만 제거하고 워킹에서는 지우지 않음
    git rm log/\*.log   # 패턴 사용으로 다수의 파일 또는 디렉토리 삭제하기
    git rm \*~          # ~ 로 끝나는 파일을 모두 삭제
    git mv README.md CHANGE.md     # 파일 이름 변경하기
    git commit -m "Add new file README"
    git remote add origin git@github.com:...? # 푸시할 원격 저장소 추가 (처음 한번)
    git remove -v # 모든 원격 리포지토리 보기
    git remote show origin #
    git clone https://github.com/ViVaKR/Git.git
    git log -p  # 커밋기록 및 변경사항 포함.
    git show commit-id # 특정커밋 표시
    git push -u origin main # 원격 저장소에 저장
```

## 흐름도

`Working Directory -> (git add) -> Staging Area -> (git commit) -> Commit History -> (git push) -> Remote Server`

## 용어

* Repository : 프로젝트가 보관되는 폴더의 위치로 저장소를 의미함
* Directory : 폴더 (Folder)

---

## 명령어

* clone : Github 에서 호스팅 된는 리포지토리를 내 컴퓨터의 현재 폴더로 가져옴.
* add : Git 에서 추가한 파일의 변경사항을 추척하도록 함
* commit : Git 에 파일을 저장함
* push : 커밋(Commit) 된 파일과 내용들을 Github 원격(Remote) 저장소(Repository)에 전송(Upload)합니다.
* pull : push 의 반대로 원격(Remote) 저장소(Repository)에서 내 컴퓨터 현재 프로젝트로 가져옵니다.

## ssh key 생성

```bash
    ssh-keygen -t rsa -b 4096 -C "이메일 주소"
    # Generatin pulblic/private rsa key pair.
    # Enter file in shich to save the key (/Users/../.ssh/id_rsa): testkey
    # Enter passphrase (empty for no passphrase) : (empty)
    # generated..
    ls | grep testkey
    # testkey (private key, 개인키)
    # testkey.pub  (public key, 공개키)
    cat testkey.pub
    # printed key string....
    # copy or
    pbcopy < ~/.ssh/testkey.pub # copy command
```

## Github SSH and GPG keys 에 등록하기

* Add new
* Add Title, Key (Paste to here)
* Add SSH key

## (MacOS) Add SSH key to the ssh-agent

```bash
    eval "$(ssh-agent -s)"
    # result -> Agent pid 74204
    ssh-add -K ~/.ssh/id_rsa
```

## 추적

```bash
    git add demo.txt # demo.txt 파일 변경사항 추적 시작
    git add .  # 모든 변경사항을 스테이지에 올림
    git add ./src # src 디렉토리의 모든 변경사항을 스테이지에 올림
    git add -p # 변경된 사항 하나하나 확인하면서 스테이지에 올림

    # Cancel `add` 취소
    git rm --cached <file>
    git rm --cached -r .

    # git add 취소 후 직전 버전으로 복원
    git reset Head --  # back to working directory
    git restore <file> # 복원
    git resotre --staged <file>

    # read git file
    cat `git ls-files | grep main-note`

    # Cancel `commit` 커밋을 취소하고 해당 파일들은 staged 상태로
    # 워킹 디렉토리에 보존
    git reset --soft HEAD^

    ## 커밋을 취소하고 해당 파일들은 unstaged 상태로
    # 워킹 디렉토리에 보존
    git reset --mixed HEAD^ # 기본옵션사항
    git reset HEAD^ # 위와 동일
    git reset HEAD~2 # 마지막 2개의 commit 취소

    # 커밋을 취소하고 해당 파일들은 unstaged 상태로
    # 워킹 디렉토리에서 삭제
    git reset --hard HEAD^
```

## 상태

```bash
git status
git status -s # 짤막하게 확인하기
git status -v # 변경사항 확인
```

## commit

## 제외, 무시

> `$ touch .gitignore` // 프로젝트 루트에 '.gitignore' 파일 생성
> `$ echo \*.log >> .gitignore` // 모든 log 파일 무시 예시

* 표준 Glob 패턴 사용, 프로젝트 전체에 해당됨

1. `.a` : 확장자가 .a 인 파일 전체 제외
2. `!lib.a` : lib.a 는 제외하지 않음
3. `build` : build/ 디렉토이에 있는 모든 파일 무시
4. `doc/\**/*.pdf` : doc 디렉토리의 모든 .pdf 파일 무시

* 기본예제 : [GitIgnore](https://github.com/github/gitignore)

## Refresh the indexes

```bash
    git rm --cached -r .
    git reset --hard
```

## 수정 후 staged 상태가 아닌 파일 비교

```bash
    git diff # Unstaged 상태인 것만 보여줌
```

## commit 전에 staging area 의 변경부분 확인

> $ git diff --staged

## 변경사항 커밋

> $ git commit -m "message note"
> $ git commit -a -m "staging area & commit"

## Gitignore Global

```bash

    # macos
    $ git config --global core.excludesFile '~/.gitignore'

    # windows
    $ git config --global core.excludesFile "%USERPROFILE%\.gitignore"
    $ git config --global core.excludesFile "$Env:USERPROFILE\.gitignore"
```

## Delete file or floder

```bash

    # 원격 저장소와 로컬 저장소에 있는 파일을 삭제
    $ git rm <fileName>

    # example
    $ git rm --cached .DS_Store

    # 원격 저장소의 파일만 삭제, 로컬 저장소 파일은 삭제하지 않음
    $ git rm --cached <fileName>

    # 폴더 삭제 #
    # 로컬 디렉토리와 git 저장소에서 모두 삭제
    $ git rm -rf 폴더명
    $ git commit -m "delete folder"

    # 로컬 디펙토리의 폴더는 유지하고 git 저장소에서만 폴더 삭제
    $ git rm --cached -r bin/

    # 모두 삭제
    $ git commit -m "delete folder"

    # Remove from .gitignore #

    # signle target
    $ git rm -r --cached <file or folder>

    # all target from .gitignore
    $ git rm --cached `git ls-files -i -c --exclude-from=.gitignore`

    # check git files
    $ git ls-files

    $ git commit -m 'Removed all files that are in the .gitignore'
    $ git push origin main
```

## 저장소 복제, Clone

```bash
$ git clone [깃링크](git@github.com:ViVaKR/GitNote.git)

## 로그
$ git log : 상태 확인
$ `git log -p` : 각 커밋의 diff 결과 표시
$ `git log -p -2` : 최근 두개의 결과만 표시, 즉 동료가 무엇을 했는지 확인가능
$ `git log --stat` : 각 커밋의 통계 정보 조회, 요약정보는 가장 뒤에 표시됨
$ `git log --pretty=oneline` : 각 커밋을 하나의 라인으로 보여줌
$ `git log --pretty=format:"%h - %an, %ar : %s"` : 결과를 포맷 일치 파싱

# format options :
# %H, %h, %T, %t, %P, %p, %an, %ae, %ad, %ar, %cn, %ce, % %cd, %cr, %s
# 저자(Author) : 원 작업을 수행한 사람
# 커미터(Committer) : 마지막으로 이 작업을 적용한(저장소에 포함시킨) 사람
# (ex) 어떤 프로젝트에 패치를 보냈고 그 프로젝트의 담당자가 패치를 적용했다면 두 명의 정보 중
# 당신이 저자이고 담당자가 커미터가 됨

```

## 되돌리기 (Undo)

* 완료한 커밋을 수정해야 할 때
* 너무 일찍 커밋, 파일누락, 커밋 메시지를 수정할 때
* 주의 : 한번 되돌리면 복구할 수 없음

```bash

    # 가장 최근의 커밋을 수정하고 변경 내용을 추가
    $ git commit --amend

    # 준비되지 않은 변경사항 되돌리기
    $ git checkout filename

    # 마지막 커밋 롤백
    $ git revert HEAD

    # 이전 커밋을 롤백
    $ git revert commit-id

```

## Restore

```bash
    git add .

    # 파일상태를 Unstage 로 변경하기
    git restore --staged README.md
    git reset HEAD <file>
```

## branch

```bash
    # 새로운 분기 생성
    git branch branch-name

    # 새로우 분기로 전환
    git checkout branch-name

    # 분기 생성 후 즉시 전환
    git checkout -b branch-name

    # 분기 목록
    git branch

    # 분기삭제
    git branch -d branch-name

    # 원격 분기 이름표시
    git branch -r
```

## log

```bash
    git log
    git log --stat
    git log --oneline

    git log -p -2   # -p: 각 커밋의 diff 결과, -2: 최근 두개의 결과

    # 어떤 파일이 수정/변경/추가 또는 삭제 되었는지에 대한 요약정보
    git log stat
    # oneline : 각각의 커밋을 한 라인으로 보여줌
    git log --pretty=oneline

    # format : (형식지정) 67da7a9 - ViVaKR, 8분 전 : Update README [ETC]
    # %H : 커밋해시
    # %h :
    # %T :
    # %t : 짧은
    # %P : 부모 해시
    # an : 저자 이름
    # ae : 저자 메일
    # %ad : 저자 시각 (형식은 –-date=옵션 참고)
    # %ar : 저자 상대적 시각
    # %cn : 커미터 이름
    # %ce : 커미터 메일
    # %cd : 커미터 시각
    # %cr : 커미터 상대적 시각
    # %s : 요약
    # 저자 (Author) : 원래의 작업을 수행한 원작자
    # 커미터 (Committer) : 마지막으로 이작업을 적용한 (저장소에 포함시킨) 사람
    git log --pretty=format:"%h - %an, %ar : %s"
    git log --pretty=format: "%h %s" --graph

    # -p : 각 커밋에 적용된 패치를 보여줌
    # --stat : 각 커밋에서 수정된 파일의 통계정보
    # --shortstat : --stat 명령의 결과 중에서 수정한 파일, 추가된 라인, 삭제된 라인만
    # --name-only : 커밋 정보중에서 수정된 파일의 목록만 보여줌
    # --name-status : 커밋 구분을 보여줌 (추가, 수정, 삭제 여부)
    # --abbrev-commit : 체크섬의 처음 몇 자만 보여줌
    # --relative-date : 상대적인 시간을 보여줌
    # --graph : 브랜치와 머지 히스토리 정보를 아스키 그래프로 보여줌
    # --pretty : online, short, full, fuller, format
    # --oneline : --pretty=oneline, --abbrev-commit 두옵션을 함께 사용하는 것
    # -(n) : 최근 n 개의 커밋만 조회
    # --since, --after : 명시한 날짜 이후의 커밋만 조회
    # --until, --befor : 명시한 날짜 이전의 커밋만 조회
    # --author : 입력한 저장의 커밋만 보여줌
    # --committer : 입력한 커미터의 커밋만 보여줌
    # --grep : 커밋 메시지 안의 텍스트를 검색
    # -S : 커밋 내용안의 텍스틀 검색
    git log --since=2.weeks  # 지난 2주간

    # 커밋 로그를 그래프로 표시
    git log --graph --oneline
    git log --graph --oneline --all
```

## Reset

```bash
    git reset HEAD filename
    git reset HEAD -p

    # 해당 시점으로 롤백 이후 작업 모두 삭제
    git reset --hard <uuid>

```

## merge

```bash
    git merge --abort
    git merge origin/main
    git remote update
```

## rebase

```bash
    git rebase branch-name

    # 대화식 실행
    git rebase -i main
```

## ETC

```bash
    git push -u origin branch-name
    git push --delete origin branch-name
    git push -f
```

## [Git Ignore (.gitignore) Example](https://git-scm.com/docs/gitignore)

* 파일 또는 폴더
    file-or-folder

* 폴더와 그안의 내용
    folder/

* 폴더 내부의 info.txt 와 .sec 파일들
    folder/info.txt
    folder/\*.sec

* 특정 확장자만 허용
    !not_ignore_this.cs

* 폴더 내부 또는 서브 폴더의 db.bak 파일
    folder/\*\*/db.bak

## 과거로 돌아가기

    >- git log 또는 reflog 명령을 사용하여 되돌릴 커밋의 ID 를 찾기

    >- reset
        - 이전 기록을 삭제하는 방식으로 회귀,
        - 특정 커밋 까지 모두 취소함

    >- revert
        - 이전 기록위에 추가하는 방식으로 회귀
        - 복원 기록 자체를 기록으로 남길 때
        - 해당 커밋의 작업만 취소하고 새로운 커밋을 생성함.
        - 협업시 한번 공유된 기록이 있을 시 사용함

    >- restore :
        - git restore --staged hello.c #-> 수정 원본은 복원하지 않음, Add 만 취소.
        - git restore --worktree hello.c #-> 수정 원본을 복원함 Add 하기 전.
        - git restore --source=HEAD --staged --worktree hello.c # 커밋하기 전에 원상복구 하기
        - git restore -W@ -SQ hello.c
        ________                          ______________________                ________________
        | HEAD |  <-- restor --staged --> | STAGE AREA (Index) | <-- restor --> | Working TREE |
        --------                          ----------------------                ----------------
                  <-------(commit)                               <-------(add)
                                                 ^                                             ^
        |----------------------------------------|---------------------------------------------|
                                        restore --staged --worktree

    >- git checkout
        -
        -

    >- git switch
> Commands

```bash

git reflog

git reset --hard HEAD@{돌아갈 번호}
git reset --hard <uuid>

git reset HEAD@{1}
git reset f48441c33

```

> 테스트 커밋 작성 후 Revert 실행

```bash

$ touch alpha.html
$ git add . && git commit -m "1st git commit: file 1"

$ touch beta.html
$ git add . && git commit -m "2nd git commit: files 2"

$ touch chrlie.html
$ git add . && git commit -m "3rd git commit: files 3"

$ touch delta.html
$ git add . && git commit -m "4th git commit: files 4"

$ touch elis.html
$ git add . && git commit -m "5th git commit: files 5"

$ ls

$ git reflog
$ git revert HEAD@{2}

>-> deleted 'charlie.html' file, other files is not deleted

```

    >- e.g. before reset -> HEAD@{6}

```bash
f86c30d (HEAD -> main) HEAD@{0}: commit: modify main-note for test
0e3de9b HEAD@{1}: reset: moving to HEAD
0e3de9b HEAD@{2}: commit: Add main-note
186934f (origin/main, origin/HEAD) HEAD@{3}: reset: moving to HEAD
186934f (origin/main, origin/HEAD) HEAD@{4}: reset: moving to HEAD
186934f (origin/main, origin/HEAD) HEAD@{5}: commit: After unset and re commit
9ba11eb HEAD@{6}: commit: Create new main-note

$ nl main-note
     1	1 3 5 7 9 11 13 15 17 19
     2	git reset HEAD --
     3	      2월 2024
     4	일 월 화 수 목 금 토
     5	             1  2  3
     6	 4  5  6  7  8  9 10
     7	11 12 13 14 15 16 17
     8	18 19 20 21 22 23 24
     9	25 26 27 28 29
    10
    11	---------------------

$ git reset --hard HEAD@{7}

>- results
◯ ⭄ git log --pretty=oneline
4c28f3327588c0f21d2486f2848c98e6a3719d0a (HEAD -> main) Modify note add line seq command
347552d5a16142e2456c30579f742d885b7e8d0e Test Commit
9aca4403e79fc763befaa5f660b24b0b3639a1ec Create Temp
3bf99870b9f11ab8100eec79b8e5b1f279415265 Create demoA
ca1843f8491c613c452810b76bbb694e1253a32c Create demoB
cbf73d85e0d57229f7974d6076ac44961568f3a8 Rebase demo

$ nl main
◯ ⭄ nl main-note
nl: main-note: No such file or directory

>- reset 으로 -> 파일도 삭제되었음..

>- 백업 본으로 원상복구 하기
$ git reset --hard


>-  Revert : 해당 커밋만 새로 추가
$ git revert c571e7318ec04a745202b548c5d5e01a1ee64e99 # 자동 커밋
$ git revert --no-commit <uuid>

>- result
(to added new commit) --> 87bd6f0cea319330278fe5619759d5b1dcbe3f97 (HEAD -> vivmac) Revert "rollback demo"
5f3ec3e42d152af55ecb69b0a9d1bd7c8bf01c39 Create temp.log and move to temp.txt
a2341dc0c15a4e2857208b3e2eac11867bc25121 (origin/vivmac) Create foler fd and touch files
10b234ad509e93425b6c057257fb679b80008ba5 Create new main-note
(from) --> c571e7318ec04a745202b548c5d5e01a1ee64e99 rollback demo
0ef41fdd822f1dd5604b93095926a1981755bc47 modified README

>- 충돌 발생시 해결 후 -> $ git revert --continue

>- 다시 깨끗하게 복구 -> $ git reset -hard <uuid>
```
