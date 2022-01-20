## ToDoList 기능 구현 중 삽질_id 값의 타입 형변환과 DOM의 간단한 개념
상황은 대충 To Do List를 작성하는 기능 구현 중 원하는 항목을 삭제 버튼 클릭시 미리 저장해뒀던 To Do List 배열 요소들의 id 값과 삭제할 계획의 id값을 비교해 일치하는 항목만 삭제할 예정이었는데 삭제가 되지 않아 
삽질을 좀 했다... 분명 id값은 Date.now()로 number type인데 왜 삭제할 계획의 id 값은 strig인 것인가 ???

### 문제 코드
```html
<form id="todo-form">
        <input type="text" placeholder="Wrtie a To Do and Press Enter" required />
</form>

<ul id="todo-list">
</ul>
```
```js
const toDoForm = document.getElementById("todo-form");
const toDoInput = toDoForm.querySelector("input");
const toDoList = document.getElementById("todo-list");

const TODOS_KEY = "todos";

let toDos = []; 

// localStorage에 작성한 todolist 배열 저장
function saveToDos() {
    localStorage.setItem(TODOS_KEY, JSON.stringify(toDos));
}

// X 버튼 클릭시 화면에서 todo 지우고 localStorage에서도 지움
function deleteToDo(event) {
    const li = event.target.parentElement;

    // 삽질 포인트
    // filtering이 되지 않아 toDo.id의 typeof도 봤는데 li.id type 체크하는 걸 생각 못함...
    // li.id는 stirng 배열 id는 number이므로 형변환 필요
    toDos = toDos.filter((toDo) => toDo.id !== li.id);  // @@ 문제의 코드 @@
    console.log(toDos);
    saveToDos();
    li.remove();
}

// 화면에서 todo 생성되는 함수
function paintToDo(newToDo) {
    const li = document.createElement("li");
    li.id = newToDo.id;
    const span = document.createElement("span");
    span.innerText = newToDo.text;

    const button = document.createElement("button");
    button.innerText = "❌";
    button.addEventListener("click", deleteToDo);
    li.appendChild(span);
    li.appendChild(button);
    
    toDoList.appendChild(li);
}

// 사용자가 todo 작성 후 엔터 눌렀을 때 일어나는 함수
function handleToDoSubmit(event) {
    event.preventDefault();
    const newToDo = toDoInput.value;
    toDoInput.value = "";
    const TodoObj = {
        text: newToDo,
        id: Date.now()
    };    
    toDos.push(TodoObj);
    saveToDos();
    paintToDo(TodoObj);
}

toDoForm.addEventListener("submit", handleToDoSubmit);

const savedToDos = localStorage.getItem(TODOS_KEY);

// 화면 새로고침 혹은 접속시 미리 저장해둔 ToDoList가 있는지 체크, 없다면 null
if(savedToDos !== null){
    const parsedToDos = JSON.parse(savedToDos);
    toDos = parsedToDos;
    parsedToDos.forEach(paintToDo);
}
```
  1. 사용자가 계획 입력 후 enter시 `handleToDoSubmit()` 함수 실행   
     - `TodoObj` - {id : `Date.now()`, text: 해야 할일} 형태로 객체 생성
     - `toDos` - TodoObj 객체를 배열에 push
     - `saveToDos()` - toDos 배열을 직렬화하여 localStorage에 저장.  
     - `paintToDo()` - ul 태그 내부에 계획에 대한 li 태그 생성 후 각 계획들이 구분될 수 있도록 `li.id` 값에 TodoObj.id를 대입   
     → 즉 `li.id`에 `Date.now()`를 대입.  
  2. X 버튼 클릭시 `deleteToDo()` 함수 실행  
      - 클릭된 버튼의 부모 요소인 li 태그 찾음
      - `toDos.filter()` - 미리 저장해둔 계획 객체들이 들어있는 배열에서 filtering하여 `모든 요소의 id값`과 삭제할 `li 태그의 id 값`을 비교   
      - `toDos = toDos.filter((toDo) => toDo.id !== li.id);` 가 바로 문제의 코드.  
      - 분명 paintToDo() 함수에서 `li.id = newToDo.id;` 라는 코드로 Date.now() 라는 `number` 타입의 값을 넣어줬는데 deleteToDo() 함수 실행시 `li.id`는 number가 아닌 `string`이 된 건가??  

### 에러 원인
- 찾아보니 li 태그는 `DOM`을 직접 건드린 건데, DOM의 `id` 특성 값은 `문자열`(string)이라고 한다.
- 따라서 li.id로 `number` 타입의 값을 넣어줬어도 DOM에서는 string으로 `형변환`해서 받아들이게 된다.

### 해결 방법
```js
// 수정 전 코드
toDos = toDos.filter((toDo) => toDo.id !== li.id);

// 수정 코드
toDos = toDos.filter((toDo) => toDo.id !== Number(li.id));
```
- toDo.id는 number 타입이므로 li.id를 `string으로 형변환`하였고 정상적으로 코드가 실행되었다.

### 근데 DOM이 정확히 뭐지?
- `Document Object Model`의 약자로 Documemt는 문서(html), Object는 객체, Model은 모델링한다, 형체를 만들다의 의미이다.
- 즉, `HTML을 JavaScript가 이해할 수 있는 객체란 형태로 만들어 주는 것`이다.
- 부연 설명으로는 서버에서 받아온 HTML을 Object 형태로 파싱한 것으로 웹페이지에 대한 `인터페이스`다.(DOM API)
- DOM에 접근할 수 있는 API인 `document` 객체를 브라우저에서 제공하며, DOM에 접근하는 `entry point`(시작점)이다. (window.document)


