# CH6 데이터 타입

- 값의 종류를 말함
- 원시 타입(primitive type)
  - 숫자 타입
  - 문자열 타입
  - 불리언 타입
  - undefined 타입
  - null 타입
  - 심벌 타입
- 객체 타입(object/reference type)
  - 객체, 함수, 배열 등

## 6.1 숫자 타입

```javascript
var integer = 10; // 정수
var double = 10.12; // 실수
var negative = -20; // 음의 정수

// 65
var binary = 0b01000001; // 2진수
var octal = 0o101;
var hex = 0x41;
console.log(binary === octal); // true

// 숫자 타입은 모두 실수로 처리된다.
console.log(1 === 1.0) === true;

// 숫자 타입의 세 가지 특별한 값
console.log(10 / 0); // Infinity
console.log(10 / -0); // -Infinity
console.log(1 * "String"); // NaN

// 자바스크립트는 대소문자를 구별한다.
var x = nan; // ReferenceError: nan is not defined
```

## 6.2 문자열 타입

```javascript
// 따옴표로 감싸지 않은 hello를 식별자로 인식한다.
var string = hello; // ReferenceError: hello is not defined
```

## 6.3 템플릿 리터럴

- 백틱을 사용해 표현
- 개행이 허용됨(\n)

## 6.4 불리언 타입

&nbsp;`true`, `false`

## 6.5 undefined 타입

- var 키워드로 선언한 변수는 암묵적으로 undefined로 초기화됨
- 자바스크립트 엔진이 변수를 초기화할 때 사용하므로, 변수에 값이 없다는 걸 명시하고 싶을 때는 `null`을 할당하자

## 6.6 null 타입

- 자바스크립트는 대소문자를 구분하므로 `Null`과 `NULL` 등과 다름
- 변수에 `null`을 할당하는 것은 이전에 할당되어 있는 값에 대한 참조를 명시적으로 제거하는 것을 뜻하며, 자바스크립트 엔진은 이 메모리 공간에 대해 가비지 콜렉션을 수행할 것

## 6.7 심벌 타입

&nbsp;다른 값과 중복되지 않는 유일무이한 값

```javascript
var a = Symbol("abc");
var b = Symbol("abc");

a === b; // false
```

## 6.8 객체 타입

&nbsp;자바스크립트를 이루고 있는 거의 모든 것이 객체(객체, 함수, 배열 등)

## 6.9 데이터 타입의 필요성

- 값을 저장할 때 확보해야 하는 **메모리 공간의 크기**를 결정
- 값을 참조할 때 한 번에 읽어 들여야 할 **메모리 공간의 크기**를 결정
- 메모리에서 읽어 들인 **2진수를 어떻게 해석**할지 결정

```javascript
var a = 100;
var b = 100;

// 0x123: 100
// 0x111: 식별자 a, 값은 0x123 참조
// 0x112: 식별자 b, 값은 0x123 참조

b = 111;
// 0x124: 111
// 0x112: 식별자 b, 값은 0x124 참조

// 이처럼 참조하는 방식이 아니면 같은 값임에도 계속 새로 저장해 메모리 낭비를 일으킬 수 있음
```

## 6.10 동적 타이핑

### 자바스크립트는 동적 타입 언어

- 자바스크립트의 변수는 선언이 아닌 할당에 의해 타입이 결정(타입 추론)됨
- 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있음

## 6.\*

- `NaN`도 `number`다

  ```javascript
  typeof NaN === "number"; // true
  ```

- BigInt

  &nbsp;`Number` 원시 값이 안정적으로 나타낼 수 있는 최대치인 2^53-1보다 큰 정수를 표현할 수 있는 내장 객체

  ```javascript
  12345123124125124n === BigInt(12345123124125124n); //true
  ```

- Object.is()

  - 두 값이 같은 값인지 결정
  - 강제 형변환을 하는 `==`와 달리 어느 값도 강제하지 않음
  - `===`와는 부호 있는 0과 `NaN`처리가 다름

    ```javascript
    console.log(0 === -0); // true
    console.log(Object.is(0 === -0)); // false

    console.log(NaN === NaN); // false
    console.log(Object.is(NaN, NaN)); // true
    ```

- null의 타입은 object

  ```javascript
  typeof null; // "object"
  ```

  - null은 원시타입인데 객체라고?
  - 이를 수정할 경우, 기존 코드에 문제가 생겨 수정될 수 없는 버그라고 함
  - [한 번쯤 읽어보면 재밌는 자료](https://2ality.com/2013/10/typeof-null.html)

- 참고자료
  - [모던 자바스크립트 딥다이브 스터디 #1-2 (CH6, 7)](https://www.youtube.com/watch?v=rPVrtODy9P0&list=PLjQV3hketAJnP_ceUiPCc8GnNQ0REpCqr&index=2)
  - [mdn BigInt](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
  - [The history of “typeof null”](https://2ality.com/2013/10/typeof-null.html)
