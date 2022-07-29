---
title: "[Fluent-bit] Fluent-bit 사용 방법"
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

개발 시에는 매번 CI/CD를 태우기 어려우니 로컬에서 테스트할 필요가 있다.

Mac OS에 간단히 설치해보자.

`brew install fluent-bit`

### 주요 명령어

- fluent-bit 버전 확인 : `fluent-bit --version`
- foreground로 fluent-bit 실행 : `fluent-bit -c [CONFIG 파일 경로]`

# 2. Usage

## Nginx

### Nginx 설치
Nginx 로그를 Fluent-bit으로 처리하기 전에 Nginx부터 설치한다.

아래 링크를 보고 Nginx를 설치, Config 변경, 시작하면 된다.

<a href="/nginx/nginx-usage/">Nginx 설치 방법</a>

<br>

### Fluent-bit Config 작성

원하는 경로에 config를 작성한다.

<a href="https://github.com/gilbertlim/fluent-bit/tree/main/tutorial/nginx">소스 코드</a>

nginx access.log가 한 줄씩 생성될 때 마다 Fluent-bit에서 읽고 작성된 정규식을 거쳐 필드/값 형태로 파싱된다.
파싱된 로그는 standard output으로 출력된다.

fluent-bit.conf
```
[SERVICE]
    Parsers_File parsers.conf

[INPUT]
    Name tail
    Path /usr/local/etc/nginx/access.log 
    Tag  nginx
    Parser nginx

[OUTPUT]
    Name stdout
    Match nginx
```

parsers.conf
```
[PARSER]
    Name   nginx
    Format regex
    Regex ^(?<remote>[^ ]*) (?<host>[^ ]*) (?<user>[^ ]*) \[(?<time>[^\]]*)\] "(?<method>\S+)(?: +(?<path>[^\"]*?)(?: +\S*)?)?" (?<code>[^ ]*) (?<size>[^ ]*)(?: "(?<referer>[^\"]*)" "(?<agent>[^\"]*)")
    Time_Key time
    Time_Format %d/%b/%Y:%H:%M:%S %z
```

<br>

작성된 Config를 가지고 Fluent-bit을 실행한다.

`fluent-bit -c [파일경로]/fluent-bit.conf`

<br>

브라우저에서 localhost:8080에 접속하면
브라우저에서 새로 고침 할 때마다 원본 로그는 fluent-bit에 의해 파싱되어 필드=>값으로 출력된다.

<img src="/assets/images/posts/devops/flb_nginx_tutorial.png" alt="Fluent-bit Nginx 로그" width="100%" />