## [this] this와 참조 타입, apply & call & bind
일반적으로 객체지향 언어에서 this는 함수가 속해 있는 객체, 자기자신과 밀접한 관련이 있지만 JS에서는 함수가 일급객체다.   
즉, 함수를 변수나 데이터, 함수의 인수, 반환 값으로 사용할 수 있기에 다양한 환경에서 호출될 수 있기에 자기 자신이라는 개념이 모호할 때가 있다.   
this에 대해 공부하면서 헷갈렸던 부분들을 정리한 글이다.    

### this란?
- 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 `자기 참조 변수`를 뜻한다.   
- 단, this가 가리키는 값, 즉 this 바인딩은 `함수 호출 방식`에 의해 동적으로 결정된다.
( 바인딩 : 식별자와 값을 연결하는 과정 )

### binding rules 따른 this
#### 1. 기본 바인딩
- 함수 단독 실행시, 기본적으로 `this`에는 `전역 객체`가 바인딩된다.
- 하지만 'use strict'가 적용된 일반 함수 내부의 this는 undefined가 된다.
```js
function foo() {
    console.log(this); // window
}

function fooStrict() {
    'use strict'
    console.log(this); // undefined
}
foo();
fooStrict();
```

#### 2. 암시적 바인딩
- 객체의 메소드로 호출시 `this`는 점연산 바로 앞에 있는 객체에 바인딩돤다.
```js
const obj = {
    name: "raccoon",
    getName() {
        return this.name;
    },
};
console.log(obj.getName());
```

<br/>

여기서 주의할 부분이 있다.
```js
const obj = {
    name: "raccoon",
    getName() {
        return this.name;
    },
};
const getName = obj.getName;
getName();  // undefined
```
Q) obj.getName()에서 getName()은 바인딩되어 있으니까 마지막 라인인 getName() 호출시 obj.getName()이 정상적으로 실행될 것 같은데.. undefined ??   
→ 결론적으로는 getName = obj.getName은 obj.getName이 아닌 `getName의 참조값`만을 할당해주는 코드다. 이런 암시적 바인딩에선 `암시적 소실`이라는 쉬운 함정에 빠지게 된다.

#### 참조 타입 ?
obj.getName()이라는 코드가 있을 때 2가지의 연산이 실행된다.   
  1. 점`.`은 객체 프로퍼티 obj.getName에 접근한다.
  2. 괄호`()`는 접근한 프로퍼티(메서드)를 실행한다.   
getName = obj.getName 에서는 함수가 변수에 할당된다. 그런데 마지막 줄과는 완전히 독립적으로 동작하기에 this에는 아무런 값도 저장되지 않는다.
obj.getName()를 의도한대로 동작하기 위해 자바스크립트는 `.`이 함수가 아닌 `참조 타입값`을 반환하게 된다.

<br/>

참조 타입에 속하는 값은 (`base`, `name`, `strict`) 형태를 띈다.
  - base - 객체
  - name - 프로퍼티명
  - strict - 엄격 모드에서 true
즉, obj.getName으로 접근하면 함수가 아닌, `참조형`(참조 타입)값을 반환한다. 그리고 참조형 값에 괄호()를 붙여 호출하면 객체, 객체의 메서드와 연관된 모든 정보를 받는다.   

이 정보를 기반으로 this(=obj)가 결정된다.  
이렇게 참조 타입은 내부에서 점.연산에서 알아낸 정보를 괄호()로 전달해주는 '중개인'역할을 한다.

<br/>

그런데, 점연산, 대괄호 이외의 연산(할당연산)은 참조 타입을 통째로 버리고 `obj.getName`값(함수)만 받아 전달하기에 점 이외의 연산에선 `this` 정보가 사라진다.    
따라서 getName()을 호출하는 것은 obj.getName()을 호출하는 것이 아니라 기본 `getName()`을 호출하게 되고 여기서 `this`는 window가 되기에 undifined가 출력된다.   

<br/>


즉, `함수 레퍼런스에 대한 컨텍스트 객체가 존재할 때 암시적 바인딩 규칙에 따르면 바로 이 컨텍스트 객체가 함수 호출시 this에 바인딩된다.`   
그렇기에 `obj.getName()` 같이 점을 사용하거나 `obj[method]()` 같이 대괄호를 사용해 함수를 호출했을 때만 this 값이 의도한 대로 전달된다. 


### 3. 명시적 바인딩
- 말 그대로 `call()`, `apply()`메서드를 사용해 바인딩할 객체를 명시해주는 것이다.(`bind()`의 경우 엄밀히 말하면 하드 바인딩으로 조금 다르다.)
- apply()는 호출할 함수의 인수를 `배열`로 묶어 전달, call()은 호출함 함수의 인수를 쉼표로 구분한 `리스트 형식`을 전달한다는 차이뿐, 역할은 같다.
```js
function func() {
    console.log(this.val);
}

const obj = {
    val: 2
};
func.call(obj);     // 2
func.apply(obj);    // 2

// Function.prototype.call()
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

console.log(new Food('cheese', 5).name);  // "cheese"

// Function.prototype.apply()
const numbers = [5, 6, 2, 3, 7];

const max = Math.max.apply(null, numbers);

console.log(max); // 7

const min = Math.min.apply(null, numbers);

console.log(min); // 2
```

- 하드 바인딩 - 해당 함수를 특정 객체에 `바인딩한 새로운 함수를 반환`해줘 변수에 저장할 수 있게끔 하는 것이다.
- 따라서 하드 바인딩을 하면 일회성의 apply(), call() 메서드와는 다르게 아예 바인딩 된 함수를 가져와 변수처럼 사용할 수 있다.
- 또한 이미 하드 바인딩된 함수는 call(), apply()를 활용해 다시 바인딩할 수 없다는 점이다.
- 이는 써드 파티 라이브러리를 활용하거나 다수의 개발자들이 협업하는 경우, 바인딩이 꼬이지 않도록 해준다.
```js
function func() {
    console.log(this.val);
}

const obj = {
    val: 2
}

const bound = func.bind(obj); 
bound(); // 2

const obj2 = { // 새로 바인딩 시도할 객체
    val: 10
}

bound.call(obj2); // 2 (하드 바인딩 객체에 새롭게 바인딩할 수 없다)
```

### 4. new 바인딩
- new 키워드를 사용해 실행하면 다음과 같은 과정이 진행된다.
    1. 새 객체가 생성됨
    2. 새로 생성된 객체의 [[Prototype]]이 연결됨
    3. 새로 생성된 객체는 해당 함수 호출 시 this 바인딩 됨
    4. 이 함수가 자신의 또 다른 객체를 반환하지 않는 한 new와 함께 호출된 함수는 자동으로 새로 생성된 객체를 반환 

```js
// new 바인딩
function func(val) {
    this.val = val;
}

const obj = new func(2);
console.log(obj.val);
```

위 코드를 보면 new를 붙여 func() 함수를 호출했고, 새로 생성된 객체는 func() 호출시 this에 바인딩된다.

### this 바인딩 규칙 적용 우선순위
1. new 바인딩 - 생성자 함수가 (미래에) 생성할 인스턴스
2. 명시적 바인딩(apply, call, bind) - 해당 메서드에 첫번째 인수로 전달한 객체
3. 암시적 바인딩 - 메서드를 호출한 객체
4. 기본 바인딩 - 전역 객체
    
### 참고 자료(Reference)
[모던 자바스크립트 딥 다이브](https://ko.javascript.info/reference-type)     
[명시적 바인딩 - mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)


