---
title: "[Machine Learning] 모델 평가(Model Validation)"
category: Machine Learning
use_math: true
---

## Model Validation
- 만든 모델이 실제로 사용하기에 적합한지 확인(검증)하는 작업
- 만든 모델이 데이터의 분포를 설명하기에 충분한지 확인하는 것
- Testing Data를 모델에 입력하여 MSE를 평가하는 것  

## Model Capacity
데이터에 대한 모델의 설명 능력

- 파라미터 개수에 따라 여러 개 모델을 만들 수 있고, 모델별 성능을 비교할 수 있음
- 일반적으로 파라미터 개수가 많을 수록 설명력이 좋아지나 무조건적인 것은 아님
- 파라미터 개수 별 MSE를 계산하여 **오차가 가장 적은 모델**을 선택함
    - 파라미터가 1개(1차 함수)인 모델 : First order
    - 파라미터가 2개(2차 함수) 이상인 모델 : High order
        - 고차 모델은 사이킷런을 활용하여 입력값을 transformation하여 사용함

## Training Error
Training Data에 모델을 적용하여 확인한 실제 값과 예측 값의 차이(오차)

- $mean((y-\hat{y})^2$)
- 학습은 Training Error를 최소화하는 방향으로 진행됨

## 과적합(Overfitting)
학습한 결과가 Training Data에만 최적화된 모델을 생성하여 새로운 Data에서 성능이 급격하게 낮아지는 현상

- 모델을 생성하고 평가하는데 같은 데이터를 사용하므로 '과적합'이라는 부작용이 발생함
- 같은 데이터로 평가된 결과는 과거의 특징을 알아보는 용도로만 사용 가능하고, 미래에 대한 예측은 논리적으로 설명이 불가함

## Training Data & Testing Data
객관성을 얻기 위해 데이터를 Training Data로 모델을 만들고, Testing Data에 평가를 진행함
- Training Data : 학습을 위해 제공되는 데이터
- Testing Data : 학습 결과를 평가하기 위한 데이터
- 데이터의 크기에 따라 비율(8:2, 7:3, ...)을 결정함(**Hyper parameter**)

### Data Split
- 판다스에서는 데이터를 DataFrame, Series 형태로 나눌 수 있음
- 일반적으로 Random split을 진행하지만, 항상 그런 것은 아님
- 데이터를 무작위로 나누지 않으면, Training Data 모델로 Testing Data를 평가할 수 없는 경우가 발생할 수 있음

## Model Validation Process
1. Data Split : Training Data + Testing Data
2. Training Data -> ML Algorithm -> Models
3. Testing Data -> Models -> Model Select

## Modeling
일반화된 Model을 만드는(학습시키는) 것

<br>
일반화
- Model 생성(학습) 시 사용되지 않은 데이터에서도 유사한 성능 보유
- Train Error, Test Error의 차이가 크지 않음(Non overfitting)

## Validation Approach
과적합을 방지하기 위해 Training Data를 Training Data와 Validation Data로 분리하여 모델을 평가하는 방법
- 데이터를 Training, Testing으로 나누어 학습한 방법도 Testing Data가 모델 평가 과정에서 사용되어지는 문제점 발생하기 때문
- sklearn을 통해 데이터를 분리할 경우 한번에 나눌 수 없기 때문에 두 차례로 나눠서 진행해야 함 

### Validation Data & Testing Data
Validation Data 
- 최적의 모델을 선택하기 위한 데이터
- Training 과정에서 사용됨

Testing Data
- 모델의 최종 성능을 평가하기 위한 데이터
- 실무 데이터에 대한 Generalization Error 추정을 위한 데이터
- Testing Data에 최적 모델을 적용해 얻은 실 값과 예측 값의 차이(오차)
- Training 과정에서 사용되지 않음

<br>
# 실습
<a href="https://github.com/gilbertlim/TIL/tree/master/AI/Machine_Learning/Model_validation">Github Repository 참조</a>