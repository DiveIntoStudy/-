# CH25 클래스

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

> 갑자기 뭔가 소설 같은 제목이..

&nbsp;아니다. 새로운 객체 생성 매커니즘이다.

#### 클래스와 생성자 함수의 차이점

1. 클래스를 new 연산자 없이 호출하면 에러 발생, 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출
2. 클래스는 상속을 지원하는 extends, super 키워드 제공, 생성자 함수는 그런 거 없음
3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작, 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생
4. 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없으나, 생성자 함수는 strict mode가 지정되지 않음
5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 false, 즉 열거되지 않음

## 25.2 클래스의 정의

- 클래스는 값으로 사용할 수 있는 일급 객체
  - 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
  - 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
  - 함수의 매개변수에게 전달할 수 있다.
  - 함수의 반환값으로 사용할 수 있다.

```javascript
class Person {
  // 생성자
  constructor(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log("Hello");
  }
}
```

## 25.3 클래스 호이스팅

```javascript
const Person = "";

{
  // 호이스팅이 발생하지 않는다면 ''이 출력돼야 한다.
  console.log(Person);
  // ReferenceError: Cannot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
}
```

## 25.4 인스턴스 생성

&nbsp;클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스를 생성한다.

## 25.5 메서드

### 25.5.1 constructor

- constructor는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 됨
- 생략할 수 있음
- 내부에 return 문을 작성하면 안 됨 -> 암묵적으로 this를 반환하게 되어있음

### 25.5.2 프로토타입 메서드

- 클래스 몸체에서 정의한 메서드는 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 됨
- 클래스는 생성자 함수와 같이 인스턴스를 생성하는 생성자 함수

### 25.5.3 정적 메서드

&nbsp;인스턴스를 생성하지 않아도 호출할 수 있는 메서드

### 25.5.4 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

&nbsp;메서드 내부에서 인스턴스 프로퍼티를 참조할 필요가 있다면 this 사용해야 하며, 이러한 경우 프로토타입 메서드로 정의해야한다. 아닌 경우는 정적 메서드로 정의하는 것이 좋다.

### 25.5.5 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다.
3. 암묵적으로 strict mode로 실행된다.
4. for ... in 문이나 Object.keys 메서드 등으로 열거할 수 없다. 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다.
5. 내부 메서드 [[Construct]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함께 호출할 수 없다.

## 25.6 클래스의 인스턴스 생성 과정

1. 인스턴스 생성과 this 바인딩
   - new 연산자와 함께 클래스 호출 시 암묵적으로 빈 객체가 생성
   - 빈 객체(인스턴스)는 this에 바인딩됨
2. 인스턴스 초기화
   - constructor의 내부 코드가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화
3. 인스턴스 반환
   - 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환됨

## 25.7 프로퍼티

### 25.7.1 인스턴스 프로퍼티

&nbsp;인스턴스 프로퍼티는 constructor 내부에서 정의해야 한다.

### 25.7.2 접근자 프로퍼티

&nbsp;자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티

```javascript
const person = {
  // 데이터 프로퍼티
  firstName: "Ungmo",
  lastName: "Lee",

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  },
};
```

### 25.7.3 클래스 필드 정의 제안

#### 클래스 필드란,

&nbsp;클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어

- 인스턴스 생성 시, 클래스 필드를 초기화할 필요가 있다면 constructor 내부에서 정의 권장(어차피 초기화 시, 자동 추가됨)
- 클래스 필드에 함수 할당 시, 이는 프로토타입 메서드가 아닌 인스턴스 메서드가 되므로 이 방법은 권장되지 않음

```javascript
class Person {
  constructor(name) {
    // 권장
    this.name = name;
  }

  // 이하의 방법 권장하지 않음
  // 클래스 필드
  name = "Lee";

  getName = function () {
    return this.name;
  };
}
```

### 25.7.4 private 필드 정의 제안

- 반드시 클래스 몸체에 정의 필요

```javascript
class Person {
  // constructor 내부에서 정의 시 에러 발생
  #name = "";
}
```

### 25.7.5 static 필드 정의 제안

```javascript
class MyMath {
  // static public
  static PI = 22 / 7;

  // static private
  static #num = 10;

  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}
```

## 25.8 상속에 의한 클래스 확장

### 25.8.1 클래스 상속과 생성자 함수 상속

&nbsp;상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것

### 25.8.2 extends 키워드

```javascript
class Base {}

class Derived extends Base {}
```

### 25.8.3 동적 상속

```javascript
function Base1() {}

class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
class Derived extends (condition ? Base1 : Base2) {}
```

### 25.8.4 서브클래스의 constructor

&nbsp;수퍼클래스와 서브클래스 모두 constructor를 생략했을 때 암묵적으로 이렇게 동작한다.

```javascript
class Base {
  constructor() {}
}

class Derived extends Base {
  constructor(...args) {
    super(...args);
  }
}

const derived = new Derived();
```

### 25.8.5 super 키워드

- super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출한다.
- super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

#### super 호출

1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에서는 반드시 super를 호출해야 한다.
2. 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다.
3. super는 반드시 서브클래스의 constructor에서만 호출한다. 서브클래스가 아닌 클래스의 constructor나 함수에서 super를 호출하면 에러가 발생한다.

#### super 참조

&nbsp;메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

### 25.8.6 상속 클래스의 인스턴스 생성 과정

1. 서브클래스의 super 호출
2. 수퍼클래스의 인스턴스 생성과 this 바인딩
3. 수퍼클래스의 인스턴스 초기화
4. 서브클래스 constructor로의 복귀와 this 바인딩

   &nbsp;이때 super가 반환한 인스턴스가 this에 바인딩된다. 서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용한다.

5. 서브클래스의 인스턴스 초기화
6. 인스턴스 반환

### 25.8.7 표준 빌트인 생성자 함수 확장

&nbsp;extends 키워드 다음에는 클래스뿐만이 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다(String, Number, Array 등).
