---
title: URI , URL , URN
author: cotes
date: 2024-03-18 01:33:00 +0800
categories: [Study, CS]
tags: [CS, 네트워크]
---

> ## URL / URI / URN 차이점

<img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/d620e8cd-98f5-40b1-916c-fe4f7f141651" width="60%" height="40%" alt="image"/>

- URI - 자원의 식별자

- URL - 위치(Location)

- URN - 이름(Name)

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/8f435310-a2b6-4a0c-917d-e9e3d9d88755)

<br/>

---

<br/>

> ## URI (Uniform Resource Identifier)

- **통합 자원 식별자(Uniform Resource Identifier)는 인터넷에 있는 자원을 어디에 있는지 자원 자체를 식별하는 방법**이다

  - Uniform: 리소스 식별하는 통일된 방식

  - Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)

  - Identifier: 다른 항목과 구분하는데 필요한 정보

- URI의 존재는 인터넷에서 요구되는 기본조건으로서 인터넷 프로토콜에 항상 붙어 다닌다.

- **URI의 하위개념으로 URL, URN** 이 있다

<br/>

---

<br/>

> ## URL (Uniform Resource Locator)

- **파일식별자(Uniform Resource Locator)는 네트워크 상에서 자원이 어디 있는지 위치를 알려주기 위한 규약**이다.

- 즉, 컴퓨터 네트워크와 검색 메커니즘에서의 위치를 지정하는, 웹 리소스에 대한 참조이다.

- 흔히 우리는 URL을 웹 사이트 주소로만 알고 있지만, URL은 웹 사이트 주소뿐만 아니라 컴퓨터 네트워크상의 자원을 모두 나타내는 표기법이다.

- 해당 주소에 접속하려면 URL에 맞는 프로토콜(http, sftp, smp ..등)을 알아야 하고, 그와 동일한 프로토콜로 접속해야 한다.

### URL 구성

<img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/2855bcc8-5ff4-4890-880b-37bdb7209d81" width="90%" height="70%" alt="image"/>

- 프로토콜(https)

- 호스트명(www.google.com)

- 포트 번호(443)

- 패스(/search)

- 쿼리 파라미터(q=hello&hl=ko)

---

#### scheme

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/4cf49b5d-48d8-454a-8d23-3da78af14463)

• 주로 프로토콜 사용

• 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙

• 예) http, https, ftp 등등

• http는 80 포트, https는 443 포트를 주로 사용, 포트는 생략 가능

• https는 http에 보안 추가 (HTTP Secure)

<br/>

#### userinfo

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/e19de6ac-982f-48ac-a4ed-8bc0e1bd87d7)

• URL에 사용자정보를 포함해서 인증

• 거의 사용하지 않음

<br/>

#### host

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/52b81d0d-e790-4616-ab06-fe1f72c48562)

• 호스트명

• 도메인명 또는 IP 주소를 직접 사용가능

<br/>

#### port

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/0f0f9e1b-119b-4039-9727-cc451a6eeefc)

• 포트(PORT)

• 접속 포트

• 일반적으로 생략, 생략시 http는 80, https는 443

<br/>

#### path

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/c827c59a-5919-4c3f-b357-84f09ce5aa5d)

• 리소스 경로(path), 계층적 구조

• 예)

• /home/file1.jpg

• /members

• /members/100, /items/iphone12

<br/>

#### query

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/87c93811-861e-4403-8dda-f47406397ae3)

• **key=value 형태**

• ?로 시작, &로 추가 가능 ?keyA=valueA&keyB=valueB

• query parameter, query string 등으로 불림, 웹서버에 제공하는 파라미터, 문자 형태

<br/>

#### fragment

![image](https://github.com/hajung00/hajung00.github.io/assets/66300154/30466ec5-50f6-4488-8cb3-5ec8a3272b2e)

• fragment

• html 내부 북마크 등에 사용

• 서버에 전송하는 정보 아님

<br/>

---

<br/>

> ## URN (Uniform Resource Name)

- **통합 자원 이름(Uniform Resource Name)은 urn:scheme 을 사용하는 URI를 위한 역사적인 이름**이다.

- **URL이 리소스가 있는 위치**를 지정한다면, **URN은 리소스에 이름**을 부여하는 것이다.

- URN은 영속적이고, 위치에 독립적인 자원을 위한 지시자로 사용하기 위해 1997년도 RFC 2141 문서에서 정의되었다.

- 하지만 리소스가 이름에 매핑되어 있어야 하기 때문에 **이름으로 부여하면 거의 찾기가 힘들다.** 그래서 **대부분 URL만 쓴다.** (즉 URN은 몰라도 된다)

<br/>

---

<br/>

> ## URI / URL / URN 구분하기

위에서 URI / URL / URN 의 정의에 대해 알아보았지만, 아직도 모호하다.

인터넷 상의 자원의 위치(URL)와 자원의 식별자(URI)는 언뜻 보면 같은 것을 의미하는 듯 하다. 하지만 **'자원의 위치'라는 것은 결국은 '하나의 파일 위치'를 나타내는 것임을 명심**하자.

<br/>

예를들어 다음과 같은 홈페이지 링크가 있다고 하자.

`http://www.naver.com/index.html?page=1232950&id=776`

<br/>

http://www.naver.com/ 서버에 위치한 index.html 페이지는 query string인 page의 값에 따라 여러가지 화면 결과를 나타나게 된다.

이때 여기서 **URL은 index.html의 위치를 표기한 `http://www.naver.com/index.html`** 까지이다.

하지만 **사용자가 원하는 정보에 도달 하기위해서는 ?page=1232950&id=776라는 식별자(Identifier)가 필요한 것**이다.

따라서 엄격히 구분하자면 위의 **`http://www.naver.com/index.html?page=1232950&id=776` 주소는 URI**이고, **식별자가 빠진 `http://www.naver.com/index.html`를 URL**이라고 하는 것이다.

<br/>

이유는 URL은 자원의 위치를 나타내 주는 것이고 URI는 자원의 식별자인데, ?page=1232950&id=776 이 부분은 위치를 나타내는 것이 아니라 page값이 1232950이고 id가 776인 것을 나타내는 식별하는 부분이기 때문이다.

물론 통상적으로 대충 URL이라고 얘기를 하지만 엄격하게는 URI라고 하는 것이 맞다.

<br/>

조금 억지스러운 예를 들면, **아래의 두 주소는 같은 URL이고 다른 URI**라고 할 수 있다.

`http://www.naver.com/index.html?page=1232950&id=776`

`http://www.naver.com/index.html?page=9923145&id=122`

<br/>

---

<br/>

> ## 📑 참고 자료

[ URL / URI / URN 차이점 - 한방 이해하기](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-URI-%EC%B0%A8%EC%9D%B4)

[URL 구성 요소 & 요청 흐름 정리](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-%EA%B5%AC%EC%84%B1-%EC%9A%94%EC%86%8C-%EC%9A%94%EC%B2%AD-%ED%9D%90%EB%A6%84-%EC%A0%95%EB%A6%AC)
