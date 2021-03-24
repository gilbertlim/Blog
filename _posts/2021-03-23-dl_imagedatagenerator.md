---
title: "[Deep Learning] ImageDataGenerator()"
category: Deep Learning
use_math: true
---

<br>

# 1. 이미지를 분류하면서 나타나는 문제들

## 1) 이미지 사이즈
> 이미지들의 사이즈가 각기 다른 경우 어떻게 할까?

이미지를 동일한 크기로 줄일 수 있다. 이미지를 줄이면서 가로, 세로 비율이 변경되어도 '분류'에서는 크게 문제되지 않는다.

하지만, '인식'에서는 문제가 될 수 있다.

**-> ImageDataGenerator를 사용하면 이미지 사이즈를 손쉽게 조절할 수 있다.**

<br>

## 2) 이미지 라벨링
> 이미지를 구했는데, 어떤 이미지인지 라벨링되지 않았다. 어떻게 할까?

이미지를 라벨링하고, **Class 별로 디렉토리를 만들어 이미지를 저장**하여 사용한다.

예 : cat 폴더에는 고양이 사진, dog 폴더에는 강아지 사진을 나눠서 저장

**-> ImageDataGenerator를 사용하면 폴더에 저장된 이미지를 그대로 가져와서 학습에 사용할 수 있다.**

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

'Train/Validation Data'에 대해서만 사용해야 의미가 있다.

**-> ImageDataGenerator를 사용하면 이미지 증강을 적용할 수 있다.**

<br>
<br>

# 2. ImageDataGenerator()
> **디렉토리에 Target 별로 나누어 저장된 이미지 파일** 또는 텐서로 저장된 이미지를 바로 가져와서 전처리하고 모델에 집어넣는 방법

- ImageDataGenerator는 이미지 텐서 정규화, 이미지 크기 변경, 이미지 증강(Image Augmentation) 등 이미지 전처리 기능을 제공한다. 
- 이미지들이 디렉토리별로 나누어져 있다면 ImageDataGenerator 클래스의 flow_from_directory 메서드를 사용할 수 있는데, 배치사이즈 지정, 클래스 모드 설정이 가능하다.
- <a href="https://keras.io/api/preprocessing/image/">파라미터 종류</a>

## 1) 디렉토리에 저장된 이미지 파일을 사용하는 경우 

### (1) valid 데이터가 따로 저장되어 있는 경우

```python
train_dir = 'train'
valid_dir = 'validation'
test_dir = 'test'

batch_size = 20

from keras.preprocessing.image import ImageDataGenerator

# train data 전처리 옵션 설정
train_datagen = ImageDataGenerator(rescale = 1./255, # 정규화
                                   
                                   # 이미지 증강
                                   rotation_range = 40,
                                   width_shift_range = 0.2,
                                   height_shift_range = 0.2,
                                   shear_range = 0.2,
                                   zoom_range = 0.2,
                                   horizontal_flip=True,
                                   vertical_flip = True,
                                   fill_mode = 'nearest')

# 'train_dir' 디렉토리에 저장된 이미지 전처리
train_generator = train_datagen.flow_from_directory(train_dir, # 디렉토리명  
                                                    target_size = (150, 150), # 이미지 크기 변경
                                                    batch_size = batch_size, # 배치 사이즈 지정
                                                    class_mode = 'categorical') # 클래스 모드 지정                         

###################################################################################################

# validation data 전처리 옵션 설정
valid_datagen = ImageDataGenerator(rescale = 1./255,
                                   rotation_range = 40,
                                   width_shift_range = 0.2,
                                   height_shift_range = 0.2,
                                   shear_range = 0.2,
                                   zoom_range = 0.2,
                                   horizontal_flip=True,
                                   vertical_flip = True,
                                   fill_mode = 'nearest')

# 'valid_dir' 디렉토리에 저장된 이미지 전처리
valid_generator = valid_datagen.flow_from_directory(valid_dir,  # 디렉토리명  
                                                    target_size = (150, 150),
                                                    batch_size = batch_size,
                                                    class_mode = 'categorical')

###################################################################################################

# test data 전처리 옵션 설정
test_datagen = ImageDataGenerator(rescale = 1./255) # test data에는 이미지 증강을 사용하지 않는다.


# 'test_dir' 디렉토리에 저장된 이미지 전처리
test_generator = test_datagen.flow_from_directory(test_dir, # 디렉토리명  
                                                  target_size = (150, 150),
                                                  batch_size = batch_size,
                                                  class_mode = 'categorical')         

###################################################################################################

# 컴파일
model.compile(loss='categorical_crossentropy',
              optimizer='adam',
              metrics=['accuracy'])

# 학습
history = model.fit(train_generator,
                  steps_per_epoch = train_generator.samples//batch_size,                   
                  epochs=100,
                  validation_data=valid_generator,
                  validation_steps = valid_generator.samples//batch_size)

# 평가
loss, accuracy = model.evaluate(test_generator,
                                steps = test_generator//batch_size)
print('Loss = {:.5f}'.format(loss))
print('Accuracy = {:.5f}'.format(accuracy))
```


### (2) valid 데이터가 따로 저장되어 있지 않아 train 데이터에서 split 해서 사용하고 싶은 경우
```python
train_dir = 'train'
valid_dir = 'validation'
test_dir = 'test'

batch_size = 20

from keras.preprocessing.image import ImageDataGenerator

# train data 전처리 옵션 설정
train_datagen = ImageDataGenerator(rescale = 1./255, # 정규화
                                   
                                   # 이미지 증강 옵션
                                   rotation_range = 40,
                                   width_shift_range = 0.2,
                                   height_shift_range = 0.2,
                                   shear_range = 0.2,
                                   zoom_range = 0.2,
                                   horizontal_flip=True,
                                   vertical_flip = True,
                                   fill_mode = 'nearest',
                                   validation_split=0.2) # validation split 비율 설정

# 'train_dir' 디렉토리에 저장된 이미지 전처리
train_generator = train_datagen.flow_from_directory(train_dir, # 디렉토리명  
                                                    target_size = (150, 150), # 이미지 크기 변경
                                                    batch_size = 20, # 배치 사이즈 지정
                                                    class_mode = 'categorical', # 클래스 모드 지정
                                                    subset='training') # training data로 명시                         

###################################################################################################

# 'train_dir' 디렉토리에 저장된 이미지의 일부를 validation data로 split하여 전처리(같은 객체 사용)
valid_generator = train_datagen.flow_from_directory(train_dir, # train data와 같은 디렉토리명 지정  
                                                    target_size = (150, 150),
                                                    batch_size = 20,
                                                    class_mode = 'categorical',
                                                    subset='valdation') # validation data로 설정

###################################################################################################

# test data 전처리 옵션 설정
test_datagen = ImageDataGenerator(rescale = 1./255) # test data에는 이미지 증강을 사용하지 않는다.


# 'test_dir' 디렉토리에 저장된 이미지 전처리
test_generator = test_datagen.flow_from_directory(test_dir, # 디렉토리명  
                                                  target_size = (150, 150),
                                                  batch_size = batch_size,
                                                  class_mode = 'categorical')     

###################################################################################################

# 컴파일
model.compile(loss='categorical_crossentropy',
              optimizer='adam',
              metrics=['accuracy'])

# 학습
history = model.fit(train_generator,
                  steps_per_epoch = train_generator.samples//batch_size,                   
                  epochs=100,
                  validation_data=valid_generator,
                  validation_steps = valid_generator.samples//batch_size)

# 평가
loss, accuracy = model.evaluate(test_generator,
                                steps = test_generator//batch_size)
print('Loss = {:.5f}'.format(loss))
print('Accuracy = {:.5f}'.format(accuracy))
```

<br>

## 2) 텐서로 변환된 이미지를 사용하는 경우 
### (1) valid 데이터가 따로 저장되어 있는 경우

```python
batch_size = 100

from keras.preprocessing.image import ImageDataGenerator

# train data 전처리 옵션 설정
train_datagen = ImageDataGenerator(rescale = 1./255, # 정규화
                                   
                                   # 이미지 증강
                                   rotation_range = 40,
                                   width_shift_range = 0.2,
                                   height_shift_range = 0.2,
                                   shear_range = 0.2,
                                   zoom_range = 0.2,
                                   horizontal_flip=True,
                                   vertical_flip = True,
                                   fill_mode = 'nearest')

# 'train data' 이미지 전처리
train_generator = train_datagen.flow(X_train, y_train,  
                                     batch_size = batch_size)                         

###################################################################################################

# validation data 전처리 옵션 설정
valid_datagen = ImageDataGenerator(rescale = 1./255,
                                   rotation_range = 40,
                                   width_shift_range = 0.2,
                                   height_shift_range = 0.2,
                                   shear_range = 0.2,
                                   zoom_range = 0.2,
                                   horizontal_flip=True,
                                   vertical_flip = True,
                                   fill_mode = 'nearest')

# 'validation data'  이미지 전처리
valid_generator = valid_datagen.flow(X_valid, y_valid,  
                                     batch_size = batch_size)
                                                    
###################################################################################################

# test data 전처리 옵션 설정
test_datagen = ImageDataGenerator(rescale = 1./255) # test data에는 이미지 증강을 사용하지 않는다.


# 'test data' 이미지 전처리
test_generator = test_datagen.flow(X_test, y_test,  
                                    batch_size = batch_size)
         
###################################################################################################

# 컴파일
model.compile(loss='categorical_crossentropy',
              optimizer='adam',
              metrics=['accuracy'])

# 학습
history = model.fit(train_generator,
                  steps_per_epoch = len(X_train)//batch_size,                   
                  epochs=100,
                  validation_data=valid_generator,
                  validation_steps = len(X_valid)//batch_size)

# 평가
loss, accuracy = model.evaluate(test_generator,
                                steps = len(X_test)//batch_size)
print('Loss = {:.5f}'.format(loss))
print('Accuracy = {:.5f}'.format(accuracy))
```


### (2) valid 데이터가 따로 저장되어 있지 않아 train 데이터에서 split 해서 사용하고 싶은 경우

```python
batch_size = 100

from keras.preprocessing.image import ImageDataGenerator

# train data 전처리 옵션 설정
train_datagen = ImageDataGenerator(rescale = 1./255, # 정규화
                                   
                                   # 이미지 증강
                                   rotation_range = 40,
                                   width_shift_range = 0.2,
                                   height_shift_range = 0.2,
                                   shear_range = 0.2,
                                   zoom_range = 0.2,
                                   horizontal_flip=True,
                                   vertical_flip = True,
                                   fill_mode = 'nearest',
                                   validation_split=0.2)

# 'train data' 이미지 전처리
train_generator = train_datagen.flow(X_train, y_train,  
                                     batch_size = batch_size,
                                     subset='training')                         

###################################################################################################

# 'validation data'  이미지 전처리
valid_generator = train_datagen.flow(X_train, y_train,  
                                     batch_size = batch_size,
                                     subset='valdation')
                                                    
###################################################################################################

# test data 전처리 옵션 설정
test_datagen = ImageDataGenerator(rescale = 1./255) # test data에는 이미지 증강을 사용하지 않는다.


# 'test data' 이미지 전처리
test_generator = test_datagen.flow_(X_test, y_test,  
                                    batch_size = batch_size)
         
###################################################################################################

# 컴파일
model.compile(loss='categorical_crossentropy',
              optimizer='adam',
              metrics=['accuracy'])

# 학습
history = model.fit(train_generator,
                  steps_per_epoch = len(X_train)*0.8//batch_size,                   
                  epochs=100,
                  validation_data=valid_generator,
                  validation_steps = len(X_train)*0.2//batch_size)

# 평가
loss, accuracy = model.evaluate(test_generator,
                                steps = len(X_test)//batch_size)
print('Loss = {:.5f}'.format(loss))
print('Accuracy = {:.5f}'.format(accuracy))
```

- 학습 옵션
  - steps_per_epoch
      - 한 번 epoch를 돌 때 Train set을 몇 번 볼 것인지 설정
      - Train data 수 / 배치 사이즈
  
  - validation_steps
      - 한 번 epoch를 돌고난 후 validation accuracy를 측정할 때 validation set을 몇 번 볼 것인지 설정
      - validation data 수 / 배치 사이즈