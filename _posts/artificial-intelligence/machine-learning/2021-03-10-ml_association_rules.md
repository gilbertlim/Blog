---
title: "[Machine Learning] 연관 규칙(Association Rules)"
category: Machine Learning
use_math: true
---

# Association Rules(연관 규칙)
> 데이터 사이의 연관된 규칙을 찾는 방법 <br>
> 특정 사건 발생 시 함께 자주 발생하는 다른 사건(조건부 확률)의 규칙(Rule)

- 비지도 학습의 일종
- 유사도를 측정하는 방법 중 하나
- 방향성이 존재함
  - Confidence, Lift : $A \rightarrow B$(A를 산사람이 B도 구매)
- 예
    - 상품 A를 구매한 고객이 상품 B를 함께 구매할 확률(구매 패턴) 확인
    - 마케팅에서 고객별 장바구니 내 품목 간 관계 분석

## Support, Confidence, Lift

설명을 위한 Table

|||
|---|---|
|고객1|감자칩, 맥주, 쥐포, 빵|
|고객2|감자칩, 맥주, 쥐포|
|고객3|감자칩, 맥주|
|고객4|감자칩, 콜라|
|고객5|기저귀, 맥주, 쥐포, 빵|
|고객6|기저귀, 맥주, 쥐포|
|고객7|기저귀, 맥주|
|고객8|기저귀, 콜라|


### 1) Support(지지도)
> 특정 품목 집합이 얼마나 자주 등장하는지 확인(예 : 빈번하게 판매되는 물품 확인)

- 특정 품목 집합을 포함하는 거래의 비율로 계산
- 방향성 없음($A \rightarrow B == B \rightarrow A$)

#### 공식
> 전체 거래에 대한 A와 B가 동시에 발생할 확률

$support = \cfrac{A와 B가 포함된 거래 수}{전체 거래 수}$

$support({감자칩, 맥주, 쥐포}) = \cfrac{2}{8}=0.25$

#### Support threshold
> 빈번한 품목 집합을 구분하는 기준

- 임계값보다 지지도가 큰 품목은 빈번히 등장하는 것으로 볼 수 있음

### 2) Confidence(신뢰도)
> 상품 A가 존재할 때 상품 B가 나타나는 빈도

- 상품 A가 포함된 거래 중, 상품 B를 포함하는 거래 비율로 계산
- 방향성 있음($A(원인) \rightarrow B(결과)$)

#### 공식
> 조건(LHS) 발생 시 결과(RHS)가 동시에 일어날 확률<br>
> A가 구매될 때, B가 구매되는 경우의 조건부 확률

$confidence = \cfrac{A(조건)와 B(결과)가 동시에 포함된 거래 수}{A(조건)를 포함한 거래 수}$

$confidence({감자칩} \rightarrow {맥주}) = \cfrac{Support(A, B}{Support(A)} = \cfrac{3}{4} = 0.75$

- 감자칩을 사면 75% 확률로 맥주를 구매한다고 볼 수 있지만,
- 맥주의 판매 빈도를 고려하지 않고 감자칩의 판매 빈도만 고려했기 때문에 맥주 자체가 자주 거래되는 상품이라면 신뢰도를 부풀릴 수 있음
- **Lift를 활용하여 두 물품 모두의 기반 빈도(Base Frequency)를 고려해야 함**

### 3) Lift(향상도)
> 상품 A와 상품 B가 함께 팔리는 빈도

- 두 물품이 각각 얼마나 자주 거래되는지를 고려함
- 방향성 있음($A(원인)\rightarrow B(결과)$))
- Lift 값
  - $1$ : 두 물품 사이에 연관성이 없음(**독립 사건**)
  - $<1$ : 상품 A 거래 시 상품 B가 거래될 가능성 작음
  - $>1$ : 상품 A 거래 시 상품 B가 거래될 가능성 있음

#### 공식
>$Confidence({A}\rightarrow{B})$를 상품B의 빈도($Support({B})$)로 나눔

$Lift = \cfrac{Confidence({A}\rightarrow{B})}{Support({B})}$

$Lift({감자칩}\rightarrow{맥주}) = \cfrac{Confidence({감자칩}\rightarrow{맥주})}{Support({맥주})}=\cfrac{0.75}{0.75}=1$
