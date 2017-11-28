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
`x = y` 로 x에 y의 값을 assignment
operator와 함께 사용하여, operator의 계산이 이루어진 값을 바로 assign할 수 있다.

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
- **strict comparison**은 비교 대상의 타입과 그 내용이 모두 일치하는 경우에만 true가 된다.
- **abstract comparison**은 비교전에 두 대상의 타입을 일치시키기 위해 *type converting*을 거친다.
