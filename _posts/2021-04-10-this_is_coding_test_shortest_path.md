---
title: "[이것이 취업을 위한 코딩 테스트다] 최단 경로(Shortest Path)"
category: Algorithm Theory
use_math: true
---

# 1. 최단 경로(Shortest Path) 알고리즘
- 특정 지점까지 가장 빠르게 도달하는 방법을 찾는 알고리즘(= 길 찾기 문제)
- 그래프(노드, 간선)를 이용하여 표현
- 출제 유형
  - **다익스트라 최단 경로 알고리즘**, **플로이드 워셜 알고리즘**, 벨만 포드 알고리즘
  - 실제 문제에서는 **특정 노드에서 다른 특정 노드까지의 최단 거리를 출력**하도록 요청하는 편이다.

<br>

# 2. 종류

## 1) 다익스트라(Dijkstra) 최단 경로 알고리즘
- 그래프에서 여러 개의 노드가 있을 때, 특정한 노드에서 출발하여 **다른 노드로 가는 '각각의' 최단 경로**를 구하는 알고리즘 
- **'음의 간선'이 없을 때 정상적으로 동작**
    - 음의 간선 : 0보다 작은 값을 가지는 간선
- GPS 소프트웨어의 기본 알고리즘으로 사용됨
- 가장 비용이 적은 노드를 선택해서 과정을 반복하기 때문에 그리디 알고리즘으로 분류됨

<br>

### 다익스트라 알고리즘의 원리

1. 출발 노드 설정
2. 최단 거리 테이블 초기화
   - 출발 노드 까지의 거리는 0으로 초기화
   - 나머지 노드 까지의 거리는 int(1e9)와 같은 무한으로 초기화
3. **방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택(순차 탐색)**
4. **해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신(그리디 알고리즘의 성격)**
5. 3, 4 번 반복

<br>

### 특징
- 최단 경로를 구할 때 '각 노드에 대한 현재까지의 최단 거리' 정보를 항상 1차원 리스트(= 최단 거리 테이블)에 저장하며 리스트를 지속적으로 갱신
    - 매번 처리하고 있는 노드를 기준으로 주변 간선을 확인
    - 현재 처리하고 있는 노드와 인접 노드로 도달하는 더 짧은 경로를 찾으면, 그 경로를 가장 짧은 경로로 판단함
- 한 단계당 하나의 노드에 대해서만 최단 거리를 확실하게 찾는다.

<br>

### 구현 방법

#### (1) 간단한 다익스트라 알고리즘 
- 알고리즘을 그대로 구현하는 방법
- **구현하기 쉽지만 느리게 동작함**
- 시간 복잡도 : $O(V^2), V = 노드 개수$
- 리스트를 사용하여 구현함
    - 각 노드에 대한 최단 거리를 담는 1차원 리스트 선언
    - 단계마다 '방문하지 않은 노드 중 최단 거리가 가장 짧은 노드를 선택'하기 위해 단계마다 1차원 리스트의 모든 원소를 확인(순차 탐색)
- 입력 데이터의 수가 많아지므로 `sys.std.readline()`을 사용한다.
- **$V \le 5,000$ 에서만 적용 가능** (파이썬은 1초당 약 2천만번의 연산)

#### 예

![Dijkstra](/assets/images/posts/algorithm/this_is_coding_test/dijkstra.jpg)

```python
import sys

input = sys.stdin.readline
INF = int(1e9)

n, m = map(int, input().split()) 
# 6, 11 (노드 개수, 간선)

start = int(input()) 
# 1 (시작 노드)

graph = [[] for i in range(n+1)] # 노드 정보 리스트 생성

visited = [False] * (n + 1) # 방문 체크 리스트 생성

distance = [INF] * (n + 1) # 최단 거리 테이블 무한 초기화

for _ in range(m):
    a, b, c = map(int, input().split()) # a 노드에서 b 노드로 가는 비용 : c
    # 1 2 2
    # 1 3 5
    # 1 4 1
    # 2 3 3
    # 2 4 2
    # 3 2 3
    # 3 6 5
    # 4 3 3
    # 4 5 1
    # 5 3 1
    # 5 6 2
    graph[a].append((b, c))

# 방문하지 않은 노드 중에서, 가장 최단 거리가 짧은 노드의 번호 반환
def get_smallest_node():
    min_value = INF
    index = 0 # 가장 최단 거리가 짧은 노드(인덱스)
    for i in range(1, n+1):
        if distance[i] < min_value and not visited[i]:
            min_value = distance[i]
            index = i
    return index

def dijkstra(start):
    distance[start] = 0 # 시작 노드 초기화
    visited[start] = True
    for j in graph[start]:
        distance[j[0]] = j[1]
    for i in range(n-1): # 시작 노드를 제외한 전체 n-1개의 노드에 대해 반복
        now = get_smallest_node() # 현재 가장 짧은 노드를 꺼내 방문 처리
        visited[now] = True

        for j in graph[now]:
            cost = distance[now] + j[1]
            if cost < distance[j[0]]: # 현재 노드를 거쳐서 다른 노드로 이동한 거리가 더 짧은 경우(갱신)
                distance[j[0]] = cost

# 다익스트라 알고리즘 수행
dijkstra(start)

# 모든 노드로 가기 위한 최단 거리 출력
for i in range(1, n+1):
    if distance[i] == INF: # 도달 불가능한 경우
        print("INFINITY")
    else: # 도달 가능한 경우
        print(distance[i])
# 0
# 2
# 3
# 1
# 2
# 4
```

<br>

#### (2) 개선된 다익스트라 알고리즘
- 최단 거리가 가장 짧은 노드를 기존 방법보다 더 빠르게 찾는 방법
- **현재 가까운 노드를 저장하기 위한 목적으로만 우선순위 큐를 사용(최소 힙 기준)**
- **구현하기 까다롭지만 빠르게 동작함**
- 시간 복잡도 : $O(ElogV), V = 노드 개수, E = 간선 개수$
- 힙(Heap) 자료구조를 사용하여 구현함(현재 가장 가까운 노드 저장)

##### 힙(Heap)
- 우선순위 큐를 구현하기 위해 사용하는 자료구조

##### 우선순위 큐(Priority Queue)
- 우선순위가 가장 높은 데이터를 가장 먼저 삭제
- 라이브러리 : **heapq(더 빠름)**, PriorityQueue
- 데이터를 '(가치 = 우선순위, 물건)' 형태로 묶어서 우선순위 큐 자료구조에 넣음
- 내부적으로 **최소 힙** 또는 **최대 힙**을 기반으로 구현
    - **최소 힙**
      - 값이 낮은 데이터가 먼저 삭제됨
      - 파이썬의 우선순위 큐 라이브러리는 최소 힙을 기반으로 함
    - 최대 힙
      - 값이 큰 데이터가 먼저 삭제됨
      - 최소힙에서 우선순위에 (-)를 붙였다가 우선순위 큐에서 꺼낸 다음 다시 (-)를 붙여 원래 값으로 돌리는 방식으로 구현 가능
- 우선순위 큐는 리스트로 구현 가능 하나 큐로 구현하는 것이 더 빠르다.
    - 데이터 개수가 N일 때, 데이터를 모두 넣고 다시 모두 꺼낸다면
      - 리스트 : $N \times \Big( O(1) + O(N) \Big) => O(N^2)$
      - 힙 : $N \times \Big( O(logN) + O(logN)  \Big) => O(NlogN)$

##### 예

!!!!! 이미지 업데이트 필요

![Dijkstra]()

```python
# 다익스트라 알고리즘(Dijkstra)

# Heap을 이용한 방법 : 복잡 + 빠름
import heapq
import sys

input = sys.stdin.readline
INF = int(1e9)

n, m = map(int, input().split()) 
# 6, 11 (노드 개수, 간선)

start = int(input()) 
# 1 (시작 노드)

graph = [[] for i in range(n+1)] # 노드 정보 리스트 생성

visited = [False] * (n + 1) # 방문 체크 리스트 생성

distance = [INF] * (n + 1) # 최단 거리 테이블 무한 초기화

for _ in range(m):
    a, b, c = map(int, input().split()) # a 노드에서 b 노드로 가는 비용 : c
    # 1 2 2
    # 1 3 5
    # 1 4 1
    # 2 3 3
    # 2 4 2
    # 3 2 3
    # 3 6 5
    # 4 3 3
    # 4 5 1
    # 5 3 1
    # 5 6 2
    graph[a].append((b, c))

def dijkstra(start):
    q = []
    heapq.heappush(q, (0, start)) # 시작 노드로 가기 위한 최단 경로는 0으로 설정 후 큐 삽입
    distance[start] = 0
    while q: # 큐가 비어있지 않다면
        dist, now = heapq.heappop(q) # 가장 최단 거리가 짧은 노드에 대한 정보 꺼내기
        if distance[now] < dist: # 현재 노드가 이미 처리된 적 있다면 무시
            continue
        for i in graph[now]: # 현재 노드와 연결된 다른 인접 노드 확인
            cost = dist + i[1]
            if cost < distance[i[0]]: # 현재 노드를 거쳐서 다른 노드로 이동하는 거리가 더 짧은 경우
                distance[i[0]] = cost
                heapq.heappush(q, (cost, i[0]))

# 다익스트라 알고리즘 수행
dijkstra(start)

# 모든 노드로 가기 위한 최단 거리 출력
for i in range(1, n+1):
    if distance[i] == INF: # 도달 불가능한 경우
        print("INFINITY")
    else: # 도달 가능한 경우
        print(distance[i])
# 0
# 2
# 3
# 1
# 2
# 4
```

<br>

## 2) 플로이드 워셜 알고리즘(Floyd-Warshall Algorithm)
