---
title: "[Programmers] 완주하지 못한 선수"
category: Algorithm Test
---

## 문제
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

## 다른 풀이
1.zip 함수를 사용한 방법 

```python
def solution(participant, completion):
    participant.sort()
    completion.sort()

    for p, c in zip(participant, completion): # 인덱스가 같은 것끼리 튜플로 묶음
        if p != c:
            return p # 묶여진 값들이 다를 경우 참여자 이름 리턴

    return participant.pop()
    # 묶여진 값들이 없으면, 마지막 참여자 이름 리턴(항상 p는 c보다 원소가 1개가 많음)
```
<br>
2.collections 모듈의 Counter 함수를 사용한 방법<br>
`Counter()` : 해당 객체의 값을 '{값:개수}'로 출력, 함수 간 +/- 연산이 가능함

```python
from collections import Counter
def solution2(participant, complection):
    answer = Counter(participant) - Counter(complection)
    return list(answer)[0]
```
<br>
3.해시함수를 사용한 방법
- 해시함수 입력값이 같으면 해시값이 같음을 이용함
- 참여자 전원의 해시값의 합에서 완주자 전원의 해시값의 합을 빼면, 완주하지 못한 사람의 해시값이 나옴

```python
def solution3(participant, completion):
    temp = 0
    dic = {}
    for part in participant:
        dic[hash(part)] = part # 딕셔너리에 '해시값:참여자이름'으로 저장
        temp += int(hash(part)) # 참여자마다 해시값을 temp에 더함
        print(dic)
    for com in completion:
        temp -= hash(com) # 참여자마다 해시값을 temp에서 뺌
    answer = dic[temp] # 최종적으로 temp에 저장된 변수 = 완주하지 못한사람의 해시값

    return answer
```

## 총평
간단한 문제로 판단했으나, 시간이 너무 오래걸려 풀지 못했다.<br>
다른 풀이를 리뷰하면서 학습했고, 해시 함수를 사용한 풀이가 출제 의도와 가장 근접한 풀이인 것 같다.
