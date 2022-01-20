## 이벤트가 발생한 대상(요소) 관련 정보 찾기_event.target
ToDoList 기능 구현 중 각 계획에는 삭제 버튼이 존재하는데 삭제 버튼 클릭시 모든 계획이 아닌 삭제 버튼 옆에 있는 계획만 지우고 싶을 경우 어떻게 찾을 수 있을까?

### Event.target
- `Event` 인터페이스의 `target` 속성은 이벤트가 발생한 대상 객체를 가리킨다.

### 코드 예시
```html
<ul id="todo-list">
  <li id="1642711078630">
    <span>js 공부하기</span>
    <button>❌</button>
  </li>
  <li id="1642714451128">
    <span>js 인강 듣기</span0>
    <button>❌</button>
  </li>
  <li id="1642714457168">
    <span>냥이랑 놀기</span>
    <button>❌</button>
  </li>
</ul>
```
```js
const button = document.createElement("button");
button.addEventListener("click", deleteToDo);

// X 버튼 클릭시 화면에서 todo 지우고 localStorage에서도 지움
function deleteToDo(event) {
    // event.tartget : 이벤트를 만든? 타켓이 무엇인지 -> 클릭된 HTML element
    // button.parentElement parentNode : 해당 태그의 부모 태그 알려줌
    // 버튼이 여러개지만 클릭한 버튼의 부모요소를 찾아서 삭제할 수 있게됨
    const li = event.target.parentElement;

    toDos = toDos.filter((toDo) => toDo.id !== Number(li.id));
    saveToDos();
    li.remove();
}
```
- 삭제 버튼 클릭시 `event`가 발생하며 deleteToDo() 함수가 실행된다.
- 이때 `event.target`은 button이 되고 `parentElement`로 부모 요소인 li를 가리키게 된다 (`parentNode`를 사용해도 된다) 
- li 태그의 id 값을 이용하여 해당 계획을 삭제하는 그런 코드이다.
