version: 1.9.2
## package constant

  `import "go/constant"`

## Overview

Package constant implements Values representing untyped Go constants and their
corresponding operations.

A special Unknown value may be used when a value is unknown due to an error.
Operations on unknown values produce unknown values unless specified otherwise.

## Index

- [func BitLen(x Value) int](#BitLen)
- [func BoolVal(x Value) bool](#BoolVal)
- [func Bytes(x Value) []byte](#Bytes)
- [func Compare(x_ Value, op token.Token, y_ Value) bool](#Compare)
- [func Float32Val(x Value) (float32, bool)](#Float32Val)
- [func Float64Val(x Value) (float64, bool)](#Float64Val)
- [func Int64Val(x Value) (int64, bool)](#Int64Val)
- [func Sign(x Value) int](#Sign)
- [func StringVal(x Value) string](#StringVal)
- [func Uint64Val(x Value) (uint64, bool)](#Uint64Val)
- [type Kind](#Kind)
- [type Value](#Value)
  - [func BinaryOp(x_ Value, op token.Token, y_ Value) Value](#BinaryOp)
  - [func Denom(x Value) Value](#Denom)
  - [func Imag(x Value) Value](#Imag)
  - [func MakeBool(b bool) Value](#MakeBool)
  - [func MakeFloat64(x float64) Value](#MakeFloat64)
  - [func MakeFromBytes(bytes []byte) Value](#MakeFromBytes)
  - [func MakeFromLiteral(lit string, tok token.Token, zero uint) Value](#MakeFromLiteral)
  - [func MakeImag(x Value) Value](#MakeImag)
  - [func MakeInt64(x int64) Value](#MakeInt64)
  - [func MakeString(s string) Value](#MakeString)
  - [func MakeUint64(x uint64) Value](#MakeUint64)
  - [func MakeUnknown() Value](#MakeUnknown)
  - [func Num(x Value) Value](#Num)
  - [func Real(x Value) Value](#Real)
  - [func Shift(x Value, op token.Token, s uint) Value](#Shift)
  - [func ToComplex(x Value) Value](#ToComplex)
  - [func ToFloat(x Value) Value](#ToFloat)
  - [func ToInt(x Value) Value](#ToInt)
  - [func UnaryOp(op token.Token, y Value, prec uint) Value](#UnaryOp)

### Package files
 [value.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go)

<h2 id="BitLen">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L465">BitLen</a>
    <a href="#BitLen">¶</a></h2>
<pre>func BitLen(x <a href="#Value">Value</a>) <a href="/builtin/#int">int</a></pre>

BitLen returns the number of bits required to represent the absolute value x in
binary representation; x must be an Int or an Unknown. If x is Unknown, the
result is 0.

<h2 id="BoolVal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L360">BoolVal</a>
    <a href="#BoolVal">¶</a></h2>
<pre>func BoolVal(x <a href="#Value">Value</a>) <a href="/builtin/#bool">bool</a></pre>

BoolVal returns the Go boolean value of x, which must be a Bool or an Unknown.
If x is Unknown, the result is false.

<h2 id="Bytes">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L518">Bytes</a>
    <a href="#Bytes">¶</a></h2>
<pre>func Bytes(x <a href="#Value">Value</a>) []<a href="/builtin/#byte">byte</a></pre>

Bytes returns the bytes for the absolute value of x in little- endian binary
representation; x must be an Int.

<h2 id="Compare">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L1176">Compare</a>
    <a href="#Compare">¶</a></h2>
<pre>func Compare(x_ <a href="#Value">Value</a>, op <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a>, y_ <a href="#Value">Value</a>) <a href="/builtin/#bool">bool</a></pre>

Compare returns the result of the comparison x op y. The comparison must be
defined for the operands. If one of the operands is Unknown, the result is
false.

<h2 id="Float32Val">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L417">Float32Val</a>
    <a href="#Float32Val">¶</a></h2>
<pre>func Float32Val(x <a href="#Value">Value</a>) (<a href="/builtin/#float32">float32</a>, <a href="/builtin/#bool">bool</a>)</pre>

Float32Val is like Float64Val but for float32 instead of float64.

<h2 id="Float64Val">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L442">Float64Val</a>
    <a href="#Float64Val">¶</a></h2>
<pre>func Float64Val(x <a href="#Value">Value</a>) (<a href="/builtin/#float64">float64</a>, <a href="/builtin/#bool">bool</a>)</pre>

Float64Val returns the nearest Go float64 value of x and whether the result is
exact; x must be numeric or an Unknown, but not Complex. For values too small
(too close to 0) to represent as float64, Float64Val silently underflows to 0.
The result sign always matches the sign of x, even for 0. If x is Unknown, the
result is (0, false).

<h2 id="Int64Val">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L387">Int64Val</a>
    <a href="#Int64Val">¶</a></h2>
<pre>func Int64Val(x <a href="#Value">Value</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#bool">bool</a>)</pre>

Int64Val returns the Go int64 value of x and whether the result is exact; x must
be an Int or an Unknown. If the result is not exact, its value is undefined. If
x is Unknown, the result is (0, false).

<h2 id="Sign">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L481">Sign</a>
    <a href="#Sign">¶</a></h2>
<pre>func Sign(x <a href="#Value">Value</a>) <a href="/builtin/#int">int</a></pre>

Sign returns -1, 0, or 1 depending on whether x < 0, x == 0, or x > 0; x must be
numeric or Unknown. For complex values x, the sign is 0 if x == 0, otherwise it
is != 0. If x is Unknown, the result is 1.

<h2 id="StringVal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L373">StringVal</a>
    <a href="#StringVal">¶</a></h2>
<pre>func StringVal(x <a href="#Value">Value</a>) <a href="/builtin/#string">string</a></pre>

StringVal returns the Go string value of x, which must be a String or an
Unknown. If x is Unknown, the result is "".

<h2 id="Uint64Val">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L403">Uint64Val</a>
    <a href="#Uint64Val">¶</a></h2>
<pre>func Uint64Val(x <a href="#Value">Value</a>) (<a href="/builtin/#uint64">uint64</a>, <a href="/builtin/#bool">bool</a>)</pre>

Uint64Val returns the Go uint64 value of x and whether the result is exact; x
must be an Int or an Unknown. If the result is not exact, its value is
undefined. If x is Unknown, the result is (0, false).

<h2 id="Kind">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L15">Kind</a>
    <a href="#Kind">¶</a></h2>
<pre>type Kind <a href="/builtin/#int">int</a></pre>

Kind specifies the kind of value represented by a Value.

<pre>const (
    <span class="comment">// unknown values</span>
    <span id="Unknown">Unknown</span> <a href="#Kind">Kind</a> = <a href="/builtin/#iota">iota</a>

    <span class="comment">// non-numeric values</span>
    <span id="Bool">Bool</span>
    <span id="String">String</span>

    <span class="comment">// numeric values</span>
    <span id="Int">Int</span>
    <span id="Float">Float</span>
    <span id="Complex">Complex</span>
)</pre>


<h2 id="Value">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L32">Value</a>
    <a href="#Value">¶</a></h2>
<pre>type Value interface {
    <span class="comment">// Kind returns the value kind.</span>
    Kind() <a href="#Kind">Kind</a>

    <span class="comment">// String returns a short, quoted (human-readable) form of the value.</span>
    <span class="comment">// For numeric values, the result may be an approximation;</span>
    <span class="comment">// for String values the result may be a shortened string.</span>
    <span class="comment">// Use ExactString for a string representing a value exactly.</span>
    String() <a href="/builtin/#string">string</a>

    <span class="comment">// ExactString returns an exact, quoted (human-readable) form of the value.</span>
    <span class="comment">// If the Value is of Kind String, use StringVal to obtain the unquoted string.</span>
    ExactString() <a href="/builtin/#string">string</a>
    <span class="comment">// contains filtered or unexported methods</span>
}</pre>

A Value represents the value of a Go constant.

<h3 id="BinaryOp">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L940">BinaryOp</a>
    <a href="#BinaryOp">¶</a></h3>
<pre>func BinaryOp(x_ <a href="#Value">Value</a>, op <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a>, y_ <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

BinaryOp returns the result of the binary expression x op y. The operation must
be defined for the operands. If one of the operands is Unknown, the result is
Unknown. BinaryOp doesn't handle comparisons or shifts; use Compare or Shift
instead.

To force integer division of Int operands, use op == token.QUO_ASSIGN instead of
token.QUO; the result is guaranteed to be Int in this case. Division by zero
leads to a run-time panic.

<h3 id="Denom">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L604">Denom</a>
    <a href="#Denom">¶</a></h3>
<pre>func Denom(x <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

Denom returns the denominator of x; x must be Int, Float, or Unknown. If x is
Unknown, or if it is too large or small to represent as a fraction, the result
is Unknown. Otherwise the result is an Int >= 1.

<h3 id="Imag">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L652">Imag</a>
    <a href="#Imag">¶</a></h3>
<pre>func Imag(x <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

Imag returns the imaginary part of x, which must be a numeric or unknown value.
If x is Unknown, the result is Unknown.

<h3 id="MakeBool">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L273">MakeBool</a>
    <a href="#MakeBool">¶</a></h3>
<pre>func MakeBool(b <a href="/builtin/#bool">bool</a>) <a href="#Value">Value</a></pre>

MakeBool returns the Bool value for b.

<h3 id="MakeFloat64">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L291">MakeFloat64</a>
    <a href="#MakeFloat64">¶</a></h3>
<pre>func MakeFloat64(x <a href="/builtin/#float64">float64</a>) <a href="#Value">Value</a></pre>

MakeFloat64 returns the Float value for x. If x is not finite, the result is an
Unknown.

<h3 id="MakeFromBytes">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L550">MakeFromBytes</a>
    <a href="#MakeFromBytes">¶</a></h3>
<pre>func MakeFromBytes(bytes []<a href="/builtin/#byte">byte</a>) <a href="#Value">Value</a></pre>

MakeFromBytes returns the Int value given the bytes of its little-endian binary
representation. An empty byte slice argument represents 0.

<h3 id="MakeFromLiteral">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L307">MakeFromLiteral</a>
    <a href="#MakeFromLiteral">¶</a></h3>
<pre>func MakeFromLiteral(lit <a href="/builtin/#string">string</a>, tok <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a>, zero <a href="/builtin/#uint">uint</a>) <a href="#Value">Value</a></pre>

MakeFromLiteral returns the corresponding integer, floating-point, imaginary,
character, or string value for a Go literal string. The tok value must be one of
token.INT, token.FLOAT, token.IMAG, token.CHAR, or token.STRING. The final
argument must be zero. If the literal string syntax is invalid, the result is an
Unknown.

<h3 id="MakeImag">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L626">MakeImag</a>
    <a href="#MakeImag">¶</a></h3>
<pre>func MakeImag(x <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

MakeImag returns the Complex value x*i; x must be Int, Float, or Unknown. If x
is Unknown, the result is Unknown.

<h3 id="MakeInt64">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L279">MakeInt64</a>
    <a href="#MakeInt64">¶</a></h3>
<pre>func MakeInt64(x <a href="/builtin/#int64">int64</a>) <a href="#Value">Value</a></pre>

MakeInt64 returns the Int value for x.

<h3 id="MakeString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L276">MakeString</a>
    <a href="#MakeString">¶</a></h3>
<pre>func MakeString(s <a href="/builtin/#string">string</a>) <a href="#Value">Value</a></pre>

MakeString returns the String value for s.

<h3 id="MakeUint64">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L282">MakeUint64</a>
    <a href="#MakeUint64">¶</a></h3>
<pre>func MakeUint64(x <a href="/builtin/#uint64">uint64</a>) <a href="#Value">Value</a></pre>

MakeUint64 returns the Int value for x.

<h3 id="MakeUnknown">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L270">MakeUnknown</a>
    <a href="#MakeUnknown">¶</a></h3>
<pre>func MakeUnknown() <a href="#Value">Value</a></pre>

MakeUnknown returns the Unknown value.

<h3 id="Num">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L582">Num</a>
    <a href="#Num">¶</a></h3>
<pre>func Num(x <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

Num returns the numerator of x; x must be Int, Float, or Unknown. If x is
Unknown, or if it is too large or small to represent as a fraction, the result
is Unknown. Otherwise the result is an Int with the same sign as x.

<h3 id="Real">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L639">Real</a>
    <a href="#Real">¶</a></h3>
<pre>func Real(x <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

Real returns the real part of x, which must be a numeric or unknown value. If x
is Unknown, the result is Unknown.

<h3 id="Shift">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L1120">Shift</a>
    <a href="#Shift">¶</a></h3>
<pre>func Shift(x <a href="#Value">Value</a>, op <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a>, s <a href="/builtin/#uint">uint</a>) <a href="#Value">Value</a></pre>

Shift returns the result of the shift expression x op s with op == token.SHL or
token.SHR (<< or >>). x must be an Int or an Unknown. If x is Unknown, the
result is x.

<h3 id="ToComplex">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L743">ToComplex</a>
    <a href="#ToComplex">¶</a></h3>
<pre>func ToComplex(x <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

ToComplex converts x to a Complex value if x is representable as a Complex.
Otherwise it returns an Unknown.

<h3 id="ToFloat">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L724">ToFloat</a>
    <a href="#ToFloat">¶</a></h3>
<pre>func ToFloat(x <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

ToFloat converts x to a Float value if x is representable as a Float. Otherwise
it returns an Unknown.

<h3 id="ToInt">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L670">ToInt</a>
    <a href="#ToInt">¶</a></h3>
<pre>func ToInt(x <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

ToInt converts x to an Int value if x is representable as an Int. Otherwise it
returns an Unknown.

<h3 id="UnaryOp">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/constant/value.go#L779">UnaryOp</a>
    <a href="#UnaryOp">¶</a></h3>
<pre>func UnaryOp(op <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a>, y <a href="#Value">Value</a>, prec <a href="/builtin/#uint">uint</a>) <a href="#Value">Value</a></pre>

UnaryOp returns the result of the unary expression op y. The operation must be
defined for the operand. If prec > 0 it specifies the ^ (xor) result size in
bits. If y is Unknown, the result is Unknown.


