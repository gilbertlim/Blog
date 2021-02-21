---
title: "[이것이 취업을 위한 코딩 테스트다] 코딩 테스트를 위한 파이썬 문법"
category: Algorithm Theory
---

이미 알고 있는 내용은 생략되었고, 모르고 있거나 기억하면 도움될 내용들만 정리함.

## 1. 리스트 컴프리헨션
1) 2차원 리스트(행렬) 초기화

```python
# N X M 크기의 2차원 리스트 초기화
n = 3
m = 4
array = [[0] * m for _ in range(n)]
print(array)
# [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
```

2) 특정 값을 포함하지 않는 리스트 초기화
```python
a = [1, 2, 3, 4, 5, 5, 5]
remove_set = [3, 5] # 리스트, 튜플, 집합 모두 다 가능
result = [i for i in a if i not in remove_set]
print(result)
# [1, 2, 4]
```

3) 2차원 리스트(행렬) 90도 회전
```python
def rotate_a_matrix_by_90_degree(a):
  row_length = len(a)
  column_length = len(a[0])
  
  res = [[0] * row_length for _ in range(column_length)]
  for r in range(row_length):
    for c in range(column_length):
      res[c][row_length - 1 - r] = a[r][c]
      
  return res

a = [[1, 2, 3, 4],
     [5, 6, 7, 8],
     [9, 10, 11, 12]]

print(rotate_a_matrix_by_90_degree(a))
# [[9, 5, 1],
#  [10, 6, 2],
#  [11, 7, 3],
#  [12, 8, 4]]
```

## 2. 리스트 관련 기타 메서드

|메서드명|설명|시간 복잡도|
|---|---|---|
|append()|원소 한개 삽입|O(1)|
|sort()|오름차순 정렬, 내림차순 정렬(reverse=True)|O(NlogN)|
|reverse()|원소 순서를 뒤집어 놓음|O(N)|
|insert(위치, 값)|특정 인덱스에 원소 삽입|O(N)|
|count()|특정 값의 개수 출력|O(N)|
|remove()|특정 원소 값 제거(여러개면 하나만)|O(N)|

## 3. sys.readline().rstrip()
> 입력 개수가 많을 때 input()을 사용하면 시간 초과가 발생하므로, sys.stdin.readline()을 사용

```python
import sys
data = sys.stdin.readline().rstrip()
print(data)
```

## 4. eval()
> 문자열 형식의 수식을 계산한 결과를 반환하는 함수

```python
result = eval("(3+5) * 7")
print(result)
# 56
```

## 5. itertools
> 파이썬에서 반복되는 데이터를 처리하는 기능을 포함하고 있는 라이브러리

1) permutations(순열) : r개의 데이터를 뽑아 일렬로 나열하는 모든 경우

```python
from itertools import permutations

data = ['A', 'B', 'C']
result = list(permutations(data, 3))
print(result)
# [('A', 'B', 'C'), ('A', 'C', 'B'), ('B', 'A', 'C'), ('B', 'C', 'A'), ('C', 'A', 'B'), ('C', 'B', 'A')]
```

2) combinations(조합) : r개의 데이터를 뽑아 순서를 고려하지 않고 나열하는 모든 경우
```python
from itertools import combinations

data = ['A', 'B', 'C']
result = list(combinations(data, 2))
print(result)
# [('A', 'B'), ('A', 'C'), ('B', 'C')]
```


## 6. heapq
> 우선순위 큐 기능을 구현하고자 할 때 사용되는 라이브러리

- 파이썬은 최소 힙(Min Heap)으로 구성되어 있으므로 단순이 원소를 힙에 넣었다가 빼는 것만으로도 오름차순으로 정렬됨<br> 
- 시간복잡도 : O(NlogN)<br>
- PriorityQueue 라이브러리보다 빠름

1) 오름차순 정렬
```python
import heapq
def heapsort(iterable):
    h = []
    result = []
    # 모든 원소를 힙에 삽입
    for value in iterable:
        heapq.heappush(h, value)
    # 힙에 삽입된 원소를 꺼내기
    for i in range(len(h)):
        result.append(heapq.heappop(h))
    return result

arr = [1, 3, 5, 7, 9, 2, 4, 6, 8, 0]
print(heapsort(arr))
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

2) 내림차순 정렬(원소 부호 변경)
```python
import heapq
def heapsort(iterable):
    h = []
    result = []
    for value in iterable:
        heapq.heappush(h, -value)
    for i in range(len(h)):
        result.append(-heapq.heappop(h))
    return result
arr = [1, 3, 5, 7, 9, 2, 4, 6, 8, 0]
print(heapsort(arr))
# [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```
 
## 7. bisect
> 이진 탐색을 위한 라이브러리

- '정렬된 배열'에서 특정 원소를 찾아야할 때 매우 효과적으로 사용됨
- 시간 복잡도 : O(logN)
- bisect_left : 정렬된 순서를 유지하면서, 리스트 a에 데이터 x를 삽입할 가장 왼쪽 인덱스 출력
- bisect_right : 정렬된 순서를 유지하면서, 리스트 a에 데이터 x를 삽입할 가장 오른쪽 인덱스 출력

1) 정렬된 리스트에 새로운 데이터를 삽입하려하는 경우 삽입될 위치 찾기
```python
from bisect import bisect_left, bisect_right

a = [1, 2, 4, 4, 8]
x = 4

print(bisect_left(a, x)) # 2
print(bisect_right(a, x)) # 4
```

2) 정렬된 리스트에서 값이 특정 범위에 속하는 원소의 개수 찾기
```python
from bisect import bisect_left, bisect_right

def count_by_range(a, left_value, right_value):
    right_index = bisect_right(a, right_value)
    left_idex = bisect_right(a, left_value)
    return right_index - left_idex

a = [1, 2, 3, 3, 3, 3, 4, 4, 8 ,9]

print(count_by_range(a, 4, 4)) # 2
print(count_by_range(a, -1, 3)) # 6
```

## 8. collections
> 유용한 자료구조를 제공하는 표준 라이브러리

- 주로 사용되는 클래스 : deque, Counter

1) deque : 스택 또는 큐의 기능을 모두 포함하고 있음
- 큐(Queue)
  - popleft() : 첫 번째 원소 제거
  - appendleft(x) : 첫 번째 인덱스에 원소 x 삽입
- 스택(Stack)
  - pop() : 마지막 원소 제거
  - append(x) : 마지막 인덱스에 원소 삽입

- 리스트와 비교

  ||리스트|deque|
  |---|---|---|
  |가장 앞쪽에 원소 추가|O(N)|O(1)|
  |가장 뒤쪽에 원소 추가|O(1)|O(1)|
  |가장 앞쪽에 원소 제거|O(N)|O(1)|
  |가장 뒤쪽에 원소 제거|O(1)|O(1)|

```python
from collections import deque

data = deque([2, 3, 4])
data.appendleft(1)
data.append(5)
print(list(data)) 
# [1, 2, 3, 4, 5]
```

2) Counter : 등장 횟수를 세는 기능을 제공하는 클래스
- iterable 객체가 주어졌을 때, 해당 객체 내부의 원소가 몇 번씩 등장 했는지를 알려줌

```python
from collections import Counter

counter = Counter(['red', 'blue', 'red', 'green', 'blue', 'blue'])

print((counter['blue'])) # 3
print((counter['green'])) # 1
print(dict(counter)) # {'red': 2, 'blue': 3, 'green': 1}
```

## 9. math
> 자주 사용되는 수학적인 기능을 포함하고 있는 라이브러리

1) factorial(x) : 팩토리얼 계
```python
import math
print(math.factorial(5)) # 5! = 120
```

2) sqrt(x) : 제곱근 계산
```python
import math
print(math.sqrt(7)) # 2.46XXX
```

3) gcd(a, b) : 최대 공약수 계산
```python
import math
print(math.gcd(21, 14)) # 7
```

4) pi, e
```python
import math
print(math.pi) # 3.14XXX
print(math.e) # 2.7XXX
```