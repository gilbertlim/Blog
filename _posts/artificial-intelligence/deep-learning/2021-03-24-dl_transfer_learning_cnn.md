---
title: "[Deep Learning] CNN에서의 전이 학습(Transfer Learning)"
category: Deep Learning
use_math: true
---

<br>

# 1. CNN에서의 전이 학습(Transfer Learning)
> 사전 학습된 모델(Pre-trained Model)을 적용하는 것

사전 학습된 모델은 내가 분류하고자 하는 Class가 유사하면서 많은 양의 데이터로 이미 학습되어 있는 모델이다.

이미 공개되어 있는 모델을 Import해서 사용하면 된다.

<br>

## 1) ILSVC(ImageNet Large Scale Visual Challenge)
> 이미지 인식(= 이미지 분류) 경진대회

ImageNet은 1000여개의 클래스로 분류된 100만장이 넘는 이미지를 담고 있는 데이터셋이다.

ILSVC에서는 이미지넷이 제공하는 이미지 클래스를 분류하여 정확도를 겨룬다.

매년 우승하는 팀의 알고리즘이 공개되는데,

우리가 분류할 이미지들이 이미지넷의 클래스와 유사하다면 공개된 알고리즘을 적용(전이학습)하여 우리의 문제를 해결할 수 있다.

<br>

전이 학습은 두 가지로 나뉜다.

**1) 모델을 그대로 가져와서 사용하는 방법**
  - ILSVC와 동일한 조건이라면 사용해도 되지만, 현실적으로는 부분적으로 가져와서 사용한다.

**2) 모델을 부분적으로 가져와서 사용하는 방법**
  - **특성 추출(Feature Extraction)**
  - **미세 조정(Fine Tuning)**

<br>

이러한 모델들은 내가 분류하고자하는 이미지의 크기와 Class 개수가 다를 수 있으므로, 그대로 적용할 수는 없고 해결할 문제에 맞춰 몇 가지를 수정해야 한다.

따라서 **1) Input Shape 변경, 2) DNN Layer 변경** 등이 필요하다.

<br>

경우에 따라 경진대회에서 사용하지 못하게 하는 경우도 있다.

<br>

종류로는 LeNet, AlexNet, GoogLeNet, ResNet, VGGNet 등이 있다.

<br>

**역대 우승 알고리즘의 에러율**
![](/assets/images/posts/dl/ilsvc.png)
https://cvml.tistory.com/1

<br>

## 2) CNN 알고리즘의 종류

### (1) LeNet
> Yann Lecun 연구팀에서 개발한 최초의 CNN 구조

- Convolution, Subsampling(Max Pooling), Full Connection(Classification) 구조를 만들었다.

### (2) AlexNet
> 캐나다 토론토 대학에서 개발한 알고리즘 <br>
> LeNet과 유사한 구조이며 2개의 CPU로 병렬연산을 수행한다는 차이점이 있다.

![](/assets/images/posts/dl/alexnet.png)
https://medium.com/coinmonks/paper-review-of-alexnet-caffenet-winner-in-ilsvrc-2012-image-classification-b93598314160

### (3) GoogLeNet
> Google이 개발한 알고리즘<br>
> Inception Module을 사용하는 알고리즘

- 총 22개의 Layer와 9개의 Inception Module로 구성됨
- Inception Module 
  입력값에 대해 4가지 종류의 Convolutionm Pooling을 수행하고, 합치는 모듈로서 다양한 종류의 Feature를 도출할 수 있다.
  
![](/assets/images/posts/dl/googlenet.png)
https://kangbk0120.github.io/articles/2018-01/inception-googlenet-review

### (4) ResNet
> Microsoft가 개발한 알고리즘 <br>
> GoogLeNet의 7배인 152 Layer로 구성된 알고리즘

- VGG-19의 구조를 뼈대로 하고, Convolution Layer를 추가해서 깊게 만들고 Shortcut을 추가한 구조
- Layer가 깊어져 학습이 되지 않는 문제를 'Skip Connection'을 통해 해결함

![](/assets/images/posts/dl/resnet.jpeg)
![](/assets/images/posts/dl/residualblock.png)
https://bskyvision.com/644

### (5) VGGNet
> 옥스포드 대학에서 개발한 알고리즘 <br>
> Layer의 개수에 따라 VGG16, VGG19로 불린다.

![](/assets/images/posts/dl/vggnet.png)
https://ichi.pro/ko/geomto-vggnet-ilsvrc-2014ui-1-wi-jun-useung-imiji-bunlyu-useungja-hyeonjihwa-228850174016017

<br>

## 3) 전이 학습 방법
> 전이 학습은 사전 학습된 모델을 그대로 가져와 학습하는 방법과 부분적으로 가져와 사용하는 방법이 있다.<br>

### (1) 특성 추출(Feature Extraction)
> 사전 학습된 모델을 사용하여 <a href="https://gilbertlim.github.io/deep%20learning/dl_cnn/">Feature Extraction</a> 목적으로 사용하는 방법

CNN은 Feature Extraction + Classification 구조로 되어 있다.

특성 추출은 사전 학습된 모델의 파라미터를 재사용하는 방법으로, 이미 학습된 파라미터를 다시 사용하겠다는 의미이다(**학습이 아님**).

즉, 내가 입력한 이미지가 이미 다른 모델에 의해 학습된 파라미터에 의해 특성이 추출되는 것이다.

ImageNet 데이터를 사용하여 만들어진 모델들은 1000개의 클래스를 분류하도록 되어있기 때문에

특성 추출을 사용하기 위해서는 자신이 만들 모델의 목적에 맞게 DNN Layer를 변경해서 사용해야 한다.

<br>

### (2) 미세 조정(Fine Tuning)
> 사전 학습된 모델을 미세하게 튜닝하여 재학습하는 방법

특성 추출이 사전 학습된 모델의 파라미터를 전부 그대로 가져왔다면 

미세 조정은 필요한 부분만 가져오고 나머지는 조정하여 사용하는 방법이다.

앞 단 Layer보다 **말단 Layer의 Feature Map이 더 많은 특징을 포함**하고 있기 때문에

주로 모델의 Feature Extraction 단계에서 말단 Layer를 조정한다.

말 단 Feature Map에는 "우리가 준비한 이미지의 특성을 좀 더 반영하겠다" 라는 것이다.

DNN Layer는 Feature Extaction과 마찬가지로 분류 목적에 맞게 새로 정의하여 재학습한다.

<br>

그대로 사용할 Layer는 Frozen(trainable = True)시키고, 변경할 Layer는 Unfrozen(trainable = False)하여 사용한다.

Layer가 A > B > C > D > E일 때, 

A > B > C 를 그대로 사용하고 나머지를 수정하려면 수정하려면, D를 Unfrozen 시키면 모델에서 Frozen된 A > B > C만 가져온다.

<br>

이미 학습된 모델의 일부를 가져와서 재학습하는 것이므로, 오차 함수의 **학습률(Learning Rate)을 낮게 주어야 한다.**

이미 학습된 모델은 경사하강법에 의해 이미 Error가 최소가 되게끔 파라미터가 학습되어 있다.

이러한 모델을 다시 학습할 때 학습률을 크게 준다면 에러가 커지는 방향으로 발산하는 문제가 생긴다.

따라서, 꼭 **학습률을 낮게 설정**해야 한다.

<!-- <br>

## 실습
- <a href="https://colab.research.google.com/drive/1lElwqN_3hbV2fJVXmT7SYYzWcWJhbGNV?usp=sharing">Cats and Dogs 분류 - VGG 16 Feature Extraction</a>
- <a href="https://colab.research.google.com/drive/1Oi8_W6S0Wlc6LLoVpKXhNIT9amTRHeIb?usp=sharing">Cats and Dogs 분류 - VGG 16 Fine Tuning</a>
- <a href="https://drive.google.com/file/d/1_hjxCj_EMbL5JxY9ei3kuKQvrQWy4YJB/view?usp=sharing">CNN 모델의 Feature Map 생성 과정 시각화</a>
- <a href="https://colab.research.google.com/drive/1G69Eqa5S00XsRcB9Onr7J2RiCTAkxjQN?usp=sharing">MobileNetV2</a> -->