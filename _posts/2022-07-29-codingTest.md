---
layout: single
title: "2022-07-29 Test"
categories: Coding-Test
tag: [Coding-Test, JavaScript]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 백준 2588번

### 문제

(세 자리 수) × (세 자리 수)는 다음과 같은 과정을 통하여 이루어진다.

![image](https://user-images.githubusercontent.com/66300154/182405967-89a74724-ef4c-4f78-9dde-c2f712ff37ea.png)

(1)과 (2)위치에 들어갈 세 자리 자연수가 주어질 때 (3), (4), (5), (6)위치에 들어갈 값을 구하는 프로그램을 작성하시오.

#### 입력

<hr>
첫째 줄에 (1)의 위치에 들어갈 세 자리 자연수가, 둘째 줄에 (2)의 위치에 들어갈 세자리 자연수가 주어진다.

#### 출력

<hr>
첫째 줄부터 넷째 줄까지 차례대로 (3), (4), (5), (6)에 들어갈 값을 출력한다.

### 내가 한 풀이

1. 입력받은 문자열 split('\n)으로 분리
2. 두번째 문자열 하나하나 분리 후 두 문자열 숫자로 변환
3. 두번째 숫자 하나하나가 첫번째 숫자랑 곱해짐

```javascript
const fs = require("fs");
const [input1, input2] = fs
  .readFileSync("/dev/stdin")
  .toString()
  .trim()
  .split("\n");
const num1 = parseInt(input1);
const num2 = input2.split("").map(Number);
let result = num2.reverse().map((x) => x * num1);
result.push(num1 * parseInt(input2));
console.log(result.join("\n"));
```

### 다른 풀이

일,십,백의 자리를 추출 후 곱하기

```javascript
const fs = require("fs");
const [input1, input2] = fs
  .readFileSync("/dev/stdin")
  .toString()
  .split("\n")
  .map(Number);
let num2 = input2 % 10;
console.log(input1 * num2);
num2 = parseInt((input2 / 10) % 10);
console.log(input1 * num2);
num2 = parseInt(input2 / 100);
console.log(input1 * num2);
console.log(input1 * input2);
```

## programmers) 직사각형 별찍기

### 문제

이 문제에는 표준 입력으로 두 개의 정수 n과 m이 주어집니다.
별(\*) 문자를 이용해 가로의 길이가 n, 세로의 길이가 m인 직사각형 형태를 출력해보세요.

#### 입력

<hr>
5 3

#### 출력

<hr>

![image](https://user-images.githubusercontent.com/66300154/182406242-9e5ad8a5-36d4-47d7-af9a-d5776bb52f73.png)

### 내가 한 풀이

```javascript
process.stdin.setEncoding("utf8");
process.stdin.on("data", (data) => {
  const n = data.split(" ");
  const a = Number(n[0]),
    b = Number(n[1]);
  let result = [];
  for (let i = 0; i < b; i++) {
    for (let j = 0; j < a; j++) {
      result.push("*");
    }
    console.log(result.join(""));
    result = [];
  }
});
```

### 다른 풀이

```javascript
process.stdin.setEncoding("utf8");
process.stdin.on("data", (data) => {
  const n = data.split(" ");
  const a = Number(n[0]),
    b = Number(n[1]);
  const row = "*".repeat(a);
  for (let i = 0; i < b; i++) {
    console.log(row);
  }
});
```

### \* 알게된 점

- String.repeat(number): 문자열을 반복한 값을 반환하는 메서드
- 매개변수(parameter)
  number: 반복 횟수

ex) 'abc'.repeat(2) => abcabc

## programmers) x만큼 간격이 있는 n개의 숫자

### 문제

함수 solution은 정수 x와 자연수 n을 입력 받아, x부터 시작해 x씩 증가하는 숫자를 n개 지니는 리스트를 리턴해야 합니다. 다음 제한 조건을 보고, 조건을 만족하는 함수, solution을 완성해주세요.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; x &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp; n &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp; answer &nbsp;&nbsp;&nbsp;&nbsp; |
| :-------------------------------------------------: | :-------------------------------------------------: | :------------------------------------------------------: |
|                          2                          |                          5                          |                       [2,4,6,8,10]                       |
|                          4                          |                          3                          |                         [4,8,12]                         |
|                         -4                          |                          2                          |                         [-4, -8]                         |

### 내가 한 풀이

1. 입력받은 x에 *1, *2, *3 ... *n
2. 인덱스 + 1을 한 값을 x에 n번 곱해줌

```javascript
function solution(x, n) {
  var answer = [];
  for (let i = 1; i <= n; i++) {
    answer[i - 1] = x * i;
  }
  return answer;
}
```

### 다른 풀이

```javascript
function solution(x, n) {
  return Array(n)
    .fill(x)
    .map((v, i) => (i + 1) * v);
}
```

### \* 알게된 점

Array.prototype.fill( )

- fill( ) : 배열의 시작 인덱스부터 끝 인덱스의 이전까지 정적인 값 하나로 채움.

Array.prototype.map( )

- map( ) : 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환.

## programmers) 행렬의 덧셈

### 문제

행렬의 덧셈은 행과 열의 크기가 같은 두 행렬의 같은 행, 같은 열의 값을 서로 더한 결과가 됩니다. 2개의 행렬 arr1과 arr2를 입력받아, 행렬 덧셈의 결과를 반환하는 함수, solution을 완성해주세요.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; arr1 &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp; arr2 &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp; return &nbsp;&nbsp;&nbsp;&nbsp; |
| :----------------------------------------------------: | :----------------------------------------------------: | :------------------------------------------------------: |
|                     [[1,2],[2,3]]                      |                     [[3,4],[5,6]]                      |                      [[4,6],[7,9]]                       |
|                       [[1],[2]]                        |                       [[3],[4]]                        |                        [[4],[6]]                         |

### 내가 한 풀이

```javascript
function solution(arr1, arr2) {
  var answer = [[]];
  for (let i = 0; i < arr1.length; i++) {
    answer[i] = [];
    for (let j = 0; j < arr1[i].length; j++)
      answer[i][j] = arr1[i][j] + arr2[i][j];
  }
  return answer;
}
```

### 다른 풀이

```javascript
function solution(A, B) {
  return A.map((a, i) => a.map((b, j) => b + B[i][j]));
}
```
