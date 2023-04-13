# Django 인증 시스템(AbstractUser - 로그인, 로그아웃, 세션부여)

# **Django Authentication System**

```python
사용자 인증과 관련된 기능을 모아 놓은 시스템    
인증과 권한 부여를 함께 제공 및 처리

INSTALLED_APPS = [
      'articles',
      'django_extensions',
      'django.contrib.admin',
      **'django.contrib.auth',** 
      'django.contrib.contenttypes',
      'django.contrib.sessions',
      'django.contrib.messages',
      'django.contrib.staticfiles',
  ]
```

### Authentication(인증) **: 사용자가 누구인지 확인하는 것**

### Authorization(권한) : 인증된 사용자가 수행할 수 있는 작업을 결정

---

## 주의사항 💥

**유저 관련 된 앱의 이름을 accounts라는 이름으로 생성하는걸 권장**

**auth와 관련한 경로나 키워드들을 django 내부적으로 accounts라는 이름으로 사용하고 있음**

# AbstractUser

- Django에서 제공하는 클래스 중 하나로, Django에서 기본적으로 제공하는 사용자(User) 모델을 확장하여 사용자 정의 사용자 모델을 만들 때 사용
- django.contrib.auth.models 모듈에 정의되어 있으며, Django의 기본 사용자 모델(AbstractBaseUser)과 PermissionsMixin 클래스를 상속받아 구현
- AbstractUser를 **확장**하여 필요한 속성이나 메서드를 추가 가능

```python
# accounts/models.py

  from django.contrib.auth.models import AbstractUser

  class User(AbstractUser):
    pass
```

### 기본 User모델을 우리가 작성한 User모델로 지정(수정 전 'auth.User')

```python
 # settings.py

  AUTH_USER_MODEL = 'accounts.User' # '앱이름.테이블명'
```

`주의` : 프로젝트 중간에 **AUTH_USER_MODEL**을 변경할 수 없음

### 기본 User모델이 아니기 때문에 등록하지 않으면 admin site에 출력되지 않음

```python
 # accounts/admin.py

  from django.contrib import admin
  from django.contrib.auth.admin import UserAdmin
  from .models import User

  admin.site.register(User, UserAdmin)
```

Django는 새 프로젝트를 시작하는 경우  커스텀 User모델을 설정하는 것은 `강력하게 권장`

커스텀 User모델은 기본 User모델과 동일하게 작동하면서도 필요한 경우 나중에 `맞춤 설정 가능`

User모델 대체 작업은 프로젝트의 모든 migrations혹은 첫 migrate를 `실행하기 전에` 이 작업을 마쳐야 함

### 이미 테이블을 생성했다면?

- 회원과 관련된 앱에 migrations폴더에서 `__pycache__,` `__init__.py`를 제외한 나머지 삭제
- DB 백업후 db 삭제\

### 공식문서: **[Django 인증 커스터마이징하기](https://docs.djangoproject.com/ko/3.2/topics/auth/customizing/)**

---

# Login

```python
# accounts/urls.py

  from django.urls import path
  from . import views

  app_name = 'accounts'
  urlpatterns = [
      path('login/', views.login, name='login'),

  ]
```

```html
 <!-- login.html -->

  <h1>로그인</h1>
  <form action="{% url 'accounts:login' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
```

```python
# accounts/views.py

  from django.shortcuts import render, redirect
  from django.contrib.auth.forms import AuthenticationForm # 로그인을 위한 built-in form
  from django.contrib.auth import login as auth_login

  def login(request):
      if request.method == 'POST':
									# 로그인을 위한 built-in form
          form = AuthenticationForm(request, request.POST)
          if form.is_valid():
							# 사용자에게 쿠키에 세션 전달
              auth_login(request, form.get_user())
              return redirect('articles:index')
      else:
          form = AuthenticationForm()

      return render(request, 'accounts/login.html', {'form' : form})
```

- login(request, user)
    - 인증된 사용자를 로그인 하는 함수
    - Django의 세션 프레임워크를 사용하여 세션에 사용자 ID를 저장한다.
- get_user()
    - AuthenticationForm의 인스턴스 메서드
    - 유효성 검사를 통과했을 경우 로그인 한 사용자 객체를 반환
    
    **[Django 인증 시스템 공식문서](https://docs.djangoproject.com/ko/4.2/topics/auth/default/)** 💥
    

---

# Logout

```python
 from django.urls import path
 from . import views

  app_name = 'accounts'
  urlpatterns = [
      path('logout/', views.logout, name='logout')
  ]
```

```html
 <!-- articles/index.html -->

  <h1>Articles</h1>
  <a href="{% url 'accounts:login' %}">Login</a>
  <form action="" method="POST">
    {% csrf_token %}
    <input type="submit" value='Logout'>
  </form>
```

```python
# accounts/views.py

  from django.shortcuts import render, redirect
  from django.contrib.auth.forms import AuthenticationForm
  from django.contrib.auth import login as auth_login
  from django.contrib.auth import logout as auth_logout

  def logout(request):
    auth_logout(request)
    return redirect('articles:index')
```