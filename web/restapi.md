# REST API

# 1. REST API란?

 `Representational State Transfer`

```markdown
자원을 이름 (자원의 표현)으로 구분하여 자원의 상태 를 주고 받는 모든 것을 의미

API 시스템을 구현하는 아키텍처 중 하나로 가장 널리 사용되는 네트워크 아키텍처이다.
```

---

# 2. **자원(Resource): URI**

<aside>
💡 REST API에서 자원(Resource)은 URI(Uniform Resource Identifier)를 통해 표현한다.

</aside>

## 예시) 특정 포스트

```markdown
http://example.com/blog/posts/1234
```

### 서버 - http://example.com

### 앱 - blog

### 자원 - posts

### 자원의 고유한 id - 1234

## 특정 자원을 선택하였다

## 그렇다면 자원의 대한 요청은 어떻게 할까?

---

# 3. 요청: HTTP 메서드

<aside>
💡 REST API에서는 **HTTP 메서드**를 사용하여 클라이언트가 자원에 대한 **요청을 전달**

</aside>

- GET: 자원을 조회
- POST: 자원을 생성
- PUT: 자원을 수정
- DELETE: 자원을 삭제

```
**GET**    http://example.com/blog/posts/1234: ID가 1234인 글 **조회**
**POST**   http://example.com/blog/posts:      새 글 **생성**
**PUT**    http://example.com/blog/posts/1234: ID가 1234인 글 **수정**
**DELETE** http://example.com/blog/posts/1234: ID가 1234인 글 **삭제**
```

---

# 4. 응답: HTTP 상태코드(Status Code)

<aside>
💡 요청에 대한 서버의 응답은 **HTTP 상태 코드**(Status Code)와 함께 반환

</aside>

```

**200** OK: 성공적으로 요청을 처리하고 응답 데이터가 있음.
**201** Created: 성공적으로 요청을 처리하고 새로운 자원을 생성함.
**204** No Content: 요청은 성공적으로 처리되었지만 응답 데이터가 없음.
**400** Bad Request: 요청이 잘못되었거나, 필요한 데이터가 누락됨.
**401** Unauthorized: 인증이 필요한 요청이지만, 인증이 실패함.
**404** Not Found: 요청한 자원이 존재하지 않음.
**500** Internal Server Error: 서버 내부 오류가 발생함.
```

---

# 5. **RESTful API 설계 주의사항**

참고: **[RESTful API 설계 가이드](https://sanghaklee.tistory.com/57), [REST 아키텍처를 훌륭하게 적용하기 위한 몇 가지 디자인 팁](https://spoqa.github.io/2012/02/27/rest-introduction.html)**

### URL

- _(underbar) 대신 -(dash)를 사용한다.
- **정보의 자원**을 표현해야 한다.
- 자원은 **복수 명사**를 사용
- **행위**(method)는 URL에 **포함하지 않는다.**
- 소문자를 사용한다
- 컨트롤 자원을 의미하는 URL 예외적으로 동사를 허용한다.

```
1번 사용자의 정보를 받을때

GET /users/show/1 **X** 

GET /user/1 **X**

GET /users/1 **O**

글 생성

****POST /createPost **X**

POST /post **X**

POST /posts **O**
```

### Form은 어떻게 받아오지?

| HTTP Method | Path | action | used for |
| --- | --- | --- | --- |
| GET | /photos | index | 모든 사진 목록 표시 |
| GET | /photos/new | new | 새 사진 생성을 위한 HTML 양식을 반환합니다. |
| POST | /photos | create | 새 사진 만들기 |
| GET | /photos/:id | show | 특정 사진 표시 |
| GET | /photos/:id/edit | edit | 사진 편집을 위한 HTML 양식을 반환합니다. |
| PUT | /photos/:id | update | 특정 사진 업데이트 |
| DELETE | /photos/:id | destroy | 특정 사진 삭제 |