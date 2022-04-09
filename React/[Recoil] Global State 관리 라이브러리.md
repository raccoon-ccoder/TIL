## [Recoil] Global State 관리 라이브러리
react js에서 사용할 수 있는 상태 관리 라이브러리로 각각의 value를 나타내는 atom을 생성하고 원하는 component와 연결하여 사용한다.

### Local State vs Global State
#### local state
- React에서 local state는 `props`와 `state`로 나뉜다.
- props는 상속받은 상태에 대한 관리이고, state는 component가 생성한 상태에 대한 관리이다.

#### global state
- 애플리케이션이 `전체에서 공유되는 state`를 의미하며 예를 들면 다크 모드, 로그인 여부에 사용된다.
- component에서 다른 component로 state를 전달하기 위해선 반복적인 props 사용을 통해 전달하기에 `props drilling`현상이 발생한다.
- 따라서 전체적인 component에서 사용할 state가 있다면 별도의 `global state`로 관리하여 사용하는 것이 편리하다.

React애서 global state 관리 라이브러리 중 Recoil에 대해 알아본다.

## Recoil 사용
### 1. 설치
```js
npm install recoil   
```
### 2. index.tsx에서 RecoilRoot 생성
```js
import { RecoilRoot } from 'recoil';
... 생략

ReactDOM.render(
  <React.StrictMode>
    <RecoilRoot> // (*)
      <QueryClientProvider client={queryClient}>
          <App />
      </QueryClientProvider>
    </RecoilRoot>
  </React.StrictMode>,
  document.getElementById('root')
);
```
### 3. atom 생성
```js
import { atom } from "recoil";

export const isDarkAtom = atom({
    key: "isDarkMode",
    default: false,
});
```
- atom
  - 상태(`state`)의 일부를 나타내며 어떤 컴포넌트에서든 읽고 쓸 수 있다.
  - atom에 어떤 변화가 있다면 atom과 연결된 컴포넌트들은 리렌더링된다.
  - atom의 첫번째 인자는 `unique key`, 2번째 인자는 `default value`가 된다.
  
### 4. 원하는 컴포넌트와 value 연결
```js
const isDarkMode = useRecoilValue(isDarkAtom);  
```
- `useRecoilValue(state)`
  - 인자로는 위에 설정한 atom 변수명이 들어가며 주어진 `Recoil 상태 값`을 리턴한다.
  - 컴포넌트가 상태를 읽을 수만 있게 하고 싶을 때 추천하는 hook이다.

### 5. value 변경
```js
const isDarkMode = useRecoilValue(isDarkAtom);
const setDarkAtom = useSetRecoilState(isDarkAtom);
const toggleDarkMode = () => setDarkAtom((prev) => !prev);
```
- `useSetRecoilState(state)`
  - Recoil 상태의 값을 업데이트 하기 위한 `setter`함수를 리턴한다.
  - 인자로는 atom 혹은 selector를 전달한다.
  - 위에서는 setDarkAtom이라는 modifier 함수를 생성해 toggleDarkMode 함수 내에 사용해 다크 모드 여부를 변경하였다.



## 참고 자료(Reference)
[recoil doc](https://recoiljs.org/ko/docs/api-reference/core/useSetRecoilState/)
