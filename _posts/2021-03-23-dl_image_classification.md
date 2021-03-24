---
title: "[Deep Learning] 이미지를 분류하면서 나타나는 문제들"
category: Deep Learning
use_math: true
---

<br>

# 1. 이미지를 분류하면서 나타나는 문제들

## 1) 이미지 사이즈
> 이미지들의 사이즈가 각기 다른 경우 어떻게 할까?

이미지를 동일한 크기로 줄일 수 있다. 이미지를 줄이면서 가로, 세로 비율이 변경되어도 '분류'에서는 크게 문제되지 않는다.

하지만, '인식'에서는 문제가 될 수 있다.

<br>

## 2) 이미지 라벨링
> 이미지를 구했는데, 어떤 이미지인지 라벨링되지 않았다. 어떻게 할까?

이미지를 라벨링하고, Class 별로 디렉토리를 만들어 이미지를 저장하여 사용한다.

예 : cat 폴더에는 고양이 사진, dog 폴더에는 강아지 사진을 나눠서 저장

### 디렉토리에 Target 별로 나누어 저장된 이미지의 크기를 변경하는 방법(ImageDataGenerator)
```python
train_dir = 'train'
valid_dir = 'validation'

from keras.preprocessing.image import ImageDataGenerator

train_datagen = ImageDataGenerator(rescale = 1./255)
valid_datagen = ImageDataGenerator(rescale = 1./255)

train_generator = train_datagen.flow_from_directory( # 폴더별로 이미지 구분
                  train_dir,
                  target_size = (75, 75), # 이미지를 75 x 75 픽셀 크기로 변경
                  batch_size = 8,
                  class_mode = 'categorical')

valid_generator = valid_datagen.flow_from_directory(
                  valid_dir,
                  target_size = (75, 75),
                  batch_size = 8,
                  class_mode = 'categorical')

Hist_dandc = model.fit(train_generator,
                       steps_per_epoch = 62,
                       epochs = 200,
                       validation_data = valid_generator,
                       validation_steps = 22)
```

<br>

## 3) 이미지 개수
> 데이터 포인트가 적을 때 Overfitting이 발생하기 쉽다. <br>
> Overfitting을 회피하기 위해서는 더 많은 이미지 데이터가 필요하다.

데이터를 추가적으로 획득하는 방법과 **기존 데이터로부터 더 많은 데이터를 생성하는 방법**이 있다.

### Image Augmentation(이미지 증강)
> 기존 이미지 데이터에 변화를 주면서 더 많은 데이터를 생성하는 방법

![](/assets/images/posts/dl/cat.png)

하나의 이미지를 변화시켜 여러개 이미지로 바꾸었다.

사람은 같은 이미지로 생각하지만, CNN에서는 다른 이미지로 생각한다.

**Train Data**에 대해서만 사용해야 의미가 있다.

#### ImageDataGenerator를 사용한 이미지 증강 방법

- <a href="https://keras.io/api/preprocessing/image/">파라미터</a>

```python
from keras.preprocessing.image import ImageDataGenerator

train_datagen = ImageDataGenerator(rescale = 1./255,
                                   rotation_range = 40,
                                   width_shift_range = 0.2,
                                   height_shift_range = 0.2,
                                   shear_range = 0.2,
                                   zoom_range = 0.2,
                                   horizontal_flip=True,
                                   vertical_flip = True,
                                   fill_mode = 'nearest')

train_generator = train_datagen.flow_from_directory(train_dir, 
                                                    target_size = (150, 150),
                                                    batch_size = 20,
                                                    class_mode = 'binary')
# 모델 학습 시 train_generator를 입력으로 사용
```