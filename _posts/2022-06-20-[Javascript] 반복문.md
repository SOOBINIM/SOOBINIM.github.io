## forEach

```jsx
const heroes = ["아이언맨", "캡틴아메리카", "토르"];

// for 구문 사용
for (let i = 0; i < heroes.length; i++) {
  console.log(heroes[i]);
}
// forEach 방법 (콜백함수)
heroes.forEach((hero) => {
  console.log(hero);
});
```

## MAP

```jsx
const heroes = ["아이언맨", "캡틴아메리카", "토르"];

// 새로운 배열을 만들고 싶을 때
const name = [];

// for 구문 사용
for (let i = 0; i < heroes.length; i++) {
  name.push(heroes[0] + "숩");
}

// forEach 구문 사용
heroes.forEach((hero) => {
  name.push(hero + "숩");
});

// map 구문 사용
const name = heroes.map((soob) => soob + "숩");
```

```
key ={index}

```
