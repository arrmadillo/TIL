# Static / Media Files

# Static Files

- 서버 측에서 변경되지 않고 고정적으로 제공되는 파일
- 이미지, JS, CSS파일 등

## STATIC_URL 설정

```python
# settings.py

  STATIC_URL = '/static/'
  # 경로 : 앱이름/static/
```

- 기본 경로 및 추가 경로에 위치한 정적 파일을 참조하기 위한 URL
- 실제 파일이나 디렉토리가 아니며, URL로만 존재
- 비어 있지 않은 값으로 설정한다면 반드시 /로 끝나야 함
- URL + STATIC_URL + 정적파일 경로
- ex) [http://127.0.0.1:8000/static/articles/sample-1.png](http://127.0.0.1:8000/static/articles/sample-1.png)
- 기본 경로 static file 제공하기
    - articles/static/articles/ 경로에 이미지 파일 배치
    - static tag를 사용해 이미지 파일에 대한 url 제공
    
    ```python
    <!-- articles/index.html -->
    
      {% load static %}
    
      <img src="{% 'articles/sample-1.png' %}" alt="img">
    ```
    

### 추가 경로

```python
 # settings.py

  # 추가 경로
  STATICFILES_DIRS = [
    BASE_DIR / 'static',
  ]
  # 경로 : static/
```

```html
<!-- articles/index.html -->

  <img src="{% static 'sample-2.png' %}" alt="img">
```

### css파일 가져오기

```html
 <!-- articles/index.html -->

  <link rel='stylesheet' href="{% static 'style.css' %}">
```

---

# **Media Files**

### 사용자가 웹에서 업로드하는 정적 파일

- ImageField()
    - 이미지 업로드에 사용하는 모델 필드
    - 이미지 객체가 직접 저장되는 것이 아닌 이미지 파일의 경로 문자열이 DB에 저장

```python
# settings.py

  # 미디어 파일들이 위치하는 디렉토리의 절대 경로
  MEDIA_ROOT = BASE_DIR / 'media'

  # MEDIA_ROOT에서 제공되는 미디어 파일에 대한 주소를 생성
  MEDIA_URL = '/media/'
```

```python
# url.py

from django.conf import settings
from django.conf.urls.static import static

  urlpatterns = [
    ...,
  ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

## **이미지 업로드 및 제공하기**

```python
# articles/models.py

class Article(models.Model):
  image = models.ImageField(blank=True)

#blank=True 속성을 작성해 빈 문자열이 저장될 수 있도록 설정
```

## pillow 설치 밎 마이그레이션

```
$ pip install pillow

$ python manage.py makemigrations
$ python manage.py migrate

$ pip freeze > requirements.txt
```

## 

```html
<!-- aricles/create.html -->

<h1>CREATE</h1>
<form action="{% 'articles:create' %}" method="POST" enctype="multipart/form-data">
```

**form 요소의 enctype 속성 추가(파일이기 때문에** multipart/form-data 사용**)**

```python
# articles/view.py

  def create(request):
    if request.method = "POST":
      form = ArticlesForm(request.POST, request.FILES)
```

```html
<!-- articles/detail.html -->{% load static %}

{% if article.image %}
  <img src="{{ article.image.url }}" alt="img">
{% else %}
  <img src="{% static 'no-image.png' %}" alt="no-img">
{% endif %}
```

• 이미지 데이터가 없을 경우 'no-image.png' 출력

## 참고

upload_to 사용해 경로 설정

```python
# 1 media/images/파일 저장
  image = models.ImageField(blank=True, upload_to='images/')

  # 2 media/2023/04/10/파일 저장
  image = models.ImageField(blank=True, upload_to='%Y/%m/%d/')

  # 3 media/images/username/파일 저장
  def articles_image_path(instance, filename):
    return f'images/{instance.user.username}/{filename}'

  image = models.ImageField(blank=True, upload_to=articles_image_path)
```

## django-imagekit

패키지를 활용하여 사용자가 업로드한 이미지 리사이즈

• [django-imagekit](https://django-imagekit.readthedocs.io/en/latest/)

```python
$ pip install pillow
$ pip install django-imagekit
```

```python
settings.py

INSTALLED_APPS = [
  ...,
  'imagekit',
]
```

## **django-cleanup**

이미지파일 자동 삭제

• [https://github.com/un1t/django-cleanup](https://github.com/un1t/django-cleanup)

```python
$ pip install django-cleanup
```

```python
settings.py

INSTALLED_APPS = [
  ...,
  'django_cleanup.apps.CleanupConfig',
]
```