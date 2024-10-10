# CH10 객체 리터럴

## 10.1 객체란?

- 변경 가능한 값
- 0개 이상의 프로퍼티와 메서드로 구성된 집합

#### 프로퍼티

&nbsp;키와 값으로 구성된 객체의 상태를 나타내는 값

#### 메서드

&nbsp;프로퍼티를 참조하고 조작할 수 있는 동작

## 10.2 객체 리터럴에 의한 객체 생성

- 중괄호({...}) 내에 0개 이상의 프로퍼티를 정의
- 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성함

#### 인스턴스

&nbsp;클래스에 의해 생성되어 메모리에 저장된 실체로, 객체지향 프로그래밍에서는 객체는 클래스와 인스턴스를 포함한 개념이다. 클래스는 인스턴스를 생성하기 위한 템플릿의 역할을 한다.

```javascript
var person = {
  name: "Lee",
  sayHello: function () {
    console.log(`Hello! My name is ${this.name}.`);
  },
};

console.log(typeof person); // object
console.log(person); // {name: "Lee", sayHello: f}
```

## 10.3 프로퍼티

- 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성됨
- 식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용해야 함

#### 프로퍼티 키

&nbsp;빈 문자열을 포함하는 모든 문자열 또는 심벌 값

#### 프로퍼티 값

&nbsp;자바스크립트에서 사용할 수 있는 모든 값

```javascript
var person = {
  firstName: "Ung-mo",
  "last-name": "Lee", // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키여서 따옴표로 감쌈
};
```

```javascript
var obj = {};
var key = "hello";

// ES5: 프로퍼티 키 동적 생성
obj[key] = "world";

console.log(obj); // {hello: "world"}
```

키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어씀

```javascript
var foo = {
  name: "Lee",
  name: "Kim",
};

console.log(foo); // {name: "Kim"}
```

## 10.4 메서드

&nbsp;객체에 묶여있는 함수

```javascript
var circle = {
  radius: 5,
  getDiameter: function () {
    return 2 * this.radius;
  },
};

console.log(circle.getDiameter()); // 10
```

-> 메서드 내부에서 사용한 `this` 키워드는 객체 자신을 가리키는 참조변수

## 10.5 프로퍼티 접근

- 마침표 표기법(`.`)
- 대괄호 표기법(`[...]`)

```javascript
var person = {
  name: "Lee",
};

console.log(person.name); // Lee
console.log(person["name"]); // Lee
```

```javascript
var person = {
  "last-name": "Lee",
  1: 10,
};
// person.last-name;
// 브라우저 환경에서는 undefined - name -> NaN
// NodeJS 환경에서는 어디에도 name이 없으므로 ReferenceError: name is not defined 에러 발생
```

## 10.6 프로퍼티 값 갱신

```javascript
var person = {
  name: "Lee",
};

person.name = "Kim";

console.log(person); // {name: "Kim"}
```

## 10.7 프로퍼티 동적 생성

```javascript
var person = {
  name: "Lee",
};

// 프로퍼티 동적 생성
person.age = 20;

console.log(person); // {name: "Lee", age: 20}
```

## 10.8 프로퍼티 삭제

```javascript
var person = {
  name: "Lee",
};

// 프로퍼티 동적 생성
person.age = 20;

// 삭제
delete person.age;

// 없는 애는 삭제할 수 없으나 에러 발생하지 않음
delete person.address;

console.log(person); // {name: "Lee"}
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

### 10.9.1 프로퍼티 축약 표현

```javascript
let x = 1,
  y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

### 10.9.2 계산된 프로퍼티 이름

```javascript
// ES6
const prefix = "prop";
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

### 10.9.3 메서드 축약 표현

```javascript
const obj = {
  name: "Lee",
  sayHi() {
    console.log(`Hi! ${this.name}`);
  },
};

obj.sayHi(); // Hi! Lee
```
