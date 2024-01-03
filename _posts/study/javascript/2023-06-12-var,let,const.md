---
title: var, let, const 차이점
author: cotes
date: 2023-06-12 11:33:00 +0800
categories: [Study, Javascript]
tags: [favicon]
---

> ## 변수 선언 방식

### 1. var

```javascript
var name = "javascript";
console.log(name); // javascript

var name = "react";
console.log(name); // react
```

- **중복 선언 가능**: **var 로 선언한 변수**는 동일한 이름으로 **여러 번 중복해서 선언이 가능**하다. 이와 같은 경우, 마지막에 할당된 값이 변수에 저장된다. <br/>

  \* 이는 필요할 때마다 변수를 유연하게 사용할 수 있다는 장점이 될 수도 있지만, **기존에 선언해둔 변수의 존재를 잊고 값을 재할당하는 등의 실수가 발생할 가능성**이 크다.

  \* 특히 코드량이 많아졌을 때, 같은 이름의 변수명이 여러 번 선언되었다면 **어디 부분에서 문제가 발생하는지 파악하기 힘들뿐더러 값이 바뀔 우려가 있다.**

- **값 재할당 가능**

- 이를 보완하기 위해 **ES6부터 추가**된 변수 선언 방식이 **let 과 const**이다.

### 2. let

```javascript
let name = "javascript";
console.log(name); // javascript

let name = "react";
console.log(name);
// Uncaught SyntaxError: Identifier 'name' has already been declared

name = "vue";
console.log(name); // vue
```

- **중복 선언 불가능**: var 키워드와 다르게 **중복 선언을 할 경우** 해당 변수는 이미 정의되었다고 **에러가 출력**된다.

- **값 재할당 가능**: name = 'vue' 와 같이 변수 선언 및 초기화 이후 반복해서 다른 값을 **재할당 할 수는 있음.**

### 3. const

```javascript
const name = "javascript";
console.log(name); // javascript

const name = "react";
console.log(name);
// Uncaught SyntaxError: Identifier 'name' has already been declared

name = "vue";
console.log(name);
// Uncaught TypeError: Assignment to constant variable
```

- **중복 선언 불가능**: var 키워드와 다르게 **중복 선언을 할 경우** 해당 변수는 이미 정의되었다고 **에러가 출력**된다.

- **값 재할당 불가능**: **let 과 const 의 차이점**은 변수의 **immutable 여부**로 const는 상수, 고정값으로 사용되기 때문에 값을 **재할당 시 에러가 출력**된다.

<br/>

---

<br/>

> ## 차이점 1) 스코프

스코프는 **변수에 접근할 수 있는 범위로 var는 함수 스코프를 let, const는 블록 스코프**를 따른다.

_스코프의 자세한 내용은[스코프](https://hajung00.github.io/posts/스코프/) 게시글을 통해 확인 할 수 있다._

### var: 함수 스코프(function-level scope)

```javascript
function example() {
  var x = 10; // 함수 스코프 내에서 선언된 전역 변수
  if (true) {
    var y = 20; // 블록 내에서 선언된 전역 변수
  }
  console.log(x); // 10
  console.log(y); // 20
}

console.log(x); // Uncaught ReferenceError: x is not defined
```

var 키워드로 생성된 변수는 함수 스코프 내에서 전역 변수로 취급된다.

**함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다.**

즉, 함수 내부에서 선언한 변수는 지역 변수이고 함수 외부에서 선언한 변수는 모두 전역 변수로 취급된다.

### let, const: 블록 스코프(block-level scope)

```javascript
function func() {
  if (true) {
    let a = 5;
    console.log(a); // 5
  }
  console.log(a); // ReferenceError: a is not defined
}

console.log(a); // ReferenceError: a is not defined
```

함수, if문, for문, while문, try/catch문 등의 모든 코드 블록 ({...}) 내부에서 선언된 변수는 **코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다.**

즉, 코드 블록 내부에서 선언한 변수는 지역 변수로 취급된다.

<br/>

---

<br/>

> ## 차이점 2) 호이스팅

**함수 내부에 있는 선언들을 모두 끌어올려 해당 함수 유효 범위의 최상단에 선언**하는 것을 뜻한다.

즉, 스코프 안에 있는 선언들을 모두 **스코프의 최상위로 끌어올리는 것**이다.

_스코프의 자세한 내용은[호이스팅](https://hajung00.github.io/posts/호이스팅/) 게시글을 통해 확인 할 수 있다._

var, let, const 모두 호이스팅이 발생하지만 **var와 let,const는 다른방식으로 동작**된다.

### var

- 선언과 동시에 **undefined로 초기화 됨.**

\* **호이스팅 전**

```javascript
console.log(a); // undefined

var a = 5;
console.log(a); // 5
```

\* **호이스팅 후**

```javascript
var a; // undefined로 초기화
console.log(a); // undefined

a = 5;
console.log(a); // 5
```

### let, const

- 선언만 될뿐, **초기화가 이루어지지 않음.**

- 호이스팅이 일어나지만 초기화가 되지 않아 **let선언부 위는 TDZ**이 됨.

- const키워드는 선언과 동시에 초기화 되어야 하므로 **const선언부 위는 TDZ**이 됨.

<br/>

> **\* TDZ(Temporal Dead Zone)**<br/>
> let과 const로 선언한 변수는 호이스팅되면서 **해당 변수가 할당되기 전까지 일시적 사각지대(TDZ)에 머물게 된다.**<br/>
> TDZ 내에서 변수에 접근하려고 하면 ReferenceError가 발생한다.

\* 호이스팅 전

```javascript
console.log(a); //  ReferenceError: a is not defined

let a = 5;
```

\* 호이스팅 후

```javascript
let a; // undefined로 초기화되지 않음
console.log(a); //  ReferenceError: a is not defined

a = 5;
```

<br/>

최종적으로 정리한 표이다.

| 변수  | 재선언 | 재할당 | 유효범위    | 호이스팅           |
| ----- | ------ | ------ | ----------- | ------------------ |
| var   | 가능   | 가능   | 함수 스코프 | undefined로 초기화 |
| let   | 불가능 | 가능   | 블록 스코프 | 초기화 X           |
| const | 불가능 | 불가능 | 블록 스코프 | 초기화 X           |

<br/>

---

<br/>

> ## 📑 참고 자료

[[인간 JS 엔진 되기]-제로초 ](https://www.youtube.com/watch?v=eXQQipdastk&list=PLcqDmjxt30Rt9wmSlw1u6sBYr-aZmpNB3)

[var, let, const 차이점 ](https://80000coding.oopy.io/e1721710-536f-43f2-823b-663389f5fbfa)
