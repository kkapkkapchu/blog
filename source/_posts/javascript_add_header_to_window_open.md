---
title: "[Javascript] window.open()에 헤더 추가하기"
categories:
    - Web
    - Javascript
tags:
    - window.open
    - add_header
date: 2017-09-05 14:24:01
---

`window.open(url);` 을 하면 새 창이 열린다. 
새 창을 열 때 헤더를 추가하려면 아래와 같이 하면 된다.

```jsx
let xhttp = new XMLHttpRequest();

xhttp.open("GET", url, true);
xhttp.setRequestHeader("Content-type", "application/json"]);
xhttp.responseType = "blob";

xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
        // Typical action to be performed when the document is ready:
        window.open(URL.createObjectURL(xhttp.response));
    }
};

xhttp.send();
```