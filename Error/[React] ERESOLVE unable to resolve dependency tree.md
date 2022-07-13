## [React] ERESOLVE unable to resolve dependency tree
리액트 프로젝트를 클론후 npm i 실행시 만난 에러다.

### 원인
오류 메세지는 캡쳐하지 못했지만 react-query와 react-dev-tools의 의존성 충돌 문제로 인해 생긴 에러였다.



### 해결 방법
2가지의 방법이 있는데 `npm install --force` 혹은 `npm install --legacy-peer-deps`를 하면 된다.    
현재 npm은 8버전인데 4-6버전과는 약간의 차이가 있다.   
> Automatically installing peer dependencies is an exciting new feature introduced in npm 7. In previous versions of npm (4-6), peer dependencies conflicts presented a warning that versions were not compatible, but would still install dependencies without an error. npm 7 will block installations if an upstream dependency conflict is present that cannot be automatically resolved. - in npm github blog     
> You have the option to retry with --force to bypass the conflict or --legacy-peer-deps command to ignore peer dependencies entirely (this behavior is similar to versions 4-6). - in npm github blog
- npm 4-6버전에서는 peer dependencies 가 있으면 경고만 뜨고 설치는 되었다. 7버전은 설치가 막힌다.
- `--force`로 충돌을 우회하거나 `--legacy-peer-deps`로 충돌을 무시하든지(npm 4-6버전과 비슷한 방식) 선택해야 한다.

#### 결론
- `--force`를 사용해 package-lock.json에 몇가지의 다른 의존 버전들을 추가한다.
- `--legacy`를 사용해 peerDependency가 맞지 않아도 일단 설치한다.
- 즉, force로 다른 의존 버전들을 추가해 실행해보고 안되면 legacy를 사용하는 것이 더 좋은 방법인듯 하다.
<br />

### 참고자료(Reference)
[stackoverflow](https://stackoverflow.com/questions/66020820/npm-when-to-use-force-and-legacy-peer-deps)    


