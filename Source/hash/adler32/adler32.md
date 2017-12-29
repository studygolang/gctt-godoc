version: 1.9.2
## package adler32

  `import "hash/adler32"`

## Overview

Package adler32 implements the Adler-32 checksum.

It is defined in RFC 1950:

    Adler-32 is composed of two sums accumulated per byte: s1 is
    the sum of all bytes, s2 is the sum of all s1 values. Both sums
    are done modulo 65521. s1 is initialized to 1, s2 to zero.  The
    Adler-32 checksum is stored as s2*65536 + s1 in most-
    significant-byte first (network) order.

## Index

- [Constants](#pkg-constants)
- [func Checksum(data []byte) uint32](#Checksum)
- [func New() hash.Hash32](#New)

### Package files
 [adler32.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/adler32/adler32.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="Size">Size</span> = 4</pre>

The size of an Adler-32 checksum in bytes.

<h2 id="Checksum">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/adler32/adler32.go#L80">Checksum</a>
    <a href="#Checksum">¶</a></h2>
<pre>func Checksum(data []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#uint32">uint32</a></pre>

Checksum returns the Adler-32 checksum of data.

<h2 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/adler32/adler32.go#L27">New</a>
    <a href="#New">¶</a></h2>
<pre>func New() <a href="/hash/">hash</a>.<a href="/hash/#Hash32">Hash32</a></pre>

New returns a new hash.Hash32 computing the Adler-32 checksum. Its Sum method
will lay the value out in big-endian byte order.


