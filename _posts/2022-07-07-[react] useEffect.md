# useEffect

## 첫 번째 파라미터에는 함수

### 함수를 반환 할 수 있는데 이를 cleanup(useEffect에 대한 뒷정리) 함수라고 부름

## 두번째 파라미터에는 의존값 (배열)

### -> 배열이 없을 때

    컴포넌트가 처음 나타날때에만 useEffect에 등록한 함수가 호출

    컴포넌트가 사라질 때 cleanup 함수 호출

### -> 배열이 있을 때

    컴포넌트가 처음 마운트 될 때 호출, 지정 값이 바뀔 때 호출

    언마운터시에 호출, 값이 바뀌기 직전에도 호출

    useEffect 안에서 사용하는 state, props가 있다면 두번째 파라미터에 넣어줘야 함

    (안넣어주면 최신화 안됨)

---

## 사용 사례

```jsx
useEffect(() => {
  console.log("user 값이 설정됨");
  console.log(user);
  return () => {
    console.log("user가 바뀌기 전..");
    console.log(user);
  };
}, [user]);
```

---

### 마운트

- props 로 받은 값을 컴포넌트의 로컬 상태로 설정
- 외부 API 요청 (REST API)
- 라이브러리 사용 (D3, Video.js)
- setInterval을 통한 반복작업 혹은 setTimeout을 통한 작업 예약

### 언마운트

- setInterval, setTimeout을 사용하여 등록한 작업들 clear 하기 (clearInterval, clearTimeout)
- 라이브러리 인스턴스 제거
