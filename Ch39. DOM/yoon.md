# CH39 DOM

&nbsp;DOM(Document Object Model)은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조

## 39.1 노드

### 39.1.1 HTML 요소와 노드 객체

&nbsp;HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다. HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.

#### 트리 자료구조

&nbsp;트리 자료구조는 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조를 표현하는 비선형 자료구조를 말한다. 노드 객체들로 구성된 트리 자료구조를 DOM이라 한다.

### 39.1.2 노드 객체의 타입

#### 문서 노드

&nbsp;문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드로서 `document` 객체를 가리킨다. 이는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 한다.

#### 요소 노드

&nbsp;요소 노드는 HTML 요소를 가리키는 객체다.

#### 어트리뷰트 노드

&nbsp;어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체다.

#### 텍스트 노드

&nbsp;텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체다.

### 39.1.3 노드 객체의 상속 구조

&nbsp;DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.

## 39.2 요소 노드 취득

### 39.2.1 id를 이용한 요소 노드 취득

`Document.prototype.getElementById`

### 39.2.2 태그 이름을 이용한 요소 노드 취득

`Document.prototype/Element.prototype.getElementsByTagName`

### 39.2.3 class를 이용한 요소 노드 취득

`Document.prototype/Element.prototype.getElementsByClassName`

### 39.2.4 CSS 선택자를 이용한 요소 노드 취득

`Document.prototype/Element.prototype.querySelector`, `Document.prototype/Element.prototype.querySelectorAll`

### 39.2.5 특정 요소 노드를 취득할 수 있는지 확인

`Element.prototype.matches`로 이벤트 위임을 사용할 때 유용하다.

### 39.2.6 HTMLCollection과 NodeList

#### HTMLCollection

&nbsp;`getElementsByTagName`, `getElementsByClassName` 메서드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 DOM 컬렉션 객체다.

#### NodeList

&nbsp;HTMLCollection 객체의 부작용(상태 변화 실시간 반영으로 인해 발생하는 것)을 해결하기 위해 `querySelectorAll` 메서드를 사용하는 방법도 있다. 이는 NodeList 객체를 반환한다. NodeList 객체는 상태 변경을 실시간으로 반영하지 않지만 childNodes 프로퍼티가 반환하는 NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하므로 주의가 필요하다.

&nbsp;노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장한다.

## 39.3 노드 탐색

`parentNode`, `previousSibling`, `firstChilde`, `childNodes`는 `Node.prototype`이 제공하고, 프로퍼티 키에 `Element`가 포함된 `previousElementSibling`, `nextElementSibling`과 `children` 프로퍼티는 `Element.prototype`이 제공한다.

### 39.3.1 공백 텍스트 노드

&nbsp;HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드를 생성한다.

### 39.3.2 자식 노드 탐색

- Node.prototype.childNodes
  - 반환된 NodeList에는 텍스트 노드도 포함되어 있을 수 있음
- Element.prototype.children
  - 반환된 HTMLCollection에는 텍스트 노드가 포함되지 않음
- Node.prototype.firstChild
  - 첫 번째 자식 노드 반환
  - 텍스트 노드이거나 요소 노드
- Node.prototype.lastChild
  - 마지막 자식 노드 반환
  - 텍스트 노드이거나 요소 노드
- Element.prototype.firstElementChild
  - 첫 번째 자식 요소 노드를 반환
  - 요소 노드만 반환
- Element.prototype.lastElementChild
  - 마지막 자식 요소 노드를 반환
  - 요소 노드만 반환

### 39.3.3 자식 노드 존재 확인

- `Node.prototype.hasChildNodes`
- 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 `children.length` 또는 `childElementCount` 프로퍼티를 사용

### 39.3.4 요소 노드의 텍스트 노드 탐색

`firstChild`

### 39.3.5 부모 노드 탐색

`Node.prototype.parentNode`

### 39.3.6 형제 노드 탐색

- `Node.prototype.previousSibling`
  - 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환
  - 요소 노드, 텍스트 노드
- `Node.prototype.nextSibling`
  - 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환
  - 요소 노드, 텍스트 노드
- `Element.prototype.previousElementSibling`
  - 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환
  - 요소 노드
- `Element.prototype.nextElementSibling`
  - 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환
  - 요소 노드

## 39.4 노드 정보 취득

- `Node.prototype.nodeType`
  - 노드 타입을 나타내는 상수 반환
  - Node.ELEMENT_NODE: 1
  - Node.TEXT_NODE: 3
  - Node.DOCUMENT_NODE: 9
- `Node.prototype.nodeName`
  - 노드 이름을 문자열로 반환
  - 요소 노드: 대문자 문자열로 태그이름 반환
  - 텍스트 노드: 문자열 "#text"를 반환
  - 문서 노드: 문자열 "#document"를 반환

## 39.5 요소 노드의 텍스트 조작

### 39.5.1 nodeValue

&nbsp;텍스트 노드의 값을 가져오거나 조작할 수 있다. 그 외 노드에서는 null값을 반환한다.

### 39.5.2 textContent

&nbsp;내부의 텍스트 값이 반환된다. 이때, HTML 마크업은 무시된다.

#### innerText 사용을 지양해야 하는 이유

- CSS에 의해 비표시되는 요소 노드의 텍스트를 반환하지 않음
- CSS를 고려해야하므로 textContent보다 느림

## 39.6 DOM 조작

&nbsp;DOM 조작은 리플로우와 리페인트 과정에서 성능에 영향을 주므로 성능 최적화를 위해 주의해서 다뤄야한다.

### 39.6.1 innerHTML

- 이를 이용해 값을 할당할 경우, HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영(HTML 마크업이 파싱되지 않고 문자열로 인식되는 textContent와의 차이점)
- 크로스 사이트 스크립팅 공격에 취약

### 39.6.2 insertAdjacentHTML 메서드

- 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입하기 때문에 innerHTML보다 효율적이고 빠름
- 크로스 사이트 스크립팅 공격에 취약

### 39.6.3 노드 생성과 추가

#### 요소 노드 생성

`Document.prototype.createElement(tagName)`

#### 텍스트 노드 생성

`Document.prototype.createTextNode(text)`

#### 텍스트 노드를 요소 노드의 자식 노드로 추가

`Node.prototype.appendChild(childNode)`

### 39.6.4 복수의 노드 생성과 추가

&nbsp;`DocumentFragment` 노드를 생성하고 DOM에 추가할 요소 노드를 자식 요소로 추가, `DocumentFragment`노드를 기존 DOM에 추가하는 방식이 효율적

### 39.6.5 노드 삽입

#### 마지막 노드로 추가

`Node.prototype.appendChild`

#### 지정한 위치에 노드 삽입

`Node.prototype.insertBefore(newNode, childNode)`

### 39.6.6 노드 이동

`appendChild`, `insertBefore`

### 39.6.7 노드 복사

`Node.prototype.cloneNode([deep: true | false])`

### 39.6.8 노드 교체

`Node.prototype.replaceChild(newChild, oldChild)`

### 39.6.9 노드 삭제

`Node.prototype.removeChild(child)`

## 39.7 어트리뷰트

### 39.7.1 어트리뷰트 노드와 attributes 프로퍼티

`Element.prototype.attributes`

### 39.7.2 HTML 어트리뷰트 조작

`Element.prototype.getAttribute/setAttribute`

### 39.7.3 HTML 어트리뷰트 vs. DOM 프로퍼티

&nbsp;HTML 어트리뷰트의 역할은 HTML 요소의 초기 상태를 지정하는 것이다. 즉, HTML 어트리뷰트 값은 HTML 요소의 초기 상태를 의미하며 이는 변하지 않는다.

&nbsp;요소 노드의 초기 상태는 어트리뷰트 노드가 관리하며, 요소 노드의 최신 상태는 DOM 프로퍼티가 관리한다.

#### HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계

&nbsp;HTML 어트리뷰트와 DOM 프로퍼티가 언제나 1:1로 대응하는 것은 아니며, HTML 어트리뷰트 이름과 DOM 프로퍼티 키가 반드시 일치하는 것도 아니다.

#### DOM 프로퍼티 값의 타입

&nbsp;`getAttribute` 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다. 하지만 DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수 있다.

### 39.7.4 data 어트리뷰트와 dataset 프로퍼티

&nbsp;data 어트리뷰트의 값은 `HTMLElement.dataset` 프로퍼티로 취득할 수 있다.

## 39.8 스타일

### 39.8.1 인라인 스타일 조작

`HTMLElement.prototype.style`

### 39.8.2 클래스 조작

#### className

`Element.prototype.className`

#### classList

`Element.prototype.classList`, DOMTokenList 객체를 반환

- DOMTokenList가 제공하는 메서드
  - add
  - remove
  - item(index)
  - contains(className)
  - replace(oldClassName, newClassName)
  - toggle

### 39.8.3 요소에 적용되어 있는 CSS 스타일 참조

`window.getComputedStyle(element[, pseudo])`

## 39.9 DOM 표준

// 문서 참고
