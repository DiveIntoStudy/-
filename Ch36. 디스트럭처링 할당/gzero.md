- 구조 분해 할당(destructuring assignment)은 구조화된 배열과 같은 이터러블 또는 객체를 비구조화(destructuring)하여 1개 이상의 변수에 개별적으로 할당하는 것
- 배열과 같은 이터러블, 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용하다

## 36.1 배열 디스트럭처링 할당

- 배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 하며, 할당 기준은 배열의 인덱스다. 즉, 순서대로 할당된다.
  ```jsx
  const arr = [1, 2, 3];
  const [a, b, c] = arr;
  console.log(a, b, 3); // 1 2 3
  ```
- 배열 디스트럭처링 할당을 위한 변수에 Rest 요소를 사용할 수 있다.
  ```jsx
  const [x, ...y] = [1, 2, 3];
  console.log(x, y); // 1 [2, 3]
  ```

## 36.2 객체 디스트럭처링 할당

- 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.
- 객체 디스트럭처링 할당의 대상(할당문의 우변)은 객체어야 하고, 할당 기준은 프로퍼티 키다
- 순서는 의미가 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당된다
  ```jsx
  const { firstName = "Ungmo", lastName } = { lastName: "Lee" };
  console.log(firstName, lastName); // Ungmo Lee

  const { firstName: fn = "hoho", lastName: ln } = { lastName: "Lee" };
  console.log(fn, ln); // hoho Lee
  ```
- 배열 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있다
  ```jsx
  const todos = [
    { id: 1, content: "HTML", completed: true },
    { id: 2, content: "CSS", completed: false },
    { id: 3, content: "JS", completed: false },
  ];

  const [, { id }, { content }] = todos;
  console.log(id, content); // 2 JS
  ```
