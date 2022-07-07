# useMemo

- 특정 연산된 값을 재사용/ 새로 만들어야 함

  -> 첫번째 파라미터 : 어떻게 연산할지 정의하는 함수

  -> 두번째 파라미터 : 배열

- 배열 안에 넣은 내용이 바뀌면, 우리가 등록한 함수를 호출해서 값을 연산

- 내용이 바뀌지 않으면, 이전에 연산한 값을 재사용

---

## 사용 사례

```jsx
function countActiveUsers(users) {
  console.log("활성 사용자 수를 세는 중...");
  return users.filter((user) => user.active).length;
}

const count = useMemo(() => countAcitveUsers(users), [users]);
```

---

## 사용 이유

활성 사용자 수를 세는건, users에 변화가 있을때만 세야하는데,

input값이 바뀔 때에도 컴포넌트가 리렌더링 되므로

useMemo를 사용한다.

---

## 참고

리렌더링 되는 조건

- 부모 컴포넌트 렌더링
  - 부모 컴포넌트가 렌더링되면 자식 컴포넌트들 모두 리렌더링
  - 코드양이 많아지면 렌더링량이 많아져 최적화 필수
- state 변경
  - setState가 아닌 직접 변경시 감지 못함
- 새로운 props
