## [TypeScript] TypeScript isNan only accepts a number
React + TypeScript 프로젝트에서 테스트 코드를 작성하던 중 만난 에러다.

### 원인
number인지 아닌지 체크하는 `isNaN`메서드는 타입스크립트에서 사용할 경우, `number`타입만 인자로 받기에 다른 타입의 변수를 인자로 전달하면 에러가 발생한다.

### 해결 방법
`Number()`메서드로 내부 인자를 number로 형변환 후 `isNaN`을 사용한다.
```jsx
Number("10") // 10
Number("abc") // NaN

isNaN(Number("abc")) // true
```
### 참고자료 (Reference)
[stackoverflow](https://stackoverflow.com/questions/42120046/typescript-isnan-only-accepts-a-number)
