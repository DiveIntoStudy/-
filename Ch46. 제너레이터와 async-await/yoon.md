# CH46 제너레이터와 async/await

## 46.1 제너레이터란?

&nbsp;코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

## 46.2 제너레이터 함수의 정의

&nbsp;제너레이터 함수는 function\* 키워드로 선언하고, 하나 이상의 yield 표현식을 포함한다.

```javascript
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}
```

## 46.3 제너레이터 객체

&nbsp;제너레이터 함수를 호출하면 일반 함수처럼 함수 코드블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.

```javascript
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();

// 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 갖는다.
console.log("next" in generator); // true
```

- next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

## 46.4 제너레이터의 일시 중지와 재개

&nbsp;yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.

```javascript
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc();

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 첫 번째 yield 표현식에서 yield된 값 1이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 1, done: false}

// 다시 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 세 번째 yield 표현식에서 yield된 값 3이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next());

// 다시 next 메서드를 호출하면 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행한다.
// next 메서드는 이터레이터 리절트 객체({value, down})를 반환한다.
// value 프로퍼티에는 제너레이터 함수의 반환값 undefined가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 true가 할당된다.
console.log(generator.next());
```

## 46.5 제너레이터의 활용

### 46.5.1 이터러블의 구현

```javascript
// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
})();

// infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8 ... 2584 4181 6765
}
```

### 46.5.2 비동기 처리

&nbsp;프로미스의 후속 처리 메서드 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.

```javascript
const fetch = require("node-fetch");

// 제너레이터 실행기
const async = (generatorFunc) => {
  const generator = generatorFunc();

  const onResolved = (arg) => {
    const result = generator.next(arg);

    return result.done
      ? result.value
      : result.value.then((res) => onResolved(res));
  };

  return onResolved;
};

async(function* fetchTodo() {
  const url = "https://jsonplaceholder.typicode.com/todos/1";

  const response = yield fetch(url);
  const todo = yield response.json();
  console.log(todo);
});
```

## 46.6 async/await

&nbsp;async/await는 프로미스를 기반으로 동작하며, async/await를 사용하면 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

### 46.6.1 async 함수

&nbsp;await 키워드는 반드시 async 함수 내부에서 사용해야 한다. async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.

### 46.6.2 await 함수

&nbsp;await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다. await 키워드는 반드시 프로미스 앞에서 사용해야한다.

### 46.6.3 에러 처리

- try-catch문을 이용
- 비동기 함수의 콜백함수를 호출한 것은 비동기 함수가 아니기 때문에 try-catch로 에러를 캐치할 수 없음
- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.

> 제너레이터는 언제 쓰는 걸까..? 역사 공부인가?
