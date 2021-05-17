# Vue - Day 1

## 1. Intro

공식문서 [↗](https://kr.vuejs.org/v2/guide/index.html)



### SPA

- 단일 페이지 애플리케이션 



- 과거 웹 사이트들은 요청에 따라 매번 새로운 페이지를 응답하는 방식
  - MPA (Muti page Form)
- 스마트폰이 등장하면서 모바일 최적화에 대한 필요성이 생김
  - 모바일 네이티브 앱과 같은 형태의 웹 페이지가 필요해짐
- 이러한 문제를 해결하기 위해 Vue와 같은 프론트엔드 프레임워크가 등장
  - CSR, SPA의 등장
- 1개의 웹페이지에서 여러 동작이 이루어지며 모바일 앱과 비슷한 형태의 사용자 경험을 제공

### CSR

- CSR (Client Side Rendering)
- 최초 요청 시 서버에서 빈 문서를 응답하고 이후 클라이언트에서 데이터를 요청해 데이터를 받아 DOM을 렌더링하는 방식
- SSR보다 초기 전송되는 페이지의 속도는 빠르지만, 서비스에서 필요한 데이터를 클라이언트에서 추가로 요청하여 재구성해야 하기 때문에 전제적인 페이지 완료 시점은 SSR보다 느림
- SPA가 사용하는 렌더링 방식
- 장점
  - 서버와 클라이언트 간의 트래픽 감소
    - 웹 어플리케이션에 필요한 모든 정적 리소스를 최초에 한번 다운로드
  - 사용자 경험 향상
    - 전체 페이지를 다시 렌더링하지 않고 변경되는 부분만을 갱식
- 단점
  - SEO (검색엔진) 최적화 문제가 있음

### SSR

- SSR (Server Side Rendering)
- 서버에서 사용자
- 장점
  - 초기 로딩 속도가 빠르기 때문에 사용자가 컨텐츠를 빨리 볼 수 있음
  - SEO 가 가능
- 단점
  - 모든 요청에 새로고침이 되기 때문에 사용자 경험이 떨어짐

### SEO

- Search Engine Optimizaion (검색 엔진 최적화)
- 웹 페이지 검색엔진이 자료를 수집하고 순위를 메기는 방식에 맞게 웹페이지를 구성해서 검색 결과의 상위에 노출될 수 있도록 하는 작업
- 인터넷 마케팅 방법 중 하나
- 구글 등장 이후 검색엔진들이 컨텐츠의 신뢰도를 파악하는 기초 지표로 사용됨
  - 다른 웹 사이트에서 얼마나 인용되었나를 반영
  - 결국 타 사이트에 인용되는 횟수를 늘리는 방향으로 최적화

### SEO 문제 대응

- Vue.js 또는 React 등의 SPA 프레임워크는 SSR을 지원하는 SEO 대응 기술이 이미 존재
  - SEO 대응이 필요한 페이지에 대해서는 선별적 SEO 대응 가능
- 추가적으로 프레임워크를 사용하기도 함
  - Nuxt
  - Next.js

### SPA with SSR

- CSR과 SSR을 적절히 사용
  - 예를 들어, django에서 axios를 활용한 좋아요/팔로우 로직의 경우 대부분은 Server에서 완성된 HTML을 제공하는 구조(SSR)
  - 다만, 특정 요소(좋아요/팔로우)만 AJAX



### Why Vue.js ?

- 현대 웹 페이지는 페이지 규모가 계속해서 커지고 있으며 그만큼 사용하는 데이터도 늘어나고, 사용자와의 상호작용도 많이 이루어짐
- 결국 Vanilla JS 만으로는 관리하기가 어려움
  - "페이스북의 친구가 이름을 변경했을 경우 변경되어야 하는 것들"
  - 타임라인의 이름, 페이스북 메시지 상의 이름, 내 주소록 등



### 결론

- Vanilla JS
  - 한 유저가 100만개의 게시글을 작성했다고 가정
  - 이 유저가 닉네임을 변경하면, 100만개의 게시글의 작성자 이름이 모두 수정되어야 함
  - '모든 요소'를 선택해서 '이벤트'를 등록하 값을 변경해야 함
- Vue.js
  - DOM과 Data가 연결되어 있으면



## 2. Concepts of Vue.js

### 2.1. MVVM Pattern

- 애플리케이션 로직은 UI로부터 분리
  - Model
  - ViewModel : `DOM과 Data의 중개자`
  - View : HTML

### Model

- "Vue에서 Model은 JavaScript Object다"
- 객체입니다



### View

- "Vue에서 Model은 HTML다"



### ViewModel

- "Vue에서 ViewModel은 모든 Vue Instance다"





### Django & Vue.js 코드 작성 순서

- Vue.js
  - "Data가 변경되면 DOM이 변경된다."
  - 1. Data 로직 작성
    2. DOM 작성



### Practice

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vue Quick Start</title>
</head>
<body>
  <!-- 2. 선언적 렌더링 -->
  <h2>선언적 렌더링</h2>
  <div id="app">
    {{ message }}
  </div>

  <!-- 3. 엘리멘트 속성 바인딩 -->
  <h2>Element 속성 바인딩</h2>
  <div id="app-2">
    <span v-bind:title="message">
      내 위에 잠시 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!
    </span>
  </div>

  <!-- 4. 조건 -->
  <h2>조건</h2>
  <div id="app-3">
    <p v-if="seen">이제 나를 볼 수 있어요</p>
  </div>

  <!-- 5. 반복 -->
  <h2>반복</h2>
  <div id="app-4">
    <ol>
      <li v-for="todo in todos">
        {{ todo.text }}
      </li>
    </ol>
  </div>

  <!-- 6. 사용자 입력 핸들링 -->
  <h2>사용자 입력 핸들링</h2>
  <div id="app-5">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">메시지 뒤집기</button>
  </div>


  <div id="app-6">
    <p>{{ message }}</p>
    <input v-model="message">
  </div>

  <!-- 1. Vue CDN -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    // 2. 선언적 렌더링
    const app = new Vue({
      el: '#app',
      data: {
        message: '안녕하세요 Vue!'
      }
    })
    // 3. 엘리먼트 속성 바인딩
    const app2 = new Vue ({
      el: '#app-2',
      data: {
        message: '이 페이지는 ' + new Date() + ' 에 로드 되었습니다'
      }
    })

    // 4. 조건
    const app3 = new Vue({
      el: '#app-3',
      data: {
        seen: false
      }
    })

    // 5. 반복
    // app4.todos = app4.$data.todos
    const app4 = new Vue({
      el: '#app-4',
      data: {
        todos: [
          { text: 'JavaScript 배우기' },
          { text: 'Vue 배우기' },
          { text: '무언가 멋진 것을 만들기' }
        ]
      }
    })

    // 6. 사용자 입력 핸들링
    const app5 = new Vue({
      el: '#app-5',
      data: {
        message: '안녕하세요! Vue.js!'
      },
      methods: {
        reverseMessage: function () {
          this.message = this.message.split('').reverse().join('')
        }
      }
    })

    const app6 = new Vue({
      el: '#app-6',
      data: {
        message: '안녕하세요 Vue!'
      }
    })
  </script>
</body>
</html>
```



## 3. Template Syntax

#### Template Syntax

- 렌더링 된 DOM을 기본 Vue 인스턴스의 데이터에 선언적으로 바인딩 할 수 있는 HTML 기반 템플릿 구문을 사용
  - Interpolation
  - Directive

### 3.1. 보간법 Interpolation

- Text
- Raw HTML
- Attributes
- JS 표현식



### 3.2. 디렉티브 Directive

- v- 접두사가 있는 특수 속성
- 속성 값은 단일 JS 표현식이 됨
- 표현식의 값이 변경될 때 반응적으로 DOM에 적용하는 역할을 함
- 전달인자 Arguments
  - `:` 콜론을 통해 전달인자를 받을 수도 있음
- 수식어 Modifiers
  - `.` 점으로 표시되는 특수 접미사
  - 디렉티브를 특별한 방법으로 바인딩 해야 함



### 3.3. Basic Syntax

#### v-text

- 엘리먼트의 textContent를 업데이트
- 내부적으로

#### v-html

- 엘리먼트의 innerHTML을 업데이트
  - XSS 공격에 취약

#### v-show

- 조건부 렌더링 중 하나
- 엘리먼트는 항상 렌더링되고 DOM에
- 단순히 엘리먼트에 display CSS 속성

#### v-if, v-else-if, v-else

- 조건부 렌더링 중 하나
- 조건에 따라 블록을 렌더링
- 디렉티브의 표현식이 true일떄만 렌더링
- 엘리먼트 및 포함된 디렉티브는 토글하는 동안 삭제되고 다시 작성됨

> v-show와 v-if
>
> - v-show
>   - CSS display 속성을 hidden으로 만들어 toggle
>   - 실제
> - v-if
>   - 전달인자가 false인 경우 렌더링 되지 않음
>   - ​

#### v-for

- 원본 데이터를 기반으로 엘리먼트 또는 템플릿 블록을 여러 번 렌더링
- item in items 구문 사용
- item 위치의 변수를 각 요소에서 사용할 수 있음
  - 객체의 경우 key
- v-for 사용 시 반드시 key 속성을 각 요소에 작성
- v-if와 함께 사용하는 경우 v-for는 v-if보다 우선순위가 높음

> 핵심요소
>
> - :key 속성 작성
> - v-if와 분리

#### v-on

- ​

#### v-bind

- HTML 요소의 속성에 Vue의 상태 데이터를 값으로 할당



#### v-model



### 3.4. 데이터 Options/Data

#### computed

- 데이터를 기반으로 하는 계산된 속성
- 함수의 형태로 정의하지만 함수가 아닌 함수의 반환 값이 바인딩 됨
- 종속된 대상을 따라 저장됨
- `종속된 대상이 변경될 때만 함수를 실행`
- 즉, Data.now()처럼 아무 곳에도 의존하지 않는 computed 속성의 경우 절대로 업데이트되지 않음
- 반드시 반환 값이 있어야 함

#### computed & method

- computed 속성 대신 methods에 함수를 정의할 수도 있음
  - 최종 결과에 대해 두 가지 접근 방식은 서로 동일
- 차이점은 computed 속성은 종속 대상을 따라 저장 됨
- 즉, computed는 종속된 대상이 변경되지 않는 한 computed에 작성된 함수를 여러 번 호출해도 계산을 다시 하지 않고 계산되어 있던 결과를 반환
- 이에 비해 method

#### watch

- 데이터를 감시
- 데이터에 변화가 일어났을 때 실행되는 함수

#### computed & watch

computed

- 특정 값이 변동하면 작업을 한다. 
- 선언형 프로그래밍 방식

watch

- 특정 값이 변동하면 다른 작업을 한다.
- 특정 대상이 변경되었을 때 콜백 함수를 실행 시키기 위한 트리거


- 특정 데이터의 변화 상황에 맞춰 다른 data 등이 바뀌어야 할 때 주로 사용
- 감시할 데이터를 지정하고 그 데이터가 바뀌면 이런 함수를 실행하라는 방식으로 소프트웨어 공학에서 이야기하는 '명령형 프로그래밍' 방식

#### filters

- 텍스트 형식화를 적용할 수 있는 필터
- interpolation 혹은 v-bind를 이용할 때 사용 가능
- 필터는 자바스크립트 표현식 마지막에 "|" 와 함께 추가되어야 함
- 체이닝 가능



### 3.5.  Lifecycle Hooks [↗](https://kr.vuejs.org/v2/guide/instance.html#%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%ED%9B%85)

- 각 Vue 인스턴스는 생성될 때 일련의 초기화 단계를 거침
  - 예를 들어 데이터 관찰 설정이 필요한 경우, 인스턴스를 DOM에 마운트하는 경우, 그리고 데이터가 변경되어 DOM를 업데이트하는 경우 등
- 그 과정에서 사용자 정의 로직을 실행할 수 있는 라이프사이클 훅도 호출됨
- ​