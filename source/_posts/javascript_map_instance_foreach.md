---
title: "[Javascript] Map 객체의 forEach 구문 주의!!"
categories:
  - Web
  - Javascript
tags:
  - new_map
  - forEach
date: 2019-07-03 18:08:00
---

### Map 객체

- Map() 은 자바스크립트의 key-value 페어(pair) 로 이루어진 컬렉션
- `new Map()` 으로 Map 객체 생성
- key 를 사용해서 value 를 `get`, `set` 할 수 있음
- key 들은 중복될 수 없음: 하나의 key 에는 하나의 value 만
- key 로 사용할 수 있는 데이터형: string, symbol(ES6), object, function
  (>> `number` 는 사용할 수 없음에 주의!)

```js
let me = new Map();

me.set('name', 'PIKA');
me.set('age', 25);

console.log(me.get('age'); // 25
```

### forEach 구문

`forEach` 의 경우, 인자 순서가 이상한데(key, value 순서가 반대) Array.prototype.forEach() 구문과 통일성을 유지하기 위함(value, index, array 순서인 것)

```js
myMap.forEach(function(value, key) {
  console.log(key + " = " + value);
});
```

### 참고사이트

- [MDN - Map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map)
- [[JS #5] ES6 Map(), Set()](https://medium.com/@hongkevin/js-5-es6-map-set-2a9ebf40f96b)
