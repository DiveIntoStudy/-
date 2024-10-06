# CH11 원시 값과 객체의 비교

| 원시 값                   | 객체 값                   |
| ------------------------- | ------------------------- |
| 변경 불가능한 값          | 변경 가능한 값            |
| 저장 시, 실제 값이 저장됨 | 저장 시, 참조 값이 저장됨 |
| 값에 의한 전달            | 참조에 의한 전달          |

## 11.1 원시 값

### 11.1.1 변경 불가능한 값

&nbsp;불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없다.

```javascript
// const 키워드를 사용해 선언한 변수는 재할당이 금지된다. 상수는 재할당이 금지된 변수일 뿐이다.
const o = {};

// const 키워드를 사용해 선언한 변수에 할당한 원시 값(상수)은 변경할 수 없다.
// 하지만 const 키워드를 사용해 선언한 변수에 할당한 객체는 변경할 수 있다.
o.a = 1;
console.log(o); // {a: 1}
```

### 11.1.2 문자열과 불변성

```javascript
var str = "Hello";
str = "world";
```

1. 첫 번째 문 실행 시, 문자열 "Hello"가 생성되고 str은 해당 문자열이 저장된 메모리 공간의 첫 번째 메모리 셀 주소를 가리킴
2. 두 번째 문 실행 시, 문자열 "world"를 메모리에 생성하고 str은 이걸 가리킴

-> "Hello"와 "world"는 둘 다 메모리 상에 존재하고 있음

#### 유사 배열 객체

&nbsp;배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말함(ex 문자열)

```javascript
var str = "string";

// 문자열은 원시 값이므로 변경할 수 없다. 이때 에러가 발생하지 않는다.
str[0] = "S";

console.log(str); // string
```

### 11.1.3 값에 의한 전달

&nbsp;변수에 원시 값을 갖는 변수를 할당하면 할당받는 변수에는 할당되는 변수의 원시 값이 복사되어 전달된다. 이를 **값에 의한 전달**이라 한다.

```javascript
var score = 80;

var copy = score;

console.log(score, copy); // 80 80
console.log(score === copy); // true

score = 100;

console.log(score, copy); // 100 80
console.log(score === copy); // false
```

1. score에는 80의 주소(0x00000F2)가 저장됨
2. copy는 score의 주소를 통해 메모리 공간에 접근해 값을 참조, 복사해 저장한 80의 주소(0x00001332)를 저장함
3. score는 100을 새로운 메모리 주소(0x0669F913)에 저장해, 그 주소를 저장함

#### 결론

- "값에 의한 전달"도 사실은 값을 전달하는 것이 아니라 메모리 주소를 전달

  단, 전달된 메모리 주소를 통해 메모리 공간에 접근하면 값을 참조할 수 있는 것

- 두 변수의 원시 값은 서로 다른 메모리 공간에 저장된 별개의 값이 되어 어느 한쪽에서 재할당을 통해 값을 변경하더라도 서로 간섭할 수 없음

## 11.2 객체

- 프로퍼티의 개수가 정해져 있지 않으며, 동적으로 추가되고 삭제할 수 있음
- 원시 값처럼 확보해야할 메모리 공간의 크기를 사전에 정해 둘 수 없음
- V8 자바스크립트 엔진에서는 프로퍼티에 접근하기 위해 **히든 클래스**라는 방식을 사용해 C++ 객체의 프로퍼티에 접근하는 정도의 성능을 보장

- 읽어보면 좋은 글
  - [자바스크립트 성능의 비밀 (V8과 히든 클래스)](https://ui.toast.com/posts/ko_20210909)
  - [V8의 히든 클래스 이야기](https://engineering.linecorp.com/ko/blog/v8-hidden-class)

### 11.2.1 변경 가능한 값

```javascript
var person = {
  name: "Lee",
};

// 프로퍼티 값 갱신
person.name = "Kim";

// 프로퍼티 동적 생성
person.address = "Seoul";

console.log(person); // {name: "Kim", address: "Seoul"}
```

- person 식별자는 객체가 저장된 메모리 주소를 참조함
- 프로퍼티가 변경, 갱신되어도 참조하는 메모리 주소는 변하지 않음

  -> 객체를 할당한 변수는 재할당 없이 객체를 직접 변경할 수 있기 때문

- 여러 개의 식별자가 하나의 객체를 공유할 수 있음

#### 얕은 복사와 깊은 복사

- 얕은 복사
  - 한 단계까지만 복사하는 것
- 깊은 복사
  - 객체에 중첩되어 있는 객체까지 모두 복사하는 것

```javascript
const o = { x: { y: 1 } };

// 얕은 복사
const c1 = { ...o };
console.log(c1 === o); // false
console.log(c1.x === o.x); // true

// lodash의 cloneDeep을 사용한 깊은 복사
const _ = require("lodash");
// 깊은 복사
const c2 = _.cloneDeep(o);
console.log(c2 === o); // false
console.log(c2.x === o.x); // false
```

### 11.2.2 참조에 의한 전달

- 참조 값이 복사되어 전달되는 것
- 여러 개의 식별자가 하나의 객체를 공유

```javascript
var person = {
  name: "Lee",
};

// 참조 값을 복사(얕은 복사). copy와 person은 동일한 참조 값을 갖는다.
var copy = person;

// copy와 person은 동일한 객체를 참조한다.
console.log(copy === person); // true

// copy를 통해 객체를 변경한다.
copy.name = "Kim";

// person을 통해 객체를 변경한다.
person.address = "Seoul";

// copy와 person은 동일한 객체를 가리킨다.
// 따라서 어느 한쪽에서 객체를 변경하면 서로 영향을 주고받는다.
console.log(person);
console.log(copy);
```

&nbsp;결국 "값에 의한 전달"과 "참조에 의한 전달"은 식별자가 기억하는 메모리 공간에 저장되어 있는 값을 복사해서 전달한다는 면에서 동일하다. 따라서 자바스크립트에는 "참조에 의한 전달"은 존재하지 않고 "값에 의한 전달"만이 존재한다고 말할 수 있다.
