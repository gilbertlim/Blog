---
title: "[Machine Learning] 로지스틱 회귀(Logistic Regression)"
category: Machine Learning
use_math: true
---

# 로지스틱 회귀(Logistic Regression)

### Logistic
> **= Sigmoid**, Sigmoid 함수를 사용한다는 의미 <br>
> **= Probability**, 0 ~ 1의 값을 가지는 의미 

### Regression
> 선형 회귀에서는 $- \infty \ \sim \ + \infty$ 범위의 데이터로부터 $\hat{y} = wx + b$ 을 구하여 연속형 값 $\hat{y}$을 예측  

- 로지스틱 회귀는 **이진 분류**를 이용하여 예측 값이 0 또는 1에 가깝도록 학습하여 0 또는 1에 속할 확률을 구함
- 데이터의 범위를 $0\ \sim\ 1$로 만들기 위해 활성화함수를 Sigmoid로 사용함<br>$\hat{y} = sigmoid(wx+b)$
- 범주형 데이터는 **인코딩** 필수

### Logistic Regression
> Classification(범주 예측) 모델<br>
> 수치 예측이 아닌 어떤 범주에 속하는지에 대한 예측(확률)을 모델링

- 수치 예측 모델에 Sigmoid() 필터(활성화 함수, Activation Function)를 적용하여 구현
    - 일반적으로 분류기준은 0.5이며 변경 가능함
    - **0.5보다 크면 1, 0.5보다 작으면 0으로 분류**
- 분류 결과에 대한 **신뢰도 검증** 필요(Model Validation)
    - Confusion Matrix(혼돈 행렬)
    - Accuracy(정확도), Precision(정밀도), Recall(재현율)

### 시그모이드(Sigmoid)
> 로지스틱 회귀에서 필터로 사용하는 함수

$sigmoid(x)= \cfrac{1}{1+e^{-x}}$

![](/assets/images/posts/ml/sigmoid.png)

<br>

#### 수치 예측 모델에 Sigmoid() 필터 적용

$\hat{y} = sigmoid(wx+b)= \cfrac{1}{1+e^{-wx+b}}$
- $w$는 기울기, $b$에 따라 좌우로 이동함

$0 \le \hat{y} \le 1$

<br>

#### 시그모이드 함수에서의 학습

![](/assets/images/posts/ml/sigmoid_learn.png)

<br>

## 실습

- <a href="https://colab.research.google.com/drive/1VxUIMVZ7JU9P-3V50-czlB5nkd822COb?usp=sharing">Logistic Regression</a>