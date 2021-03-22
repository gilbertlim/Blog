---
title: "[Deep Learning] Early Stopping"
category: Deep Learning
use_math: true
---

<br>

# Early Stopping

![](/assets/images/posts/dl/es.png)

위와 같이 오차가 나타나는 경우, 일반적으로 학습을 계속 진행하여도 오차가 다시 내려가는 경우는 드물다.

그렇다면 오차가 가장 적은 지점을 찾고 싶고, 그 지점에서 학습을 멈춘다면 오차가 가장 적은 모델이 아닌가? 

이러한 것을 도와주는 콜백 함수가 있다.

바로 EarlyStopping이다.

사용 방법은 아래와 같다.

## 1) EarlyStopping() 객체 생성
- monitor : 모니터링 대상 성능
- mode : 모니터링 대상을 최소화(min) 또는 최대화(max)
- patience : 성능이 개선되지 않는 epoch 횟수(patience=50 : 50번 연속 최소값보다 낮은 값을 찾지 못하면 학습 종료)
- verbose : 출력 결과를 자세히 보기 위한 옵션

```python
from keras.callbacks import EarlyStopping

es = EarlyStopping(monitor='val_mae',
                   mode='min',
                   patience=50,
                   verbose=1)
```

<br>

에러가 최솟값을 돌파할때마다 모델을 파일로 저장하고 싶다면, ModelCheckpoint 함수와 같이 사용할 수 있다.
- 'best_boston.h5' : 최적 모델이 저장될 경로
- save_best_only : 최적 모델만 저장할지 결정하는 옵션

```python
from keras.callbacks import ModelCheckpoint

mc = ModelCheckpoint('best_boston.h5',
                     monitor='val_mae',
                     mode='min',
                     save_best_only=True, 
                     verbose=1)
```

<br>

## 2) Model Fit with callbacks
- Model을 학습(Fit)시킬 때 옵션으로 추가하면 된다.

```python
Hist_boston = boston.fit(X_train, y_train,
                         epochs=500,
                         batch_size=1,
                         validation_data=(X_valid,y_valid),
                         callbacks=[es, mc], # modelCheckpoint를 사용하지 않는다면 mc는 생략 가능
                         verbose=1)
```

### 실습
- <a href="https://colab.research.google.com/drive/1uWfvoWjiygaL9h_YKqT0Jkm4liatu6UU?usp=sharing">회귀 분석</a>