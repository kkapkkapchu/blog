---
title: "[Javascript] Object 의 key/value 값 가져오기"
categories:
    - Web
    - Javascript
tags:
    - object
    - key/value
date: 2017-09-05 14:24:01
---

### Object 의 key 값 가져오기
```jsx
let requestHeaderObject = {
    key1: value1,
    key2: value2,
};

console.log(Object.keys(requestHeaderObject)); // ["key1", "key2"]
```
`Object.keys` 안에 key 값을 가져올 객체를 넣으면 객체의 key 값이 배열로 떨어진다.

value 를 가져올 때는 객체의 key 배열을 돌면서 value 를 가져오면 된다.

### Object 의 value 값 가져오기
```jsx
let requestHeaderObject = {
    key1: value1,
    key2: value2,
};

Object.keys(requestHeaderObject).map((value) => {
    console.log(value, requestHeaderObject[value]);
});

// key1 value1
// key2 value2
```