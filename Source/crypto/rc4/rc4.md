version: 1.10
## package rc4

  `import "crypto/rc4"`

## Overview

Package rc4 implements RC4 encryption, as defined in Bruce Schneier's Applied
Cryptography.

RC4 is cryptographically broken and should not be used for secure applications.

## Index

- [type Cipher](#Cipher)
  - [func NewCipher(key []byte) (*Cipher, error)](#NewCipher)
  - [func (c *Cipher) Reset()](#Cipher.Reset)
  - [func (c *Cipher) XORKeyStream(dst, src []byte)](#Cipher.XORKeyStream)
- [type KeySizeError](#KeySizeError)
  - [func (k KeySizeError) Error() string](#KeySizeError.Error)

### Package files
 [rc4.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rc4/rc4.go) [rc4_asm.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rc4/rc4_asm.go)

<h2 id="Cipher">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rc4/rc4.go#L5">Cipher</a>
    <a href="#Cipher">¶</a></h2>
<pre>type Cipher struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Cipher is an instance of RC4 using a particular key.

<h3 id="NewCipher">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rc4/rc4.go#L18">NewCipher</a>
    <a href="#NewCipher">¶</a></h3>
<pre>func NewCipher(key []<a href="/builtin/#byte">byte</a>) (*<a href="#Cipher">Cipher</a>, <a href="/builtin/#error">error</a>)</pre>

NewCipher creates and returns a new Cipher. The key argument should be the RC4
key, at least 1 byte and at most 256 bytes.

<h3 id="Cipher.Reset">func (*Cipher) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rc4/rc4.go#L37">Reset</a>
    <a href="#Cipher.Reset">¶</a></h3>
<pre>func (c *<a href="#Cipher">Cipher</a>) Reset()</pre>

Reset zeros the key data so that it will no longer appear in the process's
memory.

<h3 id="Cipher.XORKeyStream">func (*Cipher) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rc4/rc4_asm.go#L3">XORKeyStream</a>
    <a href="#Cipher.XORKeyStream">¶</a></h3>
<pre>func (c *<a href="#Cipher">Cipher</a>) XORKeyStream(dst, src []<a href="/builtin/#byte">byte</a>)</pre>

XORKeyStream sets dst to the result of XORing src with the key stream. Dst and
src must overlap entirely or not at all.

<h2 id="KeySizeError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rc4/rc4.go#L10">KeySizeError</a>
    <a href="#KeySizeError">¶</a></h2>
<pre>type KeySizeError <a href="/builtin/#int">int</a></pre>


<h3 id="KeySizeError.Error">func (KeySizeError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rc4/rc4.go#L12">Error</a>
    <a href="#KeySizeError.Error">¶</a></h3>
<pre>func (k <a href="#KeySizeError">KeySizeError</a>) Error() <a href="/builtin/#string">string</a></pre>



