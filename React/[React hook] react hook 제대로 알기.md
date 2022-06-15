## [React hook] react hook 제대로 알기
react hook에 대해 제대로 알지 못했던 부분들에 대해서 [본글](https://overreacted.io/ko/a-complete-guide-to-useeffect/)을 읽고 정리한 글이다.


### deps에 거짓말하지 말라
아래의 코드는 언뜻 보면 정상적인 코드 같지만 한가지 문제가 발생한다.    
`count`변수는 0 -> 1로 한번 증가한 후 더이상 증가하지 않는다.
```js
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1);
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return <h1>{count}</h1>;
}
```
- 첫 번째 렌더링에서 `count`는 `0`이다. 따라서 첫 번째 렌더링의 이팩트에서 `setCount(count + 1)`는 `setCount(0 + 1)`이라는 뜻이다.
- deps를 `[]`라고 정의했기 때문에 이팩트를 절대 다시 실행하지 않고, 결국 그로 인해 매 초마다 `setCount(0 + 1)`를 호출한다.
- 즉, 클로저로 인해 count는 처음에 렌더링 당시의 값 `0`을 보고 있기 때문에 count가 계속 증가하지 않는다.

### 해결 방법 1. deps를 솔직하게
아래의 코드는 count가 변할 때마다 effect 함수가 갱신되기에, 매 초마다 카운트가 상승한다.    
하지만 effect의 특성상, 리렌더링시 clean-up 코드가 실행되고 effect가 실행되기에, interval의 설정과 해제를 매 초마다 반복한다.
```js
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1);
    }, 1000);
    return () => clearInterval(id);
  }, [count]);
  return <h1>{count}</h1>;
}
```

### 해결 방법 2. 의존성 제거(콜백 사용하기)
effect 함수 자체를 수정해, 의존성 자체를 제거하는 방법도 있다.   
`setCount`함수에서 `count`상태를 사용하지 않고, 콜백으로 처리하면 이를 해결할 수 있다.   
실제로 카운트를 올리기 위해 count값 자체가 필요하기 보다는 `이전의 값 + 1`이 필요한 것이기에, 의존성을 제거할 수 있다.
```js
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount((prevCount) => prevCount + 1);
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return <h1>{count}</h1>;
}
```

<br />

***

### 액션을 업데이트로부터 분리하기 (useReducer)
```js
function Counter() {
  const [count, setCount] = useState(0);
  const [step, setStep] = useState(1);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(c => c + step);
    }, 1000);
    return () => clearInterval(id);
  }, [step]);
  return (
    <>
      <h1>{count}</h1>
      <input value={step} onChange={e => setStep(Number(e.target.value))} />
    </>
  );
}
```
위의 코드는 정상적으로 동작한다. 인터벌은 step 입력값에 따라 count를 더하고 step이 변경되면 인터벌을 다시 시작한다.   
만약 step이 바뀌어도 인터벌 시계가 초기화되지 않기를 원하거나 이팩트 의존성 배열에서 step을 제거하려면 어떻게 해야 할까?


<br />

```js
function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const { count, step } = state;

  useEffect(() => {
    const id = setInterval(() => {
      dispatch({ type: 'tick' }); // setCount(c => c + step) 대신에
    }, 1000);
    return () => clearInterval(id);
  }, [dispatch]);
}

const initialState = {
  count: 0,
  step: 1,
};

function reducer(state, action) {
  const { count, step } = state;
  if (action.type === 'tick') {
    return { count: count + step, step };
  } else if (action.type === 'step') {
    return { count, step: action.step };
  } else {
    throw new Error();
  }
}
```
- 위처럼 어떤 상태 변수가 다른 상태 변수의 현재 값에 연관되도록 설정하려고 한다면, 두 상태 변수 모두 `useReducer`로 해결하면 된다.   
- 리액트는 컴포넌트가 유지되는 한 dispatch함수가 항상 같다는 것을 보장한다. 따라서 위 예제에서 인터벌을 다시 구독할 필요가 없게 된다.


<br />

***

### Effect 훅 내에서 함수 호출
- 아래의 경우 useEffect훅의 deps를 빈 배열로 설정해서, 컴포넌트가 mount된 이후 수행되지 않는다.     
- 하지만 예를 들어, 사용자의 id, 닉네임에 따라 변경되어야 할 수도 있다. 이 경우에는 useEffect에 deps를 추가해야 하는데, 여러 함수를 호출하면 deps에 실수할 가능성이 있다.
```js
function SearchResults() {
  const [query, setQuery] = useState('react');
  const [data, setData] = useState({ hits: [] });
  
  // 이 함수가 길다고 상상해 봅시다
  function getFetchUrl() {
    return 'https://hn.algolia.com/api/v1/search?query=' + query;
  }

  // 이 함수가 길다고 상상해 봅시다
  async function fetchData() {
    const result = await axios(getFetchUrl());
    setData(result.data);
  }

  useEffect(() => {
    fetchData();
  }, []);

  // ...
}
```

<br />

- **어떠한 함수를 이펙트 안에서만 사용한다면, 그 함수를 직접 이펙트 안으로 옮겨서 문제를 해결할 수 있다.**
- 위 코드와 달리 effect 훅 내부에서 사용되고 있는 함수들을 이행적으로 찾아가며, deps를 추가할 필요가 없어진다.
```js
function SearchResults() {
  // ...
  useEffect(() => {
    // 아까의 함수들을 안으로 옮겼어요!
    function getFetchUrl() {
      return 'https://hn.algolia.com/api/v1/search?query=react';
    }
    async function fetchData() {
      const result = await axios(getFetchUrl());
      setData(result.data);
    }
    fetchData();
  }, []); // ✅ Deps는 OK
  // ...
}
```

<br/>

- 나중에 `getFetchUrl`을 수정하고 query props를 사용해야 한다면, 이팩트 내부의 함수만 수정하고 deps에 query만 추가하면 되기에 쉽게 해결할 수 있다.
```js
function SearchResults(query) {

  useEffect(() => {
    function getFetchUrl() {
      return 'https://hn.algolia.com/api/v1/search?query=' + query;
    }

    async function fetchData() {
      const result = await axios(getFetchUrl());
      setData(result.data);
    }

    fetchData();
  }, [query]); // ✅ Deps는 OK
  // ...
}
```

<br />

### Effect 내부로 함수를 옮기지 못하는 상황
- 함수가 여러 곳에 사용되는데, 이를 내부로 옮기게 된다면 중복된 코드가 많아지기 때문에 effect 훅 내부로 함수를 옮기는 것이 좋은 방법은 아니다.
- 아래 코드의 경우 `getFetchUrl`함수를 사용함에도 의존성 배열에 추가하지 않았다. 물론 실제 동작에는 문제가 없다.
- 하지만 `getFetchUrl`함수의 경우, 매 렌더링마다 새로 함수가 만들어지는데 함수 내용이 변하지도 않고 effect 훅 내에서도 사용되는 함수 인자도 상수이기에 effect의 의존성에 거짓말을 하고 있는 것이다.
```js
function SearchResults() {
  function getFetchUrl(query) {
    return 'https://hn.algolia.com/api/v1/search?query=' + query;
  }

  useEffect(() => {
    const url = getFetchUrl('react');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, []); // 🔴 빠진 dep: getFetchUrl

  useEffect(() => {
    const url = getFetchUrl('redux');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, []); // 🔴 빠진 dep: getFetchUrl

  // ...
}
```

<br/>

- 아래의 경우, `getFetchUrl`함수에 쿼리스트링으로 넘어온 userId를 추가한다면, effect 훅은 userId props가 변경되어도 동기화하는데 실패하게 된다.
```js
function SearchResults({ userId}) {
  function getFetchUrl(query) {
    return `https://hn.algolia.com/api/v1/search?query=${query}?id=${userId}`;
  }

  useEffect(() => {
    const url = getFetchUrl('react');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, []); 

  useEffect(() => {
    const url = getFetchUrl('redux');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, []);
  // ...
}
```

<br/>

- 그러면 만약에 반대로 의존성 배열에 getFetchUrl함수를 추가하면 어떨까?
- 아래의 코드는 매 렌더링마다 해당 함수를 생성하기에 deps 설정은 올바르게 하였지만 state나 props 변화에 따라 항상 effect 훅을 호출하게 된다.
```js
function SearchResults() {
  // 🔴 매번 랜더링마다 모든 이펙트를 다시 실행한다
  function getFetchUrl(query) {
    return 'https://hn.algolia.com/api/v1/search?query=' + query;
  }
  useEffect(() => {
    const url = getFetchUrl('react');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, [getFetchUrl]); // 🚧 Deps는 맞지만 너무 자주 바뀐다

  useEffect(() => {
    const url = getFetchUrl('redux');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, [getFetchUrl]); // 🚧 Deps는 맞지만 너무 자주 바뀐다

  // ...
}
```

<br />

### 해결 방법 1. 함수를 컴포넌트와 분리하기 
- 함수가 컴포넌트 스코프 안의 어떠한 것도 사용하지 않는다면, `컴포넌트 외부로 분리`하여 이펙트 안에서 자유롭게 사용할 수 있다.
- 하지만 props나 state의 변경이 있을 경우 deps는 빈 배열이기 떄문에 데이터의 변화가 일어나도 동기화되지 않는다.
```js
// ✅ 데이터 흐름에 영향을 받지 않는다
function getFetchUrl(query) {
  return 'https://hn.algolia.com/api/v1/search?query=' + query;
}
function SearchResults() {
  useEffect(() => {
    const url = getFetchUrl('react');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, []); // ✅ Deps는 OK

  useEffect(() => {
    const url = getFetchUrl('redux');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, []); // ✅ Deps는 OK

  // ...
}
```

<br />

### 해결 방법 2. useCallback 사용하기
- 아래와 같이 사용되는 함수를 `useCallback`함수를 통해 정의할 수 있다.
- 이렇게 되면, 매 함수 렌더링 마다 getFetchUrl을 다시 생성하지 않게 되고, effect에 getFetchUrl을 의존성 배열에 추가해도 매 렌더링마다 실행되지 않을 것이다.
```js
function SearchResults() {
    const getFetchUrl = useCallback((query) => {
        return 'https://hn.algolia.com/api/v1/search?query=' + query;
    }, []);

  useEffect(() => {
    const url = getFetchUrl('react');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, [getFetchUrl]); 

  useEffect(() => {
    const url = getFetchUrl('redux');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, [getFetchUrl]); 

  // ...
}
```

<br />

- 만약 state, props를 `getFetchUrl`에서 사용한다면 이를 알아차리는 것도 쉽고, effect도 그에 따라 동기화 될 것이다.
- 아래의 경우, 쿼리스트링에 id가 추가되었고, props의 id를 추가한 후 의존성 배열에 추가한 코드다.
```js
function SearchResults({userId}) {
    const getFetchUrl = useCallback((query) => {
        return `https://hn.algolia.com/api/v1/search?query=${query}?id=${userId}`;
    }, [userId]);

  useEffect(() => {
    const url = getFetchUrl('react');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, [getFetchUrl]); 

  useEffect(() => {
    const url = getFetchUrl('redux');
    // ... 데이터를 불러와서 무언가를 한다 ...
  }, [getFetchUrl]); 

  // ...
}
```
