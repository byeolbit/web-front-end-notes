# Numbers

Javascript의 모든 숫자는 *double-precision 64-bit binary format IEEE754(number between -(2<sup>53</sup>-1 and 2<sup>53</sup>-1)*. Integer를 따로 정의하고 있지는 않다. `+infinity`와 `-infinity` 그리고 `NaN`을 포함하고 있다.

숫자 리터럴을 decimal, binary, octal, hexadecimal 네가지로 사용할 수 있다.

### Decimal numbers
일반적으로 숫자를 쓰듯이 작성하면 된다.
0을 앞에 붙이는 경우에 다른 진법으로 다뤄질 수 있다.

### Binary numbers
2진법을 표현하기 위해서 숫자의 맨 앞에 `0b`혹은 `0B`를 붙이면 된다. `0b`뒤에 0이나 1의 숫자값이 붙지 않으면 `"Missing binary digits after 0b".`에러를 출력한다.

### Octal numbers
8진법을 표현하기 위해서는 숫자의 맨 앞에 0을 붙이면 된다. 0이후에 붙는 숫자가 0~7의 범위를 넘어선다면 10진수로 취급된다.

### hexadecimal numbers
16진수를 표현하기 위해서 숫자의 제일 앞에 '0x' 혹은 '0X'를 붙인다. 그 뒤에 오는 숫자 범위는`01234567890ABCDEF`가 된다.

# Number object
Number object에는 몇가지 property들과 method들이 있다.

|Property|	Description|
|--------|-------------|
|Number.MAX_VALUE	|The largest representable number
|Number.MIN_VALUE	|The smallest representable number
|Number.NaN	|Special "not a number" value
|Number.NEGATIVE_INFINITY	|Special negative infinite value; returned on overflow
|Number.POSITIVE_INFINITY	|Special positive infinite value; returned on overflow
|Number.EPSILON	|Difference between one and the smallest value greater than one that can be represented as a Number.
|Number.MIN_SAFE_INTEGER	|Minimum safe integer in JavaScript.
|Number.MAX_SAFE_INTEGER	|Maximum safe integer in JavaScript.|


|Method	|Description|
|-------|-----------|
|Number.parseFloat()|	Parses a string argument and returns a floating point number. Same as the global parseFloat() function.
|Number.parseInt()|	Parses a string argument and returns an integer of the specified radix or base. Same as the global parseInt() function.
|Number.isFinite()|	Determines whether the passed value is a finite number.
|Number.isInteger()|	Determines whether the passed value is an integer.
|Number.isNaN()|	Determines whether the passed value is NaN. More robust version of the original global isNaN().
|Number.isSafeInteger()|	Determines whether the provided value is a number that is a safe integer.

# Math object
Math object에는 수학적 계산을 위한 property와 method들이 포함되어있다.

# Date object
JavaScript에는 date타입은 없지만, 빌트인 오브잭트인 date object를 사용하여 날짜와 시간을 계산할 수 있다.



