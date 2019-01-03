---
title: "[Javascript] Span 태그에 text 넣기"
date: 2019-01-03 15:23:00
categories:
  - Web
  - Javascript
tags:
  - span
  - text
---

## Span 태그에 text 넣기

span 태그에 text 를 넣는 방법은 두 가지가 있다.

```javascript
document.getElementById("myspan").innerHTML = "newtext";
```

```javascript
document.getElementById("myspan").textContent = "newtext"; // for mordern browsers
```

필요에 따라 선택해서 사용하면 된다.
