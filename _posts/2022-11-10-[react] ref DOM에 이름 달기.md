---
categories: react
---

# ref는 어떤 상황에서 사용해야 할까?

ref (reference)는 리액트 프로젝트 내부에서 DOM에 이름을 다는 벙법

ref는 전역적으로 작동하지 않고 컴포넌트 내부에서만 작동

<aside>
💡 리액트 컴포넌트 안에서는 id를 사용하면 안될까?

</aside>

권장 하지 않는다.

다른 라이브러리나 프레임워크와 함께 id를 사용해야 하는 상황이 발생하면

컴포넌트를 만들 때마다 id 뒷부분에 추가 텍스트를 붙여야 한다.

중복 id가 발생하는것을 방지해야 한다.

DOM을 꼭 직접적으로 건드려야할 때 ref를 사용한다.

## 예제 컴포넌트 생성

```css
.success {
  background-color: lightgreen;
}

.failure {
  background-color: lightcoral;
}
```

```jsx
import { Component } from "react";
import "./ValidationSample.css";

class ValidationSample extends Component {
  state = {
    password: "",
    clicked: false,
    validated: false,
  };

  handleChange = (e) => {
    this.setState({
      password: e.target.value,
    });
  };

  handleButtonClick = () => {
    this.setState({
      clicked: true,
      validated: this.state.password === "0000",
    });
  };

  render() {
    return (
      <div>
        <input
          type="password"
          value={this.state.password}
          onChange={this.handleChange}
          className={
            this.state.clicked
              ? this.state.validated
                ? "success"
                : "failure"
              : ""
          }
        ></input>
        <button onClick={this.handleButtonClick}>검증하기</button>
      </div>
    );
  }
}

export default ValidationSample;
```

input 안에 onChange 이벤트 발생시 handleChange 호출하여 state의 password 값 업데이트

button 안에 onClick 이벤트 발생시 handleButtonClick 호출하여 clicked 값을 참으로 설정, validated 값 검증 결과로 설정

input의 className 값은 누르기 전에는 비어있는 문자열 전달

누른 후에는 검증 결과에 따라 success 값 또는 failure 값으로 설정

이 값에 따라 input 색상이 초록생 또는 빨간색으로 바뀐다.

## DOM을 꼭 사용해야 하는 상황

- 특정 input에 포커스 주기
- 스크롤 박스 조작하기
- Canvas 요소에 그림 그리기 등

이런 경우에 ref를 사용

# ref 사용

ref를 사용하는 방법은 2가지

## 콜백 함수를 통한 ref 설정

ref를 달고자 하는 요소에 ref라는 콜백 함수를 props로 전달

ref 값을 파라미터로 전달 받는다.

함수 내부에서 파라미터로 받은 ref를 컴포넌트의 멤버 변소로 설정

```jsx
<input
  ref={(ref) => {
    this.input = ref;
  }}
/>
// input 요소의 DOM을 가리킴
// this.soob = ref 처럼 이름 마음대로 정해도 됨
```

## createRef를 통한 ref 설정

리액트 내장 함수 createRef 사용

```jsx
import React, { Component } from "react";

class RefSample extends Component {
  input = React.createRef();
  // 멤버변수 선언

  handleFocus = () => {
    this.input.current.focus();
  };

  render() {
    return (
      <div>
        <input ref={this.input} />
      </div>
    );
  }
}
export default RefSample;
```

input이라는 멤버 변수에 React.createRef()를 담아 준다.

input 멤버 변수를 사용하고자 하는곳에 ref props로 넣어준다.

DOM에 접근 하려면 this.input.current로 조회

# 컴포넌트에 ref 달기

```jsx
<MyComponent
  ref={(ref) => {
    this.myComponent = ref;
  }}
/>
```

MyComponent 내부의 메서드 및 멤버 변수에도 접근 가능

내부의 ref에도 접근 가능

(예: myComponent.handleClick, myComponent.input 등)

```jsx
import React, { Component } from "react";
class ScrollBox extends Component {
  scrollToBottom = () => {
    const { scrollHeight, clientHeight } = this.box;
    this.box.scrollTop = scrollHeight - clientHeight;
    /* 앞 코드에는 비구조화 할당 문법을 사용
				const scrollHeight = this.box.scrollHeight;
				const clientHeight = this.box.cliengHeight;
				*/
  };

  render() {
    const style = {
      border: "1px solid black",
      height: "300px",
      width: "300px",
      overflow: "auto",
      position: "relative",
    };
    const innerStyle = {
      width: "100%",
      height: "650px",
      background: "linear-gradient(white, black)",
    };
    return (
      <div
        style={style}
        ref={(ref) => {
          this.box = ref;
        }}
      >
        <div style={innerStyle} />
      </div>
    );
  }
}
export default ScrollBox;
```

스크롤 제어시 DOM 노드가 가진 값 사용

- scrollTop : 세로 스크롤바 위치 (0~350)
- scrollHeight : 스크롤이 있는 박스 안의 div 높이 (650)
- clientHeight : 스크롤이 있는 박스의 높이 (300)

scrollToBottom 메서드는 부모 컴포넌트인 App 컴포넌트에서

ScrollBox에 ref를 달아서 사용가능

```jsx
import React, { Component } from 'react';
import ScrollBox from './ScrollBox';
class App extends Component {
  render() {
    return (
      <div>
        <ScrollBox ref={(ref) => this.scrollBox = ref} />
        <button onClick={() => this.scrollBox.scrollToBottom()}>
		 // <button onClick={this.scrollBox.scrollToBottom()}>
		 // 처음 랜더링될 때 this.scrollBox값이 undefined임으로 오류 발생
          맨 밑으로
        </button>
      </div>
    );
  }
}
export default App;
```

# 정리

DOM에 접근할 때 ref 사용

서로 다른 컴포넌트끼리 데이터를 교류할 떄는 ref 사용 금지

예를 들면 컴포넌트에 ref를 달고 그 ref를 다른 컴포넌트에 전달하는 것 금지

그렇게 되면 스파게티 코드가되어 유지보수가 불가능
