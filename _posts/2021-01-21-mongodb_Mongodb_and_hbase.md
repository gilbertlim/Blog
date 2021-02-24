---
title: "[MongoDB] MongoDB 개요"
category: MongoDB
---

# 1. MongoDB 개요

## 1.1 NoSQL

### 1.1.1 Bigdata

> - Volume : 다루어야하는 데이터의 규모가 크고, 기존에 관리하지 않았던 데이터를 분석하는 경우 증가
>
> - Velocity : 데이터가 매우 빠른 속도로 생성되고 사라짐, 빠르게 처리하고 분석해야하는 경우가 증가
>
> - Variety : 미리 정의된 데이터 모델이 없거나 데이터가 미리 정의된 방식으로 정리되지 않음(비정형), 수치화가 힘든 데이터가 많음

#### 빅데이터 솔루션

- 수집 및 저장 기술 : NoSQL(**MongoDB**, Casandra, Hbase, ...)
- 추출 및 분산 기술 : Hadoop, Storm, Spark, ...
- 분석 및 통계 기술 : R, SAS, SPSS, ...

### 1.1.2 NoSQL

> 특정 데이터 모델에 대해서 특정 목적에 맞추어 구축되는 DB
>
> 특별한 이슈에 대응하기 좋은 DB
>
> 클라우드 컴퓨팅 환경에서 발생하는 빅데이터를 효과적으로 수집, 저장하는 새로운 기술

#### NoSQL의 장점

- 클라우드 컴퓨팅 환경에 적합
    - 오픈 소스
    - 하드웨어 확장에 대한 유연한 대처 가능
    - 저렴한 비용으로 분산처리 및 병렬처리 가능

- 유연한 데이터 모델
    - 비정형 데이터 구조 설계로 비용 감소
    - RDBM의 Relation, Join을 Linking, Embedded로 구현하여 성능이 빠름

- 빅데이터 처리에 효과적
    - Memory Mapping 기능을 통해 Read/Write가 빠름
    - 전형적인 OS, Hardware에 구축 가능
    - 기존 RDB와 동일하게 데이터 처리 가능

#### NoSQL 제품군

1. Key-Value DB

    > 딕셔너리, 해시로 잘 알려져 있는 자료 구조인 연관 배열의 저장, 검색, 관리를 위해 설계된 데이터베이스

    - Redis, Amazon DynamoDB, Microsoft Azure Cosmos DB, ...

2. Wide Column DB

    > 같은 테이블 내 행별로 컬럼과 자료형이 다양한 데이터베이스(2차원 Key-Value DB)

    - Cassandra, Hbase, Microsoft Azure Cosmos DB, ...

3. Document DB

    > JSON 유사 형식의 문서로 데이터를 저장 및 쿼리하도록 설계된 데이터베이스

    - **MongoDB**, Azure Cosmos DB, Microsoft Azure Cosmos DB, ...

4. Graph DB

    > 그래프 구조를 저장하고 표현하기 위해 만들어진 데이터베이스
    
    - Neo4j, Microsoft Azure Cosmos DB, OrientDB...

#### CAP 이론

> 어떤 분산 시스템도 CAP 3가지를 모두 만족시킬 수 없다는 이론
>

1. Consistency(C, 일관성) : 같은 내용의 데이터를 보여줘야 함

2. Availabilty(A, 가용성) : 모든 사용자들이 읽기/쓰기 가능해야 함

3. Partition Tolerent(P, 지속성) : 통신에 문제가 생기더라도 동작해야함

- RDBMS : CA

- MongoDB : CP

## 1.2 NoSQL 적용 사례

- Disney, GYT, Forbes, Shutterfly, foursquare, National Archives, ...

## 1.3 MongoDB와 Hbase

### 1.3.1 MongoDB

>  데이터의 구축과 조작을 비관계형(NoSQL) 방식으로 관리하는DBMS
>
>  Document-Oriented DBMS

#### MongoDB 특징

- 인기

    > Nosql DBMS 중 인기 1위, 가장 원하는 데이터베이스 기술로 3년 연속 1위

- 빠른 속도와 확장성

    > 현대 애플리케이션이 요구하는 빠른 속도와 확장성을 적절하게 충족함

- 친숙함과 이용의 편리성

    > 개발자들이 자주 보는 구조와 문법을 사용(JavaScript 활용, JSON 형식과 유사한  정보 저장 방식)
    >
    > 데이터가 어떤 값을 가질지, 크기가 얼마나 될지 정하지 않고(Schemaless) 저장함(도큐먼트 지향 스토어 방식)

- 쉽고 빠른 분산 컴퓨팅 환경 구성

    > DB의 안정성과 속도를 높일 수 있는 복제(Replicate)와 샤딩(SHarding) 기능을 제공함

- MapReduce(분산/병렬처리) 기능 제공

    > 구글에서 분산 컴퓨팅을 지원하기 위한 목적으로 제작한 소프트웨어 프레임워크
    >
    > > 대용량 데이터 처리(Batch Processing) 및 집합(Aggregation)을 위해 만들어짐
    > >
    > > 비 공유 구조로 연결된 여러 개의 노드에서 병렬 처리 방식으로 대용량 데이터를 처리함
    > >
    > > Map과 Reduce 함수 만으로 병렬 프로그래밍이 가능함
    > >
    > > MAP : 입력 데이터를 여러개의 작은 Split-Peace로 분산 입력하는 함수
    > >
    > > REDUCE : 중복 데이터 제거 후, 사용자가 원하는 형태로 데이터를 집계하는 함수

- CRUD(Create, Read, Update, Delete) 위주의 다중 트랜잭션 처리 가능

- Memmory Mapping 기술을 기반으로 빅데이터 처리에 탁월한 성능 발휘

#### "MongoDB를 언제 써야 할까?"

- **스키마가 자주 바뀌는 환경**

    > **아직 추가될 기능이 확실하지 않고, 상황에 따라 애플리케이션에 저장될 자료가 지속적으로 달라지는 환경**
    >
    > 스키마가 없는 구조로 되어 있어, 스키마 변경이 비교적 자유로움(**로그 기록 시 용이**)
    >
    > 스키마 : 어떤 종류의 정보를 어떤 구조에 어떤 관계를 가지고 있는지 정의한 구조

- **데이터 안정성이 떨어지거나 읽기/쓰기 속도가 줄어드는 환경(분산 컴퓨팅이 필요한 환경**

    > MongoDB는 샤딩과 복제를 지원하므로, 분산 컴퓨팅 환경을 쉽게 만들 수 있음

    - **Distributed Computing(분산 컴퓨팅, 병렬 컴퓨팅)**

        > 정보를 네트워크에 연결된 여러대의 컴퓨터(서버)로 나눠서 저장하고 처리하는 분산처리 모델

    - **Replication(복제)**

      > **DB의 지속성과 안전성을 위해 여러개의 DB에 동일한 데이터를 동기화 하는 과정**
      >
      > **다양한 상황에서도 원하는 데이터를 얻을 수 있는 방법**
      >
      > > 복제된 DB가 있다면, 이것들을 연결시켜서 데이터의 흐름을 복구할 수 있음
      - Master-Slave Replication

      	> 		Read/Write를 담당하는 Master node,
      	> 		마스터 노드의 운영 로그(oplog)를 통해 데이터 백업을 수행하는 Slave node로 이루어짐
      - Replica Sets    

          > 같은 데이터를 저장하고 백업하기 위해 여러 대의 DB 서버가 운영되고, 
          > 각각 서버에 유지하는 복수개의 mongd 인스턴스가 모인 추상적인 그룹
          > (primary node-secondary node들의 묶음)

    - **Sharding(Partition)**

        > **데이터를 읽고 쓰는 속도를 향상시키기 위해 정보를 분산해서 저장하는 방식**
        >
        > **저장용량이 늘어나면서 읽기/쓰기 속도가 줄어들 때 속도를 높이는 방법**
        >
        > >  같은 테이블 스키마를 가진 데이터를 다수의 DB에 분산하여 저장하는 방법(도큐먼트를 나눠서 저장)
        > >
        > > 파티셔닝을 통한 데이터 분산 처리와 성능 향상을 위한 Load Balancing
        > >
        > > 빅데이터의 효율적 관리와 백업 및 복구 전략 수립을 위한 솔루션

#### MongoDB 구조

- MongoDB 구성 요소

    > Database > Collection > Document
    >
    > 도큐먼트는 JSON과 유사한 BSON 구조로 되어 있음(`{필드1:값1, 필드2:값2}`)
    >
    > BSON 구조는 거의 모든 정보를 표현할 수 있음(배열, 숫자, 3차원 좌표, ...)

**RDBMS와의 용어 비교**

| MongoDB            | RDBMS         |
| ------------------ | ------------- |
| Collection         | Tables        |
| BSON Field         | Field(Column) |
| BSON Document      | Record(Row)   |
| _ID Field          | Primary Key   |
| Embedded & Linking | RelationShip  |

### 1.3.2 HBASE

>  Hadoop의 HDFS위에 만들어진 분산 컬럼 기반의 데이터베이스

#### 특징

- 대용량의 데이터를 안정적으로 다루는데 효과적
- 대량의 데이터 분석 처리 지원에 적합
- Zookeeper를 이용한 가용성, 확장성 보장
- 1000개 이상의 멀티 클러스터 노드 확장 가능

#### Hadoop

> 빅데이터의 저장과 분석을 위한 **분산 컴퓨팅** 솔루션
>
> 빅데이터를 **여러대의 컴퓨터로 나눠서 저장 하고 처리할 수 있는** 자바 기반의 오픈소스 프레임워크

### 1.3.3 MongoDB & Hbase 비교

https://www.mongodb.com/compare/mongodb-hbase
