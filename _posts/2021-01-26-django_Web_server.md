---
title: "[Django] 웹 서버(Web server) 구축하기"
category: Django
---

# Web Server 구축하기

## Bitnami Mamp Stack

> Management Console로, 아파치 서버를 관리하는 도구

## 웹서버 접속

> 1. 웹브라우저가 웹 서버에게 HTTP 프로토콜을 사용하여 `index.html` 파일을 Request
> 2. 웹서버는 같은 컴퓨터에 설치되어 있는 `mampstack-8.0.1-0/apache2/htdocs/index.html`을 찾아 웹브라우저에게 Response
> 3. 웹브라우저는 `index.html` 파일에 저장되어 있는 코드를 해석하여 웹페이지에 표시

### 1. Bitnami Mamp Stack을 사용하여 Apache2 웹서버를 시작

### 2. 웹브라우저 접속

2.1 웹서버가 설치된 컴퓨터의 웹브라우저에서 접속

> http://127.0.0.1:80/index.html **http://localhost**

- `http://` : Hyper Text Transfer Protocol, HTML 문서와 같은 리소스를 가져올 수 있도록 해주는 프로토콜
- `127.0.0.1` : =localhost, 웹서버가 설치된 컴퓨터 자체
- `:80` : 웹서버 기본 포트(생략가능)
- `index.html` : 서버에 접속 시 자동으로 연결되는 html파일(생략가능)

2.2 같은 공유기에 연결된 기기(내부망)에서 접속

> [http://내부망주소/index.html](http://xn--220bt7o11cpsf8sl/index.html) 예 : **http://192.168.0.2**

- `http://` : HTML 문서와 같은 리소스를 가져올 수 있도록 해주는 프로토콜
- `내부망주소`  : 웹서버가 설치된 컴퓨터의 IP
- `index.html` : 서버에 접속 시 자동으로 연결되는 html파일(생략가능)