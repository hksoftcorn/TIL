# Python 정리

### 01 - Intro

- 실수의 연산

  ```python
  3.5 - 3.12 == 0.38
  False
  ```

  > ✔ 실수의 연산 해결방안
  >
  > 1. round(3.5 - 3.12, 2)
  > 2. abs((3.5 - 3.12) - 0.38) <= 1e-10
  > 3. abs((3.5 - 3.12) - 0.38) <= sys.float_info.epsilin
  > 4. math.isclose((3.5 - 3.12), 0.38)

- 복소수

  ```python
  a = 3 - 4j  # OK
  complex('1+2j')  # OK
  complex('1 + 2j')  # ValueError
  ```

  > ✔ 복소수 표현
  >
  > - 문자열을 복소수로 변환할 때, 문자열은 중앙의 + 또는 - 연산자 주위에 공백을 포함해서는 안 됩니다.

- f-string

  ```python
  import datetime
  today = datetime.datetime.now()
  print(today) # 2021-01-31 22:32:50.519390
  print(f'오늘은 {today:%y}년 {today:%m}월 {today:%d}일 {today:%A}')
  
  pi = 3.141592
  print(f'원주율은 {pi:.4}! 반지름이 2일때 원의 넓이는 {pi*2*2}')
  ```

  > ✔ f-string
  >
  > - f-string에서도 `변수:%` 혹은 `변수:.n`으로 표현하는 응용법이 있습니다.

- 표현식 Expression

  > ✔ 표현식(Expression)=> evaluate => 값
  >
  > - 하나의 값(value)으로 환원(reduce)될 수 있는 문장
  > - 식별자, 값(리터럴), 연산자로 구성됩니다.
  > - 표현식을 만드는 문법(syntax)은 일반적인 (중위표기) 수식의 규칙과 유사합니다.

  

### 02 - Container

- Sequence 형

  > 시퀀스형 컨테이너 특징과 종류
  >
  > - 순서가 있다, 특정 위치의 데이터를 포인트 할 수 있다.
  > - 리스트, 튜플, 레인지, 문자형, 바이너리

- 튜플

  ```python
  # 하나의 항목으로 구성된 튜플은 값 뒤에 쉼표를 붙여서 만듭니다.
  single_tuple = ('hello',)
  print(type(single_tuple)) # tuple
  print(len(single_tuple)) # 1
  
  # 튜플은 변경 불가능합니다.
  my_tuple = (1, 3)
  my_tuple[0] = '첫번째'
  print(my_tuple) # 'tuple' object does not support item assignment
  ```

- Non-Sequence 형

  > 비 시퀀스형 컨테이너 종류
  >
  > - 셋, 딕셔너리

- 셋(set)

  ```python
  # 셋의 활용법
  set_a = {1, 2, 3}
  set_b = {3, 6, 9}
  
  set_a - set_b # 차집합
  set_a | set_b # 합집합
  set_a & set_b # 교집합
  list(set([2, 1, 2, 3, 1, 1, 2])) # [1, 2, 3] 중복제거
  ```

- 딕셔너리(dictionary)

  ```python
  # 키값이 중복된다면?
  d = {'name': 'hong', 'year': 2020, 'name': 1}
  list(d) # ['name', 'year']
  tuple(d) # ('name', 'year')
  set(d) # {'name', 'year'}
  ```

  > 딕셔너리 활용법
  >
  > - key는 변경 불가능(immutable)한 데이터만 가능하다. 
  >
  >   (`immutable : string, integer, float, boolean, tuple, range`)
  >
  > - value는 `list`, `dictionary`를 포함한 모든 것이 가능하다.



### 03 - Control Statement

- 조건 표현식 (삼항 연산자)

  ```python
  num = 1
  print('0 보다 큼') if num > 0 else print('0 보다 크지않음')
  
  value = num if num >= 0 else -num
  ```

  > 조건 표현식 기본 문법
  >
  > - `true_value if <조건식> else false_value`



### 04 - Function

- 매개변수(parameter) & 인자(argument)

  - 매개변수

    ```python
    def func(x):
        return x + 2
    ```

    - `x` 는 매개변수(parameter)
    - 입력을 받아 함수 내부에서 활용할 `변수`라고 생각하면 된다.
    - 함수의 정의 부분에서 볼 수 있다.

  - (전달)인자

    ```python
    func(2)
    ```

    - `2` 는 (전달)인자(argument)
    - 실제로 전달되는 `입력값`이라고 생각하면 된다.
    - 함수를 호출하는 부분에서 볼 수 있다.

  - 인자의 종류

    - 위치 인자 : 함수는 기본적으로 인자를 위치로 판단합니다.
    - 기본 인자 값 : 함수가 호출될 때, 인자를 지정하지 않아도 기본 값을 설정할 수 있습니다.
    - 키워드 인자 : 직접 변수의 이름으로 특정 인자를 전달할 수 있습니다.
    - 가변 인자 리스트(*args) : `tuple` 형태로 처리가 되며, 매개변수에 `*`로 표현합니다.
    - 가변 키워드 인자(\**args) : `dict`형태로 처리가 되며, `**`로 표현합니다.

    ```python
    def func(a, b, *args):
    # *args : 임의의 개수의 위치인자를 받음을 의미
    # 보통, 이 가변 인자 리스트는 매개변수 목록의 마지막에 옵니다.
    
    def students(*agrs):
        for student in agrs:
            print(student)
            
    
    def func(**kwargs):
    # **kwargs : 임의의 개수의 키워드 인자를 받음을 의미
    
    def my_url(**kwargs):
        url = 'https://api.go.kr?'
        # kwargs : dictionary
        # kwargs.items() : dict_items([('sidoname', '서울'), ('key', 'asdf')])
        # print(kwargs.items())
        for name, value in kwargs.items():
            url += f'{name}={value}&'
        return url
    my_url(sidoname='서울', key='asdf')
    ```

- 이름 검색(resolution) 규칙

  > 파이썬에서 사용되는 이름(식별자)들은 이름공간(namespace)에 저장되어 있습니다.
  >
  > 이것을, `LEGB Rule` 이라고 부르며, 아래와 같은 순서로 이름을 찾아나갑니다.
  >
  > - `L`ocal scope: 정의된 함수
  >
  > - `E`nclosed scope: 상위 함수
  >
  > - `G`lobal scope: 함수 밖의 변수 혹은 import된 모듈
  >
  > - `B`uilt-in scope: 파이썬안에 내장되어 있는 함수 또는 속성

- 재귀 함수

  ```python
  def factorial(n):
      if n == 1:
          # base case...
          return 1
      else:
          return n * factorial(n-1)
      
  def fib(n):
      # base case!
      # 그냥 if 0일때, return 0 / if 1일때, return 1
      if n < 2:
          return 1
      else:
          return fib(n-1) + fib(n-2)
  ```

  > 재귀 함수 정의와 활용
  >
  > - 재귀 함수는 함수 내부에서 자기 자신을 호출 하는 함수를 뜻한다.
  > - 재귀함수를 작성시에는 반드시, `base case`가 존재 하여야 한다.
  > - 코드가 더 직관적이고 이해하기 쉬운 경우가 있다.
  > - 재귀 호출은 `변수 사용` 을 줄여줄 수 있다.
  > - 하지만, 함수가 호출될 때마다 메모리 공간에 쌓여 메모리 스택이 넘치거나(Stack overflow) 프로그램 실행 속도가 늘어지는 단점이 생긴다.
  > - 파이썬에서는 이를 방지하기 위해 3,000번이 넘어가게 되면 더이상 함수를 호출하지 않고 종료된다. (최대 재귀 깊이)



### 05 - Data Structure

데이터 구조(Data Structure)란 데이터에 편리하게 접근하고, 변경하기 위해서 데이터를 저장하거나 조작하는 방법을 말한다.

> Program = Data Structure + Algorithm   -*Niklaus Wirth*

- String

  - `.find(x)` : x의 첫 번째 위치를 반환합니다. 없으면, `-1`을 반환합니다.

  - `.index(x)` : x의 첫 번째 위치를 반환합니다. 없으면, 오류가 발생합니다.

  - `.replace(oldm new [, count])` : 바꿀 대상 글자를 새로운 글자로 바꿔서 반환합니다.

    ​														count를 지정하면 해당 갯수만큼만 시행합니다.

  - `.strip([cahrs])` : 특정한 문자들을 지정하면, 양쪽을 제거하거나 왼쪽/오른쪽을 제거합니다(lstrip, rstrip).

    ​							지정하지 않으면 공백을 제거합니다.

  - `.split()` : 문자열을 특정한 단위로 나누어 리스트로 반환합니다.
  - `'separator'.join(iterable)` : 반복가능한(iterable) 컨테이너의 요소들을 separator를 구분자로 합쳐 문자열로 반환합니다.
  - `.capitalize()` : 앞글자를 대문자로 만들어 반환한다.
  - `.title()` : 어포스트로피나 공백 이후를 대문자로 만들어 반환한다.
  - `.upper()` : 모두 대문자로 만들어 반환한다.
  - `.lower()` : 모두 소문자로 만들어 반환한다.
  - `.swapcase()` : 대 <-> 소문자로 변경하여 반환한다.
  - 검증 메소드 : .isalpha(), .isdecimal(), .isdigit(), .isnumeric(), .isspace(), .isupper(), .istitle(), .islower()

- List

  - `.append(x)` : 리스트에 값을 추가할 수 있습니다.

  - `.extend(iterable)` : 리스트에 iterable(list, range, tuple, string**[주의]**) 값을 붙일 수가 있습니다.

  - `.insert(i, x)` : 정해진 위치 i에 x값을 추가합니다.

  - `.remove(x)` : 리스트에서 값이 x인 것을 삭제합니다.

  - `.pop(i)` : 정해진 위치 `i`에 있는 값을 삭제하며, 그 항목을 반환합니다. i가 지정되지 않으면 마지막 항목을 삭제하고 되돌려줍니다.

  - `.clear` : 리스트의 모든 항목을 삭제합니다.

  - `.index(x)` : x 값을 찾아 해당 index 값을 반환합니다.

  - `.count(x)` : 원하는 값의 개수를 확인할 수 있습니다.

  - `.sort()` : 정렬합니다. 내장함수 `sorted()` 와는 다르게 **원본 list를 변형**시키고, **`None`**을 리턴합니다.

  - `.reverse()` : 반대로 뒤집습니다. (정렬 아님)

    > 리스트의 복사
    >
    > 1. slice 연산자 사용 [:]
    > 2. list() 활용
    > 3. import copy  copy.deepcopy()

    ```python
    # List Comprehension
    [expression for 변수 in iterable]
    
    [number**3 for number in numbers]
    [i for i in range(1, 11) if i % 2 == 0]
    # 조건 표현식
    ['홀수' if i % 2 == 1 else '짝수' for i in range(1, 11)]
    # 뒤에 조건식에는 else 사용 불가
    ['홀수' for i in range(1, 11) if i % 2 == 1 else '짝수']
    ```

- iterable function : 유용한 Built-in Function 

  - `map(function, iterable)` : 순회가능한 데이터 구조(iterable)의 모든 요소에 function을 적용한 후 그 결과를 돌려준다. return은 `map_object` 형태이다.
  - `filter(function, iterable)` : iterable에서 function의 반환된 결과가 `True` 인 것들만 구성하여 반환한다.
  - `zip(*iterables`) : 복수의 iterable 객체를 모아(`zip()`)준다. 결과는 튜플의 모음으로 구성된 `zip object` 를 반환한다.

- Set

  - `.add(elem)` : elem을 세트에 추가합니다.
  - `.update(*others)` : 여러가지의 값을 추가합니다. 인자로는 반드시 iterable 데이터 구조를 전달해야합니다.
  - `.remove(elem)` : elem을 세트에서 삭제하고, 없으면 KeyError가 발생합니다.
  - `.discard(elem)` : elem을 세트에서 삭제하고 없어도 에러가 발생하지 않습니다.
  - `.pop()` : **임의의 원소**를 제거해 반환합니다.

- Dictionary

  - `.get(key[, default])` : key를 통해 value를 가져옵니다. 절대로 KeyError가 발생하지 않습니다. default는 기본적으로 None입니다.
  - `.pop(key[, default])` : key가 딕셔너리에 있으면 제거하고 그 값을 돌려줍니다. 그렇지 않으면 default를 반환합니다. default가 없는 상태에서 딕셔너리에 없으면 KeyError가 발생합니다.
  - `.update()` : 값을 제공하는 key, value로 덮어씁니다.
  - `.keys()` : 키를 번환합니다.
  - `.values()` : 값을 번환합니다.
  - `.items()` : 키와 값을 번환합니다.

  ```python
  book_title =  ['great', 'expectations', 'the', 'adventures', 'of', 'sherlock', 'holmes', 'the', 'great', 'gasby', 'hamlet', 'adventures', 'of', 'huckleberry', 'fin']
  # 문제 발생!! key 있을 때랑 없을때를 나누자!
  count = {}
  # 단어들을 반복하면서
  for word in book_title:
      # 만약에 키가 있으면,
      if word in count.keys():
          # count 딕셔너리에 해당하는 key의 value를 증가시킨다.
          count[word] = count[word] + 1
      # 없으면,
      else:
          count[word] = 1
  print(count)
  
  
  book_title =  ['great', 'expectations', 'the', 'adventures', 'of', 'sherlock', 'holmes', 'the', 'great', 'gasby', 'hamlet', 'adventures', 'of', 'huckleberry', 'fin']
  # 문제 발생!! key 있을 때랑 없을때를 나누자!
  count = {}
  # 단어들을 반복하면서
  for word in book_title:
      # 가지고 오는데 없으면 0..
      count[word] = count.get(word, 0) + 1 # 중요!
  count
  ```

  ```python
  # Dictionary comprehension
  dusts = {'서울': 72, '대전': 82, '구미': 29, '광주': 45, '중국': 200}
  result = {key: value for key, value in dusts.items() if value > 80}
  result = {key: '나쁨' if value > 80 else '보통' for key, value in dusts.items()}
  result = {key: '매우나쁨' if value > 150 else '나쁨' if value > 80 else '보통' if value > 30 else '좋음' for key, value in dusts.items()}
  ```



### 06 - OOP

<span style="color:red">`Object Oriented Programming`</span>  객체를 지향하다.

>  1. 객체(Object)
>     - 객체는 자신 고유의 **속성(attribute)**을 가지며 클래스에서 정의한 **행위(behavior)**를 수행할 수 있다.
>  2. 클래스(Class) 
>     - 공통된 속성(attribute)과 행위(behavior)를 정의한 것으로 객체지향 프로그램의 기본적인 **사용자 정의 데이터형(user-defined data type)**
>  3. 인스턴스(Instance) 
>     - 특정 `class`로부터 생성된 해당 클래스의 실체/예시(instance) 
>  4. 속성(Attribute) 
>     - 클래스/인스턴스가 가지는 속성(값/데이터)
>  5. 메서드(Method) 
>     -  클래스/인스턴스에 적용 가능한 조작법(method) & 클래스/인스턴스가 할 수 있는 행위(함수)

- OOP란 : S + V 형태로 표현한 것.

```python
'Hi'.lower() # 객체 지향 (단축형)
str.lower('Hi') # 절차 지향 (self가 인스턴스 자기 자신인 이유입니다.)

[1, 2, 3].sort()
```

- 객체 지향 프로그래밍 예시

```python
class Rectangle:
    
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def area():
        return self.x * self.y
    
    def circumference():
        return 2 * (self.x * self.y)
    
r1 = Rectangle(10, 30)    
r1.area()
r1.circumference()
```

- 객체(Object)

  > Python에서 모든 것은 객체(object)이다.
  > 모든 객체는 <span style="color:red">`타입(type)`</span>, <span style="color:red">`속성(attribute)`</span>,  <span style="color:red">`조작법(method)`</span>을 가진다.

- 객체(Object)의 특징
  - **타입(type)**: 어떤 연산자(operator)와 조작(method)이 가능한가?

  - **속성(attribute)**: 어떤 상태(데이터)를 가지는가?

  - **조작법(method)**: 어떤 행위(함수)를 할 수 있는가?

- OOP 특징 비교

  > 객체 지향 프로그래밍(영어: Object-Oriented Programming, OOP)은 컴퓨터 프로그래밍의 패러다임의 하나이다. 객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것이다. 
  - 타입(Type)과 인스턴스(Instance)

  - 속성(Attribute)과 메서드(Method)

  - 절차 중심 vs. Object 중심

- OOP의 장점

  >  객체 지향 프로그래밍은 프로그램을 유연하고 변경이 용이하게 만들기 때문에 대규모 소프트웨어 개발에 많이 사용된다.또한 프로그래밍을 더 배우기 쉽게 하고 소프트웨어 개발과 보수를 간편하게 하며, 보다 직관적인 코드 분석을 가능하게 하는 장점을 갖고 있다.

  - 코드의 **직관성**

  - 활용의 **용이성**

  - 변경의 **유연성**


- 객체 지향 프로그래밍

  > 1. 클래스(Class) 생성
  > 2. 인스턴스(Instance) 생성
  > 3. 메서드(Method) 정의
  > 4. 속성(Attribute) 정의
  > 5. 매직메서드

- 메서드의 종류

  - 인스턴스 메서드
    - 인스턴스가 사용할 메서드
    - 클래스 내부에 정의되는 메서드의 기본값은 인스턴스 메서드
    - **호출시, 첫번째 인자로 인스턴스 자기자신 `self`이 전달됨**
  - 클래스 메서드
    - 클래스가 사용할 메서드
    - `@classmethod` 데코레이터를 사용하여 정의
    - **호출시, 첫 번째 인자로 클래스 `cls`가 전달됨**
  - 스태틱 메서드
    - 클래스가 사용할 메서드
    - `@staticmethod` 데코레이터를 사용하여 정의
    - **호출시, 어떠한 인자도 전달되지 않음**

  ```python
  class Puppy:
      population = 0
      
      def __init__(self, name, breed='멍멍이'):
          self.name = name
          self.breed = breed
          Puppy.population += 1
      
      def bark(self):
          # instance메서드?
          # instance변수의 값을 활용하는 함수니까
          print(f'멍멍 {self.name}!!! 나는 {self.breed}')
          
      @staticmethod
      def info():
          # class메서드 하셔도 되지 않을까요?..
          # 상관없어요
          # 근데 class를 쓰나요?
          print('우리집 강아지입니다 >_<')
          
      @classmethod
      def get_population(cls):
          # 클래스 변수의 값을 쓸거! 함수에서
          # 그러니까 클래스 메서드로 정의를 하고,
          # cls로 넘겨주는 클래스를 활용해서 코드를 짠다.
          print(f'{cls.population}')
      
      def __del__(self):
          Puppy.population -= 1
  ```

- 상속

  - 부모/자식 타입 검사 방법

  ```python
  isinstance(True, int) # boolean 값이랑 int랑 비교 => True
  type(True) is int # False
  # 그 이유는 boolean은 int를 상속받아 만들어짐
  issubclass(bool, int) # True
  bool.mro() # [bool, int, object]
  float.mro() # [float, object]
  ```

  - super()

  ```python
  class Person:
      population = 0
      
      def __init__(self, name):
          self.name = name
          Person.population += 1
          
      def talk(self):
          print(f'반갑습니다. {self.name}입니다.')
  
  class Student(Person):
      # 학생은 생성할 때, 학번을 추가로 받고 싶어요......
      def __init__(self, name, student_id):
          super().__init__(name) # 여기가 실행되는 것은 부모클래스의 init()실행하고
          # 추가 작업
          self.student_id = student_id
  ```

  - 메서드 오버라이딩
  - 다중 상속

  ```python
  class Person:
      
      def __init__(self, name):
          self.name = name
          
      def talk(self):
          print('사람입니다.')
          
  class Mom(Person):
      gene = 'XX'
      
      def swim(self):
          print('첨벙첨벙')
          
  class Dad(Person):
      gene = 'XY'
      
      def walk(self):
          print('씩씩하게 걷기')
                 
  class FirstChild(Mom, Dad):
      
      def cry(self):
          print('응애')
          
      def walk(self):
          print('아자아장')
  ```

  