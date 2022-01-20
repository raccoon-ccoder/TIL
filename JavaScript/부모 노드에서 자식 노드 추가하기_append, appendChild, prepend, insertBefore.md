## 부모 노드에서 자식 노드 추가하기_append, appendChild, prepend, insertBefore
모두 부모 노드에서 자식 노드를 추가하는 메서드이지만 약간의 차이가 있다.

### append
- `노드 객체`나 `DOMString`(text)를 사용할 수 있고 한번에 `여러 개의 자식 요소`를 설정할 수 있다.
- 추가하게 되면 부모 노드 내부 `마지막`에 자식 요소가 추가된다.

#### Node Object 사용 예시
```js
// Node object 삽입
const parent = document.createElement("div");
const child = document.createElement("p");

parent.append(child);

// 결과
// <div><p></p><div>
```

#### 문자열 사용 예시
```js
// DOMString 삽입
const parent = document.createElement("div");
parent.append("append 예시");

// 결과
// <div>append 예시<div>
```

#### 여러 개의 자식 요소를 설정하는 예시
```js
const div = document.createElement("div");
const span = document.createElement("span");
const p = document.createElement("p");

document.body.append(div, "hello", span, p);

// 결과
<body>
    <div></div>
    hello
    <span></span>
    <p></p>
</body>
```
#### append 메서드는 `return 값을 반환하지 않음`
```js
const div = document.createElement("div");
const span = document.createElement("span");
const p = document.createElement("p");

console.log(document.body.append(div, "hello", span, p)); // undefined
```

### appendChild()
- `Node 객체만` 받을 수 있으며 한번에 오직 `하나의 노드만` 추가할 수 있다.
- 추가하게 되면 부모 노드 내부 `마지막`에 자식 요소가 추가되며 반대의 개념으로는 `prepend()`가 있다.
```js
const parent = document.createElement("div");
const child = document.createElement("p");

parent.appendChild(child);

// 결과
// <div><p></p><div>
```

#### 문자열 사용 예시
```js
const parent = document.createElement("div");
parent.appendChild("append 예시");

// Uncaught TypeError: Failed to execute 'appendChild' on 'Node': parameter 1 is not of type 'Node'
```

#### appendChild()는 `return 값을 반환함`
```js
const div = document.createElement("div");
console.log(document.body.appendChild(div)); // div(Node Object)
```

### 참고
- 선택 요소의 자식 요소로 맨 앞, 맨 뒤가 아닌 중간에 추가하고 싶다면 `insertBefore`(추가할 요소, 이동할 자식 요소)를 사용하면 된다.

### 결론
||append|appendChild|
|:---:|:---:|:---:|
|노드 객체|O|O|
|문자열|O|X|
|반환값|X|O|
|다중 값 허용|O|X|

### 참고자료(Reference)
- https://developer.mozilla.org/ko/docs/Web/API/Node/appendChild
