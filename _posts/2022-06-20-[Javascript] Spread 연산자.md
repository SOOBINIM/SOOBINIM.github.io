### Spread 연산자

연산자의 대상 또는 배열 또는 이터러블 개별 요소로 분리

```
Console.log(…[1,2,3]) // 1,2,3
Console.log(…'HELLO'); // H E L L O
Console.log(…new Map([['a', '1'], ['b', 2]]))

Const arr = [1,2,3];
Console.log(…arr, 4,5,6); = console.log(arr.concat([4,5,6]));

```

혼동 주의

Rest 파라미터 와 혼동 할 수 있다.

Spread 연산자를 사용한 매개변수 정의를 Rest 파라미터라고 한다.

객체 병합

`Const merged = {…{x:1, y:2}, …{y:10, z:3}}; // {x:1, y:10, z:3}`

특정 프로퍼티 변경

`Const chaged = {…{x:1, y:2}, y:100}; // {x:1, y: 100}`

프로퍼티 추가

`Const added = {…{x:1, y:2}, z:0} // {x:1, y:2, z:0}`

객체 안에 연산자 변경

```
Let Object = {
A: '1',
B: '2',
C: {
	D: '3',
	E: '4',
	F: {
		Change_this_value: '0',
		This_stays_same: '6'
		}
	}
}

Let changed = {
	…Object,
	C: {
		…Object.c,
		F: {
			…Object.c.f,
			Change_this_value : '5'
		}
	}
}
```
