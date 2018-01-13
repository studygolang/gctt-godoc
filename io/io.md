version: 1.9.2
## package io

  `import "io"`

## 概述

io 包提供了基本的 I/O 原语接口。它主要用于将类似 os 包中的已有 I/O 原语实现封装进公共抽象接口中，还附加了一些其他相关原语。

因为这些接口和原语封装了多个底层实现，如果没有特殊的说明，用户不能认为是并发安全的。

## 索引

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

### 例子

- [Copy](#exampleCopy)
- [CopyBuffer](#exampleCopyBuffer)
- [CopyN](#exampleCopyN)
- [LimitReader](#exampleLimitReader)
- [MultiReader](#exampleMultiReader)
- [MultiWriter](#exampleMultiWriter)
- [ReadAtLeast](#exampleReadAtLeast)
- [ReadFull](#exampleReadFull)
- [SectionReader](#exampleSectionReader)
- [SectionReader.ReadAt](#exampleSectionReader_ReadAt)
- [SectionReader.Seek](#exampleSectionReader_Seek)
- [TeeReader](#exampleTeeReader)
- [WriteString](#exampleWriteString)

### 文件
 [io.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go) [multi.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/multi.go) [pipe.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/pipe.go)

<h2 id="pkg-constants">常量</h2>

<pre>const (
    <span id="SeekStart">SeekStart</span>   = 0 <span class="comment">// 找到文件开始的位置</span>
    <span id="SeekCurrent">SeekCurrent</span> = 1 <span class="comment">// 找到文件的当前位置</span>
    <span id="SeekEnd">SeekEnd</span>     = 2 <span class="comment">// 找到文件的结束位置</span>
)</pre>

表示从哪里开始查找的一系列常量。

<h2 id="pkg-variables">变量</h2>

<pre>var <span id="EOF">EOF</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;EOF&#34;)</pre>

当没有更多数据可以被读取的时候将会返回 EOF 错误。EOF 只应该在成功读取所有的输入数据后返回。如果在读取结构化数据流中意外的发生了 EOF，那么可以选择返回 ErrUnexpectedEOF 或者其他错误来提供更多的错误信息。

<pre>var <span id="ErrClosedPipe">ErrClosedPipe</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;io: read/write on closed pipe&#34;)</pre>

当我们对一个已关闭的 pipe 读或写，会返回 ErrClosedPipe。

<pre>var <span id="ErrNoProgress">ErrNoProgress</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;multiple Read calls return no data or error&#34;)</pre>

当用户多次调用 io.Reader，既不返回错误也不返回数据，返回 ErrNoProgress。这通常意味着 io.Reader 接口的实现因为某种原因导致崩溃。

<pre>var <span id="ErrShortBuffer">ErrShortBuffer</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;short buffer&#34;)</pre>

当读操作需要一个更大的缓存时，返回 ErrShortBuffer。

<pre>var <span id="ErrShortWrite">ErrShortWrite</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;short write&#34;)</pre>

当写入的字节少于最低需要的字符但是没有返回错误时，返回 ErrShortWrite。

<pre>var <span id="ErrUnexpectedEOF">ErrUnexpectedEOF</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;unexpected EOF&#34;)</pre>

当读取一个固定的数据块或者数据结构时遇到 EOF，返回 ErrUnexpectedEOF。

<h2 id="Copy">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L351">Copy</a>
    <a href="#Copy">¶</a></h2>
<pre>func Copy(dst <a href="#Writer">Writer</a>, src <a href="#Reader">Reader</a>) (written <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

Copy 方法将从 src 读取，直到遇见 EOF 或者发生错误，然后将读取到的数据写入 dst。它返回已经拷贝成功的字节数和第一个遇到的错误。

如果拷贝成功应该返回 err == nil, 而不是 err == EOF。因为拷贝的定义就是读取 src 直到遇见 EOF，它在读取到 EOF 时不应当将其视作一个错误。

如果 src 实现了 WriterTo 接口，拷贝操作将会调用 src.WriteTo(dst)。
如果 dst 实现了 ReaderFrom 接口，那么拷贝操作将会调用 dst.ReadFrom(src)。

<a id="exampleCopy"></a>
例:

    r := strings.NewReader("some io.Reader stream to be read\n")

    if _, err := io.Copy(os.Stdout, r); err != nil {
        log.Fatal(err)
    }

    // Output:
    // some io.Reader stream to be read

<h2 id="CopyBuffer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L359">CopyBuffer</a>
    <a href="#CopyBuffer">¶</a></h2>
<pre>func CopyBuffer(dst <a href="#Writer">Writer</a>, src <a href="#Reader">Reader</a>, buf []<a href="/builtin/#byte">byte</a>) (written <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

CopyBuffer 和 Copy 很相似，唯一的区别就是它使用用户提供的缓存进行拷贝，而不是使用临时的缓存。如果 buf 是 nil，那么将会为其分配一个缓存，如果缓存的长度为 0，那么 CopyBuffer 将会 panic。

<a id="exampleCopyBuffer"></a>
例:

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

<h2 id="CopyN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L327">CopyN</a>
    <a href="#CopyN">¶</a></h2>
<pre>func CopyN(dst <a href="#Writer">Writer</a>, src <a href="#Reader">Reader</a>, n <a href="/builtin/#int64">int64</a>) (written <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

CopyN 从 src 拷贝 n 字节到 dst，它返回拷贝的字节数和遇到的第一个错误。只当 err == nil 时，才会 n == written。

如果 dst 实现了 ReaderFrom 接口，那么拷贝将会使用 ReaderFrom 接口。

<a id="exampleCopyN"></a>
例:

    r := strings.NewReader("some io.Reader stream to be read")

    if _, err := io.CopyN(os.Stdout, r, 5); err != nil {
        log.Fatal(err)
    }

    // Output:
    // some

<h2 id="ReadAtLeast">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L293">ReadAtLeast</a>
    <a href="#ReadAtLeast">¶</a></h2>
<pre>func ReadAtLeast(r <a href="#Reader">Reader</a>, buf []<a href="/builtin/#byte">byte</a>, min <a href="/builtin/#int">int</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadAtLeast 从 r 至少读取 min 个字节到 buf 中。它返回成功拷贝的字节数，并且在字节数没有达到最小值时，返回一个错误。只有当没有字节可以读取的时候才返回 EOF。如果读取了小于 min 个字节后产生 EOF 错误。那么 ReadAtLeast 将返回 ErrUnexpectedEOF。如果 min 大于 buf 的长度，ReaderAtLeast 将返回 ErrShortBuffer。只有当 err == nil 的时候才会返回 n >= min。

<a id="exampleReadAtLeast"></a>
例:

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

<h2 id="ReadFull">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L316">ReadFull</a>
    <a href="#ReadFull">¶</a></h2>
<pre>func ReadFull(r <a href="#Reader">Reader</a>, buf []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadFull 从 r 读取 len(buf) 个字节到 buf。它返回拷贝的字节数，而当读取少于指定字节时，返回一个错误。只有没有字节可以读取的时候才会返回 EOF。如果一个 EOF 不是在读取最后一个字节后产生的，那么应该返回 ErrUnexpectedEOF。只有当 err == nil 的时候才会返回 n == len(buf)。

<a id="exampleReadFull"></a>
例:

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

<h2 id="WriteString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L279">WriteString</a>
    <a href="#WriteString">¶</a></h2>
<pre>func WriteString(w <a href="#Writer">Writer</a>, s <a href="/builtin/#string">string</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteString 会将字符串 s 写入 w。如果 w 实现了 WriteString 方法。它将会被直接调用，否则会调用一次 w.Write。

<a id="exampleWriteString"></a>
例:

    io.WriteString(os.Stdout, "Hello World")

    // Output: Hello World

<h2 id="ByteReader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L229">ByteReader</a>
    <a href="#ByteReader">¶</a></h2>
<pre>type ByteReader interface {
    ReadByte() (<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)
}</pre>

ByteReader 声明了 ReadByte 方法。

ReadByte 返回输入流中返回下一个字节和错误。如果 ReadByte 返回一个错误，输入流中的字符将不会被使用，并且返回字节的值也是未定义的。

<h2 id="ByteScanner">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L240">ByteScanner</a>
    <a href="#ByteScanner">¶</a></h2>
<pre>type ByteScanner interface {
    <a href="#ByteReader">ByteReader</a>
    UnreadByte() <a href="/builtin/#error">error</a>
}</pre>

ByteScanner 为 ReadByte 方法添加对应的 UnreadByte 方法。

UnreadByte 将会使下一次的 ReadByte 调用返回与之前调用相同的字节。如果在两个 ReadByte 之间调用 UnreadByte 两次有可能发生错误。

<h2 id="ByteWriter">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L246">ByteWriter</a>
    <a href="#ByteWriter">¶</a></h2>
<pre>type ByteWriter interface {
    WriteByte(c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a>
}</pre>

ByteWriter 接口声明了 WriteByte 方法。

<h2 id="Closer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L88">Closer</a>
    <a href="#Closer">¶</a></h2>
<pre>type Closer interface {
    Close() <a href="/builtin/#error">error</a>
}</pre>

Closer 接口声明了核心的 Close 方法。

如果先调用 Close 方法发生的行为是未定义的。具体实现可能会说明他们自己的函数行为。

<h2 id="LimitedReader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L416">LimitedReader</a>
    <a href="#LimitedReader">¶</a></h2>
<pre>type LimitedReader struct {
<span id="LimitedReader.R"></span>    R <a href="#Reader">Reader</a> <span class="comment">// underlying reader</span>
<span id="LimitedReader.N"></span>    N <a href="/builtin/#int64">int64</a>  <span class="comment">// max bytes remaining</span>
}</pre>

一个 LimitedReader 只从 R 中读取 N 字节。为了不断获取新的数据，每次读取都会更新 N 的值。当 N <= 0 时或者读取 R 时到了 EOF，函数会返回 EOF。

<h3 id="LimitedReader.Read">func (*LimitedReader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L421">Read</a>
    <a href="#LimitedReader.Read">¶</a></h3>
<pre>func (l *<a href="#LimitedReader">LimitedReader</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

<h2 id="PipeReader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/pipe.go#L110">PipeReader</a>
    <a href="#PipeReader">¶</a></h2>
<pre>type PipeReader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

PipeReader 是管道的读取端。

<h3 id="Pipe">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/pipe.go#L181">Pipe</a>
    <a href="#Pipe">¶</a></h3>
<pre>func Pipe() (*<a href="#PipeReader">PipeReader</a>, *<a href="#PipeWriter">PipeWriter</a>)</pre>

Pipe 创建一个同步的内存管道。它能够用来连接代码中需要 io.Reader 接口和 io.Wrtier 接口的部分。

在管道中，读取和写入是一一对应的，除了多个读取消耗单个写入的情况。所以写操作将会一直阻塞到有一个或多个读操作将写的内容全部接收。管道没有缓存，数据会直接从写入端拷贝到对应的读入端。

管道的读操作，写操作和关闭操作是可以并行的，并且并发调用读和写都是安全的，每个单独的调用都会顺序执行。

<h3 id="PipeReader.Close">func (*PipeReader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/pipe.go#L125">Close</a>
    <a href="#PipeReader.Close">¶</a></h3>
<pre>func (r *<a href="#PipeReader">PipeReader</a>) Close() <a href="/builtin/#error">error</a></pre>

Close 会关闭管道的读取端。随后的写入操作将会返回 ErrClosedPipe 错误。

<h3 id="PipeReader.CloseWithError">func (*PipeReader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/pipe.go#L131">CloseWithError</a>
    <a href="#PipeReader.CloseWithError">¶</a></h3>
<pre>func (r *<a href="#PipeReader">PipeReader</a>) CloseWithError(err <a href="/builtin/#error">error</a>) <a href="/builtin/#error">error</a></pre>

CloseWithError 会关闭读取端。写入端将会返回 err 错误。

<h3 id="PipeReader.Read">func (*PipeReader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/pipe.go#L119">Read</a>
    <a href="#PipeReader.Read">¶</a></h3>
<pre>func (r *<a href="#PipeReader">PipeReader</a>) Read(data []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read 方法实现了标准的 Read 接口：它会从管道中读取数据，并且阻塞到有新数据被写入或者写入端被关闭。如果写入端发生错误而关闭，那么这个错误将会被返回给调用者，否则将返回 EOF。

<h2 id="PipeWriter">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/pipe.go#L137">PipeWriter</a>
    <a href="#PipeWriter">¶</a></h2>
<pre>type PipeWriter struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

PipeWriter 是管道的写入端。

<h3 id="PipeWriter.Close">func (*PipeWriter) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/pipe.go#L152">Close</a>
    <a href="#PipeWriter.Close">¶</a></h3>
<pre>func (w *<a href="#PipeWriter">PipeWriter</a>) Close() <a href="/builtin/#error">error</a></pre>

Close 会关闭写入端。对管道的读操作将会返回 0 字节和 EOF。

<h3 id="PipeWriter.CloseWithError">func (*PipeWriter) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/pipe.go#L161">CloseWithError</a>
    <a href="#PipeWriter.CloseWithError">¶</a></h3>
<pre>func (w *<a href="#PipeWriter">PipeWriter</a>) CloseWithError(err <a href="/builtin/#error">error</a>) <a href="/builtin/#error">error</a></pre>

CloseWithError 会关闭写入端。如果设置了错误信息，那么管道的读取端随后的读操作将会获取到这个错误信息，如果错误信息是 nil。那么读取端将会返回 EOF。

CloseWithError 总是返回 nil。

<h3 id="PipeWriter.Write">func (*PipeWriter) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/pipe.go#L146">Write</a>
    <a href="#PipeWriter.Write">¶</a></h3>
<pre>func (w *<a href="#PipeWriter">PipeWriter</a>) Write(data []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write 方法实现了标准的 Write 接口：它将数据写入管道然后一直阻塞到一个或多个 reader 读取到所有的数据，或者读取端被关闭。如果读操作发生错误导致关闭，那么这个错误将会被返回给调用者。否则将会返回 ERRClosedPipe 错误。

<h2 id="ReadCloser">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L116">ReadCloser</a>
    <a href="#ReadCloser">¶</a></h2>
<pre>type ReadCloser interface {
    <a href="#Reader">Reader</a>
    <a href="#Closer">Closer</a>
}</pre>

ReadCloser 接口组合了 Read 和 Close 方法。

<h2 id="ReadSeeker">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L135">ReadSeeker</a>
    <a href="#ReadSeeker">¶</a></h2>
<pre>type ReadSeeker interface {
    <a href="#Reader">Reader</a>
    <a href="#Seeker">Seeker</a>
}</pre>

ReadSeeker 接口组合了 Read 和 Seek 方法。

<h2 id="ReadWriteCloser">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L128">ReadWriteCloser</a>
    <a href="#ReadWriteCloser">¶</a></h2>
<pre>type ReadWriteCloser interface {
    <a href="#Reader">Reader</a>
    <a href="#Writer">Writer</a>
    <a href="#Closer">Closer</a>
}</pre>

ReadWriteCloser 接口组合了 Read、Write 和 Close 方法。

<h2 id="ReadWriteSeeker">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L147">ReadWriteSeeker</a>
    <a href="#ReadWriteSeeker">¶</a></h2>
<pre>type ReadWriteSeeker interface {
    <a href="#Reader">Reader</a>
    <a href="#Writer">Writer</a>
    <a href="#Seeker">Seeker</a>
}</pre>

ReadWriteSeeker 接口组合了 Read、Write 和 Seek 方法。

<h2 id="ReadWriter">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L110">ReadWriter</a>
    <a href="#ReadWriter">¶</a></h2>
<pre>type ReadWriter interface {
    <a href="#Reader">Reader</a>
    <a href="#Writer">Writer</a>
}</pre>

ReadWriter 接口组合了 Read 和 Write 方法。

<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L67">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader interface {
    Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

Reader 接口声明了基本的 Read 方法。

Read 方法最多读取 len(p) 个字节并保存在切片 p 中。返回值有 2 个。n 代表成功读取的字节数，err 代表读取过程中发生的错误。即使读取操作返回的 n < len(p)，它也会将 p 全部用于暂存空间。如果成功读取的数据不够 len(p) 个字节，Read 方法会直接返回读取成功的数据，而不会阻塞等待更多数据。

当 Read 方法发生了错误或者成功读取数据并到达文件末尾的时候，他将返回读成功读取到的字节数。这时可以在此次调用 Read 时返回非 nil 的错误或者在下次调用 Read 的时候返回这个错误并且返回 n == 0。一个这种情况的例子：当 Reader 成功读取到数据并且到达文件末尾的时候，可以在本次调用时返回 err == nil 或者 err == EOF。下一次调用应该返回 0，EOF。

调用者应该优先处理 n > 0 时的错误。这样做可以正确的处理那些读取到一半发生的错误，和 2 个允许返回 EOF 的情况。

Read 的实现不推荐同时返回 0 和 err == nil，除了 len(p) == 0 的情况。调用者应该把返回值为 0 和 nil 的情况看作什么也没发生，而不是 EOF。

Reader 的实现不能保存 p。

<h3 id="LimitReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L410">LimitReader</a>
    <a href="#LimitReader">¶</a></h3>
<pre>func LimitReader(r <a href="#Reader">Reader</a>, n <a href="/builtin/#int64">int64</a>) <a href="#Reader">Reader</a></pre>

LimitReader 返回一个从 r 中读取 n 个字符后返回 EOF 的 Reader。
底层的实现是 *LimitedReader。

<a id="exampleLimitReader"></a>
例:

    r := strings.NewReader("some io.Reader stream to be read\n")
    lr := io.LimitReader(r, 4)

    if _, err := io.Copy(os.Stdout, lr); err != nil {
        log.Fatal(err)
    }

    // Output:
    // some

<h3 id="MultiReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/multi.go#L38">MultiReader</a>
    <a href="#MultiReader">¶</a></h3>
<pre>func MultiReader(readers ...<a href="#Reader">Reader</a>) <a href="#Reader">Reader</a></pre>

MultiReader 返回一个逻辑上连接多个 Reader 的 Reader。它们会依次被读取。只有在所有的输入端全部返回 EOF 的时候，Read 才会返回 EOF。如果在这当中任何一个 Reader 返回了一个非 nil，非 EOF 的错误，那么这个错误都将被返回。

<a id="exampleMultiReader"></a>
例:

    r1 := strings.NewReader("first reader ")
    r2 := strings.NewReader("second reader ")
    r3 := strings.NewReader("third reader\n")
    r := io.MultiReader(r1, r2, r3)

    if _, err := io.Copy(os.Stdout, r); err != nil {
        log.Fatal(err)
    }

    // Output:
    // first reader second reader third reader

<h3 id="TeeReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L505">TeeReader</a>
    <a href="#TeeReader">¶</a></h3>
<pre>func TeeReader(r <a href="#Reader">Reader</a>, w <a href="#Writer">Writer</a>) <a href="#Reader">Reader</a></pre>

TeeReader 返回一个将所有从 r 中读取到的数据全部写入进 w 中的 Reader。
所有对 r 的读取操作都会对应将数据写入 w 的操作。它的内部没有缓冲机制，使用读取操作时必须等待内部写入 w 的操作完成后才能返回，所有写操作时的错误都会被作为读取操作的错误返回。

<a id="exampleTeeReader"></a>
例:

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

<h2 id="ReaderAt">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L201">ReaderAt</a>
    <a href="#ReaderAt">¶</a></h2>
<pre>type ReaderAt interface {
    ReadAt(p []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

ReaderAt 接口声明了 ReadAt 方法。

ReadAt 从输入源根据 off 指定的偏移量开始将 len(p) 字节读入 p。他会返回成功读取的字节数(0 <= n <= len(p))和读取过程中发生的任何错误。

当 ReadAt 返回 n < len(p) 的时候，将会返回一个 非 nil 的错误来解释为什么其他的字节没有被读取。在这方面 ReadAt 比 Read 更加严格。

即使 ReadAt 返回了 n < len(p)，它也可能会将 p 的所有元素用作暂存空间。如果成功读取的数据不能填满 p，那么 ReadAt 函数将会一直阻塞直到 p 被填满或者发生错误。在这方面 ReadAt 和 Read 是不同的。

如果 ReadAt 在读取 n = len(p) 后正好到达输入数据的末尾，ReadAt 可以选择返回 err == EOF 或者 err == nil 都可以。

如果 ReadAt 在一个已经拥有偏移量的输入数据上读取，那么 ReadAt 不能影响数据源原本的偏移量。

ReadAt 对于同一个输入资源的并发调用也是安全的。

ReadAt 的实现不能保存 p。

<h2 id="ReaderFrom">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L160">ReaderFrom</a>
    <a href="#ReaderFrom">¶</a></h2>
<pre>type ReaderFrom interface {
    ReadFrom(r <a href="#Reader">Reader</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)
}</pre>

ReaderFrom 接口声明了 ReadFrom 方法。

ReadFrom 从 r 中读取数据直到读取到 EOF 或者发生的错误。
返回值 n 是成功读取到的字节数。除了 io.EOF 任何读取时发生的错误都会被返回。

如果 Copy 的 dst 实现了 ReaderFrom 接口。那么 ReadFrom 会被 Copy 函数调用。

<h2 id="RuneReader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L255">RuneReader</a>
    <a href="#RuneReader">¶</a></h2>
<pre>type RuneReader interface {
    ReadRune() (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

RuneReader 接口声明了 ReadRune 方法。

ReadRune 会读取一个 UTF-8 编码的字符并且返回这个字符和他的字节数。如果没有合法的字符，将会返回错误。

<h2 id="RuneScanner">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L266">RuneScanner</a>
    <a href="#RuneScanner">¶</a></h2>
<pre>type RuneScanner interface {
    <a href="#RuneReader">RuneReader</a>
    UnreadRune() <a href="/builtin/#error">error</a>
}</pre>

RunScanner 接口为 ReadRune 方法添加对应的 UnreadRune 方法。

UnreadRune 会让下一次调用 ReadRune 和之前返回相同的值。如果在 ReadRune 调用的间隔中连续调用 2 次 UnreadRune 可能会发生错误。

<h2 id="SectionReader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L441">SectionReader</a>
    <a href="#SectionReader">¶</a></h2>
<pre>type SectionReader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

SectionReader 在 ReaderAt 获取到的数据片段上实现了 Read、Seek 和 ReadAt 接口。

<a id="exampleSectionReader"></a>
例:

    r := strings.NewReader("some io.Reader stream to be read\n")
    s := io.NewSectionReader(r, 5, 17)

    if _, err := io.Copy(os.Stdout, s); err != nil {
        log.Fatal(err)
    }

    // Output:
    // io.Reader stream

<h3 id="NewSectionReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L435">NewSectionReader</a>
    <a href="#NewSectionReader">¶</a></h3>
<pre>func NewSectionReader(r <a href="#ReaderAt">ReaderAt</a>, off <a href="/builtin/#int64">int64</a>, n <a href="/builtin/#int64">int64</a>) *<a href="#SectionReader">SectionReader</a></pre>

NewSectionReader 返回一个 SectionReader，它能读取 r 中以 off 作为偏移量以后的 n 个字节的数据，返回的 Reader 在读取完第 n 个字节后返回 EOF。

<h3 id="SectionReader.Read">func (*SectionReader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L448">Read</a>
    <a href="#SectionReader.Read">¶</a></h3>
<pre>func (s *<a href="#SectionReader">SectionReader</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

<h3 id="SectionReader.ReadAt">func (*SectionReader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L481">ReadAt</a>
    <a href="#SectionReader.ReadAt">¶</a></h3>
<pre>func (s *<a href="#SectionReader">SectionReader</a>) ReadAt(p []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

<a id="exampleSectionReader_ReadAt"></a>
例:

    r := strings.NewReader("some io.Reader stream to be read\n")
    s := io.NewSectionReader(r, 5, 16)

    buf := make([]byte, 6)
    if _, err := s.ReadAt(buf, 10); err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s\n", buf)

    // Output:
    // stream

<h3 id="SectionReader.Seek">func (*SectionReader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L463">Seek</a>
    <a href="#SectionReader.Seek">¶</a></h3>
<pre>func (s *<a href="#SectionReader">SectionReader</a>) Seek(offset <a href="/builtin/#int64">int64</a>, whence <a href="/builtin/#int">int</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>

<a id="exampleSectionReader_Seek"></a>
例:

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

<h3 id="SectionReader.Size">func (*SectionReader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L498">Size</a>
    <a href="#SectionReader.Size">¶</a></h3>
<pre>func (s *<a href="#SectionReader">SectionReader</a>) Size() <a href="/builtin/#int64">int64</a></pre>

Size 返回 SectionReader 中的字节数。

<h2 id="Seeker">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L105">Seeker</a>
    <a href="#Seeker">¶</a></h2>
<pre>type Seeker interface {
    Seek(offset <a href="/builtin/#int64">int64</a>, whence <a href="/builtin/#int">int</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)
}</pre>

Seeker 接口声明了 Seek 方法。

Seek 方法设置了下一次读或写的偏移位置。根据 whence 参数的 3 种情况：

- SeekStart 表示从文件开始处进行偏移。
- SeekCurrent 表示从当前位置进行偏移。
- SeekEnd 表示从文件末尾进行偏移。

Seek 返回相对于文件开始处的偏移量，如果发生了错误，也会返回错误。

偏移到超前于文件开始处的位置将会返回一个错误。一个正向偏移永远是合法的，但是在随后的 I/O 操作的行为依赖于具体实现。

<h2 id="WriteCloser">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L122">WriteCloser</a>
    <a href="#WriteCloser">¶</a></h2>
<pre>type WriteCloser interface {
    <a href="#Writer">Writer</a>
    <a href="#Closer">Closer</a>
}</pre>

WriteCloser 接口组合了 Write 和 Close 方法。

<h2 id="WriteSeeker">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L141">WriteSeeker</a>
    <a href="#WriteSeeker">¶</a></h2>
<pre>type WriteSeeker interface {
    <a href="#Writer">Writer</a>
    <a href="#Seeker">Seeker</a>
}</pre>

WriteSeeker 接口组合了 Write 和 Seek 方法。

<h2 id="Writer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L80">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer interface {
    Write(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

Writer 接口声明了 Write 方法。

Write 方法将 p 中的所有元素写入底层数据流。并且返回成功写入 p 中的字节数和在写入过程中引起写入失败的错误。如果 Write 方法返回的 n < len(p)，那么它必须返回一个非 nil 的错误。Write 方法不能修改切片中的数据，即使这个切片中的数据是暂时的。

实现不能保留切片 p。

<h3 id="MultiWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/multi.go#L88">MultiWriter</a>
    <a href="#MultiWriter">¶</a></h3>
<pre>func MultiWriter(writers ...<a href="#Writer">Writer</a>) <a href="#Writer">Writer</a></pre>

MultiWriter 会创建一个将写入的字符复制给每个 Writer 的 Wrtier。与 Unix 下的 tee(1) 命令相似。

<a id="exampleMultiWriter"></a>
例:

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

<h2 id="WriterAt">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L220">WriterAt</a>
    <a href="#WriterAt">¶</a></h2>
<pre>type WriterAt interface {
    WriteAt(p []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

WriteAt 接口声明了基本的 WriteAt 方法。

WriteAt 将 p 中的 len(p) 个字节写入到数据流偏移量 off 的位置。它返回从 p 中成功写入的字节数 (0 <= n <= len(p)) 和写入过程中发生的任何使写入过早中断的错误。WriteAt 在返回的 n < len(p) 时必须返回一个非 nil 的错误。

如果 WriteAt 执行在一个带有偏移量的数据流上，WriteAt 不应该影响底层数据流原本的偏移量。

只要写入的区域没有重叠，我们就可以并发的调用 WriteTo 方法。

所有该接口的实现都不能保留 p。

<h2 id="WriterTo">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/io.go#L171">WriterTo</a>
    <a href="#WriterTo">¶</a></h2>
<pre>type WriterTo interface {
    WriteTo(w <a href="#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)
}</pre>

WriterTo 接口声明了 WriteTo 方法。

WriterTo 会将所有数据写入 w 或者当写入发生错误时中断操作。返回值 n 代表写入的字节数。err 代表在写入过程中发生的错误。

如果Copy 函数的 src 实现了WriterTo方法，那么在 Copy 函数中将会使用 WriterTo 接口。

## Subdirectories
- [..](..)
- [ioutil](ioutil/)
