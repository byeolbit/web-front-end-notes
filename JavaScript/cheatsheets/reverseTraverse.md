# Iterator를 이용해서 거꾸로 순회하기(ES6)

## for를 이용한 역순회

여기 리스트가 있다.
리스트를 거꾸로 순회하려고 하면 어떻게 하는게 좋을까?

``` js
let list = ['a','b','c','d','e'];
```

원시적으로는..

``` js
for(let i=list.length-1; i>=0; i--) {
    console.log(list[i]); // e, d, c, b, a
}
```

`for...of`를 쓰고 싶다면 어떻게 해아할까?

## `Array.prototype.reverse`?

``` js
list.reverse();
for(let item of list) {
    console.log(list[i]);
}
```

리스트를 거꾸로 순회하기 위해서 `reverse`를 쓰는것은 매우 간편한 방법일 것이다. 그러나 속사정은 그리 간단하지 않다. `reverse`는 배열을 뒤집기 위해 배열을 다시 순회하는 과정을 거친다.

아래는[ecma-262](https://www.ecma-international.org/ecma-262/6.0/#sec-array.prototype.reverse) 에 적혀있는 `Array.prototype.reverse`의 상세이다. `reverse`는 단순히 뒤집는것만이 아니라 비연속적인 데이터가 있으면 배열의 간격을 채우는 요소를 만드는 과정까지 포함한다. 

```
1. Let O be ToObject(this value).
2. ReturnIfAbrupt(O).
3. Let len be ToLength(Get(O, "length")).
4. ReturnIfAbrupt(len).
5. Let middle be floor(len/2).
6. Let lower be 0.
7. Repeat, while lower ≠ middle
    a. Let upper be len− lower −1.
    b. Let upperP be ToString(upper).
    c. Let lowerP be ToString(lower).
    d. Let lowerExists be HasProperty(O, lowerP).
    e. ReturnIfAbrupt(lowerExists).
    f. If lowerExists is true, then
        i. Let lowerValue be Get(O, lowerP).
        ii. ReturnIfAbrupt(lowerValue).
    g. Let upperExists be HasProperty(O, upperP).
    h. ReturnIfAbrupt(upperExists).
    i. If upperExists is true, then
        i. Let upperValue be Get(O, upperP).
        ii. ReturnIfAbrupt(upperValue).
    j. If lowerExists is true and upperExists is true, then
        i. Let setStatus be Set(O, lowerP, upperValue, true).
        ii. ReturnIfAbrupt(setStatus).
        iii. Let setStatus be Set(O, upperP, lowerValue, true).
        iv. ReturnIfAbrupt(setStatus).
    k. Else if lowerExists is false and upperExists is true, then
        i. Let setStatus be Set(O, lowerP, upperValue, true).
        ii. ReturnIfAbrupt(setStatus).
        iii. Let deleteStatus be DeletePropertyOrThrow (O, upperP).
        iiii. ReturnIfAbrupt(deleteStatus).
    l. Else if lowerExists is true and upperExists is false, then
        i. Let deleteStatus be DeletePropertyOrThrow (O, lowerP).
        ii. ReturnIfAbrupt(deleteStatus).
        iii. Let setStatus be Set(O, upperP, lowerValue, true).
        iiii. ReturnIfAbrupt(setStatus).
    m. Else both lowerExists and upperExists are false,
        i. No action is required.
    n. Increase lower by 1.
8. Return O .
```

때문에 작은 크기의 리스트에서는 문제가 없겠지만, 리스트의 크기가 커진다면 이야기가 달라진다.
또한 `reverse`는 원본을 수정하기 때문에, 원본을 보존하면서 순회를 해야 한다면...

``` js
list.slice().reverse()
```

와 같이 쓸 수 있겠지만.. 메모리사용량도 같이 늘어나게 될 것이다... 어쩌면 `for...of`를 포기하고 `for`를 사용하는것이 나을지도 모른다.

## Custom `iterator`

하지만 `iterator`를 이용하면 좀 더 유연하게 해결할 수 있다.

``` js
list[Symbol.iterator] = function() {
    let i = this.length;
    return {
      next: () => {
        return {
          done: --i < 0,
          value: this[i],
        };
      },
    };
};

for (let item of list) {
  console.log(item); // e, d, c, b, a
}
```

`for...of`는 `iterable`오브젝트에만 사용할 수 있다. 기본적으로 Array, Map, Set, String, TypedArray, arguments등의 오브젝트는 built-in `iterator`가 있기 때문에 `iterable`오브젝트이다. 그래서 따로 `iterator`를 구현해주지 않아도 `for...of`를 통해 순회가 가능하다.

하지만 `iterator`를 유저가 원하는 순서대로 순회하도록 새로 구현해준다면, 정방향이 아닌 임의의 방법으로도 순회가 가능해진다.

> `for...of`는 `iterable`오브젝트에만 사용할 수 있다. `iterable`은 `iterator`메소드를 갖고 있는 오브젝트를 말한다. `iterator`란 `next()`메소드를 제공하는 메소드를 가리키는데, 이 메소드는 프로퍼티로 `done`과 `value`를 가지는 오브젝트를 리턴한다. `next()`가 불렸을 때 리턴한 오브젝트중 `done`의 값이 `true`일 때 순회가 종료된다.

## With `generator`

`generator`를 사용하면 좀더 짧게, 명시적으로 작성할 수 있다. `generator` 오브젝트는  `iterator` 이면서 `iterable` 이다.

> generator함수는 안의 `yield`를 만나면 값을 리턴하고, 상태가 정지된다. 이런 특성은 반복자의 까다로운 내부 상태 변화를 유지에 유리하게 사용될 수 있다. 

``` js
list[Symbol.iterator] = function* () {
  let i = this.length;
  while(i>0) {
    yield this[--i];
  }
};

for (let item of list) {
  console.log(item); // e, d, c, b, a
}
```
