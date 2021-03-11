---
title: "[Practical Statistics] Descriptive Statistics(기술 통계)"
category: Practical Statistics
use_math: true
---

# Descriptive Statistics(기술 통계)

## 1. 정의

### 1) Descriptive
> 적는다

### 2) Statistics
> 데이터를 수집 및 정리하여 **특징을 확인**하는 것

- 통계학에서 최종적으로 알고 싶은 것 : **모집단(전체 집합)의 특징**
- 특징 : **<mark style='background-color: yellow'>중심화 경향치, 산포도</mark>**

### 3) Descriptive Statistics(기술 통계)
> 모집단의 특징을 확인하기 위해 데이터를 수집, 분석, 시각화하는 것

- 특정 집단에 관한 현상을 **수학적으로 연산**하여 기술
- 특징을 기술(설명)할 수 있으면 어떤 사실을 **객관적으로 표현** 가능
- 하나의 수치로 결론을 내릴순 없지만, **결론을 향한 기반을 제공**

<br>

## 2. 통계 변수와 척도
> 통계 변수에 따라 분석 방법이 달라진다. <br>
> 변수는 머신러닝 관점에서 X, Input, Feature와 동일함

<table>
    <tr>
        <td></td>
        <td align="center">통계 척도(Scale)</td>
        <td align="center">연산</td>
    </tr>
    <tr>
        <td align="center" rowspan="2">범주형 변수<br>(정성적)</td>
        <td>명목형(명명) 척도</td>
        <td align="center">==, !=</td>
    </tr>
    <tr>
        <td>순서형(서열) 척도</td>
        <td align="center"> &gt;, &lt; , &gt;&#61;, &lt;&#61;</td>
    </tr>
    <tr>
        <td align="center" rowspan="2">수치형 변수<br>(정량적)</td>
        <td>이산형(등간) 척도</td>
        <td align="center">+, -</td>
    </tr>
    <tr>
        <td>연속형(비율) 척도</td>
        <td align="center">*, /</td>
    </tr>
</table>

- 명명(Nominal) : 측정값의 같고 다름만 확인, 순서 없음(A형, O형, B형)
- 서열(Ordinal) : 순서가 있지만, 간격이 동일하지 않음(금메달, 은메달, 동메달)
- 등간(Interval) : 순서가 있고, 간격이 동일함, '0'의 의미가 없음(온도), 셀 수 있음
- 비율(Ratio) : '0'의 의미가 있고, 사칙연산이 모두 가능함, 셀 수 없음

### 0 의 의미
> 아무것도 존재하지 않는 상태(절대 영점)

- 이산형(등간) 척도 : 온도가 0도라는 것은 과학적으로 정의한 숫자를 0으로 표기한 것이므로 아무것도 없는 상태가 아님
- 연속형(비율) 척도 : 길이가 0이라는 것은 아무것도 없는 상태를 의미함

<br>
  
## 3. 모집단의 특징

### 1) 중심화 경향치(Measure of Central Tendency)
> 무엇을 중심으로 모여 있는 가? <br>
> 집단을 대표할 가능성이 큰 값

#### 중심화 경향치 종류

##### 평균(mean)
> 전체 데이터를 더한 다음, 데이터의 개수로 나눠준 값

- 이상치(Outlier)
    > 평균에 급격하게 영향을 주는 값

    - 이상치 때문에 집단을 대표하지 못할 수도 있음


##### 중앙값(median)
> 전체 데이터를 크기 순으로 정렬하여 순서상 중앙에 위치한 값

##### 최빈값(mode)
> 전체 데이터에서 가장 빈번하게 관찰된 값

<br>

### 2) 산포도(Degree of Dispersion)
> 집단 내 데이터가 흩어져 있는 정도

- 중심화 경향치를 기준으로 얼마나 떨어져 있는가?(이론적으로 중심화 경향치가 먼저 계산되어야 함)
- 중심화 경향치가 없다면 범위(Range)로 산포 확인 가능

#### 산포도 종류

##### 범위(Range)
> 연속형 값의 최솟값과 최댓값 사이의 거리

- 범위는 Outlier를 포함하고 있음
- $ Range = 최댓값 - 최솟값$

##### 사분위 범위(Interquartile Range)
> 0%, 25%, 50%, 75%, 100% 구간으로 나눈 사분위수에서<br> 25%와 75% 사이 값들(Boxplot)

![](/assets/images/posts/statistics/boxplot.png)
https://towardsdatascience.com/understanding-boxplots-5e2df7bcbd51

##### 분산(Variance)
> 관측치들이 평균(중심 경향치)에서 평균적으로 엄라나 떨어져 있는지 확인
> 편차값(관측치 - 평균)의 제곱의 합을 데이터의 수로 나눈 값>

- 편차값으로는 흩어진 정도를 조사할 수 없으므로, 편차값 제곱의 평균으로 계산
- 제곱하였기 때문에 값이 너무 커지는 단점이 있다.

###### ddof(degree of freedom)
> pandas와 numpy에서 분산과 표준편차 계산 시 모분산/표본 분산을 고려하여 계산해야 한다.

- 모분산
  - 편차값 제곱의 합을 n으로 나눔
  - ddof = 0(numpy default)
-  표본분산(불편 분산)
  - 편차값 제곱의 합을 n-1으로 나눔
  - ddof = 1(pandas default)

##### 표준편차(Standard deviation)
> 분산의 양의 제곱근

<br>

## 실습
<a href="https://colab.research.google.com/drive/1LjQmR_ojtdJTXgjjWFgFDQiAlwuP2Y3w?usp=sharing">Descriptive Statistics</a>