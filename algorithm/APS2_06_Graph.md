# Spanning Tree

## 0. Priority Queue with Heap

**우선순위 큐(Priority Queue)는 들어간 순서에 상관없이** **우선순위가 높은 데이터가 먼저 나오는 것** 것을 말합니다. 우선순위 큐는 힙(Heap)이라는 자료구조를 가지고 구현할 수 있기 때문에 이 둘을 묶어서 같이 쓴 것입니다.

### 0.1. Heap


(1) 힙은 `Complete Binary Tree(완전 이진 트리)` 이다.

(2) 모든 노드에 저장된 값(우선순위)들은 자식 노드들의 것보다 (우선순위가) 크거나 같다.

> 최대 힙 or 최소 힙

### 0.2. Heap Operation

(1) Insert

새 노드를 `heap[]` 의 맨 마지막 노드에 추가하고 , 그제서야 대소 규칙을 만족시키기 위해 해당 노드의 자리를 찾아가는 방식으로 한다면 `insert` 연산을 쉽게 할 수 있을 것 같습니다.

(2) Get

힙의 모양 규칙을 만족 시키면서 트리에서 하나의 노드를 삭제하면 힙의 최종 모양은 마지막 노드가 한 개 줄어든 트리의 모양이 됩니다. 트리의 마지막 노드를 지우고, 이 노드의 값을 루트 노드에 덮어씌운다. 그리고 우선순위를 비교하며 자리를 찾아갑니다.



## 1. 최소신장트리 MST

### 1.0 concept

`신장트리(Spanning Tree)`란 (1) 원 그래프의 모든 노드를 포함하고 (2) 모든 노드가 서로 연결되어 있으면서 (3) 트리(Tree)의 속성을 만족하는 그래프를 가리킵니다. 최소신장트리(MST)는 가능한 신장트리 가운데 엣지 가중치의 합이 최소인 신장트리를 말합니다. 

MST는 노드 간 연결성을 보장하면서 노드 사이를 잇는 거리/비용 등을 최소로 하는 그래프를 의미하기 때문에 응용 범위가 넓습니다. 이 글에서는 원 그래프에서 MST를 찾아내는 기법인 **크루스칼 알고리즘(Kruskal’s algorithm)**과 **프림 알고리즘(Prim’s algorithm)**을 살펴보도록 하겠습니다.

> Why we learn **MST** ? 
>
> i.g. 시골에 다섯 집이 있다고 하자. 모든 집집마다 전기를 공급하고자 하는데, 연결하는 전기선이 최소가 되는 방법을 찾는다면? MST를 이용하면 찾기 쉽다!

### 1.1. 크루스칼 알고리즘(Kruskal’s algorithm)

Greedy 하는 방법으로 `E(w)` 를 중심으로 그래프를 만들게 됩니다. 시간복잡도는 nlogn입니다. 정렬의 시간복잡도 입니다. 나머지 Union 과 ~~ 는 시간 복잡도에 영향을 주지 않습니다. 즉 크루스칼 알고리즘은 E를 중심으로 정렬하는 것이 가장 중요한 알고리즘이라고 할 수 있습니다.

서로소들이 하나의 집합으로 Union 하게 됩니다. 합집합을 그려봅니다. 

i.g. 전국 국토에 고속도로를 설치하는 것을 가정합시다. 다섯 개의 지점을 잇는 고속도로를 설치합니다. 1-2를 이어주면 2의 대표자가 1로 바뀌게 됩니다. 2-3을 이어주면 3의 대표자는 2가 됩니다. 만약 1-3을 이을려고 한다면 3의 부모는 2가 되고, 2의 부모는 1이기 때문에 이미 한 집합에 소속임을 알 수 있습니다. 따라서 cycle을 생성하지 않기 위해서 1-3을 이어주지 않습니다.

```python
# 하나로
import sys; sys.stdin = open('sample_input.txt', 'r')

def get_distance(v, u):
    return ((v[0] - u[0]) ** 2 + (v[1] - u[1]) ** 2) * TAX

class DisjointSet:
    def __init__(self, n):
        self.parent = list(range(n))
        self.ranks = [0] * n
    
    # find_root + path compression
    def find_root(self, node):
        parent = self.parents[node]
        if node != parent:
            self.parents[node] = self.find_root(parent)
        return self.parents[node]
    
    def union(self, node_v, node_u):
        root1 = self.find_root(node_v)
        root2 = self.find_root(node_u)
        
        if self.ranks[root1] >= self.ranks[root2]:
            self.parents[root2] = root1
            if self.ranks[root1] == self.ranks[root2]:
                self.ranks[root1] += 1
        else:
            self.parents[root1] = root2
        


T = int(input())
for tc in range(1, T + 1):
	N = int(input())
    xs = list(map(str, input().split()))
    ys = list(map(str, input().split()))
    TAX = float(input())  # 세율
    coords = [[xs[idx], ys[idx]] for idx in range(V)]
    # adj-matrix : 양방향일 때 유리, 공간낭비가 없음
    adj_matrix = [[get_distance(coords[i], coords[j]) for i in range(N)] for j in range(N)]
    
    # 모든 간선을 가중치 기준으로 정렬합니다.
    edges = []
    for i in range(N):
        for j in range(i+1, N):
            w = adj_matrix[i][j]
            edges.append((w, i, j))
    edges.sort() # w를 기준으로 sort하게 됩니다.
    
    ans = 0
    for w, v, u inedges:
        root_v, root_u = find_root(v), find_root(u)
        if root_v != root_u:
            # 간선을 그리겠다. (집합을 합치다)
            union(root_v, root_u)
            ans += w
            
    
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



### 1.2. 프림 알고리즘(Prim’s algorithm)

```python

def prim(start_node):
    global adj_matrix
    INF = float('inf')
    parents = [None] * N
    costs = [INF] * N
    costs[start_node] = 0
    
    visited = [False for _ in range()]
	
    for _ in range(V):
        minimum_cost = INF
        for node in range(V):
            if not visited[node] and costs[node] < minimum_cost:
                next_node = node
                minimum_costs = costs[node]
        visited[next_node] = True
        
        adj_nodes = adj_matrix[next_node]
        for adj_node in adj_nodes:
            w = adj_nodes[adj_node] # 가중치
           	if not visited[adj_node] and w < costs[adj_node]:
                costs[adj_node] = w
                parents[adj_node] = next_node
                              
    return sum(costs)            
```



```python
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





## 2. 최단거리 SSSP

### 2.1. 다익스트라 알고리즘 (Dijkstra algorithm)



### 2.2. 벨만-포드 알고리즘 (Bellman-ford algorithm)

