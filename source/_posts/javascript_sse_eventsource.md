---
title: "[Javascript] EventSource를 이용해서 SSE 받기"
categories:
  - Web
  - Javascript
tags:
  - EventSource를
  - SSE
date: 2019-07-03 18:12:00
---

### SSE 란?

HTML5의 Server-Sent Events(이하 SSE)는 이러한 문제 없이 서버가 필요할 때마다 클라이언트에게 데이터를 줄 수 있게 해주는 서버 푸시 기술입니다.

### SSE 장점

- 전통적인 HTTP를 통해 통신하므로 다른 프로토콜이 필요가 없습니다.
- 재접속 처리 같은 대부분의 저수준 처리가 자동으로 됩니다.
- 표준 기술답게 IE를 제외한 브라우저 대부분을 지원합니다.
- HTML과 JavaScript만으로 구현할 수 있으므로 현재 지원되지 않는 브라우저(IE 포함)도 polyfill을 이용해 크로스 브라우징이 가능합니다. (여기서 polyfill이란 브라우저가 지원하지 않는 API를 플러그인이나 JavaScript 등으로 흉내 내 구현한 것을 뜻합니다.)

### 사용 방법

위의 SSE의 장점에서 보듯이 SSE는 구현이 아주 간편합니다. 클라이언트는 서버로부터 스트림을 받아 `EventSource` 객체를 통해 서버가 푸시하는 데이터를 받아 처리하기만 하면 됩니다.

Client 측 코드

```js
const es = new EventSource(stream_url);

es.onmessage = function(e) {
  // 이벤트 설정이안된 기본 데이터 처리
  console.log(e.data);
};
es.addEventListener(
  "myevent",
  function(e) {
    // 'myevent' 이벤트의 데이터 처리
    console.log(e.data);
  },
  false
);
```

`EventSource` 객체의 속성은 다음과 같습니다.

- `onmessage`
  - 기본 메시지가 왔을 때 호출
- `onopen`
  - 접속이 맺어졌을 때 호출
- `onerror`
  - 오류 발생 시 호출

`EventSource`의 `addEventListener()`를 사용하면 위 3개의 이벤트뿐만 아니라 따로 지정된 이벤트의 데이터도 받아 처리할 수 있습니다.

### 주의사항

서버와 연결이 되어있으니 사용이 끝나거나 페이지 이동 시에는 반드시 `close()`를 해줘야 한다. 서버에 부하가 걸려서 죽을 수 있음.

```js
es.close();
```

### 참고사이트

- [MDN - EventSource](https://developer.mozilla.org/en-US/docs/Web/API/EventSource/EventSource)
- [SSE를 이용한 실시간 웹앱](https://spoqa.github.io/2014/01/20/sse.html)
