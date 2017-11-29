# Text formatting

## Strings
JavaScript *string*타입은 16bit unsigned int값으로 이루어진 요소들의 집합이다.

### String literals
작은따옴표`''` 혹은 큰따옴표`""`로 문자를 표현할 수 있다.
```JavaScript
'text'
"text"
```

#### Hexadecimal escape sequences
```JavaScript
'\xA9' // "©"
```

#### Unicode escape sequences
```JavaScript
'\u00A9' // "©"
```

#### Unicode code point escapes
```JavaScript
'\u{2F804}'

// the same with simple Unicode escapes
'\uD87E\uDC04'
```

### String object
*string object*는 *string primitive data type*을 감싸는 wrapper이다.
JavaScript는 string literal들을 일시적으로 string object로 만들어 method들을 사용할 수 있게 만든다. method가 불린 후에 일시적으로 만들어진 object들은 파괴된다.

**하지만!**

실제로 자바스크립트 엔진들은 매번 literal들을 object로 박싱 언박싱하는 일을 피하기 위해 prmitive data type의 object에 method를 invoke해놓았기 때문에 literal에서도 바로 auto-boxing 없이 method를 호출하게 된다.
따라서 오히려 new를 사용하여 string object를 만들게 되면 string object가 wrapping하고 있는 primitive data type object의 method를 호출하기 위해 단계를 더 거치게 되므로 퍼포먼스 저하가 일어나게 된다.

### Multi-line template literals
\` 를 사용해서 여러 줄의 텍스트를 쓸 수 있다.
또한 `${expression}`를 사용하면 텍스트 안에 expression의 값을 출력할 수 있다.

eg.
```JavaScript
var a = 5;
var b = 10;
console.log(`Fifteen is ${a + b} and\nnot ${2 * a + b}.`));
// "Fifteen is 15 and
// not 20."
```

## Internationalization 

ECMAScript Internationalization API는 언어별 문자 비교, 숫자 포매팅, 시간과 날짜 포매팅을 제공하기 위한 것으로 `Intl`오브젝트를 통해 제공한다.


