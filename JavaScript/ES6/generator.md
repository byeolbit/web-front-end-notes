# Generator function

제네레이터는 컨텍스트에서 나갔다가 다시 들어올수 있는 함수들을 말합니다. 컨텍스트는 나간 시점(yield가 실행된 시점)에서 다시 들어오기까지 멈춰있게 됩니다.

## Generator 함수는?

`GeneratorFunction` 생성자와 `function*` 을 사용해서 정의할 수 있습니다.
이렇게 정의된 function은 `Generator`객체를 반환합니다.

### Generator 객체의 생성

#### GeneratorFunction 사용
```js
new GeneratorFunction ([arg1[, arg2[, ...argN]],] functionBody)
```

#### function\* 사용
```js
function* name([param[, param[, ... param]]]) {
   statements
}
```

## Generator 객체?

`generator`객체는 iteratable protocol과 iterator protocol을 준수하는 객체입니다.

### iterable protocol?

iterable 프로토콜은 자바스크립트 객체들이 자기 자신의 값들을 for...of 루프 안에서 순회할수 있도록 정의하거나 수정할 수 있도록 해줍니다. Array나 Map과같은 객체들은 기본적인 순회 행동이 있는 **빌트인 iterable객체** 입니다.

iterator 메소드가 구현되어 있어야 하며, 객체내에  `Symbol.iterator`를 키로 하는 프로퍼티에서 iterator 메소드에 접근할 수 있어야 합니다.

### iterator protocol?

iterator 프로토콜은 (무한하거나 유한한) 값들을 순차적으로 제공하기 위한 방법을 정의합니다.

 `done`과 `value` 두개의 프로퍼티를 포함하는 객체를 리턴하는 next()메소드가 구현되어 있는 객체를 iterator라 부릅니다.
 

### Inside generator object

실제로 Generator 객체 안에 next() 메소드가 존재하고, 또한 [Symbol.iterator]가 존재하는것을 확인 할 수 있습니다.

![generator](http://gdurl.com/cEGY)


## Generator의 사용


### for...of iterator

Generator는 iterable과 iterator 프로토콜을 준수하기 때문에 for...of를 이용한 객체의 순회에도 사용될 수 있습니다.

```js
let list = ['a','b','c','d','e'];

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

### 코루틴?

![routine and coroutine](http://gdurl.com/n_5H)

일반적으로 함수는 실행될때 콜스택으로 올라오고, 리턴되면 콜스택에서 사라집니다. 진입지점은 항상 함수의 시작점이고, 콜스택에서 사라진 함수를 다시 불러올수 없게 됩니다. 이러한 개념을 서브루틴이라 합니다.

Generator는 `yield`를 만나기 전까지 실행되다가, `yield`를 만나면 해당 시점까지의 컨텍스트를 복사해놓고 콜스택에서 제거되었다가, `next()`메소드가 호출되면 저장해두었던 컨텍스트를 불러와 실행합니다.

이렇게 서브루틴과는 다르게 함수의 진입지점을 임의로 지정하여 컨텍스트로 들어가는 개념을 코루틴이라 부릅니다.

다만 제네레이터 함수는 컨텍스트는 저장되지만 돌아갈 위치를 지정할 수는 없으므로 세미-코루틴이라 불립니다.

이러한 특성을 이용해서 비동기 코드를 좀 더 쉽게 짤 수 있습니다.

### Generator와 async

Generator를 이용하면 비동기식 코드를 동기식처럼 작성할 수 있습니다.

#### callback hell
```js
function getId(phoneNumber, callback) { /* … */ }
function getEmail(id, callback) { /* … */ }
function getName(email, callback) { /* … */ }
function order(name, menu, callback) { /* … */ }

function orderCoffee(phoneNumber, callback) {
    getId(phoneNumber, function(id) {
        getEmail(id, function(email) {
            getName(email, function(name) {
                order(name, 'coffee', function(result) {
                    callback(result);
                });
            });
        });
    });
}
```

#### promise
```js
function orderCoffee(phoneNumber) {
    return getId(phoneNumber)
        .then(id => getEmail(id))
        .then(email => getName(email))
        .then(name => order(name, 'coffee'));
}
```

#### generator with promise
```js
function* orderCoffee(phoneNumber) {
    const id = yield getId(phoneNumber);
    const email = yield getEmail(id);
    const name = yield getName(email);
    const result = yield order(name, 'coffee');
    return result;
}


// 각 get*** method는 promise 객체를 반환한다고 가정

const iterator = orderCoffee('010-1010-1111');
let ret;

(function runNext(val) {
    ret = iterator.next(val);

    if (!ret.done) {
        ret.value.then(runNext);
    } else {
        console.log('result : ', ret.value);
    }
})();
```

### async / await in Babel

바벨에서는 generator를 구현한 라이브러리인 [regenerator](https://github.com/facebook/regenerator)를 이용하여 async / await를 구현하고 있습니다.


es6
```js
let func = async (callback) => {
  const result = await callback();
  return result;
}
```

transpiled
```js
"use strict";

function _asyncToGenerator(fn) {
  return function() {
    var gen = fn.apply(this, arguments);
    return new Promise(function(resolve, reject) {
      function step(key, arg) {
        try {
          var info = gen[key](arg);
          var value = info.value;
        } catch (error) {
          reject(error);
          return;
        }
        if (info.done) {
          resolve(value);
        } else {
          return Promise.resolve(value).then(
            function(value) {
              step("next", value);
            },
            function(err) {
              step("throw", err);
            }
          );
        }
      }
      return step("next");
    });
  };
}

var func = (function() {
  var _ref = _asyncToGenerator(
    /*#__PURE__*/ regeneratorRuntime.mark(function _callee(callback) {
      var result;
      return regeneratorRuntime.wrap(
        function _callee$(_context) {
          while (1) {
            switch ((_context.prev = _context.next)) {
              case 0:
                _context.next = 2;
                return callback();

              case 2:
                result = _context.sent;
                return _context.abrupt("return", result);

              case 4:
              case "end":
                return _context.stop();
            }
          }
        },
        _callee,
        undefined
      );
    })
  );

  return function func(_x) {
    return _ref.apply(this, arguments);
  };
})();

```

# 참고
- http://meetup.toast.com/posts/73
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterator_protocol
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*
- https://blueshw.github.io/2016/01/25/python-co-routine-vs-sub-routine/
- https://en.wikipedia.org/wiki/Coroutine
