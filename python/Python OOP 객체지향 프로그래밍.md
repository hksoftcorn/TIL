# OOP 객체지향 프로그래밍

<span style="color:red">`Object Oriented Programming`</span>  객체를 지향하다.

### 1. Intro 소개 
>  Object = 객체 혹은 무언가(Stuff)

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



### 2. Practice 실습

##### 2.1. 객체(Object)
> Python에서 모든 것은 객체(object)이다.
> 모든 객체는 <span style="color:red">`타입(type)`</span>, <span style="color:red">`속성(attribute)`</span>,  <span style="color:red">`조작법(method)`</span>을 가진다.

객체(Object)의 특징

- **타입(type)**: 어떤 연산자(operator)와 조작(method)이 가능한가?

- **속성(attribute)**: 어떤 상태(데이터)를 가지는가?

- **조작법(method)**: 어떤 행위(함수)를 할 수 있는가?

  

##### 2.2. OOP 특징 비교
>  객체 지향 프로그래밍(영어: Object-Oriented Programming, OOP)은 컴퓨터 프로그래밍의 패러다임의 하나이다. 객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것이다. 

- 타입(Type)과 인스턴스(Instance)

- 속성(Attribute)과 메서드(Method)

- 절차 중심 vs. Object 중심

- OOP의 장점

  >  객체 지향 프로그래밍은 프로그램을 유연하고 변경이 용이하게 만들기 때문에 대규모 소프트웨어 개발에 많이 사용된다.또한 프로그래밍을 더 배우기 쉽게 하고 소프트웨어 개발과 보수를 간편하게 하며, 보다 직관적인 코드 분석을 가능하게 하는 장점을 갖고 있다.
  
  - 코드의 **직관성**
  
  - 활용의 **용이성**
    
  - 변경의 **유연성**
  
    

##### 2.3. 객체 지향 프로그래밍
> 1. 클래스(Class) 생성
> 2. 인스턴스(Instance) 생성
> 3. 메서드(Method) 정의
> 4. 속성(Attribute) 정의
> 5. 매직메서드



### 3. Apply 활용

##### 3.1. 인스턴스 & 클래스 변수

##### 3.2. 인스턴스 & 클래스간의 이름공간

##### 3.3. 메서드 종류







### 4. Recap 정리

> 객체 Object : Attribute (속성) / Method (메서드)
>
> ​	class  : 파이썬의 타입을 표현하는 방법, 객체를 표현하기 위한 문법입니다.
>
> ​				 i.g. 클래스 변수, 클래스 메서드
>
> ​    Instance : 특정 타입(type)의 실제 데이터 예시(instance), 파이썬에서 모든 것은 객체이고, 
>
> ​					   **모든 객체는 특정 타입의 인스턴스**입니다.
>
> ​					   i.g. 인스턴스 변수, 인스턴스 메서드
>
> ​	@staticmethod 스태틱 메서드

