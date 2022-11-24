---
categories: react
---

# immer 설치하고 사용방법 알아보기

객체의 구조가 엄청나게 깊어지면 불변성을 유지하면서 업데이트를 하기 어려움

immer 라이브러리를 사용하면, 구조가 복잡한 객체도 매우 쉽고 짧은 코드를 사용하여 불변성을 유지하면서 업데이트해 줄 수 있음.

## 프로젝트 준비

yarn을 통해 immer 설치

```scss
$yarn add immer
```

## immer를 사용하지 않고 불변성 유지

```jsx
import { useState, useRef, useCallback } from "react";

const App = () => {
  const nextID = useRef(1);
  const [form, setForm] = useState({ name: "", username: "" });
  const [data, setData] = useState({
    array: [],
    uselessValue: null,
  });

  const onChange = useCallback(
    (e) => {
      const { name, value } = e.target;
      setForm({
        ...form,
        [name]: [value],
      });
    },
    [form]
  );

  const onSubmit = useCallback(
    (e) => {
      e.preventDefault();
      const info = {
        id: nextID.current,
        name: form.name,
        username: form.username,
      };

      setData({
        ...data,
        array: data.array.concat(info),
      });

      setForm({
        name: "",
        username: "",
      });
      nextID.current += 1;
    },
    [data, form.name, form.username]
  );

  const onRemove = useCallback(
    (id) => {
      setData({
        ...data,
        array: data.array.filter((info) => info.id !== id),
      });
    },
    [data]
  );

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          name="username"
          placeholder="아이디"
          value={form.username}
          onChange={onChange}
        />
        <input
          name="name"
          placeholder="이름"
          value={form.name}
          onChange={onChange}
        />
        <button type="submit">등록</button>
      </form>
      <div>
        <ul>
          {data.array.map((info) => (
            <li key={info.id} onClick={() => onRemove(info.id)}>
              {info.username} ({info.name})
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
};

export default App;
```

폼에서 아이디, 이름을 입력하면 하단 리스트에 추가 됨.

리스트 항목을 클릭하면 삭제 됨.

위 예제는 배열 내장 함수(concat, filter,map)를 사용하여 불변성을 유지함.

## immer 사용법

```jsx
import produce from "immer";
const nextState = produce(originState, (draft) => {
  // 바꾸고 싶은 값 바꾸기
  draft.somewhere.deep.inside = 5;
});
```

첫 번째 파라미터 : 수정하고 싶은 상태

두 번째 파라미터 : 상태를 어떻게 업데이트할지 정의

두 번째 파라미터로 전달되는 함수 내부에서 원하는 값을 변경하면,

produce 함수가 불변성 유지를 대신해 주면서 새로운 상태를 생성

라이브러리 핵심 : 불변성에 신경 쓰지 않는 것처럼 코드를 작성하되 불변성 관리는 제대로 해 주는 것

## App 컴포넌트에 immer 적용하기

```jsx
import { useState, useRef, useCallback } from "react";
import produce from "immer";

const App = () => {
  const nextID = useRef(1);
  const [form, setForm] = useState({ name: "", username: "" });
  const [data, setData] = useState({
    array: [],
    uselessValue: null,
  });

  // input 수정을 위한 함수
  const onChange = useCallback(
    (e) => {
      const { name, value } = e.target;
      setForm(
        produce(form, (draft) => {
          draft[name] = value;
        })
      );
      // setForm({
      //   ...form,
      //   [name]: [value]
      // });
    },
    [form]
  );

  // form 등록을 위한 함수
  const onSubmit = useCallback(
    (e) => {
      e.preventDefault();
      const info = {
        id: nextID.current,
        name: form.name,
        username: form.username,
      };
      // array에 새 항목 등록
      setData(
        produce(data, (draft) => {
          draft.array.push(info);
        })
      );
      // setData({
      //   ...data,
      //   array: data.array.concat(info)
      // });

      // form 초기화
      setForm({
        name: "",
        username: "",
      });
      nextID.current += 1;
    },
    [data, form.name, form.username]
  );

  // 항목을 삭제하는 함수
  const onRemove = useCallback(
    (id) => {
      setData(
        produce(data, (draft) => {
          draft.array.splice(
            draft.array.findIndex((info) => info.id === id),
            1
          );
        })
      );
      // setData({
      //   ...data,
      //   array: data.array.filter(info => info.id !== id)
      // });
    },
    [data]
  );

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          name="username"
          placeholder="아이디"
          value={form.username}
          onChange={onChange}
        />
        <input
          name="name"
          placeholder="이름"
          value={form.name}
          onChange={onChange}
        />
        <button type="submit">등록</button>
      </form>
      <div>
        <ul>
          {data.array.map((info) => (
            <li key={info.id} onClick={() => onRemove(info.id)}>
              {info.username} ({info.name})
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
};

export default App;
```

immer를 사용하여 컴포넌트 상태를 작성할 때는 객체 안에 있는 값을 직접 수정, 배열에 직접적인 변화를 일으키는 push, splice 등의 함수를 사용해도 됨.

자바스크립트에 익숙하면 immer와 함께 사용해도 불변성을 유지 할 수 있다.

## useState의 함수형 업데이트와 immer 함께 쓰기

```jsx
import { useState, useRef, useCallback } from "react";
import produce from "immer";

const App = () => {
  const nextID = useRef(1);
  const [form, setForm] = useState({ name: "", username: "" });
  const [data, setData] = useState({
    array: [],
    uselessValue: null,
  });

  // input 수정을 위한 함수
  const onChange = useCallback((e) => {
    const { name, value } = e.target;
    setForm(
      produce((draft) => {
        draft[name] = value;
      })
    );

    // setForm(
    //   produce(form, draft => {
    //     draft[name] = value
    //   })
    // )

    // setForm({
    //   ...form,
    //   [name]: [value]
    // });
  }, []);

  // form 등록을 위한 함수
  const onSubmit = useCallback(
    (e) => {
      e.preventDefault();
      const info = {
        id: nextID.current,
        name: form.name,
        username: form.username,
      };
      // array에 새 항목 등록
      setData(
        produce((draft) => {
          draft.array.push(info);
        })
      );

      // setData(
      //   produce(data, draft => {
      //     draft.array.push(info);
      //   })
      // )

      // setData({
      //   ...data,
      //   array: data.array.concat(info)
      // });

      // form 초기화
      setForm({
        name: "",
        username: "",
      });
      nextID.current += 1;
    },
    [form.name, form.username]
  );

  // 항목을 삭제하는 함수
  const onRemove = useCallback((id) => {
    setData(
      produce((draft) => {
        draft.array.splice(
          draft.array.findIndex((info) => info.id === id),
          1
        );
      })
    );
    // setData(
    //   produce(data, draft => {
    //     draft.array.splice(draft.array.findIndex(info => info.id === id), 1);
    //   })
    // )

    // setData({
    //   ...data,
    //   array: data.array.filter(info => info.id !== id)
    // });
  }, []);

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          name="username"
          placeholder="아이디"
          value={form.username}
          onChange={onChange}
        />
        <input
          name="name"
          placeholder="이름"
          value={form.name}
          onChange={onChange}
        />
        <button type="submit">등록</button>
      </form>
      <div>
        <ul>
          {data.array.map((info) => (
            <li key={info.id} onClick={() => onRemove(info.id)}>
              {info.username} ({info.name})
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
};

export default App;
```

immer에서 제공하는 produce함수를 호출할 때,

첫 번째 파라미터가 함수형태라면 업데이트 함수를 반환한다.

# 정리

컴퓨넌트의 상태 업데이트가 조금 까다로울 때 사용하면 좋음

상태 관리 라이브러리인 리덕스를 배워서 사용할 때도 immer를 쓰면 코드를 매우 쉽게 작성 가능.

immer를 사용하는 것이 오히려 불편하게 느껴지면 사용하지 않아도 됨.
