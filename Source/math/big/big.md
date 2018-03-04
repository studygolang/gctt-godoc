version: 1.10
## package big

  `import "math/big"`

## Overview

Package big implements arbitrary-precision arithmetic (big numbers). The
following numeric types are supported:

    Int    signed integers
    Rat    rational numbers
    Float  floating-point numbers

The zero value for an Int, Rat, or Float correspond to 0. Thus, new values can
be declared in the usual ways and denote 0 without further initialization:

    var x Int        // &x is an *Int of value 0
    var r = &Rat{}   // r is a *Rat of value 0
    y := new(Float)  // y is a *Float of value 0

Alternatively, new values can be allocated and initialized with factory
functions of the form:

    func NewT(v V) *T

For instance, NewInt(x) returns an *Int set to the value of the int64 argument
x, NewRat(a, b) returns a *Rat set to the fraction a/b where a and b are int64
values, and NewFloat(f) returns a *Float initialized to the float64 argument f.
More flexibility is provided with explicit setters, for instance:

    var z1 Int
    z1.SetUint64(123)                 // z1 := 123
    z2 := new(Rat).SetFloat64(1.25)   // z2 := 5/4
    z3 := new(Float).SetInt(z1)       // z3 := 123.0

Setters, numeric operations and predicates are represented as methods of the
form:

    func (z *T) SetV(v V) *T          // z = v
    func (z *T) Unary(x *T) *T        // z = unary x
    func (z *T) Binary(x, y *T) *T    // z = x binary y
    func (x *T) Pred() P              // p = pred(x)

with T one of Int, Rat, or Float. For unary and binary operations, the result is
the receiver (usually named z in that case; see below); if it is one of the
operands x or y it may be safely overwritten (and its memory reused).

Arithmetic expressions are typically written as a sequence of individual method
calls, with each call corresponding to an operation. The receiver denotes the
result and the method arguments are the operation's operands. For instance,
given three *Int values a, b and c, the invocation

    c.Add(a, b)

computes the sum a + b and stores the result in c, overwriting whatever value
was held in c before. Unless specified otherwise, operations permit aliasing of
parameters, so it is perfectly ok to write

    sum.Add(sum, x)

to accumulate values x in a sum.

(By always passing in a result value via the receiver, memory use can be much
better controlled. Instead of having to allocate new memory for each result, an
operation can reuse the space allocated for the result value, and overwrite that
value with the new result in the process.)

Notational convention: Incoming method parameters (including the receiver) are
named consistently in the API to clarify their use. Incoming operands are
usually named x, y, a, b, and so on, but never z. A parameter specifying the
result is named z (typically the receiver).

For instance, the arguments for (*Int).Add are named x and y, and because the
receiver specifies the result destination, it is called z:

    func (z *Int) Add(x, y *Int) *Int

Methods of this form typically return the incoming receiver as well, to enable
simple call chaining.

Methods which don't require a result value to be passed in (for instance,
Int.Sign), simply return the result. In this case, the receiver is typically the
first operand, named x:

    func (x *Int) Sign() int

Various methods support conversions between strings and corresponding numeric
values, and vice versa: *Int, *Rat, and *Float values implement the Stringer
interface for a (default) string representation of the value, but also provide
SetString methods to initialize a value from a string in a variety of supported
formats (see the respective SetString documentation).

Finally, *Int, *Rat, and *Float satisfy the fmt package's Scanner interface for
scanning and (except for *Rat) the Formatter interface for formatted printing.

<a id="example_eConvergents"></a>
Example:

    package big_test

    import (
        "fmt"
        "math/big"
    )

    // Use the classic continued fraction for e
    //     e = [1; 0, 1, 1, 2, 1, 1, ... 2n, 1, 1, ...]
    // i.e., for the nth term, use
    //     1          if   n mod 3 != 1
    //  (n-1)/3 * 2   if   n mod 3 == 1
    func recur(n, lim int64) *big.Rat {
        term := new(big.Rat)
        if n%3 != 1 {
            term.SetInt64(1)
        } else {
            term.SetInt64((n - 1) / 3 * 2)
        }

        if n > lim {
            return term
        }

        // Directly initialize frac as the fractional
        // inverse of the result of recur.
        frac := new(big.Rat).Inv(recur(n+1, lim))

        return term.Add(term, frac)
    }

    // This example demonstrates how to use big.Rat to compute the
    // first 15 terms in the sequence of rational convergents for
    // the constant e (base of natural logarithm).
    func Example_eConvergents() {
        for i := 1; i <= 15; i++ {
            r := recur(0, int64(i))

            // Print r both as a fraction and as a floating-point number.
            // Since big.Rat implements fmt.Formatter, we can use %-13s to
            // get a left-aligned string representation of the fraction.
            fmt.Printf("%-13s = %s\n", r, r.FloatString(8))
        }

        // Output:
        // 2/1           = 2.00000000
        // 3/1           = 3.00000000
        // 8/3           = 2.66666667
        // 11/4          = 2.75000000
        // 19/7          = 2.71428571
        // 87/32         = 2.71875000
        // 106/39        = 2.71794872
        // 193/71        = 2.71830986
        // 1264/465      = 2.71827957
        // 1457/536      = 2.71828358
        // 2721/1001     = 2.71828172
        // 23225/8544    = 2.71828184
        // 25946/9545    = 2.71828182
        // 49171/18089   = 2.71828183
        // 517656/190435 = 2.71828183
    }


<a id="example_fibonacci"></a>
Example:

    // Initialize two big ints with the first two numbers in the sequence.
    a := big.NewInt(0)
    b := big.NewInt(1)

    // Initialize limit as 10^99, the smallest integer with 100 digits.
    var limit big.Int
    limit.Exp(big.NewInt(10), big.NewInt(99), nil)

    // Loop while a is smaller than 1e100.
    for a.Cmp(&limit) < 0 {
        // Compute the next Fibonacci number, storing it in a.
        a.Add(a, b)
        // Swap a and b so that b is the next number in the sequence.
        a, b = b, a
    }
    fmt.Println(a) // 100-digit Fibonacci number

    // Test a for primality.
    // (ProbablyPrimes' argument sets the number of Miller-Rabin
    // rounds to be performed. 20 is a good value.)
    fmt.Println(a.ProbablyPrime(20))

    // Output:
    // 1344719667586153181419716641724567886890850696275767987106294472017884974410332069524504824747437757
    // false


<a id="example_sqrt2"></a>
Example:

    // We'll do computations with 200 bits of precision in the mantissa.
    const prec = 200

    // Compute the square root of 2 using Newton's Method. We start with
    // an initial estimate for sqrt(2), and then iterate:
    //     x_{n+1} = 1/2 * ( x_n + (2.0 / x_n) )

    // Since Newton's Method doubles the number of correct digits at each
    // iteration, we need at least log_2(prec) steps.
    steps := int(math.Log2(prec))

    // Initialize values we need for the computation.
    two := new(big.Float).SetPrec(prec).SetInt64(2)
    half := new(big.Float).SetPrec(prec).SetFloat64(0.5)

    // Use 1 as the initial estimate.
    x := new(big.Float).SetPrec(prec).SetInt64(1)

    // We use t as a temporary variable. There's no need to set its precision
    // since big.Float values with unset (== 0) precision automatically assume
    // the largest precision of the arguments when used as the result (receiver)
    // of a big.Float operation.
    t := new(big.Float)

    // Iterate.
    for i := 0; i <= steps; i++ {
        t.Quo(two, x)  // t = 2.0 / x_n
        t.Add(x, t)    // t = x_n + (2.0 / x_n)
        x.Mul(half, t) // x_{n+1} = 0.5 * t
    }

    // We can use the usual fmt.Printf verbs since big.Float implements fmt.Formatter
    fmt.Printf("sqrt(2) = %.50f\n", x)

    // Print the error between 2 and x*x.
    t.Mul(x, x) // t = x*x
    fmt.Printf("error = %e\n", t.Sub(two, t))

    // Output:
    // sqrt(2) = 1.41421356237309504880168872420969807856967187537695
    // error = 0.000000e+00

## Index

- [Constants](#pkg-constants)
- [func Jacobi(x, y *Int) int](#Jacobi)
- [type Accuracy](#Accuracy)
  - [func (i Accuracy) String() string](#Accuracy.String)
- [type ErrNaN](#ErrNaN)
  - [func (err ErrNaN) Error() string](#ErrNaN.Error)
- [type Float](#Float)
  - [func NewFloat(x float64) *Float](#NewFloat)
  - [func ParseFloat(s string, base int, prec uint, mode RoundingMode) (f *Float, b int, err error)](#ParseFloat)
  - [func (z *Float) Abs(x *Float) *Float](#Float.Abs)
  - [func (x *Float) Acc() Accuracy](#Float.Acc)
  - [func (z *Float) Add(x, y *Float) *Float](#Float.Add)
  - [func (x *Float) Append(buf []byte, fmt byte, prec int) []byte](#Float.Append)
  - [func (x *Float) Cmp(y *Float) int](#Float.Cmp)
  - [func (z *Float) Copy(x *Float) *Float](#Float.Copy)
  - [func (x *Float) Float32() (float32, Accuracy)](#Float.Float32)
  - [func (x *Float) Float64() (float64, Accuracy)](#Float.Float64)
  - [func (x *Float) Format(s fmt.State, format rune)](#Float.Format)
  - [func (z *Float) GobDecode(buf []byte) error](#Float.GobDecode)
  - [func (x *Float) GobEncode() ([]byte, error)](#Float.GobEncode)
  - [func (x *Float) Int(z *Int) (*Int, Accuracy)](#Float.Int)
  - [func (x *Float) Int64() (int64, Accuracy)](#Float.Int64)
  - [func (x *Float) IsInf() bool](#Float.IsInf)
  - [func (x *Float) IsInt() bool](#Float.IsInt)
  - [func (x *Float) MantExp(mant *Float) (exp int)](#Float.MantExp)
  - [func (x *Float) MarshalText() (text []byte, err error)](#Float.MarshalText)
  - [func (x *Float) MinPrec() uint](#Float.MinPrec)
  - [func (x *Float) Mode() RoundingMode](#Float.Mode)
  - [func (z *Float) Mul(x, y *Float) *Float](#Float.Mul)
  - [func (z *Float) Neg(x *Float) *Float](#Float.Neg)
  - [func (z *Float) Parse(s string, base int) (f *Float, b int, err error)](#Float.Parse)
  - [func (x *Float) Prec() uint](#Float.Prec)
  - [func (z *Float) Quo(x, y *Float) *Float](#Float.Quo)
  - [func (x *Float) Rat(z *Rat) (*Rat, Accuracy)](#Float.Rat)
  - [func (z *Float) Scan(s fmt.ScanState, ch rune) error](#Float.Scan)
  - [func (z *Float) Set(x *Float) *Float](#Float.Set)
  - [func (z *Float) SetFloat64(x float64) *Float](#Float.SetFloat64)
  - [func (z *Float) SetInf(signbit bool) *Float](#Float.SetInf)
  - [func (z *Float) SetInt(x *Int) *Float](#Float.SetInt)
  - [func (z *Float) SetInt64(x int64) *Float](#Float.SetInt64)
  - [func (z *Float) SetMantExp(mant *Float, exp int) *Float](#Float.SetMantExp)
  - [func (z *Float) SetMode(mode RoundingMode) *Float](#Float.SetMode)
  - [func (z *Float) SetPrec(prec uint) *Float](#Float.SetPrec)
  - [func (z *Float) SetRat(x *Rat) *Float](#Float.SetRat)
  - [func (z *Float) SetString(s string) (*Float, bool)](#Float.SetString)
  - [func (z *Float) SetUint64(x uint64) *Float](#Float.SetUint64)
  - [func (x *Float) Sign() int](#Float.Sign)
  - [func (x *Float) Signbit() bool](#Float.Signbit)
  - [func (z *Float) Sqrt(x *Float) *Float](#Float.Sqrt)
  - [func (x *Float) String() string](#Float.String)
  - [func (z *Float) Sub(x, y *Float) *Float](#Float.Sub)
  - [func (x *Float) Text(format byte, prec int) string](#Float.Text)
  - [func (x *Float) Uint64() (uint64, Accuracy)](#Float.Uint64)
  - [func (z *Float) UnmarshalText(text []byte) error](#Float.UnmarshalText)
- [type Int](#Int)
  - [func NewInt(x int64) *Int](#NewInt)
  - [func (z *Int) Abs(x *Int) *Int](#Int.Abs)
  - [func (z *Int) Add(x, y *Int) *Int](#Int.Add)
  - [func (z *Int) And(x, y *Int) *Int](#Int.And)
  - [func (z *Int) AndNot(x, y *Int) *Int](#Int.AndNot)
  - [func (x *Int) Append(buf []byte, base int) []byte](#Int.Append)
  - [func (z *Int) Binomial(n, k int64) *Int](#Int.Binomial)
  - [func (x *Int) Bit(i int) uint](#Int.Bit)
  - [func (x *Int) BitLen() int](#Int.BitLen)
  - [func (x *Int) Bits() []Word](#Int.Bits)
  - [func (x *Int) Bytes() []byte](#Int.Bytes)
  - [func (x *Int) Cmp(y *Int) (r int)](#Int.Cmp)
  - [func (x *Int) CmpAbs(y *Int) int](#Int.CmpAbs)
  - [func (z *Int) Div(x, y *Int) *Int](#Int.Div)
  - [func (z *Int) DivMod(x, y, m *Int) (*Int, *Int)](#Int.DivMod)
  - [func (z *Int) Exp(x, y, m *Int) *Int](#Int.Exp)
  - [func (x *Int) Format(s fmt.State, ch rune)](#Int.Format)
  - [func (z *Int) GCD(x, y, a, b *Int) *Int](#Int.GCD)
  - [func (z *Int) GobDecode(buf []byte) error](#Int.GobDecode)
  - [func (x *Int) GobEncode() ([]byte, error)](#Int.GobEncode)
  - [func (x *Int) Int64() int64](#Int.Int64)
  - [func (x *Int) IsInt64() bool](#Int.IsInt64)
  - [func (x *Int) IsUint64() bool](#Int.IsUint64)
  - [func (z *Int) Lsh(x *Int, n uint) *Int](#Int.Lsh)
  - [func (x *Int) MarshalJSON() ([]byte, error)](#Int.MarshalJSON)
  - [func (x *Int) MarshalText() (text []byte, err error)](#Int.MarshalText)
  - [func (z *Int) Mod(x, y *Int) *Int](#Int.Mod)
  - [func (z *Int) ModInverse(g, n *Int) *Int](#Int.ModInverse)
  - [func (z *Int) ModSqrt(x, p *Int) *Int](#Int.ModSqrt)
  - [func (z *Int) Mul(x, y *Int) *Int](#Int.Mul)
  - [func (z *Int) MulRange(a, b int64) *Int](#Int.MulRange)
  - [func (z *Int) Neg(x *Int) *Int](#Int.Neg)
  - [func (z *Int) Not(x *Int) *Int](#Int.Not)
  - [func (z *Int) Or(x, y *Int) *Int](#Int.Or)
  - [func (x *Int) ProbablyPrime(n int) bool](#Int.ProbablyPrime)
  - [func (z *Int) Quo(x, y *Int) *Int](#Int.Quo)
  - [func (z *Int) QuoRem(x, y, r *Int) (*Int, *Int)](#Int.QuoRem)
  - [func (z *Int) Rand(rnd *rand.Rand, n *Int) *Int](#Int.Rand)
  - [func (z *Int) Rem(x, y *Int) *Int](#Int.Rem)
  - [func (z *Int) Rsh(x *Int, n uint) *Int](#Int.Rsh)
  - [func (z *Int) Scan(s fmt.ScanState, ch rune) error](#Int.Scan)
  - [func (z *Int) Set(x *Int) *Int](#Int.Set)
  - [func (z *Int) SetBit(x *Int, i int, b uint) *Int](#Int.SetBit)
  - [func (z *Int) SetBits(abs []Word) *Int](#Int.SetBits)
  - [func (z *Int) SetBytes(buf []byte) *Int](#Int.SetBytes)
  - [func (z *Int) SetInt64(x int64) *Int](#Int.SetInt64)
  - [func (z *Int) SetString(s string, base int) (*Int, bool)](#Int.SetString)
  - [func (z *Int) SetUint64(x uint64) *Int](#Int.SetUint64)
  - [func (x *Int) Sign() int](#Int.Sign)
  - [func (z *Int) Sqrt(x *Int) *Int](#Int.Sqrt)
  - [func (x *Int) String() string](#Int.String)
  - [func (z *Int) Sub(x, y *Int) *Int](#Int.Sub)
  - [func (x *Int) Text(base int) string](#Int.Text)
  - [func (x *Int) Uint64() uint64](#Int.Uint64)
  - [func (z *Int) UnmarshalJSON(text []byte) error](#Int.UnmarshalJSON)
  - [func (z *Int) UnmarshalText(text []byte) error](#Int.UnmarshalText)
  - [func (z *Int) Xor(x, y *Int) *Int](#Int.Xor)
- [type Rat](#Rat)
  - [func NewRat(a, b int64) *Rat](#NewRat)
  - [func (z *Rat) Abs(x *Rat) *Rat](#Rat.Abs)
  - [func (z *Rat) Add(x, y *Rat) *Rat](#Rat.Add)
  - [func (x *Rat) Cmp(y *Rat) int](#Rat.Cmp)
  - [func (x *Rat) Denom() *Int](#Rat.Denom)
  - [func (x *Rat) Float32() (f float32, exact bool)](#Rat.Float32)
  - [func (x *Rat) Float64() (f float64, exact bool)](#Rat.Float64)
  - [func (x *Rat) FloatString(prec int) string](#Rat.FloatString)
  - [func (z *Rat) GobDecode(buf []byte) error](#Rat.GobDecode)
  - [func (x *Rat) GobEncode() ([]byte, error)](#Rat.GobEncode)
  - [func (z *Rat) Inv(x *Rat) *Rat](#Rat.Inv)
  - [func (x *Rat) IsInt() bool](#Rat.IsInt)
  - [func (x *Rat) MarshalText() (text []byte, err error)](#Rat.MarshalText)
  - [func (z *Rat) Mul(x, y *Rat) *Rat](#Rat.Mul)
  - [func (z *Rat) Neg(x *Rat) *Rat](#Rat.Neg)
  - [func (x *Rat) Num() *Int](#Rat.Num)
  - [func (z *Rat) Quo(x, y *Rat) *Rat](#Rat.Quo)
  - [func (x *Rat) RatString() string](#Rat.RatString)
  - [func (z *Rat) Scan(s fmt.ScanState, ch rune) error](#Rat.Scan)
  - [func (z *Rat) Set(x *Rat) *Rat](#Rat.Set)
  - [func (z *Rat) SetFloat64(f float64) *Rat](#Rat.SetFloat64)
  - [func (z *Rat) SetFrac(a, b *Int) *Rat](#Rat.SetFrac)
  - [func (z *Rat) SetFrac64(a, b int64) *Rat](#Rat.SetFrac64)
  - [func (z *Rat) SetInt(x *Int) *Rat](#Rat.SetInt)
  - [func (z *Rat) SetInt64(x int64) *Rat](#Rat.SetInt64)
  - [func (z *Rat) SetString(s string) (*Rat, bool)](#Rat.SetString)
  - [func (x *Rat) Sign() int](#Rat.Sign)
  - [func (x *Rat) String() string](#Rat.String)
  - [func (z *Rat) Sub(x, y *Rat) *Rat](#Rat.Sub)
  - [func (z *Rat) UnmarshalText(text []byte) error](#Rat.UnmarshalText)
- [type RoundingMode](#RoundingMode)
  - [func (i RoundingMode) String() string](#RoundingMode.String)
- [type Word](#Word)
- [Bugs](#pkg-note-BUG)

### Examples

- [Float.Add](#exampleFloat_Add)
- [Float.Cmp](#exampleFloat_Cmp)
- [Float.Scan](#exampleFloat_Scan)
- [Float (Shift)](#exampleFloat_shift)
- [Int.Scan](#exampleInt_Scan)
- [Int.SetString](#exampleInt_SetString)
- [Rat.Scan](#exampleRat_Scan)
- [Rat.SetString](#exampleRat_SetString)
- [RoundingMode](#exampleRoundingMode)
- [Package (EConvergents)](#example_eConvergents)
- [Package (Fibonacci)](#example_fibonacci)
- [Package (Sqrt2)](#example_sqrt2)

### Package files
 [accuracy_string.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/accuracy_string.go) [arith.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/arith.go) [arith_decl.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/arith_decl.go) [decimal.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/decimal.go) [doc.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/doc.go) [float.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go) [floatconv.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/floatconv.go) [floatmarsh.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/floatmarsh.go) [ftoa.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ftoa.go) [int.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go) [intconv.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intconv.go) [intmarsh.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intmarsh.go) [nat.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/nat.go) [natconv.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/natconv.go) [prime.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/prime.go) [rat.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go) [ratconv.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratconv.go) [ratmarsh.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratmarsh.go) [roundingmode_string.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/roundingmode_string.go) [sqrt.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/sqrt.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="MaxExp">MaxExp</span>  = <a href="/math/">math</a>.<a href="/math/#MaxInt32">MaxInt32</a>  <span class="comment">// largest supported exponent</span>
    <span id="MinExp">MinExp</span>  = <a href="/math/">math</a>.<a href="/math/#MinInt32">MinInt32</a>  <span class="comment">// smallest supported exponent</span>
    <span id="MaxPrec">MaxPrec</span> = <a href="/math/">math</a>.<a href="/math/#MaxUint32">MaxUint32</a> <span class="comment">// largest (theoretically) supported precision; likely memory-limited</span>
)</pre>

Exponent and precision limits.

<pre>const <span id="MaxBase">MaxBase</span> = 10 + (&#39;z&#39; - &#39;a&#39; + 1) + (&#39;Z&#39; - &#39;A&#39; + 1)</pre>

MaxBase is the largest number base accepted for string conversions.

<h2 id="Jacobi">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L672">Jacobi</a>
    <a href="#Jacobi">¶</a></h2>
<pre>func Jacobi(x, y *<a href="#Int">Int</a>) <a href="/builtin/#int">int</a></pre>

Jacobi returns the Jacobi symbol (x/y), either +1, -1, or 0. The y argument must
be an odd integer.

<h2 id="Accuracy">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L132">Accuracy</a>
    <a href="#Accuracy">¶</a></h2>
<pre>type Accuracy <a href="/builtin/#int8">int8</a></pre>

Accuracy describes the rounding error produced by the most recent operation that
generated a Float value, relative to the exact value.

<pre>const (
    <span id="Below">Below</span> <a href="#Accuracy">Accuracy</a> = -1
    <span id="Exact">Exact</span> <a href="#Accuracy">Accuracy</a> = 0
    <span id="Above">Above</span> <a href="#Accuracy">Accuracy</a> = +1
)</pre>

Constants describing the Accuracy of a Float.

<h3 id="Accuracy.String">func (Accuracy) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/accuracy_string.go#L1">String</a>
    <a href="#Accuracy.String">¶</a></h3>
<pre>func (i <a href="#Accuracy">Accuracy</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="ErrNaN">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L61">ErrNaN</a>
    <a href="#ErrNaN">¶</a></h2>
<pre>type ErrNaN struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

An ErrNaN panic is raised by a Float operation that would lead to a NaN under
IEEE-754 rules. An ErrNaN implements the error interface.

<h3 id="ErrNaN.Error">func (ErrNaN) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L65">Error</a>
    <a href="#ErrNaN.Error">¶</a></h3>
<pre>func (err <a href="#ErrNaN">ErrNaN</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Float">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L49">Float</a>
    <a href="#Float">¶</a></h2>
<pre>type Float struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A nonzero finite Float represents a multi-precision floating point number

    sign × mantissa × 2**exponent

with 0.5 <= mantissa < 1.0, and MinExp <= exponent <= MaxExp. A Float may also
be zero (+0, -0) or infinite (+Inf, -Inf). All Floats are ordered, and the
ordering of two Floats x and y is defined by x.Cmp(y).

Each Float value also has a precision, rounding mode, and accuracy. The
precision is the maximum number of mantissa bits available to represent the
value. The rounding mode specifies how a result should be rounded to fit into
the mantissa bits, and accuracy describes the rounding error with respect to the
exact result.

Unless specified otherwise, all operations (including setters) that specify a
*Float variable for the result (usually via the receiver with the exception of
MantExp), round the numeric result according to the precision and rounding mode
of the result variable.

If the provided result precision is 0 (see below), it is set to the precision of
the argument with the largest precision value before any rounding takes place,
and the rounding mode remains unchanged. Thus, uninitialized Floats provided as
result arguments will have their precision set to a reasonable value determined
by the operands and their mode is the zero value for RoundingMode
(ToNearestEven).

By setting the desired precision to 24 or 53 and using matching rounding mode
(typically ToNearestEven), Float operations produce the same results as the
corresponding float32 or float64 IEEE-754 arithmetic for operands that
correspond to normal (i.e., not denormal) float32 or float64 numbers. Exponent
underflow and overflow lead to a 0 or an Infinity for different values than
IEEE-754 because Float exponents have a much larger range.

The zero (uninitialized) value for a Float is ready to use and represents the
number +0.0 exactly, with precision 0 and rounding mode ToNearestEven.

<a id="exampleFloat_shift"></a>
Example:

    // Implement Float "shift" by modifying the (binary) exponents directly.
    for s := -5; s <= 5; s++ {
        x := big.NewFloat(0.5)
        x.SetMantExp(x, x.MantExp(nil)+s) // shift x by s
        fmt.Println(x)
    }
    // Output:
    // 0.015625
    // 0.03125
    // 0.0625
    // 0.125
    // 0.25
    // 0.5
    // 1
    // 2
    // 4
    // 8
    // 16

<h3 id="NewFloat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L72">NewFloat</a>
    <a href="#NewFloat">¶</a></h3>
<pre>func NewFloat(x <a href="/builtin/#float64">float64</a>) *<a href="#Float">Float</a></pre>

NewFloat allocates and returns a new Float set to x, with precision 53 and
rounding mode ToNearestEven. NewFloat panics with ErrNaN if x is a NaN.

<h3 id="ParseFloat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/floatconv.go#L268">ParseFloat</a>
    <a href="#ParseFloat">¶</a></h3>
<pre>func ParseFloat(s <a href="/builtin/#string">string</a>, base <a href="/builtin/#int">int</a>, prec <a href="/builtin/#uint">uint</a>, mode <a href="#RoundingMode">RoundingMode</a>) (f *<a href="#Float">Float</a>, b <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ParseFloat is like f.Parse(s, base) with f set to the given precision and
rounding mode.

<h3 id="Float.Abs">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L1155">Abs</a>
    <a href="#Float.Abs">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Abs(x *<a href="#Float">Float</a>) *<a href="#Float">Float</a></pre>

Abs sets z to the (possibly rounded) value |x| (the absolute value of x) and
returns z.

<h3 id="Float.Acc">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L212">Acc</a>
    <a href="#Float.Acc">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Acc() <a href="#Accuracy">Accuracy</a></pre>

Acc returns the accuracy of x produced by the most recent operation.

<h3 id="Float.Add">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L1424">Add</a>
    <a href="#Float.Add">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Add(x, y *<a href="#Float">Float</a>) *<a href="#Float">Float</a></pre>

Add sets z to the rounded sum x+y and returns z. If z's precision is 0, it is
changed to the larger of x's or y's precision before the operation. Rounding is
performed according to z's precision and rounding mode; and z's accuracy reports
the result error relative to the exact (not rounded) result. Add panics with
ErrNaN if x and y are infinities with opposite signs. The value of z is
undefined in that case.

BUG(gri) When rounding ToNegativeInf, the sign of Float values rounded to 0 is
incorrect.

<a id="exampleFloat_Add"></a>
Example:

    // Operate on numbers of different precision.
    var x, y, z big.Float
    x.SetInt64(1000)          // x is automatically set to 64bit precision
    y.SetFloat64(2.718281828) // y is automatically set to 53bit precision
    z.SetPrec(32)
    z.Add(&x, &y)
    fmt.Printf("x = %.10g (%s, prec = %d, acc = %s)\n", &x, x.Text('p', 0), x.Prec(), x.Acc())
    fmt.Printf("y = %.10g (%s, prec = %d, acc = %s)\n", &y, y.Text('p', 0), y.Prec(), y.Acc())
    fmt.Printf("z = %.10g (%s, prec = %d, acc = %s)\n", &z, z.Text('p', 0), z.Prec(), z.Acc())
    // Output:
    // x = 1000 (0x.fap+10, prec = 64, acc = Exact)
    // y = 2.718281828 (0x.adf85458248cd8p+2, prec = 53, acc = Exact)
    // z = 1002.718282 (0x.faadf854p+10, prec = 32, acc = Below)

<h3 id="Float.Append">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ftoa.go#L49">Append</a>
    <a href="#Float.Append">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Append(buf []<a href="/builtin/#byte">byte</a>, fmt <a href="/builtin/#byte">byte</a>, prec <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

Append appends to buf the string form of the floating-point number x, as
generated by x.Text, and returns the extended buffer.

<h3 id="Float.Cmp">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L1651">Cmp</a>
    <a href="#Float.Cmp">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Cmp(y *<a href="#Float">Float</a>) <a href="/builtin/#int">int</a></pre>

Cmp compares x and y and returns:

    -1 if x <  y
     0 if x == y (incl. -0 == 0, -Inf == -Inf, and +Inf == +Inf)
    +1 if x >  y

<a id="exampleFloat_Cmp"></a>
Example:

    inf := math.Inf(1)
    zero := 0.0

    operands := []float64{-inf, -1.2, -zero, 0, +1.2, +inf}

    fmt.Println("   x     y  cmp")
    fmt.Println("---------------")
    for _, x64 := range operands {
        x := big.NewFloat(x64)
        for _, y64 := range operands {
            y := big.NewFloat(y64)
            fmt.Printf("%4g  %4g  %3d\n", x, y, x.Cmp(y))
        }
        fmt.Println()
    }

    // Output:
    //    x     y  cmp
    // ---------------
    // -Inf  -Inf    0
    // -Inf  -1.2   -1
    // -Inf    -0   -1
    // -Inf     0   -1
    // -Inf   1.2   -1
    // -Inf  +Inf   -1
    //
    // -1.2  -Inf    1
    // -1.2  -1.2    0
    // -1.2    -0   -1
    // -1.2     0   -1
    // -1.2   1.2   -1
    // -1.2  +Inf   -1
    //
    //   -0  -Inf    1
    //   -0  -1.2    1
    //   -0    -0    0
    //   -0     0    0
    //   -0   1.2   -1
    //   -0  +Inf   -1
    //
    //    0  -Inf    1
    //    0  -1.2    1
    //    0    -0    0
    //    0     0    0
    //    0   1.2   -1
    //    0  +Inf   -1
    //
    //  1.2  -Inf    1
    //  1.2  -1.2    1
    //  1.2    -0    1
    //  1.2     0    1
    //  1.2   1.2    0
    //  1.2  +Inf   -1
    //
    // +Inf  -Inf    1
    // +Inf  -1.2    1
    // +Inf    -0    1
    // +Inf     0    1
    // +Inf   1.2    1
    // +Inf  +Inf    0

<h3 id="Float.Copy">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L651">Copy</a>
    <a href="#Float.Copy">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Copy(x *<a href="#Float">Float</a>) *<a href="#Float">Float</a></pre>

Copy sets z to x, with the same precision, rounding mode, and accuracy as x, and
returns z. x is not changed even if z and x are the same.

<h3 id="Float.Float32">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L814">Float32</a>
    <a href="#Float.Float32">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Float32() (<a href="/builtin/#float32">float32</a>, <a href="#Accuracy">Accuracy</a>)</pre>

Float32 returns the float32 value nearest to x. If x is too small to be
represented by a float32 (|x| < math.SmallestNonzeroFloat32), the result is (0,
Below) or (-0, Above), respectively, depending on the sign of x. If x is too
large to be represented by a float32 (|x| > math.MaxFloat32), the result is
(+Inf, Above) or (-Inf, Below), depending on the sign of x.

<h3 id="Float.Float64">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L934">Float64</a>
    <a href="#Float.Float64">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Float64() (<a href="/builtin/#float64">float64</a>, <a href="#Accuracy">Accuracy</a>)</pre>

Float64 returns the float64 value nearest to x. If x is too small to be
represented by a float64 (|x| < math.SmallestNonzeroFloat64), the result is (0,
Below) or (-0, Above), respectively, depending on the sign of x. If x is too
large to be represented by a float64 (|x| > math.MaxFloat64), the result is
(+Inf, Above) or (-Inf, Below), depending on the sign of x.

<h3 id="Float.Format">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ftoa.go#L380">Format</a>
    <a href="#Float.Format">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Format(s <a href="/fmt/">fmt</a>.<a href="/fmt/#State">State</a>, format <a href="/builtin/#rune">rune</a>)</pre>

Format implements fmt.Formatter. It accepts all the regular formats for
floating-point numbers ('b', 'e', 'E', 'f', 'F', 'g', 'G') as well as 'p' and
'v'. See (*Float).Text for the interpretation of 'p'. The 'v' format is handled
like 'g'. Format also supports specification of the minimum precision in digits,
the output field width, as well as the format flags '+' and ' ' for sign
control, '0' for space or zero padding, and '-' for left or right justification.
See the fmt package for details.

<h3 id="Float.GobDecode">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/floatmarsh.go#L54">GobDecode</a>
    <a href="#Float.GobDecode">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) GobDecode(buf []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

GobDecode implements the gob.GobDecoder interface. The result is rounded per the
precision and rounding mode of z unless z's precision is 0, in which case z is
set exactly to the decoded value.

<h3 id="Float.GobEncode">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/floatmarsh.go#L10">GobEncode</a>
    <a href="#Float.GobEncode">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) GobEncode() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

GobEncode implements the gob.GobEncoder interface. The Float value and all its
attributes (precision, rounding mode, accuracy) are marshaled.

<h3 id="Float.Int">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L1055">Int</a>
    <a href="#Float.Int">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Int(z *<a href="#Int">Int</a>) (*<a href="#Int">Int</a>, <a href="#Accuracy">Accuracy</a>)</pre>

Int returns the result of truncating x towards zero; or nil if x is an infinity.
The result is Exact if x.IsInt(); otherwise it is Below for x > 0, and Above for
x < 0. If a non-nil *Int argument z is provided, Int stores the result in z
instead of allocating a new Int.

<h3 id="Float.Int64">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L759">Int64</a>
    <a href="#Float.Int64">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Int64() (<a href="/builtin/#int64">int64</a>, <a href="#Accuracy">Accuracy</a>)</pre>

Int64 returns the integer resulting from truncating x towards zero. If
math.MinInt64 <= x <= math.MaxInt64, the result is Exact if x is an integer, and
Above (x < 0) or Below (x > 0) otherwise. The result is (math.MinInt64, Above)
for x < math.MinInt64, and (math.MaxInt64, Below) for x > math.MaxInt64.

<h3 id="Float.IsInf">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L320">IsInf</a>
    <a href="#Float.IsInf">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) IsInf() <a href="/builtin/#bool">bool</a></pre>

IsInf reports whether x is +Inf or -Inf.

<h3 id="Float.IsInt">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L326">IsInt</a>
    <a href="#Float.IsInt">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) IsInt() <a href="/builtin/#bool">bool</a></pre>

IsInt reports whether x is an integer. ±Inf values are not integers.

<h3 id="Float.MantExp">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L250">MantExp</a>
    <a href="#Float.MantExp">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) MantExp(mant *<a href="#Float">Float</a>) (exp <a href="/builtin/#int">int</a>)</pre>

MantExp breaks x into its mantissa and exponent components and returns the
exponent. If a non-nil mant argument is provided its value is set to the
mantissa of x, with the same precision and rounding mode as x. The components
satisfy x == mant × 2**exp, with 0.5 <= |mant| < 1.0. Calling MantExp with a nil
argument is an efficient way to get the exponent of the receiver.

Special cases are:

    (  ±0).MantExp(mant) = 0, with mant set to   ±0
    (±Inf).MantExp(mant) = 0, with mant set to ±Inf

x and mant may be the same in which case x is set to its mantissa value.

<h3 id="Float.MarshalText">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/floatmarsh.go#L91">MarshalText</a>
    <a href="#Float.MarshalText">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) MarshalText() (text []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

MarshalText implements the encoding.TextMarshaler interface. Only the Float
value is marshaled (in full precision), other attributes such as precision or
accuracy are ignored.

<h3 id="Float.MinPrec">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L199">MinPrec</a>
    <a href="#Float.MinPrec">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) MinPrec() <a href="/builtin/#uint">uint</a></pre>

MinPrec returns the minimum precision required to represent x exactly (i.e., the
smallest prec before x.SetPrec(prec) would start rounding x). The result is 0
for |x| == 0 and |x| == Inf.

<h3 id="Float.Mode">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L207">Mode</a>
    <a href="#Float.Mode">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Mode() <a href="#RoundingMode">RoundingMode</a></pre>

Mode returns the rounding mode of x.

<h3 id="Float.Mul">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L1559">Mul</a>
    <a href="#Float.Mul">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Mul(x, y *<a href="#Float">Float</a>) *<a href="#Float">Float</a></pre>

Mul sets z to the rounded product x*y and returns z. Precision, rounding, and
accuracy reporting are as for Add. Mul panics with ErrNaN if one operand is zero
and the other operand an infinity. The value of z is undefined in that case.

<h3 id="Float.Neg">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L1163">Neg</a>
    <a href="#Float.Neg">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Neg(x *<a href="#Float">Float</a>) *<a href="#Float">Float</a></pre>

Neg sets z to the (possibly rounded) value of x with its sign negated, and
returns z.

<h3 id="Float.Parse">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/floatconv.go#L240">Parse</a>
    <a href="#Float.Parse">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Parse(s <a href="/builtin/#string">string</a>, base <a href="/builtin/#int">int</a>) (f *<a href="#Float">Float</a>, b <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Parse parses s which must contain a text representation of a floating- point
number with a mantissa in the given conversion base (the exponent is always a
decimal number), or a string representing an infinite value.

It sets z to the (possibly rounded) value of the corresponding floating- point
value, and returns z, the actual base b, and an error err, if any. The entire
string (not just a prefix) must be consumed for success. If z's precision is 0,
it is changed to 64 before rounding takes effect. The number must be of the
form:

    number   = [ sign ] [ prefix ] mantissa [ exponent ] | infinity .
    sign     = "+" | "-" .
    prefix   = "0" ( "x" | "X" | "b" | "B" ) .
    mantissa = digits | digits "." [ digits ] | "." digits .
    exponent = ( "E" | "e" | "p" ) [ sign ] digits .
    digits   = digit { digit } .
    digit    = "0" ... "9" | "a" ... "z" | "A" ... "Z" .
    infinity = [ sign ] ( "inf" | "Inf" ) .

The base argument must be 0, 2, 10, or 16. Providing an invalid base argument
will lead to a run-time panic.

For base 0, the number prefix determines the actual base: A prefix of "0x" or
"0X" selects base 16, and a "0b" or "0B" prefix selects base 2; otherwise, the
actual base is 10 and no prefix is accepted. The octal prefix "0" is not
supported (a leading "0" is simply considered a "0").

A "p" exponent indicates a binary (rather then decimal) exponent; for instance
"0x1.fffffffffffffp1023" (using base 0) represents the maximum float64 value.
For hexadecimal mantissae, the exponent must be binary, if present (an "e" or
"E" exponent indicator cannot be distinguished from a mantissa digit).

The returned *Float f is nil and the value of z is valid but not defined if an
error is reported.

<h3 id="Float.Prec">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L192">Prec</a>
    <a href="#Float.Prec">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Prec() <a href="/builtin/#uint">uint</a></pre>

Prec returns the mantissa precision of x in bits. The result may be 0 for |x| ==
0 and |x| == Inf.

<h3 id="Float.Quo">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L1604">Quo</a>
    <a href="#Float.Quo">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Quo(x, y *<a href="#Float">Float</a>) *<a href="#Float">Float</a></pre>

Quo sets z to the rounded quotient x/y and returns z. Precision, rounding, and
accuracy reporting are as for Add. Quo panics with ErrNaN if both operands are
zero or infinities. The value of z is undefined in that case.

<h3 id="Float.Rat">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L1111">Rat</a>
    <a href="#Float.Rat">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Rat(z *<a href="#Rat">Rat</a>) (*<a href="#Rat">Rat</a>, <a href="#Accuracy">Accuracy</a>)</pre>

Rat returns the rational number corresponding to x; or nil if x is an infinity.
The result is Exact if x is not an Inf. If a non-nil *Rat argument z is
provided, Rat stores the result in z instead of allocating a new Rat.

<h3 id="Float.Scan">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/floatconv.go#L279">Scan</a>
    <a href="#Float.Scan">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Scan(s <a href="/fmt/">fmt</a>.<a href="/fmt/#ScanState">ScanState</a>, ch <a href="/builtin/#rune">rune</a>) <a href="/builtin/#error">error</a></pre>

Scan is a support routine for fmt.Scanner; it sets z to the value of the scanned
number. It accepts formats whose verbs are supported by fmt.Scan for floating
point values, which are: 'b' (binary), 'e', 'E', 'f', 'F', 'g' and 'G'. Scan
doesn't handle ±Inf.

<a id="exampleFloat_Scan"></a>
Example:

    // The Scan function is rarely used directly;
    // the fmt package recognizes it as an implementation of fmt.Scanner.
    f := new(big.Float)
    _, err := fmt.Sscan("1.19282e99", f)
    if err != nil {
        log.Println("error scanning value:", err)
    } else {
        fmt.Println(f)
    }
    // Output: 1.19282e+99

<h3 id="Float.Set">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L627">Set</a>
    <a href="#Float.Set">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Set(x *<a href="#Float">Float</a>) *<a href="#Float">Float</a></pre>

Set sets z to the (possibly rounded) value of x and returns z. If z's precision
is 0, it is changed to the precision of x before setting z (and rounding will
have no effect). Rounding is performed according to z's precision and rounding
mode; and z's accuracy reports the result error relative to the exact (not
rounded) result.

<h3 id="Float.SetFloat64">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L525">SetFloat64</a>
    <a href="#Float.SetFloat64">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) SetFloat64(x <a href="/builtin/#float64">float64</a>) *<a href="#Float">Float</a></pre>

SetFloat64 sets z to the (possibly rounded) value of x and returns z. If z's
precision is 0, it is changed to 53 (and rounding will have no effect).
SetFloat64 panics with ErrNaN if x is a NaN.

<h3 id="Float.SetInf">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L614">SetInf</a>
    <a href="#Float.SetInf">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) SetInf(signbit <a href="/builtin/#bool">bool</a>) *<a href="#Float">Float</a></pre>

SetInf sets z to the infinite Float -Inf if signbit is set, or +Inf if signbit
is not set, and returns z. The precision of z is unchanged and the result is
always Exact.

<h3 id="Float.SetInt">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L573">SetInt</a>
    <a href="#Float.SetInt">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) SetInt(x *<a href="#Int">Int</a>) *<a href="#Float">Float</a></pre>

SetInt sets z to the (possibly rounded) value of x and returns z. If z's
precision is 0, it is changed to the larger of x.BitLen() or 64 (and rounding
will have no effect).

<h3 id="Float.SetInt64">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L512">SetInt64</a>
    <a href="#Float.SetInt64">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) SetInt64(x <a href="/builtin/#int64">int64</a>) *<a href="#Float">Float</a></pre>

SetInt64 sets z to the (possibly rounded) value of x and returns z. If z's
precision is 0, it is changed to 64 (and rounding will have no effect).

<h3 id="Float.SetMantExp">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L301">SetMantExp</a>
    <a href="#Float.SetMantExp">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) SetMantExp(mant *<a href="#Float">Float</a>, exp <a href="/builtin/#int">int</a>) *<a href="#Float">Float</a></pre>

SetMantExp sets z to mant × 2**exp and and returns z. The result z has the same
precision and rounding mode as mant. SetMantExp is an inverse of MantExp but
does not require 0.5 <= |mant| < 1.0. Specifically:

    mant := new(Float)
    new(Float).SetMantExp(mant, x.MantExp(mant)).Cmp(x) == 0

Special cases are:

    z.SetMantExp(  ±0, exp) =   ±0
    z.SetMantExp(±Inf, exp) = ±Inf

z and mant may be the same in which case z's exponent is set to exp.

<h3 id="Float.SetMode">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L184">SetMode</a>
    <a href="#Float.SetMode">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) SetMode(mode <a href="#RoundingMode">RoundingMode</a>) *<a href="#Float">Float</a></pre>

SetMode sets z's rounding mode to mode and returns an exact z. z remains
unchanged otherwise. z.SetMode(z.Mode()) is a cheap way to set z's accuracy to
Exact.

<h3 id="Float.SetPrec">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L148">SetPrec</a>
    <a href="#Float.SetPrec">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) SetPrec(prec <a href="/builtin/#uint">uint</a>) *<a href="#Float">Float</a></pre>

SetPrec sets z's precision to prec and returns the (possibly) rounded value of
z. Rounding occurs according to z's rounding mode if the mantissa cannot be
represented in prec bits without loss of precision. SetPrec(0) maps all finite
values to ±0; infinite values remain unchanged. If prec > MaxPrec, it is set to
MaxPrec.

<h3 id="Float.SetRat">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L597">SetRat</a>
    <a href="#Float.SetRat">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) SetRat(x *<a href="#Rat">Rat</a>) *<a href="#Float">Float</a></pre>

SetRat sets z to the (possibly rounded) value of x and returns z. If z's
precision is 0, it is changed to the largest of a.BitLen(), b.BitLen(), or 64;
with x = a/b.

<h3 id="Float.SetString">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/floatconv.go#L12">SetString</a>
    <a href="#Float.SetString">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) SetString(s <a href="/builtin/#string">string</a>) (*<a href="#Float">Float</a>, <a href="/builtin/#bool">bool</a>)</pre>

SetString sets z to the value of s and returns z and a boolean indicating
success. s must be a floating-point number of the same format as accepted by
Parse, with base argument 0. The entire string (not just a prefix) must be valid
for success. If the operation failed, the value of z is undefined but the
returned value is nil.

<h3 id="Float.SetUint64">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L505">SetUint64</a>
    <a href="#Float.SetUint64">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) SetUint64(x <a href="/builtin/#uint64">uint64</a>) *<a href="#Float">Float</a></pre>

SetUint64 sets z to the (possibly rounded) value of x and returns z. If z's
precision is 0, it is changed to 64 (and rounding will have no effect).

<h3 id="Float.Sign">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L222">Sign</a>
    <a href="#Float.Sign">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Sign() <a href="/builtin/#int">int</a></pre>

Sign returns:

    -1 if x <   0
     0 if x is ±0
    +1 if x >   0

<h3 id="Float.Signbit">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L315">Signbit</a>
    <a href="#Float.Signbit">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Signbit() <a href="/builtin/#bool">bool</a></pre>

Signbit returns true if x is negative or negative zero.

<h3 id="Float.Sqrt">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/sqrt.go#L13">Sqrt</a>
    <a href="#Float.Sqrt">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Sqrt(x *<a href="#Float">Float</a>) *<a href="#Float">Float</a></pre>

Sqrt sets z to the rounded square root of x, and returns it.

If z's precision is 0, it is changed to x's precision before the operation.
Rounding is performed according to z's precision and rounding mode.

The function panics if z < 0. The value of z is undefined in that case.

<h3 id="Float.String">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ftoa.go#L43">String</a>
    <a href="#Float.String">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) String() <a href="/builtin/#string">string</a></pre>

String formats x like x.Text('g', 10). (String must be called explicitly,
Float.Format does not support %s verb.)

<h3 id="Float.Sub">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L1495">Sub</a>
    <a href="#Float.Sub">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) Sub(x, y *<a href="#Float">Float</a>) *<a href="#Float">Float</a></pre>

Sub sets z to the rounded difference x-y and returns z. Precision, rounding, and
accuracy reporting are as for Add. Sub panics with ErrNaN if x and y are
infinities with equal signs. The value of z is undefined in that case.

<h3 id="Float.Text">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ftoa.go#L33">Text</a>
    <a href="#Float.Text">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Text(format <a href="/builtin/#byte">byte</a>, prec <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

Text converts the floating-point number x to a string according to the given
format and precision prec. The format is one of:

    'e'	-d.dddde±dd, decimal exponent, at least two (possibly 0) exponent digits
    'E'	-d.ddddE±dd, decimal exponent, at least two (possibly 0) exponent digits
    'f'	-ddddd.dddd, no exponent
    'g'	like 'e' for large exponents, like 'f' otherwise
    'G'	like 'E' for large exponents, like 'f' otherwise
    'b'	-ddddddp±dd, binary exponent
    'p'	-0x.dddp±dd, binary exponent, hexadecimal mantissa

For the binary exponent formats, the mantissa is printed in normalized form:

    'b'	decimal integer mantissa using x.Prec() bits, or -0
    'p'	hexadecimal fraction with 0.5 <= 0.mantissa < 1.0, or -0

If format is a different character, Text returns a "%" followed by the
unrecognized format character.

The precision prec controls the number of digits (excluding the exponent)
printed by the 'e', 'E', 'f', 'g', and 'G' formats. For 'e', 'E', and 'f' it is
the number of digits after the decimal point. For 'g' and 'G' it is the total
number of digits. A negative precision selects the smallest number of decimal
digits necessary to identify the value x uniquely using x.Prec() mantissa bits.
The prec value is ignored for the 'b' or 'p' format.

<h3 id="Float.Uint64">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L714">Uint64</a>
    <a href="#Float.Uint64">¶</a></h3>
<pre>func (x *<a href="#Float">Float</a>) Uint64() (<a href="/builtin/#uint64">uint64</a>, <a href="#Accuracy">Accuracy</a>)</pre>

Uint64 returns the unsigned integer resulting from truncating x towards zero. If
0 <= x <= math.MaxUint64, the result is Exact if x is an integer and Below
otherwise. The result is (0, Above) for x < 0, and (math.MaxUint64, Below) for x
> math.MaxUint64.

<h3 id="Float.UnmarshalText">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/floatmarsh.go#L103">UnmarshalText</a>
    <a href="#Float.UnmarshalText">¶</a></h3>
<pre>func (z *<a href="#Float">Float</a>) UnmarshalText(text []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

UnmarshalText implements the encoding.TextUnmarshaler interface. The result is
rounded per the precision and rounding mode of z. If z's precision is 0, it is
changed to 64 before rounding takes effect.

<h2 id="Int">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L8">Int</a>
    <a href="#Int">¶</a></h2>
<pre>type Int struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

An Int represents a signed multi-precision integer. The zero value for an Int
represents the value 0.

<h3 id="NewInt">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L51">NewInt</a>
    <a href="#NewInt">¶</a></h3>
<pre>func NewInt(x <a href="/builtin/#int64">int64</a>) *<a href="#Int">Int</a></pre>

NewInt allocates and returns a new Int set to x.

<h3 id="Int.Abs">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L85">Abs</a>
    <a href="#Int.Abs">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Abs(x *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Abs sets z to |x| (the absolute value of x) and returns z.

<h3 id="Int.Add">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L99">Add</a>
    <a href="#Int.Add">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Add(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Add sets z to the sum x+y and returns z.

<h3 id="Int.And">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L877">And</a>
    <a href="#Int.And">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) And(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

And sets z = x & y and returns z.

<h3 id="Int.AndNot">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L907">AndNot</a>
    <a href="#Int.AndNot">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) AndNot(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

AndNot sets z = x &^ y and returns z.

<h3 id="Int.Append">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intconv.go#L19">Append</a>
    <a href="#Int.Append">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) Append(buf []<a href="/builtin/#byte">byte</a>, base <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

Append appends the string representation of x, as generated by x.Text(base), to
buf and returns the extended buffer.

<h3 id="Int.Binomial">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L180">Binomial</a>
    <a href="#Int.Binomial">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Binomial(n, k <a href="/builtin/#int64">int64</a>) *<a href="#Int">Int</a></pre>

Binomial sets z to the binomial coefficient of (n, k) and returns z.

<h3 id="Int.Bit">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L837">Bit</a>
    <a href="#Int.Bit">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) Bit(i <a href="/builtin/#int">int</a>) <a href="/builtin/#uint">uint</a></pre>

Bit returns the value of the i'th bit of x. That is, it returns (x>>i)&1. The
bit index i must be >= 0.

<h3 id="Int.BitLen">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L430">BitLen</a>
    <a href="#Int.BitLen">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) BitLen() <a href="/builtin/#int">int</a></pre>

BitLen returns the length of the absolute value of x in bits. The bit length of
0 is 0.

<h3 id="Int.Bits">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L69">Bits</a>
    <a href="#Int.Bits">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) Bits() []<a href="#Word">Word</a></pre>

Bits provides raw (unchecked but fast) access to x by returning its absolute
value as a little-endian Word slice. The result and x share the same underlying
array. Bits is intended to support implementation of missing low-level Int
functionality outside this package; it should be avoided otherwise.

<h3 id="Int.Bytes">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L423">Bytes</a>
    <a href="#Int.Bytes">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) Bytes() []<a href="/builtin/#byte">byte</a></pre>

Bytes returns the absolute value of x as a big-endian byte slice.

<h3 id="Int.Cmp">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L303">Cmp</a>
    <a href="#Int.Cmp">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) Cmp(y *<a href="#Int">Int</a>) (r <a href="/builtin/#int">int</a>)</pre>

Cmp compares x and y and returns:

    -1 if x <  y
     0 if x == y
    +1 if x >  y

<h3 id="Int.CmpAbs">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L328">CmpAbs</a>
    <a href="#Int.CmpAbs">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) CmpAbs(y *<a href="#Int">Int</a>) <a href="/builtin/#int">int</a></pre>

CmpAbs compares the absolute values of x and y and returns:

    -1 if |x| <  |y|
     0 if |x| == |y|
    +1 if |x| >  |y|

<h3 id="Int.Div">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L230">Div</a>
    <a href="#Int.Div">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Div(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Div sets z to the quotient x/y for y != 0 and returns z. If y == 0, a
division-by-zero run-time panic occurs. Div implements Euclidean division
(unlike Go); see DivMod for more details.

<h3 id="Int.DivMod">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L279">DivMod</a>
    <a href="#Int.DivMod">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) DivMod(x, y, m *<a href="#Int">Int</a>) (*<a href="#Int">Int</a>, *<a href="#Int">Int</a>)</pre>

DivMod sets z to the quotient x div y and m to the modulus x mod y and returns
the pair (z, m) for y != 0. If y == 0, a division-by-zero run-time panic occurs.

DivMod implements Euclidean division and modulus (unlike Go):

    q = x div y  such that
    m = x - y*q  with 0 <= m < |y|

(See Raymond T. Boute, ``The Euclidean definition of the functions div and
mod''. ACM Transactions on Programming Languages and Systems (TOPLAS),
14(2):127-144, New York, NY, USA, 4/1992. ACM press.) See QuoRem for T-division
and modulus (like Go).

<h3 id="Int.Exp">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L439">Exp</a>
    <a href="#Int.Exp">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Exp(x, y, m *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Exp sets z = x**y mod |m| (i.e. the sign of m is ignored), and returns z. If y
<= 0, the result is 1 mod |m|; if m == nil or m == 0, z = x**y.

Modular exponentation of inputs of a particular size is not a cryptographically
constant-time operation.

<h3 id="Int.Format">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intconv.go#L53">Format</a>
    <a href="#Int.Format">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) Format(s <a href="/fmt/">fmt</a>.<a href="/fmt/#State">State</a>, ch <a href="/builtin/#rune">rune</a>)</pre>

Format implements fmt.Formatter. It accepts the formats 'b' (binary), 'o'
(octal), 'd' (decimal), 'x' (lowercase hexadecimal), and 'X' (uppercase
hexadecimal). Also supported are the full suite of package fmt's format flags
for integral types, including '+' and ' ' for sign control, '#' for leading zero
in octal and for hexadecimal, a leading "0x" or "0X" for "%#x" and "%#X"
respectively, specification of minimum digits precision, output field width,
space or zero padding, and '-' for left or right justification.

<h3 id="Int.GCD">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L467">GCD</a>
    <a href="#Int.GCD">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) GCD(x, y, a, b *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

GCD sets z to the greatest common divisor of a and b, which both must be > 0,
and returns z. If x or y are not nil, GCD sets their value such that z = a*x +
b*y. If either a or b is <= 0, GCD sets z = x = y = 0.

<h3 id="Int.GobDecode">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intmarsh.go#L23">GobDecode</a>
    <a href="#Int.GobDecode">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) GobDecode(buf []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

GobDecode implements the gob.GobDecoder interface.

<h3 id="Int.GobEncode">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intmarsh.go#L8">GobEncode</a>
    <a href="#Int.GobEncode">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) GobEncode() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

GobEncode implements the gob.GobEncoder interface.

<h3 id="Int.Int64">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L354">Int64</a>
    <a href="#Int.Int64">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) Int64() <a href="/builtin/#int64">int64</a></pre>

Int64 returns the int64 representation of x. If x cannot be represented in an
int64, the result is undefined.

<h3 id="Int.IsInt64">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L369">IsInt64</a>
    <a href="#Int.IsInt64">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) IsInt64() <a href="/builtin/#bool">bool</a></pre>

IsInt64 reports whether x can be represented as an int64.

<h3 id="Int.IsUint64">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L378">IsUint64</a>
    <a href="#Int.IsUint64">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) IsUint64() <a href="/builtin/#bool">bool</a></pre>

IsUint64 reports whether x can be represented as a uint64.

<h3 id="Int.Lsh">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L813">Lsh</a>
    <a href="#Int.Lsh">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Lsh(x *<a href="#Int">Int</a>, n <a href="/builtin/#uint">uint</a>) *<a href="#Int">Int</a></pre>

Lsh sets z = x << n and returns z.

<h3 id="Int.MarshalJSON">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intmarsh.go#L59">MarshalJSON</a>
    <a href="#Int.MarshalJSON">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) MarshalJSON() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

MarshalJSON implements the json.Marshaler interface.

<h3 id="Int.MarshalText">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intmarsh.go#L39">MarshalText</a>
    <a href="#Int.MarshalText">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) MarshalText() (text []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

MarshalText implements the encoding.TextMarshaler interface.

<h3 id="Int.Mod">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L247">Mod</a>
    <a href="#Int.Mod">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Mod(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Mod sets z to the modulus x%y for y != 0 and returns z. If y == 0, a
division-by-zero run-time panic occurs. Mod implements Euclidean modulus (unlike
Go); see DivMod for more details.

<h3 id="Int.ModInverse">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L653">ModInverse</a>
    <a href="#Int.ModInverse">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) ModInverse(g, n *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

ModInverse sets z to the multiplicative inverse of g in the ring ℤ/nℤ and
returns z. If g and n are not relatively prime, the result is undefined.

<h3 id="Int.ModSqrt">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L791">ModSqrt</a>
    <a href="#Int.ModSqrt">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) ModSqrt(x, p *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

ModSqrt sets z to a square root of x mod p if such a square root exists, and
returns z. The modulus p must be an odd prime. If x is not a square mod p,
ModSqrt leaves z unchanged and returns nil. This function panics if p is not an
odd integer.

<h3 id="Int.Mul">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L141">Mul</a>
    <a href="#Int.Mul">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Mul(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Mul sets z to the product x*y and returns z.

<h3 id="Int.MulRange">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L159">MulRange</a>
    <a href="#Int.MulRange">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) MulRange(a, b <a href="/builtin/#int64">int64</a>) *<a href="#Int">Int</a></pre>

MulRange sets z to the product of all integers in the range [a, b] inclusively
and returns z. If a > b (empty range), the result is 1.

<h3 id="Int.Neg">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L92">Neg</a>
    <a href="#Int.Neg">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Neg(x *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Neg sets z to -x and returns z.

<h3 id="Int.Not">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L1000">Not</a>
    <a href="#Int.Not">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Not(x *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Not sets z = ^x and returns z.

<h3 id="Int.Or">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L940">Or</a>
    <a href="#Int.Or">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Or(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Or sets z = x | y and returns z.

<h3 id="Int.ProbablyPrime">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/prime.go#L16">ProbablyPrime</a>
    <a href="#Int.ProbablyPrime">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) ProbablyPrime(n <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>

ProbablyPrime reports whether x is probably prime, applying the Miller-Rabin
test with n pseudorandomly chosen bases as well as a Baillie-PSW test.

If x is prime, ProbablyPrime returns true. If x is chosen randomly and not
prime, ProbablyPrime probably returns false. The probability of returning true
for a randomly chosen non-prime is at most ¼ⁿ.

ProbablyPrime is 100% accurate for inputs less than 2⁶⁴. See Menezes et al.,
Handbook of Applied Cryptography, 1997, pp. 145-149, and FIPS 186-4 Appendix F
for further discussion of the error probabilities.

ProbablyPrime is not suitable for judging primes that an adversary may have
crafted to fool the test.

As of Go 1.8, ProbablyPrime(0) is allowed and applies only a Baillie-PSW test.
Before Go 1.8, ProbablyPrime applied only the Miller-Rabin tests, and
ProbablyPrime(0) panicked.

<h3 id="Int.Quo">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L194">Quo</a>
    <a href="#Int.Quo">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Quo(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Quo sets z to the quotient x/y for y != 0 and returns z. If y == 0, a
division-by-zero run-time panic occurs. Quo implements truncated division (like
Go); see QuoRem for more details.

<h3 id="Int.QuoRem">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L221">QuoRem</a>
    <a href="#Int.QuoRem">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) QuoRem(x, y, r *<a href="#Int">Int</a>) (*<a href="#Int">Int</a>, *<a href="#Int">Int</a>)</pre>

QuoRem sets z to the quotient x/y and r to the remainder x%y and returns the
pair (z, r) for y != 0. If y == 0, a division-by-zero run-time panic occurs.

QuoRem implements T-division and modulus (like Go):

    q = x/y      with the result truncated to zero
    r = x - y*q

(See Daan Leijen, ``Division and Modulus for Computer Scientists''.) See DivMod
for Euclidean division and modulus (unlike Go).

<h3 id="Int.Rand">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L641">Rand</a>
    <a href="#Int.Rand">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Rand(rnd *<a href="/math/rand/">rand</a>.<a href="/math/rand/#Rand">Rand</a>, n *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Rand sets z to a pseudo-random number in [0, n) and returns z.

As this uses the math/rand package, it must not be used for security-sensitive
work. Use crypto/rand.Int instead.

<h3 id="Int.Rem">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L203">Rem</a>
    <a href="#Int.Rem">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Rem(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Rem sets z to the remainder x%y for y != 0 and returns z. If y == 0, a
division-by-zero run-time panic occurs. Rem implements truncated modulus (like
Go); see QuoRem for more details.

<h3 id="Int.Rsh">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L820">Rsh</a>
    <a href="#Int.Rsh">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Rsh(x *<a href="#Int">Int</a>, n <a href="/builtin/#uint">uint</a>) *<a href="#Int">Int</a></pre>

Rsh sets z = x >> n and returns z.

<h3 id="Int.Scan">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intconv.go#L218">Scan</a>
    <a href="#Int.Scan">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Scan(s <a href="/fmt/">fmt</a>.<a href="/fmt/#ScanState">ScanState</a>, ch <a href="/builtin/#rune">rune</a>) <a href="/builtin/#error">error</a></pre>

Scan is a support routine for fmt.Scanner; it sets z to the value of the scanned
number. It accepts the formats 'b' (binary), 'o' (octal), 'd' (decimal), 'x'
(lowercase hexadecimal), and 'X' (uppercase hexadecimal).

<a id="exampleInt_Scan"></a>
Example:

    // The Scan function is rarely used directly;
    // the fmt package recognizes it as an implementation of fmt.Scanner.
    i := new(big.Int)
    _, err := fmt.Sscan("18446744073709551617", i)
    if err != nil {
        log.Println("error scanning value:", err)
    } else {
        fmt.Println(i)
    }
    // Output: 18446744073709551617

<h3 id="Int.Set">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L56">Set</a>
    <a href="#Int.Set">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Set(x *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Set sets z to x and returns z.

<h3 id="Int.SetBit">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L860">SetBit</a>
    <a href="#Int.SetBit">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) SetBit(x *<a href="#Int">Int</a>, i <a href="/builtin/#int">int</a>, b <a href="/builtin/#uint">uint</a>) *<a href="#Int">Int</a></pre>

SetBit sets z to x, with x's i'th bit set to b (0 or 1). That is, if b is 1
SetBit sets z = x | (1 << i); if b is 0 SetBit sets z = x &^ (1 << i). If b is
not 0 or 1, SetBit will panic.

<h3 id="Int.SetBits">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L78">SetBits</a>
    <a href="#Int.SetBits">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) SetBits(abs []<a href="#Word">Word</a>) *<a href="#Int">Int</a></pre>

SetBits provides raw (unchecked but fast) access to z by setting its value to
abs, interpreted as a little-endian Word slice, and returning z. The result and
abs share the same underlying array. SetBits is intended to support
implementation of missing low-level Int functionality outside this package; it
should be avoided otherwise.

<h3 id="Int.SetBytes">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L416">SetBytes</a>
    <a href="#Int.SetBytes">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) SetBytes(buf []<a href="/builtin/#byte">byte</a>) *<a href="#Int">Int</a></pre>

SetBytes interprets buf as the bytes of a big-endian unsigned integer, sets z to
that value, and returns z.

<h3 id="Int.SetInt64">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L32">SetInt64</a>
    <a href="#Int.SetInt64">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) SetInt64(x <a href="/builtin/#int64">int64</a>) *<a href="#Int">Int</a></pre>

SetInt64 sets z to x and returns z.

<h3 id="Int.SetString">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L397">SetString</a>
    <a href="#Int.SetString">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) SetString(s <a href="/builtin/#string">string</a>, base <a href="/builtin/#int">int</a>) (*<a href="#Int">Int</a>, <a href="/builtin/#bool">bool</a>)</pre>

SetString sets z to the value of s, interpreted in the given base, and returns z
and a boolean indicating success. The entire string (not just a prefix) must be
valid for success. If SetString fails, the value of z is undefined but the
returned value is nil.

The base argument must be 0 or a value between 2 and MaxBase. If the base is 0,
the string prefix determines the actual conversion base. A prefix of ``0x'' or
``0X'' selects base 16; the ``0'' prefix selects base 8, and a ``0b'' or ``0B''
prefix selects base 2. Otherwise the selected base is 10.

For bases <= 36, lower and upper case letters are considered the same: The
letters 'a' to 'z' and 'A' to 'Z' represent digit values 10 to 35. For bases >
36, the upper case letters 'A' to 'Z' represent the digit values 36 to 61.

<a id="exampleInt_SetString"></a>
Example:

    i := new(big.Int)
    i.SetString("644", 8) // octal
    fmt.Println(i)
    // Output: 420

<h3 id="Int.SetUint64">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L44">SetUint64</a>
    <a href="#Int.SetUint64">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) SetUint64(x <a href="/builtin/#uint64">uint64</a>) *<a href="#Int">Int</a></pre>

SetUint64 sets z to x and returns z.

<h3 id="Int.Sign">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L21">Sign</a>
    <a href="#Int.Sign">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) Sign() <a href="/builtin/#int">int</a></pre>

Sign returns:

    -1 if x <  0
     0 if x == 0
    +1 if x >  0

<h3 id="Int.Sqrt">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L1016">Sqrt</a>
    <a href="#Int.Sqrt">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Sqrt(x *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Sqrt sets z to ⌊√x⌋, the largest integer such that z² ≤ x, and returns z. It
panics if x is negative.

<h3 id="Int.String">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intconv.go#L26">String</a>
    <a href="#Int.String">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Int.Sub">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L120">Sub</a>
    <a href="#Int.Sub">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Sub(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Sub sets z to the difference x-y and returns z.

<h3 id="Int.Text">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intconv.go#L10">Text</a>
    <a href="#Int.Text">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) Text(base <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

Text returns the string representation of x in the given base. Base must be
between 2 and 62, inclusive. The result uses the lower-case letters 'a' to 'z'
for digit values 10 to 35, and the upper-case letters 'A' to 'Z' for digit
values 36 to 61. No prefix (such as "0x") is added to the string.

<h3 id="Int.Uint64">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L364">Uint64</a>
    <a href="#Int.Uint64">¶</a></h3>
<pre>func (x *<a href="#Int">Int</a>) Uint64() <a href="/builtin/#uint64">uint64</a></pre>

Uint64 returns the uint64 representation of x. If x cannot be represented in a
uint64, the result is undefined.

<h3 id="Int.UnmarshalJSON">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intmarsh.go#L64">UnmarshalJSON</a>
    <a href="#Int.UnmarshalJSON">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) UnmarshalJSON(text []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

UnmarshalJSON implements the json.Unmarshaler interface.

<h3 id="Int.UnmarshalText">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/intmarsh.go#L47">UnmarshalText</a>
    <a href="#Int.UnmarshalText">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) UnmarshalText(text []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

UnmarshalText implements the encoding.TextUnmarshaler interface.

<h3 id="Int.Xor">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/int.go#L970">Xor</a>
    <a href="#Int.Xor">¶</a></h3>
<pre>func (z *<a href="#Int">Int</a>) Xor(x, y *<a href="#Int">Int</a>) *<a href="#Int">Int</a></pre>

Xor sets z = x ^ y and returns z.

<h2 id="Rat">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L6">Rat</a>
    <a href="#Rat">¶</a></h2>
<pre>type Rat struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Rat represents a quotient a/b of arbitrary precision. The zero value for a Rat
represents the value 0.

<h3 id="NewRat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L14">NewRat</a>
    <a href="#NewRat">¶</a></h3>
<pre>func NewRat(a, b <a href="/builtin/#int64">int64</a>) *<a href="#Rat">Rat</a></pre>

NewRat creates a new Rat with numerator a and denominator b.

<h3 id="Rat.Abs">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L335">Abs</a>
    <a href="#Rat.Abs">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) Abs(x *<a href="#Rat">Rat</a>) *<a href="#Rat">Rat</a></pre>

Abs sets z to |x| (the absolute value of x) and returns z.

<h3 id="Rat.Add">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L464">Add</a>
    <a href="#Rat.Add">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) Add(x, y *<a href="#Rat">Rat</a>) *<a href="#Rat">Rat</a></pre>

Add sets z to the sum x+y and returns z.

<h3 id="Rat.Cmp">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L459">Cmp</a>
    <a href="#Rat.Cmp">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) Cmp(y *<a href="#Rat">Rat</a>) <a href="/builtin/#int">int</a></pre>

Cmp compares x and y and returns:

    -1 if x <  y
     0 if x == y
    +1 if x >  y

<h3 id="Rat.Denom">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L392">Denom</a>
    <a href="#Rat.Denom">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) Denom() *<a href="#Int">Int</a></pre>

Denom returns the denominator of x; it is always > 0. The result is a reference
to x's denominator; it may change if a new value is assigned to x, and vice
versa.

<h3 id="Rat.Float32">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L254">Float32</a>
    <a href="#Rat.Float32">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) Float32() (f <a href="/builtin/#float32">float32</a>, exact <a href="/builtin/#bool">bool</a>)</pre>

Float32 returns the nearest float32 value for x and a bool indicating whether f
represents x exactly. If the magnitude of x is too large to be represented by a
float32, f is an infinity and exact is false. The sign of f always matches the
sign of x, even if f == 0.

<h3 id="Rat.Float64">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L270">Float64</a>
    <a href="#Rat.Float64">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) Float64() (f <a href="/builtin/#float64">float64</a>, exact <a href="/builtin/#bool">bool</a>)</pre>

Float64 returns the nearest float64 value for x and a bool indicating whether f
represents x exactly. If the magnitude of x is too large to be represented by a
float64, f is an infinity and exact is false. The sign of f always matches the
sign of x, even if f == 0.

<h3 id="Rat.FloatString">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratconv.go#L223">FloatString</a>
    <a href="#Rat.FloatString">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) FloatString(prec <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

FloatString returns a string representation of x in decimal form with prec
digits of precision after the decimal point. The last digit is rounded to
nearest, with halves rounded away from zero.

<h3 id="Rat.GobDecode">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratmarsh.go#L32">GobDecode</a>
    <a href="#Rat.GobDecode">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) GobDecode(buf []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

GobDecode implements the gob.GobDecoder interface.

<h3 id="Rat.GobEncode">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratmarsh.go#L9">GobEncode</a>
    <a href="#Rat.GobEncode">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) GobEncode() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

GobEncode implements the gob.GobEncoder interface.

<h3 id="Rat.Inv">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L349">Inv</a>
    <a href="#Rat.Inv">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) Inv(x *<a href="#Rat">Rat</a>) *<a href="#Rat">Rat</a></pre>

Inv sets z to 1/x and returns z.

<h3 id="Rat.IsInt">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L377">IsInt</a>
    <a href="#Rat.IsInt">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) IsInt() <a href="/builtin/#bool">bool</a></pre>

IsInt reports whether the denominator of x is 1.

<h3 id="Rat.MarshalText">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratmarsh.go#L51">MarshalText</a>
    <a href="#Rat.MarshalText">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) MarshalText() (text []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

MarshalText implements the encoding.TextMarshaler interface.

<h3 id="Rat.Mul">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L482">Mul</a>
    <a href="#Rat.Mul">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) Mul(x, y *<a href="#Rat">Rat</a>) *<a href="#Rat">Rat</a></pre>

Mul sets z to the product x*y and returns z.

<h3 id="Rat.Neg">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L342">Neg</a>
    <a href="#Rat.Neg">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) Neg(x *<a href="#Rat">Rat</a>) *<a href="#Rat">Rat</a></pre>

Neg sets z to -x and returns z.

<h3 id="Rat.Num">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L385">Num</a>
    <a href="#Rat.Num">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) Num() *<a href="#Int">Int</a></pre>

Num returns the numerator of x; it may be <= 0. The result is a reference to x's
numerator; it may change if a new value is assigned to x, and vice versa. The
sign of the numerator corresponds to the sign of x.

<h3 id="Rat.Quo">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L497">Quo</a>
    <a href="#Rat.Quo">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) Quo(x, y *<a href="#Rat">Rat</a>) *<a href="#Rat">Rat</a></pre>

Quo sets z to the quotient x/y and returns z. If y == 0, a division-by-zero
run-time panic occurs.

<h3 id="Rat.RatString">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratconv.go#L213">RatString</a>
    <a href="#Rat.RatString">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) RatString() <a href="/builtin/#string">string</a></pre>

RatString returns a string representation of x in the form "a/b" if b != 1, and
in the form "a" if b == 1.

<h3 id="Rat.Scan">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratconv.go#L16">Scan</a>
    <a href="#Rat.Scan">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) Scan(s <a href="/fmt/">fmt</a>.<a href="/fmt/#ScanState">ScanState</a>, ch <a href="/builtin/#rune">rune</a>) <a href="/builtin/#error">error</a></pre>

Scan is a support routine for fmt.Scanner. It accepts the formats 'e', 'E', 'f',
'F', 'g', 'G', and 'v'. All formats are equivalent.

<a id="exampleRat_Scan"></a>
Example:

    // The Scan function is rarely used directly;
    // the fmt package recognizes it as an implementation of fmt.Scanner.
    r := new(big.Rat)
    _, err := fmt.Sscan("1.5000", r)
    if err != nil {
        log.Println("error scanning value:", err)
    } else {
        fmt.Println(r)
    }
    // Output: 3/2

<h3 id="Rat.Set">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L326">Set</a>
    <a href="#Rat.Set">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) Set(x *<a href="#Rat">Rat</a>) *<a href="#Rat">Rat</a></pre>

Set sets z to x (by making a copy of x) and returns z.

<h3 id="Rat.SetFloat64">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L20">SetFloat64</a>
    <a href="#Rat.SetFloat64">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) SetFloat64(f <a href="/builtin/#float64">float64</a>) *<a href="#Rat">Rat</a></pre>

SetFloat64 sets z to exactly f and returns z. If f is not finite, SetFloat
returns nil.

<h3 id="Rat.SetFrac">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L283">SetFrac</a>
    <a href="#Rat.SetFrac">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) SetFrac(a, b *<a href="#Int">Int</a>) *<a href="#Rat">Rat</a></pre>

SetFrac sets z to a/b and returns z.

<h3 id="Rat.SetFrac64">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L298">SetFrac64</a>
    <a href="#Rat.SetFrac64">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) SetFrac64(a, b <a href="/builtin/#int64">int64</a>) *<a href="#Rat">Rat</a></pre>

SetFrac64 sets z to a/b and returns z.

<h3 id="Rat.SetInt">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L312">SetInt</a>
    <a href="#Rat.SetInt">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) SetInt(x *<a href="#Int">Int</a>) *<a href="#Rat">Rat</a></pre>

SetInt sets z to x (by making a copy of x) and returns z.

<h3 id="Rat.SetInt64">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L319">SetInt64</a>
    <a href="#Rat.SetInt64">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) SetInt64(x <a href="/builtin/#int64">int64</a>) *<a href="#Rat">Rat</a></pre>

SetInt64 sets z to x and returns z.

<h3 id="Rat.SetString">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratconv.go#L35">SetString</a>
    <a href="#Rat.SetString">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) SetString(s <a href="/builtin/#string">string</a>) (*<a href="#Rat">Rat</a>, <a href="/builtin/#bool">bool</a>)</pre>

SetString sets z to the value of s and returns z and a boolean indicating
success. s can be given as a fraction "a/b" or as a floating-point number
optionally followed by an exponent. The entire string (not just a prefix) must
be valid for success. If the operation failed, the value of z is undefined but
the returned value is nil.

<a id="exampleRat_SetString"></a>
Example:

    r := new(big.Rat)
    r.SetString("355/113")
    fmt.Println(r.FloatString(3))
    // Output: 3.142

<h3 id="Rat.Sign">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L372">Sign</a>
    <a href="#Rat.Sign">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) Sign() <a href="/builtin/#int">int</a></pre>

Sign returns:

    -1 if x <  0
     0 if x == 0
    +1 if x >  0

<h3 id="Rat.String">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratconv.go#L194">String</a>
    <a href="#Rat.String">¶</a></h3>
<pre>func (x *<a href="#Rat">Rat</a>) String() <a href="/builtin/#string">string</a></pre>

String returns a string representation of x in the form "a/b" (even if b == 1).

<h3 id="Rat.Sub">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/rat.go#L473">Sub</a>
    <a href="#Rat.Sub">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) Sub(x, y *<a href="#Rat">Rat</a>) *<a href="#Rat">Rat</a></pre>

Sub sets z to the difference x-y and returns z.

<h3 id="Rat.UnmarshalText">func (*Rat) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/ratmarsh.go#L59">UnmarshalText</a>
    <a href="#Rat.UnmarshalText">¶</a></h3>
<pre>func (z *<a href="#Rat">Rat</a>) UnmarshalText(text []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

UnmarshalText implements the encoding.TextUnmarshaler interface.

<h2 id="RoundingMode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L116">RoundingMode</a>
    <a href="#RoundingMode">¶</a></h2>
<pre>type RoundingMode <a href="/builtin/#byte">byte</a></pre>

RoundingMode determines how a Float value is rounded to the desired precision.
Rounding may change the Float value; the rounding error is described by the
Float's Accuracy.

<pre>const (
    <span id="ToNearestEven">ToNearestEven</span> <a href="#RoundingMode">RoundingMode</a> = <a href="/builtin/#iota">iota</a> <span class="comment">// == IEEE 754-2008 roundTiesToEven</span>
    <span id="ToNearestAway">ToNearestAway</span>                     <span class="comment">// == IEEE 754-2008 roundTiesToAway</span>
    <span id="ToZero">ToZero</span>                            <span class="comment">// == IEEE 754-2008 roundTowardZero</span>
    <span id="AwayFromZero">AwayFromZero</span>                      <span class="comment">// no IEEE 754-2008 equivalent</span>
    <span id="ToNegativeInf">ToNegativeInf</span>                     <span class="comment">// == IEEE 754-2008 roundTowardNegative</span>
    <span id="ToPositiveInf">ToPositiveInf</span>                     <span class="comment">// == IEEE 754-2008 roundTowardPositive</span>
)</pre>

These constants define supported rounding modes.

<a id="exampleRoundingMode"></a>
Example:

    operands := []float64{2.6, 2.5, 2.1, -2.1, -2.5, -2.6}

    fmt.Print("   x")
    for mode := big.ToNearestEven; mode <= big.ToPositiveInf; mode++ {
        fmt.Printf("  %s", mode)
    }
    fmt.Println()

    for _, f64 := range operands {
        fmt.Printf("%4g", f64)
        for mode := big.ToNearestEven; mode <= big.ToPositiveInf; mode++ {
            // sample operands above require 2 bits to represent mantissa
            // set binary precision to 2 to round them to integer values
            f := new(big.Float).SetPrec(2).SetMode(mode).SetFloat64(f64)
            fmt.Printf("  %*g", len(mode.String()), f)
        }
        fmt.Println()
    }

    // Output:
    //    x  ToNearestEven  ToNearestAway  ToZero  AwayFromZero  ToNegativeInf  ToPositiveInf
    //  2.6              3              3       2             3              2              3
    //  2.5              2              3       2             3              2              3
    //  2.1              2              2       2             3              2              3
    // -2.1             -2             -2      -2            -3             -3             -2
    // -2.5             -2             -3      -2            -3             -3             -2
    // -2.6             -3             -3      -2            -3             -3             -2

<h3 id="RoundingMode.String">func (RoundingMode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/roundingmode_string.go#L1">String</a>
    <a href="#RoundingMode.String">¶</a></h3>
<pre>func (i <a href="#RoundingMode">RoundingMode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Word">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/big/arith.go#L4">Word</a>
    <a href="#Word">¶</a></h2>
<pre>type Word <a href="/builtin/#uint">uint</a></pre>

A Word represents a single digit of a multi-precision unsigned integer.

<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/release-branch.go1.10/src/math/big/float.go#L1423)  When rounding ToNegativeInf, the sign of Float values rounded to 0 is
  incorrect.


