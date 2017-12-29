version: 1.9.2
## package des

  `import "crypto/des"`

## Overview

Package des implements the Data Encryption Standard (DES) and the Triple Data
Encryption Algorithm (TDEA) as defined in U.S. Federal Information Processing
Standards Publication 46-3.

DES is cryptographically broken and should not be used for secure applications.

## Index

- [Constants](#pkg-constants)
- [func NewCipher(key []byte) (cipher.Block, error)](#NewCipher)
- [func NewTripleDESCipher(key []byte) (cipher.Block, error)](#NewTripleDESCipher)
- [type KeySizeError](#KeySizeError)
  - [func (k KeySizeError) Error() string](#KeySizeError.Error)

### Examples

- [NewTripleDESCipher](#example_NewTripleDESCipher)

### Package files
 [block.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/des/block.go) [cipher.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/des/cipher.go) [const.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/des/const.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="BlockSize">BlockSize</span> = 8</pre>

The DES block size in bytes.

<h2 id="NewCipher">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/des/cipher.go#L18">NewCipher</a>
    <a href="#NewCipher">¶</a></h2>
<pre>func NewCipher(key []<a href="/builtin/#byte">byte</a>) (<a href="/crypto/cipher/">cipher</a>.<a href="/crypto/cipher/#Block">Block</a>, <a href="/builtin/#error">error</a>)</pre>

NewCipher creates and returns a new cipher.Block.

<h2 id="NewTripleDESCipher">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/des/cipher.go#L40">NewTripleDESCipher</a>
    <a href="#NewTripleDESCipher">¶</a></h2>
<pre>func NewTripleDESCipher(key []<a href="/builtin/#byte">byte</a>) (<a href="/crypto/cipher/">cipher</a>.<a href="/crypto/cipher/#Block">Block</a>, <a href="/builtin/#error">error</a>)</pre>

NewTripleDESCipher creates and returns a new cipher.Block.

<a id="example_NewTripleDESCipher"></a>
Example:

    // NewTripleDESCipher can also be used when EDE2 is required by
    // duplicating the first 8 bytes of the 16-byte key.
    ede2Key := []byte("example key 1234")

    var tripleDESKey []byte
    tripleDESKey = append(tripleDESKey, ede2Key[:16]...)
    tripleDESKey = append(tripleDESKey, ede2Key[:8]...)

    _, err := des.NewTripleDESCipher(tripleDESKey)
    if err != nil {
        panic(err)
    }

    // See crypto/cipher for how to use a cipher.Block for encryption and
    // decryption.

<h2 id="KeySizeError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/des/cipher.go#L6">KeySizeError</a>
    <a href="#KeySizeError">¶</a></h2>
<pre>type KeySizeError <a href="/builtin/#int">int</a></pre>


<h3 id="KeySizeError.Error">func (KeySizeError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/des/cipher.go#L8">Error</a>
    <a href="#KeySizeError.Error">¶</a></h3>
<pre>func (k <a href="#KeySizeError">KeySizeError</a>) Error() <a href="/builtin/#string">string</a></pre>



