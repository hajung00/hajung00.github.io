---
title: 스플래시, 온보딩 페이지 구현하기(React)
author: cotes
date: 2024-04-04 00:55:00 +0800
categories: [Project, Americanote]
tags: [Next.js, Swiper]
---

Americanote는 웹앱 서비스로 스플레시, 온보딩 화면을 제작하게 되었다. 스플레시나 온보딩은 주로 앱서비스에서 사용하는 경향이 있어 웹서비스만 제작하던 나에겐 생소한 개념이였다.

이번 포스트에서는 **스플레시, 온보딩의 개념 및 구현 방법**에 대해 알아보려한다.

> ## 스플래시(Splash)

<img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/c168f689-229f-484e-9c2e-380a3fc4ff26" width="60%" height="30%" alt="image"/>

**스플래시는 앱을 실행할 때 가장 먼저 표시되는 화면**이다. 보편적으로 로고 또는 간단한 서비스 소개와 함께 앞으로 이용하게 될 앱의 전반적인 분위기를 설정한다. 스플래시 화면의 주 목적은 다음과 같다.

**브랜딩**
앱을 처음 실행했을 때 마주하는 화면으로 가장 빨리 인지될 수 있는 위치해 있다. 반복적인 노출로 서비스(브랜드)에 대한 인지도를 강화하고 신뢰감과 친숙함을 형성한다.

**기대설정(Expectation Setting)**
앱을 실행하고 사용하기 위해 정보를 불러오고 있음을 사용자에게 알립니다. 기본 인터페이스(ex)로그인화면, 메인화면)이 나타나기 전까지 사용자에게 잠깐의 대기 시간을 부여할 수 있다.

<br/>

---

<br/>

> ## 온보딩(Onboarding)

<img src="https://github.com/hajung00/hajung00.github.io/assets/66300154/49bef69a-ccda-4b74-96a5-7e06a9c5cbf0" width="70%" height="30%" alt="image"/>

온보딩 화면은 스플래시 화면 다음 단계이다. 이 앱의 목적은 앱의 핵심 기능에 대한 개요를 제공하는 것이다. 온보딩 화면의 목적은 다음과 같다.

**신규 사용자 참여**
앱의 주요 이점과 기능을 보여줌으로써 사용자의 관심을 사로잡고 온보딩 프로세스를 계속 진행하도록 설득한다.

**사용자 교육**
그것은 새로운 사용자들에게 앱의 내비게이션에 익숙하게 하고 그들의 삶에 가져다주는 가치를 강조한다.

**사용자 등록/로그인**
온보드 화면에는 새 계정을 만들거나 로그인하는 옵션이 포함되어 기기 간에 개인화된 경험과 데이터 동기화가 가능하다.

<br/>

---

<br/>

> ## 구현 방법

### 스플래시(Splash)

대부분의 경우 스플래시 화면은 1초에서 3초 사이의 시간 동안 보여주는 것이 적절하다고 한다.

1.  setTimeout으로 opacity조정

    ```javascript
    const [opacity, setOpacity] = useState(100);

    if (opacity > 90) {
      const timer1 = setTimeout(() => {
        setOpacity(opacity - 1);
        clearTimeout(timer1);
      }, 100);
    } else if (opacity > 5) {
      const timer2 = setTimeout(() => {
        setOpacity(opacity - 8);
        clearTimeout(timer2);
      }, 70);
    }

    if (opacity == 0) {
      // 페이지 이동
    }
    ```

    <br/>

2.  변수로 opacity 스타일 지정

    ```javascript
    const SplashStyle = styled.div<Props>`
        opacity: ${(props) => `${props.opacity}%`};
    `

    return(
        <SplashStyle opacity={opacity}>
    )

    ```

<br/>

### 온보딩(Onboarding)

- 두 번째 방문에서 노출 X
- 가장 흔한 방법은 쿠키 또는 로컬 스토리지를 사용하여 사용자의 방문 기록을 추적하여 페이지를 보여주거나 숨김

<br/>

1.  온보딩 페이지 방문 시 cookie 생성

    ```javascript
    import Cookies from "js-cookie";

    useEffect(() => {
      const onboardingVisited = Cookies.get("onboarding_visited");
      if (!onboardingVisited) {
        Cookies.set("onboarding_visited", "true", { expires: 30 }); // 만료 날짜를 30일로 설정
      }
    }, []);
    ```

    <br/>

2.  swiper onSlideChange 함수로 온보딩 마지막 페이지에서 버튼 활성화 및 페이지 이동

    ```javascript
    const [activeIndex, setActiveIndex] = useState(0);

    const handleSlideChange = (swiper: any) => {
      setActiveIndex(swiper.activeIndex);
      console.log(swiper.activeIndex);
    };

    const onClickHandler = useCallback(() => {
      if (activeIndex == 2) {
        router.push("/home");
      }
    }, [activeIndex]);

    return (
      <Swiper
        // install Swiper modules
        modules={[Pagination]}
        slidesPerView={1}
        navigation
        pagination={{ clickable: true }}
        scrollbar={{ draggable: true }}
        onSlideChange={handleSlideChange}
      >
        // 디자인
      </Swiper>
    );
    ```

    <br/>

3.  두 번째 방문 시, 스플레시 페이지에서 쿠키 확인 및 쿠키 여부에 따른 페이지 이동

    ```javascript
    // Splash 페이지
    import Cookies from "js-cookie";

    const onboardingVisited = Cookies.get("onboarding_visited");

    if (opacity == 0) {
      if (onboardingVisited) {
        // 쿠키 있는 경우(온보딩 페이지 건너뛰고 main페이지로 이동)
        router.push("/home");
      } else {
        // 쿠키 없는 경우(온보딩 페이지로 이동)
        router.push("/onboarding");
      }
    }
    ```

### 🖥️ 적용 화면

![-Clipchamp5-ezgif com-video-to-gif-converter](https://github.com/hajung00/hajung00.github.io/assets/66300154/80f55acf-fd28-4429-9c8c-86c363ac1a76)

<br/>

---

<br/>

> ## 📑 참고 자료

[앱에 처음 여정(탐색)을 돕는 장치들에 대한 이해(스플래시, 온보딩, 튜토리얼, 코치마크)](https://inblog.ai/junq/957)
