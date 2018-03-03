version: 1.10
## package crypto

  `import "crypto"`

## Overview

Package crypto collects common cryptographic constants.

## Index

- [func RegisterHash(h Hash, f func() hash.Hash)](#RegisterHash)
- [type Decrypter](#Decrypter)
- [type DecrypterOpts](#DecrypterOpts)
- [type Hash](#Hash)
  - [func (h Hash) Available() bool](#Hash.Available)
  - [func (h Hash) HashFunc() Hash](#Hash.HashFunc)
  - [func (h Hash) New() hash.Hash](#Hash.New)
  - [func (h Hash) Size() int](#Hash.Size)
- [type PrivateKey](#PrivateKey)
- [type PublicKey](#PublicKey)
- [type Signer](#Signer)
- [type SignerOpts](#SignerOpts)

### Package files
 [crypto.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go)

<h2 id="RegisterHash">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L90">RegisterHash</a>
    <a href="#RegisterHash">¶</a></h2>
<pre>func RegisterHash(h <a href="#Hash">Hash</a>, f func() <a href="/hash/">hash</a>.<a href="/hash/#Hash">Hash</a>)</pre>

RegisterHash registers a function that returns a new instance of the given hash
function. This is intended to be called from the init function in packages that
implement hash functions.

<h2 id="Decrypter">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L137">Decrypter</a>
    <a href="#Decrypter">¶</a></h2>
<pre>type Decrypter interface {
    <span class="comment">// Public returns the public key corresponding to the opaque,</span>
    <span class="comment">// private key.</span>
    Public() <a href="#PublicKey">PublicKey</a>

    <span class="comment">// Decrypt decrypts msg. The opts argument should be appropriate for</span>
    <span class="comment">// the primitive used. See the documentation in each implementation for</span>
    <span class="comment">// details.</span>
    Decrypt(rand <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, msg []<a href="/builtin/#byte">byte</a>, opts <a href="#DecrypterOpts">DecrypterOpts</a>) (plaintext []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)
}</pre>

Decrypter is an interface for an opaque private key that can be used for
asymmetric decryption operations. An example would be an RSA key kept in a
hardware module.

<h2 id="DecrypterOpts">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L148">DecrypterOpts</a>
    <a href="#DecrypterOpts">¶</a></h2>
<pre>type DecrypterOpts interface{}</pre>


<h2 id="Hash">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L6">Hash</a>
    <a href="#Hash">¶</a></h2>
<pre>type Hash <a href="/builtin/#uint">uint</a></pre>

Hash identifies a cryptographic hash function that is implemented in another
package.

<pre>const (
    <span id="MD4">MD4</span>         <a href="#Hash">Hash</a> = 1 + <a href="/builtin/#iota">iota</a> <span class="comment">// import golang.org/x/crypto/md4</span>
    <span id="MD5">MD5</span>                         <span class="comment">// import crypto/md5</span>
    <span id="SHA1">SHA1</span>                        <span class="comment">// import crypto/sha1</span>
    <span id="SHA224">SHA224</span>                      <span class="comment">// import crypto/sha256</span>
    <span id="SHA256">SHA256</span>                      <span class="comment">// import crypto/sha256</span>
    <span id="SHA384">SHA384</span>                      <span class="comment">// import crypto/sha512</span>
    <span id="SHA512">SHA512</span>                      <span class="comment">// import crypto/sha512</span>
    <span id="MD5SHA1">MD5SHA1</span>                     <span class="comment">// no implementation; MD5+SHA1 used for TLS RSA</span>
    <span id="RIPEMD160">RIPEMD160</span>                   <span class="comment">// import golang.org/x/crypto/ripemd160</span>
    <span id="SHA3_224">SHA3_224</span>                    <span class="comment">// import golang.org/x/crypto/sha3</span>
    <span id="SHA3_256">SHA3_256</span>                    <span class="comment">// import golang.org/x/crypto/sha3</span>
    <span id="SHA3_384">SHA3_384</span>                    <span class="comment">// import golang.org/x/crypto/sha3</span>
    <span id="SHA3_512">SHA3_512</span>                    <span class="comment">// import golang.org/x/crypto/sha3</span>
    <span id="SHA512_224">SHA512_224</span>                  <span class="comment">// import crypto/sha512</span>
    <span id="SHA512_256">SHA512_256</span>                  <span class="comment">// import crypto/sha512</span>
    <span id="BLAKE2s_256">BLAKE2s_256</span>                 <span class="comment">// import golang.org/x/crypto/blake2s</span>
    <span id="BLAKE2b_256">BLAKE2b_256</span>                 <span class="comment">// import golang.org/x/crypto/blake2b</span>
    <span id="BLAKE2b_384">BLAKE2b_384</span>                 <span class="comment">// import golang.org/x/crypto/blake2b</span>
    <span id="BLAKE2b_512">BLAKE2b_512</span>                 <span class="comment">// import golang.org/x/crypto/blake2b</span>

)</pre>


<h3 id="Hash.Available">func (Hash) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L83">Available</a>
    <a href="#Hash.Available">¶</a></h3>
<pre>func (h <a href="#Hash">Hash</a>) Available() <a href="/builtin/#bool">bool</a></pre>

Available reports whether the given hash function is linked into the binary.

<h3 id="Hash.HashFunc">func (Hash) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L9">HashFunc</a>
    <a href="#Hash.HashFunc">¶</a></h3>
<pre>func (h <a href="#Hash">Hash</a>) HashFunc() <a href="#Hash">Hash</a></pre>

HashFunc simply returns the value of h so that Hash implements SignerOpts.

<h3 id="Hash.New">func (Hash) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L72">New</a>
    <a href="#Hash.New">¶</a></h3>
<pre>func (h <a href="#Hash">Hash</a>) New() <a href="/hash/">hash</a>.<a href="/hash/#Hash">Hash</a></pre>

New returns a new hash.Hash calculating the given hash function. New panics if
the hash function is not linked into the binary.

<h3 id="Hash.Size">func (Hash) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L61">Size</a>
    <a href="#Hash.Size">¶</a></h3>
<pre>func (h <a href="#Hash">Hash</a>) Size() <a href="/builtin/#int">int</a></pre>

Size returns the length, in bytes, of a digest resulting from the given hash
function. It doesn't require that the hash function in question be linked into
the program.

<h2 id="PrivateKey">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L101">PrivateKey</a>
    <a href="#PrivateKey">¶</a></h2>
<pre>type PrivateKey interface{}</pre>

PrivateKey represents a private key using an unspecified algorithm.

<h2 id="PublicKey">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L98">PublicKey</a>
    <a href="#PublicKey">¶</a></h2>
<pre>type PublicKey interface{}</pre>

PublicKey represents a public key using an unspecified algorithm.

<h2 id="Signer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L105">Signer</a>
    <a href="#Signer">¶</a></h2>
<pre>type Signer interface {
    <span class="comment">// Public returns the public key corresponding to the opaque,</span>
    <span class="comment">// private key.</span>
    Public() <a href="#PublicKey">PublicKey</a>

    <span class="comment">// Sign signs digest with the private key, possibly using entropy from</span>
    <span class="comment">// rand. For an RSA key, the resulting signature should be either a</span>
    <span class="comment">// PKCS#1 v1.5 or PSS signature (as indicated by opts). For an (EC)DSA</span>
    <span class="comment">// key, it should be a DER-serialised, ASN.1 signature structure.</span>
    <span class="comment">//</span>
    <span class="comment">// Hash implements the SignerOpts interface and, in most cases, one can</span>
    <span class="comment">// simply pass in the hash function used as opts. Sign may also attempt</span>
    <span class="comment">// to type assert opts to other types in order to obtain algorithm</span>
    <span class="comment">// specific values. See the documentation in each package for details.</span>
    <span class="comment">//</span>
    <span class="comment">// Note that when a signature of a hash of a larger message is needed,</span>
    <span class="comment">// the caller is responsible for hashing the larger message and passing</span>
    <span class="comment">// the hash (as digest) and the hash function (as opts) to Sign.</span>
    Sign(rand <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, digest []<a href="/builtin/#byte">byte</a>, opts <a href="#SignerOpts">SignerOpts</a>) (signature []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)
}</pre>

Signer is an interface for an opaque private key that can be used for signing
operations. For example, an RSA key kept in a hardware module.

<h2 id="SignerOpts">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/crypto.go#L127">SignerOpts</a>
    <a href="#SignerOpts">¶</a></h2>
<pre>type SignerOpts interface {
    <span class="comment">// HashFunc returns an identifier for the hash function used to produce</span>
    <span class="comment">// the message passed to Signer.Sign, or else zero to indicate that no</span>
    <span class="comment">// hashing was done.</span>
    HashFunc() <a href="#Hash">Hash</a>
}</pre>

SignerOpts contains options for signing with a Signer.

## Subdirectories
- [..](..)
- [aes](aes/)
- [cipher](cipher/)
- [des](des/)
- [dsa](dsa/)
- [ecdsa](ecdsa/)
- [elliptic](elliptic/)
- [hmac](hmac/)
- [md5](md5/)
- [rand](rand/)
- [rc4](rc4/)
- [rsa](rsa/)
- [sha1](sha1/)
- [sha256](sha256/)
- [sha512](sha512/)
- [subtle](subtle/)
- [tls](tls/)
- [x509](x509/)
