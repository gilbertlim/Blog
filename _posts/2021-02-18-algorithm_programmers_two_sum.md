---
title: "[Programmers] 두 개 뽑아서 더하기"
category: Algorithm Test
---

## 문제
정수 배열 numbers가 주어집니다. numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.

## 제한사항
numbers의 길이는 2 이상 100 이하입니다.
numbers의 모든 수는 0 이상 100 이하입니다.

## 나의 풀이
```python
def solution(numbers):
    answer = {}
    for idx, n in enumerate(numbers):
        for i in numbers[idx + 1:]:
            answer[n + i] = 1
    return sorted(answer.keys())

# test case
numbers = [2, 1, 3, 4, 1]

print(solution(numbers))
# [2, 3, 4, 5, 6, 7]
```

## 다른 풀이
```python
def solution(numbers):
    answer = []
    for i in range(len(numbers)):
        for j in range(i+1, len(numbers)):
            answer.append(numbers[i] + numbers[j])
    return sorted(list(set(answer)))
```

## 총평
중복을 제거하는 방법이 핵심인 것 같다.
딕셔너리로 풀어봤는데, 리스트로 풀고 집합으로 처리하는 방법도 있었다. 