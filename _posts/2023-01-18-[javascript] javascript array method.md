---
categories: javascript
---

# array.splice()

```jsx
// splice (시작, 갯수, 추가) : 시작 부터 갯수만큼 지우고 그 위치에 추가

let arr = [5, 31, "수빈", 45, "자바스크립트"];
const result = arr.splice(3, 1, "은 잘하고 싶다");
// 삭제된 요소 반환
console.log(result);
// [ 45 ]

// 조건 삭제 후 배열 값
console.log(arr);
// [ 5, 31, '수빈', '은 잘하고 싶다', '자바스크립트' ]
```

2번째 매개변수에 0을 넣으면 제거하지 않고 추가도 할 수 있다.

# array.slice

```jsx
// slice (시작, n번째 자리 앞까지) 반환

let arr = [5, 31, "수빈", 45, "자바스크립트"];

const result = arr.slice(1, 3);
console.log(result);
// [ 31, '수빈' ]
console.log(arr);
// [ 5, 31, '수빈', 45, '자바스크립트' ]
```

매개 변수로 아무것도 넣지 않으면 배열이 복사가 된다.

# array.concat

```jsx
// concat 합쳐서 새 배열 반환

let arr = [5, 31, "수빈", 45, "자바스크립트"];

const result = arr.concat([1, 2], 3);
console.log(result);
// [ 5, 31, '수빈', 45, '자바스크립트', 1, 2, 3 ]
```

concat 메서드 안에 값은 배열이 벗겨져서 들어간다.

# array.forEach(fn)

```jsx
// arr.forEach(fn) : 배열 반복
// arr.forEach((해당 요소, 인덱스, 해당 배열 자체)

let user = ["soob", "lim", "bin"];

user.forEach((item, index, array) => {
  console.log(`item: ${item}, index: ${index}, array : ${array}`);
});

// item: soob, index: 0, array : soob,lim,bin
// item: lim, index: 1, array : soob,lim,bin
// item: bin, index: 2, array : soob,lim,bin
```

# array.indexOf

```jsx
// array.indexOf() : 위치 값 반환
// array.indexOf(찾는 값, 시작할 위치 인덱스)

let arr = [1, 2, 3, 4, 5, 6, 1, 2, 3];

const result = arr.indexOf(1, 1);
console.log(result);
// 6
```

값의 위치 인덱스를 찾아 준다.

# array.includes

```jsx
// array.includes()
// 포함하는지 확인 불리언 값으로 반환

let array = [1, 2, 3];

const result = array.includes(2);
console.log(result);
// true
```

# array.find(fn)

```jsx
// array.find(fn)
// 판별 함수를 만족하는 첫 번째 요소의 값만 반환
// 아닐 경우 undefined 반환

let userList = [
  { name: "soob", age: 31 },
  { name: "lim", age: 25 },
  { name: "bin", age: 40 },
];

const result = userList.find((user) => {
  if (user.age < 35) {
    return true;
  }
  return false;
});

console.log(result);
// { name: 'soob', age: 31 }
```

# array.findIndex(fn)

```jsx
// array.findIndex(fn)
// 판별 함수를 만족하는 첫 번째 요소에 대한 인덱스를 반환
// 아닐 경우 -1 반환

let userList = [
  { name: "soob", age: 31 },
  { name: "lim", age: 25 },
  { name: "bin", age: 40 },
];

const result = userList.findIndex((user) => {
  if (user.age < 35) {
    return true;
  }
  return false;
});

console.log(result);
// 0
```

# array.filter(fn)

```jsx
// array.filter(fn)
// find와 비슷하나
// filter는 만족하는 모든 값을 반환

let userList = [
  { name: "soob", age: 31 },
  { name: "lim", age: 25 },
  { name: "bin", age: 40 },
];

const result = userList.filter((user) => {
  if (user.age < 35) {
    return true;
  }
  return false;
});

console.log(result);
// [ { name: 'soob', age: 31 }, { name: 'lim', age: 25 } ]
```

# array.map(fn)

```jsx
// array.map(fn)
// 함수를 받아 특정 기능을 시해안 후
// 새로운 배열을 반환

let userList = [
  { name: "soob", age: 31 },
  { name: "lim", age: 12 },
  { name: "bin", age: 40 },
];

let newUserList = userList.map((user, index) => {
  return Object.assign({}, user, {
    id: index + 1,
    isAdult: user.age > 19,
  });
});

console.log(newUserList);
// [
//     { name: 'soob', age: 31, id: 1, isAdult: true },
//     { name: 'lim', age: 12, id: 2, isAdult: false },
//     { name: 'bin', age: 40, id: 3, isAdult: true }
// ]
```

# array.join()

```jsx
// array.join()
// default 값은 , 이다

let arr = ["자바스크립트", "잘하고싶습니다", "리액트도!"];

const result = arr.join("-");
console.log(result);
// 자바스크립트-잘하고싶습니다-리액트도!
```

# array.split()

```jsx
// array.split()
// join과 반대로 조건으로 나누어 배열로 반환
// 빈값으로 두면 각 글자마다 잘라서 배열로 반환

let arr = "자바스크립트-잘하고싶습니다-리액트도!";

const result = arr.split("-");
console.log(result);
// [ '자바스크립트', '잘하고싶습니다', '리액트도!' ]
```

# array.isArray()

```jsx
// array.isArray()
// 인자가 Array인지 판별

let userList = [
  { name: "soob", age: 31 },
  { name: "lim", age: 12 },
  { name: "bin", age: 40 },
];

let user = {
  name: "soobin",
  age: 32,
};

console.log(typeof userList);
// object
console.log(typeof user);
// object

console.log(Array.isArray(userList));
// true
console.log(Array.isArray(user));
// false
```

# array.sort(fn)

```jsx
// array.sort(fn)
// 배열 자체가 변경
// 문자열로 고치고 비교

let array = [27, 8, 5, 13];

function fn(a, b) {
  console.log(a, b);
  return a - b;
}

array.sort(fn);
console.log(array);
// 8과 27 비교 -> [8,27,5,13]
// 5와 8 비교 -> [5,8,27,13]
// 13과 5 비교 -> [5,8,27,13]
// 13과 8 비교 -> [5,8,27,13]
// 13과 27 비교 -> [5,8,13,27]

// 결과 [5,8,13,27]
```

# array.reduce(fn)

```jsx
// array.reduce(fn)
// (누적 계산값, 현재 계산값) => { return 계산값 }, 초깃값;

let array = [1, 2, 3, 4, 5];

const result = array.reduce((prev, curr) => {
  return prev + curr;
}, 0);
console.log(result);
// 15
```

```jsx
// array.reduce(fn)
// (누적 계산값, 현재 계산값) => { return 계산값 }, 초깃값;

let userList = [
  { name: "soob", age: 31 },
  { name: "lim", age: 12 },
  { name: "bin", age: 40 },
];

const result = userList.reduce((prev, curr) => {
  if (curr.age < 35) {
    prev.push(curr.name);
  }
  return prev;
}, []);

console.log(result);
// [ 'soob', 'lim' ]
```
