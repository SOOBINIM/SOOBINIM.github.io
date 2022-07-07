## 생성

### onCreate

```jsx
const [users, setUsers] = useState([회원리스트])
const [inputs, setInputs] = useState({입력할 회원정보})

const nextId = useRef(4)
// nextId = user.id 값
const onCreate = () => {
	const user = {
		id : nextId.current,
		// nexId의 current는 useRef를 사용 했기 때문에 DOM에 접근 가능
		username : username,
		email : email
};
setUsers([...users, user]);
setUsers(users.concat(user))
// 배열에 새 항목을 추가할 때
// 2개 방식 모두 기존의 배열을 수정하지 않고, 새로운 원소가 추가된 새로운 배열을 만듬
// 첫번째 방식 : spread 연산자 사용
// 두번째 방식 : concat 함수 사용

setInputs({
	username : '',
	email : ''
	// 입력한 회원정보 값 초기화
})

nextId.current += 1;
// user.id 의 인덱스 값 1증가
}
```

## 삭제

### onRemove

```jsx
// userList 안에 버튼
<button onClick={() => onRemove(user.id)}></button>;

const onRemove = (id) => {
  setUsers(users.filter((user) => user.id !== id));
};
// 버튼 클릭시 해당 user.id 값을 전달 받는다.
// id = 전달 받은 user.id 값
// user.id = 회원 리스트에 있는 id 값
// 회원리스트에 있는 id 값중에 클릭한 id 값을 제외하고 새로운 배열을 만듬
```

## 변경

### onChange

```jsx
const [inputs, setInput] = useState({
  username: "",
  email: "",
});

const onChange = (e) => {
  const { name, value } = e.target;
  // name = e.target.name
  // <input name="여기!!"></input>
  // value = e.target.value
  // 키보드로 입력한 값들
  setInput({
    ...inputs,
    // 기존 값 복사
    [name]: value,
    // 기존 값에 새로운 값 추가
  });
};
```

### onToggle

```jsx
const onToggle = (id) => {
	setUsers(
		users.map((user) => user.id === id ? {...user, activate: !user.activate} : user))}
		// user.id : 리스트에 적혀 있는 id 값
		// id : onClick에서 전달받은 내가 클릭한 id 값

// userList 안에 <b> 안에 스타일
style{{ cursor : "pointer", color : user.active ? "green" : "black"}}
onClick = {() => onToggle(user.id)}
```
