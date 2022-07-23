---
title: "[Linux] 인터넷(Internet), 네트워크(Network), 서버(Server)"
category: Linux
---

# Internet, Network, Server

## Internet

- Internet은 Client-Server로 이루어져 있음

- Client-Server

    - 인터넷에서 Client는 Web browser, Server는 Web server에 해당함
        - Web browser : Chrome, firefox, ...
        - Web Server : Apache, nginx, ...
    - Request & Response

    1. Client에서 사람이 domain name 입력
    2. Client는 DNS Server에 접속하여 ip address 확인(domain name → ip address)
    3. 해당 ip adress로 Server에 request를 보냄
    4. Server에서는 Client로 response 전송(서버 HDD에 있는 index.html 파일 정보를 가져옴)

- ip address

    - `ip addr`
        - 사설 ip(Private address) 확인
        - 컴퓨터에 부여된 실제 ip(예 : 192.168.0.2)
    - `curl ipinfo.io/ip` (`curl [IP or DOMAIN_NAME]` : 해당 페이지 접속후 화면 html형식으로 출력)
        - 공인 ip(Public address) 확인
        - 어떤 ip로 접속하였는지 확인(예 : 219.112.40.122)
    - 하나의 컴퓨터에서 두 개의 ip가 발생하는 이유(대부분의 컴퓨터)
        - 라우터(Router) : 공유기에 해당
        - '대표 전화' - '내선 전화'의 관계와 유사
    - 사설 ip 그대로는 서버로 쓸 수 없음

## Web server(Apache2)

### 설치 및 시작

- `sudo apt-get update` : 패키지 버전 업데이트
- `sudo apt-get install apache2` : 웹서버 설치
- `sudo service apache2 start` : 웹서버 시작
- `sudo service apache2 stop` : 웹서버 종료
- `sudo service apache2 restart` : 웹서버 재시작

### elinks

- 텍스트 기반 콘솔 웹 브라우저
- `elinks` : elinks 실행

### 콘솔 웹 브라우저를 활용한 본인의 서버 접속하기

- `elinks http://[PRIVATE IP]` : 아이피를 이용한 내부망 서버 접속(elinks 127.0.0.1)
- `elinks <http://localhost`> : 127.0.0.1의 도메인 네임을 이용한 내부망 서버 접속

## 사용자가 접속했을 때 서버 내부에서 일어나는 일

- 사용자가 접속했을 때 웹 서버는 /etc/appache2 디렉토리에 있는 설정 파일들(.conf)을 참고해서 어떤 스토리지에 있는 어떤 파일(/var/www/html/*.html)을 실행시키는 것

### Document Root

- 웹 브라우저가 사용자가 요청한 파일을 찾는 최상위 디렉토리(웹 페이지를 찾는 최상위 디렉토리)
- Linux OS : /var/www/html (000-default.conf에 도큐먼트 루트로 지정되어 있음)

## Log

- /var/log/appache2 디렉토리에 로그를 보관
    - access.log
    - error.log
- 실시간 로그 확인 방법
    - `tail -f /var/log/apache2/[LOG_FILE]` : 파일의 내용 중 최근 1줄만 출력(실시간)