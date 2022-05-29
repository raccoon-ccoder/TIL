## [React-App-Rewired] React + TypeScript에서 절대 경로 설정하기
리액트 + 타입스크립트 기반의 프로젝트를 진행하는데 패키지의 구조가 깊어질 수록 파일 간의 관계 파악에 어려워 절대경로를 사용하게 되었다.    
절대 경로를 사용함으로써 간결해보이고 의미가 명시되어 보이며, 프로젝트 관리가 더 쉬워진다는 장점이 있다.   

<br/>

지정된 절대 경로로 import하는 방법은 크게 3가지가 있다.
  - Babel plugin
  - Webpack, Fusebox...(module bundlers)
  - create-react-app, create-react-app-ts
현재 CRA를 통해 프로젝트를 생성했기 때문에 바벨과 웹팩을 직접 건드는 방식이 아닌 `CRA(create-react-app)`방식에서 절대 경로를 설정했다.     
물론 `eject`로 웹팩에 alias 설정을 통해 절대 경로를 설정할 수 있지만 절대 경로 하나 때문에 CRA가 주는 이점을 포기하게 되기 때문에 다른 방법을 통해 설정했다.   

### eject를 사용하면 안되는 이유?
- eject 사용시 숨겨져 있던 모든 설정들(웹팩, 바벨 등..)과 패키지들이 가지는 의존성을 볼 수 있지만 한번 실행하면 이전 상태로 되돌릴 수 없다.    
- CRA의 모든 configuration을 직접 유지 보수해야 하기에 CRA가 주는 편리함을 잃게 된다.
- One Build Dependency의 장점을 잃게 되어 작업 도중 하나의 패키지를 설치 혹은 삭제시, 항상 다른 패키지들의 의존성을 신경써야 한다.   

그럼에도 불구하고 eject를 하고 싶다면 몇가지 고려사항이 있다.
  1. 얼마나 많은 설정을 기존 CRA 설정에 추가하거나 뺄 것인지?
  2. 추가하고 싶은 설정의 중요성이 eject를 하기 이전에 가지는 장점보다 큰지?

### React-App-Rewired, Customize-CRA 소개
반드시 eject하지 않아도 기존 CRA 프로젝트에 약간 커스터마이징을 할 수 있는 방법이 있다.(물론 eject 하는 것보단 자유롭지 않다)   
`customize-cra`,`react-app-rewired`가 위와 같은 역할의 라이브러리이다. `craco`라는 패키지도 있지만 CRA4까지만 지원하기에 아래와 같은 라이브러리를 선택했다.

### React-App-Rewired, Customize-CRA 사용
```js
npm i -D customize-cra react-app-rewired
```
위의 패키지를 설정했다면 `package.json`의 스크립트 파일을 수정한다.
```js
scripts: {
  ...,
  "start": "react-app-rewired start",
  "build": "react-app-rewired build",
  "test": "react-app-rewired test",
  "eject": "react-scripts eject",
  ...,
}
```
이제 웹팹을 오버라이딩하기 위해 프로젝트 root 경로에 `config-overrides.js`를 생성하고 아래와 같이 작성한다.   
프로젝트에 절대 경로를 적용해 `src`폴더를 `@`키워드로 접근할 수 있도록 설정했으며 `addWebpackAlias`는 `customize-cra`에서 경로를 설정할 수 있도록 제공하는 오버라이딩 메서드다.   
꼭 아래에 있는 코드 같은 별칭을 사용하지 않아도 괜찮다. 
```js
const { override, addWebpackAlias } = require("customize-cra");
const path = require("path");

module.exports = override(
  addWebpackAlias({
    "@": path.resolve(__dirname, "src"),
    "@api": path.resolve(__dirname, "src/api"),
    "@components": path.resolve(__dirname, "src/components"),
  })
);
```
현재 타입스크립트를 사용하고 있기에 타입스크립트에서 이해할 수 있도록 프로젝트 root 경로에 `tsconfig.paths.json`파일을 생성하여 아래와 같이 입력한다.
```js
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@api/*": ["src/api/*"],
      "@components/*": ["src/components/*"],
    }
  }
}
```
마지막 설정으로 tsconfig.paths.json에서 설정한 내용을 이해할 수 있도록 `tsconfig.json`에서 추가 설정을 아래와 같이 한다.
```js
{
  ... 중략
  "include": ["src"],
  "extends": "./tsconfig.paths.json", -> 추가
}
```
설정을 완료했다면 에디터를 껐다 킨 후 `index.tsx`파일을 다음과 같이 수정하고 프로젝트를 실행해 정상적으로 동작하는지 확인한다.
```js
import React from "react";
import ReactDOM from "react-dom";
import { ThemeProvider } from "styled-components";
import App from "@/App"; // 위에서 설정한 경로로 수정

ReactDOM.render(
  <React.StrictMode>
      <ThemeProvider theme={theme}>
        <App />
      </ThemeProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

## 참고 자료(Reference)
[react-app-rewired 홈페이지](https://github.com/timarney/react-app-rewired)     
[customize-cra 홈페이지](https://www.npmjs.com/package/customize-cra)
[타입스크립트 절대경로 관련 블로그 자료](https://velog.io/@hoo00nn/React-TypeScript-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C-%EC%A0%88%EB%8C%80%EA%B2%BD%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
