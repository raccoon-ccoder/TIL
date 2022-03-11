## [State] useState와 setState 동작원리
- setState 사용시 리렌더링되며 컴포넌트 함수 내부 코드들이 재실행되는데 어떻게 이전 state를 기억하는 것일까?     
- 그리고 이벤트 핸들러 내에서 여러 setState 함수를 호출해도 리렌더링이 1번만 일어날 수 있는 이유는 무엇일까?   

<br/> 

간단한 예시가 있다. 이 코드를 실행하고 div 태그를 2번 클릭하면 console.log가 어떻게 찍힐까?
```js
import { useState } from "react";

function App() {
  const [state, setState] = useState(0);
 
  const onClickHandler = () => {
    console.log("click!!");

    setState((prev) => prev + 1);

    if(state === 1) {
      console.log("실행될까?");
    }
  }

  const style = {
    width: "200px",
    height: "200px",
    backgroundColor: "tomato",
    textAlign: "center",
    lineHeight: "200px"
  };

  return (
    <div>
      <div style={style} onClick={onClickHandler}>Click me</div>
    </div>
  );
}
```
![ezgif com-gif-maker](https://user-images.githubusercontent.com/77538818/157892537-5e6bab14-0bb1-4701-b58c-ae018fee86ea.gif)  

<br/>

첫번째 클릭 후 콘솔창에 실행될까?가 출력될거라 생각할 수 있지만 아래 결과를 보면 그렇지 않다.   
두 번째 클릭에서 실행될까? 문구가 출력된다. 왜 그런 걸까?

<br/>

### useState의 동작원리는 어떻게 될까?
노마드코더의 강의를 보며 여지껏 setState가 state를 변경한다고 생각했는데 이건 오해였다...    
const[state, setState] = useState(initialState); 라고 사용하는데 state는 const기 때문에 값이 변할 수가 없기 때문에 말이 안되는 것이다!   

#### useState의 반환값은 어디에서 올까?
![스크린샷 2022-03-11 오후 11 07 24](https://user-images.githubusercontent.com/77538818/157883251-9da48490-b966-4119-937b-c89525d38e6c.png)
> import {useState} from 'react'    

우리는 보통 react라는 모듈에서 useState를 named import해서 사용하며 위 사진은 node_modules/react/cjs/react.development.js 내부에 각종 hooks 함수가 선언된 곳이다.    
useState는 dispatcher라는 인스턴스 생성 후 인자로 초기값을 받아 dispatcher.useState에 전달하여 반환값을 return한다. 즉

  - `dispatcher`의 메소드 useState에 initialState를 전달하면 배열을 반환하고
  - 그 안에 우리가 사용할 state와 setState가 있다는 의미다.

그러면 dispatcher를 반환하는 `resolveDispatcher`함수에 대해 알아보자.

<br/>

![스크린샷 2022-03-11 오후 11 12 39](https://user-images.githubusercontent.com/77538818/157884100-818f18a0-cd90-455b-a9f3-1f66d9047086.png)     
이 함수는 어디선가 `dispatcher`를 가져오고 에러처리를 한다.   
새로운 키워드로 `ReactCurrentDispatcher`가 나왔으니 이 키워드에 대해 탐색해보면 아래와 같다.

<br/>

![스크린샷 2022-03-11 오후 11 14 37](https://user-images.githubusercontent.com/77538818/157884422-dc3d3a2e-234e-4cba-8c5d-8e86f4dd8525.png)     
`ReactCurrentDispatcher`라는 객체는 전역에 선언된 키워드로 속성으로 current를 가지고 있다. 이 current가 우리가 찾던 dispatcher가 담길 곳이다.   
언제 dispatcher가 담기는지 알아보면 너무 길어지니 생략하고 넘어간다.

<br/>

위 과정을 요약해보면,
  - useState를 포함한 hooks는 react 모듈에 선언되있는 함수이고,
  - __실행 될 때마다 dispatcher를 선언하고 useState 메소드를 실행해서 그 값을 반환한다.__
  - 할당부를 거슬러 올라가면 dispatcher는 전역 변수 ReactCurrentDispatcher로부터 가져온다.
계속 위로 거슬러 올라가 상위에 있는 값에 접근하는 것, `Closure`가 이 개념이다.

***

### setState 함수는 어떻게 상태를 변화시킬까?
처음 예시 코드와 간단하게 작성한 react 모듈 코드를 보자.   
```js
import { useState } from "react";

function App() {
  const [state, setState] = useState(0);
 
  const onClickHandler = () => {
    console.log("click!!");

    setState((prev) => prev + 1);

    if(state === 1) {
      console.log("실행될까?");
    }
  }

  const style = {
    width: "200px",
    height: "200px",
    backgroundColor: "tomato",
    textAlign: "center",
    lineHeight: "200px"
  };

  return (
    <div>
      <div style={style} onClick={onClickHandler}>Click me</div>
    </div>
  );
}
```
```js
let _currentValue

export useState(initialValue) {

  if(_vaule === undefined) {
    _vaule = initialValue
  }

  const setState = (newValue) => {
    _vaule = newValue
  }

  return [_vaule, setState]
}
```
App 컴포넌트 함수는 실행되면 먼저 useState를 호출해 반환값을 비구조화 할당으로 추출해 변수에 저장한다.   
여기서 중요한건, App도 `함수`라는 것이다. jsx를 반환하는 함수일 뿐이다. `렌더링`이 시작되면 이 함수가 호출되어 새로운 jsx를 반환한다.

우측의 react 모듈 코드를 보면, useState 밖에 전역으로 선언된 `_value`가 있다. 우리가 useState를 통해 관리하는 '상태'는 바로 이 친구다.  
`setState`는 App 함수에 선언된 state가 아니라, 자신이 선언된 위치에서 접근할 수 있는 `_value`를 변경한다. 이는 closure의 개념을 알면 바로 이해할 수 있다.

### 웹이 로딩되고 최초로 App 함수 호출
- App은 인수로 0을 전달하며 useState를 호출
- useState는 실행될 때 마다 초기값을 전달받지만, 내부적으로 `_value`(value)값이 undefined인지 확인해 __최초의 호출에만 초기값을 value에 할당하고, 이후 초기값은 사용되지 않는다.__
- 이후 value와 그 값을 재할당하는 setState 함수를 배열에 담아 반환
### setState 호출
- 전달 받은 1은 react 모듈 상단의 value에 할당
- 이 코드에는 나와 있지 않지만, 컴포넌트 리렌더링을 trigger한다.
### setState가 실행되어 리렌더링 발생 (App 함수 2번째 실행)
- 다시 인수 `0`을 `useState`에 전달하며 호출
- useState는 내부적으로 value값을 확인해, undefined가 아닌 값이 할당되어 있기에 초기값 할당문을 실행하지 않는다.
- 이후 useState가 현재 시점의 value와 setState를 반환 (이 시점에서 value는 `1`)
- 두 번째 실행된 App 함수 내부에서 useState가 반환한 값을 비구조화 할당으로 추출해 변수에 할당    

즉, setState 함수는 `__자신과 함께 반환된 변수를 변경시키는 게 아니라(const)__`, 다음 useState가 반환할 react 모듈의 value를 변경시키고,
컴포넌트를 리렌더링 시키는 역할을 한다. `__변경된 값은 useState가 가져온다.__`

***

### 여러 setState 함수를 호출해도 리렌더링이 1번만 일어날 수 있는 이유?
위 예제에서 코드를 몇줄 추가하였다. div 클릭시 되면 두개의 state가 변경되니 렌더링도 2번 일어나게 되어 rendered라는 문구가 콘솔창에 2번씩 출력되지 않을까?
```js
function App() {
  const [state, setState] = useState(0);
  const [count, setCount] = useState(0);
  const onClickHandler = () => {
    console.log("click!!");

    setState((prev) => prev + 1);
    setCount((prev) => prev + 1);
  }

  console.log("rendered");
  
  const style = {
    width: "200px",
    height: "200px",
    backgroundColor: "tomato",
    textAlign: "center",
    lineHeight: "200px"
  };

  return (
    <div>
      <div style={style} onClick={onClickHandler}>Click me</div>
    </div>
  );
}
```
![ezgif com-gif-maker (1)](https://user-images.githubusercontent.com/77538818/157896148-48876ef3-07e1-46cd-bb66-ccac5dc47d14.gif)  

<br/>

결과는 div 클릭시 rendered는 1번만 출력된다. 즉, 리렌더링이 2번이 아닌 1번씩만 일어난다는 것이다.

### React batch update
리액트 공식 문서에 따르면 이런 불필요한 렌더링을 해결하기 위해 `React batch updating`을 지원한다.   
state를 각각 업데이트 하는 대신 batch update(일괄 업데이트)를 해 컴포넌트의 __렌더링 횟수를 최소화 할 수 있다.__    
> React batch update : 리액트 성능을 위해 setState를 단일 업데이트 (batch update)로 한번에 처리하는 것      
> React 18 이전과 18에는 약간의 차이가 있다.    
 
참고로 React 18 이전까지는, React 이벤트 핸들러 내부에서 발생하는 업데이트만 일괄 배치하고 React 18의 createRoot를 통해 timeout, promise, native 이벤트 핸들러와 모든 여타 이벤트는 React에서 제공하는 이벤트와 동일하게 state 업데이트를 배칭할 수 있다.     
아래의 간단한 예시로 18 이전과 18 버전의 일괄 배치 차이를 보여준다.

```js
function App() {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);

  function handleClick() {
    fetchSomething().then(() => {
      // React 17과 이전 버전에서는 이 업데이트들이
      // 이벤트가 *진행되는 중*이 아닌, *완료된 후의* 콜백에서 실행되기 때문에
      // 배칭되지 않았다.
      setCount((c) => c + 1); // 리렌더링을 발생시킨다. (1번째 리렌더링)
      setFlag((f) => !f); // 리렌더링을 발생시킨다.     (2번째 리렌더링)
    });
  }

  return (
    <div>
      <button onClick={handleClick}>Next</button>
      <h1 style=>{count}</h1>
    </div>
  );
}

function App() {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);

  function handleClick() {
    fetchSomething().then(() => {
      // React 18과 이후 버전에서는 아래 항목들을 배칭한다.
      setCount((c) => c + 1);
      setFlag((f) => !f);
      // React는 이 콜백이 끝났을 때만 리렌더링을 하게 된다 (이제 여기도 배칭이 들어간다!)
    });
  }

  return (
    <div>
      <button onClick={handleClick}>Next</button>
      <h1 style=>{count}</h1>
    </div>
  );
}
```





## 결론
- 위 예시는 react로직을 아주 단순히 표현했을 뿐, 실제와 많이 다르다. useState를 여러번 사용해도 각기 다른 상태 값과 갱신 함수를 사용할 수 있는건 단순히 value가 아니라 여러 값을 저장하고 있기 때문이다.
- setState가 state를 변경시키는게 아니기 때문에, __setState 호출 이후 로직에서도 state의 값은 이전과 동일하다.__
- __변경된 값은 다음 컴포넌트 함수가 실행될 때 useState가 가져온다.__
- React 이벤트 핸들러에서는 setState를 일괄적으로 처리해 렌더링 횟수를 최소화한다.

## 참고 자료 (Reference)
[컴포넌트 State](https://ko.reactjs.org/docs/faq-state.html)   
[State와 생명주기](https://ko.reactjs.org/docs/state-and-lifecycle.html)    
[React setState 순서](https://stackoverflow.com/questions/48563650/does-react-keep-the-order-for-state-updates/48610973#48610973)     
[useState 동작 원리](https://velog.io/@jjunyjjuny/React-useState%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%A0%EA%B9%8C)    
[React batch update](https://immigration9.github.io/react/2021/06/12/automatic-batching-react.html)






