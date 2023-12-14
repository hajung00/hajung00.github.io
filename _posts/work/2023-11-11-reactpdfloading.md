---
title: 뇌파 결과지 PDF Loading 적용_React-pdf/renderer
author: cotes
date: 2023-11-11 00:34:00 +0800
categories: [Work-Devlog, 뇌파 분석 시스템]
tags: [Work-Devlog, React-pdf, React-pdf/renderer]
---

<!-- 프로젝트 작업하면서 했던 고민, 어떻게 해결했는지에 대한 내용이 담겨져있습니다. -->

> ## 해결 방법

이전 게시글에서 react-pdf/render를 통해 pdf를 생성하였다.

여러 분석 종류를 합하여 pdf를 생성하다보니 로딩시간이 8~10초 정도 걸려 로딩화면을 추가하려했으나 <br/>

**@react-pdf/render에는 pdf가 다 랜더링 되고 난 후에 실행되는 콜백 함수나, 이벤트가 존재하지 않아**

아래와 같은 해결법을 적용해 보려한다.

### Process

> - @react-pdf/render에서 pdf가 생성이 되면, blob로 저장 후, react-pdf로 랜더링<br/>
> - react-pdf의 onLoadSuccess를 사용해 pdf가 랜더링 된 후에 로딩 제거

<br/>

---

<br/>

> ## 해결하기

### 1. @react-pdf/render에서 pdf가 생성이 되면, blob로 저장 

```javascript
useEffect(() => {
  const generatePDF = async () => {
    try {
      // PDF를 생성하고 Blob으로 변환
      const blob = await pdf(
        <MergeReport
          useremail={user.email}
          fileID={fileID}
          imgSrc={pdfURL}
          reportInfo={reportInfo}
        />
      ).toBlob();
      setPdfBlob(blob);
    } catch (error) {
      console.error("PDF 생성 중 오류 발생:", error);
    }
  }; // s3의 이미지 정보 다 받아오면 pdf생성
  if (pdfURL.length !== 0) {
    generatePDF();
  }
}, [pdfURL]);
```

### 2. react-pdf로 랜더링

```javascript
<Document
  file={window.URL.createObjectURL(pdfBlob)}
  onLoadSuccess={handlePdfLoad}
>
  {Array.from(new Array(numPages), (_, index) => {
    return (
      <Page
        key={`page_${index + 1}`}
        pageNumber={index + 1}
        width={700}
        renderAnnotationLayer={false}
      />
    );
  })}
</Document>
```

### 3. onLoadSuccess 정의

```javascript
// pdf 다 그려지면 로딩 false
const handlePdfLoad = ({ numPages }: any) => {
  setNumPages(numPages);
  setIsLoading(false);
};
```

### 4. 로딩중일 경우 Loading컴포넌트 랜더링

```javascript
{
  isLoading && <Loading />;
}
```

<br/>

---

<br/>

> ## ❗ 또 다른 문제점

이전에 **pdf를 React-pdf/render 생성하면 toolbar가 있어 ‘저장/출력’ 기능을 별도로 추가하지 않아도 가능**하였으나, **@react-pdf 라이브러리는 툴바를 제공하지 않는다**는 검색결과가 있었다.

따라서 **저장, 인쇄 기능을 구현**해야한다.

> ## 💡 해결

### 1. 저장 기능 추가

pdf 생성 시, 저장한blob를 url로 생성해 다운로드 받을 수 있도록

```javascript
<a
  href={window.URL.createObjectURL(pdfBlob)}
  download={`${reportInfo?.SubjInfo_Name}_${reportInfo?.FileInfo_RecordDate}`}
>
  <Download fill="#A6D4FF" alt="download icon" id="icon" />
</a>
```

---

### 2. 인쇄 기능 추가

**react-to-print 라이브러리 사용**하여 **인쇄 기능** 구현

#### 1. 설치

```
 $ npm i react-to-print
```

#### 2. 사용 방법

인쇄하고 싶은 **컴포넌트 ref로 지정**

```javascript
import ReactToPrint from "react-to-print";

const ref = useRef(null);

<ReactToPrint
  trigger={() => (
    <button>
      <Printer alt="print icon" id="icon" />
    </button>
  )}
  content={() => ref.current}
/>;

// 인쇄하고 싶은 부분
<div className="pdf-content" ref={ref}>
  ...
</div>;
```

🖥️ 적용 화면

![ezgif com-crop (1)](https://github.com/hajung00/SidePJ-next-node-full-sns/assets/66300154/586ef0db-0b23-449e-af76-1ae2a57092e6)

<br/>

---

<br/>

> ## 🖥️ 로딩 적용 화면 확인

![ezgif com-crop (1) (1)](https://github.com/hajung00/SidePJ-next-node-full-sns/assets/66300154/0fab8f49-6ec1-4cac-98f3-b5b9acf22c61)
