---
title: "[Javascript] axios 로 API 호출 시 request params 보내기"
categories:
    - Web
    - Javascript
    - Library
tags:
    - axios
    - params
date: 2018-12-10 14:31:00
---

### axios 라이브러리
axios 라이브러리가 뭔지는 [git 주소](https://github.com/axios/axios) 를 참고하자.

### axios 로 API 호출하기
- axios.request(config)
- axios.get(url[, config])
- axios.delete(url[, config])
- axios.head(url[, config])
- axios.options(url[, config])
- axios.post(url[, data[, config]])
- axios.put(url[, data[, config]])
- axios.patch(url[, data[, config]])

### axios 로 API 호출 시 request params 보내기
`request params` 은 `path` 뒤에 붙게 되는 쿼리 파라미터를 뜻한다.

`request params` 는 전달 인자의 `config` 부분에 넣어주면 된다.

```js
// GET / DELETE
axios.get(url, {
    params: {
        key: value
    }
})

// POST / PUT
axios.post(url, data, {
    params: {
        key: value
    }
})
```

`body` 에 보낼 `data` 가 없다면 빈 값('')으로 보내주면 된다.
