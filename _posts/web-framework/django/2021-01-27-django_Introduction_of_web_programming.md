---
title: "[Django] 웹 프로그래밍(Web programming)"
category: Django
---

# 1. 웹 프로그래밍의 이해

## 1.1 웹 프로그래밍

> HTTP(S) 프로토콜로 통신하는, 클라이언트와 서버를 개발하는 것
>
> 파이썬 웹 프로그래밍 : 장고(Django)와 같은 웹 프레임워크를 사용하여 웹 서버 개발

**웹 클라이언트 - 서버**

> 웹 클라이언트가 http(s) 프로토콜을 사용하여 웹 서버로 요청(Request)을 보내면 웹서버는 동일 프로토콜을 사용하여 웹 클라이언트로 응답(Response)함
>
> - 웹 클라이언트 : 보통은 웹 브라우저 사용, 개발자 직접 개발 가능
>     - 웹 브라우저들은 이미 웹 클라이언트로서 개발되어 있지만, 실제 프로젝트 에서는 웹 클라이언트를 개발해야하는 상황이 많이 발생함
>
> - 웹 서버 : 웹 프레임워크를 활용하여 웹 서버를 개발

<br>

## 1.2 다양한 웹 클라이언트

1. 웹 브라우저를 사용하여 요청 : 주소창에 ` [URL]`  입력
2. 리눅스 `curl` 명령을 사용하여 요청 : `curl [URL]`

3. Telnet을 사용하여 요청

    ```
    telnet [URL] [포트번호]
    GET / HTTP/1.1
    Host: [URL]
    ```

4. 직접 만든 클라이언트로 요청

    ```
    import urllib.request
    print(urllib.request.urlopen("http://www.example.com").read().decode('utf-8'))
    ```

<br>

## 1.3 HTTP 프로토콜

> 웹 서버와 웹 클라이언트 사이에서 데이터를 주고받기 위해 사용하는 통신방식

### 1.3.1 HTTP 메시지의 구조

> | Start Line | Header | Blank Line | Body |

예

- Request

    >  GET /book/shakespeare HTTP/1.1
    > Host: www.example.com:8080

- Response

    > HTTP/1.1 200 OK
    >
    > Content-Type: application/xhtml+xml; charset=utf-8
    >
    > <html> .... </html>

### 1.3.2 HTTP 처리 방식

> HTTP 메서드를 통해서 클라이언트가 원하는 처리방식을 서버에 알려줌

#### HTTP 메서드 종류

| 메서드명 | 의미                                                         |
| -------- | ------------------------------------------------------------ |
| **GET**  | 지정한 URL의 정보를 가져옴(가장 많이 사용하는 메서드)        |
| **POST** | 리소스 생성, 리소스 데이터 추가(e.g., 블로그 글 등록 및 수정) |
| PUT      | 리소스 변경(URL 결정권이 클라이언트에 있을 때 사용)          |
| DELETE   | 리소스 삭제                                                  |
| HEAD     | 리소스의 헤더(메타데이터) 취득                               |
| OPTIONS  | 리소스가 서포트하는 메서드 취득                              |
| TRACE    | 루프백 시험에 사용                                           |
| CONNECT  | 프록시 동작의 터널 접속으로 변경                             |

### 1.3.3 GET & POST

예 : 폼에서 사용자가 입력한 데이터들을 서버로 보낼 때

- GET 방식

    >  URL의 '?' 뒤에 이름=값 쌍으로 이어붙여 보냄
    >
    > URL 길이 제한으로 많은 양의 데이터를 보내기 어렵고, 정보가 주소창에 노출됨
    >
    > GET http://docs.djangoproject.com/search/?**q=forms&release=1** HTTP/1.1

- POST 방식

    > 파라미터들을 요청 메시지의 바디에 넣음
    >
    > POST http://docs.djangoproject.com/search/ HTTP/1.1
    >
    > Content-Type: application/x-www-form-urlencoded
    >
    > **q=forms&release=1** 

### 1.3.4 상태 코드

> 서버에서의 처리 결과는 응답 메시지의 상태라인에 있는 상태 코드로 파악 가능
>
> 상태 코드 예 : https://developer.mozilla.org/ko/docs/Web/HTTP/Status

<br>

## 1.4 URL 설계

> Uniform Resource Locater
>
> 웹 서버 로직 설계의 첫 걸음
>
> 사용자 또는 웹 클라이언트에게 웹 서버가 가지고 있는 기능을 명시해주는 중요한 단계

#### URL 구성 항목

> URL 스킴 | 호스트명 | 포트번호 | 경로 | 쿼리스트링 | 프라그먼트
>
> http:// | www.example.com | :80 | /services? | category=2&kind=patents | #n10

- URL 스킴 : URL에 사용된 프로토콜
- 호스트명 : 웹서버의 호스트명, 도메인명 또는 IP로 표현
- 포트번호 : 웹 서버 내 서비스 포트번호(Default: 80(http), 443(https))
- 경로 : 파일 또는 애플리케이션 경로
- 쿼리스트링 : 질의 문자열, '&'로 구분된 이름=값 쌍 형식으로 표현
- 프라그먼트 : 문서 내 앵커 등 조각을 지정

#### 1.4.1 URL을 바라보는 측면

> 웹 클라이언트에서 호출한다는 시점에서 보면, 웹 서버에 존재하는 애플리케이션에 대한 API
>
> API의 명명 규칙을 정하는 방법에 따른 측면
>
> 1. RPC(Remote Procedure Call)
>
>     > 클라이언트가 네트워크상에서 원격에 있는 서버가 제공하는 API 함수를 호출하는 방식
>     >
>     > **웹 클라이언트에서 URL을 전송하는 것은 웹 서버의 API 함수를 호출한다고 인식하는 것**
>     >
>     > 예 : http://blog.example.com/search?q=test&debug=true
>
> 2. REST(Representational State Transfer)
>
>     > 서버에 존재하는 요소들을 모두 리소스라고 정의하고, URL을 통해 웹 서버의 특정 리소스를 표현
>     >
>     > 리스소스에 대한 조작을 GET, POST, ... 등의 HTTP 메서드로 구분
>     >
>     > **웹 클라이언트에서 URL을 전송하는 것은 웹 서버에 있는 리소스 상태에 대한 데이터를 주고 받는 것**
>     >
>     > 예 : http://blog.example.com/search/test

### 1.4.2 간편 URL

> 쿼리스트링 없이 경로만 가진 간단한 구조의 URL

- 예
    - http://example.com/index.php?page=foo
    - http://example.com/foo

<br>

## 1.5 웹 애플리케이션 서버

**웹 서버** 

> 웹 클라이언트의 요청을 받아 요청을 처리하고, 그 결과를 **웹 클라이언트에게 응답**
>
> 정적페이지인 HTML, CSS, JavaScript, 이미지 파일을 웹 클라리언트에 제공할 때 사용
>
> Apache httpd, Nginx, lighttpd, ...

**웹 애플리케이션 서버**

> 웹 서버로부터 **동적 페이지** 요청을 받아서 요청을 처리하고, 그 **결과를 웹 서버로 반환**
>
> 동적페이지 생성을 위한 프로그램 실행과 DB 연동 기능을 처리
>
> Apache Tomcat, JBoss, WebLogic, ...

### 1.5.1 정적 페이지 vs 동적 페이지

#### 정적(Static)과 동적(Dynamic)

> 사용자가 페이지를 요청하는 시점에 "페이지 내용이 유지되는가, 또는 변경되는가"를 구분해주는 용어

#### 정적 페이지

> 항상 같은 내용을 표시하는 웹페이지
>
> **해당 웹 서비스의 제공자가 사전에 준비하여 서버 측에 배치한 것**
>
> 동일한 리소스(URL)의 요청에 대해서는 항상 동일한 내용의 페이지를 반환함
>
> **HTML, CSS, JavaScript, 이미지만으로 이루어진 페이지가 해당됨**

#### 동적 페이지

> 동일한 리소스(URL)의 요청이라도 누가, 언제, 어떻게 요구했는지에 따라 각각 다른 내용이 반환되는 페이지
>
> **프로그래밍 코드가 포함되어 있어 페이지 요청 시점에 HTML 문장을 만들어내는 것**
>
> **현재시각을 보여주는 페이지, 사용자마다 다른 정보를 보여주는 페이지**

### 1.5.2 CGI 방식의 단점

#### CGI(Common Gateway Interface)

> DB 처리에 대한 요구가 많아짐에 따라 웹 서버와는 다른 프로그램이 필요해짐
>
> **별도의 프로그램과 웹 서버 사이에 정보를 주고받는 규칙을 정의한 것**

#### 단점

> 각각의 클라이언트 요청에 대하여 독립적인 별도의 프로세스가 생성(오버헤드)되어 시스템에 많은 부하를 주게 됨

### 1.5.3 CGI 방식의 대안 기술

> 1. 별도의 애플리케이션을 PHP 등의 스크립트 언어로 작성하고, 스크립트를 처리하는 스크립트 엔진(인터프리터)을 웹 서버에 내장시켜서 오버헤드를 줄이는 방식
>
> 2. 애플리케이션을 처리하는 프로세스를 미리 데몬으로 기동시키고, 웹 서버의 요청을 데몬에서 처리하는 방식

### 1.5.4 애플리케이션 서버 방식

> CGI 애플리케이션을 별도의 데몬으로 처리하는 방식은 애플리케이션 전용 데몬인 '애플리케이션 서버 방식'으로 발전함
>
> 웹 서버가 직접 프로그램을 호출하기 보다는, **웹 애플리케이션 서버를 통해 간접적으로 웹 애플리케이션 프로그램을 실행함**
>
> **애플리케이션 프로그램의 실행 결과를 웹 서버에 전달**, **웹 서버는 웹 애플리케이션 서버로부터 전달받은 응답 결과를 웹 클라이언트에 전송**

<img src="https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F77c1167e-81c4-4273-bd56-644fc748daad%2FUntitled.png?table=block&id=6ce2536b-0d6a-4c44-a5d7-e0119635a5b6&width=6470&userId=&cache=v2" width=70% align=center>

https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html