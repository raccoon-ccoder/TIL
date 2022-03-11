## [React] Warning: Each child in a list should have a unique "key" prop
리액트 프로젝트에서 자바스크립트 map 함수를 사용한 경우 보게 된 주의 문구다.

```js
{["AAA", "BBB", "CCC"].map(item =>
      <li>{item}</li>
  )}
```

React는 `key prop`을 사용해 컴포넌트와 DOM 요소 간의 관계를 생성한다. 리액트 라이브러리는 이 관계를 이용해 컴포넌트 리렌더링 여부를 결정한다. 
불필요한 리렌더링 방지를 위해 각 자식 컴포넌트마다 `독립적인 key`값을 넣어줘야 한다.

### 해결 방법
배열로 map 함수를 사용해 JSX 리스트 구현시 key prop을 자식 컴포넌트마다 넣어줘야 한다.
```js
<li key="uniqueID">{item}</li>
```
하지만 자바스크립트의 배열은 정적이지 않다. 즉, 배열의 길이나 원소 등이 변할 수 있다는 의미기에 배열의 index를 key prop으로 사용하는 것은 지양해야 한다.    
배열 원소의 순서가 바뀌면 index도 바뀌고 컴포넌트마다 고유해야 하는 key값도 같이 바뀌기에 리액트는 리렌더링 해야하는 컴포넌트를 헷갈려 잘못된 컴포넌트를 리렌더링 할 위험이 있다.   

<br/>

key가 전역적으로 고유할 필요는 없고 `형제 요소`에서 고유해야 한다.
```js
// Bad Case
{["AAA", "BBB", "CCC"].map((item,index) =>
      <li key={index}>{item}</li>
  )}

// Good Case
{["AAA", "BBB", "CCC"].map(item =>
    <li key="{item}">{item}</li>
  )}
```
