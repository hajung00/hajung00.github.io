---
layout: single
title: "2022-08-05 Test"
categories: Coding-Test
tag: [Coding-Test, JavaScript]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## programmers) 평균 구하기

### 문제

정수를 담고 있는 배열 arr의 평균값을 return하는 함수, solution을 완성해보세요.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; arr &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| :---------------------------------------------------: | :------------------------------------------------------------------: |
|                       [1,2,3,4]                       |                                 2.5                                  |
|                         [5,5]                         |                                  5                                   |

### 내가 한 풀이

```javascript
function solution(arr) {
  var answer = 0;
  let b = arr.reduce((a, i) => {
    return a + i;
  });
  answer = b / arr.length;
  return answer;
}
```

## programmers) 콜라츠 추측

### 문제

1937년 Collatz란 사람에 의해 제기된 이 추측은, 주어진 수가 1이 될 때까지 다음 작업을 반복하면, 모든 수를 1로 만들 수 있다는 추측입니다. 작업은 다음과 같습니다.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; n &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; result &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| :-------------------------------------------------: | :------------------------------------------------------------------: |
|                          6                          |                                  8                                   |
|                         16                          |                                  4                                   |
|                       626331                        |                                  -1                                  |

### 내가 한 풀이

```javascript
function solution(num) {
  var answer = 0;
  if (num === 1) return answer;
  else {
    do {
      if (num % 2 === 0) {
        num = num / 2;
        answer++;
      } else {
        num = num * 3 + 1;
        answer++;
      }
    } while (num != 1 && answer < 500);
    if (answer >= 500) return -1;
    else return answer;
  }
}
```

### 다른 풀이

```javascript
function collatz(num) {
  var answer = 0;
  while (num != 1 && answer != 500) {
    num % 2 == 0 ? (num = num / 2) : (num = num * 3 + 1);
    answer++;
  }
  return num == 1 ? answer : -1;
}
```

## programmers) 최대공약수와 최소공배수

### 문제

두 수를 입력받아 두 수의 최대공약수와 최소공배수를 반환하는 함수, solution을 완성해 보세요.
배열의 맨 앞에 최대공약수, 그다음 최소공배수를 넣어 반환하면 됩니다.
예를 들어 두 수 3, 12의 최대공약수는 3, 최소공배수는 12이므로 solution(3, 12)는 [3, 12]를 반환해야 합니다.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; n &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp; m &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp; return &nbsp;&nbsp;&nbsp;&nbsp; |
| :-------------------------------------------------: | :-------------------------------------------------: | :------------------------------------------------------: |
|                          3                          |                         12                          |                          [3,12]                          |
|                          2                          |                          5                          |                          [1,10]                          |

### 내가 한 풀이

- 유클리드 호제법

  A와 B의 최대공약수를 구하기 위해서 A를 B로 나눈 나머지 R1을 구합니다.
  B를 R1으로 나눈 나머지 R2를 구합니다.
  R1을 R2로 나눈 나머지 R3를 구합니다.
  이 과정을 계속 반복하여, 어느 한 쪽이 나누어떨어질 때까지 반복합니다. 이 직전 얻은 나머지가 최대공약수입니다.

1. n과 m이 같지 않을 경우, n과 m을 나눈 나머지를 배열(b)에 저장해두고 b(i) % b(i+1)을 반복
2. n과 m이 같을 경우, n과 m둘중 하나의 값 정해서 출력

```javascript
function solution(n, m) {
  var answer = [];
  let b = [m];

  if (n === m) return [n, n];
  else {
    b.push(n % m);
    for (let i = 0; i < 100; i++) {
      if (b[i] % b[i + 1] === 0) {
        answer.push(b[i + 1], (n * m) / b[i + 1]);
        break;
      } else b.push(b[i] % b[i + 1]);
    }
  }

  return answer;
}
```

❗예외 케이스 n=12, m=3

- 문제점: n%m에서 나누어 떨어지는 경우에도 유클리드 호제법 실시
- 해결법: n%m에서 나누어 떨어지는 경우 유클리드 호제법 실시하지 않고 n과m 자리만 바꿔준다.

💡수정 후

```javascript
function solution(n, m) {
  var answer = [];
  let b = [m];

  if (n === m) return [n, n];
  else {
    b.push(n % m);
    for (let i = 0; i < 10000; i++) {
      if (n % m != 0) {
        if (b[i] % b[i + 1] === 0) {
          answer.push(b[i + 1], (n * m) / b[i + 1]);
          console.log(b);
          break;
        } else b.push(b[i] % b[i + 1]);
      } else return [m, n];
    }
    return answer;
  }
}
```

### 다른 풀이

```javascript
function gcdlcm(a, b) {
  var r;
  for (var ab = a * b; (r = a % b); a = b, b = r) {}
  return [b, ab / b];
}
```
