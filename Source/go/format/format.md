version: 1.9.2
## package format

  `import "go/format"`

## Overview

Package format implements standard formatting of Go source.

## Index

- [func Node(dst io.Writer, fset *token.FileSet, node interface{}) error](#Node)
- [func Source(src []byte) ([]byte, error)](#Source)

### Examples

- [Node](#exampleNode)

### Package files
 [format.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/format/format.go) [internal.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/format/internal.go)

<h2 id="Node">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/format/format.go#L23">Node</a>
    <a href="#Node">¶</a></h2>
<pre>func Node(dst <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, node interface{}) <a href="/builtin/#error">error</a></pre>

Node formats node in canonical gofmt style and writes the result to dst.

The node type must be *ast.File, *printer.CommentedNode, []ast.Decl, []ast.Stmt,
or assignment-compatible to ast.Expr, ast.Decl, ast.Spec, or ast.Stmt. Node does
not modify node. Imports are not sorted for nodes representing partial source
files (i.e., if the node is not an *ast.File or a *printer.CommentedNode not
wrapping an *ast.File).

The function may return early (before the entire result is written) and return a
formatting error, for instance due to an incorrect AST.

<a id="exampleNode"></a>
Example:

    const expr = "(6+2*3)/4"

    // parser.ParseExpr parses the argument and returns the
    // corresponding ast.Node.
    node, err := parser.ParseExpr(expr)
    if err != nil {
        log.Fatal(err)
    }

    // Create a FileSet for node. Since the node does not come
    // from a real source file, fset will be empty.
    fset := token.NewFileSet()

    var buf bytes.Buffer
    err = Node(&buf, fset, node)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println(buf.String())

    // Output: (6 + 2*3) / 4

<h2 id="Source">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/format/format.go#L72">Source</a>
    <a href="#Source">¶</a></h2>
<pre>func Source(src []<a href="/builtin/#byte">byte</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Source formats src in canonical gofmt style and returns the result or an (I/O or
syntax) error. src is expected to be a syntactically correct Go source file, or
a list of Go declarations or statements.

If src is a partial source file, the leading and trailing space of src is
applied to the result (such that it has the same leading and trailing space as
src), and the result is indented by the same amount as the first line of src
containing code. Imports are not sorted for partial source files.


