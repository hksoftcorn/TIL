# Vue - Day 2

- Vue CLI



## 1. SFC (Single File Component)

### 1.1. Component 컴포넌트

- 기본 HTML 엘리먼트를 확장하여 `재사용` 가능한 코드를 캡슐화 하는데 도움을 줌
- CS에서는 다시 사용할 수 있는 범용성을 위해 개발된 소프트웨어 구성 요소를 의미
- 즉, 컴포넌트는 개발을 함에 있어 유지보수를 쉽게 만들어 줄 뿐만 아니라, 재사용성의 측면에서도 매우 강력한 기능을 제공
- Vue 컴포넌트 === Vue 인스턴스



### 1.2. SFC 

- Vue의 컴포넌트 기반 개발의 핵심 특징
- 하나의 컴포넌트는 .vue라는 하나의 파일 안에서 작성되는 코드의 결과물
- 화면의 특정 영역에 대한 HTML, CSS, JavaScript 코드를 하나의 파일 (.vue)에서 관리
- 즉, .vue 확장자를 가진 싱글 파일 컴포넌트를 통해 개발하는 방식
- Vue 컴포넌트 === vue 인스턴스  (= .vue 파일)





## 2. Vue CLI

- Vue.js 개발을 위한 표준 도구
- 프로젝트의 구성을 도와주는 역할을 하며 Vue 개발 생태계에서 표준 tool 기준을 목표로 함
- 확장 플러그인, GUI, ES2015 구성 요소 제공 등 다양한 tool 제공



### 2.1. Node.js

- 자바스크립트를 브라우저가 아닌 환경에서도 구동할 수 있도록 하는 자바스크립트 런타임 환경
- 브라우저 밖을 벗어날 수 없던 자바스크립트 언어의 태생적 한계를 해결
- Chrome V8 엔진을 제공하여 여러 OS 환경에서 실행할 수 있는 환경을 제공
- 즉, 단순히 브라우저만 조작할 수 있었던 자바스크립트를 SSR에서도 사용 가능하도록 함



### 2.2. NPM (Node Package Manage)

- 자바스크립트 언어를 위한 패키지 관리자
  - python의 pip가 있다면 node.js에는 NPM
  - pip와 마찬가지로 다양한 의존성 패키지 관리
- node.js의 기본 패키지 관리자
- node.js와 함께 자동으로 설치 됨



```javascript
npm install -g @vue/cli

vue --version

vue create my-first-vue-app

npm run serve
```

- VS코드에서 (Interactive Terminal) Vue를 생성하게 됩니다.
- Vue version 2를 선택합니다.
- 자동으로 git init 과 .gitignore이 만들어져 있습니다.





### 2.3. Babel & Webpack

#### Babel

- JavaScript Transcomiler
- 자바스크립트의 신버전 코드를 구버전으로 번역/변환 해주는 도구
- 자바스크립트 역사에 있어서 파편화와 표준화의 영향으로 작성된 코드의 스펙트럽이 매우 다양
  - 최신 문버을 사용해도 브라우저의 버전별로 동작하지 않는 상황이 발생
  - 같은 의미의 다른 코드를 작성하는 등의 대응이 필요해졌고, 이러한 문제를 해결하기 위한 도구
- 원시 코드를 목적 코드으로 옮기는 번역기가 등장하면서 개발자는 더 이상 내 코드가 특정 브라우저에서 동작하지 않는 상황에 대해 크게 고민한지 않을 수 있음



#### Module

- 모듈은 단지 파일 하나를 의미
- 배경
  - 브라우저만 조작할 수 있었던 시기의 자바스크립트는 모듈 관련 문법 없이 사용 되어짐
  - 하지만 자바스크립트와 애플리케이션이 복잡해지고 크기가 커지자 전역 스코프를 공유하는 형태의 기존 개발 방식의 한계점이 드러남
  - 그래서 라이브러리를 만들어 필요한 모듈을 언제든지 불러오거나 코드를 모듈 단위로 작성하는 등의 시도가 이루어짐
- 과거 모듈 시스템
  - AMD, CommonJS, UMD
- 모듈 시스템 2015년 표준으로 등재 되었으며 현재는 대부분의 브라우저와 Node.js가 모듈 시스템을 구동



#### Module 의존성 문제

- 모듈의 수가 많아지고 라이브러리 혹은 모듈 간의 의존성이 깊어지면서 특정한 곳에서 발생한 문제가 어떤 모듈간의 문제인지 파악하기 어려워 짐(의존성 문제)
- Webpack은 모듈 간의 의존성 문제를 해결하기 위해 존재하는 도구



#### Bundler - webpack

- 모듈 의존성 문제를 해결해주는 작업이 Bunding이고 이러한 일을 해주는 도구가 Bundler 이고, Webpack은 다양한 Bundler 중 하나
- 모듈들을 하나로 묶어주고 묶인 파일은 하나로 만들어짐
- ​



#### package.json





### *Vue 살펴보기

SRC - App.vue

```
Template : HTML

Script : JavaScript

Style : CSS
```

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

SRC - NewComponent.vue

```vue
<template>
  <div>
    <h2>New Component!</h2>
  </div>
</template>

<script>
export default {
  name: ' NewComponent',
}
</script>

<style>

</style>
```

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <!-- 3. 보여주기 -->
    <NewComponent/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
// 1. 불러오기
import NewComponent from './components/NewComponent.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    // 2. 등록하기
    NewComponent
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

> 1. 불러오고
> 2. 등록하고
> 3. 보여주고





## 3. Pass Props & Emit Event

### 3.1. Pass Props & Emit Event

- 컴포넌트는 부모-자식 관계에서 가장 일반적으로 함께 사용하기 위함
- 부모는 자식에게 데이터를 전달(Pass props)하며, 자식은 자신에게 일어난 일을 부모에게 알림(Emit Evnet)
  - 부모와 자식이 명확하게 정의된 인터페이스를 통해 격리된 상태로 유지할 수 있음
- props는 아래로, event는 위로!
- 부모는 props를 통해 자식에게 데이터를 전달하고, 자식은 events를 통해 부모에게 메시지를 보냄



### 3.2. Props

- prop는 상위 컴포넌트의 정보를 전달하기 위한 사용자 지정 특성
- 하위 컴포넌트는 props 옵션을 사용하여 수신하는 props를 명시적으로 선언해야 함
- 즉, 데이터는 props 옵션을 사용하여 하위 컴포넌트로 전달 됨
- 주의 : 하위 컴포넌트의 템플릿에서 상위 데이터를 직접 참조할 수 없음

> HTML : kebab-case
>
> script : camel case



### 3.3. 단방향 데이터 흐름

- 모든 props 는 하위 속성과 상위 속성 사이의 단방향 바인딩을 형성
- 부모의 속성이 변경되면 자식 속성에게 전달되지만, 반대 방향으로는 안됨
  - 자식 요소가 의도치 않게 부모 요소의 상태를 변경함으로써 앱의 데이터 흐름을 이해히기 어렵게 만드는 일을 막기 위함
- 부모 컴포넌트가 업데이트될 때 마다 자식 요소의 모든 prop들이 최신 값으로 업데이트 됨

```vue
// App.vue
<NewComponent my-message="This is a prop data"/>
```

```vue
// NewComponent.vue

<template>
  <div>
    <h2>New Component!</h2>
    <h2>{{ myMessage }}</h2>

  </div>
  
</template>

<script>
export default {
  name: ' NewComponent',
  props: {
    myMessage: {
      type: String,
      required: true,
    },
  }
}
</script>

<style>

</style>
```



### 3.4. Emit event

- $emit(event)
  - 현재 인스턴스에서 이벤트를 트리거
  - 추가 인자는 리스너의 콜백 함수로 전달
- 부모 컴포넌트는 자식 컴포넌트가 사용되는 템플릿에서 v-on을 사용하여 자식 컴포넌트가 보낸 이벤트를 청취 (v-on을 이용한 사용자 지정 이벤트)

> Event 이름
>
> - kebab-case로 작성



## 4. Router

```vue
vue add router
y
y
npm run serve
```



### 4.1. router-link

- index.js 파일에 정의한 경로에 등록한 특정한 컴포넌트와 매핑
- HTML5 히스토리 모드에서, router-link는 클릭 이벤트를 차단하여 브라우저가 페이지를 다시 로드하지 않도록 함
- a 태그지만 우리가 알고 있는 GET 요청을 보내는 a 태그와 조금 다르게 기본 GET 요청을 보내는 이벤트를 제거한 형태로 구성



### 4.2. Vue Router

- 실제 component가 DOM에 부착되어 보이는 자리를 의미
- router-link를 클릭하면 해당 경로와 연결되어 있는 index.js에 정의한 컴포넌트가 위치



### 4.3. History mode

- HTML history API를 사용해서 router를 구현한 것
- 브라우저의 히스토리는 남기지만 실제 페이지는 이동하지 않는 기능을 지원



### 4.4. Vue Router가 필요한 이유1

#### SPA 등장 이전

- 서버가 모든 라우팅을 통제
- 요청 경로에 맞는 HTML를 제공

#### SPA 등장 이후

- 서버는 index.html 하나만 제공
- 이후 모든 처리는 HTML 위에서 JS코드를 활용해 진행
- 즉, 요청에 대한 처리를 더 이상 서버가 하지 않음 (할 필요가 없어짐)

#### 라우팅 처리 차이

- SSR : 라우팅에 대한 결정권을 서버가 가짐
- CSR : 클라이언트는 더 이상 서버로 요청을 보내지 않고 응답 받은 HTML 문서안에서 주소가 변경되면 특정 주소에 맞는 컴포넌트를 렌더링 // 이는 라우팅에 대한 결정권을 클라이언트가 가짐
- Vue Router는 라우팅의 결정권을 가진 Vue.js에서 라우팅을 편리하게 할 수 있는 Tool을 제공해주는 라이브러리



```vue
// NewComponent.vue

<template>
  <div>
    <h2>New Component!</h2>
    <h2>{{ myMessage }}</h2>
    <input @keyup.enter="childInputChange" v-model="childInputData" type="text">

  </div>
  
</template>

<script>
export default {
  name: ' NewComponent',
  data: function () {
    return {
      childInputData: '',
    }
  },
  props: {
    myMessage: {
      type: String,
      required: true,
    },
  },
  methods: {
    childInputChange: function () {
      this.$emit('child-input-change', this.childInputData)
    }
  }
}
</script>

<style>

</style>

```



```vue
// About.vue

<template>
  <div>
    <h1>this is about page</h1>
    <NewComponent my-message="this is prop data" @child-input-change="parentGetChange"/>
  </div>
</template>

<script>
// import NewComponent from '../components/NewComponent.vue'
// @ === '/src'
import NewComponent from '@/components/NewComponent.vue'

export default {
  name: 'About',
  components: {
    NewComponent
  },
  methods: {
    parentGetChange: function (textInput) {
      console.log(`hi ${textInput}`)
    }
  }
}
</script>

<style>

</style>
```



```vue
<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{name: 'Home'}">Home</router-link> |
      <router-link :to="{name: 'About'}">About</router-link>
    </div>
    <router-view/>
  </div>
</template>
```



```vue

```



### 4.5. Components vs Views

- 컴포넌트를 만들어 갈 때 정해진 구조가 있는 것은 아님
- views/
  - router(index.js)에 매핑되는 컴포넌트를 모아두는 폴더
  - i.g. App 컴포넌트 내부에 About & Home 컴포넌트 등록
- components/
  - router에 매핑된 컴포넌트 내부에 작성하는 컴포넌트를 모아두는 폴더
  - i.g. 컴포넌트 내부에 HelloWorld 컴포넌트 등록



### * Practice1 - TheLunch

```vue
vue create my-second-vue-app

cd my-second-vue-app

vue add router

npm run serve
```

```vue
// views - TheLunch.vue
// 이름에 The가 붙어있다면 하위 컴포넌트가 붙어있지 않음을 명시적으로 알려줄 수 있습니다.

<template>
  <div>
    <h2>New Component!</h2>
  </div>
</template>

<script>
export default {
  name: 'TheLunch',
}
</script>

<style>

</style>
```

```vue
// router - index.js

import TheLunch from '@/views/TheLunch.vue'

const routes = [
  {
    path: '/lunch',
    name: 'TheLunch',
    component: TheLunch
  },
```

```vue
// App.vue
<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{name: 'TheLunch'}">Lunch</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

```

```vue
// loadash를 설치합니다.

npm i --save lodash
```

```vue
// views - TheLunch.vue
// lodash를 불러옵니다.
// data는 함수로 감싸줍니다(스코프 구분)
// method를 작성합니다.
// html에서 보여줍니다.

<template>
  <div>
    <h2>점심메뉴 추천</h2>
    <button @click="pickOneLunchMenu">메뉴추천!</button>
    <p>오늘의 점심 메뉴 : {{ selectedLunchMenu }}</p>
  </div>
</template>

<script>
import _ from 'lodash'
  
  
export default {
  name: 'TheLunch',
  data: function () {
    return {
      lunch: ['계란', '마요', '밥'],
      selectedLunchMenu: '',      
    }
  },
  methods: {
    pickOneLunchMenu: function () {
      this.selectedLunchMenu = _.sample(this.lunch)
    }
  }
}
</script>

<style>

</style>
```

### * Practice2 - Lotto

```vue
// views - TheLotto.vue

<template>
  <div>
    <h2>New Component!</h2>
  </div>
</template>

<script>
export default {
  name: 'TheLotto',
}
</script>

<style>

</style>
```

```vue
// router - index.js
import TheLotto from '@/views/TheLotto.vue'


const routes = [
  {
    path: '/lotto',
    name: 'TheLotto',
    component: TheLotto
  },
```

```vue
// App.vue
<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{name: 'TheLunch'}">Lunch</router-link>
      <router-link :to="{name: 'TheLotto'}">Lotto</router-link>
    </div>
    <router-view/>
  </div>
</template>
```

```vue
// views - TheLotto.vue

<template>
  <div>
    <h2>TheLotto</h2>
    <button @click="getLottoNums">로또 추첨!</button>
    <p>{{ selectedLottoNums }}</p>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  name: 'TheLotto',
  data: function () {
    return {
      sampleNums: [],
      selectedLottoNums: [],
      }
  },
  methods: {
    getLottoNums: function () {
      const numbers = _.range(1, 46)
      this.sampleNums = _.sampleSize(numbers, 6)
      this.selectedLottoNums = _.sortBy(this.sampleNums)
    }
  }
}
</script>

<style>

</style>

```

- 동적 인자 라우팅 -> lottoNums 동적으로 바꾸기

```vue
// router - index.js
import TheLotto from '@/views/TheLotto.vue'


const routes = [
  {
    path: '/lotto/:lottoNum',
    name: 'TheLotto',
    component: TheLotto
  },
```

```vue
// views - TheLotto.vue

<template>
  <div>
    <h2>New Component!</h2>
    <button @click="getLottoNums">로또 추첨!</button>
    <p><{{ $route.params }}/p>
    <p><{{ selectedLottoNums }}/p>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  name: 'TheLotto',
  data: function () {
    return {
      sampleNums: [],
      selectedLottoNums: [],
      }
  },
  methods: {
    getLottoNums: function () {
      const numbers = _.range(1, 46)
      this.sampleNums = ._sampleSize(number, this.$route.params.lottoNum)
      this.selectedLottoNums = .sortBy(this.sampleNums)
    }
  }
}
</script>

<style>

</style>

```



### * Practice 3 - TheLunch & TheLotto

```vue
// views - TheLunch.vue
// lodash를 불러옵니다.
// data는 함수로 감싸줍니다(스코프 구분)
// method를 작성합니다.
// html에서 보여줍니다.

<template>
  <div>
    <h2>점심메뉴 추천</h2>
    <button @click="pickOneLunchMenu">메뉴추천!</button>
    <p>오늘의 점심 메뉴 : {{ selectedLunchMenu }}</p>
    // 
    <input v-model.number="lottoNum" type="number">
    <button @click="setLottoNum">Lotto</button>
  </div>
</template>

<script>
import _ from 'lodash'
  
  
export default {
  name: 'TheLunch',
  data: function () {
    return {
      lunch: ['계란', '마요', '밥'],
      selectedLunchMenu: '',      
    }
  },
  methods: {
    pickOneLunchMenu: function () {
      this.selectedLunchMenu = _.sample(this.lunch)
    }
    setLottoNum: function () {
      // 심화 : 점심 추천 이후에 로또 번호를 6개를 보여준다면
      this.$router.push({ name: 'TheLotto' }, params: { lottoNum: this.lottoNum })
	  }
  }
}
</script>

<style>

</style>
```

```vue
// App.vue
<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{name: 'TheLunch'}">Lunch</router-link>
      <router-link :to="{name: 'TheLotto', params: { lottoNum: 6}} ">Lotto</router-link>
    </div>
    <router-view/>
  </div>
</template>
```





