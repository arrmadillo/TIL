# 인증시스템2(User 객체, CUD)

# 회원가입 / 회원 정보 수정

### UserCreationForm

```jsx
django.contrib.auth.forms 앱에서 제공하는 회원가입을 위한 from이다.

AbstractUser를 상속받은 유저 클래스를 사용해야지 form을 이용할 수 있다.

커스텀 유저 모델을 사용하려면 UserCreationForm을 상속한 클래스를 만들어야함.
- class Meta: model=User가 등록된 form이기 때문이다.

공식문서
[https://github.com/django/django/blob/main/django/contrib/auth/forms.py#L106](https://github.com/django/django/blob/main/django/contrib/auth/forms.py#L106)
```

### form 파일

```python
# accounts/forms.py

  from django.contrib.auth.forms import UserCreationForm, UserChangeForm
  # django User 객체에 대한 직접 참조를 권장하지 않음
  # from .models import User

  # 대신 다음과 같은 함수를 제공
  # get_user_model은 현재 프로젝트에 활성화 되어있는 User객체를 반환해줌
  from django.contrib.auth import get_user_model

#회원가입
  class CustomUserCreationForm(UserCreationForm):
      class Meta(UserCreationForm.Meta):
          # 현재 우리가 사용하는 User class로 재정의
          model = get_user_model()

#회원정보 수정
 class CustomUserChangeForm(UserChangeForm):
    class Meta(UserChangeForm.Meta):
        model = get_user_model()
        fields = ('email','first_name','last_name',)

#일반 사용자가 접근해서는 안 될 정보들(fields)까지 모두 수정이 가능해짐
#admin 인터페이스에서 사용되는 ModelForm이기 때문
#따라서 CustomUserChangeForm에서 접근 가능한 필드를 조정
```

### User 모델을 직접 참조하지 않는 이유

- User 모델을 `get_user_model()`를 사용해 참조하면 커스텀 User모델을 반환해준다.
    - AUTH_USER_MODEL = 'accounts.User’ - settings.py에 작성한 것을 참조
- Django는 User클래스를 직접 참조하는 대신 get_user_model()을 사용해 참조해야 한다고 강조

### view 파일

```python
# accounts/views.py

  from .forms import CustomUserCreationForm

  def signup(request):
    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('articles:index')
    else:
        form = CustomUserCreationForm()
    context = {
        'form': form
    }
    return render(request, 'accounts/signup.html', context)

  def update(request):
    if request.method == 'POST':
        form = CustomUserChangeForm(request.POST, instance=request.user)
        if form.is_valid():
            form.save()
            return redirect('articles:index')
    else:
        form = CustomUserChangeForm(instance=request.user)
    context = {
        'form': form
    }
    return render(request, 'accounts/update.html', context)
```

---

### 회원탈퇴

```python
# accounts/views.py

  def delete(request):
    request.user.delete()
    return redirect('articles:index')

# request에 있는 유저객체를 접근해 delete()메소드를 이용해 삭제
```

---

### 비밀번호 변경

### PasswordChangeForm()

- 비밀번호 변경을 위한 폼으로 장고에서 제공
- [https://github.com/django/django/blob/stable/3.2.x/django/contrib/auth/forms.py#L360](https://github.com/django/django/blob/stable/3.2.x/django/contrib/auth/forms.py#L360)

```python
 # accounts/views.py

  from django.contrib.auth.forms import PasswordChangeForm

  def change_password(request):
    if request.method == 'POST':
        form = PasswordChangeForm(request.user, request.POST)
        if form.is_valid():
            form.save()
            return redirect('articles:index')
    else:
        form = PasswordChangeForm(request.user)
    context = {
        'form': form
    }
    return render(request, 'accounts/change_password.html', context)
```

## 암호 변경 시 로그인 유지

- 비밀번호가 변경되면 기존 세션과의 회원 인증 정보가 일치하지 않게 되어버려 로그인 상태가 유지되지 못함
- 비밀번호는 잘 변경되었으나 비밀번호가 변경되면서 기존 세션과의 회원 인증 정보가 일치하지 않기 때문
- 따라서 기존 세션은 필요없어져 세션을 삭제시킴
- 유지하기 위한 함수를 사용 가능
- update_session_auth_hash(request, user)
- [https://docs.djangoproject.com/en/3.2/topics/auth/default/#django.contrib.auth.update_session_auth_hash](https://docs.djangoproject.com/en/3.2/topics/auth/default/#django.contrib.auth.update_session_auth_hash)

```python
# accounts/views.py

  from django.contrib.auth import update_session_auth_hash

  def change_password(request):
    if request.method == 'POST':
        form = PasswordChangeForm(request.user, request.POST)
        if form.is_valid():
            user = form.save()
            # 변경한 비밀번호를 토대로 세션 업데이트
            update_session_auth_hash(request, user)
            return redirect('articles:index')
    else:
        form = PasswordChangeForm(request.user)
    context = {
        'form': form
    }
    return render(request, 'accounts/change_password.html', context)
```

---

## is_authenticated (User 속성)

- 사용자가 인증되었는지 여부를 알 수 있는 User model의 속성(attributes)
- 모든 User 인스턴스에 대해 항상 `True`인 읽기 전용 속성이며, AnonymousUser에 대해서는 항상 `False`임
- 권한(permission)과는 관련이 없으며, 사용자가 활성화 상태(active)이거나 유효한 세션(valid session)을 가지고 있는지도 확인하지 않음

```python
# accounts/views.py

  def login(request):
    if request.user.is_authenticated:
        return redirect('articles:index')

  def signup(request):
    if request.user.is_authenticated:
        return redirect('articles:index')
```

---

## login_required

- 인증된 사용자에 대해서만 view함수를 실행시키는 데코레이터
- 로그인하지 않은 사용자의 경우 `/accounts/login/`주소로 redirect시킴
- 인증된 사용자만 게시글을 작성/수정/삭제 할 수 있도록 수정

```python
from django.contrib.auth.decorators import login_required

  @login_required
  def create(request):
    pass

  @login_required
  def delete(request, article_pk):
    pass

  @login_required
  def update(request, article_pk):
    pass
```

---

## 회원가입시 로그인 진행

```python
# accounts/views.py

  def signup(request):
    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            user = form.save() # 변수에 저장
            auth_login(request, user) # 로그인
            return redirect('accounts:index')
    else:
        form = CustomUserCreationForm()
    return render(request, 'accounts/signup.html', {'form':form})
```

## 탈퇴하면서 유저의 세션 정보 삭제

```python
# accounts/views.py

  def delete(request):
    request.user.delete()
    auth_logout(request)
```

• 먼저 로그아웃 해버리면 해당 요청 객체 정보가 없어지기 때문에 탈퇴에 필요한 유저 정보 또한 없어지므로 순서 중요