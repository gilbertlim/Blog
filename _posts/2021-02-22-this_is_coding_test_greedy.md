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

- 시간 제한 : 1초 
- 메모리 제한 : 128MB
- 입력 조건
  - N(2 <= N <= 1,000)
  - M(1 <= M <= 10,000)
  - K(1 <= K <= 10,000)

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
```

> 숫자 카드 게임
