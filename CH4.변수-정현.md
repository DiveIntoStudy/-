# CH4 변수

## 4.1 변수란 무엇인가? 왜 필요한가?

### 변수란,

1. 하나의 값을 저장하기 위해 확보한 메모리 공간 자체
2. 메모리 공간을 식별하기 위해 붙인 이름(식별자)

```javascript
var result = 10 + 20;
```

10, 20, 그리고 연산한 결과인 30은 각각 다른 메모리 공간에 저장되어 있음

- 변수 이름(변수명)
  - 메모리 공간에 저장된 값을 식별할 수 있는 고유한 이름
- 변수 값
  - 변수에 저장된 값(위 예제에서는 30)
- 할당
  - 변수에 값을 저장하는 것

## 4.2 식별자

- 어떤 값을 구별해서 식별할 수 있는 고유한 이름
- 값이 아니라 **메모리 주소를 기억**

## 4.3 변수 선언

&nbsp;변수를 사용하려면 반드시 선언이 필요하고, `var`, `let`, `const` 키워드를 사용해 선언할 수 있다.

```javascript
var score; // 변수 선언(변수 선언문)
```

`var` 키워드로 선언한 변수는 자동으로 `undefined`로 초기화가 된다.

선언하지 않은 식별자에 접근하면 `ReferenceError(참조 에러)`가 발생한다.

## 4.4 변수 선언의 실행 시점과 변수 호이스팅

```javascript
console.log(score); // undefined

var score; // 변수 선언문
```

&nbsp;변수 선언이 소스코드가 한 줄씩 순차적으로 실행되는 시점, 즉 런타임이 아니라 그 이전 단계에서 먼저 실행되기 때문에 참조 에러가 발생하지 않고 `undefined`가 출력된다.

&nbsp;이처럼 변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 **변수 호이스팅**이라 한다. 이는 변수 선언뿐만이 아니라 `var`, `let`, `const`, `function`, `class` 키워드를 사용해 선언하는 모든 식별자에서도 동일하게 발생한다.

## 4.5 값의 할당

```javascript
var score; // 변수 선언
score = 80; // 값의 할당

var score = 80; // 변수 선언과 값의 할당
```

&nbsp;변수 선언은 소스코드가 순차적으로 실행되는 시점인 런타임 이전에 먼저 실행되지만 값의 할당은 소스코드가 순차적으로 실행되는 시점인 런타임에 실행된다(아래 예시 참조)

```javascript
console.log(score); // undefined

var score = 80; // 변수 선언과 값의 할당

console.log(score); // 80
```

그렇다면?

```javascript
console.log(score); // undefined

score = 80; // 값의 할당
var score; // 변수 선언

console.log(score); // ??
```

=> 정답은 `80`

```javascript
var score;

console.log(score);

score = 80;

console.log(score);
```

과 동일하기 때문(호이스팅으로 인해 변수 선언문이 최상단으로 끌어올려짐)

## 4.6 값의 재할당

&nbsp;이미 값이 할당되어 있는 변수에 새로운 값을 또다시 할당하는 것을 **재할당**이라고 한다. 재할당 할 수 없는, 단 한 번만 할당할 수 있는 변수는 **상수**라고 한다.

```javascript
var score = 80;
score = 90;
```

이 경우, score에는 `undefined`, 80, 90 차례로 할당이 되는데 새로 할당 될 때마다 새로운 메모리 공간에 값을 저장하게 된다. 더 이상 참조하지 않게 된 불필요한 값들은 가비지 콜렉터에 의해 메모리에서 자동 해제된다. 단, 메모리에서 언제 해제될지는 예측할 수 없다.

### 가비지 콜렉터

- 애플리케이션이 할당한 메모리 공간을 주기적으로 검사하여 더 이상 사용되지 않는 메모리를 해제하는 기능
- 어떤 식별자도 참조하지 않는 메모리 공간을 더 이상 사용되지 않는 메모리라 칭함

## 4.\*

- 강한 참조

  - 어떤 식별자에 의해 참조되고 있다면 가비지 컬렉션의 대상이 되지 않음

  ```javascript
  //  user가 객체를 강한 참조하고 있음
  let user = { name: "John" };

  // admin이 아까 그 객체를 강한 참조하고 있음
  let admin = user;

  // user에 null 할당
  user = null;

  // 여전히 admin 값을 통해 객체에 접근할 수 있음
  ```

- 약한 참조

  - 어떤 식별자도 참조하지 않는 것과 동일하게 언젠가 제거될 수 있음
  - [WeakRef](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakRef)

  ```javascript
  //  user가 객체를 강한 참조하고 있음
  let user = { name: "John" };

  //  admin이 이 객체를 약한 참조하고 있음
  let admin = new WeakRef(user);

  // 강한 참조를 끊음
  user = null;

  // 이제 해당 객체가 강한 참조되고 있지 않기 때문에 가비지 콜렉터에 의해 제거되었을 수도 있고 살아있을 수도 있음
  ```

  - 참고자료 - [WeakRef and FinalizationRegistry
    ](https://javascript.info/weakref-finalizationregistry)
