### 1. 좋아요 구현

##### models.py

```python
class Article(models.Model):
	user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL)
```

> **오류메세지!**
>
> 생성 되는 모델 매니저의 이름이 겹치게 됩니다. 
>
> - 1 : N 관계에서 article.user // user.article_set 으로 사용하고 있음
> - M : N 관계에서 article.like_user // user.article_set 으로 생성되어 이름이 겹치게 됨
>
> 일반적으로 M:N 관계의 모델 매니저 이름을 바꾸게 됩니다.

```python
class Article(models.Model):
	like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
```

##### views.py

```python
def likes(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    
    if article.like_users.filter(pk=request.user.pk).exists():
    #if request.user in article.like_users.all():
        article.like_users.remove(request.user)
    else:
        article.like_users.add(request.user)
    return redirect('articles:index')
```

##### base.html

```django
# font awesome
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.15.3/css/all.css" integrity="sha384-SZXxX4whJ79/gErwcOYf+zWLeJdY/qpuqC4cAa9rOGUstPomtqpuNWT9wdPEn2fk" crossorigin="anonymous">
```

##### index.html

```django
<div>
    <form action="{% url 'articles:likes' article.pk %}" method="POST">
        {% csrf_token %}
        {% if request.user in article.like_users.all %}
        <button class="btn btn-link">
            <i class="fas fa-heart" style="color: crimson;"></i>
        </button>
        {% else %}
        <button class="btn btn-link">
            <i class="far fa-heart" style="color: crimson;"></i>
        </button>
        {% endif %}
    </form>
</div>
    
```



### 2. 팔로우 구현

##### models.py

```python
from django.db import models
from django.contrib.auth.models import AbstractUser

# Create your models here.
class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
```

##### urls.py

```python
path('<str:username>/', views.profile, name='profile'),
path('<str:username>/follow/', views.follow, name='follow'),
```

##### views.py

```python
def profile(request, username):
    if request.user.is_authenticated:
        person = get_object_or_404(get_user_model(), username=username)
        context = {
            'person': person,
        }
        return render(request, 'accounts/profile.html', context)
    return redirect('accounts:login')


@require_POST
def follow(request, username):
    if request.user.is_authenticated:
        person = get_object_or_404(get_user_model(), username=username)
        if request.user != person:
            if request.user in person.followers.all():
                person.followers.remove(request.user)
            else:
                person.followers.add(request.user)
        return redirect('accounts:profile', username)
    return redirect('accounts:login')
```

##### templates

```django
{% extends 'base.html' %}

{% block content %}
<h1>{{ person.username }}'s 프로필</h1>

<div>
  {% if request.user.is_authenticated and request.user != person %}
    <form action="{% url 'accounts:follow' person.username %}" method="POST">
      {% csrf_token %}
      {% if request.user in person.followers.all %}
        <button>Unfollow</button>
      {% else %}
        <button>Follow</button>
      {% endif %}
    </form>
  {% endif %}
</div>
<div>
  <p>followings: {{ person.followings.all|length }}</p>
  <ul>
    {% for following in person.followings.all %}
      <li>{{ following.username }}</li>
    {% endfor %}
  </ul>
  
  <p>followers: {{ person.followers.all|length }}</p>
  <ul>
    {% for following in person.followers.all %}
      <li>{{ following.username }}</li>
    {% endfor %}
  </ul>
</div>


<div>
  <a href="{% url 'articles:index' %}">BACK</a>
</div>
{% endblock %}
```



















