version: 1.9.2
## package syntax

  `import "regexp/syntax"`

## Overview

Package syntax parses regular expressions into parse trees and compiles parse
trees into programs. Most clients of regular expressions will use the facilities
of package regexp (such as Compile and Match) instead of this package.


Syntax

The regular expression syntax understood by this package when parsing with the
Perl flag is as follows. Parts of the syntax can be disabled by passing
alternate flags to Parse.

Single characters:

    .              any character, possibly including newline (flag s=true)
    [xyz]          character class
    [^xyz]         negated character class
    \d             Perl character class
    \D             negated Perl character class
    [[:alpha:]]    ASCII character class
    [[:^alpha:]]   negated ASCII character class
    \pN            Unicode character class (one-letter name)
    \p{Greek}      Unicode character class
    \PN            negated Unicode character class (one-letter name)
    \P{Greek}      negated Unicode character class

Composites:

    xy             x followed by y
    x|y            x or y (prefer x)

Repetitions:

    x*             zero or more x, prefer more
    x+             one or more x, prefer more
    x?             zero or one x, prefer one
    x{n,m}         n or n+1 or ... or m x, prefer more
    x{n,}          n or more x, prefer more
    x{n}           exactly n x
    x*?            zero or more x, prefer fewer
    x+?            one or more x, prefer fewer
    x??            zero or one x, prefer zero
    x{n,m}?        n or n+1 or ... or m x, prefer fewer
    x{n,}?         n or more x, prefer fewer
    x{n}?          exactly n x

Implementation restriction: The counting forms x{n,m}, x{n,}, and x{n} reject
forms that create a minimum or maximum repetition count above 1000. Unlimited
repetitions are not subject to this restriction.

Grouping:

    (re)           numbered capturing group (submatch)
    (?P<name>re)   named & numbered capturing group (submatch)
    (?:re)         non-capturing group
    (?flags)       set flags within current group; non-capturing
    (?flags:re)    set flags during re; non-capturing

    Flag syntax is xyz (set) or -xyz (clear) or xy-z (set xy, clear z). The flags are:

    i              case-insensitive (default false)
    m              multi-line mode: ^ and $ match begin/end line in addition to begin/end text (default false)
    s              let . match \n (default false)
    U              ungreedy: swap meaning of x* and x*?, x+ and x+?, etc (default false)

Empty strings:

    ^              at beginning of text or line (flag m=true)
    $              at end of text (like \z not Perl's \Z) or line (flag m=true)
    \A             at beginning of text
    \b             at ASCII word boundary (\w on one side and \W, \A, or \z on the other)
    \B             not at ASCII word boundary
    \z             at end of text

Escape sequences:

    \a             bell (== \007)
    \f             form feed (== \014)
    \t             horizontal tab (== \011)
    \n             newline (== \012)
    \r             carriage return (== \015)
    \v             vertical tab character (== \013)
    \*             literal *, for any punctuation character *
    \123           octal character code (up to three digits)
    \x7F           hex character code (exactly two digits)
    \x{10FFFF}     hex character code
    \Q...\E        literal text ... even if ... has punctuation

Character class elements:

    x              single character
    A-Z            character range (inclusive)
    \d             Perl character class
    [:foo:]        ASCII character class foo
    \p{Foo}        Unicode character class Foo
    \pF            Unicode character class F (one-letter name)

Named character classes as character class elements:

    [\d]           digits (== \d)
    [^\d]          not digits (== \D)
    [\D]           not digits (== \D)
    [^\D]          not not digits (== \d)
    [[:name:]]     named ASCII class inside character class (== [:name:])
    [^[:name:]]    named ASCII class inside negated character class (== [:^name:])
    [\p{Name}]     named Unicode property inside character class (== \p{Name})
    [^\p{Name}]    named Unicode property inside negated character class (== \P{Name})

Perl character classes (all ASCII-only):

    \d             digits (== [0-9])
    \D             not digits (== [^0-9])
    \s             whitespace (== [\t\n\f\r ])
    \S             not whitespace (== [^\t\n\f\r ])
    \w             word characters (== [0-9A-Za-z_])
    \W             not word characters (== [^0-9A-Za-z_])

ASCII character classes:

    [[:alnum:]]    alphanumeric (== [0-9A-Za-z])
    [[:alpha:]]    alphabetic (== [A-Za-z])
    [[:ascii:]]    ASCII (== [\x00-\x7F])
    [[:blank:]]    blank (== [\t ])
    [[:cntrl:]]    control (== [\x00-\x1F\x7F])
    [[:digit:]]    digits (== [0-9])
    [[:graph:]]    graphical (== [!-~] == [A-Za-z0-9!"#$%&'()*+,\-./:;<=>?@[\\\]^_`{|}~])
    [[:lower:]]    lower case (== [a-z])
    [[:print:]]    printable (== [ -~] == [ [:graph:]])
    [[:punct:]]    punctuation (== [!-/:-@[-`{-~])
    [[:space:]]    whitespace (== [\t\n\v\f\r ])
    [[:upper:]]    upper case (== [A-Z])
    [[:word:]]     word characters (== [0-9A-Za-z_])
    [[:xdigit:]]   hex digit (== [0-9A-Fa-f])

## Index

- [func IsWordChar(r rune) bool](#IsWordChar)
- [type EmptyOp](#EmptyOp)
  - [func EmptyOpContext(r1, r2 rune) EmptyOp](#EmptyOpContext)
- [type Error](#Error)
  - [func (e *Error) Error() string](#Error.Error)
- [type ErrorCode](#ErrorCode)
  - [func (e ErrorCode) String() string](#ErrorCode.String)
- [type Flags](#Flags)
- [type Inst](#Inst)
  - [func (i *Inst) MatchEmptyWidth(before rune, after rune) bool](#Inst.MatchEmptyWidth)
  - [func (i *Inst) MatchRune(r rune) bool](#Inst.MatchRune)
  - [func (i *Inst) MatchRunePos(r rune) int](#Inst.MatchRunePos)
  - [func (i *Inst) String() string](#Inst.String)
- [type InstOp](#InstOp)
  - [func (i InstOp) String() string](#InstOp.String)
- [type Op](#Op)
- [type Prog](#Prog)
  - [func Compile(re *Regexp) (*Prog, error)](#Compile)
  - [func (p *Prog) Prefix() (prefix string, complete bool)](#Prog.Prefix)
  - [func (p *Prog) StartCond() EmptyOp](#Prog.StartCond)
  - [func (p *Prog) String() string](#Prog.String)
- [type Regexp](#Regexp)
  - [func Parse(s string, flags Flags) (*Regexp, error)](#Parse)
  - [func (re *Regexp) CapNames() []string](#Regexp.CapNames)
  - [func (x *Regexp) Equal(y *Regexp) bool](#Regexp.Equal)
  - [func (re *Regexp) MaxCap() int](#Regexp.MaxCap)
  - [func (re *Regexp) Simplify() *Regexp](#Regexp.Simplify)
  - [func (re *Regexp) String() string](#Regexp.String)

### Package files
 [compile.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/compile.go) [doc.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/doc.go) [parse.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/parse.go) [perl_groups.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/perl_groups.go) [prog.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go) [regexp.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/regexp.go) [simplify.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/simplify.go)

<h2 id="IsWordChar">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L97">IsWordChar</a>
    <a href="#IsWordChar">¶</a></h2>
<pre>func IsWordChar(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsWordChar reports whether r is consider a ``word character'' during the
evaluation of the \b and \B zero-width assertions. These assertions are
ASCII-only: the word characters are [A-Za-z0-9_].

<h2 id="EmptyOp">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L52">EmptyOp</a>
    <a href="#EmptyOp">¶</a></h2>
<pre>type EmptyOp <a href="/builtin/#uint8">uint8</a></pre>

An EmptyOp specifies a kind or mixture of zero-width assertions.

<pre>const (
    <span id="EmptyBeginLine">EmptyBeginLine</span> <a href="#EmptyOp">EmptyOp</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a>
    <span id="EmptyEndLine">EmptyEndLine</span>
    <span id="EmptyBeginText">EmptyBeginText</span>
    <span id="EmptyEndText">EmptyEndText</span>
    <span id="EmptyWordBoundary">EmptyWordBoundary</span>
    <span id="EmptyNoWordBoundary">EmptyNoWordBoundary</span>
)</pre>


<h3 id="EmptyOpContext">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L69">EmptyOpContext</a>
    <a href="#EmptyOpContext">¶</a></h3>
<pre>func EmptyOpContext(r1, r2 <a href="/builtin/#rune">rune</a>) <a href="#EmptyOp">EmptyOp</a></pre>

EmptyOpContext returns the zero-width assertions satisfied at the position
between the runes r1 and r2. Passing r1 == -1 indicates that the position is at
the beginning of the text. Passing r2 == -1 indicates that the position is at
the end of the text.

<h2 id="Error">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/parse.go#L6">Error</a>
    <a href="#Error">¶</a></h2>
<pre>type Error struct {
<span id="Error.Code"></span>    Code <a href="#ErrorCode">ErrorCode</a>
<span id="Error.Expr"></span>    Expr <a href="/builtin/#string">string</a>
}</pre>

An Error describes a failure to parse a regular expression and gives the
offending expression.

<h3 id="Error.Error">func (*Error) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/parse.go#L11">Error</a>
    <a href="#Error.Error">¶</a></h3>
<pre>func (e *<a href="#Error">Error</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="ErrorCode">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/parse.go#L16">ErrorCode</a>
    <a href="#ErrorCode">¶</a></h2>
<pre>type ErrorCode <a href="/builtin/#string">string</a></pre>

An ErrorCode describes a failure to parse a regular expression.

<pre>const (
    <span class="comment">// Unexpected error</span>
    <span id="ErrInternalError">ErrInternalError</span> <a href="#ErrorCode">ErrorCode</a> = &#34;regexp/syntax: internal error&#34;

    <span class="comment">// Parse errors</span>
    <span id="ErrInvalidCharClass">ErrInvalidCharClass</span>      <a href="#ErrorCode">ErrorCode</a> = &#34;invalid character class&#34;
    <span id="ErrInvalidCharRange">ErrInvalidCharRange</span>      <a href="#ErrorCode">ErrorCode</a> = &#34;invalid character class range&#34;
    <span id="ErrInvalidEscape">ErrInvalidEscape</span>         <a href="#ErrorCode">ErrorCode</a> = &#34;invalid escape sequence&#34;
    <span id="ErrInvalidNamedCapture">ErrInvalidNamedCapture</span>   <a href="#ErrorCode">ErrorCode</a> = &#34;invalid named capture&#34;
    <span id="ErrInvalidPerlOp">ErrInvalidPerlOp</span>         <a href="#ErrorCode">ErrorCode</a> = &#34;invalid or unsupported Perl syntax&#34;
    <span id="ErrInvalidRepeatOp">ErrInvalidRepeatOp</span>       <a href="#ErrorCode">ErrorCode</a> = &#34;invalid nested repetition operator&#34;
    <span id="ErrInvalidRepeatSize">ErrInvalidRepeatSize</span>     <a href="#ErrorCode">ErrorCode</a> = &#34;invalid repeat count&#34;
    <span id="ErrInvalidUTF8">ErrInvalidUTF8</span>           <a href="#ErrorCode">ErrorCode</a> = &#34;invalid UTF-8&#34;
    <span id="ErrMissingBracket">ErrMissingBracket</span>        <a href="#ErrorCode">ErrorCode</a> = &#34;missing closing ]&#34;
    <span id="ErrMissingParen">ErrMissingParen</span>          <a href="#ErrorCode">ErrorCode</a> = &#34;missing closing )&#34;
    <span id="ErrMissingRepeatArgument">ErrMissingRepeatArgument</span> <a href="#ErrorCode">ErrorCode</a> = &#34;missing argument to repetition operator&#34;
    <span id="ErrTrailingBackslash">ErrTrailingBackslash</span>     <a href="#ErrorCode">ErrorCode</a> = &#34;trailing backslash at end of expression&#34;
    <span id="ErrUnexpectedParen">ErrUnexpectedParen</span>       <a href="#ErrorCode">ErrorCode</a> = &#34;unexpected )&#34;
)</pre>


<h3 id="ErrorCode.String">func (ErrorCode) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/parse.go#L38">String</a>
    <a href="#ErrorCode.String">¶</a></h3>
<pre>func (e <a href="#ErrorCode">ErrorCode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Flags">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/parse.go#L43">Flags</a>
    <a href="#Flags">¶</a></h2>
<pre>type Flags <a href="/builtin/#uint16">uint16</a></pre>

Flags control the behavior of the parser and record information about regexp
context.

<pre>const (
    <span id="FoldCase">FoldCase</span>      <a href="#Flags">Flags</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a> <span class="comment">// case-insensitive match</span>
    <span id="Literal">Literal</span>                         <span class="comment">// treat pattern as literal string</span>
    <span id="ClassNL">ClassNL</span>                         <span class="comment">// allow character classes like [^a-z] and [[:space:]] to match newline</span>
    <span id="DotNL">DotNL</span>                           <span class="comment">// allow . to match newline</span>
    <span id="OneLine">OneLine</span>                         <span class="comment">// treat ^ and $ as only matching at beginning and end of text</span>
    <span id="NonGreedy">NonGreedy</span>                       <span class="comment">// make repetition operators default to non-greedy</span>
    <span id="PerlX">PerlX</span>                           <span class="comment">// allow Perl extensions</span>
    <span id="UnicodeGroups">UnicodeGroups</span>                   <span class="comment">// allow \p{Han}, \P{Han} for Unicode group and negation</span>
    <span id="WasDollar">WasDollar</span>                       <span class="comment">// regexp OpEndText was $, not \z</span>
    <span id="Simple">Simple</span>                          <span class="comment">// regexp contains no counted repetition</span>

    <span id="MatchNL">MatchNL</span> = <a href="#ClassNL">ClassNL</a> | <a href="#DotNL">DotNL</a>

    <span id="Perl">Perl</span>        = <a href="#ClassNL">ClassNL</a> | <a href="#OneLine">OneLine</a> | <a href="#PerlX">PerlX</a> | <a href="#UnicodeGroups">UnicodeGroups</a> <span class="comment">// as close to Perl as possible</span>
    <span id="POSIX">POSIX</span> <a href="#Flags">Flags</a> = 0                                         <span class="comment">// POSIX syntax</span>
)</pre>


<h2 id="Inst">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L102">Inst</a>
    <a href="#Inst">¶</a></h2>
<pre>type Inst struct {
<span id="Inst.Op"></span>    Op   <a href="#InstOp">InstOp</a>
<span id="Inst.Out"></span>    Out  <a href="/builtin/#uint32">uint32</a> <span class="comment">// all but InstMatch, InstFail</span>
<span id="Inst.Arg"></span>    Arg  <a href="/builtin/#uint32">uint32</a> <span class="comment">// InstAlt, InstAltMatch, InstCapture, InstEmptyWidth</span>
<span id="Inst.Rune"></span>    Rune []<a href="/builtin/#rune">rune</a>
}</pre>

An Inst is a single instruction in a regular expression program.

<h3 id="Inst.MatchEmptyWidth">func (*Inst) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L252">MatchEmptyWidth</a>
    <a href="#Inst.MatchEmptyWidth">¶</a></h3>
<pre>func (i *<a href="#Inst">Inst</a>) MatchEmptyWidth(before <a href="/builtin/#rune">rune</a>, after <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

MatchEmptyWidth reports whether the instruction matches an empty string between
the runes before and after. It should only be called when i.Op ==
InstEmptyWidth.

<h3 id="Inst.MatchRune">func (*Inst) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L184">MatchRune</a>
    <a href="#Inst.MatchRune">¶</a></h3>
<pre>func (i *<a href="#Inst">Inst</a>) MatchRune(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

MatchRune reports whether the instruction matches (and consumes) r. It should
only be called when i.Op == InstRune.

<h3 id="Inst.MatchRunePos">func (*Inst) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L193">MatchRunePos</a>
    <a href="#Inst.MatchRunePos">¶</a></h3>
<pre>func (i *<a href="#Inst">Inst</a>) MatchRunePos(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#int">int</a></pre>

MatchRunePos checks whether the instruction matches (and consumes) r. If so,
MatchRunePos returns the index of the matching rune pair (or, when len(i.Rune)
== 1, rune singleton). If not, MatchRunePos returns -1. MatchRunePos should only
be called when i.Op == InstRune.

<h3 id="Inst.String">func (*Inst) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L270">String</a>
    <a href="#Inst.String">¶</a></h3>
<pre>func (i *<a href="#Inst">Inst</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="InstOp">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L14">InstOp</a>
    <a href="#InstOp">¶</a></h2>
<pre>type InstOp <a href="/builtin/#uint8">uint8</a></pre>

An InstOp is an instruction opcode.

<pre>const (
    <span id="InstAlt">InstAlt</span> <a href="#InstOp">InstOp</a> = <a href="/builtin/#iota">iota</a>
    <span id="InstAltMatch">InstAltMatch</span>
    <span id="InstCapture">InstCapture</span>
    <span id="InstEmptyWidth">InstEmptyWidth</span>
    <span id="InstMatch">InstMatch</span>
    <span id="InstFail">InstFail</span>
    <span id="InstNop">InstNop</span>
    <span id="InstRune">InstRune</span>
    <span id="InstRune1">InstRune1</span>
    <span id="InstRuneAny">InstRuneAny</span>
    <span id="InstRuneAnyNotNL">InstRuneAnyNotNL</span>
)</pre>


<h3 id="InstOp.String">func (InstOp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L44">String</a>
    <a href="#InstOp.String">¶</a></h3>
<pre>func (i <a href="#InstOp">InstOp</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Op">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/regexp.go#L21">Op</a>
    <a href="#Op">¶</a></h2>
<pre>type Op <a href="/builtin/#uint8">uint8</a></pre>

An Op is a single regular expression operator.

<pre>const (
    <span id="OpNoMatch">OpNoMatch</span>        <a href="#Op">Op</a> = 1 + <a href="/builtin/#iota">iota</a> <span class="comment">// matches no strings</span>
    <span id="OpEmptyMatch">OpEmptyMatch</span>                   <span class="comment">// matches empty string</span>
    <span id="OpLiteral">OpLiteral</span>                      <span class="comment">// matches Runes sequence</span>
    <span id="OpCharClass">OpCharClass</span>                    <span class="comment">// matches Runes interpreted as range pair list</span>
    <span id="OpAnyCharNotNL">OpAnyCharNotNL</span>                 <span class="comment">// matches any character except newline</span>
    <span id="OpAnyChar">OpAnyChar</span>                      <span class="comment">// matches any character</span>
    <span id="OpBeginLine">OpBeginLine</span>                    <span class="comment">// matches empty string at beginning of line</span>
    <span id="OpEndLine">OpEndLine</span>                      <span class="comment">// matches empty string at end of line</span>
    <span id="OpBeginText">OpBeginText</span>                    <span class="comment">// matches empty string at beginning of text</span>
    <span id="OpEndText">OpEndText</span>                      <span class="comment">// matches empty string at end of text</span>
    <span id="OpWordBoundary">OpWordBoundary</span>                 <span class="comment">// matches word boundary `\b`</span>
    <span id="OpNoWordBoundary">OpNoWordBoundary</span>               <span class="comment">// matches word non-boundary `\B`</span>
    <span id="OpCapture">OpCapture</span>                      <span class="comment">// capturing subexpression with index Cap, optional name Name</span>
    <span id="OpStar">OpStar</span>                         <span class="comment">// matches Sub[0] zero or more times</span>
    <span id="OpPlus">OpPlus</span>                         <span class="comment">// matches Sub[0] one or more times</span>
    <span id="OpQuest">OpQuest</span>                        <span class="comment">// matches Sub[0] zero or one times</span>
    <span id="OpRepeat">OpRepeat</span>                       <span class="comment">// matches Sub[0] at least Min times, at most Max (Max == -1 is no limit)</span>
    <span id="OpConcat">OpConcat</span>                       <span class="comment">// matches concatenation of Subs</span>
    <span id="OpAlternate">OpAlternate</span>                    <span class="comment">// matches alternation of Subs</span>
)</pre>


<h2 id="Prog">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L7">Prog</a>
    <a href="#Prog">¶</a></h2>
<pre>type Prog struct {
<span id="Prog.Inst"></span>    Inst   []<a href="#Inst">Inst</a>
<span id="Prog.Start"></span>    Start  <a href="/builtin/#int">int</a> <span class="comment">// index of start instruction</span>
<span id="Prog.NumCap"></span>    NumCap <a href="/builtin/#int">int</a> <span class="comment">// number of InstCapture insts in re</span>
}</pre>

A Prog is a compiled regular expression program.

<h3 id="Compile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/compile.go#L70">Compile</a>
    <a href="#Compile">¶</a></h3>
<pre>func Compile(re *<a href="#Regexp">Regexp</a>) (*<a href="#Prog">Prog</a>, <a href="/builtin/#error">error</a>)</pre>

Compile compiles the regexp into a program to be executed. The regexp should
have been simplified already (returned from re.Simplify).

<h3 id="Prog.Prefix">func (*Prog) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L139">Prefix</a>
    <a href="#Prog.Prefix">¶</a></h3>
<pre>func (p *<a href="#Prog">Prog</a>) Prefix() (prefix <a href="/builtin/#string">string</a>, complete <a href="/builtin/#bool">bool</a>)</pre>

Prefix returns a literal string that all matches for the regexp must start with.
Complete is true if the prefix is the entire match.

<h3 id="Prog.StartCond">func (*Prog) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L158">StartCond</a>
    <a href="#Prog.StartCond">¶</a></h3>
<pre>func (p *<a href="#Prog">Prog</a>) StartCond() <a href="#EmptyOp">EmptyOp</a></pre>

StartCond returns the leading empty-width conditions that must be true in any
match. It returns ^EmptyOp(0) if no matches are possible.

<h3 id="Prog.String">func (*Prog) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/prog.go#L109">String</a>
    <a href="#Prog.String">¶</a></h3>
<pre>func (p *<a href="#Prog">Prog</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Regexp">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/regexp.go#L8">Regexp</a>
    <a href="#Regexp">¶</a></h2>
<pre>type Regexp struct {
<span id="Regexp.Op"></span>    Op       <a href="#Op">Op</a> <span class="comment">// operator</span>
<span id="Regexp.Flags"></span>    Flags    <a href="#Flags">Flags</a>
<span id="Regexp.Sub"></span>    Sub      []*<a href="#Regexp">Regexp</a>  <span class="comment">// subexpressions, if any</span>
<span id="Regexp.Sub0"></span>    Sub0     [1]*<a href="#Regexp">Regexp</a> <span class="comment">// storage for short Sub</span>
<span id="Regexp.Rune"></span>    Rune     []<a href="/builtin/#rune">rune</a>     <span class="comment">// matched runes, for OpLiteral, OpCharClass</span>
<span id="Regexp.Rune0"></span>    Rune0    [2]<a href="/builtin/#rune">rune</a>    <span class="comment">// storage for short Rune</span>
<span id="Regexp.Min"></span>    Min, Max <a href="/builtin/#int">int</a>        <span class="comment">// min, max for OpRepeat</span>
<span id="Regexp.Cap"></span>    Cap      <a href="/builtin/#int">int</a>        <span class="comment">// capturing index, for OpCapture</span>
<span id="Regexp.Name"></span>    Name     <a href="/builtin/#string">string</a>     <span class="comment">// capturing name, for OpCapture</span>
}</pre>

A Regexp is a node in a regular expression syntax tree.

<h3 id="Parse">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/parse.go#L691">Parse</a>
    <a href="#Parse">¶</a></h3>
<pre>func Parse(s <a href="/builtin/#string">string</a>, flags <a href="#Flags">Flags</a>) (*<a href="#Regexp">Regexp</a>, <a href="/builtin/#error">error</a>)</pre>

Parse parses a regular expression string s, controlled by the specified Flags,
and returns a regular expression parse tree. The syntax is described in the
top-level comment.

<h3 id="Regexp.CapNames">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/regexp.go#L296">CapNames</a>
    <a href="#Regexp.CapNames">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) CapNames() []<a href="/builtin/#string">string</a></pre>

CapNames walks the regexp to find the names of capturing groups.

<h3 id="Regexp.Equal">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/regexp.go#L52">Equal</a>
    <a href="#Regexp.Equal">¶</a></h3>
<pre>func (x *<a href="#Regexp">Regexp</a>) Equal(y *<a href="#Regexp">Regexp</a>) <a href="/builtin/#bool">bool</a></pre>

Equal returns true if x and y have identical structure.

<h3 id="Regexp.MaxCap">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/regexp.go#L282">MaxCap</a>
    <a href="#Regexp.MaxCap">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) MaxCap() <a href="/builtin/#int">int</a></pre>

MaxCap walks the regexp to find the maximum capture index.

<h3 id="Regexp.Simplify">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/simplify.go#L4">Simplify</a>
    <a href="#Regexp.Simplify">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) Simplify() *<a href="#Regexp">Regexp</a></pre>

Simplify returns a regexp equivalent to re but without counted repetitions and
with various other simplifications, such as rewriting /(?:a+)+/ to /a+/. The
resulting regexp will execute correctly but its string representation will not
produce the same parse tree, because capturing parentheses may have been
duplicated or removed. For example, the simplified form for /(x){1,2}/ is
/(x)(x)?/ but both parentheses capture as $1. The returned regexp may share
structure with or be the original.

<h3 id="Regexp.String">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/syntax/regexp.go#L235">String</a>
    <a href="#Regexp.String">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) String() <a href="/builtin/#string">string</a></pre>



