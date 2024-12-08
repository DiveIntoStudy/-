## 45.1 비동기 처리를 위한 콜백 패턴의 단점

- 콜백 헬로 인해 가독성이 나쁘고 비동기 처리 중 발생한 에러의 처리가 곤란하며 여러 개의 비동기 처리를 한번에 처리하는 데 한계가 있다

## 45.2 프로미스의 생성

```jsx
const promiseGet = (url) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.send();
    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(JSON.parse(xhr.response));
      } else {
        reject(new Error(xhr.status));
      }
    };
  });
};

promiseGet("https://~~~");
```

- Promise 생성자 함수를 new 연산자와 함께 호출하면 프로미스(Promise 객체)를 생성한다
- ES6에서 도입된 Promise는 호스트 객체가 아닌 ECMAScript 사양에 정의된 표준 빌트인 객체다
- Promise 생성자 함수는 비동기 처리를 수행할 콜백함수를 인수로 전달받는데 이 콜백 함수는 resolve와 reject 함수를 인수로 전달받는다
- 프로미스 상태는 resolve 또는 reject 함수를 호출하는 것으로 결정된다
  | **프로미스의 상태 정보** | **의미**                              | **상태 변경 조건**               |
  | ------------------------ | ------------------------------------- | -------------------------------- |
  | pending                  | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
  | fulfilled                | 비동기 처리가 수행된 상태(성공)       | resolve 함수 호출                |
  | rejected                 | 비동기 처리가 수행된 상태(실패)       | reject 함수 호출                 |
- settled
  - fulfilled 또는 rejected 상태를 settle라고 한다.
  - fulfilled 또는 rejected 상태와 상관없이 pending이 아닌 상태로 비동기 처리가 수행된 상태를 말한다
  - 일단 settled 상태가 되면 더는 다른 상태로 변화할 수 없다
- 프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체다

## 45.3 프로미스의 후속 처리 메서드

### 45.3.1 Promise.prototype.then

then 메서드는 두 개의 콜백 함수를 인수로 전달받는다.

```jsx
new Promise(resolve => resolve('fulfilled'))
	.then(v => console.log(v), e => console.log(e));

new Promise((_, reject) => reject(new Error('rejected'))
	.then(v => console.log(v), e => console.log(e));
```

### 45.3.2 Promise.prototype.catch

프로미스가 rejected 상태인 경우만 호출된다

```jsx
new Promise((_, reject) => reject(new Error('rejected'))
	.catch(e => console.log(e))
```

### 45.3.3 Promise.prototype.finally

- 성공(fulfilled), 실패(rejected)와 상관없이 무조건 한 번만 호출된다.
- finally 메서드도 then/catch 메서드와 마찬가지로 언제나 프로미스를 반환한다.

```jsx
promiseGet("https://~~~")
  .then((res) => console.log(res))
  .catch((error) => console.log(error))
  .finally(() => console.log("Bye!"));
```

## 45.4 프로미스의 에러 처리

- catch 메서드를 모든 then 메서드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러(rejected상태)뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치할 수 있다
- 에러처리는 catch 메서드에서 하는 것을 권장한다

## 45.5 프로미스 체이닝

책 읽어보기

## 45.6 프로미스의 정적 메서드

### 45.6.1 Promise.resolve / Promise.reject

### 45.6.2 Promise.all

```jsx
const req1 = () => new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const req2 = () => new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const req3 = () => new Promise((resovle) => setTimeout(() => resolve(3), 1000));

Promise.all([req1(), req2(), req3()]).then(console.log).catch(console.error);
```

- 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용
- 인수로 전달받은 배열의 모든 프로미스가 모두 fulfilled 상태가 되면 종료한다
- 인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료한다

### 45.6.3 Promise.race

- 인수로 전달받은 배열의 프로미스 중 가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스로 반환한다

### 45.6.4 Promise.allSettled

- 전달받은 프로미스가 모두 settled 상태가 되면 처리 결과값을 배열로 반환한다

## 45.7 마이크로태스크 큐

```jsx
setTimeout(() => console.log(1), 0);

Promise.resolve()
  .then(() => console.log(2))
  .then(() => console.log(3));
```

- 프로미스의 후속 처리 메서드도 비동기로 동작하므로 1 → 2 → 3 순으로 출력될 것처럼 보이지만 2 → 3 → 1 순으로 출력된다
- 그 이유는 프로미스의 후속 처리 메서드의 콜백함수는 태스크 큐가 아니라 마이크로 태스크 큐에 저장되기 때문이다
- 마이크로 태스크큐는 태스크 큐와 별도의 큐다. 마이크로태스크 큐에는 프로미스의 후속 처리 메서드의 콜백 함수가 일시 저장된다. 그 외의 비동기 함수의 콜백 함수나 이벤트 핸들러는 태스크 큐에 일시 저장된다
- 콜백 함수나 이벤트 핸들러를 일시 저장한다는 점에서 태스크 큐와 동일하지만 마이크로태스크 큐는 태스크큐보다 우선순위가 높다
- 이벤트 루프는 콜 스택이 비면 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행한다. 이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행한다
