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
DFS/BFS : 그래프(Graph) 탐색 알고리즘

### 그래프(Graph) 자료구조
- 노드(Node)와 간선(Edge)로 표현됨
    - 노드 = 정점(Vertex)

![](/assets/images/posts/algorithm/graph.png)

- 그래프 탐색
    - 하나의 노드를 시작으로 다수의 노드를 방문하는 것
    - 두 노드가 간선으로 연결되어 있다면 두 노드는 인접하다(Adjacent)고 표현 함
- 표현 방법
    - 인접 행렬(Adjacency Matrix) : 2차원 배열로 그래프의 연결 관계를 표현하는 방식
        - 2차원 배열에 각 노드가 연결된 형태를 기록하는 방식
        - 2차원 리스트로 구현
        - 연결되어있지 않는 노드는 무한(987654321)으로 초기화

        ![](/assets/images/posts/algorithm/Adjacency-matrix.png)

    - 인접 리스트(Adjacency List) : 리스트로 그래프의 연결관계를 표현하는 방식
        - 모든 노드에 연결된 노드에 대한 정보를 차례대로 연결하여 저장
        - 2차원 리스트로 구현(`append((노드, 거리))`)

        ![](/assets/images/posts/algorithm/Adjacency-list.png)

    - 인접 행렬 vs 인접 리스트
        - 메모리 측면
            - 인접 리스트 : 연결된 정보만을 저장하기 때문에 메모리 낭비가 적음
            - 인접 행렬 : 모든 관계(자기자신, 연결, 연결X)를 저장하므로 노드 개수가 많을 수록 메모리 낭비됨
        - 노드 간 연결 정보 확인 속도
            - 인접 리스트 : 노드 자신이 연결된 정보만 저장하기 때문에 특정한 두 노드가 연결되어 있는지에 대한 정보를 얻는 속도가 느림
            - 인접 행렬 : 단순 연결 정보 확인 시 효율적(예 : 노드 1과 7이 연결되어 있는 지 확인)
    
#### 동작 방식

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
- 동작 원리 : 스택
- 사용법 : **재귀 함수** 이용
- 동작 과정
    1. 탐색 시작 노드를 스택에 삽입하고 방문 처리를 한다.
    2. 스택 안 최상단 노드에 **방문하지 않은 인접 노드가 있으면** 그 인접 노드를 스택에 넣고 방문 처리를 한다. **방문하지 않은 인접 노드가 없으면** 스택에서 최상단 노드를 꺼낸다.
    3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다.
    - "방문 처리" : 스택에 한 번 삽입되어 처리된 노드가 다시 삽입되지 않게 체크하는 것. 방문 처리를 함으로써 각 노드를 한 번씩만 처리할 수 있음
    - 인접한 노드 중, 방문하지 않은 노드가 여러개 있으면 **번호가 낮은 순서부터 처리(관행적)**
- 예제
  
    ![](/assets/images/posts/algorithm/dfs.png)

#### 동작 방식

```python
def dfs(graph, v, visited): # (노드 정보, 시작 숫자, 방문 정보)
    # 현재 노드를 방문 처리
    visited[v] = True
    print(v, end=' ')

    # 현재 노드와 연결된 다른 노드를 재귀적으로 방문
    for i in graph[v]:
        if not visited[i]: # visited[i]가 False면,
            dfs(graph, i, visited)

# 인접 리스트 방식
graph = [
    [],
    [2, 3, 8], # 1번은 2, 3, 8과 연결됨
    [1, 7],
    [1, 4, 5],
    [3, 5],
    [3, 4],
    [7],
    [2, 6, 8],
    [1, 7]
]

# 각 노드가 방문된 정보를 리스트 자료형으로 표현(1차원 리스트)
visited = [False] * 9 # [False, False, ...]

# 정의된 dfs 함수 호출
dfs(graph, 1, visited)
# 1 2 7 6 8 3 4 5
```

### BFS(Breadth First Search, 너비 우선 탐색)

- 가까운 노드부터 탐색하는 알고리즘
- 시간복잡도 : O(N), 일반적인 경우 DFS보다 수행시간이 좋은 편
- 동작 원리 : 큐
- 사용법 : **큐 자료구조** 이용
- 동작 과정
    1. 탐색 시작 노드를 큐에 삽입하고 방문 처리를 한다.
    2. 큐에서 노드를 꺼내 해당 노드의 **인접 노드 중에서 방문하지 않은 노드를 모두** 큐에 삽입하고 방문 처리를 한다.
    3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다.
- 예제

    ![](/assets/images/posts/algorithm/bfs.png)

```python
from collections import deque

def bfs(graph, start, visited):
    # Queue 구현을 위해 deque 라이브러리 사용
    queue = deque([start])

    # 현재 노드를 방문 처리
    visited[start] = True

    # 큐가 빌 때까지 반복
    while queue:
        # 큐에서 하나의 원소를 뽑아 출력
        v = queue.popleft()
        print(v, end=" ")

        # 해당 원소와 연결된, 아직 방문하지 않은 원소들을 큐에 삽입
        for i in graph[v]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True

# 인접 리스트 방식
graph = [
    [],
    [2, 3, 8], # 1번은 2, 3, 8과 연결됨
    [1, 7],
    [1, 4, 5],
    [3, 5],
    [3, 4],
    [7],
    [2, 6, 8],
    [1, 7]
]

# 각 노드가 방문된 정보를 리스트 자료형으로 표현(1차원 리스트)
visited = [False] * 9 # [False, False, ...]

# 정의된 dfs 함수 호출
bfs(graph, 1, visited)
# 1 2 3 8 7 4 5 6
```

#### DFS, BFS 문제 유형에서 1/2차원 배열의 경우에도 '그래프 형태'로 생각하면 문제를 풀 수 있음
- 2차원 배열에서 탐색해야하는 문제는 그래프 형태로 바꿔서 생각하자
- 탐색 문제를 보면 먼저 그래프 형태로 표현할 것

![](/assets/images/posts/ml/Matrix_location.png)

## 3. 실전 문제 

> 음료수 얼려 먹기

N * M 크기의 얼음틀이 있다. 구멍이 뚫려있는 부분은 0, 칸막이가 존재하는 부분은 1로 표시된다.
구멍이 뚫려 있는 부분끼리 상, 하, 좌, 우로 붙어 있는 경우 서로 연결되어 있는 것으로 간주한다. 
이때 얼음 틀의 모양이 주어졌을 때 생성되는 총 아이스크림의 개수를 구하는 프로그램을 작성하시오. 
다음의 4 X 5 얼음 틀 예시에서는 아이스크림이 총 3개 생성된다.<br>

00110<br>
00011<br>
11111<br>
00000<br>

한 번에 만들 수 있는 아이스크림의 개수를 출력하라

- 시간 제한 : 1초
- 메모리 제한 : 128MB
- 입력 조건 : $1 <= N, M <= 1,000$

-> 얼음을 얼릴 수 있는 공간(상, 하, 좌, 우로 연결된 공간)을 그래프 형태로 모델링하여 DFS로 문제를 해결하는 것이 핵심
1. 특정한 지점 주변 상, 하, 좌, 우를 살펴본 뒤에 주변 지점 중에서 값이 '0'이면서 아직 방문하지 않은 지점이 있다면 방문
2. 방문한 지점에서 다시 상, 하, 좌, 우를 살펴보면서 방문을 다시 진행하면, 연결된 지점 모두 방문 가능
3. 1 ~ 2번 과정을 모든 노드에 반복하며 방문하지 않은 지점의 수를 센다.

```python
n, m = map(int, input().split())
# 3, 3

graph = []
for i in range(n):
    graph.append((list(map(int, input()))))
# graph = [
#   [001],
#   [010],
#   [101]
#   ]

# DFS로 특정한 노드 방문 뒤 연결된 모든 노드들 방문
def dfs(x, y):
    if x <= -1 or x >= n or y <= -1 or y >=m:
        return False

    if graph[x][y] == 0: 
        graph[x][y] = 1 # 방문하지 않았다면 방문 처 

        # 상, 하, 좌, 우 위치의 노드들도 재귀적으로 호출
        dfs(x-1, y)
        dfs(x, y-1)
        dfs(x+1, y)
        dfs(x, y+1)

        return True
    return False

# 모든 노드에 대하여 음료수 채우기
result = 0
for i in range(n): # 0, 1, 2
    for j in range(m): # 0, 1, 2
        if dfs(i, j) == True: # 현재 위치에서 DFS 수행
            result += 1

print(result)
# 3
```