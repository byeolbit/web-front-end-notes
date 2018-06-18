# Greedy and lazy quantifiers

**quantifier**는 앞에 온 대상의 양을 결정하는 부호다.
`+`는 1회 이상, `*`는 0회 이상, `?`는 0또는 1회를 의미한다.

기본적으로 **quantifiers**뒤에 아무것도 붙지 않는다면 _greedy_ 알고리즘으로 동작한다. _greedy_ 로 작동한다는 것은 말 그대로 탐욕적이란 말이다. 최대한 큰 결과를 갖도록 동작한다.

## Greedy

```
What's <yours> is mine, and what's <mine> is my own.
```

예를들어 위 문장에서  `<`와 `>`사이에 있는 모든 문자를 매치하고 싶다고 가정해보자.

그러면 정규식은 아래의 코드와 같이 작성할 수 있을 것이다.

```js
let greedy = /<.+>/g;
```

`.`은 `line break`를 제외한 하나의 문자에 일치한다.
플어보면 `<`가 오고, `line break`를 제외한 문자가 하나이상오고 끝에 `>`가 오는 문자열로 해석할 수 있을 것이다.

그리고 위 정규식을 사용하여 문자를 찾아내려면 다음과 같이 할 수 있을것이다.

```js
let text = `What's <yours> is mine, and what's <mine> is my own.`;

text.match(greedy);
```
그리고 그 결과는 다음과 같다.

```
<yours> is mine, and what's <mine>
```

<script async src="//jsfiddle.net/Lcqubryf/2/embed/js,result/dark/"></script>

우리가 찾고 싶었던 결과는 `<yours>`와 `<mine>` 이지만 결과는 달랐다. 이는 위에서 말했듯, _greedy_ 로 작동하기 때문이다.

최대의 결과를 찾아내는 _greedy_ 는 가장 마지막의 `>`를 만날때 까지 문자를 찾아낸 것이다.

우리가 원하는 결과를 얻기 위해서는 _lazy_ 를 사용하여야 한다.

## Lazy

_lazy_ 는 게으르다는 말 그대로 최소의 결과만을 찾아낸다.
`quantifiers`뒤에 `?`를 붙이면 _lazy_ 로 찾아낼 수 있다.

```js
let lazy = /<.+?>/g;
```
아까의 정규식의 `quantifier`뒤에 `?`를 붙여 _lazy_ 로 동작하게 수정하였다. 
```js
text.match(lazy);
```
다시 문자열에 대해서 match를 실행하면 결과는 다음과 같이 바뀐다.
```
<yours>, <mine>
```
<script async src="//jsfiddle.net/Lcqubryf/5/embed/js,result/dark/"></script>

_lazy_ 는 제일 처음에 만나는 `>`에서 매치를 중단했기 때문에, `<yours>`와 `<mine>` 두 개의 텍스트를 찾아낸 것이다.

#### 만약 `?`뒤에 아무것도 오지 않는다면?

뒤에 매치할 패턴이 없으므로 `quantifier`앞에 온 패턴을 처음 찾은 이후로 더 진행하지 않는다.

ex)
```js
let regex = /abc+?/g;
let text = 'abcde abc';
text.match(regex); // abc, abc
```

### lazy로 찾는 다른 방법

```js
let lazy = /<[^<\n]+>/g;
```
위 정규식을 풀이하면 
1. `<`로 시작하고, 
2. `<`이 오지 않으며 `line break`가 아닌 문자들이 한번 이상 반복되다가
3. `>`로 끝나는 문자열

이 된다.

그 결과는 `quantifiers`뒤에 `?`를 사용한것과 다르지 않다.

```js
text.match(lazy); // <yours>, <mine>
```
<script async src="//jsfiddle.net/Lcqubryf/7/embed/js,result/dark/"></script>


## 때에맞는 적절한 활용

좀 더 정확히, `quantifiers`뒤에 오는 `?`는 그 뒤에 오는 패턴에 대해 일치를 한다.
class가 bar인 img태그를 추출하는 정규식의 예제를 보자
```js
let el = '<img src="foo" class="qux"/><img src="baz" class="bar"/>'
let barRegex = /<img src=".*?" class="bar"\/>/g;

el.match(barRegex);
// result is...
// <img src="foo" class="qux"/><img src="baz" class="bar"/>
```
의도대로라면 분명 class가 bar인 img태그 문자열 하나만 나와야 하지만 그렇지 않았다.

이유는 위 정규식이 의미하는 바가 첫`"`에 일치할때까지가 아니라 `" class="bar"`전체에 처음 일치할 때 까지 이기 때문이다.

그래서 첫  `" class="bar"`가 나오는 부분까지 모든 문자들을 포함시킨 것이다.

조건을 제대로 충족시키기 위해서는 조건안에 `"`가 포함되면 안된다는 사항이 들어가 있어야 한다.

그래서 위 정규식은 다음과 같이 고칠 수 있다.
```js
let barRegex = /<img src="[^"]*" class="bar"\/>/g;
```
새 정규식을 분해해보면
1. `<img src="`가 오고
2. `"`가 포함되지 않은 문자들이 0번이상 반복되다가
3. `" class="bar"\/>`이 오게 된다.

그러므로 새 정규식으로 다시 매치를 하면
```js
el.match(barRegex)
// <img src="baz" class="bar"/>
```
의도하는 결과를 얻을 수 있게 된다.

## 참고
- [https://javascript.info/regexp-greedy-and-lazy](https://javascript.info/regexp-greedy-and-lazy)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp#quantifiers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp#quantifiers)
