# CH9 타입 변환과 단축 평가

## 9.1 타입 변환이란?

- 개발자가 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환** 또는 **타입 캐스팅**이라고 함

  ```javascript
  var x = 10;

  // 명시적 타입 변환
  // 숫자를 문자열로 타입 캐스팅한다.
  var str = x.toString();
  console.log(typeof str, str); // string 10

  // x 변수의 값이 변경된 것은 아니다.
  console.log(typeof x, x); // number 10
  ```

- 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 하는데, 이를 **암묵적 타입 변환** 또는
  **타입 강제 변환**이라 함

  ```javascript
  var x = 10;

  // 암묵적 타입 변환
  // 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
  var str = x + "";
  console.log(typeof str, str); // string 10

  // x 변수의 값이 변경된 것은 아니다.
  console.log(typeof x, x); // number 10
  ```

## 9.2 암묵적 타입 변환

### 9.2.1 문자열 타입으로 변환

```javascript
// 숫자 타입
0 + ""; // "0"
-0 + ""; // "0"
-1 + ""; // "-1"
NaN + ""; // "NaN"

// 불리언 타입
true + ""; // "true"

// null 타입
null + ""; // "null"

// undefined 타입
undefined + ""; // "undefined"

// 심벌 타입
Symbol() + ""; // TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ""; // "[object Object]"
[] + ""; // ""
[10, 20] + ""; // "10,20"
(function () {}); // "function(){}"
Array + ""; // "function Array() { [native code] }
```

### 9.2.2 숫자 타입으로 변환

- 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행

```javascript
// 문자열 타입
+""; // 0
+"0"; // 0
+"1"; // 1
+"string"; // NaN

// 불리언 타입
+true; // 1
+false; // 0

// null 타입
+null; // 0

// undefined 타입
+undefined; // NaN

// 심벌 타입
+Symbol(); // TypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{}; // NaN
+[]; // 0
+[10, 20]; // NaN
```

### 9.2.3 불리언 타입으로 변환

- 자바스크립트 엔진은 불리언 타입이 아닌 값을 참으로 평가되는 값 또는 거짓으로 평가되는 값으로 구분
- false로 평가되는 값
  - false
  - undefined
  - null
  - 0, -0
  - NaN
  - ''(빈 문자열)

## 9.3 명시적 타입 변환

### 9.3.1 문자열 타입으로 변환

1. String 생성자 함수를 new 연산자 없이 호출
2. Object.prototype.toString 메서드 사용
3. 문자열 연결 연산자 이용

### 9.3.2 숫자 타입으로 변환

1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
2. parseInt, parseFloat 함수를 사용(문자열만 숫자 타입으로 변환 가능)
3. `+` 단항 산술 연산자를 이용
4. `*` 산술 연산자를 이용

### 9.3.3 불리언 타입으로 변환

1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
2. `!` 부정 논리 연산자를 두 번 사용하는 방법

## 단축 평가

### 9.4.1 논리 연산자를 사용한 단축 평가

```javascript
// 두 번째 피연산자가 논리곱 연산자 표현식의 평가 결과를 결정
"Cat" && "Dog"; // "Dog"

// 논리 연산의 결과를 결정한 첫 번째 피연산자를 반환
"Cat" || "Dog"; // "Cat"
```

-> 논리곱(`&&`) 연산자와 논리합(`||`) 연산자는 **논리 연산의 결과를 결정하는 피연산자**를 타입 변환하지 않고 그대로 반환

#### 객체를 가리키기를 기대한느 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

```javascript
// 프로그램 강제 종료됨
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null

// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가됨
var value = elem && elem.value; // null
```

#### 함수 매개변수에 기본값을 설정할 때

```javascript
// 단축 평가 사용
function getStringLength(str) {
  str = str || "";
  return str.length;
}

// ES6
function getStringLength(str = "") {
  return str.length;
}
```

### 9.4.2 옵셔널 체이닝 연산자

- `?.`
- 좌항의 피연산자가 `null` 또는 `undefined`인 경우 `undefined`를 반환, 그렇지 않으면 우항의 프로퍼티 참조

```javascript
var elem = null;
var value = elem?.value;
console.log(value); // undefined

// 옵셔널 체이닝 도입 전에는 단축평가를 사용
var value = elem && elem.value;
console.log(value); // null
```

### 9.4.3 null 병합 연산자

- `??`
- 좌항의 피연산자가 `null` 또는 `undefined`인 경우

```javascript
var foo = null ?? "default string";
console.log(foo); // "default string"

// null이나 undefined가 아니면 좌항이 Falsy 값이어도 그 값을 반환
var foo = "" ?? "default string";
console.log(foo); // ""
```

## 9.\*

- Number와 parseInt의 차이

  - Number는 소수점까지 숫자타입으로 변환
  - parseInt는 정수만 변환

  ```javascript
  var foo = "12.345";
  console.log(Number(foo)); // 12.345
  console.log(parseInt(foo)); // 12
  ```
