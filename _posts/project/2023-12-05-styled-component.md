---
title: Next.js13 + styled-components_에러 및 해결
author: cotes
date: 2023-11-02 20:55:00 +0800
categories: [Project, PT 예약 사이트]
tags: [Next.js, styled-components, 에러 해결]
---

> ## 에러 상황

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/861e1fc4-ceb5-4ce7-860d-7435e7f9be52)

처음에는 서버와 클라이언트의 클래스명이 다르다고 나와서 핫 리로딩이 적용되지 않아 발생하는 문제인 줄 알았지만 next.js 13에서 emotion 관련된 해결이 안된 것으로 보인다.

> 핫 리로딩(hot reloading)<br/>
> 앱을 처음부터 다시 시작하지 않고 새로운 코드 변경에 따른 코드 변경사항만 표시하며 변경된 코드에만 적용된다.

<br/>

---

<br/>

> ## 원인

Next 공식문서를 보니까 styled-component는 어느정도 해결이 되어 있으므로 emotion과 매우 유사한 방식으로 개선하고 싶다면 styled component를 사용하는 것을 추천한다하여 styled-component로 변경하였고 next에서도 이와 같은 에러가 발생할 수 있어 styled-component 설정도 해주었다.

<br/>

그렇다면 next에서 스타일 적용 시, 왜 이런 에러가 발생할까?

<br/>

Next.js는 첫 페이지 로드가 SSR로 동작하기 때문에, 서버에서 생성된 컴포넌트와 CSR로 클라이언트에서 생성된 컴포넌트의 클래스명이 서로 달라지게 된다.

해결 방법은 두 가지가 있는데, 아래 두가지 방법 중 하나로 해결하면 됩니다. 만일 사용중인 Next.js가 최신 버전이라면 두번째 방법을 사용하는 것을 추천한다.

> 두 가지 방법을 동시에 적용하면 바벨이 충돌나서 제대로 작동하지 않는다.

<br/>

---

<br/>

> ## 해결 방법

### 방법 1. babel-plugin-styled-components

#### 1. 설치

```
 $ npm I –D babel-plugin-styled-components
```

#### 2. babelrc 설정

프로젝트 루트에 .babelrc를 생성한 뒤 설정을 추가하고, 서버를 재실행합니다.

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

> ## 해결되지 않는 문제

![ezgif com-video-to-gif](https://github.com/hajung00/hajung00.github.io/assets/66300154/f5aa80fe-53f0-42b8-af7e-b1a23647da2b)

styled-component를 next.config로 ssr 설정을 해주었지만 적용이 되지 않고 깜빡임이 발생되었다.

스타일이 적용되지 않은 html이 랜더링 된 후 자바스크립트에서 선언되는 styled-components의 스타일이 뒤늦게 로드되어 깜빡임이 발생하였다.

<br/>

---

<br/>

> ## 해결

공식 문서를 확인 후 해결하였다.

### 1. root에서 lib 폴더를 생성

```javascript
// lib/registry.tsx
"use client";

import React, { useState } from "react";
import { useServerInsertedHTML } from "next/navigation";
import { ServerStyleSheet, StyleSheetManager } from "styled-components";

export default function StyledComponentsRegistry({
  children
}: {
  children: React.ReactNode
}) {
  // Only create stylesheet once with lazy initial state
  // x-ref: https://reactjs.org/docs/hooks-reference.html#lazy-initial-state
  const [styledComponentsStyleSheet] = useState(() => new ServerStyleSheet());

  useServerInsertedHTML(() => {
    const styles = styledComponentsStyleSheet.getStyleElement();
    styledComponentsStyleSheet.instance.clearTag();
    return <>{styles}</>;
  });

  if (typeof window !== "undefined") return <>{children}</>;

  return (
    <StyleSheetManager sheet={styledComponentsStyleSheet.instance}>
      {children}
    </StyleSheetManager>
  );
}
```

### 2. app/layout에 lib/registry 세팅

```javascript
// app / layout.tsx;
import StyledComponentsRegistry from "./lib/registry";

export default function RootLayout({
  children
}: {
  children: React.ReactNode
}) {
  return (
    <html>
      <body>
        <StyledComponentsRegistry>{children}</StyledComponentsRegistry>
      </body>
    </html>
  );
}
```

> ## 📑 참고 자료

[[Next.js] Next.js에서 Prop `className` did not match 경고가 뜨는 이유](https://tesseractjh.tistory.com/164)

[(Next.js + styled-components) Props `classname did not match` 에러 및 해결](https://velog.io/@sukyoungshin/Next.js-styled-components-Props-classname-did-not-match-%EC%97%90%EB%9F%AC)

[[classpick 개발일지 #1] Next.js13으로 마이그레이션하기 - emotion 써도 되나? 고민하기](https://velog.io/@gene028/classpick-%EA%B0%9C%EB%B0%9C%EC%9D%BC%EC%A7%80-1-Next.js13%EC%9C%BC%EB%A1%9C-%EB%A7%88%EC%9D%B4%EA%B7%B8%EB%A0%88%EC%9D%B4%EC%85%98%ED%95%98%EA%B8%B0-emotion%EC%9D%B4-%EC%95%88%EB%90%9C%EB%8B%A4%EA%B3%A0)

[Next공식문서](https://nextjs.org/docs/app/building-your-application/styling/css-in-js#styled-components)
