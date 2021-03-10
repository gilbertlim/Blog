---
title: "[Machine Learning] 주성분 분석(Principal Component Analysis)"
category: Machine Learning
use_math: true
---

# 주성분 분석(Principal Component Analysis, PCA)
> Data points를 가장 잘 구별해주는 주성분(배후의 변수)을 찾는 기법<br>
> 각 항목을 가장 잘 구별해주는 변수를 찾는 일

- **차원 축소 기법**
  - 주성분(적은수의 변수)으로 전체 데이터 세트 표현 가능
  - 데이터 세트에서 의미 있는 선(축)을 찾는 과정
- 주성분
    - Data points가 가장 넓게 분포(분산이 큰)하는 차원
    - 가장 많은 정보를 포함한 차원을 따라 데이터가 넓게 분포함
- 각 차원이 직교인 경우 PCA가 유용함
- 비지도 학습의 일종

<br>

### 고차원 공간의 데이터를 저차원 공간으로 변환(2차원 -> 선형)
<a href="https://stats.stackexchange.com/questions/2691/making-sense-of-principal-component-analysis-eigenvectors-eigenvalues">그림 출처</a>

![](/assets/images/posts/ml/pca.gif)
