---
title: "[Javascript] keycode 라이브러리 사용시 키보드 오른쪽 keypad 숫자 가져오기"
categories:
    - Web
    - Javascript
    - Library
tags:
    - IE
    - keycode
date: 2017-08-17 11:01:12
---

`keycode` 라이브러리를 사용해서 값을 가져오고 숫자만 뽑아내기 위해서 정규식을 쓰는 경우가 있다.
이때 조심해야 하는 경우가 있는데 바로 키보드 오른쪽의 keypad 로 입력했을 때이다.

IE에서는 keypad 로 숫자 입력시 `keycode` 라이브러리로 값을 가져오면 "`numpad 숫자`" 로 찍힌다.
그래서 숫자 정규식을 쓰려면 숫자만 뽑아내야 한다. 

```jsx
import keycode from 'keycode'

// 맨 뒤의 숫자만 가져오기
// console.log(keycode(event));  // IE: "numpad 숫자", Others: "숫자"
let keycodeStr = keycode(event).slice(-1);

// 숫자만인지 체크하는 정규식
const regNumber = /^[0-9]*$/;
if ( !regNumber.test(keycodeStr) ) return;
```