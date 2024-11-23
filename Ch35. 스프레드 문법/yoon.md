# CH35 스프레드 문법

&nbsp;스프레드 문법을 사용할 수 있는 대상은 Array, String, Map, Set, DOM 컬렉션(NodeList, HTMLCollection), arguments처럼 for ... of 문으로 순회할 수 있는 이터러블에 한정된다.

## 35.1 함수 호출문의 인수 목록에서 사용하는 경우

```javascript
const arr = [1, 2, 3];

const max = Math.max(...arr); // 3
```

&nbsp;Rest 파라미터와 혼동할 수 있는데, Rest 파라미터는 여러개를 하나로 뭉치는 거고, 스프레드 문법은 하나를 여러개로 펼치는, 서로 반대되는 개념이다.

## 35.2 배열 리터럴 내부에서 사용하는 경우

### 35.2.1 concat

```javascript
// before(ES5)
var arr = [1, 2].concat([3, 4]);

// after(ES6)
const arr = [...[1, 2], ...[3, 4]];
```

### 35.2.2 splice

```javascript
// before
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));

// after
arr1.splice(1, 0, ...arr2);
```

### 35.2.3 배열 복사

```javascript
// 얕은 복사
const origin = [1, 2];
const copy = [...origin];
```

### 35.2.4 이터러블을 배열로 변환

```javascript
// 스프레드 문법
function sum() {
  return [...arguments].reduce((pre, cur) => pre + cur, 0);
}

// Rest 파라미터
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);
```

## 35.3 객체 리터럴 내부에서 사용하는 경우

```javascript
// 객체 병합, 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };

// 특정 프로퍼티 변경
const changed = { ...{ x: 1, y: 2 }, y: 100 };

// 프로퍼티 추가
const added = { ...{ x: 1, y: 2 }, z: 0 };
```
