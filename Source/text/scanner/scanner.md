version: 1.9.2
## package scanner

  `import "text/scanner"`

## Overview

Package scanner provides a scanner and tokenizer for UTF-8-encoded text. It
takes an io.Reader providing the source, which then can be tokenized through
repeated calls to the Scan function. For compatibility with existing tools, the
NUL character is not allowed. If the first character in the source is a UTF-8
encoded byte order mark (BOM), it is discarded.

By default, a Scanner skips white space and Go comments and recognizes all
literals as defined by the Go language specification. It may be customized to
recognize only a subset of those literals and to recognize different identifier
and white space characters.

<a id="example_"></a>
Example:

    const src = `
    // This is scanned code.
    if a > 10 {
    	someParsable = text
    }`
    var s scanner.Scanner
    s.Init(strings.NewReader(src))
    s.Filename = "example"
    for tok := s.Scan(); tok != scanner.EOF; tok = s.Scan() {
        fmt.Printf("%s: %s\n", s.Position, s.TokenText())
    }

    // Output:
    // example:3:1: if
    // example:3:4: a
    // example:3:6: >
    // example:3:8: 10
    // example:3:11: {
    // example:4:2: someParsable
    // example:4:15: =
    // example:4:17: text
    // example:5:1: }

## Index

- [Constants](#pkg-constants)
- [func TokenString(tok rune) string](#TokenString)
- [type Position](#Position)
  - [func (pos *Position) IsValid() bool](#Position.IsValid)
  - [func (pos Position) String() string](#Position.String)
- [type Scanner](#Scanner)
  - [func (s *Scanner) Init(src io.Reader) *Scanner](#Scanner.Init)
  - [func (s *Scanner) Next() rune](#Scanner.Next)
  - [func (s *Scanner) Peek() rune](#Scanner.Peek)
  - [func (s *Scanner) Pos() (pos Position)](#Scanner.Pos)
  - [func (s *Scanner) Scan() rune](#Scanner.Scan)
  - [func (s *Scanner) TokenText() string](#Scanner.TokenText)

### Examples

- [Package](#example_)

### Package files
 [scanner.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="ScanIdents">ScanIdents</span>     = 1 &lt;&lt; -<a href="#Ident">Ident</a>
    <span id="ScanInts">ScanInts</span>       = 1 &lt;&lt; -<a href="#Int">Int</a>
    <span id="ScanFloats">ScanFloats</span>     = 1 &lt;&lt; -<a href="#Float">Float</a> <span class="comment">// includes Ints</span>
    <span id="ScanChars">ScanChars</span>      = 1 &lt;&lt; -<a href="#Char">Char</a>
    <span id="ScanStrings">ScanStrings</span>    = 1 &lt;&lt; -<a href="#String">String</a>
    <span id="ScanRawStrings">ScanRawStrings</span> = 1 &lt;&lt; -<a href="#RawString">RawString</a>
    <span id="ScanComments">ScanComments</span>   = 1 &lt;&lt; -<a href="#Comment">Comment</a>
    <span id="SkipComments">SkipComments</span>   = 1 &lt;&lt; -skipComment <span class="comment">// if set with ScanComments, comments become white space</span>
    <span id="GoTokens">GoTokens</span>       = <a href="#ScanIdents">ScanIdents</a> | <a href="#ScanFloats">ScanFloats</a> | <a href="#ScanChars">ScanChars</a> | <a href="#ScanStrings">ScanStrings</a> | <a href="#ScanRawStrings">ScanRawStrings</a> | <a href="#ScanComments">ScanComments</a> | <a href="#SkipComments">SkipComments</a>
)</pre>

Predefined mode bits to control recognition of tokens. For instance, to
configure a Scanner such that it only recognizes (Go) identifiers, integers, and
skips comments, set the Scanner's Mode field to:

    ScanIdents | ScanInts | SkipComments

With the exceptions of comments, which are skipped if SkipComments is set,
unrecognized tokens are not ignored. Instead, the scanner simply returns the
respective individual characters (or possibly sub-tokens). For instance, if the
mode is ScanIdents (not ScanStrings), the string "foo" is scanned as the token
sequence '"' Ident '"'.

<pre>const (
    <span id="EOF">EOF</span> = -(<a href="/builtin/#iota">iota</a> + 1)
    <span id="Ident">Ident</span>
    <span id="Int">Int</span>
    <span id="Float">Float</span>
    <span id="Char">Char</span>
    <span id="String">String</span>
    <span id="RawString">RawString</span>
    <span id="Comment">Comment</span>
)</pre>

The result of Scan is one of these tokens or a Unicode character.

<pre>const <span id="GoWhitespace">GoWhitespace</span> = 1&lt;&lt;&#39;\t&#39; | 1&lt;&lt;&#39;\n&#39; | 1&lt;&lt;&#39;\r&#39; | 1&lt;&lt;&#39; &#39;</pre>

GoWhitespace is the default value for the Scanner's Whitespace field. Its value
selects Go's white space characters.

<h2 id="TokenString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L88">TokenString</a>
    <a href="#TokenString">¶</a></h2>
<pre>func TokenString(tok <a href="/builtin/#rune">rune</a>) <a href="/builtin/#string">string</a></pre>

TokenString returns a printable string for a token or Unicode character.

<h2 id="Position">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L18">Position</a>
    <a href="#Position">¶</a></h2>
<pre>type Position struct {
<span id="Position.Filename"></span>    Filename <a href="/builtin/#string">string</a> <span class="comment">// filename, if any</span>
<span id="Position.Offset"></span>    Offset   <a href="/builtin/#int">int</a>    <span class="comment">// byte offset, starting at 0</span>
<span id="Position.Line"></span>    Line     <a href="/builtin/#int">int</a>    <span class="comment">// line number, starting at 1</span>
<span id="Position.Column"></span>    Column   <a href="/builtin/#int">int</a>    <span class="comment">// column number, starting at 1 (character count per line)</span>
}</pre>

A source position is represented by a Position value. A position is valid if
Line > 0.

<h3 id="Position.IsValid">func (*Position) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L26">IsValid</a>
    <a href="#Position.IsValid">¶</a></h3>
<pre>func (pos *<a href="#Position">Position</a>) IsValid() <a href="/builtin/#bool">bool</a></pre>

IsValid reports whether the position is valid.

<h3 id="Position.String">func (Position) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L28">String</a>
    <a href="#Position.String">¶</a></h3>
<pre>func (pos <a href="#Position">Position</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Scanner">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L102">Scanner</a>
    <a href="#Scanner">¶</a></h2>
<pre>type Scanner struct {

<span id="Scanner.Error"></span>    <span class="comment">// Error is called for each error encountered. If no Error</span>
    <span class="comment">// function is set, the error is reported to os.Stderr.</span>
    Error func(s *<a href="#Scanner">Scanner</a>, msg <a href="/builtin/#string">string</a>)

<span id="Scanner.ErrorCount"></span>    <span class="comment">// ErrorCount is incremented by one for each error encountered.</span>
    ErrorCount <a href="/builtin/#int">int</a>

    <span class="comment">// The Mode field controls which tokens are recognized. For instance,</span>
    <span class="comment">// to recognize Ints, set the ScanInts bit in Mode. The field may be</span>
    <span class="comment">// changed at any time.</span>
<span id="Scanner.Mode"></span>    Mode <a href="/builtin/#uint">uint</a>

    <span class="comment">// The Whitespace field controls which characters are recognized</span>
    <span class="comment">// as white space. To recognize a character ch &lt;= &#39; &#39; as white space,</span>
    <span class="comment">// set the ch&#39;th bit in Whitespace (the Scanner&#39;s behavior is undefined</span>
    <span class="comment">// for values ch &gt; &#39; &#39;). The field may be changed at any time.</span>
<span id="Scanner.Whitespace"></span>    Whitespace <a href="/builtin/#uint64">uint64</a>

<span id="Scanner.IsIdentRune"></span>    <span class="comment">// IsIdentRune is a predicate controlling the characters accepted</span>
    <span class="comment">// as the ith rune in an identifier. The set of valid characters</span>
    <span class="comment">// must not intersect with the set of white space characters.</span>
    <span class="comment">// If no IsIdentRune function is set, regular Go identifiers are</span>
    <span class="comment">// accepted instead. The field may be changed at any time.</span>
    IsIdentRune func(ch <a href="/builtin/#rune">rune</a>, i <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a>

    <span class="comment">// Start position of most recently scanned token; set by Scan.</span>
    <span class="comment">// Calling Init or Next invalidates the position (Line == 0).</span>
    <span class="comment">// The Filename field is always left untouched by the Scanner.</span>
    <span class="comment">// If an error is reported (via Error) and Position is invalid,</span>
    <span class="comment">// the scanner is not inside a token. Call Pos to obtain an error</span>
    <span class="comment">// position in that case, or to obtain the position immediately</span>
    <span class="comment">// after the most recently scanned token.</span>
    <a href="#Position">Position</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Scanner implements reading of Unicode characters and tokens from an io.Reader.

<h3 id="Scanner.Init">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L167">Init</a>
    <a href="#Scanner.Init">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Init(src <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Scanner">Scanner</a></pre>

Init initializes a Scanner with a new source and returns s. Error is set to nil,
ErrorCount is set to 0, Mode is set to GoTokens, and Whitespace is set to
GoWhitespace.

<h3 id="Scanner.Next">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L290">Next</a>
    <a href="#Scanner.Next">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Next() <a href="/builtin/#rune">rune</a></pre>

Next reads and returns the next Unicode character. It returns EOF at the end of
the source. It reports a read error by calling s.Error, if not nil; otherwise it
prints an error message to os.Stderr. Next does not update the Scanner's
Position field; use Pos() to get the current position.

<h3 id="Scanner.Peek">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L303">Peek</a>
    <a href="#Scanner.Peek">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Peek() <a href="/builtin/#rune">rune</a></pre>

Peek returns the next Unicode character in the source without advancing the
scanner. It returns EOF if the scanner's position is at the last character of
the source.

<h3 id="Scanner.Pos">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L633">Pos</a>
    <a href="#Scanner.Pos">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Pos() (pos <a href="#Position">Position</a>)</pre>

Pos returns the position of the character immediately after the character or
token returned by the last call to Next or Scan. Use the Scanner's Position
field for the start position of the most recently scanned token.

<h3 id="Scanner.Scan">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L529">Scan</a>
    <a href="#Scanner.Scan">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) Scan() <a href="/builtin/#rune">rune</a></pre>

Scan reads the next token or Unicode character from source and returns it. It
only recognizes tokens t for which the respective Mode bit (1<<-t) is set. It
returns EOF at the end of the source. It reports scanner errors (read and token
errors) by calling s.Error, if not nil; otherwise it prints an error message to
os.Stderr.

<h3 id="Scanner.TokenText">func (*Scanner) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/text/scanner/scanner.go#L655">TokenText</a>
    <a href="#Scanner.TokenText">¶</a></h3>
<pre>func (s *<a href="#Scanner">Scanner</a>) TokenText() <a href="/builtin/#string">string</a></pre>

TokenText returns the string corresponding to the most recently scanned token.
Valid after calling Scan().


