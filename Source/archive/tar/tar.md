version: 1.9.2
## package tar

  `import "archive/tar"`

## Overview

Package tar implements access to tar archives. It aims to cover most of the
variations, including those produced by GNU and BSD tars.

References:

    http://www.freebsd.org/cgi/man.cgi?query=tar&sektion=5
    http://www.gnu.org/software/tar/manual/html_node/Standard.html
    http://pubs.opengroup.org/onlinepubs/9699919799/utilities/pax.html

<a id="example_"></a>
Example:

    // Create a buffer to write our archive to.
    buf := new(bytes.Buffer)

    // Create a new tar archive.
    tw := tar.NewWriter(buf)

    // Add some files to the archive.
    var files = []struct {
        Name, Body string
    }{
        {"readme.txt", "This archive contains some text files."},
        {"gopher.txt", "Gopher names:\nGeorge\nGeoffrey\nGonzo"},
        {"todo.txt", "Get animal handling license."},
    }
    for _, file := range files {
        hdr := &tar.Header{
            Name: file.Name,
            Mode: 0600,
            Size: int64(len(file.Body)),
        }
        if err := tw.WriteHeader(hdr); err != nil {
            log.Fatalln(err)
        }
        if _, err := tw.Write([]byte(file.Body)); err != nil {
            log.Fatalln(err)
        }
    }
    // Make sure to check the error on Close.
    if err := tw.Close(); err != nil {
        log.Fatalln(err)
    }

    // Open the tar archive for reading.
    r := bytes.NewReader(buf.Bytes())
    tr := tar.NewReader(r)

    // Iterate through the files in the archive.
    for {
        hdr, err := tr.Next()
        if err == io.EOF {
            // end of tar archive
            break
        }
        if err != nil {
            log.Fatalln(err)
        }
        fmt.Printf("Contents of %s:\n", hdr.Name)
        if _, err := io.Copy(os.Stdout, tr); err != nil {
            log.Fatalln(err)
        }
        fmt.Println()
    }

    // Output:
    // Contents of readme.txt:
    // This archive contains some text files.
    // Contents of gopher.txt:
    // Gopher names:
    // George
    // Geoffrey
    // Gonzo
    // Contents of todo.txt:
    // Get animal handling license.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [type Header](#Header)
  - [func FileInfoHeader(fi os.FileInfo, link string) (*Header, error)](#FileInfoHeader)
  - [func (h *Header) FileInfo() os.FileInfo](#Header.FileInfo)
- [type Reader](#Reader)
  - [func NewReader(r io.Reader) *Reader](#NewReader)
  - [func (tr *Reader) Next() (*Header, error)](#Reader.Next)
  - [func (tr *Reader) Read(b []byte) (int, error)](#Reader.Read)
- [type Writer](#Writer)
  - [func NewWriter(w io.Writer) *Writer](#NewWriter)
  - [func (tw *Writer) Close() error](#Writer.Close)
  - [func (tw *Writer) Flush() error](#Writer.Flush)
  - [func (tw *Writer) Write(b []byte) (n int, err error)](#Writer.Write)
  - [func (tw *Writer) WriteHeader(hdr *Header) error](#Writer.WriteHeader)

### Examples

- [Package](#example_)

### Package files
 [common.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/common.go) [format.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/format.go) [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/reader.go) [stat_atim.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/stat_atim.go) [stat_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/stat_unix.go) [strconv.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/strconv.go) [writer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="TypeReg">TypeReg</span>           = &#39;0&#39;    <span class="comment">// regular file</span>
    <span id="TypeRegA">TypeRegA</span>          = &#39;\x00&#39; <span class="comment">// regular file</span>
    <span id="TypeLink">TypeLink</span>          = &#39;1&#39;    <span class="comment">// hard link</span>
    <span id="TypeSymlink">TypeSymlink</span>       = &#39;2&#39;    <span class="comment">// symbolic link</span>
    <span id="TypeChar">TypeChar</span>          = &#39;3&#39;    <span class="comment">// character device node</span>
    <span id="TypeBlock">TypeBlock</span>         = &#39;4&#39;    <span class="comment">// block device node</span>
    <span id="TypeDir">TypeDir</span>           = &#39;5&#39;    <span class="comment">// directory</span>
    <span id="TypeFifo">TypeFifo</span>          = &#39;6&#39;    <span class="comment">// fifo node</span>
    <span id="TypeCont">TypeCont</span>          = &#39;7&#39;    <span class="comment">// reserved</span>
    <span id="TypeXHeader">TypeXHeader</span>       = &#39;x&#39;    <span class="comment">// extended header</span>
    <span id="TypeXGlobalHeader">TypeXGlobalHeader</span> = &#39;g&#39;    <span class="comment">// global extended header</span>
    <span id="TypeGNULongName">TypeGNULongName</span>   = &#39;L&#39;    <span class="comment">// Next file has a long name</span>
    <span id="TypeGNULongLink">TypeGNULongLink</span>   = &#39;K&#39;    <span class="comment">// Next file symlinks to a file w/ a long name</span>
    <span id="TypeGNUSparse">TypeGNUSparse</span>     = &#39;S&#39;    <span class="comment">// sparse file</span>
)</pre>

Header type flags.

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="ErrWriteTooLong">ErrWriteTooLong</span>    = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;archive/tar: write too long&#34;)
    <span id="ErrFieldTooLong">ErrFieldTooLong</span>    = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;archive/tar: header field too long&#34;)
    <span id="ErrWriteAfterClose">ErrWriteAfterClose</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;archive/tar: write after close&#34;)
)</pre>


<pre>var (
    <span id="ErrHeader">ErrHeader</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;archive/tar: invalid tar header&#34;)
)</pre>


<h2 id="Header">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/common.go#L37">Header</a>
    <a href="#Header">¶</a></h2>
<pre>type Header struct {
<span id="Header.Name"></span>    Name       <a href="/builtin/#string">string</a>    <span class="comment">// name of header file entry</span>
<span id="Header.Mode"></span>    Mode       <a href="/builtin/#int64">int64</a>     <span class="comment">// permission and mode bits</span>
<span id="Header.Uid"></span>    Uid        <a href="/builtin/#int">int</a>       <span class="comment">// user id of owner</span>
<span id="Header.Gid"></span>    Gid        <a href="/builtin/#int">int</a>       <span class="comment">// group id of owner</span>
<span id="Header.Size"></span>    Size       <a href="/builtin/#int64">int64</a>     <span class="comment">// length in bytes</span>
<span id="Header.ModTime"></span>    ModTime    <a href="/time/">time</a>.<a href="/time/#Time">Time</a> <span class="comment">// modified time</span>
<span id="Header.Typeflag"></span>    Typeflag   <a href="/builtin/#byte">byte</a>      <span class="comment">// type of header entry</span>
<span id="Header.Linkname"></span>    Linkname   <a href="/builtin/#string">string</a>    <span class="comment">// target name of link</span>
<span id="Header.Uname"></span>    Uname      <a href="/builtin/#string">string</a>    <span class="comment">// user name of owner</span>
<span id="Header.Gname"></span>    Gname      <a href="/builtin/#string">string</a>    <span class="comment">// group name of owner</span>
<span id="Header.Devmajor"></span>    Devmajor   <a href="/builtin/#int64">int64</a>     <span class="comment">// major number of character or block device</span>
<span id="Header.Devminor"></span>    Devminor   <a href="/builtin/#int64">int64</a>     <span class="comment">// minor number of character or block device</span>
<span id="Header.AccessTime"></span>    AccessTime <a href="/time/">time</a>.<a href="/time/#Time">Time</a> <span class="comment">// access time</span>
<span id="Header.ChangeTime"></span>    ChangeTime <a href="/time/">time</a>.<a href="/time/#Time">Time</a> <span class="comment">// status change time</span>
<span id="Header.Xattrs"></span>    Xattrs     map[<a href="/builtin/#string">string</a>]<a href="/builtin/#string">string</a>
}</pre>

A Header represents a single header in a tar archive. Some fields may not be
populated.

<h3 id="FileInfoHeader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/common.go#L193">FileInfoHeader</a>
    <a href="#FileInfoHeader">¶</a></h3>
<pre>func FileInfoHeader(fi <a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a>, link <a href="/builtin/#string">string</a>) (*<a href="#Header">Header</a>, <a href="/builtin/#error">error</a>)</pre>

FileInfoHeader creates a partially-populated Header from fi. If fi describes a
symlink, FileInfoHeader records link as the link target. If fi describes a
directory, a slash is appended to the name. Because os.FileInfo's Name method
returns only the base name of the file it describes, it may be necessary to
modify the Name field of the returned header to provide the full path name of
the file.

<h3 id="Header.FileInfo">func (*Header) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/common.go#L56">FileInfo</a>
    <a href="#Header.FileInfo">¶</a></h3>
<pre>func (h *<a href="#Header">Header</a>) FileInfo() <a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a></pre>

FileInfo returns an os.FileInfo for the Header.

<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/reader.go#L19">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Reader provides sequential access to the contents of a tar archive. A tar
archive consists of a sequence of files. The Next method advances to the next
file in the archive (including the first), and then it can be treated as an
io.Reader to access the file's data.

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/reader.go#L90">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Reader">Reader</a></pre>

NewReader creates a new Reader reading from r.

<h3 id="Reader.Next">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/reader.go#L95">Next</a>
    <a href="#Reader.Next">¶</a></h3>
<pre>func (tr *<a href="#Reader">Reader</a>) Next() (*<a href="#Header">Header</a>, <a href="/builtin/#error">error</a>)</pre>

Next advances to the next entry in the tar archive.

io.EOF is returned at the end of the input.

<h3 id="Reader.Read">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/reader.go#L685">Read</a>
    <a href="#Reader.Read">¶</a></h3>
<pre>func (tr *<a href="#Reader">Reader</a>) Read(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Read reads from the current entry in the tar archive. It returns 0, io.EOF when
it reaches the end of that entry, until Next is called to advance to the next
entry.

Calling Read on special types like TypeLink, TypeSymLink, TypeChar, TypeBlock,
TypeDir, and TypeFifo returns 0, io.EOF regardless of what the Header.Size
claims.

<h2 id="Writer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L23">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Writer provides sequential writing of a tar archive in POSIX.1 format. A tar
archive consists of a sequence of files. Call WriteHeader to begin a new file,
and then call Write to supply that file's data, writing at most hdr.Size bytes
in total.

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L36">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) *<a href="#Writer">Writer</a></pre>

NewWriter creates a new Writer writing to w.

<h3 id="Writer.Close">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L342">Close</a>
    <a href="#Writer.Close">¶</a></h3>
<pre>func (tw *<a href="#Writer">Writer</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the tar archive, flushing any unwritten data to the underlying
writer.

<h3 id="Writer.Flush">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L39">Flush</a>
    <a href="#Writer.Flush">¶</a></h3>
<pre>func (tw *<a href="#Writer">Writer</a>) Flush() <a href="/builtin/#error">error</a></pre>

Flush finishes writing the current file (optional).

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L320">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (tw *<a href="#Writer">Writer</a>) Write(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write writes to the current entry in the tar archive. Write returns the error
ErrWriteTooLong if more than hdr.Size bytes are written after WriteHeader.

<h3 id="Writer.WriteHeader">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L69">WriteHeader</a>
    <a href="#Writer.WriteHeader">¶</a></h3>
<pre>func (tw *<a href="#Writer">Writer</a>) WriteHeader(hdr *<a href="#Header">Header</a>) <a href="/builtin/#error">error</a></pre>

WriteHeader writes hdr and prepares to accept the file's contents. WriteHeader
calls Flush if it is not the first header. Calling after a Close will return
ErrWriteAfterClose.


