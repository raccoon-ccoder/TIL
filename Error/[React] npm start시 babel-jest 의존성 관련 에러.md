## [React] npm start시 babel-jest 의존성 관련 에러
리액트 프로젝트를 클론해 실행시키던 중에 만난 에러다.

### 원인
작업 폴더 외부에 node_modules 폴더로 인해 의존성 충돌로 인해 생긴 문제다.

### 해결 방법
외부에 있는 node_modules 폴더가 만들어져 있었는데 사용 중인 폴더가 아니라 삭제 후 프로젝트를 실행한다.     
콘솔창에 나온 방법(package.json 삭제)는 굳이 하지 않아도 해결된다.

### 참고 자료(Reference)
[에러 관련 stackoverflow](https://stackoverflow.com/questions/65910495/need-help-on-a-react-js-error-that-i-keep-on-getting?rq=1)
