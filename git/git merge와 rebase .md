# Git Pull

- 원격저장소에서 로컬 저장소로 가져올 때 사용하는 명령어로 pull과 fetch가 있음
- 이 둘의 차이는 가져온 소스를 바로 merge하느냐 안하느냐의 차이
- pull은 원격 저장소에서 가져온 소스가 더 최신이라고 판단되면 지금 버전을 해당 소스에 맞춰서 merge
  pull은 별도의 옵션없이 사용하면 원하지 않는 방향으로 Merge될 수 있다고 경고함.  
  <img src="https://github.com/yangseungin/TIL/blob/master/git/%EC%82%AC%EC%A7%84/git%20pull.png?raw=true" width="80%">

# Git Merge

- 브랜치를 병합할 때 사용

```
"Incorporates changes from the named commits (since the time their histories diverged from the current branch) into the current branch.”
```

래퍼런스의 description에서는 현재 브랜치에서 분리된 이후에 명명된 커밋의 변화를 현재 브랜치로 통합한다고 설명하고 있다.

## FAST-FORWARD Merge

```
In this case, a new commit is not needed to store the combined history; instead, the HEAD (along with the index) is updated to point at the named commit, without creating an extra merge commit.
```

래퍼런스에서 발췌한 내용인데 히스토리를 결합하기위한 새로운 커밋이 필요하지 않고 대신에 HEAD는 추가 merge 커밋을 생성하는것 없이 명명된 커밋을 가리키도록 업데이트한다. 이 의미는 현재 브랜치의 커밋이 변경이 일어난 브랜치의 베이스 커밋과 동일한 경우 즉 선형구조로 되어있는 경우

### 선형구조 vs 비선형구조

- 선형구조: 순차적으로 나열된 형태

  <img src="https://github.com/yangseungin/TIL/blob/master/git/%EC%82%AC%EC%A7%84/%EC%84%A0%ED%98%95%EA%B5%AC%EC%A1%B0.png?raw=true" width="50%">

- 비선형구조: 하나에서 여러개의 자료가 존재할 수 있는 형태

  <img src="https://github.com/yangseungin/TIL/blob/master/git/%EC%82%AC%EC%A7%84/%EB%B9%84%EC%84%A0%ED%98%95%EA%B5%AC%EC%A1%B0.png?raw=true" width="50%">

로컬에서 관리되는 버전 관리 시스템

- git init: 현재 디렉토리를 로컬 저장소로 설정할 때
- git status: Git이 인식하고 있는 파일 상태를 확인할 떄
  - git이 관리하는 파일은 Tracked와 Untracked로 나뉜다.
  - Tracked: 파일의 상태가 Git에 의해 추적 중인 상태로 3가지로 분류된다.
