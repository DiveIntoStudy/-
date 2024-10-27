# CH17 생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수

&nbsp;new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

&nbsp;객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하다. 하지만 객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성한다. 따라서 **동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 비효율적**이다.

```javascript
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  },
};

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  },
};
```

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

&nbsp;프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다. new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.

```javascript
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);
```

### 17.2.3 생성자 함수의 인스턴스 생성 과정

- 생성자 함수의 역할
  - 인스턴스를 생성
  - 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)

1. 인스턴스 생성과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this가 바인딩된다.

  // 2. this에 바인딩되어있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  // 명시적으로 원시값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
  // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}
```

#### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

&nbsp;일반 객체는 호출할 수 없지만 함수는 호출할 수 있다. 따라서 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위해 함수 객체만을 위한 `[[Environment]]`, `[[FormalParameters]]` 등의 내부 슬롯과 `[[Call]]`, `[[Construct]]` 같은 내부 메서드를 추가로 가지고 있다.

```javascript
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출된다.
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출된다.
new foo();
```

### 17.2.5 constructor와 non-constructor의 구분

- constructor
  - 함수 선언문
  - 함수 표현식
  - 클래스(클래스도 함수다)
- non-constructor
  - 메서드(ES6 메서드 축약 표현)
  - 화살표 함수

```javascript
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
const baz = {
  x: function () {},
};

// 일반 함수로 정의된 함수만이 constructor다.
new foo(); // foo {}
new bar(); // bar {}
new baz.x(); // x {}

// 화살표 함수 정의
const arrow = () => {};

new arrow(); // TypeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만 메서드로 인정한다.
const obj = {
  x() {},
};

new obj.x(); // TypeError: obj.x is not a constructor
```

### 17.2.6 new 연산자

&nbsp;new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다. 단, new 연산자와 함께 호출하는 함수는 non-constructor가 아닌 constructor이어야 한다.

### 17.2.7 new.target

&nbsp;new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.

```javascript
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았따면 new.target은 undefined다.
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());
```

#### 스코프 세이프 생성자 패턴

&nbsp;new.target은 IE에서 지원하지 않는다.

```javascript
// 스코프 세이프 생성자 패턴
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았따면 이 시점의 this는 전역 객체 window를 가리킨다.
  // 즉, this와 Circle은 프로토타입에 의해 연결되지 않는다.
  if (!(this instanceof Circle)) {
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());
```
