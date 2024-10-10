# CH8 제어문

## 8.1 블록문

- 0개 이상의 문을 중괄호로 묶은 것
- 코드 블록 또는 블록이라고도 부름
- 자바스크립트는 이를 하나의 실행 단위로 취급
- 단독으로 사용할 수 있으나 일반적으로 제어문이나 함수를 정의할 때 사용하는 것이 일반적

```javascript
// 블록문(단독 사용한 경우인데 이렇게는 안 쓰지..)
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

## 8.2 조건문

- if ... else 문

```javascript
var x = 2;
var result;

if (x % 2) {
  result = "홀수";
} else {
  result = "짝수";
}

// 삼항 조건 연산자로 바꿔 쓸 수 있다.
var result = x % 2 ? "홀수" : "짝수";
```

- switch 문

```javascript
// default의 순서는 상관이 없으나, 중간에 위치할 경우 break를 걸어주어야 한다.
// 일반적으로 default는 가장 마지막에 작성한다.
var = month = 13;
var monthName;

switch (month) {
    case 1: monthName = "January"; break;
    case 2: monthName = "February"; break;
    // ... 중략
    default: monthName = "Invalid month"; break;
    case 12: monthName = "December"; break;
}

console.log(monthName); // Invalid month
```

## 8.3 반복문

### 8.3.1 for 문

- 조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행

```md
1. 변수 선언문 실행
2. 조건식 실행
3. 조건식의 평가 결과가 참일 경우, 코드 블록 실행
4. 증감식 실행
5. 2~4 반복(조건식의 평가 결과가 거짓일 경우 종료)
```

### 8.3.2 while 문

- 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행
- for 문은 반복 횟수가 명확할 때, while문은 반복 횟수가 불명확할 때 주로 사용
- 조건식의 평가 결과가 언제나 참이면 무한루프가 되고, 탈출하기 위해 break문을 사용

```javascript
var count = 0;

// 무한루프
while (true) {
  console.log(count);
  count++;
  if (count === 3) break;
} // 0 1 2
```

### 8.3.3 do ... while 문

- 코드 블록을 먼저 실행 후 조건식 평가
- 무조건 블록이 한 번 이상 실행됨

```javascript
var count = 0;

do {
  console.log(count); // 0 1 2
  count++;
} while (count < 3);
```

### 8.3.4 break 문

- 레이블 문, 반복문, switch 문의 코드 블록을 탈출
- 레이블 문은 식별자가 붙은 문을 의미(딱히 몰라도 됨... 가독성이 나쁘고 쓰지 않는 것을 권장)

  ```javascript
  foo: console.log("foo");
  ```

- 이 외의 코드 블록에서 사용할 시 SyntaxError(문법 에러)가 발생

### 8.3.5 continue 문

- 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킴

```javascript
for (var i = 0; i < string.length; i++) {
  if (string[i] === search) {
    count++;
    // 그 외 코드들 주루룩 작성
  }
}

// continue 문을 사용하면 if 문 밖에 코드를 작성할 수 있다.
for (var i = 0; i < string.length; i++) {
  if (string[i] !== search) continue;

  count++;
  // 그 외 코드들 주루룩 작성
}
```
