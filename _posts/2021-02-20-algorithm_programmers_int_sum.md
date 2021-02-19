---
title: "[Programmers] 두 정수 사이의 합"
category: Algorithm Test
---

## 문제
두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 모든 정수의 합을 리턴하는 함수, solution을 완성하세요. <br>
예를 들어 a = 3, b = 5인 경우, 3 + 4 + 5 = 12이므로 12를 리턴합니다.


## 제한사항
- a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.
- a와 b는 -10,000,000 이상 10,000,000 이하인 정수입니다.
- a와 b의 대소관계는 정해져있지 않습니다.

## 나의 풀이
```python
def solution(a, b):
    if a > b:
        int_sum = sum(range(b, a + 1))
    elif a < b:
        int_sum = sum(range(a, b + 1))
    return int_sum

# test case
a = 3
b = 5

print(solution(a, b))
# 12
```

## 다른 풀이
1.대소비교 후 a와 b를 서로 바꾸는 방법

```python
def adder(a, b):
    if a > b: a, b = b, a
    return sum(range(a,b+1))
```
<br>
2.수열의 합 공식을 사용한 방법(S = n(a + l) / 2)

```python
def adder(a, b):
    return (abs(a-b)+1)*(a+b)//2
```
## 총평
다른 풀이는 어렵지 않았지만, 문제를 푸는 당시에 생각나지 않으면 소용없다.
- 수학적으로 접근해보기
- 단순화하려고 노력해보기