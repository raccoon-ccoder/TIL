## 자바스크립트 선언 방식_var, let, const

JavaScript에서 변수 선언 방식인 `var`, `let` ,`const`의 차이점 정리

### 1. var
- `재선언 & 재할당이 가능`하다는 점이 있지만 언어로부터 보호받지 못하기에 실수로 재선언 혹은 재할당을 하여도, 오류가 발생했다는 걸 알려주지 않는다. (const 변수 재할당시 오류 발생)
- 이런 유연한 변수 선언으로 간단한 테스트에는 편리할 수 있지만, 코드량이 많아진다면 어디에서 어떻게 사용 될지도 파악하기 힘들기에 ES6 이후, 이를 보완하기 위해 추가된 변수 선언 방식이 `let`과 `const`이다.
- let, const 를 사용함으로써 `목적`에 따라 `언어를 보호`할 수 있고 변수 선언 방식만 봐도 `코드의 의미`를 빠르게 찾을 수 있다.
```js
var name = "Raccoon";
console.log(name); // Raccoon

var name = "Cat";
console.log(name);  // Cat
// 재선언을 하였음에도 오류가 발생하지 않는다
```

### 2. const
- `constant`(상수)라는 것으로, 바뀌지 않는 변수기에 값을 `재선언, 재할당 할 수 없다`.
```js
const name = "Raccoon";
console.log(name);  // Raccoon

const name = "Cat";
console.log(name);
// Uncaught SyntaxError: Identifier 'name' has already been declared

name = "Turtle";
console.log(name);
//Uncaught TypeError: Assignment to constant variable.
```

### 3. let
- `값이 바뀔 수도 있는 변수`에 대해서 사용하며 `재선언은 불가`하지만, `재할당은 가능`하다.
```js
let name = "Raccoon";
console.log(name); // Raccoon

let name = "Cat";
console.log(name);
// Uncaught SyntaxError: Identifier 'name' has already been declared

name = "Turtle";
console.log(name);  // Turtle
```

### 결론
> always const, sometimes let, never var !   
> 변수 선언은 기본적으로 `const`를 사용하고, 재할당이 필요한 경우에 한정해 `let`으로 변경한다.
