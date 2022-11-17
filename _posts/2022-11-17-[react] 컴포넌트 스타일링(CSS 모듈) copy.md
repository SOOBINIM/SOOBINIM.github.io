---
categories: react
---

# CSS Module

[파일이름]_[클래스이름]_[해시값] 형태로 자동으로 만들어서 컴포넌트 스타일 클래스 이름이 중첩되는 현상을 방지해주는 기술

.module.css 확장자로 파일 저장하면 CSS Module 적용

```css
.wrapper {
  background: black;
  padding: 1rem;
  color: white;
  font-size: 2rem;
}

:global .something {
  font-weight: 800;
  color: aqua;
}
```

CSS Module은 클래스 이름을 지을 때 그 고유성에 대해 고민하지 않아도 됨.

:global을 사용하면 웹 페이지 전역에서 사용 가능

```css
.wrapper {
  background: black;
  padding: 1rem;
  color: white;
  font-size: 2rem;
}

.inverted {
  color: black;
  background: white;
  border: 1px solid black;
}

:global .something {
  font-weight: 800;
  color: aqua;
}
```

```jsx
import React from "react";
import styles from "./CSSModule.module.css";

const CSSModule = () => {
  return (
    // 클래스 이름을 두개 이상 사용 할 때,
    <div className={`${styles.wrapper} ${styles.inverted}`}>
      안녕하세요, 저는 <span className="something">CSS module! 입니다.</span>
    </div>
  );
};

export default CSSModule;
```

className = {styles.[클래스 이름]} 형태로 전달

:global은 그냥 문자열로 전달

여러 클래스를 전달 할 때는 `문자 백틱으로 감싸서 리터럴을 사용하여 문자열을 합쳐서 사용.

---

## classnames

---

CSS 클래스를 조건부로 설정할 때 유용한 라이브러리

```powershell
yarn add classnames
```

```jsx
import classNames from 'classnames';

classNames('one', 'two'); // = 'one two'
classNames('one', { two: true }); // = 'one two'
classNames('one', { two: false }); // = 'one'
classNames('one', ['two', 'three']); // = 'one two three'

const myClass = 'hello';
classNames('one', myClass, { myCondition: true }); // = 'one
hello myCondition'
```

props 값에 따라 다른 스타일을 주기가 쉬움

```jsx
// 일반 코드
const MyComponent = ({ highlighted, theme }) => (
<div className={`MyComponent ${theme} ${highlighted ? 'high
	lighted' : ''}`}>Hello
</div>
);

// classnames 문법
const MyComponent = ({ highlighted, theme }) => (
	<div className={classNames('MyComponent', { highlighted },
	theme)}>Hello</div>
);
```

highlighted 값이 true이면 highlighted 클래스가 적용, false이면 적용 안됨. 추가로 theme으로 전달받는 문자열은 내용 그대로 클래스에 적용

```jsx
// 일반 코드
import React from "react";
import styles from './CSSModule.module.css'

const CSSModule = () => {
    return (
				// 클래스 이름을 두개 이상 사용 할 때,
        <div className={`${styles.wrapper} ${styles.inverted}`}>
            안녕하세요, 저는 <span className="something">
                CSS module! 입니다.
            </span>
        </div>
    )
}

export default CSSModule;

// CSS Module + classnames 문법
import React from 'react';
import classNames from 'classnames/bind';
import styles from './CSSModule.module.css';

const cx = classNames.bind(styles); // 미리 styles에서 클래스를 받아 오도록 설정하고
const CSSModule = () => {
    return (
        <div className={cx('wrapper', 'inverted')}>
            안녕하세요, 저는 <span className="something">CSS Module!</span>
        </div>
    );
};
export default CSSModule;
```

classnames에 내장되어 있는 bind 함수 사용하면

클래스를 넣어줄 때마다 styles.[클래스이름] 안해도됨.

사전에 styles에서 받아 온 후 cx(’클래스 이름’, ‘클래스 이름2’) 형태로 사용 가능

---

## Sass와 함께 사용하기

---

```jsx
.wrapper {
    background: black;
    padding: 1rem;
    color: white;
    font-size: 2rem;

    &.inverted {
        color: black;
        background: white;
        border: 1px solid black;
    }
}

:global {
    .something {
        font-weight: 800;
        color: aqua;
    }
}
```

Sass에도 사용 가능
