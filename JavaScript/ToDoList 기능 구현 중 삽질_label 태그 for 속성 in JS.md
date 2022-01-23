## ToDoList 기능 구현 중 삽질_label 태그 for 속성 in JS
checkbox 타입의 input 태그와 label 태그를 JavaScript에서 생성하다 삽질한 부분에 대해 정리한 글이다.

### 문제상황
```js
const label = document.createElement("label");
label.for = `input-${newToDo.id}`;
```
- input checkbox의 css를 적용하기 위해 `label` 태그를 생성하고 `for` 속성을 지정하였는데 for 속성이 지정되지 않는 오류였다.

### 해결방법
```js
const label = document.createElement("label");
label.htmlFor = `input-${newToDo.id}`;
```
- JavaScript에서 label 태그의 for 속성을 지정할 경우, `htmlFor` 속성을 사용해야 한다.
- 만약 비슷한 상황이 또 올 경우, console.dir()를 이용해 해당 객체에 어떤 속성을 사용할 수 있는지 콘솔창에서 확인하면 되겠다.
