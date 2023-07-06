# #3 REACT REDUX

### #3.0 Setup

REACT로 기본 세팅

react, react router dom 설치 & 세팅



### #3.1 **Connecting the Store**

전에 진행한 store, reducer, dispatcher, action 내용 추가

react로 연결하는 과정에서 Provider 사용

*   코드

    ```jsx
    // index.js
    import React from "react";
    import ReactDOM from "react-dom";
    import { Provider } from "react-redux";
    import App from "./components/App";
    import store from "./store"

    const root = ReactDOM.createRoot(document.getElementById("root"))

    root.render(
        <Provider store={store}>
            <App />
        </Provider>
    )
    ```



### #3.2 **mapStateToProps**

Connect & mapStateToProps를 사용해 state를 컴포넌트에 연결하는 과정

connect - return 내용을 컴포넌트의 prop에 추가한다.

mapStateToProps(state, props)

(\* Hook - useSelector()로 state를 가져올 수 있음) [참고](https://www.samdawson.dev/article/react-redux-use-selector-vs-connect)

mapStateToProps = useSelector = getState state를 가져오는 역할

*   코드

    ```jsx
    // Home.js
    import React, { useState } from "react";
    import { connect } from "react-redux";

    function Home({ toDos }) {
      const [text, setText] = useState("");
      function onChange(e) {
        setText(e.target.value);
      }

      function onSubmit(e) {
        e.preventDefault();
        setText("");
      }

      return (
        <>
          <h1>To Do</h1>
          <form onSubmit={onSubmit}>
            <input type="text" value={text} onChange={onChange} />
            <button>ADD</button>
          </form>
          <ul>{JSON.stringify(toDos)}</ul>
        </>
      );
    }

    function mapStateToProps(state) {
      return { toDos: state };
    }

    export default connect(mapStateToProps)(Home);
    ```



### #3.3 **mapDispatchToProps**

mapDispatchToProps을 사용해 dispatch 기능을 수행한다.

but 최근에는 connect와 mapStateToProps, mapDispatchToProps를 사용하지 않고

Hooks를 사용해 useDispatch, useSelector를 사용한다.

*   코드

    ```jsx
    // Home.js
    import React, { useState } from "react";
    import { connect } from "react-redux";
    import { actionCreators } from "../store";

    function Home({ toDos, addToDo }) {
      const [text, setText] = useState("");
      function onChange(e) {
        setText(e.target.value);
      }

      function onSubmit(e) {
        e.preventDefault();
        addToDo(text);
        setText("");
      }

      return (
        <>
          <h1>To Do</h1>
          <form onSubmit={onSubmit}>
            <input type="text" value={text} onChange={onChange} />
            <button>ADD</button>
          </form>
          <ul>{JSON.stringify(toDos)}</ul>
        </>
      );
    }

    function mapStateToProps(state) {
      return { toDos: state };
    }

    function mapDispatchToProps(dispatch) {
      return {
        addToDo: (text) => dispatch(actionCreators.addToDo(text))
      };
    }

    export default connect(mapStateToProps, mapDispatchToProps)(Home);
    ```
*   수정 코드

    ```jsx
    // Home.js
    import React, { useState } from "react";
    import { useDispatch, useSelector } from "react-redux";
    import { addToDo } from "../store"

    function Home() {
      const [text, setText] = useState("");
      const toDos = useSelector((state)=>state)
      const dispatch = useDispatch()

      function onChange(e) {
        setText(e.target.value);
      }

      function onSubmit(e) {
        e.preventDefault();
        dispatch(addToDo(text))
        setText("");
      }

      return (
        <>
          <h1>To Do</h1>
          <form onSubmit={onSubmit}>
            <input type="text" value={text} onChange={onChange} />
            <button>ADD</button>
          </form>
          <ul>{JSON.stringify(toDos)}</ul>
        </>
      );
    }

    export default Home;
    ```



### #3.4 **Deleting To Do**

onClick 함수를 만들어 deleteToDo함수를 디스패치 할 수 있도록 만들었다.

강의 내용이 아닌 useDispatch를 사용해 생각보다 시간이 더 소요됐다.

그래도 막히는 부분을 찾아가면서 해결하니 성장한 기분이 든다.

*   코드

    ```jsx
    // Home.js
    import React, { useState } from "react";
    import { useDispatch, useSelector } from "react-redux";
    import ToDo from "../components/ToDo";
    import { addToDo, deleteToDo } from "../store"

    function Home() {
      const [text, setText] = useState("");
      const toDos = useSelector((state)=>state)

      const dispatch = useDispatch()

      function onChange(e) {
        setText(e.target.value);
      }

      function onSubmit(e) {
        e.preventDefault();
        dispatch(addToDo(text))
        setText("");
      }

      const onClick = (e) => {
        dispatch(deleteToDo(e))
      }

      return (
        <>
          <h1>To Do</h1>
          <form onSubmit={onSubmit}>
            <input type="text" value={text} onChange={onChange} />
            <button>ADD</button>
          </form>
          <ul>
            {toDos.map(toDo => (
                <ToDo {...toDo} id={toDo.id} onClick={onClick} key={toDo.id}  />
            ))}
          </ul>
        </>
      );
    }

    export default Home;

    // ToDo.js
    import React from "react";

    function ToDo({ text, id, onClick }) {
      return (
        <li>
          {text}
          <button id={id}onClick={() => onClick(id)}>❌</button>
        </li>
      );
    }

    export default ToDo;
    ```



### #3.5 **Detail Screen**

(+ 추가 개념) /id Detail 페이지에서 상태 확인하기

*   코드

    ```jsx
    //Detail.js
    import React from "react";
    import {useSelector} from "react-redux"
    import { useParams } from "react-router-dom";

    function Detail() {
        const toDos = useSelector((state) => state);
        const doId = useParams().id;
        const toDoText=toDos.find((todo)=>todo.id === parseInt(doId))

      return (
        <>
        <div>{toDoText?.text}</div>
        <div>User ID:{toDoText?.id}</div>
        </>
      )
    }

    export default Detail;
    ```



### #3.6 Introduction

**+ LocalStorage에 저장하기 구현**

store.js - persistReducer, combineReducers

index.js - PersistGate

[참고](https://haragoo30.medium.com/redux-persist%EB%A5%BC-%EC%86%8C%EA%B0%9C%ED%95%A9%EB%8B%88%EB%8B%A4-94cb7c8d7020) [참고2](https://kyounghwan01.github.io/blog/React/redux/redux-persist/#reducer%E1%84%8B%E1%85%A6-persist-store-%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B4) [참고3](https://www.npmjs.com/package/redux-persist) [다른사람완성본](https://githubgw.github.io/react-redux/#/) - 나도 CSS까지 적용해보자 !

*   코드

    ```jsx
    // store.js
    import { combineReducers, createStore } from "redux";
    import storage from "redux-persist/lib/storage";
    import { persistReducer } from "redux-persist";

    const persistConfig = {
    key:"todo", //localStorage에 저장될 key값
    storage:storage
    };

    const ADD = "ADD";
    const DELETE = "DELETE";

    export const addToDo = (text) => {
      return {
        type: ADD,
        text,
      };
    };

    export const deleteToDo = (id) => {
      return {
        type: DELETE,
        id,
      };
    };

    const reducer = (state = [], action) => {
      switch (action.type) {
        case ADD:
          return [{ text: action.text, id: state.length }, ...state];
        case DELETE:
          return state.filter((toDo) => toDo.id !== action.id);
        default:
          return state;
      }
    };

    const allReducer = combineReducers({
        reducer
        });

    const store = createStore(persistReducer(persistConfig, allReducer));

    export default store;

    /////////////////////////////////////

    // index.js
    import React from "react";
    import { createRoot } from "react-dom/client";
    import { Provider } from "react-redux";
    import App from "./components/App";
    import store from "./store";
    import { persistStore } from "redux-persist";
    import { PersistGate } from "redux-persist/integration/react";

    const rootElement = document.getElementById("root");
    const root = createRoot(rootElement);
    const persistor = persistStore(store);

    root.render(
      <Provider store={store}>
        <PersistGate loading={null} persistor={persistor}>
          <App />
        </PersistGate>
      </Provider>
    );
    ```

