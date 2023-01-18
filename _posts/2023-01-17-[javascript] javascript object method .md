---
categories: javascript
---

# Computed Property

```jsx
let a = "age";
const user = {
  name: "soob",
  // age : 30
  [a]: 30,
};

console.log(user);
// { name: 'soob', age: 30 }
```

age : 30 대신에 [a] : 30로 작성 해도 된다.

이를 계산된 프로퍼티라고 한다.

# Object.assign()

`객체 복사`

### 복제가 안되는 예제

```jsx
const user = {
  name: "soob",
  age: 30,
};

const cloneUser = user;
// 메모리 주소인 개체에 대한 참조 값이 저장
console.log(cloneUser);
// { name: 'soob', age: 30 }
user.name = "lim";
// cloneUser 객체의 값을 변경하면 user 값도 같이 변경 된다.
console.log(cloneUser);
// { name: 'lim', age: 30 }
```

객체를 새로운 변수에 대입을 하면 메모리 주소 값이 (참조값만 복사)들어가게된다.

따라서 기존 값이 바뀌면 값이 같이 바뀌게 된다.

### assign 메소드를 사용 했을 때

```jsx
const user = {
  name: "soob",
  age: 30,
};

const newUser = Object.assign({}, user);
// {} : 초깃 값
// 두번째 매개변수 : {} 와 병합될 값
console.log(newUser);
//{ name: 'soob', age: 30 }
user.name = "lim";
// 기존 값을 변경해도 assign 메서드로 복제한 객체의 값은 변경되지 않는다.
console.log(newUser);
//{ name: 'soob', age: 30 }
```

완벽하게 객체가 복사가 되었다.

첫 번째 매개변수는 초깃값이다.

두 번째 매개변수는 초깃값에 병합될 값이다.

### 초깃값을 설정했을 때

```jsx
const user = {
  name: "soob",
  age: 30,
};

const newUser = Object.assign({ gender: "man" }, user);
console.log(newUser);
// 초깃값에 성별 설정 후 기존 객체와 병합
// { gender: 'man', name: 'soob', age: 30 }
```

초깃값에 설정한 값과 기존 객체가 병합되었다.

### 초깃값에 키가 같은 경우

```jsx
const user = {
  name: "soob",
  age: 30,
};

const newUser = Object.assign({ name: "lim" }, user);
// 키 값이 같은 경우
console.log(newUser);
// { name: 'soob', age: 30 }
```

기존 값에 있는 키값을 초깃값으로 설정하게 될 경우

기존값이 초깃값을 덮어 쓰게 된다.

### 2개이상의 객체 합치기

```jsx
const userName = {
  name: "soob",
};

const userAge = {
  age: 30,
};

const userGender = {
  gender: "man",
};

const mergeUser = Object.assign(userName, userAge, userGender);
console.log(mergeUser);
// { name: 'soob', age: 30, gender: 'man' }
```

초깃값 userName 객체에 userAge, userGender을 합쳐 주었다.

# Object.keys()

`키 배열 반환`

### 키 배열 반환

```jsx
const user = {
  name: "soob",
  age: 30,
  gender: "man",
};

const userKeys = Object.keys(user);
console.log(userKeys);
// [ 'name', 'age', 'gender' ]
```

keys 메소드는 객체 프로퍼티 키를 배열로 반환한다.

# Object.values()

`값 배열 반환`

### 값 배열 반환

```jsx
const user = {
  name: "soob",
  age: 30,
  gender: "man",
};

const userValues = Object.values(user);
console.log(userValues);
// [ 'soob', 30, 'man' ]
```

values 메소드는 객체의 값들을 배열로 반환한다.

# Object.entries()

`키와 값 모두 배열로 반환`

### 키와 값을 모두 배열로 반환

```jsx
const user = {
  name: "soob",
  age: 30,
  gender: "man",
};

const userKeysValues = Object.entries(user);
console.log(userKeysValues);
// [ [ 'name', 'soob' ], [ 'age', 30 ], [ 'gender', 'man' ] ]
```

배열 안에 배열로 키와 값이 같이 반환 된다.

# Object.fromEntries()

`배열을 객체로 반환`

### 배열을 객체로 만들어줄 때

```jsx
const userKeysValues = [
  ["name", "soob"],
  ["age", 30],
  ["gender", "man"],
];

const fromEntriesUser = Object.fromEntries(userKeysValues);
console.log(fromEntriesUser);
// { name: 'soob', age: 30, gender: 'man' }
```

배열 값을 객체로 만들어줄 때 fromEntries 메서드를 사용하면 된다.
