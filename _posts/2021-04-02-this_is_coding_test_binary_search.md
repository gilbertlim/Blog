---
title: "[이것이 취업을 위한 코딩 테스트다] 이진 탐색(Binary Search)"
category: Algorithm Theory
use_math: true
---

## 1. 이진 탐색(Binary Search)
- 탐색 범위를 반으로 좁혀가며 빠르게 탐색하는 알고리즘

### 1) 순차탐색(Sequential Search)

- 리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법
- 리스트에 데이터가 아무리 많아도 시간만 충분하다면 원하는 데이터를 찾을 수 있음
- 사용 환경
    - 정렬되지 않은 리스트에서 데이터를 찾아야할 때 사용함
- 시간 복잡도 : **O(N)**

```python
def sequential_search(n, target, array):
    for i in range(n):
        if array[i] == target:
            return i + 1

print("생성할 원소 개수를 입력한 다음 한 칸 띄고 찾을 문자열을 입력하세요.")
# 5 Dongbin

input_data = input().split()
n = int(input_data[0])
target = input_data[1]

print("앞서 적은 원소 개수만큼 문자열을 입력하세요. 구분은 띄어쓰기 한 칸으로 합니다.")
array = input().split()
# Hanul Jonggu Dongbin Taeil Sangwook

print(sequential_search(n, target, array))
# 3
```

### 2) 이진 탐색(Binary Search)

- 변수 3개(시작점, 끝점, 중간점)를 사용하여 찾으려는 데이터와 중간점 위치에 있는 데이터를 반복적으로 비교해서 원하는 데이터를 찾는 방법
- 데이터가 무작위가 아닐 때 이미 정렬되어 있다면 매우 빠르게 데이터를 찾을 수 있음
- 코딩 테스트에서 단골로 나오는 문제
- 다른 알고리즘에서도 폭 넓게 적용되는 원리와 유사하기 때문에 중요한 알고리즘
- 사용 환경
    - **배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘**
    - **탐색 범위가 1,000만을 넘어가면 접근해볼 것**(파이썬은 1초에 2,000만번의 연산을 수행할 수 있음)
        - 1,000만 단위 이상으로 넘어가면 O(logN)의 속도를 내야함
        - **input 보다는 sys.stdin.readline()을 써서 시간 초과를 면할 것**
    - 탐색 범위의 크기가 1,000억 이상이라면 이진탐색으로 풀어야하는 문제
- 시간 복잡도 : **O(logN)**, 단계마다 탐색해야하는 원소가 절반으로 줄어듬

![이진탐색(Binary Search)](/assets/images/posts/algorithm/this_is_coding_test/binary_search.png)

<br>

- 재귀함수로 구현

```python
def binary_search(array, target, start, end):
    if start > end:
        return None
    mid = (start + end) // 2 # (0 + 9) // 2 = 4

    if array[mid] == target:
        return mid
    elif array[mid] > target:
        return binary_search(array, target, start, mid - 1)
    else:
        return binary_search(array, target, mid + 1, end)

n, target = list(map(int, input().split()))
# 10 7

array = list(map(int, input().split()))
# 1 3 5 7 9 11 13 15 17 19

result = binary_search(array, target, 0, n - 1)
if result == None:
    print("원소가 존재하지 않습니다.")
else:
    print(result + 1)
# 4
```

- 단순 반복문으로 구현

```python
def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2

        if array[mid] == target:
            return mid
        elif array[mid] > target:
            end = mid - 1
        else:
            start = mid + 1
    return None

n, target = list(map(int, input().split()))

array = list(map(int, input().split()))

result = binary_search(array, target, 0, n - 1)
if result == None:
    print("원소가 존재하지 않습니다.")
else:
    print(result + 1)
```

## 2. 트리 자료구조(Tree)

- 노드와 노드의 연결로 표현
    - 노드 : 정보의 단위, 어떠한 정보를 가지고 있는 개체
- 그래프 자료구조의 일종으로 데이터베이스 시스템이나 파일 시스템과 같은 곳에서 많은 양의 데이터를 관리하기위한 목적으로 사용함
- 큰 데이터를 처리하는 소프트웨어는 대부분 데이터를 트리 자료구조로 저장해서 이진탐색과 같은 탐색 기법을 이용해 빠르게 탐색이 가능함
- 특징
    - 부모 노드와 자식 노드의 관계로 표현
    - 최상단 노드 : 루트 노드
    - 최하단 노드 : 단말 노드
    - 트리에서 일부를 떼어내도 트리 구조이며, 서브 트리로 불림
    - 파일 시스템과 같이 계층적이고 정렬된 데이터를 다루기에 적합함

<br>

- DB에서의 트리 자료구조
    - DB에는 내부적으로 대용량 데이터 처리에 적합한 '트리 자료 구조'를 이용하여 항상 데이터가 정렬되어 있음
    - DB는 이진 탐색과 유사한 방법을 이용해 탐색을 항상 빠르게 수행하도록 설계되어 있어 데이터가 많아도 탐색하는 속도가 빠름

### 이진 탐색 트리

- 이진 탐색이 동작할 수 있도록 고안된, 효율적인 탐색이 가능한 자료구조
- 트리 자료구조 중 가장 간단한 형태
- 이진 탐색 트리를 구현하는 방법은 출제 빈도가 낮음
- 특징

    - 부모 노드보다 왼쪽 자식 노드가 작다
    - 부모 노드보다 오른쪽 자식 노드가 크다
    - 17 < 30 < 48 (성립)

![이진 트리(Binary Tree)](/assets/images/posts/algorithm/this_is_coding_test/binary_tree.png)

## 3. 데이터 빠르게 입력 받기

- sys 라이브러리 활용
    - 데이터 개수가 많거나 탐색 범위가 클 경우 input() 함수를 사용하면 동작속도가 느려서 시간 초과 판정을 받을 수 있음

```python
import sys

# 하나의 문자열 데이터 입력 받기
input_data = sys.stdin.readline().rstrip() 
# rstrip() : readline() 사용 시 줄바꿈(\n)이 입력되어 제거하기 위한 용도

print(intput_data) # 입력받은 문자열 그대로 출력
```

## 3. 실전 문제

> 부품 찾기

동빈이네 전자 매장에는 부품이 N개 있다. 각 부품은 정수 형태의 고유한 번호가 있다.
어느날 손님이 M개 종류의 부품을 대량으로 구매하겠다며 당일 날 견적서를 요청했다.
동빈이는 때를 놓치지않고 손님이 문의한 부품 M개 종류를 모두 확인해서 견적서를 작성해야한다.
이때 가게 안에 부품이 모두 있는지 확인하는 프로그램을 작성해보자.

예를 들어 가게의 부품이 총 5개 일 때 부품 번호가 다음과 같다고 하자.

N = 5

[8, 3, 7, 9, 2]

손님은 총 3개의 부품이 있는지 확인 요청했는데 부품 번호는 다음과 같다.

M = 3

[5, 7, 9]

이때 손님이 요청한 부품 번호의 순서대로 부품을 확인해 부품이 있으면 yes를, 없으면 no를 출력한다.
구분은 공백으로 한다.

- 시간 제한 : 1초
- 메모리 제한 : 128MB
- 입력 조건
  - 첫째 줄에 정수 N이 주어진다. ($1 \le N \le 1,000,000$)
  - 둘째 줄에는 공백으로 구분하여 N개의 정수가 주어진다. 이때 정수는 1보다 크고 1,000,000 이하이다.
  - 셋째 줄에는 정수 M이 주어진다. ($1 \le M \le 100,000$)
  - 넷째 줄에는 공백으로 구분하여 M개의 정수가 주어진다. 이때 정수는 1보다 크고 1,000,000 이하이다.

-> 이진 탐색을 포함한 여러 가지 방법으로 풀 수 있음

- 이진 탐색
  - $O((M+N) \times logN$

```python
def binary_search(arr, target, start, end):
    if start > end:
        return None

    mid = (start + end) // 2

    if arr[mid] == target:
        return mid
    elif arr[mid] > target:
        return binary_search(arr, target, start, mid - 1)
    else:
        return binary_search(arr, target, mid + 1, end)

n = int(input())
# 5
arr = list(map(int, input().split()))
# 8 3 7 9 2

arr.sort() # NlogN

m = int(input())
# 3
x = list(map(int, input().split()))
# 5 7 9

for i in x: # M X log N
    result = binary_search(arr, i, 0, n - 1)
    if result == None:
        print("no", end=" ")
    else:
        print("yes", end=" ")
# no yes yes
```

- 계수 정렬
  - $O(N+K)$ K: 최댓값

```python
n = int(input())
array = [0] * 1000001

for i in input().split():
    array[int(i)] = 1

m = int(input())

x = list(map(int, input().split()))

for i in x:
    if array[i] == 1:
        print("yes", end=" ")
    else:
        print("no", end=" ")
```

- 집합
  - $O(M)$

```python
n = int(input())
array = set(map(int, input().split()))

m = int(input())
x = list(map(int, input().split()))

for i in x: # O(M)
    if i in array: # 집합의 경우에만 O(1)
        print("yes", end=" ")
    else:
        print("no", end=" ")
```

> 떡볶이 떡 만들기

오늘 동빈이는 여행 가신 부모님을 대신해서 떡집 일을 하기로 했다. 
오늘은 떡볶이 떡을 만드는 날이다. 
동빈이네 떡볶이 떡은 재밌게도 떡볶이 떡의 길이가 일정하지 않다. 
대신에 한 봉지 안에 들어가는 떡의 총 길이는 절단기로 잘라서 맞춰준다.
절단기의 높이(H)를 지정하면 줄지어진 떡을 한 번에 절단한다. 
높이가 H보다 긴 떡은 H 위의 부분이 잘릴 것이고, 낮은 떡은 잘리지 않는다.
예를 들어 높이가 19, 14, 10, 17cm인 떡이 나란히 있고 절단기 높이를 15cm로 지정하면 자른 뒤 떡의 높이는 15, 14, 10, 15cm가 될 것이다. 
잘린 떡의 길이는 차례대로 4, 0, 0, 2cm이다. 손님은 6cm만큼의 길이를 가져간다.
손님이 왔을 때 요청한 총 길이가 M일 때 적어도 M만큼의 떡을 얻기 위해 절단기에 설정할 수 있는 높이의 최댓값을 구하는 프로그램을 작성하시오.

- 시간 제한 : 2초
- 메모리 제한 : 128MB
- 입력 조건
  - 첫째 줄에 떡의 개수 N과 요청한 떡의 길이 M이 주어진다. ($1 \le N \le 1,000,000, 1 \le M \le 2,000,000,000$)
  - 둘째 줄에는 떡의 개별 높이가 주어진다. 떡 높이의 총합은 항상 M 이상이므로, 손님은 필요한 양만큼 떡을 사갈 수 있다. 높이는 10억보다 작거나 같은 양의 정수 또는 0이다.

```python
n, m = list(map(int, input().split()))
# 4 6

array = list(map(int, input().split()))
# 19 15 10 17

start = 0
end = max(array)

result = 0
while start <= end:
    total = 0
    mid = (start + end ) // 2

    for x in array:
        if x > mid:
            total += x - mid

    if total < m:
        end = mid - 1
    else:
        result = mid
        start = mid + 1

print(result)
# 15
```

-> 변수의 범위가 크므로 이진탐색으로 푸는 문제

- 문제 해설
  - 이진탐색이자 파라메트릭 서치(Parametric Search) 유형의 문제임 
    - 파라매트릭 서치
      - 최적화 문제를 '예' or '아니오' **결정 문제로 바꾸어 해결하는 기법**
      - 원하는 조건을 만족하는 가장 알맞은 값을 찾는 문제에서 사용
      - 이진탐색을 반복문을 이용하여 구현(재귀적인 방법 사용 X)
  - 예) 범위 내에서 조건을 만족하는 가장 큰 값을 찾으라는 최적화 문제라면 이진탐색으로 결정 문제를 해결 가능
    - 본 문제는 절단기의 높이(탐색 범위)가 10억 이내 정수 이므로, '이진탐색'을 떠올려야 한다.
    - N은 최대 100만개이므로 3000만번정도의 연산으로 풀 수 있음
    - 제한시간이 2초이므로 아슬아슬하게 시간초과를 받지 않고 정답 가능
    - 시작 점 : 0
    - 끝점 : 가장 긴 떡의 길이
    - 중간점 : 절단기의 높이
    - 잘린 떡의 길이의 합 : Target