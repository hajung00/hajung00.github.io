---
title: Git branch
author: cotes
date: 2023-05-01 11:33:00 +0800
categories: [Study, Git]
tags: [favicon]
---

> ## Git branch란?

**독립적으로 어떤 작업을 진행하기 위한 개념**이다.

개발을 하다 보면, 한 페이지 안에 여러 기능을 따로 구현하거나, 이전 코드와 비교를 위해 여러 코드를 복사해야 하는 일이 자주 있다.

**Git의 브랜치를 활용하면, 코드를 통째로 복사한 후, 원래 코드에 영향을 주지않고 독립적으로 개발할 수 있다.**

주로 여러명이 동시에 작업할 때, 다른 사람에게 영향을 주거나 받지 않기 위해, 팀 프로젝트에서 많이 활용되고 있다.

개발자들은 각자 맡은 역할의 기능을 구현하면서 "작업단위"로 업무가 진행된다. 이때 **서로에게 영향을 주지 않고 내용을 모두 기록하기 위해 만들어진 대책**이다.

<br/>

---

<br/>

> ## Git Branch 작업 순서

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/f98bbf5d-d35e-477f-b489-237996809ea7)

- **기본적으로 Git은 master 브랜치를 만든다.**

- 처음 커밋하면 이 master 브랜치가 생성된 커밋을 가리킨다.

- 이후 커밋을 만들면 master 브랜치는 자동으로 가장 마지막 커밋을 가리킨다.

### 1. 새 브랜치 생성

`git branch testing`

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/edd54194-39fd-4dc7-8b7b-707f2cb4c1f6)

- **브랜치를 새로 만들었지만, Git은 아직 master 브랜치를 가리키고 있다.**

- git branch 명령은 브랜치를 만들기만 하고 브랜치를 옮기지 않는다.

### 2. 브랜치 이동

`git branch testing`

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/795f585f-aca1-4269-9fbc-3c8760f9e10a)

- HEAD는 testing 브랜치를 가리킨다.

<br/>

HEAD가 testing 브랜치를 가르킨 상태에서 파일 수정 후 commit을 새로 한 번 해보자.

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/099565e3-f326-4aaa-b10a-be195a8c7926)

- **새로 커밋해서 testing 브랜치는 앞으로 이동**했다.

- 하지만, **master 브랜치는 여전히 이전 커밋을 가리킨다.**

<br/>

---

<br/>

> ## Git Branch 명령어

**브랜치 생성**

`git branch [새로운 브랜치 이름]`

<br/>

**브랜치 전환**

`git switch -c [새로운 브랜치 이름]`
`git checkout -b [새로운 브랜치 이름]`

<br/>

**브랜치 목록 확인**

`git branch`

<br/>

**브랜치 목록과 각 브랜치 최근 커밋 확인**

`git branch -v`

<br/>

**브랜치 삭제**

`git branch -d [삭제할 브랜치 이름]`

<br/>

**브랜치 강제 삭제**

`git branch -D`

<br/>

**브랜치 병합**

- master 1 -> branch2로 병합할 때

`git checkout master1`
`git merge branch2`

<br/>

**로그에 모든 브랜치 그래프로 표현**

`git log --branches --graph --decorate`

<br/>

**아직 commit 하지 않은 작업 스택에 임시 저장**

`git stash`

<br/>

---

<br/>

> ## 📑 참고 자료

[Git 브랜치 - 브랜치란 무엇인가](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

[Git(깃) 브랜치(Branch) 사용방법 및 명령어 || Git | Branch ||](https://borntodevelop.tistory.com/entry/Git%EA%B9%83-%EB%B8%8C%EB%9E%9C%EC%B9%98Branch-%EA%B4%80%EB%A6%AC-%EB%AA%85%EB%A0%B9%EC%96%B4-Git-Branch-%EB%AA%85%EB%A0%B9%EC%96%B4)
