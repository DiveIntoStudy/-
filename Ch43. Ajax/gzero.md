## 43.1 Ajax란?

- 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수산하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다
- 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작한다

## 43.2 JSON

- JSON(Javascript Object Notation) 은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.
- JSON 키는 반드시 큰따옴표(작은따옴표 사용 불가)로 묶어야 한다
- JSON 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다. 문자열은 반드시 큰따옴표로 묶어야 한다

### JSON.stringify

- 객체를 JSON 포맷의 문자열로 변환한다
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화라 한다

### JSON.parse

- JSON포맷의 문자열을 객체로 변환한다
- 서버로부터 클라이언트에게 전송된 JSON데이터는 문자열이여서, 객체로 사용하려면 역직렬화를 해야한다

## 43.3. XMLHttpRequest

- 가볍게 책 읽어보기

---

> **JSON.stringify 를 했다가, JSON.parse를 하면 깊은 복사가 이뤄진다?!**

### **얕은 복사와 깊은 복사의 차이**

### **1. 얕은 복사 (Shallow Copy)**

- 객체의 **1단계 속성값**만 복사하며, 중첩된 객체(참조형 데이터)는 **참조값(메모리 주소)**만 복사합니다.
- 원본과 복사본이 **중첩된 데이터(객체, 배열 등)를 공유**하게 되어, 한쪽을 변경하면 다른 쪽에도 영향을 미칠 수 있습니다.

**예:**

```jsx
javascript
코드 복사
const original = { a: 1, b: { c: 2 } };
const shallowCopy = { ...original }; // 얕은 복사

shallowCopy.b.c = 3; // 복사된 b가 원본의 b와 같은 참조를 가짐

console.log(original.b.c); // 3 (원본이 영향을 받음)

```

### **2. 깊은 복사 (Deep Copy)**

- 객체의 모든 계층을 **재귀적으로 복사**하여, 완전히 독립적인 객체를 만듭니다.
- 원본과 복사본은 중첩된 데이터도 **서로 독립적**입니다.

**예:**

```jsx
javascript
코드 복사
const original = { a: 1, b: { c: 2 } };

// 깊은 복사를 수행하는 함수 (JSON 활용)
const deepCopy = JSON.parse(JSON.stringify(original));

deepCopy.b.c = 3; // 복사된 b는 원본과 독립적

console.log(original.b.c); // 2 (원본은 영향을 받지 않음)

```

---

### **`JSON.stringify`와 `JSON.parse`를 이용한 깊은 복사**

`JSON.stringify`와 `JSON.parse`를 조합하면 **깊은 복사**를 수행할 수 있습니다.

### **작동 원리**

1. `JSON.stringify`는 객체를 JSON 문자열로 변환합니다.
2. `JSON.parse`는 이 문자열을 다시 객체로 변환하여 **새로운 참조값을 가진 객체**를 만듭니다.

**예:**

```jsx
javascript
코드 복사
const original = { a: 1, b: { c: 2 } };
const deepCopy = JSON.parse(JSON.stringify(original));

deepCopy.b.c = 3;

console.log(original.b.c); // 2 (깊은 복사로 인해 원본은 영향을 받지 않음)

```

### **주의사항**

`JSON.stringify`와 `JSON.parse`는 일부 데이터 구조와 타입을 올바르게 처리하지 못할 수 있습니다:

1. **순환 참조**: 객체가 자기 자신을 참조할 경우 에러가 발생합니다.
2. **함수**: 함수는 JSON으로 변환되지 않습니다.
3. **`undefined`, `Symbol`**: JSON 문자열로 변환될 때 무시됩니다.
4. **Date 객체**: `toISOString` 문자열로 변환됩니다.
5. **특수 객체 (Map, Set)**: 변환되지 않고 일반 객체로 처리됩니다.

---

### **결론**

`JSON.stringify`와 `JSON.parse`를 이용하면 대부분의 경우에 깊은 복사가 이루어집니다. 다만, 위에서 언급한 **데이터 타입 제한**을 고려해야 합니다. 만약 제한 사항이 문제가 된다면, `lodash`의 `cloneDeep` 같은 라이브러리를 사용하는 것이 더 안전합니다.
