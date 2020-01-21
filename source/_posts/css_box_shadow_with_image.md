---
title: "[CSS] box-shadow 에 이미지 넣기"
date: 2020-01-21 14:15:00
categories:
  - Web
  - CSS
tags:
  - box-shadow
---

### box-shadow 에 이미지 넣기 CSS

```css
div {
  background: transparent;
  background-image: url("top-shadow.png"), url("bottom-shadow.png");
  background-repeat: no-repeat;
  background-position: left top, right bottom;
}
```

### 참고사이트

- [box-shadow-with-image](https://css-tricks.com/forums/topic/how-to-div-box-shadow-with-image/#post-86314)
