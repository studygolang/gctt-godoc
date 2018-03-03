version: 1.10
## package lzw

  `import "compress/lzw"`

## Overview

Package lzw implements the Lempel-Ziv-Welch compressed data format, described in
T. A. Welch, ``A Technique for High-Performance Data Compression'', Computer,
17(6) (June 1984), pp 8-19.

In particular, it implements LZW as used by the GIF and PDF file formats, which
means variable-width codes up to 12 bits and the first two non-literal codes are
a clear code and an EOF code.

The TIFF file format uses a similar but incompatible version of the LZW
algorithm. See the golang.org/x/image/tiff/lzw package for an implementation.

## Index

- [func NewReader(r io.Reader, order Order, litWidth int) io.ReadCloser](#NewReader)
- [func NewWriter(w io.Writer, order Order, litWidth int) io.WriteCloser](#NewWriter)
- [type Order](#Order)

### Package files
 [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/lzw/reader.go) [writer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/lzw/writer.go)

<h2 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/lzw/reader.go#L229">NewReader</a>
    <a href="#NewReader">¶</a></h2>
<pre>func NewReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, order <a href="#Order">Order</a>, litWidth <a href="/builtin/#int">int</a>) <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a></pre>

NewReader creates a new io.ReadCloser. Reads from the returned io.ReadCloser
read and decompress data from r. If r does not also implement io.ByteReader, the
decompressor may read more data than necessary from r. It is the caller's
responsibility to call Close on the ReadCloser when finished reading. The number
of bits to use for literal codes, litWidth, must be in the range [2,8] and is
typically 8. It must equal the litWidth used during compression.

<h2 id="NewWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/lzw/writer.go#L231">NewWriter</a>
    <a href="#NewWriter">¶</a></h2>
<pre>func NewWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, order <a href="#Order">Order</a>, litWidth <a href="/builtin/#int">int</a>) <a href="/io/">io</a>.<a href="/io/#WriteCloser">WriteCloser</a></pre>

NewWriter creates a new io.WriteCloser. Writes to the returned io.WriteCloser
are compressed and written to w. It is the caller's responsibility to call Close
on the WriteCloser when finished writing. The number of bits to use for literal
codes, litWidth, must be in the range [2,8] and is typically 8. Input bytes must
be less than 1<<litWidth.

<h2 id="Order">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/lzw/reader.go#L19">Order</a>
    <a href="#Order">¶</a></h2>
<pre>type Order <a href="/builtin/#int">int</a></pre>

Order specifies the bit ordering in an LZW data stream.

<pre>const (
    <span class="comment">// LSB means Least Significant Bits first, as used in the GIF file format.</span>
    <span id="LSB">LSB</span> <a href="#Order">Order</a> = <a href="/builtin/#iota">iota</a>
    <span class="comment">// MSB means Most Significant Bits first, as used in the TIFF and PDF</span>
    <span class="comment">// file formats.</span>
    <span id="MSB">MSB</span>
)</pre>



