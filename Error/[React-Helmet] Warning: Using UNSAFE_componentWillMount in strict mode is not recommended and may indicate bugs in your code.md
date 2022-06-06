## [React-Helmet] Warning: Using UNSAFE_componentWillMount in strict mode is not recommended and may indicate bugs in your code
리액트 프로젝트에서 페이지별 메타태그 적용을 위해 react-helmet 라이브러리를 사용하던 중 만난 에러다.

![스크린샷 2022-06-06 오후 2 57 28](https://user-images.githubusercontent.com/77538818/172107317-1e3c117b-f5f5-4cbd-b897-2e0306a8495e.png)


### 원인
구글링 결과, 나와 비슷한 오류를 겪은 사람들이 많았으며 [nfl/react-helmet#548](https://github.com/nfl/react-helmet/issues/548), [nfl/react-helmet#623](https://github.com/nfl/react-helmet/issues/623)의 글에 따르면 원인은 
`strict mode`에서 `UNSAFE_componentWillMount`를 사용하는 것은 권장하지 않으며 종종 버그를 일으킬 수 있다는 내용이었다.

### 해결 방법
react-helmet의 상위 버전인 `react-helmet-async`라이브러리 사용시 애러 해결이 가능했다.   
[react-helmet-async 패키지 설명](https://www.npmjs.com/package/react-helmet-async)에 따르면는 Helmet과 사용법은 거의 비슷하고 스레드가 안전하지 않은 react-side-effect에 의존한다고 나와있다.    
사용법은 Helmet 태그 외부에 HelmetProvider로 감싸주면 된다. 

```js
import { Helmet, HelmetProvider } from "react-helmet-async";
 
const MetaTag = () => {
  return (
    <>
      <HelmetProvider>
        <Helmet>
          {/* contents... */}
        </Helmet>
      </HelmetProvider>
    </>
  )
}
 
export default MetaTag;
```




