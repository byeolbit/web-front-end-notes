# Keyed collections

## Map

### Map object
맵 오브젝트는 키와 값으로 이루어진 간단한 맵이다.
`for [key, value] of map`으로 삽입 순서대로 순회하며 내용을 살펴볼 수 있다.


### Object and Map compared

- Object의 키는 Strings이며, Map의 모든 값을 가질 수 있다.
- Object는 사이즈를 계속해서 직접 추적해야 하지만, Map은 사이즈를 쉽게 얻을 수 있다.
- Map은 삽입된 순서대로 반복된다.

Map과 Object를 어떻게 선택해야 하는가?
- 실행 시까지 키를 알수 없고, 모든 키가 동일한 type이며 모든 값들이 동일한 type일 때 Map사용
- key들을 primitive type으로 저장해야 하는 경우에는 Map을 사용
- 각 요소에 대해 실행될 로직이 있을 경우에는 Object를 사용

### Weakmap object
WeakMap object는 key로는 object만을 가지고, value는 임의 값을 허용하는 key/value쌍의 collection이다.
key들이 참조하고 있는 object에 대한 참조가 더이상 존재하지 않을 경우 GC의 대상이 되는 *weak*map이다. 


## Set
Set object는 값들의 collection이다. 삽입한 순서에따라 저장된 요소를 반복처리할 수 있다. Set은 중복된 값을 허용하지 않는다. 따라서 특정 값은 Set내에서 하나만 존재 하게 된다. 

### Converting between array and set
Set과 Array는 다음과 같은 방법들로 서로 변환될 수 있다.

```JavaScript
// Set to array
Array.from(set)
[...set]

// Array to set
new Set(array);
```

### Array and Set compared
- element가 collection안에 있는지 array안에서 indexOf를 사용하는것은 느리다.
- Set은 value만으로 element의 삭제가 가능하지만, array에선 index를 알아야 한다.
- Array에서는 NaN값을 indexOf로 찾을 수 없다.
- Set은 고유한 값만을 다루기 때문에, array처럼 계속해서 중복된 항목을 체크할 필요가 없다.

### WeakSet object
- `Set`과는 다르게 오직 **object**로만 이루어진 collection이다.
- Collectoin내의 참조는 *weak*하기 때문에 객체에 대한 다른 참조가 없어지면 GC의 대상이 된다.
- 내용뮬을 열거할 수 없다.

eg.
```JavaScript
var ws = new WeakSet();
var obj = {};
var foo = {};

ws.add(window);
ws.add(obj);

ws.has(window); // true
ws.has(foo);    // false, foo has not been added to the set

ws.delete(window); // removes window from the set
ws.has(window);    // false, window has been removed
```
