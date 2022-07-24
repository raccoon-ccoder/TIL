## [React] Error: Not implemented: HTMLCanvasElement.prototype.getContext (without installing the canvas npm package)
jest로 컴포넌트 테스트시 만났던 에러다.

### 해결 방법
#### (1) jest-canvas-mock 패키지 설치
```
npm i --save-dev jest-canvas-mock
```
#### (2) package.json에 설정 추가
```json
{
  "jest": {
    "setupFiles": [
      "jest-canvas-mock"
    ],
  }
}
```

## 참고자료 (Reference)
[stackoverflow](https://stackoverflow.com/questions/48828759/unit-test-raises-error-because-of-getcontext-is-not-implemented)
