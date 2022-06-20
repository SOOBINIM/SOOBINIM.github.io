### 화살표 함수

_매개변수 지정 방법_

```
() => {...}	// 매개변수가 없을 경우
X => {…}	// 매개변수가 한개인 경우, 소괄호를 생략할 수 있다.
(x, y) => {…}	// 매개변수가 여러개인 경우, 소괄호를 생략할 수 없다.
```

_함수 몸체 지정 방법_

```
X => { return x * x }	// 한줄
X => x * x	한줄이라면 위 코드와 동일하다. Return  생략
() => { return {a : 1 }; }
() => ({a :1})	//위 표현과 동일, 객체 반환시 소괄호 사용
() => {
 const x = 10;
 return x * x;
}


Var arr = [1,2,3];
Var pow = arr.map(function(x){
Return x * x;
});

Const arr = [1,2,3];
Const pow = arr.map(x => x * x );
```
