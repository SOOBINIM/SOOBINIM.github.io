---
categories: react
---

# state

컴포넌트 내부에서 바뀔 수 있는 값

props는 부모 컴포넌트가 설정하고,

컴포넌트 자신은 해당 props를 읽기 전용으로만 사용 가능

클래스형 컴포넌트 state 와

함수형 컴포넌트의 useState 함수를 통해 사용하는 state 두 종류가 있다.

## Counter 예제

```jsx
// src/Counter.js
import React, { Component } from "react"

class Counter extends Component {
    // 컴포넌트의 생성자 메서드
    constructor(props) {
        // super(props)를 호출하면
        // 클래스형 컴포넌트가 상속하고 있는
        // 리액트의 Component 클래스가 지닌 생성자 함수를 호출
        super(props);
        // 초깃값 설정, 컴포넌트의 state는 객체 형식
        this.state = {
            number: 0
        };
    }

    render() {
        // state를 조회할 떄는 this.state로 조회
        const { number } = this.state;
        return (
            <div>
                <h1>{number}</h1>
                {/* onClick을 통해 버튼이 클릭되었을 때 호출 함수를 지정  */}
                <button onClick={() => {
                    // this.setState를 사용하여 state에 새로운 값을 넣을 수 있음
                    this.setState({ number: number + 1 });
                }}
                > + 1
                </button>
            </div>
        );
    }
}

export default Counter;

// src.App.js
import Counter from "./Counter";

function App() {
  return (
    <div>
      <Counter />
    </div>
  );
}

export default App;
```

컴포넌트에 state를 설정할 때는 constructor 메서드를 작성 해야 한다.

클래스 컴포넌트에서 constructor를 작설 할 때는 반드시 super(props)를 호출해야 한다.

this.state 값에 객체 형태로 초깃값을 설정해야 한다.

render 함수에서 state를 조회할 때는 this.state를 조회해야 한다.

button 안에 이벤트 함수를 넣어 줄 때는 화살표 함수 문법을 사용 해야 한다.

this.setState 함수는 state 값을 바꿀 수 있게 해준다.

## state 객체 안에 여러 값이 있을 때

```jsx
(...)
constructor(props){
	super(props);
	this.state = {
		number : 0,
		fixedNumber : 0
	};
}

render() {
	const {number, fixedNumber } = this.state;
	(...)
	<h1>{number}</h1>
	<h2>바뀌지 않는 값: {fixedNumber}</h2>
	(...)
	<button onClick={() => {this.setState({ number: number + 1 });}}>
	// this.setState를 사용하여 state에 새로운 값을 넣을 수 있습니다.
	(...)
```

## state를 constructor에서 꺼내기

```jsx
class Counter extends Component {
	state = {
		number: 0,
		fixedNumber: 0
	};
	render() {
			const { number, fixedNumber } = this.state; // state를 조
			회할 때는 this.state로 조회합니다.
			return (...);
		}
}
```

constructor 메서드를 선언하지 않고도

state 초깃값을 설정할 수 있음

## this.setState에 객체 대신 함수 인자 전달

```jsx
this.setState({ number: number + 1 });

this.setState((prevState, props) => { retrun
		//업데이트하고 싶은 내용
	}
})
```

prevState는 기존 상태

props는 현재 지니고 있는 props (생략 가능)

```jsx
<button
  // onClick을 통해 버튼이 클릭되었을 때 호출할 함수를 지정합니다.
  onClick={() => {
    this.setState((prevState) => {
      return {
        number: prevState.number + 1,
      };
    });

    // 위 코드와 아래 코드는 완전히 똑같은 기능을 합니다.
    // 아래 코드는 함수에서 바로 객체를 반환한다는 의미입니다.
    this.setState((prevState) => ({
      number: prevState.number + 1,
    }));
  }}
>
  {" "}
  +1
</button>
```

화살표 함수에서 값을 바로 반환하고 싶을 때는 {return {}}를 생략 하면 된다.

prevState ⇒ ({ }) 로 사용하면 바로 반환

## this.setState가 끝난 후 특정 작업 실행

setState의 두 번째 파라미터로 콜백 함수를 등록하여 작업을 처리

```jsx
<button
  // onClick을 통해 버튼이 클릭되었을 때 호출할 함수를 지정합니다.
  onClick={() => {
    this.setState(
      {
        number: number + 1,
      },
      () => {
        console.log("방금 setState가 호출되었습니다.");
        console.log(this.state);
      }
    );
  }}
>
  +1
</button>
```

## 함수형 컴포넌트에서 useState 사용

16.8 이전에는 사용 불가

16.8 버전 이후 useState 함수 사용하여 state 사용 가능

### 배열 비구조화 할당

```jsx
const arr = [1, 2];
const one = arr[0];
const two = arr[1];

// 배열 비구조화 할당
const arr = [1, 2];
const [one, two] = arr;
```

### useState 사용

```jsx
import { useState } from "react";

const Say = () => {
  const [message, setMessage] = useState("");
  const onClickEnter = () => setMessage("안녕하세요!");
  const onClickLeave = () => setMessage("안녕히계세요!");

  return (
    <div>
      <button onClick={onClickEnter}>입장</button>
      <button onClick={onClickLeave}>퇴장</button>
      <h1>{message}</h1>
    </div>
  );
};

export default Say;
```

useState 함수의 인자에는 상태의 초깃값을 넣어준다.

state 초깃값의 형태는 자유 (클래스 컴포넌트는 객체 형태)

배열의 첫 번째 원소는 현재 상태, 두번째 원소는 상태를 바꾸어주는 함수

바꾸어주는 함수를 세터(Setter) 함수라고 한다.

### 한 컴포넌트에서 useState 여러 번 사용하기

```jsx
(...)
const [message, setMessage] = useState('');
const onClickEnter = () => setMessage('안녕하세요!');
const onClickLeave = () => setMessage('안녕히 가세요!');
const [color, setColor] = useState('black');
(...)
<button onClick={onClickEnter}>입장</button>
<button onClick={onClickLeave}>퇴장</button>
<h1 style={{ color }}>{message}</h1>
<button style={{ color: 'red' }} onClick={() => setColor('red')}>
빨간색
</button>
<button style={{ color: 'green' }} onClick={() => setColor('green')}>
초록색
</button>
```

# state를 사용할 때 주의 사항

state 값을 바꾸어야 할 때는 setState, useState를 통해 전달받은 세터 함수를 사용해야 한다.

객체, 배열을 업데이트 할 때는 반드시 사본을 만들고 그 사본 값을 업데이터 한 후,

사본의 상태를 setState 혹은 세터 함수를 통해 업데이트 한다.

```jsx
// (잘못된 코드) 클래스형 컴포넌트
this.state.number = this.state.number + 1;
this.state.array = this.array.push(2);
this.state.object.value = 5;

// (잘못된 코드) 함수형 컴포넌트
const [object, setObject] = useState({ a: 1, b: 1 });
object.b = 2;

// (잘된 코드) 객체 다루기
const object = { a: 1, b: 2, c: 3 };
const nextObject = { ...object, b: 2 }; // 사본을 만들어서 b 값만 덮어 쓰기

// 배열 다루기
const array = [
{ id: 1, value: true },
{ id: 2, value: true },
{ id: 3, value: false }
];
let nextArray = array.concat({ id: 4 }); // 새 항목 추가
nextArray.filter(item => item.id != = 2); // id가 2인 항목 제거
nextArray.map(item => (item.id === 1 ? { ...item, value: false } : item));
// id가 1인 항목의 value를 false로 설정

```

객체에 대한 사본을 만들 때는 spread 연산자라 불리는 …을 사용하여 처리

# 정리

props는 부모 컴포넌트가 설정

state는 컴포넌트 자체적으로 지닌 값으로 컴포넌트 내부에서 값을 업데이트

props는 무조건 고정적이지 않다.

부모 컴포넌트의 state를

자식 컴포넌트의 props로 전달

자식 컴포넌트에서 특정 이벤트가 발생할 때

부모 컴포넌트의 메서드를 호출시

props도 유동적으로 사용할 수 있음
