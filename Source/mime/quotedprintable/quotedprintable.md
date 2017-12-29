version: 1.9.2
## package quotedprintable

  `import "mime/quotedprintable"`

## Overview

Package quotedprintable implements quoted-printable encoding as specified by RFC
2045.

## Index

- [type Reader](#Reader)
  - [func NewReader(r io.Reader) *Reader](#NewReader)
  - [func (r *Reader) Read(p []byte) (n int, err error)](#Reader.Read)
- [type Writer](#Writer)
  - [func NewWriter(w io.Writer) *Writer](#NewWriter)
  - [func (w *Writer) Close() error](#Writer.Close)
  - [func (w *Writer) Write(p []byte) (n int, err error)](#Writer.Write)

### Examples

- [NewReader](#example_NewReader)
- [NewWriter](#example_NewWriter)

### Package files
 [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/mime/quotedprintable/reader.go) [writer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/mime/quotedprintable/writer.go)

<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/mime/quotedprintable/reader.go#L7">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Reader is a quoted-printable decoder.

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/mime/quotedprintable/reader.go#L14">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Reader">Reader</a></pre>

NewReader returns a quoted-printable reader, decoding from r.

<a id="example_NewReader"></a>
Example:

    for _, s := range []string{
        `=48=65=6C=6C=6F=2C=20=47=6F=70=68=65=72=73=21`,
        `invalid escape: <b style="font-size: 200%">hello</b>`,
        "Hello, Gophers! This symbol will be unescaped: =3D and this will be written in =\r\none line.",
    } {
        b, err := ioutil.ReadAll(quotedprintable.NewReader(strings.NewReader(s)))
        fmt.Printf("%s %v\n", b, err)
    }
    // Output:
    // Hello, Gophers! <nil>
    // invalid escape: <b style="font-size: 200%">hello</b> <nil>
    // Hello, Gophers! This symbol will be unescaped: = and this will be written in one line. <nil>

<h3 id="Reader.Read">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/mime/quotedprintable/reader.go#L62">Read</a>
    <a href="#Reader.Read">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read reads and decodes quoted-printable data from the underlying reader.

<h2 id="Writer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/mime/quotedprintable/writer.go#L2">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
<span id="Writer.Binary"></span>    <span class="comment">// Binary mode treats the writer&#39;s input as pure binary and processes end of</span>
    <span class="comment">// line bytes as binary data.</span>
    Binary <a href="/builtin/#bool">bool</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Writer is a quoted-printable writer that implements io.WriteCloser.

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/mime/quotedprintable/writer.go#L14">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) *<a href="#Writer">Writer</a></pre>

NewWriter returns a new Writer that writes to w.

<a id="example_NewWriter"></a>
Example:

    w := quotedprintable.NewWriter(os.Stdout)
    w.Write([]byte("These symbols will be escaped: = \t"))
    w.Close()

    // Output:
    // These symbols will be escaped: =3D =09

<h3 id="Writer.Close">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/mime/quotedprintable/writer.go#L57">Close</a>
    <a href="#Writer.Close">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the Writer, flushing any unwritten data to the underlying
io.Writer, but does not close the underlying io.Writer.

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/mime/quotedprintable/writer.go#L21">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Write(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write encodes p using quoted-printable encoding and writes it to the underlying
io.Writer. It limits line length to 76 characters. The encoded bytes are not
necessarily flushed until the Writer is closed.


