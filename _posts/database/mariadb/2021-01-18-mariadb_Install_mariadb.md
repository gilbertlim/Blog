---
title: "[MariaDB] MariaDB 설치"
category: MariaDB
---

# 2. MariaDB 설치

### 1.  `brew install mariadb`

mariadb 패키지 설치



### 2. `brew service start mariadb`

mariadb 서버 시작

- mac OS에서 Homebrew를 통한 mariaDB 설치 시 사용하는 명령어

- 타 OS에서는 `mysql.server start` 사용

- 먼저 `mysql.server start` 를 사용하였을 경우, `ps aux | grep mysql` 을 통해 모든 프로세스를 확인하고

    `sudo kill [pid] [pid] ...` 를 통해 종료 시키고 `brew service start mariadb` 로 재접속할 것

    

### 3. `sudo mysql -uroot`

mariaDB 접속

- 강제 접속 명령어로 초기 1회만 사용 이후에는 `mysql -u root -p` 사용



### 4. `use mysql;`

mysql DB 선택



### 5. `set password for 'root'@'localhost'=PASSWORD('원하는 비밀번호');`

root 비밀번호 설정



### 6. `mysql -u root -p`

mariaDB 접속 명령어



### 7. DB 서버 관리 명령어

- `brew services start mariadb` : DB 서버 시작

- `brew services stop mariadb` : DB 서버 종료
- `brew services restart mariadb` : DB 서버 재시작

- `brew services list` : brew로 설치된 패키지의 서버 상태 확인



### mySQL DB tool for mac OS

- Workbench
- Sequel Pro
