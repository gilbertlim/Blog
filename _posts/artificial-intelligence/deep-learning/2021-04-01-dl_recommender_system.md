---
title: "[Deep Learning] 추천 시스템(Recommender System)"
category: Deep Learning
use_math: true
---

<br>

# 1. 추천 시스템(Recommender System) 
> 사용자 본인도 잘 몰랐던 취향이나 관심사를 발견하여 사용자에게 추천하는 시스템 

**Cold Start** 문제 때문에 사용자로부터 정보를 직접 제공받아야 추천 시스템을 만들 수 있다.

### 추천 시스템에 사용되는 데이터
- 사용자가 어떤 상품을 구매했는가? 
- 사용자가 어떤 상품을 둘러보았는가?
- 사용자가 어떤 상품을 장바구니에 넣었는가?
- 사용자가 상품에 어떤 평점을 부여했는가?
- 사용자가 가입 시 선택한 관심분야는 무엇인가?

<br>

# 2. 추천 시스템의 종류
> Collaborative Filtering & Content-based Recommendation(생략)

<br>

## Collaborative Filtering
> 사용자로부터 얻은 정보를 기반으로 예측하는 방법

- 사용자 행동양식(User Behavior) 데이터를 사용함
    - 사용자(User)가 아이템(Item)에 매긴 평점정보, 상품구매이력 등
- 최근접이웃(K-Nearest Neighbor) 방식과 잠재요인(Latent Factor) 방식이 있음
    - 두 방식 모두 'User-Item Matrix' 사용

<br>

### Matrix

#### (1) User-Item Matrix
- 축적된 '사용자 행동양식(User Behavior)' 데이터 기반
- 사용자가 아직 평가하지 않은 아이템을 '예측 평가(Predicted Rating)'

||Item1|Item2|Item3|Item4|
|----|---|---|---|---|
|User1|4||3|<font color = 'red'>PR|
|User2|5|3||4|
|User3||1|2|3|

- User1과 User2의 'Item'에 대한 평점 유사도가 높음
- User1이 아직 구매하지 않은 Item4와 Item5를 추천(Predicted Rating)

#### (2) Item-User Matrix

||User1|User2|User3|User4|User5|
|----|---|---|---|---|---|
|<font color = 'red'>Item1|5|4|5|5|5|
|<font color = 'red'>Item2|5|4|4|<font color = 'blue'>PR|5|
|Item3|4|2|2||4|

- Item1과 Item2의 평점 유사도가 높음
- User4가 아직 구매하지 않은 Item2를 추천(Predicted Rating)

<br>

### 1) KNN Collaborative Filtering식
> User-Item Matrix와 Item-User Matrix를 활용하여 추천하는 방식

##### (1) User-Item Matrix를 활용하는 경우
- 사용자와 유사한 다른 사용자를 Top-N으로 선정
- Top-N 사용자가 좋아하는 아이템을 추천하는 방식
   - 특정 사용자와 다른 사용자 간의 **유사도(Similarity)**를 측정
   - 가장 유사도가 높은 Top-N 사용자를 추출
   - 그들이 선호하는 아이템을 사용자에게 추천

#### (2) Item-User Matrix를 활용하는 경우
- 사용자의 아이템에 대한 평가 결과가 유사한 아이템을 추천
- 행과 열이 사용자 기반 필터링과 반대
  - 아이템에 대한 평점 간 '유사도(Similarity)' 측정
  - 평점 유사도가 높은 아이템를 추출
  - 그들이 선호하는 아이템을 사용자에게 추천
  
<br>

### 2) Latent Factor Filtering
> User-Item Matrix를 사용하여 '잠재요인(Latent Factor)'을 추출하여 추천의 근거로 사용하는 방식

- '잠재요인'이 무엇인지 명확하게 정의할 수 없음
- User-Item Matrix를 '잠재요인' 기반의 저차원 밀집행렬인 '사용자-잠재요인'과 '잠재요인-아이템'으로 행렬 분해(Matrix Factorization)
  - SVD(Singular Vector Decomposition)
  - NMF(Non-Negative Matrix Factorization)
- 이 두 행렬의 내적을 통해서 새로운 '사용자-아이템 평점' 행렬 생성
- 사용자가 평점을 부여하지 않은 아이템에 대한 '예측평점' 생성 가능

<br>

#### 행렬 분해 방법
> 희소행렬(Sparse Matrix)을 저차원의 밀집행렬(Dense Matrix)로 분해하는 방법을 사용 

- 확률적 경사하강(Stochastic Gradient Descent) 기반 행렬 분해
- P와 Q 행렬로 계산된 예측 R 행렬의 값이 실제 R 행렬의 값과 가장 최소의 오류를 가질 수 있도록 반복적으로 'Cost Function' 최적화를 통해 P와 Q를 유추해내는 것

#### 순서
1. 임의의 값을 갖는 P와 Q 행렬 초기화
2. P와 Q 행렬로 예측 R 행렬 계산 후 실제 R 행렬과 오차값 계산
3. 오차를 최소화 하도록 P와 Q 행렬의 값을 업데이트
4. 2단계와 3단계를 반복하며 P와 Q 행렬값을 학습
5. 행렬 분해가 완료된 P와 Q 행렬로 예측평점 행렬 생성

<br>

#### 예시

##### (1) User-Item Matrix -> 희소행렬

||Item1|Item2|Item3|Item4|Item5|
|---|---|---|---|---|---|
|User1|5| |4| |5|
|User2| | | |5|3|
|User3|4| | |2| |
|User4| |2| | |4|

##### (2) User-'Latent Factor' Matrix -> 밀집행렬1

||Factor1|Factor2|
|---|---|---|
|User1|5|4|
|User2|5|3|
|User3|4|3|
|User4|3|2|

##### (3) 'Latent Factor'-Item Matrix -> 밀집행렬2

||Item1|Item2|Item3|Item4|Item5|
|---|---|---|---|---|---|
|Factor1|5|4|4|5|5|
|Factor2|5|3|4|5|3|

##### (4) '예측 평점' User-Item Matrix -> 최종 밀집행렬

||Item1|Item2|Item3|Item4|Item5|
|---|---|---|---|---|---|
|User1|5|<font color = 'blue'>4|4|<font color = 'blue'>5|5|
|User2|<font color = 'blue'>5|<font color = 'blue'>3|<font color = 'blue'>4|5|3|
|User3|4|<font color = 'blue'>3|<font color = 'blue'>3|2|<font color = 'blue'>5|
|User4|<font color = 'blue'>3|2|<font color = 'blue'>5|<font color = 'blue'>2|4|

<!-- <br>

## 실습
- <a href="https://colab.research.google.com/drive/1QRpgh1VdK05jf33GU-lsrzes25pi2Zdi?usp=sharing">Content-Based Filtering</a>
- <a href="https://colab.research.google.com/drive/1vHBgdTVV2ThvP8mDNhEERjksAaOQ4Uy2?usp=sharing">Collaborative Filtering(KNN)</a>
- <a href="https://drive.google.com/file/d/1OBBn5iazdvwfB_lXAC9KC6QGmPvWNHKX/view?usp=sharing">Collaborative Filtering(Latent Factor)</a>
- <a href="https://colab.research.google.com/drive/10qA4Sma7E7qwblg3FGN8cfnKLXOuVKxv?usp=sharing">Suprise Package</a> -->