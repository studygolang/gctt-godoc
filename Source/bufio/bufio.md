version: 1.9.2
## package bufio

  `import "bufio"`

## Overview

Package bufio implements buffered I/O. It wraps an io.Reader or io.Writer
object, creating another object (Reader or Writer) that also implements the
interface but provides buffering and some help for textual I/O.

## Index

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
  - [func (b *Writer) Write(p []byte) (nn int, err error)](#Writer.Write)
  - [func (b *Writer) WriteByte(c byte) error](#Writer.WriteByte)
  - [func (b *Writer) WriteRune(r rune) (size int, err error)](#Writer.WriteRune)
  - [func (b *Writer) WriteString(s string) (int, error)](#Writer.WriteString)

### Examples

- [Scanner (Custom)](#exampleScanner_custom)
- [Scanner (EmptyFinalToken)](#exampleScanner_emptyFinalToken)
- [Scanner (Lines)](#exampleScanner_lines)
- [Scanner (Words)](#exampleScanner_words)
- [Writer](#exampleWriter)

### Package files
 [bufio.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go) [scan.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// MaxScanTokenSize is the maximum size used to buffer a token</span>
    <span class="comment">// unless the user provides an explicit buffer with Scan.Buffer.</span>
    <span class="comment">// The actual maximum token size may be smaller as the buffer</span>
    <span class="comment">// may need to include, for instance, a newline.</span>
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

Errors returned by Scanner.

<pre>var <span id="ErrFinalToken">ErrFinalToken</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;final token&#34;)</pre>

ErrFinalToken is a special sentinel error value. It is intended to be returned
by a Split function to indicate that the token being delivered with the error is
the last token and scanning should stop after this one. After ErrFinalToken is
received by Scan, scanning stops with no error. The value is useful to stop
processing early or when it is necessary to deliver a final empty token. One
could achieve the same behavior with a custom error value but providing one here
is tidier. See the emptyFinalToken example for a use of this value.

<h2 id="ScanBytes">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L268">ScanBytes</a>
    <a href="#ScanBytes">¶</a></h2>
<pre>func ScanBytes(data []<a href="/builtin/#byte">byte</a>, atEOF <a href="/builtin/#bool">bool</a>) (advance <a href="/builtin/#int">int</a>, token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ScanBytes is a split function for a Scanner that returns each byte as a token.

<h2 id="ScanLines">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L329">ScanLines</a>
    <a href="#ScanLines">¶</a></h2>
<pre>func ScanLines(data []<a href="/builtin/#byte">byte</a>, atEOF <a href="/builtin/#bool">bool</a>) (advance <a href="/builtin/#int">int</a>, token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ScanLines is a split function for a Scanner that returns each line of text,
stripped of any trailing end-of-line marker. The returned line may be empty. The
end-of-line marker is one optional carriage return followed by one mandatory
newline. In regular expression notation, it is `\r?\n`. The last non-empty line
of input will be returned even if it has no newline.

<h2 id="ScanRunes">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L283">ScanRunes</a>
    <a href="#ScanRunes">¶</a></h2>
<pre>func ScanRunes(data []<a href="/builtin/#byte">byte</a>, atEOF <a href="/builtin/#bool">bool</a>) (advance <a href="/builtin/#int">int</a>, token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ScanRunes is a split function for a Scanner that returns each UTF-8-encoded rune
as a token. The sequence of runes returned is equivalent to that from a range
loop over the input as a string, which means that erroneous UTF-8 encodings
translate to U+FFFD = "\xef\xbf\xbd". Because of the Scan interface, this makes
it impossible for the client to distinguish correctly encoded replacement runes
from encoding errors.

<h2 id="ScanWords">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L374">ScanWords</a>
    <a href="#ScanWords">¶</a></h2>
<pre>func ScanWords(data []<a href="/builtin/#byte">byte</a>, atEOF <a href="/builtin/#bool">bool</a>) (advance <a href="/builtin/#int">int</a>, token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ScanWords is a split function for a Scanner that returns each space-separated
word of text, with surrounding spaces deleted. It will never return an empty
string. The definition of space is set by unicode.IsSpace.

<h2 id="ReadWriter">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L717">ReadWriter</a>
    <a href="#ReadWriter">¶</a></h2>
<pre>type ReadWriter struct {
    *<a href="#Reader">Reader</a>
    *<a href="#Writer">Writer</a>
}</pre>

ReadWriter stores pointers to a Reader and a Writer. It implements
io.ReadWriter.

<h3 id="NewReadWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L723">NewReadWriter</a>
    <a href="#NewReadWriter">¶</a></h3>
<pre>func NewReadWriter(r *<a href="#Reader">Reader</a>, w *<a href="#Writer">Writer</a>) *<a href="#ReadWriter">ReadWriter</a></pre>

NewReadWriter allocates a new ReadWriter that dispatches to r and w.

<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L21">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Reader implements buffering for an io.Reader object.

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L51">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(rd <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Reader">Reader</a></pre>

NewReader returns a new Reader whose buffer has the default size.

<h3 id="NewReaderSize">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L36">NewReaderSize</a>
    <a href="#NewReaderSize">¶</a></h3>
<pre>func NewReaderSize(rd <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, size <a href="/builtin/#int">int</a>) *<a href="#Reader">Reader</a></pre>

NewReaderSize returns a new Reader whose buffer has at least the specified size.
If the argument io.Reader is already a Reader with large enough size, it returns
the underlying Reader.

<h3 id="Reader.Buffered">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L291">Buffered</a>
    <a href="#Reader.Buffered">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) Buffered() <a href="/builtin/#int">int</a></pre>

Buffered returns the number of bytes that can be read from the current buffer.

<h3 id="Reader.Discard">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L144">Discard</a>
    <a href="#Reader.Discard">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) Discard(n <a href="/builtin/#int">int</a>) (discarded <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Discard skips the next n bytes, returning the number of bytes discarded.

If Discard skips fewer than n bytes, it also returns an error. If 0 <= n <=
b.Buffered(), Discard is guaranteed to succeed without reading from the
underlying io.Reader.

<h3 id="Reader.Peek">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L113">Peek</a>
    <a href="#Reader.Peek">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) Peek(n <a href="/builtin/#int">int</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Peek returns the next n bytes without advancing the reader. The bytes stop being
valid at the next read call. If Peek returns fewer than n bytes, it also returns
an error explaining why the read is short. The error is ErrBufferFull if n is
larger than b's buffer size.

<h3 id="Reader.Read">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L177">Read</a>
    <a href="#Reader.Read">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read reads data into p. It returns the number of bytes read into p. The bytes
are taken from at most one Read on the underlying Reader, hence n may be less
than len(p). At EOF, the count will be zero and err will be io.EOF.

<h3 id="Reader.ReadByte">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L223">ReadByte</a>
    <a href="#Reader.ReadByte">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadByte() (<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadByte reads and returns a single byte. If no byte is available, returns an
error.

<h3 id="Reader.ReadBytes">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L398">ReadBytes</a>
    <a href="#Reader.ReadBytes">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadBytes(delim <a href="/builtin/#byte">byte</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadBytes reads until the first occurrence of delim in the input, returning a
slice containing the data up to and including the delimiter. If ReadBytes
encounters an error before finding a delimiter, it returns the data read before
the error and the error itself (often io.EOF). ReadBytes returns err != nil if
and only if the returned data does not end in delim. For simple uses, a Scanner
may be more convenient.

<h3 id="Reader.ReadLine">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L356">ReadLine</a>
    <a href="#Reader.ReadLine">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadLine() (line []<a href="/builtin/#byte">byte</a>, isPrefix <a href="/builtin/#bool">bool</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadLine is a low-level line-reading primitive. Most callers should use
ReadBytes('\n') or ReadString('\n') instead or use a Scanner.

ReadLine tries to return a single line, not including the end-of-line bytes. If
the line was too long for the buffer then isPrefix is set and the beginning of
the line is returned. The rest of the line will be returned from future calls.
isPrefix will be false when returning the last fragment of the line. The
returned buffer is only valid until the next call to ReadLine. ReadLine either
returns a non-nil line or it returns an error, never both.

The text returned from ReadLine does not include the line end ("\r\n" or "\n").
No indication or error is given if the input ends without a final line end.
Calling UnreadByte after ReadLine will always unread the last byte read
(possibly a character belonging to the line end) even if that byte is not part
of the line returned by ReadLine.

<h3 id="Reader.ReadRune">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L258">ReadRune</a>
    <a href="#Reader.ReadRune">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadRune() (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadRune reads a single UTF-8 encoded Unicode character and returns the rune and
its size in bytes. If the encoded rune is invalid, it consumes one byte and
returns unicode.ReplacementChar (U+FFFD) with a size of 1.

<h3 id="Reader.ReadSlice">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L303">ReadSlice</a>
    <a href="#Reader.ReadSlice">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadSlice(delim <a href="/builtin/#byte">byte</a>) (line []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadSlice reads until the first occurrence of delim in the input, returning a
slice pointing at the bytes in the buffer. The bytes stop being valid at the
next read. If ReadSlice encounters an error before finding a delimiter, it
returns all the data in the buffer and the error itself (often io.EOF).
ReadSlice fails with error ErrBufferFull if the buffer fills without a delim.
Because the data returned from ReadSlice will be overwritten by the next I/O
operation, most clients should use ReadBytes or ReadString instead. ReadSlice
returns err != nil if and only if line does not end in delim.

<h3 id="Reader.ReadString">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L445">ReadString</a>
    <a href="#Reader.ReadString">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) ReadString(delim <a href="/builtin/#byte">byte</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ReadString reads until the first occurrence of delim in the input, returning a
string containing the data up to and including the delimiter. If ReadString
encounters an error before finding a delimiter, it returns the data read before
the error and the error itself (often io.EOF). ReadString returns err != nil if
and only if the returned data does not end in delim. For simple uses, a Scanner
may be more convenient.

<h3 id="Reader.Reset">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L57">Reset</a>
    <a href="#Reader.Reset">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) Reset(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>)</pre>

Reset discards any buffered data, resets all state, and switches the buffered
reader to read from r.

<h3 id="Reader.UnreadByte">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L238">UnreadByte</a>
    <a href="#Reader.UnreadByte">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) UnreadByte() <a href="/builtin/#error">error</a></pre>

UnreadByte unreads the last byte. Only the most recently read byte can be
unread.

<h3 id="Reader.UnreadRune">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L280">UnreadRune</a>
    <a href="#Reader.UnreadRune">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) UnreadRune() <a href="/builtin/#error">error</a></pre>

UnreadRune unreads the last rune. If the most recent read operation on the
buffer was not a ReadRune, UnreadRune returns an error. (In this regard it is
stricter than UnreadByte, which will unread the last byte from any read
operation.)

<h3 id="Reader.WriteTo">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L452">WriteTo</a>
    <a href="#Reader.WriteTo">¶</a></h3>
<pre>func (b *<a href="#Reader">Reader</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteTo implements io.WriterTo. This may make multiple calls to the Read method
of the underlying Reader.

<h2 id="Scanner">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L20">Scanner</a>
    <a href="#Scanner">¶</a></h2>
<pre>type Scanner struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Scanner provides a convenient interface for reading data such as a file of
newline-delimited lines of text. Successive calls to the Scan method will step
through the 'tokens' of a file, skipping the bytes between the tokens. The
specification of a token is defined by a split function of type SplitFunc; the
default split function breaks the input into lines with line termination
stripped. Split functions are defined in this package for scanning a file into
lines, bytes, UTF-8-encoded runes, and space-delimited words. The client may
instead provide a custom split function.

Scanning stops unrecoverably at EOF, the first I/O error, or a token too large
to fit in the buffer. When a scan stops, the reader may have advanced
arbitrarily far past the last token. Programs that need more control over error
handling or large tokens, or must run sequential scans on a reader, should use
bufio.Reader instead.

<a id="exampleScanner_custom"></a>
Example:

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
Example:

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
Example:

    scanner := bufio.NewScanner(os.Stdin)
    for scanner.Scan() {
        fmt.Println(scanner.Text()) // Println will add back the final '\n'
    }
    if err := scanner.Err(); err != nil {
        fmt.Fprintln(os.Stderr, "reading standard input:", err)
    }


<a id="exampleScanner_words"></a>
Example:

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

<h3 id="NewScanner">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L71">NewScanner</a>
    <a href="#NewScanner">¶</a></h3>
<pre>func NewScanner(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Scanner">Scanner</a></pre>

NewScanner returns a new Scanner to read from r. The split function defaults to
ScanLines.

<h3 id="Scanner.Buffer">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L246">Buffer</a>
    <a href="#Scanner.Buffer">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Buffer(buf []<a href="/builtin/#byte">byte</a>, max <a href="/builtin/#int">int</a>)</pre>

Buffer sets the initial buffer to use when scanning and the maximum size of
buffer that may be allocated during scanning. The maximum token size is the
larger of max and cap(buf). If max <= cap(buf), Scan will use this buffer only
and do no allocation.

By default, Scan uses an internal buffer and sets the maximum token size to
MaxScanTokenSize.

Buffer panics if it is called after scanning has started.

<h3 id="Scanner.Bytes">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L90">Bytes</a>
    <a href="#Scanner.Bytes">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Bytes() []<a href="/builtin/#byte">byte</a></pre>

Bytes returns the most recent token generated by a call to Scan. The underlying
array may point to data that will be overwritten by a subsequent call to Scan.
It does no allocation.

<h3 id="Scanner.Err">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L80">Err</a>
    <a href="#Scanner.Err">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Err() <a href="/builtin/#error">error</a></pre>

Err returns the first non-EOF error that was encountered by the Scanner.

<h3 id="Scanner.Scan">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L118">Scan</a>
    <a href="#Scanner.Scan">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Scan() <a href="/builtin/#bool">bool</a></pre>

Scan advances the Scanner to the next token, which will then be available
through the Bytes or Text method. It returns false when the scan stops, either
by reaching the end of the input or an error. After Scan returns false, the Err
method will return any error that occurred during scanning, except that if it
was io.EOF, Err will return nil. Scan panics if the split function returns 100
empty tokens without advancing the input. This is a common error mode for
scanners.

<h3 id="Scanner.Split">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L258">Split</a>
    <a href="#Scanner.Split">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Split(split <a href="#SplitFunc">SplitFunc</a>)</pre>

Split sets the split function for the Scanner. The default split function is
ScanLines.

Split panics if it is called after scanning has started.

<h3 id="Scanner.Text">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L96">Text</a>
    <a href="#Scanner.Text">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Text() <a href="/builtin/#string">string</a></pre>

Text returns the most recent token generated by a call to Scan as a newly
allocated string holding its bytes.

<h2 id="SplitFunc">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/scan.go#L50">SplitFunc</a>
    <a href="#SplitFunc">¶</a></h2>
<pre>type SplitFunc func(data []<a href="/builtin/#byte">byte</a>, atEOF <a href="/builtin/#bool">bool</a>) (advance <a href="/builtin/#int">int</a>, token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

SplitFunc is the signature of the split function used to tokenize the input. The
arguments are an initial substring of the remaining unprocessed data and a flag,
atEOF, that reports whether the Reader has no more data to give. The return
values are the number of bytes to advance the input and the next token to return
to the user, plus an error, if any. If the data does not yet hold a complete
token, for instance if it has no newline while scanning lines, SplitFunc can
return (0, nil, nil) to signal the Scanner to read more data into the slice and
try again with a longer slice starting at the same point in the input.

If the returned error is non-nil, scanning stops and the error is returned to
the client.

The function is never called with an empty data slice unless atEOF is true. If
atEOF is true, however, data may be non-empty and, as always, holds unprocessed
text.

<h2 id="Writer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L511">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Writer implements buffering for an io.Writer object. If an error occurs writing
to a Writer, no more data will be accepted and all subsequent writes, and Flush,
will return the error. After all data has been written, the client should call
the Flush method to guarantee all data has been forwarded to the underlying
io.Writer.

<a id="exampleWriter"></a>
Example:

    w := bufio.NewWriter(os.Stdout)
    fmt.Fprint(w, "Hello, ")
    fmt.Fprint(w, "world!")
    w.Flush() // Don't forget to flush!
    // Output: Hello, world!

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L537">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) *<a href="#Writer">Writer</a></pre>

NewWriter returns a new Writer whose buffer has the default size.

<h3 id="NewWriterSize">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L521">NewWriterSize</a>
    <a href="#NewWriterSize">¶</a></h3>
<pre>func NewWriterSize(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, size <a href="/builtin/#int">int</a>) *<a href="#Writer">Writer</a></pre>

NewWriterSize returns a new Writer whose buffer has at least the specified size.
If the argument io.Writer is already a Writer with large enough size, it returns
the underlying Writer.

<h3 id="Writer.Available">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L574">Available</a>
    <a href="#Writer.Available">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Available() <a href="/builtin/#int">int</a></pre>

Available returns how many bytes are unused in the buffer.

<h3 id="Writer.Buffered">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L577">Buffered</a>
    <a href="#Writer.Buffered">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Buffered() <a href="/builtin/#int">int</a></pre>

Buffered returns the number of bytes that have been written into the current
buffer.

<h3 id="Writer.Flush">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L550">Flush</a>
    <a href="#Writer.Flush">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Flush() <a href="/builtin/#error">error</a></pre>

Flush writes any buffered data to the underlying io.Writer.

<h3 id="Writer.ReadFrom">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L672">ReadFrom</a>
    <a href="#Writer.ReadFrom">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) ReadFrom(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadFrom implements io.ReaderFrom.

<h3 id="Writer.Reset">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L543">Reset</a>
    <a href="#Writer.Reset">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Reset(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>)</pre>

Reset discards any unflushed buffered data, clears any error, and resets b to
write its output to w.

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L583">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Write(p []<a href="/builtin/#byte">byte</a>) (nn <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write writes the contents of p into the buffer. It returns the number of bytes
written. If nn < len(p), it also returns an error explaining why the write is
short.

<h3 id="Writer.WriteByte">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L608">WriteByte</a>
    <a href="#Writer.WriteByte">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) WriteByte(c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

WriteByte writes a single byte.

<h3 id="Writer.WriteRune">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L622">WriteRune</a>
    <a href="#Writer.WriteRune">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) WriteRune(r <a href="/builtin/#rune">rune</a>) (size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteRune writes a single Unicode code point, returning the number of bytes
written and any error.

<h3 id="Writer.WriteString">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bufio/bufio.go#L653">WriteString</a>
    <a href="#Writer.WriteString">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) WriteString(s <a href="/builtin/#string">string</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteString writes a string. It returns the number of bytes written. If the
count is less than len(s), it also returns an error explaining why the write is
short.


