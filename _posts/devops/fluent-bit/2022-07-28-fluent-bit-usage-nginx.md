---
title: "[Fluent-bit] Nginx 로그를 Fluent-bit으로 처리하는 방법"
category: Fluent-bit
---

# 1. Installation

공식 링크에서 원하는 환경을 선택하여 설치하면 된다.

<a href="https://docs.fluentbit.io/manual/installation/getting-started-with-fluent-bit">https://docs.fluentbit.io/manual/installation/getting-started-with-fluent-bit</a>

다음 환경에서 설치하는 데 큰 문제는 없었다.

- Intel Processor MacOS
- EC2(Ubuntu, Amazon Linux) + Jenkins
- EKS DaemonSet + Helm, ArgoCD

## MacOS

AWS EC2에 Nginx 서버를 운영 중이라면, Fluent-bit agent는 해당 EC2에 설치되어 있어야 한다.

K8S 로그를 처리하려면 DaemonSet이나 Pod로 배포되어 있어야 한다.

위와 같이 이미 배포된 Fluent-bit의 Config를 수정하려면 CI/CD를 태우거나, SSM으로 직접 노드에 접속해서 config를 수정해야한다.

따라서 로컬에서 Config를 수정하고 문제 없으면 CI/CD를 태워 잘 동작하는지 확인하곤 했다.

Mac OS에 간단히 설치해보자.

`brew install fluent-bit`

### 주요 명령어

- fluent-bit 버전 확인 : `fluent-bit --version`
- foreground로 fluent-bit 실행 : `fluent-bit -c [CONFIG 파일 경로]`

# 2. Usage

## Nginx

### Nginx 설치

Nginx 로그를 Fluent-bit으로 처리하기 전에 Nginx부터 설치한다.

아래 링크를 보고 Nginx를 설치 -> Config 변경 -> Nginx 데몬 실행 한다.

<a href="/nginx/nginx-usage/">Nginx 설치 방법</a>

<br>

### Fluent-bit Config 작성

원하는 경로에 config를 작성한다.

<a href="https://github.com/gilbertlim/fluent-bit/tree/main/tutorial/nginx">소스 코드</a>

nginx access.log가 한 줄씩 생성될 때 마다 Fluent-bit에서 읽고 작성된 정규식을 거쳐 필드/값 형태로 파싱된다.

파싱된 로그를 standard output으로 출력하는 코드이다.

구문은 match에 의해 연결된다. 원하는 tag 값을 적어주면 해당 tag 다음에 동작한다.

같은 tag로 연결된 경우 코드는 순차적으로 실행된다.


fluent-bit.conf
```
[SERVICE]
    Parsers_File parsers.conf # parsers.conf를 parser로 사용하겠다.

[INPUT]
    Name tail # 한 줄씩 읽어 들이겠다.
    Path /usr/local/etc/nginx/access.log # 로그 파일 경로 
    Tag  nginx # access.log에서 수집되는 로그는 tag를 nginx로 사용
    Parser nginx # name이 nginx인 parser를 사용

[OUTPUT]
    Name stdout # standard output으로 출력
    Match nginx # tag:nginx와 연결
```

parsers.conf
```
[PARSER]
    Name        nginx
    # 정규식으로 필드/값을 파싱
    Format      regex
    Regex       ^(?<remote>[^ ]*) (?<host>[^ ]*) (?<user>[^ ]*) \[(?<time>[^\]]*)\] "(?<method>\S+)(?: +(?<path>[^\"]*?)(?: +\S*)?)?" (?<code>[^ ]*) (?<size>[^ ]*)(?: "(?<referer>[^\"]*)" "(?<agent>[^\"]*)")
    Time_Key    time # time으로 사용할 필드명 
    Time_Format %d/%b/%Y:%H:%M:%S %z # time format
    Types       size:integer # size 필드는 integer 타입으로 지정, 정의되지 않은 필드는 모두 string 
```

<br>

작성된 Config를 가지고 Fluent-bit을 실행한다.

`fluent-bit -c [파일경로]/fluent-bit.conf`

<br>

브라우저에서 localhost:8080에 접속하면
브라우저에서 새로 고침 할 때마다 원본 로그는 fluent-bit에 의해 파싱되어 필드=>값으로 출력된다.

<img src="/assets/images/posts/devops/flb_nginx_tutorial.png" alt="Fluent-bit Nginx 로그" width="100%" />

# 3. Tip

nginx 로그를 발생시키기 위해 직접 nginx를 구동시켰는데, 직접 로그 파일을 수정하면 개발 시간을 단축시킬 수 있다.

## 1. Input 구문 - 로그파일 위치(Path) 변경

fluent-bit.conf
```
[SERVICE]
    Parsers_File parsers.conf

[INPUT]
    Name tail
    # Path /usr/local/etc/nginx/access.log
    Path ~/Dev/Fluent-bit/fluent-bit-repo/tutorial/nginx/test.log
    Tag  nginx
    Parser nginx

[OUTPUT]
    Name stdout
    Match nginx
```

## 2. 로그 파일 생성

IDE로 로그를 생성시키기 편하게 Fluent-bit config와 같은 경로상에 둔다.

로그를 한 줄 적고 Enter를 누른 뒤 파일을 Save하면 Fluent-bit이 Tail 되었다 판단하고 로그를 처리한다.

<img src="/assets/images/posts/devops/flb-direct-input.png" alt="" width="100%" />