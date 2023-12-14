---
title: 일정 확인, 추가, 수정, 삭제_Fullcalendar
author: cotes
date: 2023-11-02 00:34:00 +0800
categories: [Work-Devlog, 뇌파 분석 시스템]
tags: [Work-Devlog, Fullcalendar]
---

<!-- 프로젝트 작업하면서 했던 고민, 어떻게 해결했는지에 대한 내용이 담겨져있습니다. -->

> ## Fullcalendar란?

_자바스크립트 기반 오픈소스 라이브러리로 공식페이지에서는 "가장 유명한 자바스크립트 캘린더"라고 설명하고 있다._

<br/>

---

<br/>

> ## 사용 목적

아래의 기능을 구현하기 위해 캘린더 라이브러리 사용

- 기능

  - 기능 1) 일, 주, 달, 연간 달력 필요
  - 기능 2) 사용자의 일정 캘린더에 표시
  - 기능 3) 날짜 클릭 시, 이벤트 실행
    > event 1) 날짜 클릭 시, 해당 날짜의 스케줄 list 띄우기<br/>
    > event 2) 날짜 클릭 시, 해당 날짜에 스케줄 추가 할 수 있도록 modal 열기<br/>
    > event 3) 날짜 클릭 시, 해당 달이 아닐 경우 해당하는 달로 이동<br/>
    > event 4) 달력 title 변경

<br/>

---

<br/>

> ## 선택한 이유

웹앱 개발 달력이나 일정, 스케줄러 관련 구현 시 가장 많이 사용하는 라이브러리이기도 하고, 디자인이 가장 비슷하고 필요한 기능도 제공하고 있어 사용하였다.

<br/>

---

<br/>

> ## 사용 방법

[Fullcanlendar 공식 문서](https://fullcalendar.io/docs/react)

### 1. 설치

```
 $ npm install  --save  @fullcalendar/core  @fullcalendar/react  @fullcalendar/daygrid
```

### 2. 적용

```javascript
import React from "react";
import FullCalendar from "@fullcalendar/react";
import dayGridPlugin from "@fullcalendar/daygrid";

export default class DemoApp extends React.Component {
  render() {
    return (
      <FullCalendar plugins={[dayGridPlugin]} initialView="dayGridMonth" />
    );
  }
}
```

<br/>

---

<br/>

> ## 📌 기능1) 일, 주, 달, 연간 달력 표시

- plugin download 및 추가
- headerToolbar에 추가

```javascript
plugins={[
  dayGridPlugin,
  timeGridPlugin,
  multiMonthPlugin,
]}
headerToolbar={[
  start:'',
  center:'prev,title,next',
  end:'timeGridDay,timeGridWeek,dayGridMonth,multiMonthYear'.
]}
```

🖥️ 적용 화면

![image](https://github.com/hajung00/React-Sleact/assets/66300154/c84aea4d-9b84-423a-acf1-a108472e90a0)

<br/>

---

<br/>

> ## 📌 기능2) 사용자의 일정 캘린더에 표시

- useQuery로 해당 계정의 모든 일정(schedule)을 가져온다
- Fullcalendar의 event속성에 useQuery로 받아온 scheduel을 등록한다.

```javascript
evnet={schedule.map((data,i)=>{
  return {
     title: data.ScheduleInfo_Name,
     start: data.ScheduleInfo_Begin,
     end: data.ScheduleInfo_End,
     eventBackgroundColor: data.ScheduleInfo_Color,
  }
})}
```

🖥️ 적용 화면

![image](https://github.com/hajung00/React-Sleact/assets/66300154/a18e8365-e2eb-4d81-a8b3-e5b05628534a)

<br/>

---

<br/>

> ## 📌 기능3) 날짜 클릭 시, 이벤트 실행

- 날짜를 클릭할 때 발생하는 dateClick 핸들러 이용

```javascript
dateClick={(date) => {
  dateClick(date);
}}
```

- date console 확인
  ![image](https://github.com/hajung00/React-Sleact/assets/66300154/b4be40e6-9a93-4d0e-8386-ddb4b17cfde9)

### event 1) 날짜 클릭 시, 해당 날짜의 스케줄 list 띄우기

- 클릭한 날짜가 바뀔 때마다 useQuery refetch로 데이터 다시 가져옴
- 새롭게 가져온 schedule을 list로 띄워줌

- 🖥️ 적용 화면
  ![image](https://github.com/hajung00/React-Sleact/assets/66300154/5ba6edba-6a69-457e-9c36-a7ab5a1d85ac)

<br/>

### event 2) 날짜 클릭 시, 해당 날짜에 스케줄 추가 할 수 있도록 modal 열기

- 날짜 클릭 시, 해당 날짜를 일정 추가 모달로 props로 전달하여 기본값으로 설정

- 🖥️ 적용 화면
  ![image](https://github.com/hajung00/React-Sleact/assets/66300154/b7e48ad6-0e89-47ff-9efb-fbe1d87244af)

<br/>

### event 3) 날짜 클릭 시, 해당 달이 아닐 경우 해당하는 달로 이동

- dateClick함수에서 해당 날짜가 이번달이 맞는지 확인
- 클릭한 날짜가 전 달인 경우 fullCalendar의 “<” 버튼을 click으로 실행
- 클릭한 날짜가 다음 달인 경우 fullCalendar의 “>” 버튼을 click으로 실행
- 클릭한 날짜가 day, week, year에도 적용이 되어야함으로 클릭한 날짜가 변경될 때마다
  gotoDate api이용해서 적용되도록 한다.

- 🖥️ 적용 화면
  ![image](https://github.com/hajung00/React-Sleact/assets/66300154/f92a2ad6-2f0d-4b52-b67e-842bace2bb1c)

<br/>

### event 4) 달력 title 변경

- 조건

  - 조건 1) 클릭한 날짜가 없을 경우, 오늘 날짜로 설정
    - calendarClickDate의 초기값을 오늘 날짜로 설정
  - 조건 2) 클릭한 날짜가 있을 경우, 해당 날짜로 설정
    - dateClick이벤트 발생 시, calendarClickDate의 값을 클릭한 날짜로 변경

- titleFormat 속성 이용하여 calendar title변경

```javascript
titleFormat={ function (date) {
 return (
   calendarClickDate.split("-")[1] +
   "월 " +
   calendarClickDate.split("-")[2] +
   "일 " +
   calendarClickDate.split("-")[0] +
   "년 " +)}}
```

- 🖥️ 적용 화면
  ![image](https://github.com/hajung00/React-Sleact/assets/66300154/43f5c452-bc92-4359-8984-ecf410266e7f)
