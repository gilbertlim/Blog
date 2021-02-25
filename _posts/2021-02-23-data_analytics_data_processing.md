---
title: "[Data Analytics] 데이터 처리(Data Processing)"
category: Data Analytics
use_math: true
---

## Knowledge Based Management
- Data : 과거 행동의 결과들
- Information : 원본은 건들지 않으면서, Data를 가공하여 만든 것
- Knowledge : Information을 주기적으로 계산하여 특징을 파악하고 예측한 것
- Wisdom : Decision Making까지 반영된 부분

### Business Intelligence 관점
- Business Activities → Data
- Data Base, Data warehouse → Information
- Data Mining → Knowledge
- Decision Making(Planning) → Wisdom

<br>

## 시각화
- 일반적으로 2차원 공간을 시각화
- Pandas의 Series를 시각화 시킴
- 데이터 형식에 따라 시각화 방법이 다름

### 이산형 & 연속형 데이터
- 이산형 : 수치적인 의미를 가지고 있으나, 소수점으로 표현되지 않는 경우(예 : 1.5명)
- 연속형 : 수치적인 의미 / 소수점 표현 가능 / 측정 가능 데이터

<br>

## Data Preprocessing(데이터 전처리)
- 분석에 데이터를 사용할 수 있도록 하기 위한 작업(결측치 처리, ...)
- 데이터 전처리를 위한 패키지 활용이 신입사원의 주 업무

### 결측치(missing Value)
데이터의 일부가 비어있는 것

#### 특징
- 파이썬 표기법
    - `NA(Not Available)`, `NaN`, `NULL`
    - `''`, `' '` : 결측치로 처리되지 않을 수 있어 직접 눈으로 확인 필요
- 처리 방법
    1. 삭제 : 행 또는 열
      - 삭제하는 것은 데이터의 총량이 줄어들기 때문에 권장되지는 않음

    2. 대체 : 숫자냐 문자냐에 따라 다름
      - 수치 : 평균, 앞/뒤 값, ...
      - 문자 : 최빈값, 앞/뒤 값, ...
        - 바로 앞/뒤 값이 이미 결측치가 있으면 없을 때까지 이동하여 값을 찾음
  
<br>

## 데이터 처리를 위한 Pandas 연습
<a href="https://github.com/gilbertlim/TIL/tree/master/Data_Analytics">Github Repository 참조</a>