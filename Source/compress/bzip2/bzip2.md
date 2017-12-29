version: 1.9.2
## package bzip2

  `import "compress/bzip2"`

## Overview

Package bzip2 implements bzip2 decompression.

## Index

- [func NewReader(r io.Reader) io.Reader](#NewReader)
- [type StructuralError](#StructuralError)
  - [func (s StructuralError) Error() string](#StructuralError.Error)

### Package files
 [bit_reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/bzip2/bit_reader.go) [bzip2.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/bzip2/bzip2.go) [huffman.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/bzip2/huffman.go) [move_to_front.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/bzip2/move_to_front.go)

<h2 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/bzip2/bzip2.go#L36">NewReader</a>
    <a href="#NewReader">¶</a></h2>
<pre>func NewReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

NewReader returns an io.Reader which decompresses bzip2 data from r. If r does
not also implement io.ByteReader, the decompressor may read more data than
necessary from r.

<h2 id="StructuralError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/bzip2/bzip2.go#L7">StructuralError</a>
    <a href="#StructuralError">¶</a></h2>
<pre>type StructuralError <a href="/builtin/#string">string</a></pre>

A StructuralError is returned when the bzip2 data is found to be syntactically
invalid.

<h3 id="StructuralError.Error">func (StructuralError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/bzip2/bzip2.go#L9">Error</a>
    <a href="#StructuralError.Error">¶</a></h3>
<pre>func (s <a href="#StructuralError">StructuralError</a>) Error() <a href="/builtin/#string">string</a></pre>



