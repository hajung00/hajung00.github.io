---
layout: single
title: "2022-08-07 Test"
categories: Coding-Test
tag: [Coding-Test, JavaScript]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## programmers) 제일 작은 수 제거하기

### 문제

정수를 저장한 배열, arr 에서 가장 작은 수를 제거한 배열을 리턴하는 함수, solution을 완성해주세요. 단, 리턴하려는 배열이 빈 배열인 경우엔 배열에 -1을 채워 리턴하세요. 예를들어 arr이 [4,3,2,1]인 경우는 [4,3,2]를 리턴 하고, [10]면 [-1]을 리턴 합니다.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; arr &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| :---------------------------------------------------: | :------------------------------------------------------------------: |
|                       [4,3,2,1]                       |                               [4,3,2]                                |
|                         [10]                          |                                 [-1]                                 |

### 내가 한 풀이

1. arr중 가장 작은 수의 인덱스 찾기
2. splice로 제거

```javascript
function solution(arr) {
  let min = arr[0];
  let minIndex = 0;

  if (arr.length === 1) return [-1];
  else {
    for (let i = 1; i < arr.length; i++) {
      if (min > arr[i]) {
        min = arr[i];
        minIndex = i;
      }
    }
    arr.splice(minIndex, 1);
    return arr;
  }
}
```

### 다른 풀이

```javascript
function solution(arr) {
  arr.splice(arr.indexOf(Math.min(...arr)), 1);
  if (arr.length < 1) return [-1];
  return arr;
}
```

### \* 알게된 점

- Math.min() : 최솟값을 찾아주는 내장함수

  ex) Math.min(1,2,3,4,5) => 출력: 1

- Array.indexOf(search, fromIndex): 인자로 받은 값을 찾아 맞는 식별자 반환

  ex) let arr = [1,2,3,4]

  arr.indexOf(2) => 출력: 1

## programmers) 정수 제곱근 판별

### 문제

임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.
n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, n이 양의 정수 x의 제곱이 아니라면 -1을 리턴하는 함수를 완성하세요.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; n &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| :-------------------------------------------------: | :------------------------------------------------------------------: |
|                         121                         |                                 144                                  |
|                          3                          |                                  -1                                  |

### 내가 한 풀이

1. 제곱근을 찾아 정수인지 확인
2. 정수일 경우 1더해서 제곱, 아니면 -1 반환

```javascript
function solution(n) {
  var answer = 0;
  let number = Math.sqrt(n);
  Math.sqrt(n) % 1 == 0 ? (answer = (number + 1) ** 2) : (answer = -1);
  return answer;
}
```

### 다른 풀이

```javascript
function nextSqaure(n) {
  switch (n % Math.sqrt(n)) {
    case 0:
      return Math.pow(Math.sqrt(n) + 1, 2);
    default:
      return -1;
  }
}
```

### \* 알게된 점

- Math.sqrt(): 제곱근 구해줌
- Math.pow(): 제곱 구해줌

## programmers) 정수 내림차순으로 배치하기

### 문제

함수 solution은 정수 n을 매개변수로 입력받습니다. n의 각 자릿수를 큰것부터 작은 순으로 정렬한 새로운 정수를 리턴해주세요. 예를들어 n이 118372면 873211을 리턴하면 됩니다.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; n &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| :-------------------------------------------------: | :------------------------------------------------------------------: |
|                       118372                        |                                873211                                |

### 내가 한 풀이

1. sort정렬하고 reverse사용하기
2. 1번을 위해 정수n을 문자열로 변환후 split을 이용해 배열로 만들어줌
3. 배열 정렬 후 정수로 변환

```javascript
function solution(n) {
  let n1 = String(n);
  return Number(n1.split("").sort().reverse().join(""));
}
```

### 다른 풀이

```javascript
function solution(n) {
  var nums = [];
  do {
    nums.push(n % 10);
    n = Math.floor(n / 10);
  } while (n > 0);

  return nums.sort((a, b) => b - a).join("") * 1;
}
```

### \* 알게된 점

- Math.floor(): 소수점 이하를 버림한다.
- Array.sort(): 배열을 정렬한다.

❗sort() 메서드의 특징

1. 기본적으로 오름차순으로 정렬
2. 배열 요소를 문자열로 캐스팅하고 변환된 문자열을 비교하여 순서를 결정

```javascript
var numbers = [1, 10, 2, 20, 3, 30];
numbers.sort();
console.log(numbers); // [1,10,2,20,3,30]
```

❗ 문제점: 숫자의 크기 순서대로 정렬이 되지 않음.

💡 해결법: 비교 함수를 sort() 메서드에 전달해야 한다.

- 비교함수: sort() 메서드의 비교 함수는 두 개의 인수를 전달받으며 정렬 순서를 결정하는 값을 반환

  ex) 오름차순

```javascript
var numbers = [1, 10, 2, 20, 3, 30];
numbers.sort(function compare(a, b) {
  return a - b; // 내림차순은 b - a
  console.log(numbers); // [30,20,10,3,2,1]
});
```

return value > 0 이므로 a는 b 뒤에 온다.

## programmers) 자연수 뒤접어 배열로 만들기

### 문제

자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열 형태로 리턴해주세요. 예를들어 n이 12345이면 [5,4,3,2,1]을 리턴합니다.

#### 입출력 예

<hr>

| &nbsp;&nbsp;&nbsp;&nbsp; n &nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
| :-------------------------------------------------: | :------------------------------------------------------------------: |
|                        12345                        |                             [5,4,3,2,1]                              |

### 내가 한 풀이

- 문자풀이

```javascript
function solution(n) {
  var answer = [];
  let n1 = String(n).split("");
  n1.reverse().join("");
  n1.map((a) => {
    answer.push(Number(a));
  });
  return answer;
}
```

### 다른 풀이

- 숫자풀이

```javascript
function solution(n) {
  var arr = [];
  do {
    arr.push(n % 10);
    n = Math.floor(n / 10);
  } while (n > 0);
  return arr;
}
```
