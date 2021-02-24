---
title: "[이것이 취업을 위한 코딩 테스트다] 그리디(Greedy)"
category: Algorithm Theory
---

## 그리디(Greedy)
- **현재 상황에서 지금 당장 좋은 것만 고르는 방법(단순 무식하게 탐욕적으로 풀이)**
- 현재 선택이 나중에 미칠 영향에 대해서는 고려하지 않음
- 단순하지만 강력한 문제 해결 방법

### 특징
- 단순히 현재 상황에서 가장 좋아보이는 것만 선택해도 문제를 풀 수 있는지 파악할 수 있어야 함
- **최소한의 아이디어를 떠올리고 이것이 정당한지 검토할 수 있어야 함**
- 기준에 따라 좋은 것을 선택하는 알고리즘이므로 기준이 알게 모르게 제시됨(예 : 가장 큰 순서대로)
- 코딩 테스트 문제를 만났을 때 바로 문제 유형을 파악하기 어렵다면 그리디 알고리즘을 의심하고,
  이후에도 해결 방법이 없다면 다이나믹 프로그래밍, 그래프 알고리즘으로 고민 필요
- 정렬 알고리즘과 짝을 이뤄 자주 출제됨

## 1. 연습문제
> 거스름돈

손님에게 거슬러 줘야할 돈이 N원일 때 거슬러 줘야 할 동전의 최소 개수를 구하라(N은 항상 10의 배수).

-> 금액이 큰 동전부터 사용하는 것이 핵심

```python
def solution(n):
    coins = [500, 100, 50, 10]
    cnt = 0
    
    for c in coins:
        cnt += n // c
        n %= c
    return cnt

# test case
n = 1260

print(solution(n))
# 6
```

## 2. 실전문제
> 큰 수의 법칙

N개의 주어진 수들을 M번 더하여 가장 큰수를 만드는 법칙. 단, 배열의 특정한 인덱스에 해당하는 수가 연속해서 K번을 초과하여 더해질 수 없다.

배열의 크기 N, 숫자가 더해지는 횟수 M, 그리고 K가 주어질 때 따른 큰 수의 법칙에 따른 결과를 출력하시오.

- 시간 제한 : 1초 
- 메모리 제한 : 128MB
- 입력 조건
  - N(2 <= N <= 1,000)
  - M(1 <= M <= 10,000)
  - K(1 <= K <= 10,000)
  
-> 가장 큰 수를 M 번씩 더하고, 두 번째 큰 수를 중간에 1번씩 더 하는 것이 핵심

1) 단순하게 푸는 방법

```python
def solution():
    n, m, k = map(int, input().split())
    data = list(map(int, input().split()))

    data.sort()
    first = data[n - 1]
    second = data[n - 2]

    result = 0

    while True:
        for i in range(k):
            if m == 0: 
                break
            result += first
            m -= 1 # 더하는 횟수 차감
        if m == 0:
            break
        result += second
        m -= 1

    return result

# test case
# 5 8 3
# 2 4 5 4 6
 
print(solution())
# 46
```

2) 수열을 사용한 방법

```python
def solution():
    n, m, k = map(int, input().split())
    data = list(map(int, input().split()))

    data.sort()
    first = data[n - 1]
    second = data[n - 2]

    count = (m // (k + 1)) * k
    count += m % (k + 1)

    result = 0
    result += count * first
    result += (m - count) * second

    return result

# test case
# 5 8 3
# 2 4 5 4 6
 
print(solution())
# 46
```
<br>

> 숫자 카드 게임

숫자 카드 게임은 여러 개의 숫자 카드 중에서 가장 높은 숫자가 쓰인 카드 한 장을 뽑는 게임이다.
단, 게임의 룰을 지키며 카드를 뽑아야 하고 룰은 다음과 같다.
  1. 숫자가 쓰인 카드들이 N X M 형태(행 X 열)로 놓여 있다.
  2. 먼저 뽑고자 하는 카드가 포함되어 있는 행을 선택한다.
  3. 그 다음 선택된 행에 포함된 카드들 중 가장 낮은 카드를 뽑아야 한다.
  4. 따라서 처음에 카드를 골라낼 행을 선택할 때, 이후에 해당 행에서 가장 숫자가 낮은 카드를 뽑을 것을 고려하여 최종적으로 가장 높은 숫자의 카드를 뽑을 수 있도록 전략을 세워야 한다.

카드들이 N X M 형태로 놓여 있을 때, 게임의 룰에 맞게 카드를 뽑는 프로그램을 만드시오.

- 시간 제한 : 1초
- 메모리 제한 : 128MB
- 입력 조건
  - 1 <= N, M <= 100
  - 카드에 적힌 숫자는 1 이상 10,000 이하의 자연수
  
-> 행마다 가장 작은 값을 찾고, 가장 작은 값들 중 최대 값을 찾는 것이 핵심 

1) min() 함수를 사용한 방법

```python
def solution():
    n, m = map(int, input().split())
    result = 0

    for i in range(n):
        data = list(map(int, input().split()))
        min_value = min(data)
        result = max(result, min_value)
    
    return result

# test case
# 3 3
# 3 1 2
# 4 1 4
# 2 2 2
    
print(solution())
# 2
```

2) 2중 반복문 구조를 사용한 방법

```python
def solution():
    n, m = map(int, input().split())
    result = 0

    for i in range(n):
        data = list(map(int, input().split()))
        min_value = 10001
        for a in data:
            min_value = min(min_value, a)
        result = max(result, min_value)

    return result
    
# test case
# 3 3
# 3 1 2
# 4 1 4
# 2 2 2
    
print(solution())
# 2
```
<br>

> 1이 될 때까지

어떠한 수 N이 1이 될 때까지 다음의 두 과정 중 하나를 반복적으로 선택하여 수행하려고 한다.
단, 두 번째 연산은 N이 K로 나누어떨어질 때만 선택할 수 있다.
1. N에서 1을 뺀다.
2. N을 K로 나눈다.

N과 K가 주어질 때 N이 1이 될 때까지 1번 혹은 2번의 과정을 수행해야하는 최소 횟수를 구하는 프로그램을 작성하시오.

- 시간 제한 : 1초
- 메모리 제한 : 128MB
- 입력 조건
  - N(2 <= N <= 100,000)
  - K(2 <= K <= 100,000)

-> 많이 나누는 것이 빨리 1을 만드는 방법이므로, 나누어 떨어질 때까지 1씩 뺀 다음 K로 나누는 것이 핵심

1) 단순하게 푸는 방법

```python
def solution():
    n, k = map(int, input().split())
    result = 0
    
    while n >= k:
        while n % k != 0:
            n -= 1
            result += 1  
        
        n //= k
        result += 1
    
    while n > 1:
        n -= 1
        result += 1
    
    return result
    
# test case
# 25 5

print(solution())
# 2
```

2) 효율적인 방법(N이 K의 배수가 되도록 한 번에 빼는 방식)

```python
def solution():
    n, k = map(int, input().split())
    result = 0

    while True:
        target = (n // k) * k # 나누어 떨어지는 수 
        result += (n - target) # 1을 뺀 횟수
        n = target
      
        if n < k:
            break
        
        result += 1 # 나눈 횟수
        n //= k
    
    result += (n - 1) # 마지막 남은 수에서 1을 뺀 횟수
    return result

# test case
# 25 5

print(solution())
# 2
```
