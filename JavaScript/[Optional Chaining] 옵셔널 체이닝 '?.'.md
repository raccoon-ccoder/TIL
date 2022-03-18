## [Optional Chaining] 옵셔널 체이닝 '?.'
옵셔널 체이닝 사용시 프로퍼티가 없는 중첩 객체를 에러 없이 안전하게 접근할 수 있다.

### 옵셔널 체이닝이 필요한 이유
중첩 객체의 특정 프로퍼티에 접근하기 위해 거쳐야 할 구성요소들을 AND 연산자로 연결해 확인하면 코드가 아주 길어진다는 단점이 있다.   
이러한 단점을 극복하기 위해 등장한 것이 옵셔널 체이닝이다. 
```js
let user = {}; // 주소 정보가 없는 사용자
alert(user.address.street); // TypeError: Cannot read property 'street' of undefined
alert( user && user.address && user.address.street ); // undefined, 에러가 발생하지 않습니다.
```

### 옵셔널 체이닝이란
- `?.`은 `?.`'앞'의 평가 대상이 `undefined`나 `null`이면 평가를 멈추고 `undefined`를 반환한다.   
- 참고로 `?.`은 '앞'평가 대상에만 동작되고, 확장은 되지 않는다.    
- 또한 `?.`은 존재하지 않아도 괜찮은 대상에만 사용해야 한다. 
- 아래 예시를 보면 사용자 주소를 다루는데 일반적으로 user는 반드시 있어야 하고 address는 필수값이 아니기에 `user.address?.street`를 사용하는 것이 바람직하다.
```js
// user가 undefined인 경우
let user = {}; // 주소 정보가 없는 사용자
alert( user?.address?.street ); // undefined, 에러가 발생하지 않습니다.

// user가 null인 경우
let user = null;
alert( user?.address ); // undefined
alert( user?.address.street ); // undefined
```

### 옵셔널 체이닝 문법
3가지 형태로 사용할 수 있으며 다음과 같다.
  - `obj?.prop` – obj가 존재하면 obj.prop을 반환하고, 그렇지 않으면 undefined를 반환함
  - `obj?.[prop]` – obj가 존재하면 obj[prop]을 반환하고, 그렇지 않으면 undefined를 반환함
  - `obj?.method()` – obj가 존재하면 obj.method()를 호출하고, 그렇지 않으면 undefined를 반환함

참고로 옵셔널 체이닝은 선언이 완료된 변수를 대상으로만 동작하기에 ?.앞의 변수는 꼭 선언되어야 한다.
```js
// ReferenceError: user is not defined
user?.address;
```
