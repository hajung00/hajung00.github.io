---
title: function과 arrow function
author: cotes
date: 2023-06-16 11:33:00 +0800
categories: [Study, Javascript]
tags: [favicon]
---

> ## arrow function이란?

자바스크립트 **ES6**부터 등장한 **함수 표현식**이다.

먼저 자바스크립트에서 함수를 정의하는 방법, **함수 선언식과 함수 표현식**에 대해 알아보았다.

### 함수 정의

#### 1. 함수 선언식

**function** 키워드

```javascript
function add(a, b) {
  return a + b;
}

add(1 + 3);
```

<br/>

#### 2. 함수 표현식

**일반 함수 표현식**: 정의한 function을 별도의 변수에 할당

```javascript
var add = function (x, y) {
  return x + y;
};

add(1 + 3);
```

**arrow function**

```javascript
var add = (x, y) => x + y;
```

<br/>

화살표 함수는 보다 간결하고 짧은 문법을 제공하며, 함수를 변수에 할당하거나 다른 함수의 인수로 전달하는 등의 일반적인 함수 표현식을 대체할 수 있다.

하지만 **function과 용도가 완전 같지 않기 때문에 일반 function을 항상 대체할 수 있는 문법은 아니다.**

그렇다면 어떤점에서 차이가 있는지 알아보겠다.

<br/>

---

<br/>

> ## function과 arrow function 차이점

### 1. this 바인딩

#### 일반 함수(function)

자바스크립트에서 모든 함수는 실행될 때마다 함수 내부에 this라는 객체가 추가된다.

<br/>

\* 일반 함수에서의 this 바인딩

1. 함수 실행시에는 전역(window) 객체를 가리킨다.
2. 메소드 실행시에는 메소드를 소유하고 있는 객체를 가리킨다.
3. 생성자 실행시에는 새롭게 만들어진 객체를 가리킨다.

<br/>

**일반 함수**는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, **함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정**된다.

```javascript
var obj = {
  fn: function () {
    console.log(this);
  }
};

obj.fn();
```

**this는 함수가 호출될 때** 정해지므로 **fn을 호출한 객체 obj**를 가리키게 되고 {fn: ƒ}가 출력된다.

<br/>

#### 화살표 함수(arrow function)

**화살표 함수**는 함수를 선언할 때 **this에 바인딩할 객체가 정적으로 결정**된다.

화살표 함수의 **this는 언제나 상위 스코프의 this**를 가리킨다.

또한 call, apply, bind 메소드를 사용하여 this를 변경할 수 없다.

```javascript
function fnUser() {
  this.firstName = "Neo";
  this.lastName = "Anderson";

  return {
    firstName: "Heropy",
    lastName: "Part",
    age: 85,
    getFullName: function () {
      return `${this.firstName} ${this.lastName}`;
    }
  };
}

function arrUser() {
  this.firstName = "Neo";
  this.lastName = "Anderson";

  return {
    firstName: "Heropy",
    lastName: "Part",
    age: 85,
    getFullName: () => {
      return `${this.firstName} ${this.lastName}`;
    }
  };
}

const user1 = new fnUser();
console.log(user1.getFullName()); // Heropy Part

const user2 = new arrUser();
console.log(user2.getFullName()); // Neo Anderson
```

**일반 함수(fnUser)에서의 this**는 **getFullName함수가 호출 될 때 결정**되어 "Heropy Part"가 출력되는 것을 볼 수 있다.
**
반면에 **화살표 함수(arrUser)의 this**는 언제나 상위 스코프의 this를 가리키기 때문에 **함수가 정의될 때 결정되며 호출될 때 변경되지 않는다.\*\*

<br/>

즉, **일반 함수**는 this에 바인딩할 객체가 함수를 호출할 때 **this에 바인딩할 객체가 동적으로 결정**되고,

**화살표 함수**는 항상 화살표 함수가 정의된 **상위 스코프의 this를 상속**받으며 이는 **함수가 정의될 때 결정되며 호출될 때 변경되지 않는다.**

<br/>

⚠️ 여기서 잠깐!

함수 표현식이 모두 화살표 함수와 같은 this 바인딩을 한다고 생각 할 수 있는데 이는 잘못된 생각이다.

```javascript
const arrowFunction = () => {
  console.log(this);
};

const regularFunction = function () {
  console.log(this);
};

const obj = {
  arrow: arrowFunction,
  regular: regularFunction
};

obj.arrow(); // Window객체
obj.regular(); // {arrow: ƒ, regular: ƒ}
```

일반 함수 표현식(regularFunction)의 this는 객체 obj를 가리키고 화살표 함수 내에서의 this는 상위 스코프인 전역 객체를 가리킨다.

**함수 표현식은 함수가 호출되는 컨텍스트에 따라 this 바인딩이 달라지며, 화살표 함수는 항상 정적인 this 바인딩을 가진다.**

---

### 2. 생성자 함수로 사용 가능 여부

#### 일반 함수(function)

**생성자 함수로 사용할 수 있다.** 일반 함수는 생성자로 사용할 수 있으며, new 키워드를 사용하여 인스턴스를 생성할 수 있다.

#### 화살표 함수(arrow function)

**생성자로 사용할 수 없으며,** new와 함께 사용할 경우 에러가 발생한다. 이는 **prototype 프로퍼티를 가지고 있지 않기 때문**이다.

```javascript
function fun() {
  this.num = 1234;
}
const arrFun = () => {
  this.num = 1234;
};

const funA = new fun();
console.log(funA.num); // 1234

const funB = new arrFun(); // Error
```

---

### 3. arguments 사용 가능 여부

#### 일반 함수(function)

일반 함수 내에서 **arguments 객체를 사용하여 모든 인수에 접근**할 수 있다.

#### 화살표 함수 (arrow function)

화살표 함수는 자체적인 **arguments 객체를 가지지 않다.** 대신, 외부 스코프의 arguments를 참조한다.

```javascript
function fun() {
  console.log(arguments); // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
}

const arrFun = () => {
  console.log(arguments); // Uncaught ReferenceError: arguments is not defined
};

fun(1, 2, 3);
arrFun(1, 2, 3);
```

일반함수는 arguments변수가 전달되어 [1,2,3]이 찍히지만 화살표함수는 arguments를 정의할 수 없다고 에러 메세지가 출력된다.

<br/>

---

<br/>

> ## 📑 참고 자료

[[인간 JS 엔진 되기]-제로초 ](https://www.youtube.com/watch?v=eXQQipdastk&list=PLcqDmjxt30Rt9wmSlw1u6sBYr-aZmpNB3)

[[JavaScript] 함수 선언식 vs 함수 표현식](https://jsmokblog.tistory.com/27)

[JavaScript - 화살표 함수와 일반 함수의 차이](https://hhyemi.github.io/2021/06/09/arrow.html)
