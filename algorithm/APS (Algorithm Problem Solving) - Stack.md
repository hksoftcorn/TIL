[TOC]

# APS (Algorithm Problem Solving) - Stack

좋은 알고리즘이란, ① 정확성 ② 작업량 ③ 메모리 사용량 ④ 단순성 ⑤ 최적성을 만족하는 풀이를 말합니다. 이 중 알고리즘의 작업량을 표현할 때 `시간복잡도`로 표현합니다. 실제 걸리는 시간을 측정, 실행되는 명령문 수를 측정하거나 수학적으로 빅-오 표기법(Big-Oh Notation)으로 표현하기도 합니다.

## Stack

스택은 물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료구조를 말합니다. 스택에 저장된 자료는 선형 구조를 갖습니다. 선형구조는 자료 간의 관계가 1대1의 관계를 갖습니다. 반대로 비선형구조는 1:N. 스택에 자료를 삽입하거나 스택에서 자료를 꺼낼 수 있습니다, 특히 마지막에 삽입한 자료를 가장 먼저 꺼내게 되는데 이를 후입선출 `LIFO`라고 일컫습니다.

### 스택

스택의 연산은 삽입(push), 삭제(pop), 공백확인(isEmpty), 가득참 확인(IsFull), top 원소 반환(peek)이 있습니다. 

top = -1 (공백스택)

A를 push 하기 위해서 top += 1 하여 그 자리에 값을 넣게됩니다. 

B를 push 하기 위해서 top += 1 하여 그 자리에 값을 넣게됩니다. 

B를 pop 하게 되면 값을 빼내고, top -= 1을 하게 됩니다.

```python
# push, 리스트의 마지막에 데이터를 삽입합니다.
def push(item):
    # top 확인
    s.append(item)
```

```python
# pop,
def pop():
    if len(s) == 0: # underflow
        return 
    else:
        return s.pop(-1)
```

1차원 배열을 사용하여 구현할 경우 구현이 용이하다는 장점이 있지만 스택의 크기를 변경하기가 어렵다는 단점이 있습니다. 이를 해결하기 위한 방법으로 저장소를 동적으로 할당하여 스택을 구현하는 방법이 있습니다. 동적 연결리스트를 이용하여 구현하는 방법을 의미합니다. 구현이 복잡하다는 단점이 있지만 메모리를 효율적으로 사용한다는 장점을 가집니다. 리스트/배열

### 재귀호출



```python
def factorial(n):
    if n == 1:
        return 1
    return n * factorial(n-1)
```

```python
def fibo(n):
    if n < 2:
        return n
    else:
        return fibo(n-1) + fibo(n-2)
```

### Memoization

앞의 재귀호출 함수에서 엄청난 `중복호출`이라는 문제점을 살펴 볼 수 있습니다. 중복호출을 자세히 보고 싶다면 피보나치 수열의 Call Tree를 그려보면 됩니다. 

이를 해결하기 위해서 구해놓은 값을 Memoization 하면 됩니다. 즉 메모이제이션은 컴퓨터 프로그램을 실행할 때 이전에 계산한 값을 메모리에 저장해서 매번 다시 계산하지 않도록 하여 전체적인 실행속도를 빠르게 하는 기술을 말합니다. 동적 계획법(DP)의 핵심이 되는 기술입니다. 

메모이제이션(memoization)은 라틴어 memorandum (기억되어야 할 것)에서 파생된 것으로 '메모리에 넣기'라는 의미를 지닙니다. 기억하기의 memorization과 혼동해서는 안 됩니다. 메모이제이션은 memo를 위한 배열을 할당하고, 모두 0으로 초기화 합니다. 피보나치 수열을 예로, memo[0] = 0으로, memo[1] = 1로 초기화 합니다. 

```python
def fibo(n):
    global memo
    if n >= 2 and len(memo) <= n:
        memo.append(fibo(n-1) + fibo(n-2))
    return memo[n]
memo = [0, 1]
```

```python
memo = [-1] * 21
memo[0] = 0
memo[1] = 1

def fibo(n):
    if memo[n] == -1:
        memo[n] = fibo(n-1) + fibo(n-2)
    return memo[n]
```

### DP

동적 계획(Dynamic Programming) 알고리즘은 그리디 알고리즘과 같이 최적화 문제를 해결하는 알고리즘입니다. 입력 크기가 작은 문제들을 모두 해결한 후에 그 해들을 이용하여 보다 큰 크기의 문제들을 해결하여, 최종적으로 원래 주어진 입력의 문제를 해결하는 알고리즘입니다.

메모이제이션을 재귀적 구조에 사용하는 것보다 반복적 구조로 DP를 구현한 것이 성능 면에서 보다 효율적입니다. 재귀적 구조는 내부에 시스템 호출 스택을 사용하는 오버헤드가 발생하기 때문입니다.

```python
def fibo2(n):
    f = [0, 1]
    for i in range(2, n+1):
        f.append(f[i-1] + f[i-2])
    return f[n]
```

### DFS

비선형구조인 그래프 구조는 그래프로 표현된 모든 자료를 빠짐없이 검색하는 것이 중요합니다. 1) 깊이 우선 탐색(DFS) 2) 너비 우선 탐색(BFS) 방법이 있습니다.

DFS는 시작 정점의 한 방향으로 갈 수 있는 최대 경로 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 방문하는 순회방법입니다. 가장 마지막에 만났던 갈림길의 정점으로 되돌아가서 다시 깊이 우선 탐색을 반복해야 하므로 후입선출(LIFO) 구조의 스택을 이용합니다.

> DFS 방법
>
> 1) 시작 정점 v를 결정하여 방문합니다.
>
> 2) 정점 v에 인접한 정점 중에서
>
> 	- 방문하지 않은 정점 w가 있으면, 정점 v를 스택에 push하고 정점 w를 방문합니다. 그리고 w를 v로 하여 다시 2)를 반복합니다.
> 	- 방문하지 않은 정점이 없다면, 탐색의 방향을 바꾸기 위해서 스택을 pop하여 받은 가장 마지막 방문 정점을 v로 하여 다시 2)를 반복합니다.
>
> 3) 스택이 공백이 될 때까지 2)를 반복합니다.

```python
# 재귀
DFS_Recursive(G, V):
    visited[v] <- True
    
    for 
    
# 스택
visited[]
DFS(v):
    push(s, v)
    while not isEmpty(s):
        v = pop(s)
        if not visited[v]:
            visit(v)
            for each w in adjacency(v):
                if not visited[w]:
                    push(s, w)

```

> 인접 노드를 구하는 방법
>
> - 딕셔너리
> - 인접리스트 
> - 인접행렬 

#### Graph 입력받기

```python
"""
V(Vertex, Node)
E(Edge)
	Undirected Graph
	Directed Graph

V : 6
E : 5
1 4
1 3
2 3
2 5
4 6
1 6

Start : 1
Goal : 6
"""
# 1. Dictionary
graph = {
    1: [4, 3],
    2: [3, 5],
    4: [6]
}

# 2. Adjacency List : O(n)
graph = [
    [], # 0
    [4, 3], # 1
    [3, 5], # 2
    [], # 3
    [6], # 4
    [], # 5
    [], # 6
]

# 3. Adjacency Matrix : O(1) 공간복잡도 trade off 시간복잡도
graph = [[]] 2차원
```

#### DFS 구현

```python
def solution(V, E, graph, S, G):
    # index 통일! vertex == visited idx == graph idx
    visited = [False for _ in range(V+1)] # 0번 미사용
    to_visits = [S]
    
    while to_visits:
        current = to_visits.pop()
        if not visited[current]:
            visited[current] = True
            to_visites += graph[current]
            
    return visited[G]


T = int(input())
for tc in range(1, T+1):
    V, E = map(int, input().split())
    graph = [[] for _ in range(V+1)]
    
    for _ in range(E):
        start, end = map(int, input().split())
        graph[start].appnend(end)
        # undirected
        # graph[end].append(start)
        
    S, G = map(int, input().split())
    print('#{} {}'.format(tc, solution()))
```

### 계산기

#### 후위 표기법

문자열로 된 계산식이 주어질 때, 스택을 이용하여 계산식의 값을 계산할 수 있습니다. 후위표기법은 연산자를 피연산자 뒤에 표기합니다. 

##### 중위표기식 → 후위표기식 변환

> - 1) 수식의 각 연산자에 대해서 우선순위에 따라 괄호를 사용하여 다시 표현합니다. 연산자를 괄호 뒤로 이동시키고 나서, 괄호를 제거합니다.
>
>   ```python
>   A*B-C/D
>   ((A*B)-(C/D))
>   ((A B)* (C D)/)-
>   AB*CD/-
>   ```
>
> - 2) 알고리즘 이용 : ① ~ ⑥ step을 거침. 
>
>   i.g. ( 6 + 5 * (2 - 8) / 2 )
>
>   - 토근 하나 가져오기
>   - 6 피연산자면 출력
>   - +연산자가 (보다 우선순위가 높음 push()
>   - 5 피연산자면 출력
>   - *연산자가 +보다 우선순위가 높음 push()
>   - ....
>   - 결과값 : 6528-*2/*
>
>   연습. 5 +(7 + 4 / 2 * (5 -4 + 5) * 1 / 2 + 1)

##### 후기표기식 계산

> i.g. 6528-*2/+
>
> 		 - 2 - 8 = -6 → 5 * -6 = -30 → -30 / 2 = -15 → 6 + -15 = -9
> 

### 백트래킹

백트래킹 (Backtracking) 기법은 해를 찾는 도중에 막히면 되돌아가서 다시 해를 찾아 가는 기법이다. 백트래킹 기법은 최적화 문제와 결정 문제를 해결할 수 있다. 결정 문제(문제의 조건을 만족하는 해가 존재하는지의 여부)에 적용할 수 있다. i.g. 미로 찾기 / n-Queen 문제 / Map coloring / 부분 집합의 합(Subset Sun)

*백트래킹과 깊이우선탐색과의 차이*

- 어떤 노드에서 출발하는 경로가 해결책으로 이어질 것 같지 않으면 더 이상 그 경로를 따라가지 않음으로써 시도의 횟수를 줄입니다 (Prunnung 가지치기)
- 깊이우선탐색이 모든 경로를 추적하는데 비해 백트래킹은 불필요한 경로를 조기에 차단합니다.
- 깊이우선탐색을 가하기에는 경우의 수가 너무나 많습니다(N). 가지의 경우의 수를 가진 문제에 대해 깊이우선탐색을 가하면 당연히 처리 불가능한 문제입니다.
- 백트래킹 알고리즘을 적용하면 일반적으로 경우의 수가 줄어들지만 이 역시 최악의 경우에는 여전히 지수함수 시간을 요하므로 처리 불가능합니다.

즉 백트래킹은 모든 후보를 검사하지 않습니다. 어떤 노드의 **유망성**을 점검한 후에 유망하지 않다고 결정되면 그 노드의 부모로 되돌아가 다음 자식 노드로 이동합니다. 어떤 노드를 방문하였을 때 그 노드를 포함한 경로가 해답이 될 수 없으면 그 노드는 유망하지 않다고 하며, 반대로 해답의 가능성이 있으면 유망하다고 합니다. 가지치기는 유망하지 않은 노드가 포함되는 경로는 더 이상 고려하지 않는 것을 말합니다.

*백트래킹 절차*

- 상태 공간 트리의 깊이 우선 검색을 실시합니다.
- 각 노드가 유망한지를 점검합니다.
- 만일 그 노드가 유망하지 않으면, 그 노드의 부모 노드로 돌아가서 검색을 계속합니다.

#### 미로 찾기

입구와 출구가 주어진 미로에서 입구부터 출구까지의 경로를 찾는 문제입니다. 이동할 수 있는 방향은 4방향으로 제한합니다. Tip : 사방에 벽을 한 줄씩 추가하여 IndexError를 방지합니다. 

#### 부분집합 구하기

어떤 집합의 공집합과 자기자신을 포함한 모든 부분집합을 `powerset`이라고 하며 구하고자 하는 어떤 집합의 원소 개수가 n일 경우 부분집합의 개수는 2^n이 나온다.

```python
N = 3
arr = [1,2,3]
sel = [0] * N

def powerset(idx):
    if idx == N:
        print(sel)
        return 
    else:
	    sel[idx] = 1
    	powerset(idx+1)
	    sel[idx] = 0
    	powerset(idx+1)
    
powerset(0)
```

#### 순열 생성하기

동일한 숫자가 포함되지 않은 순열 구하기

##### 순열 재귀

```python
# 순열 재귀
# [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]
arr = [1,2,3]
N = 3
sel = [0] * N
check = [0] * N

def perm(idx):
    if idx == N:
        print(sel)
    else:
        for i in range(N):
            if check[i] == 0:
                sel[idx] = arr[i]
                check[i] = 1
                perm(idx+1)
                check[i] = 0
perm(0)
```

##### 순열 비트

```python
# 순열 비트
# [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]
arr = [1,2,3]
N = 3
sel = [0] * N

#check 10진수 정수
def perm(idx, check):
    if idx == N:
        print(sel)
        return

    for j in range(N):
        # j번째 원소를 활용했으면, 쓰면 안 되지
        if check & (1<<j): continue
        sel[idx] = arr[j]
        perm(idx+1, check | (1<<j))
            
perm(0, 0)
```

##### 순열 스왑

```python
# 순열 스왑
# [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]
arr = [1,2,3]
N = 3
sel = [0] * N

def perm(idx):
    if idx == N:
        print(sel)
    else:
        for i in range(idx, N):
            arr[idx], arr[i] = arr[i], arr[idx]
            perm(idx + 1)
            arr[idx], arr[i] = arr[i], arr[idx]
```

### 분할 정복 알고리즘

나폴레옹이 사용한 전략. 분할 : 해결할 문제를 여러 개의 작은 부분으로 나눈다. 정복 : 작은 문제들을 해결한다. 통합 : 해결된 해답을 모은다.

#### 거듭제곱

##### 분할 정복 거듭제곱 (log N)

```python
dif Recursive_power(x, n):
    if n== 1: return x
    if n % 2 == 0:
        y = Recursive_power(x, n // 2)
        return y * y
    else:
        y = Recursive_power(x, (n-1) // 2)
        return y * y * x
```

#### 퀵 정렬 (N log N)

주어진 배열을 두 개로 분할하고, 각각을 정렬합니다. 

*퀵 정렬 vs 합병 정렬*

- 합병 정렬은 그냥 두 부분으로 나누는 반면에, 퀵 정렬은 분할을 할 때, 기준 아이템(pivot item) 중심으로, 이보다 작은 것은 왼편, 큰 것은 오른편에 둡니다.
- 각 부분 정렬이 끝난 후, 합병정렬은 "합병"이란 후처리 작업(통합)이 필요하나, 퀵정렬은 필요로 하지 않습니다.

호어 파티션 (center)

```python
# [69, 10, 30, 2, 16, 8, 31, 22]
# pivot = 2 / L >= pivot / R < pivot
# L과 R이 만나면 피봇과 교환합니다

```





