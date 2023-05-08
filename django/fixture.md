# Fixture

# Fixture 개념

- Django 프로젝트에서 데이터를 구성하는데 사용되는 데이터 세트
- Django가 직접 만들기 때문에 데이터베이스 구조에 맞춰 작성되어있음
- 테스트 데이터를 로드하거나 초기 데이터를 설정하는 데 유용합니다. 데이터 세트는 JSON 데이터 형식으로 작성

# Fixture를 사용하는 이유

- 초기 데이터의 필요성
- Fixture를 사용하면 실제 데이터를 데이터베이스에 로드하고 복잡한 데이터를 만드는 데 드는 시간을 절약

---

# Fixture 사용예시

### 1. Fixture 파일을 만들기 위해 사전에 데이터 만들어 놓기

### 2. dumpdata

- 데이터를 추출한 **json 파일 생성**

```bash
python manage.py dumpdata [app_name[.ModelName] [app_name[.ModelName]] ...] filename.json

python manage.py dumpdata articles.article > articles.json
# articles라는 앱의 article 테이블의 데이터를 추출한 articles.json 파일을 생성한다.

python manage.py dumpdata --indent 4 articles.article > articles.json
# --indent 4 는 옵션값으로 json 내용을 보기 좋게 저장해준다.
```

---

### 3. loaddata

- json 파일을 데이터베이스에 적용

```bash
python manage.py loaddata articles.json
# json 파일명을 적으면 된다.
```

---

### 4. fixtures 기본 경로

- Django에서 fixture 파일의 기본 경로는 앱 폴더 안의 fixtures 폴더
    - `app_name/fixtures/`
    - Django는 설치된 모든 app의 디렉토리에서 fixtures폴더 이후의 경로로 fixtures파일을 찾아 load함
- fixtures 불러오기
    - `$ python manage.py loaddata users.json articles.json comments.json`
    - 나열 순서 중요!

---

### 5. 주의사항

- **모든 모델**을 한번에 dump (권장하지 않음)
    - 3개의 모델
        - `$ python manage.py dumpdata --indent 4 articles.article articles.comment accounts.user > data.json`
    - 모든 모델
        - `$ python manage.py dumpdata --indent 4 > data.json`
- encoding code 에러
    - `$ python -Xutf8 manage.py dumpdata ...`
    - 메모장 활용
        - 메모장으로 json파일 열기
        - 다른 이름으로 저장
        - 인코딩을 UTF8로 선택 후 저장
    
    ---
    

# 권장하는 방법 정리

- **dumpdata**
    - `$ python -Xutf8 manage.py dumpdata --indent 4 articles.user > users.json`
    - `$ python -Xutf8 manage.py dumpdata --indent 4 articles.article > articles.json`
    - `$ python -Xutf8 manage.py dumpdata --indent 4 articles.comment > comments.json`
    - 파일 1개 = 하나의 모델
    - json 파일 만들고 앱 폴더안에 fixtures 폴더 만들어서 옮기기
- **loaddata**
    - `$ python manage.py loaddata users.json articles.json comments.json`
    - user -> article -> comment 순 **loaddata (의존하는 외래키 때문에)**