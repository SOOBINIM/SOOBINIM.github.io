---
categories: react
---

# Context API를 사용한 전역 상태 관리 흐름 이해하기

전역적으로 상태를 관리를 해야 할 때,

리액트 애플리케이션은 컴포넌트 간에 데이터를 props로 전달하기 때문에 컴포넌트 여기저기서

필요한 데이터가 있을 때는 주로 최상위 컴포넌트인 App의 state에 넣어서 관리한다.

기존에는 최상위 컴포넌트에서 여러 컴포넌트를 거쳐 props로 원하는 상태와 함수를 전달했지만,

Context API를 사용하면 Context를 만들어 단 한 번에 원하는 값을 받아와서 사용할 수 있다.

리덕스나 MobX 같은 상태관리 라이브러리를 사용하기도 하지만,

리액트 v16.3 업데이트 이후에는 Context API가 많이 개선 되어 별도의 라이브러리를 사용하지 않아도 전역 상태를 손쉽게 관리할 수 있다.

# Context API 사용법 익히기

```powershell
$yarn create react-app context-tutorial
```

## 새 Context 만들기

```jsx
contexts / color.js;

import { createContext } from "react";

const ColorContext = createContext({ color: "black" });

export default ColorContext;
```

createContext 함수를 사용해서 새 Context를 만듬.

파라미터에 Context 기본 상태 저장.

## Consumer 사용

```jsx
components / ColorBox.js;

import React from "react";
import ColorContext from "../contexts/color";

const ColorBox = () => {
  return (
    <ColorContext.Consumer>
      {(value) => (
        <div
          style={{
            width: "64px",
            height: "64px",
            background: value.color,
          }}
        />
      )}
    </ColorContext.Consumer>
  );
};
export default ColorBox;
```

ColorContext 에서 props로 전달 받는게 아닌,

ColorContext 안에 들어 있는 Consumer 라는 컴포넌트를 통해 color 값을 가져온다.

color 값은 중괄호를 열어 그 안에 함수를 넣어 줌.

이러한 패턴을 Function as a child, Render Props라고 한다.

컴포넌트의 children이 있어야 할 자리에 일반 JSX 혹은 문자열이 아닌 함수를 전달

### Render Props 예제

```jsx
import React from "react";

const RenderPropsSample = ({ children }) => {
  return <div> 결과 : {children(5)}</div>;
};

export default RenderPropsSample;

// 사용
<RenderPropsSample>{(value) => 2 * value}</RenderPropsSample>;
```

함수를 전달

## Provider

```jsx
import React from "react";
import ColorBox from "./components/ColorBox";
import ColorContext from "./contexts/color";

const App = () => {
  return (
    <ColorContext.Provider value={{ color: "red" }}>
      <div>
        <ColorBox />
      </div>
    </ColorContext.Provider>
  );
};
export default App;
```

Provider를 사용해서 context의 value 값을 변경 할 수 있다.

Provider를 사용할 때는 value 값을 반드시 명시해 줄 것.

# 동적 Context 사용하기

Context 값을 업데이트해야 하는 경우

## Context 파일 수정하기

```jsx
import React, { createContext, useState } from "react";

const ColorContext = createContext({
  state: { color: "black", subcolor: "red" },
  actions: {
    setColor: () => {},
    setSubcolor: () => {},
  },
});

const ColorProvider = ({ children }) => {
  const [color, setColor] = useState("black");
  const [subcolor, setSubcolor] = useState("red");
  const value = {
    state: { color, subcolor },
    actions: { setColor, setSubcolor },
  };
  return (
    <ColorContext.Provider value={value}>{children}</ColorContext.Provider>
  );
};

// const ColorConsumer = ColorContext.Consumer와 같은 의미
const { Consumer: ColorConsumer } = ColorContext;
// ColorProvider와 ColorConsumer 내보내기
export { ColorProvider, ColorConsumer };

export default ColorContext;
```

Context의 value에는 상태 값 외에 함수를 전달해줄 수 도 있다.

ColorContext에 기본값으로 사용할 객체를 Provider의 value에 넣는 객체의 형태와 맞춰 준다.

ColorProvider에는 ColorContext.Provider를 렌더링 해준다.

또, value에 상태는 state, 업데이트 함수는 action으로 묶어 Provider에 넘겨준다.

## 새로워진 Contex를 프로젝트에 반영하기

```jsx
App.js;

import React from "react";
import ColorBox from "./components/ColorBox";
import { ColorProvider } from "./contexts/color";

const App = () => {
  return (
    <ColorProvider>
      <div>
        <ColorBox />
      </div>
    </ColorProvider>
  );
};
export default App;
```

ColorContext.Provider → ColorProvider 수정

```jsx
components / ColorBox.js;

import ColorConsumer from "../contexts/color";

const ColorBox = () => {
  return (
    <ColorConsumer>
      {({ state }) => (
        <>
          <div
            style={{
              width: "64px",
              height: "64px",
              background: state.color,
            }}
          />
          <div
            style={{
              width: "32px",
              height: "32px",
              background: state.subcolor,
            }}
          />
        </>
      )}
    </ColorConsumer>
  );
};

export default ColorBox;
```

ColorContext.Consumer → ColorConsumer 수정

## 색상 선택 컴포넌트 만들기

```jsx
components / SelectColors.js;

import { ColorConsumer } from "../contexts/color";

const colors = ["red", "orange", "yellow", "green", "blue", "indifo", "violet"];
const SelectColors = () => {
  return (
    <div>
      <h2>색상을 선택 하세요.</h2>
      <ColorConsumer>
        {({ actions }) => (
          <div style={{ display: "flex" }}>
            {colors.map((color) => (
              <div
                key={color}
                style={{
                  background: color,
                  width: "24px",
                  height: "24px",
                  cursor: "pointer",
                }}
                onClick={() => actions.setColor(color)}
                onContextMenu={(e) => {
                  e.preventDefault();
                  actions.setSubcolor(color);
                }}
              ></div>
            ))}
          </div>
        )}
      </ColorConsumer>
      <hr />
    </div>
  );
};

export default SelectColors;
```

Contex의 actions에 넣어 준 함수를 호출 하는 컴포넌트

onContextMenu >> e.preventDefault() 는 마우스 오른쪽 버튼 클릭시 메뉴가 뜨는 것을 무시함

# Consumer 대신 Hook 또는 static contextType 사용하기

Context에 있는 값을 가져올 때 Consumer 대신 다른 방식으로 값 가져오기

## useContext Hook 사용

```jsx
import { useContext } from "react";
import ColorContext from "../contexts/color";

const ColorBox = () => {
  const { state } = useContext(ColorContext);
  return (
    <>
      <div
        style={{
          width: "64px",
          height: "64px",
          background: state.color,
        }}
      />
      <div
        style={{
          width: "32px",
          height: "32px",
          background: state.subcolor,
        }}
      />
    </>
  );
};

export default ColorBox;
```

children에 함수를 전달하는 Render Props 패턴이 불편하다면

useContext Hook을 사용하면 훨씬 편하게 Contex 값을 조회할 수 있음.

단, Hook은 클래스 컴포넌트에서는 사용 불가

## static contextType 사용하기

```jsx
components / SelectColors.js;

import { Component } from "react";
import ColorContext from "../contexts/color";

const colors = ["red", "orange", "yellow", "green", "blue", "indifo", "violet"];

class SelectColors extends Component {
  static contextType = ColorContext;

  handleSetColor = (color) => {
    this.context.actions.setColor(color);
  };

  handleSetSubcolor = (subcolor) => {
    this.context.actions.setSubcolor(subcolor);
  };
  render() {
    return (
      <div>
        <h2>색상을 선택하세요.</h2>
        <div style={{ display: "flex" }}>
          {colors.map((color) => (
            <div
              key={color}
              style={{
                background: color,
                width: "24px",
                height: "24px",
                cursor: "pointer",
              }}
              onClick={() => this.handleSetColor(color)}
              onContextMenu={(e) => {
                e.preventDefault();
                this.handleSetSubcolor(color);
              }}
            />
          ))}
        </div>
        <hr />
      </div>
    );
  }
}

export default SelectColors;
```

클래스형 컴포넌트에서 static ContextType으로 Context 값 가져오기

단, 한 클래스에서 하나의 Context밖에 사용하지 못함.

static ContextType = ColorContext 값 지정시

this.context를 조회했을 때 현재 Context의 value를 가리키게 됨

setColor를 호출하고 싶다면 this.context.actions.setColor
