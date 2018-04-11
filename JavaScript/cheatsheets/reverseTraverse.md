# 거꾸로 순회하기(ES6)
```js
let list = ['a','b','c','d','e'];
```
리스트를 거꾸로 순회하는 방법에는 몇가지가 있다.

원시적으로는..
```js
for(let i=list.length-1; i>=0; i--) {
    console.log(list[i]); // e, d, c, b, a
}
```
하지만 `for...in`이나 `for...of`를 쓰고 싶다면 어떻게 해아할까?
```js
list.reverse();
for(let item in list) {
    ...
}
```
작은 크기의 리스트에서는 문제가 없겠지만, 리스트의 크기가 커진다면 이야기가 달라진다.

거꾸로인 배열을 만들고 싶다면 달성가능한 방법이지만, 이방법은 뒤에서 부터 세는 것이 아닌, 앞에서부터 순회를 하는것이 되어버린다.

iterator를 이용하면 좀 더 유연하게 해결할 수 있다.

> js에서 iterator란 `next()`메소드를 제공하는 오브젝트를 가리키는데, 이 메소드는 `done`과 `value`를 가지고 있다. `next()`가 불렸을 때 리턴한 오브젝트중 `done`의 값이 `true`일 때 순회가 종료된다.


```js
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

generator를 사용하면 좀더 짧게, 명시적으로 작성할 수 있다.
> generator함수는 안의 `yield`를 만나면 값을 리턴하고, 상태가 정지된다. 이런 특성은 반복자의 까다로운 내부 상태 변화를 유지에 유리하게 사용될 수 있다.

```js
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
