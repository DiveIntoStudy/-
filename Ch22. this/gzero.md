## 22.1 this 키워드

- 동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다. 이때 매서드가 자신이 속한 객체의 프로퍼티를 참조하려면 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.
- 자바스크립트 this는 함수가 호출되는 방식에 따라 this에 바인딩될 값, 즉 this 바인딩이 동적으로 결정된다. 아래 객체 리터럴과 생성자 함수를 통해 this는 상황에 따라 가리키는 대상이 다르다는 것을 알 수 있다.
- this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.
- strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다. 일반 함수 내부에서 this를 사용할 필요가 없기 때문이다.
- this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

```jsx
// 객체 리터럴
const circle = {
  // 프로퍼티: 객체 고유의 상태 데이터
  radius: 5,
  // 메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // getDiameter 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
    // 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
    return 2 * this.radius;
  },
};

console.log(circle.getDiameter()); // 10
```

```jsx
// 생성자 함수 정의
function Circle(radius) {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없기때문에, this를 사용한다.
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius;
};

// 인스턴스 생성: 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

## 22.2 함수 호출 방식과 this 바인딩

- this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.
- 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.
  - 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정된다.
  - this 바인딩은 함수 호출 시점에 결정된다.

| 함수 호출 방식                                             | this 바인딩                                                           |
| ---------------------------------------------------------- | --------------------------------------------------------------------- |
| 일반 함수 호출 (중첩 함수, 콜백 함수 포함)                 | 전역 객체                                                             |
| 메서드 호출                                                | 메서드를 호출한 객체                                                  |
| 생성자 함수 호출                                           | 생성자 함수가 (미래에)생성할 인스턴스                                 |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달한 객체 |

### Function.prototype.apply/call/bind 메서드

### **apply / call 메서드**

- apply, call 메서드는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다
- apply, call 메서드의 본질적인 기능은 **함수를 호출**하는 것이다.
- apply, call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 this로 사용할 객체를 전달하면서 함수를 호출하는 것은 동일하다.
  - apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달
  - call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달
- apply, call의 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우다
  ```jsx
  function getThisBinding() {
    console.log(arguments);
    return this;
  }

  const thisArg = { a: 1 };

  console.log(getThisBinding()); // window

  console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
  // Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
  // {a : 1}

  console.log(getThisBinding.call(thisArg, 1, 2, 3));
  // Arguments(3) [1, 2, 3, caller: f, Symbol(Symbol.iterator): f]
  // {a : 1}
  ```

### **bind 메서드**

- 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다
- 함수를 미리 특정 객체와 연결해 두는 기능이다. 이렇게 하면 그 함수를 나중에 호출할 때마다 특정 객체가 this로 고정돼서 사용된다. 즉, 함수를 어떤 객체에 묶어두고 싶을때 사용된다.

```jsx
function getThisBinding() {
	return this;
}

const thisArg = { a:1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)) // getThisBinding

// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg()) // { a:1 }
```
