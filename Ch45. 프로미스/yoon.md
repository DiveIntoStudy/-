# CH45 프로미스

## 45.1 비동기 처리를 위한 콜백 패턴의 단점

### 45.1.1 콜백 헬

&nbsp;콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고 또다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생하는데, 이를 콜백 헬이라 한다.

```javascript
get("/step1", (a) => {
  get(`/step2/${a}`, (b) => {
    get(`/step3/${b}`, (c) => {
      get(`/step4/${c}`, (d) => {
        console.log(d);
      });
    });
  });
});
```

### 45.1.2 에러 처리의 한계

```javascript
try {
  setTimeout(() => {
    throw new Error("Error!");
  }, 1000);
} catch (e) {
  // 에러를 캐치하지 못한다
  console.error(e);
}
```

## 45.2 프로미스의 생성

&nbsp;프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체다.

#### 프로미스의 상태 정보

- pending: 비동기 처리가 아직 수행되지 않은 상태
- fulfilled: 비동기 처리가 수행된 상태(성공)
- rejected: 비동기 처리가 수행된 상태(실패)

## 45.3 프로미스의 후속 처리 메서드

&nbsp;프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다.

### 45.3.1 Promise.prototype.then

- 프로미스가 fulfilled 상태가 되면 호출. 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받음.
- 프로미스가 rejected 상태가 되면 호출. 이때 콜백 함수는 프로미스의 에러를 인수로 전달받음.

### 45.3.2 Promise.prototype.catch

&nbsp;프로미스가 rejected 상태인 경우 호출된다.

### 45.3.3 Promise.prototype.finally

&nbsp;프로미스의 성공/실패 여부와 상관없이 무조건 한 번 호출된다.

## 45.4 프로미스의 에러 처리

&nbsp;catch 메서드를 사용

## 45.5 프로미스 체이닝

```javascript
(async () => {
  const { userId } = await promiseGet(url);

  const userInfo = await promiseGet(url2);

  console.log(userInfo);
})();
```

## 45.6 프로미스의 정적 메서드

### 45.6.1 Promise.resolve / Promise.reject

### 45.6.2 Promise.all

&nbsp;여러 개의 비동기 처리를 모두 병렬 처리할 때 사용

### 45.6.3 Promise.race

&nbsp;가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스를 반환

### 45.6.4 Promise.allSettled

&nbsp;전달받은 프로미스가 모두 settled 상태가 되면 처리 결과를 배열로 반환

## 45.7 마이크로태스크 큐

- 프로미스의 후속 처리 메서드의 콜백 함수가 일시 저장되는 곳
- 태스크 큐보다 우선순위가 높음

## 45.8 fetch

- XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API
- 404, 500에도 에러를 reject하지 않고 불리언 타입의 ok 상태를 false로 설정한 Response 객체를 resolve
- 오프라인 등의 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 reject
