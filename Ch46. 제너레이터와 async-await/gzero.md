## 46.1 제너레이터란

- 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다
- 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다
  - 함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도(yield)할 수 있다
- 제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있다
- 제너테리어 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면섯 동시에 이터레이터인 제너레이터 객체를 반환한다.

## 46.2 제너레이터 함수의 정의

- function\* 키워드로 선언하고, 하나 이상의 yield 표현식을 포함한다
- 에스터리스크(\*)의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관없다
- 제너레이터 함수는 화살표 함수로 정의할 수 없다
- 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할 수 없다

## 46.3 제너레이터 객체

- 제너레이터 함수를 호출하면 일반 함수처럼 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다
- 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터다
  - 제너레이터 객체는 Symbol.iterator 메서드를 상속받는 이터러블이다
  - value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터다.
- 제너레이터 객체는 이터레이터에 없는 return, throw 메서드는 갖는다
- 제너레이터 객체의 메서드
  ```jsx
  function* getFunc() {
    try {
      yield 1;
      yield 2;
      yield 3;
    } catch (e) {
      console.error(e);
    }
  }

  const generator = getFunc();

  console.log(generator.next()); // {value: 1, done: false}
  console.log(generator.return("End")); // {value: 'End', done: true}
  console.log(generator.throw("Error!")); // {value: 'Error!', done: true}
  ```
  - `next 메서드` : next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다
  - `return 메서드` : 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다
  - `throw 메서드` : 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다

## 46.4 제너레이터의 일지 중지와 재개

```jsx
function* getFunc() {
	yield 1;
	yield 2;
	yield 3;
}

const generator = getFunc();

// 1
console.log(generator.next()); {value: 1, done: false}
// 2
console.log(generator.next()); {value: 2, done: false}
// 3
console.log(generator.next()); {value: 3, done: false}
// 4
console.log(generator.next()); {value: undefined. done: true}

// 4번째 next 메서드를 호출하면 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행한다
// value 프로퍼티에는 제너레이터 함수의 반환값 undefined가 할당된다
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 true가 할당된다
```

```jsx
function* getFunc() {
  // 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
  // 이때 yield된 값 1은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다
  // x 변수에는 아직 아무것도 할당되지 않았다. x 변수의 값은 next 메서드가 두 번째 호출될 때 결정된다
  const x = yield 1;

  // 두 번째 next 메서드를 호출할 때 전달한 인수 10은 첫 번째 yield 표현식을 할당받는 x 변수에 할당된다.
  // 즉, const x = yield 1; 은 두 번째 next 메서드를 호출했을 때 완료된다
  // 두 번째 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
  // 이때 yield 된 값 x + 10은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
  const y = yield x + 10;

  // 세 번째 next 메서드를 호출할 때 전달한 인수 20은 두 번째 yield 표현식을 할당받는 y 변수에 할당된다
  // 즉, const y = yield(x + 10)은 세 번째 next 메서드를 호출했을 때 완료된다
  // 세 번째 next 메서드를 호출하면 함수 끝까지 실행된다
  // 이때 제너레이터 함수의 반환값 x + y는 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
  // 일반적으로 제너레이터의 반환값은 의미가 없다.
  // 따라서 제너레이터에서는 값을 반환할 필요가 없고 return은 종료의 의미로만 사용해야 한다
  return x + y;
}

const generator = getFunc(0);

let res = generator.next();
console.log(res); // {value: 1, done: false}

res = generator.next(10);
console.log(res); // {value: 20, done: false}

res = genertore.next(20);
console.log(res); // {value: 30, done: true}
```

## 46.5 제너레이터의 활용

이터러블의 구현과 비동기 처리를 구현할 수 있다. 예제는 책으로 확인하기

## 46.6 async/await

async/await 는 프로미스를 기반으로 동작하고, **동기 처리처럼 프로미스를 사용**할 수 있다.

### 46.6.1 async 함수

- await 키워드는 반드시 async 함수 내부에서 사용해야 한다
- async 함수는 언제나 프로미스를 반환한다
- 명시적으로 프로미스를 반환하지 않더라도 암묵적으로 반환값을 resolve 하는 프로미스를 반환한다

### 46.6.2 await 키워드

- await 키워드는 settled 상태가 될 때까지 대기하다가, settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
- await 키워드는 반드시 프로미스 앞에서 사용해야 한다. (사실 await는 어디서든 사용해도 상관없으나, 원칙은 프로미스 앞에서만 사용해야한다)
- 3개의 비동기 처리가 서로 연관이 없다면 Promise.all를 사용하는 것이 더 좋고, 서로 연관이 있다면 모든 프로미스에 await를 써서 순차적으로 처리해야한다

### 46.6.3 에러 처리

- async/await에서 에러 처리는 try … catch 문을 사용할 수 있다
- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다
