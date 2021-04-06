# TREE_02

> 2021.04.06

- **문제 사이트** : 
  - [ ] 백준
  - [ ] Programmers
  - [x] SWEA
    - [x] 1232_사칙연산
    - [x] 5174_subtree
    - [x] 5176_이진탐색
    - [x] 5177_이진힙
    - [x] 5178_노드의 합
- **사용한 개념/알고리즘**
  - 트리 / 이진트리
  - 수식트리
- 이진 탐색 트리
  - 이진 힙
- **어려웠던 점**
  - 트리 자체의 개념이 어려웠다.
  - 재귀로 표현하는 방법은 더더욱 어려웠다.
  - 이게 어떻게 되지? 하는게 많기는 했는데,, 복습을 해야겠다.
- **뿌듯한 점 & 개선할 점**
  - 새벽 1시까지,, 혼자만의 힘으로 풀었다.
  - 문제를 잘 읽고, 그림을 먼저 그려보자.
  - 코드 리뷰를 생활화 합시다.
  - 동기들의 코드를 보면서 좋은 코드는 배우자.

```python
# 1232_사칙연산
T = 10
for tc in range(1, T+1):
    V = int(input())

    # 2차원 배열로 입력을 받아줍니다.
    # 0 : 왼쪽 자식
    # 1 : 오른쪽 자식
    # 2 : 노드 데이터
    tree = [[0 for _ in range(3)] for _ in range(V+1)]
    for _ in range(V):
        input_data = input().split()
        node_no = int(input_data[0])
        node_data = input_data[1]

        tree[node_no][2] = node_data
        ans = []

        if len(input_data) == 4:
            tree[node_no][0] = int(input_data[2])
            tree[node_no][1] = int(input_data[3])
        elif len(input_data) == 3:
            tree[node_no][0] = int(input_data[2])

    # 연산자를 구현합니다.
    def calc(a, b, x):
        if x == '+':
            return b + a
        elif x == '-':
            return b - a
        elif x == '*':
            return b * a
        elif x == '/':
            return b / a

    # 스택으로 풀어봅니다.
    stack = []
    def solution(node_no):
        # 후위순회로 풀어야하나?? ㅇㅋ
        if tree[node_no][2]:
            solution(tree[node_no][0])
            solution(tree[node_no][1])

            if tree[node_no][2].isnumeric():
                stack.append(tree[node_no][2])
            else:
                a = stack.pop()
                b = stack.pop()
                # 정수형이 아닌 실수형으로 지정해줍니다.
                tmp = calc(float(a), float(b), tree[node_no][2])
                stack.append(tmp)


    solution(1)
    print('#{} {}'.format(tc, int(*stack)))
```

```python
# 5174_subtree
T = int(input())

for tc in range(1, T + 1):
    E, N = map(int, input().split())
    V = E + 1
    edge = list(map(int, input().split())) # p, c

    left = [0] * (V + 1)
    right = [0] * (V + 1)

    pa = [0] * (V + 1)

    for i in range(E):
        n1, n2 = edge[i * 2], edge[i * 2 + 1]  # n1 부모노드, n2 자식노드
        if left[n1] == 0:
            left[n1] = n2
        else:
            right[n1] = n2
        pa[n2] = n1

    cnt = 0

    def solution(n):
        global cnt
        if n == 0:
            return
        else:
            cnt += 1
            solution(left[n])
            solution(right[n])


    solution(N)
    print('#{} {}'.format(tc, cnt))
```

```python
# 5176_이진탐색
T = int(input())

def inorder(node_no):
    global cnt
    if node_no > 0:
        # 왼쪽 탐색
        inorder(tree[node_no][0])
        cnt += 1
        tree[node_no][2] = cnt
        # 오른쪽 탐색
        inorder(tree[node_no][1])


for tc in range(1, T+1):
    V = int(input())
    tree = [[0] * 3 for _ in range(V + 1)]

    for node_no in range(1, V+1):
        # node_no * 2 가 마지막 노드번호(V)보다 작다 == 왼쪽 자식이 있다.
        if node_no * 2 <= V:
            tree[node_no][0] = 2 * node_no
            # node_no * 2 + 1 이 마지막 노드번호(V)보다 작다 == 오른쪽 자식이 있다.
            if node_no * 2 + 1 <= V:
                tree[node_no][1] = 2 * node_no + 1
    cnt = 0
    inorder(1)
    
    print('#{} {} {}'.format(tc, tree[1][2], tree[V//2][2]))
```

```python
# 5177_이진힙
def solution(node_no):
    global tree
    if node_no == 1:
        return
    if tree[node_no // 2][2] > tree[node_no][2]:
        tree[node_no // 2][2], tree[node_no][2] = tree[node_no][2], tree[node_no // 2][2]
        solution(node_no // 2)


T = int(input())

for tc in range(1, T + 1):
    V = int(input())
    tree = [[0] * 3 for _ in range(V + 1)]
    input_data = list(map(int, input().split()))  # 서로 다른 자연수

    for node_no, node_data in enumerate(input_data, start=1):
        tree[node_no][2] = node_data  # 현재 들어온 데이터
        solution(node_no)

    ans = 0
    while V > 1:
        V //= 2
        ans += tree[V][2]

    print('#{} {}'.format(tc, ans))
```

```python
# 5178_노드의 합
T = int(input())

for tc in range(1, T+1):
    N, M, L = map(int, input().split())
    ans = 0
    if L == 1:
        for _ in range(M):
            node_no, node_data = map(int, input().split())
            ans += node_data
    else:
        for _ in range(M):
            node_no, node_data = map(int, input().split())
            while node_no >= L:
                if node_no == L:
                    ans += node_data
                    break
                node_no //= 2

    print('#{} {}'.format(tc, ans))
```