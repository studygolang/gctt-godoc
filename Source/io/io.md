version: 1.10
## package io

  `import "io"`

## Overview

Package io provides basic interfaces to I/O primitives. Its primary job is to
wrap existing implementations of such primitives, such as those in package os,
into shared public interfaces that abstract the functionality, plus some other
related primitives.

Because these interfaces and primitives wrap lower-level operations with various
implementations, unless otherwise informed clients should not assume they are
safe for parallel execution.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func Copy(dst Writer, src Reader) (written int64, err error)](#Copy)
- [func CopyBuffer(dst Writer, src Reader, buf []byte) (written int64, err error)](#CopyBuffer)
- [func CopyN(dst Writer, src Reader, n int64) (written int64, err error)](#CopyN)
- [func ReadAtLeast(r Reader, buf []byte, min int) (n int, err error)](#ReadAtLeast)
- [func ReadFull(r Reader, buf []byte) (n int, err error)](#ReadFull)
- [func WriteString(w Writer, s string) (n int, err error)](#WriteString)
- [type ByteReader](#ByteReader)
- [type ByteScanner](#ByteScanner)
- [type ByteWriter](#ByteWriter)
- [type Closer](#Closer)
- [type LimitedReader](#LimitedReader)
  - [func (l *LimitedReader) Read(p []byte) (n int, err error)](#LimitedReader.Read)
- [type PipeReader](#PipeReader)
  - [func Pipe() (*PipeReader, *PipeWriter)](#Pipe)
  - [func (r *PipeReader) Close() error](#PipeReader.Close)
  - [func (r *PipeReader) CloseWithError(err error) error](#PipeReader.CloseWithError)
  - [func (r *PipeReader) Read(data []byte) (n int, err error)](#PipeReader.Read)
- [type PipeWriter](#PipeWriter)
  - [func (w *PipeWriter) Close() error](#PipeWriter.Close)
  - [func (w *PipeWriter) CloseWithError(err error) error](#PipeWriter.CloseWithError)
  - [func (w *PipeWriter) Write(data []byte) (n int, err error)](#PipeWriter.Write)
- [type ReadCloser](#ReadCloser)
- [type ReadSeeker](#ReadSeeker)
- [type ReadWriteCloser](#ReadWriteCloser)
- [type ReadWriteSeeker](#ReadWriteSeeker)
- [type ReadWriter](#ReadWriter)
- [type Reader](#Reader)
  - [func LimitReader(r Reader, n int64) Reader](#LimitReader)
  - [func MultiReader(readers ...Reader) Reader](#MultiReader)
  - [func TeeReader(r Reader, w Writer) Reader](#TeeReader)
- [type ReaderAt](#ReaderAt)
- [type ReaderFrom](#ReaderFrom)
- [type RuneReader](#RuneReader)
- [type RuneScanner](#RuneScanner)
- [type SectionReader](#SectionReader)
  - [func NewSectionReader(r ReaderAt, off int64, n int64) *SectionReader](#NewSectionReader)
  - [func (s *SectionReader) Read(p []byte) (n int, err error)](#SectionReader.Read)
  - [func (s *SectionReader) ReadAt(p []byte, off int64) (n int, err error)](#SectionReader.ReadAt)
  - [func (s *SectionReader) Seek(offset int64, whence int) (int64, error)](#SectionReader.Seek)
  - [func (s *SectionReader) Size() int64](#SectionReader.Size)
- [type Seeker](#Seeker)
- [type WriteCloser](#WriteCloser)
- [type WriteSeeker](#WriteSeeker)
- [type Writer](#Writer)
  - [func MultiWriter(writers ...Writer) Writer](#MultiWriter)
- [type WriterAt](#WriterAt)
- [type WriterTo](#WriterTo)

### Examples

- [Copy](#exampleCopy)
- [CopyBuffer](#exampleCopyBuffer)
- [CopyN](#exampleCopyN)
- [LimitReader](#exampleLimitReader)
- [MultiReader](#exampleMultiReader)
- [MultiWriter](#exampleMultiWriter)
- [Pipe](#examplePipe)
- [ReadAtLeast](#exampleReadAtLeast)
- [ReadFull](#exampleReadFull)
- [SectionReader](#exampleSectionReader)
- [SectionReader.ReadAt](#exampleSectionReader_ReadAt)
- [SectionReader.Seek](#exampleSectionReader_Seek)
- [TeeReader](#exampleTeeReader)
- [WriteString](#exampleWriteString)

### Package files
 [io.go](//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go) [multi.go](//github.com/golang/go/blob/release-branch.go1.10/src/io/multi.go) [pipe.go](//github.com/golang/go/blob/release-branch.go1.10/src/io/pipe.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="SeekStart">SeekStart</span>   = 0 <span class="comment">// seek relative to the origin of the file</span>
    <span id="SeekCurrent">SeekCurrent</span> = 1 <span class="comment">// seek relative to the current offset</span>
    <span id="SeekEnd">SeekEnd</span>     = 2 <span class="comment">// seek relative to the end</span>
)</pre>

Seek whence values.

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="EOF">EOF</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;EOF&#34;)</pre>

EOF is the error returned by Read when no more input is available. Functions
should return EOF only to signal a graceful end of input. If the EOF occurs
unexpectedly in a structured data stream, the appropriate error is either
ErrUnexpectedEOF or some other error giving more detail.

<pre>var <span id="ErrClosedPipe">ErrClosedPipe</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;io: read/write on closed pipe&#34;)</pre>

ErrClosedPipe is the error used for read or write operations on a closed pipe.

<pre>var <span id="ErrNoProgress">ErrNoProgress</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;multiple Read calls return no data or error&#34;)</pre>

ErrNoProgress is returned by some clients of an io.Reader when many calls to
Read have failed to return any data or error, usually the sign of a broken
io.Reader implementation.

<pre>var <span id="ErrShortBuffer">ErrShortBuffer</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;short buffer&#34;)</pre>

ErrShortBuffer means that a read required a longer buffer than was provided.

<pre>var <span id="ErrShortWrite">ErrShortWrite</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;short write&#34;)</pre>

ErrShortWrite means that a write accepted fewer bytes than requested but failed
to return an explicit error.

<pre>var <span id="ErrUnexpectedEOF">ErrUnexpectedEOF</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;unexpected EOF&#34;)</pre>

ErrUnexpectedEOF means that EOF was encountered in the middle of reading a
fixed-size block or data structure.

<h2 id="Copy">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L351">Copy</a>
    <a href="#Copy">¶</a></h2>
<pre>func Copy(dst <a href="#Writer">Writer</a>, src <a href="#Reader">Reader</a>) (written <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

Copy copies from src to dst until either EOF is reached on src or an error
occurs. It returns the number of bytes copied and the first error encountered
while copying, if any.

A successful Copy returns err == nil, not err == EOF. Because Copy is defined to
read from src until EOF, it does not treat an EOF from Read as an error to be
reported.

If src implements the WriterTo interface, the copy is implemented by calling
src.WriteTo(dst). Otherwise, if dst implements the ReaderFrom interface, the
copy is implemented by calling dst.ReadFrom(src).

<a id="exampleCopy"></a>
Example:

    r := strings.NewReader("some io.Reader stream to be read\n")

    if _, err := io.Copy(os.Stdout, r); err != nil {
        log.Fatal(err)
    }

    // Output:
    // some io.Reader stream to be read

<h2 id="CopyBuffer">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L359">CopyBuffer</a>
    <a href="#CopyBuffer">¶</a></h2>
<pre>func CopyBuffer(dst <a href="#Writer">Writer</a>, src <a href="#Reader">Reader</a>, buf []<a href="/builtin/#byte">byte</a>) (written <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

CopyBuffer is identical to Copy except that it stages through the provided
buffer (if one is required) rather than allocating a temporary one. If buf is
nil, one is allocated; otherwise if it has zero length, CopyBuffer panics.

<a id="exampleCopyBuffer"></a>
Example:

    r1 := strings.NewReader("first reader\n")
    r2 := strings.NewReader("second reader\n")
    buf := make([]byte, 8)

    // buf is used here...
    if _, err := io.CopyBuffer(os.Stdout, r1, buf); err != nil {
        log.Fatal(err)
    }

    // ... reused here also. No need to allocate an extra buffer.
    if _, err := io.CopyBuffer(os.Stdout, r2, buf); err != nil {
        log.Fatal(err)
    }

    // Output:
    // first reader
    // second reader

<h2 id="CopyN">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L327">CopyN</a>
    <a href="#CopyN">¶</a></h2>
<pre>func CopyN(dst <a href="#Writer">Writer</a>, src <a href="#Reader">Reader</a>, n <a href="/builtin/#int64">int64</a>) (written <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

CopyN copies n bytes (or until an error) from src to dst. It returns the number
of bytes copied and the earliest error encountered while copying. On return,
written == n if and only if err == nil.

If dst implements the ReaderFrom interface, the copy is implemented using it.

<a id="exampleCopyN"></a>
Example:

    r := strings.NewReader("some io.Reader stream to be read")

    if _, err := io.CopyN(os.Stdout, r, 5); err != nil {
        log.Fatal(err)
    }

    // Output:
    // some

<h2 id="ReadAtLeast">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L293">ReadAtLeast</a>
    <a href="#ReadAtLeast">¶</a></h2>
<pre>func ReadAtLeast(r <a href="#Reader">Reader</a>, buf []<a href="/builtin/#byte">byte</a>, min <a href="/builtin/#int">int</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadAtLeast reads from r into buf until it has read at least min bytes. It
returns the number of bytes copied and an error if fewer bytes were read. The
error is EOF only if no bytes were read. If an EOF happens after reading fewer
than min bytes, ReadAtLeast returns ErrUnexpectedEOF. If min is greater than the
length of buf, ReadAtLeast returns ErrShortBuffer. On return, n >= min if and
only if err == nil.

<a id="exampleReadAtLeast"></a>
Example:

    r := strings.NewReader("some io.Reader stream to be read\n")

    buf := make([]byte, 33)
    if _, err := io.ReadAtLeast(r, buf, 4); err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%s\n", buf)

    // buffer smaller than minimal read size.
    shortBuf := make([]byte, 3)
    if _, err := io.ReadAtLeast(r, shortBuf, 4); err != nil {
        fmt.Println("error:", err)
    }

    // minimal read size bigger than io.Reader stream
    longBuf := make([]byte, 64)
    if _, err := io.ReadAtLeast(r, longBuf, 64); err != nil {
        fmt.Println("error:", err)
    }

    // Output:
    // some io.Reader stream to be read
    //
    // error: short buffer
    // error: EOF

<h2 id="ReadFull">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L316">ReadFull</a>
    <a href="#ReadFull">¶</a></h2>
<pre>func ReadFull(r <a href="#Reader">Reader</a>, buf []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadFull reads exactly len(buf) bytes from r into buf. It returns the number of
bytes copied and an error if fewer bytes were read. The error is EOF only if no
bytes were read. If an EOF happens after reading some but not all the bytes,
ReadFull returns ErrUnexpectedEOF. On return, n == len(buf) if and only if err
== nil.

<a id="exampleReadFull"></a>
Example:

    r := strings.NewReader("some io.Reader stream to be read\n")

    buf := make([]byte, 4)
    if _, err := io.ReadFull(r, buf); err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%s\n", buf)

    // minimal read size bigger than io.Reader stream
    longBuf := make([]byte, 64)
    if _, err := io.ReadFull(r, longBuf); err != nil {
        fmt.Println("error:", err)
    }

    // Output:
    // some
    // error: unexpected EOF

<h2 id="WriteString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L279">WriteString</a>
    <a href="#WriteString">¶</a></h2>
<pre>func WriteString(w <a href="#Writer">Writer</a>, s <a href="/builtin/#string">string</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteString writes the contents of the string s to w, which accepts a slice of
bytes. If w implements a WriteString method, it is invoked directly. Otherwise,
w.Write is called exactly once.

<a id="exampleWriteString"></a>
Example:

    io.WriteString(os.Stdout, "Hello World")

    // Output: Hello World

<h2 id="ByteReader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L229">ByteReader</a>
    <a href="#ByteReader">¶</a></h2>
<pre>type ByteReader interface {
    ReadByte() (<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)
}</pre>

ByteReader is the interface that wraps the ReadByte method.

ReadByte reads and returns the next byte from the input or any error
encountered. If ReadByte returns an error, no input byte was consumed, and the
returned byte value is undefined.

<h2 id="ByteScanner">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L240">ByteScanner</a>
    <a href="#ByteScanner">¶</a></h2>
<pre>type ByteScanner interface {
    <a href="#ByteReader">ByteReader</a>
    UnreadByte() <a href="/builtin/#error">error</a>
}</pre>

ByteScanner is the interface that adds the UnreadByte method to the basic
ReadByte method.

UnreadByte causes the next call to ReadByte to return the same byte as the
previous call to ReadByte. It may be an error to call UnreadByte twice without
an intervening call to ReadByte.

<h2 id="ByteWriter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L246">ByteWriter</a>
    <a href="#ByteWriter">¶</a></h2>
<pre>type ByteWriter interface {
    WriteByte(c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a>
}</pre>

ByteWriter is the interface that wraps the WriteByte method.

<h2 id="Closer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L88">Closer</a>
    <a href="#Closer">¶</a></h2>
<pre>type Closer interface {
    Close() <a href="/builtin/#error">error</a>
}</pre>

Closer is the interface that wraps the basic Close method.

The behavior of Close after the first call is undefined. Specific
implementations may document their own behavior.

<h2 id="LimitedReader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L424">LimitedReader</a>
    <a href="#LimitedReader">¶</a></h2>
<pre>type LimitedReader struct {
<span id="LimitedReader.R"></span>    R <a href="#Reader">Reader</a> <span class="comment">// underlying reader</span>
<span id="LimitedReader.N"></span>    N <a href="/builtin/#int64">int64</a>  <span class="comment">// max bytes remaining</span>
}</pre>

A LimitedReader reads from R but limits the amount of data returned to just N
bytes. Each call to Read updates N to reflect the new amount remaining. Read
returns EOF when N <= 0 or when the underlying R returns EOF.

<h3 id="LimitedReader.Read">func (*LimitedReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L429">Read</a>
    <a href="#LimitedReader.Read">¶</a></h3>
<pre>func (l *<a href="#LimitedReader">LimitedReader</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PipeReader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/pipe.go#L107">PipeReader</a>
    <a href="#PipeReader">¶</a></h2>
<pre>type PipeReader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A PipeReader is the read half of a pipe.

<h3 id="Pipe">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/pipe.go#L176">Pipe</a>
    <a href="#Pipe">¶</a></h3>
<pre>func Pipe() (*<a href="#PipeReader">PipeReader</a>, *<a href="#PipeWriter">PipeWriter</a>)</pre>

Pipe creates a synchronous in-memory pipe. It can be used to connect code
expecting an io.Reader with code expecting an io.Writer.

Reads and Writes on the pipe are matched one to one except when multiple Reads
are needed to consume a single Write. That is, each Write to the PipeWriter
blocks until it has satisfied one or more Reads from the PipeReader that fully
consume the written data. The data is copied directly from the Write to the
corresponding Read (or Reads); there is no internal buffering.

It is safe to call Read and Write in parallel with each other or with Close.
Parallel calls to Read and parallel calls to Write are also safe: the individual
calls will be gated sequentially.

<a id="examplePipe"></a>
Example:

    r, w := io.Pipe()

    go func() {
        fmt.Fprint(w, "some text to be read\n")
        w.Close()
    }()

    buf := new(bytes.Buffer)
    buf.ReadFrom(r)
    fmt.Print(buf.String())

    // Output:
    // some text to be read

<h3 id="PipeReader.Close">func (*PipeReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/pipe.go#L122">Close</a>
    <a href="#PipeReader.Close">¶</a></h3>
<pre>func (r *<a href="#PipeReader">PipeReader</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the reader; subsequent writes to the write half of the pipe will
return the error ErrClosedPipe.

<h3 id="PipeReader.CloseWithError">func (*PipeReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/pipe.go#L128">CloseWithError</a>
    <a href="#PipeReader.CloseWithError">¶</a></h3>
<pre>func (r *<a href="#PipeReader">PipeReader</a>) CloseWithError(err <a href="/builtin/#error">error</a>) <a href="/builtin/#error">error</a></pre>

CloseWithError closes the reader; subsequent writes to the write half of the
pipe will return the error err.

<h3 id="PipeReader.Read">func (*PipeReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/pipe.go#L116">Read</a>
    <a href="#PipeReader.Read">¶</a></h3>
<pre>func (r *<a href="#PipeReader">PipeReader</a>) Read(data []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read implements the standard Read interface: it reads data from the pipe,
blocking until a writer arrives or the write end is closed. If the write end is
closed with an error, that error is returned as err; otherwise err is EOF.

<h2 id="PipeWriter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/pipe.go#L133">PipeWriter</a>
    <a href="#PipeWriter">¶</a></h2>
<pre>type PipeWriter struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A PipeWriter is the write half of a pipe.

<h3 id="PipeWriter.Close">func (*PipeWriter) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/pipe.go#L148">Close</a>
    <a href="#PipeWriter.Close">¶</a></h3>
<pre>func (w *<a href="#PipeWriter">PipeWriter</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the writer; subsequent reads from the read half of the pipe will
return no bytes and EOF.

<h3 id="PipeWriter.CloseWithError">func (*PipeWriter) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/pipe.go#L157">CloseWithError</a>
    <a href="#PipeWriter.CloseWithError">¶</a></h3>
<pre>func (w *<a href="#PipeWriter">PipeWriter</a>) CloseWithError(err <a href="/builtin/#error">error</a>) <a href="/builtin/#error">error</a></pre>

CloseWithError closes the writer; subsequent reads from the read half of the
pipe will return no bytes and the error err, or EOF if err is nil.

CloseWithError always returns nil.

<h3 id="PipeWriter.Write">func (*PipeWriter) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/pipe.go#L142">Write</a>
    <a href="#PipeWriter.Write">¶</a></h3>
<pre>func (w *<a href="#PipeWriter">PipeWriter</a>) Write(data []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write implements the standard Write interface: it writes data to the pipe,
blocking until one or more readers have consumed all the data or the read end is
closed. If the read end is closed with an error, that err is returned as err;
otherwise err is ErrClosedPipe.

<h2 id="ReadCloser">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L116">ReadCloser</a>
    <a href="#ReadCloser">¶</a></h2>
<pre>type ReadCloser interface {
    <a href="#Reader">Reader</a>
    <a href="#Closer">Closer</a>
}</pre>

ReadCloser is the interface that groups the basic Read and Close methods.

<h2 id="ReadSeeker">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L135">ReadSeeker</a>
    <a href="#ReadSeeker">¶</a></h2>
<pre>type ReadSeeker interface {
    <a href="#Reader">Reader</a>
    <a href="#Seeker">Seeker</a>
}</pre>

ReadSeeker is the interface that groups the basic Read and Seek methods.

<h2 id="ReadWriteCloser">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L128">ReadWriteCloser</a>
    <a href="#ReadWriteCloser">¶</a></h2>
<pre>type ReadWriteCloser interface {
    <a href="#Reader">Reader</a>
    <a href="#Writer">Writer</a>
    <a href="#Closer">Closer</a>
}</pre>

ReadWriteCloser is the interface that groups the basic Read, Write and Close
methods.

<h2 id="ReadWriteSeeker">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L147">ReadWriteSeeker</a>
    <a href="#ReadWriteSeeker">¶</a></h2>
<pre>type ReadWriteSeeker interface {
    <a href="#Reader">Reader</a>
    <a href="#Writer">Writer</a>
    <a href="#Seeker">Seeker</a>
}</pre>

ReadWriteSeeker is the interface that groups the basic Read, Write and Seek
methods.

<h2 id="ReadWriter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L110">ReadWriter</a>
    <a href="#ReadWriter">¶</a></h2>
<pre>type ReadWriter interface {
    <a href="#Reader">Reader</a>
    <a href="#Writer">Writer</a>
}</pre>

ReadWriter is the interface that groups the basic Read and Write methods.

<h2 id="Reader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L67">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader interface {
    Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

Reader is the interface that wraps the basic Read method.

Read reads up to len(p) bytes into p. It returns the number of bytes read (0 <=
n <= len(p)) and any error encountered. Even if Read returns n < len(p), it may
use all of p as scratch space during the call. If some data is available but not
len(p) bytes, Read conventionally returns what is available instead of waiting
for more.

When Read encounters an error or end-of-file condition after successfully
reading n > 0 bytes, it returns the number of bytes read. It may return the
(non-nil) error from the same call or return the error (and n == 0) from a
subsequent call. An instance of this general case is that a Reader returning a
non-zero number of bytes at the end of the input stream may return either err ==
EOF or err == nil. The next Read should return 0, EOF.

Callers should always process the n > 0 bytes returned before considering the
error err. Doing so correctly handles I/O errors that happen after reading some
bytes and also both of the allowed EOF behaviors.

Implementations of Read are discouraged from returning a zero byte count with a
nil error, except when len(p) == 0. Callers should treat a return of 0 and nil
as indicating that nothing happened; in particular it does not indicate EOF.

Implementations must not retain p.

<h3 id="LimitReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L418">LimitReader</a>
    <a href="#LimitReader">¶</a></h3>
<pre>func LimitReader(r <a href="#Reader">Reader</a>, n <a href="/builtin/#int64">int64</a>) <a href="#Reader">Reader</a></pre>

LimitReader returns a Reader that reads from r but stops with EOF after n bytes.
The underlying implementation is a *LimitedReader.

<a id="exampleLimitReader"></a>
Example:

    r := strings.NewReader("some io.Reader stream to be read\n")
    lr := io.LimitReader(r, 4)

    if _, err := io.Copy(os.Stdout, lr); err != nil {
        log.Fatal(err)
    }

    // Output:
    // some

<h3 id="MultiReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/multi.go#L38">MultiReader</a>
    <a href="#MultiReader">¶</a></h3>
<pre>func MultiReader(readers ...<a href="#Reader">Reader</a>) <a href="#Reader">Reader</a></pre>

MultiReader returns a Reader that's the logical concatenation of the provided
input readers. They're read sequentially. Once all inputs have returned EOF,
Read will return EOF. If any of the readers return a non-nil, non-EOF error,
Read will return that error.

<a id="exampleMultiReader"></a>
Example:

    r1 := strings.NewReader("first reader ")
    r2 := strings.NewReader("second reader ")
    r3 := strings.NewReader("third reader\n")
    r := io.MultiReader(r1, r2, r3)

    if _, err := io.Copy(os.Stdout, r); err != nil {
        log.Fatal(err)
    }

    // Output:
    // first reader second reader third reader

<h3 id="TeeReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L513">TeeReader</a>
    <a href="#TeeReader">¶</a></h3>
<pre>func TeeReader(r <a href="#Reader">Reader</a>, w <a href="#Writer">Writer</a>) <a href="#Reader">Reader</a></pre>

TeeReader returns a Reader that writes to w what it reads from r. All reads from
r performed through it are matched with corresponding writes to w. There is no
internal buffering - the write must complete before the read completes. Any
error encountered while writing is reported as a read error.

<a id="exampleTeeReader"></a>
Example:

    r := strings.NewReader("some io.Reader stream to be read\n")
    var buf bytes.Buffer
    tee := io.TeeReader(r, &buf)

    printall := func(r io.Reader) {
        b, err := ioutil.ReadAll(r)
        if err != nil {
            log.Fatal(err)
        }

        fmt.Printf("%s", b)
    }

    printall(tee)
    printall(&buf)

    // Output:
    // some io.Reader stream to be read
    // some io.Reader stream to be read

<h2 id="ReaderAt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L201">ReaderAt</a>
    <a href="#ReaderAt">¶</a></h2>
<pre>type ReaderAt interface {
    ReadAt(p []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

ReaderAt is the interface that wraps the basic ReadAt method.

ReadAt reads len(p) bytes into p starting at offset off in the underlying input
source. It returns the number of bytes read (0 <= n <= len(p)) and any error
encountered.

When ReadAt returns n < len(p), it returns a non-nil error explaining why more
bytes were not returned. In this respect, ReadAt is stricter than Read.

Even if ReadAt returns n < len(p), it may use all of p as scratch space during
the call. If some data is available but not len(p) bytes, ReadAt blocks until
either all the data is available or an error occurs. In this respect ReadAt is
different from Read.

If the n = len(p) bytes returned by ReadAt are at the end of the input source,
ReadAt may return either err == EOF or err == nil.

If ReadAt is reading from an input source with a seek offset, ReadAt should not
affect nor be affected by the underlying seek offset.

Clients of ReadAt can execute parallel ReadAt calls on the same input source.

Implementations must not retain p.

<h2 id="ReaderFrom">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L160">ReaderFrom</a>
    <a href="#ReaderFrom">¶</a></h2>
<pre>type ReaderFrom interface {
    ReadFrom(r <a href="#Reader">Reader</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)
}</pre>

ReaderFrom is the interface that wraps the ReadFrom method.

ReadFrom reads data from r until EOF or error. The return value n is the number
of bytes read. Any error except io.EOF encountered during the read is also
returned.

The Copy function uses ReaderFrom if available.

<h2 id="RuneReader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L255">RuneReader</a>
    <a href="#RuneReader">¶</a></h2>
<pre>type RuneReader interface {
    ReadRune() (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

RuneReader is the interface that wraps the ReadRune method.

ReadRune reads a single UTF-8 encoded Unicode character and returns the rune and
its size in bytes. If no character is available, err will be set.

<h2 id="RuneScanner">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L266">RuneScanner</a>
    <a href="#RuneScanner">¶</a></h2>
<pre>type RuneScanner interface {
    <a href="#RuneReader">RuneReader</a>
    UnreadRune() <a href="/builtin/#error">error</a>
}</pre>

RuneScanner is the interface that adds the UnreadRune method to the basic
ReadRune method.

UnreadRune causes the next call to ReadRune to return the same rune as the
previous call to ReadRune. It may be an error to call UnreadRune twice without
an intervening call to ReadRune.

<h2 id="SectionReader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L449">SectionReader</a>
    <a href="#SectionReader">¶</a></h2>
<pre>type SectionReader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

SectionReader implements Read, Seek, and ReadAt on a section of an underlying
ReaderAt.

<a id="exampleSectionReader"></a>
Example:

    r := strings.NewReader("some io.Reader stream to be read\n")
    s := io.NewSectionReader(r, 5, 17)

    if _, err := io.Copy(os.Stdout, s); err != nil {
        log.Fatal(err)
    }

    // Output:
    // io.Reader stream

<h3 id="NewSectionReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L443">NewSectionReader</a>
    <a href="#NewSectionReader">¶</a></h3>
<pre>func NewSectionReader(r <a href="#ReaderAt">ReaderAt</a>, off <a href="/builtin/#int64">int64</a>, n <a href="/builtin/#int64">int64</a>) *<a href="#SectionReader">SectionReader</a></pre>

NewSectionReader returns a SectionReader that reads from r starting at offset
off and stops with EOF after n bytes.

<h3 id="SectionReader.Read">func (*SectionReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L456">Read</a>
    <a href="#SectionReader.Read">¶</a></h3>
<pre>func (s *<a href="#SectionReader">SectionReader</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="SectionReader.ReadAt">func (*SectionReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L489">ReadAt</a>
    <a href="#SectionReader.ReadAt">¶</a></h3>
<pre>func (s *<a href="#SectionReader">SectionReader</a>) ReadAt(p []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<a id="exampleSectionReader_ReadAt"></a>
Example:

    r := strings.NewReader("some io.Reader stream to be read\n")
    s := io.NewSectionReader(r, 5, 16)

    buf := make([]byte, 6)
    if _, err := s.ReadAt(buf, 10); err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s\n", buf)

    // Output:
    // stream

<h3 id="SectionReader.Seek">func (*SectionReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L471">Seek</a>
    <a href="#SectionReader.Seek">¶</a></h3>
<pre>func (s *<a href="#SectionReader">SectionReader</a>) Seek(offset <a href="/builtin/#int64">int64</a>, whence <a href="/builtin/#int">int</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>


<a id="exampleSectionReader_Seek"></a>
Example:

    r := strings.NewReader("some io.Reader stream to be read\n")
    s := io.NewSectionReader(r, 5, 16)

    if _, err := s.Seek(10, io.SeekStart); err != nil {
        log.Fatal(err)
    }

    buf := make([]byte, 6)
    if _, err := s.Read(buf); err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s\n", buf)

    // Output:
    // stream

<h3 id="SectionReader.Size">func (*SectionReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L506">Size</a>
    <a href="#SectionReader.Size">¶</a></h3>
<pre>func (s *<a href="#SectionReader">SectionReader</a>) Size() <a href="/builtin/#int64">int64</a></pre>

Size returns the size of the section in bytes.

<h2 id="Seeker">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L105">Seeker</a>
    <a href="#Seeker">¶</a></h2>
<pre>type Seeker interface {
    Seek(offset <a href="/builtin/#int64">int64</a>, whence <a href="/builtin/#int">int</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)
}</pre>

Seeker is the interface that wraps the basic Seek method.

Seek sets the offset for the next Read or Write to offset, interpreted according
to whence: SeekStart means relative to the start of the file, SeekCurrent means
relative to the current offset, and SeekEnd means relative to the end. Seek
returns the new offset relative to the start of the file and an error, if any.

Seeking to an offset before the start of the file is an error. Seeking to any
positive offset is legal, but the behavior of subsequent I/O operations on the
underlying object is implementation-dependent.

<h2 id="WriteCloser">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L122">WriteCloser</a>
    <a href="#WriteCloser">¶</a></h2>
<pre>type WriteCloser interface {
    <a href="#Writer">Writer</a>
    <a href="#Closer">Closer</a>
}</pre>

WriteCloser is the interface that groups the basic Write and Close methods.

<h2 id="WriteSeeker">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L141">WriteSeeker</a>
    <a href="#WriteSeeker">¶</a></h2>
<pre>type WriteSeeker interface {
    <a href="#Writer">Writer</a>
    <a href="#Seeker">Seeker</a>
}</pre>

WriteSeeker is the interface that groups the basic Write and Seek methods.

<h2 id="Writer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L80">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer interface {
    Write(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

Writer is the interface that wraps the basic Write method.

Write writes len(p) bytes from p to the underlying data stream. It returns the
number of bytes written from p (0 <= n <= len(p)) and any error encountered that
caused the write to stop early. Write must return a non-nil error if it returns
n < len(p). Write must not modify the slice data, even temporarily.

Implementations must not retain p.

<h3 id="MultiWriter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/multi.go#L92">MultiWriter</a>
    <a href="#MultiWriter">¶</a></h3>
<pre>func MultiWriter(writers ...<a href="#Writer">Writer</a>) <a href="#Writer">Writer</a></pre>

MultiWriter creates a writer that duplicates its writes to all the provided
writers, similar to the Unix tee(1) command.

Each write is written to each listed writer, one at a time. If a listed writer
returns an error, that overall write operation stops and returns the error; it
does not continue down the list.

<a id="exampleMultiWriter"></a>
Example:

    r := strings.NewReader("some io.Reader stream to be read\n")

    var buf1, buf2 bytes.Buffer
    w := io.MultiWriter(&buf1, &buf2)

    if _, err := io.Copy(w, r); err != nil {
        log.Fatal(err)
    }

    fmt.Print(buf1.String())
    fmt.Print(buf2.String())

    // Output:
    // some io.Reader stream to be read
    // some io.Reader stream to be read

<h2 id="WriterAt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L220">WriterAt</a>
    <a href="#WriterAt">¶</a></h2>
<pre>type WriterAt interface {
    WriteAt(p []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

WriterAt is the interface that wraps the basic WriteAt method.

WriteAt writes len(p) bytes from p to the underlying data stream at offset off.
It returns the number of bytes written from p (0 <= n <= len(p)) and any error
encountered that caused the write to stop early. WriteAt must return a non-nil
error if it returns n < len(p).

If WriteAt is writing to a destination with a seek offset, WriteAt should not
affect nor be affected by the underlying seek offset.

Clients of WriteAt can execute parallel WriteAt calls on the same destination if
the ranges do not overlap.

Implementations must not retain p.

<h2 id="WriterTo">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/io/io.go#L171">WriterTo</a>
    <a href="#WriterTo">¶</a></h2>
<pre>type WriterTo interface {
    WriteTo(w <a href="#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)
}</pre>

WriterTo is the interface that wraps the WriteTo method.

WriteTo writes data to w until there's no more data to write or when an error
occurs. The return value n is the number of bytes written. Any error encountered
during the write is also returned.

The Copy function uses WriterTo if available.

## Subdirectories
- [..](..)
- [ioutil](ioutil/)
