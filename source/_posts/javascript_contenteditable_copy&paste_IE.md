---
title: "[Javascript] contenteditable 에 복사한 데이터 붙여넣을 때 텍스트만 넣기 (IE 지원)"
categories:
    - Web
    - Javascript
tags:
    - contenteditable
    - copy&paste
    - execCommand
    - IE
date: 2018-10-24 15:28:00
---

아래 코드는 `contenteditable` 에 복사한 데이터 붙여넣을 때 텍스트만 넣고 싶을 때 `IE` 도 지원하는 코드이다.

```jsx
insertText(text) {
    // use insertText command if supported
    if (document.queryCommandSupported('insertText')) {
        document.execCommand('insertText', false, text);
    }
    // or insert the text content at the caret's current position
    // replacing eventually selected content
    else { // for IE
        let range = document.getSelection().getRangeAt(0);
        range.deleteContents();
        let textNode = document.createTextNode(text);
        range.insertNode(textNode);
        range.selectNodeContents(textNode);
        range.collapse(false);

        let selection = window.getSelection();
        selection.removeAllRanges();
        selection.addRange(range);
    }
};

/**
 * 텍스트를 붙여넣기 할 때 발생하는 함수입니다.
 */
handlePaste = (event) => {
    event.preventDefault();

    let pasteText = "";

    let agent = navigator.userAgent.toLowerCase();
    if ( (navigator.appName == 'Netscape' && agent.indexOf('trident') != -1) || (agent.indexOf("msie") != -1)) {
        // ie일 경우
        pasteText = window.clipboardData.getData('Text');
    } else {
        // ie가 아닐 경우
        pasteText = (event.originalEvent || event).clipboardData.getData('text/plain');
    }

    this.insertText(pasteText);
}

render() {
    return (
        <p contenteditable="true" onPaste={this.handlePaste}>
            This is a paragraph. It is editable. Try to change this text.
        </p>
    )
}
```
