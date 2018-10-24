---
title: "[Javascript] new Date('YYYY-MM-DD HH:MM') is InvalidDate in IE"
categories:
    - Web
    - Javascript
tags:
    - IE
    - dateTime
date: 2018-06-15 17:44:00
---

### new Date()으로 날짜 생성하기
자바스크립트 자체내에서 new Date() 함수를 지원한다. 
파라미터를 안 주게 되면 현재 날짜로 생성되고, 
()안에 `컴마(,)/대시(-)/슬래시(/)` 등 여러가지 방식으로 파라미터를 줄 수 있다.

하지만 이 중에 제목처럼 `new Date('YYYY-MM-DD HH:MM')` 을 하게 되면 크롬에서는 잘 나오나, 
IE에서는 InvalidDate으로 나온다.
이 문제를 해결하기 위해서는 연도와 시간 사이에 빈칸대신 `T`를 넣으면 된다.

```jsx
// for Chrome
new Date('2018-06-15 17:44') // FRI JUN 15 2018 17:44:00 GMT+0900 (KST) 

// for Chrome, IE
new Date('2018-06-15T17:44') // FRI JUN 15 2018 17:44:00 GMT+0900 (KST) 
```