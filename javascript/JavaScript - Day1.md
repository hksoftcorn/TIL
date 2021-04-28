# JavaScript - Basic

> 2021.04.28.

## 1. JS 기초

### 1.1. 왜 배워야 할까

- 브라우저 화면을 '동적'으로 만들기 위함
- 브라우저를 조작할 수 있는 유일한 언어

### 1.2. 브라우저

- 웹 서버에서 이동하며 클라이언트와 서버간 양방향으로 통신하고, HTML 문서나 파일을 출력하는 GUI 기반의 소프트웨어
- 인터넷의 컨텐츠를 검색 및 열람하도록 함
- "웹 브라우저"라고도 함
  - 크롬, 파이어폭스, 사파리

### 1.3. Most Popular Language

- JavaScript 1등!


### 1.4. ECMA

- 정보 통신에 대한 표준을 제정하는 비영리 표준화 기구
- ECMAScript는 ECMA에서 ECMA-262 규격에 따라 정의한 언어
  - ECMA-262 : 범용적인 목적의 프로그래밍 언어에 대한 명세
- ECMAScript6는 ECMA에서 제안하는 6번째 표준 명세를 말함
  - ECMAScript6 = ECMAScript2015라고도 불림

### 1.5. 세미콜론

- 자바스크립트는 세미콜론을 선택적으롷 사용 가능
- 세미콜론이 없을 경우 ASI에 의해 자동으로 세미콜론이 삽입됨
  - ASI (Automatic Semicolon Insertion)
- 본 수업에서는 자바스크립트의 문법 및 개념적 측면에 집중하기 위해 세미콜론을 사용하지 않고 진행

### 1.6. 코딩 스타일 가이드

- 코딩 스타일의 핵심은 합의된 원칙과 일관성

  - 절대적인 하나의 정답은 없으며, 상황에 맞게 원칙을 정하고 일관성 있게 사용하는 것이 중요

- 코딩 스타일은 코드의 품질에 직결되는 중요한 요소

  - 나아가 코드의 가독성, 유지보수 또는 팀원과의 커뮤니케이션 등 개발 과정 전체에 영향을 끼침

  - [Airbnb Style Guide](https://github.com/airbnb/javascript)

    ​


## 2. JS 역사

### 2.1. 인물

1. 팀 버너스리
   - WWW, URL, HTML 최초 설계자
2. 브랜던 아이크
   - 모질라 설립, 자바스크립트 창립자

### 2.2. 탄생

넷스케이프에 재직중이던 브랜던 아이크가 HTML을 동적으로 동작하기 위한 회사 내부 프로젝트를 진행 중 JS를 개발

- Mocha -> LiveScript -> JavaScript

1995년 마이크로소프트에서 Jscript를 출시하여 1차 브라우저 전쟁이 시작됨.

### 2.3. 제1차 브라우저 전쟁

- 넷스케이프 vs `마이크로소프트 IE` 

1998년 넷스케이프에서 나온 브랜던 아이크 외 후계자들은 모질라 재단을 설립

### 2.4. 제2차 브라우저 전쟁

- 마이크로소프트 vs `구글 Chrome`

압도적인 속도, 강력한 개발자 도구 제공, 웹 표준을 지키는 크롬 등장

### 2.5. 파편화와 표준화

- 크로스 브라우징 이슈 : W3C에서 채택된 표준 웹 기술을 채택해 서로 다른 
- 브라우저 마다 렌더링에 사용하는 엔진이 다르기 때문
- ECMAScript (ES1) 탄생

### 2.6. 현재의 JS

- 2015년 ES6 탄생
- JS의 고질적인 문제들을 해결
- JS의 다음 시대라고 불리울 정도로 많은 혁신과 변화를 맞이한 버전
- Vanilla JavaScript



## 3. JavaScript 개념

- DOM 조작
  - 문서(HTML) 조작
- BOM 조작
  - navigator, screen, location, frames, history, XHR
- JavaScript Core (ECMAScript)
  - Data structure

### 3.1. DOM

#### DOM (Document Object Method)

```javascript
window.document.title
```

- HTML, XML 등과 같은 문서를 다루기 위한 언어 독립적인 문서 모델 인터페이스
- 문서를 구조화하고 구조화된 구성 요소를 하나의 객체로 취급하여 다루는 논리적 트리 모델
- 문서가 구조화되어 있으며 각 요소는 객체로 취급
- 단순한 속성 접근, 메서드 활용 뿐만 아니라 프로그래밍 언어적 특성을 활용한 조작 가능
- 주요 객체
  - Window
  - document
  - navigator, location, history, screen

#### DOM - 해석

- Parsing (파싱)
  - 구문 분석, 해석



### 3.2. BOM

#### BOM (Browser Object Model)

- 자바스크립트가 브라우저와 소통하기 위한 모델
- 브라우저의 창이나 프레임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단
  - 버튼, URL 입력창, 타이틀 바 등 브라우저 윈도우 및 웹 페이지의 일부분을 제어 가능

```python
window.print()
window.open()
window.confirm()
```



## 4. DOM 조작

*"DOM  : 선택(Select) 변경(Manipulation)한다."*

### 4.1. DOM 관련 객체의 상속 구조

- EventTarget
- Node
- Element
- Document
- HTML

### 4.2. DOM 선택 - 선택 관련 메서드

- Document.querySelector()
  - 제공한 선택자와 일치하는 element 하나 선택
  - 제공한 CSS selector를 만족하는 첫 번째 element객체를 반환
- Document.querySelectorAll()
  - 제공한 선택자와 일치하는 여러 element 선택
  - 매칭할 하나 이상의 셀렉터를 포함하는 유효

#### DOM 메소드

- Document.querySelector() : 단일 element
  - getElementById()
  - querySelector()
- Document.querySelectorAll() : 복수 element
  - HTML Collection
    - getElementsByTagName()
    - getElementsByClassName()
  - NodeList
    - querySelectorALl()

#### DOM 선택 - HTML Collection & NodeList

- 둘 다 배열과 같이 각 항목을 접근하기 위한 인덱스를 제공 (유사 배열)
- Live Collection
- HTML Collection
  - name, id, 인덱스 속성으로 각 항목들에 접근 가능
- NodeList
  - 인덱스 번호로만 각 항목들에 접근 가능
  - 단, HTMLCollection과 달리 배열에서 사용하는 for each 함수 및 다양한 메서드 활용 가능
  - 주의! querySelectorAll() 는 Static Collection입니다.

#### DOM 선택 - Live Collection vs Static Collection

- Live Collection
  - 문서가 바뀔 때 실시간으로 업데이트
  - DOM의 변경사항을 실시간으로 collection 에 반영
- Static Collection
  - DOM이 변경되어도 collection 내용에는 영향을 주지 않음

```javascript
const h1 = document.querySelector('h1')
h1

const h2 = document.querySelector('h2')
h2

const secondH2 = document.querySelector('#location-header')
secondH2

const secondLiTags = document.querySelectorAll('.ssafy-location')
secondLiTags
```

#### DOM 변경 - Create

- Document.createElement()
  - 주어진 태그명을 사용해 HTML 요소를 만들어 반환
- ParentNode.append()
  - 특정 부모 노드의 자식 노드 리스트 중 마지막 자식 다음에 Node 객체나 DOMString을 삽입 (반환 값 없음)
  - 여러 개의 Node 객체, DOMString을 추가 할 수 있음
- Node.appendChild()
  - 한 노드를 특정 부모 
  - 하나만 추가 가능

#### DOM 변경 - Remove

- ChildNode.remove()
- Node.removeChild()

```javascript
let el = document.querySelector('#content')
el.remove()

let parent = document.querySelector('#parent')
let child = document.querySelector('#child')
let oldChild = parent.removeChild(child)
```

#### DOM 변경 - 변경 관련 속성 (property)

- Node.innerText
  - 노드와 그 자손의 텍스트 컨텐츠를 표현
  - 즉, 줄 바꿈을 인식하고 숨겨진 내용을 무시하는 등 최종적으로 스타일링이 적용 된 모습으로 표현
- Element.innerHTML
  - 요소 내에 포함 된 HTML 마크업을 반환
  - XSS공격에 취약점이 있으므로 사용시 주의
    - xss : 공격자가 웹 사이트 클라이언트 측 코드에 악성 스크립트를 삽입해 공격하는 방법

#### DOM 변경 - 변경 관련 메소드

- Element.setAttribute(name, value)
  - 지정된 요소의 값을 설정
- Element.getAttribute()
  - 해당 요소의 지정된 값을 반환

> DOM 조작
>
> 1. 선택한다
> 2. 변경한다



## 5. JS기본 실습

```javascript
<script>
    //1. Selection - querySelector / querySelectorAll
    //1-1. window & document
    // console.log(window)
    // console.log(document)
    // console.log(window.document)

    //1-2. querySelector
    const mainHeader = document.querySelector('h1')
    const langHeader = document.querySelector('#lang-header')
    const frameHeader = document.querySelector('#frame-header')
    // console.log(mainHeader, langHeader, frameHeader)

    //1-3. querySelectorAll
    const langLi = document.querySelectorAll('.lang')
    const frameworkLi = document.querySelectorAll('.framework')
    // console.log(langLi, frameworkLi)

    //1-4. 여러 개의 요소 -> 첫 번째로 일치하는 요소
    const selectOne = document.querySelector('.lang')
    // console.log(selectOne)
    
    //1-5. 복합 선택자
    const selectDescendant = document.querySelector('body li')
    // const selectDescendant = document.querySelectorAll('body li')
    const selectChild = document.querySelector('body > li')
    // console.log(selectDescendant) 
    // console.log(selectChild) 

    //1-6. getElementById, getElementByClassName 
    const selectSepcialLang = document.getElementById('special-lang')
    const selectAllLangs = document.getElementsByClassName('framework')
    const selectAllLiTags = document.getElementsByTagName('li')
    // console.log(selectSepcialLang, selectAllLangs, selectAllLiTags)

    
    //2. Manipulation
    //2-1. Creation
    const browserHeader = document.createElement('h2')
    const ul = document.createElement('ul')
    const li1 = document.createElement('li')
    const li2 = document.createElement('li')
    const li3 = document.createElement('li')
    // console.log(browserHeader, li1, li2, li3) 


    //2-2. innerText & innerHTML / append & appendChild
    browserHeader.innerText = 'Browsers'
    li1.innerText = 'IE'
    li2.innerText = '<strong>FireFox</strong>'
    li3.innerHTML = '<strong>Chrome</strong>'
    // console.log(browserHeader, li1, li2, li3)

    //2-3. append DOM 
    const body = document.querySelector('body')
    body.appendChild(browserHeader)
    body.appendChild(ul)

    ul.append(li1, li2, li3) 
    // ul.appendChild(li1, li2, li3) 

    //2-4. Delete
    // removeChild
    // ul.removeChild(li1) 
    // ul.removeChild(li2)

    // remove
    // ul.remove()
    // body.remove()

    //2-5. Element Styling
    // li1.style.cursor = 'pointer'
    // li2.style.color = 'blue'
    // li3.style.background = 'red'

    // setAttribute
    // li3.setAttribute('id', 'king')

    // getAttribute
    // const getAttr = li1.getAttribute('style')
    // const getAttr2 = li3.getAttribute('style')
    // console.log(getAttr)
    // console.log(getAttr2)
</script>
```

```javascript
// 01_healthSurvey.html

<script>
    // 1. 배경색 설정
    const mainBackground = document.querySelector('body')
    //mainBackground.id = 'main'
    mainBackground.setAttribute('id', 'main')

    // 2. 요소 중앙 정렬
    const nav = document.querySelector('nav')
    const header = document.querySelector('header')
    const section = document.querySelector('section')

    nav.setAttribute('class', 'box-container')
    header.setAttribute('class', 'box-container')
    section.setAttribute('class', 'box-container')

    // 3. 테두리 설정
    const mainQuestionDivList = document.querySelectorAll('section div')
    // console.log(mainQuestionDivList)
    mainQuestionDivList.forEach( function (div) {
      div.setAttribute('class', 'box-item')
    })

    // 4. 버튼 변경
    const myButton = document.querySelector('form > input')
    // console.log(myButton)
    myButton.setAttribute('class', 'button')
    myButton.setAttribute('value', '제출')

    // 5. 이미지 사이즈
    const myImage = document.querySelector('nav img')
    // console.log(myButton)
    myImage.width = '600'

    // 6. footer 생성
    const myFooter = document.createElement('footer')
    myFooter.innerText = 'Google설문지를 통해 비밀번호를 제출하지 마시오.'
    // console.log(myFooter)
    
    const myBody = document.querySelector('body')
    myBody.appendChild(myFooter)
    myFooter.setAttribute('class', 'box-container')

    // 7. Input요소 스타일링
    const myInput = document.querySelector('#name')
    myInput.style.marginTop = '50px'
</script>
```



> **Wrap up**
>
> 자바스크립트는 브라우저 화면을 동적으로 움직이게 하고 싶어 탄생했습니다.
>
> 조작방법은 선택 → 조작 으로 이루어집니다.

 

## 6. Event

### Event

- 네트워크 활동 혹은 사용자와의 상호작용 겉은 사건의 발생을 알리기 위한 객체
- 이벤트는 마우스를 클릭하거나 키보드를 누르는 등 사용자 행동에 의해 발생할 수도 있고, 특정 메서드를 호출하여 프로그래밍적으로도 만들어낼 수 있음
- 이벤트 처리기
  - EventTarget.addEvnetListerner()
  - 해당 메서드를 통해 다양한 요소에서 이벤트를 붙일 수 있음
  - removeEventListener() 를 통해 이벤트를 제거 가능

### Event 기반 인터페이스

- AnimationEvent, ClipboardEvent, DragEvent 등
- 그 중에서도 'UIEvet'
  - 간단한 사용자 인터페이스 이벤트
  - Event의 상속을 받음
  - MouseEvent, KeyboardEvent, InputEvent, FocusEvent 등 부모 객체 역할을 함

### Event handler

*"특정 이벤트가 발생하면 할 일을 등록하다"*

- EventTarget.addEventListener()

- 지정한 이벤트가 대상에 전달될 때마다 호출할 함수를 설정

- 이벤트를 지원하는 모든 객체를 대상으로 지정 가능

- target.addEventListener(type, listener[, options])

  - type : 반응 할 이벤트 유형 

  - listener : 지정된 타입의 이벤트가 발생했을 때 알림을 받는 객체

    EventListener 인터페이스 혹은 JS function 객체(콜백 함수)여야 함

```javascript
const btn = document.querySelector('button')
btn.addEventListener('click', function (event) {
  alert('버튼이 클릭되었습니다.')
  console.log(event)
})
```

- preventDefault()
  - 현재 이벤트의 기본 동작을 중단
  - 태그의 기본 동작 (a 태그는 클릭 시 페이지 이동, form 태그는 폼 데이터를 전송)
  - 이벤트의 전파를 막지 않고 이벤트의 기본동작만 중단



## 7. 변수와 식별자

### 식별자 정의와 특징

- 식별자는 변수를 구분할 수 있는 변수명을 말함
- 식별자는 반드시 문자, 달러 또는 밑줄로 시작
- 대소문자를 구분하며, 클래스명 외에는 모두 소문자로 시작
- 예약어 사용 불가능

### 식발자 작성 스타일

- 카멜 케이스
  - 변수, 객체, 함수에 사용
- 파스칼 케이스
  - 클래스, 생성자에 사용
- 대문자 스네이크 케이스
  - 상수에 사용

### 변수 선언 키워드 (let, const)

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
> - 선언 (Declaration)
>   - 변수를 생성하는 행위 또는 시점
> - 할당 (Assignment)
>   - 선언된 변수에 값을 저장하는 행위 또는 시점
> - 초기화 (Initialization)
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

