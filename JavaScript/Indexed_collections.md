# Indexed collection

## Array
Array는 순서가 있는 콜렉션이다. 콜렉션의 각 아이템에 이름과 인덱스를 통해 접근할 수 있다.

JavaScript는 명시적으로 array data type을 가지고 있지는 않다. 하지만 빌트인 Array 오브젝트와 그 메소드들을 사용하여 array를 다룰 수 있다.

Array에 정수가 아닌 인덱스로 값을 할당할 경우에, Array element로 추가되지 않고 해당 값으로 새로운 property가 추가된다.

## Typed array
JavaScript typed array는 raw binary data에 접근을 하기 위한 메커니즘을 제공하는 array-like object이다.

|Type|	Value Range|	Size in bytes|	Description	|Web IDL type	|Equivalent C type|
|----|-------------|---------------|-----|-----|-----|
|Int8Array	|-128 to 127|	1	|8-bit two's complement signed integer	|byte	|int8_t
|Uint8Array	|0 to 255|	1	|8-bit unsigned integer	|octet	|uint8_t
|Uint8ClampedArray|	0 to 255|	1	|8-bit unsigned integer (clamped)	|octet	|uint8_t
|Int16Array|	-32768 to 32767|	2	|16-bit two's complement signed |integer|	short	int16_t
|Uint16Array|	0 to 65535|	2	|16-bit unsigned integer	|unsigned short|	uint16_t
|Int32Array|	-2147483648 to 2147483647|	4	|32-bit two's complement |signed integer	|long	int32_t
|Uint32Array|	0 to 4294967295|	4	|32-bit unsigned integer	|unsigned long|	uint32_t
|Float32Array|	1.2x10-38 to 3.4x1038|	4	|32-bit IEEE floating point number ( 7 significant digits e.g. 1.1234567)	|unrestricted float	|float
|Float64Array|	5.0x10-324 to 1.8x10308|	8	|64-bit IEEE floating point number (16 significant digits e.g. 1.123...15)	|unrestricted double	|double
