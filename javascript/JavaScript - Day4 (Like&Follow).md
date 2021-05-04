# JS - Like & Follow

## 1. Like function

```django
{% extends 'base.html' %}

{% block content %}
  <h1>Articles</h1>
  {% if request.user.is_authenticated %}
    <a href="{% url 'articles:create' %}">[CREATE]</a>
  {% else %}
    <a href="{% url 'accounts:login' %}">[새 글을 작성하려면 로그인하세요.]</a>
  {% endif %}
  <hr>
  {% for article in articles %}
    <p>
      <b>작성자 : <a href="{% url 'accounts:profile' article.user.username %}">{{ article.user }}</a></b>
    </p>
    <p>글 번호 : {{ article.pk }}</p>
    <p>글 제목 : {{ article.title }}</p>
    <p>글 내용 : {{ article.content }}</p>
    <div>
      <form action="{% url 'articles:likes' article.pk %}" method="POST">
        {% csrf_token %}
        {% if request.user in article.like_users.all %}
          <button>좋아요 취소</button>
        {% else %}
          <button>좋아요</button>
        {% endif %}
      </form>
    </div>
    <p>{{ article.like_users.all|length }}명이 이 글을 좋아합니다.</p>
    <a href="{% url 'articles:detail' article.pk %}">[DETAIL]</a>
    <hr>
  {% endfor %}
{% endblock %}
```

### 0. 기본 설정하기

```django
python -m venv venv

source venv/Script/activate

pip install -r requirements.txt

python manage.py migrate

python manage.py seed articles --number=20

python manage.py createsuperuser

python manage.py runserver
```



### Step 1 - Form 설정

```django
<form class="like-form" data-article-id="{{ article.pk }}">
```

- class 이름설정 : `class="like-form"`
- data-article-id 이름설정 : `data-article-id="{{ article.pk }}"`

```javascript
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script>
const forms = document.querySelectorAll('.like-form')

forms.forEach(form => {
  form.addEventListener('submit', function (event) {
    event.preventDefault()
    const articleId = event.target.dataset.articleId
    })
})
</script>
```

- `event.preventDefault()` : submit 기능을 하지 않도록 evnet를 prevent 합니다.

- `articleId` : 

  - console.log(event) 를 찍어보면
  - event - target - dataset - articleId 속에 `articleId: "20"` 가 들어있는 것을 확인할 수 있습니다.

  ​

### Step 2 - CSRF token

```javascript
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script>

const forms = document.querySelectorAll('.like-form')
const csrftoken = document.querySelector('[name=csrfmiddlewaretoken]').value

forms.forEach(form => {
  form.addEventListener('submit', function (event) {
    event.preventDefault()
    const articleId = event.target.dataset.articleId

    axios({
      method: 'post',
      url: `http://127.0.0.1:8000/articles/${articleId}/likes/`,
      headers: {'X-CSRFToken': csrftoken},
    })
      .then(response => {
      console.log(response)
    })
  })
})
</script>
```

- axios

  - `csrftoken` : 값을 key-value 쌍으로 입력하여 줍니다.

  - `.then()` : post가 제대로 수행되었다면, response 파라미터로 들어옵니다.

    ​

### Step 3 - 좋아요 수정

```python
# views.py
from django.http import JsonResponse

@require_POST
def likes(request, article_pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=article_pk)

        if article.like_users.filter(pk=request.user.pk).exists():
            article.like_users.remove(request.user)
            liked = False
        else:
            article.like_users.add(request.user)
            liked = True
            
        context = {
            'liked': liked,
            'count': article.like_users.count(),
        }
        return JsonResponse(context)
    return redirect('accounts:login')
```

```django
<div>
  <form class="like-form" data-article-id="{{ article.pk }}">
    {% csrf_token %}
    {% if request.user in article.like_users.all %}
      <button id="like-{{ article.pk }}">좋아요 취소</button>
    {% else %}
      <button id="like-{{ article.pk }}">좋아요</button>
    {% endif %}
  </form>
</div>
  
<p>
  <span id="like-count-{{ article.pk }}">0</span>
  명이 이 글을 좋아합니다.
</p>
```

```javascript
<script>

const forms = document.querySelectorAll('.like-form')
const csrftoken = document.querySelector('[name=csrfmiddlewaretoken]').value

forms.forEach(form => {
  form.addEventListener('submit', function (event) {
    event.preventDefault()
    const articleId = event.target.dataset.articleId

    axios({
      method: 'post',
      url: `http://127.0.0.1:8000/articles/${articleId}/likes/`,
      headers: {'X-CSRFToken': csrftoken},
    })
      .then(response => {
      console.log(response)
      const count = response.data.count
      const liked = response.data.liked

      const likeButton = document.querySelector(`#like-${articleId}`)
      likeButton.innerText = liked ? '좋아요 취소' : '좋아요'

      const likeCount = document.querySelector(`#like-count-${articleId}`)
      likeCount.innerText = count
    })
  })
})
</script>
```

- 좋아요 button : `id="like-{{ article.pk }}"`
- 좋아하는 사람 수 count : `id="like-count-{{ article.pk }}"`



### Step 4 - Undefined 해결

```django
{% comment %} 1. button 보이지 않게 해결 {% endcomment %}

<div>
  <form class="like-form" data-article-id="{{ article.pk }}">
    {% csrf_token %}
    {% if request.user.is_authenticated %}
    	{% if request.user in article.like_users.all %}
    		<button id="like-{{ article.pk }}">좋아요 취소</button>
    	{% else %}
    		<button id="like-{{ article.pk }}">좋아요</button>
    	{% endif %}
    {% endif %}
  </form>
</div>
```

```python
# 2. 401 error로 수정하기
# views.py

return HttpResponse(status=401)
```

```javascript
.catch(error => {
  // console.log(error.response)
  if (error.response.status === 401){
    window.location.href = '/accounst/login/'
  }
})
```



## 2. Follow Function

```html
{% extends 'base.html' %}

{% block content %}
<h1>{{ person.username }}님의 프로필</h1>

{% with followings=person.followings.all followers=person.followers.all %}
  <div>
    <div id="follow-count">
      팔로잉 : {{ followings|length }} / 팔로워 : {{ followers|length }}
    </div>
    {% if request.user != person %}
      <div>
        <form id="follow-form" data-user-id="{{ person.pk }}">
          {% csrf_token %}
          {% if request.user in followers %}
            <button>언팔로우</button>
          {% else %}
            <button>팔로우</button>
          {% endif %}
        </form>
      </div>
    {% endif %}
  </div>
{% endwith %}


<hr>

<h2>{{ person.username }}'s 게시글</h2>
{% for article in person.article_set.all %}
  <div>{{ article.title }}</div>
{% endfor %}

<hr>

<h2>{{ person.username }}'s 댓글</h2>
{% for comment in person.comment_set.all %}
  <div>{{ comment.content }}</div>
{% endfor %}

<hr>

<h2>{{ person.username }}'s likes</h2>
{% for article in person.like_articles.all %}
  <div>{{ article.title }}</div>
{% endfor %}

<hr>

<a href="{% url 'articles:index' %}">[back]</a>

<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script>
  const form = document.querySelector('#follow-form')
  const csrftoken = document.querySelector('[name=csrfmiddlewaretoken]').value

  form.addEventListener('submit', function (event) {
    event.preventDefault()
    // console.log(event)
    const userId = event.target.dataset.userId

    axios({
      method: 'post',
      url: `accounts/${userId}/follow/`,
      headers: {'X-CSRFToken': csrftoken},
    })
      .then(response => {
        console.log(response)
        // button
        const followBtn = document.querySelector('#follow-form > button')
        const isFollowed = response.data.followed

        if (isFollowed) {
          followBtn.innerText = '언팔로우'
        } else {
          followBtn.innerText = '팔로우'
        }
        // count
        const followers_count = response.data.followers_count
        const followings_count = response.data.followings_count

        const followCountDiv = document.querySelector('#follow-count')
        followCountDiv.innerText = `팔로잉 : ${ followings_count } / 팔로워 : ${ followers_count }`
      })
      .catch(error => {
        // console.log(error)
        if (error.response.status === 401){
              window.location.href = '/accounst/login/'
      })
  })
</script>
{% endblock %}

```

```python
# views.py
@require_POST
def follow(request, user_pk):
    if request.user.is_authenticated:
        you = get_object_or_404(get_user_model(), pk=user_pk)
        me = request.user

        if you != me:
            if you.followers.filter(pk=me.pk).exists():
                you.followers.remove(me)
                followed = False
            else:
                you.followers.add(me)
                followed = True
            context = {
                'followed': followed,
                'followers_count': you.followers.count(),
                'followings_count': you.followings.count(),
            }
        return JsonResponse(context)
    return HttpResponse(status=401)
```



