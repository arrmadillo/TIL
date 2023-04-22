# Many to Many(다대다)

# N:M 관계(다대다 관계)

다대다 관계란 두 테이블이 서로를 1:N 관계로 보는 것을 말한다

ex) 

- **학생과 과목** - 한 학생은 여러 과목을 듣고, 한 과목은 여러 학생이 듣는다.
- **영화관과 영화** - 한 영화관에 여러 영화가 상영되고, 한 영화는 여러 영화관에 상영된다.
- **팔로우 기능** - 한 유저는 여러 유저를 팔로우하고,  팔로우 유저 또한 여러 유저를 팔로우 한다.
    
    
    **일반적으로 다대다 관계는 두 테이블의 키를 컬럼으로 갖는 중간 테이블을 생성하여 관리함.**
    
    # 팔로우 기능 구현해보기
    
    ### 보통 유저의 프로필에 들어가 팔로우를 하기 때문에 우선 유저 프로필을 구현
    
    ```python
    # accounts/urls.py
    
    urlpatterns = [
      ...
      path('profile/<username>/', views.profile, name='profile'),
    ]
    ```
    
    ```python
    # accounts/views.py
    
    from django.contrib.auth import get_user_model
    
    def profile(request, username):
        User = get_user_model()
        person = User.objects.get(username=username)
        context = {
            'person': person,
        }
        return render(request, 'accounts/profile.html', context)
    ```
    
    ```html
    <!-- accounts/profile.html -->
    
    <h1>{{ person.username }}님의 프로필</h1>
    
    <hr>
    
    <h2>{{ person.username }}' 게시글</h2>
    {% for article in person.article_set.all %}
      <div>{{ article.title }}</div>
    {% endfor %}
    
    <hr>
    
    <h2>{{ person.username }}' 댓글</h2>
    {% for comment in person.comment_set.all %}
      <div>{{ comment.content }}</div>
    {% endfor %}
    
    <hr>
    
    <h2>{{ person.username }}' 좋아요한 게시글</h2>
    {% for article in person.like_articles.all %}
      <div>{{ article.title }}</div>
    {% endfor %}
    
    <h2>{{ person.username }}' 좋아요한 게시글</h2>
    {% for article in person.like_comments.all %}
      <div>{{ article.title }}</div>
    {% endfor %}
    ```
    
    ```html
    <!-- articles/index.html -->
    <!-- 인덱스 페이지에서 프로필로 넘어가는 a 태그 작성 -->
    <p>
      작성자 : <a href="{% url 'accounts:profile' article.user.username %}">
    	{{ article.user }}</a>
    </p>
    ```
    
    ### Follow 구현
    
    ```python
    # accounts/models.py
    
    class User(AbstractUser):
      followings = models.ManyToManyField('self',
                                          symmetrical=False,
                                          related_name='followers')
    ```
    
    ### self
    
    : 같은 모델을 참조(유저 - 유저)
    
    ### symmetrical
    
    : ManyToManyField의 속성 중 하나로 True로 설정 된 경우 양쪽  방향에서의 관계가 대칭적이라는 것을 의미
    
    ex) A의 친구는 B - B의 친구는 A
    
    ### 그러나 팔로우 기능에서는 A가 B를 팔로우해도 B는 A를 팔로우 하지 않을 수 있음
    
    ```python
    # accounts/urls.py
    
    urlpatterns = [
      ...
      path('<int:user_pk>/follow/', views.follow, name='follow'),
    ]
    ```
    
    ```python
    # accounts/views.py
    
    @login_required
    def follow(request, user_pk):
    		# django에서 모델 파일이 아닌 곳에서 
    		# 유저 객체를 가져오려고 할때 get_user_model()를 권장
        User = get_user_model()
        person = User.objects.get(pk=user_pk)
        
    		# 자기 자신을 팔로우 x
        if person != request.user:
            if person.followers.filter(pk=request.user.pk).exists():
                person.followers.remove(request.user)
            else:
                person.followers.add(request.user)
        return redirect('accounts:profile', person.username)
    ```
    
    ```html
    <!-- accounts/profile.html -->
    
    <div>
      <div>
        팔로잉 : {{ person.followings.all|length }} / 팔로워 : {{ person.followers.all|length }}
      </div>
    
      {% if request.user != person %}
        <div>
          <form action="{% url 'accounts:follow' person.pk %}" method="POST">
            {% csrf_token %}
            {% if request.user in person.followers.all %}
              <input type="submit" value="Unfollow">
            {% else %}
              <input type="submit" value="Follow">
            {% endif %}
          </form>
        </div>
      {% endif %}
    </div>
    ```