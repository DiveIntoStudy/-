## 40.1 이벤트 드리븐 프로그래밍

- 이벤트 핸들러: 이벤트가 발생했을 때 호출될 함수
- 이벤트 핸들러 등록: 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것
- 브라우저에게 위임을 하는 이유는?
  - 예를 들어, 사용자가 버튼을 클릭했을 때 함수를 호출하여 어떤 처리를 하고 싶을때, 개발자는 사용자가 언제 버튼을 클릭할지 알 수 없으므로 브라우저에게 사용자의 버튼 클릭을 감지하여 클릭 이벤트를 발생시킬 수 있도록 위임하는 것이다

## 40.2 이벤트 타입

- 이벤트 타입은 이벤트 종류를 나타내는 문자열이다.
- 이벤트 타입은 약 200여 가지가 있기 때문에, 필요한 이벤트가 있다면 검색해서 확인할 것!

## 40.3 이벤트 핸들러 등록

- 이벤트가 발생하면 브라우저에게 의해 호출될 함수를 이벤트 핸들러라 한다

### 이벤트 핸들러 프로퍼티 방식

- 이벤트 핸들러를 등록하기 위해서는 이벤트를 발생시킬 객체인 이벤트 타킷, 이벤트 종류를 나타내는 문자열인 이벤트 타입, 이벤트 핸들러를 지정할 필요가 있다.
- 하나의 이벤트에 하나의 이벤트 핸들러만 바인딩할 수 있다

### addEventlistener 메서드 방식

```jsx
EventTarget.addEventListener('eventType', functionName [, useCapture]);

/*
EventTarget = 이벤트 타깃
eventType = 이벤트 타입
functionName = 이벤트 핸들러
useCapture = capture 사용여부 (true: capturing, false: bubbling(기본값))
```

- 하나 이상의 이벤트 핸들러를 등록할 수 있다
- 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 핸들러만 동록된다

## 40.4 이벤트 핸들러 제거

`addEventListener` 메서드로 등록한 이벤트 핸들러를 제거하려면 `removeEventListener` 메서드를 사용한다. `removeEventListenr` 메서드에 전달할 인수는 `addEventListener` 메서드와 동일해야 한다. 동일하지 않으면 이벤트 핸들러가 제거되지 않는다.

## 40.5 이벤트 객체

- 이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성된다
- 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다

### 40.5.1 이벤트 객체의 상속 구조

- 이벤트가 발생하면 이벤트 타입에 따라 다양한 타입의 이벤트 객체가 생성된다
- 이벤트 객체 중 일부는 사용자의 행위에 의해 생성된 것이고 일부는 자바스크립트 코드에 의해 인위적으로 생성된 것이다
- Event 인터페이스는 DOM 내에서 발생한 이벤트에 의해 생성되는 이벤트 객체를 나타낸다. Event 인터페이스에는 모든 이벤트 객체의 공통 프로퍼티가 정의되어 있고 FocusEvent, MouseEvent, KeyboardEvent, WheelEvent 같은 하위 인터페이스에는 이벤트 타입에 따라 고유한 프로퍼티가 정의되어 있다.

### 40.5.2 이벤트 객체의 공통 프로퍼티

Event 인터페이스의 이벤트 관련 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티다.

| **공통 프로퍼티**                                            | **설명**                             | **타입**      |
| ------------------------------------------------------------ | ------------------------------------ | ------------- |
| type                                                         | 이벤트 타입                          | string        |
| target                                                       | 이벤트를 발생시킨 DOM 요소           | DOM 요소 노드 |
| currentTarget                                                | 이벤트 핸들러가 바인딩된 DOM 요소    | DOM 요소 노드 |
| eventPhase                                                   | 이벤트 전파 단계                     |
| 0: 이벤트 없음, 1: 캡쳐링 단계, 2: 타깃 단계, 3: 버블링 단계 | number                               |
| bubbles                                                      | 이벤트를 버블링으로 전파하는지 여부. |

다음 이벤트는 bubbles:false로 버블링하지 않는다.

- 포커스 이벤트 focus/blur
- 리소스 이벤트 load/unload/abort/error
- 마우스 이벤트 mouseenter/mouseleave | boolean |
  | cancelable | preventDefault 메서드를 호출하여 이벤트의 기본 동작을 취소할 수 있는지 여부. 다음 이벤트는 cancelable: false로 취소할 수 없다
- 포커스 이벤트 focus/blur
- 리소스 이벤트 load/unload/abort/error
- 마우스 이벤트 dblclick/mouseenter/mouseleave | boolean |
  | defaultPrevented | preventDefault 메서드를 호출하여 이벤트를 취소했는지 여부 | boolean |
  | isTrusted | 사용자의 행위에 의해 발생한 이벤트인지 여부.
  예를 들어, click 메서드 또는 dispatchEvent 메서드를 통해 인위적으로 발생시킨 이벤트인 경우 isTrusted는 false다. | boolean |
  | timeStamp | 이벤트가 발생한 시각(1970/01/01/00:00:0부터 경과한 밀리초) | number |

## 40.6 이벤트 전파

- DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트는 DOM 트리를 통해 전파되는 것을 이벤트 전파라고 한다.
- 생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM 트리를 통해 전파된다.
- 이벤트 전파 3단계
  - 캡쳐링 단계: 이벤트가 상위 요소 → 하위 요소로 전파
  - 타깃 단계: 이벤트가 타깃에 도달
  - 버블링 단계: 이벤트가 하위 요소 → 상위 요소로 전파

```jsx
<!DOCTYPE html>
<html>
<body>
	<ul id="fruits">
		<li id="apple">Apple</li>
		<li id="banana">Banana</li>
		<li id="orange">Orange</li>
	</ul>
	<script>
		const $fruits = document.getElementById('fruits');

		$fruits.addEvnetListener('click', e => {
				console.log(`이벤트 타깃: ${e.targe}`) // [object HTMLLIElement]
				console.log(`커런트 타깃: ${e.currentTarget}`) // [object HTMLUListElement]
	</sciprt>
</body>
</html>
```

- li 요소 클릭 ⇒ 클릭 `이벤트 객체`가 생성되고, 클릭된 li 요소가 `이벤트 타깃`이 됨 ⇒ 이벤트 객체는 window에서 시작해서 이벤트 타깃 방향으로 전파(`캡쳐링`) ⇒ 이벤트 객체는 이벤트 타깃에 도달(`타깃` 단계) ⇒ 이벤트 객체는 이벤트 타깃에서 시작해서 window 방향으로 전파됨(`버블링`)

## 40.7 이벤트 위임

- 이벤트 위임은 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법을 말한다

## 40.8 DOM 요소의 기본 동작 조작

- **DOM 요소의 기본 동작 중단** ⇒ `preventDefault` 메서드
- **이벤트 전파 방지** ⇒ `stopPropagation` 메서드

## 40.9 이벤트 핸들러 내부의 this

- 이벤트 핸들러를 호출할 때 인수로 전달한 this는 이벤트를 바인딩한 DOM 요소를 가리킨다
- 화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this를 가리킨다

## 40.10 이벤트 핸들러에 인수 전달

- 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
