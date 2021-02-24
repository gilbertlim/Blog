---
title: "[Django] 장고(Django)"
category: Django
---

 # 3. Django 웹 프레임워크

> 웹 프로그램을 개발하는 데 사용하는 **파이썬 웹 프레임워크** 중에서 가장 준비가 잘 되어 있는 프레임워크
>
> MTV 패턴을 제공하여 API를, 템플릿을 이용하여 정적 HTML 파일을 생성해 뷰를 만들 수 있도록함
>
> 제공하는 기능이 풍부, 쉽고 빠르게 개발 가능, 사용자가 많은 장점이 있음

## 3.1 주요 기능별 특징

#### 1. MVC 패턴 기반 MVT

> MVC(Model-View-Controller)를 기반으로 한 프레임워크
>
> **MVT(Model-View-Template)로 명칭을 다르게 부름**

#### 2. ORM(Object-Relational Mapping, 객체 관계 매핑)

> **DB 시스템과 Model(파이썬 클래스)을 연결시키는 다리 역할**
>
> 다양한 DB 시스템을 지원하며 SQL 문장을 사용하지 않고도 테이블을 조작할 수 있음

#### 3. 자동으로 구성되는 관리자 화면

> 웹 서버의 콘텐츠, 즉 DB에 대한 관리 기능을 위하여 **프로젝트를 시작하는 시점에 기본 기능으로 관리자 화면 제공**
>
> 관리자 화면을 통해 애플리케이션에서 사용하는 테이블과 데이터들을 쉽게 생성 및 변경 가능, 별도 관리 기능 개발 불필요

#### 4. 우아한 URL 설계

> **URL을 직관적이고 쉽게 표현할 수 있음**
>
> 정규표현식을 사용하여 복잡한 URL 표현 가능, 각 URL 형태를 파이썬 함수에 1:1로 연결하도록 되어 있어 개발이 편리함

#### 5. 자체 템플릿 시스템

> **내부적으로 확장이 가능하고 디자인이 쉬운 강력한 템플릿 시스템**을 가지고 있음
>
> 화면 디자인과 로직에 대한 코딩을 분리하여 독립적으로 개발 진행 가능

#### 6. 캐시 시스템

> **자주 이용되는 내용을 저장해 두었다가 재사용하여 성능을 높임**
>
> 장고의 캐시 시스템은 캐시용 페이지를 메모리, DB 내부, 파일 시스템 중 아무곳에나 저장 가능함

#### 7. 다국어 지원

> 텍스트 변형, 날짜/시간/숫자 포맷, 타임존 지정 등 다국어 환경 제공

#### 8. 풍부한 개발 환경

> **테스트용 웹 서버를 포함**하고 있어 아파치 등 웹 서버가 없어도 테스트 진행 가능
>
> 디버깅 모드 사용 시 에러를 쉽게 파악하고 해결할 수 있도록 상세 메시지 출력

#### 9. 소스 변경사항 자동 반영

> *.py 파일의 변경 여부를 감시하고 있다가 변경이 되면 실행 파일에 변경 내역을 바로 반영함
>
> 테스트용 웹서버를 실행 중인 상태에서 소스 파일을 수정할 경우, **자동으로 새로운 파일이 반영됨(웹 서버 재시작 불필요)**

<br>

## 3.2 장고 프로그램 설치

#### 1. `pip install Django` : python package installer를 통해 장고 설치

#### 2. `python -m django --version` : 장고 버전 확인

<br>

## 3.3 장고에서의 애플리케이션 개발 방식

#### 장고에서의 애플리케이션

> **프로젝트 : 웹 사이트의 전체 프로그램**
>
> **애플리케이션 : 모듈화된 단위 프로그램**(프로젝트 하위의 서브 프로그램)
>
> 애플리케이션을 모아서 프로젝트 개발을 완성하는 것이 목적

### 3.3.1 MVT 패턴

#### **MVT**(MVC)

> MVT 중 한 요소가 다른 요소들에게 영향을 주지 않도록 설계하는 방식임
>
> 각 요소를 분할하여 개발을 진행할 수 있음

- **Model**(Model)

    > DB에 저장되는 **데이터**
    >
    > 블로그의 내용을 DB로부터 가지고 오거나, 저장 및 수정하는 기능

- **View**(Controller) : 프로그램 로직이 동작하여 **데이터를 가져오고 처리한 결과를 템플릿에 전달**

    > 버튼을 눌렀을 때 어떤 함수를 호출하며 데이터를 어떻게 가공할 것인지 결정하는 역할

- **Template**(View) : **사용자 인터페이스(UI)** 

    > 화면 출력을 위해 디자인과 테마를 적용해서 보여지는 페이지를 만듦

#### MVT 패턴에 따라 처리하는 과정

1. **웹 클라이언트로부터  Request**를  받으면, **URLconf를 이용하여 URL을 분석**
2. URL 분석 결과를 통해 URL에 대한 처리를 담당할 **View 결정**
3. **View는 자신의 로직을 실행**하면서, **DB 처리가 필요하면 Model을 통해 처리하고 그 결과를 반환 받음**
4. **View는 자신의 로직처리가 끝나면 Template을 사용하여 클라이언트에 전송할 HTML 파일 생성**
5. View는 최종 결과로 HTML 파일을 클라이언트에게 보내 **Response**

<img src="https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa9ce88a4-ec65-48a2-a810-506c009f839b%2FUntitled.png?table=block&id=0d1c406d-3bd9-4572-9345-7eab703d504a&width=2560&userId=&cache=v2" width="75%" align="center">

### 3.3.2 Model - DB 정의

#### Model

> 사용될 데이터에 대한 정의를 담고 있는 장고의 클래스
>
> **ORM 기법을 사용하여 애플리케이션에서 사용할 DB를 클래스로 매핑**해서 코딩할 수 있음
>
> 모델 클래스 = 테이블, 모델 클래스의 속성 = 필드
>
> **models.py**

##### ORM(Object-Relational Mapping, 객체 관계 매핑)

> 객체와 관계형 DB를 연결해주는 역할
>
> DB 대신에 객체(클래스)를 사용해 데이터를 처리할 수 있음
>
> 객체를 대상으로 필요한 작업을 실행하면, **ORM이 자동으로 적절한 SQL 구문이나 DB API를 호출해서 처리함**
> (직접 SQL 구문을 사용할 수도 있음)

- e.g. Person 테이블(모델 클래스) 생성

    ```python
    from django.db import models
    
    class Person(models.Model):
        first_name = models.CharField(max_length=30)
        last_name = models.CharField(max_length=30)
    ```

### 3.3.3 URLconf - URL 정의

> Request에 들어있는 URL이 **url.py 파일에 정의된 URL 패턴과 매칭**되는지 분석
>
> **url.py 파일에 URL과 처리함수(뷰)를 매핑하는 파이썬 코드 작성**

- **Path Converter**( `<[타입]:[이름]>`) : URL 패턴의 일부의 데이터 형식이 '타입'과 일치하면 '이름'으로 매칭됨

    ```python
    from django.urls import path
    from . import views
    
    urlpatterns = [
        path('articles/2003/', views.special_case_2003), # path('[URL]', [처리함수(뷰)]), ...
        path('articles/<int:year>/', views.year_archive),
        path('articles/<int:year>/<int:month>/', views.month_archive),
        path('articles/<int:year>/<int:month>/<slug:slug>/', views.article_detail),
    ]
    ```

    - 종류
        - str(Default) : '/'를 제외한 모든 문자열
        - int : 0 또는 양의 정수
        - slug : slug 형식의 문자열(ASCII, 숫자, '-', '_')
        - uuid : UUID 형식의 문자열(32자리의 16진수)
        - path : '/'를 포함한 모든 문자열, 전체 추출 시 많이 사용

- **정규표현식** : 복잡한 URL 표현 시 사용

    ```python
    from django.urls import path, re_path
    from . import views
    
    urlpatterns = [
        path('articles/2003/', views.special_case_2003),
        re_path(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
        re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/$', views.month_archive),
        re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<slug>[₩w-]+)/$', views.article.detail),
    ]
    ```

### 3.3.4 View - 로직 정의

> 1) 웹 클라이언트의 Request를 받아 DB 접속 등 해당 애플리케이션의 로직에 맞는 처리를 하고,
> 2) 결과 데이터를 HTML로 변환하기 위하여 템플릿 처리를 한 후에,
> 3) 최종 HTML로 된 Response 데이터를 웹 클라이언트로 반환하는 역할
>
> **views.py**에 함수 또는 클래스로 작성

- 현재 날짜와 시간을 HTML로 반환해주는 뷰

    ```python
    from django.http import HttpResponse
    import datetime

    def current_datetime(request): # request 객체를 받음
        now = datetime.datetime.now() # 로직에 맞는 처리
        html = "<html><body>It is now %s.</body></html>" % now
        return  HttpResponse(html)
        # 최종 httpResponse 객체 반환
        # 원래는 템플릿 파일을 해석해서 HTMl 코드 생성 후 HttpResponse 객체에 담아서 클라이언트에게 Response
    ```

### 3.3.5 Template - 화면 UI 정의

> **개발자가 작성하는 *.html 파일**
>
> 개발자가 Response에 사용할 *.html 파일을 작성하면,
> 장고는 이를 해석해서 최종 HTML 텍스트 Response를 생성하고 클라이언트에게 보내줌
>
> **settings.py** 파일에 정의된 TEMPLATES 항목과 INSTALLED_APPS 중
> TEMPLATES 항목에 정의된 디렉토리를 먼저 찾고난 다음, INSTALLED_APPS 항목에 등록된 각 앱의 templates 디렉토리를 검색하여 템플릿 파일을 찾음

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'polls.apps.PollsConfig',
]

TEMPLATES = [
    {
        ...
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        ...
    }
]
```

### 3.3.6 MVT 코딩 순서

>  독립적으로 개발할 수 있는 **모델**을 먼저 코딩하고, 그 다음 **뷰와 템플릿**을 코딩하는 것이 일반적

1. 프로젝트 뼈대 만들기 : 프로젝트 및 앱 개발에 필요한 데릭토리 및 파일 생성
2. 모델 코딩하기 : 테이블 관련 사항을 개발(models.py, admin.py)
3. URLconf 코딩하기 : URL 및 뷰 매핑 관계 정의(urls.py)
4. 템플릿 코딩하기 : 화면 UI 개발(templates/ 디렉토리 하위의 *.html)
5. 뷰 코딩하기 : 애플리케이션 로직 개발(views.py)

**뷰와 템플릿**

- UI 화면을 생각하면서 로직을 풀어나가고자 하는 경우, 템플릿을 먼저 코딩

- CBV(Class-Based View, 클래스형 뷰) 처럼 코딩이 매우 간단한 경우 뷰를 먼저 코딩 후 템플릿 코딩

<br>

## 3.4 애플리케이션 설계하기(실습)

> 설문에 해당하는 질문을 보여준 후, 질문에 포함되어 있는 답변 항목에 투표하면 그 결과를 알려주는 페이지 제작

### **3.4.1 화면 UI 설계**

'------------------------------index.html------------------------------'

​	**<u>What is your hobboy?</u>**

​	**<u>Who do you like best?</u>**

​	**<u>Where do you live?</u>**







'------------------------------detail.html------------------------------'

​	**What is your hobby?**



​	O Reading

​	O Soccer

​	O Climbing

​	**[ Vote ]**

'------------------------------results.html------------------------------'

- Reading - 3 votes
- Soccer - 1 vote
- Climbing - 7 votes





​	**<u>Vote Again?</u>**

'------------------------------------------------------------------------------' 

### 3.4.2 테이블 설계

**Question**

| Field         | Type         | Constraint                 | Description    |
| ------------- | ------------ | -------------------------- | -------------- |
| id            | integer      | NotNull, PK, AutoIncrement | PK             |
| question_text | varchar(200) | NotNull                    | 질문 문장      |
| pub_date      | datetime     | NotNull                    | 질문 생성 시각 |

**Choice**
| Field       | Type         | Constraint                      | Description    |
| ----------- | ------------ | ------------------------------- | -------------- |
| id          | integer      | NotNull, PK, AutoIncrement      | PK             |
| choice_text | varchar(200) | NotNull                         | 답변 항목 문구 |
| votes       | integer      | NotNull                         | 투표 카운트    |
| question    | integer      | NotNull, FK(Question.id), Index | FK             |

<br>

## 3.5 프로젝트 뼈대 만들기(실습)

### 프로젝트 뼈대 만들기

> 1. 프로젝트에 필요한 디렉토리 및 파일 구성, 설정파일 세팅
> 2. 기본 테이블 생성, 관리자 계정 생성 
> 3. 애플리케이션 디렉토리 및 파일 구성

#### 프로젝트 디렉토리의 체계 및 설명

**ch3(DIR)** : 프로젝트 관련 디렉토리 및 파일을 모아주는 최상위 루트 디렉토리(settings.py 파일에서  BASE_DIR)

- `db.sqlite3` : 테이블이 들어있는 SQLite3 DB 파일
- `mange.py` : 장고의 명령어를 처리하는 파일
- **mysite(DIR)** : 프로젝트명으로 만들어진 디렉토리로 프로젝트 관련 파일들이 들어 있음
    - `__init__.py` : 디렉토리에 이 파일이 있으면 파이썬 패키지로 인식함
    - `settings.py` : 프로젝트 설정 파일
    - `urls.py` : 프로젝트 레벨의 URL 패턴을 정의하는 최상위  URLconf
    - `wsgl.py` : Apache와 같은 웹 서버와 WSGI 규격으로 연동하기 위한 파일
- **polls(DIR)** : 애플리케이션명으로 만들어진 디렉토리로 애플리케이션 관련 파일들이 들어 있음
    - `__init__.py` : 디렉토리에 이 파일이 있으면 파이썬 패키지로 인식함
    - `admin.py` : Admin 사이트에 모델 클래스를 등록해주는 파일
    - `apps.py` : 애플리케이션의 설정 클래스를 정의하는 파일
    - **migrations(DIR)** : DB 변경사항을 관리목적의 디렉토리(DB 변경사항 발생 시 변경 내역을 기록한 파일 위치)
        - `__init__.py` : 디렉토리에 이 파일이 있으면 파이썬 패키지로 인식함
    - `models.py` : DB 모델 클래스를 정의하는 파일
    - `tests.py` : 단위 테스트용 파일
    - `views.py` : 뷰 함수를 정의하는 파일(함수형 뷰, 클래스형 뷰 모두 이 파일에 정의)

**프로젝트를 진행하면서 추가되는 디렉토리**

- **templates(DIR)** : 템플릿 파일이 들어있는 디렉토리(템플릿 파일은 프로젝트 레벨, 애플리케이션 레벨의 템플릿으로 구분하여 ch3/templates, ch3/polls/templates 경로에 생성됨)
- **static(DIR)** : CSS, Image, Javascript 파일이 들어있는 디렉토리(파일은 프로젝트 레벨, 애플리케이션 레벨의 템플릿으로 구분하여 ch3/static, ch3/polls/static 경로에 생성됨)
- **logs(DIR)** : 로그 파일이 들어있는 디렉토리(로그 파일의 위치는 settings.py 파일에서 LOGGING 항목으로 지정)

### 주의사항

- 쉘 커맨드 입력 시, 경로 상시 확인
- 장고에서 `,` 는 세미콜론과 같이 '끝났음'을 의미함

### 3.5.1 프로젝트 생성

1. `django-admin startproject mysite` : 'mysite' 프로젝트 생성
2. 프로젝트 폴더 생성 확인(mysite(root), mysite/manage.py, mysite/mysite/*.py)

3. `mv mysite ch3` : 루트 디렉토리 변경(mysite -> ch3)

### 3.5.2 애플리케이션 생성

1. `python manage.py startapp polls` : 'polls' 애플리케이션 생성

### 3.5.3 프로젝트 설정 파일 변경

> ch3/mysite/settings.py 파일을 실행하여 설정파일 변경

1. `ALLOWED_HOSTS = ['192.168.0.2', 'localhost', '127.0.0.1']` : 
    - 운영 모드(DEBUG=True)의 경우, 반드시 서버의 IP나 도메인을 지정
    - 개발 모드(DEBUG=False)의 경우, 값을 지정하지 않으면 `['localhost', '127.0.0.1']` 로 간주

2. `INSTALLED_APPS = [ ..., 'polls.apps.PollsConfig',]`
    - 'polls' 애플리케이션 등록(ch3/polls/apps.py 파일에 PollsConfig 클래스가 정의되어 있음)
    - 프로젝트에 포함되는 애플리케이션들은 모두 설정 파일에 등록되어야 함
3. DB 엔진 설정(Default 그대로 사용, SQLite3)
4. `TIME_ZONE = 'Asia/Seoul'` : 타임존을 한국 시간으로 변경

### 3.5.4 기본 테이블 생성

1. `python manage.py migrate` : 기본 테이블 생성을 위한 명령 실행(테이블을 만들지 않았어도 사용자 및 그룹 테이블 등을 만들어주기 위해 프로젝트 개발 시작 시점에 이 명령을 실행하는 것)
2. db.sqlite3 파일 생성 확인

### 3.5.5 지금까지 작업 확인하기

> 테스트용 웹 서버(runserver)를 실행하여 장고가 제공해주는 웹 페이지와 테이블 확인
>
> - `python mange.py runserver` : 127.0.0.1:8000으로 실행됨
>
> - `python manage.py runserver 8888` : 127.0.0.1:8888로 실행됨
>
> - `python manage.py runserver 0.0.0.0:8000 &` : 웹 서버 프로그램이 **백그라운드에서 실행됨**(유닉스 계열)

1. `python manage.py runserver 0.0.0.0:8000`
    - 테스트용 웹 서버 실행
    - 0.0.0.0 : 현재 명령을 실행 중인 서버의 IP와는 무관하게 웹 접속 요청을 받겠다는 의미
        (주소창에는 runserver를 실행중인 서버의 실제 IP 주소 입력)
2. '192.168.0.2:8000'에 접속하여 Rocket 환영 메시지 확인
3. `python mange.py createsuperuser` : 슈퍼유저 설정
4. '192.168.0.2:8000/admin'에 접속 후 로그인하여 Users, Group 테이블 생성 확인
    - settings.py 파일에 django.contrib.auth 애플리케이션이 등록되어 있고, auth 앱에 테이블이 미리 정의되어 있는 것

<br>

## 3.6 애플리케이션 개발하기 - Model 코딩

### 3.6.1 테이블 정의

1. polls/models.py 파일에 테이블 정의

    ``` python
    from django.db import models

    class Question(models.Model): # 테이블, django.db.models.Model 클래스를 상속받아 정의
        question_text = models.CharField(max_length=200) # 필드
        pub_date = models.DateTimeField('date published')

        def __str__(self):
            return self.question_text

    class Choice(models.Model):
        question = models.ForeignKey(Question, on_delete=models.CASCADE)
        choice_text = models.CharField(max_length=200)
        votes = models.IntegerField(default=0)

        def __str__(self):
            return self.choice_text
    ```

- PK는 클래스에 지정하지 않아도, 장고는 속성을 Not Null, Autoincrement로 id 필드를 생성함
- `DateTimeField()` : 필드 클래스에 정의한 date published는 pub_date 컬럼에 대한 레이블 문구
- FK는 참조할 필드가 있는 클래스만 지정하면 되고, FK로 지정된 필드는 _id 접미사가 붙음
- `__str__()` : 객체를 문자열로 표현할 때 사용하는 함수(쉘이나 Admin 사이트에서 테이블 명을 표시할 때 사용)

### 3.6.2 Admin 사이트에 테이블 반영

> 테이블을 새로 생성할 때는 models.py, admin.py 두개 파일을 수정해야함

1. polls/admin.py 파일에 테이블 등록

    ``` python
    from django.contrib import admin
    from polls.models import Question, Choice

    admin.site.register(Question)
    admin.site.register(Choice)
    ```

### 3.6.3 DB 변경사항 반영

> 테이블의 신규 생성, 정의 변경 등 DB에 변경이 필요한 사항이 있으면, DB에 실제로 반영해주는 작업(마이그레이션)이 필요함

**마이그레이션(migrations)**

> 테이블 및 필드의 생성, 삭제, 변경 등과 같이 DB에 대한 변경사항을 알려주는 정보
>
> 장고는 애플리케이션 디렉토리별로 마이그레이션 파일이 존재함

1. `python manage.py makemigrations` : polls/migrations 디렉토리 하위에 마이그레이션 파일 생성
2. `python manage.py migrate` : 마이그레이션 파일들을 이용해 DB에 테이블을 생성함
3. '192.168.0.2:8000/admin'에 접속하여 Choices, Questions 테이블 생성 확인

- `python manage.py sqlmigrate polls 0001` : migrate 명령으로 DB에 반영할 때 장고가 사용하는 SQL문을 직접 확인하는 방법

    ```sqlite
    >> python manage.py sqlmigrate polls 0001
    
    BEGIN;
    --
    -- Create model Question
    --
    CREATE TABLE "polls_question" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "question_text" varchar(200) NOT NULL, "pub_date" datetime NOT NULL);
    --
    -- Create model Choice
    --
    CREATE TABLE "polls_choice" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "choice_text" varchar(200) NOT NULL, "votes" integer NOT NULL, "question_id" integer NOT NULL REFERENCES "polls_question" ("id") DEFERRABLE INITIALLY DEFERRED);
    CREATE INDEX "polls_choice_question_id_c5b4b260" ON "polls_choice" ("question_id");
    COMMIT;
    ```

<br>

## 3.7 애플리케이션 개발하기 - View 및 Template 코딩

> Request부터 Response까지의 처리 흐름에 대한 로직 설계
>
> 4개의 URL과 4개의 뷰가 필요하고, 보여지는 페이지가 3개이므로 3개의 템플릿 파일이 필요함

### 처리 흐름 설계

​              URL                                       View                                   Template

/polls/                           $\rightarrow$                index()               $\rightarrow$            index.html

/polls/3/                       $\rightarrow$                detail()               $\rightarrow$            detail.html

/polls/3/vote               $\rightarrow$                vote()

​                                                             $\downarrow$  redirect

/polls/3/results/         $\rightarrow$                results()             $\rightarrow$            results.html

### URLconf 설계 - URL과 뷰 매핑

| URL 패턴          | 뷰 이름   | 뷰가 처리하는 내용                           |
| ----------------- | --------- | -------------------------------------------- |
| /polls/           | index()   | index.html 템플릿을 보여줌                   |
| /polls/3/         | detail()  | detail.html 템플릿을 보여줌                  |
| /polls/3/vote/    | vote()    | detail.html에 있는 폼을 POST 방식으로 처리함 |
| /polls/3/results/ | results() | results.html 템플릿을 보여줌                 |
| /admin            | -         | Admin 사이트를 보여줌(장고 기본 제공)        |

### 3.7.1 URLconf 코딩

> URLconf 코딩은 한 개 파일에 작성하는 방법, **두 개 파일에 나눠서 작성하는 방법(권장)**이 있음
>
> URL 패턴을 뷰에 매핑하였으나, 뷰 설계를 하지 않은 경우  runserver 구동 불가

- 한 개 파일에 작성하는 방법(상위 URLconf)

    > polls를 다른 이름으로 변경하는 경우, 4개의 패턴을 수정해야함
    
    1. ch3/mysite/urls.py에 URL과 뷰를 매핑하는 코드 작성
    
        ```python
            # 장고 제공 모듈 및 함수
            from django.contrib import admin 
            from django.urls import path 
            # views 모듈
            from polls import views

            urlpatterns = [
                path('admin/', admin.site.urls), # Admin 사이트를 사용하기 위한 코드
                path('polls/', views.index, name='index'), 
                path('polls/<int:question_id>/', views.detail, name='detail'),
                path('polls/<int:question_id>/results/', views.results, name='results'),
                path('polls/<int:question_id>/vote/', views.vote, name='vote'), 
        ```

    **path()**
    
    > `path('route', view, kwargs, name)`
    
    - route : URL 패턴을 표현하는 문자열(URL String)
    - **view** : **URL 스트링이 매칭되면 호출되는 View 함수, HttpRequest 객체와 URL 스트링에서 추출된 항목이 View 함수의 인자로 전달됨**
    - kwargs : URL 스트링에서 추출된 항목 외 추가적인 인자를 View 함수에 전달할 때 사용(Dictionary 타입)
    - name : 각 URL 패턴별로 이름을 붙임, 이름은 템플릿 파일에서 많이 사용됨

- 두 개의 파일에 작성하는 방법(상위 URLconf, 하위 URLconf)

    > 상위 URLconf에서 1개의 패턴만 수정하면됨
    >
    > 애플리케이션을 재사용하는 경우, 하위 URLconf를 그대로 가져가서 사용 가능

    1. ch3/mysite/urls.py 파일에 URL과 뷰를 매핑하는 코드 작성

        ```python
        # 장고 제공 모듈 및 함수
        from django.contrib import admin
    from django.urls import path, include
        
        urlpatterns = [
            path('admin/', admin.site.urls),
            path('polls/', include('polls.urls')), 
        # polls/로 시작되는 요청이 오면 polls/urls.py를 참조해서 매핑함
        ]
        ```
        
    2. ch3/polls/urls.py 파일에 URL과 뷰를 매핑하는 코드 작성
    
        ```python
        # 장고 제공 모듈 및 함수
        from django.urls import path
        # views 모듈
        from . import views
        
        # URL 패턴의 이름 충돌 방지를 위한 namespace
        # detail.html이 여러개일 경우 충돌 방지(e.g. polls:detail.html)
        app_name = 'polls' 
        
        urlpatterns = [
            path('', views.index, name='index'), 
            path('<int:question_id>/', views.detail, name='detail'), 
            path('<int:question_id>/results/', views.results, name='results'),
            path('<int:question_id>/vote/', views.vote, name='vote'), 
        ]
        ```

### 3.7.2 뷰 함수 index() 및 템플릿 작성

> 뷰 함수와 템플릿은 서로에게 영향을 미치기 때문에 보통 같이 작업을 진행하고,
>**템플릿**을 먼저 코딩하면 UI 화면을 생각하면서 로직을 풀어나가는 것이 쉬워짐

1. `mkdir templates/polls` : polls/template 디렉토리 및 template/polls 생성

2. 템플릿 및 View 함수 작성

    **템플릿 작성**
    {% raw %}
    > 템플릿 태그 형식 : {% tag %}
    >
    > 템플릿 변수 형식 : {{ variable }}
    {% endraw %}
    - templates/polls/index.html 에 index 템플릿 작성
        {% raw %}
        ```python
        # latest_question_list : index() 뷰 함수에서 넘겨주는 파라미터
        {% if latest_question_list %} # 객체를 순회하면서 리스트 태그 형태로 화면에 출력
        	<ul>
            {% for question in latest_question_list %} 
            	<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li> # 질문 선택 시 a 태그에 의해 URL이 넘어오면, URLconfig에 의해 detail() 뷰 함수가 호출됨
            {% endfor %}
            </ul>
        {% else %} # 객체가 없을 경우 아래 문구 출력
            <p>No polls are available.</p>
        {% endif %}
        ```
        {% endraw %}
    - templates/polls/detail.html 에 detail 템플릿 작성
        {% raw %}
        ```python
        <h1>{{ question.question_text }}</h1>
        
        # 에러 발생 시 출력하는 메시지(로직은 vote() 뷰 함수에 있음)
        {% if error_message %}<p><strong>{{ error_message }}</strong></p>{% endif %}
        
        <form action="{% url 'polls:vote' question.id %}" method="post">
        # 폼에 입력된 데이터는 POST 방식으로 polls:vote(/polls/3/vote/) URL로 보냄
        
        {% csrf_token %} # CSRF(Cross Site Request) 공격을 방지하기 위한 기능, 폼 태그 뒤에 입력
        
        {% for choice in question.choice_set.all %} 
        # 객체를 순회하면서 라디오 버튼과 라벨 출력
        
        	<input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}" /> 
            # 라디오 버튼을 선택하면 데이터가 'choice'='3'(choice.id)로 구성
            # forloop.counter : 루프를 실행한 횟수를 담고 있는 템플릿 변수
        
            <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label>
            # <label for> 속성과 <input id> 속성은 값이 같아야 서로 바인딩 됨
        <br />
            
        {% endfor %}
        
        <input type="submit" value="Vote" /> # submit 버튼
        </form>
        ```
        {% endraw %}
        - `{% raw %}{% url %}{% endraw %}`

            > 소스에 URL을 하드코딩(소스 코드 안에 데이터를 직접 기입)하는 것을 방지하기 위한 태그
            >
            > { % url '[namespace]:[view-name]' arg1 arg2 % }

            - namespace : urls.py 파일의 include() 함수 또는 app_name 변수에 정의한 네임스페이스
            - view-name : urls.py 파일에서 정의한 URL 패턴 이름
            - argN : 뷰 함수에서 사용하는 인자

        - **choice_set**

            - Question과  Choice 테이블의 관계는 1:N, 외래키로 연결되어 있음

            - 1:N 관계에서는 1 테이블에 연결된  N 테이블들의 항목이라는 의미로, xxx_set 속성을 디폴트로 제공함

                > question.choice_set.all : Question 테이블의 question 레코드에 연결된 Choice 테이블의 모든 레코드(1:N 외래키)

    - templates/polls/results.html 에 results 템플릿 작성
        {% raw %}
        ```python
        <h1>{{ question.question_text }}</h1>
        
        <ul>
        {% for choice in question.choice_set.all %}
            <li>{{ choice.choice_text }} -- {{ choice.votes }} vote{{ choice.votes|pluralize }}</li>
        {% endfor %}
        </ul>
        
        <a href="{% url 'polls:detail' question.id %}">Vote again?</a>
        ```
        
        - `{{ [값]|pluralize }}` : 앞의 값이 복수면 s를 붙여줌
        {% endraw %}
    **polls/views.py 에 index(), detail(), result(), vote() 뷰함수 설계**

    ```python
    from django.shortcuts import get_object_or_404, render
    from django.http import HttpResponseRedirect
    from django.urls import reverse
    
    from polls.models import Choice, Question
    
    from django.shortcuts import render # 장고 단축함수 임포트
    from polls.models import Question # Question 테이블에 엑세스하기 위한 Question 클래스 임포트
    
    # index() 뷰 함수
    def index(request): # request는 필수 인자
    
        latest_question_list = Question.objects.all().order_by('-pub_date')[:5]
        # question 테이블 객체에서 pub_date 필드를 역순으로 정렬하여 최근 Question 객체 5개를 저장
    
        context = {'latest_question_list': latest_question_list}
        # 템플릿에 넘겨주는 방식은 사전 타입으로, {'템플릿에 사용될 변수명':변수명에 해당하는 객체}를 저장
    
        return render(request, 'polls/index.html', context)
        # 해당 html 파일에 context 변수를 적용하여 사용자에게 보여줄 최종 HTML 텍스트를 만들고, HTML 텍스트 데이터를 담은 HttpResponse 객체 반환
    
    # detail() 뷰 함수
    def detail(request, question_id): # question_id는 URL 스트링에서 추출된 항목
        question = get_object_or_404(Question, pk=question_id)
        return render(request, 'polls/detail.html', {'question': question})
    
    # results() 뷰 함수
    def results(request, question_id):
        question = get_object_or_404(Question, pk=question_id)
        return render(request, 'polls/results.html', {'question': question})
    
    # vote() 뷰함수 및 리다이렉션
    def vote(request, question_id): # question_id는 URL 스트링에서 추출된 항목
        question = get_object_or_404(Question, pk=question_id)
        try:
            selected_choice = question.choice_set.get(pk=request.POST['choice'])
            # request.POST : 제출된 폼의 데이터를 담고 있는 객체
            # request.POST('choice') : 폼 데이터에서 키가 'choice'에 해당하는 값인 choice.id를 스트링으로 리턴
        except (KeyError, Choice.DoesNotExist):
            # 'choice'라는 키가 없으면 KeyError 익셉션 발생
            return render(request, 'polls/detail.html', { # 설문 투표 폼을 다시 보여줌
                'question': question,
                'error_message': "You didn't select a choice.",
            })
        else: # 익셉션이 발생하지 않는 정상 처리의 경우
            selected_choice.votes += 1
            selected_choice.save() # 변경사항을 Choice 테이블에 저장
            
            # POST 데이터를 정상적으로 처리하였으면, 항상 HttpResponseRedirect를 반환하여 리다이렉션 처리
            return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
            # reverse() : URL 패턴 명으로부터 URL 스트링을 구하는 함수
    ```

    - 단축함수(shorcut) : 웹 프로그램 개발 시 자주 사용되는 기능들을 장고는 내장함수로 제공하고 있음

        - `get_object_or_404([모델클래스], [검색 조건])` : 검색 조건에 맞는 객체를 조회하고, 없으면  Http404 익셉션을 발생시킴 
        - `render(request, [html 파일], [{'템플릿에 사용될 변수명':변수명에 해당하는 객체}])` : 해당 html 파일에 context 변수를 적용하여 사용자에게 보여줄 최종 HTML 텍스트를 만들고, HTML의 텍스트 데이터를 담은 HttpResponse 객체 반환

    - 리다이렉트(redirect)

        > **URL 포워딩**(URL forwarding)
        >
        > 넘겨받은 URL을 웹 브라우저가 열려고 하면 **다른 URL의 문서가 열리게 된다.**

        - `HttpResponseRedirect(리다이렉트할 타겟 URL)`

            > POST 방식의 폼 데이터를 처리하는 경우, 그 결과를 보여줄 수 있는 페이지로 이동시키기 위해 HttpResponseRedirect 객체를 리턴함