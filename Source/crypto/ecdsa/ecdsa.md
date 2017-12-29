version: 1.9.2
## package ecdsa

  `import "crypto/ecdsa"`

## Overview

Package ecdsa implements the Elliptic Curve Digital Signature Algorithm, as
defined in FIPS 186-3.

This implementation derives the nonce from an AES-CTR CSPRNG keyed by
ChopMD(256, SHA2-512(priv.D || entropy || hash)). The CSPRNG key is IRO by a
result of Coron; the AES-CTR stream is IRO under standard assumptions.

## Index

- [func Sign(rand io.Reader, priv *PrivateKey, hash []byte) (r, s *big.Int, err error)](#Sign)
- [func Verify(pub *PublicKey, hash []byte, r, s *big.Int) bool](#Verify)
- [type PrivateKey](#PrivateKey)
  - [func GenerateKey(c elliptic.Curve, rand io.Reader) (*PrivateKey, error)](#GenerateKey)
  - [func (priv *PrivateKey) Public() crypto.PublicKey](#PrivateKey.Public)
  - [func (priv *PrivateKey) Sign(rand io.Reader, msg []byte, opts crypto.SignerOpts) ([]byte, error)](#PrivateKey.Sign)
- [type PublicKey](#PublicKey)

### Package files
 [ecdsa.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/ecdsa/ecdsa.go)

<h2 id="Sign">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/ecdsa/ecdsa.go#L141">Sign</a>
    <a href="#Sign">¶</a></h2>
<pre>func Sign(rand <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, priv *<a href="#PrivateKey">PrivateKey</a>, hash []<a href="/builtin/#byte">byte</a>) (r, s *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>, err <a href="/builtin/#error">error</a>)</pre>

Sign signs a hash (which should be the result of hashing a larger message) using
the private key, priv. If the hash is longer than the bit-length of the private
key's curve order, the hash will be truncated to that length. It returns the
signature as a pair of integers. The security of the private key depends on the
entropy of rand.

<h2 id="Verify">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/ecdsa/ecdsa.go#L217">Verify</a>
    <a href="#Verify">¶</a></h2>
<pre>func Verify(pub *<a href="#PublicKey">PublicKey</a>, hash []<a href="/builtin/#byte">byte</a>, r, s *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>) <a href="/builtin/#bool">bool</a></pre>

Verify verifies the signature in r, s of hash using the public key, pub. Its
return value records whether the signature is valid.

<h2 id="PrivateKey">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/ecdsa/ecdsa.go#L43">PrivateKey</a>
    <a href="#PrivateKey">¶</a></h2>
<pre>type PrivateKey struct {
    <a href="#PublicKey">PublicKey</a>
<span id="PrivateKey.D"></span>    D *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>
}</pre>

PrivateKey represents a ECDSA private key.

<h3 id="GenerateKey">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/ecdsa/ecdsa.go#L90">GenerateKey</a>
    <a href="#GenerateKey">¶</a></h3>
<pre>func GenerateKey(c <a href="/crypto/elliptic/">elliptic</a>.<a href="/crypto/elliptic/#Curve">Curve</a>, rand <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (*<a href="#PrivateKey">PrivateKey</a>, <a href="/builtin/#error">error</a>)</pre>

GenerateKey generates a public and private key pair.

<h3 id="PrivateKey.Public">func (*PrivateKey) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/ecdsa/ecdsa.go#L53">Public</a>
    <a href="#PrivateKey.Public">¶</a></h3>
<pre>func (priv *<a href="#PrivateKey">PrivateKey</a>) Public() <a href="/crypto/">crypto</a>.<a href="/crypto/#PublicKey">PublicKey</a></pre>

Public returns the public key corresponding to priv.

<h3 id="PrivateKey.Sign">func (*PrivateKey) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/ecdsa/ecdsa.go#L61">Sign</a>
    <a href="#PrivateKey.Sign">¶</a></h3>
<pre>func (priv *<a href="#PrivateKey">PrivateKey</a>) Sign(rand <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, msg []<a href="/builtin/#byte">byte</a>, opts <a href="/crypto/">crypto</a>.<a href="/crypto/#SignerOpts">SignerOpts</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Sign signs msg with priv, reading randomness from rand. This method is intended
to support keys where the private part is kept in, for example, a hardware
module. Common uses should use the Sign function in this package directly.

<h2 id="PublicKey">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/ecdsa/ecdsa.go#L37">PublicKey</a>
    <a href="#PublicKey">¶</a></h2>
<pre>type PublicKey struct {
    <a href="/crypto/elliptic/">elliptic</a>.<a href="/crypto/elliptic/#Curve">Curve</a>
<span id="PublicKey.X"></span>    X, Y *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>
}</pre>

PublicKey represents an ECDSA public key.


