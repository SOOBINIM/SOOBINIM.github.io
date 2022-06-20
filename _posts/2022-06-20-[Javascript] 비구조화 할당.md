```javascript
const animal = {
    name : ‘쫑아’,
    age : 14
}
console.log(anmial.name)
// 결과 : 쫑아

// anmial. 을 참조하여 사용하지 않고
// “구조분해 = 비구조화 할당”을 사용 하면 더 간결하게 작성가능
================================================================
// 아래는 비구조화 할당 방법을 사용한 예
const animal = {
    name : ‘쫑아’,
    age : 14
}
const {name , age} = animal
console.log(name)
// 결과 : 쫑아

=================================================================
//비구조화 할당을 깊은 객체를 예제로 연습

const deepObject = {
	state : {
		information : {
			name : “쫑아”,
			habit : [”긁어달라하기”,”꼬리흔들기”,”짓기”]
		}
	},
age : 14
};

// 2번에 나눠서 불러오는 방법
const {name, habit} = deepObject.state.information;
const {age} = deepObject

// 1번에 불러오는 방법
const = {
	state : {
		information : {name, habit}
	},
age
} = deepObject;

const extracted = {
	name, // name : name
	habit, // habit : habit
	age, // age : age
}

console.log(extracted)
/* 결과 :
 state : {
		information : {
			name : “쫑아”,
			habit : [”긁어달라하기”,”꼬리흔들기”,”짓기”]
		}
	},
age : 14
*/
```

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
