---
title: Prototype과 Class
author: cotes
date: 2023-11-27 11:33:00 +0800
categories: [Study, Javascript]
tags: [Javascript]
---

> ## Javascript와 Proptotype

**JavaScript 객체 지향 언어**이다.

객체지향언어로는 java, python, rudy등이 있는데 **객체지향에서 빠질 수 없는 개념은 class**다.

**Class를 통해 상속받아 코드를 재활용**할 수 있다.

<br/>

하지만 **javascript에는 클래스 개념이 존재하지 않아 prototype을 기반으로 상속을 흉내** 내도록 구현해 사용한다.

그래서 흔히 **JavaScript를 프로토 타입 기반 언어(prototype-basd language)**라 부르기도 한다.

<br/>

그렇다면 prototype은 무엇이며 어떻게 사용되는지 알아보자!

<br/>

```javascript
const fruits = new Array("Apple", "Banana", "Cherry");
console.log(fruits.length);
console.log(fruits.includes("Banana"));
```

위의 코드에서 new Array를 통해 배열을 생성하고 array.length, array.includes 등 속성이나 메소드를 사용하는데

이때의 **length, includes**를 **자바스크립트에서 prototype 속성, 메소드**라고 이야기한다.

<br/>

- mdn 문서

<img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/7e998516-25b0-4eaa-9820-e73504cefc74" width="80%" height="40%" alt="image"/>

mdn 문서를 통해 Array의 속성과 메서드 확인 결과 prototype이라는 속성이 중간에 들어가 있는 것을 확인할 수 있다.

prototype은 언제 어디서 생긴 것일까?

Prototype을 알아보기 전에 **javascript의 함수와 객체 내부 구조를 이해**해야 한다.

<br/>

---

<br/>

> ## 함수와 객체 내부 구조

- 함수와 객체 내부 구조

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/c5235961-25ab-4b80-9a44-af864a81666f)

```javascript
Function Person(){}
```

- Preson 함수를 정의하고 파싱하면 내부적으로 Person함수 멤버로 prototype 속성 생성됨

- 생성된 prototype 속성은 Person 프로토타입 객체를 참조함

- Person 프로토타입 객체의 멤버 constructor는 Person함수를 참조함

---

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/5bbe9dd1-929b-4cc1-9574-8289321dca3e)

```javascript
Function Person(){}
const joon = new Person();
const jisoo = new Person();
```

- 프로토타입 객체는 new 연산자와 Person 함수를 통해 생성된 모든 객체의 원형이 되는 객체

- new 연산자로 생성된 객체(joon, jisoo) 안에는 proto 속성이 있다.

- \_\_proto\_\_속성은 객체가 만들어지기 위해 사용된 원형인 프로토타입 객체를 숨은 링크로 참조하는 역할을 하며 이 링크를 프로토타입이라고 정의한다.

  <br/>

---

<br/>

> ## 프로토타입 객체란?

함수를 정의하면 생성되는 객체로 자신이 다른 객체의 원형이 되는 객체를 말한다.

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/641ed741-1936-4511-80b1-e50980d4c45c)

```javascript
Function Person(){}
const joon = new Person();
const jisoo = new Person()

Person.prototype.getType = function (){
Return '사람';
}

console.log(joon.getType()) // 인간
console.log(jisoo.getType()) // 인간
```

- 모든 객체는 프로토타입 객체에 접근 할 수 있다.

- 프로토타입 객체에 동적으로 런타임에 멤버를 추가할 수 있다.

- 같은 원형(Personc 프로토타입 객체)을 복사로 생성된 모든 객체(joon, jisoo)는 추가된 멤버(getType)를 사용할 수 있다.

<br/>

---

<br/>

> ## Prototype이란?

프로토타입은 크게 두가지로 해석된다.

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/64086fab-afb2-4af3-8f73-c77a9426044a)

1. 함수의 멤버인 protyotype속성: 프로토타입 객체 참조하는 속성

2. 함수와 new연산자로 생성한 객체의 **proto** 속성: 자신을 만들어낸 원형인 프로토타입 객체를 참조하는 숨겨진 링크로써 프로토타입

<br/>

---

<br/>

> ## 코드의 재사용

**Javascript는 객체를 상속**하기 위해서 **객체 원형인 프로토타입이라는 방식**을 사용한다.

**프로토타입 링크를 사용자가 정의한 객체에 링크가 참조**되도록 설정하면 **코드의 재사용과 객체 지향적인 프로그래밍**을 할 수 있다.

**ES6부터 class라는 문법이 추가**되면서 객체 생성자로 구현한 코드를 좀 더 명확하고 깔끔하게 구현 할 수 있게 해준다.

<br/>

### prototype vs class

상황 a. Animal 의 기능을 재사용하여 Cat 과 Dog 라는 새로운 객체 생성자를 만든다고 가정

#### 1. prototype

- prototype으로 속성, 메서드 정의

- prototype을 공유해야 하기 때문에 Dog, Cat prototype값을 Animal.prototype으로 설정

```javascript
function Animal(type, name, sound) {
  this.type = type;
  this.name = name;
  this.sound = sound;
}

Animal.prototype.say = function () {
  console.log(this.sound);
};
Animal.prototype.sharedValue = 1;

function Dog(name, sound) {
  Animal.call(this, "개", name, sound);
}
Dog.prototype = Animal.prototype;

function Cat(name, sound) {
  Animal.call(this, "고양이", name, sound);
}
Cat.prototype = Animal.prototype;

const dog = new Dog("멍멍이", "멍멍");
const cat = new Cat("야옹이", "야옹");

dog.say();
cat.say();
```

---

#### 2. class

- 클래스 내부에 메서드를 생성하면 자동으로 prototype으로 등록

- extends 키워드로 상속

```javascript
class Animal {
  constructor(type, name, sound) {
    this.type = type;
    this.name = name;
    this.sound = sound;
  }
  say() {
    console.log(this.sound);
  }
}

class Dog extends Animal {
  constructor(name, sound) {
    super("개", name, sound);
  }
}

class Cat extends Animal {
  constructor(name, sound) {
    super("고양이", name, sound);
  }
}

const dog = new Dog("멍멍이", "멍멍");
const cat = new Cat("야옹이", "야옹");

dog.say();
cat.say();
```

<br/>

---

<br/>

> ## 📑 참고자료

[[JS] Prototype이란?](https://velog.io/@turtle601/Prototype%EC%9D%B4%EB%9E%80)

[Javascript: 프로토타입(prototype) 이해](https://www.nextree.co.kr/p7323/)
