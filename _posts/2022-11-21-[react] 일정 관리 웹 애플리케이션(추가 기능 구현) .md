---
categories: react
---

## App에서 todos 상태 사용하기

### App.js 에서 상태 정의

```jsx
// App.js

const App = () => {
  const [todos, setTodos] = useState([
    { id: 1, text: "리액트 공부하기", checked: true },
    { id: 2, text: "자바스크립트 공부하기", checked: true },
    { id: 3, text: "장고 공부하기", checked: false },
  ]);
  return (
    <TodoTemplate>
      <TodoInsert />
      <TodoList todos={todos} />
    </TodoTemplate>
  );
};
export default App;
```

useState를 사용하여 todos라는 상태 정의

todos를 TodoList의 props로 전달

### props를 TodoList.js에서 받기

```jsx
// TodoList.js

import React from "react";
import TodoListItem from "./TodoListItem";
import "./TodoList.scss";

const TodoList = ({ todos }) => {
  return (
    <div className="TodoList">
      {todos.map((todo) => (
        <TodoListItem todo={todo} key={todo.id} />
      ))}
    </div>
  );
};

export default TodoList;
```

props로 받아 온 todos 배열을 내장 함수 map을 통해 TodoListItem으로 이루어진 배열로 변환하여 렌더링.

todo 데이터는 통째로 props로 전달.

여러 종류의 값을 전달하는 경우에는 객체로 통째로 전달하는 것이 성능 최적화에 편리

### props를 TodoListItem.js에서 받기

```jsx
// TodoListItem.js

import React from "react";
import {
  MdCheckBoxOutlineBlank,
  MdCheckBox,
  MdRemoveCircleOutline,
} from "react-icons/md";
import "./TodoListItem.scss";
import cn from "classnames";

const TodoListItem = ({ todo }) => {
  const { text, checked } = todo;
  return (
    <div className="TodoListItem">
      <div className={cn("checkbox", { checked })}>
        {checked ? <MdCheckBox /> : <MdCheckBoxOutlineBlank />}
        <div className="text">{text}</div>
      </div>
      <div className="remove">
        <MdRemoveCircleOutline />
      </div>
    </div>
  );
};

export default TodoListItem;
```

조건부 스타일리을 위해 classnames 사용

TodoListItem 컴포넌트에서 받아 온 todo 값에 따라 제대로 된 UI를 보여 줄 수 있도록 수정

## 항목 추가 기능 구현

TodoInsert 컴포넌트에서 인풋 상태를 관리

App 컴포넌트에서 todos 배열에 새로운 객체를 추가하는 함수를 생성

### TodoInsert value 상태관리

```jsx
import React, { useCallback, useState } from "react";
import { MdAdd } from "react-icons/md";
import "./TodoInsert.scss";

const TodoInsert = ({ onInsert }) => {
  const [value, setValue] = useState("");
  const onChange = useCallback((e) => {
    setValue(e.target.value);
  }, []);

  const onSubmit = useCallback(
    (e) => {
      onInsert(value);
      setValue("");
      e.preventDefault();
    },
    [onInsert, value]
  );

  return (
    <form className="TodoInsert" onSubmit={onSubmit}>
      <input
        placeholder="할 일을 입력하세요"
        value={value}
        onChange={onChange}
      ></input>
      <button type="submit">
        <MdAdd />
      </button>
    </form>
  );
};

export default TodoInsert;
```

컴포넌트가 리렌더링될 때마다 함수를 새로 만드는 것이 아닌,

한 번 함수를 만들고 재사용할 수 있도록 useCallback Hook 사용

useCallback(이벤트 핸들러 함수를 필요할 때만 생성)

리액트 컴포넌트 쪽에서 해당 인풋에 무엇이 입력되어있는지 추적 하기 위해서 value, onChange 설정

### todos 배열에 새 객체 추가하기

```jsx
import React, { useCallback, useRef, useState } from "react";
import TodoTemplate from "./components/TodoTemplate";
import TodoInsert from "./components/TodoInsert";
import TodoList from "./components/TodoList";

const App = () => {
  const [todos, setTodos] = useState([
    { id: 1, text: "리액트 공부하기", checked: true },
    { id: 2, text: "자바스크립트 공부하기", checked: true },
    { id: 3, text: "장고 공부하기", checked: false },
  ]);

  const nextId = useRef(4);

  const onInsert = useCallback(
    (text) => {
      const todo = {
        id: nextId.current,
        text,
        checked: false,
      };
      setTodos(todos.concat(todo));
      nextId.current += 1;
    },
    [todos]
  );
  return (
    <TodoTemplate>
      <TodoInsert onInsert={onInsert} />
      <TodoList todos={todos} />
    </TodoTemplate>
  );
};
export default App;
```

todos 배열에 새 객체를 추가하는 onInsert 함수 생성

useRef를 사용하여 id 값 관리

useState가 아닌 useRef를 사용하는 이유는 id 값은 레더링되는 정보가 아니기 때문.

화면에 보이지 않아도 되고, 값이 바뀐다고 해서 컴포넌트가 리렌더링될 필요가 없기 때문에

단순히 참조되는 값이기 때문에 useRef 사용

onInsert 함수는 컴포넌트의 성능을 아낄 수 있도록 useCallback으로 감싸줌.

props로 전달해야 할 함수를 만들 때는 useCallback을 사용하여 함수를 감싸주는 것을 습관화하자.

### TodoInsert에서 onSubmit 이벤트 설정하기

```jsx
import React, { useCallback, useState } from "react";
import { MdAdd } from "react-icons/md";
import "./TodoInsert.scss";

const TodoInsert = ({ onInsert }) => {
  const [value, setValue] = useState("");
  const onChange = useCallback((e) => {
    setValue(e.target.value);
  }, []);

  const onSubmit = useCallback(
    (e) => {
      onInsert(value);
      setValue("");
      e.preventDefault();
    },
    [onInsert, value]
  );

  return (
    <form className="TodoInsert" onSubmit={onSubmit}>
      <input
        placeholder="할 일을 입력하세요"
        value={value}
        onChange={onChange}
      ></input>
      <button type="submit">
        <MdAdd />
      </button>
    </form>
  );
};

export default TodoInsert;
```

onSubmit 이벤트는 브라우저를 새로고침 하기 때문에, e.preventDefault() 함수를호출하면 새로고침을 방지 할 수 있음.

버튼을 onSubmit 대신 onClick을 사용하지 않는 이유는

onSubmit 이벤트의 경우 인풋에서 Enter 키가 먹히기 때문이다.

onClick을 사용한다면 onKeyPress 이벤트를 통해 Enter를 감지하는 로직을 따로 작성해야 한다.
