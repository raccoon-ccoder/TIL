## [react-beautiful-dnd] 리스트 드래그 앤 드랍
라이브러리로 쉽게 구현하는 드래그 앤 드랍 기능으로 코드는 타입스크립트 기준이다.

### 라이브러리 다운로드
```js
npm i react-beautiful-dnd
npm i --save-dev @types/react-beautiful-dnd 
```

### react-beautiful-dnd 핵심 개념
![53607406-c8f3a780-3c12-11e9-979c-7f3b5bd1bfbd](https://user-images.githubusercontent.com/77538818/166433523-268c0dc2-2ec4-4fe2-b6b4-baede1b61fd6.gif)   

<br/>

__1) DragDropContext__
```js
<DragDropContext onDragEnd={onDragEnd}>
      ...
</DragDropContext>
```
- 드래그 앤 드롭을 인식할 수 있는 영역으로 전체 애플리케이션을 감싸는 것이 좋다.
- 필수 props
  - `onDragEnd` - 드롭했을 때 발생하는 이벤트
  - children - React.Node

__2) Droppable__
```js
 <Droppable droppableId={boardId}>
        {(provided, snapshot) => (
          <Area
            isDraggingOver={snapshot.isDraggingOver} // 현재 선택한 Draggable이 특정 Droppable위에 드래깅 되고 있는지 여부 확인
            draggingFromThisWith={Boolean(snapshot.draggingFromThisWith)} // 현재 Droppable에서 벗어난 드래깅되고 있는 Draggable ID
            ref={provided.innerRef}
            {...provided.droppableProps}
          >
            {toDos.map((toDo, index) => (
              <DraggableCard
                key={toDo.id}
                index={index}
                toDoId={toDo.id}
                toDoText={toDo.text}
                boardId={boardId}
              />
            ))}
            {provided.placeholder}
          </Area>
        )}
</Droppable>
```
- `drop`될 수 있는 영역을 형성한다.
- 필수 props
  - `droppableId` - drop할 수 있는 영억의 고유값을 의미한다.
  - children - ReactElement를 반환하는 함수
- children function 매개변수
  - `provided` - provided의 `innerRef`,`droppableProps`를 children의 props로 넣어줘야 부모-자식간의 접근과 css가 작동해 dnd를 동적으로 수행할 수 있다.
  - `snapshot` - 현재 드래그 state와 관련된 몇가지 state를 제공한다.    
  
 참고로 `provided.placeholder`은 Draggable 엘리먼트를 드래그하는 동안 position: fixed(영역을 고정시킴)를 적용해 Draggable을 드래그할 때 Droppable 리스트가 작아지는 것을 방지하기 위해 필요하며 Draggable 노드의 형제로 렌더링하는 것이 좋다.
 
 __3) Draggable__
```js 
<Draggable draggableId={toDoId + ""} index={index}>
      {(provided, snapshot) => (
        <Card
          isDragging={snapshot.isDragging}
          ref={provided.innerRef}
          {...provided.draggableProps}
        >
          <span {...provided.dragHandleProps}>🍎</span>
          <DeleteBtn onClick={onDelete}>X</DeleteBtn>
        </Card>
      )}
</Draggable>
```
- 드래그할 요소들을 감싸주는 영역을 형성한다.
- 필수 props
  - `draggableId` - 식별자 역할을 하는 고유 값
  - `index` - Droppable 내에서 몇번째 위치하는지 알려주는 값으로 Droppable 내에서 고유하며 연속적이어야 한다.
  - children - ReactElement를 반환하는 함수
- children function 매개변수
  - `provided`
    -  `innerRef`,`droppableProps`를 children의 props로 넣어줘야 부모-자식간의 접근과 css가 작동해 dnd를 동적으로 수행할 수 있다. 
    -  `dragHandleProps`(Option) - drag 할 수 있는 영역과 없는 영역으로 나눌 수 있으며 위 예시를 보면 사과 이모티콘을 통해서만 드래그할 수 있고 다른 영역을 누르면 드래그되지 않는다.
  - `snapshot` - 현재 드래그 state와 관련된 몇가지 state를 제공한다.  

> 여기서 주의할 점이 map과 같은 함수를 사용할 때 고유값이 되는 `key`를 지정해야 하는데 이때 `droppableId`,`draggableId`의 고유값과 동일하게 설정해야 한 태그에 하나의 고유값이 생성되며 정상 작동한다.

### onDragEnd 이벤트 처리하기
`onDragEnd`이벤트 처리를 해야만 드래그 앤 드롭 기능이 상태와 연동되어 동작한다.
```js
function App() {
  const [toDos, setToDos] = useRecoilState(toDoState);
  const onDragEnd = (info: DropResult) => { // (*)
 
    const { destination, source } = info;
    if (!destination) return;
    if (destination.droppableId === "trash") {
      setToDos((allBoards) => {
        const sourceBoard = [...allBoards[source.droppableId]];
        sourceBoard.splice(source.index, 1);
        return { ...allBoards, [source.droppableId]: sourceBoard };
      });
    } else if (destination?.droppableId !== source.droppableId) {
      // cross board movement
      setToDos((allBoards) => {
        const sourceBoard = [...allBoards[source.droppableId]];
        const targetBoard = [...allBoards[destination.droppableId]];
        const taskObj = sourceBoard[source.index];
        // 1) Delete item on source.index
        sourceBoard.splice(source.index, 1);
        // 2) Put back the item on the destination.index
        targetBoard.splice(destination?.index, 0, taskObj);
        return {
          ...allBoards,
          [destination.droppableId]: targetBoard,
          [source.droppableId]: sourceBoard,
        };
      });
    } else if (destination?.droppableId === source.droppableId) {
      setToDos((allBoards) => {
        const boardCopy = [...allBoards[source.droppableId]];
        const taskObj = boardCopy[source.index];
        // 1) Delete item on source.index
        boardCopy.splice(source.index, 1);
        // 2) Put back the item on the destination.index
        boardCopy.splice(destination?.index, 0, taskObj);
        return {
          ...allBoards,
          [destination.droppableId]: boardCopy,
        };
      });
    }
  };

  return (
    <DragDropContext onDragEnd={onDragEnd}> // (*)
      <Wrapper>
        <CategoryBox>
          <AddCategory />
        </CategoryBox>
        <ToDoBox>
          <Boards>
            {Object.keys(toDos).map((boardId) => (
              <Board key={boardId} boardId={boardId} toDos={toDos[boardId]} />
            ))}
          </Boards>
        </ToDoBox>
        <TrashCan />
      </Wrapper>
    </DragDropContext>
  );
}
```
- todo 요소를 다른 Droppable에 드래그 했을 경우 해당하는 board에는 todo를 제거하고 추가할 board 배열에 todo를 추가하여 상태를 업데이트한다.
- `onDragEnd`의 인자로는 `DropResult`가 오는데 아래와 같은 중요한 정보를 제공한다.
  - `draggableId` - 드래그 되었던 `Draggable`의 `type`
  - `source` - `Draggable` 이 시작된 위치(location)
  - `destination` - `Draggable`이 끝난 위치(location)로 만약에 Draggable이 시작한 위치와 같은 위치로 돌아오면 이 destination값은 `null`이 된다.



## 참고 자료(Reference)
[beautiful-dnd doc 한글화](https://github.com/LeeHyungGeun/react-beautiful-dnd-kr)











