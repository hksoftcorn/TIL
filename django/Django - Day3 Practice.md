[TOC]

# Django - Day3 Practice

*가상환경 Virtual Environment*

```linux
python -m venv venv

source venv/Scripts/activate

pip list

pip install django

pip freeze > requirements.txt
```

### 1. CREATE

#### 1.1. Django Forms

> 장고 Forms의 3가지 역할
>
> - 렌더링을 위한 데이터 준비
> - 데이터에 대한 HTML Forms 생성
> - 클라이언트로부터 받은 데이터 수신 및 처리

**forms.py**

```python
from django import forms


class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField()
```

**views.py**

```python
from .forms import ArticleForm

def new(request):
    form = ArticleForm()
    context = {
        'form': form
    }
    return render(request, 'articles/new.html', context)
```

**new.html**

```django
{% extends 'base.html' %}

{% block content %}
  <h1>NEW</h1>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
     {{ form }}
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock  %}

```

#### 1.2. Django widgets

장고 위젯을 이용해 봅시다. input 타입을 바꿔줄 수 있습니다.

```django
from django import forms


class ArticleForm(forms.Form):
    REGION_A = 'SEOUL'
    REGION_B = 'DAEJEON'
    REGION_C = 'GUMI'
    REGION_D = 'GWANGJU'
    REGION_CHOICES = [
        (REGION_A, '서울'),
        (REGION_B, '대전'),
        (REGION_C, '구미'),
        (REGION_D, '광주'),
    ]

    title = forms.CharField(max_length=10)
    content = forms.CharField(widget=forms.Textarea)
    region = forms.ChoiceField(choices=REGION_CHOICES, widget=forms.RadioSelect)
```

**new.html**

```django
# p태그 형태로 생성

{{ form.as_p }} 
```

#### 1.3. Django Model form

**forms.py**

```django
class ArticleForm(forms.ModelForm):

    class Meta:
        model = Article
        fields = '__all__'
        # exclude = ('title', )
```

**views.py**

```python
def create(request):
    form = ArticleForm(request.POST)
    # 데이터가 유효한지 검증
    if form.is_valid():
        article = form.save()
        return redirect('articles:detail', article.pk)
    return redirect('articles:new')
```

#### 1.4. C : New + Create

하나의 view 함수가 request의 method에 따라서 2가지 역할을 수행합니다. 

**views.py**

```python
def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        # 데이터가 유효한지 검증
        if form.is_valid():
            # form.cleaned_data.get('title')
            # form.cleaned_data.get('content')
            article = form.save()
            return redirect('articles:detail', article.pk)
    else:
        # new
        form = ArticleForm()
        
    context = {
        'form': form
    }
    return render(request, 'articles/new.html', context)
```

**html 파일수정**

기존 new.html -> create.html

new.html 혹은 articles:new로 되어있는 부분 create로 수정

#### 1.5. Form을 custom하는 방법

**forms.py**

```python
class ArticleForm(forms.ModelForm):

    class Meta:
        model = Article
        fields = '__all__'
        # exclude = ('title', )
        widgets = {
            'title': forms.TextInput(attrs=
            {
                
            })
        }
```

```python
class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label='제목',
        widget=forms.TextInput(
            attrs={
                'class': 'title aa bb',
                'placeholder': 'Enter the Title',
                'maxlength': 10,
            }
        )
    )
    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(
            attrs={
                'class': 'content',
                'placeholder': 'Enter the Content',
                'rows': 5,
                'cols': 30,
            }
        ),
        error_messages={
            'required': '데이터를 입력해줘'
        }
    )
    

    class Meta:
        model = Article
        fields = '__all__'
        # exclude = ('title', )
```



### 2.1. UPDATE

수정이 되는 객체 지정 = `instance` 키워드 인자를 이용합니다.

**views.py**

```python
def update(request, pk):
    if request.method == 'POST':
        pass
    else:
        article = Article.objects.get(pk=pk)
        form = ArticleForm(instance=article)
    context = {
        'form': form,
        'article': article,
    }
    return render(request, 'articles/update.html', context)
```

**update.html**

```python
{% extends 'base.html' %}

{% block content %}
  <h1>UPDATE</h1>
  <form action="{% url 'articles:update' article.pk %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock  %}

```



