version: 1.10
## package tar

  `import "archive/tar"`

## Overview

Package tar implements access to tar archives.

Tape archives (tar) are a file format for storing a sequence of files that can
be read and written in a streaming manner. This package aims to cover most
variations of the format, including those produced by GNU and BSD tar tools.

<a id="example_minimal"></a>
Example:

    // Create and add some files to the archive.
    var buf bytes.Buffer
    tw := tar.NewWriter(&buf)
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
            log.Fatal(err)
        }
        if _, err := tw.Write([]byte(file.Body)); err != nil {
            log.Fatal(err)
        }
    }
    if err := tw.Close(); err != nil {
        log.Fatal(err)
    }

    // Open and iterate through the files in the archive.
    tr := tar.NewReader(&buf)
    for {
        hdr, err := tr.Next()
        if err == io.EOF {
            break // End of archive
        }
        if err != nil {
            log.Fatal(err)
        }
        fmt.Printf("Contents of %s:\n", hdr.Name)
        if _, err := io.Copy(os.Stdout, tr); err != nil {
            log.Fatal(err)
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
- [type Format](#Format)
  - [func (f Format) String() string](#Format.String)
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
  - [func (tw *Writer) Write(b []byte) (int, error)](#Writer.Write)
  - [func (tw *Writer) WriteHeader(hdr *Header) error](#Writer.WriteHeader)

### Examples

- [Package (Minimal)](#example_minimal)

### Package files
 [common.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/common.go) [format.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/format.go) [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/reader.go) [stat_actime1.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/stat_actime1.go) [stat_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/stat_unix.go) [strconv.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/strconv.go) [writer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// Type &#39;0&#39; indicates a regular file.</span>
    <span id="TypeReg">TypeReg</span>  = &#39;0&#39;
    <span id="TypeRegA">TypeRegA</span> = &#39;\x00&#39; <span class="comment">// For legacy support; use TypeReg instead</span>

    <span class="comment">// Type &#39;1&#39; to &#39;6&#39; are header-only flags and may not have a data body.</span>
    <span id="TypeLink">TypeLink</span>    = &#39;1&#39; <span class="comment">// Hard link</span>
    <span id="TypeSymlink">TypeSymlink</span> = &#39;2&#39; <span class="comment">// Symbolic link</span>
    <span id="TypeChar">TypeChar</span>    = &#39;3&#39; <span class="comment">// Character device node</span>
    <span id="TypeBlock">TypeBlock</span>   = &#39;4&#39; <span class="comment">// Block device node</span>
    <span id="TypeDir">TypeDir</span>     = &#39;5&#39; <span class="comment">// Directory</span>
    <span id="TypeFifo">TypeFifo</span>    = &#39;6&#39; <span class="comment">// FIFO node</span>

    <span class="comment">// Type &#39;7&#39; is reserved.</span>
    <span id="TypeCont">TypeCont</span> = &#39;7&#39;

    <span class="comment">// Type &#39;x&#39; is used by the PAX format to store key-value records that</span>
    <span class="comment">// are only relevant to the next file.</span>
    <span class="comment">// This package transparently handles these types.</span>
    <span id="TypeXHeader">TypeXHeader</span> = &#39;x&#39;

    <span class="comment">// Type &#39;g&#39; is used by the PAX format to store key-value records that</span>
    <span class="comment">// are relevant to all subsequent files.</span>
    <span class="comment">// This package only supports parsing and composing such headers,</span>
    <span class="comment">// but does not currently support persisting the global state across files.</span>
    <span id="TypeXGlobalHeader">TypeXGlobalHeader</span> = &#39;g&#39;

    <span class="comment">// Type &#39;S&#39; indicates a sparse file in the GNU format.</span>
    <span id="TypeGNUSparse">TypeGNUSparse</span> = &#39;S&#39;

    <span class="comment">// Types &#39;L&#39; and &#39;K&#39; are used by the GNU format for a meta file</span>
    <span class="comment">// used to store the path or link name for the next file.</span>
    <span class="comment">// This package transparently handles these types.</span>
    <span id="TypeGNULongName">TypeGNULongName</span> = &#39;L&#39;
    <span id="TypeGNULongLink">TypeGNULongLink</span> = &#39;K&#39;
)</pre>

Type flags for Header.Typeflag.

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="ErrHeader">ErrHeader</span>          = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;archive/tar: invalid tar header&#34;)
    <span id="ErrWriteTooLong">ErrWriteTooLong</span>    = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;archive/tar: write too long&#34;)
    <span id="ErrFieldTooLong">ErrFieldTooLong</span>    = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;archive/tar: header field too long&#34;)
    <span id="ErrWriteAfterClose">ErrWriteAfterClose</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;archive/tar: write after close&#34;)
)</pre>


<h2 id="Format">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/format.go#L36">Format</a>
    <a href="#Format">¶</a></h2>
<pre>type Format <a href="/builtin/#int">int</a></pre>

Format represents the tar archive format.

The original tar format was introduced in Unix V7. Since then, there have been
multiple competing formats attempting to standardize or extend the V7 format to
overcome its limitations. The most common formats are the USTAR, PAX, and GNU
formats, each with their own advantages and limitations.

The following table captures the capabilities of each format:

                      |  USTAR |       PAX |       GNU
    ------------------+--------+-----------+----------
    Name              |   256B | unlimited | unlimited
    Linkname          |   100B | unlimited | unlimited
    Size              | uint33 | unlimited |    uint89
    Mode              | uint21 |    uint21 |    uint57
    Uid/Gid           | uint21 | unlimited |    uint57
    Uname/Gname       |    32B | unlimited |       32B
    ModTime           | uint33 | unlimited |     int89
    AccessTime        |    n/a | unlimited |     int89
    ChangeTime        |    n/a | unlimited |     int89
    Devmajor/Devminor | uint21 |    uint21 |    uint57
    ------------------+--------+-----------+----------
    string encoding   |  ASCII |     UTF-8 |    binary
    sub-second times  |     no |       yes |        no
    sparse files      |     no |       yes |       yes

The table's upper portion shows the Header fields, where each format reports the
maximum number of bytes allowed for each string field and the integer type used
to store each numeric field (where timestamps are stored as the number of
seconds since the Unix epoch).

The table's lower portion shows specialized features of each format, such as
supported string encodings, support for sub-second timestamps, or support for
sparse files.

The Writer currently provides no support for sparse files.

<pre>const (

    <span class="comment">// FormatUnknown indicates that the format is unknown.</span>
    <span id="FormatUnknown">FormatUnknown</span> <a href="#Format">Format</a>

    <span class="comment">// FormatUSTAR represents the USTAR header format defined in POSIX.1-1988.</span>
    <span class="comment">//</span>
    <span class="comment">// While this format is compatible with most tar readers,</span>
    <span class="comment">// the format has several limitations making it unsuitable for some usages.</span>
    <span class="comment">// Most notably, it cannot support sparse files, files larger than 8GiB,</span>
    <span class="comment">// filenames larger than 256 characters, and non-ASCII filenames.</span>
    <span class="comment">//</span>
    <span class="comment">// Reference:</span>
    <span class="comment">//	http://pubs.opengroup.org/onlinepubs/9699919799/utilities/pax.html#tag_20_92_13_06</span>
    <span id="FormatUSTAR">FormatUSTAR</span>

    <span class="comment">// FormatPAX represents the PAX header format defined in POSIX.1-2001.</span>
    <span class="comment">//</span>
    <span class="comment">// PAX extends USTAR by writing a special file with Typeflag TypeXHeader</span>
    <span class="comment">// preceding the original header. This file contains a set of key-value</span>
    <span class="comment">// records, which are used to overcome USTAR&#39;s shortcomings, in addition to</span>
    <span class="comment">// providing the ability to have sub-second resolution for timestamps.</span>
    <span class="comment">//</span>
    <span class="comment">// Some newer formats add their own extensions to PAX by defining their</span>
    <span class="comment">// own keys and assigning certain semantic meaning to the associated values.</span>
    <span class="comment">// For example, sparse file support in PAX is implemented using keys</span>
    <span class="comment">// defined by the GNU manual (e.g., &#34;GNU.sparse.map&#34;).</span>
    <span class="comment">//</span>
    <span class="comment">// Reference:</span>
    <span class="comment">//	http://pubs.opengroup.org/onlinepubs/009695399/utilities/pax.html</span>
    <span id="FormatPAX">FormatPAX</span>

    <span class="comment">// FormatGNU represents the GNU header format.</span>
    <span class="comment">//</span>
    <span class="comment">// The GNU header format is older than the USTAR and PAX standards and</span>
    <span class="comment">// is not compatible with them. The GNU format supports</span>
    <span class="comment">// arbitrary file sizes, filenames of arbitrary encoding and length,</span>
    <span class="comment">// sparse files, and other features.</span>
    <span class="comment">//</span>
    <span class="comment">// It is recommended that PAX be chosen over GNU unless the target</span>
    <span class="comment">// application can only parse GNU formatted archives.</span>
    <span class="comment">//</span>
    <span class="comment">// Reference:</span>
    <span class="comment">//	http://www.gnu.org/software/tar/manual/html_node/Standard.html</span>
    <span id="FormatGNU">FormatGNU</span>
)</pre>

Constants to identify various tar formats.

<h3 id="Format.String">func (Format) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/format.go#L107">String</a>
    <a href="#Format.String">¶</a></h3>
<pre>func (f <a href="#Format">Format</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Header">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/common.go#L130">Header</a>
    <a href="#Header">¶</a></h2>
<pre>type Header struct {
<span id="Header.Typeflag"></span>    Typeflag <a href="/builtin/#byte">byte</a> <span class="comment">// Type of header entry (should be TypeReg for most files)</span>

<span id="Header.Name"></span>    Name     <a href="/builtin/#string">string</a> <span class="comment">// Name of file entry</span>
<span id="Header.Linkname"></span>    Linkname <a href="/builtin/#string">string</a> <span class="comment">// Target name of link (valid for TypeLink or TypeSymlink)</span>

<span id="Header.Size"></span>    Size  <a href="/builtin/#int64">int64</a>  <span class="comment">// Logical file size in bytes</span>
<span id="Header.Mode"></span>    Mode  <a href="/builtin/#int64">int64</a>  <span class="comment">// Permission and mode bits</span>
<span id="Header.Uid"></span>    Uid   <a href="/builtin/#int">int</a>    <span class="comment">// User ID of owner</span>
<span id="Header.Gid"></span>    Gid   <a href="/builtin/#int">int</a>    <span class="comment">// Group ID of owner</span>
<span id="Header.Uname"></span>    Uname <a href="/builtin/#string">string</a> <span class="comment">// User name of owner</span>
<span id="Header.Gname"></span>    Gname <a href="/builtin/#string">string</a> <span class="comment">// Group name of owner</span>

    <span class="comment">// If the Format is unspecified, then Writer.WriteHeader rounds ModTime</span>
    <span class="comment">// to the nearest second and ignores the AccessTime and ChangeTime fields.</span>
    <span class="comment">//</span>
    <span class="comment">// To use AccessTime or ChangeTime, specify the Format as PAX or GNU.</span>
    <span class="comment">// To use sub-second resolution, specify the Format as PAX.</span>
<span id="Header.ModTime"></span>    ModTime    <a href="/time/">time</a>.<a href="/time/#Time">Time</a> <span class="comment">// Modification time</span>
<span id="Header.AccessTime"></span>    AccessTime <a href="/time/">time</a>.<a href="/time/#Time">Time</a> <span class="comment">// Access time (requires either PAX or GNU support)</span>
<span id="Header.ChangeTime"></span>    ChangeTime <a href="/time/">time</a>.<a href="/time/#Time">Time</a> <span class="comment">// Change time (requires either PAX or GNU support)</span>

<span id="Header.Devmajor"></span>    Devmajor <a href="/builtin/#int64">int64</a> <span class="comment">// Major device number (valid for TypeChar or TypeBlock)</span>
<span id="Header.Devminor"></span>    Devminor <a href="/builtin/#int64">int64</a> <span class="comment">// Minor device number (valid for TypeChar or TypeBlock)</span>

<span id="Header.Xattrs"></span>    <span class="comment">// Xattrs stores extended attributes as PAX records under the</span>
    <span class="comment">// &#34;SCHILY.xattr.&#34; namespace.</span>
    <span class="comment">//</span>
    <span class="comment">// The following are semantically equivalent:</span>
    <span class="comment">//  h.Xattrs[key] = value</span>
    <span class="comment">//  h.PAXRecords[&#34;SCHILY.xattr.&#34;+key] = value</span>
    <span class="comment">//</span>
    <span class="comment">// When Writer.WriteHeader is called, the contents of Xattrs will take</span>
    <span class="comment">// precedence over those in PAXRecords.</span>
    <span class="comment">//</span>
    <span class="comment">// Deprecated: Use PAXRecords instead.</span>
    Xattrs map[<a href="/builtin/#string">string</a>]<a href="/builtin/#string">string</a>

<span id="Header.PAXRecords"></span>    <span class="comment">// PAXRecords is a map of PAX extended header records.</span>
    <span class="comment">//</span>
    <span class="comment">// User-defined records should have keys of the following form:</span>
    <span class="comment">//	VENDOR.keyword</span>
    <span class="comment">// Where VENDOR is some namespace in all uppercase, and keyword may</span>
    <span class="comment">// not contain the &#39;=&#39; character (e.g., &#34;GOLANG.pkg.version&#34;).</span>
    <span class="comment">// The key and value should be non-empty UTF-8 strings.</span>
    <span class="comment">//</span>
    <span class="comment">// When Writer.WriteHeader is called, PAX records derived from the</span>
    <span class="comment">// the other fields in Header take precedence over PAXRecords.</span>
    PAXRecords map[<a href="/builtin/#string">string</a>]<a href="/builtin/#string">string</a>

<span id="Header.Format"></span>    <span class="comment">// Format specifies the format of the tar header.</span>
    <span class="comment">//</span>
    <span class="comment">// This is set by Reader.Next as a best-effort guess at the format.</span>
    <span class="comment">// Since the Reader liberally reads some non-compliant files,</span>
    <span class="comment">// it is possible for this to be FormatUnknown.</span>
    <span class="comment">//</span>
    <span class="comment">// If the format is unspecified when Writer.WriteHeader is called,</span>
    <span class="comment">// then it uses the first format (in the order of USTAR, PAX, GNU)</span>
    <span class="comment">// capable of encoding this Header (see Format).</span>
    Format <a href="#Format">Format</a>
}</pre>

A Header represents a single header in a tar archive. Some fields may not be
populated.

For forward compatibility, users that retrieve a Header from Reader.Next, mutate
it in some ways, and then pass it back to Writer.WriteHeader should do so by
creating a new Header and copying the fields that they are interested in
preserving.

<h3 id="FileInfoHeader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/common.go#L616">FileInfoHeader</a>
    <a href="#FileInfoHeader">¶</a></h3>
<pre>func FileInfoHeader(fi <a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a>, link <a href="/builtin/#string">string</a>) (*<a href="#Header">Header</a>, <a href="/builtin/#error">error</a>)</pre>

FileInfoHeader creates a partially-populated Header from fi. If fi describes a
symlink, FileInfoHeader records link as the link target. If fi describes a
directory, a slash is appended to the name.

Since os.FileInfo's Name method only returns the base name of the file it
describes, it may be necessary to modify Header.Name to provide the full path
name of the file.

<h3 id="Header.FileInfo">func (*Header) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/common.go#L516">FileInfo</a>
    <a href="#Header.FileInfo">¶</a></h3>
<pre>func (h *<a href="#Header">Header</a>) FileInfo() <a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a></pre>

FileInfo returns an os.FileInfo for the Header.

<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/reader.go#L9">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Reader provides sequential access to the contents of a tar archive. Reader.Next
advances to the next file in the archive (including the first), and then Reader
can be treated as an io.Reader to access the file's data.

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/reader.go#L29">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Reader">Reader</a></pre>

NewReader creates a new Reader reading from r.

<h3 id="Reader.Next">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/reader.go#L38">Next</a>
    <a href="#Reader.Next">¶</a></h3>
<pre>func (tr *<a href="#Reader">Reader</a>) Next() (*<a href="#Header">Header</a>, <a href="/builtin/#error">error</a>)</pre>

Next advances to the next entry in the tar archive. The Header.Size determines
how many bytes can be read for the next file. Any remaining data in the current
file is automatically discarded.

io.EOF is returned at the end of the input.

<h3 id="Reader.Read">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/reader.go#L609">Read</a>
    <a href="#Reader.Read">¶</a></h3>
<pre>func (tr *<a href="#Reader">Reader</a>) Read(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Read reads from the current file in the tar archive. It returns (0, io.EOF) when
it reaches the end of that file, until Next is called to advance to the next
file.

If the current file is sparse, then the regions marked as a hole are read back
as NUL-bytes.

Calling Read on special types like TypeLink, TypeSymlink, TypeChar, TypeBlock,
TypeDir, and TypeFifo returns (0, io.EOF) regardless of what the Header.Size
claims.

<h2 id="Writer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L10">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Writer provides sequential writing of a tar archive. Write.WriteHeader begins a
new file with the provided Header, and then Writer can be treated as an
io.Writer to supply that file's data.

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L24">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) *<a href="#Writer">Writer</a></pre>

NewWriter creates a new Writer writing to w.

<h3 id="Writer.Close">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L447">Close</a>
    <a href="#Writer.Close">¶</a></h3>
<pre>func (tw *<a href="#Writer">Writer</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the tar archive by flushing the padding, and writing the footer. If
the current file (from a prior call to WriteHeader) is not fully written, then
this returns an error.

<h3 id="Writer.Flush">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L40">Flush</a>
    <a href="#Writer.Flush">¶</a></h3>
<pre>func (tw *<a href="#Writer">Writer</a>) Flush() <a href="/builtin/#error">error</a></pre>

Flush finishes writing the current file's block padding. The current file must
be fully written before Flush can be called.

This is unnecessary as the next call to WriteHeader or Close will implicitly
flush out the file's padding.

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L412">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (tw *<a href="#Writer">Writer</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write writes to the current file in the tar archive. Write returns the error
ErrWriteTooLong if more than Header.Size bytes are written after WriteHeader.

Calling Write on special types like TypeLink, TypeSymlink, TypeChar, TypeBlock,
TypeDir, and TypeFifo returns (0, ErrWriteTooLong) regardless of what the
Header.Size claims.

<h3 id="Writer.WriteHeader">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/archive/tar/writer.go#L58">WriteHeader</a>
    <a href="#Writer.WriteHeader">¶</a></h3>
<pre>func (tw *<a href="#Writer">Writer</a>) WriteHeader(hdr *<a href="#Header">Header</a>) <a href="/builtin/#error">error</a></pre>

WriteHeader writes hdr and prepares to accept the file's contents. The
Header.Size determines how many bytes can be written for the next file. If the
current file is not fully written, then this returns an error. This implicitly
flushes any padding necessary before writing the header.


