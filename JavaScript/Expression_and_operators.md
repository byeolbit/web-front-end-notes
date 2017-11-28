# Expression and operators

## Operators
- Assignment operators
- Comparison operators
- Arithmetic operators
- Bitwise operators
- Logical operators
- String operators
- Conditional (ternary) operator
- Comma operator
- Unary operators
- Relational operator

### Assignment operators
`x = y` 로 x에 y의 값을 assign한다.<br>
Operator와 함께 사용하여, operator의 계산이 이루어진 값을 바로 assign할 수 있다.

| Name | Shorthand operator | Meaning |
|------|----|----|
|Assignment| x = y	| x = y |
|Addition assignment|x += y|x = x + y
|Subtraction assignment|	x -= y|	x = x - y
|Multiplication assignment|	x *= y|	x = x * y
|Division assignment|	x /= y|	x = x / y
|Remainder assignment|	x %= y|	x = x % y
|Exponentiation assignment|	x **= y|	x = x ** y
|Left shift assignment|	x <<= y|	x = x << y
|Right shift assignment|	x >>= y|	x = x >> y
|Unsigned right shift assignment|	x >>>= y|	x = x >>> y
|Bitwise AND assignment|	x &= y|	x = x & y
|Bitwise XOR assignment|	x ^= y|	x = x ^ y
|Bitwise OR assignment|	x \|= y	| x = x | y

### Comparison operators
- **[strict comparison](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.6)**은 비교 대상의 타입과 그 내용이 모두 일치하는 경우에만 `true`가 된다.
- **[abstract comparison](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3)**은 비교전에 두 대상의 타입을 일치시키기 위해 *type converting*을 거친다.
- 같은 object를 참조하는 비교를 제외하고는, 두 별개의 object는 어떤 비교로도 항상 `false`가 된다.
- `NaN`의 비교는 어떠한 값과도 `false`가 된다

#### Equaility comparison
- 문자와 숫자의 비교에서 문자가 숫자로 변환된다. 숫자로 변환된 문자는 가장 가까운 숫자 타입으로 반올림 된다.
- 비교값중 하나가 Boolean이라면 Boolean은 `true`일 경우 1로, `false`일 경우 +0으로 변환된다.
- Object가 string이나 number와 비교될 때, operator들은 object를 valeOf 혹은 toString 을 이용한 primitive value로 바꾸려 한다. 이러한 Object 변환이 실패하면 런타임 에러가 생성된다.
- `String`의 타입은 `String`이지만, `string object`의 타입은 `Object`이다.

### Arithmetic operators
숫자 값에 대하여 연산을 할 수 있는 연산자를 제공한다.

### Bitwise operators
비트 연산을 하기 위한 operator를 제공한다.

|Operator|	Usage|	Description|
|----|----|----|
|Bitwise AND|	a & b|	Returns a one in each bit position for which the corresponding bits of both operands are ones.|
|Bitwise OR|	a \| b|	Returns a zero in each bit position for which the corresponding bits of both operands are zeros.|
|Bitwise XOR|	a ^ b|	Returns a zero in each bit position for which the corresponding bits are the same. Returns a one in each bit position for which the corresponding bits are different.|
|Bitwise NOT|	~ a|	Inverts the bits of its operand.|
|Left shift|	a << b|	Shifts a in binary representation b bits to the left, shifting in zeros from the right.|
|Sign-propagating right shift	|a >> b	|Shifts a in binary representation b bits to the right, discarding bits shifted off.|
|Zero-fill right shift	|a >>> b	|Shifts a in binary representation b bits to the right, discarding bits shifted off, and shifting in zeros from the left.|

### Logical operators
두 expression을 비교하기 위한 operator이다.

### String operators
String간 `+`를 사용하여 두 string을 연결할 수 있다.

### Conditional (ternary) operator
If문을 다음과 같이 작성할 수 있다.
```JavaScript
condition ? val1 : val2
```

### Comma operator
하나의 expression을 사용하도록 되어있는 곳에 여러개의 expression을 사용할 수 있도록 해줌.

eg.
```JavaScript
for (let i=0, j=9; i=<9; i++, j--) {...}
```

### Unary operators
Operand가 하나만 존재하는 Operator를 Unary Operator라고 한다.

#### delete 
object나 object의 property, 혹은 배열속의 특정 index의 element를 지운다.
eg.
```JavaScript
delete objectName;
delete objectName.property;
delete objectName[index];
```
`delete`가 성공하면, property나 element는 `undefined`가 된다. `delete`는 성공했을 때 `ture`를 리턴하고, 불가능하다면 `false`를 리턴한다.

#### typeof

```JavaScript
typeof operand
```
`operand`의 타입을 리턴한다.

#### void
```Javascript
void expression
```
`void` operator는 실행결과 값이 `undefined`를 반환하도록 요구되는 곳에 `undefined`를 반환값으로 가질 수 있는 expression을 삽입할 경우에 사용

`void(0)`을 사용해서 `undefined`값을 얻을 수 있다.

### Relational operators
Rerational operator는 operend들을 비교하고, 비교가 참인지에 따라 `Boolean`을 반환한다.

#### in
`in`operator는 어떤 property가 object안에 존재한다면 `true`를 반환한다.
```JavaScript
propNameOrNumber in objectName
```

#### instanceof
`instanceof` operator는 object의 type을 반환한다.
```JavaScript
objectname instanceof objectType
```


