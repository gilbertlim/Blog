---
title: "[MongoDB] MongoDB 쿼리(Query)"
category: MongoDB
---

# 3. 쿼리 작성하기

## 실습 DB 구축

> 실습을 위해 JSON or XML 형식의 파일을 MongoDB에 입력

##### 1. 복사할 DB 경로 진입

##### 2. `mongoimport -d [DB명] -c [컬렉션명] --file [파일명]` 

해당 파일의 데이터를 입력한 DB, 컬렉션의 도큐먼트로 저장

<br>

## 실습 DB 소개

<img src="https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6eb65c9d-6a60-4b54-95c9-144662486965%2FUntitled.png?table=block&id=a22959cb-da9c-42fc-8cad-b3a71885a648&width=3010&userId=&cache=v2" width="75%" align="center">

<br>

## 3.1 쿼리의 구조

> db.collection.find(query, projection)

- 값으로 쿼리
    - `db.area.find({city_or_province:"서울"}).pretty()`
- **연산자 사용(https://docs.mongodb.com/manual/reference/operator/query/)**
    - 서울시 중 300,000 이상의 인구수를 가진 구
        - `db.area.find({population: {$gte:300000}, city_or_province:"서울"}).pretty()`
    - 1월의 사망자수가 0이면서 사고율이 100 미만인 지역
        - `db.by_month.find({"month_data.0.death_toll":0, "month_data.0.accident_count": {$lte: 100}}).pretty()`
- 실습
    - 강릉시 교차로 내에서 일어난 총 사고 숫자
        - `db.by_road_type.find({county:"강릉시"}, {"교차로내.accident_count": 1}).pretty()`
    - 도로 종류 중에 기타단일로에서 사망자수가 0인 지역
        - `db.by_road_type.find({"기타단일로.death_toll":0}, {city_or_province:1, county:1}).pretty()`

<br>

## 3.2 논리, 비교 연산자

- `$not, $or, $and, $nor`
- 실습
    - 차대차 사고에서 100회 이상 사고가 났지만, 사망자 수가 0회인 지역
        - `db.by_type.find({type:"차대차", accident_count: {$gte : 100}, death_toll: 0}, {city_or_province:1, county:1}).pretty()`
    - 차대사람 사고 중 사망자수가 0회이거나 중상자수가 0회인 지역
        - `db.by_type.find({type:"차대사람", $or:[{death_toll:0},{heavy_injury:0}]}, {city_or_province:1, county:1}).pretty()`

## 3.3 문자열 연산자

- $regex

    > 정규 표현식과 맞는 도큐먼트 선택
    >
    > - 연산자 없이 사용(자주 사용됨)
    >
    >     >  `{ [필드명]: /[정규식]/[옵션] } }`
    >
    > - 연산자 사용(한 필드에 여러 연산자를 적용하려고 할 때 사용)
    >
    >     > e.g. `{ [필드명]: { $regex: /article0[1-9]/i, $not: /article03/ } }`
    >
    > https://docs.mongodb.com/manual/reference/operator/query/regex/

- $text 연산자

    > **문자열 검색**
    >
    > `{ $text: { $search: [값], [기타 옵션]:[값], ...  } }`
    >
    > - $search : 검색하려는 내용, 띄어쓰기 포함
    > - 기타 옵션 : 사용하는 언어 설정, 대소문자 구분 여부, 첨자 기호 허용 여부
    >
    > $text 연산자를 사용하기 위해서는 문자열 인덱스를 먼저 만들어야 함

    - 문자열 인덱스 생성
        - `db.[컬렉션명].createIndex( { [필드명] : "text", [필드명]: "text", ... } )`

- 실습
    - 2차 지역명이 시라는 이름으로 끝나는 지역의 수
        - ` db.area.find({county: /시$/}).count()`
    - 2차 지역명이 군이면서 인구수가 10만 이상인 곳
        - `db.area.find({county:/군$/, population : {$gte: 100000}}).count()`
    - 2차 지역명이 구로 끝나면서, 이름의 첫 글자의 초성이 "ㅇ"인 2차 지역명
        - `db.area.find({county: {$regex: /구$/, $gte: "아", $lt: "자"}}, {county:1})`

