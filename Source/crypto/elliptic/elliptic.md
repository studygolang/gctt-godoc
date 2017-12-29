version: 1.9.2
## package elliptic

  `import "crypto/elliptic"`

## Overview

Package elliptic implements several standard elliptic curves over prime fields.

## Index

- [func GenerateKey(curve Curve, rand io.Reader) (priv []byte, x, y *big.Int, err error)](#GenerateKey)
- [func Marshal(curve Curve, x, y *big.Int) []byte](#Marshal)
- [func Unmarshal(curve Curve, data []byte) (x, y *big.Int)](#Unmarshal)
- [type Curve](#Curve)
  - [func P224() Curve](#P224)
  - [func P256() Curve](#P256)
  - [func P384() Curve](#P384)
  - [func P521() Curve](#P521)
- [type CurveParams](#CurveParams)
  - [func (curve *CurveParams) Add(x1, y1, x2, y2 *big.Int) (*big.Int, *big.Int)](#CurveParams.Add)
  - [func (curve *CurveParams) Double(x1, y1 *big.Int) (*big.Int, *big.Int)](#CurveParams.Double)
  - [func (curve *CurveParams) IsOnCurve(x, y *big.Int) bool](#CurveParams.IsOnCurve)
  - [func (curve *CurveParams) Params() *CurveParams](#CurveParams.Params)
  - [func (curve *CurveParams) ScalarBaseMult(k []byte) (*big.Int, *big.Int)](#CurveParams.ScalarBaseMult)
  - [func (curve *CurveParams) ScalarMult(Bx, By *big.Int, k []byte) (*big.Int, *big.Int)](#CurveParams.ScalarMult)

### Package files
 [elliptic.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go) [p224.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/p224.go) [p256_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/p256_amd64.go)

<h2 id="GenerateKey">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L266">GenerateKey</a>
    <a href="#GenerateKey">¶</a></h2>
<pre>func GenerateKey(curve <a href="#Curve">Curve</a>, rand <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (priv []<a href="/builtin/#byte">byte</a>, x, y *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>, err <a href="/builtin/#error">error</a>)</pre>

GenerateKey returns a public/private key pair. The private key is generated
using the given reader, which must return random data.

<h2 id="Marshal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L295">Marshal</a>
    <a href="#Marshal">¶</a></h2>
<pre>func Marshal(curve <a href="#Curve">Curve</a>, x, y *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>) []<a href="/builtin/#byte">byte</a></pre>

Marshal converts a point into the form specified in section 4.3.6 of ANSI X9.62.

<h2 id="Unmarshal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L310">Unmarshal</a>
    <a href="#Unmarshal">¶</a></h2>
<pre>func Unmarshal(curve <a href="#Curve">Curve</a>, data []<a href="/builtin/#byte">byte</a>) (x, y *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>)</pre>

Unmarshal converts a point, serialized by Marshal, into an x, y pair. It is an
error if the point is not on the curve. On error, x = nil.

<h2 id="Curve">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L14">Curve</a>
    <a href="#Curve">¶</a></h2>
<pre>type Curve interface {
    <span class="comment">// Params returns the parameters for the curve.</span>
    Params() *<a href="#CurveParams">CurveParams</a>
    <span class="comment">// IsOnCurve reports whether the given (x,y) lies on the curve.</span>
    IsOnCurve(x, y *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>) <a href="/builtin/#bool">bool</a>
    <span class="comment">// Add returns the sum of (x1,y1) and (x2,y2)</span>
    Add(x1, y1, x2, y2 *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>) (x, y *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>)
    <span class="comment">// Double returns 2*(x,y)</span>
    Double(x1, y1 *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>) (x, y *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>)
    <span class="comment">// ScalarMult returns k*(Bx,By) where k is a number in big-endian form.</span>
    ScalarMult(x1, y1 *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>, k []<a href="/builtin/#byte">byte</a>) (x, y *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>)
    <span class="comment">// ScalarBaseMult returns k*G, where G is the base point of the group</span>
    <span class="comment">// and k is an integer in big-endian form.</span>
    ScalarBaseMult(k []<a href="/builtin/#byte">byte</a>) (x, y *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>)
}</pre>

A Curve represents a short-form Weierstrass curve with a=-3. See
http://www.hyperelliptic.org/EFD/g1p/auto-shortw.html

<h3 id="P224">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/p224.go#L31">P224</a>
    <a href="#P224">¶</a></h3>
<pre>func P224() <a href="#Curve">Curve</a></pre>

P224 returns a Curve which implements P-224 (see FIPS 186-3, section D.2.2).

The cryptographic operations are implemented using constant-time algorithms.

<h3 id="P256">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L362">P256</a>
    <a href="#P256">¶</a></h3>
<pre>func P256() <a href="#Curve">Curve</a></pre>

P256 returns a Curve which implements P-256 (see FIPS 186-3, section D.2.3)

The cryptographic operations are implemented using constant-time algorithms.

<h3 id="P384">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L370">P384</a>
    <a href="#P384">¶</a></h3>
<pre>func P384() <a href="#Curve">Curve</a></pre>

P384 returns a Curve which implements P-384 (see FIPS 186-3, section D.2.4)

The cryptographic operations do not use constant-time algorithms.

<h3 id="P521">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L378">P521</a>
    <a href="#P521">¶</a></h3>
<pre>func P521() <a href="#Curve">Curve</a></pre>

P521 returns a Curve which implements P-521 (see FIPS 186-3, section D.2.5)

The cryptographic operations do not use constant-time algorithms.

<h2 id="CurveParams">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L32">CurveParams</a>
    <a href="#CurveParams">¶</a></h2>
<pre>type CurveParams struct {
<span id="CurveParams.P"></span>    P       *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a> <span class="comment">// the order of the underlying field</span>
<span id="CurveParams.N"></span>    N       *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a> <span class="comment">// the order of the base point</span>
<span id="CurveParams.B"></span>    B       *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a> <span class="comment">// the constant of the curve equation</span>
<span id="CurveParams.Gx"></span>    Gx, Gy  *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a> <span class="comment">// (x,y) of the base point</span>
<span id="CurveParams.BitSize"></span>    BitSize <a href="/builtin/#int">int</a>      <span class="comment">// the size of the underlying field</span>
<span id="CurveParams.Name"></span>    Name    <a href="/builtin/#string">string</a>   <span class="comment">// the canonical name of the curve</span>
}</pre>

CurveParams contains the parameters of an elliptic curve and also provides a
generic, non-constant time implementation of Curve.

<h3 id="CurveParams.Add">func (*CurveParams) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L92">Add</a>
    <a href="#CurveParams.Add">¶</a></h3>
<pre>func (curve *<a href="#CurveParams">CurveParams</a>) Add(x1, y1, x2, y2 *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>) (*<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>, *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>)</pre>


<h3 id="CurveParams.Double">func (*CurveParams) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L176">Double</a>
    <a href="#CurveParams.Double">¶</a></h3>
<pre>func (curve *<a href="#CurveParams">CurveParams</a>) Double(x1, y1 *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>) (*<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>, *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>)</pre>


<h3 id="CurveParams.IsOnCurve">func (*CurveParams) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L45">IsOnCurve</a>
    <a href="#CurveParams.IsOnCurve">¶</a></h3>
<pre>func (curve *<a href="#CurveParams">CurveParams</a>) IsOnCurve(x, y *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>) <a href="/builtin/#bool">bool</a></pre>


<h3 id="CurveParams.Params">func (*CurveParams) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L41">Params</a>
    <a href="#CurveParams.Params">¶</a></h3>
<pre>func (curve *<a href="#CurveParams">CurveParams</a>) Params() *<a href="#CurveParams">CurveParams</a></pre>


<h3 id="CurveParams.ScalarBaseMult">func (*CurveParams) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L258">ScalarBaseMult</a>
    <a href="#CurveParams.ScalarBaseMult">¶</a></h3>
<pre>func (curve *<a href="#CurveParams">CurveParams</a>) ScalarBaseMult(k []<a href="/builtin/#byte">byte</a>) (*<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>, *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>)</pre>


<h3 id="CurveParams.ScalarMult">func (*CurveParams) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/elliptic/elliptic.go#L241">ScalarMult</a>
    <a href="#CurveParams.ScalarMult">¶</a></h3>
<pre>func (curve *<a href="#CurveParams">CurveParams</a>) ScalarMult(Bx, By *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>, k []<a href="/builtin/#byte">byte</a>) (*<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>, *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>)</pre>



