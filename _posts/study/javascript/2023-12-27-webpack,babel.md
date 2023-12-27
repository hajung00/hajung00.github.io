---
title: 자바스크립트 개발환경 - Webpack과 Babel
author: cotes
date: 2023-12-27 01:33:00 +0800
categories: [Study, Javascript]
tags: [Javascript, Webpack, Babel]
---

리액트를 사용해서 개발을 하고 나면 npm build 입력 한 방에(CRA 기준) 빌드가 완료가 된다.

리액트 개발에서 대표적인 라이브러리인 **create-react-app은 개발 환경부터 빌드**까지 모든 것이 다 갖추어져 있기 때문이다.

리액트를 설치할 때 내부에서 이미 웹팩을 사용해서 Development Environment 개발 환경을 생성하기 때문에 리액트를 사용할 때 아무런 설정없이 다른 파일에 있는 함수를 import 하고 이미지를 사용할수 있고 CSS 그리고 소스 코드를 적용하면 바로 반영이 되는 등의 효과를 가져올수 있다.

<br/>

하지만 **프론트엔드 개발자**라면 그 뒷단의 이야기에 대해서 알 필요가 있다. 자신의 프로젝트를 온전히 커스터마이징 하고 싶다면, CRA를 이용하지않고 **환경 설정부터 빌드 세팅**할 줄 알아야 한다.

그 중에서도 **모듈 번들러(Webpack)와 자바스크립트 트랜스파일러(Babel)**에 대해서 알아보았다.

<br/>

> ## 웹팩(Webpack)

[웹팩 공식 문서](https://webpack.js.org/)

웹팩 공식문서에서는 웹팩을 **자바스크립트 애플리케이션을 위한 정적 모듈 번들러**라고 소개하고 있다.

> - **번들러(bundler)**: **의존성이 있는 모듈**들을 하나의 파일로 **통합**시켜주는 도구

<br/>

그렇다면 여러 개의 파일을 하나로 모아주는 것은 왜 필요할까?

```
<html>
  <body>
    <script src="./index.js" />
    <script src="./app.js" />
    <script src="./src/Card.js" />
    ...
  </body>
</html>
```

이전에 모듈 번들러가 없을 때는 위의 코드처럼 **HTML에 모든 스크립트 파일**을 불러왔어야 했다.

<br/>

**ES6**부터 **import, export 구문**이 나오면서 원하는 변수, 함수만 들고오기 쉬워졌다.

```
<html>
  <body>
    <script src="./index.js" type="module" />
  </body>
</html>
```

이 때부터 **script 태그에 type="module"**으로 설정할 수 있게 되면서 **모듈 형태의 자바스크립트를 HTML에서 불러올 수 있게** 되었다.

이제 index.js 하나의 모듈만 불러오면 index.js에서 import하고 있는 다른 모듈들은 자동적으로 불러올 수 있다.

<br/>

하지만 위와 같은 것들은 아직 모든 브라우저에서 지원을 하지 않기 때문에 **모든 파일들을 하나의 모듈로 모아줄 소프트웨어**가 필요했고, 그래서 나온 것이 바로 **웹팩**이다.

![image](https://github.com/hajung00/Algorithm/assets/66300154/e36124a5-f295-49f5-b1e8-ecef4f4dabf2)

**웹팩은 하나의 진입점으로부터 의존하고 있는 파일들을 전부 모아서 하나의 파일로 만들어준다.**

웹팩과 항상 따라오는 개념은 **바벨(Babel)**이다. 웹팩으로 파일을 번들링(bundling)할 때도 바벨을 사용할 수 있도록 babel-loader를 추가한다.

그렇다면 **babel이 무엇인지 왜 필요한지**에 대해 먼저 알아보고 웹팩의 설정에 대해 알아보겠다.

<br/>

---

<br/>

> ## 바벨(Babel)

[바벨 공식 문서](https://babeljs.io/)

바벨 공식문서에서는 바벨을 **자바스크립트 컴파일러**라고 소개하며 최신 자바스크립트 문법을 사용할 수 있다고 나와있다.

최신 자바스크립트 문법을 지원하지 않는 브라우저들을 위해서 **최신 자바스크립트 문법을 구형 브라우저에서도 돌아갈 수 있게 변환** 시켜주는 라이브러리이다.

<br/>

**그렇다면 왜 바벨이 필요하고 바벨이 있어야만 자바스크립트의 최신 문법을 사용할 수 있을까?**

<br/>

그 이유는 **자바스크립트가 실행되는 환경**때문이다. 다른 언어와 달리 자바스크립트는 정말 많은 환경에서 실행된다.

웹 브라우저, NodeJS, Deno 등에서 실행되는데 웹 브라우저 또한 각자 다른 자바스크립트 엔진을 통해 자바스크립트 코드를 읽게 된다.

게다가 이렇게 **실행되는 환경의 버전에도 자바스크립트는 영향**을 받는다. 특정 버전 이상에서만 실행되는 코드가 있고 특정 브라우저에서는 실행되지 않는 코드도 있다.

그렇기 때문에 **모든 자바스크립트 실행 환경에서 정상적으로 동작할 수 있도록 하려면 바벨이 필요**하다.

<br/>

실제로 바벨 공식 페이지에 접속해 보면 바벨이 내 최신 코드들을 어떤식으로 변환해 주는지 쉽게 확인할 수 있다.
![image](https://github.com/hajung00/Algorithm/assets/66300154/13d958a6-ec07-4c0e-9fa2-fe2c0ea0b000)

<br/>

---

<br/>

> ## 바벨 설치

```
 $ npm  i -D @babel/core @babel/cli @babel/preset-env
```

각 모듈은 다음과 같은 역할을 한다.

- @babel/core: 바벨의 핵심 기능들을 포함
- @babel/cli: 터미널에서 바벨 명령어를 사용할 수 있게 도와줌
- @babel/preset-env: 함께 사용되어야 하는 Babel 플러그인을 모아 둔 것

<br/>

모듈 설치가 끝났으면 프로젝트 루트에 **.babelrc 파일**을 생성하고 아래와 같이 작성한다.

```javascript
// .babelrc
{
  "presets": ["@babel/preset-env"]
}
```

<br/>

**추가적으로 내가 사용하고자 하는 최신 문법**이 @babel/preset-env에서 지원하지 않으면 바벨 공식문서를 보고 **해당 플러그인을 설치 및 적용**해야한다.

```
 $ npm  i -D @babel/plugin-proposal-optional-chaining
```

```javascript
// .babelrc
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-optional-chaining"]
}
```

<br/>

---

<br/>

> ## 웹팩 설치 및 설정

먼저 웹팩을 설치하기 전에 위치를 return하는 함수 getRandomAddress를 rendomAddress.js파일에 작성하고 이 함수를 index.js에서 import하여 실행시키면 어떤일이 일어나는지 살펴보겠다.

📚 파일 구조<br/>
src<br/> - index.js<br/> - rendomAddress.js

<br/>

```javascript
// randomAddress.js

function getRandomAddress() {
  return "서울";
}
```

```javascript
// index.js

import getRandomAddress from "./rendomAddress";

console.log(getRandomAddress());
```

<img src="https://github.com/hajung00/SidePJ-next-node-full-sns/assets/66300154/4adde4e3-26ac-4caa-8ba0-133b28fad92c" width="80%" height="50%" alt="image"/>

**모듈 밖에서는 import를 사용할 수 없다는 에러**가 나온다. 이를 **webpack을 이용해 해결** 할 수 있다.

---

### Webpack 파일 생성

```
 $ npm i -D webpack webpack-cli
```

```javascript
// webpack.config.js

const path = require("path");
const prod = process.env.NODE_ENV === "production";

module.exports = {
  // 모드에 따라 웹팩에서 내장 최적화 제공
  mode: prod ? "production" : "development",

  // 번들링을 시작할 파일
  entry: {
    index: "./src/index.js"
  }

  output:{
    path: path.resolve(__dirname, 'dist')
    filename:'main.js',
  }
};
```

- mode: production, development, none 세가지 옵션을 사용할 수 있는데 사용한 옵션에 따라 웹팩에서 내부적으로 최적화를 해준다. 보통 개발시에는 development, 배포시에는 production을 사용
- entry: 번들링을 시작할 파일을 결정
- output: 웹팩을 통해 최종적으로 번들링 된 파일을 저장할 위치를 설정

<br/>

```javascript
"scripts": {
    "build": "NODE_ENV=production webpack --mode production --env=build"
  }
```

npm run build를 하면 src/index.js 파일을 번들링하고 번들링된 파일은 dist 폴더의 main.js에 저장된다.

---

### Webpack loader

#### 1. style loader

웹팩이 웹 애플리케이션을 해석할 때 **자바스크립트 파일이 아닌 웹 자원(HTML, CSS, Images, 폰트 등)들을 변환**할 수 있도록 도와주는 속성

<br/>

**Styling**

\* **css-loader**: @import 및 url()을 import/require()와 같이 해석하고 해결
![image](https://github.com/hajung00/SidePJ-next-node-full-sns/assets/66300154/1b0a2124-a15f-4455-aca2-8e7cd3231236)

\* **style-loader**: css-loader를 이용해서 import 구문을 이용해 js 파일에 불러들여진 스타일 파일이 html 파일 안에 style 태그로 존재할 수 있게 한다.

<br/>

```
 $ npm i -D style-loader css-loader
```

```javascript
// webpack.config.js

module.exports = {
  // 생략
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: ["style-loader", "css-loader"]
      }
    ]
  }
};
```

- test: 로더를 적용할 파일 유형 (일반적으로 정규 표현식 사용)
- use: 해당 파일에 적용할 로더의 이름으로 배열 안에 있는 값들의 역순으로 로더들이 작동

#### 2. babel loader

**웹팩으로 파일을 번들링(bundling)할 때도 바벨을 사용할수 있게** 해주는 것이 babel-loader이다.

바벨만 따로 사용할 수도 있지만 **웹팩과 연결**하면 **바벨이 코드들을 트랜스파일링 하면서 웹팩을 통해 모듈들을 번들링할 수 있기 때문에 굉장히 효율적**이다.

```
 $ npm i -D babel-loader @babel/core @babel/preset-env
```

```javascript
// webpack.config.js

module.exports = {
  // 생략
  module: {
    rules: [
      {
        test: /\.js$/,
        // 제외할 파일들
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"]
          }
        }
      }
    ]
  }
};
```

---

### Webpack plugin

plugin은 웹팩의 기본적인 동작에 **추가적인 기능을 제공**하는 속성이다.

웹팩은 로더와 플러그인의 확장 기능이 있고 웹팩의 플러그인은 로더가 할 수 없는 다른 작업을 수행할 목적으로 제공된다.

**로더는 모듈을 output으로 만들어가는 과정**에서 사용한다면 **플러그인은 webpack으로 변환한 파일에 추가적인 기능**을 더하고 싶을 때 사용한다.

<br/>

웹팩이 html 파일을 읽어서 html 파일을 빌드할 수 있게 하기 위해 **HTML Webpack Plugin 적용**해 보았다.

```

$ npm i -D html-webpack-plugin

```

```javascript
// webpack.config.js

module.exports = {
  // 생략
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: "src/index.html"
    })
  ]
};
```

- filename: 번들링된 html 파일
- template: 번들링을 시작할 html 파일

<br/>

npm run build를 하면 template에 있는 src/index.html 에 있는 소스코드가 filename에 있는 dist/index.html 로 만들어진다.

---

### Webpack development server

Webpack에서는 빠르게 개발할 수 있도록 개발서버 제공

```javascript
// webpack.config.js

const path = require("path");

module.exports = {
  // 생략
  devServer: {
    static:{
        directory:path.join(__dirname,'dist')
    }
    compress:true,
    port:3000,
    open:true,
  }
};
```

- static: 개발 서버에 실행시킬 대상
- compress: gzip 활성화 여부
- port: 포트 설정
- open: 서버가 시작된 후 브라우저를 열 것인가에 대한 설정

<br/>

```javascript
"scripts": {
    "dev": "webpack server",
    "build": "NODE_ENV=production webpack --mode production --env=build"
  }
```

npm run dev로 개발 서버 실행하게 되면 번들링된 파일의 폴더인 dist를 3000포트에서 실행하며 서버가 시작된 후 브라우저가 열린다.

<br/>

- gzip 압축

**압축은 대역폭을 절약하고 사이트 속도를 높이는 간단하고 효과적인 방법**이다.

원래는 구형 브라우저의 문제 때문에 자바스크립트 속도를 높일 때 gzip 압축을 권장하기가 힘들었지만 이제는 대부분 신형 브라우저를 사용하기 때문에 gzip 압축을 사용한다.

| case 1. gzip 압축 X                                                                                                  | case 2. gzip 압축 O                                                                                                  |
| -------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| ![image](https://github.com/hajung00/SidePJ-next-node-full-sns/assets/66300154/a51967b5-ea5a-41ef-a623-08da65262263) | ![image](https://github.com/hajung00/SidePJ-next-node-full-sns/assets/66300154/b6f19c1e-9c08-4362-809c-13a9d9dc0513) |
| \* 시스템은 작동하지만 100KB는 많은 텍스트 <br/> \* HTML 모든 태그의 닫는 태그 반복                                  | \* 대역폭과 다운로드 시간을 절약 <br> \* 페이지가 빠르게 로드되어 사용자에게 보여준다.                               |

---

### Webpack devtool

이 옵션은 **소스 맵(source map)이 생성되는지 여부와 생성 방법**을 제어한다.

- 소스 맵(Source Map)

<img src="https://github.com/hajung00/SidePJ-next-node-full-sns/assets/66300154/20f13849-61ae-43f8-a3c0-4a011219c431" width="60%" height="50%" alt="image"/>

압축 파일 내의 코드를 소스 파일의 원래 위치로 다시 매핑하는 방법을 제공한다. Chrome 및 Firefox 개발자 도구는 모두 소스 맵에 대한 기본 제공 지원과 함께 제공된다.

```javascript
// webpack.config.js

const prod = process.env.NODE_ENV === "production";

module.exports = {
  // 생략
  // 소스 맵 생성 여부 및 방법 설정
  devtool: prod ? "hidden-soure-map" : "eval"
};
```

npm run build 시, 소스 맵도 dist 파일에 생성된다.

| case 1. 소스 맵 적용 전                                                                                              | case 2. 소스 맵 적용 후                                                                                              |
| -------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| ![image](https://github.com/hajung00/SidePJ-next-node-full-sns/assets/66300154/661b13fd-be92-4fa9-a530-28b849d019d0) | ![image](https://github.com/hajung00/SidePJ-next-node-full-sns/assets/66300154/1094c330-d68f-422b-b836-fffbc6cda421) |

case 1처럼 웹팩으로 빌드된 파일은 난독화 되어있기 때문에 코드를 디버그해야 하는 경우 굉장히 어렵다.

**소스맵을 이용해 우리가 알아볼 수 있고 디버그 할 수 있도록 해준다.**

Webpack devtool의 다양한 옵션은 [Webpack devtool options](https://webpack.kr/configuration/devtool/#root)에서 확인할 수 있다.

<br/>

---

<br/>

> ## 📑 참고 자료

[자바스크립트 개발환경: 웹팩와 바벨](https://chanyeong.com/blog/post/27)

[Webpack 공식 문서](https://webpack.kr/configuration/devtool)

[패스트 캠퍼스: 프론트엔드 웹 개발의 모든 것 초격차 패키지](https://fastcampus.co.kr/)
