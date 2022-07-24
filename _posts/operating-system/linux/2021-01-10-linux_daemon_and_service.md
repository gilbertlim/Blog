---
title: "[Linux] 데몬(Daemon)과 서비스(Service)"
category: Linux
---

# 항상 실행

### Daemon or Service

- 사용자가 직접적으로 제어하지 않고, 백그라운드에서 돌면서 여러 작업을 하는 프로그램
- 언제 사용할지 모르기 때문에 항상 켜져 있음
- **서버**에 설치되어 있는 프로그램(web server)
- 항상 켜져 있는 도어락, 냉장고, 모뎀, ...
- `/etc/init.d` : 데몬 성격의 파일이 있는 경로
- 서버
    - apache : 대표적인 웹서버
        - `sudo apt-get install apache2` : 아파치 서버 설치
        - `sudo service apache2 start` : 아파치 서버 시작
        - `sudo service apache2 stop` : 아파치 서버 종료
        - `ps aux | grep apache2` : 아파치 서버 상태 확인
        - 부팅 시 자동 실행
            - `cd /etc/r` : `rcN.d` 파일이 있는 경로(Mac OS는 다를 수 있음)
            - `rc3.d` : CLI 방식으로 구동하고 있을 때의 디렉토리
            - `rc5.d` : GUI 방식으로 구동하고 있을 때의 디렉토리
            - 링크
                - S02apache2라는 이름으로 링크를 건 것(좌측 끝 l = link)
                    - S : CLI 방식으로 부팅될 때 rc3.d에 있으면 자동 실행되는 디렉토리를 의미
                    - 02 : 우선순위
                    - K : 자동으로 종료되는 디렉토리
                - `./S02apache2` : 링크 실행

### ↔ Daemon

- 필요할 때 켰다가 필요 없어지면 끄는 것
- 웹 브라우저(web browser)
- `ls`, `rm`, `mkdir`