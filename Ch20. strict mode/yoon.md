# CH20 strict mode

## 20.1 strict mode란?

```javascript
function foo() {
  x = 10;
}
foo();

console.log(x); // 10
```

&nbsp;전역 스코프에 x 변수의 선언이 없기 때문에 ReferenceError를 발생시킬 것 같지만 전역 객체에 x 프로퍼티를 동적 생성하기 때문에 에러가 발생하지 않는다. x는 전역 변수처럼 사용할 수 있다. 이런 현상을 `암묵적 전역`이라고 한다. 그런데, 이러한 현상은 오류를 발생시키는 원인이 될 가능성이 크다. ES5부터 추가된 `strict mode(엄격 모드)`가 이 현상을 잡아준다. 엄격 모드는 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

> 엄격 모드라는 말이 왜이렇게 웃기죠 ㅋㅋㅋㅋ 아 웃기다...

## 20.2 strict mode의 적용

&nbsp;전역의 선두 또는 함수 몸체의 선두에 'use strict';를 추가

## 20.3 전역에 strict mode를 적용하는 것은 피하자

&nbsp;전역에 적용한 strict mode는 스크립트 단위로 적용되기 때문

## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

## 20.5 strict mode가 발생시키는 에러

### 20.5.1 암묵적 전역

```javascript
(function () {
  "use strict";

  x = 1;
  console.log(x); // ReferenceError: x is not defined
});
```

### 20.5.2 변수, 함수, 매개변수의 삭제

&nbsp;delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생

### 20.5.3 매개변수 이름의 중복

&nbsp;중복된 매개변수 이름을 사용하면 SyntaxError가 발생

### 20.5.4 with 문의 사용

&nbsp;with 문을 사용하면 SyntaxError가 발생(with문 쓰지마)

## 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반 함수의 this

&nbsp;함수를 일반 함수로서 호출하면 this에 undefined가 바인딩됨(에러 발생 x)

```javascript
(function () {
  "use strict";

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }

  new Foo();
});
```

### 20.6.2 arguments 객체

&nbsp;매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않음

```javascript
(function (a) {
  "use strict";
  // 매개변수에 전달된 인수를 재할당하여 변경

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
})(1);
```
