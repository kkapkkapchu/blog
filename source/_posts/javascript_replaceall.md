---
title: 자바스크립트 replaceAll 하기
categories:
    - Web
    - JavaScript
tags:
    - replace
    - replaceall
date: 2018-10-17 14:05:00
---

자바스크립트에서 replaceAll 은 없다.
정규식을 이용하여 대상 스트링에서 모든 부분을 수정해 줄 수 있다.

### replace 이용
```bash
let str = "aaa#bbb#ccc";

// #를 공백으로 변경한다.
str.replace("#",""); // aaabbb#ccc
```
replace 를 이용하면 첫 번째 # 만 공백으로 변경하고 나머지는 변경이 되지 않는다.

### 정규식을 이용해서 g로 감싸기
```bash
let str = "aaa#bbb#ccc";

// #를 감싼 따옴표를 슬래시(/)로 대체하고 뒤에 gi 를 붙이면 
// replaceAll 과 같은 결과를 볼 수 있다.
str.replace(/#/g, ""); // aaabbbccc
```

### 정규식 사용법
* g : 발생할 모든 pattern에 대한 전역 검색
* i : 대/소문자 구분 안함
* m : 여러 줄 검색 (참고)