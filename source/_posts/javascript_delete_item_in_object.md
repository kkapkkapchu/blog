---
title: "[Javascript] Object의 item 삭제하기"
categories:
  - Web
  - Javascript
tags:
  - delete
  - object
date: 2019-01-10 15:21:00
---

Object 에 들어있는 값을 삭제하려면 `delete` 명려어를 사용하면 된다.

```js
let data = {
  key1: "value1",
  key2: "value2"
};

console.log(data); // {key1: "value1", key2: "value2"}
delete data["key1"];
console.log(data); // {key2: "value2"}
```
