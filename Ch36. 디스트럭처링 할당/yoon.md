# CH36 스프레드 문법

## 36.1 배열 디스트럭처링 할당

&nbsp;배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 하며, 할당 기준은 배열의 인덱스다.

```javascript
const [x, y] = [1, 2];

// 개수가 안 맞을 때
const [c, d] = [1]; // 1 undefined

// 기본값
const [a, b, c = 3] = [1, 2];

// Rest 요소
const [x, ...y] = [1, 2, 3];
```

## 36.2 객체 디스트럭처링 할당

```javascript
const { lastName, firstName } = { firstName: "Ungmo", lastName: "Lee" };

// 기본값 설정
const { lastName, firstName = "Ungmo" } = { lastName: "Lee" };

// 매개변수에 사용
function printTodo({ content, completed }) {
  console.log(`할일 ${content}은 ${completed ? "완료" : "비완료"} 상태입니다.`);
}

// Rest 프로퍼티
const { x, ...rest } = { x: 1, y: 2, z: 3 };
```
