version: 1.9.2
## package doc

  `import "go/doc"`

## Overview

Package doc extracts source code documentation from a Go AST.

## Index

- [Variables](#pkg-variables)
- [func Examples(files ...*ast.File) []*Example](#Examples)
- [func IsPredeclared(s string) bool](#IsPredeclared)
- [func Synopsis(s string) string](#Synopsis)
- [func ToHTML(w io.Writer, text string, words map[string]string)](#ToHTML)
- [func ToText(w io.Writer, text string, indent, preIndent string, width int)](#ToText)
- [type Example](#Example)
- [type Filter](#Filter)
- [type Func](#Func)
- [type Mode](#Mode)
- [type Note](#Note)
- [type Package](#Package)
  - [func New(pkg *ast.Package, importPath string, mode Mode) *Package](#New)
  - [func (p *Package) Filter(f Filter)](#Package.Filter)
- [type Type](#Type)
- [type Value](#Value)

### Package files
 [comment.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/comment.go) [doc.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/doc.go) [example.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/example.go) [exports.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/exports.go) [filter.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/filter.go) [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/reader.go) [synopsis.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/synopsis.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="IllegalPrefixes">IllegalPrefixes</span> = []<a href="/builtin/#string">string</a>{
    &#34;copyright&#34;,
    &#34;all rights&#34;,
    &#34;author&#34;,
}</pre>


<h2 id="Examples">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/example.go#L37">Examples</a>
    <a href="#Examples">¶</a></h2>
<pre>func Examples(files ...*<a href="/go/ast/">ast</a>.<a href="/go/ast/#File">File</a>) []*<a href="#Example">Example</a></pre>

Examples returns the examples found in the files, sorted by Name field. The
Order fields record the order in which the examples were encountered.

Playable Examples must be in a package whose name ends in "_test". An Example is
"playable" (the Play field is non-nil) in either of these circumstances:

    - The example function is self-contained: the function references only
      identifiers from other packages (or predeclared identifiers, such as
      "int") and the test file does not include a dot import.
    - The entire test file is the example: the file contains exactly one
      example function, zero test or benchmark functions, and at least one
      top-level function, type, variable, or constant declaration other
      than the example function.

<h2 id="IsPredeclared">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/reader.go#L812">IsPredeclared</a>
    <a href="#IsPredeclared">¶</a></h2>
<pre>func IsPredeclared(s <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

IsPredeclared reports whether s is a predeclared identifier.

<h2 id="Synopsis">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/synopsis.go#L58">Synopsis</a>
    <a href="#Synopsis">¶</a></h2>
<pre>func Synopsis(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Synopsis returns a cleaned version of the first sentence in s. That sentence
ends after the first period followed by space and not preceded by exactly one
uppercase letter. The result string has no \n, \r, or \t characters and uses
only single spaces between words. If s starts with any of the IllegalPrefixes,
the result is the empty string.

<h2 id="ToHTML">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/comment.go#L286">ToHTML</a>
    <a href="#ToHTML">¶</a></h2>
<pre>func ToHTML(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, text <a href="/builtin/#string">string</a>, words map[<a href="/builtin/#string">string</a>]<a href="/builtin/#string">string</a>)</pre>

ToHTML converts comment text to formatted HTML. The comment was prepared by
DocReader, so it is known not to have leading, trailing blank lines nor to have
trailing spaces at the end of lines. The comment markers have already been
removed.

Each span of unindented non-blank lines is converted into a single paragraph.
There is one exception to the rule: a span that consists of a single line, is
followed by another paragraph span, begins with a capital letter, and contains
no punctuation is formatted as a heading.

A span of indented lines is converted into a <pre> block, with the common indent
prefix removed.

URLs in the comment text are converted into links; if the URL also appears in
the words map, the link is taken from the map (if the corresponding map value is
the empty string, the URL is not converted into a link).

Go identifiers that appear in the words map are italicized; if the corresponding
map value is not the empty string, it is considered a URL and the word is
converted into a link.

<h2 id="ToText">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/comment.go#L400">ToText</a>
    <a href="#ToText">¶</a></h2>
<pre>func ToText(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, text <a href="/builtin/#string">string</a>, indent, preIndent <a href="/builtin/#string">string</a>, width <a href="/builtin/#int">int</a>)</pre>

ToText prepares comment text for presentation in textual output. It wraps
paragraphs of text to width or fewer Unicode code points and then prefixes each
line with the indent. In preformatted sections (such as program text), it
prefixes each non-blank line with preIndent.

<h2 id="Example">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/example.go#L12">Example</a>
    <a href="#Example">¶</a></h2>
<pre>type Example struct {
<span id="Example.Name"></span>    Name        <a href="/builtin/#string">string</a> <span class="comment">// name of the item being exemplified</span>
<span id="Example.Doc"></span>    Doc         <a href="/builtin/#string">string</a> <span class="comment">// example function doc string</span>
<span id="Example.Code"></span>    Code        <a href="/go/ast/">ast</a>.<a href="/go/ast/#Node">Node</a>
<span id="Example.Play"></span>    Play        *<a href="/go/ast/">ast</a>.<a href="/go/ast/#File">File</a> <span class="comment">// a whole program version of the example</span>
<span id="Example.Comments"></span>    Comments    []*<a href="/go/ast/">ast</a>.<a href="/go/ast/#CommentGroup">CommentGroup</a>
<span id="Example.Output"></span>    Output      <a href="/builtin/#string">string</a> <span class="comment">// expected output</span>
<span id="Example.Unordered"></span>    Unordered   <a href="/builtin/#bool">bool</a>
<span id="Example.EmptyOutput"></span>    EmptyOutput <a href="/builtin/#bool">bool</a> <span class="comment">// expect empty output</span>
<span id="Example.Order"></span>    Order       <a href="/builtin/#int">int</a>  <span class="comment">// original source code order</span>
}</pre>

An Example represents an example function found in a source files.

<h2 id="Filter">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/filter.go#L1">Filter</a>
    <a href="#Filter">¶</a></h2>
<pre>type Filter func(<a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>


<h2 id="Func">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/doc.go#L46">Func</a>
    <a href="#Func">¶</a></h2>
<pre>type Func struct {
<span id="Func.Doc"></span>    Doc  <a href="/builtin/#string">string</a>
<span id="Func.Name"></span>    Name <a href="/builtin/#string">string</a>
<span id="Func.Decl"></span>    Decl *<a href="/go/ast/">ast</a>.<a href="/go/ast/#FuncDecl">FuncDecl</a>

    <span class="comment">// methods</span>
    <span class="comment">// (for functions, these fields have the respective zero value)</span>
<span id="Func.Recv"></span>    Recv  <a href="/builtin/#string">string</a> <span class="comment">// actual   receiver &#34;T&#34; or &#34;*T&#34;</span>
<span id="Func.Orig"></span>    Orig  <a href="/builtin/#string">string</a> <span class="comment">// original receiver &#34;T&#34; or &#34;*T&#34;</span>
<span id="Func.Level"></span>    Level <a href="/builtin/#int">int</a>    <span class="comment">// embedding level; 0 means not embedded</span>
}</pre>

Func is the documentation for a func declaration.

<h2 id="Mode">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/doc.go#L69">Mode</a>
    <a href="#Mode">¶</a></h2>
<pre>type Mode <a href="/builtin/#int">int</a></pre>

Mode values control the operation of New.

<pre>const (
    <span class="comment">// extract documentation for all package-level declarations,</span>
    <span class="comment">// not just exported ones</span>
    <span id="AllDecls">AllDecls</span> <a href="#Mode">Mode</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a>

    <span class="comment">// show all embedded methods, not just the ones of</span>
    <span class="comment">// invisible (unexported) anonymous fields</span>
    <span id="AllMethods">AllMethods</span>
)</pre>


<h2 id="Note">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/doc.go#L62">Note</a>
    <a href="#Note">¶</a></h2>
<pre>type Note struct {
<span id="Note.Pos"></span>    Pos, End <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position range of the comment containing the marker</span>
<span id="Note.UID"></span>    UID      <a href="/builtin/#string">string</a>    <span class="comment">// uid found with the marker</span>
<span id="Note.Body"></span>    Body     <a href="/builtin/#string">string</a>    <span class="comment">// note body text</span>
}</pre>

A Note represents a marked comment starting with "MARKER(uid): note body". Any
note with a marker of 2 or more upper case [A-Z] letters and a uid of at least
one character is recognized. The ":" following the uid is optional. Notes are
collected in the Package.Notes map indexed by the notes marker.

<h2 id="Package">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/doc.go#L4">Package</a>
    <a href="#Package">¶</a></h2>
<pre>type Package struct {
<span id="Package.Doc"></span>    Doc        <a href="/builtin/#string">string</a>
<span id="Package.Name"></span>    Name       <a href="/builtin/#string">string</a>
<span id="Package.ImportPath"></span>    ImportPath <a href="/builtin/#string">string</a>
<span id="Package.Imports"></span>    Imports    []<a href="/builtin/#string">string</a>
<span id="Package.Filenames"></span>    Filenames  []<a href="/builtin/#string">string</a>
<span id="Package.Notes"></span>    Notes      map[<a href="/builtin/#string">string</a>][]*<a href="#Note">Note</a>

    <span class="comment">// Deprecated: For backward compatibility Bugs is still populated,</span>
    <span class="comment">// but all new code should use Notes instead.</span>
<span id="Package.Bugs"></span>    Bugs []<a href="/builtin/#string">string</a>

    <span class="comment">// declarations</span>
<span id="Package.Consts"></span>    Consts []*<a href="#Value">Value</a>
<span id="Package.Types"></span>    Types  []*<a href="#Type">Type</a>
<span id="Package.Vars"></span>    Vars   []*<a href="#Value">Value</a>
<span id="Package.Funcs"></span>    Funcs  []*<a href="#Func">Func</a>
}</pre>

Package is the documentation for an entire package.

<h3 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/doc.go#L84">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(pkg *<a href="/go/ast/">ast</a>.<a href="/go/ast/#Package">Package</a>, importPath <a href="/builtin/#string">string</a>, mode <a href="#Mode">Mode</a>) *<a href="#Package">Package</a></pre>

New computes the package documentation for the given package AST. New takes
ownership of the AST pkg and may edit or overwrite it.

<h3 id="Package.Filter">func (*Package) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/filter.go#L89">Filter</a>
    <a href="#Package.Filter">¶</a></h3>
<pre>func (p *<a href="#Package">Package</a>) Filter(f <a href="#Filter">Filter</a>)</pre>

Filter eliminates documentation for names that don't pass through the filter f.
TODO(gri): Recognize "Type.Method" as a name.

<h2 id="Type">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/doc.go#L33">Type</a>
    <a href="#Type">¶</a></h2>
<pre>type Type struct {
<span id="Type.Doc"></span>    Doc  <a href="/builtin/#string">string</a>
<span id="Type.Name"></span>    Name <a href="/builtin/#string">string</a>
<span id="Type.Decl"></span>    Decl *<a href="/go/ast/">ast</a>.<a href="/go/ast/#GenDecl">GenDecl</a>

    <span class="comment">// associated declarations</span>
<span id="Type.Consts"></span>    Consts  []*<a href="#Value">Value</a> <span class="comment">// sorted list of constants of (mostly) this type</span>
<span id="Type.Vars"></span>    Vars    []*<a href="#Value">Value</a> <span class="comment">// sorted list of variables of (mostly) this type</span>
<span id="Type.Funcs"></span>    Funcs   []*<a href="#Func">Func</a>  <span class="comment">// sorted list of functions returning this type</span>
<span id="Type.Methods"></span>    Methods []*<a href="#Func">Func</a>  <span class="comment">// sorted list of methods (including embedded ones) of this type</span>
}</pre>

Type is the documentation for a type declaration.

<h2 id="Value">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/doc/doc.go#L24">Value</a>
    <a href="#Value">¶</a></h2>
<pre>type Value struct {
<span id="Value.Doc"></span>    Doc   <a href="/builtin/#string">string</a>
<span id="Value.Names"></span>    Names []<a href="/builtin/#string">string</a> <span class="comment">// var or const names in declaration order</span>
<span id="Value.Decl"></span>    Decl  *<a href="/go/ast/">ast</a>.<a href="/go/ast/#GenDecl">GenDecl</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Value is the documentation for a (possibly grouped) var or const declaration.


