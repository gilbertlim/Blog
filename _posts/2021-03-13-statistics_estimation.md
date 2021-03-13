---
title: "[Practical Statistics] Estimation(추정)"
category: Practical Statistics
use_math: true
---

# Estimation(추정)
> 모집단에서 추출된 표본의 특징을 파악하여 모수($\mu,\ \sigma^2$)을 유추하는 것

## 1. 추정의 종류

### 점추정(Point Estimation)
> 모수의 참값이 포함될 것으로 기대하는 하나의 값을 추정하는 것

- 추출되는 표본에 따라 오류가 발생할 가능성이 큼

### 구간추정(Interval Estimation)
> 모수의 참값이 포함될 것으로 기대하는 구간을 추정하는 것 

- 하나의 값으로 추정하는 것 보다 구간으로 추정하는 것이 현실적

<br>

## 2. 신뢰구간(Interval Estimation)
> 모수의 참값이 포함될 것으로 기대하는 구간

### 95% 신뢰구간
> - 모집단에서 n개의 표본추출을 여러번 실시하여 신뢰구간 추정 시<br>
> 그 결과들 중 약 95% 정도가 모평균을 포함할 수 있음을 의미함<br>
> - 100번 중 95번은 모평균이 추정된 구간에 포함될 수 있음

#### 1) 모분산을 알고 있는 경우, 모평균 구간추정
$-Z_{\alpha/2} \le \cfrac{\bar{X}-\mu}{\sqrt{\cfrac{\sigma^2}{n}}} \le Z_{\alpha/2}$

#### 2) 모분산을 모르는 경우, 모평균 구간 추정
$-t_{\alpha/2} \le \cfrac{\bar{X}-\mu}{\sqrt{\cfrac{S^2}{n-1}}} \le t_{\alpha/2}$

#### 3) 모평균을 알고 있는 경우, 모분산 구간 추정

$\cfrac{\sum\limits_{i=1}^n(x_i - \mu)^2}{\chi_{(\alpha/2)}^2 (n)} \le \sigma^2 \le \cfrac{\sum\limits_{i=1}^n(x_i - \mu)^2}{\chi_{1-(\alpha/2)}^2 (n)}$

#### 4) 모평균을 모르는 경우, 모분산 구간 추정

$\cfrac{(n-1)S^2}{\chi_{(\alpha/2)}^2 (n-1)} \le \sigma^2 \le \cfrac{(n-1)S^2}{\chi_{1-(\alpha/2)}^2 (n-1)}$


## 3. 신뢰수준(Confidence Level)
> 모수의 참값이 표본을 통해 구해지는 신뢰구간에 **존재할* 확률

$ 1 - \alpha = 1 - 0.05 = 0.95$

- 신뢰수준에 따라 신뢰구간이 달라진다.
- **정밀도와 신뢰수준 사이의 적절한 균형 유지를 위해 일반적으로 신뢰수준 95%를 사용한다.**

## 4. 유의수준(Significance Level)
> 모수의 참값이 표본을 통해 구해지는 신뢰구간에 **존재하지 않을** 확률

$\alpha = 0.05$