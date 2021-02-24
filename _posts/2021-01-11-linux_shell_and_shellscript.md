---
title: "[Linux] 쉘 스크립트(Shell Script)"
category: Linux
---

# Shell

- Hardware : CPU, Memory, Storage, ...
- Kernel
    - 하드웨어를 직접적으로 제어하는 코어 프로그램
    - 커널으로부터 명령을 전달 받고, 처리 결과를 커널에게 전달함
- Shell
    - 사용자가 내린 명령을 해석하는 프로그램
    - 사용자와 상호작용하는 소프트웨어
    - 사용자가 쉘에 명령을 입력하면 커널이 이해할 수 있는 방식으로 전달
    - 커널로부터 처리 결과를 전달 받고, 사람이 이해할 수 있는 방식으로 출력

##### 사용자가 쉘을 선택할 수 있게 Kernel과 Shell을 나누었다고 볼 수 있다.



### 명령어

- `echo TEXT` : 텍스트를 그대로 출력
- `echo $0` : 현재 쓰는 쉘 확인

### Bash vs Zsh

- `cd`+`TAB` : 숨김 파일 보임 | 숨김 파일 안보임
- `cd` : `cd /home/gilbert` | `cd /h/g` + `TAB` → `cd /home/gilbert`



# Shell script

- 나중에 재사용할 명령어들을 어딘가에 저장하는 방법

- 적어둔 파일을 그대로 컴퓨터가 실행하게 하는 방법

- 예

    - `/bin` : 리눅스 기본 프로그램들이 위치하는 디렉토리

        ```SHELL
        #!/bin/zsh   # 아래 프로그램들은 zsh로 해석되어야 한다는 뜻
        
        if ! [ -d bak ]; then   # 현재 디렉토리에 bak라는 디렉토리가 존재하지 않는가?
        				mkdir bak
        fi
        cp *.log bak   # .log로 끝나는 파일들을 bak 디렉토리로 복사
        ```

    - `chmod +x FILENAME` : 실행가능한 파일 설정(x : excutable)

    - `./FILENAME` : 현재 디렉토리에 있는 파일 실행