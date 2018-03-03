version: 1.10
## package ascii85

  `import "encoding/ascii85"`

## Overview

Package ascii85 implements the ascii85 data encoding as used in the btoa tool
and Adobe's PostScript and PDF document formats.

## Index

- [func Decode(dst, src []byte, flush bool) (ndst, nsrc int, err error)](#Decode)
- [func Encode(dst, src []byte) int](#Encode)
- [func MaxEncodedLen(n int) int](#MaxEncodedLen)
- [func NewDecoder(r io.Reader) io.Reader](#NewDecoder)
- [func NewEncoder(w io.Writer) io.WriteCloser](#NewEncoder)
- [type CorruptInputError](#CorruptInputError)
  - [func (e CorruptInputError) Error() string](#CorruptInputError.Error)

### Package files
 [ascii85.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/ascii85/ascii85.go)

<h2 id="Decode">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/ascii85/ascii85.go#L179">Decode</a>
    <a href="#Decode">¶</a></h2>
<pre>func Decode(dst, src []<a href="/builtin/#byte">byte</a>, flush <a href="/builtin/#bool">bool</a>) (ndst, nsrc <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Decode decodes src into dst, returning both the number of bytes written to dst
and the number consumed from src. If src contains invalid ascii85 data, Decode
will return the number of bytes successfully written and a CorruptInputError.
Decode ignores space and control characters in src. Often, ascii85-encoded data
is wrapped in <~ and ~> symbols. Decode expects these to have been stripped by
the caller.

If flush is true, Decode assumes that src represents the end of the input stream
and processes it completely rather than wait for the completion of another
32-bit block.

NewDecoder wraps an io.Reader interface around Decode.

<h2 id="Encode">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/ascii85/ascii85.go#L17">Encode</a>
    <a href="#Encode">¶</a></h2>
<pre>func Encode(dst, src []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

Encode encodes src into at most MaxEncodedLen(len(src)) bytes of dst, returning
the actual number of bytes written.

The encoding handles 4-byte chunks, using a special encoding for the last
fragment, so Encode is not appropriate for use on individual blocks of a large
data stream. Use NewEncoder() instead.

Often, ascii85-encoded data is wrapped in <~ and ~> symbols. Encode does not add
these.

<h2 id="MaxEncodedLen">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/ascii85/ascii85.go#L76">MaxEncodedLen</a>
    <a href="#MaxEncodedLen">¶</a></h2>
<pre>func MaxEncodedLen(n <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

MaxEncodedLen returns the maximum length of an encoding of n source bytes.

<h2 id="NewDecoder">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/ascii85/ascii85.go#L236">NewDecoder</a>
    <a href="#NewDecoder">¶</a></h2>
<pre>func NewDecoder(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

NewDecoder constructs a new ascii85 stream decoder.

<h2 id="NewEncoder">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/ascii85/ascii85.go#L83">NewEncoder</a>
    <a href="#NewEncoder">¶</a></h2>
<pre>func NewEncoder(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/io/">io</a>.<a href="/io/#WriteCloser">WriteCloser</a></pre>

NewEncoder returns a new ascii85 stream encoder. Data written to the returned
writer will be encoded and then written to w. Ascii85 encodings operate in
32-bit blocks; when finished writing, the caller must Close the returned encoder
to flush any trailing partial block.

<h2 id="CorruptInputError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/ascii85/ascii85.go#L159">CorruptInputError</a>
    <a href="#CorruptInputError">¶</a></h2>
<pre>type CorruptInputError <a href="/builtin/#int64">int64</a></pre>


<h3 id="CorruptInputError.Error">func (CorruptInputError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/ascii85/ascii85.go#L161">Error</a>
    <a href="#CorruptInputError.Error">¶</a></h3>
<pre>func (e <a href="#CorruptInputError">CorruptInputError</a>) Error() <a href="/builtin/#string">string</a></pre>



