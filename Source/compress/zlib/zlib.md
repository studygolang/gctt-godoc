version: 1.10
## package zlib

  `import "compress/zlib"`

## Overview

Package zlib implements reading and writing of zlib format compressed data, as
specified in RFC 1950.

The implementation provides filters that uncompress during reading and compress
during writing. For example, to write compressed data to a buffer:

    var b bytes.Buffer
    w := zlib.NewWriter(&b)
    w.Write([]byte("hello, world\n"))
    w.Close()

and to read that data back:

    r, err := zlib.NewReader(&b)
    io.Copy(os.Stdout, r)
    r.Close()

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func NewReader(r io.Reader) (io.ReadCloser, error)](#NewReader)
- [func NewReaderDict(r io.Reader, dict []byte) (io.ReadCloser, error)](#NewReaderDict)
- [type Resetter](#Resetter)
- [type Writer](#Writer)
  - [func NewWriter(w io.Writer) *Writer](#NewWriter)
  - [func NewWriterLevel(w io.Writer, level int) (*Writer, error)](#NewWriterLevel)
  - [func NewWriterLevelDict(w io.Writer, level int, dict []byte) (*Writer, error)](#NewWriterLevelDict)
  - [func (z *Writer) Close() error](#Writer.Close)
  - [func (z *Writer) Flush() error](#Writer.Flush)
  - [func (z *Writer) Reset(w io.Writer)](#Writer.Reset)
  - [func (z *Writer) Write(p []byte) (n int, err error)](#Writer.Write)

### Examples

- [NewReader](#exampleNewReader)
- [NewWriter](#exampleNewWriter)

### Package files
 [reader.go](//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/reader.go) [writer.go](//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/writer.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="NoCompression">NoCompression</span>      = <a href="/compress/flate/">flate</a>.<a href="/compress/flate/#NoCompression">NoCompression</a>
    <span id="BestSpeed">BestSpeed</span>          = <a href="/compress/flate/">flate</a>.<a href="/compress/flate/#BestSpeed">BestSpeed</a>
    <span id="BestCompression">BestCompression</span>    = <a href="/compress/flate/">flate</a>.<a href="/compress/flate/#BestCompression">BestCompression</a>
    <span id="DefaultCompression">DefaultCompression</span> = <a href="/compress/flate/">flate</a>.<a href="/compress/flate/#DefaultCompression">DefaultCompression</a>
    <span id="HuffmanOnly">HuffmanOnly</span>        = <a href="/compress/flate/">flate</a>.<a href="/compress/flate/#HuffmanOnly">HuffmanOnly</a>
)</pre>

These constants are copied from the flate package, so that code that imports
"compress/zlib" does not also have to import "compress/flate".

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span class="comment">// ErrChecksum is returned when reading ZLIB data that has an invalid checksum.</span>
    <span id="ErrChecksum">ErrChecksum</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;zlib: invalid checksum&#34;)
    <span class="comment">// ErrDictionary is returned when reading ZLIB data that has an invalid dictionary.</span>
    <span id="ErrDictionary">ErrDictionary</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;zlib: invalid dictionary&#34;)
    <span class="comment">// ErrHeader is returned when reading ZLIB data that has an invalid header.</span>
    <span id="ErrHeader">ErrHeader</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;zlib: invalid header&#34;)
)</pre>


<h2 id="NewReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/reader.go#L60">NewReader</a>
    <a href="#NewReader">¶</a></h2>
<pre>func NewReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>, <a href="/builtin/#error">error</a>)</pre>

NewReader creates a new ReadCloser. Reads from the returned ReadCloser read and
decompress data from r. If r does not implement io.ByteReader, the decompressor
may read more data than necessary from r. It is the caller's responsibility to
call Close on the ReadCloser when done.

The ReadCloser returned by NewReader also implements Resetter.

<a id="exampleNewReader"></a>
Example:

    buff := []byte{120, 156, 202, 72, 205, 201, 201, 215, 81, 40, 207,
        47, 202, 73, 225, 2, 4, 0, 0, 255, 255, 33, 231, 4, 147}
    b := bytes.NewReader(buff)

    r, err := zlib.NewReader(b)
    if err != nil {
        panic(err)
    }
    io.Copy(os.Stdout, r)
    // Output: hello, world
    r.Close()

<h2 id="NewReaderDict">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/reader.go#L69">NewReaderDict</a>
    <a href="#NewReaderDict">¶</a></h2>
<pre>func NewReaderDict(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, dict []<a href="/builtin/#byte">byte</a>) (<a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>, <a href="/builtin/#error">error</a>)</pre>

NewReaderDict is like NewReader but uses a preset dictionary. NewReaderDict
ignores the dictionary if the compressed data does not refer to it. If the
compressed data refers to a different dictionary, NewReaderDict returns
ErrDictionary.

The ReadCloser returned by NewReaderDict also implements Resetter.

<h2 id="Resetter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/reader.go#L47">Resetter</a>
    <a href="#Resetter">¶</a></h2>
<pre>type Resetter interface {
    <span class="comment">// Reset discards any buffered data and resets the Resetter as if it was</span>
    <span class="comment">// newly initialized with the given reader.</span>
    Reset(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, dict []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a>
}</pre>

Resetter resets a ReadCloser returned by NewReader or NewReaderDict to to switch
to a new underlying Reader. This permits reusing a ReadCloser instead of
allocating a new one.

<h2 id="Writer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/writer.go#L17">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Writer takes data written to it and writes the compressed form of that data to
an underlying writer (see NewWriter).

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/writer.go#L33">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) *<a href="#Writer">Writer</a></pre>

NewWriter creates a new Writer. Writes to the returned Writer are compressed and
written to w.

It is the caller's responsibility to call Close on the WriteCloser when done.
Writes may be buffered and not flushed until Close.

<a id="exampleNewWriter"></a>
Example:

    var b bytes.Buffer

    w := zlib.NewWriter(&b)
    w.Write([]byte("hello, world\n"))
    w.Close()
    fmt.Println(b.Bytes())
    // Output: [120 156 202 72 205 201 201 215 81 40 207 47 202 73 225 2 4 0 0 255 255 33 231 4 147]

<h3 id="NewWriterLevel">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/writer.go#L44">NewWriterLevel</a>
    <a href="#NewWriterLevel">¶</a></h3>
<pre>func NewWriterLevel(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, level <a href="/builtin/#int">int</a>) (*<a href="#Writer">Writer</a>, <a href="/builtin/#error">error</a>)</pre>

NewWriterLevel is like NewWriter but specifies the compression level instead of
assuming DefaultCompression.

The compression level can be DefaultCompression, NoCompression, HuffmanOnly or
any integer value between BestSpeed and BestCompression inclusive. The error
returned will be nil if the level is valid.

<h3 id="NewWriterLevelDict">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/writer.go#L53">NewWriterLevelDict</a>
    <a href="#NewWriterLevelDict">¶</a></h3>
<pre>func NewWriterLevelDict(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, level <a href="/builtin/#int">int</a>, dict []<a href="/builtin/#byte">byte</a>) (*<a href="#Writer">Writer</a>, <a href="/builtin/#error">error</a>)</pre>

NewWriterLevelDict is like NewWriterLevel but specifies a dictionary to compress
with.

The dictionary may be nil. If not, its contents should not be modified until the
Writer is closed.

<h3 id="Writer.Close">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/writer.go#L170">Close</a>
    <a href="#Writer.Close">¶</a></h3>
<pre>func (z *<a href="#Writer">Writer</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the Writer, flushing any unwritten data to the underlying
io.Writer, but does not close the underlying io.Writer.

<h3 id="Writer.Flush">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/writer.go#L157">Flush</a>
    <a href="#Writer.Flush">¶</a></h3>
<pre>func (z *<a href="#Writer">Writer</a>) Flush() <a href="/builtin/#error">error</a></pre>

Flush flushes the Writer to its underlying io.Writer.

<h3 id="Writer.Reset">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/writer.go#L67">Reset</a>
    <a href="#Writer.Reset">¶</a></h3>
<pre>func (z *<a href="#Writer">Writer</a>) Reset(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>)</pre>

Reset clears the state of the Writer z such that it is equivalent to its initial
state from NewWriterLevel or NewWriterLevelDict, but instead writing to w.

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/zlib/writer.go#L137">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (z *<a href="#Writer">Writer</a>) Write(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write writes a compressed form of p to the underlying io.Writer. The compressed
bytes are not necessarily flushed until the Writer is closed or explicitly
flushed.


