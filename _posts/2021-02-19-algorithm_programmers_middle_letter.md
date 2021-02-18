---
title: "[Programmers] 가운데 글자 가져오기"
category: Algorithm Test
---

## 문제
단어 s의 가운데 글자를 반환하는 함수, solution을 만들어 보세요. 단어의 길이가 짝수라면 가운데 두글자를 반환하면 됩니다.


## 제한사항
s는 길이가 1 이상, 100이하인 스트링입니다.

## 나의 풀이
```python
def solution(s):
    word = s

    word_len = len(s)

    if word_len % 2 == 0:
        return word[int(word_len / 2) - 1 : int(word_len / 2) + 1]
    else:
        return word[int(word_len / 2)]
    
# test case
s = "abcde"

print(solution(s))
# c
```

## 총평
리스트 슬라이싱만 알면 푸는 쉬운 문제이다.