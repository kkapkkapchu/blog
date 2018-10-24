---
title: "[Javascript] HTML 요소 동적으로 생성하고 속성(Attribute) 추가하기"
categories:
    - Web
    - Javascript
tags:
    - HTML
date: 2017-08-13 16:18:27
---

### 요소를 동적으로 생성하고 원하는 위치에 붙이기
```jsx
// 요소 동적 생성
let div = document.createElement("div");

// 원하는 위치에 붙인다
document.body.appendChild(div);

// React 에서는 요소를 붙이고 싶은 위치를 ref 로 잡아서도 가능 
this.refs.test.appendChild(div);
```

### 동적으로 생성한 요소에 속성(Attribute) 추가하기
```jsx
// 요소 동적 생성
let div = document.createElement("div");

// 속성을 추가한다
div.setAttribute("style", "width: 100%; height: 100%;")

// 원하는 위치에 붙인다
document.body.appendChild(div);

// React 에서는 요소를 붙이고 싶은 위치를 ref 로 잡아서도 가능 
this.refs.test.appendChild(div);
```

div.setAttribute("style", "width: 100%; height: 100%;") 에서 첫 번째 인자가 추가할 속성의 이름이고,
두 번째 인자가 추가할 속성의 값이다.

### 동적으로 생성한 요소에 이벤트 속성 추가하기
```jsx
// 요소 동적 생성
let a = document.createElement("a");

if (a.addEventListener) {
    a.addEventListener('click', handler, false); 
} else if (el.attachEvent) {
    a.attachEvent('onclick', handler);
}
```

onclick, onblur 와 같은 이벤트 속성을 추가하고 싶을 때는 setAttribute 가 아닌 addEventListener 로 하면 된다.

### 동적으로 생성한 요소 삭제하기
```html
<ul id="myList">
    <li>Coffee</li>
    <li>Tea</li>
    <li>Milk</li>
</ul>
```
위의 예제처럼 ul, li 태그가 생성되었다고 할 때, li 태그를 지우고 싶으면 아래 예제처럼 하면 된다.
```jsx
// 삭제할 요소의 상위(부모) 요소
let myNode = document.getElementById("myList");

// 원하는 자식 요소만 지우기
myNode.removeChild(myNode.childNodes[0]);

// 자식 요소들 한번에 지우기
myNode.innerHTML = '';
```
