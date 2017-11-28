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



