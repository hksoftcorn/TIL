[TOC]

# Django - Day4

*The Web framework for perfectionists with deadlines*

## Django Model

### 1. Form

Form은 django 프로젝트의 주요 유효성 검사 도구들 중 하나이며, 공격 및 우연한 데이터 손상에 대한 중요한 방어수단입니다. django form의 역할은 다음과 같습니다.

1. 랜더링을 위한 데이터 준비 및 재구성
2. 데이터에 대한 HTML forms 생성
3. 클라이언트로부터 받은 데이터 수신 및 처리



#### 1.1. From Class

Django Form 관리 시스템의 핵심입니다. form내 field, field 배치, 디스플레이 widget, label, 초기값, 유효하지 않은 field에 관련된 에러메세지를 결정합니다. django는 사용자의 데이터를 받을 때 해야 할 과중한 작업(데이터 유효성 검증, 필요시 입력된 데이터 검증 결과 재출력, 유효한 데이터에 대해 요구되는 동작 수행 등)과 반복 코드를 줄여 주는 역할을 합니다.

**forms.py**

```python
from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField(widget=forms.Textarea)
```

**.html**

- as_p : 각 필드가 단락(paragraph)로 렌더링
- as_ul : 각 필드가 목록항목(list item)로 렌더링 
- as_table : 각 필드가 테이블 행으로 렌더링



#### 1.2. 2가지 HTML input 요소 표현 방법

1. Form fields : input에 대한 유효성 검사 로직을 처리하며 템플릿에서 직접 사용됩니다.
2. Widgets : 웹 페이지의 HTML input 요소 렌더링 및 제출(submitted)된 원시 데이터 추출을 처리합니다. widgets은 반드시 form fields에 할당됩니다.
   - Django의 HTML iput element 표현
   - HTML 렌더링을 처리
   - Form Fields와 혼동되어서는 안됨 : Form Fields는 input 유효성 검사를 처리
   - Widgets은 웹페이지에서 input element의 단순 raw한 렌더링 처리



#### 1.3. ModelForm Class

model을 통해 Form class를 만들 수 있는 Helper 입니다. 일반 Form Class와 완전히 같은 방식(객체 생성)으로 view에서 사용이 가능합니다.

- Meta Class : Model의 정보를 작성합니다. 해당 model에 정의한 field 정보를 form에 적용하기 위함입니다.

```python
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    
    class Meta:
        model = Article
        fields = '__all__'
```

> Form vs ModelForm
>
> - Form
>   - 어떤 model에 저장해야 하는지 알 수 없으므로 유효성 검사 이후 cleaned_data 딕셔너리를 생성
>   - cleaned_data 딕셔너리에서 데이터를 가져온 후 .save() 호출해야 함
>   - model에 연관되지 않은 데이터를 받을 때 사용
> - ModelForm
>   - django가 해당 model에서 양식에 필요한 대부분의 정보를 이미 정의
>   - 어떤 레코드를 만들어야 할 지 알고 있으므로 바로 .save() 호출 가능



### 2. Form custom

form 데이터를 받는 다양한 방법들

#### 2.1. form 메소드 분리

```django
<h1>CREATE</h1>
<form action="" method="POST">
	{% csrf_token %}
	{{ form.as_p }}
	<input type="submit">
</form>
<hr>

<form action="" method="POST">
    {% csrf_token %}
    <div>
        {{ form.title.errors }}
        {{ form.title.label_tag }}
        {{ form.title }}
    </div>
    <div>
        {{ form.content.errors }}
        {{ form.content.label_tag }}
        {{ form.content }}
    </div>
    <input type="submit">
</form>
<hr>

<form action="" method="POST">
    {% csrf_token %}
    {% for field in form %}
    	{{ field.errors }}
	    {{ field.label_tag }}
	    {{ field }}
    {% endfor %}
    <input type="submit">
</form>
<hr> 

<form action="" method="POST">
    {% csrf_token %}
    {% for field in form %}
    {% if field.errors %}
    <div>
        {% for error in field.errors %}
        <li><strong>{{ error|escape }}</strong></li>
    </div>  
    {% endif %}
    {{ field.label_tag }}
    {{ field }}
    {% endfor %}
    <input type="submit">
</form>
<hr>

```



#### 2.2. Bootstrap 적용하기

**forms.py**  : `form-control`

```python
class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label='제목',
        widget=forms.TextInput(
            attrs={
                'class': 'my-title form-control',
                'placeholder': 'Enter the Title',
                'maxlength': 10,
            }
        )
    )
    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(
            attrs={
                'class': 'my-content form-control',
                'placeholder': 'Enter the Content',
                'rows': 5,
                'cols': 30,
            }
        ),
        error_messages={
            'required': '데이터를 입력해줄래...?',
        }
    )

    class Meta:
        model = Article
        fields = '__all__'
        # exclude = ('title',)
```



#### 2.3. Django Bootstrap5 : {% load bootstrap5 %}

- `pip install django-bootstrap-v5`
- `pip freeze > requirements.txt`

**settings.py** 

```python
INSTALLED_APPS = [
    'articles',
    'bootstrap5',
    ...
]
```

**update.html**

```django
{% extends 'base.html' %}
{% load bootstrap5 %}


{% block content %}
  <h1>UPDATE</h1>
  <form action="{% url 'articles:update' article.pk %}" method="POST">
    {% csrf_token %}

    {% bootstrap_form form layout='horizontal' %}
	{% buttons submit='OK' reset="Cancel" %}
      <button type="submit" class="btn btn-primary">Submit</button>
    {% endbuttons %}
    
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock  %}
```

**base.html**

```html
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
    {% block content %}
    {% endblock %}
  </div>
  {% bootstrap_javascript %}
</body>
</html>
```

**examples**

```django
{% bootstrap_form form layout='horizontal' %}
{% buttons submit='OK' reset="Cancel" %}
	<button type="submit" class="btn btn-primary">Submit</button>
{% endbuttons %}
```



#### 2.4. include ".html"

**nav.html**

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
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">Navbar</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
              Dropdown
            </a>
            <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
              <li><a class="dropdown-item" href="#">Action</a></li>
              <li><a class="dropdown-item" href="#">Another action</a></li>
              <li><hr class="dropdown-divider"></li>
              <li><a class="dropdown-item" href="#">Something else here</a></li>
            </ul>
          </li>
          <li class="nav-item">
            <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
          </li>
        </ul>
        <form class="d-flex">
          <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
          <button class="btn btn-outline-success" type="submit">Search</button>
        </form>
      </div>
    </div>
  </nav>
  
</body>
</html>
```

**base.html**

```html
{% include "nav.html" %}
```



### 3. View Decorators

#### 3.1. 데코레이터 활용

- Decorator (데코레이터)
  - 어떤 함수에 기능을 추가하고 싶을 때, 해당 함수를 수정하지 않고 기능을 연장 해주는 함수
  - django는 다양한 기능을 지원하기 위해 view 함수에 적용할 수 있는 여러 데코레이터를 제공
- Allowed HTTP methods
  - 요청 메서드에 따라 view 함수에 대한 엑세스를 제한
  - 요청이 조건을 충족시키지 못하면 HttpResponseNotAllowed을 return
  - require_http_method(), require_GET(), require_POST(), require_safe()

**views.py**

```python
from django.shortcuts import render, redirect
from django.views.decorators.http import require_safe, require_http_methods, require_POST
from .models import Article
from .forms import ArticleForm

# Create your views here.
@require_safe
def index(request):
    articles = Article.objects.order_by('-pk')
    context = {
        'articles': articles,
    }
    return render(request, 'articles/index.html', context)


# 하나의 view 함수가 request의 method에 따라서 2가지 역할을 하게 됨
@require_http_methods(['GET', 'POST'])
def create(request):
    # POST일 때
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
    # POST가 아닌 다른 method일 때
    else:
        form = ArticleForm()
    context = {
        # 상황에 따른 2가지 모습
        # 1. is_valid에서 내려온 form : 에러메세지를 포함한 form
        # 2. else에서 내려온 form : 빈 form
        'form': form,
    }
    return render(request, 'articles/create.html', context)


@require_safe
def detail(request, pk):
    # 몇번 글을 조회할건지 가져와야 함
    article = Article.objects.get(pk=pk)
    context = {
        'article': article,
    }
    return render(request, 'articles/detail.html', context)


@require_POST
def delete(request, pk):
    # 삭제할 게시글 조회
    article = Article.objects.get(pk=pk)
    # 삭제 요청이 POST면 삭제, POST가 아니라면 DETAIL 페이지로 redirect
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    return redirect('articles:detail', article.pk)


@require_http_methods(['GET', 'POST'])
def update(request, pk):
    article = Article.objects.get(pk=pk)
    # update
    if request.method == 'POST':
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():
            form.save()
            return redirect('articles:detail', article.pk)
    # edit
    else:
        form = ArticleForm(instance=article)
    context = {
        'form': form,
        'article': article,
    }
    return render(request, 'articles/update.html', context)
```

#### 3.2. postman [↗](https://www.postman.com/) 사용

html 응답을 확인할 수 있습니다

#### 3.3. 정적파일

- Static files
  - 웹 사이트의 구성 요소 중에서 image, css, js 파일과 같이 해당 내용이 고정되어, 응답을 할 때 별도의 처리 없이 파일 내용을 그대로 보여주면 되는 파일
  - 즉, 사용자의 요청에 따라 내용이 바뀌는 것이 아니라 요청한 것을 그대로 응답하면 되는 파일입니다.
  - 기본 static 경로 : app_name/static

static/articles 폴더 생성

**index.html**

```django
{% load static %}
<img src="{% static 'articles/sample.png' %} alt='sample'">
```

- STATIC_ROOT
  - Django 프로젝트에서 사용하는 모든 정적 파일을 한 곳에 모아넣는 경로
  - collectstatic이 배포를 위해 정적 파일을 
- STATIC_URL
  - 
- STATICFILES_DIRS
  - 

> static file 활용 STEP
>
> 1. INSTALLED_APPS 에 'django.contrib.staticfiles' 가 등록되어 있는 지 확인
> 2. STATIC_URL = '/static/' 확인
> 3. 적용할 대상 파일에 {% load static %} 불러오고 href="{% static 'stylesheets/style.css' %}"> static을 활용하여 이미지 파일을 가져옵니다.
> 4. 폴더 구조는 my_app/static/my_app/example.jpg 입니다.

**settings.py**

```
STATIC_URL = '/static/'
# STATIC_ROOT = BASE_DIR / 'staticfiles'
STATICFILES_DIRS = [
	BASE_DIR / 'crud' / 'static',
]
```

`python manage.py collectstatic`  # static 파일 갯수 확인

**crud - static - stylesheets - style.css**

```css
h1 {
  color: crimson;
}
```

**crud - base.html**

```html
{% block css %}{% endblock %}
```

**article - index.html**

```html
{% load static %}
{% block css %}
	<link rel="stylesheet" href="{% static 'stylesheets/style.css' %}">
{% endblock css %}
```



### 4. Media

#### 4.1. Media Files in settings.py

- MEDIA_ROOT
  - 사용자가 업로드 한 파일을 보관할 디렉토리의 절대 경로
  - 실제 해당 파일의 업로드가 끝나면 파일이 저장되는 경로
  - Django는 성능을 위해 업로드 파일은 데이터베이스에 저장하지 않음 : 파일 경로만 저장
  - MEDIA_ROOT와 STATIC_URL은 서로 다른 경로를 가져야 합니다.
- MEDIA_URL
  - 업로드 된 파일의 주소(URL)를 만들어주는 역할을 합니다.
  - MEDIA_ROOT에서 제공되는 미디어 파일을 처리하는 URL : 웹 서버 사용자가 사용하는 public URL
  - 비어 있지 않는 값으로 설정한다면 반드시 `/`로 끝나야 함
  - MEDIA_URL과 STATIC_URL은 서로 다른 경로를 가져야 합니다.

#### 4.2. Media STEP

> 1. settings.py 파일 MEDIA_ROOT, MEDIA_URL 추가
> 2. models.py 파일 ImageField(blank=True) 추가 : makemigrations & migrate 실행
> 3. urls.py 파일 MEDIA_ROOT, MEDIA_URL 경로 추가하기

**settings.py**

```python
MEDIA_ROOT = BASE_DIR / 'media'
MEDIA_URL = '/media/'
```

**urls.py**

```python
from django.conf import settings
from django.conf.urls.static import static


urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

업로드 되는 미디어 파일의 url의 주소는 setting.MEDIA_URl에 있습니다. 이 주소를 통해 실제 참조할 파일의 위치는 MEDIA_ROOT에 있습니다.

**models.py**

```python
from django.db import models

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    image = models.ImageField(blank=True, upload_to='') # field 안에 blank(빈 값)을 허용
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

`python -m pip install Pillow`

`python manage.py makemigrations`

`python manage.py migrate`

`python manage.py runserver`

**views.py**

```python
def create(request):
    # POST일 때
    if request.method == 'POST':
        form = ArticleForm(request.POST, request.FILES)
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
```

request.FILES 인자를 추가로 받아줍니다. Django Github에서 BaseModelForm 클래스를 참고해보면 파라미터 순서가 data, files, ... 순서로 되어있는 것을 확인할 수 있습니다.

**create.html**

```html
{% block content %}
  <h1>CREATE</h1>
  <form action="" method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
{% endblock content %}
```

enctype : 인코딩타입을 지정합니다. 

- `application/x-www-form-urlencoded` : 기본값
- `multipart/form-data` : <input type="file">이 존재하는 경우 사용합니다.
- `text/plain` : HTML5에서 디버깅 용으로 추가된 값입니다.

accept : input 파일의 형식을 지정해줄 수 있습니다.

- `accept="image/*"` : 이미지 파일을 탐색합니다.

**detail.html**

```html
<h2>DETAIL</h2>
  {% if article.image %}
    <img src="{{ article.image.url }}" alt="{{ article.imgae }}">
  {% endif %}
```

**update.html**

```html
<form action="{% url 'articles:update' article.pk %}" method="POST" enctype="multipart/form-data">
```

**views.py**

```python
@require_http_methods(['GET', 'POST'])
def update(request, pk):
    ...
	form = ArticleForm(request.POST, request.FILES, instance=article)
```

