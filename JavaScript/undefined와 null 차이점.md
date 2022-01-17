## undefined와 null 차이점
두가지 타입 모두 자바스크립트에서 `값이 비어있음`을 나타낸다는 공통점이 있지만 차이점도 존재한다.

### undefined
- 자바스크립트 환경 내에서 기본적으로 `값이 할당되지 않은 변수`는 `undefined` 타입이며, undefined 타입의 변수는 변수 자체의 값 또한 undefined이다.
- 컴퓨터 메모리 안에는 존재한다, 즉 공간은 있지만 값이 들어가지 않은 상태를 의미한다.
- `undefined`는 `데이터 타입`이자, `값`을 나타낸다.
```js
let something;
console.log(something); // undefined
```

### null
- 변수에 아무것도 없다는 것을 의미하며 variable에게 값이 주어졌지만 그 값은 `비어있음` 이라는 상태를 나타낸다.
- `null` 또한 undefined와 마찬가지로 `값`이며 `데이터 타입`이다.
- 분명한 차이점은 `undefined`는 변수를 `선언`만 하더라도 할당되지만 `null`은 변수를 선언한 후에 `null로 값을 바꾼다`는 점이다.
- 또한 `null` 타입 변수는 typeof 결과가 null이 아닌 `Object`이기에 null 타입 변수인지 확인할 경우 일치 연산자(`===`)를 사용해야 한다.
```js
const amIFat = null;  // null 타입 변수를 생성

console.log(typeof amIFat === null);    // false
console.log(amIFat === null);   // true
```

### 결론
> `null`은 값은 값이지만 값으로써 의미없는 특별한 값이 등록되어 있는 것   
> `undefined`는 등록되어 있지 않기에 초기화도 정의되지도 않은 것
