version: 1.9.2
## package csv

  `import "encoding/csv"`

## Overview

Package csv reads and writes comma-separated values (CSV) files. There are many
kinds of CSV files; this package supports the format described in RFC 4180.

A csv file contains zero or more records of one or more fields per record. Each
record is separated by the newline character. The final record may optionally be
followed by a newline character.

    field1,field2,field3

White space is considered part of a field.

Carriage returns before newline characters are silently removed.

Blank lines are ignored. A line with only whitespace characters (excluding the
ending newline character) is not considered a blank line.

Fields which start and stop with the quote character " are called quoted-fields.
The beginning and ending quote are not part of the field.

The source:

    normal string,"quoted-field"

results in the fields

    {`normal string`, `quoted-field`}

Within a quoted-field a quote character followed by a second quote character is
considered a single quote.

    "the ""word"" is true","a ""quoted-field"""

results in

    {`the "word" is true`, `a "quoted-field"`}

Newlines and commas may be included in a quoted-field

    "Multi-line
    field","comma is ,"

results in

    {`Multi-line
    field`, `comma is ,`}

## Index

- [Variables](#pkg-variables)
- [type ParseError](#ParseError)
  - [func (e *ParseError) Error() string](#ParseError.Error)
- [type Reader](#Reader)
  - [func NewReader(r io.Reader) *Reader](#NewReader)
  - [func (r *Reader) Read() (record []string, err error)](#Reader.Read)
  - [func (r *Reader) ReadAll() (records [][]string, err error)](#Reader.ReadAll)
- [type Writer](#Writer)
  - [func NewWriter(w io.Writer) *Writer](#NewWriter)
  - [func (w *Writer) Error() error](#Writer.Error)
  - [func (w *Writer) Flush()](#Writer.Flush)
  - [func (w *Writer) Write(record []string) error](#Writer.Write)
  - [func (w *Writer) WriteAll(records [][]string) error](#Writer.WriteAll)

### Examples

- [Reader](#exampleReader)
- [Reader.ReadAll](#exampleReader_ReadAll)
- [Reader (Options)](#exampleReader_options)
- [Writer](#exampleWriter)
- [Writer.WriteAll](#exampleWriter_WriteAll)

### Package files
 [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/reader.go) [writer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/writer.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="ErrTrailingComma">ErrTrailingComma</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;extra delimiter at end of line&#34;) <span class="comment">// no longer used</span>
    <span id="ErrBareQuote">ErrBareQuote</span>     = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;bare \&#34; in non-quoted-field&#34;)
    <span id="ErrQuote">ErrQuote</span>         = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;extraneous \&#34; in field&#34;)
    <span id="ErrFieldCount">ErrFieldCount</span>    = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;wrong number of fields in line&#34;)
)</pre>

These are the errors that can be returned in ParseError.Error

<h2 id="ParseError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/reader.go#L55">ParseError</a>
    <a href="#ParseError">¶</a></h2>
<pre>type ParseError struct {
<span id="ParseError.Line"></span>    Line   <a href="/builtin/#int">int</a>   <span class="comment">// Line where the error occurred</span>
<span id="ParseError.Column"></span>    Column <a href="/builtin/#int">int</a>   <span class="comment">// Column (rune index) where the error occurred</span>
<span id="ParseError.Err"></span>    Err    <a href="/builtin/#error">error</a> <span class="comment">// The actual error</span>
}</pre>

A ParseError is returned for parsing errors. The first line is 1. The first
column is 0.

<h3 id="ParseError.Error">func (*ParseError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/reader.go#L61">Error</a>
    <a href="#ParseError.Error">¶</a></h3>
<pre>func (e *<a href="#ParseError">ParseError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/reader.go#L80">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
<span id="Reader.Comma"></span>    <span class="comment">// Comma is the field delimiter.</span>
    <span class="comment">// It is set to comma (&#39;,&#39;) by NewReader.</span>
    Comma <a href="/builtin/#rune">rune</a>
<span id="Reader.Comment"></span>    <span class="comment">// Comment, if not 0, is the comment character. Lines beginning with the</span>
    <span class="comment">// Comment character without preceding whitespace are ignored.</span>
    <span class="comment">// With leading whitespace the Comment character becomes part of the</span>
    <span class="comment">// field, even if TrimLeadingSpace is true.</span>
    Comment <a href="/builtin/#rune">rune</a>
<span id="Reader.FieldsPerRecord"></span>    <span class="comment">// FieldsPerRecord is the number of expected fields per record.</span>
    <span class="comment">// If FieldsPerRecord is positive, Read requires each record to</span>
    <span class="comment">// have the given number of fields. If FieldsPerRecord is 0, Read sets it to</span>
    <span class="comment">// the number of fields in the first record, so that future records must</span>
    <span class="comment">// have the same field count. If FieldsPerRecord is negative, no check is</span>
    <span class="comment">// made and records may have a variable number of fields.</span>
    FieldsPerRecord <a href="/builtin/#int">int</a>
    <span class="comment">// If LazyQuotes is true, a quote may appear in an unquoted field and a</span>
    <span class="comment">// non-doubled quote may appear in a quoted field.</span>
<span id="Reader.LazyQuotes"></span>    LazyQuotes    <a href="/builtin/#bool">bool</a>
<span id="Reader.TrailingComma"></span>    TrailingComma <a href="/builtin/#bool">bool</a> <span class="comment">// ignored; here for backwards compatibility</span>
    <span class="comment">// If TrimLeadingSpace is true, leading white space in a field is ignored.</span>
    <span class="comment">// This is done even if the field delimiter, Comma, is white space.</span>
<span id="Reader.TrimLeadingSpace"></span>    TrimLeadingSpace <a href="/builtin/#bool">bool</a>
<span id="Reader.ReuseRecord"></span>    <span class="comment">// ReuseRecord controls whether calls to Read may return a slice sharing</span>
    <span class="comment">// the backing array of the previous call&#39;s returned slice for performance.</span>
    <span class="comment">// By default, each call to Read returns newly allocated memory owned by the caller.</span>
    ReuseRecord <a href="/builtin/#bool">bool</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Reader reads records from a CSV-encoded file.

As returned by NewReader, a Reader expects input conforming to RFC 4180. The
exported fields can be changed to customize the details before the first call to
Read or ReadAll.

<a id="exampleReader"></a>
Example:

    in := `first_name,last_name,username
    "Rob","Pike",rob
    Ken,Thompson,ken
    "Robert","Griesemer","gri"
    `
    r := csv.NewReader(strings.NewReader(in))

    for {
        record, err := r.Read()
        if err == io.EOF {
            break
        }
        if err != nil {
            log.Fatal(err)
        }

        fmt.Println(record)
    }
    // Output:
    // [first_name last_name username]
    // [Rob Pike rob]
    // [Ken Thompson ken]
    // [Robert Griesemer gri]


<a id="exampleReader_options"></a>
Example:

    in := `first_name;last_name;username
    "Rob";"Pike";rob
    # lines beginning with a # character are ignored
    Ken;Thompson;ken
    "Robert";"Griesemer";"gri"
    `
    r := csv.NewReader(strings.NewReader(in))
    r.Comma = ';'
    r.Comment = '#'

    records, err := r.ReadAll()
    if err != nil {
        log.Fatal(err)
    }

    fmt.Print(records)
    // Output:
    // [[first_name last_name username] [Rob Pike rob] [Ken Thompson ken] [Robert Griesemer gri]]

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/reader.go#L125">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Reader">Reader</a></pre>

NewReader returns a new Reader that reads from r.

<h3 id="Reader.Read">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/reader.go#L149">Read</a>
    <a href="#Reader.Read">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Read() (record []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

Read reads one record (a slice of fields) from r. If the record has an
unexpected number of fields, Read returns the record along with the error
ErrFieldCount. Except for that case, Read always returns either a non-nil record
or a non-nil error, but not both. If there is no data left to be read, Read
returns nil, io.EOF. If ReuseRecord is true, the returned slice may be shared
between multiple calls to Read.

<h3 id="Reader.ReadAll">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/reader.go#L165">ReadAll</a>
    <a href="#Reader.ReadAll">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadAll() (records [][]<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadAll reads all the remaining records from r. Each record is a slice of
fields. A successful call returns err == nil, not err == io.EOF. Because ReadAll
is defined to read until EOF, it does not treat end of file as an error to be
reported.

<a id="exampleReader_ReadAll"></a>
Example:

    in := `first_name,last_name,username
    "Rob","Pike",rob
    Ken,Thompson,ken
    "Robert","Griesemer","gri"
    `
    r := csv.NewReader(strings.NewReader(in))

    records, err := r.ReadAll()
    if err != nil {
        log.Fatal(err)
    }

    fmt.Print(records)
    // Output:
    // [[first_name last_name username] [Rob Pike rob] [Ken Thompson ken] [Robert Griesemer gri]]

<h2 id="Writer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/writer.go#L14">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
<span id="Writer.Comma"></span>    Comma   <a href="/builtin/#rune">rune</a> <span class="comment">// Field delimiter (set to &#39;,&#39; by NewWriter)</span>
<span id="Writer.UseCRLF"></span>    UseCRLF <a href="/builtin/#bool">bool</a> <span class="comment">// True to use \r\n as the line terminator</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Writer writes records to a CSV encoded file.

As returned by NewWriter, a Writer writes records terminated by a newline and
uses ',' as the field delimiter. The exported fields can be changed to customize
the details before the first call to Write or WriteAll.

Comma is the field delimiter.

If UseCRLF is true, the Writer ends each record with \r\n instead of \n.

<a id="exampleWriter"></a>
Example:

    records := [][]string{
        {"first_name", "last_name", "username"},
        {"Rob", "Pike", "rob"},
        {"Ken", "Thompson", "ken"},
        {"Robert", "Griesemer", "gri"},
    }

    w := csv.NewWriter(os.Stdout)

    for _, record := range records {
        if err := w.Write(record); err != nil {
            log.Fatalln("error writing record to csv:", err)
        }
    }

    // Write any buffered data to the underlying writer (standard output).
    w.Flush()

    if err := w.Error(); err != nil {
        log.Fatal(err)
    }
    // Output:
    // first_name,last_name,username
    // Rob,Pike,rob
    // Ken,Thompson,ken
    // Robert,Griesemer,gri

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/writer.go#L21">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) *<a href="#Writer">Writer</a></pre>

NewWriter returns a new Writer that writes to w.

<h3 id="Writer.Error">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/writer.go#L93">Error</a>
    <a href="#Writer.Error">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Error() <a href="/builtin/#error">error</a></pre>

Error reports any error that has occurred during a previous Write or Flush.

<h3 id="Writer.Flush">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/writer.go#L88">Flush</a>
    <a href="#Writer.Flush">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Flush()</pre>

Flush writes any buffered data to the underlying io.Writer. To check if an error
occurred during the Flush, call Error.

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/writer.go#L30">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Write(record []<a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Writer writes a single CSV record to w along with any necessary quoting. A
record is a slice of strings with each string being one field.

<h3 id="Writer.WriteAll">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/csv/writer.go#L99">WriteAll</a>
    <a href="#Writer.WriteAll">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) WriteAll(records [][]<a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

WriteAll writes multiple CSV records to w using Write and then calls Flush.

<a id="exampleWriter_WriteAll"></a>
Example:

    records := [][]string{
        {"first_name", "last_name", "username"},
        {"Rob", "Pike", "rob"},
        {"Ken", "Thompson", "ken"},
        {"Robert", "Griesemer", "gri"},
    }

    w := csv.NewWriter(os.Stdout)
    w.WriteAll(records) // calls Flush internally

    if err := w.Error(); err != nil {
        log.Fatalln("error writing csv:", err)
    }
    // Output:
    // first_name,last_name,username
    // Rob,Pike,rob
    // Ken,Thompson,ken
    // Robert,Griesemer,gri


