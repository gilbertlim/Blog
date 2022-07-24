---
title: "[Linux] 사용자(User)"
category: Linux
---


# **1. 다중 사용자**

## **특징**

- 유닉스 계열 운영체제는 여러 명이 함께 사용할 수 있는 기능을 가지고 있음
- 이 기능은 강력하지만, 혹독한 대가가 따름
- 배우기 어렵고, 보안의 문제 발생 가능
- 다중 사용자 시스템이되면, 복잡도가 엄청 올라감

## **id & who**

- `id` : 자신의 id 확인("나는 누구인가")
- `who` : 현재 시스템에 누가 접속했는지 확인

# **2. 관리자와 일반 사용자**

## **super(root) user**

- `/root` 디렉토리 사용
- **`sudo` : 일시적으로 super user의 권한을 갖게 하는 명령**
- 접속방법
    - Linux OS
        - `su - root` : 슈퍼 유저로 접속
    - Mac OS
        - 루트 계정 활성화(su : sorry  해결)
            1. `sudo -s` 를 입력하여 현재 사용자 비밀번호 입력
            2. `passwd root` 를 입력하여 비밀번호 지정
            3. `su - root` 로 슈퍼 유저 접속
        - 루트 계정 비활성화
            1. `su`
            2. `dsenableroot -d`
            3. OS 공통
                - `exit` : 사용자 빠져나가기

## **user**

- `/home/[USERNAME]` 디렉토리 사용
- 프롬프트에 `$` 가 표시됨

# **3. 사용자의 추가**

## 방법

- Linux OS
    1. `sudo useradd -m [USERNAME]` : 사용자 생성
    2. `sudo passwd [USERNAME]` : 비밀번호 지정
    3. `su - [USERNAME]` : 사용자 접속
- Mac OS
    - 위 방법이 Mac OS에서는 적용되지 않음
    - 사용할 경우가 드물어 추후 참조할 링크를 남김
    - https://smallbusiness.chron.com/add-user-terminal-mac-os-x-screen-sharing-31846.html

## 다른 사용자에게 `sudo` 명령을 사용할 수 있게 하기

- `sudo usermod -a -G sudo [USERNAME]` (Linux OS)

# **4. 권한(Permission) 지정**

> 어떤 사용자가 파일과 디렉토리에 대해서 어떤 일을 할 수 있게 하거나 할 수 없게 하는 것

- File & Directory 🚷 Read & Write & Excute
- 기본적으로 자기가 소유한 파일과 디렉터리만 건들 수 있다.
- 파일 : 읽고, 쓰고, 실행할 수 있는 권한을 지정
- 디렉토리 : 디렉토리 안에 있는 파일을 조회(`ls`), 생성(`touch`)/삭제(`rm`)/이름변경(`mv`), 진입(`cd`) 할 수 있는 권한을 지정

## `ls -l` 명령어에 대한 설명

- - | rwx | r-x | r-x | 1 | gilbert staff | 0 | Dec 4 22:43 | a.txt
- Type | Permission(owner | group | other) | File Count | Owner Name Group_Name | File Size | Date | File Name |
    - type : d(directory), -(file)
    - rw- : read / write / excute

## `chmod`(change mode) : 권한 변경 명령어

- `chmod [option] [class][+ or -][r or w or e] [FILENAME]` : Accessmode(owner, group, other)에 따른 권한 추가, 삭제 방법
    - option : -R(Recursive)
    - class : u(owner), g(group), o(other), a(all)
    - operator : +(권한 부여), -(권한 뺏기), =(해당 권한만 부여)
        - `chmod u=x [FILENAME]` : --x
    - mode : r(read), w(write), e(excute)
- 파일의 실행 권한
    - 특정 프로그램(e.g., /bin/bash, 해석기, 파서)을 통해서 어떤 특정한 프로그래밍 언어를 실행시키는 것은 아무런 제약이 없음
    - 컴퓨터에 설치된 프로그램인 것처럼 실행하고 싶은 경우에는 실행 권한을 주어야 함
    - 예 : `chmod u+x [FILENAME]` → -rwx
- 모드 설정 시 8진법 사용
    - 모드
        - 0 : ---
        - 1 : --x
        - 2 : -w-
        - 3 : -wx
        - 4: r--
        - 5 : r-x
        - 6 : rw-
        - 7 : rwx
    - 예 : `chmod 110 [파일명]` = `chmod u+x [FILENAME]` + `chmod g+x [FILENAME]`

# **5. 그룹**

- 사용자 구성 : user, other, group
- group : user도 other도 아닌 2명 이상의 집합

## 사용자를 그룹에 추가

새로운 사용자 생성 시 그룹에 추가

- `useradd -G [GROUPNAME] [USERNAME]`

그룹 생성

- `sudo groupadd [GROUPNAME]`

그룹 확인

- `vi /etc/group`

기존 사용자를 그룹에 추가

- `sudo usermod -a -G [GROUPNAME] [USERNAME]`

디렉토리 class의 group 변경

- `sudo chown [PREVIOUS_GROUP]:[NEW_GROUP] [DIRECTORY_NAME]` m