---
categories: react
---

# props

properties를 줄인 표현

props 값은 부모 컴포넌트에서 설정 가능

컴포넌트 함수의 파라미터로 받아와서 사용

```jsx
// src/MyComponent.js
import React from 'react';

const MyComponent = props => {
	return <div> 안녕하세요, 제 이름은 {props.name}입니다. </div>;
};
MyComponent.defaultProps = {
	name : 'props를 설정을 별도 안했을 때 출력되는 default 값'
}
export default MyComponent ;

// src/App.js
import React from 'react';
import MyComponent from './MyComponent';

const App = () => {
	return <MyComponent name = "SOOB" />;
};
export default App;
```

## 태그 사이 내용을 보여 주는 children

```jsx
// src/MyComponent.js
import React from 'react';

const MyComponent = props => {
	return (
		<div>
			안녕하세요, 제 이름은 {props.name}입니다. <br />
			children 값은 {props.children}입니다.
		</div>;
	)
};
MyComponent.defaultProps = {
	name : 'props를 설정을 별도 안했을 때 출력되는 default 값'
}
export default MyComponent ;
// 안녕하세요, 제 이름은 props를 설정을 별도 안했을 때 출력되는 default 값입ㄴ다.
// children 값은 리액트 입니다.

// src/App.js
import React from 'react';
import MyComponent from './MyComponent';

const App = () => {
	return <MyComponent>리액트</MyComponent>;
}
export default App;
```

## 비구조화 할당 문법

객체에서 값을 추출하는 문법

함수의 파라미터가 객체라면 그 값을 바로 비구조화해서 사용 가능

```jsx
// src/MyComponent.js
// 원본
import React from 'react';

const MyComponent = props => {
	return (
		<div>
			안녕하세요, 제 이름은 {props.name}입니다. <br />
			children 값은 {props.children}입니다.
		</div>;
	)
};
MyComponent.defaultProps = {
	name : 'props를 설정을 별도 안했을 때 출력되는 default 값'
}
export default MyComponent ;

// 비구조화 할당
import React from 'react';

const MyComponent = props => {
	const {name, children} = props;
	return (
		<div>
			안녕하세요, 제 이름은 {name}입니다. <br />
			children 값은 {children}입니다.
		</div>;
	)
};
MyComponent.defaultProps = {
	name : 'props를 설정을 별도 안했을 때 출력되는 default 값'
}
export default MyComponent ;

// 함수의 파라미터가 객체일 경우 비구조화 사용
import React from 'react';

const MyComponent = ({ name, children }) => {
	return (
		<div>
			안녕하세요, 제 이름은 {name}입니다. <br />
			children 값은 {children}입니다.
		</div>;
	);
};
MyComponent.defaultProps = {
	name : 'props를 설정을 별도 안했을 때 출력되는 default 값'
}
export default MyComponent ;
```

## propTypes를 통한 props 검증

컴포넌트의 필수 props 지정

props의 타입을 지정할 때

propTypes 사용

props 타입에 지정한 값 외에 다른 값이 들어가면 오류

```jsx
// MyComponent.js
import React from 'react';
import PropTypes from 'prop-types';
const MyComponent = ({ name, children }) => {
	return (...);
};

MyComponent.defaultProps = {
	name: '기본 이름'
};

MyComponent.propTypes = {
	name: PropTypes.string
};
export default MyComponent;

// App.js
import React from 'react';
import MyComponent from './MyComponent';
const App = () => {
	return <MyComponent name={3}>리액트</MyComponent>;
};
export default App;
// props가 PropTypes에서 지정한 형태와 일치하지 않아
// PropTypes 경고 발생
```

## isRequired를 사용하여 필수 propTypes 설정

propTypes를 지정하지 않을 때 경고 메시지를 띄워줌

```jsx
import React from "react";
import PropTypes from "prop-types";

const MyComponent = ({ name, favoriteNumber, children }) => {
  return (
    <div>
      안녕하세요, 제 이름은 {name}입니다. <br />
      children 값은 {children}
      입니다.
      <br />
      제가 좋아하는 숫자는 {favoriteNumber}입니다.
    </div>
  );
};
MyComponent.defaultProps = {
  name: "기본 이름",
};
MyComponent.propTypes = {
  name: PropTypes.string,
  favoriteNumber: PropTypes.number.isRequired,
};
export default MyComponent;
```

## 클래스형 컴포넌트에서 props 사용하기

render 함수에서 this.props 조회

```jsx
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class MyComponent extends Component {
	render() {
		const {name, favoriteNumber, children } = this.props;
		return(
			<div>
				안녕하세요, 제이름은 {name} 입니다. <br />
				children 값은 {children} 입니다. <br />
				제가 좋아하는 숫자는 {favoriteNumber} 입니다.
			</div>
		)
	}
}

MyComponent.defaultProps = {
	name : Prototype.string,
	favoriteNumber : Prototype.number.isReuquired
};

export default MyComponent;

// defaultProps, propTypes 내부 설정

class MyComponent extends Component {
	static defaultProps = {
		name : '기본 이름'
	};
	static propTypes = {
		name : PropTypes.string,
		favoriteNumber : PropTypes.number.isRequired
	};
	render() {
		const { name, favoriteNumber, children } = this.props;
		return (...);
	}
}
export default MyComponent;
```

<aside>
💡 defaultProps, propTypes는 꼭 설정해야 하는가?

</aside>

협업을 위해서는 해당 컴포넌트에 어떤 props가 필요한지 쉽게 알 수 있어 개발 능률 때문에 설정해야 함
