---
title: 자바스크립트 클로저(Closure)에 대해
categories:
    - Web
    - JavaScript
tags:
    - closure
date: 2018-07-07 23:02:00
---

### 클로저란 무엇인가
함수 내부에 함수를 작성할 때마다 이미 클로저를 생성한 것이다. 
내부에 작성한 함수가 바로 클로저라닛!! 여태껏 클로저가 뭔지도 모르고 사용했다..

```bash
function outerFunction () {
  const outer = 'I see the outer variable!'
  
  return function innerFunction() {
    console.log(outer)
  }
}
outerFunction()() // I see the outer variable!
```

### 클로저를 왜 쓸까
클로저는 외부함수의 변수에 접근이 가능하다. (자바스크립의 스코프를 이해한다면 당연한 것)
때문에 일반적으로 두 가지의 목적이 있다.

1. 사이드 이펙트(side effects) 제어하기
2. private 변수 생성하기(캡슐화)

##### 사이드 이펙트(side effects) 제어하기
함수에서 값을 반환할 때를 제외하고 무언가를 행할 때 사이드 이펙트(side effects)가 발생한다. 
여러 가지 것들이 사이드 이펙트가 될 수 있는데, 예를 들어 Ajax 요청이나 timeout을 생성할 때, 
그리고 심지어 console.log를 선언하는 것도 사이드 이펙트이다.

보통 Ajax나 timeout과 같이 코드 흐름을 방해하는 것들이 신경 쓰일 때, 
클로저를 활용하여 사이드 이펙트를 제어한다.

```bash
var i;
for (i = 0; i < 10; i++) {
  setTimeout(function() {
    console.log(i);
  }, 100);
}
```

간단하게 0-9까지의 정수를 출력하는 코드이지만 실제로 돌려보면 엉뚱하게도 10만 열 번 출력되는 걸 볼 수 있다. 왜일까?
먼저 `setTimeout()`에 인자로 넘긴 익명함수는 모두 0.1초 뒤에 호출될 것이다. 
그 0.1초 동안에 이미 반복문이 모두 순회되면서 `i`값은 이미 10이 된 상태. 
그 때 익명함수가 호출되면서 이미 10이 되어버린 `i`를 참조하는 것이다.

이 경우에도 클로저를 사용하면 원하는 대로 동작하도록 만들 수 있다.

```bash
var i;
for (i = 0; i < 10; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j);
    }, 100);
  })(i);
}
```

중간에 즉시 실행 함수(IIFE)를 덧붙여 `setTimeout()`에 걸린 익명함수를 클로저로 만들었다. 
클로저는 만들어진 환경을 기억한다. 이 코드에서 `i`는 IIFE내에 `j`라는 형태로 주입되고, 클로저에 의해 각기 다른 환경속에 포함된다. 
반복문은 10회 반복되므로 10개의 환경이 생길 것이고, 10개의 서로 다른 환경에 10개의 서로 다른 `j`가 생긴다.

위의 예제에서는 IIFE를 통해서 클로저마다 환경이 생긴다. 
하지만 인자로 `i`를 넘기지 않는다면 당연히 클로저가 참조하는 IIFE의 함수 스코프에서도 `i`값이 없으므로 생성 당시의 외부 스코프인 글로벌을 탐색하게 되고 결국 모두 같은 `i`를 참조하게 된다. 
반면에, 인자로 `i`를 넘기게 되면 IIFE로 만든 10개의 스코프에 모두 `i`라는 변수가 다른 값으로 생기므로 정상적으로 동작할 수 있는 것이다.

참고로 여기서 콜백으로 넘기는 함수 자체를 IIFE로 만들면 되지 않나 싶지만, 
그렇게 하면 원하는대로 0-9까지 출력은 되지만 함수 내부가 즉시 실행되어 버리므로 setTimeout()의 0.1초 딜레이가 작동하지 않게 된다.

##### private 변수 생성하기(캡슐화)
일반적으로 JavaScript에서 객체지향 프로그래밍을 말한다면 `Prototype`을 통해 객체를 다루는 것을 말한다.
`Prototype`을 통한 객체를 만들 때의 주요한 문제 중 하나는 Private variables에 대한 접근 권한 문제이다. 예제 코드를 보자.

```bash
function Hello(name) {
  this._name = name;
}

Hello.prototype.say = function() {
  console.log('Hello, ' + this._name);
}

var hello1 = new Hello('피카츄');
var hello2 = new Hello('이상해씨');
var hello3 = new Hello('잠만보');

hello1.say(); // 'Hello, 피카츄'
hello2.say(); // 'Hello, 이상해씨'
hello3.say(); // 'Hello, 잠만보'
hello1._name = 'anonymous';
hello1.say(); // 'Hello, anonymous'
```

위에서 `Hello()`로 생성된 객체들은 모두 `_name`이라는 변수를 가지게 된다. 
변수명 앞에 underscore(_)를 포함했기 때문에 일반적인 JavaScript 네이밍 컨벤션을 생각해 봤을때 
이 변수는 Private variable으로 쓰고싶다는 의도를 알 수 있다. 
하지만 실제로는 여전히 외부에서도 쉽게 접근가능한 변수일 뿐이다.

이 경우에 클로저를 사용하여 외부에서 변수에 직접 접근하는 것을 제한할 수 있다.

```bash
function hello(name) {
  var _name = name;
  return function() {
    console.log('Hello, ' + _name);
  };
}

var hello1 = hello('피카츄');
var hello2 = hello('이상해씨');
var hello3 = hello('잠만보');

hello1(); // 'Hello, 피카츄'
hello2(); // 'Hello, 이상해씨'
hello3(); // 'Hello, 잠만보'
```
특별히 인터페이스를 제공하는 것이 아니라면, 여기서는 외부에서 _name에 접근할 방법이 전혀 없다. 
이렇게 은닉화도 생각보다 쉽게 해결할 수 있다.


### 클로저의 성능
클로저는 각자의 환경을 가진다. 이 환경을 기억하기 위해서는 당연히 메모리가 소모될 것이다.
클로저를 생성해놓고 참조를 제거하지 않는 것은 C++에서 동적할당으로 객체를 생성해놓고 delete를 사용하지 않는 것과 비슷하다. 
클로저를 통해 내부 변수를 참조하는 동안에는 내부 변수가 차지하는 메모리를 GC가 회수하지 않는다. 
따라서 클로저 사용이 끝나면 참조를 제거하는 것이 좋다.

```bash
function hello(name) {
  var _name = name;
  return function() {
    console.log('Hello, ' + _name);
  };
}

var hello1 = hello('피카츄');
var hello2 = hello('이상해씨');
var hello3 = hello('잠만보');

hello1(); // 'Hello, 피카츄'
hello2(); // 'Hello, 이상해씨'
hello3(); // 'Hello, 잠만보'

// 여기서 메모리를 release 시키기 클로저의 참조를 제거해야 한다.
hello1 = null;
hello2 = null;
hello3 = null;
```

이처럼 메모리 관리에 있어서 약점이 있지만 추가로 스코프 체인을 검색하는 시간과 새로운 스코프를 생성하는데 드는 비용도 감안하지 않을 수 없다. 
이 부분에 대해서는 JavaScript 스코프를 주제로 하는 별도의 글에서 다룰 것이다.


### 참고 문서
- [JavaScript 클로저(Closure)](https://hyunseob.github.io/2016/08/30/javascript-closure/)
- [자바스크립트 스코프와 클로저](https://medium.com/@khwsc1/%EB%B2%88%EC%97%AD-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%8A%A4%EC%BD%94%ED%94%84%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%80-javascript-scope-and-closures-8d402c976d19)