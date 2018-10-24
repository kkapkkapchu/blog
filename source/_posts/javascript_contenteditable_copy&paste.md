---
title: "[Javascript] contenteditable 에 복사한 데이터 붙여넣을 때 텍스트만 넣기"
categories:
    - Web
    - Javascript
tags:
    - contenteditable
    - copy&paste
    - execCommand
date: 2018-10-17 17:02:00
---

`contenteditable` 에 복사한 데이터를 붙여넣으면 복사했을 때의 스타일(CSS)까지 그대로 적용된다.
아래 코드를 실행시키고 스타일이 들어간 텍스트를 복사&붙여넣기 해보자.

```jsx
render() {
    return (
        <p contenteditable="true">
            This is a paragraph. It is editable. Try to change this text.
        </p>
    )
}
```

결과는 다음과 같이 나올 것이다.
![스타일(CSS)까지 붙여넣기 된 모습](https://user-images.githubusercontent.com/30403198/47073323-f2e39900-d232-11e8-9412-2df1c57b97ec.png)


이렇게 나오면 안되기 때문에 텍스트만 붙여넣을 수 있도록 작업을 해줘야 한다.
아래와 같이 하면 된다.

```jsx
/**
 * 텍스트를 붙여넣기 할 때 발생하는 함수입니다.
 */
handlePaste = (event) => {
    event.preventDefault();

    let pasteText = (event.originalEvent || event).clipboardData.getData('text/plain');
    window.document.execCommand('insertText', false, pasteText);
}

render() {
    return (
        <p contenteditable="true" onPaste={this.handlePaste}>
            This is a paragraph. It is editable. Try to change this text.
        </p>
    )
}
```

이렇게 붙여넣기 이벤트를 잡아서 `preventDefault`로 기본 동작을 막고 `execCommand` 처리를 해주면 아래 사진과 같이 텍스트만 잘 나오게 된다.
![텍스트만 붙여넣기 된 모습](https://user-images.githubusercontent.com/30403198/47073945-56ba9180-d234-11e8-9022-b3134f3659d3.png)


---
하지만 위의 방식은 IE를 지원하지 않는다. IE 를 지원하는 방법은 [다음 포스팅](https://kkapkkapchu.github.io/blog/2018/10/24/javascript_contenteditable_copy&paste_IE/)을 참고하자.