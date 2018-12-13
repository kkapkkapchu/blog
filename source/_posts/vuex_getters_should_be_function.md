---
title: "[Vuex] Error: getters should be function" 
categories:
    - Web
    - Vue
    - Vuex
tags:
    - getters
date: 2018-12-13 14:10:00
---

### Vuex - store 와 모듈
Vuex 에서 store를 정의하려면 `Vuex.Store()` 로 store 인스턴스를 생성해야 한다.

```js
import Vuex from 'vuex'

const store = new Vuex.Store({ ...options })
```

`...options` 부분에는 `state, getters, mutations, actions` 등이 들어간다.


store 에 정의할 것이 많아지면 모듈화를 하게 되는데 이 때 `store` 인스턴스가 두 번 생성되지 않도록 조심해야 한다.

모듈화를 할 때는 `store` 인스턴스를 내보내는 `index.js` 한 군데에서만 하면 된다.

각 모듈 안에서 `store` 인스턴스를 또 생성하게 되면 아래와 같은 에러가 발생한다.
![error](https://user-images.githubusercontent.com/30403198/49917496-beaef080-fee2-11e8-9265-490e156bb8b5.png)


### 결론
해당 에러 발생 시 `Vuex.Store()` 가 중복되어 사용되지 않았는지 확인해 볼 것.
`store` 인스턴스는 한 번만 생성한다.