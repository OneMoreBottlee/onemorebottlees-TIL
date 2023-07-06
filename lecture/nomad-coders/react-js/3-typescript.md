# #3 TypeScript

### #3.0 Introduction

TypeScript\
ㄴ JS 기반의 언어, 거의 비슷함\
ㄴ Strongly **Typed** Programming Language

```jsx
const plus = (a, b) => a + b;
const a = 1;
const b = 'hi';
console.log(plus(a,b)) // 1hi;
```

\=> JS는 타입을 고려하지 않음 ⇒ 작은 실수들이 발생해도 그대로 진행됨

⇒ TypeScript 탄생 ( 타입을 지정해 코드 실행 **전** 약간의 보호 작용 )

```jsx
const plus = (a:number, b:number) => a + b;
const a = 1;
const b = 'hi';
console.log(plus(a+b)) // error !!
```

[TS Playground - An online editor for exploring TypeScript and JavaScript](https://www.typescriptlang.org/play?#code/PTAEHUFMBsGMHsC2lQBd5oBYoCoE8AHSAZVgCcBLA1UABWgEM8BzM+AVwDsATAGiwoBnUENANQAd0gAjQRVSQAUCEmYKsTKGYUAbpGF4OY0BoadYKdJMoL+gzAzIoz3UNEiPOofEVKVqAHSKymAAmkYI7NCuqGqcANag8ABmIjQUXrFOKBJMggBcISGgoAC0oACCbvCwDKgU8JkY7p7ehCTkVDQS2E6gnPCxGcwmZqDSTgzxxWWVoASMFmgYkAAeRJTInN3ymj4d-jSCeNsMq-wuoPaOltigAKoASgAywhK7SbGQZIIz5VWCFzSeCrZagNYbChbHaxUDcCjJZLfSDbExIAgUdxkUBIursJzCFJtXydajBBCcQQ0MwAUVWDEQC0gADVHBQGNJ3KAALygABEAAkYNAMOB4GRonzFBTBPB3AERcwABS0+mM9ysygc9wASmCKhwzQ8ZC8iHFzmB7BoXzcZmY7AYzEg-Fg0HUiQ58D0Ii8fLpDKZgj5SWxfPADlQAHJhAA5SASPlBFQAeS+ZHegmdWkgR1QjgUrmkeFATjNOmGWH0KAQiGhwkuNok4uiIgMHGxCyYrA4PCCJSAA)



### #3.1 DefinitelyTyped

REACT에 타입 스크립트 설치

1.  전부 지우고 새로 만들기

    ```jsx
    npm create-react-app my-app --template typescript
    // or
    yarn create react-app my-app --template typescript
    ```
2.  모든 타입 스크립트 설치

    ```jsx
    npm install --save typescript @types/node @types/react @types/react-dom @types/jest @types/style-components // ... 필요한 스크립트 추가
    // or
    yarn add typescript @types/node @types/react @types/react-dom @types/jest @types/style-components
    ```



### #3.2 Typing the Props

타입 스크립트에 Props 설정하기

interface object의 shape를 설명

```jsx
interface DummyProps{
	text: string;
}

function Dummy({ text } : DummyProps){  // text의 타입은 DummyProps에서 찾아라
	return <H1>{text}</H1>
}
```



### #3.3 Optional Props

모든 Props를 필수 상태로 놓을 필요가 없다.

Optional Props를 설정하고 싶다면 interface 내부 props명 뒤에 ?를 추가한다.

```jsx
interface CircleProps{
	bgColor: string; // 필수
	borderColor?: string; // 옵션
}
```

필수인 경우 사용하지 않는다면 초깃값을 부여할 수 있다.

```jsx
1. props에 초깃값 주기
...
borderColor={borderColor ?? "white"}
...

2. function의 매개변수에 default 값 추가
function Circle({ text= "default text"}){
...
}
```



### #3.4 State

기본적인 사용법은 같다.

초깃값의 타입과 다르면 에러가 발생함 & 여러개의 타입을 설정할 수 있음

초깃값을 주지 않으면 undefined

```jsx
const [value, setValue] = useState(0); // 초깃값의 타입에 따라 value의 타입도 정해짐
setValue("hi") // error
setValue(true) // error
setValue(3) // value === 3

//// 만약 타입을 여러개 설정하고 싶다면
const [value, setValue] = useState<number|string>(0);
// value의 타입은 number나 string임 !
```



### #3.5 Forms

```jsx
import React, { useState } from "react";

function App() {
  const [value, setValue] = useState("");
  const onChange = (event: React.FormEvent<HTMLInputElement>) => {
    const {
      currentTarget: { value },
    } = event;
    setValue(value);
  };
  const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log("hello", value);
  };

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input value={value} onChange={onChange} type="text" placeholder="username" />
        <button>Log In</button>
      </form>
    </div>
  );
}

export default App;
```

event 함수 부분에서 event에 타입을 설정할 수 있음



### #3.6 Themes

styled.d.ts 파일 (d.ts ⇒ declaration file)

theme.ts 파일에 원하는 테마 설정

index.tsx에서 테마를 import

app.tsx에서 props로 받아 사용한다.



### #3.7 Recap

TypeScript 에 없는 라이브러리를 사용한다면 npm i —save-dev @types/styled-components 와 같이 타입스크립트에 그 라이브러리가 뭔지 알려주는 install을 수행해야한다.

SyntheticEvent (합성 이벤트)

이벤트 핸들러는 모든 브라우저에서 동일하게 이벤트를 처리하기 위한 Syntetic Event 객체를 전달받는다.

Keyboard Events ex) onKeyDown onKeyPress onKeyUp

Focus Events ex) onFocus onBlur

Form Events ex) onChange onInput onInvalid onReset onSubmit

Generic Events ex) onError onLoad

etc..

[SyntheticEvent - React](https://reactjs.org/docs/events.html)

