---
title: "[Machine Learning] GridSearchCV(for Hyparameter Tuning)"
category: Machine Learning
use_math: true
---

# GridSearchCV()
> sklearn에서 Hyperparameter Tuning 시 사용 되는 방법
> 
> 입력한 parameters의 경우의 수를 모두 모델에 적용하여, 지정한 score가 높은 parameters의 조합을 찾아내는 클래스

- 무조건 모델이 최적화 되었다고 맹신할 수는 없음
- 모든 파라미터들의 조합을 확인하므로 시간이 오래 걸림
