version: 1.9.2
## package crc32

  `import "hash/crc32"`

## Overview

Package crc32 implements the 32-bit cyclic redundancy check, or CRC-32,
checksum. See http://en.wikipedia.org/wiki/Cyclic_redundancy_check for
information.

Polynomials are represented in LSB-first form also known as reversed
representation.

See
http://en.wikipedia.org/wiki/Mathematics_of_cyclic_redundancy_checks#Reversed_representations_and_reciprocal_polynomials
for information.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func Checksum(data []byte, tab *Table) uint32](#Checksum)
- [func ChecksumIEEE(data []byte) uint32](#ChecksumIEEE)
- [func New(tab *Table) hash.Hash32](#New)
- [func NewIEEE() hash.Hash32](#NewIEEE)
- [func Update(crc uint32, tab *Table, p []byte) uint32](#Update)
- [type Table](#Table)
  - [func MakeTable(poly uint32) *Table](#MakeTable)

### Examples

- [MakeTable](#example_MakeTable)

### Package files
 [crc32.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc32/crc32.go) [crc32_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc32/crc32_amd64.go) [crc32_generic.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc32/crc32_generic.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// IEEE is by far and away the most common CRC-32 polynomial.</span>
    <span class="comment">// Used by ethernet (IEEE 802.3), v.42, fddi, gzip, zip, png, ...</span>
    <span id="IEEE">IEEE</span> = 0xedb88320

    <span class="comment">// Castagnoli&#39;s polynomial, used in iSCSI.</span>
    <span class="comment">// Has better error detection characteristics than IEEE.</span>
    <span class="comment">// http://dx.doi.org/10.1109/26.231911</span>
    <span id="Castagnoli">Castagnoli</span> = 0x82f63b78

    <span class="comment">// Koopman&#39;s polynomial.</span>
    <span class="comment">// Also has better error detection characteristics than IEEE.</span>
    <span class="comment">// http://dx.doi.org/10.1109/DSN.2002.1028931</span>
    <span id="Koopman">Koopman</span> = 0xeb31d82e
)</pre>

Predefined polynomials.

<pre>const <span id="Size">Size</span> = 4</pre>

The size of a CRC-32 checksum in bytes.

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="IEEETable">IEEETable</span> = simpleMakeTable(<a href="#IEEE">IEEE</a>)</pre>

IEEETable is the table for the IEEE polynomial.

<h2 id="Checksum">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc32/crc32.go#L190">Checksum</a>
    <a href="#Checksum">¶</a></h2>
<pre>func Checksum(data []<a href="/builtin/#byte">byte</a>, tab *<a href="#Table">Table</a>) <a href="/builtin/#uint32">uint32</a></pre>

Checksum returns the CRC-32 checksum of data using the polynomial represented by
the Table.

<h2 id="ChecksumIEEE">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc32/crc32.go#L194">ChecksumIEEE</a>
    <a href="#ChecksumIEEE">¶</a></h2>
<pre>func ChecksumIEEE(data []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#uint32">uint32</a></pre>

ChecksumIEEE returns the CRC-32 checksum of data using the IEEE polynomial.

<h2 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc32/crc32.go#L134">New</a>
    <a href="#New">¶</a></h2>
<pre>func New(tab *<a href="#Table">Table</a>) <a href="/hash/">hash</a>.<a href="/hash/#Hash32">Hash32</a></pre>

New creates a new hash.Hash32 computing the CRC-32 checksum using the polynomial
represented by the Table. Its Sum method will lay the value out in big-endian
byte order.

<h2 id="NewIEEE">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc32/crc32.go#L144">NewIEEE</a>
    <a href="#NewIEEE">¶</a></h2>
<pre>func NewIEEE() <a href="/hash/">hash</a>.<a href="/hash/#Hash32">Hash32</a></pre>

NewIEEE creates a new hash.Hash32 computing the CRC-32 checksum using the IEEE
polynomial. Its Sum method will lay the value out in big-endian byte order.

<h2 id="Update">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc32/crc32.go#L153">Update</a>
    <a href="#Update">¶</a></h2>
<pre>func Update(crc <a href="/builtin/#uint32">uint32</a>, tab *<a href="#Table">Table</a>, p []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#uint32">uint32</a></pre>

Update returns the result of adding the bytes in p to the crc.

<h2 id="Table">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc32/crc32.go#L31">Table</a>
    <a href="#Table">¶</a></h2>
<pre>type Table [256]<a href="/builtin/#uint32">uint32</a></pre>

Table is a 256-word table representing the polynomial for efficient processing.

<h3 id="MakeTable">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/crc32/crc32.go#L113">MakeTable</a>
    <a href="#MakeTable">¶</a></h3>
<pre>func MakeTable(poly <a href="/builtin/#uint32">uint32</a>) *<a href="#Table">Table</a></pre>

MakeTable returns a Table constructed from the specified polynomial. The
contents of this Table must not be modified.

<a id="example_MakeTable"></a>
Example:

    // In this package, the CRC polynomial is represented in reversed notation,
    // or LSB-first representation.
    //
    // LSB-first representation is a hexadecimal number with n bits, in which the
    // most significant bit represents the coefficient of x⁰ and the least significant
    // bit represents the coefficient of xⁿ⁻¹ (the coefficient for xⁿ is implicit).
    //
    // For example, CRC32-Q, as defined by the following polynomial,
    //	x³²+ x³¹+ x²⁴+ x²²+ x¹⁶+ x¹⁴+ x⁸+ x⁷+ x⁵+ x³+ x¹+ x⁰
    // has the reversed notation 0b11010101100000101000001010000001, so the value
    // that should be passed to MakeTable is 0xD5828281.
    crc32q := crc32.MakeTable(0xD5828281)
    fmt.Printf("%08x\n", crc32.Checksum([]byte("Hello world"), crc32q))
    // Output:
    // 2964d064


