---
title: "[React] propTypes 에 array 타입으로 지정할 때 array 안에 들어가는 값에 대한 타입 지정하기"
categories:
    - Web
    - React
tags:
    - propTypes
    - array
date: 2017-08-25 13:22:54
---

React 에서 속성의 타입을 정의할 때 propTypes를 사용하면 편하다.
속성의 타입이 array 라면 배열 안의 값에 대한 타입도 `arrayOf()`로 지정할 수 있다.

아래 예시는 배열 안의 값의 타입을 숫자로 정의한 것이다.
```jsx
// An array of a certain type
optionalArrayOf: PropTypes.arrayOf(PropTypes.number),
```
배열 안의 값의 타입을 `PropTypes.number` 로 지정했기 때문에 숫자말고 다른 타입이 들어오면 에러가 난다.
부모 컴포넌트에서 넘어오는 속성 값은 숫자로 된 배열이어야 한다. (ex) optionalArrayOf=[1, 2, 3])



이외에도 아래와 같이 많은 타입을 정의할 수 있도록 제공된다.
##### 제공되는 PropTypes
```jsx
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // You can declare that a prop is a specific JS primitive. By default, these
  // are all optional.
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // Anything that can be rendered: numbers, strings, elements or an array
  // (or fragment) containing these types.
  optionalNode: PropTypes.node,

  // A React element.
  optionalElement: PropTypes.element,

  // You can also declare that a prop is an instance of a class. This uses
  // JS's instanceof operator.
  optionalMessage: PropTypes.instanceOf(Message),

  // You can ensure that your prop is limited to specific values by treating
  // it as an enum.
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // An object that could be one of many types
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // An array of a certain type
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // An object with property values of a certain type
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // An object taking on a particular shape
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // You can chain any of the above with `isRequired` to make sure a warning
  // is shown if the prop isn't provided.
  requiredFunc: PropTypes.func.isRequired,

  // A value of any data type
  requiredAny: PropTypes.any.isRequired,

  // You can also specify a custom validator. It should return an Error
  // object if the validation fails. Don't `console.warn` or throw, as this
  // won't work inside `oneOfType`.
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // You can also supply a custom validator to `arrayOf` and `objectOf`.
  // It should return an Error object if the validation fails. The validator
  // will be called for each key in the array or object. The first two
  // arguments of the validator are the array or object itself, and the
  // current item's key.
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

참고사이트: https://facebook.github.io/react/docs/typechecking-with-proptypes.html