## [Generic] 타입스크립트에서 제네릭 사용법
제네릭은 재사용성이 높은 컴포넌트를 만들 떄 자주 활용되는 특징이며, 특히 한가지 타입보다 여러 가지 타입에서 동작하는 컴포넌트 생성시 사용된다.

### Generic이란?
선언 시점이 아닌 생성 시점에 타입을 명시해 하나의 타입만이 아닌 `다양한 타입`을 사용할 수 있도록 하는 기법이다.    
코드를 통한 예시는 다음과 같다.

```js
function getText(val:string):string {
  return val;
}
```
- 만약 어떤 값을 인자로 받아 반환하는 `getText`함수가 있다고 가정했을때, 인자가 문자열이 아닌 다른 값을 사용할 수도 있게 된다면 어떨까?
- 위 함수는 string 타입만의 인자를 받아 반환하기에 재사용성이 높은 함수라고 볼 수 없다. 이럴때 `제네릭`을 사용할 수 있다.

<br/>

```js
function getText<T>(val: T): T {
  return val;
}
```
- 인자, 반환값 모두 제네릭 타입으로 변경하면 다양한 타입의 인자를 사용할 수 있으며 반환하는 값 또한 인자로 들어온 타입으로 되기에 `재사용성`이 높아진다.
- 위와 같이 작성하면 함수 호출시 넘긴 타입에 대해 타입스크립트가 추정할 수 있게 되어 함수의 인자, 반환값에 대한 타입이 동일한지 검증할 수 있다.
- 물론 `any`를 사용해도 되지만, any라는 타입은 타입 검사를 하지 않기에 함수의 인자로 어떤 타입이 들어갔고 어떤 값이 반환되는지 알 수 없다.


<br/><br/>

### JSX가 설정된 타입스크립트에서 사용할 때?
- 아쉽지만, (JSX에서 설정된 타입스크립트일 때는) 화살표 함수 사용시 함수에서 여는 `<`는 컴파일러에 모호하다.   
- 제네릭 문법인지, JSX 문법인지 모호하기에 이를 구분하기 위해 약간의 도움을 주어야 하며 가장 직관적인 방법은 `extends unknown`을 쓰는 것이다.
```js
const getText = <T extends unknown>(val: T):T => val;
```

<br/><br/>

### 커스텀훅에서 Generic 사용했을때 궁금했던 점
```js
type ReturnTypes<T> = [T, React.Dispatch<React.SetStateAction<T>>, (e: React.FormEvent<HTMLInputElement>) => void];

const useInput = <T extends unknown>(initialData: T): ReturnTypes<T> => {
  const [value, setValue] = useState(initialData);
  const handler = useCallback((e: React.FormEvent<HTMLInputElement>) => {
    setValue(e.currentTarget.value as unknown as T); // (*)
  }, []);
  return [value, setValue, handler];
};

export default useInput;
```
- 회원가입시 인풋창에서 입력한 입력값에 따라 상태를 관리하는 중복된 함수들이 많아 커스텀훅을 이용해 재사용성 있는 `useInput`을 생성했다.
- 초기 매개변수를 받아 (value, setValue, 사용자가 입력시 발생하는 handler)를 반환한다.
- handler 내부에서 `setValue`시 `e.currentTarget.value as T`로 사용해도 되지 않을까?     
  - as는 자유롭게 형변환을 할 수 없지만 any나 `unkonwn`으로만 `as`로 형변환이 가능하고, string, boolean, number 등의 타입은 unknown으로 형변환할 수있다.
  - 그래서 unknown으로 먼저 변경 후 다시 원하는 것으로 바꾼다.

<br/><br/>

### unknown, as는 뭐지?
#### unknown
  - TS 3.0에 해당 타입이 도입되었으며 `알 수 없다, 모른다`라는 의미를 가진다.
  - any타입과 동일하게 모든 값을 허용하지만, 할당된 값이 어떤 타입인지 모르기에 함부로 프로퍼티나 연산을 할 수 없다.
```js
let valueStr : unknown = 'Test';


console.log(valueStr.length); // 문제 발생

if (typeof valueStr === "string") {
  console.log(valueStr.length);   // 4
}
```

#### as
  - `타입을 단언`하는 역할로 개발자가 수동으로 컴파일러에게 특정 변수에 대한 타입 힌트를 줄 때 사용한다.
  - 타입 단언 문법은 as말고도 아래와 같이 타입을 직접 기입하는 방법도 있지만 JSX문법 사용시 `<Type>`키워드가 겹치기에 불편해서 `as Type` 키워드가 추천된다.
```js
(<Wizard>character).fireBall();
(character as Wizard).fireBall();
```

### 참고 자료(Reference)
[타입스크립트 핸드북](https://joshua1988.github.io/ts/guide/generics.html#%EC%A0%9C%EB%84%A4%EB%A6%AD-generics-%EC%9D%98-%EC%82%AC%EC%A0%84%EC%A0%81-%EC%A0%95%EC%9D%98)         
[타입스크립트 any, unknown 차이](https://developer-talk.tistory.com/198)       
[화살표 함수에서 제네릭 사용시 주의점](https://ui.toast.com/weekly-pick/ko_20210521)      
[타입 단언과 as란?](https://hyunseob.github.io/2017/12/12/typescript-type-inteference-and-type-assertion/)       
