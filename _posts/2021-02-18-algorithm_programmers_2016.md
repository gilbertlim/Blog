---
title: "[Programmers] 2016"
category: Algorithm Test
---

## 문제
2016년 1월 1일은 금요일입니다. 2016년 a월 b일은 무슨 요일일까요? 두 수 a ,b를 입력받아 2016년 a월 b일이 무슨 요일인지 리턴하는 함수, solution을 완성하세요. 요일의 이름은 일요일부터 토요일까지 각각 `SUN,MON,TUE,WED,THU,FRI,SAT`
입니다. 예를 들어 a=5, b=24라면 5월 24일은 화요일이므로 문자열 TUE를 반환하세요.


## 제한사항
- 2016년은 윤년입니다(2월이 29일까지 있음).
- 2016년 a월 b일은 실제로 있는 날입니다. (13월 26일이나 2월 45일같은 날짜는 주어지지 않습니다)

## 나의 풀이
```python
def solution(a, b):
    week = ["SAT", "SUN", "MON", "TUE", "WED", "THU", "FRI"]
    days = [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    dsum = 0
    for d in days[:a - 1]:
        dsum += d
    dsum = (dsum + b - 1) % 7
    answer = week[dsum - 1]
    return answer
    
# test case
a = 5
b = 24

print(solution(a,b))
# TUE
```

## 다른 풀이
```python
def getDayName(a,b):
    months = [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    days = ['FRI', 'SAT', 'SUN', 'MON', 'TUE', 'WED', 'THU']
    return days[(sum(months[:a-1])+b-1)%7]
```

## 총평
다른 풀이도 나와 동일한 코드지만, pythonic하게 한 줄로 풀었다.