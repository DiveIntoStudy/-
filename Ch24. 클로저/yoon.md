# CH24 클로저

## 24.1 렉시컬 스코프

&nbsp;자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다. 이를 렉시컬 스코프(정적 스코프)라 한다. 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다.

## 24.2 함수 객체의 내부 슬롯 [[Environment]]

&nbsp;함수는 자신의 내부 슬롯 [[Environment]]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.

#### 함수 코드 평가 순서

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
   2.1. 함수 환경 레코드 생성
   2.2. this 바인딩
   2.3. 외부 렉시컬 환경에 대한 참조 결정

## 24.3 클로저와 렉시컬 환경

```javascript
const x = 1;

function outer() {
  const x = 10;
  const inner = function () {
    console.log(x);
  };
  return inner;
}

const innerFunc = outer(); // 3
innerFunc(); // 10 -> ???
```

&nbsp;**외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다.** 이러한 중첩 함수를 클로저라고 부른다.
&nbsp;3에서 inner를 반환하고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다. outer 함수의 렉시컬 환경은 inner 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있고 inner 함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않기 때문이다. 가비지 컬렉터는 누군가가 참조하고 있는 메모리 공간을 함부로 해제하지 않는다.

- 클로저
  - 중첩 함수가 상위 스코프의 식별자를 참조
  - 중첩 함수가 외부 함수보다 더 오래 유지되는 경우

## 24.2 클로저의 활용

- 상태를 안전하게 변경하고 유지
- 상태를 안전하게 은닉
- 특정 함수에게만 상태 변경을 허용

#### 생성자 함수 ver.

```javascript
const Counter = (function () {
  let num = 0;

  function Counter() {}

  Counter.prototype.increase = function () {
    return ++num;
  };

  Counter.prototype.decrease = function () {
    return num > 0 ? --num : 0;
  };

  return Counter;
})();

const counter = new Counter();
```

### 함수형 프로그래밍 ver.

> 어렵다...

```javascript
// 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
const counter = (function () {
  let counter = 0;

  // 함수를 인수로 전달받는 클로저를 반환
  return function (aux) {
    counter = aux(counter);

    return counter;
  };
})();

function increase(n) {
  return ++n;
}

function decrease(n) {
  return --n;
}
console.log(counter(increase));
```

## 24.5 캡슐화와 정보 은닉

- 클래스에 private 필드 정의 가능
- 25.7.4절에서 공부
- [MDN 참조](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes/Private_properties)

## 24.6 자주 발생하는 실수

&nbsp;var를 사용했을 때 발생할 수 있는 실수를 클로저, let 사용, 고차함수 사용 3가지 방법으로 보여주는 부분. 재미로 읽어보기.
