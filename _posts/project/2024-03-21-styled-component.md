---
title: Next.js13 + styled-components_에러 및 해결
author: cotes
date: 2024-03-21 20:55:00 +0800
categories: [Project, Americanote]
tags: [Next.js, styled-components, 에러 해결]
---

> ## 에러 상황

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/0977d4df-367d-4689-9020-c69db370653f)

에러 설명을 읽어보니 서버와 클라이언트의 클래스명이 다르다고 나왔다.

검색을 해보니 Next.js에서 styled-components를 사용하면 위와 같은 경고가 뜨곤 한다고 했다.

<br/>

---

<br/>

> ## 원인

그렇다면 next에서 스타일 적용 시, 왜 이런 에러가 발생할까?

Next.js는 첫 페이지 로드가 SSR로 동작하기 때문에, 서버에서 생성된 컴포넌트와 CSR로 클라이언트에서 생성된 컴포넌트의 클래스명이 서로 달라지게 된다.

해결 방법은 두 가지가 있는데, 아래 두가지 방법 중 하나로 해결하면 된다. 만일 사용중인 Next.js가 최신 버전이라면 두번째 방법을 사용하는 것을 추천한다.

- 두 가지 방법을 동시에 적용하면 바벨이 충돌나서 제대로 작동하지 않는다.

<br/>

---

<br/>

> ## 해결 방법

### 방법 1. babel-plugin-styled-components

환경에 따라 달라지는 className을 일관되게 해주는 것이 바로 babel-plugin-styled-components이다.

#### 1. 설치

```
 $ npm I –D babel-plugin-styled-components
```

#### 2. babelrc 설정

프로젝트 루트에 .babelrc를 생성한 뒤 설정을 추가하고, 서버를 재실행한다.

```javascript
// .babelrc
{
  "presets": ["next/babel"], // -> 이거 설정안하면 빌드안됨
  "plugins": [
    [
      "babel-plugin-styled-components",
      {
        "ssr": false
      }
    ]
  ]
}
```

#### 3. \_document.tsx

Next.js에서 styled-components를 사용할 때 \_document를 따로 설정해서 SSR될 때 CSS가 head에 주입되도록 해야 한다. 만약 따로 설정하지 않는다면, styled-components가 적용되지 않은 상태로 렌더링될 수 있다.

```javascript
// pages/_document.tsx
import Document, { DocumentContext, DocumentInitialProps } from "next/document";
import { ServerStyleSheet } from "styled-components";

export default class MyDocument extends Document {
  static async getInitialProps(
    ctx: DocumentContext
  ): Promise<DocumentInitialProps> {
    const sheet = new ServerStyleSheet();
    const originalRenderPage = ctx.renderPage;

    try {
      ctx.renderPage = () =>
        originalRenderPage({
          enhanceApp: (App) => (props) =>
            sheet.collectStyles(<App {...props} />)
        });

      const initialProps = await Document.getInitialProps(ctx);
      return {
        ...initialProps,
        styles: [
          <>
            {initialProps.styles}
            {sheet.getStyleElement()}
          </>
        ]
      };
    } finally {
      sheet.seal();
    }
  }
}
```

---

### 방법 2. next.config.js 옵션 설정

Next.js 최신 버전에서는 styled-components의 ssr를 잘 지원해주므로, Next.js의 컴파일러 옵션만으로 간단하게 해결할 수 있다.

```javascript
// next.config.js
module.exports = {
  compiler: {
    // ssr and displayName are configured by default
    styledComponents: true
  }
};
```

<br/>

---

<br/>

> ## 📑 참고 자료

[[Next.js] Next.js에서 Prop `className` did not match 경고가 뜨는 이유](https://tesseractjh.tistory.com/164)

[(Next.js + styled-components) Props `classname did not match` 에러 및 해결](https://velog.io/@sukyoungshin/Next.js-styled-components-Props-classname-did-not-match-%EC%97%90%EB%9F%AC)

[[classpick 개발일지 #1] Next.js13으로 마이그레이션하기 - emotion 써도 되나? 고민하기](https://velog.io/@gene028/classpick-%EA%B0%9C%EB%B0%9C%EC%9D%BC%EC%A7%80-1-Next.js13%EC%9C%BC%EB%A1%9C-%EB%A7%88%EC%9D%B4%EA%B7%B8%EB%A0%88%EC%9D%B4%EC%85%98%ED%95%98%EA%B8%B0-emotion%EC%9D%B4-%EC%95%88%EB%90%9C%EB%8B%A4%EA%B3%A0)

[Next공식문서](https://nextjs.org/docs/app/building-your-application/styling/css-in-js#styled-components)
