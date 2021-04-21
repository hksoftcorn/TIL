[TOC]

# APS - Graph

> 2021.04.21.

- 그래프 탐색 기법인 BFS와 DFS에 대해 학습합니다.
- 그래프 알고리즘에 활용되는 상홓배타 집합(DIsjoint-Sets)의 자료구조에 대해 학습합니다.
- 최소 신장 트리를 이해하고 탐욕 기법을 이용해서 그래프에서 최소 신장 트리를 찾는 알고리즘을 학습합니다.
- 그래프의 두 정점 사이의 최단 경로를 찾는 방법을 학습합니다.



## 1. 그래프 기본

### 1.1. 그래프

- 그래프는 아이템들과 이들 사이의 연결 관계를 표현합니다.
- 그래프는 정점들의 집합과 이들을 연결하는 간선들의 집합
  - V : 정점 / E : 간선의 개수
- 선형 자료구조나 트리 자료구조로 표현하기 어려운 N : N 관계를 가지는 원소들을 표현하기에 용이하다.



### 1.2. 그래프 종류

- 무향 그래프 (Undirected G)
- 유향 그래프 (Directed G)
- 가중치 그래프 (Weighted G)
- 사이클 없는 방향 그래프 (DAG, Directed Acyclic G)



### 1.3. 그래프 유형

- 완전 그래프 : 정점들에 대해 가능한 모든 간선들을 가진 그래프

- 부분 그래프 : 원래 그래프에서 일부의 정점이나 간선을 제외한 그래프

- 용어

  - 인접 : 두 개의 정점에 간선이 존재 

  - 경로 : 간선들을 순서대로 나열한 것

    - 단순경로 : 한 번만 지나는 경로
    - 사이클 : 되돌아 오는 경로

    

### 1.4. 그래프 표현

- 간선의 정보를 저장하는 방식, 메모리나 성능을 고려해서 결정
  - 인접 행렬 (Adjacent matrix)
  - 인접 리스트 (Adjacent list)
  - 간선의 배열

```python
"""
5 6  # V, E
1 2 1 3 2 3 4 2 5 5 4
"""

V, E = map(int, input().split())
edge = list(map(int, input().split()))
adj = [[0] * (V+1) for _ in range(V+1)]	# 인접 행렬
adjList = [[] for _ in range(V+1)]	# 인접 리스트
for i in range(E):
    n1 = edge[i*2]
    n2 = edge[i*2 + 1]
    adj[n1][n2] = 1 	# 무향 그래프 -> 대칭
    adj[n2][n1] = 1
    adjList[n1].append(n2)
    adjList[n2].append(n1)
```



## 2. 그래프 탐색

- i.g. 친구관계
  
- A로부터 시작해서 친구들에게 동시에 소식을 전달할 수 있다고 할 때, 가장 늦게 전달 받는 사람은 누구?
  
- 그래프 순회는 비선형구조인 그래프로 표현된 모든 자료(정점)를 빠짐없이 탐색하는 것을 의미한다.

  - DFS vs BFS

  - DFS : 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 방문하는 순회방법 -> 가장 마지막에 만났던 갈림길의 정점으로 되돌아가서 다시 깊이 우선 탐색을 반복해야 하므로 후입선출 구조의 스택(STACK) 사용

    - 재귀

      ```python
      
      ```

    - 반복

      ```python
      stack s
      visited []
      
      DFS(v):
          s.append(v)
          visited[v] = 1
          while s:
              cur_v = s.pop()
              for w in G[cur_v]:
                  if not visited[w]:
                      s.append(w)
                      visited[w] = 1
      ```

  - BFS : 너비우선탐색은 탐색 시작점의 인접한 정점들을 먼저 모두 차례로 방문한 후에, 방문했던 정점을 시작점으로 하여 다시 인접한 정점들을 차례로 방문하는 방식 -> 인접한 정점들에 대해 탐색을 진행한 후, 차례로 다시 너비우선탐색을 진행해야 하므로, 선입선출형태의 자료구조인 큐(QUEUE)를 사용

    - 구현

      ```PYTHON
      def bfs(s, V):
          Q = [s]
          visitied = [0] * (V + 1)
          visitied[s] = 1
          while Q:
              t = Q.pop(0)
              print(t)
              for i in range(1, V+1):
                  if adj[t][i] == 1 and visitied[i] == 0:
                      Q.append(i)
                      visited[i] = 1        
      ```

      

## 3. 서로소 집합들 (Disjoint-sets)

### 3.1. 서로소 집합

- 서로소 또는 상호배타 집합들은 서로 중복 포함된 원소가 없는 집합들이다. 다시 말해 교집합이 없다.
- 집합에 속한 하나의 특정 멤버를 통해 각 집합들을 구분한다. 이를 대표자라 한다.
- 상호배타 집합을 표현하는 방법
  - 연결 리스트
  - 트리
- 상호배타 집합 연산
  - make-set(x)
  - find-set(x)
  - union(x, y)

### 3.2. 상호배타집합 - 트리

- Make-Set : 유일한 멤버 x를 포함하는 새로운 집합을 생성하는 연산

  ```python
  Make-set(x):
      p[x] = x
  ```

- Find-Set : x를 포함하는 집합을 찾는 연산

  ```python
  Find-set(x):
      if x == p[x]: return x
      else: return Find-set(p[x])
  
  Find-set2(x):
      while x != p[x]:
          x = p[x]
  return x
  ```

- Union : x와 y를 포함하는 두 집합을 통합하는 연산

  ``` python
  Union(x, y):
      p[Find-set(y)] = Find-set(x)
  ```

- i.g. 연습예제

  - make-set (1) ~ (6)
  - union(1, 3)
  - union(2, 3)
  - union(5, 6)
  - findset(6)

### 3.3. 상호배타집합에 대한 연산

- Rank를 이용한 Union

  - 각 노드는 자신을 루트로 하는 subtree의 높이를 랭크Rank라는 이름으로 저장한다
  - 두 집합을 합칠 때 rank가 낮은 집합을 rank가 높은 집합에 붙인다.

- Path compression

  - Find-set을 행하는 과정에서 만나는 모든 노드들이 직접 root를 가리키도록 포인터를 바꾸어 준다.

  

## 4. 최소 신장 트리 (MST)

### Spanning Tree - Concept

`신장트리(Spanning Tree)`란 (1) 원 그래프의 모든 노드를 포함하고 (2) 모든 노드가 서로 연결되어 있으면서 (3) 트리(Tree)의 속성을 만족하는 그래프를 가리킵니다. 최소신장트리(MST)는 가능한 신장트리 가운데 엣지 가중치의 합이 최소인 신장트리를 말합니다. 

MST는 노드 간 연결성을 보장하면서 노드 사이를 잇는 거리/비용 등을 최소로 하는 그래프를 의미하기 때문에 응용 범위가 넓습니다. 이 글에서는 원 그래프에서 MST를 찾아내는 기법인 **크루스칼 알고리즘(Kruskal’s algorithm)**과 **프림 알고리즘(Prim’s algorithm)**을 살펴보도록 하겠습니다.

> Why we learn **MST** ? 
>
> i.g. 시골에 다섯 집이 있다고 하자. 모든 집집마다 전기를 공급하고자 하는데, 연결하는 전기선이 최소가 되는 방법을 찾는다면? MST를 이용하면 찾기 쉽다!

- 그래프에서 최소 비용 문제
  - 모든 정점을 연결하는 간선들의 가중치의 합이 최소가 되는 트리
  - 두 정점 사이의 최소 비용의 경로 찾기
- 신장 트리
  - n개의 정점으로 이루어진 무방향 그래프에서 n개의 정점과 n-1개의 간선으로 이루어진 트리
- 최소 신장 트리 (Minimum Spanning Tree)
  - 무방향 가중치 그래프에서 신장 트리를 구성하는 간선들의 가중치의 합이 최소 신장 트리

### 4.1. Prim 알고리즘

- 하나의 정점에서 연결된 간선들 중에 하나씩 선택하면서 MST를 만들어 가는 방식
  - 임의 정점을 하나 선택해서 시작
  - 선택한 정점과 인접하는 정점들 중의 최소 비용의 간선이 존재하는 정점을 선택
  - 모든 정점이 선택될 때 까지 1) 2) 반복
- 서로소인 2개의 집합 정보를 유지
  - 트리 정점들 - MST를 만들기 위해 선택된 정점들
  - 비트리 정점들 - 선택 되지 않은 정점들

```python
MST_PRIM(G, r):
    for u in G.V:
        u.key = 0xffffff
        u.pi = NULL
    r.key = 0
    Q = G.V
    
    while Q:
        u = Extract_MIN(Q)
        for v in G.Adj[u]:
            if v in Q and w(u, v) < v.key:
                v.pi = u
                v.key = w(u, v)
```

```python
# 5249. 문제
for tc in range(1, int(input()) + 1):
    V, E = map(int, input().split())
    G = [[] for _ in range(V + 1)]

    for _ in range(E):
        u, v, w = map(int, input().split())
        G[u].append((v, w))
        G[v].append((u, w))

    MST = [False] * (V + 1)
    key = [0xffffff] * (V + 1)
    key[0] = 0
    ans = 0

    for _ in range(V + 1):
        # key 값이 최소인 정점 찾기
        u, min_key = 0, 0xfffffff
        for i in range(V + 1):
            if not MST[i] and min_key > key[i]:
                u, min_key = i, key[i]

        # 트리에 포함된 정점으로 표시
        MST[u] = True
        ans += key[u]

        for v, w in G[u]:
            if not MST[v] and key[v] > w:
                key[v] = w

    print('#{} {}'.format(tc, ans))
```



### 4.2. KRUSKAL

- 간선을 하나씩 선택해서 MST를 찾는 알고리즘
  - 최초, 모든 간선을 가중치에 따라 오름차순으로 정렬
  - 가중치가 가장 낮은 간선부터 선택하면서 트리를 증가시킴 - 사이클이 존재하면 다음으로 가중치가 낮은 간선을 선택
  - N-1개의 간선이 선택될 때 까지 2)를 반복

```PYTHON
MST_KRUSKAL(G, w):
    A = 0
    for vertex v in G.V:
        Make_set(v)
    
    # G, E에 포함된 간선들을 가중치 W에 의해 정렬
    FOR 가중치가 가장 낮은 간선 (u, v) in G.E 선택(n-1개):
        IF Find_set(u) != Find_set(v):
            A = A U {(u, v)}
            Union(u, v)
	return A                   
```

```python
for tc in range(1, int(input()) + 1):
    V, E = map(int, input().split())

    edges = [list(map(int, input().split())) for _ in range(E)]

    #------------------------------------
    # disjoint-set
    p = [i for i in range(V + 1)]
    def findSet(v):
        if v != p[v]:
            p[v] = findSet(p[v])
        return p[v]
    #------------------------------------

    edges.sort(key=lambda e: e[2], reverse=True)
    cnt = ans = 0
    while cnt < V:  # 정점수 = V + 1, 간선수 = V
        u, v, w = edges.pop()
        a, b = findSet(u), findSet(v)
        if a == b: continue
        p[b] = a
        ans += w
        cnt += 1

    print('#{} {}'.format(tc, ans))
```



#### 📌정리

- Tree 
  - Hierachy
  - None cycle
- ST(스패닝 트리) : 그래프 내의 모든 정점을 포함하는 트리
- MST(최소 스패닝 트리) : ST 중 사용된 간선들의 가중치 합이 최소인 트리
  - None-cycle 조건
  - v - 1 == E 조건







## 5. 최단 경로

- 최단 경로 정의
  - 간선의 가중치가 있는 그래프에서 두 정점 사이의 경로들 중에 간선의 가중치의 합이 최소인 경로
- 하나의 시작 정점에서 끝 정점까지의 최단경로
  - 다익스트라 (dijkstra) 알고리즘 : 음의 가중치를 허용하지 않음
  - 벨만-포드 (Bellman-Ford) 알고리즘 : 음의 가중치 허용
- 모든 정점들에 대한 최단 경로
  - 플로이드-워샬(Floyd-Warshall) 알고리즘

### 5.1. Dijkstra 알고리즘

- 시작 정점에서 거리가 최소인 정점을 선택해 나가면서 최단 경로를 구하는 방식이다.
- 시작정점(s)에서 끝정점(t) 까지의 최단 경로에 정점 x가 존재한다.
- 이때, 최단경로는 s에서 x까지의 최단 경로와 x에서 t까지의 최단경로로 구성된다.
- 탐욕 기법을 사용한 알고리즘으로 MST의 프림 알고리즘과 유사하다.

```
'''
간단한 다익스트라 알고리즘 구현 예시

진행 단계마다 '방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택'
하기 위해 매 단계마다 1차원 리스트의 모든 원소를 확인(순차탐색) 해야한다.

간단한 다익스트라 알고리즘의 시간 복잡도는 O(V^2) [V: 노드의 갯수]이라 한다.
즉 노드의 갯수가 5000개 이상인 문제의 경우 간단한 다익스트라 알고리즘으로 문제를 풀이하는 것은 어렵다.
따라서 개선된 다익스트라 알고리즘을 이용해야 하는데, 이는 다음에..

#### 예시 문제 ####
 
1번 째 줄에 n과 m이 주어지며 n은 노드의 개수 m은 간선정보의 개수이다.
2번 째 줄에는 시작 노드가 주어진다.
이후
m개의 간선정보가 주어진다.

시작 노드부터 각 노드까지의 최단 거리를 구하시오.

##################

입력값
6 11
1
1 2 2
1 3 5
1 4 1
2 3 3
2 4 2
3 2 3
3 6 5
4 3 3
4 5 1
5 3 1
5 6 2

출력값
0
2
3
1
2
4
'''

# 데이터 입력 부분
import sys
input = sys.stdin.readline

INF = int(1e9)

n, m = map(int, input().split())
start = int(input())

# 다익스트라를 진행하기 위한 세팅
graph = [[] for i in range(n+1)]
visited = [False] * (n+1)
distance = [INF] * (n+1)

# 데이터 입력, graph부분에 추가해야함.
for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a].append((b, c)) # 시작 노드는 인덱스, 도착 노드는 튜플 형태의 원소로 저장

# 방문하지 않은 노드 중에서, 가장 최단 거리가 짧은 노드의 번호를 반환
def get_smallest_node():
    min_value = INF
    index = 0 # 인덱스 '초 기 값' 설정

    for i in range(1, n+1):
        # 방문하지도 않아야하고 최소값보다 작아야함.
        if distance[i] < min_value and not visited[i]: 
            min_value = distance[i]
            index = i
    
    return index


def dijkstra(start):
    # 시작 노드를 초기화 함
    distance[start] = 0 # 시작점은 거리가 0 이니까~
    visited[start] = True # 첫 시작 노드 방문 처리하고

    for j in graph[start]: # 시작 노드에서 도착 노드는 0번 인덱스 거리는 1번 인덱스에 있음 그리고 이는 튜플형태로 저장했었음
        distance[j[0]] = j[1] # 거리 리스트에 저장

    for i in range(n-1): # 시작 노드는 이미 처리했으니까. 남은 노드만 처리하자
        now = get_smallest_node()
        # 시작 노드는 처리되었으니까 이를 먼저 가장 거리가 짧은 노드의 번로를 반환하자.
        # 그리고 이를 방문처리하자
        visited[now] = True

        # 그리고 가장 거리가 짧은 노드가 현재의 노드의 위치이고
        # 거리를 누적했을 때 가장 짧은 아이를 저장하자.
        for j in graph[now]:
            cost = distance[now] + j[1]
            
            # 현재 노드를 거처서 다른 노드로 이동할 때 거리가 짧은 경우를 캐치!
            if cost < distance[j[0]]:
                distance[j[0]] = cost

# 다익스트라 수행
dijkstra(start)

# 거리 행렬을 보았을 때 INF가 아니면
# 시작 노드부터 해당 도착 노드까지의
# 최단거리를 출력
for i in range(1, n+1):
    if distance[i] == INF:
        print('INFINITY')
    else:
        print(distance[i])
```













