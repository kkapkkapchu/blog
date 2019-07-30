---
title: "[Vue] Plugin with Template"
categories:
  - Web
  - Vue
tags:
  - plugin
  - template
date: 2019-07-30 16:00:00
---

### Vue Plugin

Vue.js는 외부 라이브러리를 쉽게 불러올 수 있도록 표준 플러그인 기능을 제공한다.
[가이드 문서](https://kr.vuejs.org/v2/guide/plugins.html)가 아주 잘 나와있어서 그대로 따라하면 된다.

```js
MyPlugin.install = function (Vue, options) {
  // 1. 전역 메소드 또는 속성 추가
  Vue.myGlobalMethod = function () {
    // 필요한 로직 ...
  }

  // 2. 전역 에셋 추가
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 필요한 로직 ...
    }
    ...
  })

  // 3. 컴포넌트 옵션 주입
  Vue.mixin({
    created: function () {
      // 필요한 로직 ...
    }
    ...
  })

  // 4. 인스턴스 메소드 추가
  Vue.prototype.$myMethod = function (options) {
    // 필요한 로직 ...
  }
}
```

#### 전역 메소드 VS 인스턴스 메소드

인스턴스 메소드 이름에는 앞에 `$` 구분자를 추가해 명시적으로 표기한다.

### Vue Plugin with Template

하지만 가이드 문서에서는 기능적인 부분만 다루고 있다.
나는 템플릿&스타일과 기능을 입힌 컴포넌트를 사용하는 플러그인을 만들어야 해서 찾아보았다.

1. 스타일을 입힌 템플릿을 만든다.
2. 플러그인에서 템플릿을 `import` 한다.
3. `Vue.extend` 로 생성자를 만든다.
4. `new 생성자이름({})` 으로 인스턴스를 만든다.
5. <span style="color:red">document.body.appendChild(인스턴스의 엘리먼트) 로 dom 위에 올린다.</span>

```js
// plugins/notification/index.js
import NotificationTemplate from "@/components/NotificationTemplate.vue";

const VueNotification = {
  install(Vue) {
    const NotiConstructor = Vue.extend(NotificationTemplate);
    let NotiInstance = null;

    function showSuccessMsg(msg, successMsg = "successfully") {
      if (!NotiInstance) {
        NotiInstance = new NotiConstructor({
          el: document.createElement("div")
        });

        document.body.appendChild(NotiInstance.$el);
      }

      const data = {
        type: "success",
        text: `<span class="color">:)</span> ${msg} <span class="color">${successMsg}!</span>`
      };

      NotiInstance.show(data);
    }

    function showErrorMsg(msg) {
      if (!NotiInstance) {
        NotiInstance = new NotiConstructor({
          el: document.createElement("div")
        });

        document.body.appendChild(NotiInstance.$el);
      }

      const data = {
        type: "error",
        text: `<span class="color">${msg}</span>`
      };

      NotiInstance.show(data);
    }

    Vue.showSuccessMsg = showSuccessMsg;
    Vue.showErrorMsg = showErrorMsg;

    Vue.prototype.$showSuccessMsg = showSuccessMsg;
    Vue.prototype.$showErrorMsg = showErrorMsg;
  }
};

export default VueNotification;
```

### 참고사이트

- [Vue - 플러그인](https://kr.vuejs.org/v2/guide/plugins.html)
- [Vue.js 플러그인 제작 가이드](http://vuejs.kr/jekyll/update/2017/01/13/vuejs-plugin/)
