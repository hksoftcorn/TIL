# REST

*"프로그래밍을 통해 요청에 RESTful한 방식으로 JSON을 응답하는 서버 만들기"*

## 1. REST API

#### API 

- 프로그래밍 언어가 제공하는 기능을 수행할 수 있게 만든 인터페이스
  - 어플리케이션과 프로그래밍으로 소통하는 방법
- 프로그래밍을 활용해서 할 수 있는 어떤 것
- CLI, GUL 는 각각 명령줄과 그래픽을 통해서 특정 기능을 수행하는 것이며  API는 프로그래밍을 통해 그 일을 수행할 수 있음

#### Web API

- 웹 어플리케이션 개발에서 다른 서비스에 요청을 보내고 응답을 받기 위해 정의된 명세
- 현재 웹 개발은 추가로 직접 모든 것을 개발하지 않고 여러 open API를 가져와서 활용하는 추세
  - i.g. 구글, 카카오 지도 API, 우편번호, 도로명, 지번 소 검색 API 등

#### API Server

- Client - Server : 요청과 응답



#### REST (Represntational State Transfer)

> '자원'과 '주소'를 지정하는 방법
>
> 자원(URI) - 행위(HTTP Method) - 표현(Representations)

- 웹 설계 상의 장점을 최대한 활용 할 수 있는 아키텍처 방법론
- 네트워크 아키텍쳐 원리의 모음
  - 1. 자원을 정의
  - 2. 자원에 대한 주소를 지정하는 방법

- REST 원리를 따르는 시스템 혹은 API를 RESTful API라고 하기도 함



### 1.1. URI & URL & URN

#### URI

- URI (Uniform Resource Identifier)

- 통합 자원 식별자
- 인터넷의 자원을 나타내는 `유일한 주소`
- 인터넷에서 자원을 식별하거나 이름을 지정하는 데 사용되는 간단한 문자열
- 하위 개념
  - URL / URN
- 구조
  - Protocol + Host + Port + Path
  - query (?, ), fragment (#, 북마크)
- URI 설계 시 주의사항
  - 밑줄(_)이 아닌 하이픈(-) 사용 권장
  - 소문자 사용
  - 파일 확장자는 포함시키지 않음

#### URL

- URL (Uniform Resource Locator)
- 통합 자원 위치
- 네트워크 상에 자원이 어디 있는지(주소)를 알려주기 위한 약속
- 자원은 HTML 페이지, CSS 문서, 이미지 등이 될 수 있음
- '웹 주소' 또는 '링크'라고도 불림

#### URN

- URN (Uniform Resource Name)
- 통합 자원 이름
- URL고 ㅏ달리 자원의 위치에 영향을 받지않는 유일한 이름 역할을 함 (독립적 이름)
- 자원의 이름이 변하지 않는 한 자원의 위치를 이곳저곳 옮겨도 문제없이 동작

> URN은 자원의 ID를 정의하고, URL은 자원을 찾는 방법을 제공
>
> 따라서 URN과 URL은 상호 보완적임



### 1.2. HTTP Method

#### HTTP (HyperText Transfer Protocol)

- HTML 문서와 같은 자원들을 가져올 수 있도록 해주는 프로토콜 (규칙, 약속)
- 웹에서 이루어지는 모든 데이터 교환의 기초
- 클라이언트 - 서버 프로토콜
- 요청 : 클라이언트에 의해 전송되는 메시지
- 응답 : 서버에서 응답으로 전송되는 메시지

- 특징
  - 비연결지향 : 서버는 응답 후 접속을 끊음
  - 무상태 : 접속이 끊어지면 클라이언트와 서버 간의 통신이 끝나며 상태를 저장하지 않음
  - 쿠키와 세션 등장

#### HTTP Method

- 자원에 대한 행위
- HTTP는 HTTP Method를 정의하여 주어진 자원에 수행하길 원하는 행동을 나타냄
- 의미론적으로 행위를 규정하기 때문에 '실제 그 행위 자체가 수행됨'을 보장하진 않음
- HTTP verbs 라고도 함

- 종류
  - GET : 오직 데이터를 받기만 함
  - POST : 서버로 데이터를 전송하며, 서버에 변경사항을 만듦
  - PUT : 요청한 주소의 자원을 수정
  - DELETE : 지정한 자원을 삭제
  - 자원에 대한 행위를 HTTP Method로 작성하자!

```python
# Quiz
# RESTful 철학을 잘 지켰는가?
1. GET articles/1/read/
x
행위

2. GET articles/1/delete/
x
행위, 자원에 대한 행위는 HTTP Method로 작성함

```

### 1.3. Representation

#### JSON

- 자바스크립트 객체 문법을 따라며, 구조화된 데이터를 표현하기 위한 문자 기반 데이터 포맷
- 일반적으로 웹 어플리케이션에서 클라이언트로 데이터를 전송할 때 사용
- 특징
  - 사람이 읽고 쓰기 쉽고 기계가 파싱(해석 & 분석)하고 만들어 내기 쉬움
    - Key-value 구조
    - 문자열 → JSON 객체 : 파싱(Parsing)
    - JSON 객체 → 문자열 : Stringification
  - 자바스크립트가 아니여도 JSON을 읽고 쓸 수 있는 기능이 다양한 프로그래밍 언어 환경에서 지원됨



> **REST 핵심 규칙**
>
> 1. `URI`는 정보의 자원을 표현해야 한다.
> 2. 자원에 대한 (어떠한)행위는 `HTTP Method`로 표현한다.

서버에서는 JSON만 뿌려주고 → 프론트엔드 프레임워크에서 JSON을 받아 꾸미게 됩니다.



## 2. Django REST Framework

> url - view - templates

### 준비 - 익스텐션 설치

- django-seed [↗](https://pypi.org/project/django-seed/)

```python
# root - urls.py
urlpatterns = [
    path('api/v1/', include('articles.urls'))
]


# articles - urls.py
from django.urls import path
from . import views
urlpatterns = [
    path('html/', views.article_html)
]

```

```python
pip install django-seed

# settings.py
django_seed

python manage.py makemigrations
python manage.py migrate
python manage.py seed articles --number=20
```

```python
# views.py
from .models import Article

def article_html(request):
    articles = Article.objects.all()
    context = {
        'articles': articles,
    }
    return render(request, 'articles/article.html', context)
```

```django
# templates - articles - article.html

```



### 방법1 - JsonResponse

```python
# articles - urls.py
urlpatterns = [
    path('html/', views.article_html)
    path('json-1/', views.article_json_1)
]


# views.py
from .models import Article
from django.http.response import JsonResponse

def article_json_1(request):
    articles = Article.objects.all()
    articles_json = []
    
    for article in articles:
        articles_json.append(
            {
                'id': article.pk,
                'content': article.content,
            }
        )
    return JsonResponse(articles_json, safe=False) # 딕셔너리가 아닌 타입이 입력되면 safe=False
```



### 방법2 - serializers, HttpResponse

```python
# articles - urls.py
urlpatterns = [
    path('html/', views.article_html)
    path('json-1/', views.article_json_1)
    path('json-2/', views.article_json_2)
]

# views.py
from .models import Article
from django.http.response import JsonResponse, HttpResponse
from django.core import serializers

def article_json_2(request):
    articles = Article.objects.all()
    data = serializers.serialize('json', articles)
    return HttpResponse(data, content_type='application/json')
```

- content-type [↗](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)
  - 문서가 어떤 타입인가를 명시합니다.
  - 우리가 보내는 content 타입은 application/json 입니다.
- Serialization (직렬화)
  - 데이터 구조나 객체 상태를 동일하거나 다른 컴퓨터 환경에 저장하고 나중에 재구성할 수 있는 포맷으로 변환하는 과정



### 방법3 - DRF

DRF - Django Rest Framework [↗](https://www.django-rest-framework.org/)

```python
pip install djangorestframework

# settings.py
INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

- Web API 구축을 위한 강렬한 ToolKit을 제공
- REST framework 개발에 필요한 다양한 기능을 제공
- Serialization (직렬화)
  - 데이터 구조나 객체 상태를 동일하거나 다른 컴퓨터 환경에 저장하고 나중에 재구성할 수 있는 포맷으로 변환하는 과정
  - 예를 들어 DRF의 Serializer는 Django의 QuerySet 및 Model Instance와 같은 복잡한 데이터를, JSON, XML 등의 유형으로 쉽게 변환할 수 있는 Python 데이터 타입으로 만들어 줌
  - DRF의 Serializer는 Django의 Form 및 ModelForm 클래스와 매우 유사하게 작동
  - XML, JSON, JSONL, YAML

|          | Django    | DRF             |
| -------- | --------- | --------------- |
| Response | HTML      | JSON            |
| Model    | ModelForm | ModelSerializer |

```python
# articles - serializers.py
# https://www.django-rest-framework.org/api-guide/serializers/#modelserializer
from rest_framework import serializers
from .models import Article

class ArticleSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Article
        fields = '__all__'
```

```python
# articles - urls.py
urlpatterns = [
    path('html/', views.article_html)
    path('json-1/', views.article_json_1)
    path('json-2/', views.article_json_2)
    path('json-3/', views.article_json_3)
]

# views.py
# https://www.django-rest-framework.org/tutorial/2-requests-and-responses/#wrapping-api-views
from rest_framework import Response
from rest_framework.decorators import api_view

from .models import Article
from django.http.response import JsonResponse, HttpResponse
from django.core import serializers
from .serializers import ArticleSerializer

@api_view(['GET'])
def article_json_3(request):
    articles = Article.objects.all()
    serializer = ArticleSerializer(articles, many=True)
    return Response(serializer.data)
```



## 3. Practice - Article

```python
# models.py
from django.db import models

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

```

```python
# serializers.py
from rest_framework import serializers
from .models import Article


class ArticleListSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Article
        fields = ('id', 'title',)


class ArticleSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Article
        fields = '__all__'
```

#### serializer 실습

```python
# shell_plus
# 단일 객체
article = Article.objects.get(pk=1)
serializer = ArticleListSerializer(article)
serializer.data

# 복수 객체
articles = Article.objects.all()
# serializer = ArticleListSerializer(articles)
serializer = ArticleListSerializer(articles, many=True)
serializer.data
```

#### url 설계

|             | GET          | POST    | PUT          | DELETE       |
| ----------- | ------------ | ------- | ------------ | ------------ |
| articles/   | 전체 글 조회 | 글 작성 | 전체 글 수정 | 전체 글 삭제 |
| articles/1/ | 1번 글 조회  | -       | 1번 글 수정  | 1번 글 삭제  |

### GET

```python
# Create your views here.
@api_view(['GET'])
def article_list(request):
    articles = Article.objects.all()
    serializer = ArticleListSerializer(articles, many=True)
    return Response(serializer.data)
```

### POST

postman [↗](https://www.postman.com/downloads/)

Workspace - Tap - 주소 넣어서 response 확인

```python
# views.py
# RESTful 방식의 post를 참고합니다.
# https://www.django-rest-framework.org/tutorial/2-requests-and-responses/#pulling-it-all-together

from rest_framework import status
from rest_framework.response import Response
from rest_framework.decorators import api_view
from django.shortcuts import render, get_object_or_404
from .models import Article
from .serializers import ArticleListSerializer, ArticleSerializer

# Create your views here.
@api_view(['GET', 'POST'])
def article_list(request):
    if request.method == 'GET':
        articles = Article.objects.all()
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = ArticleListSerializer(data=request.data)
        # if serializer.is_valid():
        if serializer.is_valid(raise_exception=True): # 오류시 return 400을 반환합니다.
            serializer.save()
            # 201 : Successed Created
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        # 400 : Bad Request
        # return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

POST 요청 확인

- POST http://127.0.0.1:8000/api/v1/articles/
- Body - form-data
- key - value 작성하여 POST 확인하기
- 201 / 400 확인

### DELETE & PUT

```python
@api_view(['GET', 'DELETE', 'PUT'])
def article_detail(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    if request.method == 'GET':
        serializer = ArticleSerializer(article)
        return Response(serializer.data)
    elif request.method == 'DELETE':
        article.delete()
        data = {
            'delete': f'{article_pk}번 글이 삭제되었습니다.',
        }
        # 204 : No content
        return Response(data, status=status.HTTP_204_NO_CONTENT)
    elif request.method == 'PUT':
        serializer = ArticleSerializer(article, data=request.data)
        if serializer.is_valid(raise_exception=True): # 오류시 return 400을 반환합니다.
            serializer.save()
            return Response(serializer.data)
```

```json
# result
{
    "id": 22,
    "title": "수정수정",
    "content": "수정수정",
    "created_at": "2021-04-27T01:40:17.540045Z",
    "updated_at": "2021-04-27T02:12:49.405295Z"
}
```



## 4. Practice - Comment

|                      | GET            | POST      | PUT           | DELETE        |
| -------------------- | -------------- | --------- | ------------- | ------------- |
| articles/            | 전체 글 조회   | 글 작성   | 전체 글 수정  | 전체 글 삭제  |
| articles/1/          | 1번 글 조회    | -         | 1번 글 수정   | 1번 글 삭제   |
| comments/            | 전체 댓글 조회 | -         | -             | -             |
| comments/1/          | 1번 댓글 조회  | -         | 1번 댓글 수정 | 1번 댓글 삭제 |
| articles/1/comments/ | -              | 댓글 작성 | -             | -             |

### Comment - GET

```python
# Create your views here.
@api_view(['GET'])
def comment_list(request):
    comment = get_list_or_404(Comment)
    serializer = CommentListSerializer(comment, many=True)
    return Response(serializer.data)
```

### Comment - detail

```python
@api_view(['GET'])
def comment_detail(request, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    serializer = CommentListSerializer(comment)
    return Response(serializer.data)
```

### Comment - POST

```python
# serializers.py
class CommentListSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Comment
        fields = '__all__'
        read_only_fields = ('article',)
```

```python
@api_view(['POST'])
def comment_create(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    serializer = CommentListSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True):
        serializer.save(article=article)
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```

### Comment - DELETE, PUT

```python
@api_view(['GET', 'DELETE', 'PUT'])
def comment_detail(request, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    if request.method == 'GET':
        serializer = CommentListSerializer(comment)
        return Response(serializer.data)

    elif request.method == 'DELETE':
        comment.delete()
        data = {
            'delete': f'댓글 {comment_pk}번이 삭제되었습니다.'
        }
        return Response(data, status=status.HTTP_204_NO_CONTENT)

    elif request.method == 'PUT':
        serializer = CommentListSerializer(comment, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)

```



## 5. Practice - Article Comment

- 특정 게시글에 작성된 댓글 목록 출력
- 특정 게시글에 작성된 댓글의 개수 출력

```python
# serializers.py

class ArticleSerializer(serializers.ModelSerializer):
    # 기존 필드 override
    # 방법1
    # comment_set = serializers.PrimaryKeyRelatedField(many=True, read_only=True)
    # 방법2
    comment_set = CommentListSerializer(many=True, read_only=True)
    # 댓글의 개수를 구합니다.
    comment_count = serializers.IntegerField(read_only=True, source='comment_set.count')

    class Meta:
        model = Article
        fields = '__all__'

```



### SWAGGER [↗](https://swagger.io/)

```python
pip install -U drf-yasg


# settings.py
INSTALLED_APPS = [
   ...
   'drf_yasg',
   ...
]


# urls.py
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi


schema_view = get_schema_view(
   openapi.Info(
      title="Snippets API",
      default_version='v1',
      description="Test description",
      terms_of_service="https://www.google.com/policies/terms/",
      contact=openapi.Contact(email="contact@snippets.local"),
      license=openapi.License(name="BSD License"),
   ),
   public=True,
   permission_classes=(permissions.AllowAny,),
)


urlpatterns = [

    path('swagger/$', schema_view.with_ui('swagger', cache_timeout=0)),
]
```

