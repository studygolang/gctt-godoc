version: 1.9.2
## package math

  `import "math"`

## Overview

Package math provides basic constants and mathematical functions.

This package does not guarantee bit-identical results across architectures.

## Index

- [Constants](#pkg-constants)
- [func Abs(x float64) float64](#Abs)
- [func Acos(x float64) float64](#Acos)
- [func Acosh(x float64) float64](#Acosh)
- [func Asin(x float64) float64](#Asin)
- [func Asinh(x float64) float64](#Asinh)
- [func Atan(x float64) float64](#Atan)
- [func Atan2(y, x float64) float64](#Atan2)
- [func Atanh(x float64) float64](#Atanh)
- [func Cbrt(x float64) float64](#Cbrt)
- [func Ceil(x float64) float64](#Ceil)
- [func Copysign(x, y float64) float64](#Copysign)
- [func Cos(x float64) float64](#Cos)
- [func Cosh(x float64) float64](#Cosh)
- [func Dim(x, y float64) float64](#Dim)
- [func Erf(x float64) float64](#Erf)
- [func Erfc(x float64) float64](#Erfc)
- [func Exp(x float64) float64](#Exp)
- [func Exp2(x float64) float64](#Exp2)
- [func Expm1(x float64) float64](#Expm1)
- [func Float32bits(f float32) uint32](#Float32bits)
- [func Float32frombits(b uint32) float32](#Float32frombits)
- [func Float64bits(f float64) uint64](#Float64bits)
- [func Float64frombits(b uint64) float64](#Float64frombits)
- [func Floor(x float64) float64](#Floor)
- [func Frexp(f float64) (frac float64, exp int)](#Frexp)
- [func Gamma(x float64) float64](#Gamma)
- [func Hypot(p, q float64) float64](#Hypot)
- [func Ilogb(x float64) int](#Ilogb)
- [func Inf(sign int) float64](#Inf)
- [func IsInf(f float64, sign int) bool](#IsInf)
- [func IsNaN(f float64) (is bool)](#IsNaN)
- [func J0(x float64) float64](#J0)
- [func J1(x float64) float64](#J1)
- [func Jn(n int, x float64) float64](#Jn)
- [func Ldexp(frac float64, exp int) float64](#Ldexp)
- [func Lgamma(x float64) (lgamma float64, sign int)](#Lgamma)
- [func Log(x float64) float64](#Log)
- [func Log10(x float64) float64](#Log10)
- [func Log1p(x float64) float64](#Log1p)
- [func Log2(x float64) float64](#Log2)
- [func Logb(x float64) float64](#Logb)
- [func Max(x, y float64) float64](#Max)
- [func Min(x, y float64) float64](#Min)
- [func Mod(x, y float64) float64](#Mod)
- [func Modf(f float64) (int float64, frac float64)](#Modf)
- [func NaN() float64](#NaN)
- [func Nextafter(x, y float64) (r float64)](#Nextafter)
- [func Nextafter32(x, y float32) (r float32)](#Nextafter32)
- [func Pow(x, y float64) float64](#Pow)
- [func Pow10(n int) float64](#Pow10)
- [func Remainder(x, y float64) float64](#Remainder)
- [func Signbit(x float64) bool](#Signbit)
- [func Sin(x float64) float64](#Sin)
- [func Sincos(x float64) (sin, cos float64)](#Sincos)
- [func Sinh(x float64) float64](#Sinh)
- [func Sqrt(x float64) float64](#Sqrt)
- [func Tan(x float64) float64](#Tan)
- [func Tanh(x float64) float64](#Tanh)
- [func Trunc(x float64) float64](#Trunc)
- [func Y0(x float64) float64](#Y0)
- [func Y1(x float64) float64](#Y1)
- [func Yn(n int, x float64) float64](#Yn)

### Examples

- [Sqrt](#exampleSqrt)

### Package files
 [abs.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/abs.go) [acosh.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/acosh.go) [asin.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/asin.go) [asinh.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/asinh.go) [atan.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/atan.go) [atan2.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/atan2.go) [atanh.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/atanh.go) [bits.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/bits.go) [cbrt.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cbrt.go) [const.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/const.go) [copysign.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/copysign.go) [dim.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/dim.go) [erf.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/erf.go) [exp.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/exp.go) [expm1.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/expm1.go) [floor.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/floor.go) [floor_asm.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/floor_asm.go) [frexp.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/frexp.go) [gamma.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/gamma.go) [hypot.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/hypot.go) [j0.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/j0.go) [j1.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/j1.go) [jn.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/jn.go) [ldexp.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/ldexp.go) [lgamma.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/lgamma.go) [log.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/log.go) [log10.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/log10.go) [log1p.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/log1p.go) [logb.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/logb.go) [mod.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/mod.go) [modf.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/modf.go) [nextafter.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/nextafter.go) [pow.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/pow.go) [pow10.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/pow10.go) [remainder.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/remainder.go) [signbit.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/signbit.go) [sin.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/sin.go) [sincos.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/sincos.go) [sinh.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/sinh.go) [sqrt.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/sqrt.go) [tan.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/tan.go) [tanh.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/tanh.go) [unsafe.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/unsafe.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="E">E</span>   = 2.71828182845904523536028747135266249775724709369995957496696763 <span class="comment">// http://oeis.org/A001113</span>
    <span id="Pi">Pi</span>  = 3.14159265358979323846264338327950288419716939937510582097494459 <span class="comment">// http://oeis.org/A000796</span>
    <span id="Phi">Phi</span> = 1.61803398874989484820458683436563811772030917980576286213544862 <span class="comment">// http://oeis.org/A001622</span>

    <span id="Sqrt2">Sqrt2</span>   = 1.41421356237309504880168872420969807856967187537694807317667974 <span class="comment">// http://oeis.org/A002193</span>
    <span id="SqrtE">SqrtE</span>   = 1.64872127070012814684865078781416357165377610071014801157507931 <span class="comment">// http://oeis.org/A019774</span>
    <span id="SqrtPi">SqrtPi</span>  = 1.77245385090551602729816748334114518279754945612238712821380779 <span class="comment">// http://oeis.org/A002161</span>
    <span id="SqrtPhi">SqrtPhi</span> = 1.27201964951406896425242246173749149171560804184009624861664038 <span class="comment">// http://oeis.org/A139339</span>

    <span id="Ln2">Ln2</span>    = 0.693147180559945309417232121458176568075500134360255254120680009 <span class="comment">// http://oeis.org/A002162</span>
    <span id="Log2E">Log2E</span>  = 1 / <a href="#Ln2">Ln2</a>
    <span id="Ln10">Ln10</span>   = 2.30258509299404568401799145468436420760110148862877297603332790 <span class="comment">// http://oeis.org/A002392</span>
    <span id="Log10E">Log10E</span> = 1 / <a href="#Ln10">Ln10</a>
)</pre>

Mathematical constants.

<pre>const (
    <span id="MaxFloat32">MaxFloat32</span>             = 3.40282346638528859811704183484516925440e+38  <span class="comment">// 2**127 * (2**24 - 1) / 2**23</span>
    <span id="SmallestNonzeroFloat32">SmallestNonzeroFloat32</span> = 1.401298464324817070923729583289916131280e-45 <span class="comment">// 1 / 2**(127 - 1 + 23)</span>

    <span id="MaxFloat64">MaxFloat64</span>             = 1.797693134862315708145274237317043567981e+308 <span class="comment">// 2**1023 * (2**53 - 1) / 2**52</span>
    <span id="SmallestNonzeroFloat64">SmallestNonzeroFloat64</span> = 4.940656458412465441765687928682213723651e-324 <span class="comment">// 1 / 2**(1023 - 1 + 52)</span>
)</pre>

Floating-point limit values. Max is the largest finite value representable by
the type. SmallestNonzero is the smallest positive, non-zero value representable
by the type.

<pre>const (
    <span id="MaxInt8">MaxInt8</span>   = 1&lt;&lt;7 - 1
    <span id="MinInt8">MinInt8</span>   = -1 &lt;&lt; 7
    <span id="MaxInt16">MaxInt16</span>  = 1&lt;&lt;15 - 1
    <span id="MinInt16">MinInt16</span>  = -1 &lt;&lt; 15
    <span id="MaxInt32">MaxInt32</span>  = 1&lt;&lt;31 - 1
    <span id="MinInt32">MinInt32</span>  = -1 &lt;&lt; 31
    <span id="MaxInt64">MaxInt64</span>  = 1&lt;&lt;63 - 1
    <span id="MinInt64">MinInt64</span>  = -1 &lt;&lt; 63
    <span id="MaxUint8">MaxUint8</span>  = 1&lt;&lt;8 - 1
    <span id="MaxUint16">MaxUint16</span> = 1&lt;&lt;16 - 1
    <span id="MaxUint32">MaxUint32</span> = 1&lt;&lt;32 - 1
    <span id="MaxUint64">MaxUint64</span> = 1&lt;&lt;64 - 1
)</pre>

Integer limit values.

<h2 id="Abs">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/abs.go#L2">Abs</a>
    <a href="#Abs">¶</a></h2>
<pre>func Abs(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Abs returns the absolute value of x.

Special cases are:

    Abs(±Inf) = +Inf
    Abs(NaN) = NaN

<h2 id="Acos">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/asin.go#L41">Acos</a>
    <a href="#Acos">¶</a></h2>
<pre>func Acos(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Acos returns the arccosine, in radians, of x.

Special case is:

    Acos(x) = NaN if x < -1 or x > 1

<h2 id="Acosh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/acosh.go#L32">Acosh</a>
    <a href="#Acosh">¶</a></h2>
<pre>func Acosh(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Acosh returns the inverse hyperbolic cosine of x.

Special cases are:

    Acosh(+Inf) = +Inf
    Acosh(x) = NaN if x < 1
    Acosh(NaN) = NaN

<h2 id="Asin">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/asin.go#L9">Asin</a>
    <a href="#Asin">¶</a></h2>
<pre>func Asin(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Asin returns the arcsine, in radians, of x.

Special cases are:

    Asin(±0) = ±0
    Asin(x) = NaN if x < -1 or x > 1

<h2 id="Asinh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/asinh.go#L29">Asinh</a>
    <a href="#Asinh">¶</a></h2>
<pre>func Asinh(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Asinh returns the inverse hyperbolic sine of x.

Special cases are:

    Asinh(±0) = ±0
    Asinh(±Inf) = ±Inf
    Asinh(NaN) = NaN

<h2 id="Atan">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/atan.go#L85">Atan</a>
    <a href="#Atan">¶</a></h2>
<pre>func Atan(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Atan returns the arctangent, in radians, of x.

Special cases are:

    Atan(±0) = ±0
    Atan(±Inf) = ±Pi/2

<h2 id="Atan2">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/atan2.go#L19">Atan2</a>
    <a href="#Atan2">¶</a></h2>
<pre>func Atan2(y, x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Atan2 returns the arc tangent of y/x, using the signs of the two to determine
the quadrant of the return value.

Special cases are (in order):

    Atan2(y, NaN) = NaN
    Atan2(NaN, x) = NaN
    Atan2(+0, x>=0) = +0
    Atan2(-0, x>=0) = -0
    Atan2(+0, x<=-0) = +Pi
    Atan2(-0, x<=-0) = -Pi
    Atan2(y>0, 0) = +Pi/2
    Atan2(y<0, 0) = -Pi/2
    Atan2(+Inf, +Inf) = +Pi/4
    Atan2(-Inf, +Inf) = -Pi/4
    Atan2(+Inf, -Inf) = 3Pi/4
    Atan2(-Inf, -Inf) = -3Pi/4
    Atan2(y, +Inf) = 0
    Atan2(y>0, -Inf) = +Pi
    Atan2(y<0, -Inf) = -Pi
    Atan2(+Inf, x) = +Pi/2
    Atan2(-Inf, x) = -Pi/2

<h2 id="Atanh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/atanh.go#L37">Atanh</a>
    <a href="#Atanh">¶</a></h2>
<pre>func Atanh(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Atanh returns the inverse hyperbolic tangent of x.

Special cases are:

    Atanh(1) = +Inf
    Atanh(±0) = ±0
    Atanh(-1) = -Inf
    Atanh(x) = NaN if x < -1 or x > 1
    Atanh(NaN) = NaN

<h2 id="Cbrt">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cbrt.go#L15">Cbrt</a>
    <a href="#Cbrt">¶</a></h2>
<pre>func Cbrt(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Cbrt returns the cube root of x.

Special cases are:

    Cbrt(±0) = ±0
    Cbrt(±Inf) = ±Inf
    Cbrt(NaN) = NaN

<h2 id="Ceil">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/floor.go#L26">Ceil</a>
    <a href="#Ceil">¶</a></h2>
<pre>func Ceil(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Ceil returns the least integer value greater than or equal to x.

Special cases are:

    Ceil(±0) = ±0
    Ceil(±Inf) = ±Inf
    Ceil(NaN) = NaN

<h2 id="Copysign">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/copysign.go#L1">Copysign</a>
    <a href="#Copysign">¶</a></h2>
<pre>func Copysign(x, y <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Copysign returns a value with the magnitude of x and the sign of y.

<h2 id="Cos">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/sin.go#L107">Cos</a>
    <a href="#Cos">¶</a></h2>
<pre>func Cos(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Cos returns the cosine of the radian argument x.

Special cases are:

    Cos(±Inf) = NaN
    Cos(NaN) = NaN

<h2 id="Cosh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/sinh.go#L61">Cosh</a>
    <a href="#Cosh">¶</a></h2>
<pre>func Cosh(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Cosh returns the hyperbolic cosine of x.

Special cases are:

    Cosh(±0) = 1
    Cosh(±Inf) = +Inf
    Cosh(NaN) = NaN

<h2 id="Dim">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/dim.go#L3">Dim</a>
    <a href="#Dim">¶</a></h2>
<pre>func Dim(x, y <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Dim returns the maximum of x-y or 0.

Special cases are:

    Dim(+Inf, +Inf) = NaN
    Dim(-Inf, -Inf) = NaN
    Dim(x, NaN) = Dim(NaN, x) = NaN

<h2 id="Erf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/erf.go#L178">Erf</a>
    <a href="#Erf">¶</a></h2>
<pre>func Erf(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Erf returns the error function of x.

Special cases are:

    Erf(+Inf) = 1
    Erf(-Inf) = -1
    Erf(NaN) = NaN

<h2 id="Erfc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/erf.go#L257">Erfc</a>
    <a href="#Erfc">¶</a></h2>
<pre>func Erfc(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Erfc returns the complementary error function of x.

Special cases are:

    Erfc(+Inf) = 0
    Erfc(-Inf) = 2
    Erfc(NaN) = NaN

<h2 id="Exp">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/exp.go#L4">Exp</a>
    <a href="#Exp">¶</a></h2>
<pre>func Exp(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Exp returns e**x, the base-e exponential of x.

Special cases are:

    Exp(+Inf) = +Inf
    Exp(NaN) = NaN

Very large values overflow to 0 or +Inf. Very small values underflow to 1.

<h2 id="Exp2">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/exp.go#L125">Exp2</a>
    <a href="#Exp2">¶</a></h2>
<pre>func Exp2(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Exp2 returns 2**x, the base-2 exponential of x.

Special cases are the same as Exp.

<h2 id="Expm1">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/expm1.go#L114">Expm1</a>
    <a href="#Expm1">¶</a></h2>
<pre>func Expm1(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Expm1 returns e**x - 1, the base-e exponential of x minus 1. It is more accurate
than Exp(x) - 1 when x is near zero.

Special cases are:

    Expm1(+Inf) = +Inf
    Expm1(-Inf) = -1
    Expm1(NaN) = NaN

Very large values overflow to -1 or +Inf.

<h2 id="Float32bits">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/unsafe.go#L1">Float32bits</a>
    <a href="#Float32bits">¶</a></h2>
<pre>func Float32bits(f <a href="/builtin/#float32">float32</a>) <a href="/builtin/#uint32">uint32</a></pre>

Float32bits returns the IEEE 754 binary representation of f.

<h2 id="Float32frombits">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/unsafe.go#L4">Float32frombits</a>
    <a href="#Float32frombits">¶</a></h2>
<pre>func Float32frombits(b <a href="/builtin/#uint32">uint32</a>) <a href="/builtin/#float32">float32</a></pre>

Float32frombits returns the floating point number corresponding to the IEEE 754
binary representation b.

<h2 id="Float64bits">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/unsafe.go#L7">Float64bits</a>
    <a href="#Float64bits">¶</a></h2>
<pre>func Float64bits(f <a href="/builtin/#float64">float64</a>) <a href="/builtin/#uint64">uint64</a></pre>

Float64bits returns the IEEE 754 binary representation of f.

<h2 id="Float64frombits">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/unsafe.go#L11">Float64frombits</a>
    <a href="#Float64frombits">¶</a></h2>
<pre>func Float64frombits(b <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#float64">float64</a></pre>

Float64frombits returns the floating point number corresponding the IEEE 754
binary representation b.

<h2 id="Floor">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/floor.go#L3">Floor</a>
    <a href="#Floor">¶</a></h2>
<pre>func Floor(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Floor returns the greatest integer value less than or equal to x.

Special cases are:

    Floor(±0) = ±0
    Floor(±Inf) = ±Inf
    Floor(NaN) = NaN

<h2 id="Frexp">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/frexp.go#L6">Frexp</a>
    <a href="#Frexp">¶</a></h2>
<pre>func Frexp(f <a href="/builtin/#float64">float64</a>) (frac <a href="/builtin/#float64">float64</a>, exp <a href="/builtin/#int">int</a>)</pre>

Frexp breaks f into a normalized fraction and an integral power of two. It
returns frac and exp satisfying f == frac × 2**exp, with the absolute value of
frac in the interval [½, 1).

Special cases are:

    Frexp(±0) = ±0, 0
    Frexp(±Inf) = ±Inf, 0
    Frexp(NaN) = NaN, 0

<h2 id="Gamma">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/gamma.go#L120">Gamma</a>
    <a href="#Gamma">¶</a></h2>
<pre>func Gamma(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Gamma returns the Gamma function of x.

Special cases are:

    Gamma(+Inf) = +Inf
    Gamma(+0) = +Inf
    Gamma(-0) = -Inf
    Gamma(x) = NaN for integer x < 0
    Gamma(-Inf) = NaN
    Gamma(NaN) = NaN

<h2 id="Hypot">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/hypot.go#L9">Hypot</a>
    <a href="#Hypot">¶</a></h2>
<pre>func Hypot(p, q <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Hypot returns Sqrt(p*p + q*q), taking care to avoid unnecessary overflow and
underflow.

Special cases are:

    Hypot(±Inf, q) = +Inf
    Hypot(p, ±Inf) = +Inf
    Hypot(NaN, q) = NaN
    Hypot(p, NaN) = NaN

<h2 id="Ilogb">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/logb.go#L22">Ilogb</a>
    <a href="#Ilogb">¶</a></h2>
<pre>func Ilogb(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#int">int</a></pre>

Ilogb returns the binary exponent of x as an integer.

Special cases are:

    Ilogb(±Inf) = MaxInt32
    Ilogb(0) = MinInt32
    Ilogb(NaN) = MaxInt32

<h2 id="Inf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/bits.go#L7">Inf</a>
    <a href="#Inf">¶</a></h2>
<pre>func Inf(sign <a href="/builtin/#int">int</a>) <a href="/builtin/#float64">float64</a></pre>

Inf returns positive infinity if sign >= 0, negative infinity if sign < 0.

<h2 id="IsInf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/bits.go#L33">IsInf</a>
    <a href="#IsInf">¶</a></h2>
<pre>func IsInf(f <a href="/builtin/#float64">float64</a>, sign <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>

IsInf reports whether f is an infinity, according to sign. If sign > 0, IsInf
reports whether f is positive infinity. If sign < 0, IsInf reports whether f is
negative infinity. If sign == 0, IsInf reports whether f is either infinity.

<h2 id="IsNaN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/bits.go#L21">IsNaN</a>
    <a href="#IsNaN">¶</a></h2>
<pre>func IsNaN(f <a href="/builtin/#float64">float64</a>) (is <a href="/builtin/#bool">bool</a>)</pre>

IsNaN reports whether f is an IEEE 754 ``not-a-number'' value.

<h2 id="J0">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/j0.go#L66">J0</a>
    <a href="#J0">¶</a></h2>
<pre>func J0(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

J0 returns the order-zero Bessel function of the first kind.

Special cases are:

    J0(±Inf) = 0
    J0(0) = 1
    J0(NaN) = NaN

<h2 id="J1">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/j1.go#L64">J1</a>
    <a href="#J1">¶</a></h2>
<pre>func J1(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

J1 returns the order-one Bessel function of the first kind.

Special cases are:

    J1(±Inf) = 0
    J1(NaN) = NaN

<h2 id="Jn">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/jn.go#L43">Jn</a>
    <a href="#Jn">¶</a></h2>
<pre>func Jn(n <a href="/builtin/#int">int</a>, x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Jn returns the order-n Bessel function of the first kind.

Special cases are:

    Jn(n, ±Inf) = 0
    Jn(n, NaN) = NaN

<h2 id="Ldexp">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/ldexp.go#L4">Ldexp</a>
    <a href="#Ldexp">¶</a></h2>
<pre>func Ldexp(frac <a href="/builtin/#float64">float64</a>, exp <a href="/builtin/#int">int</a>) <a href="/builtin/#float64">float64</a></pre>

Ldexp is the inverse of Frexp. It returns frac × 2**exp.

Special cases are:

    Ldexp(±0, exp) = ±0
    Ldexp(±Inf, exp) = ±Inf
    Ldexp(NaN, exp) = NaN

<h2 id="Lgamma">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/lgamma.go#L164">Lgamma</a>
    <a href="#Lgamma">¶</a></h2>
<pre>func Lgamma(x <a href="/builtin/#float64">float64</a>) (lgamma <a href="/builtin/#float64">float64</a>, sign <a href="/builtin/#int">int</a>)</pre>

Lgamma returns the natural logarithm and sign (-1 or +1) of Gamma(x).

Special cases are:

    Lgamma(+Inf) = +Inf
    Lgamma(0) = +Inf
    Lgamma(-integer) = +Inf
    Lgamma(-Inf) = -Inf
    Lgamma(NaN) = NaN

<h2 id="Log">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/log.go#L70">Log</a>
    <a href="#Log">¶</a></h2>
<pre>func Log(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Log returns the natural logarithm of x.

Special cases are:

    Log(+Inf) = +Inf
    Log(0) = -Inf
    Log(x < 0) = NaN
    Log(NaN) = NaN

<h2 id="Log10">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/log10.go#L1">Log10</a>
    <a href="#Log10">¶</a></h2>
<pre>func Log10(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Log10 returns the decimal logarithm of x. The special cases are the same as for
Log.

<h2 id="Log1p">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/log1p.go#L85">Log1p</a>
    <a href="#Log1p">¶</a></h2>
<pre>func Log1p(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Log1p returns the natural logarithm of 1 plus its argument x. It is more
accurate than Log(1 + x) when x is near zero.

Special cases are:

    Log1p(+Inf) = +Inf
    Log1p(±0) = ±0
    Log1p(-1) = -Inf
    Log1p(x < -1) = NaN
    Log1p(NaN) = NaN

<h2 id="Log2">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/log10.go#L7">Log2</a>
    <a href="#Log2">¶</a></h2>
<pre>func Log2(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Log2 returns the binary logarithm of x. The special cases are the same as for
Log.

<h2 id="Logb">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/logb.go#L3">Logb</a>
    <a href="#Logb">¶</a></h2>
<pre>func Logb(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Logb returns the binary exponent of x.

Special cases are:

    Logb(±Inf) = +Inf
    Logb(0) = -Inf
    Logb(NaN) = NaN

<h2 id="Max">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/dim.go#L16">Max</a>
    <a href="#Max">¶</a></h2>
<pre>func Max(x, y <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Max returns the larger of x or y.

Special cases are:

    Max(x, +Inf) = Max(+Inf, x) = +Inf
    Max(x, NaN) = Max(NaN, x) = NaN
    Max(+0, ±0) = Max(±0, +0) = +0
    Max(-0, -0) = -0

<h2 id="Min">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/dim.go#L43">Min</a>
    <a href="#Min">¶</a></h2>
<pre>func Min(x, y <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Min returns the smaller of x or y.

Special cases are:

    Min(x, -Inf) = Min(-Inf, x) = -Inf
    Min(x, NaN) = Min(NaN, x) = NaN
    Min(-0, ±0) = Min(±0, -0) = -0

<h2 id="Mod">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/mod.go#L11">Mod</a>
    <a href="#Mod">¶</a></h2>
<pre>func Mod(x, y <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Mod returns the floating-point remainder of x/y. The magnitude of the result is
less than y and its sign agrees with that of x.

Special cases are:

    Mod(±Inf, y) = NaN
    Mod(NaN, y) = NaN
    Mod(x, 0) = NaN
    Mod(x, ±Inf) = x
    Mod(x, NaN) = NaN

<h2 id="Modf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/modf.go#L3">Modf</a>
    <a href="#Modf">¶</a></h2>
<pre>func Modf(f <a href="/builtin/#float64">float64</a>) (int <a href="/builtin/#float64">float64</a>, frac <a href="/builtin/#float64">float64</a>)</pre>

Modf returns integer and fractional floating-point numbers that sum to f. Both
values have the same sign as f.

Special cases are:

    Modf(±Inf) = ±Inf, NaN
    Modf(NaN) = NaN, NaN

<h2 id="NaN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/bits.go#L18">NaN</a>
    <a href="#NaN">¶</a></h2>
<pre>func NaN() <a href="/builtin/#float64">float64</a></pre>

NaN returns an IEEE 754 ``not-a-number'' value.

<h2 id="Nextafter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/nextafter.go#L25">Nextafter</a>
    <a href="#Nextafter">¶</a></h2>
<pre>func Nextafter(x, y <a href="/builtin/#float64">float64</a>) (r <a href="/builtin/#float64">float64</a>)</pre>

Nextafter returns the next representable float64 value after x towards y.

Special cases are:

    Nextafter(x, x)   = x
    Nextafter(NaN, y) = NaN
    Nextafter(x, NaN) = NaN

<h2 id="Nextafter32">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/nextafter.go#L3">Nextafter32</a>
    <a href="#Nextafter32">¶</a></h2>
<pre>func Nextafter32(x, y <a href="/builtin/#float32">float32</a>) (r <a href="/builtin/#float32">float32</a>)</pre>

Nextafter32 returns the next representable float32 value after x towards y.

Special cases are:

    Nextafter32(x, x)   = x
    Nextafter32(NaN, y) = NaN
    Nextafter32(x, NaN) = NaN

<h2 id="Pow">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/pow.go#L28">Pow</a>
    <a href="#Pow">¶</a></h2>
<pre>func Pow(x, y <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Pow returns x**y, the base-x exponential of y.

Special cases are (in order):

    Pow(x, ±0) = 1 for any x
    Pow(1, y) = 1 for any y
    Pow(x, 1) = x for any x
    Pow(NaN, y) = NaN
    Pow(x, NaN) = NaN
    Pow(±0, y) = ±Inf for y an odd integer < 0
    Pow(±0, -Inf) = +Inf
    Pow(±0, +Inf) = +0
    Pow(±0, y) = +Inf for finite y < 0 and not an odd integer
    Pow(±0, y) = ±0 for y an odd integer > 0
    Pow(±0, y) = +0 for finite y > 0 and not an odd integer
    Pow(-1, ±Inf) = 1
    Pow(x, +Inf) = +Inf for |x| > 1
    Pow(x, -Inf) = +0 for |x| > 1
    Pow(x, +Inf) = +0 for |x| < 1
    Pow(x, -Inf) = +Inf for |x| < 1
    Pow(+Inf, y) = +Inf for y > 0
    Pow(+Inf, y) = +0 for y < 0
    Pow(-Inf, y) = Pow(-0, -y)
    Pow(x, y) = NaN for finite x < 0 and finite non-integer y

<h2 id="Pow10">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/pow10.go#L20">Pow10</a>
    <a href="#Pow10">¶</a></h2>
<pre>func Pow10(n <a href="/builtin/#int">int</a>) <a href="/builtin/#float64">float64</a></pre>

Pow10 returns 10**n, the base-10 exponential of n.

Special cases are:

    Pow10(n) =    0 for n < -323
    Pow10(n) = +Inf for n > 308

<h2 id="Remainder">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/remainder.go#L27">Remainder</a>
    <a href="#Remainder">¶</a></h2>
<pre>func Remainder(x, y <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Remainder returns the IEEE 754 floating-point remainder of x/y.

Special cases are:

    Remainder(±Inf, y) = NaN
    Remainder(NaN, y) = NaN
    Remainder(x, 0) = NaN
    Remainder(x, ±Inf) = x
    Remainder(x, NaN) = NaN

<h2 id="Signbit">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/signbit.go#L1">Signbit</a>
    <a href="#Signbit">¶</a></h2>
<pre>func Signbit(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#bool">bool</a></pre>

Signbit returns true if x is negative or negative zero.

<h2 id="Sin">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/sin.go#L164">Sin</a>
    <a href="#Sin">¶</a></h2>
<pre>func Sin(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Sin returns the sine of the radian argument x.

Special cases are:

    Sin(±0) = ±0
    Sin(±Inf) = NaN
    Sin(NaN) = NaN

<h2 id="Sincos">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/sincos.go#L7">Sincos</a>
    <a href="#Sincos">¶</a></h2>
<pre>func Sincos(x <a href="/builtin/#float64">float64</a>) (sin, cos <a href="/builtin/#float64">float64</a>)</pre>

Sincos returns Sin(x), Cos(x).

Special cases are:

    Sincos(±0) = ±0, 1
    Sincos(±Inf) = NaN, NaN
    Sincos(NaN) = NaN, NaN

<h2 id="Sinh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/sinh.go#L15">Sinh</a>
    <a href="#Sinh">¶</a></h2>
<pre>func Sinh(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Sinh returns the hyperbolic sine of x.

Special cases are:

    Sinh(±0) = ±0
    Sinh(±Inf) = ±Inf
    Sinh(NaN) = NaN

<h2 id="Sqrt">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/sqrt.go#L82">Sqrt</a>
    <a href="#Sqrt">¶</a></h2>
<pre>func Sqrt(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Sqrt returns the square root of x.

Special cases are:

    Sqrt(+Inf) = +Inf
    Sqrt(±0) = ±0
    Sqrt(x < 0) = NaN
    Sqrt(NaN) = NaN

<a id="exampleSqrt"></a>
Example:

    const (
        a   = 3
        b   = 4
    )
    c := math.Sqrt(a*a + b*b)
    fmt.Printf("%.1f", c)
    // Output: 5.0

<h2 id="Tan">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/tan.go#L72">Tan</a>
    <a href="#Tan">¶</a></h2>
<pre>func Tan(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Tan returns the tangent of the radian argument x.

Special cases are:

    Tan(±0) = ±0
    Tan(±Inf) = NaN
    Tan(NaN) = NaN

<h2 id="Tanh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/tanh.go#L64">Tanh</a>
    <a href="#Tanh">¶</a></h2>
<pre>func Tanh(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Tanh returns the hyperbolic tangent of x.

Special cases are:

    Tanh(±0) = ±0
    Tanh(±Inf) = ±1
    Tanh(NaN) = NaN

<h2 id="Trunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/floor.go#L38">Trunc</a>
    <a href="#Trunc">¶</a></h2>
<pre>func Trunc(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Trunc returns the integer value of x.

Special cases are:

    Trunc(±0) = ±0
    Trunc(±Inf) = ±Inf
    Trunc(NaN) = NaN

<h2 id="Y0">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/j0.go#L146">Y0</a>
    <a href="#Y0">¶</a></h2>
<pre>func Y0(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Y0 returns the order-zero Bessel function of the second kind.

Special cases are:

    Y0(+Inf) = 0
    Y0(0) = -Inf
    Y0(x < 0) = NaN
    Y0(NaN) = NaN

<h2 id="Y1">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/j1.go#L144">Y1</a>
    <a href="#Y1">¶</a></h2>
<pre>func Y1(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Y1 returns the order-one Bessel function of the second kind.

Special cases are:

    Y1(+Inf) = 0
    Y1(0) = -Inf
    Y1(x < 0) = NaN
    Y1(NaN) = NaN

<h2 id="Yn">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/jn.go#L223">Yn</a>
    <a href="#Yn">¶</a></h2>
<pre>func Yn(n <a href="/builtin/#int">int</a>, x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#float64">float64</a></pre>

Yn returns the order-n Bessel function of the second kind.

Special cases are:

    Yn(n, +Inf) = 0
    Yn(n ≥ 0, 0) = -Inf
    Yn(n < 0, 0) = +Inf if n is odd, -Inf if n is even
    Yn(n, x < 0) = NaN
    Yn(n, NaN) = NaN

## Subdirectories
- [..](..)
- [big](big/)
- [bits](bits/)
- [cmplx](cmplx/)
- [rand](rand/)
