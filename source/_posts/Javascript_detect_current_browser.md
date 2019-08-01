---
title: "[Javascript] Detect Current Browser"
categories:
  - Web
  - Javascript
tags:
  - browser
  - ie
  - edge
  - chrome
  - firefox
  - safari
  - opera
date: 2019-08-01 15:00:00
---

### 현재 브라우저 종류 구분하기

IE 브라우저에서 지원하지 않는 기능들이 많기 때문에 브라우저 자체를 막아버리는 경우가 있다. 그러기 위해서는 현재 접속한 브라우저가 어떤건지 알아야 한다.

```js
// Opera 8.0+
var isOpera =
  (!!window.opr && !!opr.addons) ||
  !!window.opera ||
  navigator.userAgent.indexOf(" OPR/") >= 0;

// Firefox 1.0+
var isFirefox = typeof InstallTrigger !== "undefined";

// Safari 3.0+ "[object HTMLElementConstructor]"
var isSafari =
  /constructor/i.test(window.HTMLElement) ||
  (function(p) {
    return p.toString() === "[object SafariRemoteNotification]";
  })(
    !window["safari"] ||
      (typeof safari !== "undefined" && safari.pushNotification)
  );

// Internet Explorer 6-11
var isIE = /*@cc_on!@*/ false || !!document.documentMode;

// Edge 20+
var isEdge = !isIE && !!window.StyleMedia;

// Chrome 1 - 71
var isChrome =
  !!window.chrome && (!!window.chrome.webstore || !!window.chrome.runtime);

// Blink engine detection
var isBlink = (isChrome || isOpera) && !!window.CSS;

var output = "Detecting browsers by ducktyping:<hr>";
output += "isFirefox: " + isFirefox + "<br>";
output += "isChrome: " + isChrome + "<br>";
output += "isSafari: " + isSafari + "<br>";
output += "isOpera: " + isOpera + "<br>";
output += "isIE: " + isIE + "<br>";
output += "isEdge: " + isEdge + "<br>";
output += "isBlink: " + isBlink + "<br>";
document.body.innerHTML = output;
```

아래는 위의 코드로 테스트에 성공한 브라우저 목록이다.

- Firefox 0.8 - 61
- Chrome 1.0 - 71
- Opera 8.0 - 34
- Safari 3.0 - 10
- IE 6 - 11
- Edge - 20-42

### 참고사이트

- [How to detect Safari, Chrome, IE, Firefox and Opera browser?](https://stackoverflow.com/a/9851769)
