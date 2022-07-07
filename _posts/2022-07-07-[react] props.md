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
