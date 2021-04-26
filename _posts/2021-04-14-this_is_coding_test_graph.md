---
title: "[이것이 취업을 위한 코딩 테스트다] 그래프 이론(Graph)"
category: Algorithm Theory
use_math: true
---

# 1. 복습

## 1) 그래프
> 노드와 간선의 정보를 가지고 있는 자료구조

- 구현 방법 : 인접 행렬 or 인접 리스트
    - 간선 정보 저장 시 메모리 복잡도(V : 노드 개수, E : 간선 개수)
        - 인접 행렬 : $O(V^2)$
        - 인접 리스트 : $O(E)$
    - 노드 A -> 노드 B 간선 비용 확인 시 시간 복잡도 비교
        - 인접 행렬 : $O(1)$
        - 인접 리스트 : $O(V)$

<br>

## 2) 트리
> 부모에서 자식으로 내려오는 계층적인 자료구조

- **그래프 자료구조의 한 종류**

<br>

## 3) 그래프 vs 트리

||그래프|트리|
|---|---|---|
|방향성|방향 그래프 or 무방향 그래프|방향 그래프|
|순환성|순환 and 비순환|비순환|
|루트 노드 존재 여부|루트 노드 없음|루트 노드 있음|
|노드간 관계성|부모-자식 관계 없음|부모-자식 관계|
|모델의 종류|네트워크 모델|계층 모델|

<br>

<br>

# 2. 서로소 집합(Disjoint Sets) 자료구조

## 1) 서로소 집합
- 공통 원소가 없는 집합

<br>

## 2) 서로소 집합 자료구조(= Union-find 자료구조)
> 서로소 부분 집합들로 나누어진 원소들의 데이터를 처리하기 위한 자료구조

- 서로소 집합은 자료구조는 두 가지 연산으로 조작할 수 있음
    - 합집합(Union) : 2개의 원소가 포함된 집합을 하나의 집합으로 합침
    - 찾기(Find) : 특정 원소가 속한 집합이 어떤 집합인지 알려줌
- 시간 복잡도 : $O(V+M(1+log_{2-M/V}V))$
    - $V = 노드 개수,\ V-1 = union\ 연산\ 수,\ M = find\ 연산\ 수$

<br>

## 3) 서로소 집합 계산 알고리즘(트리 자료구조 사용)

- 1. Union 연산 확인 후 서로 연결된 A, B 노드 확인<br>
    - I) A와 B의 루트 노드 A', B'를 찾음<br>
    - II) 번호가 작은 A'를 B'의 부모노드로 설정(B'가 A'를 가르키도록)

- 2. 모든 Union 연산을 처리할 때까지 1번 과정 반복
    
<br>

### (1) 간단한 동작 방식
- union의 관계를 효과적으로 보여주기 위해 그래프 형태로 시각화함<br>
  (실제로는 각 원소의 집합 정보를 표현할 때 트리 자료구조 사용함)
    
![서로소 집합 자료구조(Disjoint Sets)](/assets/images/posts/algorithm/this_is_coding_test/disjoint.png)

### (2) 알고리즘의 동작 과정

![서로소 집합 계산 알고리즘](/assets/images/posts/algorithm/this_is_coding_test/disjoint_ex.png)

<br>

#### 경로 압축(Path Compression)
> find 함수를 재귀적으로 호출하 부모 테이블 값을 갱신하는 기법

- 최악의 경우 find 함수가 모든 노드를 확인해야하는데, 시간복잡도가 $O(VM)$이 됨.
- 경로 압축(Path Compression) 기법을 적용해야 함
- 소스 코드

    ```python
    # 특정 원소가 속한 집합 찾기
    def find_parent(parent, x):
        # 루트 노드가 아니라면, 루트 노드를 찾을 때까지 재귀적으로 호출
    
        # 경로 압축 기법 적용
        if parent[x] != x:
            # 해당 노드의 루트 노드가 바로 부모 노드가 됨
            parent[x] = find_parent(parent, parent[x])
        return parent[x]
    
    # 두 원소가 속한 집합을 합치기
    def union_parent(parent, a, b):
        a = find_parent(parent, a)
        b = find_parent(parent, b)
        if a < b:
            parent[b] = a
        else:
            parent[a] = b
    
    # 노드의 개수와 간선(union 연산)의 개수 입력 받기
    v, e = map(int, input().split())
    parent = [0] * (v + 1)
    
    # 부모 테이블상에서 부모를 자기 자신으로 초기화
    for i in range(1, v + 1):
        parent[i] = i
    
    # union 연산 수행
    for i in range(e):
        a, b = map(int, input().split())
        union_parent(parent, a, b)
    
    # 각 원소가 속한 집합 출력
    print('각 원소가 속한 집합 : ', end='')
    for i in range(1, v + 1):
        print(find_parent(parent, i), end=' ')
    
    print()
    
    # 부모 테이블 내용 출력
    print('부모 테이블 : ', end='')
    for i in range(1, v + 1):
        print(parent[i], end=' ')
    ```

<br>

## 4) 서로소 집합을 활용한 사이클 판별