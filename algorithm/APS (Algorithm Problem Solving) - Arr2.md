[TOC]

# APS (Algorithm Problem Solving) - Array2

좋은 알고리즘이란, ① 정확성 ② 작업량 ③ 메모리 사용량 ④ 단순성 ⑤ 최적성을 만족하는 풀이를 말합니다. 이 중 알고리즘의 작업량을 표현할 때 `시간복잡도`로 표현합니다. 실제 걸리는 시간을 측정, 실행되는 명령문 수를 측정하거나 수학적으로 빅-오 표기법(Big-Oh Notation)으로 표현하기도 합니다.

## 배열 2

### 2차 배열

입력방법

```python
M, N = map(int, input().split())
arr = []

#1
for i in range(N):
    arr.append(list(map(int, input().split())))
#2    
arr = [0] * N
for i in range(N):
    arr[i] = list(map(int, input().split()))
#3  
arr = [list(map(int, input().split())) for _ in range(N)] 
```

### ✔ 부분집합(Subset) 생성

```python
arr = [3, 6, 7, 1, 5, 4]
n = len(arr) # 6

for i in range(1<<n): # range(64) 100000
    for j in range(n): # (0, 1, 2, 3, 4, 5)
        if i & (1<<j): #
            print(arr[j], end=', ')
        print()
    print()
```

### 순차 검색 & 바이너리 서치

검색이란 저장되어 있는 자료 중에서 원하는 항목을 찾는 작업을 말합니다.  `순차 검색`은 일렬로 되어 있는 자료를 순서대로 검색하는 방법을 말합니다. 알고리즘이 단순하여 구현이 쉽지만, 검색 대상의 수가 많은 경우에는 수행시간이 급격히 증가하여 비효율적입니다. O(n)의 시간 복잡도를 갖습니다. `바이너리 서치`는 자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행합니다. 목적 키를 찾을 때까지 이진 검색을 순환적으로 반복 수행함으로써 검색 범위를 반으로 줄여가면서 보다 빠르게 검색을 합니다. 단, 이진 검색을 하기 위해서는 자료가 `정렬된 상태`여야 합니다.!

```python
def binary_search(arr, x):
    low = 0
    high = len(arr) - 1
    mid = 0
 
    while low <= high:
 
        mid = (high + low) // 2
 
        # If x is greater, ignore left half
        if arr[mid] < x:
            low = mid + 1
 
        # If x is smaller, ignore right half
        elif arr[mid] > x:
            high = mid - 1
 
        # means x is present at mid
        else:
            return mid
 
    # If we reach here, then the element was not present
    return -1
 
```

### 셀렉션 알고리즘

저장되어 있는 자료롤부터 k번째로 큰 혹은 작은 원소를 찾는 방법을 셀렉션 알고리즘이라 합니다. 선택 과정으로는 1) 정렬 알고리즘을 이용하여 자료를 정렬합니다. 2) 원하는 순서에 있는 원소를 가져옵니다.

### 선택 정렬

선택 정렬은 주어진 리스트에서 최소값을 찾습니다. 다음 리스트의 맨 앞에 위치한 값과 교환합니다. 맨 처음 위치를 제외한 나머지 리스트를 대상으로 위의 과정을 반복합니다. `O(n^2)`

```python
def selectionSort(a):
    for i in range(len(a)-1):
        min_idx = i
        for j in range(i+1, len(a)):
            if a[min_idx] > a[j]:
                min_idx = j
        a[i], a[min_idx] = a[min_idx], a[i]
```