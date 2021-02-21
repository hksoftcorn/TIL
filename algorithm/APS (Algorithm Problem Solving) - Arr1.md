[TOC]

# APS (Algorithm Problem Solving) - Array1

알고리즘이란 (유한한 단계를 통해) 문제를 해결하기 위한 절차. 컴퓨터가 어떤 일을 수행하기 위한 단계적인 방법을 말합니다. `슈더코드`와 `순서도`를 이용하여 알고리즘을 표현합니다.

좋은 알고리즘이란, ① 정확성 ② 작업량 ③ 메모리 사용량 ④ 단순성 ⑤ 최적성을 만족하는 풀이를 말합니다. 이 중 알고리즘의 작업량을 표현할 때 `시간복잡도`로 표현합니다. 실제 걸리는 시간을 측정, 실행되는 명령문 수를 측정하거나 수학적으로 빅-오 표기법(Big-Oh Notation)으로 표현하기도 합니다.



## 배열 1

대표적인 정렬 방식의 종류에는 ① 버블 정렬 ② 카운팅 정렬 ③ 선택 정렬 ④ 퀵 정렬 ⑤ 삽입 정렬 ⑥ 병합 정렬이 있습니다. 

### 버블 정렬

인접한 두 개의 원소를 비교하며 자리를 계속 교환하는 방식입니다. 한 단계가 끝나면 가장 큰 원소가 마지막 자리로 정렬됩니다. `O(N^2)`

```python
def BubbleSort(a):
    for i in range(len(a)-1, 0, -1): # 범위 끝 위치
        for j in range(0, i):
            if a[j] > a[j+1]:
                a[j], a[j+1] = a[j+1], a[j]
```

### 카운팅 정렬

항목들의 순서를 결정하기 위해 집합에 각 항목이 몇 개씩 있는지 세는 작업을 하여, 선형 시간에 정렬하는 효율적인 알고리즘입니다. 카운팅 정렬의 제한 사항은 **정수**나 정수로 표현할 수 있는 자료에 대해서만 적용 가능하며, 카운트들을 위한 충분한 공간을 확보하기 위해 **집합 내의 가장 큰 정수**를 알아야 한다는 조건이 있습니다.  `O(n + k)` : n은 리스트 길이, k는 정수의 최댓값

i.g. [0, 4, 1, 3, 1, 2, 4, 1]  k = 4

count = [0] * (4+1) 

count = [1, 3, 1, 1, 2]  # 각 숫자 개수 

count = [1, 4, 5, 6, 8]  # 누적합

tmp에 Data 뒤에서부터 값을 찾아서 삽입합니다.

```python
def Counting_Sort(A, B, k):
    # A [] : 입력 배열 (1 to k), k는 최대 정수
    # B [] : 정렬할 배열
    # C [] : 카운트 배열
    C = [0] * k
    for i in range(len(B)):  # 
        C[A[i]] += 1
    for i in range(1, len(C)): # 누적합
        C[i] += C[i-1]
    for i in range(len(B)-1, -1, -1):
        B[C[A[i]]-1] = A[i]
        C[A[i]] -= 1
    
```

### 완전 검색

완전 검색 방법(Exhaustive Search) = Brute-force = Generate-and-Test

문제의 해법으로 생각할 수 있는 모든 경우의 수를 나열해보고 확인하는 기법입니다. 모든 경우의 수를 테스트한 후, 최종 해법을 도출합니다. 일반적으로 경우의 수가 상대적으로 작을 때 유용합니다.  완전 검색으로 시작하여 해답을 도출한 후, 성능 개선을 위해 다른 알고리즘을 사용하고 해답을 확인하는 것이 바람직합니다. 

```python
# 순열 3P3 = (1, 2, 3) (1, 3, 2) (2, 1, 3) (2, 3, 1) (3, 1, 2) (3, 2, 1)
for i1 in range(1, 4):
    for i2 in range(1, 4):
        if i2 != i1:
            for i3 in range(1, 4):
                if i3 != i1 and i3 != i2:
                    print(i1, i2, i3)
```

### 그리디

탐욕 알고리즘(Greedy)은 최적해를 구하는 데 사용되는 근시안적인 방법을 말합니다.

여러 경우 중 하나를 결정해야 할 때마다 그 순간에 최적이라고 생각되는 것을 선택해나가는 방식으로 진행하여 최종적인ㄴ 해답에 도달합니다. 각 선택의 시점에서 이루어지는 결정은 지역적으로는 최적이지만, 그 선택들을 계속 수집하여 최종적인 해답을 만들었다고 하여, 그것이 최적이라는 보장은 없습니다.

1) 해 선택 : 현재 상태에서 부분 문제의 최적 해를 구합니다.

2) 실행 가능성 검사 : 문제의 제약 조건을 위반하지 않는지를 검사합니다.

3) 해 검사 : 새로운 부분해 집합이 문제의 해가 되는지를 확인합니다.



## 문제풀이 1

### min max

```python
max_value = 0
min_value = number[0]
for i in range(N):
    if max_value < number[i]:
        max_value = number[i]
    if min_value > number[i]:
        min_value = number[i]
```

### 전기버스

```python
for tc in range(1, T+1):
    K, N, M = map(int, input().split())
    charge = list(map(int, input().split()))
    bus_stop = [0] * (N+1)
    
    for i in charge: # 버스 충전소 
        bus_stop[i] = 1
        
	bus = 0
    ans = 0
    
    while True:
        # 버스 이동
        bus += K
        if bus >= N : break # 종점 도착
        for i in range(bus, bus-K, -1):
            if bus_stop[i]:
                ans += 1
                bus = i
                break
        else:
            ans = 0
            break
    
    print('#{} {}'.format(tc, ans))
```

```python
for tc in range(1, T+1):
    K, N, M = map(int, input().split())
    charge = list(map(int, input().split()))
    charge = [0] + charge + [N]
	ans = 0
    last = 0
    
    for i in range(1, M+2): # 충전소 사이 거리
        if charge[i] - charge[i-1] > K:
            ans = 0
            break
        if charge[i] > last + K:
            last = charge[i-1]
            ans += 1

    print('#{} {}'.format(tc, ans))
```

### 구간 합

```python
for tc in range(1, T+1):
    N, M = map(int, input().split())
    nums = list(map(int, input().split()))
    max_value = 0
    min_value = 987654321
    
    for i in range(N - M + 1):
        tmp = 0
        for j in range(M):
            tmp += nums[i + j]
         if max_value < tmp:
            max_value = tmp
         if min_value > tmp:
            min_value = tmp
            
    print('#{} {}'.format(tc, max_value - min_value))
```

```python
for tc in range(1, T+1):
    N, M = map(int, input().split())
    nums = list(map(int, input().split()))
    
    tmp = 0
    # 첫 구간 합
    for i in range(M):
        tmp += nums[i]
        
    max_value = tmp
    min_value = tmp
    
    for i in range(M, N):
        # 새로운 구간의 합 = 첫 원소 빼기, 새로운 원소 더하기
        tmp = tmp + nums[i] - nums[i - M]
    print('#{} {}'.format(tc, max_value - min_value))
```

### 숫자 카드

```python
for tc in range(1, T+1):
    N = int(input())
    
    card = input()
    
    count = [0] * 10
    max_count = -1
    max_num = -1
    
    for num in card: # 카드 숫자 세기
        count[int(i)] += 1
        # if max_count < count[int(i)]:
        #     max_count = count[int(i)]
    for i in range(len(count)):   
        if max_count <= count[i]: # <= 부호에 대한 간단한 팁 : 뒤에 나오는 큰 수가 담긴다. ()
            max_num = i
            max_count = count[i]
    print('#{} {}'.format(tc, max_num, max_count))
```

### Flatten

```python
def min_search():
    # 초기화
    min_value = 987654321
    min_idx = -1
    
    for i in range(len((box))):
        if box[i] < min_value:
            min_value = box[i]
            min_idx = i
        return min_idx
    
def max_search():
    max_value = 0
    max_idx = -1
    
    for i in range(len(box)):
        if box[i] > max_value:
            max_value = box[i]
            max_idx = i
        return max_idx
    

for tc in range(1, 10+1):
    N = int(input())
    box = list(map(int, input().split()))
    
    for i in range(N):
        box[meax_search()] -= 1
        box[min_search()] += 1
        
    print('#{} {}'.format(tc, box[max_search] - box[min_search()]))
```

```python
for tc in range(1, 10+1):
    N = int(input())
    box = list(map(int, input().split()))
    
    h_cnt = [0] * 101
    
    # 값 초기화
    min_value = 101
    max_value = 1
    
    for i in range(100):
        h_cnt[box[i]] += 1
        if box[i] > max_value:
            max_value = box[i]
        if box[i] < min_value:
            min_value = box[i]
    
    while N > 0 and min_vlaue < max_value-1:
        box[min_value] -= 1
        box[min_value + 1] += 1
        
        box[max_value] -= 1
        box[max_value - 1] += 1
    	
        if box[min_value] == 0:
            min_value += 1
        if box[max_value] == 0:
            max_value -= 1
            
        N -= 1
        
    print('#{} {}'.format(tc, max_value - min_value))
```

### 두 개의 숫자 열

```python
def check(long, short):
    max_value = -987654321
    for i in range(len(long) - len(short) + 1):
        for j in range(len(short)):
            result += long[i+j]*short[j]
        if max_vlaue < result:
            max_value = result
    return max_value
        

for tc in range(1, int(input()) + 1):
    N, M = map(int, input().split())
    
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))
    
    if N > M:
        ans = check(A, B)
    else:
        ans = check(B, A)
    print('#{} {}'.format(tc, ans))
```

### 삼성시의 버스노선

 ```python
for tc in range(1, 10+1):
    N = int(input())
    bus_stop = [0] * 5001
    
    for i in range(N):
        A, B = map(int, input().split())
        
        for j in range(A, B+1):
            bus_stop[j] += 1
    P = int(input())
    
    print('#{}'.format(tc), end=" ")
    for i in range(P):
        C = int(input())
        print(bus_stop[C], end=" ")
    print()
 ```

### 간단한 소인수분해

```python
for tc in range(1, int(input())+1):
    N = int(input())
    
    prime = [2,3,5,7,11]
    cnt = [0] * 5
    
    for i in range(len(prime)):
        while N % prime[i] == 0:
            cnt[i] += 1
            N //= print[i]
            
    print('#{} {}'.format(tc, " ".join(map(str, cnt))))
```

