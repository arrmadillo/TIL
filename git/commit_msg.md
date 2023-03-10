## `git 커밋 메시지를 잘 써야 하는 이유`

<br/>

- **가독성**
- **더 나은 협업과 리뷰 프로세스**
- **더 쉬운 코드 유지보수**
  <br/>

## 커밋 메세지 구조

```
type: subject -> 제목
(줄바꿈)
body          -> 본문
(줄바꿈)
footer        -> 꼬리말
```

<br/>

## 커밋 Type

- **Feat** - 새로운 기능을 추가할 경우
- **Fix** - 코드 수정, 버그를 고친 경우
- **Design** - CSS 등 사용자 UI 디자인 변경
- **!BREAKING CHANGE** - 커다란 API 변경의 경우
- **!HOTFIX** - 급하게 치명적인 버그를 고쳐야하는 경우
- **Style** - 코드 포맷 변경, 세미 콜론 누락, 코드 수정이 없는 경우
- **Refactor** - 프로덕션 코드 리팩토링
- **Comment** - 필요한 주석 추가 및 변경
- **Doc** - 문서를 수정한 경우
- **Test** - 테스트 추가, 테스트 리팩토링(프로덕션 코드 변경 X)
- **Chore** - 빌드 태스트 업데이트, 패키지 매니저를 설정하는 경우(프로덕션 코드 변경 X)
- **Rename** - 파일 혹은 폴더명을 수정하거나 옮기는 작업만인 경우
- **Remove** - 파일을 삭제하는 작업만 수행한 경우

  <br/>

## Subject

- 제목은 50자 이하, 첫글자는 대문자로 작성, 마침표를 붙이지 않음
- 과거시제 x, 명령어로 작성
  - Fixed --> Fix
  - Added --> Add

<br/>

## Body

- 선택 사항이다.
- 제목에 관한 더 자세한 설명을 기술
- 무엇을 변경하였고 또는 왜 변경했는지?에 관한 내용 기술

<br/>

## Footer

- 선택 사항이다.
- 보통 issie tracker id를 작성할 때 사용한다

<br/>

## 예시

```
Feat: ADD 회원가입 클래스 작성
```

<br/>

## Reference

https://doublesprogramming.tistory.com/256
