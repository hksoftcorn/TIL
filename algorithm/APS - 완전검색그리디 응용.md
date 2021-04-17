# APS - 완전검색/그리디 응용

> 2021.04.16.

- 문제풀이

  > - [x] 5188 최소합
  > - [x] 5189 전자카트
  > - [x] 5201 컨테이너 운반
  > - [x] 5202 화물도크
  > - [x] 5203 베이비진 게임
  > - [x] 4366 정식이의 은행업무
  > - [x] 2819 격자판의 숫자 이어 붙이기
  > - [ ] 1861 정사각형 방
  > - [ ] 1970 쉬운 거스름돈
  > - [ ] 1486 장훈이의 높은 선반

- **사용** **알고리즘**
  
  - Brute Force / Greedy / Permutation / Combination

## 5188 최소합

##### 접근 방법1 : RECURSIVE

- 컨셉 : 1) (0, 0) 에서 출발하여 가능한 모든 경로로 이동합니다. 이동방법은 오른쪽 혹은 아래만 이동합니다. 2) 각 칸을 지날 때마다 숫자를 더합니다.

```python
# recursive
move(i, j, s, N): # i, j 진입한 칸의 좌표, s는 지나온 칸의 합
    if i == N-1 and j == N-1: # 도착한 경우
    elif i == N or j == N: # 경계를 벗어난 경우 
        return
    else:
        move(i + 1, j, s +A[i][j], N) # 아래이동
        move(i, j + 1, s +A[i][j], N) # 오른쪽이동
```

##### 접근 방법2 : BFS

- 컨셉 : 1) 최소합을 지정합니다. 2) `D[i][j] = min(D[i-1][j], D[i][j-1]) + A[i][j]` 왼쪽, 위쪽칸에서 minimum 값을 찾아서 현재 위치의 값을 더해줍니다.

```python

```



## 5189 전자카트

##### 접근 방법1 : PERMUTATION

```python
def permutation(i, n):
    global minV
    if i == n:
        s = 0
        for j in range(1, n):
            s += arr[p[j-1]][p[j]]
        s += arr[p[n-1]][0]
        if minV > s:
            minV = s
	else:
        for j in range(n):
            if v[j] == 0:
                v[j] = 1
                p[i] = j
                permutation(i + 1, n)
                v[j] = 0


T = int(input())
for tc in range(1, T+ 1):
    N = int(input())
    arr = [list(map(int, input().split()) for _ in range(N))]
    p = [0] * N
    v= [0] * N
    v[0] = 1
    minV = 0xfffffff
    permutation(1, N)
```

##### 접근 방법2 : 최소값 비교

```python
def permutation(i, n, s): # s: 이전 방문지까지의 비용
    global minV
    if i == n:
        s += arr[p[n-1]][0]
        if minV > s:  
            minV = s
    elif minV <= s: # elif문에 추가하여 함수호출 횟수를 줄일 수 있습니다.
        return 
	else:
        for j in range(n):
            if v[j] == 0:
                v[j] = 1
                p[i] = j
                permutation(i + 1, n, s+arr[p[i-1]][j])
                v[j] = 0


T = int(input())
for tc in range(1, T+ 1):
    N = int(input())
    arr = [list(map(int, input().split()) for _ in range(N))]
    p = [0] * N
    v = [0] * N
    v[0] = 1
    minV = 0xfffffff
    permutation(1, N)


```

##### 참고

```python
def f(i, n, k):
    if i == k:
        print(p)
    else:
        for j in range(n):
            if u[j] == 0:
                u[j] = 1
                p[i] = j  # p[i]의 숫자를 정함
                f(i+1, n, k) # p[i+1] 숫자를 정하러 이동
                u[j] = 0 # j를 다른 자리에도 쓸 수 있도록 초기화
                
N = 5 # 5개
K = 3 # 3개 선택
p = [0] * K
u = [0] * N
f(0, N, K)
```



## 5201 컨테이너 운반

##### 접근방법1

- 컨셉 : 1) 화물의 무게를 기준으로 내림차순 정렬 2) 트럭이 가능한 가장 무거운 화물을 옮긴다 3) 모든 트럭에 대해 (2) 반복 4) 옮겨진 총 무게를 출력한다.

```python
def f(N, M):
    moved = [0] * N
    s = 0
    for i in range(M):
        for j in range(N):
            if moved[j] == 0 and t[i] >= w[j]:
                s += w[j]
                moved[j] = 1
                break
	return s
                

T = int(input())
for tc in range(1, T+ 1):
    N, M = map(int, input().split())
    w = list(map(int, input().split()))
    t = list(map(int, input().split()))
    w.sort(reverse=True)
    f
```

##### 접근방법2

- 컨셉 : 화물과 트럭을 크기순으로 내림차순 정렬합니다.

```python
T = int(input())
for tc in range(1, T+ 1):
    N, M = map(int, input().split())
    w = list(map(int, input().split()))
    t = list(map(int, input().split()))
    w.sort(reverse=True)
    t.sort(reverse=True)
    i = 0
    j = 0
    s = 0
    while i < M and j < N:
        if t[i] >= w[j]:
            s += w[j]
            i += 1
            j += 1
        else:
            j += 1
```



## 5202 화물 도크

```python
T = int(input())
for tc in range(1, T+1):
    N = int(input())
    arr = [list(map(int, input().split())) for i in range(N)]
    arr.sort(ket=lambda x : x[1])
    cnt = 0
    end = -1
    for i in range(N):
        if end <= arr[i][0]:
	        cnt += 1
            end = arr[i][1]
```



## 5203 베이비진 게임

```python
def game():
    count1 = list(range(10))
    count2 = list(range(10))
    for i in range(12):
        n = card[i]
        if i % 2 == 0:
            count1[n] += 1
            if count1[n] == 3:
                return 1
            if run(count1):
                return 1
        else:
            count2[n] += 1
            if count2[n] == 3:
                return 2
            if run(count2):
                return 2
   return 0

def run(count):
    for i in range(8):
        if count[i] >= 1 and count[i+1] >= 1 and count[i+2] >= 1:
            return 1

```



## 4366 정식이의 은행업무

- 컨셉
  - 2진수 : 각 한자리씩 자리를 바꾸어(0, 1) 목록을 만들어 줍니다.
  - 3진수 : 각 한자리씩 자리를 바꾸어(0, 1, 2) 목록을 만들어 줍니다.

```python
import sys; sys.stdin = open('sample_input.txt', 'r')
"""
풀이법:
1. 가능한 2진수 경우를 구합니다.
2. 2진수를 10진수로 바꾸어 set에 담습니다.

3. 가능한 3진수 경우를 구합니다.
4. 3진수를 10진수로 바꾸어 set에 담습니다.

5. 두개의 set에서 중복되는 수를 출력합니다.
"""
def change_binary(c): # c: char
    if c == '0':
        return '1'
    else:
        return '0'

def b_to_d(b: str): # b: binary
    ans = 0
    for i in range(n):
        ans += int(b[n -1 - i]) * (2 ** i)
    return ans    

def t_to_d(t: str): # t: ternary
    ans = 0
    for j in range(m):
        ans += int(t[m -1 - j]) * (3 ** j)
    return ans


T = int(input())
for tc in range(1, T+1):
    binary = list(input())
    ternary = list(input())
    n = len(binary)
    m = len(ternary)


    # 1. 가능한 2진수 경우를 구합니다.
    binary_list = []
    for i in range(n):
        b = change_binary(binary[i])
        tmp = binary[:]
        tmp[i] = b
        binary_list.append(tmp)

    # 2. 2진수를 10진수로 바꾸어 set에 담습니다.
    binary_to_decimal = set()
    for bl in binary_list:
        b = ''.join(bl)
        binary_to_decimal.add(b_to_d(b))

    # 3. 가능한 3진수 경우를 구합니다.
    ternary_list = []
    for j in range(m):
        if ternary[j] == '0':
            tmp = ['1', '2']
        elif ternary[j] == '1':
            tmp = ['0', '2']
        else:
            tmp = ['0', '1']

        for t in tmp:
            arr = ternary[:]
            arr[j] = t
            ternary_list.append(arr)

    # 4. 3진수를 10진수로 바꾸어 set에 담습니다.
    ternary_to_decimal = set()
    for tl in ternary_list:
        t = ''.join(tl)
        ternary_to_decimal.add(t_to_d(t))

    result = binary_to_decimal & ternary_to_decimal
    print('#{} {}'.format(tc, *result))
```

##### 다른 풀이법 XOR

```python
# concept
# binary
b = 0
for x in bit:
    b = b * 2 +int(x)
    
for i in range(len(bit)):
    binary.append(b ^ (1 << i))

# ternary
num = 0
for x in t:
    num = num * 3 + int(x)
    
num1 = 0
num2 = 0
for i in range(len(t)):
    for j in range(len(t)):
        if i == j:
            num1 = num1 * 3 + (int(t[j]) + 1) % 3
            num2 = num2 * 3 + (int(t[j]) + 2) % 3
            
(아래 코드 참고)
```

```python
def f(b, t):
    # bint = int(b, 2)
    bint = 0
    for x in b:
        bint = bint * 2 + int(x)
    binary = []
    for i in range(len(b)):
        binary.append(bint ^ (1 << i)) # 2진수의 1비트씩을 바꿔서 저장
        
    for i in range(len(t)): # 3진수에서 다른 두 수로 바꿔볼 자리
        num1 = 0
        num2 = 0
        for j in range(len(t)):
            if i != j:
                num1 = num1 * 3 + int(t[j])
                num2 = num2 * 3 + int(t[j])
            else:
                num1 = num1 * 3 + (int(t[j]) + 1) % 3	# 0 -> 1 / 1 -> 2 / 2 -> 0
                num2 = num2 * 3 + (int(t[j]) + 2) % 3	# 0 -> 2 / 1 -> 0 / 2 -> 1
                
        if num1 in binary:
            return num1
        if num2 in binary:
            return num2   

T = int(input())
for tc in range(1 T+1):
    b = input()
    t = input()
    r = f(b, t)
    
```



## 2819 격자판의 숫자 이어붙이기

- 컨셉
  - 배열에 저장된 개별 숫자를 정수로 바꾸기

```python
# case1
di = [0, 1, 0, -1]
dj = [1, 0, -1, 0]
for k in range(4):
    ni = i + di[k]
    nj = j + dj[k]
    if 0 <= ni < 4 and 0 <= nk < 4:
        f(n+1, ni, n)

        
# case2      
t = set()
def f(n, i, j)  # raw-concept
if n == 7:
    num = 0
    for i in range(7):
        num = num * 10 + s[i]
    t.add(num)
else:
    s[n] = A[i][j]
    f(n+1, i, j+1)
    f(n+1, i+1, j+1)
    f(n+1, i, j-1)
    f(n+1, i-1, j)
```

```python
import sys; sys.stdin = open('sample_input.txt', 'r')

dr = [-1, 1, 0, 0]
dc = [0, 0, -1, 1]
def f(r, c, n, s):
    if n == 7:
        t.add(s)
    else:
        for i in range(4):
            nr = r + dr[i]
            nc = c + dc[i]
            if 0 <= nr < 4 and 0 <= nc < 4:
                f(nr, nc, n + 1, s + str(arr[nr][nc]))
                
T = int(input())
for tc in range(1, T+1):
    arr = [list(map(int, input().split())) for _ in range(4)]
    t = set()
    for r in range(4):
        for c in range(4):
            f(r, c, 0, '')  # r, c : 현재 좌표 / n = 이동횟수 / '' = 문자열
    print('#{} {}'.format(tc, len(t)))
```





1861 정사각형 방

- 문제 설명 : 상하좌우 다른 방으로 이동할 떄, 현재 방에 적힌 숫자보다 1이 커야 한다. 처음 어떤 수가 적힌 방에서 있어야 가장 많은 개수의 방을 이동할 수 있는지 구하는 프로그램을 작성하라. 단, 이동할 수 있는 방의 개수가 최대인 방이 여럿이라면 그 중에서 적힌 수가 가장 작은 것을 출력한다.
- 컨셉 : 



1970 쉬운 거스름돈

- 문제 설명 : 돈의 최소 개수로 거슬러 주기 위하여 각 종류의 돈이 몇 개씩 필요한지 출력하라.
- 컨셉 : 



1486 장훈이의 높은 선반

- 문제 설명 : 
- 컨셉 : 

```python
# 모든 조합의 경우를 구하자
def comb(n, r):
    if r == 0:
        print(tr)
    else:
        tr[r-1] = an[n-1]
        comb(n-1, r-1)
        comb(n-1, r)


T = int(input())
for tc in range(1, T+1):
    N, B = map(int, input().split())
    H = list(map(int, input().split()))
    
```



## 11454

```python
# 3C2
for i in range(N-1):
    for j in range(i+1, N):
        print(arr[i], arr[j])
        
# 5C3
for i in range(N-2):
    for j in range(i+1, N-1):
        for j in range(j+1, N):
	        print(arr[i], arr[j], arr[k])
```

```python
# baby-gin
arr = [3, 2, 8, 8, 8]
N = len(arr)

uniq = set()
for i in range(N-1):
    for j in range(i+1, N):
        arr[i], arr[j] = arr[j], arr[i] # 1회 교환
        
        for a in range(N-1):
            for b in range(a+1, N):
                arr[a], arr[b] = arr[b], arr[a] # 2회 교환
                print(arr)
                arr[a], arr[b] = arr[b], arr[a] # 원상복구
                uniq.add(int(''.join(map(str, arr))))
        
        arr[i], arr[j] = arr[j], arr[i] # 원상복구
        
```

```python
def backtrack(k):
    global ans, N
    val = int(''.join(cards))
    if val in visited[k]: return
    visited[k].add(val)
    
    # 최종 교환 완료라면, 최댓값 갱신
    if k == cnt:
        ans = max(ans, val)
    else:
        for i in range(N-1):
            for j in range(i+1, N):
                cards[i], cards[j] = cards[j], cards[i]
		        backtrack(k+1)
                cards[i], cards[j] = cards[j], cards[i]

T = int(input())
for tc in range(1, T+1):
    input_data = list(input().split())
    cards, cnt = list(input_data[0]), int(input_data[1])
    N = len(cards)
    visited = [set() for _ in range(11)]
    ans = 0
    backtrack(o) # 0번 부터 시작
    print()
```

```python
w = [10 ** i for i in range(6)]

def swap(n, i, j):
    # 123 -> 1, 2
    a = (n // w[i]) % 10
    b = (n // w[j]) % 10
    return n - (a * w[i]) + (a * w[j]) - (b * w[j]) + (b * w[i])

T = int(input())
for tc in range(1, T+1):
    num, cnt = input().split()
    N = len(num)
    num, cnt = int(num), int(cnt)
    visited = [[] for _ in range(12)]
    ans = 0
    dq = deque()
    dq.append((num, 0))
    
    while dq:
        n, k = dq.popleft()
        if k == cnt:
            ans = max(ans, n)
        else:
            for i in range(N-1):
                for j in range(i+1, N):
                    val = swap(n, i, j)
                    if val in visited[k + 1]: continue
                    visited[k+1].append(val)
                    dq.append((val, k+1))

```

































