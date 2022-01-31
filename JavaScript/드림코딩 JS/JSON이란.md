## [드림코딩 JS - 6. JSON이란]
데이터를 주고 받을 때 사용할 수 있는 `가장 간단한 포맷`이다.   
예전에는 클라이언트 - 서버 간의 통신에서 AJAX에서 XMLHttpRequest를 사용했지만 요새는 Fetch API도 사용한다.    
AJAX에서는 `XMLHttpRequest`라는 브라우저 제공 오브젝트를 사용하는데 간단하게 서버에게 데이터르르 요청하고 받아올 수 있다.   
현재 XML보다는 JSON을 많이 이용한다.

### JSON 특징
- JSON 문서 형식은 자바스크립트 객체의 형식을 기반으로 만들어졌다.
- 자바스크립트 문법과 유사하지만 `텍스트` 형식일 뿐이다.
- 특정 언어에 종속되지 않으며, 대부분 프로그래밍 언어에서 JSON 포맷의 데이터를 핸들링 할 수 있는 라이브러리를 제공한다.
- JS를 이용해 JSON 형식의 문서를 쉽게 JS 객체로 변환할 수 있다.

### JSON? XML?
- XML은 불필요한 태그가 많이 포함되어 있어 파일 사이즈도 커지고 가독성이 좋지 않다.
- JSON을 사용하여 더 적은 데이터의 양과 더 높은 처리 성능의 장점을 살리는 것이 좋을듯 하다.

### Obejct to JSON
```js
// stringify(obj)
let json = JSON.stringify(true);
console.log(json);  // true

json = JSON.stringify(['apple', 'banana']);
console.log(json);  // ["apple","banana"] (array 보이는 string)

const rabbit = {
    name: 'tokki',
    color: 'white',
    size: null,
    birthDate: new Date(),
    // symbol: Symbol("id"),
    jump: () => {
        console.log(`${name} can jump!`);
    }
};

json = JSON.stringify(rabbit);  
console.log(json);  // {"name":"tokki","color":"white","size":null,"birthDate":"2022-01-30T14:17:04.957Z"}
// function은 직렬화 불가능

json = JSON.stringify(rabbit, ["name"]);  // {"name":"tokki"}
// 매개변수 replacer를 작성하여 원하는 property만 추출 가능
console.log(json);

json = JSON.stringify(rabbit, (key, value) => {
    console.log(`key : ${key}, value : ${value}`);
    return key === 'name' ? 'raccoon' : value;
}); 
console.log(json);

// key : , value : [object Object]
// json.js:34 key : name, value : tokki
// json.js:34 key : color, value : white
// json.js:34 key : size, value : null
// json.js:34 key : birthDate, value : 2022-01-30T14:22:42.937Z
// key : jump, value : () => {
//     console.log(`${name} can jump!`);
// }

// {"name":"raccoon","color":"white","size":null,"birthDate":"2022-01-30T14:25:08.710Z"}
// 특이한 점은 토끼를 싸고 있는 최상위 Object가 전달
```
- `JSON.stringify(obj)` 메서드로 객체를 JSON 데이터로 직렬화 할 수 있다.
- JavaScript에만 있는 개념(`Symbol`)이나 `function`은 직렬화가 불가능하다.
- 또한 stringify()의 매개변수 `replacer`를 지정하여 원하는 property만 직렬화 할 수 있다.

### JSON to Object
```js
// parse(json)
console.clear();
json = JSON.stringify(rabbit);
const obj = JSON.parse(json);
console.log(obj);   // {name: 'tokki', color: 'white', size: null, birthDate: '2022-01-30T14:26:35.633Z'}
// 함수는 seriailize시 포함되지 않았기에 다시 parse()해도 함수는 존재하지 않음

console.log(typeof rabbit.birthDate);   // object (Date)
console.log(typeof obj.birthDate);  // string
// JSON(string)을 parse()했기에 당연히 stirng으로 변환된 birthDate도 그대로 string으로 역직렬화된다

// parse() 매개변수 reviver를 지정하여 string을 다른 타입으로 parse하기 
const obj2 = JSON.parse(json, (key,value) => {
    return key === 'birthDate' ? new Date(value) : value;
    return value;
});
console.log(typeof obj2.birthDate); // object
```
- `JSON.parse()` 메서드로 JSON 타입 데이터를 Object로 역직렬화 할 수 있다.
- 위 코드에서 직렬화된 json 변수는 함수가 포함되지 않았기에 다시 obj로 역직렬화하면 함수가 포함되지 않은 것을 볼 수 있다.
- 직렬화된 JSON 데이터는 string으로 변환되기에 역직렬화 하여도 그대로 string 타입인 것을 볼 수 있다.(`birthDate`)
- parse() 메서드의 매개변수 `reviver`를 지정하면 세부적으로 핸들링할 수 있는데 위 코드에선 string 타입의 birthDate를 Date 객체로 parsing 하였다.

### 자료 링크
https://www.youtube.com/watch?v=FN_D4Ihs3LE&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=10
