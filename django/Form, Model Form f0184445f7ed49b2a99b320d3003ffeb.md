# Form, Model Form

# FORM

- FORM은 웹 페이지에서 사용자로부터 데이터를 **입력받기 위해** 사용되는 **HTML 요소**
- 웹 페이지에서 사용자가 데이터를 입력하고 **제출**하면, 해당 데이터를 **서버로 전송하여 처리**하는데 사용
- HTML FORM은 다양한 필드와 제출 버튼으로 구성

## 구성요소

- **`<form>`** 요소로 시작
- **`action`** 속성을 통해 데이터를 전송할 URL을 지정
- **`method`**속성을 통해 데이터를 전송하는 HTTP 메소드를 지정할 수 있으며, 일반적으로 **POST**나 **GET** 메소드가 사용

## 코드 예시

```html
<form action="/submit" method="post">
  <label for="username">Username:</label>
  <input type="text" id="username" name="username">
  <br>
  <label for="password">Password:</label>
  <input type="password" id="password" name="password">
  <br>
  <input type="submit" value="Submit">
</form>
```

---

# Django Form

`**유효성 검사**: 수집한 데이터가 정확하고 유효한지 확인하는 과정`

- 유효성 검사에는 입력 값, 형식, 중복, 범위, 보안 등 많은 것들을 고려해야함
    - **Django Form**은 이런 과정을 쉽게 제공해주는 도구!

## 코드 예시

```python
# articles/forms.py
from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField()
```

```python

from .forms import ArticleForm

def new(request):
    form = ArticleForm()
    context = {
        'form' : form,
    }
    return render(request, 'articles/new.html',context)
```

```html
<!-- articles/new.html -->

  <h1>New</h1>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">[back]</a>
<!-- create url, 함수 호출 -->
```

```python
# articles/views.py

def create(request):
    title = request.POST.get('title')
    content = request.POST.get('content')
    article = Article(title=title, content=content)
    article.save()

    form = ArticleForm(request.POST)
    if form.is_valid():
        article = form.save()
        return redirect('articles:detail', article.pk)
    context = {
        'form': form,
    }
    return render(request, 'articles/new.html', context)

# 사용자가 form 입력한 데이터를 받아와 저장
```

## **Widgets**

- HTML input 요소 렌더링
- Form fields에 할당 됨

```python
# articles/forms.py
from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
		#콘텐츠의 인풋을 Textarea 형식으로 받음!
    content = forms.CharField(widget=forms.Textarea)
```

## 추가 예시

```python
# todo/form.py
from django import forms
from .models import Todo

class TodoForm(forms.ModelForm):
    title = forms.CharField(
        widget=forms.TextInput(
						#속성
            attrs={'placeholder': '제목 입력'},
        ),
    )
    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(
            attrs={'class': 'my-content'},
        ),
    )
    priority = forms.IntegerField(
        widget=forms.NumberInput(
            attrs={'min': 1, 'max': 5, 'value': 3},
        )
    )
    deadline = forms.DateField(
        widget=forms.DateInput(
            attrs={'type': 'date'},
        ),
    )
    # 모델 참고
    class Meta:
        model = Todo
        fields = '__all__'
```

---

## Django **ModelForm**

위와 같은 Django Form을 이용해 db에 레코드를 생성하는 건 상당히 불편하다.

- form 파일에 **저장할 모델의 필드를 모두 직접 정의**해야함
- view 함수에서 사용자가 입력한 **필드마다 데이터를 가져와야 함**.

이런 불편한 점을 해결하는 것이 **ModelForm**

Form - > 사용자 입력 데이터를 DB 저장 X (ex 로그인)

ModelForm - >  사용자 입력 데이터를 DB에 저장 O (ex 회원가입)

위와 같은 로직을 ModelForm으로 구성해보자

## 코드 예시

### ModelForm 클래스

```python
# articles/forms.py

  from django import forms
  from .models import Article
	
  class ArticleForm(forms.ModelForm):
      class Meta: 
          model = Article #사용할 모델 클래스
          fields = '__all__' #모든 필드 출력(html 태그)
          # fields = ('title',) 타이틀 필드 출력
          # exclude = ('title',) 타이틀을 **제외한** 나머지 필드 출력
```

### 글 생성 함수

```python
# articles/views.py

  def create(request):
		# 사용자가 입력한 데이터 폼 저장
    form = ArticleForm(request.POST)
		# 유효성 검사(불린값 리턴)
    if form.is_valid():
				# 통과 후 db 레코드 생성
        article = form.save()
        return redirect('articles:detail', article.pk)

		# POST가 아니라면
    else:
        form = ArticleForm()

		# 유효성 검사 틀리면 그에 맞는 오류 출력
    context = {
        'form' : form
    }
    return render(request, 'articles/new.html', context)
```

## 글 수정 함수

```python
# articles/views.py

  def edit(request, article_pk):
    article = Article.objects.get(pk=article_pk)
		# instance 인자에 기존에 글을 넣음
    form = ArticleForm(instance=article)
    context = {
        'article': article,
        'form' : form,
    }

    return render(request, 'articles/edit.html', context)
```