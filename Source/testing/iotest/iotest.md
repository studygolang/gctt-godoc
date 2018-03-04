version: 1.10
## package iotest

  `import "testing/iotest"`

## Overview

Package iotest implements Readers and Writers useful mainly for testing.

## Index

- [Variables](#pkg-variables)
- [func DataErrReader(r io.Reader) io.Reader](#DataErrReader)
- [func HalfReader(r io.Reader) io.Reader](#HalfReader)
- [func NewReadLogger(prefix string, r io.Reader) io.Reader](#NewReadLogger)
- [func NewWriteLogger(prefix string, w io.Writer) io.Writer](#NewWriteLogger)
- [func OneByteReader(r io.Reader) io.Reader](#OneByteReader)
- [func TimeoutReader(r io.Reader) io.Reader](#TimeoutReader)
- [func TruncateWriter(w io.Writer, n int64) io.Writer](#TruncateWriter)

### Package files
 [logger.go](//github.com/golang/go/blob/release-branch.go1.10/src/testing/iotest/logger.go) [reader.go](//github.com/golang/go/blob/release-branch.go1.10/src/testing/iotest/reader.go) [writer.go](//github.com/golang/go/blob/release-branch.go1.10/src/testing/iotest/writer.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrTimeout">ErrTimeout</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;timeout&#34;)</pre>


<h2 id="DataErrReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/testing/iotest/reader.go#L35">DataErrReader</a>
    <a href="#DataErrReader">¶</a></h2>
<pre>func DataErrReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

DataErrReader changes the way errors are handled by a Reader. Normally, a Reader
returns an error (typically EOF) from the first Read call after the last piece
of data is read. DataErrReader wraps a Reader and changes its behavior so the
final error is returned along with the final data, instead of in the first call
after the final data.

<h2 id="HalfReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/testing/iotest/reader.go#L20">HalfReader</a>
    <a href="#HalfReader">¶</a></h2>
<pre>func HalfReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

HalfReader returns a Reader that implements Read by reading half as many
requested bytes from r.

<h2 id="NewReadLogger">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/testing/iotest/logger.go#L42">NewReadLogger</a>
    <a href="#NewReadLogger">¶</a></h2>
<pre>func NewReadLogger(prefix <a href="/builtin/#string">string</a>, r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

NewReadLogger returns a reader that behaves like r except that it logs (using
log.Printf) each read to standard error, printing the prefix and the hexadecimal
data read.

<h2 id="NewWriteLogger">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/testing/iotest/logger.go#L20">NewWriteLogger</a>
    <a href="#NewWriteLogger">¶</a></h2>
<pre>func NewWriteLogger(prefix <a href="/builtin/#string">string</a>, w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a></pre>

NewWriteLogger returns a writer that behaves like w except that it logs (using
log.Printf) each write to standard error, printing the prefix and the
hexadecimal data written.

<h2 id="OneByteReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/testing/iotest/reader.go#L5">OneByteReader</a>
    <a href="#OneByteReader">¶</a></h2>
<pre>func OneByteReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

OneByteReader returns a Reader that implements each non-empty Read by reading
one byte from r.

<h2 id="TimeoutReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/testing/iotest/reader.go#L65">TimeoutReader</a>
    <a href="#TimeoutReader">¶</a></h2>
<pre>func TimeoutReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

TimeoutReader returns ErrTimeout on the second read with no data. Subsequent
calls to read succeed.

<h2 id="TruncateWriter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/testing/iotest/writer.go#L1">TruncateWriter</a>
    <a href="#TruncateWriter">¶</a></h2>
<pre>func TruncateWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, n <a href="/builtin/#int64">int64</a>) <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a></pre>

TruncateWriter returns a Writer that writes to w but stops silently after n
bytes.


