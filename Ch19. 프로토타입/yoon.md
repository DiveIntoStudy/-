# CH19 프로토타입

&nbsp;자바스크립트는 객체 기반의 프로그래밍 언어이며 자바스크립트를 이루고 있는 거의 모든 것이 객체다.

## 19.1 객체지향 프로그래밍

- 객체지향 프로그래밍은 여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로프래밍 패러다임을 말함
- 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 **추상화**라함
- 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 **객체**라 함

```javascript
const circle = {
  radius: 5,

  getDiameter() {
    return 2 * this.radius;
  },

  getPerimeter() {
    return 2 * Math.PI * this.radius;
  },

  getArea() {
    return Math.PI * this.radius ** 2;
  },
};
```

&nbsp;객체의 상태를 나타내는 데이터(반지름)와 상태 데이터를 조작할 수 있는 동작(원의 지름, 둘레, 넓이를 구하는 것)을 하나의 논리적 단위로 묶어 생각하는 것을 객체지향 프로그래밍이라고 부른다.

## 19.2 상속과 프로토타입

&nbsp;상속은 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다. 자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다. 그리고 프로토타입을 기반으로 상속을 구현한다.

```javascript
function Circle(radius) {
  this.radius = radius;
}

Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // true
```

## 19.3 프로토타입 객체

&nbsp;프로토타입 객체(또는 줄여서 프로토타입)란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다.

### 19.3.1 `__proto__` 접근자 프로퍼티

&nbsp;모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 `[[Prototype]]` 내부 슬롯에 직접적으로 접근할 수 있다.

- `__proto__`는 접근자 프로퍼티
- `__proto__` 접근자 프로퍼티는 상속을 통해 사용됨
- `__proto__` 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장되지 않음

#### `__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유

&nbsp;프로토타입 체인은 단방향 링크드 리스트로 구현되어야 하는데, 서로가 자신의 프로토타입이 되는 순환 참조에 빠질 경우 프로퍼티 검색 시 무한 루프에 빠지기 때문. 따라서, 아무런 체크 없이 무조건적으로 프로토타입을 교체할 수 없도록 `__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어 있다.

### 19.3.2 함수 객체의 prototype 프로퍼티

&nbsp;함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다. 모든 객체가 가지고 있는 `__proto__` 접근자 프로퍼티와 함수 객체만이 가지고 있는 `prototype` 프로퍼티는 결국 동일한 프로토타입을 가리킨다.

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

console.log(Person.prototype === me.__proto__); // true
```

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

&nbsp;모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

console.log(me.constructor === Person); // true
```

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

책을 읽어보는 것이 더 나을 것 같음..😵‍💫

## 19.5 프로토타입의 생성 시점

&nbsp;프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

&nbsp;생성자 함수로서 호출할 수 있는 함수, 즉, constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

&nbsp;Object, String, Number, Function, Array 등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.

## 19.6 객체 생성 방식과 프로토타입의 결정

- 객체 생성 방식
  - 객체 리터럴
  - Object 생성자 함수
  - 생성자 함수
  - Object.create 메서드
  - 클래스(ES6)

&nbsp;프로토타입은 추상연산 `OrdinaryObjectCreate`에 전달되는 인수에 의해 결정된다. 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정된다.

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

&nbsp;자바스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할 때 추상 연산 `OrdinaryObjectCreate`를 호출한다. 이때 추상 연산 `OrdinaryObjectCreate`에 전달되는 프로토타입은 Object.prototype이다. 즉, 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.

```javascript
const obj = { x: 1 };

console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

&nbsp; Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지의 단계를 거침(추상 연산 `OrdinaryObjectCreate`를 호출한다. 이때 추상 연산 `OrdinaryObjectCreate`에 전달되는 프로토타입은 Object.prototype이다. 즉, 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.)

```javascript
const obj = new Object();
obj.x = 1;
```

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");
```

&nbsp;위 코드가 실행되면 추상 연산 `OrdinaryObjectCreate`에 의해 생성자 함수와 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체와 생성된 객체 사이에 연결이 만들어짐

## 19.7 프로토타입 체인

&nbsp;자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 `[[Prototype]]` 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 이를 프로토타입 체인이라 한다.

&nbsp;`Object.prototype`을 프로토타입 체인의 종점이라 한다.

&nbsp;프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘이고, 스코프 체인은 식별자 검색을 위한 메커니즘이다(프로퍼티가 아닌 식별자는 스코프 체인에서 검색한다).

## 19.8 오버라이딩과 프로퍼티 섀도잉

#### 오버라이딩

&nbsp;상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식

#### 오버로딩

&nbsp;함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식이다. 자바스크립트는 오버로딩을 지원하지 않지만 arguments 객체를 사용하여 구현할 수는 있다.

#### 프로퍼티 섀도잉

&nbsp;오버라이딩에 의해 프로퍼티가 가려지는 현상

## 19.9 프로토타입의 교체

### 19.9.1 생성자 함수에 의한 프로토타입의 교체

```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 Prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    constructor: Person,
    sayHello() {
      console.log(this.name);
    },
  };

  return Person;
})();

const me = new Person("Lee");

console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```

### 19.9.2 인스턴스에 의한 프로토타입의 교체

-> 프로토 타입은 직접 교체하지 않는 것이 좋다

## 19.10 instanceof

&nbsp;우변의 생성자 함수 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가한다.

## 19.11 직접 상속

### 19.11.1 Object.create에 의한 직접 상속

### 19.11.2 객체 리터럴 내부에서 `__proto__`에 의한 직접 상속

## 19.12 정적 프로퍼티/메서드

&nbsp;생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드로, 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.

## 19.13 프로퍼티 존재 확인

### 19.13.1 in 연산자

```javascript
/**
 * key: 프로퍼티 키
 * object: 객체로 평가되는 표현식
 */
key in object;
```

### 19.13.2 Object.prototype.hasOwnProperty

## 19.14 프로퍼티 열거

### 19.14.1 for ... in 문

### 19.14.2 Object.keys/values/entries 메서드
