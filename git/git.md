# 간략한 깃 요약!

# GIT

## `정의`

분산 버전 관리 시스템  
 백업, 협업
<br/>

## `git init`

- 버전관리를 하려는 폴더(위치)로 이동한다
- git init 명령어를 터미널에 사용하여 git에게 이 폴더에 있는 파일들을 감시하라고 한다.

## `git status`

- 현재 git 상태를 보여준다
- 수정된 파일이 있는가?
- 아직 추적을 하지 않는 파일 있는가?

## `git add 파일이름`

- 커밋을 하기 전에 명령어로 작업 디렉토리의 파일을 스테이징 영역으로 옮긴다.

## `스테이징 영역(Staging Area)`

- 작업영역과 버전영역을 잇는 징검다리!
- 자유롭게 파일을 수정하다가, 커밋 대기를 하는 영역

## `git commit -m "내용"`

- 커밋을 한다는건 간단히 말해서 버전 생성!
- 커밋 내용을 적을 수 있다. ex) README 작성

## `git log`

- 지난 커밋들을 볼 수 있음
- git log --stat 명령어를 사용하면 어떤 차이가 생겼는지 알 수 있음

## `원격저장소 활용하기`

1. 레포지토리 생성

2. git remote add origin (url)

3. 원격저장소에 push
   git push origin master
   (유저네임과, 토큰) 입력

4. 원격저장소 저장된 코드 가져오기!
   git pull origin master

## `협업(처음 프로젝트를 가져올때)`

- git clone (url)
- `다운로드`(zip)와 `clone`의 차이?
  - 다운로드 :가장 최신 버전의 파일만 가져오는것
  - clone: git 저장소를 가져오는것(이전 버전 히스토리를 볼 수 있음)

## `깃헙 Push 실패 해결`

1. 원격저장소 pull
2. 로컬에서 두 커밋 병합(merge conflict)
3. 다시 깃헙 푸쉬