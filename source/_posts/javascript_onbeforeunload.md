---
title: "[Javascript] 페이지 벗어날 때(새로고침/닫기/뒤로가기) 브라우저 확인 창 띄우기" 
categories:
    - Web
    - Javascript
tags:
    - onBeforeUnload
date: 2018-06-15 16:51:00
---

### 페이지 벗어날 때(새로고침/닫기/뒤로가기) 발생하는 이벤트
메일을 작성 중일 때 화면을 벗어나려고 하면 브라우저 자체 확인 창이 뜬다.
화면을 벗어날 때 `onBeforeUnload` 이벤트가 호출되므로 여기서 작업을 하면 된다.

```jsx
onBeforeUnload = (event) => {
    // 변경사항이 없다면 그냥 브라우저를 벗어남
    if (!this.isChange()) return;

    event.returnValue = "변경사항이 저장되지 않을 수 있습니다.";
}

componentDidMount() {
    window.addEventListener("beforeunload", this.onBeforeUnload); // 페이지 새로고침, 닫기
}

componentWillUnmount() {
    window.removeEventListener("beforeunload", this.onBeforeUnload);
}
```

하지만 여기서 중요한 거!!

나는 리액트를 사용하므로 `onBeforeUnload` 이벤트가 페이지 닫기나 새로고침시에는 잘 호출되지만
뒤로가기를 할 때에는 호출이 안된다. 아마도 라우터 때문인듯하다..
`onBeforeUnload` 이벤트는 현재 window 를 벗어날 때 호출되는 거 같은데, 
리액트는 SPA(Single Page App)이기 때문에 url이 바뀌더라도 dom이 바뀌지 window 가 바뀌지 않는다.
 
리액트 컴포넌트에서 뒤로가기 이벤트를 잡으려면 `setRouteLeaveHook` 를 이용하는 방법이 있다.
이거에 대해서는 다음 포스팅을 쓰도록 하겠다.