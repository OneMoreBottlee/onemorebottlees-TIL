# #2 Styled Componnents

### #2.0 Why Styled Components

### #2.1 Our First Styled Components

### #2.2 Adapting and Extending

### #2.3 ‘As’ and Attrs

### #2.4 Animations and Pseudo Selectors

```jsx
import styled, { keyframes } from "styled-components";

const Wrapper = styled.div`
  display: flex;
`;

const rotationAnimation = keyframes`
  0%{
    transform: rotate(0deg);
    border-radius:0px;
  }
  50%{
    transform: rotate(360deg);
    border-radius:100px;
  }
  100%{
    transform: rotate(0deg);
    border-radius:0px;
  }
`;

const Box = styled.div`
  height: 200px;
  width: 200px;
  background-color: tomato;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: ${rotationAnimation} 1s linear infinite;
  span{
    font-size:  50px;
    &:hover{
      font-size:  80px;
    }
    &:active{
      opacity: 0;
    }
  }
`;

function App() {
  return (
    <Wrapper>
      <Box>
        <span>☺️</span>
      </Box>
    </Wrapper>
  );
}

export default App;
```

***

### #2.5 Pseudo Selectors part Two

### #2.6 Super Recap

### #2.7 Themes

dark mode 만들기 50%

같은 이름의 property를 설정해야 한다.

***
