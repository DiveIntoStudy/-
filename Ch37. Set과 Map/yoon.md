# CH37 Set과 Map

## 37.1 Set

&nbsp;중복되지 않는 유일한 값들의 집합

### 37.1.1 Set 객체의 생성

&nbsp;Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성

```javascript
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) // {1, 2, 3}
```

### 37.1.2 요소 개수 확인

&nbsp;Set.prototype.size

### 37.1.3 요소 추가

&nbsp;Set.prototype.add

### 37.1.4 요소 존재 여부 확인

&nbsp;Set.prototype.has

### 37.1.5 요소 삭제

&nbsp;Set.prototype.delete

### 37.1.6 요소 일괄 삭제

&nbsp;Set.prototype.clear

### 37.1.7 요소 순회

&nbsp;Set.prototype.forEach

```javascript
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
 1 1 Set(3) {1, 2, 3}
 2 2 Set(3) {1, 2, 3}
 3 3 Set(3) {1, 2, 3}
 */
```

### 37.1.8 집합 연산

#### 교집합

```javascript
Set.prototype.intersection = function (set) {
  return new Set([...this].filter((v) => set.has(v)));
};
```

#### 합집합

```javascript
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};
```

#### 차집합

```javascript
Set.prototype.difference = function (set) {
  return new Set([...this].filter((v) => !set.has(v)));
};
```

#### 부분 집합과 상위 집합

```javascript
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every((v) => supersetArr.includes(v));
};
```

## 37.2 Map

&nbsp;키와 값의 쌍으로 이루어진 컬렉션

### 37.2.1 Map 객체의 생성

&nbsp;Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다. 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

### 37.2.2 요소 개수의 확인

&nbsp;Map.prototype.size

### 37.2.3 요소 추가

&nbsp;Map.prototype.set

### 37.2.4 요소 취득

&nbsp;Map.prototype.get

### 37.2.5 요소 존재 여부 확인

&nbsp;Map.prototype.has

### 37.2.6 요소 삭제

&nbsp;Map.prototype.delete

### 37.2.7 요소 일괄 삭제

&nbsp;Map.prototype.clear

### 37.2.8 요소 순회

&nbsp;Map.prototype.forEach

```javascript
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.forEach((v, k, map) => console.log(v, k, map));
```
