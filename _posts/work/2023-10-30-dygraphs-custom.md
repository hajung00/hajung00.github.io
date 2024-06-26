---
title: Dygraphs 커스텀
author: cotes
date: 2023-10-30 00:34:00 +0800
categories: [Work-Devlog, 뇌파 분석 시스템]
tags: [Work-Devlog, Dygraphs, Custom]
---

<!-- 프로젝트 작업하면서 했던 고민, 어떻게 해결했는지에 대한 내용이 담겨져있습니다. -->

> ## Dygraphs 기존 기능

1. 드래그 실시간 기능
2. Zoom 기능
3. y축 드래그, Zoom 기능

_[Dygraphs 공식 문서 참조](https://dygraphs.com/)_
<img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/47cd0de9-caf1-4d89-8a8c-3bd4fe536471" width="60%" height="40%" alt="image" />

<br/>

---

<br/>

> ## 📌 기능1) 구간 선택

- 조건

  - 조건 1) 같은 지점 클릭 시, 구간으로 추가하지 않음(mousedown === mouseup)
  - 조건 2) 기존에 선택한 구간의 조정
  - 조건 3) 기존에 선택한 구간을 새로 선택한 구간이 포함한다면 포함하는 범위로 변경
  - 조건 4) 기존에 선택한 구간의 중간값을 선택하지 못하도록 설정

<br/>

- 전체적인 flow

  1.  Zoom기능 제거
  2.  선택한 구간 allConent에 저장

      - 위의 조건에 만족하지 않고 새로운 구간의 추가 시, allContent에 저장

  3.  선택한 구간 화면에 표시

      - allContent에 추가되는 경우에만 화면에 표시한다.
      - mousedown의 지점에 line그리고, mouseup의 지점에 라인을 그리고 끝점과 시작점의 차이를 fillRect로 채워준다.

<br/>

- 🖥️ 구간 선택 시연 영상

  ![구간 선택 시연 영상](https://github.com/hajung00/hajung00.github.io/assets/66300154/875f6275-43c1-44bf-855e-1bf37530ef5a)

### Case1) 같은 지점 클릭 시, 화면에 표시 X

- mouseup시, 시작과 끝점 비교하여 같은 값인지 판별 후 같을 경우 allContent에 추가하지 않아 화면에 표시 안됨.

### Case2) 기존에 선택한 구간의 시작, 끝 지점 조정일 경우

- flow

  1.  mousedown시, 어떤 content를 조정하는지 currentConent에 저장

  2.  mousedown시, 시작점인지 끝점의 조정인지 방향을 sectionDirection에 저장 (시작점 = front, 끝점 = back)

  3.  mouseup시, setcitonDirection에 따라 시작/끝 점, 포함하는 구간 다시 그려줌

      - case1) 시작 점을 조정할 경우 currentContent 다 지우고 새롭게 mouseup한 부분을 시작점으로 잡고 그려준다.
      - case2) 끝 점을 조정할 경우 currentContent 다 지우고 새롭게 mouseup한 부분을 끝점으로 잡고 그려준다.

  4.  mouseup시, allContent에서 해당 content의 startX, endX 값 변경해줌

### Case3) 기존 구간을 새로 선택한 구간이 포함하는 경우

1. mouseup시, 클릭한 지점이 allContent의 content에 완전히 포함되는 경우

   - if (새로운 시작점< content의 시작점 && content의 끝점< 새로운 끝점)

2. 기존 content의 구간 지우고, allContent에서 제거

3. 새로 선택한 구간 allContent에 push

### Case4) 이미 선택된 구간의 중간값을 선택할 경우

- mousedown시, 클릭한 지점이 allContent의 하나의 content에 포함되는(중간값) 경우 값 추가 X, 화면에 그리는 부분 실행되지 않도록 설정

<br/>

---

<br/>

> ## 📌 기능2) 우클릭 시, 선택한 구간 삭제

1. mousedown시, 우클릭일경우, deleteCheckpoint실행
2. 삭제할 구간을 찾아 allContent에서 제거
3. 삭제한 구간 화면에서 제거

<br/>

---

<br/>

> ## 📌 기능3) x축 ->시간, y축 -> 채널 이름

1.  ticks 설정하는 부분에서 x축 samplingrate만큼 나누기

    ```javascript
    this.xticks.push({
      pos: pos,
      label: Math.floor(label / 60) + "s",
      has_tick: has_tick
    });
    ```

    <br/>

2.  ticks 설정하는 부분에서 y축 채널 이름으로 변경

- 기존에는 그래프의 최대값과 최솟값 사이에서 y를 나타냈으면, y축 나타내는 범위를 최댓값으로 맞춘다.

  ```javascript
  pct = (yRange[1] - y) / yRange[1];
  ```

- y축 나타내는 범위를 채널 수 만큼 나눠서 위치 지정

  ```javascript
  const distance = axis.computedValueRange[1] / channel.length;
  const ticksChannel = channel.map((item, i) => {
    var obj = {};
    obj["v"] = i * distance;
    obj["label"] = item;
    return obj;
  });
  ```
