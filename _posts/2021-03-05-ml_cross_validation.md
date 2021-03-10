---
title: "[Machine Learning] 교차 검증(Cross Validation)"
category: Machine Learning
use_math: true
---

# Cross Validation(CV, 교차 검증)
> Overfitting 방지를 위해 수행되는 검증 방법
 
- 다양하게 Training Data와 Validation Data를 변경하면서 Model Validation 수행
- Validation을 한 번만 수행하면 특정 Data에만 최적화 될수 있기 때문에 교차 검증 실시

## K-Fold Cross Validation
> Training Data를 무작위로 균등하게 K개 그룹으로 나누어서 검증하는 방법
>
> 최적의 파라미터를 찾기 위한 방법 중 하나

![](/assets/images/posts/ml/K-fold.png)

### 과정
- Training Data를 K개 그룹으로 나눔
- 각 그룹(Training Data)을 (K-1) 개의 Training Fold와 1개의 Validation Fold로 나눔
- Training Fold로 학습을 진행하고, Validation Fold에 대해 성능을 측정함
- 총 K개 그룹 결과의 평균을 측정하여 모델의 최적 parameter를 찾음
- 최적 parameter로 모델을 학습시킨 후 Test Data에 검증을 수행함

### 특징
- **K** : Hyperparameter
    - 일반적으로 5 ~ 10 사용
- Data가 충분히 많다면, K-Fold Cross Validation
- Data가 매우 적다면, 데이터 개수만큼 Cross Validation 수행(LOOCV)


### 예제
> GridSearchCV에서 cv 파라미터로 교차 검증을 사용할 수 있음

```python
from sklearn.model_selection import GridSearchCV, KFold

grid_cv = GridSearchCV(Model_rf,
                       param_grid = params,
                       scoring = 'accuracy',
                       cv = KFold(n_splits = 5,
                                  random_state = 2045),
                       refit = True,
                       n_jobs = -1)
```