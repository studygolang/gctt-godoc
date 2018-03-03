version: 1.10
## package gzip

  `import "compress/gzip"`

## Overview

Package gzip implements reading and writing of gzip format compressed files, as
specified in RFC 1952.

<a id="example_writerReader"></a>
Example:

    var buf bytes.Buffer
    zw := gzip.NewWriter(&buf)

    // Setting the Header fields is optional.
    zw.Name = "a-new-hope.txt"
    zw.Comment = "an epic space opera by George Lucas"
    zw.ModTime = time.Date(1977, time.May, 25, 0, 0, 0, 0, time.UTC)

    _, err := zw.Write([]byte("A long time ago in a galaxy far, far away..."))
    if err != nil {
        log.Fatal(err)
    }

    if err := zw.Close(); err != nil {
        log.Fatal(err)
    }

    zr, err := gzip.NewReader(&buf)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("Name: %s\nComment: %s\nModTime: %s\n\n", zr.Name, zr.Comment, zr.ModTime.UTC())

    if _, err := io.Copy(os.Stdout, zr); err != nil {
        log.Fatal(err)
    }

    if err := zr.Close(); err != nil {
        log.Fatal(err)
    }

    // Output:
    // Name: a-new-hope.txt
    // Comment: an epic space opera by George Lucas
    // ModTime: 1977-05-25 00:00:00 +0000 UTC
    //
    // A long time ago in a galaxy far, far away...

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [type Header](#Header)
- [type Reader](#Reader)
  - [func NewReader(r io.Reader) (*Reader, error)](#NewReader)
  - [func (z *Reader) Close() error](#Reader.Close)
  - [func (z *Reader) Multistream(ok bool)](#Reader.Multistream)
  - [func (z *Reader) Read(p []byte) (n int, err error)](#Reader.Read)
  - [func (z *Reader) Reset(r io.Reader) error](#Reader.Reset)
- [type Writer](#Writer)
  - [func NewWriter(w io.Writer) *Writer](#NewWriter)
  - [func NewWriterLevel(w io.Writer, level int) (*Writer, error)](#NewWriterLevel)
  - [func (z *Writer) Close() error](#Writer.Close)
  - [func (z *Writer) Flush() error](#Writer.Flush)
  - [func (z *Writer) Reset(w io.Writer)](#Writer.Reset)
  - [func (z *Writer) Write(p []byte) (int, error)](#Writer.Write)

### Examples

- [Reader.Multistream](#exampleReader_Multistream)
- [Package (WriterReader)](#example_writerReader)

### Package files
 [gunzip.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gunzip.go) [gzip.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gzip.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="NoCompression">NoCompression</span>      = <a href="/compress/flate/">flate</a>.<a href="/compress/flate/#NoCompression">NoCompression</a>
    <span id="BestSpeed">BestSpeed</span>          = <a href="/compress/flate/">flate</a>.<a href="/compress/flate/#BestSpeed">BestSpeed</a>
    <span id="BestCompression">BestCompression</span>    = <a href="/compress/flate/">flate</a>.<a href="/compress/flate/#BestCompression">BestCompression</a>
    <span id="DefaultCompression">DefaultCompression</span> = <a href="/compress/flate/">flate</a>.<a href="/compress/flate/#DefaultCompression">DefaultCompression</a>
    <span id="HuffmanOnly">HuffmanOnly</span>        = <a href="/compress/flate/">flate</a>.<a href="/compress/flate/#HuffmanOnly">HuffmanOnly</a>
)</pre>

These constants are copied from the flate package, so that code that imports
"compress/gzip" does not also have to import "compress/flate".

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span class="comment">// ErrChecksum is returned when reading GZIP data that has an invalid checksum.</span>
    <span id="ErrChecksum">ErrChecksum</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;gzip: invalid checksum&#34;)
    <span class="comment">// ErrHeader is returned when reading GZIP data that has an invalid header.</span>
    <span id="ErrHeader">ErrHeader</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;gzip: invalid header&#34;)
)</pre>


<h2 id="Header">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gunzip.go#L42">Header</a>
    <a href="#Header">¶</a></h2>
<pre>type Header struct {
<span id="Header.Comment"></span>    Comment <a href="/builtin/#string">string</a>    <span class="comment">// comment</span>
<span id="Header.Extra"></span>    Extra   []<a href="/builtin/#byte">byte</a>    <span class="comment">// &#34;extra data&#34;</span>
<span id="Header.ModTime"></span>    ModTime <a href="/time/">time</a>.<a href="/time/#Time">Time</a> <span class="comment">// modification time</span>
<span id="Header.Name"></span>    Name    <a href="/builtin/#string">string</a>    <span class="comment">// file name</span>
<span id="Header.OS"></span>    OS      <a href="/builtin/#byte">byte</a>      <span class="comment">// operating system type</span>
}</pre>

The gzip file stores a header giving metadata about the compressed file. That
header is exposed as the fields of the Writer and Reader structs.

Strings must be UTF-8 encoded and may only contain Unicode code points U+0001
through U+00FF, due to limitations of the GZIP file format.

<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gunzip.go#L64">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <a href="#Header">Header</a> <span class="comment">// valid after NewReader or Reader.Reset</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Reader is an io.Reader that can be read to retrieve uncompressed data from a
gzip-format compressed file.

In general, a gzip file can be a concatenation of gzip files, each with its own
header. Reads from the Reader return the concatenation of the uncompressed data
of each. Only the first header is recorded in the Reader fields.

Gzip files store a length and checksum of the uncompressed data. The Reader will
return an ErrChecksum when Read reaches the end of the uncompressed data if it
does not have the expected length or checksum. Clients should treat data
returned by Read as tentative until they receive the io.EOF marking the end of
the data.

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gunzip.go#L82">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (*<a href="#Reader">Reader</a>, <a href="/builtin/#error">error</a>)</pre>

NewReader creates a new Reader reading the given reader. If r does not also
implement io.ByteReader, the decompressor may read more data than necessary from
r.

It is the caller's responsibility to call Close on the Reader when done.

The Reader.Header fields will be valid in the Reader returned.

<h3 id="Reader.Close">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gunzip.go#L282">Close</a>
    <a href="#Reader.Close">¶</a></h3>
<pre>func (z *<a href="#Reader">Reader</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the Reader. It does not close the underlying io.Reader. In order
for the GZIP checksum to be verified, the reader must be fully consumed until
the io.EOF.

<h3 id="Reader.Multistream">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gunzip.go#L123">Multistream</a>
    <a href="#Reader.Multistream">¶</a></h3>
<pre>func (z *<a href="#Reader">Reader</a>) Multistream(ok <a href="/builtin/#bool">bool</a>)</pre>

Multistream controls whether the reader supports multistream files.

If enabled (the default), the Reader expects the input to be a sequence of
individually gzipped data streams, each with its own header and trailer, ending
at EOF. The effect is that the concatenation of a sequence of gzipped files is
treated as equivalent to the gzip of the concatenation of the sequence. This is
standard behavior for gzip readers.

Calling Multistream(false) disables this behavior; disabling the behavior can be
useful when reading file formats that distinguish individual gzip data streams
or mix gzip data streams with other data streams. In this mode, when the Reader
reaches the end of the data stream, Read returns io.EOF. If the underlying
reader implements io.ByteReader, it will be left positioned just after the gzip
stream. To start the next stream, call z.Reset(r) followed by
z.Multistream(false). If there is no next stream, z.Reset(r) will return io.EOF.

<a id="exampleReader_Multistream"></a>
Example:

    var buf bytes.Buffer
    zw := gzip.NewWriter(&buf)

    var files = []struct {
        name    string
        comment string
        modTime time.Time
        data    string
    }{
        {"file-1.txt", "file-header-1", time.Date(2006, time.February, 1, 3, 4, 5, 0, time.UTC), "Hello Gophers - 1"},
        {"file-2.txt", "file-header-2", time.Date(2007, time.March, 2, 4, 5, 6, 1, time.UTC), "Hello Gophers - 2"},
    }

    for _, file := range files {
        zw.Name = file.name
        zw.Comment = file.comment
        zw.ModTime = file.modTime

        if _, err := zw.Write([]byte(file.data)); err != nil {
            log.Fatal(err)
        }

        if err := zw.Close(); err != nil {
            log.Fatal(err)
        }

        zw.Reset(&buf)
    }

    zr, err := gzip.NewReader(&buf)
    if err != nil {
        log.Fatal(err)
    }

    for {
        zr.Multistream(false)
        fmt.Printf("Name: %s\nComment: %s\nModTime: %s\n\n", zr.Name, zr.Comment, zr.ModTime.UTC())

        if _, err := io.Copy(os.Stdout, zr); err != nil {
            log.Fatal(err)
        }

        fmt.Print("\n\n")

        err = zr.Reset(&buf)
        if err == io.EOF {
            break
        }
        if err != nil {
            log.Fatal(err)
        }
    }

    if err := zr.Close(); err != nil {
        log.Fatal(err)
    }

    // Output:
    // Name: file-1.txt
    // Comment: file-header-1
    // ModTime: 2006-02-01 03:04:05 +0000 UTC
    //
    // Hello Gophers - 1
    //
    // Name: file-2.txt
    // Comment: file-header-2
    // ModTime: 2007-03-02 04:05:06 +0000 UTC
    //
    // Hello Gophers - 2

<h3 id="Reader.Read">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gunzip.go#L236">Read</a>
    <a href="#Reader.Read">¶</a></h3>
<pre>func (z *<a href="#Reader">Reader</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read implements io.Reader, reading uncompressed bytes from its underlying
Reader.

<h3 id="Reader.Reset">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gunzip.go#L93">Reset</a>
    <a href="#Reader.Reset">¶</a></h3>
<pre>func (z *<a href="#Reader">Reader</a>) Reset(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/builtin/#error">error</a></pre>

Reset discards the Reader z's state and makes it equivalent to the result of its
original state from NewReader, but reading from r instead. This permits reusing
a Reader rather than allocating a new one.

<h2 id="Writer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gzip.go#L18">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
    <a href="#Header">Header</a> <span class="comment">// written at first call to Write, Flush, or Close</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Writer is an io.WriteCloser. Writes to a Writer are compressed and written to
w.

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gzip.go#L39">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) *<a href="#Writer">Writer</a></pre>

NewWriter returns a new Writer. Writes to the returned writer are compressed and
written to w.

It is the caller's responsibility to call Close on the WriteCloser when done.
Writes may be buffered and not flushed until Close.

Callers that wish to set the fields in Writer.Header must do so before the first
call to Write, Flush, or Close.

<h3 id="NewWriterLevel">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gzip.go#L50">NewWriterLevel</a>
    <a href="#NewWriterLevel">¶</a></h3>
<pre>func NewWriterLevel(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, level <a href="/builtin/#int">int</a>) (*<a href="#Writer">Writer</a>, <a href="/builtin/#error">error</a>)</pre>

NewWriterLevel is like NewWriter but specifies the compression level instead of
assuming DefaultCompression.

The compression level can be DefaultCompression, NoCompression, HuffmanOnly or
any integer value between BestSpeed and BestCompression inclusive. The error
returned will be nil if the level is valid.

<h3 id="Writer.Close">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gzip.go#L218">Close</a>
    <a href="#Writer.Close">¶</a></h3>
<pre>func (z *<a href="#Writer">Writer</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the Writer by flushing any unwritten data to the underlying
io.Writer and writing the GZIP footer. It does not close the underlying
io.Writer.

<h3 id="Writer.Flush">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gzip.go#L198">Flush</a>
    <a href="#Writer.Flush">¶</a></h3>
<pre>func (z *<a href="#Writer">Writer</a>) Flush() <a href="/builtin/#error">error</a></pre>

Flush flushes any pending compressed data to the underlying writer.

It is useful mainly in compressed network protocols, to ensure that a remote
reader has enough data to reconstruct a packet. Flush does not return until the
data has been written. If the underlying writer returns an error, Flush returns
that error.

In the terminology of the zlib library, Flush is equivalent to Z_SYNC_FLUSH.

<h3 id="Writer.Reset">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gzip.go#L78">Reset</a>
    <a href="#Writer.Reset">¶</a></h3>
<pre>func (z *<a href="#Writer">Writer</a>) Reset(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>)</pre>

Reset discards the Writer z's state and makes it equivalent to the result of its
original state from NewWriter or NewWriterLevel, but writing to w instead. This
permits reusing a Writer rather than allocating a new one.

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/compress/gzip/gzip.go#L129">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (z *<a href="#Writer">Writer</a>) Write(p []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write writes a compressed form of p to the underlying io.Writer. The compressed
bytes are not necessarily flushed until the Writer is closed.


