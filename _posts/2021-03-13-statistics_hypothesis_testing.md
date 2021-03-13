---
title: "[Practical Statistics] Hypothesis Testing(가설 검정)"
category: Practical Statistics
use_math: true
---

# Hypothesis Testing(가설 검정)
> 모집단 특징에 대한 주장인 가설에 대해 **표본으로부터 얻은 정보를 바탕으로**<br> 
> **어떤 특징을 채택할지 기각할지 판단**함으로써 모집단의 현재 특징에 대해 결정하는 과정

- 가설 검정 결과 우연에 의해 발생할 확률(p-value)이 기준(유의 수준)보다 낮은경우 기존의 주장을 기각함

## 1. 가설의 종류
### 1) 귀무가설(Null Hypothesis, 영가설)
> 일반적 기대 영역 내의 값

### 2) 대립가설(Alternative Hypothesis, 연구가설)
> **새롭게 확인하고자하는 가설**<br>
> 일반적 기대 영역 바깥에 놓인 값

## 2. 가설검정 단계
### 1) 가설 수립
- 귀무가설 및 대립가설 수립
- 귀무가설이 참이라는 가정하에 검정을 진행

### 2) 표본으로부터 검정 통계량 계산
- 모수의 특성이 이루는 표본분포로부터 표본을 통해 관찰된 통계량 계산

### 3) 가설 선택 기준 수립
- 유의수준 및 기각역 수립

#### 제1종 오류 & 제2종 오류
> 두 개의 가설 중 하나를 선택할 때 발생할 수 있는 오류

<table>
    <tr>
        <td rowspan="2">귀무가설 진위</td>
        <td colspan="2" align="center">의사결정</td>
    </tr>
    <tr>
        <td>귀무가설 채택</td>
        <td>귀무가설 기각</td>
    </tr>
    <tr>
        <td>귀무가설 사실</td>
        <td align="center">O<br> ($1-\alpha$)</td>
        <td align="center">제1종 오류<br>(유의수준 : $\alpha$)</td>
    </tr>
    <tr>
        <td>귀무가설 거짓</td>
        <td align="center">제2종 오류<br>($\beta$)</td>
        <td align="center">O<br> ($1-\beta$)</td>
    </tr>
</table>

#### 유의수준(Significance Level)
> 귀무가설의 기각여부를 결정하는데 사용하는 기준이 되는 확률

-  제 1종 오류를 범할 확률의 최대 허용 한계
- 유의수준이 $\alpha$인 검정 : 제 1종 오류를 범할 확률이 $\alpha$이하인 검정

#### 기각역(Rejection Region, Critical Region)
> 귀무가설을 기각하게 하는 구간

- 기각역의 면적의 합 =  유의수준 $\alpha$
- 기각역에 따른 검정의 종류
    - 양측 검정(Two-Tailed/Sided Test) : 양 쪽에 기각역을 두고 검정
    - 단측 검정(One-Tailed/Sided Test) : 한 쪽에 기각역을 두고 검정 
  
### 4) 판정 및 해석
> 검정 통계량과 유의확률로 귀무가설의 채택 여부 판정

#### 판정 방법
##### 1) 검정 통계량 & 기각역
- 검정 통계량이 기각역에 있으면 귀무가설 기각
- 검정 통계량이 기각역에 있지 않으면 귀무가설 채택

##### 2) 유의확률(p-value) & 유의수준
- 유의확률 < 유의수준 : 귀무가설 기각
- 유의확률 > 유의수준 : 귀무가설 채택

## 실습
- <a href="https://colab.research.google.com/drive/1uyaCSp8VWXCszCGsB9SK_aBSTWwD8ouY?usp=sharing">가설 검정</a>