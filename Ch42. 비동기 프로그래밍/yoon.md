# CH42 비동기 프로그래밍

## 42.1 동기 처리와 비동기 처리

&nbsp;자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖는다. 이처럼 자바스크립트 엔진은 한 번에 하나의 태스크만 실행할 수 있는 싱글 스레드 방식으로 동작해 블로킹(작업 중단)이 발생한다.

&nbsp;현재 실행중인 태스크가 종료할 때까지 다음에 실행될 태스크가 대기하는 방식을 동기, 현재 실행 중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하는 방식을 비동기 처리라고 한다.

&nbsp;타이머 함수인 setTimeout과 setInterval, HTTP 요청, 이벤트 핸들러는 비동기 처리 방식으로 동작한다.

## 42.2 이벤트 루프와 태스크 큐

&nbsp;자바스크립트의 동시성을 지원하는 것이 이벤트 루프이다.

#### 자바스크립트 엔진

- 콜 스택
  - 소스코드 평가 과정에서 실행된 실행 컨텍스트가 추가되고 제거되는 스택 자료구조인 실행 컨텍스트 스택
  - 함수 호출 시, 함수 실행 컨텍스트가 순차적으로 콜 스택에 푸시되어 실행됨
- 힙

  - 객체가 저장되는 메모리 공간
  - 콜 스택의 요소인 실행 컨텍스트는 힙에 저장된 객체를 참조
  - 객체는 원시 값과 달리 크기가 정해져 있지 않아 할당해야 할 메모리 공간의 크기를 런타임에 결정(동적 할당)

#### 브라우저 환경 제공

- 태스크 큐
  - 비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역
  - 마이크로태스크 큐도 따로 존재함
- 이벤트 루프
  - 콜 스택이 비어 있고 태스크 큐에 대기 중인 함수가 있다면 이벤트 루프는 순차적으로 태스크 큐에 대기 중인 함수를 콜 스택으로 이동시킴

> 자바스크립트 엔진은 싱글 스레드! 브라우저는 멀티 스레드!
