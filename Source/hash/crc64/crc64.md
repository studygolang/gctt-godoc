version: 1.10
## package crc64

  `import "hash/crc64"`

## Overview

Package crc64 implements the 64-bit cyclic redundancy check, or CRC-64,
checksum. See http://en.wikipedia.org/wiki/Cyclic_redundancy_check for
information.

## Index

- [Constants](#pkg-constants)
- [func Checksum(data []byte, tab *Table) uint64](#Checksum)
- [func New(tab *Table) hash.Hash64](#New)
- [func Update(crc uint64, tab *Table, p []byte) uint64](#Update)
- [type Table](#Table)
  - [func MakeTable(poly uint64) *Table](#MakeTable)

### Package files
 [crc64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc64/crc64.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// The ISO polynomial, defined in ISO 3309 and used in HDLC.</span>
    <span id="ISO">ISO</span> = 0xD800000000000000

    <span class="comment">// The ECMA polynomial, defined in ECMA 182.</span>
    <span id="ECMA">ECMA</span> = 0xC96C5795D7870F42
)</pre>

Predefined polynomials.

<pre>const <span id="Size">Size</span> = 8</pre>

The size of a CRC-64 checksum in bytes.

<h2 id="Checksum">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc64/crc64.go#L189">Checksum</a>
    <a href="#Checksum">¶</a></h2>
<pre>func Checksum(data []<a href="/builtin/#byte">byte</a>, tab *<a href="#Table">Table</a>) <a href="/builtin/#uint64">uint64</a></pre>

Checksum returns the CRC-64 checksum of data using the polynomial represented by
the Table.

<h2 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc64/crc64.go#L78">New</a>
    <a href="#New">¶</a></h2>
<pre>func New(tab *<a href="#Table">Table</a>) <a href="/hash/">hash</a>.<a href="/hash/#Hash64">Hash64</a></pre>

New creates a new hash.Hash64 computing the CRC-64 checksum using the polynomial
represented by the Table. Its Sum method will lay the value out in big-endian
byte order. The returned Hash64 also implements encoding.BinaryMarshaler and
encoding.BinaryUnmarshaler to marshal and unmarshal the internal state of the
hash.

<h2 id="Update">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc64/crc64.go#L171">Update</a>
    <a href="#Update">¶</a></h2>
<pre>func Update(crc <a href="/builtin/#uint64">uint64</a>, tab *<a href="#Table">Table</a>, p []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#uint64">uint64</a></pre>

Update returns the result of adding the bytes in p to the crc.

<h2 id="Table">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc64/crc64.go#L18">Table</a>
    <a href="#Table">¶</a></h2>
<pre>type Table [256]<a href="/builtin/#uint64">uint64</a></pre>

Table is a 256-word table representing the polynomial for efficient processing.

<h3 id="MakeTable">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc64/crc64.go#L27">MakeTable</a>
    <a href="#MakeTable">¶</a></h3>
<pre>func MakeTable(poly <a href="/builtin/#uint64">uint64</a>) *<a href="#Table">Table</a></pre>

MakeTable returns a Table constructed from the specified polynomial. The
contents of this Table must not be modified.


