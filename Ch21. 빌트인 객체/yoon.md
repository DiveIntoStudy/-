# CH21 빌트인 객체

## 21.1 자바스크립트 객체의 분류

- 표준 빌트인 객체

  &nbsp;ECMAScript 사양에 정의된 객체를 말하며, 애플리케이션 전역의 공통 기능을 제공, 별도의 선언 없이 전역 변수처럼 언제나 참조할 수 있다.

- 호스트 객체

  &nbsp;ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행환경에서 추가로 제공하는 객체를 말한다. 브라우저 환경에서는 DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame과 같은 클라이언트 사이드 Web API를 호스트 객체로 제공한다.

- 사용자 정의 객체

  &nbsp;사용자가 직접 정의한 객체를 말한다.

## 21.2 표준 빌트인 객체

- Object, String, Number, Boolean, Symbol, Date, Math, RegExp ...
- Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체

## 21.3 원시값과 래퍼 객체

Q. 무자열이나 숫자, 불리언 등의 원시값이 있는데도 문자열, 숫자, 불리언 객체를 생성하는 String, Number, Boolean 등의 표준 빌트인 생성자 함수가 존재하는 이유가 뭘까?
A. 원시값인 문자열, 숫자, 불리언 값의 경우 이들 원시값에 대해 마치 객체처럼 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해준다. 이렇게 이들 원시값에 객체러머 접근하면 생성되는 임시 객체를 **래퍼 객체(wrapper object)**라고 한다.

> 래퍼 객체 왜케 말이 웃기죠 ㅋㅋㅋㅋㅋ 웃기다...

```javascript
const str = "hi";

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length);
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str);
```

&nbsp;래퍼 객체의 처리가 종료되면 래퍼 객체의 `[[StringData]]` 내부 슬롯에 할당된 원시값으로 원래의 상태, 즉 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 된다.

## 21.4 전역 객체

- 코드가 실행되기 이전 단계에서 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체로, 어떤 객체에도 속하지 않은 최상위 객체
- 자바스크립트 환경에 따라 지칭하는 이름이 제각각(브라우저 - window, Node.js - global, 공통 - globalThis)
- 특징
  - 개발자가 의도적으로 생성할 수 없음(전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않음)
  - 전역 객체의 프로퍼티를 참조할 때 window(또는 global)를 생략 가능

### 21.4.1 빌트인 전역 프로퍼티

&nbsp;전역 객체의 프로퍼티

- Infinity
- NaN
- undefined

### 21.4.2 빌트인 전역 함수

&nbsp;전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.

- eval
  ```javascript
  /**
   * 주어진 문자열 코드를 런타임에 평가 또는 실행한다.
   * @params {string} code - 코드를 나타내는 문자열
   * @returns {*} 문자열 코드를 평가/실행한 결과값
   */
  eval(code);
  ```
  -> 자바스크립트 엔진에 의해 최적화가 수행되지 않으므로 쓰지 말어라
- isFinite
- isNaN
- parseFloat
- encodeURI / decodeURI
- encodeURIComponent / decodeURIComponent
  - Component 붙은 것과 안 붙은 것의 차이
    - Component는 인수로 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주
    - 안 붙은 건 인수로 전달된 문자열을 URI 전체라고 간주해 쿼리 스트링 구분자로 사용되는 =, ?, &은 인코딩하지 않음

### 21.4.3 암묵적 전역

(20장과 동일한 내용 반복)
