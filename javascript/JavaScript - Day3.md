# JavaScript - Day 3

> 2021.05.03.

- JS 역사

  - 브라우저 전쟁
  - 파편화 & 표준화

- JS 할 수 있는 일

  - DOM : 객체는 속성과 메서드로 조작 가능
  - BOM : 
  - JS Grammar
    - Event
    - Event Handler

  ​

## 1. AJAX

- Asynchronous JavaScript And XML (비동기식 js와 xml)
- 서버와 통신하기 위해 XMLHttpRequest 객체를 활용
  - XMLHttpRequest는 동기식과 비동기식 통신을 모두 지원
- 페이지 전체를 reload를 하지 않고서도 수행되는 "비동기성"
  - 사용자의 event가 있으면 전체 페이지가 아닌 일부분만을 업데이트
- 요즘은 XML 대신 JSON을 더 많이 사용함



### 1.1. AJAX 배경

- 2005년 GoogleMaps & Gmail 등에 활용되는 기술을 설명하기 위해 AJAX라는 용어를 최초로 사용
- AJAX는 특정 기술이 나닌 기존의 여러 기술을 사용하는 새로운 접근법을 설명하는 용어
  - 기존 기술을 잘 활용할 수 있는 방식으로 구성 및 재조합한 "새로운" 접근법
- Google 사용 예시
  - GMAIL : 메일의 전송 버튼을 눌러 놓고 다른 페이지로 넘어가고 메일은 알아서 전송 처리됨
  - Google Maps : 스크롤하는 행위 하나하나가 모두 요청이지만 페이지는 갱신되지 않음



### 1.2. XMLHttpRequest object

- 서버와 상호작용하기 위헤 사용되며, 전체 페이지의 새로고침 없이 URL로부터 데이터를 받아올 수 있음
- 사용자가 하는 것을 방해하지 않으면서 페이지의 일부를 업데이트할 수 있도록 해줌
- 주로 AJAX 프로그래밍에 사용
- XML 뿐만 아니라 모든 종류의 데이터를 받아오는데 사용 가능
- 생성자 : XMLHttpRequest()

```javascript
const request = new XMLHttpRequest()
const URL = 'https://google.com'

request.open('GET', URL)
request.send()

const todo = request.response
console.log(todo)
```





## 2. Asynchronous JavaScript

### 2.1. 동기와 비동기

- 동기식
  - 순차적, 직렬적 태스크 수행
  - 요청을 보낸 후 응답을 받아야만 다음 동작이 이루어짐(Blocking)
- 비동기식
  - 병렬적 태스크 수행
  - 요청을 보낸 후 응답을 기다리지 않고 다음 동작이 이루어짐(Non-Blocking)
    - 즉 요청을 보내놓고 다음 태스크로 진행



### 2.2. 왜 비동기를 사용하는가?

- 사용자 경험
  - 예를 들어, 데이터를 구동하고 실행되는 앱이 있으며 이 데이터의 크기가 굉장히 크다고 가정
  - 동기식 코드라면 데이터를 모두 로드 한 뒤에야 앱이 실행되기 때문에, 로드되는 동안 우리는 앱을 사용할 수 없는 상태로 얼마나 걸릴지 모르는 로딩 시간을 기다려야 함
  - 즉, 앱이 모두 멈춘 것처럼 보임
  - 이러첨 동기식 요청은 코드 실행을 차단하여 화면이 멈추고 응답하지 않는 사용자 경험을 만듦
  - 때문에 많은 웹 API 기능은 현재 비동기 코드를 사용하여 실행 됨
- `동기식 예시`
  - 버튼 클릭 후 alert 메시지의 확인 버튼을 누를 때까지 문장이 만들어지지 않음
  - 즉, alert의 처리가 끝날 때 까지 실행되지 않음
  - JS는 single threaded
- `비동기식 예시`
  - 요청을 보내고 응답을 기다리지 않고 다음 코드가 실행됨
  - 결과적으로 변수 todo에는 응답 데이터가 할당되지 않고 빈 문자열이 출력
  - 그렇다면 JS는 왜 기다려주지 않는 방식으로 동작하는가? JS는 single threaded



### 2.3. Threads

- 프로그램이 작업을 완료하는데 사용할 수 있는 단일 프로세스
- 각 스레드는 한번에 하나의 작업만 수행할 수 있음
- 예시 ) A → B → C
  - 다음 작업을 시작하려면 반드시 앞의 작업이 완료되어야 함
  - 컴퓨터 CPU는 여러 코어를 가지고 있기 때문에 한번에 여러가지 일을 처리할 수 있음

#### JS는 single threaded이다.

- 컴퓨터가 여러 개의 CPU를 가지고 있어도 main 스레드라 불리는 단일 스레드에서만 작업 수행
- 즉, 이벤트를 처리하는 Call Stack이 하나인 언어라는 의미
- 이 문제를 해결하기 위해 JS
  - 즉시 처리하지 못하는 이벤트를 다른 곳(Web API)으로 보내서 처리하도록 하고,
  - 처리된 이벤트들은 처리된 순서대로 대기실(Task Queue)에 줄을 세워 놓고
  - Call Stack이 비면 담당자(Event Loop)가 대기 줄에서 가장 오래 된 이벤트를 Call Stack으로 보냄



### 2.4. Concurrency Model

- 동시성 모델(Concurrency Model) : 싱글 스레드를 비동기식으로 처리하기 위한 방법
  - Call Stack
  - Web API (Browser API)
  - Task Queue (Event Queue, Message Queue)
  - Event Loop

#### (1) Call Stack

- 요청이 들어로 때마다 해당 요청을 순차적으로 처리하는 stack 형태의 자료 구조

#### (2) Web API (Browser API)

- JavaScript 엔진이 아닌 브라우저 영역에서 제공하는 API
- setTimeout(), DOM event 그리고 AJAX로 데이터를 가져오는 시간이 소요되는 일들을 처리

#### (3) Task Queue (Event Queue, Message Queue)

- 콜백 함수가 대기하는 Queue 형태의 자료 구조
- amin thread가 끝난 후 실행되어 후속 JavaScript 코드가 차단되는 것을 방지

#### (4) Event Loop

- Call Stack이 비어 있는 지 여부를 확인
- 비어있는 경우 Task Queue에서 실행 대기중인 콜백이 있는지 확인
- Task Queue에 대기중인 콜백이 있다면 가장 앞에 있는 콜백을 Call Stack으로 Puush

```javascript
console.log('HI')
setTimeout(function ssafy (){
  console.log('SSAFY')
}, 3000)

console.log('BYE')
```



### 2.5. Zero Delays

- 실제로 0ms 후에 콜백이 시작된다는 의미가 아님
- 실행은 Task Queue에 대기중인 작업 수에 따라 다르며 해당 예시에서는 콜백의 메시지가 처리되기 전에 HI와 BYE가 먼저 출력됨
- 왜냐하면 daly는 JS가 요청을 처리하는 데 필요한 최소 시간이기 때문
- 기본적으로 setTimeout은 특정 시간 제한을 지정 했더라도 대기중인 메시지의 모든 코드가 완료 될 때까지 대기해야 함



### 2.6. 순차적인 비동기 처리하기

- Web API로 들어오는 순서는 중요하지 않고, 어떤 이벤트가 먼저 처리 되느냐가 중요
- 이를 해결하기 위해 순차적인 비동기 처리를 위한 2가지 작성 방식

#### (1) Async callbacks

- 백그라운드에서 실행을 시작할 함수를 호출할 떄 인자로 지정된 함수
- addEventListner()의 두번째 인자

#### (2) promise-style

- Modern Web APIs에서의 새로운 코드 스타일
- XMLHttpRequest 보다 현대적인 스타일

```python
from time import sleep
import requests

def sleep_3_seconds():
    sleep(3)
    print('잘 잤다!')
    
print('이제 자야지')
sleep_3_seconds()
print('학교 가자')
```

```python
request = 

```

```javascript
// 1
const sleep3Seconds = function () {
  console.log('잘 잤다!')
}

console.log('이제 자야지')
setTimeout(sleep3Seconds, 3000)
console.log('학교 가자')

// 2
const URL = 'https://'
const xhr = new XMLHttpRequest()

xhr.open('GET', URL)
xhr.send()

const todo = xhr.response
console.log(todo)


// 3
xhr.onload = function () {
  if (xhr.status == 200) {
    const todo = xhr.response
	console.log(todo)
  }
}
```

```javascript
console.log('이제 자야지')
setTimeout((function () {
  console.log('SSAFY')
}, 3000)
console.log('학교 가자')
```





## 3. Call Back Function

- 다른 함수에 인자로 전달된 함수
- 외부 함수 내에서 호출되어 일종의 루틴 또는 작업을 완료함
- 동기식, 비동기식 모두 사용됨
- 비동기 작업이 완료된 후 코드 실행을 계속하는데 사용되는 경우 비동기 콜백이라고 함



### 3.1. JS의 함수는 "일급 객체"

- 일급 객체 : 다른 객체들에 작용 가능한 연산을 모두 지원하는 객체

> 일급 객체의 조건
>
> 1. 인자로 넘길 수 있어야함
> 2. 함수의 반환 값으로 사용할 수 있어야 함
> 3. 변수에 할당할 수 있어야 함



### 3.2. Async Callbacks (비동기 콜백)

- 백그라운드에서 코드 실행을 시작할 함수를 호출할 때 인자로 지정된 함수
- 백그라운드 코드 실행이 끝나면 콜백 함수를 호출하여 작업이 완료되었음을 알리거나, 다음 작업을 실행하게 할 수 있음



#### 왜 콜백을 사용할까?

- 콜백 함수는 명시적인 호출이 아닌 특정 routine 혹은 action 에 의해 호출되는 함수
- Django의 경우 "요청이 들어오면", Event의 경우 "특정 이벤트가 발생하면" 이라는 조건 하에서 함수를 호출할 수 있었던 건 'Callback Function' 메커니즘이 있기 때문에 가능
- 비동기 로직을 수행할 때 콜백 함수는 필수
  - 명시적인 호출이 아닌 특정 시점에 '알아서' 호출되도록 만들어야 하기 때문이며 기다려주지 않고 언젠가 처리해야 하는 일은 콜백 함수를 활용



### 3.3. Callback Hell

- 순차적인 연쇄 비동기 작업을 처리하기 위해 "콜백 함수를 호출하고, 그 다음 콜백 함수를 호출하고, 또 그 함수의 콜백 함수를 호출하고.." 의 패턴이 지속적으로 반복 됨
- 즉, 여러 개의 연쇄 비동기 작업을 할 때 마주하는 상황
- 이를 Callback Hell (콜백 지옥) 혹은 pyramid of doom (파멸의 피라미드)라고 합니다.
- 위와 같은 상황이 벌어질 경우 아래 사항들을 통제하기 어렵습니다.
  - 디버깅
  - 코드 가독성
- 해결방법
  - Keep your code shallow : 코드의 깊이를 얕게 유지
  - Modularize : 모듈화
  - Handle every single error : 모든 단일 오류 처리
  - Promise way : Promise 방식 사용

```javascript
// 1
const myFunc = function (func) {
  return func
}

const myArgumentFunc = function () {
  return 'Hello'
}

const result = myFunc(myArhumentFunc)
console.log(result)


// 2
const nums = [1, 2, 3]
const doubleNums = nums.map((num) => {
  return num * 2
})
cnosole.log(doubleNums)


// 3
const numbers = [1, 2, 3]
const newNumbers = []
numbers.forEach((num) => {
  newNumbers.push(num + 1)
})


// 4
const helloWorld = function () {
  console.log('happy coding!')
}
setTimeout(helloWorld, 2000)
console.log('unhappy coding')


// 5
<button>B</button>
const myButton = document.querySelector('button')

myButton.addEventListener('click', function () {
  console.log('button clicked!')
})

```





## 4. Promise

- 비동기 작업의 최종 완료 또는 실패를 나타내는 객체
  - 미래의 완료 또는 실패와 그 결과 값을 나타냄
  - 미래의 어떤 상황에 대한 약속
- 성공(이행)에 대한 약속
  - .then()
- 실패(거절)에 대한 약속
  - .catch()



### 4.1. Promise의 상태

1. `대기(pending)` : 이행하거나 거부되지 않는 초기 상태
2. `이행(fulfilled)` : 연산이 성공적으로 완료됨
3. `거부(rejected)` : 연산이 실패함



### 4.2. Promise methods 1

- .then (callback)

  - 이전 작업이 성고ㅛㅇ했을 때 수행할 작업을 나타내는 콜백 함수
  - 그리고 각 콜백 함수는 이전 작업의 성공 결과를 인자로 전달 받음
  - 따라서 성공했을 때의 코들르 콜백 함수 안에 작성

- .catch (callback)

  - .then이 하나라도 실패하면 동작 (try-exception)
  - 이전 작업의 실패로 인해 생성된 error 객체는 catch 블록 안에서 사용할 수 있음

  ​

### 4.3. Promise methods 2

- 각각의 .then() 블록은 서로 다른 promise를 반환
  - 즉, then()을 여러 개 사용(chaining)하여 연쇄적인 작업을 수행할 수 있음
  - 결국 여러 비동기 작업을 차례대로 수행할 수 있다는 뜻
- 마찬가지로 .then()과 .catch() 매서드는 모두 promise를 반환하기 때문에 chaining 가능
- 주의
  - 반환 값이 반드시 있어야 함
  - 없다면 콜백 함수가 이전의 promise 결과를 받을 수 없음



```javascript
const myPromise = new Promise

...

```

- 두 개의 콜백 함수를 인자로 받음
- 하나는 Promise가 이행했을 때 : resolve()
- 다른 하나는 거부했을 떄를 위함 : reject()



### 4.4. Promise methods 3

- .finally(callback)
- promise 객체를 반환
- 결과의 상관없이 무조건 지정된 콜백 함수가 실행
- 어떠한 인자도 전달 받지 않음
  - promise가 성공되었는지 거절되었는지 판단할 수 없기 때문
- 무조건 실행되어야 하는 절에서 활용
  - .then과 catch 블록에서의 코드 중복을 방지



### 4.5. Promise가 보장하는 것

-  Async callback 작성 스타일과 달리 Promise가 보장하는 특징

> 1. 콜백은 자바스크립트 Evnet Loop가 현재 실행중인 콜 스택을 완료하기 이전에는 절대 호출되지 않음 : Promise 콜백은 event queue에 배치되는 엄격한 순서로 호출됨
> 2. 비동기 작업이 성공하거나 실패한 뒤에 .then() 메서드를 이용하여 추가한 경우에도 1번과 똑같이 동작
> 3. .then()을 여러 번 사용하여 여러 개의 콜백을 추가할 수 있음 :ㅍㅊ 각각의 콜백은 주어진 순서대로 하나하나 실행하게 됨, Chaining은 Promise의 가장 뛰어난 장점

```javascript
const URL = ''
const xhr = new XMLHttpRequest()

xhr.open()
xhr.send()

xhr.onload = function () {
  if (xhr.status == 200) {
    const todo = xhr.response
	console.log(todo)
  }
}
```



## 5. Axios

- Promise based HTTP client for the browser and Node.js
- 브라우저를 위한 Promise 기반의 클라이언트
- 원래는 'XHR'이라는 브라우저 내장 객체를 활용해 AJAX 요청을 처리하는데, 이보다 편리한 AJAX 요청이 가능하도록 도움을 줌
  - 확장 가능한 인터페이스와 함께 패키지로 사용이 간편한 라이브러리를 제공

```javascript
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>

const myPromise = axios.get(URL)
console.log(myPromise)


myPromise
	.then((response) => {
		console.log(response)
		return response.data
	})

axios.get(URL)
	.then((response) => {
		console.log(response)
	})
	.catch((error) => {
		console.log(error)
	})
	.finally(function () {
		console.log('마지막에 무조건 실행합니다')
	})
```



## 6. async & await

- 비동기 코드를 작성하는 새로운 방법
- 기존 Promise 시스템 위에 구축된 syntactic sugar
- 비동기 코드를 조금 더 동기 코드처럼 표현하고 작동하게 하는 것이 가장 큰 장점























