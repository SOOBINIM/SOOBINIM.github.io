---
categories: javascript
---

# 컬렉션 중심 프로그래밍의 4가지 유형과 함수

## 수집하기 - map, values, pluck 등

## map 함수

```jsx
function _is_object(obj) {
  return typeof obj == "object" && !!obj;
}

function _keys(obj) {
  return _is_object(obj) ? Object.keys(obj) : [];
}

var _length = _get("length");

function _each(list, iter) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    iter(list[keys[i]], keys[i]);
  }
  return list;
}

function _map(list, mapper) {
  var new_list = [];
  _each(list, function (val, key) {
    new_list.push(mapper(val, key));
  });
  return new_list;
}
```

```jsx
var users = [
  { id: 10, name: "ID", age: 36 },
  { id: 20, name: "BJ", age: 32 },
  { id: 30, name: "JM", age: 32 },
  { id: 40, name: "PJ", age: 27 },
  { id: 50, name: "HA", age: 25 },
  { id: 60, name: "JE", age: 26 },
  { id: 70, name: "JI", age: 31 },
  { id: 80, name: "MP", age: 23 },
  { id: 90, name: "FP", age: 13 },
];

console.log(
  _map(users, function (user) {
    return user.name;
  })
);

// 출력 값
// [
//  'ID', 'BJ', 'JM',
//  'PJ', 'HA', 'JE',
//  'JI', 'MP', 'FP'
// ]
```

### values 함수

```jsx
function _map(list, mapper) {
  var new_list = [];
  _each(list, function (val, key) {
    new_list.push(mapper(val, key));
  });
  return new_list;
}

// 배열의 값들을 뽑은 함수
function _values(data) {
  return _map(data, function (val) {
    return val;
  });
}
```

```jsx
var users = [
  { id: 10, name: "ID", age: 36 },
  { id: 20, name: "BJ", age: 32 },
  { id: 30, name: "JM", age: 32 },
  { id: 40, name: "PJ", age: 27 },
  { id: 50, name: "HA", age: 25 },
  { id: 60, name: "JE", age: 26 },
  { id: 70, name: "JI", age: 31 },
  { id: 80, name: "MP", age: 23 },
  { id: 90, name: "FP", age: 13 },
];

console.log(users[0]);
// { id: 10, name: 'ID', age: 36 }

console.log(_keys(users[0]));
// [ 'id', 'name', 'age' ]

console.log(_values(users[0]));
// [ 10, 'ID', 36 ]
```

map 함수를 이용하여 value값 들을 뽑는 values 함수를 만들 수 있다.

### pluck 함수

```jsx
function _map(list, mapper) {
  var new_list = [];
  _each(list, function (val, key) {
    new_list.push(mapper(val, key));
  });
  return new_list;
}

// 키 값을 지정하여 그 해당 키 값의  값들을 출력하는 함수
function _pluck(data, key) {
  return _map(data, function (obj) {
    return obj[key];
  });
}
```

```jsx
var users = [
  { id: 10, name: "ID", age: 36 },
  { id: 20, name: "BJ", age: 32 },
  { id: 30, name: "JM", age: 32 },
  { id: 40, name: "PJ", age: 27 },
  { id: 50, name: "HA", age: 25 },
  { id: 60, name: "JE", age: 26 },
  { id: 70, name: "JI", age: 31 },
  { id: 80, name: "MP", age: 23 },
  { id: 90, name: "FP", age: 13 },
];

function _pluck(data, key) {
  return _map(data, function (obj) {
    return obj[key];
  });
}

console.log(_pluck(users, "age"));
// [
//   36, 32, 32, 27, 25,
//   26, 31, 23, 13
// ]
console.log(_pluck(users, "name"));
// [
//   'ID', 'BJ', 'JM',
//   'PJ', 'HA', 'JE',
//   'JI', 'MP', 'FP'
// ]
console.log(_pluck(users, "id"));
// [
//   10, 20, 30, 40, 50,
//   60, 70, 80, 90
// ]
```

키 값을 지정해서 해당 키값의 내부 값들을 가져올 수 있는 함수

## 거르기 - filter, reject, compact, without 등

### filter 함수

```jsx
function _is_object(obj) {
  return typeof obj == "object" && !!obj;
}

function _keys(obj) {
  return _is_object(obj) ? Object.keys(obj) : [];
}

var _length = _get("length");

function _each(list, iter) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    iter(list[keys[i]], keys[i]);
  }
  return list;
}

function _filter(list, predi) {
  var new_list = [];
  _each(list, function (val) {
    if (predi(val)) new_list.push(val);
  });
  return new_list;
}
```

```jsx
var users = [
  { id: 10, name: "ID", age: 36 },
  { id: 20, name: "BJ", age: 32 },
  { id: 30, name: "JM", age: 32 },
  { id: 40, name: "PJ", age: 27 },
  { id: 50, name: "HA", age: 25 },
  { id: 60, name: "JE", age: 26 },
  { id: 70, name: "JI", age: 31 },
  { id: 80, name: "MP", age: 23 },
  { id: 90, name: "FP", age: 13 },
];

console.log(
  _filter(users, function (user) {
    return user.age > 30;
  })
);

// [
//   { id: 10, name: 'ID', age: 36 },
//   { id: 20, name: 'BJ', age: 32 },
//   { id: 30, name: 'JM', age: 32 },
//   { id: 70, name: 'JI', age: 31 }
// ]
```

### reject 함수

```jsx
function _reject(data, predi) {
  return _filter(data, function (val) {
    return !predi(val);
  });
}

console.log(
  _reject(users, function (user) {
    return user.age > 30;
  })
);

// [
//   { id: 40, name: 'PJ', age: 27 },
//   { id: 50, name: 'HA', age: 25 },
//   { id: 60, name: 'JE', age: 26 },
//   { id: 80, name: 'MP', age: 23 },
//   { id: 90, name: 'FP', age: 13 }
// ]
```

filter 함수 결과 값의 반대가 되는 값이 나온다.

### \_negate 함수를 활용하여 reject 함수 만들기

```jsx
function _negate(func) {
  return function (val) {
    return !func(val);
  };
}

function _reject(data, predi) {
  return _filter(data, _negate(predi));
}

console.log(
  _reject(users, function (user) {
    return user.age > 30;
  })
);

// [
//   { id: 40, name: 'PJ', age: 27 },
//   { id: 50, name: 'HA', age: 25 },
//   { id: 60, name: 'JE', age: 26 },
//   { id: 80, name: 'MP', age: 23 },
//   { id: 90, name: 'FP', age: 13 }
// ]
```

언더바 자바스크립트에 선언 되어있는 \_negate함수를 가져다가 써도 된다.

함수를 조합해서 다양한 로직들을 만드는 것을 목표로 한다.

### compact 함수

```jsx
function _filter(list, predi) {
  var new_list = [];
  _each(list, function (val) {
    if (predi(val)) new_list.push(val);
  });
  return new_list;
}

function _compact(data) {
  return _filter(data, function (val) {
    return val;
  });
}

console.log(_compact([0, 1, 2, 3, 4, false, null, undefined, {}]));
// [ 1, 2, 3, 4, {} ]
```

0, null, false, undefined 값들을 제외하고 긍정적인 값(true)들만 남기는 함수

## 찾아내기 - find, some, every 등

## find 함수

```jsx
function _is_object(obj) {
  return typeof obj == "object" && !!obj;
}

function _keys(obj) {
  return _is_object(obj) ? Object.keys(obj) : [];
}

function _find(list, predi) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    var val = list[keys[i]];
    if (predi(val)) return val;
  }
}
```

```jsx
var users = [
  { id: 10, name: "ID", age: 36 },
  { id: 20, name: "BJ", age: 32 },
  { id: 30, name: "JM", age: 32 },
  { id: 40, name: "PJ", age: 27 },
  { id: 50, name: "HA", age: 25 },
  { id: 60, name: "JE", age: 26 },
  { id: 70, name: "JI", age: 31 },
  { id: 80, name: "MP", age: 23 },
  { id: 90, name: "FP", age: 13 },
];

console.log(
  _find(users, function (user) {
    return (user.id = 20);
  })
);

// { id: 20, name: 'ID', age: 36 }
```

배열을 돌면서 조건식에 맞는 값을 1개만 반환

### \_find_index

```jsx
function _find_index(list, predi) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    if (predi(list[keys[i]])) return i;
  }
  return -1;
}

console.log(
  _find_index(users, function (user) {
    return user.id == 20;
  })
);

// 1
// 배열 2번째 index 출력
```

### some 함수

```jsx
function _find_index(list, predi) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    if (predi(list[keys[i]])) return i;
  }
  return -1;
}

function _some(data, predi) {
  return _find_index(data, predi) != -1;
}

console.log(
  _some([1, 2, 3, 4, 5, 9, 10, 12], function (val) {
    return val > 10;
  })
);

// true
// 12가 있기 때문에 true
// 12가 없었으면 false
```

하나라도 만족한 값이 나오면 true 아니면 false

예제로는 유저 중에 나이가 20세 미만인 사람들이 있나? 가 있다.

### every 함수

```jsx
function _find_index(list, predi) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    if (predi(list[keys[i]])) return i;
  }
  return -1;
}

function _negate(func) {
  return function (val) {
    return !func(val);
  };
}

function _every(data, predi) {
  return _find_index(data, _negate(predi)) == -1;
}

console.log(
  _every([1, 2, 3, 4, 5, 9, 10], function (val) {
    return val > 0;
  })
);

// true
// 0 보다 전체 배열 값이 다 크기 때문에 true
```

배열에 있는 값이 조건에 전체가 만족해야 true 아니면 false

## 접기 - reduce, min, max, group_by, count_by

### reduce 함수

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

var _get = _curryr(function (obj, key) {
  return obj == null ? undefined : obj[key];
});

function _is_object(obj) {
  return typeof obj == "object" && !!obj;
}

function _keys(obj) {
  return _is_object(obj) ? Object.keys(obj) : [];
}

var _length = _get("length");

function _each(list, iter) {
  var keys = _keys(list);
  for (var i = 0, len = keys.length; i < len; i++) {
    iter(list[keys[i]], keys[i]);
  }
  return list;
}

var slice = Array.prototype.slice;
function _rest(list, num) {
  return slice.call(list, num || 1);
}

function _rest(list, num) {
  return slice.call(list, num || 1);
}

function _reduce(list, iter, memo) {
  if (arguments.length == 2) {
    memo = list[0];
    list = _rest(list);
  }
  _each(list, function (val) {
    memo = iter(memo, val);
  });
  return memo;
}
```

```jsx
console.log(_reduce([1, 2, 3], add, 0));

// 6

// memo = add(0, 1)
// memo = add(memo, 2);
// memo = add(memo, 3);
// return memo

// add(add(add(0,1),2),3);
```

### min 함수

```jsx
function _min(data) {
  return _reduce(data, function (a, b) {
    return a < b ? a : b;
  });
}

console.log(_min([1, 3, 5, 6, -2]));
// -2
```

배열 중 가장 작은 값 출력

### max 함수

```jsx
function _max(data) {
  return _reduce(data, function (a, b) {
    return a > b ? a : b;
  });
}

console.log(_max([1, 3, 5, 6, -2]));

// 6
```

배열 중 가장 큰 값 출력

### min_by 함수

```jsx
function _min_by(data, iter) {
  return _reduce(data, function (a, b) {
    return iter(a) < iter(b) ? a : b;
  });
}

console.log(_min_by([1, 5, 6, -2], Math.abs));

// 1
```

절대값을 씌운 뒤 그 중 가장 작은 값

### max_by 함수

```jsx
function _max_by(data, iter) {
  return _reduce(data, function (a, b) {
    return iter(a) > iter(b) ? a : b;
  });
}

console.log(_max_by([1, 5, 6, -10], Math.abs));

// -10
```

절대값을 씌운 뒤 그 중 가장 큰 값

### group_by 함수

```jsx
function _reduce(list, iter, memo) {
  if (arguments.length == 2) {
    memo = list[0];
    list = _rest(list);
  }
  _each(list, function (val) {
    memo = iter(memo, val);
  });
  return memo;
}

function _group_by(data, iter) {
  return _reduce(
    data,
    function (grouped, val) {
      var key = iter(val);
      (grouped[key] = grouped[key] || []).push(val);
      return grouped;
    },
    {}
  );
}
```

```jsx
var users = [
  { id: 10, name: "ID", age: 36 },
  { id: 20, name: "BJ", age: 32 },
  { id: 30, name: "JM", age: 32 },
  { id: 40, name: "PJ", age: 27 },
  { id: 50, name: "HA", age: 25 },
  { id: 60, name: "JE", age: 26 },
  { id: 70, name: "JI", age: 31 },
  { id: 80, name: "MP", age: 23 },
  { id: 90, name: "FP", age: 13 },
];

console.log(
  _group_by(users, function (user) {
    return user.age;
  })
);

// {
//     '13': [ { id: 90, name: 'FP', age: 13 } ],
//     '23': [ { id: 80, name: 'MP', age: 23 } ],
//     '25': [ { id: 50, name: 'HA', age: 25 } ],
//     '26': [ { id: 60, name: 'JE', age: 26 } ],
//     '27': [ { id: 40, name: 'PJ', age: 27 } ],
//     '31': [ { id: 70, name: 'JI', age: 31 } ],
//     '32': [ { id: 20, name: 'BJ', age: 32 }, { id: 30, name: 'JM', age: 32 } ],
//     '36': [ { id: 10, name: 'ID', age: 36 } ]
//  }
```

\_reduce 함수를 사용하여 유저의 나이로 그룹바이

### count_by 함수

```jsx
function _reduce(list, iter, memo) {
  if (arguments.length == 2) {
    memo = list[0];
    list = _rest(list);
  }
  _each(list, function (val) {
    memo = iter(memo, val);
  });
  return memo;
}

function _count_by(data, iter) {
  return _reduce(
    data,
    function (count, val) {
      var key = iter(val);
      count[key] ? count[key]++ : (count[key] = 1);
      return count;
    },
    {}
  );
}
```

```jsx
var users = [
  { id: 10, name: "ID", age: 36 },
  { id: 20, name: "BJ", age: 32 },
  { id: 30, name: "JM", age: 32 },
  { id: 40, name: "PJ", age: 27 },
  { id: 50, name: "HA", age: 25 },
  { id: 60, name: "JE", age: 26 },
  { id: 70, name: "JI", age: 31 },
  { id: 80, name: "MP", age: 23 },
  { id: 90, name: "FP", age: 13 },
];

console.log(
  _count_by(users, function (user) {
    return user.age;
  })
);

// {
//     '13': 1,
//     '23': 1,
//     '25': 1,
//     '26': 1,
//     '27': 1,
//     '31': 1,
//     '32': 2,
//     '36': 1
//  }
```

\_reduce 함수를 사용하여 유저의 나이로 카운팅
