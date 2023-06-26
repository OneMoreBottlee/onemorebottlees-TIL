---
description: https://github.com/OneMoreBottlee/TypeScript-Master/tree/main/S11
---

# \[S11] 미니 프로젝트 DOM, 타입 단언, 그리고 더 많은 내용

TS는 문서 객체의 타입을 자동으로 확인한다.

라이브러리 타입 선언 파일은 파일명.d.ts



**non-null 단언 연산자**

값이 확실하게 존재할때 ! 를 활용해 단언할 수 있다. TS 고유의 연산자임



**Type Assertions 타입 단언**

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

TS에 선언하는 것

as



**EVENT 다루기**

<figure><img src="../../../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>



**간단 TODO 만들기**

```tsx
interface Todo {
  text: string;
  completed: boolean;
}

const btn = document.getElementById("btn")!;
const input = document.getElementById("todoinput")! as HTMLInputElement;
const form = document.querySelector("form")!;
const list = document.getElementById("todolist");

const todos: Todo[] = readTodos();
todos.forEach(createTodo);

function readTodos(): Todo[] {
  const todosJSON = localStorage.getItem("todos");
  if (todosJSON === null) return [];
  return JSON.parse(todosJSON);
}

function saveTodos() {
  localStorage.setItem("todos", JSON.stringify(todos));
}

function handleSubmit(e: SubmitEvent) {
  e.preventDefault();
  const newTodo: Todo = {
    text: input.value,
    completed: false,
  };
  createTodo(newTodo);
  todos.push(newTodo);
  saveTodos();

  localStorage.setItem("todos", JSON.stringify(todos));

  input.value = "";
}

function createTodo(todo: Todo) {
  const newLI = document.createElement("lI");
  const checkbox = document.createElement("input");
  checkbox.type = "checkbox";
  checkbox.checked = todo.completed;
  checkbox.addEventListener("change", function () {
    todo.completed = checkbox.checked;
    saveTodos();
  });

  newLI.append(todo.text);
  newLI.append(checkbox);
  list?.append(newLI);
}

form.addEventListener("submit", handleSubmit);
```
