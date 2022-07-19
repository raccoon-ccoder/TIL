## 🦝 2022-07-19 (화) 
### 오늘 한 것
#### 1. jest + typescript에서 절대 경로 및 별칭 사용시 인식 안되는 문제 해결
현재 진행 프로젝트는 절대 경로 및 별칭 설정을 위한 react-app-wired, customize-cra 라이브러리를 사용하고 있다.     


<details>
<summary>해결 방법 보기</summary>
<div markdown="1">
  
  
**(1) package.json에 jest 관련 절대 경로 및 별칭 설정**
```json
"jest": {
    "moduleNameMapper": {
      "@test/(.*)": "<rootDir>/src/test/$1",
      "@/(.*)": "<rootDir>/src/$1"
    }
  }
```
**(2) 프로젝트 root에 jest.config.js 파일 생성 후 설정**
```js
module.exports = {
  "roots": [
    "<rootDir>/"
  ],
  "transform": {
    "^.+\\.tsx?$": "ts-jest"
  }
}
```

**(3) config-overrides.js, tsconfig.paths.json에도 똑같이 설정**
```js
// config-overrides.js
const { override, addWebpackAlias } = require("customize-cra");
const path = require("path");

module.exports = override(
  addWebpackAlias({
    "@": path.resolve(__dirname, "src"),
    "@test": path.resolve(__dirname, "src/test"),
   ... 중략
  })
);

// tsconfig.paths.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": [
        "src/*"
      ],
      "@test/*": [
        "src/test/*"
      ],
    ... 중략
    }
  }
}
```




</div>
</details>
  
  <br/>
  
#### 2. useQuery 사용하는 컴포넌트 테스트 진행시 QueryClientProvider가 존재하지 않다는 에러 해결
내가 직접 사용했던 해결 방법은 2가지인데 2가지 다 효율적인 방법이라고는 느껴지지 않는다.. 좀 더 찾아봐야겠다

<br />
  
**(1) hoc로 컴포넌트를 queryClientProvider로 감싸서 해결**
```js

```
  
**(2) 테스트 파일에서 직접 queryClientProvider 사용**
```js

```
