---
title: "[Vue] IE Support"
categories:
  - Web
  - Vue
tags:
  - ie
date: 2019-07-30 14:00:00
---

### Vue transpileDependencies

프로젝트 중 IE 를 지원하지 않는 [matcher](https://www.npmjs.com/package/matcher) 라이브러리를 사용해서 익스플로러 브라우저 [오류](https://github.com/sindresorhus/matcher/issues/17)가 발생했었다.

이럴때는 `vue.config.js`에서 [transpileDependencies](https://cli.vuejs.org/config/#transpiledependencies) 속성에 정의해주면 된다.

```js
// vue.config.js
module.exports = {
  transpileDependencies: ["matcher"]
};
```

이렇게만 하면 해당 라이브러리를 참조할 수 없다고 에러가 뜨는데 아래 코드를 `babel.config.js`에 추가해주면 된다.

```js
// babel.config.js
module.exports = {
  presets: [
    [
      "@vue/app",
      {
        useBuiltIns: "entry"
      }
    ]
  ]
};
```

#### Unknown option: .useBuiltIns. in babel.config.js

여기서 주의할 점은
`["@vue/app",{useBuiltIns: "entry"}]` 가 아니라
`[["@vue/app",{useBuiltIns: "entry"}]]` 이 되어야 한다.

잘 동작하지 않는다면 배열로 잘 감싸졌는지 확인해보자.

### `Promise` 이(가) 정의되지 않았습니다.

http 통신을 위해 주로 `Axios` 를 사용하는데 이 라이브러리도 IE 지원이 안된다.
이럴때는 `es6-promise` 를 적용하면 된다.

```bash
# es6-promise 설치
npm install es6-promise --save
```

라이브러리 설치 후 `main.js` 최상단에 import 한다. (주의할 것은 `vuex` 나 `axios` 보다 먼저 import 가 되어야 정상동작 한다.)

```js
// main.js
import "es6-promise/auto";
```

### `Includes` 이(가) 정의되지 않았습니다.

그 외에 흔히 사용하는 자바스크립트 함수 중 IE가 지원하지 않는 것이 있다면 `babel-polyfill` 을 적용하면 된다.

```bash
# babel-polyfill 설치
npm install babel-polyfill --save
```

라이브러리 설치 후 마찬가지로 `main.js` 최상단에 import 한다.

```js
// main.js
import "babel-polyfill";
import "es6-promise/auto";
```

### 참고사이트

- [IE에서 Vue.js 사용하기](https://lovemewithoutall.github.io/it/vue-ie-support-with-es6-promise/)
- [Unknown option: .useBuiltIns. in babel.config.js](https://github.com/vuejs/vue-cli/issues/2594)
