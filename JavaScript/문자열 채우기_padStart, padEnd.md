## 문자열 채우기_padStart(), padEnd()
현재 문자열의 시작 혹은 끝을 다른 문자열로 채워, 주어진 길이를 만족하는 새로운 문자열을 반환하는 메소드이다.

### str.padStart(targetLength [, padString])
- 현재 `문자열의 시작`을 다른 문자열로 채워, 주어진 길이를 만족하는 새로운 문자열을 만들어준다.
- `targetLength`으로 목표 문자열 길이를 설정하며 현재 문자열의 길이보다 작다면 채워넣지 않고 그대로 반환한다.
- `padString`(Optional) 으로 현재 문자열에 채워넣을 다른 문자열을 설정하며, 문자열이 너무 길어 목표 문자열 길이를 초과한다면 좌측 일부를 잘라서 넣는다. (기본값은 "")
```js
const seconds = String(new Date().getSeconds()).padStart(2,"0");
console.log(seconds); // 01,02.. 이런식으로 현재 시간(초)를 출력
```
- Date 객체를 생성 후 현재 몇 초인지를 추출하여 String으로 형변환 후 만약 `0~9`초 사이라면 `00, 01, 02` 이런식으로 길이가 2가 되게끔 앞에 0을 붙여 나타내었다.

### str.padEnd(targetLength [, padString])
- 현재 `문자열 마지막`에 다른 문자열을 채워, 주어진 길이는 만족하는 새로운 문자열을 만들어준다.
- `targetLength`으로 목표 문자열 길이를 설정하며 현재 문자열의 길이보다 작다면 채워넣지 않고 그대로 반환한다.
- `padString`(Optional) 으로 현재 문자열에 채워넣을 다른 문자열을 설정하며, 문자열이 너무 길어 목표 문자열 길이를 초과한다면 좌측 일부를 잘라서 넣는다. (기본값은 "")
```js
const str1 = 'Breaded Mushrooms';

console.log(str1.padEnd(25, '.'));
// expected output: "Breaded Mushrooms........"

const str2 = '200';

console.log(str2.padEnd(5));
// expected output: "200  "
```

### 브라우저 호환성
- 21.01.20 일자 기준으로 IE에서는 지원하지 않는다. [참고링크](https://caniuse.com/?search=padStart)

### 참고 자료(Reference)
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd
