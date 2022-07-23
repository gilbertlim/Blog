---
title: "[MariaDB] SQL 고급"
category: MariaDB
---

# 7. SQL 고급

## 7.1 MariaDB의 데이터 타입

### 7.1.1 MariaDB 데이터 타입의 종류

> 자주 사용되는 데이터 타입만 표기하고, 전체 데이터 타입은 아래 링크 참조
>
> https://mariadb.com/kb/en/data-types/

#### 숫자

| 데이터형식 | 범위                        | 설명                             |
| ---------- | --------------------------- | -------------------------------- |
| SMALLINT   | -32,768 ~ 32,767            | 정수                             |
| INT        | -21억 ~ +21억               | 정수                             |
| BIGINT     | -900경 ~ +900경             | 정수                             |
| FLOAT      | -3.40E+38 ~ -1.17E-38       | 소수점 아래 7자리                |
| DOUBLE     | -1.22E-308 ~ 1.79E+308      | 소수점 아래 15자리               |
| DECIMAL    | $-10^{38}$+1 ~ +$10^{38}$-1 | 전체자리수, 소수점 이하의 자리수 |

#### 문자

| 데이터형식 | 설명                           |
| ---------- | ------------------------------ |
| CHAR(n)    | 고정길이 문자형                |
| VARCHAR(n) | 가변길이 문자형                |
| LONGTEXT   | 최대 4GB 크기의 TEXT 데이터 값 |
| LONGBLOB   | 최대 4GB 크기의 BLOB 데이터 값 |

- BLOB(Binary Large OBject) : 그림, 오디오, 기타 멀티미디어 오브젝트

#### 날짜와 시간

| 데이터형식 | 설명                |
| ---------- | ------------------- |
| DATE       | YYYY-MM-DD          |
| DATETIME   | YYYY-MM-DD HH:MM:SS |

### 7.1.2 변수의 사용

- 스토어드 프로시저나 함수 안에서 변수를 사용하려면 DECLARE문 필요

  ``` mysql
  SET @[변수명] = 변수값; -- 변수 선언 및 초기화
  SELECT @[변수명] -- 변수값 출력()
  ```

- LIMIT

    - ``` mysql
    SET @myVar1 = 3;
        PREPARE myQuery FROM 'SELECT Name, height FROM userTBL ORDER BY height LIMIT ?';
    EXECUTE myQuery USING @myVar1 ;
      ```
    
        - ` PREPARE [쿼리명] FROM '[쿼리문]'` : 쿼리 이름에 '쿼리문'을 준비만하고 실행하지 않음
    
        - ` EXECUTE [쿼리명] USING @[변수]`  : ` @[변수]` 가 쿼리문의 '?'로 처리된 부분에 대입되어 실행

### 7.1.3 데이터의 형식과 형 변환

#### CAST & CONVERT

표현식을 해당 데이터 형식으로 형 변환

```mysql
CAST([표현식] AS [데이터형식]([길이]))
CONVERT([표현식], [데이터형식]([길이]))
```

- ` SELECT CAST('2022$12$12' AS DATE);` : 형변환 후 결과 출력
- `SELECT CONVERT('2022$12$12', DATE);` : 형변환 후 결과 출력

#### CONCAT(CONCATENATE : 사슬 같이 잇다)

여러  문자열을 하나의 문자열로 합치기

`CONCAT([문자열1], [문자열2], ...)` : ` [문자열1][문자열2],...` 

#### 암시적 형변환

- 문자 + 문자 = 정수
    - `SELECT '5' + '5'` = 10
- CONCAT(문자, 숫자) = 문자
    - `SELECT CONCAT('DEC', 12)` = 'DEC12'
- 조건식
    - '숫자+문자' 조합 = 조건식에서 숫자로 계산
        - `SELECT 2 > '13GB';` : 0 반환
    - '문자만' = 0으로 계산
        - `SELECT 0 = 'GB';` : 1 반환

## 7.2 MariaDB의 내장 함수와 윈도 함수

### 7.2.1 내장 함수

> Python에서 제공하는 내장 함수들과 유사함
>
> 속도는 DB의 내장함수가 더 빠름
>
>  자세한 내장함수 리스트는 아래 링크 참조
>
> https://mariadb.com/kb/en/built-in-functions/

#### 종류

- 제어 흐름 함수(IF(), IFNULL(), NULLIF(), CASE

- 문자열 함수(ASCII(), CHAR(), BIT_LENGTH(), ...)
- 수학 함수(ABS(), SIN(), ...)
- 날짜 및 시간 함수(ADDDATE(), SUBDATE(), ...)
- 시스템 정보 함수(USER(), DATABASE(), ...)
- 기타(JSON_ARRAY(), ...)

### 7.2.2 윈도우 함수

> 행과 행 사이의 관계를 쉽게 정의하기 위해 제공되는 함수
>
> 복잡한 SQL을 쉽게 활용 가능
>
> OVER 절이 들어간 함수
>
> 집계함수, 비집계함수와 함께 사용됨
>
> https://mariadb.com/kb/en/window-functions/

#### 형식

``` mysql
[함수명]() OVER(PARTITION BY 또는 ORDER BY)
```

- ```mysql
    SELECT ROW_NUMBER() OVER(ORDER BY height DESC) "키큰순위", name, addr, height 
    	FROM userTBL -- 회원 테이블에서 키가 큰 순으로 순위를 정함
    ```

#### 윈도우 함수와 함께 사용되는 함수들

- 집계 함수(AVG(), SUM(), ...)
- 비 집계 함수(RANK(), ROW_NUMBER(), ...)
- 분석 함수(CUME_DIST(), LEAD(), ...)

### 7.2.3 피벗과 JSON

#### 피벗

> 한 열에 포함된 여러 값을 출력하고, 
>
> 여러 열로 변환하여 테이블 반환식을 회전하고 필요시 집계까지 수행하는 것

#### JSON(JavaScript Object Notation)

>  웹과 모바일 응용프로그램 등과 데이터를 교환하기 위한 개방형 표준 데이터 포맷
>
> Key, Value로 구성
>
> MariaDB에서는 JSON 데이터 형식을 지원함

## 7.3 조인(Join)

> 두 개 이상의 테이블을 서로 묶어서 하나의 결과 집합으로 만들어 내는 것
>
> 분리된 테이블들은 관계를 가짐
>
> 관계형 DB의 핵심

<img src = "https://dataschool.com/assets/images/how-to-teach-people-sql/sqlJoins/sqlJoins_7.png" width="50%" align="left">

그림 출처 : https://dataschool.com

### INNER JOIN(내부 조인)


> 조인 중에 가장 많이 사용되는 조인
>
> **조인 조건에 만족되는 행에 대해서만 JOIN이 발생**
>
> JOIN = INNER JOIN

``` mysql
SELECT [필드명]
	FROM [첫 번째 테이블] 
		INNER JOIN [두 번째 테이블]
		ON [조인될 조건]
	WHERE [검색 조건];
```

- 조건을 작성할 때 `[첫 번째 테이블의 별칭].[필드명]` 과 같이 테이블명을 통일하여 가독성을 높이는 것이 좋음
- 하지만, 테이블에 따라 검색 속도에 차이가 발생할 수 있음

``` mysql
SELECT *
	FROM buyTbl B INNER JOIN userTbl U
		ON B.userID = U.userID
	WHERE B.userID = 'JYP';
```

- buyTbl 테이블의 userID와 userTbl 테이블에 userID가 같은 행 중에  userID가 'JYP'인 행 출력

#### EXISTS

> 서브쿼리가 반환하는 결과가 테이블에 존재하는지 확인
>
> INNER JOIN과 동일한 결과 출력

``` mysql
SELECT * 
	FROM buyTbl B 
	WHERE EXISTS(SELECT * FROM userID U WHERE B.userID = U.userID);  
```

#### 세 개의 테이블 INNER JOIN

```mysql
SELECT *
	FROM buyTbl B 
		INNER JOIN userTbl U
			ON B.userID = U.userID
    	INNER JOIN prodTbl P
    		ON U.prodName = P.prodName
```

- '첫 번째 테이블과 두 번째 테이블을 INNER JOIN한 결과'에 '세 번째 테이블'을 조인함

### 7.3.2 OUTER JOIN(외부 조인)

> 조인 조건에 만족되지 않는 행까지도 포함하여 JOIN이 발생

``` mysql
SELECT [필드명]
	FROM [첫 번째 테이블(LEFT 테이블)]
		[LEFT or RIGHT] ((OUTER)) JOIN [두 번째 테이블(RIGHT 테이블)]
		ON [조인될 조건]
   	WHERE [검색 조건];
```

#### LEFT OUTER JOIN (= LEFT JOIN)

> INNER 조인 결과 + LEFT 테이블만 가지고 있는 행(RIGHT 테이블에 없는 행은 NULL값 처리)

#### RIGHT OUTER JOIN(= RIGHT JOIN)

> INNER 조인 결과 + RIGHT 테이블만 가지고 있는 행(LEFT 테이블에 없는 행은 NULL값 처리)

#### FULL OUTER JOIN

>LEFT OUTER JOIN 결과 + RIGHT OUTER JOIN 결과 UNION(행으로 합치고, 중복 제거)

- 대부분의 DB(MariaDB 포함)에서는 FULL OUTER JOIN을 지원하지 않으므로 UNION 사용

### 7.3.3 CROSS JOIN(상호 조인)

> 한 쪽 테이블의 모든 행들과 다른 쪽 테이블의 모든 행을 JOIN
>
> 결과 개수 : 두 테이블의 개수의 곱
>
> 카티션 곱(Cartesian Product)이라고도 불림

### 7.3.4 SELF JOIN(자체 조인)

> 자기 자신과 자기자신이 조인
>
> 예 : 조직도

### 7.3.5 UNION / UNION ALL / NOT IN / IN

#### UNION

> 두 테이블의 결과를 행으로 합치는 것
>
> 조건 : 두 테이블의 열의 개수 동일, 데이터 형식 동일/호환
>
> 중복된 열은 제거되어 출력
>
> `[SQL1] UNION [SQL2]`

#### UNION ALL

> UNION과 기능/조건은 동일하나, 중복된 열까지 모두 출력
>
> `[SQL1] UNION ALL [SQL2]`

#### NOT IN

> 첫 번째 쿼리의 결과 중 두 번째 쿼리에 해당하는 것을 제외
>
> `[SQL1] NOT IN [SQL2]`

#### IN

> 첫 번째 쿼리의 결과 중 두 번째 쿼리에 해당하는 것만 출력
>
> `[SQL1] IN [SQL2]`

## 7.4 SQL 프로그래밍

#### 스토어드 프로시저

스토어드 프로시저 생성

``` mysql
DELIMITER $$
CREATE PROCEDURE [스토어드 프로시저 이름]()
BEGIN
	SQL...
END $$
DELIMITER ;
```

스토어드 프로시저 호출

` CALL [스토어드 프로시저 이름]();` 

### 7.4.1 IF ELSE

```mysql
IF [조건식] THEN
	[SQL1, ...]
ELSE
	[SQL1, ...]
END IF;
```

### 7.4.2 CASE

``` mysql
CASE
	WHEN [조건식1] THEN
		SET [변수 초기화]; 
	WHEN [조건식2] THEN
		SET [변수 초기화]; 	
	...
	ELSE
		SET [변수 초기화];
END CASE;
```

### 7.4.3 WHILE과 ITERATE/LEAVE

```mysql
[별명]:WHILE [조건식] DO
	[SQL1, ...]
END WHILE;
```

#### ITERATE

- ` ITERATE [WHILE문의 별명];`

-   Python에서 CONTINUE

#### LEAVE

- ` LEAVE [WHILE문의 별명];`

- Python에서 BREAK

### 7.4.4 오류 처리

`DECLARE [액션] HANDLER FOR [오류조건] [처리할 문장];` 

- 액션 : `CONTINUE` or `EXIT`
- 오류 조건 : 어떤 오류를 처리할 것인지 지정(상태 코드 등)
- `DECLARE CONTINUE HANDLER FOR 1146 SELECT '테이블이 없습니다.' AS '메시지';` 

### 7.4.5 동적 SQL

> 미리 쿼리문을 준비한 후 나중에 실행하는 것

#### PREPARE ... EXECUTE

``` mysql
PREPARE [쿼리명] FROM '?를 포함한 SQL문 '
EXECUTE [쿼리명] USING @[변수명]
DEALLOCATE PREPARE [쿼리명]
```

- `REPARE [쿼리명] FROM '?를 포함한 SQL문 '` : 쿼리 준비
- ` EXECUTE [쿼리명] USING @[변수명]`  : ` USING @변수` 가 쿼리문의 '?'로 처리된 부분에 대입되어 실행
- `DEALLOCATE PREPARE [쿼리명]`
    - 실행 후 문장 해제(실행한 이후 EXECUTE문을 실행하여도 ?에 대입되어 실행되지 않음)
    - PREPARE 문을 해제하는 것