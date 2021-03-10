[TOC]

# Django - Day1 Practice

### 1. 프로젝트 시작

pip install django

#### 1.1. 장고 템플릿 시작

`django-admin startproject first_pjt`

새로운 프로젝트 폴더(first_pjt)에 들어가서 vscode를 실행합니다.

#### 1.2. 장고 서버 시작

`python manage.py runserver`

로켓이 떠있는 지 확인해 봅니다.

#### 1.3. 앱 생성 및 실행
`python manage.py startapp articles`

새로운 앱(articles)을 생성합니다.

- 앱 이름은 복수형으로 지어줍니다.
- 생성 후 등록

#### 1.4. settings.py 앱 추가하기
`INSTALLED_APPS = 'articles'` 추가

- INSTALLED_APPS 등록 우선순위

  1. local apps

  2. 3rd-party apps

  3. django apps

- LANGUAGE_CODE = 'ko-kr'
- TIME_ZONE = 'Asia/Seoul'



### 2. urls - views - templates

#### 2.1. urls : urls.py 작성
```django
from articles import views

urlpatterns = [
	path('index/', views.index),
]
```

articles 에서 views 모듈을 불러옵니다. 

- `path('index/') ` 경로 뒤에 `/`를 붙여줍니다.

#### 2.2. views : articles - views.py 작성
```django
def index(request):
	context = {
	}
	return render(request, context)
```

articles app - views.py 에서 index 함수를 정의합니다.

- 변수들 입력 방법 `context`

  ```
  foods = ['apple', 'banana', 'coconut',]
  info = {
  	'name': 'Harry'
  }
  context = {
  	'info': info,
  	'foods': foods,
  }
  ```

#### 2.3. templates : articles - templates - index.html 작성
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <h1>만나서 반가워요!!!</h1>
  <a href="/greeting/">greeting</a>
  <a href="/dinner/">dinner</a>
</body>
</html>
```

templates 폴더를 만들어 html 문서 작성합니다.



### 3.  Multi Apps

#### 3.1. urls : url mapping 하기

##### 3.1.1. urls : urls.py 수정

```django
from django.urls import path, include

urlpatterns = [
    path('articles/', include('articles.urls')),
    path('admin/', admin.site.urls),
]
```

##### 3.1.2. urls : articles - urls.py 작성

```django
from django.urls import path
from . import views

app_name = 'articles'
urlpatterns = [
    path('index/', views.index, name='index'),
]
```

- app_name : 템플릿 네임을 지정합니다.

- `path('index/', views.index, name='index')` 을 지정합니다.

  페이지 경로, views 안 함수, 이름 지정하기

#### 3.2. views : namespace 경로 설정

##### 3.2.1. views : articles - views.py 작성

```django
def index(request):
    return render(request, 'articles/index.html')
```

- APP별로 templates/APP_NAMES 폴더를 만들어줍니다.

#### 3.3. templates : namespace 분리

##### 3.3.1. templates : templates - articles 폴더 생성 - index.html 작성

```django
{% extends 'base.html' %}

{% block content %}
  <h1>lorem image</h1>
  <img src="https://picsum.photos/200" alt="200x200 random image">
{% endblock content %}
```

- namespace 분리를 위해 templates 폴더에 articles 폴더를 추가합니다.
- `{% extends 'base.html' %}` 상속을 받습니다. 가장 위에 작성하게 됩니다.
- `{% block content %}{% endblock content %}` 블록 안에 콘텐츠를 작성하게 됩니다.

##### 3.3.2. templates 상속 : base.html

```django
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl" crossorigin="anonymous">
  <title>Document</title>
</head>
<body>
  <div class="container">
    {% block content %}
    {% endblock %}
  </div>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.bundle.min.js" integrity="sha384-b5kHyXgcpbZJO/tY9Ul7kGkf1S0CWuKcCD38l8YkeH8z8QjE0GmW1gYU5S9FOnJ0" crossorigin="anonymous"></script>
</body>
</html>

```

```django
'DIRS': [BASE_DIR / 'my_pjt' / 'templates'],
```

- 상속 extends를 위해 base.html 생성

- base.html 경로를 지정하기 위해 settings.py 에서 TEMPLATES 속 DIRS 경로 설정

  `[BASE_DIR / 'my_pjt' / 'templates']`

  

### 4. Practices

#### 4.1. HTML Form

웹에서 사용자 정보를 입력하는 여러 방식을 제공하고, 사용자로부터 할당된 데이터를 서버로 전송하는 역할을 담당합니다. 

```html
{% extends 'base.html' %}

{% block content %}
  <h1>THROW</h1>
  <form action="{% url 'articles:catch' %}" method="GET">
    <label for="message">INPUT HERE : </label>
    <input type="text" name="message" id="message">
    <input type="submit">
  </form>

  <a href="{% url 'articles:index' %}">뒤로</a>
{% endblock %}
```

- method = 어떤 방식으로 보낼지 'GET' / 'POST'
- action = 어디로 보낼지 url를 지정합니다.
- input = 어떤 데이터를 받을지 설정합니다.
  - label : 이름표를 붙여줍니다.
  - name : 변수명입니다.
  - id : 아이디입니다.
- submit = 데이터 제출

```html
{% extends 'base.html' %}
{% block content %}
  <h1>fakegoogle</h1>
  <form action="https://www.google.com/search" method="GET" target="_blank">
    <label for="q">Input here</label>
    <input type="text" name="q" id="q">
    <input type="submit">
  </form>

{% endblock content %}
```

- google에서 query를 입력받는 방법은 search?q= 방법을 이용합니다, 이는 개발자 도구를 활용합니다.
- 새탭으로 열기 : target="_blank"



#### 4.2. Variable Routing 동적 라우팅

두 수를 입력받아서 곱을 출력하는 페이지를 작성합니다. 

`urls.py`

```django
urlpatterns = [
    path('times/<int:num1>/<int:num2>/', views.times, name='times'),
]
```

`times.html`

```django
{% extends 'base.html' %}
{% block content %}
  <h1>{{ num1 }} x {{ num2 }} = {{ result }}</h1>
{% endblock content %}
```



#### 4.3. DTL  (Django template language)

##### 4.3.1. For tag

```django
{% for food in foods %}
	<p>{{ food }}</p>
{% endfor %}

{% for food in foods %}
	<p>{{ forloop.counter }}. {{ food }}</p>
{% endfor %}

{% for elem in empty_list %}
	<p>{{ elem }}</p>
{% empty %}
	<p> 현재 가입한 유저가 없습니다. </p>
{% endfor %}
```

- forloop.counter
- empty

##### 4.3.2. If tag

```django
{% if '닭고기' in foods %}
	<p>닭고기는 치킨이겠죠?!~</p>
{% else %}
	<p>치킨추가</p>
{% endif %}

{% for food in foods %}
	{% if forloop.first %}
		<p>{{ food }}은 미스터 초밥왕</p>
	{% else %}
		<p>{{ food }}</p>
	{% endif %}
{% endfor %}
```

- if else
- forloop.first

##### 4.3.3. filter tag

```django
{% for fruit in fruits %}
	{% if fruit|length > 5 %}
		<p>pass</p>
	{% else %}
		<p>{{ fruit }}, {{ fruit|length }} 입니다. </p>
	{% endif %}
{% endfor %}
```

##### 4.3.4. lorem ipsum & String filter

```django
{% lorem %}

{% lorem 2 p random %}

{{ 'ABC'|lower }}

{{ my_sentence|title }}

{{ foods|random }}
```

##### 4.3.5. only add

```django
{{ 4|add:6 }}
```

##### 4.3.6. Datetime

```django
{% now "DATE_FORMAT" %}

{% now "DATETIME_FORMAT" %}

{% now "SHORT_DATE_FORMAT" %}

{% now "SHORT_DATETIME_FORMAT" %}

{% now "Y-m-d H:i" %}

{% now "jS F Y H:i" %}

{% now "Y년 m월 d일 (D) H:i" %}

{% now "Y년 m월 d일 (D) A h:i" %}

Copyright {% now "Y" %}

{% now "Y" as current_year %}
변수로도 사용이 가능합니다 : {{ current_year }}
```