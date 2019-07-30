---
title: "[Etc] IE Set Cookie"
categories:
  - Web
  - etc
tags:
  - ie
  - cookie
date: 2019-07-30 15:00:00
---

### IE Get/Set Cookie

IE 는 Chrome 처럼 쿠키 에디터가 잘 되어있지 않다. IE 쿠키 지원을 위한 별도의 프로그램까지 있다.
여기서는 콘솔을 이용해 간단하게 쿠키를 가져오고 저장하는 것만 정리하도록 하겠다.

개발자도구를 열어서 콘솔창에 아래와 같이 입력하면 된다.

```js
// Get Cookie
document.cookie;

// Set Cookie
document.cookie = "YOURKEY=YOURVALUE";
```

### 참고사이트

- [IE 11 cookies in Developer tools](https://stackoverflow.com/a/44946850)
