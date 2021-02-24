---
title: "[MariaDB] 스토어드 프로그램(Stored Program)"
category: MariaDB
---

# 10. 스토어드 프로그램

## 10.1 스토어드 프로시저(Stored Procedure)

> **자주 사용되는 일반적인 쿼리를 모듈화**하여 필요할 때마다 호출
>
> 쿼리문의 집합으로 어떠한 동작을 일괄 처리하기 위한 용도로 사용
>
> MariaDB에서 제공되는 프로그래밍 기능
>
> 여러 SQL문이나 숫자 계산 등 다양한 용도로 사용

#### 형식

생성

``` mariadb
DELIMITER $$ -- 앞으로 구분문자를 '$$' 로 설정(';' 로 생각)
CREATE PROCEDURE [스토어드 프로시저명](IN 또는 OUT) -- 프로시저 생성
BEGIN

	SQL 프로그래밍 코딩(SELECT문 사용 가능)
	-- SQL 프로그래밍 코딩에서 ';'가 사용되므로 DELIMITER를 다른 문자로 선언하지 않으면 여기서 구문이 종료됨

END $$ -- 구분문자에 의해 프로시저 생성 종료
DELIMITER ; -- 구분문자를 ';'로 다시 변경함

CALL [스토어드 프로시저명](); -- 프로시저 호출
```

조회 : ` SHOW CREATE PROCEDURE [프로시저명]` 

수정 : `ALTER PROCEDURE [프로시저명]`

삭제 : `ALTER PROCEDURE [프로시저명]`

#### 특징

- MariaDB 성능 향상
- 유지관리 간편
- 모듈식 프로그래밍 가능
- 보안 강화 가능

#### 예

```mariadb
DELIMITER $$
CREATE PROCEDURE userProc()
BEGIN
    SELECT * FROM userTBL;
END $$
DELIMITER ;

CALL userProc();
```

## 10.2 스토어드 함수(Stored Funciton)

> 사용자가 직접 만들어서 사용하는 함수
>
> 계산을 통해 하나의 값을 반환하는 데 주로 사용

#### 형식

생성

``` mariadb
DELIMITER $$
CREATE FUNCTION [스토어드 함수명]([파라미터])
	RETURNS [반환 형식]
BEGIN

	프로그래밍 코딩(SELECT문 사용 불가)
	RETURN 반환값;

END $$
DELIMITER ;

SELECT [스토어드 함수명](); -- 함수호출
-- ------------------------------------------------------------
SELECT getAgeFunc(1980) INTO @age1980;  -- 함수 반환 값을 변수에 저장
```

조회 : ` SHOW CREATE FUNCTION [함수명];`

수정 : `ALTER FUNCTION`

삭제 : `DROP FUNCTION`

#### 예

````mariadb
DELIMITER $$
CREATE PROCEDURE whileProc()
BEGIN
    DECLARE str VARCHAR(100); 
    DECLARE i INT; 
    DECLARE k INT;
    SET i = 2;
    
    WHILE (i < 10) DO 
        SET str = ''; 
        SET k = 1; 
        WHILE (k < 10) DO
            SET str = CONCAT(str, '  ', i, 'x', k, '=', i*k); 
            SET k = k + 1; 
        END WHILE;
        SET i = i + 1; 
        INSERT INTO guguTBL VALUES(str);
    END WHILE;
END $$
DELIMITER ;

CALL whileProc();
````



## 10.3 커서(Cursor)

> 테이블에서 여러개 행을 쿼리한 후, 쿼리의 결과인 행 집합을 한 행씩 처리하기 위한 방식
>
> '파일 처리' 프로그랭에서 한 행씩 읽거나 썻던것 처럼, '파일 포인터'가 **자동으로 다음 행을 가르키는 방식**과 비슷함
>
> **SELECT 한 결과를 한 행씩 받아서 처리해야할 때 사용**

#### 커서의 처리 순서

1. 커서 선언(DECLARE)
    - 커서에서 사용할 변수 선언 : `DECLARE [변수명] [데이터형식 `
    - 커서 자체 선언 : `DECLARE [커서명] CURSOR FOR [SQL문];`
2. 반복 조건 선언(더이상 읽을 행이 없을 경우, 실행할 내용 설정)
    -  루프문 탈출 : `IF [조건] THEN LEAVE [루프명]; END IF;`
3. 커서 열기(OPEN) : `OPEN [커서명];` 
4. 커서에서 데이터 가져오기(FETCH) : `FETCH [커서명] INTO [필드명]` 

5. 데이터 처리 : 조건문을 사용
6. 커서 닫기(CLOSE) : `CLOSE [커서명]` 

#### 예

```mariadb
DELIMITER $$
CREATE PROCEDURE cursorProc()
BEGIN
    DECLARE userHeight INT; -- 변수 선언
    DECLARE cnt INT DEFAULT 0; 
    DECLARE totalHeight INT DEFAULT 0;

    DECLARE endOfRow BOOLEAN DEFAULT FALSE;

    DECLARE userCuror CURSOR FOR -- 커서 선언
        SELECT height FROM userTBL;

    DECLARE CONTINUE HANDLER -- 행의 끝이면 endOfRow 변수에 TRUE를 대입 
        FOR NOT FOUND SET endOfRow = TRUE;

    OPEN userCuror; -- 커서 열기

    cursor_loop: LOOP
        FETCH  userCuror INTO userHeight; -- 커서에서 데이터 가져오기

        IF endOfRow THEN -- 더이상 읽을 행이 없으면 Loop를 종료
            LEAVE cursor_loop;
        END IF;

        SET cnt = cnt + 1;
        SET totalHeight = totalHeight + userHeight;        
    END LOOP cursor_loop;
    
    SELECT CONCAT('고객 키의 평균 ==> ', (totalHeight/cnt));

    CLOSE userCuror; -- 커서 닫기
END $$
DELIMITER ;

CALL cursorProc();
```



## 10.4 트리거(Trigger)

> 테이블에 삽입, 수정, 삭제 등의 작업(이벤트)이 발생할 때 자동으로 작동되는 개체
>
> 데이터 무결성을 위한 기능
>
> 테이블에 부착되는 프로그램 코드
>
> 변경된 데이터, 변경 일자, 변경한 사람 기록
>
> 트랜잭션 로그 기록이 남지 않는 것(TRUNCATE)은 적용 불가

### 트리거 생성

``` mariadb
CREATE TRIGGER [트리거명]
	[AFTER 또는 BEFORE] [INSERT 또는 UPDATE 또는 DELETE]
	ON [부착할 테이블명]
    FOR EACH ROW
```

### 트리거의 종류

#### AFTER

> 이벤트가 발생한 이후 작동하는 트리거

- 예 : 테이블이 삭제된 이후, 테이블의 예전값을 OLD 테이블에(임시 테이블) 저장

#### BEFORE

> 이벤트가 발생하기 전에 작동(COMMIT 전에 수행되었던 이벤트)하는 트리거

- 예 : 데이터가 입력되기 전, 새로운 값을 NEW 테이블(임시 테이블)에 저장

#### AFTER & BEFORE

- 예 : 데이터가 입력되기 전, 새로운 값을 NEW 테이블(임시 테이블)에 저장 
    + 테이블이 삭제된 이후, 테이블의 예전값을 OLD 테이블에(임시 테이블) 저장

```mariadb
DROP TRIGGER IF EXISTS testTrg;
DELIMITER $$ 
CREATE TRIGGER testTrg
    AFTER DELETE
    ON testTbl -- 해당 테이블이 삭제된 이후
    FOR EACH ROW -- 각 행마다 적용시킴
BEGIN
	SET @msg = '가수 그룹이 삭제됨' 
END $$ 
DELIMITER ;
```

