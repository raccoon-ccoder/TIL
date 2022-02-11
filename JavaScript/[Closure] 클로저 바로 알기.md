## [Closure] 클로저 바로 알기
자바스크립트의 고유한 개념이 아니라, 함수형 프로그래밍 언어에서 공통적으로 발견되는 특성으로 클로저에 대해 공부하며 정리한 글이다.

### 클로저란?
일반적으로 자신의 내부가 아닌 `외부에서 선언된 변수에 접근`하는 것을 뜻한다.

```js
const rate = 1113.5;

function convertUsdToKrw(dollar) {
  return dollar * rate;
}

console.log(convertUsdToKrw(5));  // 5567.5
```
- 위 코드에서 환율(rate) 변수를 함수 외부에 선언하고 달러를 원화로 바꿔주는 convertUsdToKrw 함수 호출시 정상적으로 출력된다.   
- 이처럼 자바스크립트에서 함수는 매개 변수, 지역 변수 뿐만 아니라 외부에서 선언된 변수도 자유롭게 접근할 수 있으며, 함수가 자신의 밖에서 선언된 변수에 접근하는 것을 클로저라 한다.

### 클로저가 가능한 이유?
아래 코드의 3번에서 outer 함수 호출시 inner 함수 반환 후 생명 주기를 마감하기에 outer 함수의 지역 변수 x에는 더이상 접근할 수 없어 보이는데 4번의 결과는 전역 변수가 아닌 outer 함수의 지역 변수 x의 값인 10이 출력된다. 어떻게 된걸까?
```js
const x = 1;

// (1)
function outer() {
  const x = 10;
  const inner = function() { console.log(x); }; // (2)
  return inner;
}

const innerFunc = outer();  // (3)
innerFunc();  // (4) 10
```
- 자바스크립트는 `렉시컬 스코프`(정적 스코프)를 따르기에, 함수는 자신이 호출되는 환경과 상관없이 자신이 정의된 환경, 즉 상위 스코프를 기억하기 위해 자신의 내부 슬롯 `[[Enviroment]]`에 `상위 스코프의 참조`를 저장한다.  
    1. outer 함수가 평가되어 함수 객체 생성시, 전역 렉시컬 환경을 outer 함수 객체의 `[[Enviroment]]` 내부 슬롯에 상위 스코프로 저장
    2. 런타임에 중첩 함수 inner가 평가되며 이때, inner `[[Enviroment]]` 내부 슬롯에 현재 실행 중인 실행 컨텍스트의 환경(outer 함수의 렉시컬 환경)을 상위 스코프로 저장
    3. outer 함수 실행 종료시 inner 함수 반환 후 생명 주기 종료 (실행 컨텍스트 스택에서 outer 함수의 실행 컨텍스트 제거)   
      *Q) 이때, outer 함수의 렉시컬 환경까지 소멸하진 않는다. 왜 그럴까?*  
      → `outer` 함수의 렉시컬 환경은 (2) 과정으로 `inner` 함수의 `[[Enviroment]]` 내부 슬롯에 의해 참조된다.   
      → 위 `inner` 함수는 전역 변수 `innerFunc`에 의해 참조되고 있다.    
      → 참조로 ***가비지 컬렉터는 누군가가 참조하고 있는 메모리 공간을 함부로 해제하지 않기에*** outer 함수의 생명주기가 마감되어도 outer 함수의 지역 변수 x가 살아있는 이유다.
    4. outer 함수가 반환한 inner 함수 호출시 inner 함수의 실행 컨텍스트가 생성되고 실행 컨텍스트 스택에 푸시된다. 그리고 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 객체의 [[Enviroment]] 내부 슬롯에 저장되어 있는 참조값이 할당된다.

위와 같이 상위 스코프의 식별자를 참조 & 본인의 외부 함수보다 더 오래 살아있는 중첩 함수도 `클로저`라고 한다.

<br/>

Q) 그러면 클로저는 함수를 리턴하는 함수인가 ?  
→ 함수를 리턴하는 함수의 클로저 사례가 아주 많지만 언제나 함수를 리턴하는 것은 아니다. 아래는 return 없이도 클로저가 발생하는 경우다.
```js
(function() {
  let count = 0;
  let button = document.createElement("button");
  button.innerText = "click me!";
  button.addEventListener("click", function() {
    console.log(++count, "times clicked");
  });
  document.body.appendChild(button);
})();
// 별도의 외부 객체인 DOM의 메서드(addEventListener)에 등록한 handler 함수 내부에서 지역 변수를 참조
```
지역 변수를 참조하는 내부 함수를 외부에 전달했기에 클로저는 맞지만, **"외부로 전달"이 항상 return을 의미하는 것은 아니다.** 

### 클로저의 활용
클로저는 상태를 의도치 않게 변경되지 않도록 `상태를 안전하게 은닉`하고 `특정 함수에게만 상태 변경을 허용`하기 위해 사용한다.   

<br/>

아래는 함수가 호출된 횟수를 누적하여 출력하는 카운터를 만들려고 하는데 오류가 생기기 쉬운 코드다. 
```js
// 카운트 상태 변수
let count = 0;

// 카운트 상태 변경 함수
const increase = function() {
  // 카운트 상태 1 증가
  return ++count;
}

console.log(increase());  // 1
console.log(increase());  // 2
console.log(increase());  // 3

count = -10;
console.log(count + 5); // 5
```
카운트 상태 함수는 올바르게 1씩 증가하지만 count라는 전역 변수에 다른 사람도 쉽게 접근할 수 있고 상태 변경도 할 수 있기에 오류로 이어질 수 있다.   
따라서 올바르게 작동하기 위해선 다음과 같은 전제 조건이 지켜져야 한다.
  - 카운트 상태(count)는 increase 함수 호출전까지 변경되지 않고 유지되어야 함
  - 이를 위해 count 값은 increase 함수만이 변경할 수 있어야 함
이를 위해 클로저를 활용해 코드를 변경해본다.

```js
const increase = (function() {
  let count = 0;
  
  return function() {
    return ++count;
  }
})();


console.log(increase());  // 1
console.log(increase());  // 2
console.log(increase());  // 3

count = -10;  // ReferenceError: count is not defined
console.log(count + 5); // ReferenceError: count is not defined
```
클로저를 활용해 외부에 count 변수를 접근할 수 없게 되었고 increase 함수만이 count 변수를 다룰 수 있게 되었다.   
이처럼 `클로저`는 **상태(count)가 의도치 않게 변경되지 않도록 안전하게 은닉**하고 **특정 함수(increase)에게만 상태 변경을 허용**하여 상태를 안전하게 변경하고 유지하기 위해 사용한다. 

### 문제 - 클로저에서 흔한 실수
아래는 shooters가 요소인 배열을 만들어준다. 모든 함수는 몇 번째 shooter인지 출력해야 하는지 뭔가 잘못되었다. 그 이유가 정상적으로 실행하기 위한 수정 코드를 생각해보자.
```js
function makeArmy() {
  let shooters = [];

  let i = 0;
  while (i < 10) {
    let shooter = function() { // shooter 함수
      alert( i ); // 몇 번째 shooter인지 출력해줘야 함
    };
    shooters.push(shooter);
    i++;
  }

  return shooters;
}

let army = makeArmy();

army[0](); // 0번째 shooter가 10을 출력함
army[5](); // 5번째 shooter 역시 10을 출력함
// 모든 shooter가 자신의 번호 대신 10을 출력하고 있음
```

<details>
<summary>정답</summary>
<div markdown="1">

### 원인
  - shooters 배열에는 다음과 같은 요소가 추가된다.
```js
shooters = [
  function () { alert(i); },
  function () { alert(i); },
  function () { alert(i); },
  function () { alert(i); },
  function () { alert(i); },
  function () { alert(i); },
  function () { alert(i); },
  function () { alert(i); },
  function () { alert(i); },
  function () { alert(i); }
];
```
  - shooter 내부에 지역 변수 i 가 없기에 외부 렉시컬 환경에서 가져오게 된다.
  - loop를 돌아 i의 최종값은 10이 되기에 모든 값은 10으로 alert된다.
  
### 해답
  1. while문을 `for`문으로 변경하여 새로운 렉시컬 환경을 만들어 각각 상응하는 i 변수의 값 전달하기 
  ```js
function makeArmy() {

  let shooters = [];

  for(let i = 0; i < 10; i++) {
    let shooter = function() { // shooter function
      alert( i ); // should show its number
    };
    shooters.push(shooter);
  }

  return shooters;
}

let army = makeArmy();

army[0](); // 0
army[5](); // 5
```
                        
2. while 내부에 `또 다른 지역` 변수 선언하여 loop문 돌때마다 새로운 렉시컬 환경 생성하기
```js
function makeArmy() {
  let shooters = [];

  let i = 0;
  while (i < 10) {
    let j = i;
    let shooter = function() { // shooter function
      alert( j ); // should show its number
    };
    shooters.push(shooter);
    i++;
  }

  return shooters;
}

let army = makeArmy();

army[0](); // 0
army[5](); // 5
```
                
  
  
</div>
</details>
  
  
  

### 요약
- 클로저 - 보통 상위 스코프의 식별자를 참조하면서 본인의 외부 함수보다 더 오래 살아있는 중첩 함수를 의미
- 클로저 동작 원리 - 함수는 자신이 정의된 환경, 즉 상위 스코프를 기억하기 위해 자신의 내부 슬롯 [[Enviroment]]에 상위 스코프의 참조를 저장
- 클로저의 사용 용도
  - 전역 변수의 남용을 막음
  - 변수 값을 은닉하는 용도
  
### 참고 자료(Reference)
[모던 자바스크립트 딥 다이브 요약](https://ko.javascript.info/closure)    
[DaleSeo님 자바스크립트 클로저 설명](https://www.daleseo.com/js-closures/)


