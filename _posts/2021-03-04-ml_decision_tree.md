---
title: "[Machine Learning] 의사결정 나무(Decision Tree)"
category: Machine Learning
use_math: true
---

# Decision Tree(의사결정 나무)

## 'Machine'의 개념 업데이트
> ML을 설명할 때 Machine은 <a href="https://gilbertlim.github.io/machine%20learning/ml_intro/">Function</a>이라고 했었다.<br>
>
> 사실, ML에서 Machine은 Function(함수)와 Tree(트리)이다.

1) Function
- y를 예측하기 위해 회귀식 $\hat{y}=wx+b$ 을 바탕으로 Gradient Descent 하여 모델을 학습

2) Tree 
- 데이터를 분류하기 위해 가지(Branch)마다 Rule을 기준으로 Entropy가 낮아지는 방향으로 학습
- 일반적으로 Function 보다 성능이 좋아 자주 사용됨

## Decision Tree(의사결정 나무)
> Rule 기반 의사결정 모델
 
- Binary Question(이진 질의)의 분류 규칙을 바탕으로 Root Node의 질의 결과에 따라 Branch(가지)를 타고 이동하며, <br>
- 최종적으로 분류 또는 예측값을 나타내는 Leaf까지 도달함

### 특징
- 지도 학습 알고리즘의 일종
- Rule은 Entropy가 가장 작은 방향으로 학습
- 종류
    - 범주형 : Classification Tree, Entropy가 낮은 방향으로 Rule 결정
    - 수치형 : Regression Tree, Variance가 낮은 방향으로 Rule 결정
- 용어
  - Root Node : 최상위 노드
    - Splitting : 하위 노드로 분리되는 행위
    - Branch : 노드들의 연결(Sub-Tree)
  - Decision Node : 2개의 하위노드로 분리되는 노드
    - Parent Node : 분리가 발생하는 노드
  - Leaf(Terminal Node) : 더이상 분리되지 않는 최하위 노드
    - Child Node : 분리가 발생한 후 생성되는 노드

### Model Capacity in Decision Tree
- 의사결정 나무는 규칙 기반으로 직관적으로 이해하기 쉽고, 설명력(Model Capacity)이 좋은 알고리즘
- 각 노드별 불순도(Impurity, = Entropy)에 기반한 최적의 분류 규칙을 적용함
  - Splitting 과정을 반복하면서 의사결정 나무가 성장하며 모델의 설명력이 증가함
  - 나무가 성장하면서 설명력이 증가하면 과적합이 발생할 수 있음
- Leaf는 순도(동질성)이 높은 적은 수의 데이터 포인트를 포함함
  - 순도 높은 결과를 만들기 위해 순도 높은 Leaf가 나올 때까지 Recursive Partitioning을 수행함
  - Leaf들의 불순도는 0

  
### Overfitting(과적합)
- 노드들은 불순도가 낮은 방향으로 분리됨
- 노드들이 분리되면서 **너무 복잡하고 큰 의사결정나무 모델**을 생성하여 과적합 문제가 발생됨

### Pruning(가지치기)
- 과적합 예방 및 모델 성능향상 목적으로 의사결정나무의 파라미터를 조정하는 방법(Hyper Parameter)
- 보통 사후에 진행하고, 모델을 경량화할 수 있는 방법
- max_depth : 의사결정나무의 성장 깊이 지정
- min_samples_leaf : 리프에 들어가는 최소 샘플의 개수 지정

### 불순도(= Entropy) 계산 방식
#### 1) Entropy
- **분리 정보 이득(질문 전 Entropy - 질문 후 Entropy(불순도))**이 큰 특징으로 분리 발생
- 분리 정보 이득을 비교하여 이득이 큰 Feature로 분리 발생

  $-(\sum\limits_{i=1}^n\ p_i\ log_2\ p_i)$

- 분리 정보 이득 계산 예
  - '소득' Feature로 분리해야할지, '학생' Feature로 분리해야할지 분리 정보 이득을 계산하여 이득이 큰 쪽으로 노드 분리
  - '학생' Feature 계산은 생략

  <br>
  
  |학생|소득|연체|
  |---|---|---|
  |네|없음|Yes|
  |아니오|없음|No|
  |네|없음|Yes|
  |아니오|있음|No|
  |네|없음|Yes|
  |아니오|없음|No|
  
  <br>

  1) (소득)질문 후 Entropy 계산
    
    - $p(있음) \times E(Yes, No) + p(없음) \times E(Yes, No)$
    $= \cfrac{1}{6} \times E(0, 1) + \cfrac{5}{6} \times E(3, 2)$
    $= \cfrac{1}{6} \times (-\cfrac{0}{1} \times log_2 \cfrac{0}{1} - \cfrac{1}{1} \times log_2 \cfrac{1}{1}) + \cfrac{5}{6} \times (-\cfrac{3}{5} \times log_2 \cfrac{3}{5} - \cfrac{2}{5} \times log_2 \cfrac{2}{5})$
    $=0.966$
  
  2) (소득)분리 정보 이득 계산 
    
    - $1 - 0.966 = 0.034$
  <br>
      
  3) (학생)분리 정보 이득이 $1$이므로 '학생' Feature로 노드가 분리됨

<br>

#### 2) Gini Impurity Index
- sklearn의 기본 불순도 계산 방식
- **지니 불순도 지수(1 - 특징 지니 지수)**가 작은 특징으로 분리 발생(분리 정보 이득과 반대 개념)
- 특징 지니 지수가 클 수록 불순도가 낮음

  $1 - \sum\limits_{i=1}^n {p_i}^2$

- 지니 불순도 지수 계산 예
  - '소득' Feature로 분리해야할지, '학생' Feature로 분리해야할지 지니 불순도 지수를 계산하여 작은 쪽으로 노드 분리
  - '학생' Feature 계산은 생략

  <br>
  
  |학생|소득|연체|
  |---|---|---|
  |네|없음|Yes|
  |아니오|없음|No|
  |네|없음|Yes|
  |아니오|있음|No|
  |네|없음|Yes|
  |아니오|없음|No|

  <br>

  1) (소득)특징 지니 지수 계산
    
    - 있음 : ${\cfrac{0}{1}}^2 + {\cfrac{1}{1}}^2 = 1$ 
    - 없음 : ${\cfrac{3}{5}}^2 + {\cfrac{2}{5}}^2 = 0.52$
    - 특징 지니 지수 : $\cfrac{1}{6} * 1 + \cfrac{5}{6} * 0.52 = 0.6$
  
  2) (소득)지니 불순도 지수 계산 
    
    - $1 - 0.6 = 0.4$
  
<br>

### Feature Importance
> 모델에 대한 Feature의 기여도(중요도)

- 1 : 기여도 높음 / 0 : 기여도 없음
- Feature Importance가 0이라 할지라도, 직접 모델에 적용시켜봐야 실제 기여도를 알 수 있음

## 실습
- <a href="https://colab.research.google.com/drive/12IycDwshRn0iSfSBolLjKC3D1U2FTDZG?usp=sharing">Decision Tree1</a>
- <a href="https://colab.research.google.com/drive/1KT1oLLZZySYQWu47jThTRqerEM1GIO_X?usp=sharing">Decision Tree2</a>
- <a href="https://colab.research.google.com/drive/1SymAQM6rkcLBpmOM8NMUYj8v1dQq4WCU?usp=sharing">Logistic Regression vs Decision Tree</a>