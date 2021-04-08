# CRUD

#### 1. STEPs

1. env and settings

- [ ] django-admin startproject crud .
- [ ] python -m venv venv
- [ ] source venv/Scripts/activate
- [ ] pip install django
- [ ] pip install django-bootstrap-v5
- [ ] pip freeze > requirements.txt
- [ ] python manage.py startapp articles
- [ ] project - settings.py
- [ ] project - templates - base.html
- [ ] project - static - stylesheets - style.css



2. structure

- [ ] urls.py
- [ ] views.py
- [ ] templates - articles - index.html
- [ ] image
- [ ] models.py
- [ ] migrate
- [ ] admin.py
- [ ] python manage.py runserver



3. crud

- [ ] forms.py
- [ ] `create`
- [ ] urls.py
- [ ] views.py
- [ ] templates - articles - create.html
- [ ] `read`
- [ ] urls.py
- [ ] views.py
- [ ] templates - articles - detail.html
- [ ] `update`
- [ ] urls.py
- [ ] views.py
- [ ] templates - articles - update.html
- [ ] `delete`
- [ ] urls.py
- [ ] views.py
- [ ] forms.py - style edit
- [ ] views.py - decorator, get_object_or_404



4. user crud

- [ ] python manage.py startapp accounts
- [ ] setting.py
- [ ] urls.py
- [ ] views.py
- [ ] templates
- [ ] `create`  : UserCreationForm
- [ ] `login / logout` : AuthenticationForm
- [ ] auth_login, auth_logout
- [ ] 'next'
- [ ] base.html 수정
- [ ] `update` : UserChangeForm
- [ ] forms.py
- [ ] CustomUserChangeForm
- [ ] `delete`
- [ ] views.py - decorators
- [ ] `password` : PasswordChangeForm
- [ ] update_session_auth_hash



5. comments

- [ ] 















