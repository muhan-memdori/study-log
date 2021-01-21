# 익숙해지면 좋을 깃(git) 명령어 모음집

>  출처: https://urbanbase.github.io/dev/2021/01/15/GitCommand.html

모든 git 명령어 뒤에 --help 옵션을 사용하면 활용가능한 옵션을 알 수 있다.

___

### git status

git 프로젝트에서 파일들의 상태를 보여준다. git의 영역과 상태에 대해서는 다음 두 링크를 참고하는 것이 좋다.

- https://dololak.tistory.com/303
- https://dololak.tistory.com/304

> **git의 세가지 영역**
>
> ![](https://t1.daumcdn.net/cfile/tistory/99E5A03B5AD9520D38)
>
> - **git 디렉토리(.git 디렉토리, 깃 저장소)**
>   git 프로젝트의 모든 메타데이터와 객체 데이터베이스가 저장되는 곳이다. clone으로 원격 저장소를 복사해서 가져올 때, 이 .git 디렉토리를 만들고 원격 저장소의 모든 데이터를 복사해서 가져온다.
> - **워킹 트리(워킹 디렉토리)**
>   git 디렉토리에서 특정 버전을 checkout한 것이다. 
> - **Staging Area**
>   Staging Area이며 실제로 git 디렉토리에 파일의 형태로 존재한다. Index 영역이라고도 불린다. Staging Area는 워킹 트리에서 작업한 내용이 git 디렉토리에 commit 되기 전에 거쳐가는 공간이지만 이곳을 거치지 않고 바로 commit 될 수도 있다.

> **영역별 상태**
>
> ![](https://t1.daumcdn.net/cfile/tistory/999235365AD9AF5C04)
>
> 1. git 디렉토리에 존재하는 파일들은 committed 상태
> 2. git 디렉토리로부터 워킹 트리에 checkout하고 파일을 수정하고 staging area에 추가했다면 staged 상태이다.
> 3. git 디렉토리로부터 워킹 트리에 checkout하고 파일을 수정했지만 아직 staging area에 추가하지 않은 경우에는 modified 상태이다.

___

 ### git add

파일의 변경 내용을 스테이징 영역(staging area)에 추가하기 위해 사용하는 명령이며 스테이징 영역으로 추가된 변경 이력만 commit이 가능하다.

___

### git rm

파일을 지우거나 스테이지에서 해제할 때 사용한다.

```shell
`# 파일 삭제
git rm README.md

# README.md 파일을 추적하지 않은 상태로 만듦
git rm --cached README.md
```

___

### git restore(2.23)

워킹 트리(Working tree)의 변경된 파일을 복원해주는 역할을 한다.

```shell
# Unstaged 상태의 변경 파일을 원상 복구
git restore [파일명]

# git add로 Staging된 파일을 Unstaged 상태로 되돌림
```

___

### git clean

추적되지 않은 상태(untracked)의 파일을 삭제한다. 삭제가 되면 복구할 수 없으니 stash를 고려해보는 것도 좋다.

```shell
# 디렉토리를 제외한 파일만 삭제
git clean -f

# 디렉토리 포함 삭제
git clean -f -d

# .gitignore에 설정된 파일도 삭제
Git clean -f -d -x

# 가상 실행
git clean -n
```

___

### git commit

변경된 내용을 저장한다. 

```shell
# 메시지와 함께 커밋
git commit -m 'First Commit'

# 신규 파일을 제외한 변경사항을 Staging 후 커밋
git commit -a

# 이전 커밋 변경
git commit --amend
```

___

### git log

commit 목록을 볼 수 있다. git log --help를 통해 자신에게 맞는 옵션 조합을 활용해보자.

```shell
# branch 그래프를 추가하여 보기
git log --graph

# 모든 branch 보기
git log --all

# commit 메시지 제목만 한줄로 보기
git log --oneline
```

___

### git show

commit의 상세 정보를 확인한다.

```shell
# 현재 branch의 가장 최근 commit 정보를 확인
git show

# 특정 commit 정보를 확인
git show [commit 해시값]

# 특정 branch의 가장 최근 commit 정보를 확인
git show [branch명]
```

___

### git reset HEAD [file]

commit / push을 취소할 수 있다.

```shell
# commit을 취소하고 해당 파일들은 스테이징 영역에 보존
git reset --soft HEAD^

# commit을 취소하고 해당 파일들은 Unstaging
git reset --mixed HEAD^
git reset HEAD^

# commit을 취소하고 해당 파일들의 변경점 삭제
git reset --hard HEAD^

# push 취소
git reset HEAD^
git push -f origin 브랜치명
git pull
```

___

### git remote

원격 저장소 (remote repository)를 관리하는 명령어이다.

```shell
# 설정된 원격 저장소 보기
git remote -v

# test라는 이름으로 원격 저장소 추가하기
git remote add test https://gitHub.com/test/test
```

___

### git push

원격 저장소(remote repository)에 코드 변경분을 업로드한다.

```shell
# 기본 사용법
git push [저장소명] [branch]

# 최초 1회 저장소, branch 지정. 이후 생략 가능
git push -u [저장소명] [branch]

# 로컬에서 생성한 branch를 push
git push --set-upstream [저장소명] [branch]
```

___

### git branch

branch에 관련한 명령어이다.

```shell
# 로컬 branch 목록 확인
git branch

# 원격 저장소를 포함한 모든 branch 목록 확인
git branch -a

# test라는 branch 생성하기
git branch test

# test 로컬 branch를 origin이라는 원격 저장소의 test branch에 연결
git branch --set-upstream-to=origin/test test

# test branch 삭제
git branch -d test

# test branch 강제 삭제
git branch -D test
```

___

### git switch (2.23)

branch를 변경하는 명령어이다.

```shell
# HEAD를 test branch로 변경하기
git switch test

# test2라는 branch를 새로 생성하고 HEAD를 test2 branch로 변경하기
git switch -c test2
```

___

### git fetch

원격 저장소(remote repository)의 데이터를 가져온다. pull로 병합하기 전에 어떤 변경점이 있나 살펴볼 떄 사용하기 좋다.

```shell
# origin이라는 원격 저장소의 데이터를 가져옴
git fetch origin

# 모든 원격 저장소의 데이터를 가져옴
git fetch --all

# 원격 저장소에서 삭제된 branch를 로컬에서도 삭제
git fetch --prune
```

___

### git stash

현재 작업 중인 변경점을 임시 저장하거나 불러올 수 있다. 현재와 다른 branch로 가서 작업을 하기 전에 사용하면 유용하다.

```shell
# 현재 변경점 testStash라는 이름으로 저장
git stash save testStash

# stash 목록(stack) 확인하기
git stash list

# testStash라는 stash를 불러와 적용하기
git stash apply testStash

# testStash라는 stash를 불러와 Staged 상태까지 적용하기
git stash apply testStash --index

# 가장 최근의 stash를 가져와 적용하고 스택에서 삭제하기
git stash pop

# 가장 최근의 stash 제거하기
git stash drop

# testStash라는 stash 제거하기
git stash drop testStash
```

___

### git blame

특정 파일의 수정 이력을 확인할 수 있다. 각 라인별로 누가, 언제 마지막으로 수정했는지 알 수 있다.

```shell
# test.txt 파일의 수정 이력 확인
git blame test.txt

# test.txt 파일의 5부터 10번 라인까지만 확인
git blame -L 5, 10 test.txt

# 파일명이 변경 되었다면, 변경전의 파일명과 함께 확인
git blame -C newTest.txt

# 공백 변경을 무시
git blame -w test.txt
```

___

### git diff

소스를 비교하여 볼 수 있다.

```shell
# 마지막으로 커밋된 소스와 현재 Unstaged 상태의 변경점과 비교
git diff

# 마지막으로 커밋된 소스와 현재 Staging된 변경점과 비교
git diff --staged

# 커밋간 비교
git diff [커밋해시1]..[커밋해시2]

# branch간 비교
git diff [branch1] [branch2]
```

____

### git revert

지정한 커밋으로 되돌려 커밋한다. 되돌린 이력이 남기 때문에 충돌이 발생할 위험이 적으며 협업 상황에서 사용하는 것을 추천한다.

```shell
# 특정 커밋으로 되돌리고 커밋
git revert [커밋해시]

# 특정 태그로 되돌리고 커밋
git revert [태그명]

# 특정 커밋으로 되돌리지만 커밋은 안한채로 Staging 상태
git revert [커밋해시] -n

# 병합한 커밋으로 되돌릴 때 메인이 되는 커밋을 지정하여 되돌리기
git revert [커밋해시] -m 1
```

___

### git tag

특정 커밋에 표기하는 기능이다. 주로 릴리즈 시 이용한다. 두 가지 졸유가 있는데 Lightweight 태그와 Annotated 태그이다. 

- Lightweight 태그
  단순히 버전 등의 이름을 남길 때 사용한다. 
- Annotated 태그
  만든 사람의 이름, 이메일, 날짜, 메시지까지 저장하며, GPG(GNU Privacy Guard)로 서명까지 가능하다.

```shell
# 현재 HEAD에 v1.0.0이라는 Lightweight 태그 생성
git tag v1.0.0

# 현재 HEAD에 v1.0.0이라는 Annotated 태그 생성
git tag -a v1.0.0 -m 'message'

# 특정 커밋에 v1.0.0이라는 Lightweight 태그 생성
git tag v1.0.0 [커밋해시]

# v1.0.0 태그를 원격 저장소에 푸시하기
git push origin v1.0.0

# 모든 로컬 태그를 원격 저장소에 푸시하기
git push origin --tags

# 로컬의 v1.0.0 삭제
git tag -d v1.0.0

# 원격 저장소의 V1.0.0 태그 삭제
git push -d origin v1.0.0
```

___

### git merge

현재 브랜치를 특정 브랜치의 소스와 병합한다. merge 하기 전엔 워킹 디렉토리를 stash 등으로 깔끔하게 정리하고 진행하는 것을 추천한다.

```shell
# master branch를 병합
git merge master

# 병합 충돌(Conflict) 발생 시 취소
git merge --abort

# 공백으로 인한 병합 충돌을 무시하고 병합
git merge -Xignore-all-space
```