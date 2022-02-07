## [Function, Class] 생성자 함수와 Class의 차이
프로토타입에 대해 공부하면서 생성자 함수와 클래스는 같은 역할을 하는듯 한데 두개의 정확한 차이점이 궁금해 정리한 글이다.

### 클래스는 단순한 syntactic sugar일까?
```js
class User {
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name); }
}

// 클래스는 함수입니다.
alert(typeof User); // function

// class User와 동일한 기능을 하는 순수 함수를 만들어보겠습니다.

// 1. 생성자 함수를 만듭니다.
function User(name) {
  this.name = name;
}
// 모든 함수의 프로토타입은 'constructor' 프로퍼티를 기본으로 갖고 있기 때문에
// constructor 프로퍼티를 명시적으로 만들 필요가 없습니다.

// 2. prototype에 메서드를 추가합니다.
User.prototype.sayHi = function() {
  alert(this.name);
};

// 사용법:
let user = new User("John");
user.sayHi();
```
- ES6에 Class 문법이 추가되었고 Class라는 키워드는 클래스 역할을 하는 함수를 선언할 수 있기에 단순히 syntactic sugar라고 볼 수도 있지만 두 개의 방법은 중요한 차이점이 존재한다.

***

#### 1. [[IsClassConstructor]]: true 
   - class로 만든 함수엔 특수 내부 프로퍼티인 `[[IsClassConstructor]]: true`가 붙는다.   
    - class 생성자는 `new`와 함께 호출하지 않으면 에러가 발생하는데 이 때 `[[IsClassConstructor]]: true`가 사용된다.
```js
class User {
  constructor() {}
}

alert(typeof User); // function
User(); // TypeError: Class constructor User cannot be invoked without 'new'
```
#### 2. 클래스의 메서드는 열거할 수 없다. (non-enumerable)
   - 클래스의 `prototype` 프로퍼티에 추가된 메서드 전체의 `enumerable` 플래그는 `false`이기 때문이다.
   - 즉, `for..in`으로 객체 순회시, 메서드가 대상에서 제외된다.
#### 3. 'use strict'
   - 클래스는 항상 엄격모드로 실행된다. 클래스 생성자 안 코드 전체에는 자동으로 `엄격모드`가 적용된다.
#### 4. 함수 선언과 클래스 선언 호이스팅 차이
  - 함수의 경우, 정의하기 전에 호출할 수 있지만, 클래스는 반드시 정의한 뒤 사용할 수 있다.
  - **Q) 예외가 발생하는 이유?** → 클래스는 `호이스팅`될 때 `초기화는 되지 않기 때문`이다.
```js
const p = new Polygon();  //  ReferenceError: Cannot access 'Polygon' before initialization

class Polygon{}
```

### 참고 자료(Reference)
[mozilla mdn](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)   
[모던 자바스크립트 딥다이브](https://ko.javascript.info/class#ref-203)
