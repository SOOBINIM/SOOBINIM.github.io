```javascript
// && 연산자로 코드 단축시키기

const dog = {
	name : 쫑아
}

functin getName(animal){
	if (animal){
		return animal.name
	}
	return undefined
}

const nameEmpty = getName()
console.log(nameEmpty)
// 결과 : undefinded

const nameTrue = getName(dog)
// 결과 : 쫑아

// 위에서 조건문을 간결하게 작성할 수 있다.
// 단추 평가 논리 계산법을 적용
const dog = {
	name : 쫑아
}

functin getName(animal){

	return animal && animal.name
/* 단축 평가 논리 계산법
	if (animal){
		return animal.name
	}
	return undefined
*/
}

const nametrue = getName(dog);
console.log(name);
// 결과 : 쫑아

/*
	작동이유 :
	A && B 연산시 A가 true 이면 B가 결과값
	A && B 연산시 A가 false 이면 A가 결과값

	animal && animal.name 연산시 animal 값이 객체로 dog를 받았을 경우 true 이기 때문에
															결과 값은 animal.name
	animal && animal.name 연산시 animal 값이 객체로 아무것도 안받았 을 경우
															undefined 즉 false이기 때문에 결과값은 animal
															animal은 undefinded이기 때문에 결과값은 undefined

*/
console.log(true && 'hello'); // hello
console.log(false && 'hello'); // false


// || 연산자로 코드 단축시키기
const dog = {
	name : '' // false
}

functin getName(animal){
	const name =  animal && animal.name
  return name || '이름이 없는 동물입니다.'
}

const nametrue = getName(dog);
console.log(name);
// 결과값 : 이름이 없는 동물입니다.

/*
	작동이유 :
	A || B 연산시 A가 true일 경우 결과는 A
  A || B 연산시 A가 false일 경우 결과는 B

  name || '이름이 없는 동물입니다.' 연산시 name이 '' 즉 false이기 때문에
																		결과값은 '이름이 없는 동물입니다.'

	name || '이름이 없는 동물입니다.' 연산시 name 값이 있는 경우에는 결과값은 name
*/
```
