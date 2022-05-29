## [React-Helmet] styled-components 사용시 웹폰트 적용 문제점
리액트 프로젝트에서 styled-components를 사용하는데 전역 스타일 적용을 위한 createGlobalStyle에 웹폰트 적용시 겪은 문제와 해결 방법이다.

### 문제
```js
import { createGlobalStyle } from "styled-components";
import reset from "styled-reset";

const GlobalStyle = createGlobalStyle`
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700;900&display=swap');
    @import url('https://fonts.googleapis.com/icon?family=Material+Icons|Material+Icons+Outlined&display=swap');
    ${reset}
    ... 중략
`;

export default GlobalStyle;
```
어플리케이션 레벨 스타일상에서 공통 웹 폰트 적용을 위해 위와 같이 createGlobalStyle에 import문을 폰트를 적용했는데 아래와 같은 경고 메세지가 발생했다.

<br/>

<img width="812" alt="스크린샷 2022-05-30 오전 12 40 03" src="https://user-images.githubusercontent.com/77538818/170878077-ab9e9f0e-f256-48e8-9077-c29db223fab2.png">     

경고문을 보면 @import CSS 구문을 createGlobalStyle 상에서 사용하는 것을 권장하지 않는다는 내용이다.   
번들링 production 단계에서는 CSSOM 관련 APIs를 제대로 컨트롤할 수 없기 때문이다.

### 해결 방법
styled-components 문서에서는 이에 대한 [해결 방법](https://styled-components.com/docs/faqs#note-regarding-css-import-and-createglobalstyle)으로 index.html 파일 내부에 style 태그를 이용하는 것이 좋다고 추천한다.     
작업하는 프로젝트에선 react-helmet을 사용하고 있기에 이로 해결했다.    
참고로 react는 기본적으로 SPA를 서비스하기에 SEO 관리가 어렵다는 한계가 있다. 그래서 react-helmet을 이용해 각 컴포넌트의 헤더 부분에 메타 데이터를 적용할 수 있다.

```js
import { Helmet } from "react-helmet";

function MetaTag() {
  return (
    <Helmet>
     ... 중략
      <link
        href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700;900&display=swap"
        rel="stylesheet"
      />
      <link
        href="https://fonts.googleapis.com/icon?family=Material+Icons|Material+Icons+Outlined&display=swap"
        rel="stylesheet"
      />
    </Helmet>
  );
}

export default MetaTag;
```
Helmet 컴포넌트를 따로 분리해 해당 컴포넌트에서 import하여 사용했더니 폰트가 정상적으로 적용되었다.

## 참고 자료(Reference)
[styled-components 공식 문서](https://styled-components.com/docs/faqs)      
[react-helmet 공식 문서](https://github.com/nfl/react-helmet#readme)




