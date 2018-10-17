---
title: 자바스크립트 IE 브라우저 체크하기
categories:
    - Web
    - JavaScript
tags:
    - IE
date: 2018-10-17 14:11:00
---

개발하다 보면 IE 와 크롬이 동작이 달라서 브라우저를 체크해서 처리해야 할 경우가 있다.

아래 코드는 모든 IE일 경우 체크하게 하는 조건문이다.

```bash
let agent = navigator.userAgent.toLowerCase();
if ( (navigator.appName == 'Netscape' && agent.indexOf('trident') != -1) || (agent.indexOf("msie") != -1)) {
     // ie일 경우
} else {
     // ie가 아닐 경우
}
```