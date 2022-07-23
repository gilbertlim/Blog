---
title: "[Linux] 터미널 커맨드(Terminal Command)"
category: Linux
---

# Terminal Command

# Intro

- 쉘

    - 터미널 명령어는 쉘(Shell)에 씀
    - 사용자의 명령을 입력 받아서 처리하고 결과를 출력함
    - 커널과 사용자 사이의 인터페이스

- 프로그램

    - Code

- 프로세스

    - 프로그램이 실행되고 있는 상태

- 명령어

    - 리눅스 명령어는 일종의 프로그램을 실행시키는 것과 유사함

    - 사용 빈도수가 높은 명령어를 중심으로 학습하고, 나머지는 맥락적/경험적으로 알아가면 됨

        ![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9203bc08-b7b6-468d-90ad-76b5f3ebdc1b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210112T131144Z&X-Amz-Expires=86400&X-Amz-Signature=fa74b0cdcd2d70bea15d31f345caf84e80903fca5e7485f620c433661efc67f7&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

# Notice

- **명령어는 현재 있는 디렉토리에서 이루어지므로 `pwd`를 통해 현재 경로를 필수적으로 확인 (잘못해서 다른 경로에서 작업될 수 있음)**
- 필요한 명령어는 Google 검색
    - 예 : "create directory in linux"

# Option

- 명령어 뒤에 `-option` 를 붙여 기본 동작과 다른 동작을 실행 시킴

# Command

- UI

    - `clear` : 터미널 창 정리

- Semicolon

    - `mkdir why; cd why` : 세미콜론을 활용하여 한 줄에 여러 커맨드를 실행할 수 있음

- **Manual**

    - `--help` : 명령어 사용 설명서 출력(현재 화면)

        ```
        gilbert@gilbert-Ubuntu:~/Desktop$ mkdir --help
        Usage: mkdir [OPTION]... DIRECTORY...
        Create the DIRECTORY(ies), if they do not already exist.
        
        Mandatory arguments to long options are mandatory for short options too.
          -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask
          -p, --parents     no error if existing, make parent directories as needed
          -v, --verbose     print a message for each created directory
          -Z                   set SELinux security context of each created directory
                                 to the default type
              --context[=CTX]  like -Z, or if CTX is specified then set the SELinux
                                 or SMACK security context to CTX
              --help     display this help and exit
              --version  output version information and exit
        
        GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
        Full documentation at: <https://www.gnu.org/software/coreutils/mkdir>
        or available locally via: info '(coreutils) mkdir invocation'
        ```

    - `man` : 전용 화면으로 이동하여 상세 매뉴얼 출력

        - **mac OS의 매뉴얼 사용법**
        - `/Word`  : 화면 내 단어 찾기
            - `n` :  찾은 단어가 있는 행으로 순차적 이동
        - `q` : 매뉴얼 종료

- `!!` : 직전 명령어를 의미함

- `tail -f [FILENAME]`

    - 파일의 내용 중 최근 1줄만 출력(실시간)
    - 주로 로그 확인 시 사용

- Permission

    - `sudo COMMAND`  : sudo(=super user(root user) do), 권한이 필요한 커맨드 앞에 붙여 사용
        - 기본적으로 일반사용자로 사용하다가, 경우에 따라서 관리자 권한으로 사용함
            - `rm -rf /`(루트 디렉토리에 있는 모든 파일 삭제)와 같은 명령어를 잘못실행할 경우 큰일...

- Directory

    - `whereis COMMAND` : 해당 프로그램의 위치 출력
    - `ls`  : 현재 디렉토리의 상태 확인
        - `ls -l` : 자세히 보기
        - `ls -a`  : 숨김 파일 까지 보기
            - 숨김 파일 : `.FILENAME` 으로 표현
        - `ls -al` : 숨김 파일까지 자세히 보기
        - `ls -alS` : 숨김 파일까지 자세히 보기(파일크기 내림차순)
    - `ls [DIRECTORY]` : 해당 디렉토리의 상태 확인
    - `pwd` : 현재 디렉토리 확인
    - `mkdir DIRECTORY`  : 디렉토리 생성
        - `mkdir -p PARENT_DIRECTORY/DIRECTORY/...` : 부모 디렉토리와 함께 디렉토리 생성
    - `cd TOP_LEVEL_DIRECTOR/DIRECTORY/...` : 현재 디렉토리와 관계없이 해당 디렉토리로 이동(절대 경로)
    - `cd /` or `cd ~` : 최상위 디렉토리(root)로 이동
    - `cd DIRECTORY`  : 현재 디렉토리를 기준으로 이동(상대 경로)
        - `./` : 현재 디렉토리
        - `cd ..`  : 현재 디렉토리의 부모 디렉토리로 이동
            - `cd ../../` : 현재 디렉토리에서 부모 디렉토리로 2회 이동

- File

    - Copy & Paste
        - `cp DIRECTORY1 DIRECTORY2`  : 복사한 파일을 붙여넣기
    - Move & Rename
        - `mv DIRECTORY1 DIRECTORY2` : 파일 이동
        - `mv FILENAME1 FILENAME2` : 파일명 변경
    - Create
        - `touch FILENAME` : 임의로 파일 생성
    - Remove
        - `rm FILENAME` : 파일 삭제
        - `rm -r DIRECTORY` : 디렉토리 삭제

- Display the contents of a file

    - `cat FILENAME` : 파일 내용 출력
    - `grep FIND_INFO FILENAME` : 파일 내용 중 특정 정보가 포함된 행을 모두 찾아줌
    - `CONTENTS(ex cat FILENAME) | grep FIND_INFO`  : 파이프로 연결된 정보에서 특정 정보가 포함된 행을 찾아줌

- Program List

    - `ps aux` : 현재 실행되고 있는 프로그램 출력

- Web

    - `curl + [IP or DOMAIN_NAME]` : 해당 페이지 접속후 화면 html형식으로 출력)

# 연속적으로 명령 실행시키기

- `;` : 앞의 명령어가 실패해도 다음 명령어가 실행
- `&&` : 앞의 명령어가 성공했을 때 다음 명령어가 실행
- `&` : 앞의 명령어를 백그라운드로 돌리고 동시에 뒤의 명령어를 실행

# 명령어 그룹핑 {}

`{ cd User; touch a.txt; echo 'grouping' }`