---
title: "[Vue] Recursive Component"
categories:
  - Web
  - Vue
tags:
  - recursive
date: 2019-07-30 16:00:00
---

### 재귀 컴포넌트 (Recursive Component)

재귀 함수처럼 컴포넌트 자체를 재귀적으로 사용해야 할 때가 있다.
`재귀 컴포넌트(Recursive Component)`는 템플릿에서 자기 자신을 호출하는 컴포넌트이다. 반드시 `name` 옵션을 지정해야한다.

```html
<!-- child.vue -->
<template>
  <ul>
    <li v-for="(a, index) in examlist">
      <recursive :examlist="a.examlist"></recursive>
    </li>
  </ul>
</template>

<script>
  export default {
    name: "recursive",
    props: ["examlist"]
  };
</script>
```

```html
<!-- parent.vue -->
<template>
  <div>
    <recursive :examlist="list"></recursive>
  </div>
</template>

<script>
  export default {
      name: 'parent',
      components : { Child },
      data () {
          return {
              list : {
                  title: "title1", type: "title"
                  examlist: [
                      {
                          title: "subtitle1", type: "subtitle",
                          examlist: [
                              { title: "A", type: "item" },
                              { title: "B", type: "item" },
                          ]
                      },
                      {
                          title: "subtitle2", type: "subtitle",
                          examlist: [
                              { title: "C", type: "item" },
                              { title: "D", type: "item" },
                          ]
                      },
                      {
                          title: "subtitle3", type: "subtitle",
                          examlist: [
                              { title: "E", type: "item" },
                              { title: "F", type: "item" },
                          ]
                      }
                  ]
              }
          }
      }
  }
</script>
```

### 참고사이트

- [Vue - 재귀 컴포넌트](https://kr.vuejs.org/v2/guide/components.html#%EC%9E%AC%EA%B7%80-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)
- [[Vue.js Quick Start] Webpack, 컴포넌트 심화](https://mkki.github.io/vue.js/js/2018/05/06/start-vuejs-9.html)
