## useRef

```jsx
// DOM 접근 하기 위해 사용
// React -> useRef
// Javascript -> getElementById, querySelector 사용

// Ref 객체 만들기
const nameInput = useRef();
// Ref 객체의 .current 값으로 DOM 에 접근
nameInput.current.focus();

// useRef 안에 파라미터 값은 current 값의 기본값
// 예를 들면 회원 추가를 할 때, 아이디 값에 해당
userRef(값);
```
