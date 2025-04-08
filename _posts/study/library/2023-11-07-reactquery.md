---
title: React-Query 적용하기
author: cotes
date: 2023-11-07 00:34:00 +0800
categories: [Study, Library]
tags: [Study, Redux, State Management, Library]
---

<!-- 프로젝트 작업하면서 했던 고민, 어떻게 해결했는지에 대한 내용이 담겨져있습니다. -->

> ## React-Query란?

_리액트 애플리케이션에 데이터를 불러오고 캐싱하며, 서버 데이터와의 동기화 및 업데이트 하는 작업을 <br/> 개발자가 쉽고 간단하게 할 수 있도록 도와주는 라이브러리이다._

<br/>

---

<br/>

> ## 사용 목적

서버로부터 받아오는 데이터를 관리하기 위해 **서버 상태 관리를 위한 라이브러리**를 찾아보았다.

검색 결과 대부분 **redux에서 swr이나 react-query로 바꿔가거나 같이 사용하는 추세**임을 확인하였다.

swr과 react-query의 차이점을 파악해 보았고 프로젝트에 적합한 react-query를 사용하기로 하였다.

_SWR과 React-Query의 차이에 대한 내용은 [SWR과 React-Query 비교하기](https://hajung00.github.io/posts/swr,reactquery/) 게시글을 통해 확인할 수 있습니다._

<br/>

---

<br/>

> ## 사용 방법

### 1. 설치

```
 $ npm install @tanstack/react-query @tanstack/react-query-devtools
```

<br/>

### 2. 적용하기

```javascript
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient();

function App({ Component, pageProps }) {
  return (
    <QueryClientProvider client={queryClient}>
      <Component {...pageProps} />
    </QueryClientProvider>
  );
}
```

<br/>

### 3. React-Query SSR에 적용하기

- React-Query로 **SSR을 적용하는 방법**은 **InitialData 속성 사용, Hydration** 두가지가 존재한다.

  #### 방법 1. InitialData

  SSR 메서드로 불러온 **응답을 React Query 기본값**으로 넣어주는 방법

  ```javascript
  export async function getStaticProps() {
    const posts = await getPosts();
    return { props: { posts } };
  }

  function Posts(props) {
    const { data } = useQuery({
      queryKey: ["posts"],
      queryFn: getPosts,
      initialData: props.posts
    });

    // ...
  }
  ```

  - initialData로 데이터를 명시해주면 되기 때문에 **구현을 간단하지만 여러 컴포넌트에서 데이터를 사용한다면 props계속 내려줘야하기 때문에 비효율적**이다.

    ***

  #### 방법 2. Hydration

  **SSR 내에서 prefetch**를 통해 쿼리를 불러온 뒤, **queryClient에서 dehydrate한 상태값으로 페이지에 전달**

    <!-- 링크 달기 -->

  _hydrate에 대한 내용은 [hydrate 알아보기](https://velog.io/@pjh1011409/React-Query-HydrationSSR) 게시글을 통해 확인할 수 있습니다._

  \_app.jsx

  ```javascript
  import {
    Hydrate,
    QueryClient,
    QueryClientProvider
  } from "@tanstack/react-query";

  const queryClient = new QueryClient();

  function App({ Component, pageProps }) {
    return (
      <QueryClientProvider client={queryClient}>
        <Hydrate state={pageProps.dehydratedState}>
          <Component {...pageProps} />
        </Hydrate>
      </QueryClientProvider>
    );
  }
  ```

  // 페이지 SSR

  ```javascript
  export async function getServerSideProps() {
    const queryClient = new QueryClient();
    await queryClient.prefetchQuery(["schedules "], () =>
      getSchedule(user.email)
    );
    return { dehydratedProps: dehydrate(queryClient) };
  }
  ```

  - 모든 컴포넌트에서 schedule 데이터 요청할 때, schedules의 key로 가져올 수 있음.

  ```javascript
  const { data: schedule } = useQuery(["schedules"], () =>
    getSchedule(useremail)
  );
  ```

<br/>

### 4. 데이터 Get - useQuery

```javascript
import { useQuery } from "react-query";
const { data, isLoading, error } = useQuery(queryKey, queryFn, options);
```

- QueryKey

  - QueryKey 를 기반으로 데이터 캐싱을 관리한다.

  - 문자열 또는 배열로 지정할 수 있다.

  - 쿼리가 변수에 의존하는 경우에는 QueryKey 에도 해당 변수를 추가해주어야한다.

  ```javascript
  const { data, isLoading, error } = useQuery(["todos", id], () =>
    axios.get(`http://.../${id}`)
  );
  ```

<br/>

- Query Functions

  - useQuery 의 두번째 인자에는 promise 를 반환하는 함수

<br/>

- Query Options

  - 다양한 옵션은 [eact-query 공식 문서](https://tanstack.com/query/latest/docs/react/reference/useQuery?from=reactQueryV3&original=https%3A%2F%2Ftanstack.com%2Fquery%2Fv3%2Fdocs%2Freference%2FuseQuery)에서 보실 수 있습니다.

<br/>

### 5. 데이터 Post - useMutate

```javascript
import { useMutation } from "react-query";

const { mutate } = useMutation(mutationFn, {
  onSuccess: () => {
    // mutate가 정상적으로 실행되면, 함수를 실행합니다.
    console.log("success");
  },
  onError: () => {
    // mutate가 실패하면, 함수를 실행합니다.
    console.log("error");
  }
});
```

<br/>

- Mutation Functions

  - 어떻게 동작할지 정의만 하고, mutate 함수를 통해 나중에 실행된다.

<br/>

- queryClient.invalidQueries

  - 전달받은 queryKey의 Query를 invalid 처리하고, 해당 Query가 active할 경우 다시 refetch

  - **특정 API 호출 후, 데이터를 갱신**하고 싶을 때 사용할 수 있습니다

  ```javascript
    const POST_KEY = ["post"];

    const { data: post1 } = useQuery(POST_KEY, () => getPost(1).then((res) => res));

    const { mutate } = useMutation((post: Post) => update(post), {
    onSuccess: () => queryClient.invalidQueries(POST_KEY);
    });

    const queryClient = useQueryClient();

    mutate({
    id: 1,
    userId: 2,
    title: 'title from 2',
    body: 'body from 2',
    });
  ```
