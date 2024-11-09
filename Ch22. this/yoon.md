# CH22 this

## 22.1 this 키워드

- 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수
- this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조
- this가 가리키는 값, this 바인딩은 **함수 호출 방식에 의해 동적으로 결정**

```javascript
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: "Lee",
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: f}
    return this.name;
  },
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person("Lee");
```

-> strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다. 일반 함수 내부에서 this를 사용할 필요가 없기 때문이다.

## 22.2 함수 호출 방식과 this 바인딩

&nbsp;this 바인딩은 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

#### 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.

- 렉시컬 스코프
  - 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정
- this 바인딩
  - 함수 호출 시점에 결정

### 22.2.1 일반 함수 호출

- 기본적으로 this에는 전역 객체가 바인딩됨
- 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩됨
- strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩됨
- 콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩됨

  - 화살표 함수를 사용하면 this 바인딩을 상위 스코프와 일치시킬 수 있음

    ```javascript
    const obj = {
      value: 100,
      foo() {
        // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
        setTimeout(() => console.log(this.value), 100); // 100
      },
      bar() {
        // 일반 함수 내부의 this는 전역 객체를 가리킴
        setTimeout(function () {
          console.log(this.value);
        }, 100); // window
      },
    };

    obj.foo();
    obj.bar();
    ```

### 22.2.2 메서드 호출

&nbsp;메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체가 바인딩된다.

### 22.2.3 생성자 함수 호출

&nbsp;생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

&nbsp;해당 메서드에 첫 번째 인수로 전달한 객체에 바인딩된다.
