---
title: "[Deep Learning] 다층 퍼셉트론(Multi-Layer Perceptron)"
category: Deep Learning
use_math: true
---

<br>

# 다층 퍼셉트론(Multi-Layer Perceptron, MLP)

<br>

## 1. 인공 신경망(Artificial Neural Network, ANN)
> 머신러닝 분야에서 연구되는 학습 알고리즘

- 인간의 뇌 구조(뉴런)를 모방하여 동작원리를 수학의 함수로 정의한 알고리즘
- 수치예측, 범주예측, 패턴 인식, 제어 분야에 응용

### 생물학적 신경망 Vs. 인공 신경망

![](/assets/images/posts/dl/neuron.png)
Roffo, Giorgio. (2017). Ranking to Learn and Learning to Rank: On the Role of Ranking in Pattern Recognition Applications. 

<br>

## 2. 퍼셉트론(Perceptron)
> 인공신경망의 한 종류(선형 분리기)로 다수의 입력으로부터 하나의 결과를 내보내는 알고리즘<br>
> 동작 방법이 뉴런과 유사함

$y = f \left( \sum\limits_i w_i x_i + b \right)$

![](/assets/images/posts/dl/perceptron.png)

https://wikidocs.net/24958

- 가장 간단한 형태의 피드 포워드 신경망(Feed-Forward Neural Network, FFNN)
- Frank Rosenblatt에 의해 고안됨
- 동작 원리
    - 노드의 입력(Input, $X$)과 가중치(Weight, $w$)의 곱을 모두 합함
    - 합한 값이 활성화 함수의 임계치보다 크면 1, 작으면 0을 출력
- 단층 퍼셉트론으로 AND, OR, NAND는 구현 가능하지만, **XOR 게이트는 구현이 불가능함**

### Logic Gate
> 불 대수를 물리적 장치에 구현한 것으로, 하나 이상의 논리적 입력값에 대해 논리 연산을 수행하여 하나의 논리적 출력값을 얻는 전자회로

![](/assets/images/posts/dl/logic_gate.png)

https://medium.com/@lucaspereira0612/solving-xor-with-a-single-perceptron-34539f395182

<br>

## 3. 다층 퍼셉트론(Multi-Layer Perceptron, MLP)
> 퍼셉트론으로 해결할 수 없는 비선형 분리 문제를 해결하기 위한 인공 신경망

![](/assets/images/posts/dl/mlp.png)

https://wikidocs.net/24958

- 여러 층(Layer)의 퍼셉트론(Node)을 쌓아서 동작함(Input Layer - Hidden Layer - Output Layer)
- 은닉층(Hidden Layer)이 1개인 인공 신경망
- **XOR 게이트는 기존의 NAND, OR, AND 게이트를 조합하여 만들 수 있음**
- 추후에 나올 심층 신경망(Deep Neural Network, DNN)은 Hidden Layer가 2개 이상이다.

### 1) MLP의 Parameter 학습법
> <a href="https://gilbertlim.github.io/machine%20learning/ml_gradient_descent/">경사하강법(Gradient Descent)</a>

- 경사하강법을 사용하여 $w$와 $b$를 학습함
- 입력($x$), Node 개수, Hidden Layer 개수가 증가할 수록 오차(Loss)가 줄어든다.
- 파라미터 개수($w,\ b$)가 늘어날수록 학습하는데 시간이 많이 소요된다.

#### 2) 가중치 행렬 표기법
$W^{Layer}_{Node\ \ Parameter}$

$W^{1}_{21}$ (= w1_21) : 1번 레이어 입력측 1번 노드에서 출력측 2번 노드의 가중치

#### 3) 경사하강법에서 사용되는 미분의 개념

![](/assets/images/posts/dl/differential_1.png)

![](/assets/images/posts/dl/differential_2.png)

<br>

## 4. 다중 분류 문제(Categorical Classification)

다중 분류 문제에서 시그모이드 함수를 활성화 함수로 사용하게 되면, <br>
출력 값을 공정하게 평가하기 어려운 문제가 발생한다(전체 출력값의 합이 1 이상). 

따라서 다른 활성화 함수가 필요하다.

- 이진 분류에서는 시그모이스 함수를 사용해도 된다.

### 1) 소프트맥스 함수(Softmax Function)
> 다중 분류 수행 시 마지막 레이어(출력층)의 활성화 함수로 사용되는 함수 

소프트맥스는 **전체 출력값의 합이 1이 되도록 출력값을 확률값으로 만든다**.<br>
큰 값은 강조하고, 작은 값은 약화하는 효과를 갖기 때문에 확률적으로 분류를 가능하게 한다. 

$y = \cfrac{exp(a_k)}{\sum\limits_{i=1}^n exp(a_i)}$

![](/assets/images/posts/dl/softmax.png)

### 2) 교차 엔트로피 오차(Cross-Entropy Error, CEE)
> 다중 분류에서 사용되는 오차 함수

3개 이상의 클래스를 분류하는 다중 분류에서 Mean Squared Error(MSE)를 사용하면,<br>
계산량이 많아지기 때문에 **상대적으로 계산량이 적은 CEE를 사용한다**.

$CEE = -\sum \Big( y \times log(\hat{y}) \Big) $

- 이진분류에서는 Binary CEE, 다중분류에서는 Categorical CEE를 사용한다.

#### MSE Vs. CEE

$y_1\ =\ [1,\ 0,\ 0],\ y_2\ =\ [0,\ 1,\ 0],\ y_3\ =\ [0,\ 0,\ 1]$

$\hat{y}\ =\ [0.1,\ 0.7,\ 0.2]$

$y_2$ 에 대해서 오차를 계산하면,

$$MSE= \cfrac{((0-0.1)^2 + (1-0.7)^2 + (0-0.2)^2)}{3}$$

$$ \begin{aligned} CEE &= -0 \times log(0.1) -1 \times log(0.7) -0 \times log(0.2) \\ &= -1 \times log(0.7) \end{aligned} $$

y를 One-Hot Encoding 하면, $y$ 값이 1인 인덱스의 $\hat{y}$ 값에 대해서만 $-log(\hat{y})$로 계산하면 되기 때문에 계산량이 줄어든다. 