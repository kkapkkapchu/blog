---
title: "[Javascript] this에 대해"
categories:
    - Web
    - Javascript
tags:
    - this
date: 2018-10-04 20:17:00
---

### this 란 무엇인가
this 란 함수의 현재 실행 문맥이다.
자바스크립트에는 4가지의 함수 실행 타입이 있다.

1) 함수 실행: alert('Hello World!')
2) 메소드 실행: console.log('Hello World!')
3) 생성자 실행: new RegExp('\d')
4) 간접 실행: alert.call(undefined, 'Hello World!')

각각의 타입은 서로 다른 문맥을 가지며, 엄격 모드(strict mode) 역시 실행 문맥에 영향을 끼친다.
this 를 이해하려면 함수 실행이 문맥에 어떤 영향을 미치는지 확실히 이해해야 한다.
함수 호출이 this 에 어떤 영향을 미치는지 알아보자.

### 1. 함수 실행
함수 실행은 `alert('Hello World!')` 와 같이 `함수이름()`으로 호출하는 것이다.

> 함수 실행에서의 this 는 전역 객체이다.

전역 객체는 실행 환경에 따라 결정된다. 웹 브라우저에서는 `window`가 전역 객체이다.
함수 실행에서의 실행 문맥은 전역 객체이다.

```jsx
function sum(a, b) {
   console.log(this === window); // => true
   this.myNumber = 20; // 전역 객체에 'myNumber'라는 속성을 추가
   return a + b;
}
// sum()은 함수 호출이다.
// sum()에서의 this는 전역 객체다. (window)
sum(15, 16);     // => 31
window.myNumber; // => 20
```

this가 함수 스코프 밖(최상단: 전역 실행 문맥)에서 사용되었을 경우, 여기서의 this 역시 전역 객체를 참조하게 된다.
```jsx
console.log(this === window); // => true
this.myString = 'Hello World!';
console.log(window.myString); // => 'Hello World!'
```

#### 엄격 모드
> 엄격 모드에서 함수 실행에서의 this는 undefined 이다.

엄격 모드는 코드 안정성과 더 나은 오류 검증을 제공하기 위해 ECMA Script 5.1 버전에서 처음 소개 되었다. 엄격 모드로 작성하기 위해 함수 내부의 최상단에 'use strict'라는 예약어를 적는다. 이 모드는 실행 문맥인 this를 undefined로 만든다. 실행 문맥은 더이상 전역 객체로 되지 않고, 위의 케이스와 반대의 상황이 된다.

엄격 모드로 실행되는 예제 코드
```jsx
function multiply(a, b) {
  'use strict'; // 엄격 모드
  console.log(this === undefined); // => true
  return a * b;
}
// multiply() 함수는 엄격 모드로 실행됨
// multiply()에서의 this는 undefined
multiply(2, 5); // => 10
```
multiply(2, 5) 함수가 실행될 때 this는 undefined다.

엄격 모드는 현재 스코프 뿐만 아니라 내부 스코프에서도 적용된다. (내부에 정의된 모든 함수에 적용됨)

#### 내부 함수에서 this 를 사용할 때
함수를 실행할 때 흔히 하는 실수가 외부 함수에서의 this와 내부 함수에서의 this를 동일하게 생각하는 것이다.

사실 내부 함수의 문맥은 외부 함수의 문맥에 의존되는 게 아니라 오직 실행 환경에 좌우된다. 기대하는 되로 this가 동작되려면 수정이 필요하다. (call이라 apply 메소드를 사용하는 간접 실행, 또는 바인딩 함수를 적용)

```jsx
var numbers = {
   numberA: 5,
   numberB: 10,
   sum: function() {
     console.log(this === numbers); // => true
     function calculate() {
       // this는 window, 엄격 모드였으면 undefined
       console.log(this === numbers); // => false
       return this.numberA + this.numberB;
     }
     return calculate();
     // 문맥을 수정하기 위해 .call() 메소드를 적용해야함
     // return calculate.call(this);
   }
};
numbers.sum(); // NaN, 엄격 모드였으면 TypeError
```

### 메소드 실행
메소드는 객체의 속성으로 있는 함수이다.

```jsx
var myObject = {
  // helloFunction is a method
  helloFunction: function() {
    return 'Hello World!';
  }
};
var message = myObject.helloFunction(); // 메소드 실행
```
여기서 `helloFunction`는 myOjbect의 메소드다. 메소드에 접근하기 위해서는 `myObject.helloFunction`와 같은 속성 접근자를 이용하면 된다.

메소드 실행은 속성 접근자 형태의 표현식이 함수 객체로 계산되면서 실행된다. 

앞서 설명한 함수 실행과 메소드 실행이 다르다는 점은 중요하다. 왜냐하면 둘은 서로 다른 타입이기 때문이다. 둘의 가장 큰 차이점은 메소드 실행은 속성 접근자를 통해 function (.functionProperty() or 'functionProperty')를 호출한다. 반면에 함수 실행은 속성 접근자를 사용하지 않고, (())와 같이 바로 호출한다.

#### 메소드 실행에서의 this
> 메소드 실행에서의 this는 메소드를 소유하고 있는 객체이다.

객체 내에 있는 메소드를 실행할 때, 여기서의 this는 객체 자신이다.
```jsx
var calc = {
  num: 0,
  increment: function() {
    console.log(this === calc); // => true
    this.num += 1;
    return this.num;
  }
};
// 메소드 실행. 여기서의 this는 calc.
calc.increment(); // => 1
calc.increment(); // => 2
```

ECMAScript 6의 `class` 예약어에서 메소드 실행 문맥은 위와 마찬가지로 인스턴스 자신을 가리킨다.
```jsx
class Planet {
  constructor(name) {
    this.name = name;
  }
  getName() {
    console.log(this === earth); // => true
    return this.name;
  }
}
var earth = new Planet('Earth');
// 메소드 실행. 여기서의 this는 earth.
earth.getName(); // => 'Earth'
```

#### 객체로부터 메소드를 분리할 때
객체 내에 있는 메소드는 별도의 변수로 분리할 수 있다. 이 변수를 통해 메소드를 호출할 때, 당신은 아마도 여기서의 `this`가 메소드가 정의되어있는 객체라고 생각할 것이다.

객체 밖에 있는 메소드를 호출할 경우, 함수 실행을 한 결과와 같다. 함수 실행을 할 경우 `this`는 전역 객체인 `window`를 가리킨다. (엄격 모드에서는 undefined) `.bind()` 바인딩 함수를 사용해서 문맥을 수정할 경우, 메소드를 객체에 포함시킬 수 있다.

```jsx
function Animal(type, legs) {
  this.type = type;
  this.legs = legs;
  this.logInfo = function() {
    console.log(this === myCat); // => false
    console.log('The ' + this.type + ' has ' + this.legs + ' legs');
  }
}
var myCat = new Animal('Cat', 4);
// "The undefined has undefined legs" 출력
// 혹은 엄격모드라면 TypeError 출력
setTimeout(myCat.logInfo, 1000);

// .bind 메소드를 사용해 문맥을 강제로 지정시킬 수 있다
// setTimeout(myCat.logInfo.bind(myCat), 1000);
```

`.bind` 함수로 실행 문맥을 myCat 으로 지정해주면 함수 실행임에도 불구하고, 여기서의 this는 myCat을 가리키게 된다.

### 생성자 실행
생성자 실행은 표현식 앞에 new라는 키워드가 붙었을 때, 함수 객체로 계산되어 수행된다.

> 생성자 실행에서의 this 는 새롭게 만들어진 객체이다.

생성자 실행에서의 문맥은 새롭게 만들어진 객체다. 생성자 실행은 객체에 초기값을 셋팅하기 위해 사용된다. 초기값 셋팅의 예로는 생성자 함수의 매개 변수로 받은 데이터, 속성을 위한 환경 변수, 이벤드 핸들러 등이 있다.

```jsx
function Foo () {
  console.log(this instanceof Foo); // => true
  this.property = 'Default Value';
}
// 생성자 실행
var fooInstance = new Foo();
fooInstance.property; // => 'Default Value'
```

ES6에서 사용 가능한 `class` 문법 역시 같은 형식이다. 초기값 셋팅은 오직 생성자 메소드에서 할 수 있다.
```jsx
class Bar {
  constructor() {
    console.log(this instanceof Bar); // => true
    this.property = 'Default Value';
  }
}
// Constructor invocation
var barInstance = new Bar();
barInstance.property; // => 'Default Value'
```

생성자를 호출할 때에는 꼭 `new` 연산자를 사용해야 한다.

new 연산자를 사용하지 않으면 객체 안의 this는 함수 실행이 되므로 window 객체를 가리키게 된다. 그리고 Vehicle('Car', 4)은 속성을 window 객체에 추가한다. 잘못된 사용이다. 새로운 객체가 만들어지지 않는다.


## 결론
* `this` 는 함수의 실행 문맥이다.
* 함수 실행은 아래와 같이 4가지 타입으로 나뉘며, 이에 따라 함수의 실행 문맥이 달라진다.
  * 함수 실행 / 엄격 모드
  * 메소드 실행
  * 생성자 실행
  * 간접 실행
* 함수 실행에서의 `this` 는 전역 객체이다. 웹 브라우저에서는 `window` 이다.
* 엄격 모드에서 함수를 실행한다면, 이 함수의 `this` 는 `undefined` 이다.
* 메소드 실행에서의 `this` 는 메소드를 소유하는 객체이다.
* 생성자 실행에서의 `this` 는 새로 생성된 객체이다.
* 주의 - 메소드가 함수로 사용될 때 `.bind` 를 사용해 문맥 강제시키기.
* 주의 - 생성자 호출시에는 `new` 키워드 꼭 사용할 것.

### 참고 문서
- [자바스크립트에서 사용되는 this에 대한 설명 1](http://webframeworks.kr/tutorials/translate/explanation-of-this-in-javascript-1/)