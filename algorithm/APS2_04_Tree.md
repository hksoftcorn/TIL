# APS - Tree

### 복습

##### 서브트리

```python
# 5174_subtree
# 순회를 합니다
def traverse(node):
    global cnt
    cnt += 1
    childs = tree[node]
    for new_node in childs:
        traverse(new_node)

T = int(input())
for tc in range(1, T+1):
    E, root_node = map(int, input().split())
    info = list(map(int. input().split()))
    # 노드의 개수 : E + 1
    # 0을 제외한 번호로 시작하므로, E + 2
    tree = [[] for _ in range(E+2)]
    
    # 트리를 표현합니다.
    for i in range(E):
        parent, child = info[i * 2 : (i + 1) * 2]
        tree[parent].append(child)
    cnt = 0
    traverse(root_node)
    print(f'#{tc} {cnt}')

"""
tree = [
    [],
    [6],
    [1, 5],
    [],
    [],
    [3],
    [4]
]
"""
```

##### 사칙연산

중위순회 inorder 방법을 택합니다. 특정 노드에 계산한 값을 return 하게 됩니다. 

```python
T = 10
for tc in range(1, T+1):
    V = int(input())
    """이진트리
    tree = [
    0:	[0, 0, 0]
    1:	['*', 2, 3]
    2:	['/', 4, 5]
    3:	[3, 0, 0]
    4:	[9, 0, 0]
    5:	['-', 6, 7]
    6:	[6, 0, 0]
    7:	[4, 0, 0]
    ]
    """
    # 1. 트리 만들기
    tree = [[0 for _ in range(3)] for _ in range(N+1)]
    for _ in range(N):
        # [node_no, data (, left, right)]
        # 연산자 노드는 4개의 데이터를 받습니다.
        input_data = list(input().split())
        node_no = int(input_data[0])
        # 연산자 노드라면
        if len(input_data) == 4:
            # tree에 data 입력
            tree[node_no][0] = input_data[1]
            tree[node_no][1] = int(input_data[2])
            tree[node_no][2] = int(input_data[3])
        # 숫자라면
        else:
            tree[node_no][0] = int(input_data[1])
    
    # 2. 중위순회
	def inorder(node):
        data = tree[node][0]
        if data: # i.g. inorder(6) -> inorder(0), inorder(0) = 0 반환합니다.
            left, right = tree[node][1], tree[node][2]
            if data == '+':
				return inorder(left) + inorder(right)
            elif data == '-':
				return inorder(left) - inorder(right)
            elif data == '/':
				return inorder(left) / inorder(right)
            elif data == '*':
				return inorder(left) * inorder(right)
            else:
                return data
    
	print(f'#{tc} {int(inorder(1))}')   
```

##### 공통조상

class Node, Tree 구현하여 Tree => Node 들의 합으로 구성합니다.

```python
# tree : 1개
# node : 13개
# Node : pk, parent, child1, child2
# Tree : Node * n
""" i.g.
Node 
본인 | 	1	   |
p	| 	None	|
c	| c1  |  c2 |
"""

class Node:
    def __init__(self, pk, parent=None, child=None):
        self.pk =pk
        self.parent = parent
        self.child1 = child1
        self.child2 = None
        
    def add_child(self, pk):
        pass
    
    def add_child(self, pk):
        pass

    
class Tree:
    def __init__(self, info):
           
```









