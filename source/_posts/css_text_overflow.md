---
title: "[CSS] 말줄임 처리하기"
date: 2019-07-03 18:33:00
categories:
  - Web
  - CSS
tags:
  - text_overflow
---

### 1줄 일 때 말줄임 CSS

```css
div {
  text-overflow: ellipsis;
  white-space: nowrap;
  word-wrap: normal;
  width: 100px;
  overflow: hidden;
}
```

- **text-overflow**

  - 글자가 지정한 너비를 넘어갈경우 어떤식으로 처리할것인지를 선택하는 속성입니다.
  - `clip` : 기본값입니다.
  - `ellipsis` : 말줄임표로 처리합니다. `상위 요소의 너비가 auto로 되어있는 경우는 적용할 수 없습니다.`
  - `inherit` : 상위요소의 속성과 동일하게 사용합니다.

- **white-space**

  - 요소안에서 공백은 어떤식으로 처리할것인지를 선택하는 속성입니다. 공백은 줄바꿈(line-break), 들여쓰기(tab), 공백(space)입니다.
  - `normal` : 기본값입니다. 공백을 여러개 넣어도 공백을 1개로 처리합니다.
  - `nowrap` : 공백을 여러개 넣어도 1개로 처리합니다. 텍스트가 길어져도 `줄바꿈을 하지않고 1줄로 표시`합니다.
  - `pre` : 공백을 코드에 있는 그대로 표시합니다. 코드에 줄바꿈이 없으면 줄바꿈을 실행하지 않습니다.
  - `pre-wrap` : 공백을 코드에 있는 그대로 표시합니다. 코드에 줄바꿈이 없어도 자동으로 줄바꿈을 실행합니다.
  - `pre-line` : 공백을 여러개 넣어도 1개로 처리합니다. 코드에 줄바꿈이 없어도 자동으로 줄바꿈을 실행합니다. 코드에 줄바꿈이 있을때도 그대로 실행합니다.

- **word-wrap**
  - 긴 텍스트를 강제로 끊어서 줄바꿈을 해주는 속성입니다. (overflow 와 비슷합니다)
  - `normal` : 기본값입니다. 글자가 길어도 끊어지지 않고 한줄에 계속 표시
  - `break-word` : 강제로 끊어서 줄바꿈합니다.

### 2줄 이상일 때 말줄임 CSS

```css
div {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 3; /* 라인수 */
  -webkit-box-orient: vertical;
  word-wrap: break-word;
  line-height: 1.2em;
  height: 3.6em; /* line-height 가 1.2em 이고 3라인을 자르기 때문에 height는 1.2em * 3 = 3.6em */
}
```

-webkit-line-clamp 속성의 경우, 웹킷 엔진을 사용하지 않는 브라우져의 경우 문제가 될수 있으므로,

`line-height` 속성과 `height` 속성을 이요하여 높이를 계산하여 넣어줍니다. (height = line-height \* 줄수)

### 참고사이트

- [CSS 말줄임 처리하기](http://blog.tjsrms.me/css-%EB%A7%90%EC%A4%84%EC%9E%84-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0/)
