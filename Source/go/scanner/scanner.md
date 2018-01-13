version: 1.9.2
## package scanner

  `import "go/scanner"`

## Overview

Package scanner implements a scanner for Go source text. It takes a []byte as
source which can then be tokenized through repeated calls to the Scan method.

## Index

- [func PrintError(w io.Writer, err error)](#PrintError)
- [type Error](#Error)
  - [func (e Error) Error() string](#Error.Error)
- [type ErrorHandler](#ErrorHandler)
- [type ErrorList](#ErrorList)
  - [func (p *ErrorList) Add(pos token.Position, msg string)](#ErrorList.Add)
  - [func (p ErrorList) Err() error](#ErrorList.Err)
  - [func (p ErrorList) Error() string](#ErrorList.Error)
  - [func (p ErrorList) Len() int](#ErrorList.Len)
  - [func (p ErrorList) Less(i, j int) bool](#ErrorList.Less)
  - [func (p *ErrorList) RemoveMultiples()](#ErrorList.RemoveMultiples)
  - [func (p *ErrorList) Reset()](#ErrorList.Reset)
  - [func (p ErrorList) Sort()](#ErrorList.Sort)
  - [func (p ErrorList) Swap(i, j int)](#ErrorList.Swap)
- [type Mode](#Mode)
- [type Scanner](#Scanner)
  - [func (s *Scanner) Init(file *token.File, src []byte, err ErrorHandler, mode Mode)](#Scanner.Init)
  - [func (s *Scanner) Scan() (pos token.Pos, tok token.Token, lit string)](#Scanner.Scan)

### Examples

- [Scanner.Scan](#exampleScanner_Scan)

### Package files
 [errors.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go) [scanner.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/scanner.go)

<h2 id="PrintError">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L106">PrintError</a>
    <a href="#PrintError">¶</a></h2>
<pre>func PrintError(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, err <a href="/builtin/#error">error</a>)</pre>

PrintError is a utility function that prints a list of errors to w, one error
per line, if the err parameter is an ErrorList. Otherwise it prints the err
string.

<h2 id="Error">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L9">Error</a>
    <a href="#Error">¶</a></h2>
<pre>type Error struct {
<span id="Error.Pos"></span>    Pos <a href="/go/token/">token</a>.<a href="/go/token/#Position">Position</a>
<span id="Error.Msg"></span>    Msg <a href="/builtin/#string">string</a>
}</pre>

In an ErrorList, an error is represented by an *Error. The position Pos, if
valid, points to the beginning of the offending token, and the error condition
is described by Msg.

<h3 id="Error.Error">func (Error) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L15">Error</a>
    <a href="#Error.Error">¶</a></h3>
<pre>func (e <a href="#Error">Error</a>) Error() <a href="/builtin/#string">string</a></pre>

Error implements the error interface.

<h2 id="ErrorHandler">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/scanner.go#L16">ErrorHandler</a>
    <a href="#ErrorHandler">¶</a></h2>
<pre>type ErrorHandler func(pos <a href="/go/token/">token</a>.<a href="/go/token/#Position">Position</a>, msg <a href="/builtin/#string">string</a>)</pre>

An ErrorHandler may be provided to Scanner.Init. If a syntax error is
encountered and a handler was installed, the handler is called with a position
and an error message. The position points to the beginning of the offending
token.

<h2 id="ErrorList">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L27">ErrorList</a>
    <a href="#ErrorList">¶</a></h2>
<pre>type ErrorList []*<a href="#Error">Error</a></pre>

ErrorList is a list of *Errors. The zero value for an ErrorList is an empty
ErrorList ready to use.

<h3 id="ErrorList.Add">func (*ErrorList) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L30">Add</a>
    <a href="#ErrorList.Add">¶</a></h3>
<pre>func (p *<a href="#ErrorList">ErrorList</a>) Add(pos <a href="/go/token/">token</a>.<a href="/go/token/#Position">Position</a>, msg <a href="/builtin/#string">string</a>)</pre>

Add adds an Error with given position and error message to an ErrorList.

<h3 id="ErrorList.Err">func (ErrorList) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L95">Err</a>
    <a href="#ErrorList.Err">¶</a></h3>
<pre>func (p <a href="#ErrorList">ErrorList</a>) Err() <a href="/builtin/#error">error</a></pre>

Err returns an error equivalent to this error list. If the list is empty, Err
returns nil.

<h3 id="ErrorList.Error">func (ErrorList) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L83">Error</a>
    <a href="#ErrorList.Error">¶</a></h3>
<pre>func (p <a href="#ErrorList">ErrorList</a>) Error() <a href="/builtin/#string">string</a></pre>

An ErrorList implements the error interface.

<h3 id="ErrorList.Len">func (ErrorList) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L38">Len</a>
    <a href="#ErrorList.Len">¶</a></h3>
<pre>func (p <a href="#ErrorList">ErrorList</a>) Len() <a href="/builtin/#int">int</a></pre>

ErrorList implements the sort Interface.

<h3 id="ErrorList.Less">func (ErrorList) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L41">Less</a>
    <a href="#ErrorList.Less">¶</a></h3>
<pre>func (p <a href="#ErrorList">ErrorList</a>) Less(i, j <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>


<h3 id="ErrorList.RemoveMultiples">func (*ErrorList) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L68">RemoveMultiples</a>
    <a href="#ErrorList.RemoveMultiples">¶</a></h3>
<pre>func (p *<a href="#ErrorList">ErrorList</a>) RemoveMultiples()</pre>

RemoveMultiples sorts an ErrorList and removes all but the first error per line.

<h3 id="ErrorList.Reset">func (*ErrorList) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L35">Reset</a>
    <a href="#ErrorList.Reset">¶</a></h3>
<pre>func (p *<a href="#ErrorList">ErrorList</a>) Reset()</pre>

Reset resets an ErrorList to no errors.

<h3 id="ErrorList.Sort">func (ErrorList) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L63">Sort</a>
    <a href="#ErrorList.Sort">¶</a></h3>
<pre>func (p <a href="#ErrorList">ErrorList</a>) Sort()</pre>

Sort sorts an ErrorList. *Error entries are sorted by position, other errors are
sorted by error message, and before any *Error entry.

<h3 id="ErrorList.Swap">func (ErrorList) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/errors.go#L39">Swap</a>
    <a href="#ErrorList.Swap">¶</a></h3>
<pre>func (p <a href="#ErrorList">ErrorList</a>) Swap(i, j <a href="/builtin/#int">int</a>)</pre>


<h2 id="Mode">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/scanner.go#L81">Mode</a>
    <a href="#Mode">¶</a></h2>
<pre>type Mode <a href="/builtin/#uint">uint</a></pre>

A mode value is a set of flags (or 0). They control scanner behavior.

<pre>const (
    <span id="ScanComments">ScanComments</span> <a href="#Mode">Mode</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a> <span class="comment">// return comments as COMMENT tokens</span>

)</pre>


<h2 id="Scanner">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/scanner.go#L22">Scanner</a>
    <a href="#Scanner">¶</a></h2>
<pre>type Scanner struct {

    <span class="comment">// public state - ok to modify</span>
<span id="Scanner.ErrorCount"></span>    ErrorCount <a href="/builtin/#int">int</a> <span class="comment">// number of errors encountered</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Scanner holds the scanner's internal state while processing a given text. It
can be allocated as part of another data structure but must be initialized via
Init before use.

<h3 id="Scanner.Init">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/scanner.go#L103">Init</a>
    <a href="#Scanner.Init">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Init(file *<a href="/go/token/">token</a>.<a href="/go/token/#File">File</a>, src []<a href="/builtin/#byte">byte</a>, err <a href="#ErrorHandler">ErrorHandler</a>, mode <a href="#Mode">Mode</a>)</pre>

Init prepares the scanner s to tokenize the text src by setting the scanner at
the beginning of src. The scanner uses the file set file for position
information and it adds line information for each line. It is ok to re-use the
same file when re-scanning the same file as line information which is already
present is ignored. Init causes a panic if the file size does not match the src
size.

Calls to Scan will invoke the error handler err if they encounter a syntax error
and err is not nil. Also, for each error encountered, the Scanner field
ErrorCount is incremented by one. The mode parameter determines how comments are
handled.

Note that Init may call err if there is an error in the first character of the
file.

<h3 id="Scanner.Scan">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/scanner/scanner.go#L592">Scan</a>
    <a href="#Scanner.Scan">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Scan() (pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, tok <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a>, lit <a href="/builtin/#string">string</a>)</pre>

Scan scans the next token and returns the token position, the token, and its
literal string if applicable. The source end is indicated by token.EOF.

If the returned token is a literal (token.IDENT, token.INT, token.FLOAT,
token.IMAG, token.CHAR, token.STRING) or token.COMMENT, the literal string has
the corresponding value.

If the returned token is a keyword, the literal string is the keyword.

If the returned token is token.SEMICOLON, the corresponding literal string is
";" if the semicolon was present in the source, and "\n" if the semicolon was
inserted because of a newline or at EOF.

If the returned token is token.ILLEGAL, the literal string is the offending
character.

In all other cases, Scan returns an empty literal string.

For more tolerant parsing, Scan will return a valid token if possible even if a
syntax error was encountered. Thus, even if the resulting token sequence
contains no illegal tokens, a client may not assume that no error occurred.
Instead it must check the scanner's ErrorCount or the number of calls of the
error handler, if there was one installed.

Scan adds line information to the file added to the file set with Init. Token
positions are relative to that file and thus relative to the file set.

<a id="exampleScanner_Scan"></a>
Example:

    // src is the input that we want to tokenize.
    src := []byte("cos(x) + 1i*sin(x) // Euler")

    // Initialize the scanner.
    var s scanner.Scanner
    fset := token.NewFileSet()                      // positions are relative to fset
    file := fset.AddFile("", fset.Base(), len(src)) // register input "file"
    s.Init(file, src, nil /* no error handler */, scanner.ScanComments)

    // Repeated calls to Scan yield the token sequence found in the input.
    for {
        pos, tok, lit := s.Scan()
        if tok == token.EOF {
            break
        }
        fmt.Printf("%s\t%s\t%q\n", fset.Position(pos), tok, lit)
    }

    // output:
    // 1:1	IDENT	"cos"
    // 1:4	(	""
    // 1:5	IDENT	"x"
    // 1:6	)	""
    // 1:8	+	""
    // 1:10	IMAG	"1i"
    // 1:12	*	""
    // 1:13	IDENT	"sin"
    // 1:16	(	""
    // 1:17	IDENT	"x"
    // 1:18	)	""
    // 1:20	;	"\n"
    // 1:20	COMMENT	"// Euler"


