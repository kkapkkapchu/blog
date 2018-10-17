---
title: 마우스 우클릭 이벤트와 좌클릭 이벤트 구분하기
categories:
    - Web
    - JavaScript
tags:
    - mouse_click_event
    - onTouchTap
date: 2017-08-28 16:22:44
---

리액트에서 onTouchTap 을 사용했을 때 마우스 우클릭을 하면 처음에는 이벤트 호출이 안되는데, 
두 번째 우클릭부터는 onTouchTap 이벤트가 호출된다.
(onClick 은 무조건 마우스 좌클릭만 호출된다.)

스크립트에서 마우스 우클릭과 좌클릭을 구분해야 할 때는 onClick 이나 onTouchTap 이 아닌 `onMouseDown` 이나 `onMouseUp` 이벤트를 사용해야 한다.

```bash
handleMouseUp = (event) => {
    console.log(event.button) // 1: left, 2: right
}

<button onMouseUp={this.handleMouseUp}>마우스 클릭이벤트 구분</button>
```
