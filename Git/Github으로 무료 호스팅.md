## Github으로 무료 호스팅.md

#### 1. 호스팅 할 프로젝트 repo의 settings > Pages
#### 2. GitHub Pages의 Source에서 None 클릭 > 원하는 branch 선택 후 Save
![스크린샷 2022-01-09 오전 2 23 30](https://user-images.githubusercontent.com/77538818/148694104-15411ae2-7339-4e71-bcc8-6beaab1d3986.png)
#### 3. 아래와 같이 사이트 생성 완료 → (https://{깃허브 아이디}.github.io/{repo 이름})
![스크린샷 2022-01-09 오전 2 24 20](https://user-images.githubusercontent.com/77538818/148694106-1650c9aa-371d-4a99-a8a1-f291c30b27a6.png)

***

참고로 해당 사이트가 보여주는 파일은 
  - 1순위. index.html   
  - 2순위. readme 파일이다.   

따라서 최상단 디렉토리에 `index`라는 이름의 메인 html 파일이 존재해야 한다. 또한 repo는 private이 아닌 `public`이어야 한다.

## React 프로젝트 gh-pages로 배포
배포할 repo에서 pages는 미리 설정한다.
#### 1. React 프로젝트에 gh-pages 설치
```js
npm install gh-pages --save-dev
```
#### 2. package.json 수정
```js
...
"scripts": {
    ... 
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
  },
  "homepage": "pages를 통해 발급받은 주소",
  ...
```
#### 3. gh-pages 배포 후 repo에서 다시 설정
아래의 명령어를 실행시킨 후 내 repo의 Pages에서 Source를 gh-pages로 설정한다.
```js
npm run deploy
```
