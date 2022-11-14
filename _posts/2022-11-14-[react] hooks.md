---
categories: react
---

리액트 v16.8에 새로 도입된 기능

함수형 컴포넌트에서도 상태관리를 할 수 있는 useState

렌더링 직후 작업을 설정하는 useEffect 등의 기능을 제공

# useState

```jsx
import React, { useState } from "react";

const Info = () => {
  const [name, setName] = useState("");
  const [nick, setNick] = useState("");

  const onChangeName = (e) => {
    setName(e.target.value);
  };

  const onChangeNick = (e) => {
    setNick(e.target.value);
  };

  return (
    <div>
      <div>
        <input value={name} onChange={onChangeName} />
        <input value={nick} onChange={onChangeNick} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임:</b> {nick}
        </div>
      </div>
    </div>
  );
};
export default Info;
```

관리할 상태가 여러 개인 경우에도 useState로 편하게 관리

# useEffect

컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정

클래스 컴포턴트의 componentDidMount, componentDidUpdate를 합친 형태

```jsx
useEffect(() => {
        console.log('렌더링이 완료 되었습니다.');
        console.log({name, nick});
    });
});
```

브라우저 실행과 동시에 console.log 창에 ‘렌더링이 완료 되었습니다” name, nick 값 표시

상태값 업데이트시 ‘렌더링이 완료 되었습니다” name, nick 값 표시

## 마운트될 때만 실행하고 싶을 때

```jsx
useEffect(() => {
  console.log("마운트될 때만 실행됩니다.");
}, []);
```

useEffect에서 설정한 함수를 컴포넌트가 화면에 맨 처음 렌더링될 때만 실행하고

업데이트될 때는 실행하지 않으려면 함수의 두번째 파라미터로 비어 있는 배열을 넣어준다.

## 특정 값이 업데이트될 때만 실행하고 싶을 때

```jsx
// 클래스 컴포넌트 라이프 사이클
// props 안에 있는 value 값이 바뀔 때만 특정 작업을 실행
componentDidUpdate(prevProps, prevState) {
	if (prevProps.value !== this.props.value) {
		doSomething();
	}
}

// useEffect hook 함수 사용
useEffect(() => {
		console.log(name);
	}, [name]);
```

배열안에 검사하고 싶은 값을 넣어주면 된다.

## 뒷정리

useEffect는 기본적으로 렌더링되고 난 직후마다 실행,

두 번째 파라미터 배열에 무엇을 넣는지에 따라 실행되는 조건이 달라짐

```jsx
// src/Info.js
import React, { useState, useEffect } from "react";

const Info = () => {
  const [name, setName] = useState("");
  const [nick, setNick] = useState("");

  useEffect(() => {
    console.log("effect");
    console.log(name);
    return () => {
      console.log("cleanup");
      console.log(name);
    };
  }, []);

  const onChangeName = (e) => {
    setName(e.target.value);
  };

  const onChangeNick = (e) => {
    setNick(e.target.value);
  };

  return (
    <div>
      <div>
        <input value={name} onChange={onChangeName} />
        <input value={nick} onChange={onChangeNick} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임:</b> {nick}
        </div>
      </div>
    </div>
  );
};
export default Info;

// src/App.js
import { useState } from "react";
import Info from "./Info";

const App = () => {
  const [visible, setVisible] = useState(false);
  return (
    <div>
      <button
        onClick={() => {
          setVisible(!visible);
        }}
      >
        {visible ? "숨기기" : "보이기"}
      </button>
      <hr />
      {visible && <Info />}
    </div>
  );
};
```

컴포넌트가 언마운트되기 전이나 업데이트되기 직전에 어떠한 작업을 수행 하고자 할 떄는

useEffect에서 뒷정리(cleanup) 함수를 반환해 주어야 한다.

뒷정리(cleanup) 함수가 호출될 떄는 업데이트되기 적전의 값을 보여준다.

언마운트 때만 함수를 호출하고 싶다면

useEffect 함수의 두번째 파라미터에 비어 있는 배열을 넣으면 된다.

# useReducer

useState보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트해 주고 싶을 때 사용.

리듀서는 현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션(action)값을 전달받아 새로운 상태를 반환하는 함수

반드시 불변성을 지켜주어야 한다.

```jsx
import { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { value: state.value + 1 };
    case "DECREMENT":
      return { value: state.value - 1 };
    default:
      // 아무것도 해당되지 않을 때 기존 상태 반환
      return state;
  }
}

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { value: 0 });

  return (
    <div>
      <p>
        현재 카운터 값은 <b>{state.value} 입니다.</b>
      </p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+1</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>-1</button>
    </div>
  );
};

export default Counter;
```

useReducer의 첫 번째 파라미터에는 리듀서 함수를 넣고,

두 번째 파라미터에는 해당 리듀서의 기본값을 넣어 준다.

useReducer를 사용하면 state 값과, dispatch 함수를 가져온다.

state는 현재 가리키고 있는 상태

dispatch는 액션을 발생시키는 함수

dispatch(action)으로 함수 안에 파라미터로 액션 값을 넣어주면 리듀서 함수가 호출

useReducer를 사용했을 때 가장 큰 장점은

컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있다는 점

## 인풋 상태 관리하기

```jsx
import { useReducer } from "react";

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  };
}

const Info = () => {
  const [state, dispatch] = useReducer(reducer, {
    name: "",
    nick: "",
  });

  const { name, nick } = state;
  const onChange = (e) => {
    dispatch(e.target);
  };

  return (
    <div>
      <div>
        <input name="name" value={name} onChange={onChange}></input>
        <input name="nick" value={nick} onChange={onChange}></input>
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임: </b> {nick}
        </div>
      </div>
    </div>
  );
};

export default Info;
```

클래스형 컴포넌트에서 input 태그에 name 값을 할당하고

e.target.name을 참고해서 setState를 해준것과 유사한 방식

useReducer에서의 액션은 어떤 값도 사용 가능

e.target 값 자체를 액션으로 사용.

인풋이 아무리 많아져도 코드를 짧고 깔끔하게 유지 가능

# useMemo

렌더링하는 과정에서 특정 값이 바뀌었을 때만 연산을 실행

원하는 값이 바뀌지 않았다면 이전에 연산했던 결과를 다시 사용

```jsx
import { useState } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중 ..");
  if (numbers.length === 0) return 0;

  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = (e) => {
    setNumber(e.target.value);
  };

  const onInsert = (e) => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
  };

  return (
    <div>
      <input value={number} onChange={onChange}></input>
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값 : </b> {getAverage(list)}
      </div>
    </div>
  );
};

export default Average;
```

숫자를 등록할 때뿐만 아니라,

인풋 내용이 수정될 때도 getAverage 함수가 호출됨

인풋 내용이 바뀔 때는 평균값을 다시 계산할 필요가 없음

렌더링할 때마다 계산하는 것은 낭비.

위와 같은 문제를 useMemo Hook을 사용하여 작업을 최적화할 수 있다.

```jsx
// useMemo 사용

const Average = () => {
    const [list, setList] = useState([]);
    const [number, setNumber] = useState('');
//		(...)
    }

    const avg = useMemo(() => getAverage(list), [list])

    return (
        <div>
{/*				(...) */}
            <div>
                <b>평균값 : </b> {avg}
            </div>
        </div>
    )
}

export default Average;
```

list 배열이 바뀔 때만 getAverage 함수 호출

# useCallback

useMemo와 비슷한 함수

렌더링 성능 최적화 하는 상황에서 사용

이벤트 핸들러 함수를 필요할 때만 생성

```jsx
import { useState, useMemo, useCallback } from "react";

const getAverage = numbers => {
//  (...)
    const onChange = useCallback(e => {
        setNumber(e.target.value);
    }, []);

    const onInsert = useCallback(e => {
        const nextList = list.concat(parseInt(number));
        setList(nextList);
        setNumber('');
    }, [number, list]);

// (...)
```

useCallback의 첫 번째 파라미터에는 생성하고 싶은 함수를 넣는다.

두 번째 파라미터에는 배열을 넣는다.

이 배열에는 어떤 값이 바뀌었을 때 함수를 새로 생성해야 하는지 명시해야 한다.

onChange함수에서 비어 있는 배열을 넣으면

컴포넌트가 렌더링될 때 단 한 번만 함수가 생성된다.

onInsert함수에서는 배열 안에 number,list를 넣었는데

인풋 내용이 바뀌거나 새로운 항목이 추가될 때마다 함수가 생성된다.

이 처럼 함수 내부에서 상태 값을 의존해야 할 경우에 그값을 반드시 두 번째 파라미터에 포함

## useMemo와 useCallback 차이

useCallback은 결국 useMemo로 함수를 반환하는 상황에서 더 편하게 사용할 수 있는 HOOK

useMemo는 숫자, 문자열, 객체처럼 일반 값을 재사용할 때

useCacllback은 함수를 재사용할 때

# useRef

ref를 쉽게 사용할 수 있도록 해주는 Hook

```jsx
import { useState, useMemo, useCallback, useRef } from "react";

// (...)
const Average = () => {
    const [list, setList] = useState([]);
    const [number, setNumber] = useState('');
    const inputEl = useRef(null);

    const onChange = useCallback(e => {
        setNumber(e.target.value);
    }, []);

    const onInsert = useCallback(e => {
        const nextList = list.concat(parseInt(number));
        setList(nextList);
        setNumber('');
        inputEl.current.focus();
    }, [number, list]);

    const avg = useMemo(() => getAverage(list), [list])

    return (
        <div>
            <input value={number} onChange={onChange} ref={inputEl}></input>
{/* (...) */}
```

useRef를 통해 만든 객체 안의 current 값이 실제 엘리먼트를 가리킴

## 로컬 변수 사용하기

로컬 변수란 렌더링과 상관없이 바뀔 수 있는 값

```jsx
import React, { useRef } from "react";
const RefSample = () => {
  const id = useRef(1);
  const setId = (n) => {
    id.current = n;
  };
  const printId = () => {
    console.log(id.current);
  };
  return <div>refsample</div>;
};
export default RefSample;
```

ref 안의 값이 바뀌어도 컴포넌트가 렌더링되지 않는다.

렌더링과 관련되지 않은 값을 관리할 때만 이러한 방식으로 코드 작성

# 커스텀 Hooks 만들기

```jsx
// src/useInputs.js
// 컨스텀 Hooks 생성

import { useReducer } from "react";

function reducer(state, action) {
    return {
        ...state,
        [action.name]: action.value
    };
}

export default function useInputs(initialForm) {
    const [state, dispatch] = useReducer(reducer, initialForm);
    const onChange = e => {
        dispatch(e.target);
    }
    return [state, onChange]
}

// src/Info.js
import useInputs from "./useInputs";

const Info = () => {
    const [state, onChange] = useInputs({
        name: '',
        nick: ''
    });

    const { name, nick } = state;

    return (
        <div>
            <div>
                <input name="name" value={name} onChange={onChange}></input>
                <input name="nick" value={nick} onChange={onChange}></input>
            </div>
            <div>
                <div>
                    <b>이름:</b> {name}
                </div>
                <div>
                    <b>닉네임: </b> {nick}
                </div>
            </div>
        </div>
    )
}

export default Info;
```

# 다른 Hooks

- [https://nikgraf.github.io/react-hooks/](https://nikgraf.github.io/react-hooks/)
- [https://github.com/rehooks/awesome-react-hooks](https://github.com/rehooks/awesome-react-hooks)

# 정리

리액트 매뉴얼에 따르면, 기존의 클래스형 컴포넌트는 앞으로도 계속해서 지원될 예정

클래스 컴포넌트를 사용한 프로젝트에서는 함수형 컴포넌트와 Hooks로 굳이 전환하지 않아도 됨

리액트 메뉴얼에서는 새로 작성하는 컴포넌트의 경우 함수형 컴포넌트와 Hooks를 사용할 것을 권장
