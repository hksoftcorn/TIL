[TOC]

# APS (Algorithm Problem Solving) - String

좋은 알고리즘이란, ① 정확성 ② 작업량 ③ 메모리 사용량 ④ 단순성 ⑤ 최적성을 만족하는 풀이를 말합니다. 이 중 알고리즘의 작업량을 표현할 때 `시간복잡도`로 표현합니다. 실제 걸리는 시간을 측정, 실행되는 명령문 수를 측정하거나 수학적으로 빅-오 표기법(Big-Oh Notation)으로 표현하기도 합니다.

## 문자열 

1967년, 미국 `ASCII`라는 문자 인코딩 표준이 제정되었습니다. ASCII는 7bit 인코딩으로 128문자를 표현하며 33개의 출력 불가능한 제어문자들과 공백을 비롯한 95개의 출력 가능한 문자들로 이루어져 있습니다. 65 == 'A', 97 == 'a'

인터넷이 전 세계로 발전하면서 ASCII를 만들었을 때의 문제와 같은 문제가 국가간에 정보를 주고 받을 때 발생하게 되었습니다. 자국의 코드체계를 타 국가가 가지고 있지 않으면 정보를 잘못 해석 할 수 밖에 없었기 때문에 다국어 처리를 위해 표준을 마련했습니다. 이를 `유니코드`라고 일컫습니다.

유니코드도 다시 Character Set으로 분류됩니다. 유니코드를 저장하는 변수의 크기를 정의했지만 바이트 순서에 대해서 표준화하지 못했습니다. 다시 말해 파일을 인식 시 이 파일이 UCS-2 / UCS-4인지 인식하고 각 경우를 구분해서 모두 다르게 구현해야 하는 문제가 발생하게 되는데, 그래서 유니 코드의 적당한 외부 인코딩이 필요하게 되었습니다. 유니코드의 인코딩(UTF) 방법으로 UTF-8, UTF-16, UTF-32이 있습니다. 

C와 Java의 String 처리의 기본적인 차이점 : C는 아스키 코드로 저장합니다, Java는 유니코드(UTF16, 2byte)로 저장합니다. Python은 유니코드(UTF8)로 저장합니다.

### atoi / itoa

```python
def atoi(num_str):
    value = 0
    for i in  range(len(num_str)):
        value *= 10
        value += ord(num_str[i]) - ord('0') # ord('0') = 48
    return value
    
def itoa(num):
    def digit_length(num):
        count = 0
        while num:
            num //= 10
            count += 1
        return count
    count = digit_length(num)
    value = ''
    for i in range(count-1, -1, -1):
        num_int //= (10**i)
        num -= num_int * (10**i)
        value += chr(num_int + 48)
    return value
```

### 패턴 매칭

#### 1) 고지식한 패턴(Brute-Force) 검색 알고리즘

```python
P = 'is'
t = 'This is a book'
M = len(P)
N = len(t)

def BruteForce(p, t):
    i = 0 # t의 인덱스
    j = 0 # p의 인덱스
    while j < M and i < N:
        if t[i] != p[j]:
            i = i - j
            j -= 1
        i += 1
        j += 1
    if j == M : return i - M
    else: return -1
    
def BruteForce2(p, t):
	N = len(t)
    M = len(p)
    
    for i in range(N-M+1):
        cnt = 0
        for j in range(M):
            if t[i+j] == p[j]:
                cnt+=1
            else:
                break
        if cnt == M:
            return i
    return -1
```

#### 2) KMP 알고리즘

전처리과정을 통해서 각 자리마다 겹치는 숫자 cnt 배열을 만들어 줍니다. 패턴과 틀린 문자가 나오면 해당 인덱스의 이전 (idx-1) cnt 값을 참고하여 인덱스를 옮깁니다. `O(M+N)`. 

#### 3) 보이어-무어 알고리즘

오른쪽에서 왼쪽으로 비교합니다. 패턴에 오른쪽 끝에 있는 문자가 불일치 하고, 이 문자가 패턴 내에 존재하지 않는 경우, 이동 거리는 무려 패턴의 길이 만큼이 됩니다. `O(MN)`. 보이어-문자 알고리즘의 장점은 텍스트 문자를 다 보지 않아도 된다는 것입니다. 패턴의 오른쪽부터 비교하면서 패턴 내에 문자가 있는지 확인합니다. 최악의 경우는 브루트-포스와 같은 반복 횟수를 반환합니다.



### 회문

```python
words = [input() for i in range(N)]
zwords = list(zip(*words))
```

```python

```



### 문자열 암호화 & 문자열 압축

#### 1) 문자열 암호화 : 시저 암호 / 단일 치환 암호

#### 2) bit열의 암호화 : XOR

#### 3) 문자열 압축 : Run-length Encoding 알고리즘



### 



