---
추가 일시: Invalid date
강의: Git
---
- [[#git과 github의 차이]]
- [[#repository와 commit]]
- [[#commit 해보기]]
- [[#Git의 3가지 작업영역]]
- [[#git에서 파일들의 상태]]
- [[#git reset]]
- [[#git push]]
- [[#git pull]]

  

# git과 github의 차이

git → 버전 관리를 할 때 사용하는 소프트웨어 자체

github → git으로 관리하는 프로젝트의 복사본을 저장하는 서버를 제공

# repository와 commit

repository(저장소)

→ 프로젝트 디렉토리의 변화하는 모습, 버전별 변경사항에 대한 설명이 있는 것

**→ 커밋이 저장되는 곳**

→ .git 디렉토리

  

commit

→ 프로젝트 디렉토리의 모습을 하나의 버전으로 남기는 행위&결과물

  

```JavaScript
git init -> 비어있는 레포지토리를 생
```

  

# commit 해보기

→ 처음으로 커밋을 하기 전 사용자의 이름과 이메일 주소를 설정

→ 커밋 메시지 남기기 (옵션 -m)

→ 커밋할 파일을 git add로 지정해주기

  

add

→ 커밋할 파일을 미리 지정해 줘야함

→ 수정된 파일의 모습이 커밋에 포함될 것이라 지정하는것

# Git의 3가지 작업영역

working directory : 프로젝트 디렉토리

staging area : git add를 한 파일들이 존재하는 영역

repository : working directory의 변경 이력들이 저장되어 있는 영역 ( 커밋들이 저장되는 영역)

![[/image 4.png|image 4.png]]

  

# git에서 파일들의 상태

  

Untracked : 변동사항이 추적되지 않을 때, 예를들어 파일을 생성하고 한번도 git add를 해주지 않았을 때

tracked

→ Staged : 파일의 내용이 수정되고 나서, staging area에 올라와 있는 상태

→ Unmodified : 현재 파일의 내용이 최신커밋과 바뀐게 없는 상태

→ Modified : 최신 커밋과 비교했을 때 바뀐 내용이 있는 상태

  

# git reset

staging area에서 파일 제거

변경된 새 모습은 그대로 working directory에 남아있음

  

# git push

로컬 레포지토리(내컴퓨터)에서 새로운 커밋을 할때마다

git push로

리모트 레포지토리(깃허브)에 반영해 준다

  

# git pull

리모트 레포지토리에서 추가한 커밋을 로컬 레포지토리에 반