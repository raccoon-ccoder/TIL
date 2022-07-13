## [React] 'digital envelope routines' 에러
리액트 프로젝트를 클론하여 실행하던 중에 만난 에러다.

<br />

### 원인
Node.js 버전이 최신 버전이라 생긴 에러인 것 같다.

<br />

### 해결 방법
Node.js를 최신 버전이 아닌 안정적인 lts 버전으로 다운그레이드 한다.

#### 1. Node.js 버전 확인
```js
node -v
```
#### 2. Cache 삭제
```js
sudo npm cache clean --force
```
#### 3. n 플러그인 설치
n을 사용하면 Node.js의 버전을 쉽고 간편하게 관리할 수 있다.
```js
sudo npm install -g n
```
#### 4. lts 버전 설치
Permission denied가 뜬다면 맨 앞에 sudo를 붙여준다.     
n을 통해 다운로드 목록을 볼 수 있으며 엔터키로 선택해 원하는 버전을 설치할 수 있다.
```js
n lts

// 다운로드 목록 보기
n
```
![스크린샷 2022-07-13 오후 7 55 28](https://user-images.githubusercontent.com/77538818/178717699-6f535c73-2f95-460d-a267-683d4f449f54.png)    
#### 5. 특정 버전 제거
```js
// 캐시된 버전 제거
n rm [버전]

// 설치된 버전 외 캐시된 버전 모두 제거
n prune

// 설치된 Node.js 제거 (설치된 Node.js를 제거하며 캐시된 버전에는 영향을 미치지 않는다)
n uninstall
```

### 참고 자료(Reference)
[n package](https://www.npmjs.com/package/n)      
[관련 stackoverflow](https://stackoverflow.com/questions/69692842/error-message-error0308010cdigital-envelope-routinesunsupported)


