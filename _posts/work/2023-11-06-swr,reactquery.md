---
title: SWR과 React-Query 비교하기
author: cotes
date: 2023-11-06 00:34:00 +0800
categories: [Work-Devlog, 뇌파 분석 시스템]
tags: [Work-Devlog, SWR, React-Query]
---

<!-- 프로젝트 작업하면서 했던 고민, 어떻게 해결했는지에 대한 내용이 담겨져있습니다. -->

> ### SWR과 React-Query

**SWR**

먼저 캐시에서 데이터를 반환한 다음, 서버에 데이터를 가져오는 요청을 보내고, 마지막으로 최신 데이터를 제공하는 전략이다.

**React-Query**

리액트 애플리케이션에 서버 상태를 가져오고, 캐싱하고, 동기화하고, 업데이트하는 것을 쉽게 해준다.

<br/>

---

<br/>

> ### 사용 목적

서버로부터 받아오는 데이터를 관리하기 위해 **서버 상태 관리를 위한 라이브러리**를 찾아보았다.

검색 결과 대부분 **redux에서 swr이나 react-query로 바꿔가거나 같이 사용하는 추세**임을 확인하였다.

Redux의 불편함을 개인 프로젝트 시에도 느꼈기에 swr이나 react-query를 도입해보고자 한다.

_Redux에 대한 내용은 [Redux의 동작원리 및 문제점](https://hajung00.github.io/posts/redux/) 게시글을 통해 확인할 수 있습니다._

<br/>

---

<br/>

> ### 사용 방법

- next.js 기준으로 swr과 react-query의 사용법을 알아보았다.

#### 1. SWR

- **key를 url로써 fetcher에 넘겨주는 방식**

```javascript
import useSWR from "swr";

const fetcher = (url: string) => axios.get(url).then((res) => res.data);
const { data, isValidating, error, mutate } = useSWR(`/api/allassets`, fetcher);
```

#### 2. React-Query

- **data fetching을 직접 구현한 fetcher**을 넘겨받는다.

- **Provider을 제공**해주어야한다.

<br/>

**\_app.tsx**

```javascript
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

function App({ Component, pageProps }: AppProps) {
  const [queryClient] = useState(() => new QueryClient());

  return (
    <QueryClientProvider client={queryClient}>
      <Component {...pageProps} />
    </QueryClientProvider>
  );
}
```

**\_component**

```javascript
import { useQuery } from "@tanstack/react-query";

const fetcher = () => {
  return axios.get(`/api/allassets,fetcher`);
};

const { isLoading, error, data, isFetching } = useQuery("assets", fetcher);
```

<br/>

---

<br/>

> ### 주요 기능 차이

**1) Status**

**SWR은 isValidating을 이용해 상태를 표현**하는 데 반해, **React Query는 isLoading, isFetching**을 통해 데이터의 상태를 보여 준다.

특히 **isFetching은 첫 번째 로드를 제외한 데이터 업데이트 시의 상태**를 나타내는 값으로 **모든 데이터 로드 상태를 나타내는 isValidating과 구별**된다.

<br/>

**2) Mutation**

SWR과 React Query는 모두 뮤테이션이라는 개념을 가지고 있다. 하지만 두 라이브러리에게 있어 이 개념은 다르게 작용한다.

둘 다 변형시킨다는 의미에서는 같지만, **React Query의 뮤테이션은 post/patch/put/delete를 통해 서버의 상태를 변형**시키는 것이고,

**SWR의 뮤테이션은 useSWR()을 통해 받아온 데이터를 클라이언트 사이드에서 변형**시켜 업데이트해 주는 개념이다.

<br/>

**3) Offline Mutation**

위에서 말했듯 SWR과 React Query의 뮤테이션은 다르게 작동한다.

**React Query에서는 오프라인 상태에서 뮤테이션을 시도했을 때 해당 요청을 잠시 멈췄다가 온라인 상태가 되면 요청을 재시도**한다.

이러한 API 요청을 멈췄다가 다시 시도해 서버의 데이터를 변경하는 것은 **SWR에는 없는 기능**이다.

<br/>

**4) Data Optimization**

**React Query는 렌더링 퍼포먼스 측면에서도 뛰어난 라이브러리**다.

**쿼리가 업데이트될 때만 컴포넌트를 업데이트**한다. 또한 여러 컴포넌트가 같은 쿼리를 사용하고 있을 때는 **한꺼번에 묶어 업데이트**해 준다.

이 항목은 **SWR에는 해당되지 않는다**.

<br/>

**5) Auto Garbage Collection**

**React Query는 쿼리가 지정된 시간**(설정하지 않을 경우 기본 5분) 동안 **쿼리가 사용되지 않을 경우 자동으로 Garbage Collection을 지원**한다.

이 또한 **SWR에는 해당되지 않는 항목**이다.

<br/>

이외에도 **React Query에서만 지원하는 많은 기능**들이 더 있다.

자세히 알아보고 싶다면 [React Query vs SWR](https://tanstack.com/query/latest/docs/react/comparison?from=reactQueryV3&original=https%3A%2F%2Ftanstack.com%2Fquery%2Fv3%2Fdocs%2Fcomparison)를 눌러 확인해 보자.

<br/>

---

<br/>

> ### 요약

**간단하게 데이터를 가져오기에는 SWR가 좋고, 데이터 캐싱 및 더 많은 제어가 필요한 경우에는 React Query가 좋다**고 할 수 있다.

즉, 데이터를 가져오는 간단한 방법이 필요하고 **데이터가 오래되어도 상관 없다면 SWR**이 좋은 옵션이다. 하지만 **캐싱에 대한 보다 세밀한 제어가 필요할 때는 React Query**가 좋은 선택이다.

<br/>

---

<br/>

> ### 프로젝트의 적합성 판단

**SWR은 주로 get요청에 사용**된다. 하지만 프로젝트에서 유저 등록, 스케줄 등록, 파일 업로드 등 **post 요청이 많이 사용**되는 부분,

컴포넌트의 랜더링에 성능을 높이기 위해 **데이터를 최적화** 하는 부분,

이외에도 **React-Query에서 다양한 기능을 제공하기 때문에 사용하는 것이 적합**하다고 생각하였다.

<br/>

---

<br/>

> ### 📑 참고 자료

[SWR vs React-Query, 서버데이터 관리](https://velog.io/@turtlemana/SWR-vs-React-Query-%EC%84%9C%EB%B2%84%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B4%80%EB%A6%AC#swr-react-query-%EC%99%9C-%EC%93%B0%EB%8A%94%EA%B1%B8%EA%B9%8C)

[useSWR vs React(TanStack) Query를 비교해보자](https://voyage-dev.tistory.com/159)

[REACT QUERY VS SWR](https://tech.madup.com/react-query-vs-swr/)
