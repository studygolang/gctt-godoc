version: 1.10
## package regexp

  `import "regexp"`

## Overview

Package regexp implements regular expression search.

The syntax of the regular expressions accepted is the same general syntax used
by Perl, Python, and other languages. More precisely, it is the syntax accepted
by RE2 and described at https://golang.org/s/re2syntax, except for \C. For an
overview of the syntax, run

    go doc regexp/syntax

The regexp implementation provided by this package is guaranteed to run in time
linear in the size of the input. (This is a property not guaranteed by most open
source implementations of regular expressions.) For more information about this
property, see

    http://swtch.com/~rsc/regexp/regexp1.html

or any book about automata theory.

All characters are UTF-8-encoded code points.

There are 16 methods of Regexp that match a regular expression and identify the
matched text. Their names are matched by this regular expression:

    Find(All)?(String)?(Submatch)?(Index)?

If 'All' is present, the routine matches successive non-overlapping matches of
the entire expression. Empty matches abutting a preceding match are ignored. The
return value is a slice containing the successive return values of the
corresponding non-'All' routine. These routines take an extra integer argument,
n; if n >= 0, the function returns at most n matches/submatches.

If 'String' is present, the argument is a string; otherwise it is a slice of
bytes; return values are adjusted as appropriate.

If 'Submatch' is present, the return value is a slice identifying the successive
submatches of the expression. Submatches are matches of parenthesized
subexpressions (also known as capturing groups) within the regular expression,
numbered from left to right in order of opening parenthesis. Submatch 0 is the
match of the entire expression, submatch 1 the match of the first parenthesized
subexpression, and so on.

If 'Index' is present, matches and submatches are identified by byte index pairs
within the input string: result[2*n:2*n+1] identifies the indexes of the nth
submatch. The pair for n==0 identifies the match of the entire expression. If
'Index' is not present, the match is identified by the text of the
match/submatch. If an index is negative, it means that subexpression did not
match any string in the input.

There is also a subset of the methods that can be applied to text read from a
RuneReader:

    MatchReader, FindReaderIndex, FindReaderSubmatchIndex

This set may grow. Note that regular expression matches may need to examine text
beyond the text returned by a match, so the methods that match text from a
RuneReader may read arbitrarily far into the input before returning.

(There are a few other methods that do not match this pattern.)

<a id="example"></a>
Example:

    // Compile the expression once, usually at init time.
    // Use raw strings to avoid having to quote the backslashes.
    var validID = regexp.MustCompile(`^[a-z]+\[[0-9]+\]$`)

    fmt.Println(validID.MatchString("adam[23]"))
    fmt.Println(validID.MatchString("eve[7]"))
    fmt.Println(validID.MatchString("Job[48]"))
    fmt.Println(validID.MatchString("snakey"))
    // Output:
    // true
    // true
    // false
    // false

## Index

- [func Match(pattern string, b []byte) (matched bool, err error)](#Match)
- [func MatchReader(pattern string, r io.RuneReader) (matched bool, err error)](#MatchReader)
- [func MatchString(pattern string, s string) (matched bool, err error)](#MatchString)
- [func QuoteMeta(s string) string](#QuoteMeta)
- [type Regexp](#Regexp)
  - [func Compile(expr string) (*Regexp, error)](#Compile)
  - [func CompilePOSIX(expr string) (*Regexp, error)](#CompilePOSIX)
  - [func MustCompile(str string) *Regexp](#MustCompile)
  - [func MustCompilePOSIX(str string) *Regexp](#MustCompilePOSIX)
  - [func (re *Regexp) Copy() *Regexp](#Regexp.Copy)
  - [func (re *Regexp) Expand(dst []byte, template []byte, src []byte, match []int) []byte](#Regexp.Expand)
  - [func (re *Regexp) ExpandString(dst []byte, template string, src string, match []int) []byte](#Regexp.ExpandString)
  - [func (re *Regexp) Find(b []byte) []byte](#Regexp.Find)
  - [func (re *Regexp) FindAll(b []byte, n int) [][]byte](#Regexp.FindAll)
  - [func (re *Regexp) FindAllIndex(b []byte, n int) [][]int](#Regexp.FindAllIndex)
  - [func (re *Regexp) FindAllString(s string, n int) []string](#Regexp.FindAllString)
  - [func (re *Regexp) FindAllStringIndex(s string, n int) [][]int](#Regexp.FindAllStringIndex)
  - [func (re *Regexp) FindAllStringSubmatch(s string, n int) [][]string](#Regexp.FindAllStringSubmatch)
  - [func (re *Regexp) FindAllStringSubmatchIndex(s string, n int) [][]int](#Regexp.FindAllStringSubmatchIndex)
  - [func (re *Regexp) FindAllSubmatch(b []byte, n int) [][][]byte](#Regexp.FindAllSubmatch)
  - [func (re *Regexp) FindAllSubmatchIndex(b []byte, n int) [][]int](#Regexp.FindAllSubmatchIndex)
  - [func (re *Regexp) FindIndex(b []byte) (loc []int)](#Regexp.FindIndex)
  - [func (re *Regexp) FindReaderIndex(r io.RuneReader) (loc []int)](#Regexp.FindReaderIndex)
  - [func (re *Regexp) FindReaderSubmatchIndex(r io.RuneReader) []int](#Regexp.FindReaderSubmatchIndex)
  - [func (re *Regexp) FindString(s string) string](#Regexp.FindString)
  - [func (re *Regexp) FindStringIndex(s string) (loc []int)](#Regexp.FindStringIndex)
  - [func (re *Regexp) FindStringSubmatch(s string) []string](#Regexp.FindStringSubmatch)
  - [func (re *Regexp) FindStringSubmatchIndex(s string) []int](#Regexp.FindStringSubmatchIndex)
  - [func (re *Regexp) FindSubmatch(b []byte) [][]byte](#Regexp.FindSubmatch)
  - [func (re *Regexp) FindSubmatchIndex(b []byte) []int](#Regexp.FindSubmatchIndex)
  - [func (re *Regexp) LiteralPrefix() (prefix string, complete bool)](#Regexp.LiteralPrefix)
  - [func (re *Regexp) Longest()](#Regexp.Longest)
  - [func (re *Regexp) Match(b []byte) bool](#Regexp.Match)
  - [func (re *Regexp) MatchReader(r io.RuneReader) bool](#Regexp.MatchReader)
  - [func (re *Regexp) MatchString(s string) bool](#Regexp.MatchString)
  - [func (re *Regexp) NumSubexp() int](#Regexp.NumSubexp)
  - [func (re *Regexp) ReplaceAll(src, repl []byte) []byte](#Regexp.ReplaceAll)
  - [func (re *Regexp) ReplaceAllFunc(src []byte, repl func([]byte) []byte) []byte](#Regexp.ReplaceAllFunc)
  - [func (re *Regexp) ReplaceAllLiteral(src, repl []byte) []byte](#Regexp.ReplaceAllLiteral)
  - [func (re *Regexp) ReplaceAllLiteralString(src, repl string) string](#Regexp.ReplaceAllLiteralString)
  - [func (re *Regexp) ReplaceAllString(src, repl string) string](#Regexp.ReplaceAllString)
  - [func (re *Regexp) ReplaceAllStringFunc(src string, repl func(string) string) string](#Regexp.ReplaceAllStringFunc)
  - [func (re *Regexp) Split(s string, n int) []string](#Regexp.Split)
  - [func (re *Regexp) String() string](#Regexp.String)
  - [func (re *Regexp) SubexpNames() []string](#Regexp.SubexpNames)

### Examples

- [Package](#example)
- [MatchString](#exampleMatchString)
- [Regexp.Expand](#exampleRegexp_Expand)
- [Regexp.ExpandString](#exampleRegexp_ExpandString)
- [Regexp.FindAllString](#exampleRegexp_FindAllString)
- [Regexp.FindAllStringSubmatch](#exampleRegexp_FindAllStringSubmatch)
- [Regexp.FindAllStringSubmatchIndex](#exampleRegexp_FindAllStringSubmatchIndex)
- [Regexp.FindString](#exampleRegexp_FindString)
- [Regexp.FindStringIndex](#exampleRegexp_FindStringIndex)
- [Regexp.FindStringSubmatch](#exampleRegexp_FindStringSubmatch)
- [Regexp.MatchString](#exampleRegexp_MatchString)
- [Regexp.ReplaceAllLiteralString](#exampleRegexp_ReplaceAllLiteralString)
- [Regexp.ReplaceAllString](#exampleRegexp_ReplaceAllString)
- [Regexp.Split](#exampleRegexp_Split)
- [Regexp.SubexpNames](#exampleRegexp_SubexpNames)

### Package files
 [backtrack.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/backtrack.go) [exec.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/exec.go) [onepass.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/onepass.go) [regexp.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go)

<h2 id="Match">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L458">Match</a>
    <a href="#Match">¶</a></h2>
<pre>func Match(pattern <a href="/builtin/#string">string</a>, b []<a href="/builtin/#byte">byte</a>) (matched <a href="/builtin/#bool">bool</a>, err <a href="/builtin/#error">error</a>)</pre>

Match checks whether a textual regular expression matches a byte slice. More
complicated queries need to use Compile and the full Regexp interface.

<h2 id="MatchReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L436">MatchReader</a>
    <a href="#MatchReader">¶</a></h2>
<pre>func MatchReader(pattern <a href="/builtin/#string">string</a>, r <a href="/io/">io</a>.<a href="/io/#RuneReader">RuneReader</a>) (matched <a href="/builtin/#bool">bool</a>, err <a href="/builtin/#error">error</a>)</pre>

MatchReader checks whether a textual regular expression matches the text read by
the RuneReader. More complicated queries need to use Compile and the full Regexp
interface.

<h2 id="MatchString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L447">MatchString</a>
    <a href="#MatchString">¶</a></h2>
<pre>func MatchString(pattern <a href="/builtin/#string">string</a>, s <a href="/builtin/#string">string</a>) (matched <a href="/builtin/#bool">bool</a>, err <a href="/builtin/#error">error</a>)</pre>

MatchString checks whether a textual regular expression matches a string. More
complicated queries need to use Compile and the full Regexp interface.

<a id="exampleMatchString"></a>
Example:

    matched, err := regexp.MatchString("foo.*", "seafood")
    fmt.Println(matched, err)
    matched, err = regexp.MatchString("bar.*", "seafood")
    fmt.Println(matched, err)
    matched, err = regexp.MatchString("a(b", "seafood")
    fmt.Println(matched, err)
    // Output:
    // true <nil>
    // false <nil>
    // false error parsing regexp: missing closing ): `a(b`

<h2 id="QuoteMeta">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L619">QuoteMeta</a>
    <a href="#QuoteMeta">¶</a></h2>
<pre>func QuoteMeta(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

QuoteMeta returns a string that quotes all regular expression metacharacters
inside the argument text; the returned string is a regular expression matching
the literal text. For example, QuoteMeta(`[foo]`) returns `\[foo\]`.

<h2 id="Regexp">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L71">Regexp</a>
    <a href="#Regexp">¶</a></h2>
<pre>type Regexp struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Regexp is the representation of a compiled regular expression. A Regexp is safe
for concurrent use by multiple goroutines, except for configuration methods,
such as Longest.

<h3 id="Compile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L122">Compile</a>
    <a href="#Compile">¶</a></h3>
<pre>func Compile(expr <a href="/builtin/#string">string</a>) (*<a href="#Regexp">Regexp</a>, <a href="/builtin/#error">error</a>)</pre>

Compile parses a regular expression and returns, if successful, a Regexp object
that can be used to match against text.

When matching against text, the regexp returns a match that begins as early as
possible in the input (leftmost), and among those it chooses the one that a
backtracking search would have found first. This so-called leftmost-first
matching is the same semantics that Perl, Python, and other implementations use,
although this package implements it without the expense of backtracking. For
POSIX leftmost-longest matching, see CompilePOSIX.

<h3 id="CompilePOSIX">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L145">CompilePOSIX</a>
    <a href="#CompilePOSIX">¶</a></h3>
<pre>func CompilePOSIX(expr <a href="/builtin/#string">string</a>) (*<a href="#Regexp">Regexp</a>, <a href="/builtin/#error">error</a>)</pre>

CompilePOSIX is like Compile but restricts the regular expression to POSIX ERE
(egrep) syntax and changes the match semantics to leftmost-longest.

That is, when matching against text, the regexp returns a match that begins as
early as possible in the input (leftmost), and among those it chooses a match
that is as long as possible. This so-called leftmost-longest matching is the
same semantics that early regular expression implementations used and that POSIX
specifies.

However, there can be multiple leftmost-longest matches, with different submatch
choices, and here this package diverges from POSIX. Among the possible
leftmost-longest matches, this package chooses the one that a backtracking
search would have found first, while POSIX specifies that the match be chosen to
maximize the length of the first subexpression, then the second, and so on from
left to right. The POSIX rule is computationally prohibitive and not even
well-defined. See http://swtch.com/~rsc/regexp/regexp2.html#posix for details.

<h3 id="MustCompile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L227">MustCompile</a>
    <a href="#MustCompile">¶</a></h3>
<pre>func MustCompile(str <a href="/builtin/#string">string</a>) *<a href="#Regexp">Regexp</a></pre>

MustCompile is like Compile but panics if the expression cannot be parsed. It
simplifies safe initialization of global variables holding compiled regular
expressions.

<h3 id="MustCompilePOSIX">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L238">MustCompilePOSIX</a>
    <a href="#MustCompilePOSIX">¶</a></h3>
<pre>func MustCompilePOSIX(str <a href="/builtin/#string">string</a>) *<a href="#Regexp">Regexp</a></pre>

MustCompilePOSIX is like CompilePOSIX but panics if the expression cannot be
parsed. It simplifies safe initialization of global variables holding compiled
regular expressions.

<h3 id="Regexp.Copy">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L104">Copy</a>
    <a href="#Regexp.Copy">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) Copy() *<a href="#Regexp">Regexp</a></pre>

Copy returns a new Regexp object copied from re.

When using a Regexp in multiple goroutines, giving each goroutine its own copy
helps to avoid lock contention.

<h3 id="Regexp.Expand">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L809">Expand</a>
    <a href="#Regexp.Expand">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) Expand(dst []<a href="/builtin/#byte">byte</a>, template []<a href="/builtin/#byte">byte</a>, src []<a href="/builtin/#byte">byte</a>, match []<a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

Expand appends template to dst and returns the result; during the append, Expand
replaces variables in the template with corresponding matches drawn from src.
The match slice should have been returned by FindSubmatchIndex.

In the template, a variable is denoted by a substring of the form $name or
${name}, where name is a non-empty sequence of letters, digits, and underscores.
A purely numeric name like $1 refers to the submatch with the corresponding
index; other names refer to capturing parentheses named with the (?P<name>...)
syntax. A reference to an out of range or unmatched index or a name that is not
present in the regular expression is replaced with an empty slice.

In the $name form, name is taken to be as long as possible: $1x is equivalent to
${1x}, not ${1}x, and, $10 is equivalent to ${10}, not ${1}0.

To insert a literal $ in the output, use $$ in the template.

<a id="exampleRegexp_Expand"></a>
Example:

    content := []byte(`
    	# comment line
    	option1: value1
    	option2: value2
    
    	# another comment line
    	option3: value3
    `)

    // Regex pattern captures "key: value" pair from the content.
    pattern := regexp.MustCompile(`(?m)(?P<key>\w+):\s+(?P<value>\w+)$`)

    // Template to convert "key: value" to "key=value" by
    // referencing the values captured by the regex pattern.
    template := []byte("$key=$value\n")

    result := []byte{}

    // For each match of the regex in the content.
    for _, submatches := range pattern.FindAllSubmatchIndex(content, -1) {
        // Apply the captured submatches to the template and append the output
        // to the result.
        result = pattern.Expand(result, template, content, submatches)
    }
    fmt.Println(string(result))
    // Output:
    // option1=value1
    // option2=value2
    // option3=value3

<h3 id="Regexp.ExpandString">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L816">ExpandString</a>
    <a href="#Regexp.ExpandString">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) ExpandString(dst []<a href="/builtin/#byte">byte</a>, template <a href="/builtin/#string">string</a>, src <a href="/builtin/#string">string</a>, match []<a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

ExpandString is like Expand but the template and source are strings. It appends
to and returns a byte slice in order to give the calling code control over
allocation.

<a id="exampleRegexp_ExpandString"></a>
Example:

    content := `
    	# comment line
    	option1: value1
    	option2: value2
    
    	# another comment line
    	option3: value3
    `

    // Regex pattern captures "key: value" pair from the content.
    pattern := regexp.MustCompile(`(?m)(?P<key>\w+):\s+(?P<value>\w+)$`)

    // Template to convert "key: value" to "key=value" by
    // referencing the values captured by the regex pattern.
    template := "$key=$value\n"

    result := []byte{}

    // For each match of the regex in the content.
    for _, submatches := range pattern.FindAllStringSubmatchIndex(content, -1) {
        // Apply the captured submatches to the template and append the output
        // to the result.
        result = pattern.ExpandString(result, template, content, submatches)
    }
    fmt.Println(string(result))
    // Output:
    // option1=value1
    // option2=value2
    // option3=value3

<h3 id="Regexp.Find">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L712">Find</a>
    <a href="#Regexp.Find">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) Find(b []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

Find returns a slice holding the text of the leftmost match in b of the regular
expression. A return value of nil indicates no match.

<h3 id="Regexp.FindAll">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L973">FindAll</a>
    <a href="#Regexp.FindAll">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindAll(b []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) [][]<a href="/builtin/#byte">byte</a></pre>

FindAll is the 'All' version of Find; it returns a slice of all successive
matches of the expression, as defined by the 'All' description in the package
comment. A return value of nil indicates no match.

<h3 id="Regexp.FindAllIndex">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L991">FindAllIndex</a>
    <a href="#Regexp.FindAllIndex">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindAllIndex(b []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) [][]<a href="/builtin/#int">int</a></pre>

FindAllIndex is the 'All' version of FindIndex; it returns a slice of all
successive matches of the expression, as defined by the 'All' description in the
package comment. A return value of nil indicates no match.

<h3 id="Regexp.FindAllString">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L1009">FindAllString</a>
    <a href="#Regexp.FindAllString">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindAllString(s <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) []<a href="/builtin/#string">string</a></pre>

FindAllString is the 'All' version of FindString; it returns a slice of all
successive matches of the expression, as defined by the 'All' description in the
package comment. A return value of nil indicates no match.

<a id="exampleRegexp_FindAllString"></a>
Example:

    re := regexp.MustCompile("a.")
    fmt.Println(re.FindAllString("paranormal", -1))
    fmt.Println(re.FindAllString("paranormal", 2))
    fmt.Println(re.FindAllString("graal", -1))
    fmt.Println(re.FindAllString("none", -1))
    // Output:
    // [ar an al]
    // [ar an]
    // [aa]
    // []

<h3 id="Regexp.FindAllStringIndex">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L1027">FindAllStringIndex</a>
    <a href="#Regexp.FindAllStringIndex">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindAllStringIndex(s <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) [][]<a href="/builtin/#int">int</a></pre>

FindAllStringIndex is the 'All' version of FindStringIndex; it returns a slice
of all successive matches of the expression, as defined by the 'All' description
in the package comment. A return value of nil indicates no match.

<h3 id="Regexp.FindAllStringSubmatch">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L1087">FindAllStringSubmatch</a>
    <a href="#Regexp.FindAllStringSubmatch">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindAllStringSubmatch(s <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) [][]<a href="/builtin/#string">string</a></pre>

FindAllStringSubmatch is the 'All' version of FindStringSubmatch; it returns a
slice of all successive matches of the expression, as defined by the 'All'
description in the package comment. A return value of nil indicates no match.

<a id="exampleRegexp_FindAllStringSubmatch"></a>
Example:

    re := regexp.MustCompile("a(x*)b")
    fmt.Printf("%q\n", re.FindAllStringSubmatch("-ab-", -1))
    fmt.Printf("%q\n", re.FindAllStringSubmatch("-axxb-", -1))
    fmt.Printf("%q\n", re.FindAllStringSubmatch("-ab-axb-", -1))
    fmt.Printf("%q\n", re.FindAllStringSubmatch("-axxb-ab-", -1))
    // Output:
    // [["ab" ""]]
    // [["axxb" "xx"]]
    // [["ab" ""] ["axb" "x"]]
    // [["axxb" "xx"] ["ab" ""]]

<h3 id="Regexp.FindAllStringSubmatchIndex">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L1112">FindAllStringSubmatchIndex</a>
    <a href="#Regexp.FindAllStringSubmatchIndex">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindAllStringSubmatchIndex(s <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) [][]<a href="/builtin/#int">int</a></pre>

FindAllStringSubmatchIndex is the 'All' version of FindStringSubmatchIndex; it
returns a slice of all successive matches of the expression, as defined by the
'All' description in the package comment. A return value of nil indicates no
match.

<a id="exampleRegexp_FindAllStringSubmatchIndex"></a>
Example:

    re := regexp.MustCompile("a(x*)b")
    // Indices:
    //    01234567   012345678
    //    -ab-axb-   -axxb-ab-
    fmt.Println(re.FindAllStringSubmatchIndex("-ab-", -1))
    fmt.Println(re.FindAllStringSubmatchIndex("-axxb-", -1))
    fmt.Println(re.FindAllStringSubmatchIndex("-ab-axb-", -1))
    fmt.Println(re.FindAllStringSubmatchIndex("-axxb-ab-", -1))
    fmt.Println(re.FindAllStringSubmatchIndex("-foo-", -1))
    // Output:
    // [[1 3 2 2]]
    // [[1 5 2 4]]
    // [[1 3 2 2] [4 7 5 6]]
    // [[1 5 2 4] [6 8 7 7]]
    // []

<h3 id="Regexp.FindAllSubmatch">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L1045">FindAllSubmatch</a>
    <a href="#Regexp.FindAllSubmatch">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindAllSubmatch(b []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) [][][]<a href="/builtin/#byte">byte</a></pre>

FindAllSubmatch is the 'All' version of FindSubmatch; it returns a slice of all
successive matches of the expression, as defined by the 'All' description in the
package comment. A return value of nil indicates no match.

<h3 id="Regexp.FindAllSubmatchIndex">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L1069">FindAllSubmatchIndex</a>
    <a href="#Regexp.FindAllSubmatchIndex">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindAllSubmatchIndex(b []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) [][]<a href="/builtin/#int">int</a></pre>

FindAllSubmatchIndex is the 'All' version of FindSubmatchIndex; it returns a
slice of all successive matches of the expression, as defined by the 'All'
description in the package comment. A return value of nil indicates no match.

<h3 id="Regexp.FindIndex">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L725">FindIndex</a>
    <a href="#Regexp.FindIndex">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindIndex(b []<a href="/builtin/#byte">byte</a>) (loc []<a href="/builtin/#int">int</a>)</pre>

FindIndex returns a two-element slice of integers defining the location of the
leftmost match in b of the regular expression. The match itself is at
b[loc[0]:loc[1]]. A return value of nil indicates no match.

<h3 id="Regexp.FindReaderIndex">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L764">FindReaderIndex</a>
    <a href="#Regexp.FindReaderIndex">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindReaderIndex(r <a href="/io/">io</a>.<a href="/io/#RuneReader">RuneReader</a>) (loc []<a href="/builtin/#int">int</a>)</pre>

FindReaderIndex returns a two-element slice of integers defining the location of
the leftmost match of the regular expression in text read from the RuneReader.
The match text was found in the input stream at byte offset loc[0] through
loc[1]-1. A return value of nil indicates no match.

<h3 id="Regexp.FindReaderSubmatchIndex">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L963">FindReaderSubmatchIndex</a>
    <a href="#Regexp.FindReaderSubmatchIndex">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindReaderSubmatchIndex(r <a href="/io/">io</a>.<a href="/io/#RuneReader">RuneReader</a>) []<a href="/builtin/#int">int</a></pre>

FindReaderSubmatchIndex returns a slice holding the index pairs identifying the
leftmost match of the regular expression of text read by the RuneReader, and the
matches, if any, of its subexpressions, as defined by the 'Submatch' and 'Index'
descriptions in the package comment. A return value of nil indicates no match.

<h3 id="Regexp.FindString">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L738">FindString</a>
    <a href="#Regexp.FindString">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindString(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

FindString returns a string holding the text of the leftmost match in s of the
regular expression. If there is no match, the return value is an empty string,
but it will also be empty if the regular expression successfully matches an
empty string. Use FindStringIndex or FindStringSubmatch if it is necessary to
distinguish these cases.

<a id="exampleRegexp_FindString"></a>
Example:

    re := regexp.MustCompile("foo.?")
    fmt.Printf("%q\n", re.FindString("seafood fool"))
    fmt.Printf("%q\n", re.FindString("meat"))
    // Output:
    // "food"
    // ""

<h3 id="Regexp.FindStringIndex">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L751">FindStringIndex</a>
    <a href="#Regexp.FindStringIndex">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindStringIndex(s <a href="/builtin/#string">string</a>) (loc []<a href="/builtin/#int">int</a>)</pre>

FindStringIndex returns a two-element slice of integers defining the location of
the leftmost match in s of the regular expression. The match itself is at
s[loc[0]:loc[1]]. A return value of nil indicates no match.

<a id="exampleRegexp_FindStringIndex"></a>
Example:

    re := regexp.MustCompile("ab?")
    fmt.Println(re.FindStringIndex("tablett"))
    fmt.Println(re.FindStringIndex("foo") == nil)
    // Output:
    // [1 3]
    // true

<h3 id="Regexp.FindStringSubmatch">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L934">FindStringSubmatch</a>
    <a href="#Regexp.FindStringSubmatch">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindStringSubmatch(s <a href="/builtin/#string">string</a>) []<a href="/builtin/#string">string</a></pre>

FindStringSubmatch returns a slice of strings holding the text of the leftmost
match of the regular expression in s and the matches, if any, of its
subexpressions, as defined by the 'Submatch' description in the package comment.
A return value of nil indicates no match.

<a id="exampleRegexp_FindStringSubmatch"></a>
Example:

    re := regexp.MustCompile("a(x*)b(y|z)c")
    fmt.Printf("%q\n", re.FindStringSubmatch("-axxxbyc-"))
    fmt.Printf("%q\n", re.FindStringSubmatch("-abzc-"))
    // Output:
    // ["axxxbyc" "xxx" "y"]
    // ["abzc" "" "z"]

<h3 id="Regexp.FindStringSubmatchIndex">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L954">FindStringSubmatchIndex</a>
    <a href="#Regexp.FindStringSubmatchIndex">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindStringSubmatchIndex(s <a href="/builtin/#string">string</a>) []<a href="/builtin/#int">int</a></pre>

FindStringSubmatchIndex returns a slice holding the index pairs identifying the
leftmost match of the regular expression in s and the matches, if any, of its
subexpressions, as defined by the 'Submatch' and 'Index' descriptions in the
package comment. A return value of nil indicates no match.

<h3 id="Regexp.FindSubmatch">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L777">FindSubmatch</a>
    <a href="#Regexp.FindSubmatch">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindSubmatch(b []<a href="/builtin/#byte">byte</a>) [][]<a href="/builtin/#byte">byte</a></pre>

FindSubmatch returns a slice of slices holding the text of the leftmost match of
the regular expression in b and the matches, if any, of its subexpressions, as
defined by the 'Submatch' descriptions in the package comment. A return value of
nil indicates no match.

<h3 id="Regexp.FindSubmatchIndex">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L925">FindSubmatchIndex</a>
    <a href="#Regexp.FindSubmatchIndex">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) FindSubmatchIndex(b []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#int">int</a></pre>

FindSubmatchIndex returns a slice holding the index pairs identifying the
leftmost match of the regular expression in b and the matches, if any, of its
subexpressions, as defined by the 'Submatch' and 'Index' descriptions in the
package comment. A return value of nil indicates no match.

<h3 id="Regexp.LiteralPrefix">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L413">LiteralPrefix</a>
    <a href="#Regexp.LiteralPrefix">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) LiteralPrefix() (prefix <a href="/builtin/#string">string</a>, complete <a href="/builtin/#bool">bool</a>)</pre>

LiteralPrefix returns a literal string that must begin any match of the regular
expression re. It returns the boolean true if the literal string comprises the
entire regular expression.

<h3 id="Regexp.Longest">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L155">Longest</a>
    <a href="#Regexp.Longest">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) Longest()</pre>

Longest makes future searches prefer the leftmost-longest match. That is, when
matching against text, the regexp returns a match that begins as early as
possible in the input (leftmost), and among those it chooses a match that is as
long as possible. This method modifies the Regexp and may not be called
concurrently with any other methods.

<h3 id="Regexp.Match">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L429">Match</a>
    <a href="#Regexp.Match">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) Match(b []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

Match reports whether the Regexp matches the byte slice b.

<h3 id="Regexp.MatchReader">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L419">MatchReader</a>
    <a href="#Regexp.MatchReader">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) MatchReader(r <a href="/io/">io</a>.<a href="/io/#RuneReader">RuneReader</a>) <a href="/builtin/#bool">bool</a></pre>

MatchReader reports whether the Regexp matches the text read by the RuneReader.

<h3 id="Regexp.MatchString">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L424">MatchString</a>
    <a href="#Regexp.MatchString">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) MatchString(s <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

MatchString reports whether the Regexp matches the string s.

<a id="exampleRegexp_MatchString"></a>
Example:

    re := regexp.MustCompile("(gopher){2}")
    fmt.Println(re.MatchString("gopher"))
    fmt.Println(re.MatchString("gophergopher"))
    fmt.Println(re.MatchString("gophergophergopher"))
    // Output:
    // false
    // true
    // true

<h3 id="Regexp.NumSubexp">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L254">NumSubexp</a>
    <a href="#Regexp.NumSubexp">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) NumSubexp() <a href="/builtin/#int">int</a></pre>

NumSubexp returns the number of parenthesized subexpressions in this Regexp.

<h3 id="Regexp.ReplaceAll">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L568">ReplaceAll</a>
    <a href="#Regexp.ReplaceAll">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) ReplaceAll(src, repl []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ReplaceAll returns a copy of src, replacing matches of the Regexp with the
replacement text repl. Inside repl, $ signs are interpreted as in Expand, so for
instance $1 represents the text of the first submatch.

<h3 id="Regexp.ReplaceAllFunc">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L596">ReplaceAllFunc</a>
    <a href="#Regexp.ReplaceAllFunc">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) ReplaceAllFunc(src []<a href="/builtin/#byte">byte</a>, repl func([]<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ReplaceAllFunc returns a copy of src in which all matches of the Regexp have
been replaced by the return value of function repl applied to the matched byte
slice. The replacement returned by repl is substituted directly, without using
Expand.

<h3 id="Regexp.ReplaceAllLiteral">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L586">ReplaceAllLiteral</a>
    <a href="#Regexp.ReplaceAllLiteral">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) ReplaceAllLiteral(src, repl []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ReplaceAllLiteral returns a copy of src, replacing matches of the Regexp with
the replacement bytes repl. The replacement repl is substituted directly,
without using Expand.

<h3 id="Regexp.ReplaceAllLiteralString">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L483">ReplaceAllLiteralString</a>
    <a href="#Regexp.ReplaceAllLiteralString">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) ReplaceAllLiteralString(src, repl <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ReplaceAllLiteralString returns a copy of src, replacing matches of the Regexp
with the replacement string repl. The replacement repl is substituted directly,
without using Expand.

<a id="exampleRegexp_ReplaceAllLiteralString"></a>
Example:

    re := regexp.MustCompile("a(x*)b")
    fmt.Println(re.ReplaceAllLiteralString("-ab-axxb-", "T"))
    fmt.Println(re.ReplaceAllLiteralString("-ab-axxb-", "$1"))
    fmt.Println(re.ReplaceAllLiteralString("-ab-axxb-", "${1}"))
    // Output:
    // -T-T-
    // -$1-$1-
    // -${1}-${1}-

<h3 id="Regexp.ReplaceAllString">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L469">ReplaceAllString</a>
    <a href="#Regexp.ReplaceAllString">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) ReplaceAllString(src, repl <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ReplaceAllString returns a copy of src, replacing matches of the Regexp with the
replacement string repl. Inside repl, $ signs are interpreted as in Expand, so
for instance $1 represents the text of the first submatch.

<a id="exampleRegexp_ReplaceAllString"></a>
Example:

    re := regexp.MustCompile("a(x*)b")
    fmt.Println(re.ReplaceAllString("-ab-axxb-", "T"))
    fmt.Println(re.ReplaceAllString("-ab-axxb-", "$1"))
    fmt.Println(re.ReplaceAllString("-ab-axxb-", "$1W"))
    fmt.Println(re.ReplaceAllString("-ab-axxb-", "${1}W"))
    // Output:
    // -T-T-
    // --xx-
    // ---
    // -W-xxW-

<h3 id="Regexp.ReplaceAllStringFunc">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L493">ReplaceAllStringFunc</a>
    <a href="#Regexp.ReplaceAllStringFunc">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) ReplaceAllStringFunc(src <a href="/builtin/#string">string</a>, repl func(<a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ReplaceAllStringFunc returns a copy of src in which all matches of the Regexp
have been replaced by the return value of function repl applied to the matched
substring. The replacement returned by repl is substituted directly, without
using Expand.

<h3 id="Regexp.Split">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L1141">Split</a>
    <a href="#Regexp.Split">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) Split(s <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) []<a href="/builtin/#string">string</a></pre>

Split slices s into substrings separated by the expression and returns a slice
of the substrings between those expression matches.

The slice returned by this method consists of all the substrings of s not
contained in the slice returned by FindAllString. When called on an expression
that contains no metacharacters, it is equivalent to strings.SplitN.

Example:

    s := regexp.MustCompile("a*").Split("abaabaccadaaae", 5)
    // s: ["", "b", "b", "c", "cadaaae"]

The count determines the number of substrings to return:

    n > 0: at most n substrings; the last substring will be the unsplit remainder.
    n == 0: the result is nil (zero substrings)
    n < 0: all substrings

<a id="exampleRegexp_Split"></a>
Example:

    a := regexp.MustCompile("a")
    fmt.Println(a.Split("banana", -1))
    fmt.Println(a.Split("banana", 0))
    fmt.Println(a.Split("banana", 1))
    fmt.Println(a.Split("banana", 2))
    zp := regexp.MustCompile("z+")
    fmt.Println(zp.Split("pizza", -1))
    fmt.Println(zp.Split("pizza", 0))
    fmt.Println(zp.Split("pizza", 1))
    fmt.Println(zp.Split("pizza", 2))
    // Output:
    // [b n n ]
    // []
    // [banana]
    // [b nana]
    // [pi a]
    // []
    // [pizza]
    // [pi a]

<h3 id="Regexp.String">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L96">String</a>
    <a href="#Regexp.String">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) String() <a href="/builtin/#string">string</a></pre>

String returns the source text used to compile the regular expression.

<h3 id="Regexp.SubexpNames">func (*Regexp) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/regexp/regexp.go#L263">SubexpNames</a>
    <a href="#Regexp.SubexpNames">¶</a></h3>
<pre>func (re *<a href="#Regexp">Regexp</a>) SubexpNames() []<a href="/builtin/#string">string</a></pre>

SubexpNames returns the names of the parenthesized subexpressions in this
Regexp. The name for the first sub-expression is names[1], so that if m is a
match slice, the name for m[i] is SubexpNames()[i]. Since the Regexp as a whole
cannot be named, names[0] is always the empty string. The slice should not be
modified.

<a id="exampleRegexp_SubexpNames"></a>
Example:

    re := regexp.MustCompile("(?P<first>[a-zA-Z]+) (?P<last>[a-zA-Z]+)")
    fmt.Println(re.MatchString("Alan Turing"))
    fmt.Printf("%q\n", re.SubexpNames())
    reversed := fmt.Sprintf("${%s} ${%s}", re.SubexpNames()[2], re.SubexpNames()[1])
    fmt.Println(reversed)
    fmt.Println(re.ReplaceAllString("Alan Turing", reversed))
    // Output:
    // true
    // ["" "first" "last"]
    // ${last} ${first}
    // Turing Alan

## Subdirectories
- [..](..)
- [syntax](syntax/)
