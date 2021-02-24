---
title: "[MariaDB] SQL 기본"
category: MariaDB
---

# 6. SQL 기본

- 데이터 처리의 기본 기능 : CRUD(Create, Read, Update, Delete)

- 대소문자를 구분하지 않으나 대문자 또는 소문자 등으로 통일하는 것이 좋음

## 6.1 SELECT문

### 6.1.1 원하는 데이터를 가져와 주는 기본적인 구문

> 가장 많이 사용하는 구문으로, DB 내 테이블에서 원하는 정보를 추출하는 명령
>
> `SELECT [필드명] FROM [테이블명] WHERE [조건];` : 테이블에서(FROM) 조건에 맞는 필드의 정보를 조회

#### SELECT 활용 구문

- `SELECT * FROM [테이블명];` : 테이블 내 전체 데이터 조회
- `SELECT * FROM [테이블명] LIMIT [정수];` : 테이블 내 전체 데이터 중 상위 '정수'개 조회
- `SELECT COUNT(*) FROM [테이블명]` : 테이블 내 데이터의 개수 확인 
- `SELECT [필드명1], [필드명2], ... FROM [테이블명];` : 테이블 내 해당 필드의 데이터 조회

- `[DB명].[테이블명]` : 해당 DB에 있는 테이블을 의미
- `[필드명] AS [별칭]` : 필드명의 별칭 지정(공백이 있을경우 홑따옴표(' ')로 감싸야 함)

#### 정보 조회

- `SHOW TABLE STATUS` : 해당 DB의 테이블 정보 조회
- `DESC [테이블]` : 해당 테이블의 필드 정보 조회

#### USE 구문

> SELECT문을 사용하려면 먼저 사용할 DB 지정이 필요함
>
> `USE [DB명]` : 사용할 DB 지정(앞으로 모든 쿼리는 해당 DB에서 수행하라는 의미) 

### 6.1.2 특정한 조건의 데이터만 조회하는 구문

> `SELECT [필드명] FROM [테이블명] WHERE [조건];`

#### 기본 구문

- `WHERE [조건1] [연산자] [조건2]`  : 조건 연산자(=, <, ...)와 관계 연산자(NOT, AND, OR, ...) 사용

#### BETWEEN ... AND & IN() & LIKE

- `WHERE [필드명] BETWEEN [값1] AND [값2]` : 필드의 값 중 값1~값2에 포함되는 값이 있는 경우
- `WHERE [필드명] IN (리스트)`  : 필드의 값 중 리스트에 포함되는 값이 있는 경우
- `WHERE [필드명] LIKE '[문자열과 연산자의 조합]'` : 문자열의 내용 검색
    - '김%' : '김'이 제일 앞글자인 데이터
    - '_종신' : 맨 앞 글자가 한 글자이고, 그다음이 '종신'인 데이터

#### 서브 쿼리(SubQuery)

- `WHERE [필드명] [연산자] ([서브쿼리])`  : 서브쿼리에서 나온 결과를 값으로 사용(비교값의 데이터타입 일치 필요)
    - ` SELECT Name, height FROM userTbl WHERE height > (SELECT height FROM userTbl WHERE name='김경호');` 
- `WHERE [필드명] [연산자] ANY ([서브쿼리])` : 서브쿼리의 여러 결과 중 한가지만 만족해도 되는 조건
    - `SELECT Name, height FROM userTbl WHERE height >= ANY (SELECT height FROM userTbl WHERE addr = '경남');`
- `WHERE [필드명] =ANY ([서브쿼리])` : `WHERE [필드명] IN ([서브쿼리])`과 동일한 구문
- `WHERE [필드명] [연산자] SOME ([서브쿼리])` : 서브쿼리의 여러 결과 중 한가지만 만족해도 되는 조건
- `WHERE [필드명] [연산자] ALL ([서브쿼리])` : 서브쿼리의 여러 결과를 모두 만족해야하는 조건

#### 정렬

- `ORDER BY [필드명]` : 해당 필드를 기준으로 오름차순 정렬

- `ORDER BY [필드명] DESC` : 해당 필드를 기준으로 내림차순 정렬
- `ORDER BY [필드명1] [정렬기준2], [필드명1] [정렬기준2], ...;`  : 첫 기준에서 값이 같은 데이터는, 다음 기준에따라 정렬
    - `SELECT Name, height FROM userTbl ORDER BY height DESC, name ASC;`

#### 중복된 것을 하나만 남김

- `SELECT DISTINCT [필드명] FROM [테이블명]` : 중복된 것을 1개씩만 출력

#### 출력 개수 제한

- `SELECT [필드명] FROM [테이블명] LIMIT [정수]` : 상위 '정수'개만 출력

#### 테이블 복사

> `CREATE TABLE [새로운테이블명] (SELECT [복사할필드명] FROM [기존테이블명]);` 

- PK, FK 등 제약 조건은 복사되지 않음
- 테이블을 테이블로 복사
- 열 정보를 테이블로 복사
- `CREATE TABLE buyTbl3 (SELECT userID, prodName FROM buyTbl);`

### 6.1.3 GROUP BY 및 HAVING 그리고 집계 함수

#### GROUP BY

> 해당 필드별로 묶어 집계함수에 따라 계산
>
> `SELECT [필드명] [집계함수] FROM [테이블명] GROUP BY [필드명];` 

- ` SELECT userID, SUM(amount) FROM buyTbl GROUP BY userID;`  : 사용자별 구매양의 총합 집계

#### 집계 함수

> SUM() : 합
>
> AVG() : 평균
>
> MIN() : 최솟값
>
> MAX() : 최댓값
>
> COUNT() : 행의 개수
>
> COUNT(DISTINCT) : 행의 개수(중복은 1개만 인정)
>
> STDEV() : 표준편차
>
> VAR_SAMP() : 분산

#### HAVING

> 집계 함수에 대해 조건을 제한하는 절(집계함수는 WHERE절에 나타날 수 없음)
>
> GROUP BY 절 뒤에 위치
>
> `GROUP BY [필드명] HAVING [조건]`

- `SELECT userID AS '사용자', SUM(price*amount) AS '총구매액' FROM buyTbl GROUP BY userID ;`
- `SELECT userID AS '사용자', SUM(price*amount) AS '총구매액' FROM buyTbl GROUP BY userID HAVING SUM(price*amount) > 1000;` 

#### ROLLUP

> 총합 또는 중간 합계가 필요한 경우 사용하는 절
>
> '각 그룹별' 하단에 집계함수 값의 합계, 최종 행에 '전체 데이터'의 합계 출력
>
> GROUP BY 절 뒤에 위치
>
> `GROUP BY [필드명1], [필드명2], ... WITH ROLLUP;` 

- ` SELECT groupName, SUM(price * amount) AS '비용' FROM buyTbl GROUP BY  groupName WITH ROLLUP;` : 총합 출력

- `SELECT groupName, num, SUM(price * amount) AS '비용' FROM buyTbl GROUP BY  groupName, num WITH ROLLUP;` : 부분합, 총합 출력

### 6.1.4 SQL의 분류

#### DML(Data Manipulation Language)

> 데이터 조작(선택, 삽입, 수정, 삭제)에 사용되는 구문

- SELECT, INSERT, UPDATE, DELETE, 트랜잭션이 발생하는 SQL
- 트랜잭션(Transaction)
    - 테이블의 데이터 변경 시 실제 테이블에 완전히 적용하지 않고, 임시로 적용시키는 것
    - COMMIT을 해야 트랜잭션이 실제 테이블에 적용됨
- SELECT도 트랜잭션을 발생시키기는 하지만, 다른 구문과 성격이 조금 다름

#### DDL(Data Definition Language)

> DB, 테이블, 뷰, 인덱스 등 DB 개체를 생성, 삭제, 변경하는데 사용되는 구문

- CREATE, DROP, ALTER
- 트랜잭션을 발생시키지 않으므로, 실행 즉시 DB에 바로 적용됨

#### DCL(Data Control Language)

> 사용자에게 어떤 권한을 부여하거나 빼앗을 때 주로 사용하는 구문

- GRANT, REVOKE, DENY

## 6.2 데이터 변경을 위한 SQL

> SELECT FROM
>
> INSERT INTO
>
> UPDATE SET **WHERE**
>
> DELETE FROM **WHERE**

### 6.2.1 데이터 삽입 : INSERT

>`INSERT INTO [테이블명]([필드명1], [필드명2], ...) VALUES ([값1], [값2], ...)` 

- 필드명을 생략할 경우, 삽입할 값과 필드의 **순서, 개수, 데이터 타입**이 일치해야 함
- 특정 필드를 생략하고 값을 삽입할 경우, NULL값이 들어가기 때문에 해당 필드가 NULL값을 허용하지 않을 경우 필드를 생략할 수 없음

#### AUTO_INCREMENT

- 자동으로 1부터 증가하는 값을 입력하는 필드의 속성
- 필드의 속성을 AUTO_INCREMENT로 지정할 때, 필드를 PK 또는 UNIQUE로 지정 필요
- 숫자 형식만 사용 가능

- `CREATE TABLE [테이블명] ([필드명] int AUTO_INCREMENT=[시작값]);` : 테이블 생성 시 증가값 설정
- `ALTER TABLE [테이블명] AUTO_INCREMENT=[시작값];` : 테이블 생성 이후 증가값 변경
- `SET @@auto_increment_increment=[정수];` : 증가값 변경

#### 대량의 데이터 삽입

- `INSERT INTO [테이블명] ([필드명1], [필드명2], ...) SELECT [필드명] FROM [DB명].[테이블명]`  : 기존 테이블에 삽입
- `CREATE TABLE [테이블명] (SELECT [필드명] FROM [DB명].[테이블명])` : 신규 테이블에 삽입

### 6.2.2 데이터 수정 : UPDATE

> UPDATE [테이블명] SET [필드명1]=[값1], [필드명2]=[값2], ... WHERE [조건];

- **조건문을 꼭 지정하여 사용하는 습관 필요**
- 사용 전 SELECT 문을 사용하여 조건에 맞는 데이터를 조회하여 확인
- `UPDATE testTbl4 SET Lname='없음' WHERE Fname = 'Kyoichi';`

### 6.2.3 데이터 삭제 : DELETE FROM

>  DELETE FROM [테이블명] WHERE [조건];

- **조건문을 꼭 지정하여 사용하는 습관 필요**
- ` DELETE FROM testTbl4 WHERE Fname='Aamer';` 
- ` DELETE FROM testTbl4 WHERE Fname='Mary' LIMIT 5;` 

#### DELETE, DROP, TRUNCATE

테이블 삭제 관련 구문

- DELETE(권장)
    - 트랜잭션 로그 기록 > 성능 나쁨
    - Affected Rows 표시
    - 테이블 구조는 남음
    - `DELETE FROM bigTBL1;`
- DROP
    - 테이블 자체 삭제
    - `DROP TABLE bigTBL2;`
- TRUNCATE
    - 트랜잭션 로그 기록 X > 빠름
    - 테이블 구조는 남음
    - `TRUNCATE TABLE bigTBL3;`

### 6.2.4 조건부 데이터 입력, 변경

#### 여러 건을 입력하는데 오류가 생겨도 진행하는 방법

- INSERT IGNORE
    - 에러가 생겨도 그 뒤로 입력이 되도록 하는 구문
    - 보통 에러가 생기면, 그 다음 데이터는 입력되지 않음
- ON DUPLICATE UPDATE
    - PK가 중복되지 않으면 일반 INSERT
    - PK가 중복되면 그 뒤의 UPDATE문 수행

## 6.3 WITH절과 CTE

### 6.3.1 WITH절과 CTE 개요

- WITH 절은 CTE를 표현하기 위한 구문

- CTE(Common Table Expression)
    - 기존의 뷰, 파생 테이블, 임시 테이블 등으로 사용되던 것을 대신할 수 있음
    - 더 간결한 식으로 보여짐
    - 비재귀적 CTE, 재귀적 CTE

### 6.3.2 비재귀적 CTE

```mysql
WITH [CTE_테이블명]([필드명])
AS
(
	쿼리문
)
SELECT [필드명] FROM [CTE_테이블명];
```

```mysql
WITH abc(userid, total)
AS
(SELECT userid AS '사용자', SUM(price * amount) AS '총구매액' FROM buyTbl GROUP By userid)
SELECT * FROM abc ORDER BY total DESC;
```

- abc 테이블의 필드 userid, total을 조회하는데 total을 기준으로 내림차순 정렬
- abc 테이블은 AS 이후에 나온 서브 쿼리를 뜻함