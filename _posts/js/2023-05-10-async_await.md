---
layout: single
title: "asyne/await"
categories: javascript
tag: [javascript]
toc: false
author_profile: false
sidebar:
  nav: "docs"
toc: true
toc_label: "목록"
toc_icon: "bars"
toc_sticky: true
---

### async, await

```javascript
async function fetchAndPrint(){
	const response = await fetch('url')
	const result = await response,text()
}
```

- await은 async로 선언된 함수 안에서만 사용 가능
- await은 그 뒤의 코드를 실행하고 그것이 리턴하는 promise객체가 fulfilled상태가 될때까지 기다리고 작업 성공 결과를 리턴함. -> 그 다음 코드 실행
- async 함수는 promise객체를 리턴
- async함수 안의 async함수 사용 가능 (await 붙여서)
