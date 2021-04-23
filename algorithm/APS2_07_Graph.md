# 2021.04.23.

- **문제 사이트** : 
  - [x] SWEA
    - [ ] 5247 연산
    - [ ] 1251 하나로
    - [ ] 5251 최소이동거리
    - [ ] 2814 최장 경로


- **사용한 알고리즘**
  - GRAPH / Heap / Kruskal / Prim / Dijkstra

- 반성 : 무엇을 하고 있나? 어떤 개념을 연습하고 있다거나 확장하고 있다는 느낌이 전혀 없었다. 아직 이론적 토양이 제대로 다져지지 않았기 때문이라고 생각함. 무엇을 연습하고 있는지 다시 돌아보자.





## 5247 연산

- BFS : Runtime Error
  - 연산결과가 1이상 100만 이하면 인접 정점
  - 이미 만든 자연수는 다시 만들면 연산 횟수가 늘어나므로 만들지 않음
  - 큐 구현에 의한 시간 초과 : `선형 큐` or deque 객체 사용

##### solution.py

```python
def bfs(n, m):
    visited = [0] * 100001
    queue = [0] * 100001
    front = -1
    rear = -1
    rear += 1
    queue[rear] = n
    visitied[n] = 1
    while front != rear:
        front += 1
        n = queue[front]
        if n == m:
            return visited[n] - 1
        t = [n-10, n-1, n+1, n*2]
        for x in t:
            if 1 <= x <= 1000000:
                if not visited[x]:
                    visited[x] = visited[n] + 1
                    rear += 1
                    queue[rear] = x


T = int(input())
for tc in range(1, T+1):
    N, M = map(int, input().split())
    
    print('#{} {}'.format(tc, bfs(N, M)))
```



## 5248 그룹 나누기

- 문제 설명 : 1번부터 N번까지의 출석번호가 있고, M 장의 신청서가 제출되었을 때 전체 몇 개의 조가 만들어지는지 반환하시오
- 컨셉 : dfs가 반복된 횟수를 반환합니다.

```python
# dfs가 반복된 횟수를 반환합니다.
def dfs(v):
    global visited
    visited[v] = 1
    
    for w in G[v]:
        if not visited[w]:
            dfs(w)


T = int(input())
for tc in range(1, T+1):
    N, M = map(int, input().split())
    input_data = list(map(int, input().split()))
    G = [[] for _ in range(N + 1)]
    visited = [0] * (N + 1)
    for i in range(M):
        p, c = input_data[2 * i : 2 * (i + 1)]
        G[p].append(c)
        G[c].append(p)

    cnt = 0
    for idx in range(1, N + 1):
        if not visited[idx]:        
            cnt += 1
            dfs(idx)
            
    print('#{} {}'.format(tc, cnt))
```

##### solution.py - Heap

- `Heap : find_set & union`

```python
def find_set(x):
    while p[x] != x:
        x = p[x]
    return x


T = int(input())
for tc in range(1, T+1):
    N, M = map(int, input().split())
    arr = list(map(int, input().split()))
    p = list(range(N+1))  # 대표원소 초기화 = 자기자신이 대표
    
    for i in range(M): # union(a, b)
        a, b= arr[i*2], arr[i*2+1]
        p[find_set(b)] = find_set(a)
        
    cnt = 0
    for i in range(1, N + 1):	# 대표원소 수 == 그룹 수
        if p[i] == i:
            cnt += 1
    print('#{} {}'.format(tc, cnt))
```



## 5249 최소신장트리

MST 최소신장트리

- 문제 설명 : 그래프에서 사이클을 제거하고 모든 노드를 포함하는 트리를 구성할 때, 가중치의 합이 최소가 되도록 만드는 최소신장트리를 구하시오
- 컨셉 : Prim's Algorithm

```python
T = int(input())
for tc in range(1,T+1):
    V, E = map(int, input().split()) # V(Vertex): 정점은 0번부터 V까지, E(Edge): 간선의 수
    G = [[] for _ in range(V + 1)]

    for _ in range(E):
        u, v, w = map(int, input().split())
        G[u].append((v, w))
        G[v].append((u, w))
    
    MST = [0] * (V + 1)
    key = [0xfffffff] * (V + 1)
    key[0] = 0
    ans = 0
    
    for _ in range(V + 1): # 0 ~ V 까지 모든 정점을 돌아봅니다.
        u, min_key = 0, 0xfffffff   # 값을 초기화 합니다.
        for i in range(V + 1):
            if not MST[i] and min_key > key[i]:
                u, min_key = i, key[i]
        MST[u] = 1
        ans += key[u]

        for v, w in G[u]:
            if not MST[v] and key[v] > w:
                key[v] = w

    print('#{} {}'.format(tc, ans))

```

- Prim 또는 Kruskal 알고리즘으로 문제를 해결합니다.
  - SUDO CODE ?

##### solution1.py - Kruskal

```python
T = int(input())
for tc in range(1,T+1):
    V, E = map(int, input().split()) # V(Vertex): 정점은 0번부터 V까지, E(Edge): 간선의 수
    edges = [list(map(int, input().split())) for i in range(E)]
    edges.sort(key=lambda x : x[2]) # w로 정렬합니다. # quick_sort를 구현합니다. ㅎㅅㅎ
    
	p = [i for i in range(V + 1)] # 대표 원소 초기화
    # N개의 정점이 있으면 사이클이 생기지 않도록 N-1 간선을 선택
    # MST에 포함된 간선의 가중치의 합 구하기
    N = V + 1	# 0 ~ V번 까지의 정점
    cnt = 0
    total = 0	# 가중치의 합
    for u, v, w in edge:	# N-1개의 간선 선택 루프
        if find_set(u) != find_set(v): # 사이클을 형성하지 않으면, 즉 같은 소속이 아니면
            cnt += 1
            total += w	# 가중치의 합
            p[find_set(v)] = find_set(u)	# v의 대표원소를 u의 대표원소로 바꿈
            if cnt == N-1:	# N-1개의 간선 선택 완료
                break
                
    print('#{} {}'.format(tc, total))
```

##### solution2.py - Prim

```python
def extract_min(MST, key, V):
    minV = INF
    u = 0
    for i in range(1, V+1):
        if MST[i] == 0:
            if key[i] < minV:
                u = i
                minV = key[i] # 여기까지 
	return u	# MST에 속하지 않은 정ㅈ머 중 MST연결 비용이 최소인 지점


def prim(start, V):	# MST 가중치의 합을 리턴하는 함수
    key = [INF] * (V + 1)
    key[start] = 0
    MST = [0] * (V + 1)
    pi = [0] * (V + 1)
    
    for i in range(start, V): # 모든 정점을 돌아다닙니다.
        # MST에 포함되지 않은 정점 중 key[u]가 최소인 u 찾기
        u = extract_min(MST, key, V)
        MST[u] = 1
        for v in range(start, V + 1):
            if MST[v] == 0 and adj[u][v] != 0:
                if key[v] > adj[u][v]:
                    key[v] = adj[u][v]
                    pi[v] = u
                    
    return sum(key[start:])
        

INF = 10000
T = int(input())
for tc in range(1,T+1):
    V, E = map(int, input().split()) # V(Vertex): 정점은 0번부터 V까지, E(Edge): 간선의 수
    
    adj = [[0] * (V + 1) for _ in range(V + 1)] # 인접행렬
    
    for _ in range(E):
        u, v, w = map(int, input().split())
        adj[u][v] = w
        adj[v][u] = w	# 무향 그래프 MST 구성
        
    print('#{} {}'.format(tc, prim(0, V)))
```

- 크루스칼 





## 5250 최소비용

- 문제 설명 : N x N Matrix에서 (0, 0)에서 출발하여 (N-1, N-1)까지 도달하는 최소 비용을 구하시오.
- 컨셉 : Dijkstra Algorithm / (1) 이동마다 cost는 +1 / (2) 기입된 숫자는 높이를 말하며, 낮은 곳에서 높은 곳으로 이동 시에는 높이 차만큼의 cost가 붙음

```python
import sys; sys.stdin = open('sample_input.txt', 'r')


dr = [-1, 1, 0, 0]
dc = [0, 0, -1, 1]
def bfs(r, c):
    Q = [(r, c)]
    visited = [[0] * N for _ in range(N)]
    visited[r][c] = 1
    dist[r][c] = 0
    
    while Q:                                                          # BFS 시작
        cur_r, cur_c = Q.pop(0)                                       # 자료구조 : 큐
        for i in range(4):                                            # 4방향을 돌아다닙니다.
            nr = cur_r + dr[i]
            nc = cur_c + dc[i]
            if 0 > nr or nr >= N or 0 > nc or nc >= N: continue
            # if not visited[nr][nc]:   
            if arr[nr][nc] - arr[cur_r][cur_c] > 0:                   # 경사를 올라간다면 cost를 더해줍니다.
                cost = arr[nr][nc] - arr[cur_r][cur_c] + 1
            else:                                                     # 아니라면 그대로 +1
                cost = 1
            if dist[nr][nc] > dist[cur_r][cur_c] + cost:              # 다음지역과 현재지역 + cost를 비교합니다.
                dist[nr][nc] = dist[cur_r][cur_c] + cost
                visited[nr][nc] = 1
                Q.append((nr, nc))


T = int(input())
for tc in range(1, T+1):
    N = int(input())
    arr = [list(map(int, input().split())) for _ in range(N)]
    s = (0, 0)
    dist = [[0xfffffff] * N for _ in range(N)]
    bfs(*s)
    print('#{} {}'.format(tc, dist[N-1][N-1]))
```

##### solution.py - dijkstra algorithm

```python
def dijkstra(N):
    INF = 1000000
    D = [[INF] * N for _ in range(N)]
    U = [[0] * N for _ in range(N)]
    D[0][0] = 0 # FOR 모든 정점 D[v] = arr[s][v]
    
    for _ in range(N * N):
        wi, wj = 0, 0
        minV = INF
        for i in range(N):
            for j in range(N):
                if U[i][j] == 0 and minV > D[i][j]:
                    minV = D[i][j]
                    wi, wj = i, j
    U[wi][wj] = 1
	for di, dj in [(0, 1), (1, 0), (0, -1), (-1, 0)]:
        ni, nj =wi + di, wj + dj
        if 0 > nr or nr >= N or 0 > nc or nc >= N: continue
        diff = H[ni][nj] - H[wi][wj] if H[ni][nj] > H[wi][wj] else 0
        D[ni][nj] = min(D[ni][nj], D[wi][wj] + diff + 1)
        
    return D[N-1][N-1]


T = int(input())
for tc in range(1, T+1):
    N = int(input())
    arr = [list(map(int, input().split())) for _ in range(N)]
    
    print('#{} {}'.format(tc, dijkstra(N)))


```

##### solution2.py - dijkstra + 우선순위 Q

```python
def dijkstra(N):
    INF = 1000000
    D = [[INF] * N for _ in range(N)]
    U = [[0] * N for _ in range(N)]
    D[0][0] = 0 # FOR 모든 정점 D[v] = arr[s][v]
    Q = []
    heapq.heappush(Q, (D[0][0], 0, 0))	# import heapq
    
    while Q:
        minV, wi, wj = heapq.heappop(Q)
        U[wi][wj] = 1	# U u {w}
        for di, dj in [(0, 1), (1, 0), (0, -1), (-1, 0)]:
            ni, nj = wi + di, wj + dj
        if 0 > nr or nr >= N or 0 > nc or nc >= N: continue
        diff = H[ni][nj] - H[wi][wj] if H[ni][nj] > H[wi][wj] else 0
        if D[ni][nj] > minV + diff + 1:    
	        D[ni][nj] = min(D[ni][nj], D[wi][wj] + diff + 1)
            heapq.heappush(Q, (D[ni][nj], ni, nj))    
```

##### solution3.py - BFS 변형

```python

```





## 1251 하나로

- 문제 설명 : 모든 섬을 연결하는 최소 비용을 구하시오
- 접근 방법 
  - 섬을 정점, 섬을 잇는 해저 터널을 간선으로 생각
  - 모든 정점에 대한 연결을 갖는 `완전 그래프`에서 MST를 찾는 문제
  - 모든 연결에 대한 비용을 미리 계산해 인접 행렬을 만들어 놓는다.
  - MST : PRIM or KRUSKAL Algorithm 선택

```python
def find_set(x):
    while p[x] != x:
        x = p[x]
    return x


def Kruskal(N):
    p = list(range(N)) # 대표원소 초기화
    L2 = 0
    cnt = 0
    for w, u, v in edge:
        if find_set(u) != find_set(v):
            p[find_set(v)] = find_set(u)
            cnt += 1
            L2 += w
            if cnt == N-1:
                return L2
    return -1
            

T = int(input())
for tc in range(1, T+1):
    N = int(input())
    X = list(map(int, input().split()))
    Y = list(map(int, input().split()))
    E = float(input())
    
    # 완전그래프 정리
    adj = [[0] * N for _ in range(N)] # 인접행렬
    for i in range(N):
        for j in range(N):
            adj[i][j] = (X[i] - X[j]) ** 2 + (Y[i] - Y[j]) **2
            adj[j][i] = adj[i][j]
    edge = []
    for i in range(N):
        for j in range(i+1, N):
            edge.append(((X[i] - X[j]) ** 2 + (Y[i] - Y[j]) **2), i, j)
    edge.sort()
    
    
```





## 5251 최소이동거리

- 
- 



## 2814 최장 경로

- 



## 7465 창용 마을 무리의 개수

- 상호 배타 집합의 대표 원소 개념을 이용해 해결

```python
def find_set(x):
    while p[x] != x:
        x = p[x]
    return x


T = int(input())
for tc in range(1, T+1):
    N, M = map(int, input().split())
    arr = list(map(int, input().split()))
    p = list(range(N+1))  # 대표원소 초기화 = 자기자신이 대표
    
    for i in range(M): # union(a, b)
        a, b= arr[i*2], arr[i*2+1]
        p[find_set(b)] = find_set(a)
        
    cnt = 0
    for i in range(1, N + 1):	# 대표원소 수 == 그룹 수
        if p[i] == i:
            cnt += 1
    print('#{} {}'.format(tc, cnt))
```



## 1795 인수의 생일 파티

- 문제 설명 : x번 집으로 오고 가는데 드는 시간 중에서 가장 오래 걸리는 집은 어느 정도 걸리는지 구하시오
- 컨셉 : 다익스트라는 그리디한 접근 방법입니다. 
- 1번 노드에서 출발하여 각자의 집으로 돌아가는 경우
  - 1번 노드를 시작 정점으로 하는 다익스트라 알고리즘 적용
- 1번 노드에 도착하는(모이는) 경우
  - 1번 진입하는 비용을 기준으로 다익스트라 알고리즘을 적용
- N개의 정점에 대해 각각 다익스트라를 적용하는 대신, 2회만 적용해 해결

```python
def dij(N, X, adj, d):
    for i in range(N+1):
        d[i] = adj[X][i]
    v = [0] * (N + 1)
    v[X] = 1
    for _ in range(N-1):
        minIdx = 0
        for i in range(1, N+1):
            if v[i] == 0 and d[i] < d[maxIdx]:
                minIdx = i
                v[minIdx] = 1
                for i in range(1, N+1):
                    if minIdx != i and adj[minIdx][i] < 1000000:
                        d[i] = min(d[i], d[minIdx] + adj[minIdx][i])
                        

T = int(input())
for tc in range(1, T+1):
    N, M, X = map(int, input().split())
    adj1 = [[0xffffff] * (N + 1) for _ in range(N + 1)]
    adj2 = [[0xffffff] * (N + 1) for _ in range(N + 1)]
    for i in range(N+1):
        adj1[i][i] = 0
        adj2[i][i] = 0
        
    for _ in range(M):
        u, v, w = map(int, input().split())
        adj1[u][v] = 2
        adj2[u][v] = 2
    d1 = [0xffffff] * (N + 1) 
    d2 = [0xffffff] * (N + 1) 
    dij(N, X, adj1, d1) # 출발 - 도착 기준
    dij(N, X, adj2, d2) # 도착 - 출발 기준
    maxV = 0
    for i in range(1, N+1):
        if d1[i] + d2[i] > maxV:
            maxV = di[i] + d2[i]
	print('#{} {}'.format(tc, maxV))
```

#### 다익스트라

## 경유지

- i → j : Dij
- i → k → j : Dik + Dkj 경유하여 



## 1249 보급로





## 1753 최단경로 (boj)

```python
def djikstra(G, start):
    INF = float('inf')
    costs = [INF for _ in range(V+1)]
    visited = [False for _ in range(V+1)]
    costs[start] = 0
    
    for _ in range(V):
        min_cost = INF
        for node in range(1, V+1):
            if not visited[node] and costs[node] < min_cost:
                min_cost = costs[node]
                next_node = node
        visited[next_node] = True
        
        adj_nodes = G[next_node]
        for adj_node, adj_cost in adj_nodes:
            if not visited[adj_node] and adj_cost < costs[adj_node]:
                costs[adj_node] = adj_cost

    
V, E = map(int, input().split())
start_node = int(input())
graph = [[] for _ in range(V+1)]
for _ in range(E):
    u, v, w = map(int, input().split())
    graph[u].append([v, w])
    print(dijkstra(graph, start_node))
```

```python
import heapq

def dijkstra(G, start):
    INF = float('inf')
    costs = [INF for _ in range(V+1)]
    costs[start] = 0
    
    hq = []
    heapq.heappush(hq, [costs[start], start])
    
    while hq:  
        current_cost, current_node = heapq.heappop(hq)
        
        adj_nodes = G[current_node]
        for adj_node, adj_cost in adj_nodes:
            new_cost = current_cost + adj_cost
            old_cost = costs[adj_node]
            
            if new_cost < old_cost:
                costs[adj_node] = new_cost
                heapq.heappush(hq, [new_cost, adj_node])
                
                
V, E = map(int, input().split())
start_node = int(input())
graph = [[] for _ in range(V+1)]
for _ in range(E):
    u, v, w = map(int, input().split())
    graph[u].append([v, w])
    print(dijkstra(graph, start_node))
```

