---
title: "[Programmers] 내적"
category: Algorithm Test
---

## 문제
길이가 같은 두 1차원 정수 배열 a, b가 매개변수로 주어집니다. a와 b의 내적을 return 하도록 solution 함수를 완성해주세요.
이때, a와 b의 내적은 `a[0]*b[0] + a[1]*b[1] + ... + a[n-1]*b[n-1]` 입니다. (n은 a, b의 길이)

## 제한사항
- a, b의 길이는 1 이상 1,000 이하입니다.
- a, b의 모든 수는 -1,000 이상 1,000 이하입니다.

## 나의 풀이
```python
def solution(a, b):
    sum = 0
    for i in range(len(a)):
        sum += a[i] * b[i]        
    return sum
     
# test case
a = [1, 2, 3, 4]
b = [-3, -1, 0, 2]

print(solution(a, b))
# 3
```

## 다른 풀이
zip() 함수를 사용한 방법

```python
def solution(a, b):
    return sum([x * y for x, y in zip(a, b)])
```

## 총평
zip() 함수가 있다는 것을 기억하자!