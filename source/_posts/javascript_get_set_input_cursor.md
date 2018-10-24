---
title: "[Javascript] input 에서 마우스 커서 위치 얻어오기 및 변경하기"
categories:
    - Web
    - Javascript
tags:
    - mouse_cursor_get
    - mouse_cursor_set
    - caret_position
date: 2017-08-28 16:22:44
---

### input 요소의 마우스 커서 위치 얻어오기

```jsx
/**
 * 커서의 위치를 반환합니다.
 */
getCaretPosition = () => {
    let targetElement = this.refs.input;

    if (document.selection) { // IE < 9
        targetElement.focus();
        let range = document.selection.createRange();
        let rangeLength = range.text.length;
        range.moveStart('character', -targetElement.value.length);

        let start = range.text.length - rangeLength;
        return {'start': start, 'end': start + rangeLength};
    } else if (targetElement.selectionStart || targetElement.selectionStart == '0') { // IE >= 9 and Other Browsers
        return {'start': targetElement.selectionStart, 'end': targetElement.selectionEnd};
    } else {
        return {'start': 0, 'end': 0};
    }
};
```

### input 요소의 마우스 커서 위치 변경하기

```jsx
/**
 * 커서의 위치를 지정합니다.
 */
setCaretPosition = (start, end) => {
    let targetElement = this.refs.input;

    if (targetElement.setSelectionRange) { // IE >= 9 and Other Browsers
        targetElement.focus();
        targetElement.setSelectionRange(start, end);
    } else if (targetElement.createTextRange) { // IE < 9
        let range = targetElement.createTextRange();
        range.collapse(true);
        range.moveEnd('character', end);
        range.moveStart('character', start);
        range.select();
    }
};
```
