---
categories: javascript
---

# suffle

배열안의 값을 섞기

```jsx
class _suffle {
  static suffle(array) {
    // return array.sort((a, b) => a - b)
    return array.sort(() => (Math.random() > 0.5 ? 1 : -1));
  }
}

console.log("1. 셔플 : ", _suffle.suffle([3, 6, 2, 5, 1, 1]));
// 1. 셔플 :  [ 1, 1, 2, 3, 5, 6 ]
```

# uniq

배열의 중복값 제거

```jsx
class _uniq {
  static uniq(array) {
    return array.filter((item, index) => array.indexOf(item) === index);
  }
}

console.log("2. 유니크 : ", _uniq.uniq([1, 3, 5, 5, 6, 2, 3]));
// 2. 유니크 :  [ 1, 3, 5, 6, 2 ]
```

# chunk

지정 값 만큼 묶기

```jsx
class _chunk {
  static chunk(array, size) {
    let newArray = [];
    let index = 0;

    while (index < array.length) {
      newArray.push(array.slice(index, size + index));
      index += size;
    }
    return newArray;
  }
}

console.log("3. 청크 : ", _chunk.chunk(["a", "b", "c", "d", "e"], 3));
// 3. 청크 :  [ [ 'a', 'b', 'c' ], [ 'd', 'e' ] ]
```

# compact

배열 내에 null, false, undefined, 빈값 제거

```jsx
class _compact {
    static compact(array) {
        return array.filter((item) => {
            if (item === false || item == 0 || item == null || item == '' || item == undefined) {
                return false
            }
            return true
        })
    }
}

console.log('4. 컴백트 : ', _compact.compact([0, 1, false, 2, '', 3]))
4. 컴백트 :  [ 1, 2, 3 ]
```
