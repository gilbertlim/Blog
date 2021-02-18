---
title: "[Programmers] 체육복"
category: Algorithm Test
---

## 문제
점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다. 다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다. 학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다. 예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다. 체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.

전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 전체 학생의 수는 2명 이상 30명 이하입니다.
- 체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.
- 여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.

## 나의 풀이
```python
def solution(n, lost, reserve):
    dic = {}
    for i in range(1, n+1):
        dic[i] = 1
    for r in reserve:
        dic[r] = 2
    for l in lost:
        if dic[l] == 2:
            dic[l] = 1
        else:
            dic[l] = 0
    for idx, i in enumerate(dic.keys()):
        for j in list(dic.keys())[idx+1:]:
            if abs(i - j) == 1:
                if abs(dic[i] - dic[j]) == 2:
                    dic[i] = 1
                    dic[j] = 1
    for i in dic:
        if dic[i] == 2:
            dic[i] = 1
    return sum(dic.values())

# test case
n = 5
lost = [1, 2]
reserve = [1, 3]

print(solution(n, lost, reserve))
# 5
```

## 다른 풀이
```python
def solution(n, lost, reserve):
    # reserve이면서 lost인 사람(1개 있음) 고려하기 위해 _lost와 _reserve에 각각 중복을 피해 재정의(차집합)
    _reserve = list(set(reserve) - set(lost)) # _reserve = [r for r in reserve if r not in lost] 보다 빠름
    _lost = list(set(lost) - set(reserve))

    for r in _reserve:
        # 양 옆을 확인 하기 위한 계산
        f = r - 1
        b = r + 1

        # 양 옆에 있으면 체육복을 나눠줄 수 있으므로, _lost에서 삭제
        # 리턴 시 전체 사람 수에서 체욱복이 없는 사람의 수를 빼므로 _lost에 값이 없을 수록 체육복을 입은 사람의 수 증가
        if f in _lost:
            _lost.remove(f)
        elif b in _lost:
            _lost.remove(b)

    return n - len(_lost)
```

## 총평
체육복이 2개 있으면서 도난당한 경우를 고려해야하는 것이 핵심인 문제다. 나는 딕셔너리를 활용해서 처리했다.<br>
나는 미리 학생들이 가진 체육복의 수를 계산하고, 양 옆에 체육복의 차가 2일 경우 서로 1개씩 나누도록 계산했다.
다른 풀이를 보면 나의 코드보다 간결한 것을 알 수 있다. 참고하자.