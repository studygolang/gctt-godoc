version: 1.10
## package textproto

  `import "net/textproto"`

## Overview

Package textproto implements generic support for text-based request/response
protocols in the style of HTTP, NNTP, and SMTP.

The package provides:

Error, which represents a numeric error response from a server.

Pipeline, to manage pipelined requests and responses in a client.

Reader, to read numeric response code lines, key: value headers, lines wrapped
with leading spaces on continuation lines, and whole text blocks ending with a
dot on a line by itself.

Writer, to write dot-encoded text blocks.

Conn, a convenient packaging of Reader, Writer, and Pipeline for use with a
single network connection.

## Index

- [func CanonicalMIMEHeaderKey(s string) string](#CanonicalMIMEHeaderKey)
- [func TrimBytes(b []byte) []byte](#TrimBytes)
- [func TrimString(s string) string](#TrimString)
- [type Conn](#Conn)
  - [func Dial(network, addr string) (*Conn, error)](#Dial)
  - [func NewConn(conn io.ReadWriteCloser) *Conn](#NewConn)
  - [func (c *Conn) Close() error](#Conn.Close)
  - [func (c *Conn) Cmd(format string, args ...interface{}) (id uint, err error)](#Conn.Cmd)
- [type Error](#Error)
  - [func (e *Error) Error() string](#Error.Error)
- [type MIMEHeader](#MIMEHeader)
  - [func (h MIMEHeader) Add(key, value string)](#MIMEHeader.Add)
  - [func (h MIMEHeader) Del(key string)](#MIMEHeader.Del)
  - [func (h MIMEHeader) Get(key string) string](#MIMEHeader.Get)
  - [func (h MIMEHeader) Set(key, value string)](#MIMEHeader.Set)
- [type Pipeline](#Pipeline)
  - [func (p *Pipeline) EndRequest(id uint)](#Pipeline.EndRequest)
  - [func (p *Pipeline) EndResponse(id uint)](#Pipeline.EndResponse)
  - [func (p *Pipeline) Next() uint](#Pipeline.Next)
  - [func (p *Pipeline) StartRequest(id uint)](#Pipeline.StartRequest)
  - [func (p *Pipeline) StartResponse(id uint)](#Pipeline.StartResponse)
- [type ProtocolError](#ProtocolError)
  - [func (p ProtocolError) Error() string](#ProtocolError.Error)
- [type Reader](#Reader)
  - [func NewReader(r *bufio.Reader) *Reader](#NewReader)
  - [func (r *Reader) DotReader() io.Reader](#Reader.DotReader)
  - [func (r *Reader) ReadCodeLine(expectCode int) (code int, message string, err error)](#Reader.ReadCodeLine)
  - [func (r *Reader) ReadContinuedLine() (string, error)](#Reader.ReadContinuedLine)
  - [func (r *Reader) ReadContinuedLineBytes() ([]byte, error)](#Reader.ReadContinuedLineBytes)
  - [func (r *Reader) ReadDotBytes() ([]byte, error)](#Reader.ReadDotBytes)
  - [func (r *Reader) ReadDotLines() ([]string, error)](#Reader.ReadDotLines)
  - [func (r *Reader) ReadLine() (string, error)](#Reader.ReadLine)
  - [func (r *Reader) ReadLineBytes() ([]byte, error)](#Reader.ReadLineBytes)
  - [func (r *Reader) ReadMIMEHeader() (MIMEHeader, error)](#Reader.ReadMIMEHeader)
  - [func (r *Reader) ReadResponse(expectCode int) (code int, message string, err error)](#Reader.ReadResponse)
- [type Writer](#Writer)
  - [func NewWriter(w *bufio.Writer) *Writer](#NewWriter)
  - [func (w *Writer) DotWriter() io.WriteCloser](#Writer.DotWriter)
  - [func (w *Writer) PrintfLine(format string, args ...interface{}) error](#Writer.PrintfLine)

### Package files
 [header.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/header.go) [pipeline.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/pipeline.go) [reader.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go) [textproto.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go) [writer.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/writer.go)

<h2 id="CanonicalMIMEHeaderKey">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L562">CanonicalMIMEHeaderKey</a>
    <a href="#CanonicalMIMEHeaderKey">¶</a></h2>
<pre>func CanonicalMIMEHeaderKey(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

CanonicalMIMEHeaderKey returns the canonical format of the MIME header key s.
The canonicalization converts the first letter and any letter following a hyphen
to upper case; the rest are converted to lowercase. For example, the canonical
key for "accept-encoding" is "Accept-Encoding". MIME header keys are assumed to
be ASCII only. If s contains a space or invalid header field bytes, it is
returned without modifications.

<h2 id="TrimBytes">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L127">TrimBytes</a>
    <a href="#TrimBytes">¶</a></h2>
<pre>func TrimBytes(b []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimBytes returns b without leading and trailing ASCII space.

<h2 id="TrimString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L116">TrimString</a>
    <a href="#TrimString">¶</a></h2>
<pre>func TrimString(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimString returns s without leading and trailing ASCII space.

<h2 id="Conn">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L48">Conn</a>
    <a href="#Conn">¶</a></h2>
<pre>type Conn struct {
    <a href="#Reader">Reader</a>
    <a href="#Writer">Writer</a>
    <a href="#Pipeline">Pipeline</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Conn represents a textual network protocol connection. It consists of a Reader
and Writer to manage I/O and a Pipeline to sequence concurrent requests on the
connection. These embedded types carry methods with them; see the documentation
of those types for details.

<h3 id="Dial">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L71">Dial</a>
    <a href="#Dial">¶</a></h3>
<pre>func Dial(network, addr <a href="/builtin/#string">string</a>) (*<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Dial connects to the given address on the given network using net.Dial and then
returns a new Conn for the connection.

<h3 id="NewConn">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L56">NewConn</a>
    <a href="#NewConn">¶</a></h3>
<pre>func NewConn(conn <a href="/io/">io</a>.<a href="/io/#ReadWriteCloser">ReadWriteCloser</a>) *<a href="#Conn">Conn</a></pre>

NewConn returns a new Conn using conn for I/O.

<h3 id="Conn.Close">func (*Conn) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L65">Close</a>
    <a href="#Conn.Close">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the connection.

<h3 id="Conn.Cmd">func (*Conn) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L104">Cmd</a>
    <a href="#Conn.Cmd">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) Cmd(format <a href="/builtin/#string">string</a>, args ...interface{}) (id <a href="/builtin/#uint">uint</a>, err <a href="/builtin/#error">error</a>)</pre>

Cmd is a convenience method that sends a command after waiting its turn in the
pipeline. The command text is the result of formatting format with args and
appending \r\n. Cmd returns the id of the command, for use with StartResponse
and EndResponse.

For example, a client might run a HELP command that returns a dot-body by using:

    id, err := c.Cmd("HELP")
    if err != nil {
    	return nil, err
    }

    c.StartResponse(id)
    defer c.EndResponse(id)

    if _, _, err = c.ReadCodeLine(110); err != nil {
    	return nil, err
    }
    text, err := c.ReadDotBytes()
    if err != nil {
    	return nil, err
    }
    return c.ReadCodeLine(250)

<h2 id="Error">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L26">Error</a>
    <a href="#Error">¶</a></h2>
<pre>type Error struct {
<span id="Error.Code"></span>    Code <a href="/builtin/#int">int</a>
<span id="Error.Msg"></span>    Msg  <a href="/builtin/#string">string</a>
}</pre>

An Error represents a numeric error response from a server.

<h3 id="Error.Error">func (*Error) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L31">Error</a>
    <a href="#Error.Error">¶</a></h3>
<pre>func (e *<a href="#Error">Error</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="MIMEHeader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/header.go#L1">MIMEHeader</a>
    <a href="#MIMEHeader">¶</a></h2>
<pre>type MIMEHeader map[<a href="/builtin/#string">string</a>][]<a href="/builtin/#string">string</a></pre>

A MIMEHeader represents a MIME-style header mapping keys to sets of values.

<h3 id="MIMEHeader.Add">func (MIMEHeader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/header.go#L3">Add</a>
    <a href="#MIMEHeader.Add">¶</a></h3>
<pre>func (h <a href="#MIMEHeader">MIMEHeader</a>) Add(key, value <a href="/builtin/#string">string</a>)</pre>

Add adds the key, value pair to the header. It appends to any existing values
associated with key.

<h3 id="MIMEHeader.Del">func (MIMEHeader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/header.go#L33">Del</a>
    <a href="#MIMEHeader.Del">¶</a></h3>
<pre>func (h <a href="#MIMEHeader">MIMEHeader</a>) Del(key <a href="/builtin/#string">string</a>)</pre>

Del deletes the values associated with key.

<h3 id="MIMEHeader.Get">func (MIMEHeader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/header.go#L21">Get</a>
    <a href="#MIMEHeader.Get">¶</a></h3>
<pre>func (h <a href="#MIMEHeader">MIMEHeader</a>) Get(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Get gets the first value associated with the given key. It is case insensitive;
CanonicalMIMEHeaderKey is used to canonicalize the provided key. If there are no
values associated with the key, Get returns "". To access multiple values of a
key, or to use non-canonical keys, access the map directly.

<h3 id="MIMEHeader.Set">func (MIMEHeader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/header.go#L11">Set</a>
    <a href="#MIMEHeader.Set">¶</a></h3>
<pre>func (h <a href="#MIMEHeader">MIMEHeader</a>) Set(key, value <a href="/builtin/#string">string</a>)</pre>

Set sets the header entries associated with key to the single element value. It
replaces any existing values associated with key.

<h2 id="Pipeline">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/pipeline.go#L18">Pipeline</a>
    <a href="#Pipeline">¶</a></h2>
<pre>type Pipeline struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Pipeline manages a pipelined in-order request/response sequence.

To use a Pipeline p to manage multiple clients on a connection, each client
should run:

    id := p.Next()	// take a number

    p.StartRequest(id)	// wait for turn to send request
    «send request»
    p.EndRequest(id)	// notify Pipeline that request is sent

    p.StartResponse(id)	// wait for turn to read response
    «read response»
    p.EndResponse(id)	// notify Pipeline that response is read

A pipelined server can use the same calls to ensure that responses computed in
parallel are written in the correct order.

<h3 id="Pipeline.EndRequest">func (*Pipeline) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/pipeline.go#L42">EndRequest</a>
    <a href="#Pipeline.EndRequest">¶</a></h3>
<pre>func (p *<a href="#Pipeline">Pipeline</a>) EndRequest(id <a href="/builtin/#uint">uint</a>)</pre>

EndRequest notifies p that the request with the given id has been sent (or, if
this is a server, received).

<h3 id="Pipeline.EndResponse">func (*Pipeline) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/pipeline.go#L54">EndResponse</a>
    <a href="#Pipeline.EndResponse">¶</a></h3>
<pre>func (p *<a href="#Pipeline">Pipeline</a>) EndResponse(id <a href="/builtin/#uint">uint</a>)</pre>

EndResponse notifies p that the response with the given id has been received
(or, if this is a server, sent).

<h3 id="Pipeline.Next">func (*Pipeline) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/pipeline.go#L26">Next</a>
    <a href="#Pipeline.Next">¶</a></h3>
<pre>func (p *<a href="#Pipeline">Pipeline</a>) Next() <a href="/builtin/#uint">uint</a></pre>

Next returns the next id for a request/response pair.

<h3 id="Pipeline.StartRequest">func (*Pipeline) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/pipeline.go#L36">StartRequest</a>
    <a href="#Pipeline.StartRequest">¶</a></h3>
<pre>func (p *<a href="#Pipeline">Pipeline</a>) StartRequest(id <a href="/builtin/#uint">uint</a>)</pre>

StartRequest blocks until it is time to send (or, if this is a server, receive)
the request with the given id.

<h3 id="Pipeline.StartResponse">func (*Pipeline) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/pipeline.go#L48">StartResponse</a>
    <a href="#Pipeline.StartResponse">¶</a></h3>
<pre>func (p *<a href="#Pipeline">Pipeline</a>) StartResponse(id <a href="/builtin/#uint">uint</a>)</pre>

StartResponse blocks until it is time to receive (or, if this is a server, send)
the request with the given id.

<h2 id="ProtocolError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L37">ProtocolError</a>
    <a href="#ProtocolError">¶</a></h2>
<pre>type ProtocolError <a href="/builtin/#string">string</a></pre>

A ProtocolError describes a protocol violation such as an invalid response or a
hung-up connection.

<h3 id="ProtocolError.Error">func (ProtocolError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/textproto.go#L39">Error</a>
    <a href="#ProtocolError.Error">¶</a></h3>
<pre>func (p <a href="#ProtocolError">ProtocolError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Reader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L8">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
<span id="Reader.R"></span>    R *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Reader implements convenience methods for reading requests or responses from a
text protocol network connection.

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L19">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(r *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>) *<a href="#Reader">Reader</a></pre>

NewReader returns a new Reader reading from r.

To avoid denial of service attacks, the provided bufio.Reader should be reading
from an io.LimitReader or similar Reader to bound the size of responses.

<h3 id="Reader.DotReader">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L286">DotReader</a>
    <a href="#Reader.DotReader">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) DotReader() <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

DotReader returns a new Reader that satisfies Reads using the decoded text of a
dot-encoded block read from r. The returned Reader is only valid until the next
call to a method on r.

Dot encoding is a common framing used for data blocks in text protocols such as
SMTP. The data consists of a sequence of lines, each of which ends in "\r\n".
The sequence itself ends at a line containing just a dot: ".\r\n". Lines
beginning with a dot are escaped with an additional dot to avoid looking like
the end of the sequence.

The decoded form returned by the Reader's Read method rewrites the "\r\n" line
endings into the simpler "\n", removes leading dot escapes if present, and stops
with error io.EOF after consuming (and discarding) the end-of-sequence line.

<h3 id="Reader.ReadCodeLine">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L209">ReadCodeLine</a>
    <a href="#Reader.ReadCodeLine">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadCodeLine(expectCode <a href="/builtin/#int">int</a>) (code <a href="/builtin/#int">int</a>, message <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadCodeLine reads a response code line of the form

    code message

where code is a three-digit status code and the message extends to the rest of
the line. An example of such a line is:

    220 plan9.bell-labs.com ESMTP

If the prefix of the status does not match the digits in expectCode,
ReadCodeLine returns with err set to &Error{code, message}. For example, if
expectCode is 31, an error will be returned if the status is not in the range
[310,319].

If the response is multi-line, ReadCodeLine returns an error.

An expectCode <= 0 disables the check of the status code.

<h3 id="Reader.ReadContinuedLine">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L80">ReadContinuedLine</a>
    <a href="#Reader.ReadContinuedLine">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadContinuedLine() (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ReadContinuedLine reads a possibly continued line from r, eliding the final
trailing ASCII white space. Lines after the first are considered continuations
if they begin with a space or tab character. In the returned data, continuation
lines are separated from the previous line only by a single space: the newline
and leading white space are removed.

For example, consider this input:

    Line 1
      continued...
    Line 2

The first call to ReadContinuedLine will return "Line 1 continued..." and the
second will return "Line 2".

A line consisting of only white space is never continued.

<h3 id="Reader.ReadContinuedLineBytes">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L101">ReadContinuedLineBytes</a>
    <a href="#Reader.ReadContinuedLineBytes">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadContinuedLineBytes() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadContinuedLineBytes is like ReadContinuedLine but returns a []byte instead of
a string.

<h3 id="Reader.ReadDotBytes">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L402">ReadDotBytes</a>
    <a href="#Reader.ReadDotBytes">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadDotBytes() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadDotBytes reads a dot-encoding and returns the decoded data.

See the documentation for the DotReader method for details about dot-encoding.

<h3 id="Reader.ReadDotLines">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L410">ReadDotLines</a>
    <a href="#Reader.ReadDotLines">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadDotLines() ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ReadDotLines reads a dot-encoding and returns a slice containing the decoded
lines, with the final \r\n or \n elided from each.

See the documentation for the DotReader method for details about dot-encoding.

<h3 id="Reader.ReadLine">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L25">ReadLine</a>
    <a href="#Reader.ReadLine">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadLine() (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ReadLine reads a single line from r, eliding the final \n or \r\n from the
returned string.

<h3 id="Reader.ReadLineBytes">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L31">ReadLineBytes</a>
    <a href="#Reader.ReadLineBytes">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadLineBytes() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadLineBytes is like ReadLine but returns a []byte instead of a string.

<h3 id="Reader.ReadMIMEHeader">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L458">ReadMIMEHeader</a>
    <a href="#Reader.ReadMIMEHeader">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadMIMEHeader() (<a href="#MIMEHeader">MIMEHeader</a>, <a href="/builtin/#error">error</a>)</pre>

ReadMIMEHeader reads a MIME-style header from r. The header is a sequence of
possibly continued Key: Value lines ending in a blank line. The returned map m
maps CanonicalMIMEHeaderKey(key) to a sequence of values in the same order
encountered in the input.

For example, consider this input:

    My-Key: Value 1
    Long-Key: Even
           Longer Value
    My-Key: Value 2

Given that input, ReadMIMEHeader returns the map:

    map[string][]string{
    	"My-Key": {"Value 1", "Value 2"},
    	"Long-Key": {"Even Longer Value"},
    }

<h3 id="Reader.ReadResponse">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/reader.go#L244">ReadResponse</a>
    <a href="#Reader.ReadResponse">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadResponse(expectCode <a href="/builtin/#int">int</a>) (code <a href="/builtin/#int">int</a>, message <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadResponse reads a multi-line response of the form:

    code-message line 1
    code-message line 2
    ...
    code message line n

where code is a three-digit status code. The first line starts with the code and
a hyphen. The response is terminated by a line that starts with the same code
followed by a space. Each line in message is separated by a newline (\n).

See page 36 of RFC 959 (http://www.ietf.org/rfc/rfc959.txt) for details of
another form of response accepted:

    code-message line 1
    message line 2
    ...
    code message line n

If the prefix of the status does not match the digits in expectCode,
ReadResponse returns with err set to &Error{code, message}. For example, if
expectCode is 31, an error will be returned if the status is not in the range
[310,319].

An expectCode <= 0 disables the check of the status code.

<h2 id="Writer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/writer.go#L5">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
<span id="Writer.W"></span>    W *<a href="/bufio/">bufio</a>.<a href="/bufio/#Writer">Writer</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Writer implements convenience methods for writing requests or responses to a
text protocol network connection.

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/writer.go#L11">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(w *<a href="/bufio/">bufio</a>.<a href="/bufio/#Writer">Writer</a>) *<a href="#Writer">Writer</a></pre>

NewWriter returns a new Writer writing to w.

<h3 id="Writer.DotWriter">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/writer.go#L33">DotWriter</a>
    <a href="#Writer.DotWriter">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) DotWriter() <a href="/io/">io</a>.<a href="/io/#WriteCloser">WriteCloser</a></pre>

DotWriter returns a writer that can be used to write a dot-encoding to w. It
takes care of inserting leading dots when necessary, translating line-ending \n
into \r\n, and adding the final .\r\n line when the DotWriter is closed. The
caller should close the DotWriter before the next call to a method on w.

See the documentation for Reader's DotReader method for details about
dot-encoding.

<h3 id="Writer.PrintfLine">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/textproto/writer.go#L19">PrintfLine</a>
    <a href="#Writer.PrintfLine">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) PrintfLine(format <a href="/builtin/#string">string</a>, args ...interface{}) <a href="/builtin/#error">error</a></pre>

PrintfLine writes the formatted output followed by \r\n.


