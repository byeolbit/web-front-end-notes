# 배열을 거꾸로 순회하기(ES6)
```js
let list = ['a','b','c','d'];
```
배열을 거꾸로 순회하는 방법에는 몇가지가 있다.

원시적으로는..
```js
for(let i=list.length-1; i>=0; i--) {
    console.log(list[i]); // 5, 4, 3, 2, 1
}
```
하지만 `for...in`이나 `for...of`를 쓰고 싶다면 어떻게 해아할까?
```js
list = list.reverse();
for(let item in list) {
    ...
}
```
작은 크기의 리스트에서는 문제가 없겠지만, 리스트의 크기가 커진다면 이야기가 달라진다.

iteratable을 이용하면 원래 원본을 건드리지 않고도 좀 더 유연하게 해결할 수 있다.

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
  console.log(item); // 5, 4, 3, 2, 1
}
```
