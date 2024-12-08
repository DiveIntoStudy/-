# CH43 Ajax

&nbsp;Ajax(Asynchronous Javascript and XML)란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.

#### Ajax의 장점

- 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
- 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜박이는 현상이 발생하지 않는다.
- 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

## 43.2 JSON

### 43.2.1 JSON 표기 방식

### 43.2.2 JSON.stringify

&nbsp;객체를 JSON 포맷의 문자열로 변환한다.

### 43.2.3 JSON.parse

&nbsp;JSON 포맷의 문자열을 객체로 변환한다.

## 43.3 XMLHttpRequest

&nbsp;자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.

### 43.3.1 XMLHttpRequest 생성

```javascript
const xhr = new XMLHttpRequest();
```

> 뒤에는 그때그때 필요할 때 읽어보기
