---
title: Git 명령어
author: cotes
date: 2023-05-02 11:33:00 +0800
categories: [Study, Git]
tags: [favicon]
---

> ## git working flow - 작업 흐름

로컬 저장소는 git이 관리하는 세 그루의 나무로 구성되어 있다.

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/c5c95150-644c-4180-923d-890ce23ffb24)

- **작업 디렉토리(Working directory): 로컬(실제) 파일**

- **인덱스(Index): 준비 영역(staging area)의 역할**

- **나무인 HEAD: 최종 확정본(commit)**

<br/>

---

<br/>

> ## 자주쓰는 git 명령어

### git init

이 과정은 **처음 한 번만 진행되며 git init후에는 .git폴더가 생성되어 git이 내부적으로 버전 관리를 해줌.**

git 저장소가 생기고 HEAD는 아직 없는 브랜치를 가리킨다.

<img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/7591f490-3fe0-4d41-ad49-056d890a599c" width="70%" height="50%" alt="image"/>

<br/>

---

### git add <'filename'>

**어떤 파일을 git에 추가 할 것인가를 지정**해준다.

파일 이름을 직접적으로 사용해도 되고, 모든 파일을 추가하고 싶다면 `git add .`을 입력하면 된다.

<img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/449da812-19c3-417d-ac94-b1f612c54551" width="70%" height="50%" alt="image"/>

<br/>

> - **.gitignore** <br/>
>   git에 특정 파일이 올라가는 것을 막고싶다면 .gitignore 폴더 생성후 무시하고 싶은 파일을 추가하면 해당 파일들은 제거해서 git에 추가해준다.

<br/>

---

### git commit

`git commit -m "commit message"`

**git에서 커밋은 변경사항을 내 컴퓨터에 저장한다는 의미**이다.

위 명령어를 실행하면 이제 작업흐름상에 **변경된 파일이 HEAD에 반영될 것이다. 하지만, 원격 저장소에는 아직 반영이 되지는 않은 상태**이다.

<img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/fff712e8-806c-41f8-995e-1474d135bf62" width="70%" height="50%" alt="image"/>

<br/>

> **Tip)** git add와 commit 한번에 쓰는법<br/> > `git commit -am "commit message"`

<br/>

---

### git status

**파일의 상태를 확인하려면 git status 명령**을 사용한다.

git add 명령어로 **추가한 파일 및 수정한 파일을 확인**할 수 있다.

<img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/fff712e8-806c-41f8-995e-1474d135bf62" width="70%" height="50%" alt="image"/>

위의 그림에서는 **세 트리가 모두 같기 때문에 `git status`명령을 실행하면 아무런 변경 사항이 없다**고 나온다.

<br/>

> - **Untracked 와 Tracked** <br/><br/>
>   Tracked 파일은 이미 스냅샷에 포함돼 있던 파일이고, Tracked 파일은 또 Unmodified(수정하지 않음)와 Modified(수정함) 그리고 Staged(커밋하면 저장소에 기록되는) 상태 중 하나이다.
>
> * Unstaged 상태의 파일은 커밋되지 않는다.

<br/>

---

### git reset

**commit을 되돌리는 명령어**이다.

`git reset <option> HEAD~<되돌리고 싶은 만큼>`

<br/>

**옵션에 따른 reset 동작**

1. **--soft**

    <img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/ee534c5b-b2f7-47d4-9a5d-23751dec108e" width="70%" height="50%" alt="image"/>

   - HEAD가 가리키는 브랜치를 옮긴다.

   <br/>

2. **--mixed(default option)**

   <img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/25ffc1f0-02d1-4fa5-b526-1136ef7ea9c8" width="70%" height="50%" alt="image"/>

   - HEAD가 가리키는 브랜치를 옮긴다.

   - Index를 HEAD가 가리키는 상태로 만든다.

    <br/>

3. **--hard**

    <img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/b541250e-b777-4ba5-8149-5b41ddcc4453" width="70%" height="50%" alt="image"/>

   - HEAD가 가리키는 브랜치를 옮긴다.

   - Index를 HEAD가 가리키는 상태로 만든다.

   - 워킹 디렉토리를 Index의 상태로 만든다.

<br/>

---

### git remote

**외부의 깃헙 저장소가 있는 url에 대한 name을 만들어 관리하기 위한 명령어**

- github에서 repository 생성하면 url도 같이 생성

- `git remote add <name> <url> `

  - **원격 서버 주소(url)을 name으로 추가**

- `git remote get-url origin`

  - name을 origin이라 설정한 것의 url을 가져오겠다.

- `git remote -v`

  - 연결된 원격 저장소를 확인

<br/>

---

### git push

push는 마지막으로 **커밋한 사항을 git repository 에 올리겠다는 뜻**이다.

**push가 안되면 원격 서버에 변경사항이 저장되지 않는다. 다시말해, 프로젝트를 공유하고 싶을 때 리모트 저장소에 Push할 수 있다.**

이 명령은 `git push [리모트 저장소 이름] [브랜치 이름]`으로 단순하다.

commit 까지만 실행했다면 현재의 변경 내용은 아직 로컬 저장소의 HEAD 안에 머물고 있을 것이다.

이제 이 변경 내용을 원격 서버로 올리기 위해 `git push - u origin master` 명령을 실행한다.

<br/>

---

### git pull

- **원격 저장소의 갱신된 내용을 추가로 내려받는 작업**이다.

- 로컬 저장소보다 최신인 갱신된 원격 저장소의 커밋 정보를 현재 로컬 저장소로 내려받는다.

- **pull 명령어를 주기적으로 사용하면 최신 커밋 정보로 로컬 저장소를 유지할 수 있다.**

`git pull [리모트 저장소 이름] [브랜치 이름]` 명령어로 원격 저장소의 갱신된 내용을 내려받을 수 있다.

<br/>

---

### git merge, git rebase

**한 브랜치에서 다른 브랜치로 합치는 방법은 merge와 rebase가 있다.**

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/ab393150-978e-4612-b49e-ababe5825f67)

위의 commit 이력을 가졌다고 가정하고 merge와 rebase의 동작 원리와 차이점에 대해 설명하고자 한다.

_git branch에 대한 내용은 [Git branch](https://hajung00.github.io/posts/git-branch/)게시글을 통해 확인할 수 있습니다._

<br/>

#### 방법 1) merge

**다른 브랜치를 현재 Checkout된 브랜치에 Merge 하는 명령**

`git merge 노란색 branch`

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/30081aea-9aee-4769-bf68-588c293023f2)

<br/>

ex) 다른 branch에서 같은 줄을 고치고 합칠 경우

| 초록색 branch(master) | 노란색 branch |
| --------------------- | ------------- |
| const a = 10;         | const a = 20; |

1. 변경 후 각자 commit

2. 초록색 branch(master)에서 노란색 branch를 merge

3. **같은 부분을 수정했기때문에 충돌(conflict) 발생**

4. 초록색 branch(master) or 노란색 branch or 둘다 합침

5. git add .

6. git merge --continue

<br/>

---

#### 방법 2) rebase

**다른 브랜치를 현재 Checkout된 브랜치에 rebase 하는 명령**

`git rebase 노란색 branch`

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/7d148305-8ff3-4dd4-b72f-acee5193c0dc)

- 어떤 특정 브랜치를 base로 커밋 이력을 재정렬하겠다는 명령어

  - **base: 브런치가 생성될 때 기준점**

- **재정렬되는 commit 이력이기 때문에, 재정렬되는 commit 이력에는 이전과는 다른 새로운 해쉬 ID가 부여**

<br/>

---

### git cherry-pick

**특정 commit을 가져올 때 사용**

- branchA: 중요한 commit

- branckB: 별로 안 중요한 commit

master에서 branchA의 중요한 commit만을 가져오고 싶을 때, `git cherry-pick <원하는 커밋 id>`

<br/>

> Tip) Commit에 tag 설정<br/><br/>
> 매번 커밋 id찾기 번거롭다 -> tag사용 (git tag <tag 이름>)<br/>
> 중요한 기점이 되는 commit에 tag를 달아 접근하기 편하게 함.

<br/>

---

### git stash

**임시 저장**

<br/>

**상황1) branchA에서 작업하는 중에 branchB로 갔다오고 싶음**

git checkout branckB => branchA의 변경 사항을 commit하던지, git stash하라고 나옴

git stash하고 branchB에 감 => 다시 branchA로 돌아옴 => git stash apply => 기존 작업 하던 것 보여줌.

<br/>

**상황2) branch 착각하고 잘 못 썼을때**

branchA의 stash 한 것을 branchB에서 불러올 수 있음.(B로가서 git stash apply)

<br/>

---

### git fetch

마지막 pull 이후 원격 저장소 또는 브랜치에 적용된 변경 사항을 확인할 수 있다.

- **git pull을 하면 로컬까지 바뀌어 버리니까 fetch를 통해 원격이랑 로컬이랑 비교 할 수 있음.**

- push: fetch + merge

<br/>

---

<br/>

> ## 📑 참고 자료

[Git 기초- 깃(git) 명령어 배워보기](https://webclub.tistory.com/317)

[Git--distributed-even-if-your-workflow-isnt](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0)

[[GIT] Merge vs Rebase 차이](https://dongminyoon.tistory.com/9)
