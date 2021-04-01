[TOC]

# Django - Day5

*The Web framework for perfectionists with deadlines*

## 1. Authentication System

### 1.1. Authentication & Authorization

- Authentication : 인증, 자신이 누구라고 주장하는 사람의 신원을 확인하는 것
- Authorization : 권한/허가, 가고 싶은 곳으로 가도록 혹은 원하는 정보를 얻도록 허용하는 과정



### 1.2. Django Authentication System

- 장고는 인증과 권한 부여를 함께 제공하며, 이러한 기능이 어느 정도 결합되어 있기 때문에 일반적으로 authentication system(인증 시스템)이라고 합니다.
- 크게 User Object와 Web Request에 대한 인증 시스템 두 가지로 살펴볼 수 있습니다.



### 1.3. Login & Logout

#### Authentication in Web Requests

- 장고는 세션과 미들웨어를 사용하여 인증 시스템을 request 객체에 연결
- 이를 통해 사용자를 나타내는 모든 요청에 request.user를 제공
- 현재 사용자가 로그인하지 않는 경우 AnoymousUser 클레스의 인스턴스로 설정되며, 그렇지 않으면 user 클래스의 인스턴스로 설정됩니다.

#### 로그인 & 로그아웃

- login()
  - 로그인은 Session을 Create하는 로직과 같습니다.
  - 현재 세션에 연결하려는 인증된 사용자가 있는 경우 login() 함수로 로그인 진행
  - request 객체와 user 객체를 통해 로그인 진행
  - Django의 session framework를 통해 사용자의 ID를 세션에 저장합니다.
- logout()
  - 로그인은 Session을 Delete하는 로직과 같습니다.
  - request 객체를 받으며 return이 없습니다.
  - 현재 요청에 대한 DB의 세션 데이터를 삭제하고 클라이언트 쿠키에서도 sessionid를 삭제합니다.

#### HTTP

- HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜(규칙, 약속)
- 웹에서 이루어지는 모든 데이터 교환의 기초가 됩니다.
- HTTP의 특징
  - 비연결지향(connectionless) : 서버는 응답 후 접속을 끊습니다.
  - 무상태(stateless) : 접속이 끊어지면 클라이언트와 서버간의 통신이 끝나며 상태를 저장하지 않습니다.

#### Cookie (쿠키)

- 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각입니다.
- 브라우저(클라이언트)는 전송 받은 쿠키를 로컬에 key-value의 데이터 형식으로 저장합니다 -> 동일한 서버에 재요청 시 저장된 쿠키를 함께 전송합니다.
- 웹 페이지에 접속하면 요청한 웹 페이지를 받으며 쿠키를 로컬에 저장하고, 클라이언트가 재요청시마다 웹 페이지 요청과 함께 쿠키 값도 같이 전송합니다.
- 사용 목적
  - `세션 관리` : 로그인, 아이디 자동완성, 공지 하루 안보기, 팝업 체크, 장바구니
  - 개인화 : 사용자 선호, 테마 세팅
  - 트래킹 : 사용자 행동을 기록 및 분석

#### Session (세션)

- 사이트와 특정 브라우저 사이의 "State(상태)"를 유지시키는 것
- 클라이언트가 서버에 접속하면 서버가 특정 session id를 발급하고 클라이언트는 session id를 쿠키를 사용해 저장, 클라이언트가 다시 서버에 접속할 때 해당 쿠키(session id가 저장된)를 이용해 서버에 session id를 전달
- django는 특정 session id를 포함하는 쿠키를 사용해서 각각의 브라우저와 사이트가 연결된 세션을 알아냄. 세션 정보는 dajngo DB의 django_session 테이블에 저장
- 주로 로그인 상태 유지에 사용

#### Cookie lifetime

1. Session cookie
   - 현재 세션이 종료되면 삭제
   - 브라우저는 현재 세션이 종료되는 시기를 정의
   - 일부 브라우저는 다시 시작할 때 세션 복원을 사용해 계속 지속될 수 있도록 함
2. Permanent cookie
   - Expires 속성에 지정된 날짜 혹은 Max-Age 속성에 지정된 기간이 지나면 삭제





### practice

#### 1. Login & Logout

- `python manage.py startapp accounts`
- settings 앱 등록
- urls.py 작성
- views.py 작성
- templates 작성

**urls.py**

```python
from django.urls import path
from . import views


app_name = 'accounts'
urlpatterns = [
    path('login/', views.login, name='login'),
]
```

**views.py**

```python
from django.shortcuts import render, redirect
from django.contrib.auth import login as auth_login
from django.contrib.auth.forms import AuthenticationForm


# Create your views here.
def login(request):
    if request.method == 'POST':
        form = AuthenticationForm(request, request.POST)
        # form = AuthenticationForm(request, data=request.POST)
        if form.is_valid():
            # 세션 CREATE
            auth_login(request, form.get_user())
            return redirect('articles:index')
    else:
        form = AuthenticationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/login.html', context)

```

**login.html**

```django
{% extends 'base.html' %}

{% block content %}
<h1>로그인</h1>
<form action="{% url 'accounts:login' %}" method="POST">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit">
</form>
{% endblock %}
```

**base.html**

```django
{% load bootstrap5 %}

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  {% bootstrap_css %}
  <title>Document</title>
</head>
<body>
  <div class="container">
    <h3>Hello, {{ request.user }}</h3>
    {% comment %} request.user.username {% endcomment %}
    <a href="{% url 'accounts:login' %}">Login</a>
    {% block content %}
    {% endblock %}
  </div>
  {% bootstrap_javascript %}
</body>
</html>

```

> - Session의 생명주기를 바꿔줄 수 있습니다.
>
> settings.py
>
> DAY_IN_SECONDS = 86400
>
> SESSION_COOKIE_AGE = DAY_IN_SECONDS

**urls.py**

```python
from django.urls import path
from . import views


app_name = 'accounts'
urlpatterns = [
    path('login/', views.login, name='login'),
    path('logout/', views.logout, name='logout'),
]
```

**views.py**

```python
from django.contrib.auth import logout as auth_logout
from django.views.decorators.http import require_POST

@require_POST
def logout(request):
    auth_logout(request)
    return redirect('articles:index')
```

**base.html**

```django
<a href="{% url 'accounts:login' %}">Login</a>
<form action="{% url 'accounts:logout' %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="Logout">
</form>
```

> - 로그인과 로그아웃 유저를 보여주는 화면분리
> - 로그인 상태에서 로그인 페이지로 접근할 수 없어야 합니다.
>
> 1. attribute
>    - request.user.is_authenticated
>
> 2. login required decorator
>    - LOGIN_URL의 기본 값은 'accounts/login/'
>    - from django.contrib.auth.decorators import login_required
>    - @login_required



### 2. User - CRUD

**signup.html**

```django
{% extends 'base.html' %}

{% block content %}
<h1>회원가입</h1>
<form action="" method="POST">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit">
</form>
{% endblock %}
```

**views.py**

```python
def signup(request):
    if request.user.is_authenticated:
        return redirect('articles:index')
    if request.method == "POST":
        form = UserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            auth_login(request, user)
            return redirect('articles:index')
    else:
        form = UserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/signup.html', context)
```









## RECAP

### 1. Login

- 쿠키와 세션
- AuthenticationForm
- .get_user()

### 2. Logout

### 3. 로그인 사용자에 대한 접근권한

- is_authenticated attribute
- login_required decorator
- Settings.py : Login_URL = 'accounts/login'
- `next` parameter

### 4. 회원가입

- UserCreationForm

### 5. 회원탈퇴

- .delete()
- 만약 탈퇴하면서 세션도 지우고 싶다면 반드시 "탈퇴 후 로그아웃" 순서로 진행

### 6. 회원정보수정

- UserChangeForm
- CustomUserChangeForm
- 기본 제공 필드가 과하게 많음
- User 참고 -> `get_user_model()`
- AbstractUser


