---
title: 자바스크립트 contenteditable 에 복사한 데이터 붙여넣을 때 텍스트만 넣기
categories:
    - Web
    - JavaScript
tags:
    - contenteditable
    - copy&paste
    - execCommand
date: 2018-10-17 17:02:00
---

`contenteditable` 에 복사한 데이터를 붙여넣으면 복사했을 때의 스타일(CSS)까지 그대로 적용된다.

아래 사진은 타이틀에 있는 텍스트를 복사해 `contenteditable` 로 되어있는 댓글 입력 창에 붙여넣기 한 모습이다.
![스타일(CSS)까지 붙여넣기 된 모습](https://user-images.githubusercontent.com/30403198/47071167-d2651000-d22d-11e8-93fd-baf44a6e85a8.png)

이렇게 나오면 안되기 때문에 텍스트만 붙여넣을 수 있도록 작업을 해줘야 한다.
아래와 같이 하면 된다.

```bash
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
        <div
            className="reply_inputbox"
            contentEditable
            dangerouslySetInnerHTML={{__html: html}}
            onPaste={this.handlePaste}
        ></div>
        <span className="reply_placeholder">댓글을 입력하세요.</span>
    )
}
```

이렇게 붙여넣기 이벤트를 잡아서 `execCommand` 처리를 해주면 아래 사진과 같이 텍스트만 잘 나오게 된다.
![텍스트만 붙여넣기 된 모습](https://user-images.githubusercontent.com/30403198/47071993-fe819080-d22f-11e8-9f29-d46c83e854ba.png)
