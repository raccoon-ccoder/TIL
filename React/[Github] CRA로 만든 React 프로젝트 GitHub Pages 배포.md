## [Github] CRA로 만든 React 프로젝트 GitHub Pages 배포
노마드 코더의 리액트로 영화 추천 웹앱 만들기 강의를 보며 정리한 글이다.

### 개요
- 참고로 해당 프로젝트는 create-react-app과 react-router가 적용되었다.
- react ver 17, react-router-dom ver 6

### gh-pages package 설치
먼저 프로젝트가 github repository와 연결되었는지 확인 후 gh-pages package를 설치한다.   
gh-pages란 결과물을 github pages에 업로드 할 수 있게 해주는 패키지다.
```js
git remote -v

// 아무것도 나오지 않는다면 다음과 같이 입력 
git remote add origin [github repo 주소]
// ex : git remote add origin https://github.com/raccoon-ccoder/react-movie

npm install --save gh-pages
```

### 프로젝트 설정 추가 
create-react-app으로 개발한 React 프로젝트는 기본적으로 루트(/)URL을 기준으로 프로젝트가 생성된다.     
하지만 우리가 업로드한 Github Pages는 URL에 repository 이름을 가지고 있기에 React 프로젝트에서 루트 URL이 아닌 repo 이름을 가진 URL을 사용하도록 수정해야 한다.    
설치가 완료되면 package.json 파일을 열어 프로덕션 빌드를 생성하고 Github Pages에 배포하기 위한 스크립트를 추가한다.
- package.json 최하단
  -   "homepage": "https://[Github 유저명].github.io/[프로젝트 저장소명]"
- scrpit 내부 추가
  - "predeploy": "npm run build"
  - "deploy": "gh-pages -d build"
<img width="623" alt="스크린샷 2022-03-15 오후 4 15 24" src="https://user-images.githubusercontent.com/77538818/158325841-b6884fe9-3fc0-4309-a5ff-4714929e7b68.png">   
    
>npm run build 실행시 (react-scripts build)
>- 웹사이트의 production ready code(압축되고 최적화된 코드)를 생성하며 해당 명령어 실행시 위 사진에서 알 수 있듯이 react-scripts build가 실행된다.    
>
>npm run deploy 실행시 ((predeploy)npm run build → (deploy)gh-pages -d build)   
>- predeploy가 우선적으로 자동 실행되며 따로 설정한 이유는 build하고 난 다음 deploy해야 한다는 것을 기억하고 싶지 않기에 번거로움을 줄이기 위해 설정하였다.    
>- 따라서 build(predeploy) 후 방금 설치한 gh-pages를 실행시켜 build라는 디렉토리를 가져가 gh-pages가 build 폴더를 homepage에 명시한 웹사이트에 업로드한다.    
 

그 후 BrowserRouter의 basename에 PUBLIC_URL을 설정한다. 여기서 설정한 PUBLIC_URL은 package.json에 설정한 URL이 적용된다.   
```js
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Home from "./routes/Home";
import Detail from "./routes/Detail";

function App() {
  return (
  <Router basename={process.env.PUBLIC_URL}>
    <Routes>  
      <Route path="/" element={<Home />} />
      <Route path="/movie/:id" element={<Detail />} />
    </Routes>
  </Router>
  );
}
```

### 프로젝트 배포
다음 명령어를 사용해 React 프로젝트를 배포한다.
```js
npm run deploy
```
<img width="695" alt="스크린샷 2022-03-15 오후 7 09 24" src="https://user-images.githubusercontent.com/77538818/158355144-5d42eedc-c3ab-4a75-8f4e-73d778ca7a58.png">
<img width="273" alt="스크린샷 2022-03-15 오후 7 08 52" src="https://user-images.githubusercontent.com/77538818/158355150-01bde7d3-4f75-4539-a4a2-5ac13dee9686.png">    

<br/>

명령어 실행시 위 사진처럼 build가 되고 gh-pages가 실행되어 Published 된 것을 볼 수 있다.    
몇 분 후 homepages에 기재한 웹사이트에 접속하면 정상적으로 작동하는 것을 볼 수 있다.   
[배포한 프로젝트](https://raccoon-ccoder.github.io/react-movie/)


