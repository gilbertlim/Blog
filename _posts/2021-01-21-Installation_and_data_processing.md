---
title: "[MongoDB] MongoDB 설치 및 데이터 처리"
category: MongoDB
---

# 2. MongoDB 설치와 데이터 처리

## 2.1 MongoDB 설치와 환경 설정

> MongoDB는 Homebrew master 저장소에 포함되어있지 않으므로, 다른 저장소에서 가져와야 함
>
> Homebrew가 다른 공식 저장소를 이용할 수 있도록 하는 방법 : ` brew tap [저장소 이름]` 

#### 1. `brew tap mongodb/brew` 

Homebrew에서  mongodb/brew 저장소 엑세스

#### 2. `brew install mongodb-community`

MongoDB community server 설치

- `mongo --version` : 설치된 버전 확인  

## 2.2 MongoDB 시작과 종료

### DB 접속

#### 1. `brew services start mongodb-community`

MongoDB 서버 시작

#### 2. `mongo`

MongoDB Shell 진입서버 접속

### DB 종료

#### 1. `db.logout()`

로그 아웃

#### 2. `brew services stop mongodb-community`

MongoDB 서버 종료

#### MongoDB 쉘에서 종료하는 방법

1. `use admin`
2. `db.shutdownServer()`
3. ` exit`

## 2.3 MongoDB 데이터 처리

### 2.3.1 DB

#### 인스턴스

> 특정한 순간 DBf에 저장되어 있는 정보의 모임

#### Default DB

- admin
    - root DB. 
    - root DB에 추가된 사용자는 모든 DB에 대하여 모든 권한을 획득함
    - 서버 전역에 걸친 모든 명령어는 이곳에서만 실행 가능함

- local
    - 특정 서버에만 보관하는 정보를 담는 곳(복제 불가능)

- config
    - 샤드 정보를 저장하는데 사용됨

#### DB 생성 및 삭제

- `use [DB명]` : DB 생성 및 이동
- `db.dropDatabase()` : DB 삭제
- DB명을 바꾸는 기능은 없음

#### DB 정보 조회

- `db`  : 현재 접속된 DB 확인
- `show dbs` : DB 목록 확인
- `db.getCollectionInfos()` : 현재 DB의 컬렉션들의 정보를 리스트로 반환
- `db.serverStatus()` : 호스트, 프로세스ID, Lock 옵션, 스토리지 엔진이름 등 정보 제공
- `db.stats()` : DB 내의 컬렉션, 뷰, 오브젝트 개수와 크기에 대한 통계 제공

#### 주석 기입

- `// [주석]` : 주석 기입

### 2.3.2 컬렉션

#### 도큐먼트 형식

```
{
   field1: value1,
   field2: value2,
   field3: value3,
   ...
   fieldN: valueN
}
```

#### 컬렉션의 종류

- Capped Collection(Overwriting) / `capped:true`
    > 최초 제한된 크기로 생성된 공간 내에서만 데이터를 저장할 수 있고,
    > 만약 최초 공간이 모두 사용되면 다시 처음으로 돌아가서(오래된 데이터부터 삭제) 기존 공간을 **재사용**함
    > 로그 데이터, 일정 시간만 보관하는 통계 데이터 보관 시 유용함
    >
    > 컬렉션의 크기가 4096바이트보다 더 크다면, 256의 정수 배로 증가시킴(10,000 $\rightarrow$ 10240(256*40))
    >
    > 실제 저장된 크기와 컬렉션 크기사이에 오차가 있음(컬렉션에 대한 정보가 함께 저장되기 때문)
- Non Capped Collection(Non overwriting) / `capped:false`

    > RDB의 테이블처럼 디스크 공간이 허용하는 범위 내에서 데이터를 계속적으로 저장

#### 컬렉션 생성 및 삭제

##### 컬렉션 생성

> 컬렉션의 구성 요소(필드명, 데이터 타입, 길이 등)가 결정되어 있지 않더라도 데이터 저장 구조를 생성

- ` db.createCollection("[컬렉션명]", {capped:[true 또는 false], size:[제한할 크기(Byte)]})` : 컬렉션 생성**(첫 번째 컬렉션 생성 시 DB 자동 생성)**
- `db.[컬렉션명].renameCollection("[컬렉션명]")` : 해당 컬렉션의 이름 변경

##### 컬렉션 삭제

- `db.[컬렉션명].drop()` : 해당 컬렉션 삭제(**모든 컬렉션이 삭제되면 DB 자동 삭제**)

##### 컬렉션 정보 조회


- `show collections` : 현재 DB의 컬렉션 리스트 확인
- `db.[컬렉션명].stats()` : 컬렉션의 크기, 도큐먼트 개수, 스토리지 엔진의 통계 제공
- `db.[컬렉션명].isCapped()` : Capped 컬렉션일경우 true 반환
- `db.[컬렉션명].latencyStats()` : 컬렉션의 지연 시간 통계 반환
- `db.[컬렉션명].storageSize()` : 컬렉션 스토리지 크기 반환
- `db.[컬렉션명].totalIndexSize()` : 컬렉션의 인덱스 크기 반환
- `db.[컬렉션명].totalSize()` : 컬렉션 스토리지 + 인덱스 크기 반환

#### 뷰

> MongoDB는 RDBMS에 존재하는 뷰와 동일한 기능을 제공함

### 2.3.3 데이터 입력, 수정, 삭제

#### 데이터 타입

- 기본 타입

    - Null(null), 정수형(int, long), 실수형(double), 문자열(string), Boolean(boolean)
    - Object(object) : 임베디드 도큐먼트(Embeded Document)라고도 불리며, 도큐먼트 내부의 값으로 도큐먼트를 갖는 것
    - Array(array) : 배열 속 임베디드 도큐먼트

- 시간

    - 타임스탬프

        > 출력 형식 : Timestamp(유닉스 시간, 같은 유닉스 시간 내 몇번 째인지)
        >
        > **같은 시각에 만들어도, 2번째 인자의 값이 다르기 때문에 하나의 인스턴스에서 같은 타임스탬프는 존재할 수 없음**

        - `db.[컬렉션명].insert({[필드명]: new Timestamp()})` : 해당 컬렉션에 타임스탬프 삽입
        - `ObjectId("[오브젝트ID]").getTimestamp()` : 해당 도큐먼트ID의 타임스탬프 출력

    - 날짜

        > 출력 형식 : ISOdate("날짜T시각Z")
        >
        > JavaScript Date 객체 관련 문법 참고

        - ` db.[컬렉션명].insert({[필드명]: new Date()})`  : 해당 컬렉션에 날짜 삽입

    - ObjectID

        > **MongoDB는 도큐먼트를 생성할 때마다 PK로 _id 필드를 자동적으로 생성함**
        >
        > _id 값을 알게 되면 하나의 도큐먼트를 특정지을 수 있음
        >
        > >  출력형식
        > >
        > > - ObjectID("유닉스시간 기기ID 프로세스ID 카운터")
        > >
        > > - 예 : 600d9a58 bd12ff 562d 4869b1
        > > - 카운터는 랜덤 값부터 시작하므로, 도큐먼트가 동시에 같은 조건에서 생성되더라도 서로 다른 ObjectId를 가짐

#### 데이터 입력

> 데이터 입력 시 컬렉션이 없다면, 컬렉션을 생성하면서 데이터를 저장함

- **Save**

    > 하나의 도큐먼트에 특정 필드만 변경하더라도 도큐먼트 단위로 데이터를 변경하는 방법
    >
    > 도큐먼트가 존재하면 update, 존재하지 않으면 insert 
    >
    > **도큐먼트 단위로 데이터를 변경하는 경우 효율적**, 필드 단위로 변경하는 경우에는 Update문이 효율적

    - 변수를 활용한 데이터 입력
        1. `[변수명]={[필드명]:[값]}` : 변수 초기화
        2. `db.[컬렉션명].save([변수명])` : 변수에 저장된 데이터를 컬렉션에 입력
    - 컬렉션에 직접 데이터 입력(**동일한 _id에 데이터를 입력할 경우, 덮어 씀**)
        - `db.[컬렉션명].save({[필드명1]:[값1], ...})`
    - 반복문을 활용한 데이터 입력
        - `for (var n = 1103; n <= 1120; n++) db.things.save({n : n , m : "test"})`

- **Insert**

    > 컬렉션에 하나의 도큐먼트를 최초 저장할 때 일반적으로 사용되는 메서드
    >
    > **동일한 _id에 데이터 입력 불가**

    - `db.[컬렉션명].insert({[필드명1]:[값1], ...})` : 단일 또는 다수의 도큐먼트 입력
    - `db.[컬렉션명].insertOne({[필드명1]:[값1], ...})` : 단일 도큐먼트 입력
    - `db.[컬렉션명].insertMany([{[필드명1]:[값1], ...}])` : 다수의 도큐먼트 입력
    - `db.[컬렉션명].[Insert 메서드]({_id: [값], ...})`  : _id 필드에 값을 직접 설정하여 도큐먼트 입력

- **원자성**

    > 모 아니면 도와 같은 상태만 존재하고, 중간 상태가 존재하지 않는 성질
    >
    > InsertMany 메서드는 원자성이 성립하지 않아 중간에 오류가 발생할 경우, 오류가 발생 전의 도큐먼트는 입력됨

#### 데이터 조회

- **find 메서드**

    > find([쿼리], [프로젝션]) : **쿼리 연산자를** 이용하고(**쿼리**), **반환할 필드를 선택**(**프로젝션**)하여 도큐먼트를 조회

    - ` db.[컬렉션명].find()` : 컬렉션에 저장된 모든 도큐먼트 반환
    - ` db.[컬렉션명].find({[필드명]:[값]})`  : '필드명이 값'인 도큐먼트 검색

- **기타 메서드**
  
    - `db.[컬렉션명].insertedId` : 도큐먼트의 Id 반환
- `db.[컬렉션명].find().pretty()` : 한 눈에 알아볼 수 있도록 들여쓰기하여 출력
    
    - `it` : 출력된 결과가 20개 이상일 경우, 추가 출력하는 방법
    
- **점 연산자**

    > 오브젝트 안에 들어있는 값을 불러오기 위해 사용되는 연산자

    ```
    var myVar = {hello:'world'}
    myVar.hello //world

    # 값으로 또 오브젝트를 가지는 경우
    var a = {name:{firstName:"Karoid", lastName:"Jeong"}}
    a.name.firstName //Karoid

    # 값으로 리스트를 가지는 경우
    db.Num.insertMany([{numbers:[101,32,21,11]}, {numbers:[64,94,15]}, {numbers:[52,68,75]} ])
    db.Num.find({"numbers.0:52"}) //numbers의 0번째 요소가 52인 도큐먼트 반환
    //{ "_id" : ObjectId("600ec494142e24fa8e1f8a10"), "numbers" : [ 52, 68, 75 ] }
    ```

- **프로젝션**

    > 도큐먼트의 어떤 필드를 노출할 지 결정하는 파라미터
    >
    > _id 필드는 false로 값을 설정하지 않을 경우 항상 출력
    >
    > 하나의 필드만 True로 설정할 경우, 하나의 필드와 _id 필드 출력
    
    - `db.[컬렉션명].find([쿼리], {[필드]:true 또는 false, ...})` : 프로젝션 파라미터 위치에 불러올 필드의 값을 true 또는 1, 불러오지 않을 필드의 값을 false 또는 0으로 입력
    - `db.containerBox.find(null, {name: 1, category: false})` : name 필드만 출력
    
- **커서(Cursor)**

    > 쿼리 결과에 대한 포인터
    >
    > find 메서드는 **결과를 직접 반환하지 않고, '커서'를 반환함**
    >
    > 10분을 넘기면 비활성 상태로 전환
    >
    > 도큐먼트 전체를 반환하는 메서드를 사용하는 것 대신,gi 커서를 사용하여 필요한 도큐먼트를 순차적으로 불러와서 사용하면 **메모리 절약에 도움이 됨**

    - `db.[컬렉션명].find().forEach([함수]([도큐먼트])){  })` : 각각의 도큐먼트를 순차적으로 불러와서 작업

#### 데이터 수정 및 삭제

- **ReplaceOne**

    > 도큐먼트를 찾아 교체하는 명령
    >
    > 교체 시 기존 내용이 전부 사라짐
    >
    > **_id 필드는 바뀌지 않음**

    - `db.[컬렉션명].replaceOne({[조건]}, {[새로운필드]:[새로운값]})` : 조건에 해당하는 하나의 도큐먼트 교체

- **Update**

    > 하나의 도큐먼트에서 특정 필드만을 수정하는 경우 사용되는 메서드
    >
    > **수정 시 '수정 연산자'를 사용해야 함**
    >
    > 하나의 도큐먼트가 여러 필드로 구성되어 있더라도, 해당 필드만 수정하기 때문에 효율적
    >
    > **빅데이터의 빠른 수정이 요구되는 경우, 가장 적합한 방법**
    >
    > `db.[컬렉션명].update({[조건]}, {[수정연산자]: {변경할 쿼리}})`

    **수정연산자**

    | 파라미터     | 타입                                                         |
    | ------------ | ------------------------------------------------------------ |
    | $set         | 해당 값으로 수정                                             |
    | $currentDate | 현재 timestamp나 date 값으로 수정                            |
    | $inc         | 해당 값 만큼 증가시킴                                        |
    | $min         | 해당 값 이하면, 최솟값으로 수정                              |
    | $max         | 해당 값 이상이면, 최댓값으로 수정                            |
    | $mul         | 해당 값을 곱한 값으로 수정                                   |
    | $rename      | 해당 값으로 필드명 변경                                      |
    | $setOnInsert | Upsert 값이 true이면서 문서를 생성할 때만 사용되는 연산자, 쿼리와 $set 연산자에 나온 필드와 값 이외의 필드와 값을 함께 설정 |
    | $unset       | 해당 필드 제거                                               |

    - `db.article.update({author:"Karoid"}, {$set:{author:"Steve"}})` : author가 'Karoid'인 도큐먼트의 author 필드의 값을 'Steve'로 변경
    - `db.article.update({author:"noname"}, {$unset:{author:""}})` : author가 'noname'인 도큐먼트의 author 필드 삭제

    ##### 도큐먼트 수정 배열 연산자

    | 연산자명  | 설명                                                         |
    | --------- | ------------------------------------------------------------ |
    | $         | 찾은 값의 위치를 기억하는 연산자                             |
    | $[]       | 찾은 모든 값을 기억하는 연산자                               |
    | $addToSet | 배열 안에 해당 값이 없으면 추가하고, 있으면 추가하지 않음    |
    | $pop      | 배열의 첫 번째 혹은 마지막 요소 삭제                         |
    | $pull     | 쿼리에 해당하는 요소 하나를 제거                             |
    | $push     | 해당 요소를 배열에 추가                                      |
    | $pullAll  | 해당 값을 가지는 요소를 전부 제거                            |
    | 기타      | https://docs.mongodb.com/manual/reference/operator/update-array/ |

- **Remove**

    > 하나의 도큐먼트에서 해당 조건을 만족하는 데이터를 삭제

	- `db.[컬렉션명].remove({[조건]})` : 조건을 만족하는 데이터 검색 후 삭제
- ` db.[컬렉션명].remove({})` : 전체 도큐먼트 삭제
	- `db.[컬렉션명].delteOne({[조건]})` : 조건에 맞는 첫번째 도큐먼트 삭제
	- `db.[컬렉션명].delteMany({[조건]})` : 조건에 맞는 전체 도큐먼트 삭제

### 2.3.4 JSON & BSON(Binary Serial Object Notation)

>  MongoDB에서 모든 데이터는 반드시 JSON 타입으로 표현되지만,
> DB 내에 저장될 때는 BSON 타입의 Binary 데이터로 변환되어 저장됨

### 2.3.5 MongoDB 연산자의 종류

https://docs.mongodb.com/manual/reference/operator/

### 2.3.6 MongoDB에서의 빅데이터 추출과 분석

1. MongoDB의 Aggregation Framework 함수를 사용한 빅데이터의 추출
    - 과다한 프로그래밍을 통한 비용과 시간 낭비 문제를 최소화
    - 최소한의 코딩과 빠른 읽기 작업 가능
2. MongoDB의 MapReduce 기능을 이용한 빅데이터의 추출
    - Map, Reduce 함수를 이용하여 Javascript 형태로 제공되는 문법을 통해 컬렉션 내의 데이터를 빠르게 읽고/가공하고/처리할 수 있는 기능
3. MongoDB와 Hadoop의 MapReduce를 연동한 빅데이터의 추출
    - MongoDB로부터 데이터를 읽고, Hadoop의 MapReduce를 이용하여 데이터 분석/가공/처리 하는 방법

## 2.4 MongoDB 데이터 관리

#### Lock 정책

> MongoDB에서는 읽기 일관성과 데이터 공유를 위해 V1.8까지 Global Lock을 제공하였고,
>
> V3.0부터는 Document Lock를 제공함
