---
title: "[HTML] 폭 없는 공백"
date: 2019-01-03 15:23:00
categories:
  - Web
  - HTML
tags:
  - ZWSP
  - span
  - cursor
---

## 폭 없는 공백

유니코드에서 폭 없는 공백은 U+200B (HTML: `&#8203;`) 에 할당되어있다.

## 커서 위치 지정

폭 없는 공백을 사용하면 커서 위치를 지정할 때 아주 유용하다.

에디터에서 `colorPicker` 로 색상 변경 시 선택된 텍스트의 색상을 바꿀 때
`contenteditable` 안에 스타일을 입힌 `span` 태그를 추가하도록 구현했다.

이때 `span` 안의 내용이 빈 값이면 커서가 인식을 못하는 문제가 있었다.
그래서 텍스트의 선택된 범위가 없다면 `span` 안에 빈 값(`''`) 대신 `&#8203;` 을 넣어주고 커서 위치를 저장했다가 복구시키니 잘 동작하는 것을 확인했다.

이거 때문에 한참동안 헤맸는데..
다음엔 더 유용하게 사용할 수 있기를..

```javascript
saveCaret(node); // 커서 위치 저장

const range = window.getSelection().getRangeAt(0);
const colorElement = document.createElement("span");
colorElement.style = `color:${color}`;

colorElement.innerHTML = isSelected ? selectedText : "&#8203;"; // 폭 없는 공백

restoreCaret(node); // 커서 위치 복구
```
