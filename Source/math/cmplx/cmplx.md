version: 1.10
## package cmplx

  `import "math/cmplx"`

## Overview

Package cmplx provides basic constants and mathematical functions for complex
numbers.

## Index

- [func Abs(x complex128) float64](#Abs)
- [func Acos(x complex128) complex128](#Acos)
- [func Acosh(x complex128) complex128](#Acosh)
- [func Asin(x complex128) complex128](#Asin)
- [func Asinh(x complex128) complex128](#Asinh)
- [func Atan(x complex128) complex128](#Atan)
- [func Atanh(x complex128) complex128](#Atanh)
- [func Conj(x complex128) complex128](#Conj)
- [func Cos(x complex128) complex128](#Cos)
- [func Cosh(x complex128) complex128](#Cosh)
- [func Cot(x complex128) complex128](#Cot)
- [func Exp(x complex128) complex128](#Exp)
- [func Inf() complex128](#Inf)
- [func IsInf(x complex128) bool](#IsInf)
- [func IsNaN(x complex128) bool](#IsNaN)
- [func Log(x complex128) complex128](#Log)
- [func Log10(x complex128) complex128](#Log10)
- [func NaN() complex128](#NaN)
- [func Phase(x complex128) float64](#Phase)
- [func Polar(x complex128) (r, θ float64)](#Polar)
- [func Pow(x, y complex128) complex128](#Pow)
- [func Rect(r, θ float64) complex128](#Rect)
- [func Sin(x complex128) complex128](#Sin)
- [func Sinh(x complex128) complex128](#Sinh)
- [func Sqrt(x complex128) complex128](#Sqrt)
- [func Tan(x complex128) complex128](#Tan)
- [func Tanh(x complex128) complex128](#Tanh)

### Examples

- [Abs](#exampleAbs)
- [Exp](#exampleExp)
- [Polar](#examplePolar)

### Package files
 [abs.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/abs.go) [asin.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/asin.go) [conj.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/conj.go) [exp.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/exp.go) [isinf.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/isinf.go) [isnan.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/isnan.go) [log.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/log.go) [phase.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/phase.go) [polar.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/polar.go) [pow.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/pow.go) [rect.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/rect.go) [sin.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/sin.go) [sqrt.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/sqrt.go) [tan.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/tan.go)

<h2 id="Abs">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/abs.go#L2">Abs</a>
    <a href="#Abs">¶</a></h2>
<pre>func Abs(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#float64">float64</a></pre>

Abs returns the absolute value (also called the modulus) of x.

<a id="exampleAbs"></a>
Example:

    fmt.Printf("%.1f", cmplx.Abs(3+4i))
    // Output: 5.0

<h2 id="Acos">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/asin.go#L77">Acos</a>
    <a href="#Acos">¶</a></h2>
<pre>func Acos(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Acos returns the inverse cosine of x.

<h2 id="Acosh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/asin.go#L83">Acosh</a>
    <a href="#Acosh">¶</a></h2>
<pre>func Acosh(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Acosh returns the inverse hyperbolic cosine of x.

<h2 id="Asin">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/asin.go#L41">Asin</a>
    <a href="#Asin">¶</a></h2>
<pre>func Asin(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Asin returns the inverse sine of x.

<h2 id="Asinh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/asin.go#L54">Asinh</a>
    <a href="#Asinh">¶</a></h2>
<pre>func Asinh(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Asinh returns the inverse hyperbolic sine of x.

<h2 id="Atan">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/asin.go#L125">Atan</a>
    <a href="#Atan">¶</a></h2>
<pre>func Atan(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Atan returns the inverse tangent of x.

<h2 id="Atanh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/asin.go#L145">Atanh</a>
    <a href="#Atanh">¶</a></h2>
<pre>func Atanh(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Atanh returns the inverse hyperbolic tangent of x.

<h2 id="Conj">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/conj.go#L1">Conj</a>
    <a href="#Conj">¶</a></h2>
<pre>func Conj(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Conj returns the complex conjugate of x.

<h2 id="Cos">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/sin.go#L88">Cos</a>
    <a href="#Cos">¶</a></h2>
<pre>func Cos(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Cos returns the cosine of x.

<h2 id="Cosh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/sin.go#L107">Cosh</a>
    <a href="#Cosh">¶</a></h2>
<pre>func Cosh(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Cosh returns the hyperbolic cosine of x.

<h2 id="Cot">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/tan.go#L167">Cot</a>
    <a href="#Cot">¶</a></h2>
<pre>func Cot(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Cot returns the cotangent of x.

<h2 id="Exp">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/exp.go#L41">Exp</a>
    <a href="#Exp">¶</a></h2>
<pre>func Exp(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Exp returns e**x, the base-e exponential of x.

<a id="exampleExp"></a>
Example:

    fmt.Printf("%.1f", cmplx.Exp(1i*math.Pi)+1)
    // Output: (0.0+0.0i)

<h2 id="Inf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/isinf.go#L8">Inf</a>
    <a href="#Inf">¶</a></h2>
<pre>func Inf() <a href="/builtin/#complex128">complex128</a></pre>

Inf returns a complex infinity, complex(+Inf, +Inf).

<h2 id="IsInf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/isinf.go#L1">IsInf</a>
    <a href="#IsInf">¶</a></h2>
<pre>func IsInf(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#bool">bool</a></pre>

IsInf returns true if either real(x) or imag(x) is an infinity.

<h2 id="IsNaN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/isnan.go#L1">IsNaN</a>
    <a href="#IsNaN">¶</a></h2>
<pre>func IsNaN(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#bool">bool</a></pre>

IsNaN returns true if either real(x) or imag(x) is NaN and neither is an
infinity.

<h2 id="Log">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/log.go#L47">Log</a>
    <a href="#Log">¶</a></h2>
<pre>func Log(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Log returns the natural logarithm of x.

<h2 id="Log10">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/log.go#L52">Log10</a>
    <a href="#Log10">¶</a></h2>
<pre>func Log10(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Log10 returns the decimal logarithm of x.

<h2 id="NaN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/isnan.go#L12">NaN</a>
    <a href="#NaN">¶</a></h2>
<pre>func NaN() <a href="/builtin/#complex128">complex128</a></pre>

NaN returns a complex ``not-a-number'' value.

<h2 id="Phase">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/phase.go#L1">Phase</a>
    <a href="#Phase">¶</a></h2>
<pre>func Phase(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#float64">float64</a></pre>

Phase returns the phase (also called the argument) of x. The returned value is
in the range [-Pi, Pi].

<h2 id="Polar">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/polar.go#L1">Polar</a>
    <a href="#Polar">¶</a></h2>
<pre>func Polar(x <a href="/builtin/#complex128">complex128</a>) (r, θ <a href="/builtin/#float64">float64</a>)</pre>

Polar returns the absolute value r and phase θ of x, such that x = r * e**θi.
The phase is in the range [-Pi, Pi].

<a id="examplePolar"></a>
Example:

    r, theta := cmplx.Polar(2i)
    fmt.Printf("r: %.1f, θ: %.1f*π", r, theta/math.Pi)
    // Output: r: 2.0, θ: 0.5*π

<h2 id="Pow">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/pow.go#L39">Pow</a>
    <a href="#Pow">¶</a></h2>
<pre>func Pow(x, y <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Pow returns x**y, the base-x exponential of y. For generalized compatibility
with math.Pow:

    Pow(0, ±0) returns 1+0i
    Pow(0, c) for real(c)<0 returns Inf+0i if imag(c) is zero, otherwise Inf+Inf i.

<h2 id="Rect">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/rect.go#L1">Rect</a>
    <a href="#Rect">¶</a></h2>
<pre>func Rect(r, θ <a href="/builtin/#float64">float64</a>) <a href="/builtin/#complex128">complex128</a></pre>

Rect returns the complex number x with polar coordinates r, θ.

<h2 id="Sin">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/sin.go#L43">Sin</a>
    <a href="#Sin">¶</a></h2>
<pre>func Sin(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Sin returns the sine of x.

<h2 id="Sinh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/sin.go#L63">Sinh</a>
    <a href="#Sinh">¶</a></h2>
<pre>func Sinh(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Sinh returns the hyperbolic sine of x.

<h2 id="Sqrt">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/sqrt.go#L48">Sqrt</a>
    <a href="#Sqrt">¶</a></h2>
<pre>func Sqrt(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Sqrt returns the square root of x. The result r is chosen so that real(r) ≥ 0
and imag(r) has the same sign as imag(x).

<h2 id="Tan">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/tan.go#L49">Tan</a>
    <a href="#Tan">¶</a></h2>
<pre>func Tan(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Tan returns the tangent of x.

<h2 id="Tanh">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/math/cmplx/tan.go#L73">Tanh</a>
    <a href="#Tanh">¶</a></h2>
<pre>func Tanh(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#complex128">complex128</a></pre>

Tanh returns the hyperbolic tangent of x.


