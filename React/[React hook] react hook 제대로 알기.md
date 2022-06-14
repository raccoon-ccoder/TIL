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

### 해결 방법1. deps를 솔직하게
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

### 해결 방법2. 의존성 제거(콜백 사용하기)
effect 함수 자체를 수정해, 의존성 자체를 제거하는 방법도 있다.   
`setCount`함수에서 `count`상태를 사용하지 않고, 콜백으로 처리하면 이를 해결할 수 있다.   
실제로 카운트를 올리기 위해 count값 자체가 필요하기 보다는 `이전의 값 + 1`이 필요한 것이기에, 의존성을 제거할 수 있다.
```js
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount((prevCount) => prevCcount + 1);
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return <h1>{count}</h1>;
}
```
