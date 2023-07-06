# #2 PURE REDUX: TO DO LIST

### #2.0 **Vanilla ToDo**

Vanilla JS > To Do 리스트를 작성, 하나씩 추가하는 기능

**Redux화**

state 설정, 간단한 reducer 작성(작동X), dispatcher에 연결까지 확인

*   코드

    ```jsx
    import { createStore } from "redux";

    const form = document.querySelector("form");
    const input = document.querySelector("input");
    const ul = document.querySelector("ul");

    const ADD_TODO = "ADD_TODO";
    const DELETE_TODO = "DELETE_TODO";

    const reducer = (state = [], action) => {
      switch (action.type) {
        case ADD_TODO:
          return [];

        case DELETE_TODO:
          return [];

        default:
          return state;
      }
    };

    const store = createStore(reducer);

    const onSubmit = (e) => {
      e.preventDefault();
      const toDo = input.value;
      input.value = "";
      store.dispatch({ type: ADD_TODO, text: toDo });
    };

    form.addEventListener("submit", onSubmit);
    ```



### #2.1 **State Mutation**

ADD 버튼 클릭시 state 값을 추가할 수 있게 수정 > mutate가 아닌 New state를 리턴하는 방식으로

> state는 single source of truth 이고, read-only 이기에 절대 mutate하는 방식으로 수정하면 안된다. 새로운 Object를 리턴해야함

*   코드

    ```jsx
    import { createStore } from "redux";

    const form = document.querySelector("form");
    const input = document.querySelector("input");
    const ul = document.querySelector("ul");

    const ADD_TODO = "ADD_TODO";
    const DELETE_TODO = "DELETE_TODO";

    const reducer = (state = [], action) => {
      switch (action.type) {
        case ADD_TODO:
          return [...state, {text: action.text, id: Date.now()}]; //ES6 SPREAD

        case DELETE_TODO:
          return [];

        default:
          return state;
      }
    };

    const store = createStore(reducer);

    store.subscribe(() => console.log(store.getState()))

    const onSubmit = (e) => {
      e.preventDefault();
      const toDo = input.value;
      input.value = "";
      store.dispatch({ type: ADD_TODO, text: toDo });
    };

    form.addEventListener("submit", onSubmit);
    ```



### #2.2 **Delete To Do**

ADD 버튼 클릭시 리스트 추가 & DEL 버튼 추가된 상태

아직 DEL 구현이 안되서 전체가 삭제됨

*   코드

    ```jsx
    import { createStore } from "redux";

    const form = document.querySelector("form");
    const input = document.querySelector("input");
    const ul = document.querySelector("ul");

    const ADD_TODO = "ADD_TODO";
    const DELETE_TODO = "DELETE_TODO";

    const addToDo = (text) => {
      return {
        type: ADD_TODO,
        text,
      };
    };

    const deleteToDo = (id) => {
      return {
        type: DELETE_TODO,
        id,
      };
    };

    const reducer = (state = [], action) => {
      switch (action.type) {
        case ADD_TODO:
          return [{ text: action.text, id: Date.now() }, ...state]; //ES6 SPREAD

        case DELETE_TODO:
          return [];

        default:
          return state;
      }
    };

    const store = createStore(reducer);

    store.subscribe(() => console.log(store.getState()));

    const dispatchAddToDo = (text) => {
      store.dispatch(addToDo());
    };

    const dispatchDeleteToDo = (e) => {
      const id = e.target.parentNode.id;
      store.dispatch(deleteToDo(id));
    };

    const paintToDos = () => {
      const toDos = store.getState();
      ul.innerHTML = "";
      toDos.forEach((toDo) => {
        const li = document.createElement("li");
        const btn = document.createElement("button");
        btn.innerHTML = "DEL";
        btn.addEventListener("click", dispatchDeleteToDo);
        li.id = toDo.id;
        li.innerText = toDo.text;
        li.appendChild(btn);
        ul.appendChild(li);
      });
    };

    store.subscribe(paintToDos);

    const onSubmit = (e) => {
      e.preventDefault();
      const toDo = input.value;
      input.value = "";
      dispatchAddToDo(toDo);
    };

    form.addEventListener("submit", onSubmit);
    ```



### #2.3 Delete To Do part Two

state를 filter로 정리해 Delete 구현

Do NOT Mutate ! return NEW STATE !!

*   완성 코드

    ```jsx
    import { createStore } from "redux";

    const form = document.querySelector("form");
    const input = document.querySelector("input");
    const ul = document.querySelector("ul");

    const ADD_TODO = "ADD_TODO";
    const DELETE_TODO = "DELETE_TODO";

    const addToDo = (text) => {
      return {
        type: ADD_TODO,
        text,
      };
    };

    const deleteToDo = (id) => {
      return {
        type: DELETE_TODO,
        id,
      };
    };

    const reducer = (state = [], action) => {
      switch (action.type) {
        case ADD_TODO:
          return [{ text: action.text, id: Date.now() }, ...state]; //ES6 SPREAD

        case DELETE_TODO:   // mutate가 되면 안되기에 slice는 사용하지 말자
          return state.filter((toDo) => toDo.id !== action.id);

        default:
          return state;
      }
    };

    const store = createStore(reducer);

    store.subscribe(() => console.log(store.getState()));

    const dispatchAddToDo = (text) => {
      store.dispatch(addToDo());
    };

    const dispatchDeleteToDo = (e) => {
      const id = parseInt(e.target.parentNode.id);
      store.dispatch(deleteToDo(id));
    };

    const paintToDos = () => {
      const toDos = store.getState();
      ul.innerHTML = "";
      toDos.forEach((toDo) => {
        const li = document.createElement("li");
        const btn = document.createElement("button");
        btn.innerHTML = "DEL";
        btn.addEventListener("click", dispatchDeleteToDo);
        li.id = toDo.id;
        li.innerText = toDo.text;
        li.appendChild(btn);
        ul.appendChild(li);
      });
    };

    store.subscribe(paintToDos);

    const onSubmit = (e) => {
      e.preventDefault();
      const toDo = input.value;
      input.value = "";
      dispatchAddToDo(toDo);
    };

    form.addEventListener("submit", onSubmit);
    ```



### #2.4 **Conclusions**

1. **fuction**을 사용해 redux 환경을 구성한다. action을 dispatch하는 fn reducer에 보낼 object를 return하는 fn +a
2. **Do Not Mutate !! Return New State !!!** 코드스테이츠에서 학습한 내용과 가장 큰 차이를 보이는 구간이다. 강의 내내 이 부분을 강조하며 기존의 상태를 변경하는 것이 아닌 새로운 상태를 선언하는 것임을 각인시켰다.

