## Array 문법 정리
자주 쓰이는 array 관련 메소드를 정리한 글이다.

### Array.prototype.unshift()
```js
const array1 = [1, 2, 3];

console.log(array1.unshift(4, 5));  // 5

console.log(array1);  // Array [4, 5, 1, 2, 3]
```
- 인수로 전달받은 모든 값을 원본 배열의 `선두`에 요소로 `추가`하며 변경된 length property값을 반환한다.

### Array.prototype.shift()
```js
const array1 = [1, 2, 3];
const firstElement = array1.shift();
```
- 원본 배열의 `첫번쨰 요소`를 `제거`하고 제거한 요소를 반환한다.
- 원본 배열이 빈 배열이면 undefined를 반환한다.

### Array.prototype.concat()
```js
const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];

console.log(array1.concat(array2)); // Array ["a", "b", "c", "d", "e", "f"]
```
- 인수로 전달된 값들을 원본 배열의 `마지막 요소`로 `추가`한 새로운 배열을 반환하며 원본 배열은 변경되지 않는다.

### Array.prototype.splice()
```js
splice(start, deleteCount, item1, item2, itemN)
```
- 원본 배열의 `중간`에 요소 `추가/제거`시 사용한다.  
  - `start` - 원본 배열의 요소를 제거하기 시작할 인덱스, start만 지정시 start부터 모든 요소 제거
  - `deleteCount`(Op) - start부터 제거할 요소의 개수, 0일 경우 제거되지 않음
  - `item`(Op) - 제거한 위치에 삽입할 요소들의 목록
```js
const months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 0, 'Feb');
console.log(months);  // Array ["Jan", "Feb", "March", "April", "June"]

months.splice(4, 1, 'May');
console.log(months); // Array ["Jan", "Feb", "March", "April", "May"]
```

### Array.prototype.includes()
```js
arr.includes(valueToFind[, fromIndex])
```
- ES7에서 도입된 메서드로 배열 내의 `특정 요소가 포함`되어 있는지 `Boolean` 값을 반환한다.
- 1번째 인수로 검색할 대상, 2번째 인수로 검색을 시작할 인덱스를 전달하며 2번째 인수를 생략하면 기본값 0이 설정된다.
```js
[1, 2, 3].includes(2);     // true
[1, 2, 3].includes(4);     // false
[1, 2, 3].includes(3, 3);  // false

// 배열에 요소 3이 포함되어 있는지 인덱스 2(array.length -1)부터 확인
[1, 2, 3].includes(3, -1);  // true
```

## Array 고차 함수
고차 함수란 함수를 인수로 전달받거나 함수를 반환하는 함수로 배열에 다양한 고차 함수에 대해 정리하였다.

### Array.prototype.sort()
- 배열의 요소를 `정렬`하며 원본 배열을 직접 변경하며 정렬된 배열을 반환한다.
- 기본적으로 `오름차순`으로 정렬한다.
- 내림차순으로 정렬하려면 sort() 메서드 사용하여 오름차순으로 정렬 후 `reverse()` 메서드를 사용한다.
- `숫자 요소` 정렬시 sort() 메서드에 정렬 순서를 정의하는 `비교 함수`를 인수로 전달하여 사용한다.
- 비교 함수의 반환값이 음수면 비교 함수의 첫번째 인수, 0이면 정렬하지 않으며,  두번째 인수를 우선하여 정렬한다.
- [문제링크](https://github.com/pkiop/JS-Array-Challenge/tree/master/Problems/sortByPriceAndQuantity)
```js
// sort() 예시
const months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();
console.log(months);
// expected output: Array ["Dec", "Feb", "Jan", "March"]

const array1 = [1, 30, 4, 21, 100000];
array1.sort();
console.log(array1);
// expected output: Array [1, 100000, 21, 30, 4]
```
```js
## 설명
배열안의 객체를 price를 기준으로 오름차순 정렬한 배열을 출력하세요
만약 price가 같다면 quantity기준으로 오름차순 정렬하세요

## Expected Output 
[
  { name: '사과', price: 1000, quantity: 2 },
  { name: '오이', price: 2000, quantity: 49 },
  { name: '당근', price: 2000, quantity: 50 },
  { name: '참외', price: 5000, quantity: 10 },
  { name: '수박', price: 5000, quantity: 20 }
]
```
```js
function solution(inputArray) {
    return inputArray.sort((a,b)=> a.price - b.price || a.quantity - b.quantity);
}
```
- a.price - b.price의 결과값으로 양수, 0, 음수에 따라 정렬할 수 있으며 만약 0인 경우 a.quantity - b.quantity를 이용해 정렬하였다.
- 논리 연산자는 하나라도 truthy 값이 있다면 그 값을 반환하고 quantity 값이 일치하는 경우는 없기에 조건에 맞게 정렬할 수 있다.
- [논리 연산자 || 관련 정리](https://github.com/raccoon-ccoder/TIL/blob/main/JavaScript/%EB%85%BC%EB%A6%AC%EC%97%B0%EC%82%B0%EC%9E%90_%EC%B2%AB%EB%B2%88%EC%A7%B8%20truthy%EB%A5%BC%20%EC%B0%BE%EB%8A%94%20OR%20%EC%97%B0%EC%82%B0%EC%9E%90%20%7C%7C.md)

### Array.prototype.forEach()
- 주어진 함수를 배열 요소 각각에 대해 실행하며 반환값은 언제나 `undefined`이다.
- forEach 메서드의 콜백함수는 forEach 메서드를 호출한 요소값, 인덱스, forEach 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다. 
- 원본 배열을 변경하지 않지만 콜백 함수를 통해 원본 배열을 변경할 수 있다.
- [문제링크](https://github.com/pkiop/JS-Array-Challenge/tree/master/Problems/forEachMap)
```
## 설명

제곱한 후 3으로 나눈 나머지가 홀수인 것 을 뽑은 배열의 총 합을 구하세요.
```
```js
function solution(inputArray) {
    inputArray.forEach((item, index, arr) => {
        arr[index] = `${item}%`;
    });
    return inputArray;
}
```

### Array.prototype.map()
- 배열 내의 모든 요소 각각에 대하여 `콜백 함수를 호출한 결과`를 모아 `새로운 배열`을 반환한다.
- 원본 배열은 변경되지 않는다.
- `forEach`와 비슷한 역할이지만 forEach 메서드는 언제나 `undefined`를 반환하고 `map` 메서드는 콜백 함수의 반환값들로 구성된 `새로운 배열`을 반환하는 차이가 있다.
- map 메서드의 콜백함수는 map 메서드를 호출한 요소값, 인덱스, map 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.   
- [문제링크](https://github.com/pkiop/JS-Array-Challenge/tree/master/Problems/mapAppendOrder)
```
## 설명
배열의 값을 name 프로퍼티에 넣고 몇번째 원소인지를 order에 넣은 객체의 배열을 출력하세요

const test1 = {
  input: ['홍길동', '둘리', '루피'],
  answer: [
    { name: '홍길동', order: 1 },
    { name: '둘리', order: 2 },
    { name: '루피', order: 3 },
  ],
};
```
```js
function solution(inputArray) {
    return inputArray.map((item, idx) => {
        let obj = {};
        obj["name"] = item;
        obj["order"] = idx+1;
        return obj;
    });

}
```

### Array.prototype.some()
- 배열의 요소 중 콜백 함수 조건을 만족하는 요소가 `1개 이상` 존재하면 `true`, 모두 만족하지 않으면 `false`를 반환한다.
- `빈 배열`에서 호출하면 무조건 `false`를 반환한다.
- some 메서드의 콜백함수는 some 메서드를 호출한 요소값, 인덱스, some 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.   
- [문제링크](https://github.com/pkiop/JS-Array-Challenge/tree/master/Problems/findWord)
```
## 설명
용가리라는 단어가 있으면 true 없으면 false를 출력

## Expected Output 
true
```
```js
function solution(inputArray) {
    return inputArray.some((item) => item === "용가리");
}
```

### Array.prototype.every()
- 배열 안의 `모든 요소`가 콜백 함수를 통해 정의한 조건을 만족하는지 확인하여 `Boolean` 값을 반환한다.
- `빈 배열`인 경우 언제가 `true`를 반환한다.
- every 메서드의 콜백함수는 every 메서드를 호출한 요소값, 인덱스, every 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.   
- [문제링크](https://github.com/pkiop/JS-Array-Challenge/tree/master/Problems/everyArray)
```
## 설명
every를 이용해서 모든 원소가 짝수인지 아닌지를 판별하세요

## Expected Output 
true
```
```js
function solution(inputArray) {
    return inputArray.every((item) => item % 2 === 0);
}
```

### Array.prototype.filter()
- 주어진 콜백 함수의 반환값이 `true`인 요소로만 구성된 `새로운 배열`로 반환한다.
- 이때 원본 배열은 `변경되지 않는다`.
- filter 메서드의 콜백함수는 filter 메서드를 호출한 요소값, 인덱스, filter 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.
- [문제링크](https://github.com/pkiop/JS-Array-Challenge/tree/master/Problems/filterIntersection)
```
## 설명
두 배열의 교집합을 출력하세요!
```
```js
function solution(inputArray1, inputArray2) {
    return inputArray1.filter((item) => inputArray2.includes(item));
}
```
- inputArray 배열의 모든 요소를 순회하며 해당 요소가 inputArray2 배열에도 포함되어 있는지 includes() 메서드를 이용해 확인하였다.

### Array.prototype.reduce()
```js
[1, 2, 3, 4].reduce((accumulator, currentValue, currentIndex, array) => accumulator + currentValue, 0); // 10
[1, 2, 3, 4].reduce( (prev, curr) => prev + curr ); // 10
```
- 배열의 각 요소에 대해 `콜백 함수의 반환값`을 다음 순회 시 `콜백 함수의 첫 번째 인수`로 전달하면서 콜백 함수를 호출하여 `하나의 결과값`을 만들어 반환한다.
- 원본 배열은 변경되지 않는다.
- reduce 매개변수
  1. `콜백함수`
      - 초기값 또는 콜백 함수의 이전 반환값(accumulator) - 콜백의 첫 번째 호출이면서 초기값을 제공한 경우 초기값이 된다.
     - reduce 메서드를 호출한 배열의 요소값(currentValue)
      - 인덱스(currentIndex)
      - reduce 메서드를 호출한 배열 자체(array)
  2. `초기값`(Optional) - 제공하지 않으면 배열의 첫 번째 요소를 사용한다. 빈 배열에서 초기값 없이 reduce()를 호출하면 오류 발생
- [문제링크](https://github.com/pkiop/JS-Array-Challenge/tree/master/Problems/reduceNameNickname)
```
## 설명
입력받은 객채배열의 nickname을 key, name을 value로 하는 객체를 출력하세요

const test1 = {
  input: [
    {
      name: '홍길동',
      nickname: 'hong',
    },
    {
      name: '둘리',
      nickname: '2li',
    },
    {
      name: '오스트랄로피테쿠스',
      nickname: '1Cin',
    },
  ],
  answer: { hong: '홍길동', '2li': '둘리', '1Cin': '오스트랄로피테쿠스' },
};
```
```js
function solution(inputArray) {
    return inputArray.reduce((acc, cur) => {
        acc[cur.anme] = cur.nickname;
        return acc;
    }, {});
}
```

### Array.prototype.find()
- ES6에서 도인된 메서드로 콜백함수를 만족하는 `첫 번째 요소`를 반환한다. true인 요소가 없다면 `undefined`를 반환한다.
- find 메서드의 콜백함수는 find 메서드를 호출한 요소값, 인덱스, find 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.
- `filter` 메서드는 `배열`을 반환하지만 `find` 메서드의 결과값은 배열이 아닌 `해당 요소값`이다.
```
const array1 = [5, 12, 8, 130, 44];

const found = array1.find(element => element > 10);

console.log(found);
// expected output: 12
```
