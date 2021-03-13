---
title: "[Machine Learning] K-평균 군집(K-means Clustering)"
category: Machine Learning
use_math: true
---

# K-means Clustering(K-평균 군집)
> 데이터 간 포함관계를 확인하기 위해 입력된 데이터를 K개의 군집(Cluster)으로 묶는 분석 방법

- 비지도학습의 일종
- **동일한 군집에 속하는 데이터는 유사성이 높고, 군집 간에는 유사성이 낮음**
- 데이터간 유사성(Similarity)을 계산하여 유사성이 높은 개체의 군집을 생성
- 유사한 값을 갖는 특징을 적은 개수의 동질적 그룹으로 단순화
- **거리 계산을 위해 정량화할 수 있는 데이터가 필요함(명목형 X)**

## **동작 방식**
  - 최초 K개의 의사 중심점(Pseudo Center) 지정
  - **군집 내**의 데이터들 간의 거리를 **최소화**
  - **군집 간**의 거리를 **최대화**
  - 분류된 데이터들의 평균점을 구하고 이동하는 과정을 반복
  - 데이터는 오직 1개의 군집에만 포함됨

## K
> 몇 개의 군집으로 분류할 것인가?

- 비즈니스 의사결정에 도움이 되는 수를 권장
- 군집의 개수를 늘릴수록 **군집 내 유사성은 증가, 군집 간 차이점은 감소함**
  - 군집이 너무 많으면, 군집 간 거리가 가까워져 군집 간 차이점이 없어지게 된다.
- Scree Plot(스크리 도표) 활용

## 실습
- <a href="https://colab.research.google.com/drive/1H3_GzAE_etK7Qc1S-A1S8WzxwZkGR_A_?usp=sharing">K-means Clustering</a>