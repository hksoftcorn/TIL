# Django - Day6

*The Web framework for perfectionists with deadlines*

## 1. Authentication System

### 1.1. Authentication & Authorization

- Authentication : 인증, 자신이 누구라고 주장하는 사람의 신원을 확인하는 것
- Authorization : 권한/허가, 가고 싶은 곳으로 가도록 혹은 원하는 정보를 얻도록 허용하는 과정





## 2. Model Relationship

### 2.1. M : N 관계

##### 좋아요 & 팔로우하기

> 좋아요 : Article - User
>
> 팔로우하기 : User - User

##### i.g. 병원 진료 시스템 

- 내원하는 환자 / 의사간의 예약 시스템을 구축
- 환자와 의사 관계 - N : M
- class Reservation 중개 모델을 생성합니다.
- {Model}_set.all()
- `'through = reservation'`
- 다대 다 테이블에서 하나의 테이블에만 FK를 넣게 됩니다 
  - 환자 : through = reservation
  - 의사 : related_name = 'patients' (역참조)
  - source 모델과 target 모델을 정해줍니다.
  - source 모델은 through / tartget 모델은 related_name을 설정합니다.



### 2.2. 구현하기

#### 1) M:N 역참조 (_set)

```
pip install django django-extentions ipython
django-admin startproject crud .
python manage.py startapp hospitals
```

##### settings.py

```python
INSTALLED_APPS = [
    'hospitals',
    'django_extensions',
]
```

##### models.py

```python
from django.db import models

class Doctor(models.Model):
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'
    
    
class Patient(models.Model):
    name = models.TextField()
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)  # through=''
    
    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

```python
python manage.py makemigrations
python manage.py migrate
python manage.py shell_plus
```

##### shell_plus

```python
doctor1 = Doctor.objects.create(name='justin')
doctor2 = Doctor.objects.create(name='eric')
patient1 = Doctor.objects.create(name='tony', doctor=doctor1)
patient2 = Doctor.objects.create(name='harry', doctor=doctor2)
```

```python
# 1번 환자는 2번 의사로 바꾸고 싶다.
patient3 = Doctor.objects.create(name='tony', doctor=doctor2)
# 2번 환자는 1, 2번 의사와 함께 진료받고 싶다.
patient4 = Doctor.objects.create(name='harry', doctor=doctor1, doctor=doctor2)
```

> 1:N 의 한계
>
> - 기존 환자가 다른 의사로 예약을 하려고 하면 새로운 객체를 만들어야 한다.
> - 한 명의 환자가 두 명 이상의 의사에게 예약을 하려고 하면 레코드에 두 개 이상의 데이터가 들어간다(오류)

##### models.py

```python
from django.db import models

class Doctor(models.Model):
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'
    
    
class Patient(models.Model):
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
    
    
class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    
    def __str__(self):
        return f'{doctor.pk}번 의사와 {patient.pk}번 환자'
```

##### shell_plus

```python
doctor1 = Doctor.objects.create(name='justin')
patient1 = Doctor.objects.create(name='tony')
Reservation.objects.create(doctor=doctor1, patient=patient1)
```

```python
doctor1.reservation_set.all()
patient1.reservation_set.all()
```

```python
patient2 = Doctor.objects.create(name='harry', doctor=doctor2)
Reservation.objects.create(doctor=doctor1, patient=patient2)
doctor1.reservation_set.all()
for reservation in doctor1.reservation_set.all():
    print(reservation.patient.name)
```



#### 2) ManyToMany Field

##### models.py

```python
from django.db import models

class Doctor(models.Model):
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'
    
    
class Patient(models.Model):
    name = models.TextField()
    doctors = models.ManyToManyField(Doctor)
    
    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

> DB를 살펴보면 만들어진 테이블의 개수가 3개 입니다. 즉 중개 테이블이 자동으로 만들어지는 것을 확인할 수 있습니다.
>
> - hospitals_doctor
> - hospitals_patient
> - hospitals_patient_doctors (중개 모델)

```python
doctor1 = Doctor.objects.create(name='justin')
doctor2 = Doctor.objects.create(name='eric')
patient1 = Doctor.objects.create(name='tony')
patient2 = Doctor.objects.create(name='harry')
```

```python
patient1.doctors.add(doctor1)

patient1.doctors.all()
doctor1.patient_set.all()

doctor1.patient_set.add(patient2)
patient2.doctors.all()
```

```python
doctor1.patient_set.remove(patient1)
patient2.doctors.remove(doctor1)
```

##### 역참조 시 이름 바꾸기 (related_name)

```python
class Patient(models.Model):
    name = models.TextField()
    doctors = models.ManyToManyField(Doctor, related_name='patients')
    
    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

```python
patient1.doctors.all()
doctor1.patients.all()
```



#### 3) Related manager

1:N 또는 M:N 관련 컨텍스트에서 사용되는 매니저입니다. 같은 이름의 메서드여도 각 관계에 따라 다르게 작동할 수 있습니다. 또한 1:N에서 Target 모델 객체만 사용이 가능합니다. M:N은 Tartget / Source 모델 둘 다 사용이 가능합니다. `add`, `create`, `remove`, `clear`, `set`

> **Symmetrical**
>
> ```python
> class Person(models.Model):
>     friends = models.ManyToManyField('self')
> ```
>
> - 위처럼 동일한 모델을 가리키는 정의의 경우 Person 클래스에 person_set 매니저를 추가 하지 않음
> - 대신 대칭적(Symmetrical)이라고 간주하며, source 인스턴스가 target 인스턴스를 참조하면 target 인스턴스도 source 인스턴스를 참조하게 됩니다.
> - self 와의 M:N 관계에서 대칭을 원하지 않는 경우 Symmetrical를 False로 설정합니다.

> **Through**
>
> - django는 다대다 관계를 관리하는 중개 테이블을 자동으로 생성함
> - 하지만, 중개 테이블을 직접 지젇ㅇ하려면 through 옵션을 사용하여 중개 테이블을 나타내는 Django 모델을 지정할 수 있음
> - 일반적으로 추가 데이터를 다대다 관계와 연결하려는 경우에 사용합니다.

> **add()**
>
> - 지정된 객체를 관련 객체 집합에 추가
> - 이미 존재하는 관계에 다시 사용하면 관계가 복제되지 않음
>
> **remove()**
>
> - 관련 객체 집합에서 지정된 모델 객체를 제거



## 3. 좋아요&팔로우 기능

### 3.1. 좋아요

##### models.py

```python
# Create your models here.
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
    ...
    
```

좋아요를 누른 유저를 가쟈옵니다.

> makemigratoins 오류
>
> - 1 : N 관계에서 user.article_set 기능(유저가 작성한 모든 게시글 조회)과 M : N 관계에서 user.article_set (좋아요를 누른 모든 게시글 조회) 기능이 겹치게 됩니다. 
> - 따라서 related_name='like_articles' 을 지정하여 역참조하는 직관적인 이름으로 바뀌어 줍니다.

##### views.py

```python
@require_POST
def likes(request, article_pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=pk)

        if request.user in article.like_users.all():
            # 좋아요 취소
            article.like_users.remove(request.user)
        else:
            # 좋아요 누름
            article.like_users.add(request.user)
        return redirect('articles:index')
    return redirect('accounts:login')
```

##### index.html

```django
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
```

##### views.py

```python
@require_POST
def likes(request, article_pk):
    ...
        if article.like_users.filter(pk=request.user.pk).exists():
        # if request.user in article.like_users.all():
            # 좋아요 취소
            article.like_users.remove(request.user)

	...
```

> *queryset is lazy*
>
> .exists() : 쿼리를 날리는 데 최적화된 함수를 고민해 봅니다. 
>
> 실제 쿼리셋을 만드는 작업에는 DB가 관여하지 않습니다. 아직 요청을 보내지 않는다는 것입니다. 쿼리셋을 평가하는 순간 쿼리를 DB로 날립니다. 또한 쿼리셋 캐시를 만들게 됩니다. 반복/출력/bool 등을 진행할 때 쿼리셋을 평가하게 됩니다. 평가 이후에 결과를 쿼리셋의 내장 캐시에 저장됩니다. 다음 번 쿼리셋을 만들 때 캐시에 저장된 데이터를 재사용합니다.



### 3.2. 프로필 보기 기능

##### accounts - urls.py

```python
urlpatterns = [
    ...
	path('<str:username>/', views.profile, name='profile'),
]
```

##### views.py

```python
from django.shortcuts import get_object_or_404
from django.contrib.auth import get_user_model


def profile(request, username):
    person = get_object_or_404(get_user_model(), username=username)
    context = {
        'person': person,
    }
    return render(request, 'accounts/profile.html', context)
```

##### base.html

```django
<a href="{% url 'accounts:profile' request.user.username %}">[내 프로필]</a>
```

##### index.html

```django
<b>작성자 : <a href="{% url 'accounts:profile' article.user.username %}">{{ article.user }}</a></b>
```



### 3.3. 팔로우 기능

##### accounts - models.py

```python
class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')    
```

##### urls.py

```python
path('<int:user_pk>/follow/', views.follow, name='follow'),
```

##### views.py

```python
def follow(request, user_pk):
    if request.user.is_authenticated:
        # 팔로우 받는 사람
        person = get_object_or_404(get_user_model(), pk=user_pk)

        if person != request.user:
            if person.followers.filter(pk=request.user.pk):
                # 팔로우 끊음
                person.followers.remove(request.user)
            else:
                # 팔로우 신청
                person.followers.add(request.user)
        return redirect('accounts:profile', person.username)
    return redirect('accounts:login')
```

##### profile.html

```django
  {% with followings=person.followings.all followers=person.followers.all %}
    <div>
      <div>
        팔로잉 : {{ followings|length }} / 팔로워 : {{ followers|length }}
      </div>
      {% if request.user != person %}
        <div>
          <form action="{% url 'accounts:follow' person.pk %}" method="POST">
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
```











