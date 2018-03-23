version: 1.10
## package bufio

  `import "bufio"`

## 概述

bufio 包实现了带缓存的 I/O。它会封装 io.Reader 或 io.Writer 对象，并实现带缓存的（Reader 或 Writer）接口用来更好的支持文本 I/O。

## 索引

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func ScanBytes(data []byte, atEOF bool) (advance int, token []byte, err error)](#ScanBytes)
- [func ScanLines(data []byte, atEOF bool) (advance int, token []byte, err error)](#ScanLines)
- [func ScanRunes(data []byte, atEOF bool) (advance int, token []byte, err error)](#ScanRunes)
- [func ScanWords(data []byte, atEOF bool) (advance int, token []byte, err error)](#ScanWords)
- [type ReadWriter](#ReadWriter)
  - [func NewReadWriter(r *Reader, w *Writer) *ReadWriter](#NewReadWriter)
- [type Reader](#Reader)
  - [func NewReader(rd io.Reader) *Reader](#NewReader)
  - [func NewReaderSize(rd io.Reader, size int) *Reader](#NewReaderSize)
  - [func (b *Reader) Buffered() int](#Reader.Buffered)
  - [func (b *Reader) Discard(n int) (discarded int, err error)](#Reader.Discard)
  - [func (b *Reader) Peek(n int) ([]byte, error)](#Reader.Peek)
  - [func (b *Reader) Read(p []byte) (n int, err error)](#Reader.Read)
  - [func (b *Reader) ReadByte() (byte, error)](#Reader.ReadByte)
  - [func (b *Reader) ReadBytes(delim byte) ([]byte, error)](#Reader.ReadBytes)
  - [func (b *Reader) ReadLine() (line []byte, isPrefix bool, err error)](#Reader.ReadLine)
  - [func (b *Reader) ReadRune() (r rune, size int, err error)](#Reader.ReadRune)
  - [func (b *Reader) ReadSlice(delim byte) (line []byte, err error)](#Reader.ReadSlice)
  - [func (b *Reader) ReadString(delim byte) (string, error)](#Reader.ReadString)
  - [func (b *Reader) Reset(r io.Reader)](#Reader.Reset)
  - [func (r *Reader) Size() int](#Reader.Size)
  - [func (b *Reader) UnreadByte() error](#Reader.UnreadByte)
  - [func (b *Reader) UnreadRune() error](#Reader.UnreadRune)
  - [func (b *Reader) WriteTo(w io.Writer) (n int64, err error)](#Reader.WriteTo)
- [type Scanner](#Scanner)
  - [func NewScanner(r io.Reader) *Scanner](#NewScanner)
  - [func (s *Scanner) Buffer(buf []byte, max int)](#Scanner.Buffer)
  - [func (s *Scanner) Bytes() []byte](#Scanner.Bytes)
  - [func (s *Scanner) Err() error](#Scanner.Err)
  - [func (s *Scanner) Scan() bool](#Scanner.Scan)
  - [func (s *Scanner) Split(split SplitFunc)](#Scanner.Split)
  - [func (s *Scanner) Text() string](#Scanner.Text)
- [type SplitFunc](#SplitFunc)
- [type Writer](#Writer)
  - [func NewWriter(w io.Writer) *Writer](#NewWriter)
  - [func NewWriterSize(w io.Writer, size int) *Writer](#NewWriterSize)
  - [func (b *Writer) Available() int](#Writer.Available)
  - [func (b *Writer) Buffered() int](#Writer.Buffered)
  - [func (b *Writer) Flush() error](#Writer.Flush)
  - [func (b *Writer) ReadFrom(r io.Reader) (n int64, err error)](#Writer.ReadFrom)
  - [func (b *Writer) Reset(w io.Writer)](#Writer.Reset)
  - [func (b *Writer) Size() int](#Writer.Size)
  - [func (b *Writer) Write(p []byte) (nn int, err error)](#Writer.Write)
  - [func (b *Writer) WriteByte(c byte) error](#Writer.WriteByte)
  - [func (b *Writer) WriteRune(r rune) (size int, err error)](#Writer.WriteRune)
  - [func (b *Writer) WriteString(s string) (int, error)](#Writer.WriteString)

### 例子

- [Scanner (Custom)](#exampleScanner_custom)
- [Scanner (EmptyFinalToken)](#exampleScanner_emptyFinalToken)
- [Scanner (Lines)](#exampleScanner_lines)
- [Scanner (Words)](#exampleScanner_words)
- [Writer](#exampleWriter)

### 文件

 [bufio.go](//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go) [scan.go](//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// 如果用户不用 Scan.Buffer 显式提供缓存，那么默认使用 MaxScanTokenSize 作为缓存一块数据所允许的最大尺寸。</span>
    <span class="comment">// 实际数据的大小有可能小于所需占用的缓存大小（例如：一个新行）。</span>
    <span id="MaxScanTokenSize">MaxScanTokenSize</span> = 64 * 1024
)</pre>


<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="ErrInvalidUnreadByte">ErrInvalidUnreadByte</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;bufio: invalid use of UnreadByte&#34;)
    <span id="ErrInvalidUnreadRune">ErrInvalidUnreadRune</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;bufio: invalid use of UnreadRune&#34;)
    <span id="ErrBufferFull">ErrBufferFull</span>        = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;bufio: buffer full&#34;)
    <span id="ErrNegativeCount">ErrNegativeCount</span>     = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;bufio: negative count&#34;)
)</pre>


<pre>var (
    <span id="ErrTooLong">ErrTooLong</span>         = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;bufio.Scanner: token too long&#34;)
    <span id="ErrNegativeAdvance">ErrNegativeAdvance</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;bufio.Scanner: SplitFunc returns negative advance count&#34;)
    <span id="ErrAdvanceTooFar">ErrAdvanceTooFar</span>   = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;bufio.Scanner: SplitFunc returns advance count beyond input&#34;)
)</pre>

Scanner 可能返回的错误。

<pre>var <span id="ErrFinalToken">ErrFinalToken</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;final token&#34;)</pre>

ErrFinalToken 是一个特殊的错误。切割函数用它来表示切割出的最后一块数据，在这之后可以停止获取新数据。在 Scan 获取到 ErrFinalToken 之后，会停止获取数据但不会返回错误。它可以用来让进程提前结束或发送最后一块空数据。你也可以使用自定义错误来完成上述逻辑，这里提供出来只是为了让代码更加整洁。在 emptyFinalToken 中你会看到使用案例。

<h2 id="ScanBytes">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L269">ScanBytes</a>
    <a href="#ScanBytes">¶</a></h2>
<pre>func ScanBytes(data []<a href="/builtin/#byte">byte</a>, atEOF <a href="/builtin/#bool">bool</a>) (advance <a href="/builtin/#int">int</a>, token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ScanBytes 是 Scanner 的切割函数，它会将数据中的每个字节作为数据块返回。

<h2 id="ScanLines">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L330">ScanLines</a>
    <a href="#ScanLines">¶</a></h2>
<pre>func ScanLines(data []<a href="/builtin/#byte">byte</a>, atEOF <a href="/builtin/#bool">bool</a>) (advance <a href="/builtin/#int">int</a>, token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ScanLines  是 Scanner 的切割函数，它会返回文本中的每一行（不包含尾部标记）。返回的行有可能是空的。行末的标记可以是换行符中的一种，一般为 `\r?\n`，最后一个非空行即使没有换行符也会返回。

<h2 id="ScanRunes">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L284">ScanRunes</a>
    <a href="#ScanRunes">¶</a></h2>
<pre>func ScanRunes(data []<a href="/builtin/#byte">byte</a>, atEOF <a href="/builtin/#bool">bool</a>) (advance <a href="/builtin/#int">int</a>, token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ScanRunes  是 Scanner 的切割函数，它返回每个 UTF-8 编码的 rune。返回的 rune 序列和循环

字符串得到的序列是相同的，其中非 UTF-8 内容会被翻译成 `U+FFFD = "\xef\xbf\xbd"`。

由于 Scan 接口的原因，用户不能区分出返回的 rune 是有效还是无效的 UTF-8 字符。

<h2 id="ScanWords">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L375">ScanWords</a>
    <a href="#ScanWords">¶</a></h2>
<pre>func ScanWords(data []<a href="/builtin/#byte">byte</a>, atEOF <a href="/builtin/#bool">bool</a>) (advance <a href="/builtin/#int">int</a>, token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ScanWords 是 Scanner 的切割函数，它返回文本中每个被空白符分割的单词（空白符会被删除）。

它不会返回空字符串，空白符由 unicode.IsSpace 定义。

<h2 id="ReadWriter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L723">ReadWriter</a>
    <a href="#ReadWriter">¶</a></h2>
<pre>type ReadWriter struct {
    *<a href="#Reader">Reader</a>
    *<a href="#Writer">Writer</a>
}</pre>

ReadWriter 保存 Reader 和 Writer 的指针。它实现了 io.ReadWriter。

<h3 id="NewReadWriter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L729">NewReadWriter</a>
    <a href="#NewReadWriter">¶</a></h3>
<pre>func NewReadWriter(r *<a href="#Reader">Reader</a>, w *<a href="#Writer">Writer</a>) *<a href="#ReadWriter">ReadWriter</a></pre>

NewReadWriter 会创建一个新的 ReadWriter，它与 r 和 w 传递数据。

<h2 id="Reader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L21">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <span class="comment">// 包含过滤或非导出字段</span>
}</pre>

Reader 实现了对 io.Reader 对象的带缓存读取操作。

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L51">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(rd <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Reader">Reader</a></pre>

NewReader 创建一个默认缓存大小的 Reader。

<h3 id="NewReaderSize">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L36">NewReaderSize</a>
    <a href="#NewReaderSize">¶</a></h3>
<pre>func NewReaderSize(rd <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, size <a href="/builtin/#int">int</a>) *<a href="#Reader">Reader</a></pre>

NewReaderSize 创建一个缓存大小为 size 的 Reader。如果 io.Reader 已经有足够大的缓存会直接返回它。

<h3 id="Reader.Buffered">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L294">Buffered</a>
    <a href="#Reader.Buffered">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) Buffered() <a href="/builtin/#int">int</a></pre>

Buffered 返回当前缓存中能够读取到的字节数。

<h3 id="Reader.Discard">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L147">Discard</a>
    <a href="#Reader.Discard">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) Discard(n <a href="/builtin/#int">int</a>) (discarded <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Discard 会丢弃接下来的 n 字节，并返回丢弃的字节数量。

如果 Discard 丢弃少于 n 字节，他还会返回一个错误。如果 0<=n<=b.Buffered()，说明 Discard

执行成功，并且没有从底层 io.Reader 读取数据。

<h3 id="Reader.Peek">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L116">Peek</a>
    <a href="#Reader.Peek">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) Peek(n <a href="/builtin/#int">int</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Peek 会返回接下来的 n 字节但不会向前移动。在下次读取时还能读取到这些字节。如果 Peek 返回少于 n 字节。

它还会返回一个错误解释原因。如果 n 大于 b 的缓存大小会返回 ErrBufferFull。

<h3 id="Reader.Read">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L180">Read</a>
    <a href="#Reader.Read">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read 将数据读入 p 中。它返回读入 p 中数据的字节数。最多调用一次底层 Reader 来获取数据，因此

n 有可能小于 len(p)。如果读到 EOF，n 会为 0 并且返回 io.EOF 错误。

<h3 id="Reader.ReadByte">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L226">ReadByte</a>
    <a href="#Reader.ReadByte">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadByte() (<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadByte 读取一个字节并返回。如果没有字节能够读取，会返回错误。

<h3 id="Reader.ReadBytes">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L401">ReadBytes</a>
    <a href="#Reader.ReadBytes">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadBytes(delim <a href="/builtin/#byte">byte</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadBytes 读取到分割字节 delim 的字节集合，它返回包括分割字节 delim 在内的数据切片。

如果 ReadBytes 在找到分割字节前发生错，它会返回当前获取到的数据和遇到的错误（一般都是 io.EOF）。

ReadBytes 只在返回的数据不以分割符结尾时返回错误。一般情况下 Scanner 会更加方便使用。

<h3 id="Reader.ReadLine">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L359">ReadLine</a>
    <a href="#Reader.ReadLine">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadLine() (line []<a href="/builtin/#byte">byte</a>, isPrefix <a href="/builtin/#bool">bool</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadLine 是一个比较原始的底层方法，大多数用户因该使用 ReadBytes('\n') 或

ReadString('\n') 或 Scanner。

ReadLine 会返回单独一行，其中不包括换行符。如果一行的长度大于缓存大小，就会设置 isPrefix 并返回数据。行内其他内容会在接下来的调用中返回。isPrefix 会在最后一段行数据返回以后变成 false。返回的切片只在下次 ReadLine 调用前有效。ReadLine 返回的 line 和 err 不会同时为非 nil。

ReadLine 返回的内容不包括换行符（"\r\n" 或 "\n"）。如果输入不以换行符结尾也不会报错。在 ReadLine 之后调用 UnreadByte 会返回到最后读取到的字节位置（即使在 ReadLine 中并没有返回不过也可能是换行符）。

<h3 id="Reader.ReadRune">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L261">ReadRune</a>
    <a href="#Reader.ReadRune">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadRune() (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadRune 读取一个 UTF-8 字符并返回这个字符和字符占用的字节数。如果是无效的 UTF-8 字符，会被当作是一个单字节字符并返回 unicode.ReplacementChar 和 1。

<h3 id="Reader.ReadSlice">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L306">ReadSlice</a>
    <a href="#Reader.ReadSlice">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadSlice(delim <a href="/builtin/#byte">byte</a>) (line []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadSlice 从输入中读取到 delim 字节的内容，并返回缓存中相应内容的字节切片。切片会在下次调用读取函数后失效。如果 ReadSlice 在找到 delim 字节前发生了错误，它会返回从缓存中读取到的数据和相应错误（一般是 io.EOF）。

如果整个缓存中都有没有 delim 字节，那么 ReadSlice 会返回 ErrBufferFull。因为下次读取数据会把缓存中的内容覆盖掉，这时大多数用户应该使用 ReadBytes 或 ReadString。ReadSlice 在 line 不是以 delim 字节结尾时返回错误。

<h3 id="Reader.ReadString">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L448">ReadString</a>
    <a href="#Reader.ReadString">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadString(delim <a href="/builtin/#byte">byte</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ReadString 从输入中读取到 delim 字节的内容，返回包括 delim 字节在内的字符串。如果 ReadString 在找到 delim 字节前发生了错误，它会返回已经读取到的数据和错误信息。ReadString 只在读取到的数据不以 delim 字节结尾时返回错误信息。一般情况下 Scanner 更加方便。

<h3 id="Reader.Reset">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L60">Reset</a>
    <a href="#Reader.Reset">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) Reset(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>)</pre>

Reset 丢弃缓存中的数据，重置所有状态并准备从 r 读取数据。

<h3 id="Reader.Size">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L56">Size</a>
    <a href="#Reader.Size">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Size() <a href="/builtin/#int">int</a></pre>

Size 返回底层缓存的字节数。

<h3 id="Reader.UnreadByte">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L241">UnreadByte</a>
    <a href="#Reader.UnreadByte">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) UnreadByte() <a href="/builtin/#error">error</a></pre>

UnreadByte 重新读取上一个字节，只能回退最近读入的字节。

<h3 id="Reader.UnreadRune">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L283">UnreadRune</a>
    <a href="#Reader.UnreadRune">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) UnreadRune() <a href="/builtin/#error">error</a></pre>

UnreadRune 重新读取上一个 rune。如果缓存中最近的读取操作不是 ReadRune，

UnreadRune 会返回错误（在这点上它比 UnreadByte 更加严格）。

<h3 id="Reader.WriteTo">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L455">WriteTo</a>
    <a href="#Reader.WriteTo">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteTo 实现了 WriteTo 接口。它可能多次调用底层 Reader 的 Read 方法。

<h2 id="Scanner">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L20">Scanner</a>
    <a href="#Scanner">¶</a></h2>
<pre>type Scanner struct {
    <span class="comment">// 包含过滤或非导出字段</span>
}</pre>

Scanner 提供了一个方便读取数据的接口，像以换行符分割数据。每次成功调用 Scan 方法都会获取数据的一段内容（不包括每段内容之间的字节）。它使用类型为 SplitFunc 的函数来分割数据内容；默认的分割函数是以换行符分割内容。本包内包含的分割方法包括分割行，分割字节，分割 UTF-8 字符和分割空白字符串。用户可以提供自己的分割函数来替换它们。

数据读取操作会在 EOF，发生 I/O 错误，或数据超过缓存大小时停止。当读取操作停止以后，读取操作可能已经读取了很多的内容。如果程序需要自己处理错误和大数据块或进行序列化的读取时应该使用 bufio.Reader。

<a id="exampleScanner_custom"></a>
例:

    // An artificial input source.
    const input = "1234 5678 1234567901234567890"
    scanner := bufio.NewScanner(strings.NewReader(input))
    // Create a custom split function by wrapping the existing ScanWords function.
    split := func(data []byte, atEOF bool) (advance int, token []byte, err error) {
        advance, token, err = bufio.ScanWords(data, atEOF)
        if err == nil && token != nil {
            _, err = strconv.ParseInt(string(token), 10, 32)
        }
        return
    }
    // Set the split function for the scanning operation.
    scanner.Split(split)
    // Validate the input
    for scanner.Scan() {
        fmt.Printf("%s\n", scanner.Text())
    }
    
    if err := scanner.Err(); err != nil {
        fmt.Printf("Invalid input: %s", err)
    }
    // Output:
    // 1234
    // 5678
    // Invalid input: strconv.ParseInt: parsing "1234567901234567890": value out of range

<a id="exampleScanner_emptyFinalToken"></a>
<a id="exampleScanner_emptyFinalToken"></a>
例:

    // Comma-separated list; last entry is empty.
    const input = "1,2,3,4,"
    scanner := bufio.NewScanner(strings.NewReader(input))
    // Define a split function that separates on commas.
    onComma := func(data []byte, atEOF bool) (advance int, token []byte, err error) {
        for i := 0; i < len(data); i++ {
            if data[i] == ',' {
                return i + 1, data[:i], nil
            }
        }
        // There is one final token to be delivered, which may be the empty string.
        // Returning bufio.ErrFinalToken here tells Scan there are no more tokens after this
        // but does not trigger an error to be returned from Scan itself.
        return 0, data, bufio.ErrFinalToken
    }
    scanner.Split(onComma)
    // Scan.
    for scanner.Scan() {
        fmt.Printf("%q ", scanner.Text())
    }
    if err := scanner.Err(); err != nil {
        fmt.Fprintln(os.Stderr, "reading input:", err)
    }
    // Output: "1" "2" "3" "4" ""

<a id="exampleScanner_lines"></a>
<a id="exampleScanner_lines"></a>
例:

    scanner := bufio.NewScanner(os.Stdin)
    for scanner.Scan() {
        fmt.Println(scanner.Text()) // Println will add back the final '\n'
    }
    if err := scanner.Err(); err != nil {
        fmt.Fprintln(os.Stderr, "reading standard input:", err)
    }

<a id="exampleScanner_words"></a>
<a id="exampleScanner_words"></a>
例:

    // An artificial input source.
    const input = "Now is the winter of our discontent,\nMade glorious summer by this sun of York.\n"
    scanner := bufio.NewScanner(strings.NewReader(input))
    // Set the split function for the scanning operation.
    scanner.Split(bufio.ScanWords)
    // Count the words.
    count := 0
    for scanner.Scan() {
        count++
    }
    if err := scanner.Err(); err != nil {
        fmt.Fprintln(os.Stderr, "reading input:", err)
    }
    fmt.Printf("%d\n", count)
    // Output: 15

<h3 id="NewScanner">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L71">NewScanner</a>
    <a href="#NewScanner">¶</a></h3>
<pre>func NewScanner(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Scanner">Scanner</a></pre>

NewScanner 创建一个 r 的新 Scanner 实例。分割函数默认为 ScanLines。

<h3 id="Scanner.Buffer">func (*Scanner) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L247">Buffer</a>
    <a href="#Scanner.Buffer">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Buffer(buf []<a href="/builtin/#byte">byte</a>, max <a href="/builtin/#int">int</a>)</pre>

Buffer 设置 Scanner 用来读取数据所需的最大缓存（它会在读取的时候分配）。最大的数据块的尺寸为 `cap(buf)` 和 max 中的最大值。如果 max<=cap(buf)，Scan 会直接使用 buf 而不重新分配空间。

默认情况下，Scan 使用内部缓存并且将最大的数据块大小设置为 MaxScanTokenSize。

如果已经开始获取数据，Buffer 会 panic。

<h3 id="Scanner.Bytes">func (*Scanner) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L90">Bytes</a>
    <a href="#Scanner.Bytes">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Bytes() []<a href="/builtin/#byte">byte</a></pre>

Bytes 最近一次调用 Scan 获取到的数据。下次调用 Scan 会覆盖底切片层数组中的数据而不会分配新的内存。

<h3 id="Scanner.Err">func (*Scanner) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L80">Err</a>
    <a href="#Scanner.Err">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Err() <a href="/builtin/#error">error</a></pre>

Err 返回 Scanner 遇到的第一个不是 EOF 的错误。

<h3 id="Scanner.Scan">func (*Scanner) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L119">Scan</a>
    <a href="#Scanner.Scan">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Scan() <a href="/builtin/#bool">bool</a></pre>

Scan 前移 Scanner 读取下一段数据，数据可以通过 Bytes 或者 Text 方法导出。它会在读取全部输入或遇到错误时返回 false。在 Scan 返回 false 以后，Err 会返回读取过程中发生的错误。如果是 io.EOF，Err 会返回 nil。如果分割函数返回太多空数据而刷新输入。这是用户经常遇到的问题。

<h3 id="Scanner.Split">func (*Scanner) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L259">Split</a>
    <a href="#Scanner.Split">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Split(split <a href="#SplitFunc">SplitFunc</a>)</pre>

Split 设置 Scanner 对应的分割函数。默认的分割函数是 ScanLines。

如果在开始获取数据之后调用 Split 会 panic。

<h3 id="Scanner.Text">func (*Scanner) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L96">Text</a>
    <a href="#Scanner.Text">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Text() <a href="/builtin/#string">string</a></pre>

Text 返回最近一次调用 Scan 得到的字符串副本。

<h2 id="SplitFunc">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/scan.go#L50">SplitFunc</a>
    <a href="#SplitFunc">¶</a></h2>
<pre>type SplitFunc func(data []<a href="/builtin/#byte">byte</a>, atEOF <a href="/builtin/#bool">bool</a>) (advance <a href="/builtin/#int">int</a>, token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

SplitFunc 是用来分割数据的函数签名。参数包括一个未处理过的字符子串和 atEOF 参数（用来表示是否还有其他数据等待读取）。返回值包括需要前移的字节数和获取到的数据块和错误（如果存有的话）。如果没有拿到完整的数据，例如读取不到换行符，SplitFunc 可以返回 （0，nil，nil）来表示 Scanner 需要读取更多的数据到切片中并使用更长的切片再相同的位置重新读取。

如果返回的错误不为 nil 会停止读取操作并将错误返回给用户。

除非 atEOF 为 true，否则不会被空切片调用。如果 atEOF 为 true，但是 data 不为空，一般因为还有数据没有处理完成。

<h2 id="Writer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L514">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
    <span class="comment">// 包含过滤和非导出字段</span>
}</pre>

Writer 实现了对 io.Writer 对象的缓存读取。如果在写入时发生错误，将不会接受任何数据，接下来的写入和刷新操作都会返回错误。在所有数据都写入完成以后，用户应该调用 Flush 方法确保所有数据都写入了底层的 io.Writer。

<a id="exampleWriter"></a>
例:

    w := bufio.NewWriter(os.Stdout)
    fmt.Fprint(w, "Hello, ")
    fmt.Fprint(w, "world!")
    w.Flush() // Don't forget to flush!
    // Output: Hello, world!

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L540">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) *<a href="#Writer">Writer</a></pre>

NewWriter 返回一个默认大小缓存的 Writer。

<h3 id="NewWriterSize">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L524">NewWriterSize</a>
    <a href="#NewWriterSize">¶</a></h3>
<pre>func NewWriterSize(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, size <a href="/builtin/#int">int</a>) *<a href="#Writer">Writer</a></pre>

NewWriterSize 返回一个缓存大小至少为 size 的 Writer。如果 io.Writer 缓存足够大会直接返回它。

<h3 id="Writer.Available">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L580">Available</a>
    <a href="#Writer.Available">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Available() <a href="/builtin/#int">int</a></pre>

Available 返回在缓存中还有多少没发送的字节。

<h3 id="Writer.Buffered">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L583">Buffered</a>
    <a href="#Writer.Buffered">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Buffered() <a href="/builtin/#int">int</a></pre>

Buffered 返回已经写入缓存的字节数。

<h3 id="Writer.Flush">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L556">Flush</a>
    <a href="#Writer.Flush">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Flush() <a href="/builtin/#error">error</a></pre>

Flush 将所有缓存中的数据写入 io.Writer。

<h3 id="Writer.ReadFrom">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L678">ReadFrom</a>
    <a href="#Writer.ReadFrom">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) ReadFrom(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadFrom 实现了 io.ReaderFrom 接口。

<h3 id="Writer.Reset">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L549">Reset</a>
    <a href="#Writer.Reset">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Reset(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>)</pre>

Reset 丢弃所有未刷新的数据，清除所有错误并准备向 w 写入数据。

<h3 id="Writer.Size">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L545">Size</a>
    <a href="#Writer.Size">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Size() <a href="/builtin/#int">int</a></pre>

Size 返回底层缓存的大小。

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L589">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Write(p []<a href="/builtin/#byte">byte</a>) (nn <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write 将 p 写入缓存。它返回写入的字节数。如果 nn<len(p)，他还会返回错误解释原因。

<h3 id="Writer.WriteByte">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L614">WriteByte</a>
    <a href="#Writer.WriteByte">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) WriteByte(c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

WriteByte 写入单个字节。

<h3 id="Writer.WriteRune">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L628">WriteRune</a>
    <a href="#Writer.WriteRune">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) WriteRune(r <a href="/builtin/#rune">rune</a>) (size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteRune 写入单个 Unicode 代码点，它返回写入的字节数和任何发生的错误。

<h3 id="Writer.WriteString">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/bufio/bufio.go#L659">WriteString</a>
    <a href="#Writer.WriteString">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) WriteString(s <a href="/builtin/#string">string</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteString 写入一个字符串。它返回写入的字节数。如果写入的字节数小于 len(s)，它还会返回错误解释原因。


