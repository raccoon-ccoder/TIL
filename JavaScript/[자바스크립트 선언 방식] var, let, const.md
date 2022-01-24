## [자바스크립트 선언 방식] var, let, const

JavaScript에서 변수 선언 방식인 `var`, `let` ,`const`의 차이점 정리

### 1. var
- `재선언 & 재할당이 가능`하다는 점이 있지만 언어로부터 보호받지 못하기에 실수로 재선언 혹은 재할당을 하여도, 오류가 발생했다는 걸 알려주지 않는다. (const 변수 재할당시 오류 발생)
- block scope 없음, var hoisiting 등 단점이 존재한다.
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
- const 사용시 장점
  - `security` - 해커들이 한번 작성한 값을 변경하지 못하게 함
  - `thread safety` - thread들이 동시에 값에 접근하여 변경하는 것을 막아줌
  - `reduce human mistakes` - 코드 변경을 해도 실수 방지   
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

### 호이스팅
`변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작`하는 자바스크립트 고유의 특징을 변수 호이스팅(variables hoisitng)이라한다.
```js
console.log(score); // undefined
var score = 100;
```
- 자바스크립트 엔진은 변수 선언을 포함한 `모든 선언문`을 소스코드에서 찾아내 `먼저 실행`한다. 그 후 선언문을 제외한 `소스코드를 한 줄씩 순차적으로 실행`한다.
- 따라서 위처럼 변수 선언이 런타임(소스코드가 한 줄씩 실행되는 시점)이 아닌 그 이전에 먼저 실행되기에 undifined가 출력된다.
- 변수 선언뿐 아니라 ES6에서 도입된 let, const를 포함한 모든 선언(var, let, function 등) 식별자는 호이스팅된다. (모든 선언문은 런타임 이전 단계에서 먼저 실행되기 때문)

하지만 `var`로 선언된 변수와 달리 `let`으로 선언된 변수를 선언문 이전에 참조하면 참조 에러가 발생한다.(ReferenceError)   
이는 `let`으로 선언된 변수는 스코프의 시작에서 변수의 선언까지 `일시적 사각지대`(Temporal Dead Zone; TDZ)에 빠지기 때문이다. 
(**Temporal Dead Zone** : 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간)
```js
console.log(foo); // undefined
var foo;

console.log(bar); // Error: Uncaught ReferenceError: bar is not defined
let bar;
```

- 참고로 변수는 `선언 단계` > `초기화 단계` > `할당 단계`에 걸쳐 생성된다
  - 선언 단계 - 변수 이름을 등록해서 자바스크립트 엔진이 변수의 존재를 알린다. (`런타임 이전`에 실행된다.)
  - 초기화 단계 - 값을 저장하기 위한 메모리 공간을 확보하고 암묵적으로 undefined를 할당해 초기화한다.
  - 힐당 단계 - 변수에 값을 할당하는 단계로 값의 할당은 소스코드가 순차적으로 실행되는 `런타임 시점`에 실행된다.   
   
`var`로 선언된 변수는 선언 단계, 초기화 단계가 `한번에` 이뤄진다.
```js
// 스코프의 선두에서 선언 단계와 초기화 단계가 실행된다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 있다.

console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```
`let`으로 선언된 변수는 선언 단계와 초기화 단계가 `분리`되어 진행된다.
```js
// 스코프의 선두에서 선언 단계가 실행된다.
// 아직 변수가 초기화(메모리 공간 확보와 undefined로 초기화)되지 않았다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 없다.
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

### 결론
> always const, sometimes let, never var !   
> 변수 선언은 기본적으로 `const`를 사용하고, 재할당이 필요한 경우에 한정해 `let`으로 변경한다.
