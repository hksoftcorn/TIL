# Python 기초

### 1. 개요

- PEP-8 스타일 준수

- ```python
  print('hello\
  world')
  ```

- ```python
  lunch = [
      '짜장면', '짬뽕', '탕수육',
      '군만두', '물만두', '왕만두'
  ]
  ```

- swap : 변수 x와 y의 값을 바꿔봅시다.

  ```python
  x = 10
  y = 100
  x, y = y, x
  ```

- 변수

  ```python
  # 식별자 : 영문알파벳(대, 소), 밑줄(_), 숫자
  # 첫 글자에 숫자가 올 수 없음
  # 길이 제한 없음
  # 아래 키워드로 사용할 수 없음
  import keyword
  print(keyword.kwlist)
  ```

- 오버플로우 vs 임의 정밀도 산술

  ```python
  import sys
  # sys.maxsize의 값은 2**64 -1 (부호비트 제외)
  max_int = sys.maxsize
  ```

- n진수

  ```python
  '2진수' : 0b10
  '8진수' : 0o10
  '10진수' : 10
  '16진수' : 0x10
  ```

- float 실수 연산

  ```python
  # a = 3.5, b = 3.14
  # a-b ?
  (1) round(a-b, 2)
  (2) abs(a-b) <= 1e-10
  (3) abs(a-b) <= sys.float_info.epsilon
  (4) math.isclose(a, b)
  ```

- 복소수

  ```python
  # 문자열을 복소수로 변환할 때 띄어쓰기를 넣지 않습니다.
  complex('1+2j')
  ```

- String Interpolation

  ```python
  (1) %-formatting
  (2) str.format()
  (3) f-strings
  ```

- Bool

  ```python
  # False로 변환되는 것!
  0, 0.0, (), [], {}, '', None
  ```

- 형 변환

  ```python
  - string -> integer
  - integer -> string
  - float -> int
  ```

- 몫과 나머지

  ```python
  quotient, remainder = divmod(5, 2)
  ```

- 단축평가

  ```python
  'a' and 'b' -> 'b'
  'a' or 'b' -> 'a'
  vowels = 'aeiou'
  ('a' and 'b') in vowels -> False
  ('b' and 'a') in vowels -> True
  ```

- `id` : Identity -> python object interning

  ```python
  # 파이썬에서 -5 부터 256까지 id가 동일합니다.
  a = 1, b=10
  print(a is b)
  print(id(a), id(b))
  ```

- 연산자 우선순위

  > 0. ()을 통한 grouping
  > 1. Slicing
  > 2. Indexing
  > 3. 제곱연산자 **
  > 4. 단항연산자 (음수/양수 부호)
  > 5. 산술연산자 (*, /, %)
  > 6. 산술연산자 (+, -)
  > 7. 비교연산자 in, is
  > 8. not
  > 9. and
  > 10. or
```python
# i.g. 
-3 ** 6 = -729
(-3) ** 6 = 729

```