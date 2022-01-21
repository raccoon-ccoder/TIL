## 논리연산자_첫번째 truthy를 찾는 OR 연산자 ||

### OR 연산자 ||
- 인수 중 하나라도 true면 true를 반환, 그렇지 않으면 false를 반환하는 연산자다.
- 피연산자가 모두 false인 경우를 제외하고 연산 결과는 항상 true이다.
- 피연산자가 불린형이 아니면, 평가를 위해 불린형으로 변환된다.
```js
if(1 || 0){ // if(true || false) 와 동일하게 동작한다
  alert("truthy");
}
```

### 첫번째 truthy를 찾는 OR 연산자 ||
- 자바스크립트에서만 제공하는 논리연산자 OR의 추가 기능으로 OR 연산자와 피연산자가 여러 개인 경우 다음과 같이 동작한다.
```js
result = value1 || value2 || value3;
```
1. 가장 왼쪽 피연산자부터 시작해 오른쪽으로 나아가며 피연산자를 평가한다.
2. 각 피연산자를 불린형으로 변환한다. 변환 후 그 값이 true면 연산을 멈추고 해당 피연산자의 `변환 전 원래 값`을 반환한다.
3. 피연산자 모두를 평가한 경우(모든 피연산자가 false로 평가되는 경우)엔 마지막 피연산자를 반환한다.

즉, OR 연산자를 여러 개 체이닝(chaining)하면 첫번째 truthy를 반환한다. 피연산자에 truthy가 하나도 없다면 마지막 피연산자를 반환한다.

### 예시
```js
alert( 1 || 0 ); // 1 (1은 truthy임)

alert( null || 1 ); // 1 (1은 truthy임)
alert( null || 0 || 1 ); // 1 (1은 truthy임)

alert( undefined || null || 0 ); // 0 (모두 falsy이므로, 마지막 값을 반환함)
```

### 예시2
유투버 라매개발자님의 자바스크립트 array 챌린지 문제를 풀다가 다른 분들의 답을 봤는데 처음에 이해가 가지 않았던 문제이다.  
[문제링크](https://github.com/pkiop/JS-Array-Challenge/tree/master/Problems/sortByPriceAndQuantity)
```
# 문제제목

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
// 내 코드
function solution(inputArray) {
  return inputArray.sort((a, b) => {
    if (a.price === b.price) {
      return a.quantity - b.quantity;
    }
    return a.price - b.price;
  });
}

exports.solution = solution;

// 다른 분들의 코드
function solution(inputArray) {
    return inputArray.sort((a,b)=> a.price - b.price || a.quantity - b.quantity);
}

exports.solution = solution;
```
- 해당 문제는 array의 sort()를 이용해서 풀어야 했던 문제이다.
- 내가 아는 지식은 || 연산자 사용시 하나라도 true가 있다면 true를 반환하는줄만 알았다. 그래서 처음에 이해가 가지 않았다...
- 만약 price가 같다면 a.price - b.price는 0이 되므로 false가 되고, a.quantity - b.quantity 값이 반환된다. (quantity는 다 다르기에 0이 나올 수가 없다)
- a.price - b.price의 값이 0이 아닌 이상, 항상 a.price - b.price 값이 반환되므로 price에 따라 배열을 정렬할 수 있다.

