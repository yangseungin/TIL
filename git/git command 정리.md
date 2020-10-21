git, github을 사용하면서 자주 사용하는 커맨드를 정리하였다.

# Git
로컬에서 관리되는 버전 관리 시스템
- git init: 현재 디렉토리를 로컬 저장소로 설정할 때
- git status: Git이 인식하고 있는 파일 상태를 확인할 떄
  - git이 관리하는 파일은 Tracked와 Untracked로 나뉜다.
  - Tracked: 파일의 상태가 Git에 의해 추적 중인 상태로 3가지로 분류된다.
    1. Unmodified: 변경된 내용이 없는 상태
    2. Modified: 변경된 내용이 있는 상태
    3. Staged: 변경된 내용이 staging area에 올라간 상태
    <img src="https://github.com/yangseungin/TIL/blob/master/git/%EC%82%AC%EC%A7%84/git%20%ED%8C%8C%EC%9D%BC%20%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4.png?raw=true" width="80%">
  - Untracked: 파일을 추적하고 있지 않음
- git add [파일명 or 디렉토리명]: 수정사항이 있는 파일 혹은 디렉토리를 staging area에 올림
- git reset [파일명]: staging area에 올린 파일을 다시 내리기
- git reset [옵션] [커밋 아이디]
  - 옵션에 따라 다르게 수행함
  - --soft: index를 보존(staged상태)
  - --mixed: index 취소(unstaged상태) 옵션을 주지 않으면 기본으로 mixed로 동작
  - --hard: index 취소(unstaged상태), 워킹디렉토리의 파일 삭제
- git commit: 현재 staging area에 있는 것들을 확정
  - 여러 옵션이 존재한다
  - git commit -m "메세지": 커밋 메세지를 "메세지"로 하여 커밋
  - git commit -a: Modified상태의 파일을 자동으로 add 후 커밋
- git diff [커밋1 아이디] [커밋2 아이디]: 두 커밋의 차이점 비교
- git tag [태그명] [커밋 아이디]: 특정 커밋에 태그를 붙임
- git branch [새 브랜치 이름]: 새로운 브랜치 생성
- git checkout [새 브랜치 이름]: 브랜치로 이동
  - -b 옵션을 주면 브랜치를 생성하고 바로 이동한다
  ~~~
  git checkout -b
  ~~~
- git branch -d [브랜치 이름]: 브랜치 삭제
- git merge [브랜치 이름]: 현재 브랜치에 다른 브랜치를 머지
- git help [커맨드 이름]: 해당 커맨드의 자세한 사용 메뉴얼 출력

# Github
클라우드 저장소로 관리되는 버전 관리 시스템
- git push: 로컬 저장소에 commit 된 내용을 원격저장소에 추가로 파라미터가 없으면 origin 저장소에 푸쉬
  - git push [원격저장소 이름] [브랜치명]: 원격저장소의 해당브랜치에 푸쉬
- git pull : 리모트 레포지토리의 내용을 로컬 레포지토리로 가져오기
- git clone [리포트 레포지토리 주소]: Github에 해당 url의 프로젝트를 내 PC로 가져오기

# 이미지 출처
https://git-scm.com/book/ko/v2/