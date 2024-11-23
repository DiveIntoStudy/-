# CH34 이터러블

## 34.1 이터레이션 프로토콜

- 이터러블 프로토콜
  - Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현 또는 Symbol.iterator 메서드 호출 시 이터레이터 반환
  - 이터러블은 for ... of 문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용가능
- 이터레이터 프로토콜
  - Symbol.iterator 메서드 호출 시 이터레이터를 반환하고, 이의 next 메서드를 호출하면 이터러블을 순회하며 value와 done 프로퍼티를 갖는 이터레이터 리적트 객체를 반환
  - 이터레이터는 이터러블의 요소를 탐색하기 위한 포인터역할을 한다.

### 34.1.1 이터러블

&nbsp;이터러블은 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다. 예를 들어, 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.

### 34.1.2 이터레이터

&nbsp;이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.

```javascript
// 이터러블
const array = [1, 2, 3];

// 이터레이터
const iterator = array[Symbol.iterator]();

// 이터레이터 리절트 객체 반환
console.log(iterator.next()); // {value: 1, done: false}
```

## 34.2 빌트인 이터러블

Array, String, Map, Set, TypedArray, arguments, DOM 컬렉션

## 34.3 for ... of 문

```javascript
for (변수선언문 of 이터러블) { ... }
```

## 34.4 이터러블과 유사 배열 객체

&nbsp;유사 배열 객체에는 Symbol.iterator 메서드가 없기 때문에 for ... of 문으로 순회할 수 없다. 다만, Array.from 메서드를 사용해 배열로 간단히 변환할 수 있다.

```javascript
// 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

const arr = Array.from(arrayLike);
console.log(arr); // [1, 2, 3]
```

## 34.5 이터레이션 프로토콜의 필요성

&nbsp;다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정하여 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할을 한다.

## 34.6 사용자 정의 이터러블

// 한 번 읽어보기만 해도 될듯..
