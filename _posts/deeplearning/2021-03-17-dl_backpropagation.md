---
title: "[Deep Learning] 역전파(Backpropagation)"
category: Deep Learning
use_math: true
---

<br>
# 역전파(Backpropagation)

## 1. 역전파(Backpropagation) 개념
> 미분의 연쇄법칙 + 오차 역전파 알고리즘

- 수치미분 과정 없이 학습을 위한 경사값을 계산
- Hidden Layer나 Node가 증가하여 파라미터 개수가 늘어나도 빠른 속도로 학습(= 파라미터 업데이트) 가능

함수형 Model의 학습 원리인 '경사하강법'은 학습 단계의 파라미터 업데이트를 위해 **파라미터별 편미분 값을 계산한다**.

Hidden Layer와 Node 개수가 증가하여 **파라미터 개수가 많아진다면, 수치 미분으로 계산하는데 많은 컴퓨팅 자원과 시간이 소요된다**.

**역전파는 수치미분 과정없이 연쇄법칙을 통해 미분값을 획득**하는 알고리즘으로서 **경사값을 계산하는데 걸리는 시간을 줄여준다.**

<br>

## 2. 연쇄 법칙(Chain Rule)
> 합성 함수의 미분은 합성 함수를 구성하는 **개별 함수 미분의 곱**

<mark style="background-color:yellow">신경망은 간단한 함수들의 중첩(합성함수)로 구성되어 있다고 할 수 있다.</mark>

input(X) -> Node 1 -> Node 2 -> Node 3-> Output($\hat{y}$) 이라면,

$\hat{y} = sigmoid(W_3 \ast sigmoid(W_2 \ast sigmoid(W_1 X + b_1) + b_2) + b_3)$

합성함수의 미분은 구성된 개별 함수 미분의 곱이다(연쇄 법칙).

### 미분의 연쇄 법칙 

<mark style="background-color:yellow">합성 함수의 미분은 $ {f(g(x))}^\prime \ = \ f^\prime (g(x)) \times g^\prime (x)$ 로 개별 함수 미분의 곱이다.</mark>

<br>

## 3. 역전파(Backpropagation) 방법

1) 순전파(Forward propagation)를 수행하여 $\hat{y}$를 계산하고, 오차값(MSE or CEE)을 계산한다.

<mark style="background-color:yellow"> 2) 오차값이 감소하는 방향으로 가중치(weight) 수정한다.</mark>
- <mark style="background-color:yellow">효율적인 경사값(Gradient) 계산(순전파에서 계산된 값을 재사용)을 위해 역전파를 수행</mark>
- <mark style="background-color:yellow">파라미터 업데이트를 위해 출력층(해당 층)의 오차값을 은닉층(앞 층)으로 전달</mark>

### 예

![](/assets/images/posts/dl/backpropagation.png)

<br>

## 4. 경사 소실(Vanishing Gradient)
> 파라미터 학습을 위한 미분 값이 0에 가까워지는 것

- 역전파는 출력층으로부터 하나씩 앞으로 돌아오면서 각 층의 가중치를 학습함
- 가중치 학습을 위해서는 미분값이 필요함
- **Sigmoid 함수를 미분하면 최대치가 0.25로, 1보다 작은 값을 생성함**
    - **은닉층이 증가하면 미분값이 0이되는 문제 발생**
- 해결 방법 : 활성화 함수를 다른 함수로 대체하여 학습

### 활성화 함수 종류

![](/assets/images/posts/dl/activation_functions.png)
https://ayyucekizrak.medium.com/derin-öğrenme-için-aktivasyon-fonksiyonlarının-karşılaştırılması-cee17fd1d9cd


<br>

## 실습
- <a href="https://colab.research.google.com/drive/1f4jJe4UIHcfTiqyBCAFRbXQNP5-dsdnE?usp=sharing">역전파</a>