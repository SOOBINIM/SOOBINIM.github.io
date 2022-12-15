---
categories: javascript
---

# 함수형 프로그래밍

부수효과를 미워한다 → 순수 함수를 만든다

조합성을 강조한다 → 모듈화 수준을 높인다.

순수함수 → 오류를 줄이고 안정성을 높인다

모듈화 수준이 높다 → 생산성을 높인다.

## 순수 함수

원래 있던 값은 그대로 두고

원래 이떤 값을 복제해서 사용

외부 상태에 영향을 주지 않는 것

## 일급 함수

함수를 변수에 담을 수 있는 값으로 다룰 수 있는 것

## add_maker

순수함수, 일급 함수와 클로저가 함께 사용된 예제

```jsx
function add_maker(a) {
  return function (b) {
    return a + b;
  };
}

var add10 = add_maker(10);
console.log(add10(20));
// 30

var add5 = add_maker(5);
var add15 = add_maker(15);

console.log(add5(10));
// 15
console.log(add15(10));
// 25
```

a, b는 중간에 변경되지 않는 순수 함수 이다.

함수를 값으로 다루는 일급 함수 개념도 포함되어 있다.

### 데이터 (객체) 기준

```jsx
duck.moveLeft();
duck.moveRight();
dog.moveLeft();
dog.moveRight();
```

### 함수 기준

```jsx
moveLeft(dog);
moveRight(duck);
moveLeft({ x: 5, y: 2 });
moveRight(dog);
```

### 클로저

- 자신을 내포한느 함수의 컨텍스트에 접근 가능한 함수
- 이미 생명 주기상 끝난 외부 함수의 변수를 참조하는 함수

[[코어 자바스크립트] 05\_클로저](https://soobinim.github.io/javascript/%EC%BD%94%EC%96%B4-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-05_%ED%81%B4%EB%A1%9C%EC%A0%80/)
