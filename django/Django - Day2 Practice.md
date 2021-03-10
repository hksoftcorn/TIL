[TOC]

# Django - Day2 Practice

*장고 익스텐션 설치 Django extension installation*

`pip install django-extensions`   # shell_plus 사용

```
#settings.py
INSTALLED_APPS = (
    ...
    'django_extensions',
)
```

`vscode : sqlite 설치`  # DB조회 가능



### 1. Model

#### 1.1. models.py

```python
from django.db import models

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title
    
```

django.db에서 임포트한 models 모듈을 사용합니다. 

Article 클래스를 생성하면서 models 안의 Model 클래스를 상속 받습니다. title과 content의 데이터필드를 설정합니다. created_at과 updated_at의 데이터필드는 DateTimeField로 동일하지만 설정값에 auto_now와 auto_now_add 차이가 있습니다. 

models.py 에서 모델 설계가 끝났으면 makemigrations 명령어를 통해 DB설계도면을 만들고 migrate 를 입력하여 DB를 생성합니다.

#### 1.2. makemigrations

`python manage.py makemigrations`

migrations 파일에 python파일들이 생성된 것을 확인할 수 있습니다. 즉 DB설계 도면이 만들어 진 것입니다.

#### 1.3. migrate

`python manage.py migrate`

참고) 깃헙을 pull 받으면 DB 자료들이 올라가지 않습니다. 설계 도면, 즉 makemigrations만 되어있는 상태이기 때문에 migrate를 하면 됩니다.

#### 1.4. admin.py

```python
from django.contrib import admin
from .models import Article

# Register your models here.
class ArticleAdmin(admin.ModelAdmin):
    list_display = ('pk', 'title', 'content', 'created_at', 'updated_at',)


# admin site에 register 하겠다.
admin.site.register(Article, ArticleAdmin)
```

관리자 파일입니다. `admin.site.register(Article)`을 통해 Article 클래스를 추가합니다. 추가로 admin 페이지에서 컬럼을 여러 개 확인하기 위해서 `admin.ModelAdmin` `list_display = ('pk', 'title', 'content', 'created_at', 'updated_at',)` 설정 클래스도 추가합니다.



### 2. CRUD

#### 2.1. Read

**views.py**

```python
from .models import Article

def index(request):
    # all() 모든 객체를 조회합니다.
    articles = Article.objects.all() 
    context = {
        'articles': articles,
    }
    return render(request, 'articles/index.html', context)
```

**index.html**

```django
{% extends 'base.html' %}

{% block content %}
  <h1>Articles</h1>
  {% for article in articles %}
    <p>글 번호 : {{ article.pk }}</p>
    <p>글 제목 : {{ article.title }}</p>
    <p>글 내용 : {{ article.content }}</p>
    <hr>
  {% endfor %}
{% endblock %}
```

Article 클래스에서 조회한 모든 객체를 articles에서 담아서 DTL 문법을 이용하여 {% for article in articles %} 반복문으로 요소들을 출력합니다. 요소들은 dot + 속성으로 접근할 수 있습니다.



#### 2.2. Create - Read

**views.py**

```python
def new(request):
    return render(request, 'articles/new.html')

def create(request):
    # new가 던진 데이터는 request에 들어가게 됩니다.
    title = request.GET.get('title')
    content = request.GET.get('content')

    # 받은 데이터를 DB에 저장합니다.
    """
    1. 인스턴스 만들고 / 인스턴스 변수 넣고 / DB저장
    2. 인스턴스 만들면서 키워드 변수 넣고 / DB저장
    3. 인스턴스 x, create로 바로 넣기
    """
    
    # 1. 길다
    # article = Article()
    # article.title = title
    # article.content = content
    # article.save()

    # 2.
    article = Article(title=title, content=content)
    article.save()

    # 3. 검증x
    # Article.objects.create(title=title, content=content)

    return render(request, 'articles/index.html')
```

**new.html**

```django
{% extends 'base.html' %}
{% block content %}
  <h1>NEW</h1>
  <form action="{% url 'articles:create' %}" method="GET">
    <label for="title">Title: </label>
    <input type="text" name="title" id="title"><br>
    <label for="content">Content: </label>
    <textarea type="text" name="content" id="content" cols="30" rows="5"></textarea>
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>

{% endblock content %}
```

new에서 두 개의 입력 값을 받습니다. title과 content입니다. 이 데이터는 create로 넘어가게 됩니다. label과 input 변수들을 잘 살펴보시길 바랍니다. 

**create.html**

```django
{% extends 'base.html' %}
{% block content %}
  <h1>CREATE</h1>
  <h2>게시글이 잘 작성되었습니다.</h2>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock content %}
```

views.py에서 정의한 create 함수에서 request 변수 속 GET을 통해서 new에서 보낸 데이터들을 받을 수 있습니다. 그리고 article = Article(title=title, content=content) 인스턴스를 받아서 article.save()를 통해 DB에 저장합니다. 



#### 2.3. POST

**new.html**

```django
{% block content %}
  <h1>NEW</h1>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
```

**views.py**

```python
def create(request):
    # new가 던진 데이터는 request에 들어가게 됩니다.
    title = request.POST.get('title')
    content = request.POST.get('content')
    '''
```

데이터를 전송(POST)할 때 인증을 목적으로 csrf_token(cookie)을 함께 데이터를 보내게 됩니다. create에서도 GET방식이 아닌 데이터를 보내는 방식인 POST로 수정하게 됩니다.

