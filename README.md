# Git

# 커밋하지 않고 리버트하기

```bash
git revert —no-commit (되돌릴 커밋 해시값)
```

git revert는 자동으로 커밋도 해주지만 위의 명령어는 커밋은 되지 않은 상태 이므로 **커밋 메시지 및 내역을 더 추가할 수 있다.**

# Branch

```bash

# branch 목록
git branch 

# branch 생성
git branch {브랜치 이름}
# ex.
git branch new-teams

# branch 이름 변경 
git branch -m {기존 브랜치 이름} {새 브랜치 이름}
# ex.
git branch -m new-teams new-coach

# branch 삭제
# 병합되지 않은 커밋이 있는 경우 실수 방지를 위해 브랜치를 삭제할 수 없다.
git branch -d {삭제할 브랜치 이름}

# branch 강제 삭제
git branch -D {삭제할 브랜치 이름}

# branch 전환
git switch {브랜치 이름}

# branch 생성 및 전환
git switch -c {새 브랜치 이름}

# local / remote 포함 브랜치 목록
git branch -a

# remote branch를 추적(track)하는 브랜치를 로컬에 생성
# git branch {원격 브랜치 이름} 을 해도 똑같음
git switch -t {원격 이름}/{원격 브랜치 이름}
ex. git switch -t origin/from-remote
```

# 리셋

git reset —hard: 작업 내역 자체를 지움

git reset —mixed: 작업 내역을 작업 디렉터리 영역으로 변경

- 기본값 git reset만 해도 동일

git reset —soft: 작업 내역을 스테이지 영역으로 변경

```bash

# 해당 커밋 위의 모든 커밋 삭제
git reset --hard {커밋 해쉬 값}

# 작업 디렉터리(Working Directory)와 스테이지 영역 파일 내역 되돌리기
git reset --hard

# 해당 커밋 위의 모든 커밋의 파일 내역을 작업 디렉터리로 변경
git reset --mixed {커밋 해쉬 값}

# 작업 내역을 디렉터리 영역으로 변경
git reset --mixed

# 해당 커밋 위의 모든 커밋의 파일 내역을 스테이지 영역으로 변경 
git reset --soft {커밋 해쉬 값} 

# 해당 커밋 위의 모든 커밋 삭제
git reset {옵션} HEAD~{원하는 단계}
git reset --hard HEAD~2
```

# 작업 내용 되돌리기

```bash
# 작업 디렉터리에 있는 파일 내역 되돌리기
git restore {파일 이름}
git restore pumas.yaml

# 스테이지 영역에 있는 파일 내역을 작업 디렉터리로 변경
git restore --staged {파일 이름}
git restore --staged panthers.yaml
```

# 브랜치 합치기

## 머지

두 가지 브랜치를 이어 붙이는 것 이 과정에서 커밋이 하나 더 생긴다. 

**자기 브랜치 커밋 위에 대상 브랜치 커밋 쌓음**

- 리모트 브랜치를 머지할 때는 리모트 브랜치 위에 자기 브랜치 커밋이 쌓임

### 충돌

충돌하는 파일만 수정해서 하나의 커밋으로 해결

```bash
# 현재 브랜치에 대상 브랜치를 병합한다.
git merge {대상 브랜치 이름}

# 머지 중단
git merge --abort

# 브랜치 커밋 합쳐서 병합 - 브랜치 기록 병합 안남음
git merge --squash {가져올 브랜치 이름}
```

## 리베이스

브랜치를 다른 브랜치에 옮겨 붙이는 것 

**대상 브랜치 커밋을 가져와서 그 위에 나의 브랜치 커밋 쌓음**

rebase 작업에 따라 커밋의 개수가 달라질 수 있음

ex. git rebase feat/a

- feat/A 브랜치의 a 커밋과 현재 브랜치 B 커밋이 충돌을 일으켰고 a 커밋으로 해결하면 feat/A 브랜치의 a 커밋과 같은 내용이 되므로 b 커밋은 없어짐

### 충돌

모든 커밋마다 충돌을 하나씩 차례대로 해결해 주어야함

```bash
# 대상 브랜치를 커밋을 땡겨오고 해당 커밋 위로 현재 브랜치의 커밋들을 쌓는다.
git rebase {대상 브랜치 이름}

# 리베이스 중단
git rebase --abort

# 도착 브랜치 -> 출발 브랜치 생성 -> 출발 브랜치에서 이동할 브랜치로 생성
# 이때 도착 브랜치로 이동할 브랜치의 커밋(잔가지)만 가져오고 싶을 때 사용
git rebase --onto {도착 브랜치} {출발 브랜치} {이동할 브랜치}
```

## 차이

1. 작업 내역이 다르게 처리
    
    리베이스. 작업 내역이 깔끔하게 한 줄로 정리
    
    머지. 브랜치의 흔적이 남음
    

브랜치의 사용 내역을 남겨 둘 필요가 있다면 머지를 쓰는 것이 좋고, 작업 내역을 깔끔하게 만드는게 중요하다면 리베이스가 더 적절한 선택

1. 코드 충돌 여부

이미 팀원들 간에 공유된 커밋에 대해서는 리베이스를 사용하지 않는게 좋다.

한 팀원이 임의로 다른 팀원의 작업 내역을 이어 붙이면 각자의 코드가 충돌할 수도 있습니다.

# 원격 저장소

```bash
# 원격 저장소 목록
git remote

# 원격 저장소를 origin이라는 이름으로 추가 / DNS서버랑 비슷한 느낌 192.xxx.xxx.xxx -> naver.com
git remote add origin https://github.com/choiseunghyeon/git-practice.git

# git push - 내 컴퓨터에 있는 커밋 내역 중 원격 저장소에 없는 커밋을 업로드 한다.
# -u는 (--set-upstream) 기본 값 설정 origin(어느 원격 저장소) main (어느 브랜치)
# 해당 명령어 이후 git push만 입력해도 origin main에 푸시
git push -u origin main

# git push시 로컬 브랜치 이름 그대로 리모트 브랜치에 푸시
git config --global push.default current

# 즐겨찾기 삭제처럼 원격 저장소와 로컬 프로젝트의 연결만 없애는 것으로 깃허브의 저장소는 삭제되지 않는다.
git remote remove (origin 등 원격 저장소 이름)
```

## Push / Pull

Pull은 원격 저장소의 최신 커밋을 로컬 컴퓨터로 가져와 머지하거나 리베이스 한다.

```bash
# 원격 저장소에 로컬 커밋 올리기
git push 

# 원격 저장소 커밋 가져오기
git pull

# merge 방식 pull
# local commit 위에 remote commit을 쌓고 Merge brnch commit을 생성함
git pull --no-rebase

# rebase 방식 pull
# remote commit 위에 local commit을 쌓음 (remote가 먼저고 local commit이 그 이후에 적용된 것으로 함)
git pull --rebase

# merge / rebase 기본 방식 설정
# false -> merge가 기본값
# true -> rebase가 기본값
git config pull.rebase false
git config pull.rebase true

# 원격 브랜치 삭제
git push {원격 저장소 이름} --delete (원격 브랜치 이름)
git push origin --delete from-remote
```

## Fetch

원격 저장소의 최신 커밋을 로컬로 가져오기만 한다.

# 깃

### 스냅샷 방식

각 버전별로 파일을 그대로 저장한다.

다른 툴의 경우 델타 방식을 사용하지만 이 방식은 버전 내역이 길어질수록 느리다.

### 분산 버전 관리 시스템

로컬에서 깃의 작업 내역을 모두 갖고 있기 때문에 자유롭게 작업을 할 수 있다.

다른 툴의 경우 중앙집중식 버전 관리라 인터넷 연결에 문제가 생기면 로컬에서 작업하는 데 제한이 생긴다.  

## 깃의 세가지 공간: 작업 디렉터리(Working Directory), 스테이지 영역, 저장소(Repository)

### 작업 디렉터리

새로 파일이 추가되거나 기존 파일이 변경되거나 삭제되는 등 수정 사항이 생기면 작업 디렉터리에 위치

git add 를 통해 스테이지 영역으로 이동시킨다.

### 작업 디렉터리의 상태

- tracked
    - 깃의 관리 대상에 등록된 파일
- untracked
    - .gitignore 파일에 추가되어 깃이 무시하는 파일 또는 아직 관리 되지 않은 파일
    - git add 명령을 적용하지 않은 파일

### 스테이지 영역

git commit 을 통해 저장소의 커밋 상태로 옮긴다.

커밋되는 저장소를 .git directory라고 부르기도 한다.

계속 저장소에 존재하다가 파일을 수정하면 다시 작업 디렉터리로 들어간다.

# 헤드

특정 브랜치의 최신 커밋을 의미

헤드는 다음과 같은 상황에서 바뀔 수 있다.

- 새로운 커밋이 현재 브랜치에 추가될 때 헤드가 해당 커밋을 가리킨다.
- 새로운 브랜치를 생성할 때 헤드가 새로운 브랜치를 가리킨다.
- 다른 브랜치로 이동할 때 헤드가 이동한 브랜치를 가리킨다.

## 체크아웃

작업 내역을 그대로 두고 파일의 상태만 과거 시점으로 이동하는 것

마치, 타임라인을 건드리지 않고 타임머신으로 과거의 원하는 시점으로 여행하는 것

```bash
# 한 단계 돌아가기 ^(개수만큼 돌아감)
git checkout HEAD^

# 3단계 뒤로 돌아가기
git checkout HEAD^^^
git checkout HEAD~3

# 특정 커밋으로 돌아가기
git checkout {커밋 해쉬값}

# 한 단계 앞으로 가기
git checkout -

# 특정 태그가 있는 커밋으로 가기
git checkout {태그 이름}
git checkout v1.2.1
```

checkout으로 과거로 돌아간다는 것은 해당 시점에 아직 이름이 지어지지 않은 임시 브랜치를 생성한뒤 그쪽으로 이동하는 것

![image](https://github.com/choiseunghyeon/git-practice/assets/20368822/e08e0fef-45bc-4cc6-86ed-99d7b5c4a912)

헤드 이동 단계에서 

git switch -c {새 브랜치 이름}

을 하게 되면 어떤 브랜치의 해당 시점(HEAD가 있는 시점)에서 새로운 브랜치를 따게 된다.

### 원격 저장소 내용을 페치하고 체크아웃으로 전체 폴더의 상태 확인

```bash
git fetch
git checkout {원격 저장소 이름}/{원격 저장소 브랜치 이름}
# ex. git checkout origin/main
```

# 스태시

작업하던 내용을 깃에서 다른 공간에 잠시 치워 두는 기능

스태시를 하려면 먼저 깃의 관리 대상(tracked)이 되어야 한다.

- 새로 생성한 파일이라면 git add 를 통해 관리 대상으로 만들자

```bash
# 현재 작업들 치워 두기
git stash
# 이름 붙여서 치워 두기
git stash -m {메시지}

# 치워 둔 마지막 항목(번호 없을 시) 적용
git stash apply
git stash applu stash@{1}

# 치워 둔 마지막 항목(번호 없을 시) 삭제
git stash drop

# 치워 둔 마지막 항목(번호 없을 시) 적용 및 삭제 - apply + drop가 동일
git stash pop 

# 새 브랜치를 생성하여 pop - 충돌 사항이 있는 상황에 유용
git stash branch {브랜치 이름}

# 치워 둔 모든 항목들 비우기
git stash clear

# stash된 작업 목록
git stash list
```

# 커밋 자세하게 다루기

```bash
# 마지막 커밋 메시지 수정 및 스테이지 영역의 작업 내역 추가
git commit --amend
```

## 과거 커밋 수정, 삭제, 병합, 분할하기

p, pick - 커밋 그대로 두기

r, reword - 커밋 메시지 변경

e, edit - 수정을 위해 정지

d, drop - 커밋 삭제

s, squash - 이전 커밋에 합치기

```bash
git rebase -i {대상 커밋의 이전 커밋 해시값}

#e, edit의 경우
# 이전 커밋으로 이동 후
git reset HEAD^

# git add & git commit 으로 원하는 파일만 분리하여 커밋

# 이후 진행
git rebase --continue
```

## 리셋 취소하기

```bash
# 수행한 깃 작업의 모든 내역 표시
git reflog

# reflog 커밋에서 리셋하고 싶은 커밋의 이전 커밋 해시값 넣기
git reset --hard {이전 커밋 해시값}
```

## 태그

```bash
# 태그 목록 확인
git tag
# 특정 모든 태그 확인
git tag -l 'v1.*'

#태그 달기
git tag {태그 이름} {커밋 해시값} -m {태그 메시지}
git tag v1.0.0 a3b0f98c150d501db466de66f6c72a6287b417d8 -m "업그레이드 버전"

# 태그 보기
git show v1.0.0 

# 특정 로컬 태그 하나를 원격 저장에 올리기(동기화)
git push {원격 저장소 이름} {태그 이름}

# 로컬 태그들을 원격 저장소에 올리기(동기화)
git push --tags

# 원격 저장소에서 태그 삭제
git push --delete {원격 저장소 이름} {태그 이름}

```

## fast forward vs 3-way merge

### fast forward

변경 사항이 더 최근인 브랜치로 통합

- 어떤 브랜치를 사용했고 언제 병합했는지 기록이 남지 않는다.

커밋을 만들어 병합혀려면 `git merge —no-ff {병합할 브랜치 이름}` 을 사용하기도 한다.

### 3-way merge

공통 조상 커밋을 기준으로 두 개의 브랜치를 자동으로 통합 

공통 조상 커밋과 A 브랜치 커밋 B 브랜치 커밋을 비교하기 때문에 3-way merge라고 부른다.

## 오류가 발생한 시점 찾아내기

이진 탐색으로 어느 커밋에서 오류가 발생했는지 탐색한다.

v20 - v10까지의 커밋 어딘가에서 오류가 발생되었다고 잡았을 때 

v15 → v17과 같은 식으로 오류 지점 탐색

```
# 오류 탐색 시작
git bisect start

# 오류 발샐 안할 시
git bisect good

# 오류 발생 시
git bisect bad

# 처음 시작
git bisect start
git bisect bad
git checkout {의심 지점 커밋 해쉬값}

# 오류 발생 여부에 따라
git bisect good / git bisect bad

# 최종 적으로 오류 발생 커밋 해시값 및 커밋 이름 알려줌

# 끝나면
git bisect reset
```

![image](https://github.com/choiseunghyeon/git-practice/assets/20368822/46804e78-852e-4096-9a64-7bc4e28e2c37)
