## [React Query] react-query란?
- react 애플리케이션에서 서버 state를 fetching, caching, synchronizing, updating 할 수 있도록 도와주는 라이브러리다.   
- 즉, 클라이언트 상태인 `state`를 직접 이용해 구현하기에는 번거롭고 복잡한 비동기 로직들을 전부 알아서 챙겨주는 라이브러리다.   
- 비동기 작업과 관련된 대부분의 귀찮은 작접들을 모두 간단한 코드 몇줄로 안전하게 처리할 수 있고, `캐싱` 덕에 성능 또한 우수하다.

## react-query 사용법 (useQuery 훅)
정확히 api 통신 로직 자체를 다루지는 않고 이들이 제공하는 정확한 기능은 비동기 함수와 그것의 리턴값에 대한 데이터 캐싱 등의 기능이고,
실제로 Promise를 리턴하는 axios.get('https://...') 등의 코드는 개발자가 직접 구현해야 한다.

<br/>

이들은 각각의 데이터에 `query`라는 이름을 붙인다. 그리고 각각의 query마다 unique한 `key` 값을 제공하기를 요구한다.   
이 key 값을 통해 해당하는 함수와 데이터를 식별하고, 그 데이터를 알아서 잘 처리해준다.   
이를 `queryKey`라고 한다.

</br>

또한 각각의 query에는 매치되는 비동기 함수 `queryFn`이 있다. 이 queryFn은 실제로 axios등을 이용해 비동기 로직을 수행하고, 결과를 리턴한다.

<br/>

그리고 이들 query 값, 상태 등은 `queryClient`에 저장되고, `QueryClientProvider`를 통해 제공되며 `useQuery`훅을 통헤 접근할 수 있다.   
물론 quetClient에 직접 접근할 수도 있다.

### Installing React Query
```js
npm install react-query
``` 
### Using React Query
```js
import { QueryClient, QueryClientProvider } from "react-query";
...

const queryClient = new QueryClient();
render(
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>,
  rootElement
);
``` 
- App.js에 Context Provider로 이하 컴포넌트를 감싸고 queryClient를 내려보내줌 → 이 context는 앱에서 비동기 요청을 알아서 처리하는 background 게층이 됨
```js
const { status, data, error, isFetching, isPreviousData } = useQuery(
  ['projects', page],
  () => fetchProjects(page),
  { keepPreviousData: true, staleTime: 5000 }
)

// 예외처리 -> reject쓰지말고 무조건 throw Error
const { error } = useQuery(['todos', todoId], async () => {
  if (somethingGoesWrong) {
    throw new Error('Oh no!')
  }

  return data
 })
``` 
- useQuery(query key, fetcher func)
  - `query key` - 쿼리의 unique한 key를 첫번째 인자로 넣으며 위 예시에는 배열을 넣었으며 주로 배열을 사용한다.
  - `fetcher func` - 꼭 promise를 반환해야 한다.
- useQuery 반환값
  - 객체, 요청의 상태를 나타내는 몇가지 프로퍼티가 있다
  - `isLoading`, isError, isSuccess, status
  - error, `data`, isFetching → 런타임간 무조건 요청이 한 번 이상 발생했다면 값이 존재한다.

즉, `useState`와 `fetch`함수 대신에 `useQuery`로 한방에 코드를 작성할 수 있다.   
또한 react-query는 반환된 데이터를 캐시에 저장하기에 만약 실시간 데이터가 필요하다면 `refetchInterval`를 이용해 밀리초 단위로 계속해 refetch 할 수 있다.   

### 예시 - onSuccess
```js
let { isLoading, data } = useQuery<ICoins[]>("allCoins", fetchCoins, {onSuccess : (data) => setCoinList(data)});
    const [coinName, setCoinName] = useState("");
    const [coinList, setCoinList] = useState<ICoins[]>();

    const onChange = (e: React.FormEvent<HTMLInputElement>) => {
        const { 
            currentTarget: {value},
        } = e;
        if(value !== "") {
            setCoinName(value);
            setCoinList(data?.filter(v => v.name.includes(coinName)));
        }else {
            setCoinName(value);
            setCoinList(data);
        }
    };
``` 
 - fetchCoins라는 fetcher 함수를 호출해 만약에 데이터 요청에 성공했다면 `onSuccess`를 이용해 데이터를 setState하는 코드다.
 - 사용자 검색에 따른 코인들만 보여줘야 하기에 따로 state를 생성해 coinList는 사용자가 원하는 코인 데이터만 넣도록 설정하였다.

### 예시 - refetchInterval
```js
const { isLoading: priceLoading, data: price } = useQuery<IPrice>(
        ["tickers",coinId], 
        () => fetchPriceInfo(coinId),
        {
            refetchInterval: 5000,
        }
    );
```
- 코인 상세페이지에선 실시간 코인 데이터를 가져오고 싶어서 `refetchInterval`함수를 사용했고 5초간 한번씩 refetch가 일어나도록 하였다.
- 코인의 종류가 많아서 query key는 배열로 하였고 isLoading, data는 다른 변수명을 사용하고 싶어서 위와 같이 설정하였다.
- fetcher 함수가 화살표 함수인 이유는 파라미터에 있는 코인 id를 가져와 함수에 넣어야 하는데 fetchPriceInfo(coinId)라고 사용하면 바로 실행되기에 화살표 함수로 리턴하도록 하였다.

## 개발자 도구로 데이터 확인하기
- useQuery는 데이터를 캐시로 저장하는데 이를 개발자 도구를 통해 시각적으로 확인할 수 있다.
```js
import { ReactQueryDevtools } from 'react-query/devtools'

function App() {
  return (
    <>
      <GlobalStyle />
      <Router />
      //개발자 도구 컴포넌트 추가
      <ReactQueryDevtools initialIsOpen={true} />
    </>
  );
}
```
![스크린샷 2022-04-08 오후 9 29 18](https://user-images.githubusercontent.com/77538818/162435945-f82a8c08-e7f9-4557-a6f5-6ec5041463b3.png)   
- 화면 하단에 개발자 도구 아이콘이 표시된다.
![스크린샷 2022-04-08 오후 9 29 32](https://user-images.githubusercontent.com/77538818/162435935-62fd0312-e9ee-4624-8b39-0f972d41e9a0.png)
- 개발자 도구를 열어보면 useQuery를 이용해 받아온 데이터가 캐시로 저장된 것을 확인할 수 있다.

## 참고 자료(Reference)
[react-query doc](https://react-query.tanstack.com/devtools)







