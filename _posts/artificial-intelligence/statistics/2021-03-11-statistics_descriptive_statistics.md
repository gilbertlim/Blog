---
title: "[Practical Statistics] 기술통계(Descriptive Statistics)"
category: Practical Statistics
use_math: true
---

# Descriptive Statistics(기술 통계)

## 1. 정의

### 1) Descriptive
> 기술하다(적다)

### 2) Statistics
> 데이터를 수집 및 정리하여 **특징을 확인**하는 것

- 통계학에서 최종적으로 알고 싶은 것 : **모집단(전체 집합)의 특징**
- 특징 : **<mark style='background-color: yellow'>중심화 경향치, 산포도</mark>**

### 3) Descriptive Statistics(기술 통계)
> 모집단의 특징을 확인하기 위해 데이터를 수집, 분석, 시각화하는 것

- 특정 집단에 관한 현상을 **수학적으로 연산**하여 기술하는 것
- 특징을 기술(설명)할 수 있으면함, 어떤 사실을 **객관적으로 표현** 가능하다.
- 하나의 수치로 결론을 내릴순 없지만, **결론을 향한 기반을 제공함**

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
        <td align="center" rowspan="2">범주형 변수<br>(Categorical)</td>
        <td>명목형(Nominal)</td>
        <td align="center">==, !=</td>
    </tr>
    <tr>
        <td>순서형(Ordinal)</td>
        <td align="center"> &gt;, &lt; , &gt;&#61;, &lt;&#61;</td>
    </tr>
    <tr>
        <td align="center" rowspan="2">수치형 변수<br>(Numerical)</td>
        <td>구간형(Interval)</td>
        <td align="center">+, -</td>
    </tr>
    <tr>
        <td>비율형(Ration)</td>
        <td align="center">*, /</td>
    </tr>
</table>

### 범주형(Categorical) 변수
- 명목형(Nominal)
  - 측정값의 같고 다름만 확인 가능
  - 순서 없음 
  - 예 : 혈액형, 성별, 결혼여부(이진)
  - 시각화 방법 : Pie Chart, Bar Chart
- 순서형(Ordinal)
  - **순서 있음(값의 차이는 의미 없음)**
  - 예 : 서비스 만족도, 학력, 사회계급
  - 시각화 방법 : Bar Chart(열의 순서 중요)

### 수치형(Numerical) 변수
- 구간형(Interval)
  - 순서가 있고, 간격이 동일함
  - **'0'의 의미가 없음(상대 영점)**
  - **셀 수 있음(Finite)**
  - 소수점이 의미가 없음(자동차는 1대, 2대로 세지만, 1.5대로 세지 않음)
  - 예 : 온도, 발생 빈도, 성적
  - Visualization Methods : Bar Chart(열의 순서 중요)
- 비율형(Ratio)
  - 사칙연산이 모두 가능함  
  - **'0'의 의미가 있음(절대 영점)**
  - **셀 수 없음(Infinite)**
  - 소수점이 의미가 있음
  - 예 : 나이, 키, 무게, 길이, 혈압 
  - Visualization Methods : Boxplot, Histogram
  
### 수치적인 의미에 따른 분류
- 이산형 : 수치적인 의미를 가지고 있으나, 소수점으로 표현되지 않는 경우(예 : 1.5명)
- 연속형 : 수치적인 의미 / 소수점 표현 가능 / 측정 가능 데이터

### 0 의 의미
> 아무것도 존재하지 않는 상태(절대 영점)

- 구간형(Interval) : 온도가 0도라는 것은 과학적으로 정의한 숫자를 0으로 표기한 것이므로 비율적 의미를 부여할 수 없음
- 비율형(Ratio) : 길이가 0이라는 것은 아무것도 없는 상태를 의미함

<br>
  
## 3. 모집단의 특징

### 1) 중심화 경향치(Measure of Central Tendency)
> 무엇을 중심으로 모여 있는 가? <br>
> 집단을 대표할 가능성이 큰 값

#### 중심화 경향치 종류

##### (1) 평균(mean)
> 전체 데이터를 더한 다음, 데이터의 개수로 나눠준 값

- 이상치(Outlier)
    > 평균에 급격하게 영향을 주는 값

    - 이상치 때문에 집단을 대표하지 못할 수도 있음


##### (2) 중앙값(median)
> 전체 데이터를 크기 순으로 정렬하여 순서상 중앙에 위치한 값

##### (3) 최빈값(mode)
> 전체 데이터에서 가장 빈번하게 관찰된 값

<br>

### 2) 산포도(Degree of Dispersion)
> 집단 내 데이터가 흩어져 있는 정도

- 중심화 경향치를 기준으로 얼마나 떨어져 있는가?(이론적으로 중심화 경향치가 먼저 계산되어야 함)
- 중심화 경향치가 없다면 범위(Range)로 산포 확인 가능

#### 산포도 종류

##### (1) 범위(Range)
> 연속형 값의 최솟값과 최댓값 사이의 거리

- 범위는 Outlier를 포함하고 있음
- $ Range = 최댓값 - 최솟값$

##### (2) 사분위 범위(InterQuartile Range, IQR)
> 0%, 25%, 50%, 75%, 100% 구간으로 나눈 사분위수에서<br> 25%와 75% 사이 값들(Boxplot)

![](/assets/images/posts/statistics/boxplot.png)
https://towardsdatascience.com/understanding-boxplots-5e2df7bcbd51

##### (3) 분산(Variance)
> 관측치들이 평균(중심 경향치)에서 평균적으로 얼마나 떨어져 있는지 확인하기 위한 값<br>
> 편차값(관측치 - 평균)의 제곱의 합을 데이터의 수로 나눈 값

- 편차값으로는 흩어진 정도를 조사할 수 없으므로, 편차값 제곱의 평균으로 계산
- 제곱하였기 때문에 값이 너무 커지는 단점이 있다.

###### ddof(degree of freedom)
> pandas와 numpy에서 분산을 계산할 때 모분산이냐 표본 분산이냐에 따라 ddof값을 다르게 설정해줘야한다.<br>
> 표준편차도 마찬가지.

- 모분산
  - 편차값 제곱의 합을 n으로 나눔
  - ddof = 0(numpy default)

- 표본분산(불편 분산)
  - 편차값 제곱의 합을 n-1으로 나눔
  - ddof = 1(pandas default)

##### (4) 표준편차(Standard deviation)
> 분산의 양의 제곱근
