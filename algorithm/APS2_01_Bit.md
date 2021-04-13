# APS - Bit

> 2021.04.13.

### 연습문제

##### 1번 문제

0과 1로 이루어진 1차 배열에서 7개 byte를 묶어서 10진수로 출력하기

```python
#1. 연습문제1
# 0000001 0001101
# 1, 13
#  0, 120, 12, 7, 76, 24, 60, 121, 124, 103

# 편의상 10개씩 끊었지만 실제로는 7 Bytes의 데이터를 표현한 것
arr = [
    0,0,0,0,0,0,0,1,1,1, 1,0,0,0,0,0,0,1,1,0,  0,0,0,0,0,1,1,1,1,0,
       0,1,1,0,0,0,0,1,1,0, 0,0,0,1,1,1,1,0,0,1, 1,1,1,0,0,1,1,1,1,1, 1,0,0,1,1,0,0,1,1,1]

result = []

for i in range(10):
    n = 0
    # 7개씩 7개의 배열 set에 접근
    for j in range(i*7, i*7+7, 1):
        # arr[j] -> 0 or 1
        n = n * 2 + arr[j]
    result.append(n)
print(*result)

#  1,     1,     1,     1,     0,     0,    0
# 2^6    2^5    2^4    2^3    2^2    2^1   2&0
```

##### 2번 문제

16진수 문자로 이루어진 1차 배열이 주어질 때 앞에서부터 7bit씩 묶어 십진수로 변환하여 출력하기

```python
#2. 연습문제2
# 0F97A3
# 000011111001011110100011
# 7 101 116 3

asc = [[0, 0, 0, 0],  #2진법 - 0(16진법)
       [0, 0, 0, 1],  #2진법 - 1(16진법)
       [0, 0, 1, 0],  #2진법 - 2(16진법)
       [0, 0, 1, 1],  #2진법 - 3(16진법)
       [0, 1, 0, 0],  #2진법 - 4(16진법)
       [0, 1, 0, 1],  #2진법 - 5(16진법)
       [0, 1, 1, 0],  #2진법 - 6(16진법)
       [0, 1, 1, 1],  #2진법 - 7(16진법)
       [1, 0, 0, 0],  #2진법 - 8(16진법)
       [1, 0, 0, 1],  #2진법 - 9(16진법)
       [1, 0, 1, 0],  #2진법 - A(16진법) - 10
       [1, 0, 1, 1],  #2진법 - B(16진법) - 11
       [1, 1, 0, 0],  #2진법 - C(16진법) - 12
       [1, 1, 0, 1],  #2진법 - D(16진법) - 13
       [1, 1, 1, 0],  #2진법 - E(16진법) - 14
       [1, 1, 1, 1]]  #2진법 - F(16진법) - 15

# ASCII -> Hexadecimal
def aToh(c):
    # 9이하이면
    if c <= '9':
        return ord(c) - ord('0')
    # 10이상이면
    else:
        return ord(c) - ord('A') + 10

def makeT(x):
    for i in range(4):
        t.append(asc[x][i])

t = []
arr = "0F97A3"
for i in range(len(arr)):
       makeT(aToh(arr[i]))

n = 0
for i in range(len(t)):
    # 7bit씩 묶어서 십진수 변환
    n = n * 2 + t[i]
    if i % 7 == 6:
        print(n, end=", ")
        n = 0

if i % 7 != 6 :
    print(n)
```

##### 3번 문제

```python
#3. 연습문제3
# 0DEC
# 0 2
# 0269FAC9A0
# 1 1 7 8 0

asc = [[0, 0, 0, 0],  #0
       [0, 0, 0, 1],  #1
       [0, 0, 1, 0],  #2
       [0, 0, 1, 1],  #3
       [0, 1, 0, 0],  #4
       [0, 1, 0, 1],  #5
       [0, 1, 1, 0],  #6
       [0, 1, 1, 1],  #7
       [1, 0, 0, 0],  #8
       [1, 0, 0, 1],  #9
       [1, 0, 1, 0],  #A
       [1, 0, 1, 1],  #B
       [1, 1, 0, 0],  #C
       [1, 1, 0, 1],  #D
       [1, 1, 1, 0],  #E
       [1, 1, 1, 1]]  #F

code = [[[[[[0 for _ in range(2)]for _ in range(2)]for _ in range(2)]for _ in range(2)]for _ in range(2)]for _ in range(2)]
code[0][0][1][1][0][1] = 0
code[0][1][0][0][1][1] = 1
code[1][1][1][0][1][1] = 2
code[1][1][0][0][0][1] = 3
code[1][0][0][0][1][1] = 4
code[1][1][0][1][1][1] = 5
code[0][0][1][0][1][1] = 6
code[1][1][1][1][0][1] = 7
code[0][1][1][0][0][1] = 8
code[1][0][1][1][1][1] = 9

def aToh(c):
    if c <= '9':
        return ord(c) - ord('0')
    else:
        return ord(c) - ord('A') + 10

def makeT(x):
    global pos
    for i in range(4):
        t.append(asc[x][i])

t = []
arr = "0269FAC9A0"

ans = []
pos = -1

for i in range(len(arr)):
       makeT(aToh(arr[i]))

# 뒤에서 1 찾기
for i in range(len(t)-1, -1, -1):
    if t[i] == 1:
        pos = i
        break

while pos - 6 > 0:
    x = code[t[pos-5]][t[pos-4]][t[pos-3]][t[pos-2]][t[pos-1]][t[pos]]
    ans.append(x)
    pos -= 6

for i in range(len(ans)):
    print(ans.pop(), end=" ")
print()
```



##### Homework - 단순이진코드

```python
# class Scanner():

def scan(barcode):
    for i in range(N):
        for j in range(M-1, 0, -1):
            if arr[i][j] == '0': continue
            decrypt_numbers = []
            for k in range(j-56+1, j , 7):
                decrypt_numbers.append(P[barcode[i][k:k+7]])
                
            odd_sum = 0
            even_sum = 0
            for idx in range(len(decrypt_numbers)):
                if idx % 2:
                    even_sum += decrypt_numbers[idx]
                else:
                    odd_sum += decrypt_numbers[idx]
            if (odd_sum * 3 + even_sum) % 10:
                return 0
            else:
                return odd_sum + even_sum
               

P = {
    '0001101': 0,
    '0011001': 1,
    '0010011': 2,
    '0111101': 3,
    '0100011': 4,
    '0110001': 5,
    '0101111': 6,
    '0111011': 7,
    '0110111': 8,
    '0001011': 9
}


T = int(input())
for tc in range(1, T+1):
    N, M = map(int, input().split())
    barcode = [input() for _ in range(N)]
    print('#{} {}'.format(tc, scan(barcode)))    
```













