---
layout: single
title: "2022-08-02 Test"
categories: Coding-Test
tag: [Coding-Test, JavaScript]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## programmers) 핸드폰 번호 가리기

### 문제

프로그래머스 모바일은 개인정보 보호를 위해 고지서를 보낼 때 고객들의 전화번호의 일부를 가립니다.
전화번호가 문자열 phone_number로 주어졌을 때, 전화번호의 뒷 4자리를 제외한 나머지 숫자를 전부 \*으로 가린 문자열을 리턴하는 함수, solution을 완성해주세요.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; phone_number &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| :------------------------------------------------------------: | :------------------------------------------------------------------: |
|                         "01033334444"                          |                           "**\*\*\***4444"                           |
|                          "027778888"                           |                             "**\***8888"                             |

### 내가 한 풀이

1. 뒤의 4자리 저장
2. 앞의 자리 '\*'로 replace

```javascript
function solution(phone_number) {
  var answer = "";
  let back = phone_number.substr(-4, 4);
  let front = phone_number.substr(0, phone_number.length - 4);
  answer = front.replace(/[0-9]/g, "*") + back;
  return answer;
}
```

### 다른 풀이

```javascript
function hide_numbers(s) {
  return s.replace(/\d(?=\d{4})/g, "*");
}
```

### \* 알게된 점

String.substr(start,length): 문자열에서 start부터 length만큼의 문자들을 반환하는 함수

- 매개변수(parameter)

1. start: 문자열에서 추출을 시작하는 위치
2. length: 추출할 문자들의 총 개수( 생략된 경우 start부터 문자열의 끝까지 추출)

String.replace(regexp/substr, newSubstr/function): 문자열 내에서 특정 문자를 다른 문자로 치환하는 함수

- 매개변수(parameter)

1. regexp/substr: 정규식 객체 또는 문자열로 치환하기 위해 찾는 파라미터
2. newSubstr/function: 첫 번째 파라미터를 대신할 문자열 또는 함수

<br>
정규표현식: 특정 패턴의 문자열을 찾기 위한 표현 방식

형식: /패턴/플래그
/[0-9]/g: '숫자 0-9'를 모두 찾는다. \*플래그는 하나만 찾을지, 모두 다 찾을지 등을 설정하는 옵션(g: 모든 문자 검색)

String.slice(beginIndex, endIndex): 문자열에서 beginIndex부터 endIndex전까지의 문자들을 추출하여 반환하는 함수

- substr보다는 slice 사용 권장

## programmers) 핸드폰 번호 가리기

### 문제

양의 정수 x가 하샤드 수이려면 x의 자릿수의 합으로 x가 나누어져야 합니다. 예를 들어 18의 자릿수

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; arr &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| :---------------------------------------------------: | :------------------------------------------------------------------: |
|                          10                           |                                 true                                 |
|                          12                           |                                 true                                 |
|                          11                           |                                false                                 |
|                          13                           |                                false                                 |

### 내가 한 풀이

1. 정수 x를 string으로 변환 후 하나하나 분리
2. 분리한 문자를 숫자로 변환해 더함
3. 더한 값이 정수x와 나눴을 때 나머지가 0이면 true

```javascript
function solution(x) {
  var answer = true;
  let x1 = String(x);
  let a = x1.split("");
  let sum = 0;
  for (let i = 0; i < a.length; i++) {
    sum = sum + Number(a[i]);
  }
  if (x % sum != 0) answer = false;
  return answer;
}
```

### 다른 풀이

1. n에 ''붙여서 문자열로 바꾼 뒤, split('')로 배열로 반환
2. reduce로 i에 더한 값을 누적시킴 (이때 +를 붙여 다시 숫자로 변환후 더함)

```javascript
function solution(n) {
  return !(
    n %
    (n + "").split("").reduce(function (i, sum) {
      return +sum + +i;
    })
  );
}
```
