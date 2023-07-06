# #1 PURE REDUX: COUNTER

### #1.0 **Vanilla Counter**

Vanilla JS

1씩 더하고 빼는 기능 구현



### #1.1 **Store and Reducer**

**store** = state 저장하는 곳

**state** = 어플리케이션에서 변하는 data

**createStore()** = reducer 역할을 할 함수를 인자로 요구함.

**reducer** = data를 modify하는 함수. reducer가 return하는 것은 application에 있는 data가 된다. (=> state가 변경된다)

```jsx
import { createStore } from "redux";

const reducer = (state) => { // 여기의 state는 초깃값
	return abcdfefef
}

const store = createStore(reducer) // reducer에 store 역할 부여

//reducer의 return 값이 state가 됨
```



### #1.2 **Actions**

**Action** - reducer 함수의 두번째 파라미터

dispatch()를 통해 {type: value}를 전송할 수 있다.

Action은 반드시 **object**, **type**이 포함되어야 함

```jsx
const reducer = (state = 0, action) => { // reducer 함수의 두번째 파라미터
  if(action.type === "ADD"){
    return count +1;
  } else if( action.type === "MINUS"){
    return count -1;
  }else{
    return count;
  }
}

const store = createStore(reducer);

reducer.dispatch({type: "ADD"}) //{type: "ADD"} ACTION을 reducer로 전송함 count = count + 1 => 0 + 1 = 1
reducer.dispatch({type: "MINUS"}) //count = count-1 => 1 - 1 =0

console.log(countStore.getState()) // 0 > 1 > 0
```



### #1.3 **Subscriptions**

**Subscribe** - store 내부의 변화 감지

store.subscribe(func) > store 내부의 변화를 감지하면 func 실행

```jsx
import { createStore } from "redux";

const add = document.getElementById("add")
const minus = document.getElementById("minus")
const number = document.querySelector("span")

number.innerText = 0

const reducer = (state = Number(number.innerText), action) => {
  if(action.type === "ADD"){
    return state +1;
  } else if( action.type === "MINUS"){
    return state -1;
  }else{
    return state;
  }
}

const store = createStore(reducer);

const onChange = () => {
  number.innerText = store.getState()
}

store.subscribe(onChange) // onchange

const handleAdd = () => {       // 1번
  store.dispatch({type: "ADD"})
}

const handleMinus = () => {     // 2번
  store.dispatch({type: "MINUS"})
}

add.addEventListener("click", () => store.dispatch({type: "ADD"}))     // 1번
minus.addEventListener("click", () => store.dispatch({type: "MINUS"})) // 2번
```



### #1.4 **Recap Refactor**

코드 수정

1. 문자열을 변수로 선언 문자열을 사용하면 오류를 발견하기 어려움 ADD / MINUS를 변수로 선언해서 사용함
2. if문을 switch로 변경 if문에 비해 확인하기 편함

```jsx
import { createStore } from "redux";

const add = document.getElementById("add")
const minus = document.getElementById("minus")
const number = document.querySelector("span")

number.innerText = 0;

const ADD = "ADD"; // 1
const MINUS = "MINUSE";

const countModifier = (count = Number(number.innerText), action) => {
  switch (action.type) { // 2
    case ADD:
      return count +1;
    case MINUS:
      return count -1;
    default:
      return count;
  }
}

const countStore = createStore(countModifier);

const onChange = () => {
  number.innerText = countStore.getState()
}

countStore.subscribe(onChange)

const handleAdd = () => {
  countStore.dispatch({type: ADD})
}

const handleMinus = () => {
  countStore.dispatch({type: MINUS})
}

add.addEventListener("click", handleAdd)
minus.addEventListener("click", handleMinus)
```

