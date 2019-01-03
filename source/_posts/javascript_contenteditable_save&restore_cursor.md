---
title: "[Javascript] contenteditable 의 커서 위치 저장/복구 하기"
categories:
  - Web
  - Javascript
tags:
  - contenteditable
  - cursor
  - caret
date: 2019-01-03 15:03:00
---

## contenteditable 의 커서 위치 저장/복구 안됐던 이유

지난번에 `contenteditable` 커서 관련하여 [쓴 글](https://kkapkkapchu.github.io/blog/2018/10/17/javascript_contenteditable_set_cursor_to_end/)이 있다.

이 때는 커서 위치를 저장하고 복구 시키려다가 실패해서 결국 맨 끝으로 보내게 했는데
안 되었던 이유를 알았다.

그것은 바로 `contenteditable` 의 `내용(innerHTML/innerText)` 을 바꾸기 전에 입력 창을 `blur` 시키고, 바꾸고 나서 커서 위치를 복구 시킬때 `focus` 를 강제로 해줘야 하는데 안해서 그랬던 것!!!

사실 문서에 다 써있었는데 대충 읽어서 그 부분만 빼고 테스트하다 실패한 것..ㅠ
문서 좀 제대로 읽자...!

## contenteditable 의 커서 위치 저장/복구 하기

커서 위치 저장/복구 하는 방법은 [여기에](http://blog.naver.com/PostView.nhn?blogId=writer0713&logNo=220499119278) 아주 잘 나와있다.
그대로만 따라하면 잘 됨.

나는 `set/get` 의 의미가 좀 헷갈려서 `save/restore` 로 바꾸었다.
그리고 `restore` 할 때 에러가 발생하는 경우가 있어서 추가로 예외처리를 해주었다.

```js
saveCaret(node) {
    const caretID = 'caret'
    const cc = document.createElement('span')
    cc.id = caretID

    window
    .getSelection()
    .getRangeAt(0)
    .insertNode(cc)

    node.blur()
},
restoreCaret(node) {
    const caretID = 'caret'
    const range = document.createRange()
    const cc = document.getElementById(caretID)

    node.focus()

    if (!cc) {
        return
    }

    range.selectNode(cc)
    const selection = window.getSelection()
    selection.removeAllRanges()
    selection.addRange(range)
    range.deleteContents()
}
```

내용을 바꾸기 전에 `saveCaret` 으로 마지막 선택된 커서 위치를 저장하고
내용을 바꾼 후 `restoreCaret` 으로 커서 위치를 복구시킨다.
