---
layout: single
title: "2022-08-09 Test"
categories: Coding-Test
tag: [Coding-Test, JavaScript]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## programmers) 자릿수 더하기

### 문제

자연수 N이 주어지면, N의 각 자릿수의 합을 구해서 return 하는 solution 함수를 만들어 주세요.
예를들어 N = 123이면 1 + 2 + 3 = 6을 return 하면 됩니다.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; n &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; answer &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| :-------------------------------------------------: | :------------------------------------------------------------------: |
|                         123                         |                                  6                                   |
|                         987                         |                                  24                                  |

### 내가 한 풀이

```javascript
function solution(n) {
  var answer = 0;
  let a = [];
  do {
    a.push(n % 10);
    n = Math.floor(n / 10);
  } while (n > 0);
  a.map((a, i) => {
    answer = answer + a;
  });
  return answer;
}
```

### 다른 풀이

- 문자 풀이

```javascript
function solution(n) {
  // 쉬운방법
  return (n + "").split("").reduce((acc, curr) => acc + parseInt(curr), 0);
}
```

## programmers) 이상한 문자 만들기

### 문제

문자열 s는 한 개 이상의 단어로 구성되어 있습니다. 각 단어는 하나 이상의 공백문자로 구분되어 있습니다. 각 단어의 짝수번째 알파벳은 대문자로, 홀수번째 알파벳은 소문자로 바꾼 문자열을 리턴하는 함수, solution을 완성하세요.

#### 입출력 예

<hr>
"try hello world"	"TrY HeLlO WoRlD"

### 내가 한 풀이

1. 공백으로 분리 후 배열로 저장
2. 각 문자의 길이만큼 실행시켜 짝수는 대문자, 홀수는 소문자로 변경 후 answer에 저장
3. 맨 마지막빼고 띄어쓰기를 붙여준다.

```javascript
function solution(s) {
  let a = s.split(" ");
  let answer = "";
  for (let i = 0; i < a.length; i++) {
    for (let j = 0; j < a[i].length; j++) {
      if (j % 2 == 0) {
        answer += a[i][j].toUpperCase();
      } else {
        answer += a[i][j].toLowerCase();
      }
    }
    if (i + 1 < a.length) answer += " ";
  }
  return answer;
}
```

### 다른 풀이

- 대문자로 다 변경 후 replace로 연속된 두 문자를 짝수번째는 대문자, 홀수번째는 소문자로 변경

```javascript
function toWeirdCase(s) {
  return s.toUpperCase().replace(/(\w)(\w)/g, function (a) {
    return a[0].toUpperCase() + a[1].toLowerCase();
  });
}
```

### \* 알게된 점

- String.toUpperCase(): 문자열을 대문자로 변환하는 데 사용.
- String.toLowerCase(): 문자열을 소문자로 변환하는 데 사용.

## programmers) 약수의 합

### 문제

정수 n을 입력받아 n의 약수를 모두 더한 값을 리턴하는 함수, solution을 완성해주세요.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; n &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| :-------------------------------------------------: | :------------------------------------------------------------------: |
|                         12                          |                                  28                                  |
|                          5                          |                                  6                                   |

### 내가 한 풀이

```javascript
function solution(n) {
  var answer = 0;
  for (let i = 1; i <= n; i++) {
    if (n % i == 0) answer += i;
  }
  return answer;
}
```

### 다른 풀이

- 계산량을 줄이기 위해 n까지 다 안하고, 아래 짝을 찾으면 위아래를 같이 더함.

  ex) 12면, 3을 찾앗을때 3+4를 해주는 방식

```javascript
function solution(n) {
  var answer = 0;
  let i;
  for (i = 1; i <= Math.sqrt(n); i++) {
    if (!(n % i)) {
      answer += i + n / i;
    }
  }
  i--;
  return i === n / i ? answer - i : answer;
}
```

## programmers) 시저 암호

### 문제

어떤 문장의 각 알파벳을 일정한 거리만큼 밀어서 다른 알파벳으로 바꾸는 암호화 방식을 시저 암호라고 합니다. 예를 들어 "AB"는 1만큼 밀면 "BC"가 되고, 3만큼 밀면 "DE"가 됩니다. "z"는 1만큼 밀면 "a"가 됩니다. 문자열 s와 거리 n을 입력받아 s를 n만큼 민 암호문을 만드는 함수, solution을 완성해 보세요.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; s &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp; n &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp; result &nbsp;&nbsp;&nbsp;&nbsp; |
| :-------------------------------------------------: | :-------------------------------------------------: | :------------------------------------------------------: |
|                        "AB"                         |                          1                          |                           "BC"                           |
|                         "z"                         |                          1                          |                           "a"                            |
|                       "a B z"                       |                          4                          |                         "e F d"                          |

### 내가 한 풀이

1. 문자열 배열로 저장 후 각각의 원소를 아스키코드로 변환한다.
2. 아스키코드에 n만큼 더해준다.
3. 더한 값이 문자를 넘어갈 경우, -26해서 순환하도록
4. 공백은 그대로 공백으로 출력

```javascript
function solution(s, n) {
  let a = s.split("");
  let b = a.map((a1) => {
    if (
      (a1.charCodeAt(0) + n > 90 && a1.charCodeAt(0) <= 90) ||
      a1.charCodeAt(0) + n > 122
    )
      return a1.charCodeAt(0) + n - 26;
    if (a1 === " ") return " ";
    else return a1.charCodeAt(0) + n;
  });

  let c = b.map((b1) => {
    if (b1 === " ") return " ";
    else return String.fromCharCode(b1);
  });
  return c.join("");
}
```

### 다른 풀이

- 아스키코드 표 없이 대문자 소문자 저장

```javascript
function solution(s, n) {
  var upper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  var lower = "abcdefghijklmnopqrstuvwxyz";
  var answer = "";

  for (var i = 0; i < s.length; i++) {
    var text = s[i];
    if (text == " ") {
      answer += " ";
      continue;
    }
    var textArr = upper.includes(text) ? upper : lower;
    var index = textArr.indexOf(text) + n;
    if (index >= textArr.length) index -= textArr.length;
    answer += textArr[index];
  }
  return answer;
}
```

### \* 알게된 점

- String.charCodeAt(index): 문자열 중 해당 index를 선택하여 아스키코드 번호로 변환해주는 함수
- String.fromCodeAt(number): 아스키코드번호를 받아 문자열을 구성해주는 함수
