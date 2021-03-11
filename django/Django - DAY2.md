[TOC]

# Django

*The Web framework for perfectionists with deadlines*

## DAY2

### 1. Model

*"웹 어플리케이션의 데이터를 구조화하고 조작하기 위한 도구입니다."*

단일한 데이터에 대한 정보를 가집니다. 즉 사용자가 저장하는 데이터들의 필수적인 필드들과 동작들을 포함합니다. 저장된 데이터베이스의 구조(layout)입니다. django는 model을 통해 데이터에 접속하고 관리하게 됩니다. 일반적으로 각각의 model은 하나의 데이터베이스 테이블에 mapping을 합니다. 또한 프로젝트를 진행할 때 데이터베이스 스키마를 미리 정의하고 시작하는 경우가 많기 때문에 model 부분을 먼저 작성하는 경우가 많습니다.

#### 1.1. Database의 기본 구조

- 데이터베이스 (DB) : 체계화딘 데이터의 모임입니다.
- 쿼리 (Query) : 데이터를 조회하기 위한 명령어이자 조건에 맞는 데이터를 추출하거나 조작하는 명령어입니다.
- 스키마 (Schema) : DB에서 자료의 구조, 표현방법, 관계 등을 정의한 구조 (structure) 입니다.
- 테이블 (Table) : 필드 > 컬럼 > 속성 / 레코드 > 행 > 튜플
- 기본키 (PK) : 각 행의 고유값으로 Primary Key로 불립니다. 반드시 설정하여야하며, 데이터베이스 관리 및 관계 설정시 주요하게 활용되는 값입니다.



### 2. ORM

*"DB를 객체(object)로 조작하기 위해 ORM을 사용합니다."*

Object-Relational-Mapping 은 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템간에 (Django - SQL) 데이터를 변환하는 프로그래밍 기술입니다. 이것은 프로그래밍 언어에서 사용할 수 있는 '가상 객체 데이터베이스'를 만들어 사용합니다. 

#### 2.1. 특징

- 장점 : SQL을 잘 알지 못해도 DB조작이 가능합니다. 또한 SQL의 절차적 접근이 아닌 객체 지향적 접근으로 인한 높은 생산성을 낼 수 있습니다.
- 단점 : ORM 만으로 완전한 서비스를 구현하기 어려운 경우가 있습니다. 즉 제대로 된 프로젝트를 진행하기 위해서는 어느정도 SQL 문법을 알아야 합니다.

현대 웹 프레임워크의 사용 목적 혹은 요점은 웹 개발의 속도를 높이는 것 (생산성) 이라는 측면에서는 뛰어난 성능을 보여줍니다.

#### 2.2. 구현





### 3. Migration

*"Django가 Model에 생긴 변화를 반영하는 방법입니다."*

- makemigrations
- migrate
- sqlmigrate
- showmigrations

#### 3.1. Migrate Command

##### 3.1.1. makemigrations

Model을 변경한 것에 기반한 새로운 마이그레이션(like 설계도)을 만들 때 사용합니다.

##### 3.1.2. migrate

마이그레이션을 DB에 반영하기 위해 사용합니다. 설계도를 실제 DB에 반영하는 과정입니다. 모델에서의 변경 사항들과 DB의 스키마가 동기화를 이룹니다.

##### 3.1.3. sqlmigrate

마이그레이션에 대한 SQL 구문을 보기 위해 사용합니다. 마이그레이션이 SQL문으로 어떻게 해석되어서 동작할지 미리 확인 할 수 있습니다.

##### 3.1.4. showmigrations

프로젝트 전체의 마이그레이션 상태를 확인하기 위해 사용합니다. Migrate 되었는지 안 되었는지 여부를 확인할 수 있습니다.

#### 3.2. 구현

> **구현 3단계 절차**
>
> 1. models.py : model 변경사항 발생
> 2. python manage.py makemigrations : migrations 파일 생성
> 3. python manage.py migrate : DB 적용



### 4. DB API

*"DB를 조작하기 위한 도구"*

Django가 기본적으로 ORM을 제공함에 따른 것으로 DB를 편하게 조작할 수 있도록 도와주는 역할을 합니다. Model을 만들면 django는 객체들을 만들고(C) 읽고(R) 수정하고(U) 지울 수 있는(D) Database-abstract API를 자동으로 만들 수 있습니다. Database-abstract API = Database-access API

`Class Name + Manager + QuerySet API`

i.g.  `Article.objects.all()` : Article에 있는 모든 objects 모델을 조회하겠다.



#### 4.1. DB API 구성

##### 4.1.1. Manager

django 모델에 데이터베이스 query 작업이 제공되는 인터페이스입니다. 기본적으로 모든 django 모델 클래스 objects라는 Manager를 추가합니다. 클래스 뒤에 `objects`를 붙입니다.

##### 4.1.2. QuerySet

데이터베이스로부터 전달받은 객체 목록입니다. queryset 안의 객체는 0개, 1개 혹은 여러 개일 수 있습니다. 데이터베이스로부터 조회, 필터, 정렬 등을 수행할 수 있습니다.



#### 4.2. QuerySet API - CRUD

대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create, Read, Update, Delete 를 묶어서 일컫는 말입니다. i.g. 인스타그램에서 게시글을 만들고, 보고, 수정하고, 삭제하는 것 또한 CRUD 안에 속해있는 기능이라고 할 수 있습니다.

##### 4.2.1. Create

- `save()` method

  Saving Objects : 객체를 데이터베이스에 저장합니다. 데이터 생성 시 save()를 호출하기 전에는 객체의 ID 값이 무엇인지 알 수 없습니다.

```sql
article = Article()
article.title = 'first'
article.content = 'django!'
article.save()
```

```sql
article = Article(title='second', content='django!!')
article.save()
```

```sql
Article.objects.create(title='third', content='django!!!')
```

##### 4.2.2. Read

QuerySet API method를 사용한 다양한 조회를 하는 것이 중요합니다.

> 1. Methods that return new querysets
>
>    : all(), filter()
>
> 2. Methods that do not return querysets
>
>    : get()

- .all() : 클래스 내 객체 전부를 조회합니다.

- .get() : 반드시 존재하고, 유일한 객체를 조회합니다.

  객체가 없으면 DoesNotExist 에러 발생

  두 개 이상의 객체가 있으면 Error 발생

- .filer() 

> **Field lookups**
>
> 조회 시 특정 조건을 적용시키기 위해 사용합니다.
>
> `filter`, `get`, `exclude` 에 대한 키워드 인수로 사용 됩니다.

```sql
Article.objects.filter(content__contain='!')
Article.objects.filter(pk__gt=1)
```

##### 4.2.3. Update

```sql
article = Article.objects.get(pk=1)
article.title = 'update first'
article.save()
```

##### 4.2.4. Delete

```sql
article.delete()
```



### 5. Admin Site

#### 5.1. Automatic admin interface

사용자가 아닌 서버의 관리자가 활용하기 위한 페이지입니다. Article class를 admin.py에 등록하고 관리합니다. django.contrib.auth 모듈에서 제공합니다. record 생성 여부 확인에 매우 유용하며 직접 record를 삽입할 수 있습니다.

```django
# Article을 admin에 등록합니다
admin.site.register(Article)


# admin 페이지 custom하기
class ArticleAdmin(admin.ModelAdmin):
    list_display = ('pk', 'title', 'content', 'created_at', 'updated_at')

admin.site.register(Article, ArticleAdmin)
```

> - `python manage.py createsuperuser`
>
> Username (leave blank to use 'user'): admin
> Email address:
> Password:
> Password (again):
>
> Bypass password validation and create user anyway? [y/N]: y
> Superuser created successfully.

- 계정이 들어갈 테이블은 어디에 있을까? : auth_user 테이블에 데이터가 들어가게 됩니다. 



#### 5.2. POST

POST 요청은 리소스를 생성/변경하기 위해 데이터를 HTTP BODY에 담아 전송합니다. GET방식은 url에 변수들이 담겨서 보내지게 됩니다. 하지만 이는 url의 길이를 엄청나게 길어지게 할 뿐만 아니라 정보의 노출의 문제도 있습니다. POST 방식으로 데이터를 보낼 수 있습니다.

- GET -> CRUD 에서 R에만 해당

- POST -> CRUD에서 CUD에 해당

> CSRF Token
>
> 데이터를 전송할 때 인증을 목적으로 token(cookie)을 함께 데이터를 보내게 됩니다.

**Post로 변경 후 변화하는 것들**

- request.POST.get()
- csrf token도 함께 보내주어야 함
- 더이상 url에 내가 넘기는 데이터가 노출되지 않음
- POST는 HTML을 요청하는 것이 아니기 때문에 HTML 문서를 받아볼 수 있는 곳으로 다시 redirect해야 함



#### 5.3. DETAIL page

urls.py

```python
urlpatterns = [
    path('<int:pk>/', views.detail, name='detail')
]
```

views.py

```python
def detail(request, pk):
    # 몇 번째(pk) 글을 조회할건지 가져와야 합니다.
    article = Article.objects.get(pk=pk)
    context = {
        'article': article
    }
    return render(request, 'articles/detail.html', context)
```

detail.html

```django
{% extends 'base.html' %}

{% block content %}
  <h2>DETAIL</h2>
  <hr>
  <h2>제목 : {{ article.title }}</h2>
  <h3>{{ article.pk }} 번째 글</h3>
  <hr>
  <h3>내용 : {{ article.content }}</h3>
  <h3>작성시각 : {{ article.created_at }}</h3>
  <h3>수정시각 : {{ article.updated_at }}</h3>

{% endblock content %}
```

index.html

```django
{% extends 'base.html' %}

{% block content %}
  <h1>Articles</h1>
  <a href="{% url 'articles:new' %}">[NEW]</a>
  {% for article in articles %}
    <p>글 번호 : {{ article.pk }}</p>
    <p>글 제목 : {{ article.title }}</p>
    <p>글 내용 : {{ article.content }}</p>
    <a href="{% url 'articles:detail' article.pk %}">[DETAIL]</a>
    <hr>
  {% endfor %}
{% endblock %}
```

views.py

```python
def create(request):
    title = request.POST.get('title')
    content = request.POST.get('content')

    article = Article(title=title, content=content)
    article.save()
	
    # return redirect(f'articles/{article.pk}/')
    return redirect('articles:detail', article.pk)
```

#### 5.4. Delete page

urls.py

```python
urlpatterns = [
    path("<int:pk>/delete/", views.delete , name="delete"),
]
```

views.py

```python
def delete(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    else:
        return redirect('articles:detail', article.pk)
```

detail.html

```django
  <form action="{% url 'articles:delete' article.pk %}" method="POST">
    {% csrf_token %}
    <button class="btn btn-danger">DELETE</button>
  </form>
```



#### 5.5. Update page

