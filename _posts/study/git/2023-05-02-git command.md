---
title: Git 명령어
author: cotes
date: 2023-05-01 11:33:00 +0800
categories: [Study, Git]
tags: [favicon]
---

> ## 자주쓰는 git 명령어

### git add <'filename'>

어떤 파일을 git에 추가 할 것인가를 지정해준다.

파일 이름을 직접적으로 사용해도 되고, 모든 파일을 추가하고 싶다면 `git add .`을 입력하면 된다.

> git에 특정 파일이 올라가는 것을 막고싶다면?<br/><br/>
> .gitignore 폴더 생성후 무시하고 싶은 파일을 추가하면 해당 파일들은 제거해서 git에 추가해준다.

### git status

파일의 상태를 확인하려면 보통 git status 명령을 사용한다.

git add 명령어로 추가한 파일 맟 수정한 파일을 확인할 수 있다.

<br/>

**git add와 git state 실행 순서 예시**

1. git add 'filename' = untracked files -> straged files

2. git status = filename (git add에서 추가한 파일이 담겨있음.)

3. git rm --cached 'filename' = straged files -> untracked files

4. git status = nothing

5. git add . = 모든 파일 추가

6. git status = 폴더에 있는 모든 파일 추가

> Untracked 와 Tracked <br/><br/>
> Tracked 파일은 이미 스냅샷에 포함돼 있던 파일이고, Tracked 파일은 또 Unmodified(수정하지 않음)와 Modified(수정함) 그리고 Staged(커밋하면 저장소에 기록되는) 상태 중 하나이다.

<br/>

---

<br/>

### git commit

commit = git에게 기억 해 달라하는 단위
git commit -m "commit message"

git에서 커밋은 변경사항을 내 컴퓨터에 저장한다는 의미이다. 위 명령어를 실행하면 이제 작업흐름상에 변경된 파일이 HEAD에 반영될 것이다.
하지만, 원격 저장소에는 아직 반영이 되지는 않은 상태이다.

- 수정 된걸 되돌리고 싶을 때 = git checkout -- <'파일 이름'>
  - 수정 사항이 없어짐

Tip) git add와 commit 한번에 쓰는법
git commit -am "commit message"

### git remote

remote = 외부의 깃헙 저장소가 있는 url에 대한 name을 만들어 관리하기 위한 명령어

- github에서 repository 생성하면 url도 같이 생성
  git remote add <name> <url> = 원격 서버 주소(url)을 name으로 추가해두겠다.
  git remote get-url origin = name을 origin이라 설정한 것의 url을 가져오겠다.

<br/>

---

<br/>

> ## git push, git pull, git reset

1. push: 서버로 보냄
2. pull: 서버로 부터 받아옴
3. reset: commit 되돌리기
   HEAD: 현재 commit 위치
   git reset HEAD ~<되돌리고 싶은 만큼 ex)1>
   기존 commit -> working directory -> straged files -> 새로운 commit
   hard mixed soft

ex) git reset HEAD~1 --soft
=> git add한거까지 되돌려줌
_부연설명 카톡 https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0_

<br/>

---

<br/>

> ## git revert, git branch

1. revert: 되돌리는 것까지 commit으로 생성

2. brach

- brach란?

* Software개발시 개발자들은 동일한 소스코드 위에서 신규 개발, 버그 수정 등의 업무를 협업
  ex) master는 실제 고객이 보는 프로그램
  branchA = 기능 a개발 branch--------- + => master
  branchB = 기능 b개발 branch---------

- branch 사용
  branch생성: git branch <브렌치 이름>
  branch변경: git checkout <브렌치 이름>

> ## git merge, git rebase

- 한 브랜치에서 다른 브랜치로 합치는 방법
  방법 1) merge: 다른 브랜치를 현재 Checkout된 브랜치에 Merge 하는 명령
  git merge branchB
  ?다른 branch에서 같은 줄을 고치고 합칠 경우?
  ex)
  branchA(master) branchB
  const a = 10; const a = 20;

변경 후 각자 commit -> master에서 branch를 merge -> conflict
-> master or branchB or 둘다 합침 -> git add . -> git merge --continue

방법 2) rebase: 다른 브랜치를 현재 Checkout된 브랜치에 rebase 하는 명령
git rebase branchB

- 어떤 특정 브랜치를 base로 커밋 이력을 재정렬하겠다는 명령어
  - base: 브런치가 생성될 때 기준점을 의미한다.
- 재정렬되는 commit 이력이기 때문에, 재정렬되는 commit 이력에는 이전과는 다른 새로운 해쉬 ID가 부여

- Merge와 Reabse 차이
  <그림>
  <블로그>https://dongminyoon.tistory.com/9

> ## git cherry-pick, git stash, git fetch

- cherry-pick: 특정 commit을 가져올 때 사용
  branchA: 중요한 commit
  branckB: 별로 안 중요한 commit

master에서 branchA의 중요한 commit만을 가져오고 싶을 때, branch master에서 git cherry-pick <원하는 커밋 id>
매번 커밋 id찾기 번거롭다 -> tag사용 (git tag <tag 이름>)
중요한 기점이 되는 commit에 tag를 달아 접근하기 편하게 함.

- stash: 임시 저장 같은 느낌
  상황1) branchA에서 작업하는 중에 branchB로 갔다오고 싶음
  git checkout branckB => branchA의 변경 사항을 commit하던지, git stash하라고 나옴
  git stash하고 branchB에 감 => 다시 branchA로 돌아옴 => git stash apply => 기존 작업 하던 것 보여줌.

상황2) branch 착각하고 잘 못 썼을때
branchA의 stash 한 것을 branchB에서 불러올 수 있음.(B로가서 git stash apply)

- fetch: 마지막 pull 이후 원격 저장소 또는 브랜치에 적용된 변경 사항을 확인할 수 있음.

* git pull을 하면 로컬까지 바뀌어 버리니까 fetch를 통해 원격이랑 로컬이랑 비교 할 수 있음.
  push: fetch + merge
