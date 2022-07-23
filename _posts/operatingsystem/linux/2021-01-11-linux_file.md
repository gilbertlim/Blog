---
title: "[Linux] 파일(File)"
category: Linux
---

# File

### 파일의 용도

- 데이터 보관
- 실행 파일 : 해야될 일에 대한 명령 보관

### 파일을 찾는 방법

- Locate : 파일 정보가 담긴 데이터베이스를 뒤져서 결과 출력

    1. 검색 데이터베이스 생성

    - Linux OS
        - `sudo updatedb`
    - Mac OS
        - `sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.locate.plist`

    1. 활성 상태 보기(Activity Monitor)에서 find 프로세스 완료 상태 확인(only Mac OS)
    2. `locate FILENAME`

    - 특정 파일 찾기
        - Linux OS
            - `locate *.log`
        - Mac OS
            - `locate FILENAME | grep WANT_TO_FIND_FILENAME`

    1. 데이터베이스를 뒤져서 결과 출력

- find : 지금 디렉토리에서 파일을 뒤짐

    - `find . -name FILENAME`  : 모든 디렉토리를 뒤져서 파일 찾기
    - `find ~ -name FILENAME` : 홈 디렉토리를 뒤져서 파일 찾기
    - 참조
        - https://www.tecmint.com/35-practical-examples-of-linux-find-command/

- whereis : `$PATH`, `$MANPATH`에서 실행파일을 찾기

    - `whereis FILENAME`
    - `$PATH` 
        - 환경 변수 : 시스템에 원래 설치된 변수
        - `echo $PATH` : PATH에 담겨있는 정보 출력
        - 명령을 실행할 때 마다, 명령의 전체 경로를 적지 않아도 되는 기능
        - `$PATH` 변수에 담겨있는 디렉토리들을 검색해서, 커맨드에 입력한 실행파일이 있는지 확인

