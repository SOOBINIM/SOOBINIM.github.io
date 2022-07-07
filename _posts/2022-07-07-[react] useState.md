## useState

```jsx
// 리액트 Hooks 중에 하나인 useState
// useState의 반환 값은 배열이다.
// useState의 첫번째 원소는 현재 상태, 두번째 원소는 Setter 함수

// useState 원래 코드
const numberState = useState(0);
// 첫번째 원소 = 현재 상태
const number = numberState[0];
// 두번째 원소 = Setter 함수
const setNumber = numberState[1];

// useState 배열 비구조화 할당 적용한 코드
const [number, setNumber] = useState(0);
```

### 함수형 업데이트

```jsx
const [number, setNumber] = useState(0);

// Setter 함수로 파라미터로 전달 받은 값을 최신 상태로 설정 하는 방법
// Setter 함수를 사용 할때, 업데이트 하고 싶은 새로운 값을 파라미터에 넣어줌
const onIncrease = () = > { setNumber(number + 1); }

// 기존 값을 업데이트 해주는 함수형 업데이트 방법
// 값을 업데이트 하는 함수를 파라미터로 넣음
// 컴포넌트 최적화에 사용
const onIncrease = () = > { setNumber(prevNumber => prevNumber + 1)

<h1>{number}</h1>
<button onClick={onIncrease}>
```
