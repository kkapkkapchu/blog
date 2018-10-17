---
title: 자바스크립트 contenteditable 의 커서 맨 뒤로 보내기
categories:
    - Web
    - JavaScript
tags:
    - contenteditable
    - cursor
    - caret
date: 2018-10-17 14:17:00
---

`contenteditable` 속성을 사용하면 표준 HTML5 요소들이 편집 가능한 상태가 된다.

댓글 컴포넌트를 개발하면서 div 에 `contenteditable` 속성을 사용했다. 
이유는 글자 수가 늘어날수록 입력창도 같이 늘어나기 때문.

개발하면서 몇 가지 이슈가 있었는데 정리해보자.

### 발생했던 이슈
1. `contenteditable` 속성을 사용한 div 내부에 힌트텍스트 span 요소가 들어감
2. keydown/keyup 에서 한글은 `preventDefault()` 가 안먹힘
3. contenteditable 의 커서 맨 뒤로 보내기

### 1. contenteditable 속성을 사용한 div 내부에 힌트텍스트 span 요소가 들어감
댓글 내용이 비었을 때 댓글을 입력하라는 내용의 힌트텍스트를 보여주어야 했다.
전달받은 퍼블리싱 결과물은 아래 코드의 구조처럼 되어있었다.
```bash
render() {
    return (
        <div contenteditable>
            <span className="reply_placeholder">댓글을 입력하세요.</span>
        </div>
    )
}
```

하지만 이렇게 하면 웹 브라우저 개발자 도구에서 아래 사진과 같은 오류가 발생한다.
![warning](https://user-images.githubusercontent.com/30403198/47065052-b2792080-d21c-11e8-8211-552135d84ac8.png)

리액트에서는 `contenteditable` 속성을 사용한 태그 안에 자식 요소를 넣으면 저렇게 warning 을 띄우나 보다.

이 이슈는 `contenteditable` 속성을 사용한 태그를 빈 태그로 사용하고, 
`class="reply_placeholder"` 스타일을 `position: absolute` 해서 위치 잡는 것으로 바꿔달라고 요청해서 css 를 수정하는 것으로 해결했다.

```bash
render() {
    return (
        <div
            className="reply_inputbox"
            contentEditable
            dangerouslySetInnerHTML={{__html: html}}
        ></div>
        <span className="reply_placeholder">댓글을 입력하세요.</span>
    )
}
```

### 2. keydown/keyup 에서 한글은 preventDefault() 가 안먹힘
컴포넌트 기획에 maxLength 가 있어 글자 수 초과시 입력을 막아야 했다.
그런데 keydown/keyup 에서 한글은 `preventDefault()` 가 먹히질 않았다.

찾아보니 이유는 `compositionstart -> keydown` 순서로 이벤트가 발생하기 때문에 한글이 이미 입력된 상태여서 `preventDefault()`를 해도 소용이 없었던 것.
`compositionstart` 에서 `preventDefault()` 를 시도했으나 안됨...

한글 입력 자체를 막는 `style="ime-mode:disabled"` 는 기본적인 크롬에서 지원 안하므로 패스.


`preventDefault()` 가 되지 않으니 초과된 글자를 입력 후 keyup 에서 잘라내도록 했다.
문제는 html 을 setState 하니까 안에 내용물이 바뀌게 되어서 글자 입력시마다 커서가 맨앞으로 이동하는 이슈가 생겼다.

이 이슈를 해결하기 위해 잘라내기 전 커서 위치를 저장하고 복원하는 방법을 여러가지 시도했지만 
결국 내용 자체가 바뀌게 되면서 기존에 저장해놨던 커서 위치가 날아감...ㅠ

### 3. `contenteditable` 의 커서 맨 뒤로 보내기
결국 최대 글자 수 초과 시 얼럿을 띄우고 커서를 맨 뒤로 보내기로 했다.

`contenteditable` 에서 커서를 맨 뒤로 보내는 방법은 아래 코드를 참고하면 된다.  

```bash
/**
 * 커서를 맨 뒤로 보내는 함수입니다.
 * @param contentEditableElement 커서를 움직일 노드입니다.
 */
export function setEndOfContenteditable(contentEditableElement) {
    let range,selection;

    if (document.createRange) {         //Firefox, Chrome, Opera, Safari, IE 9+
        range = document.createRange(); //Create a range (a range is a like the selection but invisible)
        range.selectNodeContents(contentEditableElement);//Select the entire contents of the element with the range
        range.collapse(false);          //collapse the range to the end point. false means collapse to end rather than the start
        selection = window.getSelection();//get the selection object (allows you to change selection)
        selection.removeAllRanges();    //remove any selections already made
        selection.addRange(range);      //make the range you have just created the visible selection
    } else if (document.selection) {    //IE 8 and lower
        range = document.body.createTextRange();         //Create a range (a range is a like the selection but invisible)
        range.moveToElementText(contentEditableElement); //Select the entire contents of the element with the range
        range.collapse(false);          //collapse the range to the end point. false means collapse to end rather than the start
        range.select();                 //Select the range (make it the visible selection
    }
}
```