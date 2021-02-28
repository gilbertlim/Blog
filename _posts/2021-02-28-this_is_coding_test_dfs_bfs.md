---
title: "[이것이 취업을 위한 코딩 테스트다] DFS/BFS"
category: Algorithm Theory
use_math: true
---

## 1. 자료구조 기초

### 탐색(Search)
- 많은 양의 데이터 중에서 원하는 데이터를 찾는 과정
- 그래프, 트리 등의 자료구조 안에서 탐색하는 문제를 자주 다룸
- 대표적 탐색 알고리즘 : DFS/BFS
    - DFS/BFS를 제대로 이해하려면 자료구조(스택과 큐)에 대해서 알아야 함

### 자료구조(Data Structure)
- 데이터를 표현하고 관리하고 처리하기 위한 구조
- 스택과 큐는 자료구조의 기초 개념으로 두 함수로 구성됨
    - 삽입(Push) : 데이터를 삽입한다
    - 삭제(Pop) : 데이터를 삭제한다
- 삽입/삭제 이외에도 오버플로와 언더플로를 고민해야 함
    - Overflow : 자료구조가 수용할 수 있는 데이터의 크기를 이미 가득 찬 상태에서 **삽입** 연산을 수행할 때 발생
    - Underflow : 자료구조에 데이터가 전혀 들어 있지 않은 상태에서 **삭제** 연산을 수행할 때 발생

### 스택(Stack) 자료구조

- 아래에서부터 위로 차곡차곡 쌓기
- 선입후출(FILO), 후입선출(LIFO)
- 사용법
    - **내장 라이브러리 활용**
    - push : append(), 가장 뒤쪽에 데이터 삽입
    - pop : pop(), 가장 뒤쪽에서 데이터 꺼냄
    - 데이터 역순 변환 : data[::-1]

```python
stack = []

stack.append(5)
stack.append(2)
stack.append(3)
stack.append(7)
stack.pop()
stack.append(1)
stack.append(4)
stack.pop()

print(stack) # 최 하단 원소부터 출력, [5, 2, 3, 1]
print(stack[::-1]) # 최 상단 원소부터 출력, [1, 3, 2, 5]
```

### 큐(Queue) 자료구조

- 새치기가 없는 대기줄
- 선입선출(FIFO)
- 사용법
    - **from collections import deque 라이브러리 활용**
      - 대부분의 코딩 테스트에서 collections 모듈과 같은 기본 라이브러리 사용을 허용함
    - push : append()
    - pop : popleft(), 가장 앞쪽에서 데이터 꺼냄
    - 데이터 역순 변환 : data.reverse()

```python
# queue 구현을 위한 deque 라이브러리 사용
from collections import deque

queue = deque()

queue.append(5)
queue.append(2)
queue.append(3)
queue.append(7)
queue.popleft()
queue.append(1)
queue.append(4)
queue.popleft()

print(queue) # deque([3, 7, 1, 4])
queue.reverse() # 나중에 들어온 원소부터 먼저 들어온 원소 순서바꾸기
print(queue) # deque([4, 1, 7, 3])

print(list(queue)) # 리스트 자료형으로 출력
# [4, 1, 7, 3]

########################################################

from collections import deque

queue = deque([1])

print(list(queue)) # [1]

queue.popleft()
print(queue) # deque([])
```

### 재귀 함수(Recursive function)

- 자기 자신을 다시 호출하는 함수(재귀 : 다시 돌아옴)
- RecursionError: maximum recursion depth exceeded while calling a Python object
    - 재귀의 최대 깊이를 초과하는 상황 발생(파이썬 인터프리터의 호출 횟수 제한)
    - **종료 조건을 꼭 명시해야 함(무한 호출 방지)**
- 가장 마지막에 호출한 함수가 수행을 끝내야 그 앞의 함수 호출이 종료된다
    - 함수 내부적으로 스택 자료구조와 동일함
- **스택 자료구조**를 활용해야하는 상당수 알고리즘은 재귀함수를 이용하여 간편하게 구현 가능(DFS)
    - Factorial( $1 \times 2 \times 3 \times \dots \times (n-1) \times n$ ) : n이 1이하가 되었을 때 함수를 종료하는 재귀함수 형태로 구현 가능
    - 재귀 함수를 사용하여 구현하면 점화식(재귀식)을 그대로 소스코드로 옮겼기 때문에 **코드가 간결함**
        - 점화식 : 특정한 함수를 자신보다 더 작은 변수에 대한 함수와의 관계로 표현한 것
            - factorial(n) = 1
            - factorial(n) = n X factorial(n-1)

```python
# 무한 재귀 함수 호출
def recursive_function():
    print("재귀 함수를 호출합니다.")
    recursive_function()

recursive_function()
# RecursionError

######################################################################

def recursive_function(i):
    if i == 100: # 종료 조건 : 100번 째 출력했을 때 종료
        return
    
    print(i, "번째 재귀 함수에서", i + 1, "번째 재귀 함수를 호출합니다.")
    recursive_function(i + 1)
    print(i, "번째 재귀 함수를 종료합니다.")

recursive_function(1)

# 1 번째 재귀 함수에서 2 번째 재귀 함수를 호출합니다.
# 2 번째 재귀 함수에서 3 번째 재귀 함수를 호출합니다.
# 3 번째 재귀 함수에서 4 번째 재귀 함수를 호출합니다.
# ...
# 99 번째 재귀 함수에서 100 번째 재귀 함수를 호출합니다.
# 99 번째 재귀 함수를 종료합니다.
# ...
# 3 번째 재귀 함수를 종료합니다.
# 2 번째 재귀 함수를 종료합니다.
# 1 번째 재귀 함수를 종료합니다.
```

## 2. 탐색 알고리즘 DFS / BFS
DFS/BFS를 배우기 앞서 그래프(Graph)의 기본 구조를 알아야 함

### 그래프(Graph) 자료구조
- 노드(Node)와 간선(Edge)로 표현됨
    - 노드 = 정점(Vertex)
- 그래프 탐색
    - 하나의 노드를 시작으로 다수의 노드를 방문하는 것
    - 두 노드가 간선으로 연결되어 있다면 두 노드는 인접하다(Adjacent)고 표현 함
- 표현 방법
    - 인접 행렬(Adjacency Matrix) : 2차원 배열로 그래프의 연결 관계를 표현하는 방식
        - 2차원 배열에 각 노드가 연결된 형태를 기록하는 방식
        - 파이썬에서 2차원 리스트로 구현(배열을 리스트자료형으로 구현가능하기 때문)
        - 연결되어있지 않는 노드는 무한(987654321)으로 초기화

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cc4daa8d-aa17-47ca-8c25-c75bc5647901/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cc4daa8d-aa17-47ca-8c25-c75bc5647901/Untitled.png)

    - 인접 리스트(Adjacency List) : 리스트로 그래프의 연결관계를 표현하는 방식
        - 모든 노드에 연결된 노드에 대한 정보를 차례대로 연결하여 저장
        - 파이썬에서 2차원 리스트로 구현(리스트 자료형이 append() 메소드를 제공하기 때문)

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e423258d-c73a-4961-acb7-d1aa9f3665b2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e423258d-c73a-4961-acb7-d1aa9f3665b2/Untitled.png)

    - 인접 행렬 vs 인접 리스트
        - 메모리 : List가 효율적
            - Matrix : 모든 관계(자기자신, 연결, 연결X)를 저장하므로 노드 개수가 많을 수록 메모리 낭비됨
        - 연결정보 확인 : 단순 연결정보확인은 Matrix가 효율적, 모든 인접노드를 순회하는 경우는 List가 메모리 낭비가 적음
            - Matrix : graph[1][7]에 0, 무한을 제외한 값이 있는지 확인
            - List : 노드 자신이 연결된 정보만 저장하기 때문에 특정한 두 노드가 연결되어 있는지에 대한 정보를 얻는 속도가 느림

```python
# 인접 행렬 방식
INF = 987654321 # 무한의 비용 선언(논리적으로 정답이 될 수 없는 큰 값)
                # 연결되어 있지 않는 노드들에 대해서 INF 입력
matrix_graph = [
    [0, 7, 5],
    [7, 0, INF],
    [5, INF, 0]
]

print(matrix_graph) # [[0, 7, 5], [7, 0, 987654321], [5, 987654321, 0]]

#########################################################################

# 인접 리스트 방식
list_graph = [[] for _ in range(3)] # 행이 3개인 2차원 리스트로 인접 리스트 표현

# 노드 0에 연결된 노드 정보 저장(노드, 거리)
list_graph[0].append((1, 7))
list_graph[0].append((2, 5))

# 노드 1에 연결된 노드 정보 저장(노드, 거리)
list_graph[1].append((0, 7))

# 노드 2에 연결된 노드 정보 저장(노드, 거리)
list_graph[2].append((0, 5))

print(list_graph) # [[(1, 7), (2, 5)], [(0, 7)], [(0, 5)]]
```

### DFS(Depth-First Search, 깊이 우선 탐색)
- 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘
- 특정한 경로를 탐색하다가 특정한 상황에서 최대한 깊숙이 들어가서 노드를 방문한 후, 다시 돌아가 다른 경로를 탐색하는 알고리즘
- 시간 복잡도 : O(N)
- **사용법 : 재귀함수로 구현, 스택 자료구조**
- 동작 과정
    1. 탐색 시작 노드를 스택에 삽입하고 방문 처리를 한다.
    2. **스택의 최상단 노드에 방문하지 않은 인접 노드가 있으면 그 인접 노드를 스택에 넣고 방문 처리를 한다. 방문하지 않은 인접 노드가 없으면 스택에서 최상단 노드를 꺼낸다.**
    3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다.
    - "방문 처리" : 스택에 한 번 삽입되어 처리된 노드가 다시 삽입되지 않게 체크하는 것. 방문 처리를 함으로써 각 노드를 한 번씩만 처리할 수 있음
    - **인접한 노드 중, 방문하지 않은 노드가 여러개 있으면 번호가 낮은 순서부터 처리(관행적)**
- 예제

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6493ddcd-409e-4c1c-8d77-a475379a0de1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6493ddcd-409e-4c1c-8d77-a475379a0de1/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e4a3fac-135d-42ca-98a1-05e84edae2d4/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e4a3fac-135d-42ca-98a1-05e84edae2d4/Untitled.png)

### BFS(Breadth First Search, 너비 우선 탐색)

- 가까운 노드부터 탐색하는 알고리즘
- 시간복잡도 : O(N), DFS보다 수행시간이 좋은 편
- **사용법 : 큐 자료구조**
- 동작 과정
    1. 탐색 시작 노드를 큐에 삽입하고 방문 처리를 한다.
    2. 큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리를 한다.
    3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다.
- 예제

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9bc23a96-9965-4739-bef7-c9e5fe5e013f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9bc23a96-9965-4739-bef7-c9e5fe5e013f/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/22be6d7c-8038-45ee-b914-cc03a0f5921e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/22be6d7c-8038-45ee-b914-cc03a0f5921e/Untitled.png)

### DFS & BFS

- 전형적인 그래프 그림을 이용하여 풀이했지만 **1, 2차원 배열의 경우에도 '그래프 형태'로 바꿔서 접근**

    []()