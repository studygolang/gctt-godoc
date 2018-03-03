version: 1.10
## package aes

  `import "crypto/aes"`

## Overview

Package aes implements AES encryption (formerly Rijndael), as defined in U.S.
Federal Information Processing Standards Publication 197.

The AES operations in this package are not implemented using constant-time
algorithms. An exception is when running on systems with enabled hardware
support for AES that makes these operations constant-time. Examples include
amd64 systems using AES-NI extensions and s390x systems using
Message-Security-Assist extensions. On such systems, when the result of
NewCipher is passed to cipher.NewGCM, the GHASH operation used by GCM is also
constant-time.

## Index

- [Constants](#pkg-constants)
- [func NewCipher(key []byte) (cipher.Block, error)](#NewCipher)
- [type KeySizeError](#KeySizeError)
  - [func (k KeySizeError) Error() string](#KeySizeError.Error)

### Package files
 [aes_gcm.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/aes/aes_gcm.go) [block.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/aes/block.go) [cipher.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/aes/cipher.go) [cipher_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/aes/cipher_amd64.go) [const.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/aes/const.go) [modes.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/aes/modes.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="BlockSize">BlockSize</span> = 16</pre>

The AES block size in bytes.

<h2 id="NewCipher">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/aes/cipher.go#L21">NewCipher</a>
    <a href="#NewCipher">¶</a></h2>
<pre>func NewCipher(key []<a href="/builtin/#byte">byte</a>) (<a href="/crypto/cipher/">cipher</a>.<a href="/crypto/cipher/#Block">Block</a>, <a href="/builtin/#error">error</a>)</pre>

NewCipher creates and returns a new cipher.Block. The key argument should be the
AES key, either 16, 24, or 32 bytes to select AES-128, AES-192, or AES-256.

<h2 id="KeySizeError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/aes/cipher.go#L11">KeySizeError</a>
    <a href="#KeySizeError">¶</a></h2>
<pre>type KeySizeError <a href="/builtin/#int">int</a></pre>


<h3 id="KeySizeError.Error">func (KeySizeError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/aes/cipher.go#L13">Error</a>
    <a href="#KeySizeError.Error">¶</a></h3>
<pre>func (k <a href="#KeySizeError">KeySizeError</a>) Error() <a href="/builtin/#string">string</a></pre>



