## 25.1 클래스는 프로토타입의 문법적 설탕인가?

- 자바스크립트는 프로토타입 기반의 객체지향 언어다
- 프로토타입 기반 객체지향 언어는 클래스가 필요없는 객체지향 프로그래밍 언어다
- 클래스 기반 언어에 익숙한 프로그래머들은 프로토타입 기반 프로그래밍 방식에 혼란을 느낄 수 있으며, 자바스크립트를 어렵게 느끼게 하는 하나의 장벽처럼 인식되었다. 그래서 ES6에 클래스가 도입되었으며, 클래스 기반 객체지향 프로그래밍 언어와 매우 흡사한 새로운 객체 생성 메커니즘을 제시한다
- **클래스는 생성자 함수와 유사하게 동작하지만 몇 가지 차이가 있다**
  - 클래스를 new 연산자 없이 호출하면 에러가 발생한다. 하지만 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다
  - 클래스는 상속을 지원하는 extends와 super 키워드를 제공한다. 하지만 생성자 함수는 extends와 super 키워드를 지원하지 않는다
  - 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
  - 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다. 하지만 생성자 함수는 암묵적으로 strict mode가 지정되지 않는다.
  - 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다. 다시 말해, 열거되지 않는다
- 클래스는 새로운 객체 생성 메커니즘으로 보는 것이 합당하다

## 25.2 클래스 정의

- 클래스는 class 키워드를 사용하여 정의한다
- 클래스는 일급 객체이며, 아래와 같은 특징을 가진다
  - 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다
  - 변수나 자료구조(객체, 배열 등)에 저장할 수 있다
  - 함수의 매개변수에게 전달할 수 있다
  - 함수의 반환값으로 사용할 수 있다

## 25.3 클래스 호이스팅

- 클래스는 함수로 평가된다
- 클래스 선언문으로 정의한 클래스는 런타임 이전에 먼저 평가되어 함수 객체를 생성한다
- 클래스는 클래스 정의 이전에 참조할 수 없다
- 클래스 선언문 이전에 일시적 사각지대에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다

## 25.4 인스턴스 생성

- 클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 new 연산자와 함께 호출해야 한다

## 25.5 메서드

- 클래스 몸체에는 0개 이상의 메서드만 선언할 수 있다
- 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드 세가지가 있다.

### 25.5.1 constructor

- 인스턴스를 생성하고 초기화하기 위한 특수한 메서드
- constructor는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다. 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다
- 클래스 내에 최대 1 개만 존재할 수 있다
- constructor를 생략하면 빈 constructor가 암묵적으로 생성되도, 빈 객체를 생성한다
- constructor 내에서 인스턴스 생성과, 인스턴스 프로퍼티 추가를 통해 인스턴스 초기화를 실행한다

```jsx
class Person {
  constructor(name, address) {
    this.name = name;
    this.address = address;
  }
}

const me = new Person("Gwon", "Seoul");
console.log(me); // Person {name: 'Gwon', address: 'Seoul'}
```

- 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시되기 때문에, constructor 내부에서 return 문을 반드시 생략해야한다.

### 25.5.2 프로포타입 메서드

- 클래스 몸체에서 정의한 메서드는 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다
- 클래스가 생성한 인스턴스는 프로토타입 체인의 일월이 된다

### 25.5.3 정적 메서드

- 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드이다.
- 클래스 메서드에 static 키워드를 붙이면 정적 메서드가 된다

```jsx
class Person {
  constructor(name) {
    this.name = name;
  }

  static sayHi() {
    console.log("Hi!");
  }
}

// 정적 메서드는 클래스로 호출한다
// 정적 메서드는 인스턴스 없이도 호출할 수 있다
Person.sayHi(); // Hi!
```

### 25.5.4 정적 메서드와 프로토타입 메서드의 차이

- 표준 빌트인 객체인 Math, Number, JSON, Object, Reflect 등은 다양한 정적 메서드를 가지고 있다. 이들 정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수다
- 클래스 또는 생성자 함수를 하나의 네임스페이스로 사용하여 정적 메서드를 모아 놓으면 이름 충돌 가능성을 줄여 주고 관련 함수들을 구조화할 수 있는 효과가 있다

### 25.5.5 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용
2. 객체 리터럴과 는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다
3. 암묵적으로 strict mode로 실행된다
4. for … in 문이나 Object.keys 메서드 등으로 열거할 수 없다. 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다
5. 내부 메서드 [[Construct]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함께 호출할 수 없다.

## 25.6 클래스의 인스턴스 생성 과정

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다
    console.log(this); // Person {}
    console.log(Object.getPrototypeOf(this) === Person.prototype); // true

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.name = name;

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  }
}
```

## 25.7 프로퍼티

### 25.7.1 인스턴스 프로퍼티

ES6 클래스는 다른 객체지향 언어처럼 private, public, protected 키워드와 같은 접근 제한자를 지원하지 않는다. 따라서 인스턴스 프로퍼티는 언제나 public 이다.

### 26.7.2 접근자 프로퍼티

```jsx
class Person {
	constructor(firstName, lastName){
		this.firstName = firstName;
		this.lastName = lastName;
	}

	// fullName은 접근자 함수로 구정된 접근자 프로퍼티다.
	// getter 함수
	get fullName() {
		return `${this.firstName} ${this.lastName}`
	}

	// setter 함수
	set fullName() {
		[this.firstName, this.lastName] = name.spli(" ");
	}
}

const me = new Person('Jiyeong', 'Gwon')

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(`${me.firstName} ${me.lastName}`) // Jiyeong Gwon

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다
me.fullName = 'Gzero Gwon';
console.log(me) // {firstName: 'Gzero', lastName: 'Gwon'}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다
console.log(me.fullName) // Gzero Gwon

// fullName은 접근자 프로퍼티다
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(Person.prototype, 'fullName'));
// {get: f, set: f, enumerable:false, configurable: ture}
```

### 25.7.3 클래스 필드 정의 제안

- 클래스 필드는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다
- 클래스 몸체에서 클래스 필드를 정의하는 경우 this 에 클래스 필드를 바인딩해서는 안 된다. this는 클래스의 constructor와 메서드 내에서만 유효하다

### 25.7.4 private 필드 정의 제안

- ES6 클래스는 다른 객체지향 언어처럼 private, public, protected 키워드와 같은 접근 제한자를 지원하지 않는다. 따라서 인스턴스 프로퍼티는 언제나 public 이다
- 하지만 2021 이후 private 필드를 정의할 수 있는 방법이 생김 → private 필드 선두에 #을 붙여준다

```jsx
class Person {
  // Private 필드 정의
  #name = "";

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person("Lee");

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name); // SyntaxError: Private field '#name' must be declared in an enclosing class
```

## 25.8 상속에 의한 클래스 확장

```jsx
// 수퍼(베이스/부모) 클래스
class Base {}

// 서브(파생/자식) 클래스
class Derived extends Base {}
```

- 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다
- 상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의한다
- extends 키워드는 클래스뿐만 아니라 생성자 함수, [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식를 상속받아 클래스를 확장할 수도 있다.

### 25.8.5 super 키워드

- super 키워드는 함수처럼 호출할 수도 있고 this와 같이 식별자처럼 참조할 수 있는 특수한 키워드다
  - super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출한다
  - super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.
- super를 호출할 때 주의사항
  - 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에서는 반드시 super를 호출해야한다
  - 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다
  - super는 반드시 서브클래스의 constructor에서만 호출한다. 서브클래스가 아닌 클래스의 constructor나 함수에서 super를 호출하면 에러가 발생한다
- super 참조
  - 메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다
  - `[[HomeObject]]` 를 가지는 함수만 super를 참조할 수 있다.
  - super 참조는 클래스의 전유물이 아니다. 객체 리터럴에서도 super참조를 사용할 수 있다. 단, ES6의 메서드 축약 표현으로 정의된 함수만 가능하다
