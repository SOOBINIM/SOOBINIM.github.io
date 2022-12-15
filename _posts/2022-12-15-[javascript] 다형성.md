---
categories: javascript
---

# 다형성

예외적인 데이터가 들어와도 애러가 발생하지 않게 하는것

## \_each의 외부 다형성 높이기

### \_each에 null 넣어도 에러 안나게 하기.

```jsx
// 기존 코드
function _each(list, iter) {
  for (var i = -0; i < list.length; i++) {
    iter(list[i]);
  }
  return list;
}

// null 값을 넣었을 때
_each(null, console.log);
// TypeError: Cannot read properties of null (reading 'length')
```

```jsx
// null 넣어도 에러 안나게 코드 수정

function _curryr(fn) {
  return function (a, b) {
    return arguments.length == 2
      ? fn(a, b)
      : function (b) {
          return fn(b, a);
        };
  };
}

// null 값이 들어오면 undefined를 반환
var _get = _curryr(function (obj, key) {
  return obj == null ? undefined : obj[key];
});

var _length = _get("length");

function _each(list, iter) {
  for (var i = -0, len = _length(list); i < len; i++) {
    iter(list[i]);
  }
  return list;
}

// null 값을 넣을 때
_each(null, console.log);
// 아무 결과도 나오지 않는다.
```

위와 같이 어떤 값이 들어왔을 때, 애러가 나지 않고 흘려 보낼 수 있는 전략을 많이 사용한다.

## \_keys 만들기

```jsx
console.log(Object.keys({ name: "soob", age: 31 }));
// [ 'name', 'age' ]

console.log(Object.keys([1, 2, 3, 4]));
// [ '0', '1', '2', '3' ]

console.log(Object.keys(10));
// []

console.log(Object.keys(null));
// TypeError: Cannot convert undefined or null to object
```

- Object.keys는 키 값만 뽑는 함수이다.
- 배열도 오브젝트입니다.
- null 값을 넣으면 애러가 난다.

## \_keys에서도 \_is_object 인지 검사하여 null 에러 안나게 하기

```jsx
// 타입이 object인지 확인 하는 함수
function _is_object(obj) {
  return typeof obj == "object" && !!obj;
}

// 타입이 object가 아니면 빈값을 반환하는 함수
function _keys(obj) {
  return _is_object(obj) ? Object.keys(obj) : [];
}

console.log(_keys({ name: "soob", age: 31 }));
// [ 'name', 'age' ]

console.log(_keys([1, 2, 3, 4]));
// [ '0', '1', '2', '3' ]

console.log(_keys(10));
// []

console.log(_keys(null));
// []
```

함수형 프로그래밍은 어떠한 형태가 들어오더라도 그럴싸한 값이 나오도록

연속적인 함수 실행에 무리가 없도록 해주어야 한다.

## \_each 외부 다형성 높이기

```jsx
function _curryr(fn) {
  return function (a, b) {
    return arguments.length == 2
      ? fn(a, b)
      : function (b) {
          return fn(b, a);
        };
  };
}

// null 값이 들어오면 undefined를 반환
var _get = _curryr(function (obj, key) {
  return obj == null ? undefined : obj[key];
});

var _length = _get("length");

function _each(list, iter) {
  for (var i = -0, len = _length(list); i < len; i++) {
    iter(list[i]);
  }
  return list;
}

_each(
  {
    13: "ID",
    19: "HD",
    29: "YD",
  },
  function (name) {
    console.log(name);
  }
);

//
```

length 값이 없어 결과창에는 아무일도 일어나지 않는다.

```jsx
function _is_object(obj) {
  return typeof obj == "object" && !!obj;
}

function _keys(obj) {
  return _is_object(obj) ? Object.keys(obj) : [];
}

function _each(list, iter) {
  // 키, 벨류로 이루어져 있는 값이라도 배열로 출력 된다.
  var keys = _keys(list);

  for (var i = -0, len = keys.length; i < len; i++) {
    iter(list[keys[i]]);
  }
  return list;
}

_each(
  {
    13: "ID",
    19: "HD",
    29: "YD",
  },
  function (name) {
    console.log(name);
  }
);

// ID
// HD
// YD
```

키 벨류 값을 \_each함수로 실행 하더라도 \_keys, *is*object에 의해 키 값이 배열로 출력이 되어 해당 배열의 길이를 얻을 수 있게 되었다.

배열의 길이가 있기 때문에 해당 키 값의 벨류 값이 출력 되었다.
