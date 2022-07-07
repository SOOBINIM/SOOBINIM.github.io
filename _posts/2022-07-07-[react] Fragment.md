## Fragment

````
app.js 에서 <div></div>로 무조건 감싸야하는데,

table 태그의 경우 div로 감싸는게 의미가 없다.

이럴 때 사용하는 태그가 Fragment 태그

사용방법 : <> </> 사이에 넣으면 된다.```
````

## 비구조화 할당 (함수)

```jsx
// 함수 매개변수에 "props" 사용
function Hello(props){retrun <div style={{color : props.color}}>{props.name}</div>}

// 함수 매개변수에 "비구조화 할당" 사용
function Hello({color, name}){return <div style={{color}}>{name}</div>}
```

## 비구조화 할당 (배열)

```jsx
// 비구조화 할당은 객체에만 가능한것이 아니다.

// 배열 비구조화 할당 사용하기 전 일반 배열
const array = [1, 2];
console.log(array[0]); // 1
console.log(array[1]); // 2

// 배열 비구조화 할당 사용
const array = [1];
const [one, two = 2] = array; // 기본값 지정 가능
console.log(one); // 1
console.log(two); // 2
```

## props 값 설정 생략 하면 = {true}

```jsx
// props 값 생략
function App(){
	return (
		<Hello name="react", color="red", isSpecial>
		// isSpecial == isSpecial={true} 같음
 );
}
```

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
