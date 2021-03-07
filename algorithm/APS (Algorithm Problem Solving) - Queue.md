

[TOC]

# APS (Algorithm Problem Solving) - Queue

## Queue

스택과 마찬가지로 삽입과 삭제의 위치가 제한적인 자료구조 : 큐의 뒤에서는 삽입만 하고, 큐의 앞에서는 삭제만 이루어지는 구조입니다. `선입선출구조(FIFO)` 큐에 삽입한 순서대로 원소가 저장되어, 가장 먼저 삽입된 원소는 가장 먼저 삭제가 됩니다. 삽입은 꼬리에 / 삭제는 머리에서 연산이 이루어집니다.

> - enQueue(item) : 큐의 뒤쪽에 원소를 삽입하는 연산
> - deQueue() : 큐의 앞쪽에서 원소를 삭제하고 반환하는 연산
> - createQueue() : 공백 상태의 큐를 생성하는 연산
> - isEmpty() : 큐가 공백상태인지를 확인하는 연산
> - isFull() : 큐가 포화상태인지를 확인하는 연산
> - Qpeek() : 큐의 앞쪽에서 원소를 삭제 없이 반환(확인)하는 연산

### 큐의 구현

#### 선형 큐

  - 1차원 배열을 이용한 큐 Q = [0] * 100
    	- 초기 상태 : front = rear = -1 초기화
    	- 공백 상태 : front = rear
    	- 포화 상태 : rear = n - 1 (n : 배열의 크기, n-1 : 배열의 마지막 원소)
  - 삽입 : enQueue(item)
    		- 마지막 원소 뒤에 새로운 원소를 삽입하기 위해
    		- rear 값을 하나 증가시켜 새로운 원소를 삽입할 자리를 마련
    		- 그 인덱스에 해당하는 배열원소 Q[rear]에 item을 저장
  - 삭제 : deQueue()
    		- 가장 앞에 있는 원소를 삭제하기 위해
    		- front 값을 하나 증가시켜 큐에 남아있는 첫 번째 원소 이동
    		- 새로운 첫 번째 원소를 리턴 함으로써 삭제와 동일한 가능함
  - 공백상태 및 포화상태 검사 : isEmpty(), isFull()
    		- Empty : front == rear
    		- Full : rear == len(Q) - 1
  - 검색 : Qpeek()
      - 가장 앞에 있는 원소를 검색하여 반환
      - Q[front + 1]

##### 선형 큐 구현

```python
class Queue:

    def __init__(self):
        self.data = []

    def enqueue(self, x):
        self.data.append(x)

    def dequeue(self):
        self.is_empty()
        self.data.pop(0)

    def is_empty(self):
        return not bool(len(self.data))

    def get_front(self):
        return self.data[0]

    def get_rear(self):
        return self.data[-1]

    def __repr__(self):
        return self.data


q = Queue()
q.enqueue(1)  # None, q.data => [1]
q.enqueue(2)  # None, q.data => [1, 2]
q.get_front()  # 1
q.get_rear()   # 2
q.dequeue()   # 1, q.data => [2]
q.dequeue()   # 2, q.data => []
```

##### 선형 큐 이용시의 문제점

- 잘못된 포화상태 인식
  - 배열의 앞부분에 활용할 수 있는 공간이 있음에도 불구하고 rear=n-1인 상태, 즉 포화상태로 인식하여 더 이상의 삽입을 수행하지 않게 되는 상태
  - 해결방법1 : 매 연산이 이루어질 때마다 저장된 원소들을 배열의 앞부분으로 모두 이동시킴
  - 해결방법2 : 1차원 배열을 사용하되, 논리적으로는 배열의 처음과 끝이 연결되어 원형 형태의 큐를 이룬다고 가정하고 사용함 // 남아있는 상태에는 잘 활용할 수 있지만, 포화상태를 해결할 수 있는 방법은 아님

#### 리스트 큐

```python
N = 20

queue = [(1,0)] # 초기화

# (0, 0) [0] 
new_people = 1
last_people = 0

while N > 0:
    num, cnt = queue.pop(0)
    
    last_people = num
    cnt += 1
    
    N -= cnt
    new_people += 1
    
    queue.append((num, cnt))
    queue.append((new_people, 0))

print(last_people)
```



#### 원형 큐

- 초기 공백 상태 : front = rear = 0
- Index의 순환 : front와 rear의 위치가 배열의 마지막 인덱스인 n-1를 가리킨 후, 그 다음에는 논리적 순환을 이루어 배열의 처음 인덱스인 0으로 이동해야 함
  - 이를 위해 나머지 연산자 % (mod)를 이용함
- front 변수 : 공백 상태와 포화 상태 구분을 쉽게 하기 위해 front가 있는 자리는 사용하지 않고 항상 빈자리
  - 삽입 : rear = (rear + 1) % n
  - 삭제 : front = (front +1) % n

#### 내장함수 큐

```python
import queue

Q = queue.Queue()
Q.put(1)
Q.put(2)
Q.put(3)

while not Q.empty():
    print(Q.get())
```

#### 연결 큐

- 단순 연결 리스트를 이용한 큐
  - 큐의 원소 : 단순 연결 리스트의 노드
  - 큐의 원소 순서 : 노드의 연결 순서, 링크로 연결되어 있음
  - front : 첫 번째 노드를 가리키는 링크
  - rear : 마지막 노드를 가리키는 링크
- 상태표현 초기/공백: front = rear = None



#### 우선순위 큐

원소를 삽입하는 과정에서 우선순위를 비교하여 적절한 위치에 삽입하는 구조입니다. 가장 앞에 최고 우선순위의 원소가 위치하게 됩니다. 

- 버퍼(Biffer) : 데이터를 한 곳에서 다른 한 곳으로 전송하는 동안 일시적으로 데이터를 보관하는 메모리 영역





## BFS

그래프를 탐색하는 방법 (1) DFS (2) BFS, BFS 너비우선탐색은 탐색 시작점의 인접한 정점들을 먼저 모두 차례로 방문한 후에 방문했던 정점을 시작점으로 하여 다시 인접한 정점들을 차례로 방문하는 방식입니다. 인접한 정점들에 대해 탐색을 한 후 차례로 다시 너비우선탐색을 진행해야하므로, 선입선출 형태의 자료구조인 큐를 활용합니다.

```python
def BFS(G, v): # G : 그래프, v : 탐색 시작점
    visited = [0] * n 
    queue = []
    queue.append(v)
    while queue:
        t = queue.pop(0)
        if not visited[t]:
            visited[t] = True
            visit(t)
        for i in G[t]:
            if not visited[i]:
                queue.append(i)
    
```

연습문제3) 다음은 연결되어 있는 두 개의 정점 사이의 간선을 순서대로 나열 해 놓은 것이다. 모든 정점을 너비우선탐색하여 경로를 출력하시오. 시작 정점을 1로 시작하시오.

1, 2, 1, 3, 2, 4, ,2 ,5, 4, 6, 5, 6, 6, 7, 3, 7



