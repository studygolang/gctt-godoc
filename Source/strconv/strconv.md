version: 1.10
## package strconv

  `import "strconv"`

## Overview

Package strconv implements conversions to and from string representations of
basic data types.


Numeric Conversions

The most common numeric conversions are Atoi (string to int) and Itoa (int to
string).

    i, err := strconv.Atoi("-42")
    s := strconv.Itoa(-42)

These assume decimal and the Go int type.

ParseBool, ParseFloat, ParseInt, and ParseUint convert strings to values:

    b, err := strconv.ParseBool("true")
    f, err := strconv.ParseFloat("3.1415", 64)
    i, err := strconv.ParseInt("-42", 10, 64)
    u, err := strconv.ParseUint("42", 10, 64)

The parse functions return the widest type (float64, int64, and uint64), but if
the size argument specifies a narrower width the result can be converted to that
narrower type without data loss:

    s := "2147483647" // biggest int32
    i64, err := strconv.ParseInt(s, 10, 32)
    ...
    i := int32(i64)

FormatBool, FormatFloat, FormatInt, and FormatUint convert values to strings:

    s := strconv.FormatBool(true)
    s := strconv.FormatFloat(3.1415, 'E', -1, 64)
    s := strconv.FormatInt(-42, 16)
    s := strconv.FormatUint(42, 16)

AppendBool, AppendFloat, AppendInt, and AppendUint are similar but append the
formatted value to a destination slice.


String Conversions

Quote and QuoteToASCII convert strings to quoted Go string literals. The latter
guarantees that the result is an ASCII string, by escaping any non-ASCII Unicode
with \u:

    q := Quote("Hello, 世界")
    q := QuoteToASCII("Hello, 世界")

QuoteRune and QuoteRuneToASCII are similar but accept runes and return quoted Go
rune literals.

Unquote and UnquoteChar unquote Go string and rune literals.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func AppendBool(dst []byte, b bool) []byte](#AppendBool)
- [func AppendFloat(dst []byte, f float64, fmt byte, prec, bitSize int) []byte](#AppendFloat)
- [func AppendInt(dst []byte, i int64, base int) []byte](#AppendInt)
- [func AppendQuote(dst []byte, s string) []byte](#AppendQuote)
- [func AppendQuoteRune(dst []byte, r rune) []byte](#AppendQuoteRune)
- [func AppendQuoteRuneToASCII(dst []byte, r rune) []byte](#AppendQuoteRuneToASCII)
- [func AppendQuoteRuneToGraphic(dst []byte, r rune) []byte](#AppendQuoteRuneToGraphic)
- [func AppendQuoteToASCII(dst []byte, s string) []byte](#AppendQuoteToASCII)
- [func AppendQuoteToGraphic(dst []byte, s string) []byte](#AppendQuoteToGraphic)
- [func AppendUint(dst []byte, i uint64, base int) []byte](#AppendUint)
- [func Atoi(s string) (int, error)](#Atoi)
- [func CanBackquote(s string) bool](#CanBackquote)
- [func FormatBool(b bool) string](#FormatBool)
- [func FormatFloat(f float64, fmt byte, prec, bitSize int) string](#FormatFloat)
- [func FormatInt(i int64, base int) string](#FormatInt)
- [func FormatUint(i uint64, base int) string](#FormatUint)
- [func IsGraphic(r rune) bool](#IsGraphic)
- [func IsPrint(r rune) bool](#IsPrint)
- [func Itoa(i int) string](#Itoa)
- [func ParseBool(str string) (bool, error)](#ParseBool)
- [func ParseFloat(s string, bitSize int) (float64, error)](#ParseFloat)
- [func ParseInt(s string, base int, bitSize int) (i int64, err error)](#ParseInt)
- [func ParseUint(s string, base int, bitSize int) (uint64, error)](#ParseUint)
- [func Quote(s string) string](#Quote)
- [func QuoteRune(r rune) string](#QuoteRune)
- [func QuoteRuneToASCII(r rune) string](#QuoteRuneToASCII)
- [func QuoteRuneToGraphic(r rune) string](#QuoteRuneToGraphic)
- [func QuoteToASCII(s string) string](#QuoteToASCII)
- [func QuoteToGraphic(s string) string](#QuoteToGraphic)
- [func Unquote(s string) (string, error)](#Unquote)
- [func UnquoteChar(s string, quote byte) (value rune, multibyte bool, tail string, err error)](#UnquoteChar)
- [type NumError](#NumError)
  - [func (e *NumError) Error() string](#NumError.Error)

### Examples

- [AppendBool](#exampleAppendBool)
- [AppendFloat](#exampleAppendFloat)
- [AppendInt](#exampleAppendInt)
- [AppendQuote](#exampleAppendQuote)
- [AppendQuoteRune](#exampleAppendQuoteRune)
- [AppendQuoteRuneToASCII](#exampleAppendQuoteRuneToASCII)
- [AppendQuoteToASCII](#exampleAppendQuoteToASCII)
- [AppendUint](#exampleAppendUint)
- [Atoi](#exampleAtoi)
- [CanBackquote](#exampleCanBackquote)
- [FormatBool](#exampleFormatBool)
- [FormatFloat](#exampleFormatFloat)
- [FormatInt](#exampleFormatInt)
- [FormatUint](#exampleFormatUint)
- [IsPrint](#exampleIsPrint)
- [Itoa](#exampleItoa)
- [NumError](#exampleNumError)
- [ParseBool](#exampleParseBool)
- [ParseFloat](#exampleParseFloat)
- [ParseInt](#exampleParseInt)
- [ParseUint](#exampleParseUint)
- [Quote](#exampleQuote)
- [QuoteRune](#exampleQuoteRune)
- [QuoteRuneToASCII](#exampleQuoteRuneToASCII)
- [QuoteToASCII](#exampleQuoteToASCII)
- [Unquote](#exampleUnquote)
- [UnquoteChar](#exampleUnquoteChar)

### Package files
 [atob.go](//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atob.go) [atof.go](//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atof.go) [atoi.go](//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atoi.go) [decimal.go](//github.com/golang/go/blob/release-branch.go1.10/src/strconv/decimal.go) [doc.go](//github.com/golang/go/blob/release-branch.go1.10/src/strconv/doc.go) [extfloat.go](//github.com/golang/go/blob/release-branch.go1.10/src/strconv/extfloat.go) [ftoa.go](//github.com/golang/go/blob/release-branch.go1.10/src/strconv/ftoa.go) [isprint.go](//github.com/golang/go/blob/release-branch.go1.10/src/strconv/isprint.go) [itoa.go](//github.com/golang/go/blob/release-branch.go1.10/src/strconv/itoa.go) [quote.go](//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="IntSize">IntSize</span> = intSize</pre>

IntSize is the size in bits of an int or uint value.

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrRange">ErrRange</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;value out of range&#34;)</pre>

ErrRange indicates that a value is out of range for the target type.

<pre>var <span id="ErrSyntax">ErrSyntax</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;invalid syntax&#34;)</pre>

ErrSyntax indicates that a value does not have the right syntax for the target
type.

<h2 id="AppendBool">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atob.go#L20">AppendBool</a>
    <a href="#AppendBool">¶</a></h2>
<pre>func AppendBool(dst []<a href="/builtin/#byte">byte</a>, b <a href="/builtin/#bool">bool</a>) []<a href="/builtin/#byte">byte</a></pre>

AppendBool appends "true" or "false", according to the value of b, to dst and
returns the extended buffer.

<a id="exampleAppendBool"></a>
Example:

    b := []byte("bool:")
    b = strconv.AppendBool(b, true)
    fmt.Println(string(b))

    // Output:
    // bool:true

<h2 id="AppendFloat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/ftoa.go#L40">AppendFloat</a>
    <a href="#AppendFloat">¶</a></h2>
<pre>func AppendFloat(dst []<a href="/builtin/#byte">byte</a>, f <a href="/builtin/#float64">float64</a>, fmt <a href="/builtin/#byte">byte</a>, prec, bitSize <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

AppendFloat appends the string form of the floating-point number f, as generated
by FormatFloat, to dst and returns the extended buffer.

<a id="exampleAppendFloat"></a>
Example:

    b32 := []byte("float32:")
    b32 = strconv.AppendFloat(b32, 3.1415926535, 'E', -1, 32)
    fmt.Println(string(b32))

    b64 := []byte("float64:")
    b64 = strconv.AppendFloat(b64, 3.1415926535, 'E', -1, 64)
    fmt.Println(string(b64))

    // Output:
    // float32:3.1415927E+00
    // float64:3.1415926535E+00

<h2 id="AppendInt">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/itoa.go#L28">AppendInt</a>
    <a href="#AppendInt">¶</a></h2>
<pre>func AppendInt(dst []<a href="/builtin/#byte">byte</a>, i <a href="/builtin/#int64">int64</a>, base <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

AppendInt appends the string form of the integer i, as generated by FormatInt,
to dst and returns the extended buffer.

<a id="exampleAppendInt"></a>
Example:

    b10 := []byte("int (base 10):")
    b10 = strconv.AppendInt(b10, -42, 10)
    fmt.Println(string(b10))

    b16 := []byte("int (base 16):")
    b16 = strconv.AppendInt(b16, -42, 16)
    fmt.Println(string(b16))

    // Output:
    // int (base 10):-42
    // int (base 16):-2a

<h2 id="AppendQuote">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L107">AppendQuote</a>
    <a href="#AppendQuote">¶</a></h2>
<pre>func AppendQuote(dst []<a href="/builtin/#byte">byte</a>, s <a href="/builtin/#string">string</a>) []<a href="/builtin/#byte">byte</a></pre>

AppendQuote appends a double-quoted Go string literal representing s, as
generated by Quote, to dst and returns the extended buffer.

<a id="exampleAppendQuote"></a>
Example:

    b := []byte("quote:")
    b = strconv.AppendQuote(b, `"Fran & Freddie's Diner"`)
    fmt.Println(string(b))

    // Output:
    // quote:"\"Fran & Freddie's Diner\""

<h2 id="AppendQuoteRune">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L146">AppendQuoteRune</a>
    <a href="#AppendQuoteRune">¶</a></h2>
<pre>func AppendQuoteRune(dst []<a href="/builtin/#byte">byte</a>, r <a href="/builtin/#rune">rune</a>) []<a href="/builtin/#byte">byte</a></pre>

AppendQuoteRune appends a single-quoted Go character literal representing the
rune, as generated by QuoteRune, to dst and returns the extended buffer.

<a id="exampleAppendQuoteRune"></a>
Example:

    b := []byte("rune:")
    b = strconv.AppendQuoteRune(b, '☺')
    fmt.Println(string(b))

    // Output:
    // rune:'☺'

<h2 id="AppendQuoteRuneToASCII">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L160">AppendQuoteRuneToASCII</a>
    <a href="#AppendQuoteRuneToASCII">¶</a></h2>
<pre>func AppendQuoteRuneToASCII(dst []<a href="/builtin/#byte">byte</a>, r <a href="/builtin/#rune">rune</a>) []<a href="/builtin/#byte">byte</a></pre>

AppendQuoteRuneToASCII appends a single-quoted Go character literal representing
the rune, as generated by QuoteRuneToASCII, to dst and returns the extended
buffer.

<a id="exampleAppendQuoteRuneToASCII"></a>
Example:

    b := []byte("rune (ascii):")
    b = strconv.AppendQuoteRuneToASCII(b, '☺')
    fmt.Println(string(b))

    // Output:
    // rune (ascii):'\u263a'

<h2 id="AppendQuoteRuneToGraphic">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L174">AppendQuoteRuneToGraphic</a>
    <a href="#AppendQuoteRuneToGraphic">¶</a></h2>
<pre>func AppendQuoteRuneToGraphic(dst []<a href="/builtin/#byte">byte</a>, r <a href="/builtin/#rune">rune</a>) []<a href="/builtin/#byte">byte</a></pre>

AppendQuoteRuneToGraphic appends a single-quoted Go character literal
representing the rune, as generated by QuoteRuneToGraphic, to dst and returns
the extended buffer.

<h2 id="AppendQuoteToASCII">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L120">AppendQuoteToASCII</a>
    <a href="#AppendQuoteToASCII">¶</a></h2>
<pre>func AppendQuoteToASCII(dst []<a href="/builtin/#byte">byte</a>, s <a href="/builtin/#string">string</a>) []<a href="/builtin/#byte">byte</a></pre>

AppendQuoteToASCII appends a double-quoted Go string literal representing s, as
generated by QuoteToASCII, to dst and returns the extended buffer.

<a id="exampleAppendQuoteToASCII"></a>
Example:

    b := []byte("quote (ascii):")
    b = strconv.AppendQuoteToASCII(b, `"Fran & Freddie's Diner"`)
    fmt.Println(string(b))

    // Output:
    // quote (ascii):"\"Fran & Freddie's Diner\""

<h2 id="AppendQuoteToGraphic">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L133">AppendQuoteToGraphic</a>
    <a href="#AppendQuoteToGraphic">¶</a></h2>
<pre>func AppendQuoteToGraphic(dst []<a href="/builtin/#byte">byte</a>, s <a href="/builtin/#string">string</a>) []<a href="/builtin/#byte">byte</a></pre>

AppendQuoteToGraphic appends a double-quoted Go string literal representing s,
as generated by QuoteToGraphic, to dst and returns the extended buffer.

<h2 id="AppendUint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/itoa.go#L38">AppendUint</a>
    <a href="#AppendUint">¶</a></h2>
<pre>func AppendUint(dst []<a href="/builtin/#byte">byte</a>, i <a href="/builtin/#uint64">uint64</a>, base <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

AppendUint appends the string form of the unsigned integer i, as generated by
FormatUint, to dst and returns the extended buffer.

<a id="exampleAppendUint"></a>
Example:

    b10 := []byte("uint (base 10):")
    b10 = strconv.AppendUint(b10, 42, 10)
    fmt.Println(string(b10))

    b16 := []byte("uint (base 16):")
    b16 = strconv.AppendUint(b16, 42, 16)
    fmt.Println(string(b16))

    // Output:
    // uint (base 10):42
    // uint (base 16):2a

<h2 id="Atoi">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atoi.go#L192">Atoi</a>
    <a href="#Atoi">¶</a></h2>
<pre>func Atoi(s <a href="/builtin/#string">string</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Atoi returns the result of ParseInt(s, 10, 0) converted to type int.

<a id="exampleAtoi"></a>
Example:

    v := "10"
    if s, err := strconv.Atoi(v); err == nil {
        fmt.Printf("%T, %v", s, s)
    }

    // Output:
    // int, 10

<h2 id="CanBackquote">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L181">CanBackquote</a>
    <a href="#CanBackquote">¶</a></h2>
<pre>func CanBackquote(s <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

CanBackquote reports whether the string s can be represented unchanged as a
single-line backquoted string without control characters other than tab.

<a id="exampleCanBackquote"></a>
Example:

    fmt.Println(strconv.CanBackquote("Fran & Freddie's Diner ☺"))
    fmt.Println(strconv.CanBackquote("`can't backquote this`"))

    // Output:
    // true
    // false

<h2 id="FormatBool">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atob.go#L11">FormatBool</a>
    <a href="#FormatBool">¶</a></h2>
<pre>func FormatBool(b <a href="/builtin/#bool">bool</a>) <a href="/builtin/#string">string</a></pre>

FormatBool returns "true" or "false" according to the value of b

<a id="exampleFormatBool"></a>
Example:

    v := true
    s := strconv.FormatBool(v)
    fmt.Printf("%T, %v\n", s, s)

    // Output:
    // string, true

<h2 id="FormatFloat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/ftoa.go#L34">FormatFloat</a>
    <a href="#FormatFloat">¶</a></h2>
<pre>func FormatFloat(f <a href="/builtin/#float64">float64</a>, fmt <a href="/builtin/#byte">byte</a>, prec, bitSize <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

FormatFloat converts the floating-point number f to a string, according to the
format fmt and precision prec. It rounds the result assuming that the original
was obtained from a floating-point value of bitSize bits (32 for float32, 64 for
float64).

The format fmt is one of 'b' (-ddddp±ddd, a binary exponent), 'e' (-d.dddde±dd,
a decimal exponent), 'E' (-d.ddddE±dd, a decimal exponent), 'f' (-ddd.dddd, no
exponent), 'g' ('e' for large exponents, 'f' otherwise), or 'G' ('E' for large
exponents, 'f' otherwise).

The precision prec controls the number of digits (excluding the exponent)
printed by the 'e', 'E', 'f', 'g', and 'G' formats. For 'e', 'E', and 'f' it is
the number of digits after the decimal point. For 'g' and 'G' it is the total
number of digits. The special precision -1 uses the smallest number of digits
necessary such that ParseFloat will return f exactly.

<a id="exampleFormatFloat"></a>
Example:

    v := 3.1415926535

    s32 := strconv.FormatFloat(v, 'E', -1, 32)
    fmt.Printf("%T, %v\n", s32, s32)

    s64 := strconv.FormatFloat(v, 'E', -1, 64)
    fmt.Printf("%T, %v\n", s64, s64)

    // Output:
    // string, 3.1415927E+00
    // string, 3.1415926535E+00

<h2 id="FormatInt">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/itoa.go#L13">FormatInt</a>
    <a href="#FormatInt">¶</a></h2>
<pre>func FormatInt(i <a href="/builtin/#int64">int64</a>, base <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

FormatInt returns the string representation of i in the given base, for 2 <=
base <= 36. The result uses the lower-case letters 'a' to 'z' for digit values
>= 10.

<a id="exampleFormatInt"></a>
Example:

    v := int64(-42)

    s10 := strconv.FormatInt(v, 10)
    fmt.Printf("%T, %v\n", s10, s10)

    s16 := strconv.FormatInt(v, 16)
    fmt.Printf("%T, %v\n", s16, s16)

    // Output:
    // string, -42
    // string, -2a

<h2 id="FormatUint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/itoa.go#L2">FormatUint</a>
    <a href="#FormatUint">¶</a></h2>
<pre>func FormatUint(i <a href="/builtin/#uint64">uint64</a>, base <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

FormatUint returns the string representation of i in the given base, for 2 <=
base <= 36. The result uses the lower-case letters 'a' to 'z' for digit values
>= 10.

<a id="exampleFormatUint"></a>
Example:

    v := uint64(42)

    s10 := strconv.FormatUint(v, 10)
    fmt.Printf("%T, %v\n", s10, s10)

    s16 := strconv.FormatUint(v, 16)
    fmt.Printf("%T, %v\n", s16, s16)

    // Output:
    // string, 42
    // string, 2a

<h2 id="IsGraphic">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L504">IsGraphic</a>
    <a href="#IsGraphic">¶</a></h2>
<pre>func IsGraphic(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsGraphic reports whether the rune is defined as a Graphic by Unicode. Such
characters include letters, marks, numbers, punctuation, symbols, and spaces,
from categories L, M, N, P, S, and Zs.

<h2 id="IsPrint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L458">IsPrint</a>
    <a href="#IsPrint">¶</a></h2>
<pre>func IsPrint(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsPrint reports whether the rune is defined as printable by Go, with the same
definition as unicode.IsPrint: letters, numbers, punctuation, symbols and ASCII
space.

<a id="exampleIsPrint"></a>
Example:

    c := strconv.IsPrint('\u263a')
    fmt.Println(c)

    bel := strconv.IsPrint('\007')
    fmt.Println(bel)

    // Output:
    // true
    // false

<h2 id="Itoa">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/itoa.go#L22">Itoa</a>
    <a href="#Itoa">¶</a></h2>
<pre>func Itoa(i <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

Itoa is shorthand for FormatInt(int64(i), 10).

<a id="exampleItoa"></a>
Example:

    i := 10
    s := strconv.Itoa(i)
    fmt.Printf("%T, %v\n", s, s)

    // Output:
    // string, 10

<h2 id="ParseBool">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atob.go#L1">ParseBool</a>
    <a href="#ParseBool">¶</a></h2>
<pre>func ParseBool(str <a href="/builtin/#string">string</a>) (<a href="/builtin/#bool">bool</a>, <a href="/builtin/#error">error</a>)</pre>

ParseBool returns the boolean value represented by the string. It accepts 1, t,
T, TRUE, true, True, 0, f, F, FALSE, false, False. Any other value returns an
error.

<a id="exampleParseBool"></a>
Example:

    v := "true"
    if s, err := strconv.ParseBool(v); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }

    // Output:
    // bool, true

<h2 id="ParseFloat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atof.go#L523">ParseFloat</a>
    <a href="#ParseFloat">¶</a></h2>
<pre>func ParseFloat(s <a href="/builtin/#string">string</a>, bitSize <a href="/builtin/#int">int</a>) (<a href="/builtin/#float64">float64</a>, <a href="/builtin/#error">error</a>)</pre>

ParseFloat converts the string s to a floating-point number with the precision
specified by bitSize: 32 for float32, or 64 for float64. When bitSize=32, the
result still has type float64, but it will be convertible to float32 without
changing its value.

If s is well-formed and near a valid floating point number, ParseFloat returns
the nearest floating point number rounded using IEEE754 unbiased rounding.

The errors that ParseFloat returns have concrete type *NumError and include
err.Num = s.

If s is not syntactically well-formed, ParseFloat returns err.Err = ErrSyntax.

If s is syntactically well-formed but is more than 1/2 ULP away from the largest
floating point number of the given size, ParseFloat returns f = ±Inf, err.Err =
ErrRange.

<a id="exampleParseFloat"></a>
Example:

    v := "3.1415926535"
    if s, err := strconv.ParseFloat(v, 32); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }
    if s, err := strconv.ParseFloat(v, 64); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }

    // Output:
    // float64, 3.1415927410125732
    // float64, 3.1415926535

<h2 id="ParseInt">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atoi.go#L146">ParseInt</a>
    <a href="#ParseInt">¶</a></h2>
<pre>func ParseInt(s <a href="/builtin/#string">string</a>, base <a href="/builtin/#int">int</a>, bitSize <a href="/builtin/#int">int</a>) (i <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

ParseInt interprets a string s in the given base (0, 2 to 36) and bit size (0 to
64) and returns the corresponding value i.

If base == 0, the base is implied by the string's prefix: base 16 for "0x", base
8 for "0", and base 10 otherwise. For bases 1, below 0 or above 36 an error is
returned.

The bitSize argument specifies the integer type that the result must fit into.
Bit sizes 0, 8, 16, 32, and 64 correspond to int, int8, int16, int32, and int64.
For a bitSize below 0 or above 64 an error is returned.

The errors that ParseInt returns have concrete type *NumError and include
err.Num = s. If s is empty or contains invalid digits, err.Err = ErrSyntax and
the returned value is 0; if the value corresponding to s cannot be represented
by a signed integer of the given size, err.Err = ErrRange and the returned value
is the maximum magnitude integer of the appropriate bitSize and sign.

<a id="exampleParseInt"></a>
Example:

    v32 := "-354634382"
    if s, err := strconv.ParseInt(v32, 10, 32); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }
    if s, err := strconv.ParseInt(v32, 16, 32); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }

    v64 := "-3546343826724305832"
    if s, err := strconv.ParseInt(v64, 10, 64); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }
    if s, err := strconv.ParseInt(v64, 16, 64); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }

    // Output:
    // int64, -354634382
    // int64, -3546343826724305832

<h2 id="ParseUint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atoi.go#L40">ParseUint</a>
    <a href="#ParseUint">¶</a></h2>
<pre>func ParseUint(s <a href="/builtin/#string">string</a>, base <a href="/builtin/#int">int</a>, bitSize <a href="/builtin/#int">int</a>) (<a href="/builtin/#uint64">uint64</a>, <a href="/builtin/#error">error</a>)</pre>

ParseUint is like ParseInt but for unsigned numbers.

<a id="exampleParseUint"></a>
Example:

    v := "42"
    if s, err := strconv.ParseUint(v, 10, 32); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }
    if s, err := strconv.ParseUint(v, 10, 64); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }

    // Output:
    // uint64, 42
    // uint64, 42

<h2 id="Quote">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L101">Quote</a>
    <a href="#Quote">¶</a></h2>
<pre>func Quote(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Quote returns a double-quoted Go string literal representing s. The returned
string uses Go escape sequences (\t, \n, \xFF, \u0100) for control characters
and non-printable characters as defined by IsPrint.

<a id="exampleQuote"></a>
Example:

    s := strconv.Quote(`"Fran & Freddie's Diner	☺"`)
    fmt.Println(s)

    // Output:
    // "\"Fran & Freddie's Diner\t☺\""

<h2 id="QuoteRune">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L140">QuoteRune</a>
    <a href="#QuoteRune">¶</a></h2>
<pre>func QuoteRune(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#string">string</a></pre>

QuoteRune returns a single-quoted Go character literal representing the rune.
The returned string uses Go escape sequences (\t, \n, \xFF, \u0100) for control
characters and non-printable characters as defined by IsPrint.

<a id="exampleQuoteRune"></a>
Example:

    s := strconv.QuoteRune('☺')
    fmt.Println(s)

    // Output:
    // '☺'

<h2 id="QuoteRuneToASCII">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L154">QuoteRuneToASCII</a>
    <a href="#QuoteRuneToASCII">¶</a></h2>
<pre>func QuoteRuneToASCII(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#string">string</a></pre>

QuoteRuneToASCII returns a single-quoted Go character literal representing the
rune. The returned string uses Go escape sequences (\t, \n, \xFF, \u0100) for
non-ASCII characters and non-printable characters as defined by IsPrint.

<a id="exampleQuoteRuneToASCII"></a>
Example:

    s := strconv.QuoteRuneToASCII('☺')
    fmt.Println(s)

    // Output:
    // '\u263a'

<h2 id="QuoteRuneToGraphic">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L168">QuoteRuneToGraphic</a>
    <a href="#QuoteRuneToGraphic">¶</a></h2>
<pre>func QuoteRuneToGraphic(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#string">string</a></pre>

QuoteRuneToGraphic returns a single-quoted Go character literal representing the
rune. The returned string uses Go escape sequences (\t, \n, \xFF, \u0100) for
non-ASCII characters and non-printable characters as defined by IsGraphic.

<h2 id="QuoteToASCII">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L114">QuoteToASCII</a>
    <a href="#QuoteToASCII">¶</a></h2>
<pre>func QuoteToASCII(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

QuoteToASCII returns a double-quoted Go string literal representing s. The
returned string uses Go escape sequences (\t, \n, \xFF, \u0100) for non-ASCII
characters and non-printable characters as defined by IsPrint.

<a id="exampleQuoteToASCII"></a>
Example:

    s := strconv.QuoteToASCII(`"Fran & Freddie's Diner	☺"`)
    fmt.Println(s)

    // Output:
    // "\"Fran & Freddie's Diner\t\u263a\""

<h2 id="QuoteToGraphic">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L127">QuoteToGraphic</a>
    <a href="#QuoteToGraphic">¶</a></h2>
<pre>func QuoteToGraphic(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

QuoteToGraphic returns a double-quoted Go string literal representing s. The
returned string uses Go escape sequences (\t, \n, \xFF, \u0100) for non-ASCII
characters and non-printable characters as defined by IsGraphic.

<h2 id="Unquote">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L340">Unquote</a>
    <a href="#Unquote">¶</a></h2>
<pre>func Unquote(s <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

Unquote interprets s as a single-quoted, double-quoted, or backquoted Go string
literal, returning the string value that s quotes. (If s is single-quoted, it
would be a Go character literal; Unquote returns the corresponding one-character
string.)

<a id="exampleUnquote"></a>
Example:

    test := func(s string) {
        t, err := strconv.Unquote(s)
        if err != nil {
            fmt.Printf("Unquote(%#v): %v\n", s, err)
        } else {
            fmt.Printf("Unquote(%#v) = %v\n", s, t)
        }
    }

    s := `\"Fran & Freddie's Diner\t\u263a\"\"`
    // If the string doesn't have quotes, it can't be unquoted.
    test(s) // invalid syntax
    test("`" + s + "`")
    test(`"` + s + `"`)
    test(`'\u263a'`)

    // Output:
    // Unquote("\\\"Fran & Freddie's Diner\\t\\u263a\\\"\\\""): invalid syntax
    // Unquote("`\\\"Fran & Freddie's Diner\\t\\u263a\\\"\\\"`") = \"Fran & Freddie's Diner\t\u263a\"\"
    // Unquote("\"\\\"Fran & Freddie's Diner\\t\\u263a\\\"\\\"\"") = "Fran & Freddie's Diner	☺""
    // Unquote("'\\u263a'") = ☺

<h2 id="UnquoteChar">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/quote.go#L228">UnquoteChar</a>
    <a href="#UnquoteChar">¶</a></h2>
<pre>func UnquoteChar(s <a href="/builtin/#string">string</a>, quote <a href="/builtin/#byte">byte</a>) (value <a href="/builtin/#rune">rune</a>, multibyte <a href="/builtin/#bool">bool</a>, tail <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

UnquoteChar decodes the first character or byte in the escaped string or
character literal represented by the string s. It returns four values:

    1) value, the decoded Unicode code point or byte value;
    2) multibyte, a boolean indicating whether the decoded character requires a multibyte UTF-8 representation;
    3) tail, the remainder of the string after the character; and
    4) an error that will be nil if the character is syntactically valid.

The second argument, quote, specifies the type of literal being parsed and
therefore which escaped quote character is permitted. If set to a single quote,
it permits the sequence \' and disallows unescaped '. If set to a double quote,
it permits \" and disallows unescaped ". If set to zero, it does not permit
either escape and allows both quote characters to appear unescaped.

<a id="exampleUnquoteChar"></a>
Example:

    v, mb, t, err := strconv.UnquoteChar(`\"Fran & Freddie's Diner\"`, '"')
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println("value:", string(v))
    fmt.Println("multibyte:", mb)
    fmt.Println("tail:", t)

    // Output:
    // value: "
    // multibyte: false
    // tail: Fran & Freddie's Diner\"

<h2 id="NumError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atoi.go#L6">NumError</a>
    <a href="#NumError">¶</a></h2>
<pre>type NumError struct {
<span id="NumError.Func"></span>    Func <a href="/builtin/#string">string</a> <span class="comment">// the failing function (ParseBool, ParseInt, ParseUint, ParseFloat)</span>
<span id="NumError.Num"></span>    Num  <a href="/builtin/#string">string</a> <span class="comment">// the input</span>
<span id="NumError.Err"></span>    Err  <a href="/builtin/#error">error</a>  <span class="comment">// the reason the conversion failed (e.g. ErrRange, ErrSyntax, etc.)</span>
}</pre>

A NumError records a failed conversion.

<a id="exampleNumError"></a>
Example:

    str := "Not a number"
    if _, err := strconv.ParseFloat(str, 64); err != nil {
        e := err.(*strconv.NumError)
        fmt.Println("Func:", e.Func)
        fmt.Println("Num:", e.Num)
        fmt.Println("Err:", e.Err)
        fmt.Println(err)
    }

    // Output:
    // Func: ParseFloat
    // Num: Not a number
    // Err: invalid syntax
    // strconv.ParseFloat: parsing "Not a number": invalid syntax

<h3 id="NumError.Error">func (*NumError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/strconv/atoi.go#L12">Error</a>
    <a href="#NumError.Error">¶</a></h3>
<pre>func (e *<a href="#NumError">NumError</a>) Error() <a href="/builtin/#string">string</a></pre>



