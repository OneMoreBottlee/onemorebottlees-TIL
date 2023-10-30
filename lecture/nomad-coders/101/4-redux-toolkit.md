# #4 Redux Toolkit

### #4.0 **Redux Toolkit**

리덕스 툴킷 - 효율적으로 리덕스 코드를 작성할 수 있도록 도와줌

npm install @reduxjs/toolkit

***

### #4.1 **createAction**

createAction 메소드를 사용해 ACTION 부분의 코드를 간결하게 작성한다.

함수를 만들고, 문자열로 전환해주는 작업없이 createAction에서 type값을 부여하고, 부여받은 type값을 reducer에서 비교하면서 action.payload로 원하는 작업을 진행함

확실히 이전 코드보다 간결해졌고 깔끔해보인다.

(reducer 내부의 리턴을 함수로 정리하면 더 깔끔해질듯?)

*   코드

    ```jsx
    // store.js
    import { combineReducers, createStore } from "redux";
    import storage from "redux-persist/lib/storage";
    import { persistReducer } from "redux-persist";
    import { createAction } from "@reduxjs/toolkit";

    const persistConfig = {
      key: "todo", //localStorage에 저장될 key값
      storage: storage,
    };

    export const addToDo = createAction("ADD");
    export const deleteToDo = createAction("DELETE");

    const reducer = (state = [], action) => {
      switch (action.type) {
        case addToDo.type:
          return [{ text: action.payload, id: state.length }, ...state];
        case deleteToDo.type:
          return state.filter((toDo) => toDo.id !== action.payload);
        default:
          return state;
      }
    };

    const allReducer = combineReducers({
      reducer,
    });

    const store = createStore(persistReducer(persistConfig, allReducer));

    export default store;
    ```

***

### #4.2 **createReducer**

createReducer 메서드를 사용, reducer 내용을 간결하게 작성한다.

1. No Switch !
2. No Case !
3. Can use **state mutate** ! toolkit이 Immer 아래서 작동하기에 state를 수정하는 것이 가능하다. 새로운 state를 사용할 때는 return으로, 기존의 state 수정은 mutate를 사용할 수 있다.

*   코드

    ```jsx
    // store.js
    import { combineReducers, createStore } from "redux";
    import storage from "redux-persist/lib/storage";
    import { persistReducer } from "redux-persist";
    import { createAction, createReducer } from "@reduxjs/toolkit";

    const persistConfig = {
      key: "todo", //localStorage에 저장될 key값
      storage: storage,
    };

    export const addToDo = createAction("ADD");
    export const deleteToDo = createAction("DELETE");

    const reducer = createReducer([], {
      [addToDo]: (state, action) => {
        state.unshift({text:action.payload, id: state.length})
      },
      [deleteToDo]: (state, action) => 
        state.filter((toDo)=> toDo.id !== action.payload)
    })

    const allReducer = combineReducers({
      reducer,
    });

    const store = createStore(persistReducer(persistConfig, allReducer));

    export default store;
    ```

***

### #4.3 **configureStore**

configureStore 메서드를 사용, createStore 기능을 대신한다.

reducer를 객체 형식으로 전달해 만들 수 있다.

크롬 확장 프로그램인 Redux DevTools

(로컬 스토리지 용으로 작성한 코드때문에 어떻게 수정할지 모르겠다)

[참고](https://www.notion.so/7822efed8f414237b709fbddc246303e?pvs=21) [참고2](https://redux-toolkit.js.org/api/configureStore)

***

### #4.4 **createSlice**

createSlice 메서드를 이용해, reducer와 action을 자동으로 생성한다.

*   코드

    ```jsx
    // store.js
    import { combineReducers, createStore } from "redux";
    import storage from "redux-persist/lib/storage";
    import { persistReducer } from "redux-persist";
    import { createAction, createReducer } from "@reduxjs/toolkit";

    const persistConfig = {
      key: "todo", //localStorage에 저장될 key값
      storage: storage,
    };

    export const addToDo = createAction("ADD");
    export const deleteToDo = createAction("DELETE");

    const reducer = createReducer([], {
      [addToDo]: (state, action) => {
        state.unshift({text:action.payload, id: state.length})
      },
      [deleteToDo]: (state, action) => 
        state.filter((toDo)=> toDo.id !== action.payload)
    })

    const allReducer = combineReducers({
      reducer,
    });

    const store = createStore(persistReducer(persistConfig, allReducer));

    export default store;
    ```

***

### #4.5 **Conclusions**

\#3과 #4은 같은 기능을 작동한다.

하지만 #4의 코드가 훨씬 간결해졌다.

Recux toolkit의 가장 큰 장점이다.

로컬스토리지를 작동하는 방법을 찾고 있는데 어렵다…

*   코드

    ```jsx
    // store.js
    import { configureStore, createSlice } from "@reduxjs/toolkit";

    const toDos = createSlice({
      name: 'toDosReducer',
      initialState: [],
      reducers: {
        add: (state, action) => {
          state.unshift({text:action.payload, id: Date.now()})},
        remove: (state, action) => state.filter(toDo=> toDo.id !== action.payload)
      }
    })

    export const {add, remove} = toDos.actions;

    export default configureStore({ reducer: toDos.reducer});
    ```

***
