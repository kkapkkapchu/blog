---
title: "[CSS] Invalid property value"
date: 2019-01-03 14:47:00
categories:
  - Web
  - CSS
tags:
  - property
---

## CSS 속성에 Invalid property value 가 뜰 때

마크업 작업을 하다가 분명히 `margin-top` 속성을 줬는데 속성이 먹히지도 않고
개발자 도구로 보면 노란색 느낌표가 뜨면서 해당 속성에 취소선이 그어져 있다.

![image](https://user-images.githubusercontent.com/30403198/50624844-1af7b400-0f67-11e9-8bda-5c60344474f1.png)

느낌표에 마우스를 갖다대면 `Invalid property value` 라는 툴팁이 뜬다.

하... 진짜 이거때매 한참을 헤맸는데... ㅠㅠ

리액트를 너무 오래썼나보다..
리액트 문법을 css 에서 사용하니 당연히 안되지 ㅠ.ㅠ

```css
div {
  margin-top: "33px"; // 이렇게 쓰면 사진처럼 나오고 속성이 안먹힘 ㅠㅠ
  margin-top: 33px; // 이렇게 써야함!!
}
```

css 에서는 `''`로 감싸지 않고 바로 33px 쓰면 된다.

앞으로 또 저런 이슈를 보게 되면 속성을 잘못 사용하지 않았는지 꼭 확인해보자!
