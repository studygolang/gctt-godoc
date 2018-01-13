version: 1.9.2
## package printer

  `import "go/printer"`

## Overview

Package printer implements printing of AST nodes.

## Index

- [func Fprint(output io.Writer, fset *token.FileSet, node interface{}) error](#Fprint)
- [type CommentedNode](#CommentedNode)
- [type Config](#Config)
  - [func (cfg *Config) Fprint(output io.Writer, fset *token.FileSet, node interface{}) error](#Config.Fprint)
- [type Mode](#Mode)

### Examples

- [Fprint](#exampleFprint)

### Package files
 [nodes.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/printer/nodes.go) [printer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/printer/printer.go)

<h2 id="Fprint">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/printer/printer.go#L1344">Fprint</a>
    <a href="#Fprint">¶</a></h2>
<pre>func Fprint(output <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, node interface{}) <a href="/builtin/#error">error</a></pre>

Fprint "pretty-prints" an AST node to output. It calls Config.Fprint with
default settings. Note that gofmt uses tabs for indentation but spaces for
alignment; use format.Node (package go/format) for output that matches gofmt.

<a id="exampleFprint"></a>
Example:

    // Parse source file and extract the AST without comments for
    // this function, with position information referring to the
    // file set fset.
    funcAST, fset := parseFunc("example_test.go", "ExampleFprint")

    // Print the function body into buffer buf.
    // The file set is provided to the printer so that it knows
    // about the original source formatting and can add additional
    // line breaks where they were present in the source.
    var buf bytes.Buffer
    printer.Fprint(&buf, fset, funcAST.Body)

    // Remove braces {} enclosing the function body, unindent,
    // and trim leading and trailing white space.
    s := buf.String()
    s = s[1 : len(s)-1]
    s = strings.TrimSpace(strings.Replace(s, "\n\t", "\n", -1))

    // Print the cleaned-up body text to stdout.
    fmt.Println(s)

    // output:
    // funcAST, fset := parseFunc("example_test.go", "ExampleFprint")
    //
    // var buf bytes.Buffer
    // printer.Fprint(&buf, fset, funcAST.Body)
    //
    // s := buf.String()
    // s = s[1 : len(s)-1]
    // s = strings.TrimSpace(strings.Replace(s, "\n\t", "\n", -1))
    //
    // fmt.Println(s)

<h2 id="CommentedNode">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/printer/printer.go#L1325">CommentedNode</a>
    <a href="#CommentedNode">¶</a></h2>
<pre>type CommentedNode struct {
<span id="CommentedNode.Node"></span>    Node     interface{} <span class="comment">// *ast.File, or ast.Expr, ast.Decl, ast.Spec, or ast.Stmt</span>
<span id="CommentedNode.Comments"></span>    Comments []*<a href="/go/ast/">ast</a>.<a href="/go/ast/#CommentGroup">CommentGroup</a>
}</pre>

A CommentedNode bundles an AST node and corresponding comments. It may be
provided as argument to any of the Fprint functions.

<h2 id="Config">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/printer/printer.go#L1267">Config</a>
    <a href="#Config">¶</a></h2>
<pre>type Config struct {
<span id="Config.Mode"></span>    Mode     <a href="#Mode">Mode</a> <span class="comment">// default: 0</span>
<span id="Config.Tabwidth"></span>    Tabwidth <a href="/builtin/#int">int</a>  <span class="comment">// default: 8</span>
<span id="Config.Indent"></span>    Indent   <a href="/builtin/#int">int</a>  <span class="comment">// default: 0 (all code is indented at least by this much)</span>
}</pre>

A Config node controls the output of Fprint.

<h3 id="Config.Fprint">func (*Config) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/printer/printer.go#L1335">Fprint</a>
    <a href="#Config.Fprint">¶</a></h3>
<pre>func (cfg *<a href="#Config">Config</a>) Fprint(output <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, node interface{}) <a href="/builtin/#error">error</a></pre>

Fprint "pretty-prints" an AST node to output for a given configuration cfg.
Position information is interpreted relative to the file set fset. The node type
must be *ast.File, *CommentedNode, []ast.Decl, []ast.Stmt, or
assignment-compatible to ast.Expr, ast.Decl, ast.Spec, or ast.Stmt.

<h2 id="Mode">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/printer/printer.go#L1257">Mode</a>
    <a href="#Mode">¶</a></h2>
<pre>type Mode <a href="/builtin/#uint">uint</a></pre>

A Mode value is a set of flags (or 0). They control printing.

<pre>const (
    <span id="RawFormat">RawFormat</span> <a href="#Mode">Mode</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a> <span class="comment">// do not use a tabwriter; if set, UseSpaces is ignored</span>
    <span id="TabIndent">TabIndent</span>                  <span class="comment">// use tabs for indentation independent of UseSpaces</span>
    <span id="UseSpaces">UseSpaces</span>                  <span class="comment">// use spaces instead of tabs for alignment</span>
    <span id="SourcePos">SourcePos</span>                  <span class="comment">// emit //line directives to preserve original source positions</span>
)</pre>



