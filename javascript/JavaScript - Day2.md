# JavaScript - Grammar

> 2021.04.29.

## 1. 변수와 식별자

### 1.1. 식별자 정의와 특징

- 식별자는 변수를 구분할 수 있는 변수명을 말함
- 식별자는 반드시 문자, 달러 또는 밑줄로 시작
- 대소문자를 구분하며, 클래스명 외에는 모두 소문자로 시작
- 예약어 사용 불가능

### 1.2. 식발자 작성 스타일

- 카멜 케이스
  - 변수, 객체, 함수에 사용
- 파스칼 케이스
  - 클래스, 생성자에 사용
- 대문자 스네이크 케이스
  - 상수에 사용

### 1.3. 변수 선언 키워드 (let, const, var)

#### let

- 재할당 할 수 있는 변수 선언 시 사용
- 변수 재선언 불가능
- 블록 스코프*

#### const

- 재할당 할 수 없는 변수 선언 시 사용
- 변수 재선언 불가능
- 블록 스코프

> **개념**
>
> - `선언 (Declaration)`
>   - 변수를 생성하는 행위 또는 시점
> - `할당 (Assignment)`
>   - 선언된 변수에 값을 저장하는 행위 또는 시점
> - `초기화 (Initialization)`
>   - 선언된 변수에 처음으로 값을 저장하는 행위 또는 시점

#### var

- var로 선언한 변수는 재선언 및 재할당 모두 가능
- ES6 이전에 변수를 선언할 때 사용되던 키워드
- 호이스팅*되는 특성으로 인해 예기치 못한 문제 발생 가능
  - 따라서 ES6 이후부터는 var 대신 const와 let을 사용하는 것을 권장
- 함수 스코프*

> **용어**
>
> - 블록 스코프*
>   - { }
>   - if, for, 함수 등의 중괄호 내부를 가리킴
>   - 블록 스코프를 가지는 변수는 블록 바깥에서 접근 불가능
> - 함수 스코프*
>   - 함수의 중괄호 내부를 가리킴
>   - 함수 스코프를 가지는 변수는 함수 바깥에서 접근 불가능
> - 호이스팅*
>   - var
>   - 변수를 선언 이전에 참조할 수 있는 현상
>   - 변수 선언 이전의 위치에서 접근 시 undifined를 반환

| 키워드   | 재선언  | 재할당  | 스코프    | 비고       |
| ----- | ---- | ---- | ------ | -------- |
| let   | X    | O    | 블록 스코프 | ES6부터 도입 |
| const | X    | X    | 블록 스코프 | ES6부터 도입 |
| var   | O    | O    | 함수 스코프 | 사용 X     |



## 2. 타입과 연산자

### 2.1. 데이터 타입 종류

- 자바스크립트의 모든 값은 특정한 데이터 타입을 가짐
- 크게 원시 타입과 참조 타입으로 분류됨
  - 원시 타입 : Number, String, Boolean, undefined, null, Symbol
  - 참조 타입 : Objects, Array, Function, ... etc

> 원시 타입과 참조 타입 비교
>
> - 원시 타입
>   - 객체가 아닌 기본 타입들을 말함
>   - 변수에 해당 타입의 값이 담김
>   - 다른 변수에 복사할 때 실제 값이 복사됨
> - 참조 타입
>   - 객체 타입의 자료형들을 말함
>   - 변수에 해당 객체의 참조 값이 담김
>   - 다른 변수에 복사할 떄 참조 값이 복사됨

### 2.2. 원시 타입(Primitive type)

#### 숫자 (Number) 타입

- 정수, 실수 구분 없는 하나의 숫자 타입
- 부동소수점 형식을 따름
- NaN



#### 문자열 (String) 타입

- 텍스트 데이터를 나타내는 타입
- 16비트 유니코드 문자의 집합
- 작은따옴표 또는 큰따옴표 모두 가능
- 템플릿 리터럴



#### undefined

- 변수의 값이 없음을 나타내는 데이터 타입
- 변수 선언 이후 직접 값을 할당하지 않으면 자동으로 undefined가 할당됨



#### null

- 변수의 값이 없음을 의도적으로 표현할 때 사용하는 데이터 타입
- null 타입과 typeof 연산자



> **undefined vs null**
>
> - undefined
>   - 빈 값을 표현하기 위한 데이터 타입
>   - 변수 선언 시 아무 값도 할당하지 않으면 자바스크립트가 자동으로 할당
>   - typeof 연산자의 결과는 undefined
> - null
>   - 빈 값을 표현하기 위한 데이터 타입
>   - 개발자가 의도적으로 필요에 의해 할당
>   - typeof 연산자의 결과는 object

#### 

#### Boolean 타입





*참조 타입 : 



### 2.2. 연산자

#### 할당 연산자

- 오른쪽에 있는 피연산자의 평가 결과를 ㄷ왼쪽 피연사자에 할당하는 연산자
- 다양한 연산에 대한 단축 연산자 지원
- Increment 및 Decrement 연산자



#### 비교 연산자

- 피연산자들을 비교하고 비교의 결과값을 불리언으로 반환하는 연산자
- 문자열은 유니코드 값을 사용하며 표준 사전순서를 기반으로 비교



#### 동등 비교 연산자 (==)

- 두  피연산자가 같은 값으로 평가되는 지 비교 후 불리언 값을 반환

- 비교할 때 `암묵적 타입 변환`을 통해 타입을 일치시킨 후 같은 값인지 비교

  - 암묵적 타입 변환

- 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별

- 예상치 못한 결과가 발생할 수 있으므로 특별한 경우를 제외하고 사용하지 않음

  ​

#### 일치 비교 연산자 (===)

- 두 피연산자가 같은 값으로 평가되는 지 비교 후 불리언 값을 반환
- 엄격한 비교가 이뤄지며 암묵적 타입 변환이 발생하지 않음
  - 엄격한 비교
- 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별



#### 논리 연산자

- 세 가지 논리 연산자로 구성
  - and (&&)
  - or (||)
  - not (!)



#### 삼항 연산자

- 가장 왼쪽의 조건식이 참이면 콜론(:) 앞의 값을 사용하고 그렇지 않으면 콜론 뒤의 값을 사용
- 삼항 연산자의 결과는 변수에 할당 가능





## 3. 조건문과 반복문

### 3.1. 조건문

#### 조건문의 종류와 특징

- if statement
  - 조건 표현식의 결과값을 Boolean 타입으로 변환 후 참/거짓을 판단
- switch statement
  - 조건 표현식의 결과값이 어느 값(case)에 해당하는지 판별



### 3.2. 반복문

#### 반복문의 종류와 특징

- while
- for
- for ... in
  - 주로 객체의 속성들을 순회할 때 사용
  - 배열도 순회 가능하지만 인덱스 순으로 순회한다는 보장이 ㅇ벗으므로 권장하지 않음
- for ... of
  - 반복 가능한 객체를 순회하며 값을 꺼낼 때 사용
  - Array, Map, Set, String



#### while

```javascript
let i = 0
while (i < 6) {
  console.log(i) 
  i += 1
}
```



#### for

```javascript
for (let i = 0; i < 6;  i++) {
  console.log(i)
}
```



#### for... in

```javascript
const bestMovie = {
  title: '벤자민 버튼의 시간은 거꾸로 간다',
  releaseYear: 2008,
  actors: ['브래드 피트', '케이트 블란쳇'],
  genres: ['romance', 'fantasy'],
}

for (let key in bestMovie) {
  console.log(`${key}: ${bestMovie[key]}`)
  console.log(typeof bestMovie[key])
}
```



#### for... of

```javascript
const movies = [
  {title: '어바웃 타임'},
  {title: '굿 윌 헌팅'},
  {title: '인턴'},
]

for (let movie of movies) {
  console.log(`title: ${movie.title}`)
}
```





## 4. 함수

### 4.1. 함수 in JavaScript

- 참조 타입 중 하나로써 function 타입에 속함
- 종류
  - 함수 선언식 (function declaration)
  - 함수 표현식 (function expression)
- 일급 함수
  - 1 변수에 할당 가능
  - 2 함수의 매개변수로 전달 가능
  - 3 함수의 반환 값으로 사용 가능



### 4.2. 함수 선언식

- 함수의 이름과 함께 정의하는 방식
- 구성
  - 함수의 이름
  - 매개변수
  - 몸통



### 4.3. 함수 표현식

- 함수를 표현식 내에서 정의하는 방식
- 함수의 이름을 생략하고 익명 함수로 정의 가능
- 구성
  - 함수의 이름(생략 가능)
  - 매개변수
  - 몸통

|      | 함수 선언식                | 함수 표현식               |
| ---- | --------------------- | -------------------- |
| 공통점  |                       |                      |
| 차이점  | 익명 함수 불가능<br />호이스팅 O | 익명 함수 가능<br />호이스팅 X |
| 비고   |                       | 권장                   |



### 4.4. Arrow Function

#### 화살표 함수

- 함수를 비교적 간결하게 정의할 수 있는 문법
- function 키워드 생략 가능
- 함수의 매개변수가 단 하나 뿐이라면, '( )'도 생략 가능
- 함수 몸통이 표현식 하나라면 '{ }'과 return도 생략 가능

```javascript
const arrow = function (name) {
  return `hello! ${name}`
}

const arrow = (name) => { return `hello! ${name}`}

const arrow = name => { return `hello! ${name}`}

const arrow = name => `hello! ${name}`
```





## 5. 배열과 객체

### 5.1. 배열 Array

- 키와 속성들을 담고 있는 참조 타입의 객체
- 순서를 보장하는 특징이 있음
- 주로 대괄호를 이용하여 생성하고, 0을 포함한 야의 정수 인덱스로 특정 값에 접근 가능
- 배열의 길이는 array.length 형태로 접근 가능

| 메서드             | 설명                           | 비고              |
| --------------- | ---------------------------- | --------------- |
| reverse         | 원본 배열의 요소들의 순서를 반대로 정렬       |                 |
| push & pop      | 배열의 가장 뒤에 요소를 추가 또는 제거       |                 |
| unshift & shift | 배열의 가장 앞에 요소를 추가 또는 제거       |                 |
| includes        | 배열에 특정 값이 존재하는지 판별 후 참/거짓 반환 |                 |
| indexOf         | 배열에 특정 값이 존재하는지 판별 후 인덱스 반환  | 요소가 없을 경우 -1 반환 |
| join            | 배열의 모든 요소를 구분자를 이용하여 연결      | 구분자 생략 시 쉼표 기준  |



### 5.2. Array Helper Method

- 배열을 순회하며 특정 로직을 수행하는 메서드
- 메서드 호출 시 인자로 callback 함수를 받는 것이 특징
  - callback 함수 : 어떤 함수의 내부에서 실행될 목적으로 인자를 넘기받는 함수

| 메서드     | 설명                                  | 비고      |
| ------- | ----------------------------------- | ------- |
| forEach | 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행         | 반환 값 없음 |
| map     | 콜백 함수의 반환 값을 요소로 하는 새로운 배열 반환       |         |
| filter  | 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환 |         |
| reduce  | 콜백 함수의 반환 값들을 하나의 값(acc)에 누적 후 반환   |         |
| find    | 콜백 함수의 반환 값이 참이면 해당 요소를 반환          |         |
| some    | 배열의 요소 중 하나라도 판별 함수를 통과하면 참을 반환     |         |
| every   | 배열의 모든 요소가 판별 함수를 통과하면 참을 반환        |         |

#### forEach

```javascript
const ssafy = [1, 2, 3, 4, 5]

ssafy.forEach((region, index) => {
  console.log(region, index)
})
```

#### map

```javascript
const numbers = [1, 2, 3, 4, 5]

const doubleNums = numbers.map((num) => {
  return num * 2
})
console.log(doubleNums)
```

#### filter

```javascript
const numbers = [1, 2, 3, 4, 5]

const oddNums = numbers.filter((num) => {
  return num % 2
})
console.log(oddNums)
```

#### reduce

- acc : 이전 callback 함수의 반환 값이 누적되는 변수
- initialValue : 최초 callback 함수 호출 시 acc에 할당되는 값으로, default는 배열 첫 번째 값

```javascript
const numbers = [1, 2, 3, 4, 5]

const result = numbers.reduce((num) => {
  return num % 2
})
console.log(oddNums)
```

#### find

```javascript
const result = avengers.find((avenger) => {
  return avernger.name === 'Tony Stark'
})
```

#### some

```javascript
const hasOddNumber = numbers.some((num) => {
  return num % 2
})
```

#### every

```javascript
const isEveryNumberOdd = numbers.every((num) => {
  return num % 2
})
```





## 6. 객체 (Objects)

### 6.1. 객체의 정의와 특징

- 객체는 속성의 집합이며 중괄호 내부에 Key와 Value의 쌍으로 표현
- key는 문자열 타입만 가능
  - key 이름에 띄어쓰기 등의 구분자가 있을 경우 따옴표로 묶어서 표현
- value는 모든 타입 가능
- 객체 요소 접근은 점 또는 대괄호로 가능



### 6.2. 객체 관련 ES6 문법

- 속성명 축약

  - key와 할당하는 변수의 이름이 같으면 축약(생략) 가능

- 메서드명 축약

  - function 키 생략 가능

- 계산된 속성명 사용

  - 객체를 정의할 때 

    ```javascript
    const data = 'mydata'
    const request = {
      [data]: data.length,
      ['TOKEN_' + data.toUpperCase()]: 4
    }
    ```

- 구조 분해 할당

  - 배열 또는 객체를 분해하여 속성을 변수에 쉽게 할당할 수 있는 문법

    ```javascript
    const { name } = userInformation
    const { userId } = userInformation
    const { phoneNumber } = userInformation
    const { email } = userInformation
    ```

    ​



### 6.3. JSON (JavaScript Object Notation)

- key-value 쌍의 형태로 데이터를 표기하는 언어 독립적 표준 포맷
- 자바스크립트의 객체와 유사하게 생겼으나 실제로는 문자열 타입
- 자바스크립트에서는 JSON을 조작하기 위한 두 가지 내장 메서드 제공
  - JSON.parse()
  - JSON.stringify









