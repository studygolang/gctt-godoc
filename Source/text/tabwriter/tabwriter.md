version: 1.9.2
## package tabwriter

  `import "text/tabwriter"`

## Overview

Package tabwriter implements a write filter (tabwriter.Writer) that translates
tabbed columns in input into properly aligned text.

The package is using the Elastic Tabstops algorithm described at
http://nickgravgaard.com/elastictabstops/index.html.

The text/tabwriter package is frozen and is not accepting new features.

<a id="example__elastic"></a>
Example:

    // Observe how the b's and the d's, despite appearing in the
    // second cell of each line, belong to different columns.
    w := tabwriter.NewWriter(os.Stdout, 0, 0, 1, '.', tabwriter.AlignRight|tabwriter.Debug)
    fmt.Fprintln(w, "a\tb\tc")
    fmt.Fprintln(w, "aa\tbb\tcc")
    fmt.Fprintln(w, "aaa\t") // trailing tab
    fmt.Fprintln(w, "aaaa\tdddd\teeee")
    w.Flush()

    // output:
    // ....a|..b|c
    // ...aa|.bb|cc
    // ..aaa|
    // .aaaa|.dddd|eeee


<a id="example__trailingTab"></a>
Example:

    // Observe that the third line has no trailing tab,
    // so its final cell is not part of an aligned column.
    const padding = 3
    w := tabwriter.NewWriter(os.Stdout, 0, 0, padding, '-', tabwriter.AlignRight|tabwriter.Debug)
    fmt.Fprintln(w, "a\tb\taligned\t")
    fmt.Fprintln(w, "aa\tbb\taligned\t")
    fmt.Fprintln(w, "aaa\tbbb\tunaligned") // no trailing tab
    fmt.Fprintln(w, "aaaa\tbbbb\taligned\t")
    w.Flush()

    // output:
    // ------a|------b|---aligned|
    // -----aa|-----bb|---aligned|
    // ----aaa|----bbb|unaligned
    // ---aaaa|---bbbb|---aligned|

## Index

- [Constants](#pkg-constants)
- [type Writer](#Writer)
  - [func NewWriter(output io.Writer, minwidth, tabwidth, padding int, padchar byte, flags uint) *Writer](#NewWriter)
  - [func (b *Writer) Flush() error](#Writer.Flush)
  - [func (b *Writer) Init(output io.Writer, minwidth, tabwidth, padding int, padchar byte, flags uint) *Writer](#Writer.Init)
  - [func (b *Writer) Write(buf []byte) (n int, err error)](#Writer.Write)

### Examples

- [Writer.Init](#example_Writer_Init)
- [Package (Elastic)](#example__elastic)
- [Package (TrailingTab)](#example__trailingTab)

### Package files
 [tabwriter.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/tabwriter/tabwriter.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// Ignore html tags and treat entities (starting with &#39;&amp;&#39;</span>
    <span class="comment">// and ending in &#39;;&#39;) as single characters (width = 1).</span>
    <span id="FilterHTML">FilterHTML</span> <a href="/builtin/#uint">uint</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a>

    <span class="comment">// Strip Escape characters bracketing escaped text segments</span>
    <span class="comment">// instead of passing them through unchanged with the text.</span>
    <span id="StripEscape">StripEscape</span>

    <span class="comment">// Force right-alignment of cell content.</span>
    <span class="comment">// Default is left-alignment.</span>
    <span id="AlignRight">AlignRight</span>

    <span class="comment">// Handle empty columns as if they were not present in</span>
    <span class="comment">// the input in the first place.</span>
    <span id="DiscardEmptyColumns">DiscardEmptyColumns</span>

    <span class="comment">// Always use tabs for indentation columns (i.e., padding of</span>
    <span class="comment">// leading empty cells on the left) independent of padchar.</span>
    <span id="TabIndent">TabIndent</span>

    <span class="comment">// Print a vertical bar (&#39;|&#39;) between columns (after formatting).</span>
    <span class="comment">// Discarded columns appear as zero-width columns (&#34;||&#34;).</span>
    <span id="Debug">Debug</span>
)</pre>

Formatting can be controlled with these flags.

<pre>const <span id="Escape">Escape</span> = &#39;\xff&#39;</pre>

To escape a text segment, bracket it with Escape characters. For instance, the
tab in this string "Ignore this tab: \xff\t\xff" does not terminate a cell and
constitutes a single character of width one for formatting purposes.

The value 0xff was chosen because it cannot appear in a valid UTF-8 sequence.

<h2 id="Writer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/tabwriter/tabwriter.go#L82">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Writer is a filter that inserts padding around tab-delimited columns in its
input to align them in the output.

The Writer treats incoming bytes as UTF-8-encoded text consisting of cells
terminated by horizontal ('\t') or vertical ('\v') tabs, and newline ('\n') or
formfeed ('\f') characters; both newline and formfeed act as line breaks.

Tab-terminated cells in contiguous lines constitute a column. The Writer inserts
padding as needed to make all cells in a column have the same width, effectively
aligning the columns. It assumes that all characters have the same width, except
for tabs for which a tabwidth must be specified. Column cells must be
tab-terminated, not tab-separated: non-tab terminated trailing text at the end
of a line forms a cell but that cell is not part of an aligned column. For
instance, in this example (where | stands for a horizontal tab):

    aaaa|bbb|d
    aa  |b  |dd
    a   |
    aa  |cccc|eee

the b and c are in distinct columns (the b column is not contiguous all the
way). The d and e are not in a column at all (there's no terminating tab, nor
would the column be contiguous).

The Writer assumes that all Unicode code points have the same width; this may
not be true in some fonts or if the string contains combining characters.

If DiscardEmptyColumns is set, empty columns that are terminated entirely by
vertical (or "soft") tabs are discarded. Columns terminated by horizontal (or
"hard") tabs are not affected by this flag.

If a Writer is configured to filter HTML, HTML tags and entities are passed
through. The widths of tags and entities are assumed to be zero (tags) and one
(entities) for formatting purposes.

A segment of text may be escaped by bracketing it with Escape characters. The
tabwriter passes escaped text segments through unchanged. In particular, it does
not interpret any tabs or line breaks within the segment. If the StripEscape
flag is set, the Escape characters are stripped from the output; otherwise they
are passed through as well. For the purpose of formatting, the width of the
escaped text is always computed excluding the Escape characters.

The formfeed character acts like a newline but it also terminates all columns in
the current line (effectively calling Flush). Tab- terminated cells in the next
line start new columns. Unless found inside an HTML tag or inside an escaped
text segment, formfeed characters appear as newlines in the output.

The Writer must buffer input internally, because proper spacing of one line may
depend on the cells in future lines. Clients must call Flush when done calling
Write.

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/tabwriter/tabwriter.go#L563">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(output <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, minwidth, tabwidth, padding <a href="/builtin/#int">int</a>, padchar <a href="/builtin/#byte">byte</a>, flags <a href="/builtin/#uint">uint</a>) *<a href="#Writer">Writer</a></pre>

NewWriter allocates and initializes a new tabwriter.Writer. The parameters are
the same as for the Init function.

<h3 id="Writer.Flush">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/tabwriter/tabwriter.go#L456">Flush</a>
    <a href="#Writer.Flush">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Flush() <a href="/builtin/#error">error</a></pre>

Flush should be called after the last call to Write to ensure that any data
buffered in the Writer is written to output. Any incomplete escape sequence at
the end is considered complete for formatting purposes.

<h3 id="Writer.Init">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/tabwriter/tabwriter.go#L177">Init</a>
    <a href="#Writer.Init">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Init(output <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, minwidth, tabwidth, padding <a href="/builtin/#int">int</a>, padchar <a href="/builtin/#byte">byte</a>, flags <a href="/builtin/#uint">uint</a>) *<a href="#Writer">Writer</a></pre>

A Writer must be initialized with a call to Init. The first parameter (output)
specifies the filter output. The remaining parameters control the formatting:

    minwidth	minimal cell width including any padding
    tabwidth	width of tab characters (equivalent number of spaces)
    padding		padding added to a cell before computing its width
    padchar		ASCII char used for padding
    		if padchar == '\t', the Writer will assume that the
    		width of a '\t' in the formatted output is tabwidth,
    		and cells are left-aligned independent of align_left
    		(for correct-looking results, tabwidth must correspond
    		to the tab width in the viewer displaying the result)
    flags		formatting control

<a id="example_Writer_Init"></a>
Example:

    w := new(tabwriter.Writer)

    // Format in tab-separated columns with a tab stop of 8.
    w.Init(os.Stdout, 0, 8, 0, '\t', 0)
    fmt.Fprintln(w, "a\tb\tc\td\t.")
    fmt.Fprintln(w, "123\t12345\t1234567\t123456789\t.")
    fmt.Fprintln(w)
    w.Flush()

    // Format right-aligned in space-separated columns of minimal width 5
    // and at least one blank of padding (so wider column entries do not
    // touch each other).
    w.Init(os.Stdout, 5, 0, 1, ' ', tabwriter.AlignRight)
    fmt.Fprintln(w, "a\tb\tc\td\t.")
    fmt.Fprintln(w, "123\t12345\t1234567\t123456789\t.")
    fmt.Fprintln(w)
    w.Flush()

    // output:
    // a	b	c	d		.
    // 123	12345	1234567	123456789	.
    //
    //     a     b       c         d.
    //   123 12345 1234567 123456789.

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/tabwriter/tabwriter.go#L484">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (b *<a href="#Writer">Writer</a>) Write(buf []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write writes buf to the writer b. The only errors returned are ones encountered
while writing to the underlying output stream.


