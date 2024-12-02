# CH40 이벤트

## 40.1 이벤트 드리븐 프로그래밍

- 이벤트 핸들러

  이벤트가 발생했을 때 호출될 함수

- 이벤트 핸들러 등록

  이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것

- 이벤트 드리븐 프로그래밍

  프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식

## 40.2 이벤트 타입

&nbsp;마우스, 키보드, 포커스, 폼, 값 변경, DOM 뮤테이션, 뷰, 리소스 등의 이벤트가 있다.

## 40.3 이벤트 핸들러 등록

### 40.3.1 이벤트 핸들러 어트리뷰트 방식

```html
<body>
  <button onclick="sayHi('Lee')">Click me</button>
  <script>
    function sayHi(name) {
      console.log(`Hi! ${name}.`);
    }
  </script>
</body>
```

&nbsp;HTML과 자바스크립트는 관심사가 다르므로 혼재하는 것보다 분리하는 것이 좋다. 이 방법은 권장하지 않는다.

### 40.3.2 이벤트 핸들러 프로퍼티 방식

```html
<body>
  <button>Click me</button>
  <script>
    const $button = document.querySelector("button");

    $button.onclick = function () {
      console.log("button click");
    };
  </script>
</body>
```

### 40.3.3 addEventListener 방식

```html
<body>
  <button>Click me</button>
  <script>
    const $button = document.querySelector("button");

    $button.onclick = function () {
      console.log("[이벤트 핸들러 프로퍼티 방식]button click");
    };
  </script>
</body>
```

## 40.4 이벤트 핸들러 제거

&nbsp;EventTarget.prototype.removeEvent

## 40.5 이벤트 객체

&nbsp;생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달

## 40.6 이벤트 전파

&nbsp;DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트는 DOM 트리를 통해 전파된다.

- 캡처링 단계: 이벤트가 상위 요소에서 하위 요소 방향으로 전파
- 타깃 단계: 이벤트가 이벤트 타깃에 도달
- 버블링 단계: 이벤트가 하위 요소에서 상위 요소 방향으로 전파

## 40.7 이벤트 위임

&nbsp;상위 요소에 이벤트 핸들러를 등록해 하위 요소에서 발생한 이벤트 처리

## 40.8 DOM 요소의 기본 동작 조작

### 40.8.1 DOM 요소의 기본 동작 중단

&nbsp;preventDefault

### 40.8.2 이벤트 전파 방지

&nbsp;stopPropagation

## 40.9 이벤트 핸들러 내부의 this

### 40.9.1 이벤트 핸들러 어트리뷰트 방식

- 이벤트 호출 시 인수로 전달한 this는 이벤트를 바인딩한 DOM 요소를 가리킴
- 그 외에는 전역 객체 window를 가리킴

### 40.9.2 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식

&nbsp;이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킴

## 40.10 이벤트 핸들러에 인수 전달

&nbsp;이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식은 이벤트 핸들러 내부에서 함수를 호출해 인수 전달 가능

## 40.11 커스텀 이벤트

// 한 번 읽어보기
