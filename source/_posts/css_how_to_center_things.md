---
title: "[CSS] CSS로 내용물 가운데 정렬하기"
date: 2019-01-10 15:06:00
categories:
  - Web
  - CSS
tags:
  - center
  - align
---

## 가운데 정렬 (가로)

### Text

텍스트는 `text-align` 속성을 이용한다.

```css
p {
  text-align: center;
}
```

### Blocks

텍스트가 아닌 블록의 형태는 `Flexbox` 속성을 이용한다.

```css
div {
  display: flex;
  justify-content: center;
}
```

만약 `Flexbox` 속성을 이용하지 못한다면 다른 방법이 있다.

```css
div {
  margin: 0 auto;
  width: 50%;
}
```

만약 가운데로 정렬하려는 요소가 inline 요소라면 위와 같은 방법을 사용할 때 `display: block;` 속성을 추가로 줘야한다.

## 가운데 정렬 (세로)

`Flexbox` 는 매우 간편하고 좋은 방법이 있다.

```css
div {
  display: flex;
  align-items: center;
}
```

## 가운데 정렬 (가로&세로)

화면의 가로와 세로 모두의 정 중앙에 정렬하고 싶다면 앞의 내용들을 합치면 된다.

```css
div {
  display: flex;
  align-items: center;
  justify-content: center;
}
```

또는 `display: grid;` 를 이용해서도 할 수 있다.

```css
body {
  display: grid;
  place-items: center;
  height: 100vh;
}
```

## 출처

[HOW TO CENTER THINGS WITH CSS](https://flaviocopes.com/css-centering/)
