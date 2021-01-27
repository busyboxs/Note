# C++ user-defined literals

|type|Sytex|
|:---:|:---|
|integer literal|<ul><li>decimal-literal</li><li>octal-literal</li><li>hex-literal</li><li>binary-literal</li></ul> |
|floating literal|<ul><li>digit-sequence</li><li>fractional-constant</li><li>exponent-part</li></ul> |
|character literal|character-literal|
|string literal|string-literal|

## integer literal

* `unsigned` : `u`, `U`
* `long` : `l`, `L`
* `long long` : `ll`, `LL`
* `size` : `z`, `Z` (since C++ 23)
* `separator` : `'`

```cpp
int d = 42;
int o = 052;
int x = 0x2a;
int X = 0X2A;
int b = 0b1010101;

unsigned long long l1 = 18446744073709550592ull; // C++11
unsigned long long l2 = 18'446'744'073'709'550'592llu; // C++14
unsigned long long l3 = 1844'6744'0737'0955'0592uLL; // C++14
unsigned long long l4 = 184467'440737'0'95505'92LLU; // C++14
```

> 当十六进制语法以 `e` 或者 `E` 结尾时，后面紧跟着 `+` 或者 `-` 时，应当在 `e` 或者 `E` 的后面添加空格或者加上括号。

```cpp
auto x = 0xE+2.0;   // error
auto y = 0xa+2.0;   // OK
auto z = 0xE +2.0;  // OK
auto q = (0xE)+2.0; // OK
```

## Floating point literal

* `float` : `f`, `F`
* `double` : 默认的小数就是 double，不需要添加后缀
* `long double` : `l`, `L`
* `separator` : `'`
* `exponent-sign` : `e`, `E`
* `exponent-sign` : `p`, `P` (since c++ 17)

```cpp
double a = 1e10;
double a = 1e-5L;

double a = 1.;
double a = 1.e-2;

double a = 3.14;
float a = .1f;
double a = 0.1e-1L;

double a = 0x1ffp10;
double a = 0X0p-1;

double a = 0x1.p0;
double a = 0xf.p-1;

double a = 0x0.123p-1;
long double a = 0xa.bp10l;
```

> 对于 hexadecimal floating literal，其指数对应的是 $2^n$

```cpp
double d = 0x1.4p3; // hex fraction 1.4 (decimal 1.25) scaled by 2^3, that is 10.0
```

## Character literal

* `Ordinary character literal` or `narrow character literal` : 没有前后缀
* `UTF-8` : `u8`
* `UTF-16` : `u`
* `UTF-32` : `U`
* `Wide char` : `L`

## string literal

* `Wide string` : `L`
* `UTF-8 Stirng` : `u8`
* `UTF-16 String` : `u`
* `UTF-32 String` : `U`
* `Raw String` : `R"delimiter(raw_characters)delimeter"` 当原始字符串中有括号时可能需要使用定界符

## Others

|header|operator|
|:---:|:---|
|`std::complex`|image part<ul><li>`operator""if`</li><li>`operator""i`</li><li>`operator""il`</li></ul>
|`std::chrono::duration`|<ul><li>`operator""h`</li><li>`operator""min`</li><li>`operator""s`</li><li>`operator""ms`</li><li>`operator""us`</li><li>`operator""ns`</li><li>`operator""y`(C++ 20)</li><li>`operator""d`(C++ 20)</li></ul>|
|`std::literals::string`|`operator""s`|
|`std::literals::string_view`|`operator""sv`|