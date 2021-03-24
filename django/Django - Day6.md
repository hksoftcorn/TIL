[TOC]

# Django - Day6

*The Web framework for perfectionists with deadlines*

## 1. Article - Model Realtionship

### 1.1. Relationship fields

댓글 기능을 구현하려고 합니다. 각 게시글마다에 댓글을 달 수 있습니다.

##### 모델 간 관계를 나타내는 필드

- Many to one (1:N) = Foreignkey()
- Many to Many (M:N) = ManyToManyField()
- One to One (1:1) = OneToOneField()

##### Foreign Key (외래키)

- RDBMS에서 한 테이블의 필드 중 다른 테이블의 행(row)을 식별할 수 있는 키
- 참조하는 테이블에서 1개의 키값은, 참조되는 측 테이블의 행 값에 대응된다.
- 하나의 테이블이 여러 개의 외래 키를 포함할 수 있다. 그리고 이러한 외래 키들은 각각 서로 다른 테이블을 참조할 수 있다.
- 참조하는 테이블과 참조되는 테이블이 동일할 수도 있다. (재귀적 외래 키)

##### Foreign Key (외래키) 특징

- 키를 사용하여 부모 테이블의 유일한 값을 참조 (참조 무결성)
- 외래 키의 값이 반드시 부모 테이블의 기본 키일 필요는 없지만 유일해야 함 (유일성)
- 데이터 무결성 : 개체 무결성, 참조 무결성, 범위 무결성

##### Foreignkey()

- django에서 일대다를 표현하기 위한 model field
- 2개의 필수 위치 인자가 필요 : 참조하는 모델 클래스, on_delete 옵션
  - on_delete는 외래키가 참조하는 객체가 사라졌을 때 외래키를 가진 객체를 어떻게 처리할 지 정의
  - 데이터 무결성을 위해서 매우 중요한 설정입니다.
  - `CASCADE` / PROTECT / SET_NULL / SET_DEFAULT / SET() / DO_NOTHING / RESTRICT

**articles - models.py**

```python
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content
```

- `python manage..py makemigrations`
- `python manage..py migrate`

- foreign key로 참조하는 클래스 article이 DB에 들어갈 때는 `article_id` 로 칼럼에 들어가게 됩니다.

##### 1:N model manager

```django
comment = Comment()
comment.content = '댓글1'
# comment.save() # 댓글내용과 게시글 번호를 같이 입력해야 합니다.

Artcile.objects.create(title='제목1', content='내용1')
article = Article.objects.get(pk=1)
comment.artilce = article
comment.save()
comment.article_id
comment.pk # 고유키
comment = Comment(content='댓글2', article=article)
comment.save()
```

**articles - admin**

```python
from django.contrib import admin
from .models import Article, Comment

# Register your models here.
admin.site.register(Article)
admin.site.register(Comment)
```

##### 역참조 Article(1)이 Comment(N)을 참조

- comment_set
- django에서는 역참조시 {모델이름}_set 형식의 manager를 생성
- article.comment_set.all()

```python
article = Article.objects.get(pk=1)
comments = article.comment_set.all()
comments
```

##### 역참조 related_name

- model의 정의부터 새로 해야함

  ```python
  class Comment(models.Model):
      article = models.ForeignKey(
      	Article,
          on_delete=models.CASCADE,
          related_name='comments'
      )
  ```

- 1:N 관계에서는 거의 사용하지 않지만, M:N 관계에서는 반드시 사용

- article.comments.all()



### 1.2. 댓글 구현

##### forms.py

```python
class CommentForm(forms.ModelForm):

    class Meta:
        model = Comment
        # fields = '__all__'
        exclude = ('article',)
```

##### views.py

```python
@require_safe
def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm()
    context = {
        'article': article,
        'comment_form': comment_form,
    }
    return render(request, 'articles/detail.html', context)
```

#### 1.2.1. Create

##### urls.py

```python
path('<int:pk>/comments/', views.comments_create, name='comments_create'),
```

##### views.py

```python
from django.http import HttpResponse
@require_POST
def comments_create(request, pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=pk)
        comment_form = CommentForm(request.POST)
        if comment_form.is_valid():
            comment = comment_form.save(commit=False)
            comment.article = article
            comment.save()
            return redirect('articles:detail', article.pk)
        context = {
            'comment_form': comment_form,
            'article': article,
        }
        return render(request, 'articles/detail.html', context)
    else:
        return redirect('accounts:login')
    	# return HttpResponse(status=401)
        
```

- comment 라는 인스턴스를 만들 때 commit=False 하여 DB에 아직 쿼리를 날리지 않습니다.
- comment 인스턴스에 article 외래키를 추가하여 줍니다.
- 마지막으로 .save() 하여 DB에 저장합니다.

##### detail.html

```django
<form action="{% url 'articles:comments_create' article.pk %}" method="POST">
    {% csrf_token %}
    {{ comment_form }}
    <input type="submit">
</form>
```

#### 1.2.2. Read

##### views.py

```python
@require_safe
def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm()
    comments = article.comment_set.all()
    context = {
        'article': article,
        'comment_form': comment_form,
        'comments': comments,
    }
    return render(request, 'articles/detail.html', context)
```

##### detail.html

```django
<h4>댓글 목록</h4>
  <ul>
    {% for comment in comments %}
      <li>
          {{ comment }}
      </li>
    {% endfor %}
  </ul>
```

#### 1.2.3. Delete

##### urls.py

```python
path('<int:article_pk>/comments/<int:comment_pk>/delete/', views.comments_delete, name='comments_delete'),
```

##### views.py

```python
@require_POST
def comments_delete(request, article_pk, comment_pk):
    if request.user.is_authenticated:
        comment = get_object_or_404(Comment, pk=comment_pk)
        comment.delete()
    return redirect('articles:detail', article_pk)
```

##### detail.html

```django
<h4>댓글 목록</h4>
{{ comments|length }}개
{{ article.comment_set.all|length }}개
{{ comments.count }}개
  <ul>
    {% for comment in comments %}
      <li>
        {{ comment }}
        <form action="{% url 'articles:comments_delete' article.pk comment.pk %}"
              method="POST" class="d-inline">
          {% csrf_token %}
          <input type="submit" value="DELETE">
        </form>
      </li>
    {% empty %}
      <p>아직 댓글이 없네요..</p>
    {% endfor %}
  </ul>
```



## 2. USER - Customizing authentication

### 2.1. Substituting a custom User model

- 일부 프로젝트에서는 built-in User model이 제공하는 인증 요구사항이 적절하지 않을 수 있습니다.
- django는 custom model을 참조하는 AUTH_USER_MODEL 설정을 제공하여 기본 user model을 재정의(override)할 수 있도록 함
- 새 프로젝트를 시작하는 경우 기본 사용자 모델이 충분하더라도, 커스텀 유저 모델을 서정하는 것을 강력하게 권장합니다.
- 커스텀 유저 모델은 기본 사용자 모델과 동일하게 작동하면서도 필요한 경우 나중에 맞춤 설정할 수 있기 때문
- 단, 프로젝트의 모든 migrations 혹은 첫 migrate를 실행하기 전에 이 작업을 마쳐야 합니다.

#### 2.1.1. AUTH_USER_MODEL

- User를 나타내는데 사용하는 모델
- 기본 값은 'auth.User'

#### 2.1.2. AbstractBaseUser & AbstractUser

model - Abstract Base User - Abstract User - User

**AbstractBaseUser**

- 기본적으로 password와 last_login만 제공
- 자유도가 높지만 필요한 다른 필드는 모두 직접 작성해야 함

**AbstractUser**

- 관리자 권한과 함께 완전한 기능을 갖춘 사용자 모델을 구현하는 기본 클래스

> Abstract base classes
>
> - 몇 가지 공통 정보를 여러 다른 모델에 넣을 때 사용하는 클래스입니다.
> - 데이터베이스 테이블을 만드는 데 사용되지 않으며, 대신 다른 모델의 기본 클래스로 사용되는 경우 해당 필드가 하위 클래스의 필드에 추가 됨

##### models.py

```python
from django.db import models
from django.contrib.auth.models import AbstractUser

# Create your models here.
class User(AbstractUser):
    pass
```

##### settings.py

```python
AUTH_USER_MODEL = 'accounts.User'
```



### 2.2. 초기화 및 재정의

#### 2.2.1. Steps

1. 0001_initial.py 0002_comment.py 등 설계도 파일 지우기

2. DB 지우기

3. 처음부터 makemigrations 하기

> - DB에서 확인 : auth.auth_user 에서 accounts_user 테이블로 바뀌게 됩니다.

#### 2.2.2. accounts_user로 바꾸기

기본 참조 모델을 바꾸어줍니다. forms.py로 들어가 `UserCreationForm` 과 `UserChangeForm` 부분을 커스텀하게 됩니다. get_user_model()을 통해 활성화되어 있는 모델을 리턴합니다.

##### forms.py

```python
from django.contrib.auth.forms import UserCreationeForm

class CustomUserCreationForm(UserCreationeForm):

    class Meta(UserCreationForm.Meta):
        model = get_user_model()
        fields = UserCreationeForm.Meta.field + ('email')
```

##### views.py

```python

@require_http_methods(['GET', 'POST'])
def signup(request):
    if request.user.is_authenticated:
        return redirect('articles:index')

    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            auth_login(request, user)
            return redirect('articles:index')
    else:
        form = CustomUserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/signup.html', context)
```

#### 2.2.3. 모델클래스 재 정의

##### models.py

```python
user = models.ForeignKey(settings.AuTH_USER_MODEL, on_delete=models.CASCADE)
```

> **유저 모델 참조하기**
>
> - `settings.AUTH_USER_MODEL` : 문자열 'accounts.User' 반환
>   - 유저 모델에 대한 외래 키 또는 M:N 관계를 정의할 때 사용
>   - 즉, models.py에서 유저 모델을 참조할 때 사용
> - `get_user_model()` : User 객체 반환
>   - django는 User 모델을 직접 참조하는 대신 get_user_model()을 사용하여 사용자 모델을 참조하라고 권장
>   - 현재 활성화(active)된 유저 모델 (지정된 커스텀 유저 모델, 그렇지 않은 경우 User)을 반환
>   - 즉, models.py가 아닌 다른 모든 곳에서 유저 모델을 참조할 때 사용

만들어지는 테이블의 FK 칼럼이름은 `user_id`가 됩니다.

##### forms.py

```python
from django import forms
from .models import Article, Comment


class ArticleForm(forms.ModelForm):

    class Meta:
        model = Article
        fields = ('title', 'content',)

        
class CommentForm(forms.ModelForm):

    class Meta:
        model = Comment
        # fields = '__all__'
        exclude = ('article',)
```









