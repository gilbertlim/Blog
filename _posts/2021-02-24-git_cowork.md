---
title: "[Git] 협업 도구로서의 Git"
category: Git
---

# 협업 도구로서의 Git
git으로 관리되는 프로젝트 = git으로 관리되는 폴더

## Git 원격 관련 명령어

### 1. `git remote`
현재 관리되고 있는 원격 저장소의 정보를 출력

- `git remote -v` : 주소까지 상세 정보를 출력(verbose mode)

### 2. `git remote add [원격저장소의 이름] [원격저장소의 주소]`
새로운 원격 저장소 정보를 추가

- `git remote add origin https://github.com/[github유저네임]/[저장소의이름].git`
- `git remote remove [원격저장소의 이름]` : 해당 원격저장소 정보를 삭제

### 3. `git push [원격저장소의 이름] [브랜치의 이름]`
원격 저장소에 코드를 업로드(밀어넣기)

- `git push origin master`
- `git push -u origin master` : 업스트림(upstream) 설정

### 4. `git clone [원격저장소의 주소] (폴더명)`
원격 저장소의 코드를 다운로드

- remote에 대한 정보가 들어가 있어 별다른 remote 설정이 필요 없음
- `git init` 과 유사한 개념으로, 한 번만 실행하는 것(.git이 두 개가 됨)
- 원래 컴퓨터는 master, clone한 컴퓨터들?은 origin으로 표기됨

### 5. `git pull [원격저장소의 이름] [브랜치의 이름]`

- `git pull origin master` : 원격저장소의 로그를 내 저장소에 업데이트

<br>

# 협업의 일반 원칙
하향식 & 일방적 & 독재적

## 깃헙 사용자 등록
다른 사용자가 내 프로젝트에 Push하기 위해서는 사용자 등록이 필요함

- Github repository - Settings - Manage access - ID 추가

<br>

# Git Branch
Branch = 평행 세계

## 동기적과 비동기적
동기적(Synchronous)
    - 앞선 일이 끝나지 않으면 진행 불가능
    - 단순, 효율적

비동기적(Asynchronous)
    - 앞선 일이 끝나지 않더라도 진행 가능
    - 복잡, 비효율적
  
## Git Branch 명령어

### 1. `git branch`
현재 생성되어 있는 branch들의 목록을 출력
- 처음 깃을 시작하면 master라는 세계(브랜치)에서 시작함

### 2. `git branch [브랜치 이름]` 
새로운 branch 생성
- `git checkout [브랜치 이름]` : 브랜치 이동

### 3. `git merge [브랜치 이름]` 
branch를 병합(현재 속한 브랜치에서 인자로 주어진 브랜치를 합병)
- `git checkout [주가 되는 브랜치]` : **병합 전 무조건 주가 되는 브랜치로 이동해야 한다!**
- `git merge test (master)` : master 브랜치가 test를 병합함

### 4. `git branch -d [브랜치 이름]`
(거의 모든, 주요 branch를 제외한) branch는 **일회용**이다. 병합된 브랜치는 항상 삭제한다.
- `-d` : 삭제(delete)
- **한 번 활용했던 세계(feature branch, master는 major branch)는 삭제 해야한다 !**

### 5. `Git push origin [브랜치 이름] ` 
브랜치를 Github으로 업로드
- 새로운 브랜치 push 후에 마스터도 push 한다면, master를 default branch로 변경

### 6. `Pull request`
Github.com에서 지원하는 merge request에 가까운 기능
- 회사에서 일하는 방식에 비유하면, 팀장님께 코드 제안하는 느낌
- mark down 지원

### 7. `git remote update branch`
원격저장소의 브랜치 최신화

### 8. `git checkout [원격 저장소의 브랜치 이름]`
원격 저장소의 브랜치를 로컬에 받아 테스트하는 방법
- detached HEAD 상태로 소스를 보고 변경 할 수 있지만 commit, push 불가

### 9. `git checkout -t origin/[원격 저장소의 브랜치 이름]`
원격저장소의 브랜치를 로컬로 가져온 뒤, 해당 브랜치로 checkout

### 10. `git checkout -b [생성할 브랜치 이름] origin/[원격저장소의 브랜치 이름]`
원격저장소의 브랜치를 로컬로 가져올 때 지정한 이름으로 가져온 뒤, 해당 브랜치로 checkout

### 기타

- Commit 시에 주어를 생략한 '명령조'로 작성한다(e.g. `"Add a txt"`)
- `README.md` 파일이 Github에 올라가면 페이지에 자동으로 보여진다.
- `git log` 시 나오는  `HEAD` 는 현재 어느 세계에 있는지를 알려준다.
- (master) 는 제일 처음 브랜치이며, 주니어는 보통 또 다른 브랜치에서 작업한다.
- `git add .` : 모든 항목을 add
- git으로는 시간이동, 또 다른 세계(branch)에서 작업이 가능하다.
- git hosting : 로컬 저장소의 버전을 업로드할 원격 저장소를 임대해주는 비즈니스, 바로 Github !

---

### 번외 질문

### commit은 얼마나 빈번히 해야하나요?
내 PJT는 내맘대로, 다른 사람들과 함께하는 합의된 '룰'대로

* 회사 룰대로 -> Logical commit (논리적으로 분리 가능한 기능 중심)
* 오류가 난 상태(빌드가 되지 않는 상태)에서는 commit하지 않는다.

### push는 얼마나 빈번히 해야하나요?
코드를 공유하거나, 다른 호스트/로컬 컴퓨터로 옮겨야할 때

- TIL을 1일 1푸쉬해보자.