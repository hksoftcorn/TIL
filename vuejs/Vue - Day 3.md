# Vue - Day 3

- Vuex
- Vuex core concept
- Todo app with Vuex



## 1. Vuex

*"상태 + 중앙 저장소"*

- Statement management pattern + library for vue.js
- 상태 관리 패턴 + 라이브러리
- 상태를 전역 저장소로 관리할 수 있도록 지원하는 라이브러리
  - state가 예측 가능한 방식으로만 변경될 수 있도록 보장하는 규칙 설정
  - 애플리케이션의 모든 컴포넌트에 대한 `중앙 집중식 저장소` 역할
- Vue의 공식 devtools와 통합되어 기타 고급 기능을 제공



### 1.1. State

- state는 data이며, 해당 어플리케이션의 핵심이 되는 요소
- DOM은 Data에 반응하여 DOM을 렌더링



### 1.2. Pass props & Emit Event

- 각 컴포넌트는 독립적으로 데이터를 관리
- 데이터는 단방향 흐름으로 부모 -> 자식 간의 전달만 가능하며 반대의 경우 이벤트를 통해 전달
  - 장점 : 데이터의 흐름을 직관적으로 파악 가능
  - 단점 : 컴포넌트 중첩이 깊어지는 경우 동위 관계의 컴포넌트로의 데이터 전달이 불편해짐



### 1.3. IN VUEX

- 중앙 저장소에서 state를 모아놓고 관리
- 규모가 큰 프로젝트에 매우 편리
- 각 컴포넌트에서는 중앙 집중 저장소의 state만 신경쓰면 됨
- 이를 공유하는 다른 컴포넌트는 알아서 동기화





## 2. Vuex core concept

### 2.1. 단방향 데이터 흐름

- 상태는 앱을 작동하는 원본 소스 (data)
- 뷰는 상태의 선언적 매핑
- 액션은 뷰에서 사용자 입력에 대해 반응적으로 상태를 바꾸는 방법 (methods)

> 단점
>
> - 공통의 상태를 공유하는 여러 컴포넌트가 있는 경우 빠르게 복잡해짐
> - i.g. 지나치게 중첩된 컴포넌트 props



### 2.2. 상태 관리 패턴

- 컴포넌트의 공유된 상태를 추출하고 이를 전역에서 관리하도록 함
- 컴포넌트는 커다란 뷰가 되며 모든 컴포넌트는 트리에 상관없이 상태에 엑세스 하거나 동작을 트리거할 수 있음
- 상태 관리 및 특정 규칙 적용과 관련된 개념을 정의하고 분리함으로써 코드의 구조와 유지 관리 기능 향상



### 2.3. 구성 요소

- STATE
- ACTIONS
- MUTATIONS
- GETTERS

![VUEX](https://vuex.vuejs.org/vuex.png)

#### (1) STATE

- 중앙에서 관리하는 모든 상태 정보 (DATA)
- MUTATIONS에 정의된 메서드에 의해 변경
- 여러 컴포넌트 내부에 있는 특정 STATE를 중앙에서 관리
  - 이전의 방식은 SSTATE를 찾기 위해 각 컴포넌트를 직접 확인
  - VUEX를 활용하는 방식은 VUEX STORE에서 컴포넌트에서 사용하는 STATE를 한 눈에 파악 가능
- STATE가 변화하면 해당 STATE를 공유하는 컴포넌트의 DOM은 알아서 렌더링
- 컴포넌트는 이제 Vue Store에서 state 정보를 가져와 사용합니다.
- `DISPATCH()`를 사용하여 ACTIONS 내부의 메서드를 호출합니다.



#### (2) ACTIONS

- COMPONENT에서 DISPATCH() 메서드에 의해 호출
- BACKEND API와 통신하여 Data Fetching 등의 작업을 수행
  - 동기적인 작업 뿐만 아니라 비동기적인 작업을 포함 가능
- 항상 context가 인자로 넘어옴
  - store.js 파일 내에 있는 모든 요소에 접근해서 속성 접근 & 메서드 호출이 가능
  - 단, (가능은 하지만) state를 직접 변경하지 않습니다.
- MUTATIONS에 정의된 메서드를 `COMMIT()` 메서드로 호출
- state는 오로지 mutations 메서드를 통해서만 조작
  - 명확한 역할 분담을 통해 서비스 규모가 커져도 state를 올바르게 관리하기 위함



#### (3) MUTATIONS

- Actions에서 commit()메서드에 의해 호출
- 비동기적으로 동작하면 state가 변화하는 시점이 달라질 수 있기 때문에 동기적인 코드만 작성
- mutations에 정의하는 메서드의 첫 번째 인자로 state가 넘어옴



#### (4) GETTERS

- state를 변경하지 않고 활용하여 계산을 수행 (computed와 유사)
  - 실제 계산된 값을 사용하는 것처럼 getters는 저장소의 상태를 기준으로 계산
  - i.g. state에 todo list의 해야 할 일의 목록의 경우  todo가 완료된 목록만 필터링해서 보여줘야 하는 경우가 있음
  - getters에서 completed의 값이 true인 요소가 필터링 해서 계산된 값을 담아 놓을 수 있음
  - getters 자체가  state 자체를 변경하지는 않음
  - state를 특정한 조건에 따라 구분만 함
  - 즉, 계산된 값을 가져옴



### 2.4. 실습

#### (1) 환경설정 - 모델링

```VUE
$ vue create todo-list-vuex-project
$ cd todo-list-vuex-project/
$ vue add vuex
```

- App
  - TodoFrom
  - TodoList
    - TodoListItem

```vue
// TodoListItem

<template>
  <div>ToDo</div>
</template>

<script>
export default {
  name: 'TodoListItem'

}
</script>

<style>

</style>
```

```vue
// TodoList

<template>
  <div>
    <TodoListItem/>
  </div>
</template>

<script>
import TodoListItem from '@/components/TodoListItem'

export default {
  name: 'TodoList',
  components: {
    TodoListItem,
  }

}
</script>

<style>

</style>
```

```vue
// TodoFrom

<template>
  <div>TodoForm</div>
</template>

<script>
export default {
  name: 'TodoForm'
}
</script>

<style>

</style>
```

```vue
// App

<template>
  <div id="app">
    <TodoForm/>
    <TodoList/>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import TodoForm from '@/components/TodoForm'

export default {
  name: 'App',
  components: {
    TodoList,
    TodoForm,
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



#### (2) store 작성1 - STATE

```vue
// store - index.js

import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    todos: [
      {
        title: '할 일1',
        completed: false,
      },
      {
        title: '할 일2',
        completed: false,
      },
    ]
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})

```

- store에 접근할 때 미리 계산하는 computed를 활용합니다.
- 데이터에 변경이 있을 때만 연산하게 됩니다.
- TodoListItem 에게 props로 데이터를 내려줍니다 `:todo="todo"`

```vue
// TodoList

<template>
  <div>
    <TodoListItem v-for="(todo, idx) in $store.state,todos" :key="idx"/>
  </div>
</template>
```

```vue
// TodoList
<template>
  <div>
    <TodoListItem v-for="(todo, idx) in todos" :key="idx" :todo="todo"/>
  </div>
</template>

<script>
import TodoListItem from '@/components/TodoListItem'

export default {
  name: 'TodoList',
  components: {
    TodoListItem,
  },
  computed: {
    todos: function () {
      return this.$store.state.todos
    }
  }

}
</script>
```

```vue
// TodoForm

<template>
  <div>
    <input type="text" v-model="todoTitle" @keyup.enter="createTodo">
    <button @click="createTodo">Add</button>
  </div>
</template>

<script>
export default {
  name: 'TodoForm',
  data: function () {
    return {
      todoTitle: '',
    }
  },
  methods: {
    createTodo: function () {
      console.log('Todo 생성')
    }
  }

}
</script>

<style>

</style>
```



#### (3) store 작성2 - MUTATIONS

- mutations는 data를 조작하는 메소드를 작성합니다.
- state만 조작
- 함수를 CAPITAL로 작성하게 됩니다. 이는 데이터를 조작하는 중요한 역할을 하기때문에 명시적으로 작성한 것입니다.
- commit을 통해 수행

```vue
// index.js

  mutations: {
    CREATE_TODO: function (state, todoItem) {
     // console.log(state)
		state.todos.push(todoItem)
    }
  },
```

```vue
// TodoForm


<script>
export default {
  name: 'TodoForm',
  data: function () {
    return {
      todoTitle: '',
    }
  },
  methods: {
    createTodo: function () {
      const todoItem = {
        title: this.todoTitle,
        completed: false,
      }
      // console.log('Todo 생성')
      // 메소드가 없는 관계로 (원래는 이렇게 하면 안되지만)
      // mutations에서 직접 호출합니다.
      this.$store.commit('CREATE_TODO', todoItem)
      // 초기화
      this.todoTitle = ''
    }
  }

}
</script>



```



#### (4) store 작성3 - ACTIONS

- actions에는


- context가 첫 번째 인자로 들어가있습니다. context에는 많은 데이터가 있음. 이는 모든 속성에 접근 가능 -> 다양한 조작 가능.
- dispatch를 통해 수행

```vue
// index.js

  actions: {
    createTodo: function (context) {
      console.log(context)
    }
```

```vue
// TodoForm


<script>
export default {
  name: 'TodoForm',
  data: function () {
    return {
      todoTitle: '',
    }
  },
  methods: {
    createTodo: function () {
      const todoItem = {
        title: this.todoTitle,
        completed: false,
      }
      // 빈 공백 값을 제외합니다.
      if (todoItem.title.trim()) {
        this.$store.dispatch('createTodo', todoItem)
      }
      this.todoTitle = ''
    }
  }

}
</script>
```

- MUTATIONS은 ACTIONS을 통해서 호출해야 합니다.
- context.commit (MUTATIONS, PAYLOAD)

```javascript
// index.js

import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    todos: [
    ]
  },
  mutations: {
    CREATE_TODO: function (state, todoItem) {
      // console.log(state)
      state.todos.push(todoItem)
     
    }
  },
  actions: {
    createTodo: function (context, todoItem) {
      // console.log(context)
      context.commit('CREATE_TODO', todoItem)
    }
  },
  modules: {
  }
})

```

> 정리
>
> 1. 컴포넌트에서 `dispatch`를 활용하여 actions를 호출
> 2. action에 정의된 메서드는 `commit`을 활용해 mutations를 호출
> 3. mutation에 정의된 메서드는 `state`를 조작



- context에서 commit만 쓰겠다면 { }

```javascript
actions: {
    createTodo: function ({ commit }, todoItem) {
      // distructuring 분해
      commit('CREATE_TODO', todoItem)
    }
  },
```

```javascript
node 00_destructuring.js
```





### 2.5. 실습 2

#### (1) Delete

- 각 요소마다 Delete 버튼을 달아줍니다.
- dispatch() 호출을 통해 Action을 호출합니다.
- Action은 commit()을 통해 해당 요소의 index를 반환받아서, splice() 삭제를 진행하게 됩니다.
- splice() : 이 메서드는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경합니다.

```vue
// TodoListItem.vue

<template>
  <div>
    <span>{{ todo.title }}</span>
    <button @click="deleteTodo">Delete</button>
  </div>
</template>

<script>
export default {
  name: 'TodoListItem',
  props: {
    todo: {
      type: Object,
    }
  },
  methods: {
    deleteTodo: function () {
      // console.log('Todo 삭제')
      this.$store.dispatch('deleteTodo', this.todo)
    }
  }
}
</script>

<style>

</style>
```

```javascript
// index.js

import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    todos: []
  },
  mutations: {
    CREATE_TODO: function (state, todoItem) {
      // console.log(state)
      state.todos.push(todoItem)
    },
    DELETE_TODO: function (state, todoItem) {
      // console.log(state)
      // 1. todoItem이 첫 번째로 만나는 요소의 index를 가져옴
      const index = state.todos.indexOf(todoItem)
      // 2. 해당 인덱스 1개만 삭제하고 나머지 요소를 토대로 새로운 배열 생성
      state.todos.splice(index, 1)
    }
  },
  actions: {
    createTodo: function ({ commit }, todoItem) {
      // console.log(context)
      // context.commit('CREATE_TODO', todoItem)
      // distructuring 분해
      commit('CREATE_TODO', todoItem)
    },
    deleteTodo: function ({ commit }, todoItem) {
      commit('DELETE_TODO', todoItem)
    }
  },
  modules: {
  }
})

```

#### (2) Update

- 토글기능을 추가합니다. todoListItem을 선택하면 completed의 값이 변경됨에 따라 ~~중간선~~이 그어지고 사라지게 됩니다.


- ```vue
  console.log(state)
  console.log(todoItem)
  // todoItem 안의 completed 값을 바꾸어줍니다.
  ```

- spread_syntax.js [↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

  - ```javascript
    let obj1 = { foo: 'bar', x: 42 };
    let obj2 = { foo: 'baz', y: 13 };

    let clonedObj = { ...obj1 };
    // Object { foo: "bar", x: 42 }

    let mergedObj = { ...obj1, ...obj2 };
    // Object { foo: "baz", x: 42, y: 13 }
    ```



```vue
// TodoListItem.vue

<template>
  <div>
    <span @click="updateTodo">{{ todo.title }}</span>
    <button @click="deleteTodo">Delete</button>
  </div>
</template>

<script>
export default {
  name: 'TodoListItem',
  props: {
    todo: {
      type: Object,
    }
  },
  methods: {
    deleteTodo: function () {
      // console.log('Todo 삭제')
      this.$store.dispatch('deleteTodo', this.todo)
    },
    updateTodo: function () {
      // console.log('Todo 수정')
      this.$store.dispatch('updateTodo', this.todo)
      
    }
  }
}
</script>

<style>

</style>
```

```javascript
// index.js

	UPDATE_TODO: function (state, todoItem) {
      // completed 상태를 -1 바꾸어 줍니다.
      state.todos = state.todos.map((todo) => {
        if (todo === todoItem) {
          return { ...todo, completed: !todo.completed }
        }
        return todo
      })
    }
```

- 취소선 style 추가

```javascript
// TodoListItem.vue
<template>
  <div>
    <span 
	  @click="updateTodo"
	  :class="{ completed: todo.completed }"
	>{{ todo.title }}</span>
    <button @click="deleteTodo">Delete</button>
  </div>
</template>

...

<style scoped>
  .completed {
    text-decoration: line-through;
  }
</style>
```



#### (3) store 작성4 - GETTERS

- 완료된 TODO 개수 반환하기

```javascript
  getters: {
    completedTodosCount: function (state) {
      return state.todos.filter((todo) => {
        return todo.completed == true
      }).length
    }
  },
```

- computed로 받아오기

```vue
// App.vue

<template>
  <div id="app">
    <h1>Todo App</h1>
    <h2>completedTodosCount: {{ completedTodosCount }}</h2>
    <TodoForm/>
    <TodoList/>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import TodoForm from '@/components/TodoForm'

export default {
  name: 'App',
  components: {
    TodoList,
    TodoForm,
  },
  computed: {
    completedTodosCount: function () {
      return this.$store.getters.completedTodosCount
    }
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

#### (4) HELPER Binding

- mapState 
- mapGetters
- mapActions

```vue
<template>
  <div>
    <TodoListItem 
      v-for="(todo, idx) in todos" 
      :key="idx"
      :todo="todo"
    />
  </div>
</template>

<script>
import { mapState } from 'vuex'
import TodoListItem from '@/components/TodoListItem'

export default {
  name: 'TodoList',
  components: {
    TodoListItem,
  },
  computed: {
    // todos: function () {
    //   return this.$store.state.todos
    // }
    ...mapState([
      'todos',
    ])
  }
}
</script>

<style>

</style>
```

```vue
<template>
  <div>
    <span 
      @click="updateTodo(todo)"
      :class="{ completed: todo.completed }"
    >{{ todo.title }}</span>
    <button @click="deleteTodo(todo)">Delete</button>
  </div>
</template>

<script>
import { mapActions } from 'vuex'

export default {
  name: 'TodoListItem',
  props: {
    todo: {
      type: Object,
    }
  },
  methods: {
    // deleteTodo: function () {
    //   this.$store.dispatch('deleteTodo', this.todo)
    // },
    // updateTodo: function () {
    //   this.$store.dispatch('updateTodo', this.todo)
    // }
    ...mapActions([
      'deleteTodo',
      'updateTodo'
    ])
  }
}
</script>

<style scoped>
  .completed {
    text-decoration: line-through;
  }
</style>
```

- mapAction의 인자는 호출시에 파라미터에 넣어주면 됩니다.

```vue
import { mapGetters } from 'vuex'  

  computed: {
    ...mapGetters([
      'completedTodosCount',
      'uncompletedTodosCount'
    ])
  }
```



### 2.6. 저장하기 [↗](https://www.npmjs.com/package/vuex-persistedstate)

```
npm install --save vuex-persistedstate


// index.js

import createPersistedState from "vuex-persistedstate";

...
plugins: [createPersistedState()],

```

- 브라우저 - 로컬 스토리지에 데이터를 저장하게 됩니다.
- Application - Local Storage



## 3. 정리

### Vuex 언제 사용해야 할까?

*"Flux는 안경과 같습니다, 필요할 때 유용성을 알아볼 수 있습니다."*

- Vuex는 공유된 상태 관리를 처리하는 데 유용하지만, 개념에 대한 이해와 시작하는 비용이 큼

- 앱이 단순하다면 Vuex가 없는 것이 더 괜찮을 수 있음

- 그러나 중대형 규모의 SPA를 구축하는 경우 Vuex는 자연스럽게 선택할 수 있는 단계가 오게 됨

- 즉, 필요한 순간이 왔을 때 사용하는 것을 권장

  ​

