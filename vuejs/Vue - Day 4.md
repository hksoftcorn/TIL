# Vue - Day 4

- Server & Client
- CORS
- Authentication & Authorization
- JWT



## 1. Server & Client

### 1.1. Server

- 클라이언트에게 '정보', '서비스'를 제공하는 컴퓨터 시스템
- 정보 & 서비스
  - Django를 통해 응답한 template 
  - 혹은 DRF를 통해 응답한 JSON

### 1.2. Client

- 서버에게 그 서버가 맞는 서비스를 요청하고
- 서비스 요청을 위해 필요한 인자를 서버가 원하는 방식에 맞게 제공하며
- 서버로부터 반환되는 응답ㄷ을 사용자에게 적절한 방식으로 표현하는 기능을 가진 시스템

> Server는 정보 제공
>
> - DB와 통신하며 데이터를 CRUD
> - 요청을 보낸 Client에게 이러한 정보를 응답
>
> Clent는 정보 요청 & 표현
>
> - Server에게 정보 요청
> - 응답 받은 정보를 잘 가공하여 화면에 보여줌



## 2. CORS

### 2.1. SOP (Same-Origin Policy)

- 두 URL의 `Protocol`, `Port`, `Host`가 모두 같아야 동일한 출처라고 할 수 있음

- 동일 출처 정책

- 특정 출처에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용하는 것을 제한하는 보안 방식

- 잠재적으로 해로울 수 있는 문서를 분리함으로써 공격받을 수 있는 경로를 줄임


> 출처가 다른 경우
>
> - 프로토콜이 다른 경우
> - 포트가 다른 경우
> - 호스트가 다른 경우



### 2.2. CORS (Cross-Origin Resource Sharing)

- 교차 출처 리소스(자원) 공유
- 추가 HTTP header를 사용하여, 특정 출처에서 실행중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제
- 리소스가 자신의 출처와 다를 때 교차 출처 HTTP 요청을 실행
- 보안 상의 이유로 브라우저는 교차 출처 HTTP 요청을 제한 (SOP)
  - XMLHttpRequest는 SOP을 따름
- 다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS header를 포함한 응답을 반환해야 함

#### CORS Policy

- 교차 출처 리소스 공유 정책
- 다른 출처에서 온 리소스를 공유하는 것에 대한 정책
- ↔ SOP

#### 교차 출처 접근 허용하기

- CORS를 사용해 교차 출처 접근 허용
- CORS는 HTTP의 일부로, 어떤 호스트에서 자신의 컨텐츠를 불러갈 수 있는지 서버에 지정할 수 있는 방법

#### Why CORS ?

- 브라우저 & 웹 애플리케이션 보호
  - 악의적인 사이트의 데이터를 가져오지 않도록 사전 차단
  - 응답으로 받는 자원에 대한 최소한의 검증
  - 서버는 정상적으로 응답하지만 브라우저에서 차단
- Server의 자원 관리
  - 누가 해당 리소스에 접근할 수 있는지 관리 기능

#### How CORS ? 

- CORS 표준에 의해 추가된 HTTP Header를 통해 이를 통제
- CORS HTTP 응답 헤더 예시
  - `Access-Control-Allow-Origin`
  - Access-Control-Allow-Credentials
  - Access-Control-Allow-Headers

#### Access-Control-Allow-Origin 응답 헤더

- 이 응답이 주어진 출처으로부터 요청 코드와 공유될 수 있는 지를 나타냄

- 예시

  - Access-Control-Allow-Origin: *
  - 브라우저 리소스에 접근하는 임의의 origin으로부터 요청을 허용한다고 알리는 응답에 포함
  - '*'는 모든 도메인에서 접근할 수 있음을 의미
  - '*'외에 특정 origin 하나를 명시할 수 있음

- CORS 시나리오 예시

  - Vue.js에서 A 서버로 요청
  - A 서버는 Access-Control-Allow-Origin에 특정한 origin을 포함 시켜 응답
    - 서버는 CORS Policy와 직접적인 연관이 없고 그저 요청에 응답함
  - 브라우저는 응답에  Access-Control-Allow-Origin 를 확인 후 허용 여부를 결정
  - 프레임워크 별로 이를 지원하는 라이브러리가 존재 : django-cors-headers 라이브러리 이용

- CORS 시나리오 예시

  - 요청 헤더의 Origin을 보면 localhost:8000으로 부터 요청이 왔다는 것을 알 수 있음
  - 서버는 이에 대한 응답으로 Access-Control-Allow-Origin 헤더를 다시 전송
  - 만약 서버의 리소스 소유자가 오직 localhost:8000의 요청만 리소스에 대한 접근을 허용하려는 경우 '*'가 아닌 'Access-Control-Allow-Origin: localhost: 8000'을 전송해야 함

  

## 3. Practice

- client & server

### 3.1. Server

```python
cd server/
python -m venv venv
source venv/Scripts/activate
pip install -r requirements.txt 
python manage.py migrate
python manage.py runserver
```

#### *Postman

```
http://127.0.0.1:8000/todos/
POST 
Body - form-data
title : 할일 1
(completed : false default값으로 설정)
```

```
http://127.0.0.1:8000/todos/1/
PUT
Body - form-data
title : 수정된 할일 1
```

```
http://127.0.0.1:8000/todos/1/
DELETE
status=status.HTTP_204_NO_CONTENT
```



### 3.2. Client

```vue
npm install
npm run serve
```

- TodoList 에서 Get Todos 누르면 에러 메세지 출력됨

  `Failed to load resource: net::ERR_CONNECTION_REFUSED`

- 서버는 응답을 해주었지만, 브라우저에서 CORS가 없어서 블락한 상황입니다.



### 3.3. Server : Django-cors [↗](https://pypi.org/project/django-cors-headers/)

```python
# settings.py

INSTALLED_APPS = [
    ...
    'corsheaders',
    ...
],


MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    ...
],


# (1) 선택 1
CORS_ALLOWED_ORIGINS = [
    "https://example.com",
    "https://sub.example.com",
    "http://localhost:8080",
    "http://127.0.0.1:9000"
],

# (2) 선택 2
CORS_ALLOWED_ORIGIN_REGEXES = [
    r"^https://\w+\.example\.com$",
]
```

- TodoList 에서 Get Todos 누르면 할일의 목록이 출력됩니다.
- CORS 설정을 완료했다는 것을 의미합니다.
- 버튼 대신 lifecycle hook인 create을 활용하여 미리 데이터를 가져옵니다.

```vue
<template>
  <div>
    <ul>
      <li v-for="(todo, idx) in todos" :key="idx">
        <span @click="updateTodoStatus(todo)" :class="{ completed: todo.completed }">{{ todo.title }}</span>
        <button @click="deleteTodo(todo)" class="todo-btn">X</button>
      </li>
    </ul>
    <!-- <button @click="getTodos">Get Todos</button> -->
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'TodoList',
  data: function () {
    return {
      todos: [],
    }
  },
  methods: {
    getTodos: function () {
      axios({
        method: 'get',
        url: 'http://127.0.0.1:8000/todos/'
      })
        .then((res) => {
          console.log(res)
          this.todos = res.data
        })
        .catch((err) => {
          console.log(err)
        })
    },
  created: function () {
    this.getTodos()
  }
}
</script>

<style scoped>
  .todo-btn {
    margin-left: 10px;
  }

  .completed {
    text-decoration: line-through;
    color: rgb(112, 112, 112);
  }
</style>

```



### 3.4. Client : Vue-Create todo

- todo리스트가 추가가 되면, TodoList 페이지로 이동하게 됩니다. 

  `this.$router.push({ name: 'TodoList' })`

```vue
<template>
  <div>
    <input type="text" v-model.trim="title" @keyup.enter="createTodo">
    <button @click="createTodo">+</button>
  </div>
</template>

<script>
import axios from'axios'

export default {
  name: 'CreateTodo',
  data: function () {
    return {
      // title: '',
      title: null,
    }
  },
  methods: {
    createTodo: function () {
      const todoItem = {
        title: this.title,
      }

      if (todoItem.title) {
        axios({
          method: 'post',
          url: 'http://127.0.0.1:8000/todos/',
          data: todoItem
        })
          .then((res) => {
            console.log(res)
            this.$router.push({ name: 'TodoList' })
          })
          .catch((err) => {
            console.log(err)
          })
        }
    },
  }
}
</script>

```



### 3.5. Client : Vue-Delete

- TodoList에서 삭제버튼을 누르면 
- 장고는 204 리스폰스를 넘겨줍니다. 이는 DB에서는 데이터가 잘 삭제되었음을 의미합니다.
- 다시 todos를 리로드 하기 위해서 `this.getTodos()`를 추가하게 됩니다.

```vue
<template>
  <div>
    <ul>
      <li v-for="(todo, idx) in todos" :key="idx">
        <span @click="updateTodoStatus(todo)" :class="{ completed: todo.completed }">{{ todo.title }}</span>
        <button @click="deleteTodo(todo)" class="todo-btn">X</button>
      </li>
    </ul>
    <!-- <button @click="getTodos">Get Todos</button> -->
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'TodoList',
  data: function () {
    return {
      todos: [],
    }
  },
  methods: {
    getTodos: function () {
      axios({
        method: 'get',
        url: 'http://127.0.0.1:8000/todos/'
      })
        .then((res) => {
          console.log(res)
          this.todos = res.data
        })
        .catch((err) => {
          console.log(err)
        })
    },
    deleteTodo: function (todo) {
      axios({
        method: 'delete',
        url: `http://127.0.0.1:8000/todos/${todo.id}/`
      })
        .then((res) => {
          console.log(res)
          // this.getTodos()
        })
        .catch((err) => {
          console.log(err)
        })
    },

  created: function () {
    this.getTodos()
  }
}
</script>

<style scoped>
  .todo-btn {
    margin-left: 10px;
  }

  .completed {
    text-decoration: line-through;
    color: rgb(112, 112, 112);
  }
</style>

```

### 3.6. Client : Vue-Update

- Delete와 마찬가지로 DB 변경사항이 즉시 반영이 안됩니다.
- 렌더링 변경사항 (todo 안의 completed 값)을 반대로 반영합니다.
- 즉, DB와 렌더링 둘 다 변경을 만족해야 함을 의미합니다.

```vue
<template>
  <div>
    <ul>
      <li v-for="(todo, idx) in todos" :key="idx">
        <span @click="updateTodoStatus(todo)" :class="{ completed: todo.completed }">{{ todo.title }}</span>
        <button @click="deleteTodo(todo)" class="todo-btn">X</button>
      </li>
    </ul>
    <!-- <button @click="getTodos">Get Todos</button> -->
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'TodoList',
  data: function () {
    return {
      todos: [],
    }
  },
  methods: {
    getTodos: function () {
      axios({
        method: 'get',
        url: 'http://127.0.0.1:8000/todos/'
      })
        .then((res) => {
          console.log(res)
          this.todos = res.data
        })
        .catch((err) => {
          console.log(err)
        })
    },
    deleteTodo: function (todo) {
      axios({
        method: 'delete',
        url: `http://127.0.0.1:8000/todos/${todo.id}/`
      })
        .then((res) => {
          console.log(res)
          this.getTodos()
        })
        .catch((err) => {
          console.log(err)
        })
    },
    updateTodoStatus: function (todo) {
      const todoItem = {
        ...todo,
        completed: !todo.completed
      }

      axios({
        method: 'put',
        url: `http://127.0.0.1:8000/todos/${todo.id}/`,
        data: todoItem,
      })
        .then((res) => {
          console.log(res)
          todo.completed = !todo.completed
        })
      },
    },
  created: function () {
    this.getTodos()
  }
}
</script>

<style scoped>
  .todo-btn {
    margin-left: 10px;
  }

  .completed {
    text-decoration: line-through;
    color: rgb(112, 112, 112);
  }
</style>

```



## 4. Authentication & Authorization

### 4.1. Authentication

- 인증, 입증
- 자신이라고 주장하는 사용자가 누구인지 확인하는 행위
- 모든 보안 프로세스의 첫 번째 단계
- 즉, 내가 누구인지를 확인하는 과정
- 401 unauthorized
  - 의미상 이 응답은 "비인증 (unauthenticated)"를 의미

### 4.2. Authorization

- 권한 부여, 허가
- 사용자에게 특정 리소스 또는 기능에 대한 액세스 권한을 부여하는 과정
- 보안 환경에서 권한 부여는 항상 인증을 따라야 함
  - 예를 들어, 사용자는 조직에 대한 액세스 권한을 부여 받기 전에 먼저 자신의 ID가 진짜인지 먼저 확인해야 함
- 서류의 등급, 웹 페이지에서 글을 조회 & 삭제 & 수정 할 수 있는 방법, 제한 구역
  - 인증이 되었어도 모든 권한을 부여 받는 것은 아님
- 403 Forbidden
  - 401과 다른 점은 서버는 클라이언트가 누구인지 알고 있음



|      | Authentication 인증       | Authorization 권한/허가     |
| ---- | ----------------------- | ----------------------- |
| 정의   | verifying who a user is | access to               |
| 설명   | 자신이라고 주장하는 유저 확인        | 유저가 자원에 접근할 수 있는지 여부 확인 |
| 차이   | 로그인 여부                  | 로그인 이후 부여되는 권한          |



## 5. Session / Toeken Based Authentication

### 5.1. Session

다양한 인증 방식

- Session based
- Token based
- Authentication platform



### 5.2. JWT (JSON Web Token)

- JSON 포맷을 활용하여 요소 간 안전하게 정보를 교환하기 위한 표준 포맷
- 암호화 알고리즘에 의한 디지털 서명이 되어있기 때문에 자체로 검증 가능하고 신뢰할 수 있는 정보 교환 체계
- JWT 자체가 필요한 정보를 모두 갖기 때문에 (self-contained) 이를 검증하기 위한 다른 검증 수단이 필요 없음
- 사용처
  - Authentication, Information Exchange

#### Why JWT ? 

- 상대적으로 HTML, HTTP 환경에서 사용하기 용이
  - Session은 유저의 session 정보를 Server에 보관해야 함
  - 하지만 JWT는 Client Side에 토큰 정보를 저장하고 필요한 요청에 같이 넣어 보내면 그 자체가 인증 수단이 됨
- 높은 보안 수준
- JSON의 범용성
- Server 메모리에 정보를 저장하지 않아 Server의 자원 절약 가능



### 5.3. JWT 구조

- Heeader
- Payload
- Signature

#### Header

- 토큰의 유형과 Hashing Algorithm으로 구성

#### Payload

- 토큰에 넣을 정보
- claim은 정보의 한 조각을 의미함며 payload에는 여러 개의 claim을 넣을 수 있음
- claim 종류
  - Registered claims
  - Public claims
  - Private claims

#### Signature

- Header와 Payload의 encoding 값을 더하고 거기에 private key로 hashing하여 생성



> **JWT STEPS**
>
> 1) Client to Server : POST /login {username, password }
>
> 2) Server to Client : (JWT 발급) 브라우저에게 JWT 응답
>
> 3) Client to Server : (브라우저에 JWT 저장) JWT와 함께 요청
>
> 4) Server to Client : JWT 검증 & JWT 유저 정보 확인 후 응답



|               | Session  | JWT      |
| ------------- | -------- | -------- |
| 인증 수단의 저장 위치  | Server   | Client   |
| 인증 수단의 정보 민감성 | Low      | High     |
| 유효 기간         | Y        | Y        |
| 확장성           | 상대적으로 낮음 | 상대적으로 높음 |



## 6. Practice

### 6.1. Client : Vue-Signup

- v-model로 데이터와 양방향 데이터 바인딩을 합니다
- 데이터가 넘어가는 것을 확인해줍니다.

```vue
<template>
  <div>
    <h1>Signup</h1>
    <div>
      <label for="username">사용자 이름: </label>
      <input type="text" id="username" v-model="credentials.username">
    </div>
    <div>
      <label for="password">비밀번호: </label>
      <input type="password" id="password" v-model="credentials.password">
    </div>
    <div>
      <label for="passwordConfirmation">비밀번호 확인: </label>
      <input type="password" id="passwordConfirmation" v-model="credentials.passwordConfirmation">
    </div>
    <button @click="signup(credentials)">회원가입</button>
  </div>
</template>

<script>
// import axios from 'axios'

// const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {
  name: 'Signup',
  data: function () {
    return {
      credentials: {
        username: null,
        password: null,
        passwordConfirmation: null,
      }
    }
  },
  methods: {
    signup: function (credentials) {
      console.log(credentials)
      
    }
  }
}
</script>
```

### 6.2. Server : models

```python
# todos - models.py

from django.db import models
from django.conf import settings

# Create your models here.
class Todo(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE, related_name='todos')
    title = models.CharField(max_length=50)
    completed = models.BooleanField(default=False)

    def __str__(self):
        return self.title
    
```

```
# DB 지우기
# python manage.py makemigrations
1
1

# python manage.py migrate
```

```python
# accounts - serializers.py

from rest_framework import serializers
from django.contrib.auth import get_user_model

User = get_user_model()

class UserSerializer(serializers.ModelSerializer):
    # write_only 시리얼라이징은 하지만 응답에는 포함시키지 않음
    password = serializers.CharField(write_only=True)

    class Meta:
        model = User
        fields = ('username', 'password')
```

```python
# accounts - views.py

from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .serializers import UserSerializer

@api_view(['POST'])
def signup(request):
	#1-1. Client에서 온 데이터를 받아서
    password = request.data.get('password')
    password_confirmation = request.data.get('passwordConfirmation')
		
	#1-2. 패스워드 일치 여부 체크
    if password != password_confirmation:
        return Response({'error': '비밀번호가 일치하지 않습니다.'}, status=status.HTTP_400_BAD_REQUEST)
		
	#2. UserSerializer를 통해 데이터 직렬화
    serializer = UserSerializer(data=request.data)
    
	#3. validation 작업 진행 -> password도 같이 직렬화 진행
    if serializer.is_valid(raise_exception=True):
        user = serializer.save()
        #4. 비밀번호 해싱 후 
        user.set_password(request.data.get('password'))
        user.save()
    	# password는 직렬화 과정에는 포함 되지만 → 표현(response)할 때는 나타나지 않는다.
    	return Response(serializer.data, status=status.HTTP_201_CREATED)

```

#### *Postman

```
POST
http://127.0.0.1:8000/accounts/signup/

Body - form-data
username : admin
password : admin123
passwordConfirmation : admin123
```

### 6.3. Client : Vue-Signup

```vue
<script>
import axios from 'axios'

// const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {
  name: 'Signup',
  data: function () {
    return {
      credentials: {
        username: null,
        password: null,
        passwordConfirmation: null,
      }
    }
  },
  methods: {
    signup: function () {
      // console.log(credentials)
      axios({
        method: 'post',
        url: 'http://127.0.0.1:8000/accounts/signup/',
        data: this.credentials
      })
        .then(res => {
          console.log(res)
          this.$router.push({ name: 'Login'})
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>

```



### 6.4. djangorestframework JWT [↗](https://jpadilla.github.io/django-rest-framework-jwt/)

```
pip install djangorestframework-jwt
pip freeze > requirements.txt


# accounts - urls.py
from rest_framework_jwt.views import obtain_jwt_token
from django.urls import path
from . import views


urlpatterns = [
    path('signup/', views.signup),
    path('api-token-auth/', obtain_jwt_token),
]


# 실습을 위해 토큰 유효기간을 연장시킵니다.
# settings.py
import datetime

JWT_AUTH = {
    'JWT_REFRESH_EXPIRATION_DELTA': datetime.timedelta(days=1),
}
```

#### *Postman

```
POST 
http://127.0.0.1:8000/accounts/api-token-auth/
username : admin
password : admin1234
```

```
# token 받아오기
token = 

# VERIFY SIGNATURE
# settings.py
SECRET_KEY = 'django-insecure-62jm0yq_9ipy#39()xwhsfzokphz3!lq(2g&a6)ki-=ix!@0gw'
```



### 6.5. Client : Vue-Login

```vue
<template>
  <div>
    <h1>Login</h1>
    <div>
      <label for="username">사용자 이름: </label>
      <input type="text" id="username" v-model="credentials.username">
    </div>
    <div>
      <label for="password">비밀번호: </label>
      <input type="password" id="password" v-model="credentials.password">
    </div>
    <button @click="login">로그인</button>
  </div>
</template>
<script>
import axios from 'axios'

// const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {
  name: 'Login',
  data: function () {
    return {
      credentials: {
        username: null,
        password: null,
      }
    }
  },
  methods: {
    login: function () {
      axios({
        method: 'post',
        url: 'http://127.0.0.1:8000/accounts/api-token-auth/',
        data: this.credentials
      })
        .then(res => {
          // console.log(res)
          localStorage.setItem('jwt', res.data.token)
          this.$router.push({ name: 'TodoList' })
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>

```

- 로그인 사용자와 비로그인 사용자에게 보여지는 화면을 다르게 합니다.
- v-if를 통해 분기처리를 합니다.

```vue
// App.vue

<template>
  <div id="app">
    <div id="nav">
      <span v-if="isLogin">
        <router-link :to="{ name: 'TodoList' }">Todo List</router-link> | 
        <router-link :to="{ name: 'CreateTodo' }">Create Todo</router-link> |
      </span>
      <span v-else>
        <router-link :to="{ name: 'Signup' }">Signup</router-link> |
        <router-link :to="{ name: 'Login' }">Login</router-link> 
      </span>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: function () {
    return {
      isLogin: false,
    }
  },
  methods: {

  },
  created: function () {
    const token = localStorage.getItem('jwt')
    if (token) {
      this.isLogin = true
    }

  }
}
</script>
```

- 로그인이 되었지만 화면에서 렌더링이 되지 않았습니다.
- App component의 isLogin 값이 바뀌지 않았습니다.
- login이 수행될 때 $emit을 통해 이벤트를 상위 컴포넌트에 전달합니다.

```vue
 methods: {
    login: function () {
      axios({
        method: 'post',
        url: 'http://127.0.0.1:8000/accounts/api-token-auth/',
        data: this.credentials
      })
        .then(res => {
          // console.log(res)
          localStorage.setItem('jwt', res.data.token)
          this.$emit('login')
          this.$router.push({ name: 'TodoList' })
        })
        .catch(err => {
          console.log(err)
        })
    }
```

```vue
// App.vue
<template>
  <div id="app">
    <div id="nav">
      <span v-if="isLogin">
        <router-link :to="{ name: 'TodoList' }">Todo List</router-link> | 
        <router-link :to="{ name: 'CreateTodo' }">Create Todo</router-link> |
      </span>
      <span v-else>
        <router-link :to="{ name: 'Signup' }">Signup</router-link> |
        <router-link :to="{ name: 'Login' }">Login</router-link> 
      </span>
    </div>
    <router-view @login="isLogin = true"/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: function () {
    return {
      isLogin: false,
    }
  },
  created: function () {
    const token = localStorage.getItem('jwt')
    if (token) {
      this.isLogin = true
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



### 6.6. Client : Vue-Logout

- router-link 기능으로 @click의 기본 이벤트가 발생하지 않기 때문에 
- `click.native`로 클릭 본연의 기능을 수행할 수 있도록 합니다.
- 또한 클라이언트 단의 `this.isLogin`의 값을 변경해주어 백엔드와 프론트간의 데이터 상태 변경을 동일하게 맞추어줍니다.

```vue
// App.vue

<router-link @click.native="logout" to="#">Logout</router-link>

  methods: {
    logout: function () {
      this.isLogin = false
      localStorage.removeItem('jwt')
      this.$router.push({ name: 'Login' })
      }
  }
```



### 6.7. 권한 AUTHORIZATION 

- 서버 측에서 권한을 검증합니다. 서버에서 토큰에 대한 인증 및 권한 절차를 진행하게 됩니다.
- 데코레이터의 순서가 중요합니다.
- 1) 유효한 토큰인지 확인합니다.
- 2) method의 권한을 인증하게 됩니다.

```python
from django.shortcuts import get_object_or_404
from rest_framework import status
from rest_framework.response import Response
from rest_framework.decorators import api_view

from rest_framework.decorators import authentication_classes, permission_classes
from rest_framework.permissions import IsAuthenticated
from rest_framework_jwt.authentication import JSONWebTokenAuthentication

from .serializers import TodoSerializer
from .models import Todo


@api_view(['GET', 'POST'])
# JWT을 활용한 인증을 할 때 JWT 자체를 검증한 인증 여부오 ㅏ상관 없이 JWT가 유효한지 여부만 파악
@authentication_classes([JSONWebTokenAuthentication])
# 인증이 되지 않은 상태로 요청이 오면
# "자격 인증 데이터"가 제공되지 않았습니다와 같은 메세지를 응답함
@permission_classes([IsAuthenticated])
def todo_list_create(request):
    if request.method == 'GET':
        # todos = Todo.objects.all()
        # 사용자(로그인한 유저)가 작성한 todos를 불러옵니다.
        serializer = TodoSerializer(request.user.todos, many=True)
        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = TodoSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            # user의 정보를 같이 넘겨줍니다.
            serializer.save(user=request.user)
            return Response(serializer.data, status=status.HTTP_201_CREATED)


@api_view(['PUT', 'DELETE'])
@authentication_classes([JSONWebTokenAuthentication])
@permission_classes([IsAuthenticated])
def todo_update_delete(request, todo_pk):
    todo = get_object_or_404(Todo, pk=todo_pk)

    # 1. 해당 todo의 유저가 아닌 경우 todo를 수정하거나 삭제하지 못하게 설정
    if not request.user.todos.filter(pk=todo_pk).exists():
        return Response({'detail': '권한이 없습니다.'}, status=status.HTTP_403_FORBIDDEN)
    if request.method == 'PUT':
        serializer = TodoSerializer(todo, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)

    elif request.method == 'DELETE':
        todo.delete()
        return Response({ 'id': todo_pk })
```

```vue
# 서버를 끊고 다시 켜줍니다.
```



#### *Postman

```
http://127.0.0.1:8000/accounts/api-token-auth/
username
password

http://127.0.0.1:8000/todos/
Headers
# JWT 띄어쓰기 꼭 포함
AUTHORIZATION : JWT ...

Body - form-data
title : 할일1
```

```vue
// TodoList.vue

  created: function () {
    if (localStorage.getItem('jwt')) {
      this.getTodos()
    } else {
      this.$router.push({ name: 'Login' })
	}
```

```vue
// App.vue

<router-view @login="isLogin = true"/>
```



### 6.8. Token 붙이기

- 토큰이 없으니까 ERROR : resource: the server responded with a status of 401 (Unauthorized)

```vue
  methods: {
    setToken: function () {
      const token = localStorage.getItem('jwt')
      const config = {
        Authorization: `JWT ${token}`
      }
      return config
    },
```

```vue
// TodoList.vue

<template>
  <div>
    <ul>
      <li v-for="(todo, idx) in todos" :key="idx">
        <span @click="updateTodoStatus(todo)" :class="{ completed: todo.completed }">{{ todo.title }}</span>
        <button @click="deleteTodo(todo)" class="todo-btn">X</button>
      </li>
    </ul>
    <!-- <button @click="getTodos">Get Todos</button> -->
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'TodoList',
  data: function () {
    return {
      todos: [],
    }
  },
  methods: {
    setToken: function () {
      const token = localStorage.getItem('jwt')
      const config = {
        Authorization: `JWT ${token}`
      }
      return config
    },
    getTodos: function () {
      axios({
        method: 'get',
        url: 'http://127.0.0.1:8000/todos/',
        headers: this.setToken()
      })
        .then((res) => {
          console.log(res)
          this.todos = res.data
        })
        .catch((err) => {
          console.log(err)
        })
    },
    deleteTodo: function (todo) {
      axios({
        method: 'delete',
        url: `http://127.0.0.1:8000/todos/${todo.id}/`,
        headers: this.setToken()
      })
        .then((res) => {
          console.log(res)
          this.getTodos()
        })
        .catch((err) => {
          console.log(err)
        })
    },
    updateTodoStatus: function (todo) {
      const todoItem = {
        ...todo,
        completed: !todo.completed
      }

      axios({
        method: 'put',
        url: `http://127.0.0.1:8000/todos/${todo.id}/`,
        headers: this.setToken(),
        data: todoItem,
      })
        .then((res) => {
          console.log(res)
          todo.completed = !todo.completed
        })
      },
    },
    created: function () {
      if (localStorage.getItem('jwt')) {
        this.getTodos()
      } else {
        this.$router.push({ name: 'Login' })
      }
  }
}
</script>

<style scoped>
  .todo-btn {
    margin-left: 10px;
  }

  .completed {
    text-decoration: line-through;
    color: rgb(112, 112, 112);
  }
</style>

```

```vue
// CreateTodo.vue
<template>
  <div>
    <input type="text" v-model.trim="title" @keyup.enter="createTodo">
    <button @click="createTodo">+</button>
  </div>
</template>

<script>
import axios from'axios'

export default {
  name: 'CreateTodo',
  data: function () {
    return {
      title: null,
    }
  },
  methods: {
    setToken: function () {
      const token = localStorage.getItem('jwt')
      const config = {
        Authorization: `JWT ${token}`
      }
      return config
    },
    createTodo: function () {
      const todoItem = {
        title: this.title,
      }

      if (todoItem.title) {
        axios({
          method: 'post',
          url: 'http://127.0.0.1:8000/todos/',
          data: todoItem,
          headers: this.setToken()
        })
          .then((res) => {
            console.log(res)
            this.$router.push({ name: 'TodoList' })
          })
          .catch((err) => {
            console.log(err)
          })
        }
    },
  }
}
</script>

```

```vue
// App.vue
<template>
  <div id="app">
    <div id="nav">
      <span v-if="isLogin">
        <router-link :to="{ name: 'TodoList' }">Todo List</router-link> | 
        <router-link :to="{ name: 'CreateTodo' }">Create Todo</router-link> |
        <router-link @click.native="logout" to="#">Logout</router-link>
      </span>
      <span v-else>
        <router-link :to="{ name: 'Signup' }">Signup</router-link> |
        <router-link :to="{ name: 'Login' }">Login</router-link> 
      </span>
    </div>
    <router-view @login="isLogin = true"/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: function () {
    return {
      isLogin: false,
    }
  },
  methods: {
    logout: function () {
      this.isLogin = false
      localStorage.removeItem('jwt')
      this.$router.push({ name: 'Login' })
    }
  },
  created: function () {
    const token = localStorage.getItem('jwt')
    if (token) {
      this.isLogin = true
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











