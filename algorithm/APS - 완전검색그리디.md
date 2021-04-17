[TOC]

# APS - 완전검색/그리디

> 2021.04.14.

### 반복과 재귀

- **반복**은 수행하는 작업이 완료될 때 까지 계속 반복

  - 초기화
  - 조건검사
  - 반복할 명령문 실행
  - 업데이트

  ```python
  # 반복을 이용한 선택정렬
  def selectionSort(A):
      n = len(A)
      for i in range(0, n-1):
          min = i
          for j in range(i+1, n):
              if A[j] < A[min]:
                  min = j
          A[min], A[i] = A[i], A[min]
  ```

- **재귀**는 주어진 문제의 해를 구하기 위해 동일하면서 더 작은 문제의 해를 이용하는 방법, 함수 호출은 프로그램 메모리 구조에서 스택을 사용합니다. 따라서 재귀 홍출은 반복적인 스택의 사용을 의미하며 메모리 및 속도에서 성능저하가 발생합니다. 지역변수가 호출되는 메모리 영역을 stack이라 부르게 됩니다. 

  - base rule (기본 부분)
  - inductive rule (유도 부분)

  ```python
  # 재귀를 이용한 선택정렬
  def sesctionSort(A):
      # base case
      if not len(A):
          return 
      # inductive case
      else:       
  
  ```

  ```python
  # 재귀를 이용한 팩토리얼
  def fact(n):
      if n < 2:
          return n
      else:
          return n * fact(n-1)
  ```

- 반복과 재귀 비교

  ```python
  # recursion
  def power_of_2(k):
      if k == 0:
          return 1
      else:
          return 2 * power_of_2(k-1)
  
  # iteration
  def power_of_2(k):
      i = 0
      power = 1
      while i < k:
          power = power * 2
          i = i + 1
     return power
  ```

- Baby-gin

  ```python
  """
  Baby gin Game Rule
   0에서 9사이의 숫자 카드에서 임의로 카드 6장을 뽑을 때
    - 3장의 카드가 연속적인 번호를 갖는 경우 run
    - 3장의 카드가 동일한 번호를 갖는 경우 triplete
  
  가능한 조합
  - run 2개
  - triplete 2개
  - run 1개 triplete 1개
  """
  
  # Baby Gin - permutation (recursion)
  
  # baby gin
  arr = [6, 6, 7, 7, 6, 7]
  # arr = [0, 5, 4, 0, 6, 0]
  
  # not baby gin
  # arr = [1, 2, 4, 7, 8, 3]
  # arr = [1, 0, 1, 1, 2, 3]
  
  
  ##############################################
  ##################기존문제풀이##################
  import sys
  sys.stdin = open('input.txt')
  
  T = int(input())
  
  for tc in range(1, T+1):
      cards = list(map(int, list(input())))
  
      counter = [0] * 10
      # 2가 되면 성공
      is_babygin = 0
  
      for card in cards:
          counter[card] += 1
  
      idx = 0
      while idx < len(counter):
          # triplet 검증
          if counter[idx] >= 3:
              is_babygin += 1
              counter[idx] -= 3
              continue
  
          # run 검증
          if idx < 8:
              if counter[idx] and counter[idx+1] and counter[idx+2]:
                  is_babygin += 1
                  counter[idx] -= 1
                  counter[idx+1] -= 1
                  counter[idx+2] -= 1
                  continue
          idx += 1
  
      if is_babygin == 2:
          result = 1
      else:
          result = 0
  
      print('#{} {}'.format(tc, result))
  
  
  ##############################################
  ##################재귀풀이###################
  def check(tree_arr):
      if tree_arr[0] == tree_arr[1] == tree_arr[2]:
          return 1
      if tree_arr[0] == tree_arr[1] -1 == tree_arr[2] - 2:
          return 1
      return 0
  
  
  def babygin(n, k):
      global found
      if k == n:
          print(arr)
          left = arr[:3]
          right = arr[3:]
          if check(left) + check(right) == 2:
              found = True
              return 
      else:
          for i in range(k ,n):
              arr[k], arr[i] = arr[i], arr[k]
              babygin(n, k + 1)
              arr[k], arr[i] = arr[i], arr[k]
              
  found = False
  arr = [6, 6, 7, 7, 6, 7]
  n = len(arr)
  babygin(n, 0)
  print(found)
  
          
  # 재귀 호출을 통한 순열 생성 
  # n : 원소의 개수, k : 현재까지 교환된 원소의 개수
  
  def perm(n, k):
      if k == n:
          print()
      else:
          for i in range(k ,n):
              arr[k], arr[i] = arr[i], arr[k]
              perm(n, k + 1)
              arr[k], arr[i] = arr[i], arr[k]
  ```

  



## 1. 문제 접근

### 1.1. Brute-Force (고지식한 방법)

- 문제를 해결하기 위한 간단하고 쉬운 접근법이다.
- force의 의미는 사람(지능)보다는 컴퓨터의 force를 의미한다.

### 1.2. 완전 검색

- 모든 경우의 수를 생성하고 테스트하기 때문에 수행 속도는 느리지만, 해답을 찾아내지 못할 확률이 작다.
- 우선 완전 검색으로 접근하여 해답을 도출한 후, 성능 개선을 위해 다른 알고리즘을 사용하고 해답을 확인하는 것이 바람직하다.

### 1.3. 순열 (Permutation)

- 서로 다른 것들 중 몇 개를 뽑아서 한 줄로 나열하는 것
- 순서화된 요소들의 집합에서 최선의 방법을 찾는 것과 관련 있음 : TSP (Traveling Salesman Problem)

#### 순열 생성하기

- 단순한 순열 생성
- 최소 변경을 통한 변경 (Johnson-Trotter)
- 재귀 호출을 통한 순열 생성
- 바이너리 카운팅을 통한 순열 생성

##### 순열

```python
# 1. for문
arr = ['A', 'B', 'C', 'D']
N = len(arr)

for i in range(0, N):
    arr[0], arr[i] = arr[i], arr[0]
    
    for j in range(1, N):
        arr[1], arr[i] = arr[i], arr[1]

        for k in range(2, N):
        	arr[2], arr[k] = arr[k], arr[2]
		    print(arr) # 바꾼 상태를 출력하고
	        arr[2], arr[k] = arr[k], arr[2]
            
        arr[1], arr[i] = arr[i], arr[1]
	    
    arr[0], arr[i] = arr[i], arr[0] # 원상 복귀!

    
# 2. 재귀호출
def perm(k):		# k: 함수 호출의 높이
    if k == N:
        print(arr)
    else:
        for i in range(k, N):
            arr[k], arr[i] = arr[i], arr[k]
            perm(k + 1)
            arr[k], arr[i] = arr[i], arr[k]

perm(0)
```

- 초기상태에서 상태공간 트리를 만들게 됩니다.
- 첫 번째 자리에 들어갈 수 있는 인자로 'A', 'B', 'C', 'D' 4가지가 있습니다.
  - idx요소로 자리를 교환한다고 표현하면 (0, 0) (0, 1) (0, 2) (0, 3) 이 됩니다.
  - 자리를 바꾼 값들이 그대로 저장되어 원복형태를 해치게 됩니다.
- A 다음에 두 번째 자리에 들어갈 수 있는 인자로 'B', 'C', 'D' 3가지가 있습니다.
  - (1, 1) (1, 2) (1, 3)

##### 최적 경로

- 문제 설명 : 출발지에서 출발하여 가장 짧은 경로로 모든 노드를 거치고 도착지에 도착하는 경로를 찾아라
- 이론 : 최적화 문제 + 완전 그래프
  - 조건을 만족하는 해(후보 해) 중에서 가장 최소가 되는 값을 찾으면 됩니다.
  - 최적화 문제는 우선적으로 `완전 검색`을 해야 합니다.
  - *TSP* (*Traveling Salesman Problem*) 이란 문자 그대로, 물건을 판매하기 위해 여행하는 세일즈맨의 문제입니다.
  - Force가 많이 필요합니다 → CPU 사용률이 높습니다. 
    - P : 쉬운 문제, big O 표현식이 다항식에 들어오는 문제라면 (컴퓨터에게) 쉬운 문제라고 분류합니다.
    - ~P : 어려운 문제, 지수 혹은 팩토리얼 계산식이 들어가면서 수많은 연산을 요구합니다.
- 컨셉 : 

```python
import sys; sys.stdin = open('05.txt', 'r')


def dist(arr):
    cnt = 0
    for i in range(N + 1, 0, -1):
        x = abs(arr[i][0] - arr[i-1][0])
        y = abs(arr[i][1] - arr[i-1][1])
        cnt += x + y
    return cnt

def perm(k):
    global min_dist
    if k == N + 1:
        tmp = dist(pos)
        if min_dist > tmp:
            min_dist = tmp

    else:
        for i in range(k, N + 1):
            pos[k], pos[i] = pos[i], pos[k]
            perm(k + 1)
            pos[k], pos[i] = pos[i], pos[k]



T = int(input())
for tc in range(1, T+1):
    N = int(input())
    G = [0] * (N + 2)
    visited = [0] * (N + 2)
    arr = list(map(int, input().split()))

    pos = [[arr[0], arr[1]]]
    for i in range(4, (N + 2) * 2, 2):
        pos.append([arr[i], arr[i + 1]])
    pos.append([arr[2], arr[3]])
    min_dist = 0xffffff

    perm(1)
    print('#{} {}'.format(tc, min_dist))


```

```python
# 단순한 순열 생성 {1, 2, 3}
for i1 in range(1, 4):
    for i2 in range(1, 4):
        if i2 != i2:
            for i3 in range(1, 4):
                if i3 != i1 and i3 != i2:
                    print(i1, i2, i3)
```

```python
# 최소 변경을 통한 방법

```

```python
# 재귀 호출을 통한 순열 생성
def perm(n, k):
    if k == n:
        print()
    else:
        for i in range(k ,n):
            arr[k], arr[i] = arr[i], arr[k]
            perm(n, k + 1)
            arr[k], arr[i] = arr[i], arr[k]
```

```python
# 바이너리 카운팅을 통한 순열 생성
n = len(arr)
for i in range(0, (1 << n)): # 1<<n : 부분집합의 개수
    for j in range(0, n):    # 원소의 수만큼 비트를 비교함
        if i & (1 << j):	 # i의 j번째 비트가 1이면 j번째 원소 출력
            print('%d' %arr[j], end=' ')
    print()
```





### 1.4. 조합 (Combination)

- 단순한 조합 nCr = n! / (n-r)! r!  (n >= r)
- 재귀를 통한 조합 nCr = n-1Cr-1 + n-1Cr
  - n개 중에서 r개를 조합하는 경우 = (포함) n-1개 중에서 r-1개 조합 + (미포함) n-1개 중에서 r개 조합
  - 3개 중에서 2개를 조합 = (포함) 2개 중에서 1개 조합 + (미포함) 2개 중에서 2개 조합

```python
# 재귀 호출을 이용한 조합 생성
# an[] : n개의 원소를 가지고 있는 배열
# tr[] : r개의 크기의 배열, 조합이 임시 저장될 배열

def comb(n, r):
    if r == 0:
        print(tr)
    else:
        tr[r-1] = an[n-1]
        comb(n-1, r-1)
        comb(n-1, r)

an = [1,2,3]
tr = [0] * 2

def comb2(i, n, r):
    if i == r:
        print(c)
    else:
        for j in range(i, n-r+i+1):
            c[i] = arr[j]
            comb2(i+2, n, r)
            
an = [1, 2, 3, 4, 5]
tr = [0] * 3
```

```python
# M개의 칸 중에서 3개를 고르는 경우 / 5 <= M <= 10
# i, j, k
# i : 0 -> M-3
# j : i+1 -> M-2
# k : j+1 -> M-1
M = 5
A = [1,2,3,4,5]
for i in range(0, M-3+1):
    for j in range(i+1, M-2+1):
        for k in range(j+1, M-1+1):
            print(A[i], A[j], A[k])
            print(i, j, k)
```

```python
def f(i, j, n, r):
    if i == r:
        print(c)
    else:
        for k in range(j, n-r+i+1):
            c[i] = A[k]
            f(i+1, k+1, n, r)
n = 6
r = 3
A = list(range(1,7))
c = [0] * r
f(0, 0, n, r)
```

### 1.5. 탐욕 알고리즘

- 해 선택 : 현재 상태에서 부분 문제의 최적 해를 구한 뒤, 이를 부분해집합에 추가
- 실행 가능성 검사 : 새로운 부분 해 집합이 실행가능한지를 확인
- 해 검사 : 새로운 부분 해 집합이 문제의 해가 되는지를 확인
- 하지만 탐욕 알고리즘은 최적해를 반드시 구한다는 보장이 없습니다.

```python
# 배낭 짐싸기
# 선택한 물건의 중량이 w를 넘지 않고, 최대의 값을 가지는 선택을 고르시오

```

- 0-1 Knapsack 
  - 넣거나(1) 빼거나(0) 두 개의 행동만 가능
  - 최적해를 구하기 어렵다
- Fractional Knapsack
  - 잘라서 DP

```python
# 회의실 배정하기

```

- 공집합이 아닌 하위 문제에 속한 활동 중 가장 빠른 활동 am
- 탐욕 기법을 적용한 반복 알고리즘
  - 종료 시간이 빠른 순서로 활동 정렬
  - 첫 번째 활동 A1 선택
  - 선택한 활동 A1의 종료시간보다 빠른 시작 시간을 가지는 활동을 모두 제거
  - 남은 활동들에 대해 앞의 과정을 반복

```python
# 1. sort하여 반복

# 2. recursive
def Recursive_Selection(i, j):
    m <- i + 1
    
    while m < j and Sm < Fm:
        m <- m + 1
        
    if m < j : return {Am} U Recursive_Selection(m, j)
    else : return {} # 공집합
```

##### 탐욕 알고리즘의 필수 요소

- 탐욕적 선택 속성
  - 탐욕적 선택은 최적해로 갈수 있음 -> 즉, 탐욕적 선택은 항상 안전하다
- 최적 부분 구조
  - 최적화 문제를 정형화하라 -> 하나의 선택을 하면 풀어야 할 하나의 하위 문제가 남는다
- [최적해 = 탐욕적 선택 + 하위 문제의 최적해]

##### 대표적인 탐욕 기법의 알고리즘

- Prim
- Kruskal
- Dijkstra
- Huffman tree & code
