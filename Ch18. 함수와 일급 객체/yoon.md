# CH18 함수와 일급 객체

## 18.1 일급 객체

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

- 자바스크립트의 함수는 일급 객체임
- 일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있음
- 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유함

## 18.2 함수 객체의 프로퍼티

- 데이터 프로퍼티
  - 일반 객ㄱ체에는 없는 함수 객체 고유의 프로퍼티
  - `arguments`, `caller`, `length`, `name`, `prototype`
- 접근자 프로퍼티
  - `__proto__`

### 18.2.1 arguments 프로퍼티

&nbsp;`arguments` 함수 객체의 프로퍼티는 `arguments` 객체로, 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다. 즉, 함수 외부에서는 참조할 수 없다.

```javascript
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

console.log(multiply()); // NaN
console.log(multiply(1)); // NaN
console.log(multiply(1, 2)); // 2
console.log(multiply(1, 2, 3)); // 2
```

&nbsp;`arguments` 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하다.

```javascript
function sum() {
    let res = 0;

    for (let i = 0; i< arguments.length; i++>){
        res += arguments[i];
    }

    return res;
}

console.log(sum()); // 0
console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3)); // 6
```

&nbsp; `arguments` 객체는 유사 배열 객체로, length 프로퍼티를 가져 for문으로 순회할 수 있는 객체이다.

```javascript
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3)); // 6
```

&nbsp; ES6에 도입된 Rest 파라미터를 이용하는 방법도 있다.

### 18.2.2 caller 프로퍼티

### 18.2.3 length 프로퍼티

- length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킴
- `arguments` 객체의 length 프로퍼티는 매개변수의 개수를 가리킴

### 18.2.4 name 프로퍼티

&nbsp;함수 이름을 나타낸다.

```javascript
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

var anonymousFunc = function () {};
// ES5 -> 빈 문자열
// ES6 -> 식별자
console.log(anonymousFunc.name); // anonymousFunc

function bar() {}
console.log(bar.name); // bar
```

### 18.2.5 `__proto__` 접근자 프로퍼티

&nbsp;`__proto__` 프로퍼티는 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다.

```javascript
const obj = { a: 1 };

console.log(obj.__proto__ === Object.prototype); // true

console.log(obj.hasOwnProperty("a")); // true
console.log(obj.hasOwnProperty("__proto__")); // false
```

#### hasOwnProperty 메서드

&nbsp;인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환하고 상속받은 프로토타입의 프로퍼티 키인 경우 false를 반환

### 18.2.6 prototype 프로퍼티

&nbsp;생성자 함수로 호출할 수 있는 함수 객체만이 소유하는 프로퍼티

```javascript
(function (){}).hasOwnProperty('prototype'); // true

({{}}).hasOwnProperty('prototype') // false
```
