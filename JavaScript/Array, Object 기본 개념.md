## Array, Object 기본 개념

### Array
- 설명이 필요하지 않은 데이터 리스트들을 의미한다.
- ex : 요일들을 배열에 담아서 daysOfWeek 라는 배열을 생성할 수 있다.

```js
const daysOfWeek = ["mon", "tue", "wed", "thu", "fri", "sat", "sun"];

console.log(daysOfWeek[2]); // wed

daysOfWeek.push("hello"); // 배열 마지막에 원소 추가
console.log(daysOfWeek);  // ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun', 'hello']
```

### Object
- 설명이 필요한 정보가 담긴 데이터 리스트들을 표현할 때 사용한다.
- ex : 만약 게임 플레이어의 이름, 나이, 좌표를 배열에 담을 경우 각각의 값이 무엇을 의미하는지 알 수 없기에 `obejct`로 만들어서 각각 `property`를 부여한다.
```js
const player = ["Raccoon", 10, true]; 
// array로 표현했을 경우 각 원소들이 무슨 의미인지 파악하기 어렵다

// Object로 표현했을 경우 
const player = {
    name: "Raccoon",
    points: 10,
    fat: true,
    sayHello: function(otherPersonName){
        console.log("hello! " + otherPersonName);
    }
};


console.log(player);
// {name: 'Raccoon', points: 10, fat: true, sayHello: ƒ}

// obejct의 property만을 호출할 때 표기법
console.log(player.name); // Raccoon
console.log(player["name"]); // Raccoon

// object에 property 값을 업데이트 하고 싶을 경우
player.fat = false;

console.log(player.fat);  // false

// object에 property를 추가하고 싶을 경우
player.lastName = "Baek";
console.log(player);  
// {name: 'Raccoon', points: 10, fat: true, lastName: 'Baek', sayHello: ƒ}
```
**Q)`player`는 `constant`인데 값 변경이 가능한 이유?**
- player 자체를 변경하는 것이 아닌 그 내부의 `property`를 변경하는 것이기에 가능하다.
- 만약 `player = false;` 이런식으로 업데이트하게 된다면 오류가 발생한다.
