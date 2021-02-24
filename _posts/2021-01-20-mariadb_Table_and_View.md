---
title: "[MariaDB] 테이블과 뷰(Table & View)"
category: MariaDB
---

# 8. 테이블과 뷰

## 8.1 테이블

### 8.1.1 테이블 생성

``` mariadb
CREATE TABLE [테이블명]
( [필드명] [데이터형식] [AUTO_INCREMENT] [NULL 허용],
   ...
   PRIMARY KEY [키명] ([필드명]) -- 기본키 제약조건
   FOREIGN KEY(필드명) REFERENCES [기준테이블명]([필드명]) -- 외래키 제약조건
);
```

### 8.1.2 제약 조건

> 데이터의 무결성을 지키기 위한 제한된 조건
>
> 특정 데이터를 입력할 때 조건을 만족해야 입력가능하도록 설정 가능

#### MariaDB가 제공하는 제약조건

1. **Primary Key(기본키)**

    > 테이블에 존재하는 많은 행 데이터를 구분할 수 있는 식별자

    - 제약 조건

        - 기본 키로 지정된 필드의 값들은 중복 및 NULL값 입력 불가
        - 자동으로 클러스터형 인덱스 생성

    - 지정 방법

        - 테이블 생성 시
            - 필드명 옆에 ` PRIMARY KEY` 입력
            - CREATE TABLE문 하단에 `((CONSTRAINT)) [제약조건명] PRIMARY KEY [키명] ([필드명])` 입력
        - 이미 테이블이 생성된 경우

            ```mariadb
            ALTER TABLE [테이블 명]
            ADD CONSTRAINT [제약조건명] PRIMARY KEY [키명] ([필드명]);
            ```

    - 키 확인 방법

        - `SHOW KEYS FROM [테이블명];` 
        - `SHOW INDEX FROM [테이블명];`

2. **Foreign Key(외래키)**

    > 두 테이블 사이의 관계를 선언함으로써, 데이터의 무결성 보장
    >
    > 하나의 테이블이 다른 테이블에 의존됨

    - 제약 조건
        - 외래키 테이블에 데이터를 입력할 때 기준 테이블에 데이터가 존재하는지 확인 필요
        - 기준 테이블은 PK, Unique 제약조건이 설정되어 있어야 함
    - 지정 방법
        - CREATE TABLE문 하단에 `FOREIGN KEY(필드명) REFERENCES [기준테이블명]([필드명]) [옵션];`
            - 옵션
                - `ON UPDATE NO ACTION`, `ON DELETE NO ACTION` : 아무 일이 일어나지 않는 디폴트 옵션
                - `ON UPDATE CASCADE`, `ON DELETE CASCADE` : 기준 테이블의 데이터가 변경되었을 때 외래 키 테이블도 자동 적용 옵션

3. **UNIQUE(Alternate Key)**

    > 중복되지 않는 유일한 값을 입력해야 하는 조건
    >
    > NULL 값 허용
    >
    > ` UNIQUE ([필드명])` 

4. **CHECK**

    > 입력되는 데이터를 점검하는 기능
    >
    > 예 : 키(height)에 마이너스 값 불가 설정
    >
    > `CHECK ([조건])` 

5. **DEFAULT**

    > 값을 입력하지 않았을 때 자동으로 입력되는 기본값 정의
    >
    > `DEFAULT [값]` 

6. **NULL**

    > '아무 것도 없다'를 의미함
    >
    > 테이블의 NULL 값 허용여부 지정
    >
    > PRIMARY KEY가 설정된 필드 = 자동 NOT NULL
    >
    > NULL 값을 많이 입력한다면, 가변길이 문자형(VARCHAR)을 사용하는 것이 좋음

 ### 8.1.3 테이블 압축

> 대용량 테이블의 공간을 절약하는 기능
>
> INSERT하는 시간은 늘어나며, 평균 행의 길이나 데이터 길이는 작아짐
>
> 필드 뒤에 `ROW_FORMAT=COMPRESSED;` 를 붙임

### 8.1.4 임시 테이블

> 임시로 사용되는 테이블
>
> 세션 내에서만 존재 가능하며 닫히면 자동 삭제됨
>
> ` CREATE TEMPORARY TABLE [테이블명]`

- 임시 테이블 삭제 시점

1. DROP TABLE
2. SQL TOOL 종료
3. 클라이언트 프로그램 종료
4. MariaDB 서비스 재시작

### 8.1.5 테이블 삭제

> 외래키 제약 조건의 '기준 테이블'은 삭제 불가(외래키 테이블 부터 삭제 필요)
>
> `DROP TABLE [테이블명1], [테이블명2], ...` 

### 8.1.6 테이블 수정

```mariadb
ALTER TABLE [테이블명]
	[ADD 또는 DROP COLUMN 또는 CHANGE COLUMN 또는 DROP PRIMARY KEY] [필드명]
```

## 8.2 뷰

### 8.2.1 개념

> 일반 사용자 입장에서는 테이블과 동일하게 사용하는 개체
>
> 한 번 생성해 놓으면 테이블이라 생각하고 사용 가능함
>
> 보통은 읽기 전용으로 사용하나, 원래 테이블의 데이터를 수정할 수는 있음

### 8.2.2 장점

1. **보안에 도움**
    - 중요한 개인정보는 열람을 제한할 수 있음

2. **복잡한 쿼리를 단순화시킴**
    - 긴 쿼리를 자주 사용할 경우 뷰를 생성하여 확인