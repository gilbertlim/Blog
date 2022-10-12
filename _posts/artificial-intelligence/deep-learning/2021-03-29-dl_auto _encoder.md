---
title: "[Deep Learning] 오토인코더(AutoEncoder)"
category: Deep Learning
use_math: true
---

<br>

## 1. 생성 모델링(Generative Modeling)
> 확률 모델 관점으로 보면 데이터셋을 생성하는 방법을 기술한 것<br>
> 모델에서 샘플링하면 새로운 데이터를 생성할 수 있다.

<br>
 
![생성 모델링(discriminative modeling)](/assets/images/posts/dl/generative_model.png)

<br>

생성 모델링의 목표는 **새로운 특성을 생성할 수 있는 모델**을 만드는 것이다.

이 모델은 원본 데이터와 **동일한 규칙**으로 **생성된 것 처럼 보이는** 특성을 만든다.

즉 **원본 데이터에는 없지만, 원본 데이터에 있을 것 같은 새롭고 완전히 다른 샘플을 생성**하는 것이다. 

또한, 모델은 매번 다른 샘플을 생성할 수 있도록 **확률적** 요소가 포함되어야 한다. 

<br>

### 생성 모델링(Generative Modeling)과 판별 모델링(Discriminative Modeling)

<br>

![판별 모델링(Discriminative Modeling)](/assets/images/posts/dl/discriminative_model.png)

<br>

판별 모델링은 X와 Y의 관계를 학습해서 Y를 판별하는 것이다.

또한, 판별 모델링을 수행할 때는 훈련 데이터에 Label이 달려 있었다(지도 학습).

**생성 모델링은 보통 Label이 없는 데이터셋에서 수행된다(비지도 학습).**

개별 클래스의 샘플을 생성하는 방법을 학습하기 위해 지도학습에도 적용될 수 있다.

<br>

## 2. 오토인코더(AutoEncoder, AE)
> 인코더와 디코더로 이루어진 신경망<br>
> 딥러닝 **생성** 모델

<br>

![오토인코더(AutoEncoder)](/assets/images/posts/dl/auto_encoder.png)

<br>

**원본 입력 데이터**는 인코더와 디코더를 지나 **입력 데이터가 재구성**된다.

오토인코더는 **입력과 재구성 사이의 손실을 최소화**하는 **인코더와 디코더의 가중치를 찾기 위해 훈련**된다.

**입력 데이터와 출력데이터가 같다**는 특징이 있다.

<br>

오토인코더의 목적은 인코더를 학습시켜 **잠재 공간(Latent Space)**을 생성하는 것이다(위의서 그림에 빨간 네모 부분).

인코더를 통해 고차원인 원본 데이터를 저차원 잠재 공간으로 압축하는데

잠재 공간은 벡터이고, 이를 **표현 벡터(Representation Vector)**로 부른다.

<br>

오토인코더는 주성분 분석(PCA)과 같이 **차원 축소** 역할을 할 수 있다.

고차원인 원본 데이터를 저차원 잠재 공간으로 만들면 잠재 공간이 원본 데이터의 특징을 모두 가지고 있기 때문이다.

<br>


### 인코더(Encoder)
> 고차원 **입력 데이터**를 저차원 **표현 벡터**로 압축

### 디코더(Decoder)
> 주어진 **표현 벡터**를 **원본 차원**으로 다시 압축 해제

<br>

## References
- https://www.oreilly.com/library/view/generative-deep-learning/9781492041931/ch01.html
- https://towardsdatascience.com/applied-deep-learning-part-3-autoencoders-1c083af4d798