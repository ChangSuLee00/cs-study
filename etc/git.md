# GIT

> VCS(version control system)의 하나이다.

주요 기능

- 버전의 관리 = 프로젝트의 시간과 차원을 관리하는 것.

  - 시간적 관리 = 프로젝트의 버젼을 언제든 바꿀 수 있다.

  - 차원적 관리 = 프로젝트를 여러 모드로 자유롭게 전환하고 변경 사항들을 쉽게 이동시킬 수 있다.

CLI vs GUI

    CLI = command line interface
    GUI = graphic line interface

## Git 시작

> $ git init

.git 생성 -> git 관리내역이 저장되어 있기 때문에 삭제되면 안된다.

> $ git status

현재 폴더의 상태를 git의 관점에서 보여준다.

## Git에게 맡기지 않을 것들

1. 포함할 필요가 없는 빌드 결과, 라이브러리

2. 포함하지 않아야할 보안상 민감한 파일 등을 git의 관리에서 배제한다.

## 변화를 타임캡슐에 묻어두기

> git status

변경사항 확인.

> git add .

모든 파일 담기.

> git commit -m "커밋 메시지"

커밋 메시지와 함께 커밋.

> git log

로그 확인.

> git commit -am "(메시지)"

add와 commit을 한번에 한다(새로 추가된 파일 없을 경우).

## 과거로 돌아가는 두 가지 방법

1. Reset: 원하는 시점으로 돌아간 뒤 이후 내역들을 지운다.

2. Revert: 되돌리기 원하는 시점의 커밋을 거꾸로 실행한다.

협업을 할때는 reset 말고 revert를 쓴다.

> git reset --hard (돌아갈 커밋 해시)

reset으로 해당 커밋으로 돌아간다.

> git reset --hard

뒤에 커밋 해시가 없으면 마지막 커밋을 가리킨다.

> git revert --hard (되돌릴 커밋 해시)

:wq로 커밋 메시지 저장. revert 한다.

> git revert --no-commit (되돌릴 커밋 해시)

커밋하지 않고 revert 한다.

### SourceTree 사용

revert = 해당 커밋에 마우스 우클릭 -> 커밋 되돌리기.

reset = 해당 커밋에 마우스 우클릭 -> 이 커밋으로 초기화 -> Hard 선택.

## 여러 branch 만들어보기

Branch = 분기된 가지 (다른 차원)

프로젝트를 하나 이상의 모습(배포용, 테스트용, 실험용 등)으로 관리해야할때 또는 여러 작업들이 각각 독립되어 진행될 때 사용한다.

> git branch (추가할 브랜치명)

브랜치 생성.

> git branch

브랜치 목록 확인.

> git switch (브랜치)

해당 브랜치로 이동.

> git switch -c (브랜치명)

생성과 동시에 해당 브랜치로 이동.

> git branch -d (삭제할 브랜치명)

브랜치 삭제하기.

> git branch -D (강제삭제할 브랜치명)

브랜치 강제 삭제.

> git branch -m (기존 브랜치명) (새 브랜치명)

브랜치 이름 바꾸기.

## branch를 합치는 두가지 방법

merge: 두 브랜치를 한 커밋에 이어 붙인다.

- 브랜치 사용 내역을 남겨야할 때 적합한 방식.

rebase: 브랜치를 다른 브랜치에 이어붙인다.

- 한 줄로 깔끔하게 정리된 내역을 유지하기 원할때 사용.

- 협업을 할때는 사용하지 않는 것이 좋다.

### merge로 합치기

main 브랜치로 이동한 후

> git merge (합칠 브랜치)

reset으로 되돌릴 수 있다.

> git branch -d (지울 브랜치)

해당 명령어로 병합된 브랜치를 삭제해준다.

### rebase로 합치기

합칠 브랜치로 이동한 후

> git rebase main

merge와는 반대로 main브랜치를 rebase한다.

> git merge (합칠 브랜치)

해당 명령어로 main의 시저믈 합칠 브랜치와 같게 해준다.

> git branch -d (지울 브랜치)

해당 명령어로 합친 브랜치를 삭제해준다.

## 충돌 해결하기

### merge

vscode에서 돋보기 클릭한 후 '<<<' 검색하면 충돌이 난 부분을 확인할 수 있다.  
해당 부분을 수정해준 뒤 다시 merge를 진행한다.  
이후 'git add.' -> 'git commit'으로 병합을 끝낸다.

> git merge --abort

당장 해결이 어려울 경우 merge 중단하는 명령어.

### rebase

> git rebase --continue

충돌 부분 수정 -> 'git add .' -> 'git rebase --continue'로 충돌이 해결될때까지 반복한다.

> git rebase --abort

당장 해결이 어려울 경우 해당 명령어로 rebase 중단.

## Gtihub

github를 사용하는 이유: 모든 업로드와 다운로드가 커밋 단위로 이루어지며 매번 최신 커밋으로 버젼이 바뀌어야 자료의 수정이 가능하기 때문에 같은 파일을 여러 사람이 수정하기가 수월하다.

> git remote add origin (원격 저장소 주소)

로컬 git 저장소에 원격 저장소 연결.  
여기서 origin은 원격 저장소의 이름이다. 보통 기본값이 origin.

> git branch -M main

메인 브렌치 이름을 main으로 하는 것.

> git push -u origin main

로컬 저장소의 커밋 내역을 원격 저장소에 업로드 한다.  
-u는 뒤에 따라오는 원격과 브랜치를 기본값으로 만든다.  
(push하면 origin의 main에 반영).

> git clone (원격 저장소 주소)

해당 레포지토리를 그대로 다운로드 한다(기록 포함).

### push와 pull

> git push

원격으로 커밋을 올린다.

> git pull

원격에서 파일을 받아서 로컬과 동기화 시킨다.

#### pull해야 할 것을 push 한 경우

> git pull --no-rebase

merge 방식: 두개의 브랜치를 merge하여 문제를 해결한다.

> git pull --rebase

rebase 방식: pull 분기를 먼저 시행하고 이후 push를 뒤에 붙인다.

#### 협업상 충돌이 발생했을때

pull을 해서 충돌상황을 해결한다.

> git pull --no-rebase

merge 방식으로 충돌상황 해결.  
이후 'git add .' -> git commit 

> git pull --rebase

rebase 방식으로 충돌상황을 해결한다(이때는 rebase 협업시라도 써도 ).  
이후 'git add .' -> git commit 

> git push --force

로컬의 내용을 강제로 push하고 레포지토리의 내역을 삭제한다(기록채로 삭제).

### 원격 branch 다루기

#### 로컬에서 브랜치 만들어 원격에 push

> git push -u origin (새 브랜치)

브랜치를 만든 뒤 위 명령어를 실행하면 새 브랜치를 해당 이름으로 원격 저장소에 브랜치 추가.

> git branch --a

로컬과 원격으 브랜치들 확인.

#### 원격의 브랜치 로컬에 받아오기

> git fetch

원격의 변경사항 로컬에 가져오기.

> git switch -t origin/(가져올 브랜치명)

로컬에 같은 이름의 브랜치를 생성, 연결, switch

> git push (원격 이름) --delete (원격의 브랜치명)

원격(origin)의 브랜치 삭제.

Reference:  

[제대로 파는 Git & Github 무료 파트](https://www.yalco.kr/lectures/git-github/)  

[제대로 파는 Git & Github 유료 파트](https://www.yalco.kr/lectures/git-github-dive/)


정리된 내용이 이미 있어 따로 내용을 정리하지 않아도 될 것 같아 이 이후로는 커밋하지 않는다.