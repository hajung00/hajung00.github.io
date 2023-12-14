---
title: 부모, 자식 컴포넌트에서의 useEffect 실행 순서
author: cotes
date: 2023-11-15 00:34:00 +0800
categories: [Work-Devlog, 뇌파 분석 시스템]
tags: [Work-Devlog, useEffect, 에러 해결]
---

<!-- 프로젝트 작업하면서 했던 고민, 어떻게 해결했는지에 대한 내용이 담겨져있습니다. -->

> ## Report 페이지 기능

### 1. 선택한 파일에서 진행한 분석 종류를 select로 선택할 수 있다.

- 초기값 = Preprocess

<img src="https://github.com/hajung00/React-Sleact/assets/66300154/60f9cf05-25ad-4e57-880e-ead105a322ec" width="90%" height="50%" alt="image"/>

### 2. select에서 선택한 분석 종류의 결과지를 보여준다.

- 분석 종류가 변경될 때마다 각 분석에 해당하는 결과지를 가져와야 한다.

<img src="https://github.com/hajung00/React-Sleact/assets/66300154/c99e0974-ebed-4db0-8e1e-cdfe9ca04511" width="90%" height="50%" alt="image"/>

🖥️ 적용 화면

<img src="https://github.com/hajung00/React-Sleact/assets/66300154/596262ff-6702-4b2c-b1e8-6c5d89c34805" width="90%" height="50%" alt="image"/>

<br/>

---

<br/>

> ## Report 페이지 구조 및 컴포넌트 별 기능

### 📚 구조

<img src="https://github.com/hajung00/SidePJ-next-node-full-sns/assets/66300154/6d42320e-5644-45d6-89d4-f1fab1aafc40" width="80%" height="50%" alt="image"/>

### 📚 컴포넌트 별 기능

report

- 파일 불러와서 파일 리스트 띄우기, 파일 선택하면 fileid값 변경

<br/>

AnalysisReport

- report에서 fileid을 변경하면, 기본값인 preprocess로 변경
- select box선택 시, 선택한 analysistype ReportLayout로 전달

<br/>

ReportLayout:

- AnalysisReport에서 받은 analysistype에 따라 각 해당하는 결과지 보여줌.

<br/>

---

<br/>

> ## ❗문제 상황

- ReportLayout에서 결과지 생성을 위한 정보를 S3에서 받아오는데 **요청이 2번 보내지는 것을 확인**

- fileid가 변경될 때, 초깃값인 preprocess에 대한 요청을 해야하는데 **이전에 선택한 분석 종류에 대한 요청을 보낸 다음 preprocess를 요청**

<br/>

---

<br/>

> ## ❗원인

- ReportLayout에서 fileid가 변경될 때 정보를 다시 받아오는데 **이전의 분석 종류가 초기화 되기 전에 실행되고 초기화 후에 실행되어 요청이 2번** 감.

- report에서 fileid가 변경되면, **AnalysisReport preprocess로 초기화 시키는데 초기화 되기 전에 ReportLayout이 실행**되는 것 같음.

- 실행 순서를 보니 맨 **하위 컴포넌트의 useeffect부터 상위로 실행**됨.<br/>
  => **하위의 useEffect가 실행되니 초기화 되기 이전의 분석 종류에 대한 결과를 가져온 다음 초기화된 결과**를 가져온다.

- 당연히 **useEffect**가 상위 -> 하위로 실행될 것이라고 생각하였는데 자료도 찾아보고 test 컴포넌트를 생성하고 테스트 해보니 **하위에서부터 실행**되었댜.

<br/>

---

<br/>

> ## 💡문제 해결

- 하위 컴포넌트의 useEffect가 먼저 실행된다면 젤 **하위에서 초기화**를 진행시키면 될 것 같았다.

- **AnalysisReport에서 preprocess로 초기화 해주던걸** props로 넘겨 **ReportLayout에서 fileid가 변경되었을 때 초기화 해주도록 변경**하였더니 **요청이 한번만 가는 것을 확인**할 수 있었다.
