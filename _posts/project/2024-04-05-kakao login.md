---
title: 카카오(KaKao)로그인 구현 - Rest API, Javascript SDK
author: cotes
date: 2024-04-04 00:55:00 +0800
categories: [Project, Americanote]
tags: [Next.js, Oauth Login, 에러 해결]
---

> ## 카카오 로그인

프로젝트에서 사용자가 별도의 회원가입 없이 간편하게 로그인할 수 있도록 하기 위해 카카오 로그인을 사용하기로 하였다.

구현을 해본 경험이 없고 개발 기간도 10일 밖에 되지 않았기 때문에 걱정이 많았지만 레퍼런스가 많아 어렵지 않게 적용시킬 수 있었다.

**카카오 로그인 구현 방법에는 REST API와 Javascript SDK 방식** 두 가지가 있고, **각각의 구현 방법과 차이점에 대해 알아보고 어떤 방법을 선택할지 고민해 보았다.**

<br/>

---

<br/>

> ## 카카오 로그인 구현 방식

### REST API 방식(REST API 키로 구현)

카카오 로그인의 **REST API 방식은 카카오 인가 코드를 받기위해서 쿼리 파라미터에 필수값들을 넣어서 만들어진 URL에 직접 접속후, 리다이렉트 된 페이지에서 인가 코드를 얻는 방식**이다.

**여기서의 핵심은 URL에 직접 접속한다는점**이다.

<br/>

**REST API 진행 방식**

1. 로그인 버튼을 누른다. (클릭하면 kakaoAuthUrl로 가게 설정)

2. KakaoAuthUrl에서 로그인 처리가 되고, RedirectUri로 자동으로 넘어간다.

3. RedirectUri 뒤에 인가코드에 같이 올 건데, 이걸 프론트에서 추출한다.

4. 이 인가코드를 api에 쿼리스트링으로 같이 파싱해서 백엔드로 준다.

5. 백엔드에서는 이 인가코드를 받아서 jwt토큰 생성 후 프론트로 넘겨준다.

6. jwt토큰을 받아 로그인을 유지한다.

<br/>

```javascript
const SocialKakao = () => {
  const Rest_api_key = "REST API KEY"; //REST API KEY
  const redirect_uri = "http://localhost:3000/auth"; //Redirect URI
  // oauth 요청 URL
  const kakaoURL = `https://kauth.kakao.com/oauth/authorize?client_id=${Rest_api_key}&redirect_uri=${redirect_uri}&response_type=code`;
  const handleLogin = () => {
    window.location.href = kakaoURL;
  };
  return (
    <>
      <button onClick={handleLogin}>카카오 로그인</button>
    </>
  );
};
export default SocialKakao;
```

<br/>

---

### Javascript SDK 방식(JavaScript 키로 구현)

**코드상에서 불러온 자바스크립트 SDK의 전역 카카오 객체를 사용하여 로그인을 진행하는 방식**이다.

- `npm install react-kakao-login`

<br/>

**Javascript SDK 진행 방식**

1. 로그인 버튼을 누른다.

2. kakao login 응답으로 받은 엑세스 토큰을 api에 쿼리스트링으로 같이 파싱해서 백엔드로 전달한다.

3. 백엔드에서는 이 인가코드를 받아서 jwt토큰 생성 후 프론트로 넘겨준다.

4. jwt토큰을 받아 로그인을 유지한다.

<br/>

```javascript
import KakaoLogin from "react-kakao-login";

const SocialKakao = () => {
  const kakaoClientId = "JavaScript KEY";
  const kakaoOnSuccess = async (data) => {
    console.log(data);
    const idToken = data.response.access_token; // 엑세스 토큰 백엔드로 전달
  };
  const kakaoOnFailure = (error) => {
    console.log(error);
  };
  return (
    <>
      <KakaoLogin
        token={kakaoClientId}
        onSuccess={kakaoOnSuccess}
        onFail={kakaoOnFailure}
      />
    </>
  );
};

export default SocialKakao;
```

<br/>

---

### REST API 방식과 Javascript SDK 방식의 차이점

참고로 PC에서 각 방법들을 실행할때는 아무런 차이가 없이 동일하게 동작하지만 **모바일에서 차이점을 확인**할 수 있다.

| Rest API                                                                                                      | Kakao SDK                                                                                                     |
| ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| ![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/c81bae5b-eb4a-4263-a3db-9f75c79db2b6) | ![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/9fb70bd9-3002-4ae2-89a5-cb5576a146be) |

> Rest API: **카카오 로그인 페이지를 먼저 접속한 후, 카카오톡으로 로그인 버튼을 통해서 카카오톡 앱에서 로그인**을 할 수 있도록 제공한다.

> Kakao SDK: **휴대폰에 카카오톡 앱이 설치되어있다면 바로 카카오톡 앱을 실행**하여 로그인을 할 수 있도록 제공한다. 만약 카카오톡 앱이 설치가 안되어있다면, Rest API 방식에서 본 로그인 페이지가 나타난다.

<br/>

우리팀은 **웹앱 어플리케이션을 제작하였기에 모바일에서 곧바로 카카오톡 앱을 실행하여 로그인을 진행한다는 점이 사용자의 전환율을 이끌어내는데 더 도움이 될 수 있겠다고 생각하여 Javascript SDK 방식을 선택**하게 되었다.

<br/>

---

<br/>

> ## 문제점

백엔드팀에서 생성한 카카오 javascript key를 전달받아 카카오 로그인을 요청하였다.

백엔드팀에서 전달받은 api문서가 있긴 했지만 서버가 정상적으로 작동하는지, 응답은 어떻게 오는지 정확히 확인하기 위해 postman을 이용했다.

확인 결과, jwt토큰이 응답 header에 정상적으로 담겨있는 것을 확인하였다.

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/a93e5cce-1c02-4f01-8c4e-7bcba83b1555)

이후 요청을 하여 **header의 Authorization에 저장된 jwt토큰을 가져와서 console로 확인해보았지만 `undefined`라고 출력**되었다.

postman에서는 잘 확인을 했었기에 응답한 결과를 잘못출력했나? 생각이 들었지만 검색한 결과 **클라이언트에서는 해줄 수 있는 일은 없고 서버 측 코드를 수정해주어야 한다**고 하였다.

<br/>

---

<br/>

> ## 해결방법

백엔드팀에게 클라이언트 측에서 응답 header의 Authorization에 접근하려면 서버측에서 아래와 같은 코드를 추가해야 값을 읽어올 수 있는 것 같다고 말씀드렸고, 수정 후 값을 잘 가져올 수 있었다.

```
app.use(
  cors({
    exposedHeaders: ['Authorization'],
  }),
);
```

이후, **가져온 jwt토큰을 쿠키에 저장하여 사용자의 로그인 여부를 확인**하였다.

<br/>

---

<br/>

> ## 📑 참고 자료

[IT/React[React] 카카오(Kakao) 로그인 구현해보기](https://stack94.tistory.com/entry/React-%EC%B9%B4%EC%B9%B4%EC%98%A4Kakao-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EA%B5%AC%ED%98%84%ED%95%B4%EB%B3%B4%EA%B8%B0#%EC%B9%B4%EC%B9%B4%EC%98%A4%20%EB%A1%9C%EA%B7%B8%EC%9D%B8%20%EA%B3%BC%EC%A0%95-1)

[카카오 로그인의 SDK와 Rest API 방식의 차이점](https://yiyb-blog.vercel.app/posts/difference-kakao-sdk-and-url)

[Axios Response header의 값이 없는 경우에 대해](https://bogmong.tistory.com/5)
