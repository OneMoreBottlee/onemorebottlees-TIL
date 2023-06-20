# #6 Trello

깃허브

{% embed url="https://github.com/OneMoreBottlee/Trello-Clone" %}



### #6.2 **Drag and Drop part One**

react-beautiful-dnd

아래 링크와 같이 Drag\&Drop 기능으로 리스트를 옮겨 수정할 때 유용한 라이브러리



DragDropContext ⇒ 내부에 있어야 작동함

Droppable ⇒ 드롭 : dnd 기능이 실제로 작동할 베이스, 함수형으로 작성해야함

Draggable ⇒ 드래그 : Drag 기능, 함수형으로 작성해야함

```tsx
<DragDropContext onDragEnd={onDragEnd}>
      <div>
        <Droppable droppableId='one'>
          {() =>
            <ul>
              <Draggable draggableId='first' index={0}>
                {() => <li>One</li>}
              </Draggable>
              <Draggable draggableId='seound' index={1}>
                {() => <li>Two</li>}
              </Draggable>
            </ul>}
        </Droppable>
      </div>
    </DragDropContext>
```

[Storybook](https://react-beautiful-dnd.netlify.app/iframe.html?id=board--simple)

```tsx
npm i react-beautiful-dnd
npm i --save-dev @types/react-beautiful-dnd
```

***

### #6.3 **Drag and Drop part Two**

**Droppable & Draggable의 내부 props**

innerRef 속성을 기반으로 수행된다.

dragHandleProps 특정 영역을 통해서만 드래그를 가능하게함

draggableProps 드래그할 영역 지정

```tsx
<DragDropContext onDragEnd={onDragEnd}>
      <div>
        <Droppable droppableId='one'>
          {(magic) =>
            <ul ref={magic.innerRef} {...magic.droppableProps}>
              <Draggable draggableId='first' index={0}>
                {(provided) => <li ref={provided.innerRef} {...provided.draggableProps} {...provided.dragHandleProps} >One</li>}
              </Draggable>
              <Draggable draggableId='seound' index={1}>
                {(provided) => <li ref={provided.innerRef} {...provided.draggableProps} {...provided.dragHandleProps} >Two</li>}
              </Draggable>
            </ul>}
        </Droppable>
      </div>
    </DragDropContext>
```

***

### #6.4 **Styles and Placeholders**

placeholder

Draggable 요소를 드래그하는동안 Droppable에 position:fixed를 적용해 크기가 줄어드는 것을 방지한다.

***

### #6.5 **Reordering**

recoil 기본 세팅

onDragEnd 드래그가 종료됐을때의 세팅

.type - 드래그된 Draggable의 type

.source - 드래그 시작 위치

.destination - 드래그 종료 위치, 시작과 같은 위치라면 null

splice(a, b, c) - a 시작위치 b 지울개수 c 추가할내용

***

### #6.6 **Reordering Two**

onDragEnd, splice로 목록 수정 구현 완료

li로 목록 구분시 key를 제대로 작성해야 한다.

일반적으로 draggableId를 key로 이용한다.

⇒ index를 key로 사용할때 경고하지 않기 때문에 & index를 key로 사용하면 순서가 뒤바뀌면서 좋지않은 결과를 초래한다

***

### #6.7 **Performance**

코드 정리

Drag로 이동하면 모든 리스트가 새로 렌더링되는게 눈에 보임 ⇒ 부모의 상태가 변경되므로 모든 자식 요소들에서 불필요한 렌더링이 발생함

⇒ **React Memo**는 prop이 변경되지 않으면 요소들이 렌더링 되지 않도록 설정함

```tsx
export default DraggableCard;
=>
export default React.memo(DraggableCard);
```

***

### #6.8 **Multi Boards**

여러 상태 박스?를 만들고 싶다.. 그런데 하나의 recoil state에서..

{1: \[], 2: \[], 3: \[]} 같은 데이터 구조이기에 다음의 기능으로 상태를 가져온다.

Object.keys(obj)

객체의 속성 key들을 반복문과 동일한 순서로 순회되는 배열로 반환한다.

.map을 이용해 다시 여러 개의 엘리먼트로 구분한다.

***

### #6.9 **Same Board Movement**

같은 Board에서 드래그할때…

state 구조가 변경되었기에 그 변화에 맞춰서 state 값을 수정한다.

하나의 상태 그룹 내에서 진행됨

***

### #6.10 **Cross Board Movement**

다른 Board에서 드래그할때…

그룹 값의 변화에 주의하면서 같은 Board 변경과 같은 방법으로 state 값을 수정한다.

***

### #6.11 **Droppable Snapshot**

드래그할 때 마지막 요소가 사라지면 높이가 사라지면서 이동에 제약이 생긴다.

display flex로 쉽게 제약을 없앨 수 있음

& DroppableStateSnapshot 드랍이 발생하는 장소(Droppable)에서 다양한 정보를 얻을 수 있음

isDraggingOver: boolean

현재 선택한 Draggable이 특정 Droppable 위에 드래깅 되는지 여부

draggingOverWith: ?DraggabledId

Droppable 위로 드래그하는 Draggable ID

draggingFromThisWith: ?DraggableId

현재 Droppable에서 벗어난 드래깅되고 있는 Draggable ID

isUsingPlaceholder: boolean

placeholder가 사용되고 있는지 여부

***

### #6.12 **Final Styles**

isDragging - boolean

Draggable이 실행되는지 여부

***

### #6.13 Refs

Refs - React 코드를 이용해 HTML 요소를 지정하고, 가져오는 방법

beautiful-dnd 가 React 요소에 접근할 수 있도록 도와줌

const \~\~ = useRef()

***

### #6.14 **Task Objects**

toDo 추가하는 기능추가

atoms에서 state data를 정비해야함

⇒ draggableId 값을 추가적으로 제공해야함

&

React Hook Form

```tsx
...
const {register, handleSubmit, setValue, getValue} = useForm<IForm>
...
```

***

### #6.15 **Creating Tasks**

id 값을 고유한 data로 제공(Date.now())하면서 오류 해결

***
