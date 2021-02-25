---
title: "[Data Analytics] 데이터 분석(Data Analytics) 개요"
category: Data Analytics
use_math: true
---

# Data Analytics
데이터의 특징을 확인하는 일

## 1. Data
- 과거 행동의 결과들
- 객관적 사실

### 특징
1. 생성되는 것(만들어지는 것)
2. Activity(활동 또는 행동)의 결과
3. 여러 개(2개 이상)
4. 과거

### 기타
- 예
    - "일 매출" : 판매 활동에 의해 만들어지는 것들
    - "출결 기록" : QR 코드를 찍어 출석하면 만들어지는 것들
- Datum : 단수형, Data : 복수형
- 한국에서는 "자료"로 번역하지만, 자료는 만드는 것
- 위조 or 변조 : 행동이 없는데 데이터가 있는 것, 사람이 기록하면 위변조될 가능성 증가

## 2. Analytics
~의 특징을 확인하는 일

- 사람이 만들어낸 무언가가 특징을 찾으면 AI
- 사람이 Python을 활용해서 특징을 찾으면 분석, 통계
- 사람이 특징을 찾을 줄 알아야 AI에게 시킬 수 있음(분석, 통계를 배우는 이유)
- Data Mining 이든 Data Analytics 든 Statistics라고 생각할 것
    - 출발점은 동일하나 약간의 차이가 있음
- 데이터가 있다고 분석하는 것은 아니고, 필요에 의해서 데이터를 분석하는 것

### Business Intelligence(BI)
    5.Decision Making(Planning)
    - 미래 행동의 결과
    - 과거 행동의 결과를 가지고 미래 행동의 결과를 바꾸고자 하는 일
    
        ↑
    
    4.Data Mining(DM)
    - 과거 행동의 특징을 캐내는 일
    - 과거와 미래를 연결하는 개연성이 있어야 함
    - Statistics ⇒ Analytics가 DM에 쓰였는데, 무언가의 특징을 확인하는 일을 해왔음
    
        ↑
    
    3.Data Warehouse(DW)
    
        ↑
    
    2.Data Base(DB)
    
        ↑ (수집/저장)
    
    1.Data
    - Business Activities로부터 발생된 과거 행동의 결과



### Business
이익을 창출하기 위한 수단(예 : 삼성전자, 제조)

## 통계적 측면에서의 Data Analytics
Data Analytics : **데이터**의 **특징**을 **확인**하는 일
- **데이터** = 집단
    - 타입 : 타입에 따라 연산이 달라짐 → 확인할 수 있는 특징이 달라짐
    - <a href ="https://m.blog.naver.com/dairum_enc/221409597367">데이터 타입</a>
- **특징**
    - 평균(중심화 경향치, 모여있는 특징, 어디쯤에 모여 있을까?)
    - 분산(산포도, 떨어져있는 특징, 평균에서 평균적으로 떨어져 있는 거리의 제곱)
    - ...
- **확인** = 수학적 연산