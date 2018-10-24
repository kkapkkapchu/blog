---
title: "[CSS] CSS에서 calc 함수 중첩해서 쓰기"
date: 2017-08-03 20:31:27
categories:
    - Web
    - CSS
tags:
    - IE
---
## Calc() function inside another calc() in CSS
``` bash
// for Chrome, IE 9
div{
  width: calc(100% - (1% + 30px)); /* calc(1% + 30px) is nested inside calc() */
}

// for Chrome
div p{
  width: calc(100% - calc(1% + 30px));
}
```

크롬은 둘 다 지원하지만 IE는 첫 번째만 지원한다.
IE에서 calc 함수를 중첩으로 사용하려면 맨 앞에 calc로 한번만 감싸야한다.

이를 위해서 안의 중첩된 calc 글자를 없애주는 라이브러리([reduce-css-calc](https://github.com/MoOx/reduce-css-calc))도 있는거 같은데,
이 라이브러리 안에서 쓰이는 "fs" 라이브러리가 없어진듯 하다... 그래서 못씀..

아무튼 해결법은 calc를 한번만 사용하도록 식을 바꾸든지, 중첩된 calc 를 계산해서 숫자로 바꿔주는 라이브러리를 만들든지 찾든지 해야한다.
