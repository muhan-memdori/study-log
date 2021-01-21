# Git 상태 확인하기 - Git Status 명령어 및 상태 파헤치기

> 출처 : https://dololak.tistory.com/304

## Git에서 세가지 영역

git 프로젝트 디렉토리에는 .git(git 디렉토리)를 포함하여 프로젝트를 구성하는 수많은 파일들이 존재한다. git 디렉토리는 git 프로젝트에서 작업한 수많은 정보들과 여러 버전에 대한 실제 데이터들을 저장하는 데이터베이스이며, 그 외의 데이터들은 git 디렉토리에서 특정 버전의 데이터들을 checkout한 것이다.

이 때 우리는 checkout하여 가져온 버전의 파일들로 프로젝트 작업을 수행하며, 이 파일들이 존재하는 곳을 워킹트리 또는 워킹디렉토리라고 한다.

![](https://t1.daumcdn.net/cfile/tistory/99A883425AD9AF5B0F)

- 워킹 디렉토리 - 프로젝트를 진행하는 실제 작업 공간으로 개발한 소스 및 자원이 존재하며 이곳에서 파일을 수정 및 추가한다.
- Staging Area - 워킹 디렉토리에서 작업한 내역을 git 디렉토리로 커밋하기 위해 커밋 대상 목록으로 담아두는 장바구니 목록 같은 영역
- git 디렉토리 - 실제로는 .git 이라는 이름의 디렉토리이며, 여러가지 버전의 커밋 데이터들과 git 프로젝트에 대한 모든 정보를 담고있는 핵심 데이터베이스 디렉토리이다.

___

## git 프로젝트에서 파일의 여러가지 상태

### Untracked와 Tracked 상태

워킹 디렉토리에 있는 여러가지 파일들은 git 추적 관리 여부에 따라 두 가지 상태로 나눌 수 있다. 

- Tracked
  git이 해당 파일을 추적 및 관리하는 상태로, 최소한 한번은 git add 명령을 통해 staging area에 포함되거나,  commit 을 통해  git 디렉토리에 저장된 파일이다.
- Untracked
  git이 해당 파일을 추적 및 관리하지 않는 상태

이는 워킹 디렉토리에 존재하는 파일이라고 해서 모두 Git이 관리하는 파일은 아니라는 뜻이기도 하다. git 관리하지 않는 파일은 어떤 이유로 잃어버려도 복구가 불가능하다. 대신 이러한 추적관리 시스템을 이용하면 tracked 상태가 아닌 불필요한 파일들을 커밋하게 되는 실수를 방지할 수 있다.

###  Unmodified와 Modified 상태

Untracked와 Tracked 추적 관리 여부 관점에서 바라본 상태였다면, 파일의 변경 여부에 따라  Modified(변경 발생)과 Unmodified(변경 없음) 상태로 나눌 수도 있다.

- Modified
  파일이 Staged 또는 Commit된 시점 이후로 변경 여부가 존재하는 상태
- Unmodified
  파일이 Staged 또는 Commit된 이후로 하나도 변경되지 않은 상태

Untracked 파일은 두 가지 변경 관련 상태를 갖지 않으며, Unmodified 또는 Modified 상태의 파일은 곧 Tracked 파일이다.

### Staged 상태

- Staged
  Staging Area에 내역을 등록한 상태
- Not staged
  파일이나 최근 변경 사항을 Staging Area에 내역을 등록하지 않은 상태로, 파일이나 파일의 변경사항을 아직 Staging Area에 등록하지 않았거나, 이미 commit하여 git 디렉토리로 변경사항이 들어간 상태에 해당한다.

___

## git status -s를 통해 상태 간략히 확인하기

```shell
$ git status -s
?? firstFile.txt
A secondFile.txt
 M thirdFile.txt
M fourthFile.txt
MM fifthFile.txt
```

-s, --short 옵션을 주게 되면 git status 명령만을 사용했을 때 나왔던 상태 메시지들을 두 자리의 문자로 표현한다. 앞자리는 staging area 기준, 뒷 자리 문자는 워킹 디렉토리 기준이다. 

| **문자** | **설명**                                                     |
| -------- | ------------------------------------------------------------ |
| ??       | 파일 자체가 새로 추가되어, Untracked인 파일                  |
| A        | 새로운 파일이 추가된 후 git add 명령을 통해 Staged 상태가 된 파일 |
| M        | 워킹 디렉터리의 파일을 수정(Modified)을 했지만 git add 하지 않은 not Staged 상태를 말합니다. (Modified, not Staged) |
| M        | 워킹 디렉터리에서 파일을 수정하여 Modifed 상태인 파일을 git add로 Staging Area에 등록한 Staged 상태를 말합니다. (Modifed, Staged) |
| MM       | 수정 후 git add 상태에서 Commit 하지 않고 다시 한 번 수정 후 git add 하지 않은 상태로 첫번째 수정 내역은 Staged 상태이지만, 다시 한 번 수정한 내역은 워킹디렉터리에만 존재하고 git add 되지 않은 상태 |