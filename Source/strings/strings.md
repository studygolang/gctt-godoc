version: 1.10
## package strings

  `import "strings"`

## Overview

Package strings implements simple functions to manipulate UTF-8 encoded strings.

For information about UTF-8 strings in Go, see https://blog.golang.org/strings.

## Index

- [func Compare(a, b string) int](#Compare)
- [func Contains(s, substr string) bool](#Contains)
- [func ContainsAny(s, chars string) bool](#ContainsAny)
- [func ContainsRune(s string, r rune) bool](#ContainsRune)
- [func Count(s, substr string) int](#Count)
- [func EqualFold(s, t string) bool](#EqualFold)
- [func Fields(s string) []string](#Fields)
- [func FieldsFunc(s string, f func(rune) bool) []string](#FieldsFunc)
- [func HasPrefix(s, prefix string) bool](#HasPrefix)
- [func HasSuffix(s, suffix string) bool](#HasSuffix)
- [func Index(s, substr string) int](#Index)
- [func IndexAny(s, chars string) int](#IndexAny)
- [func IndexByte(s string, c byte) int](#IndexByte)
- [func IndexFunc(s string, f func(rune) bool) int](#IndexFunc)
- [func IndexRune(s string, r rune) int](#IndexRune)
- [func Join(a []string, sep string) string](#Join)
- [func LastIndex(s, substr string) int](#LastIndex)
- [func LastIndexAny(s, chars string) int](#LastIndexAny)
- [func LastIndexByte(s string, c byte) int](#LastIndexByte)
- [func LastIndexFunc(s string, f func(rune) bool) int](#LastIndexFunc)
- [func Map(mapping func(rune) rune, s string) string](#Map)
- [func Repeat(s string, count int) string](#Repeat)
- [func Replace(s, old, new string, n int) string](#Replace)
- [func Split(s, sep string) []string](#Split)
- [func SplitAfter(s, sep string) []string](#SplitAfter)
- [func SplitAfterN(s, sep string, n int) []string](#SplitAfterN)
- [func SplitN(s, sep string, n int) []string](#SplitN)
- [func Title(s string) string](#Title)
- [func ToLower(s string) string](#ToLower)
- [func ToLowerSpecial(c unicode.SpecialCase, s string) string](#ToLowerSpecial)
- [func ToTitle(s string) string](#ToTitle)
- [func ToTitleSpecial(c unicode.SpecialCase, s string) string](#ToTitleSpecial)
- [func ToUpper(s string) string](#ToUpper)
- [func ToUpperSpecial(c unicode.SpecialCase, s string) string](#ToUpperSpecial)
- [func Trim(s string, cutset string) string](#Trim)
- [func TrimFunc(s string, f func(rune) bool) string](#TrimFunc)
- [func TrimLeft(s string, cutset string) string](#TrimLeft)
- [func TrimLeftFunc(s string, f func(rune) bool) string](#TrimLeftFunc)
- [func TrimPrefix(s, prefix string) string](#TrimPrefix)
- [func TrimRight(s string, cutset string) string](#TrimRight)
- [func TrimRightFunc(s string, f func(rune) bool) string](#TrimRightFunc)
- [func TrimSpace(s string) string](#TrimSpace)
- [func TrimSuffix(s, suffix string) string](#TrimSuffix)
- [type Builder](#Builder)
  - [func (b *Builder) Grow(n int)](#Builder.Grow)
  - [func (b *Builder) Len() int](#Builder.Len)
  - [func (b *Builder) Reset()](#Builder.Reset)
  - [func (b *Builder) String() string](#Builder.String)
  - [func (b *Builder) Write(p []byte) (int, error)](#Builder.Write)
  - [func (b *Builder) WriteByte(c byte) error](#Builder.WriteByte)
  - [func (b *Builder) WriteRune(r rune) (int, error)](#Builder.WriteRune)
  - [func (b *Builder) WriteString(s string) (int, error)](#Builder.WriteString)
- [type Reader](#Reader)
  - [func NewReader(s string) *Reader](#NewReader)
  - [func (r *Reader) Len() int](#Reader.Len)
  - [func (r *Reader) Read(b []byte) (n int, err error)](#Reader.Read)
  - [func (r *Reader) ReadAt(b []byte, off int64) (n int, err error)](#Reader.ReadAt)
  - [func (r *Reader) ReadByte() (byte, error)](#Reader.ReadByte)
  - [func (r *Reader) ReadRune() (ch rune, size int, err error)](#Reader.ReadRune)
  - [func (r *Reader) Reset(s string)](#Reader.Reset)
  - [func (r *Reader) Seek(offset int64, whence int) (int64, error)](#Reader.Seek)
  - [func (r *Reader) Size() int64](#Reader.Size)
  - [func (r *Reader) UnreadByte() error](#Reader.UnreadByte)
  - [func (r *Reader) UnreadRune() error](#Reader.UnreadRune)
  - [func (r *Reader) WriteTo(w io.Writer) (n int64, err error)](#Reader.WriteTo)
- [type Replacer](#Replacer)
  - [func NewReplacer(oldnew ...string) *Replacer](#NewReplacer)
  - [func (r *Replacer) Replace(s string) string](#Replacer.Replace)
  - [func (r *Replacer) WriteString(w io.Writer, s string) (n int, err error)](#Replacer.WriteString)
- [Bugs](#pkg-note-BUG)

### Examples

- [Builder](#exampleBuilder)
- [Compare](#exampleCompare)
- [Contains](#exampleContains)
- [ContainsAny](#exampleContainsAny)
- [ContainsRune](#exampleContainsRune)
- [Count](#exampleCount)
- [EqualFold](#exampleEqualFold)
- [Fields](#exampleFields)
- [FieldsFunc](#exampleFieldsFunc)
- [HasPrefix](#exampleHasPrefix)
- [HasSuffix](#exampleHasSuffix)
- [Index](#exampleIndex)
- [IndexAny](#exampleIndexAny)
- [IndexByte](#exampleIndexByte)
- [IndexFunc](#exampleIndexFunc)
- [IndexRune](#exampleIndexRune)
- [Join](#exampleJoin)
- [LastIndex](#exampleLastIndex)
- [LastIndexAny](#exampleLastIndexAny)
- [LastIndexByte](#exampleLastIndexByte)
- [LastIndexFunc](#exampleLastIndexFunc)
- [Map](#exampleMap)
- [NewReplacer](#exampleNewReplacer)
- [Repeat](#exampleRepeat)
- [Replace](#exampleReplace)
- [Split](#exampleSplit)
- [SplitAfter](#exampleSplitAfter)
- [SplitAfterN](#exampleSplitAfterN)
- [SplitN](#exampleSplitN)
- [Title](#exampleTitle)
- [ToLower](#exampleToLower)
- [ToLowerSpecial](#exampleToLowerSpecial)
- [ToTitle](#exampleToTitle)
- [ToTitleSpecial](#exampleToTitleSpecial)
- [ToUpper](#exampleToUpper)
- [ToUpperSpecial](#exampleToUpperSpecial)
- [Trim](#exampleTrim)
- [TrimFunc](#exampleTrimFunc)
- [TrimLeft](#exampleTrimLeft)
- [TrimLeftFunc](#exampleTrimLeftFunc)
- [TrimPrefix](#exampleTrimPrefix)
- [TrimRight](#exampleTrimRight)
- [TrimRightFunc](#exampleTrimRightFunc)
- [TrimSpace](#exampleTrimSpace)
- [TrimSuffix](#exampleTrimSuffix)

### Package files
 [builder.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go) [compare.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/compare.go) [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go) [replace.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/replace.go) [search.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/search.go) [strings.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go) [strings_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings_amd64.go) [strings_decl.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings_decl.go)

<h2 id="Compare">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/compare.go#L3">Compare</a>
    <a href="#Compare">¶</a></h2>
<pre>func Compare(a, b <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

Compare returns an integer comparing two strings lexicographically. The result
will be 0 if a==b, -1 if a < b, and +1 if a > b.

Compare is included only for symmetry with package bytes. It is usually clearer
and always faster to use the built-in string comparison operators ==, <, >, and
so on.

<a id="exampleCompare"></a>
Example:

    fmt.Println(strings.Compare("a", "b"))
    fmt.Println(strings.Compare("a", "a"))
    fmt.Println(strings.Compare("b", "a"))
    // Output:
    // -1
    // 0
    // 1

<h2 id="Contains">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L83">Contains</a>
    <a href="#Contains">¶</a></h2>
<pre>func Contains(s, substr <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

Contains reports whether substr is within s.

<a id="exampleContains"></a>
Example:

    fmt.Println(strings.Contains("seafood", "foo"))
    fmt.Println(strings.Contains("seafood", "bar"))
    fmt.Println(strings.Contains("seafood", ""))
    fmt.Println(strings.Contains("", ""))
    // Output:
    // true
    // false
    // true
    // true

<h2 id="ContainsAny">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L88">ContainsAny</a>
    <a href="#ContainsAny">¶</a></h2>
<pre>func ContainsAny(s, chars <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

ContainsAny reports whether any Unicode code points in chars are within s.

<a id="exampleContainsAny"></a>
Example:

    fmt.Println(strings.ContainsAny("team", "i"))
    fmt.Println(strings.ContainsAny("failure", "u & i"))
    fmt.Println(strings.ContainsAny("foo", ""))
    fmt.Println(strings.ContainsAny("", ""))
    // Output:
    // false
    // true
    // false
    // false

<h2 id="ContainsRune">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L93">ContainsRune</a>
    <a href="#ContainsRune">¶</a></h2>
<pre>func ContainsRune(s <a href="/builtin/#string">string</a>, r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

ContainsRune reports whether the Unicode code point r is within s.

<a id="exampleContainsRune"></a>
Example:

    // Finds whether a string contains a particular Unicode code point.
    // The code point for the lowercase letter "a", for example, is 97.
    fmt.Println(strings.ContainsRune("aardvark", 97))
    fmt.Println(strings.ContainsRune("timeout", 97))
    // Output:
    // true
    // false

<h2 id="Count">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings_amd64.go#L73">Count</a>
    <a href="#Count">¶</a></h2>
<pre>func Count(s, substr <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

Count counts the number of non-overlapping instances of substr in s. If substr
is an empty string, Count returns 1 + the number of Unicode code points in s.

<a id="exampleCount"></a>
Example:

    fmt.Println(strings.Count("cheese", "e"))
    fmt.Println(strings.Count("five", "")) // before & after each rune
    // Output:
    // 3
    // 5

<h2 id="EqualFold">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L867">EqualFold</a>
    <a href="#EqualFold">¶</a></h2>
<pre>func EqualFold(s, t <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

EqualFold reports whether s and t, interpreted as UTF-8 strings, are equal under
Unicode case-folding.

<a id="exampleEqualFold"></a>
Example:

    fmt.Println(strings.EqualFold("Go", "go"))
    // Output: true

<h2 id="Fields">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L309">Fields</a>
    <a href="#Fields">¶</a></h2>
<pre>func Fields(s <a href="/builtin/#string">string</a>) []<a href="/builtin/#string">string</a></pre>

Fields splits the string s around each instance of one or more consecutive white
space characters, as defined by unicode.IsSpace, returning a slice of substrings
of s or an empty slice if s contains only white space.

<a id="exampleFields"></a>
Example:

    fmt.Printf("Fields are: %q", strings.Fields("  foo bar  baz   "))
    // Output: Fields are: ["foo" "bar" "baz"]

<h2 id="FieldsFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L363">FieldsFunc</a>
    <a href="#FieldsFunc">¶</a></h2>
<pre>func FieldsFunc(s <a href="/builtin/#string">string</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) []<a href="/builtin/#string">string</a></pre>

FieldsFunc splits the string s at each run of Unicode code points c satisfying
f(c) and returns an array of slices of s. If all code points in s satisfy f(c)
or the string is empty, an empty slice is returned. FieldsFunc makes no
guarantees about the order in which it calls f(c). If f does not return
consistent results for a given c, FieldsFunc may crash.

<a id="exampleFieldsFunc"></a>
Example:

    f := func(c rune) bool {
        return !unicode.IsLetter(c) && !unicode.IsNumber(c)
    }
    fmt.Printf("Fields are: %q", strings.FieldsFunc("  foo1;bar2,baz3...", f))
    // Output: Fields are: ["foo1" "bar2" "baz3"]

<h2 id="HasPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L435">HasPrefix</a>
    <a href="#HasPrefix">¶</a></h2>
<pre>func HasPrefix(s, prefix <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

HasPrefix tests whether the string s begins with prefix.

<a id="exampleHasPrefix"></a>
Example:

    fmt.Println(strings.HasPrefix("Gopher", "Go"))
    fmt.Println(strings.HasPrefix("Gopher", "C"))
    fmt.Println(strings.HasPrefix("Gopher", ""))
    // Output:
    // true
    // false
    // true

<h2 id="HasSuffix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L440">HasSuffix</a>
    <a href="#HasSuffix">¶</a></h2>
<pre>func HasSuffix(s, suffix <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

HasSuffix tests whether the string s ends with suffix.

<a id="exampleHasSuffix"></a>
Example:

    fmt.Println(strings.HasSuffix("Amigo", "go"))
    fmt.Println(strings.HasSuffix("Amigo", "O"))
    fmt.Println(strings.HasSuffix("Amigo", "Ami"))
    fmt.Println(strings.HasSuffix("Amigo", ""))
    // Output:
    // true
    // false
    // false
    // true

<h2 id="Index">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings_amd64.go#L17">Index</a>
    <a href="#Index">¶</a></h2>
<pre>func Index(s, substr <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

Index returns the index of the first instance of substr in s, or -1 if substr is
not present in s.

<a id="exampleIndex"></a>
Example:

    fmt.Println(strings.Index("chicken", "ken"))
    fmt.Println(strings.Index("chicken", "dmr"))
    // Output:
    // 4
    // -1

<h2 id="IndexAny">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L158">IndexAny</a>
    <a href="#IndexAny">¶</a></h2>
<pre>func IndexAny(s, chars <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

IndexAny returns the index of the first instance of any Unicode code point from
chars in s, or -1 if no Unicode code point from chars is present in s.

<a id="exampleIndexAny"></a>
Example:

    fmt.Println(strings.IndexAny("chicken", "aeiouy"))
    fmt.Println(strings.IndexAny("crwth", "aeiouy"))
    // Output:
    // 2
    // -1

<h2 id="IndexByte">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings_decl.go#L1">IndexByte</a>
    <a href="#IndexByte">¶</a></h2>
<pre>func IndexByte(s <a href="/builtin/#string">string</a>, c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

IndexByte returns the index of the first instance of c in s, or -1 if c is not
present in s.

<a id="exampleIndexByte"></a>
Example:

    fmt.Println(strings.IndexByte("golang", 'g'))
    fmt.Println(strings.IndexByte("gophers", 'h'))
    fmt.Println(strings.IndexByte("golang", 'x'))
    // Output:
    // 0
    // 3
    // -1

<h2 id="IndexFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L698">IndexFunc</a>
    <a href="#IndexFunc">¶</a></h2>
<pre>func IndexFunc(s <a href="/builtin/#string">string</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#int">int</a></pre>

IndexFunc returns the index into s of the first Unicode code point satisfying
f(c), or -1 if none do.

<a id="exampleIndexFunc"></a>
Example:

    f := func(c rune) bool {
        return unicode.Is(unicode.Han, c)
    }
    fmt.Println(strings.IndexFunc("Hello, 世界", f))
    fmt.Println(strings.IndexFunc("Hello, world", f))
    // Output:
    // 7
    // -1

<h2 id="IndexRune">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L138">IndexRune</a>
    <a href="#IndexRune">¶</a></h2>
<pre>func IndexRune(s <a href="/builtin/#string">string</a>, r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#int">int</a></pre>

IndexRune returns the index of the first instance of the Unicode code point r,
or -1 if rune is not present in s. If r is utf8.RuneError, it returns the first
instance of any invalid UTF-8 byte sequence.

<a id="exampleIndexRune"></a>
Example:

    fmt.Println(strings.IndexRune("chicken", 'k'))
    fmt.Println(strings.IndexRune("chicken", 'd'))
    // Output:
    // 4
    // -1

<h2 id="Join">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L405">Join</a>
    <a href="#Join">¶</a></h2>
<pre>func Join(a []<a href="/builtin/#string">string</a>, sep <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Join concatenates the elements of a to create a single string. The separator
string sep is placed between elements in the resulting string.

<a id="exampleJoin"></a>
Example:

    s := []string{"foo", "bar", "baz"}
    fmt.Println(strings.Join(s, ", "))
    // Output: foo, bar, baz

<h2 id="LastIndex">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L98">LastIndex</a>
    <a href="#LastIndex">¶</a></h2>
<pre>func LastIndex(s, substr <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

LastIndex returns the index of the last instance of substr in s, or -1 if substr
is not present in s.

<a id="exampleLastIndex"></a>
Example:

    fmt.Println(strings.Index("go gopher", "go"))
    fmt.Println(strings.LastIndex("go gopher", "go"))
    fmt.Println(strings.LastIndex("go gopher", "rodent"))
    // Output:
    // 0
    // 3
    // -1

<h2 id="LastIndexAny">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L186">LastIndexAny</a>
    <a href="#LastIndexAny">¶</a></h2>
<pre>func LastIndexAny(s, chars <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

LastIndexAny returns the index of the last instance of any Unicode code point
from chars in s, or -1 if no Unicode code point from chars is present in s.

<a id="exampleLastIndexAny"></a>
Example:

    fmt.Println(strings.LastIndexAny("go gopher", "go"))
    fmt.Println(strings.LastIndexAny("go gopher", "rodent"))
    fmt.Println(strings.LastIndexAny("go gopher", "fail"))
    // Output:
    // 4
    // 8
    // -1

<h2 id="LastIndexByte">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L214">LastIndexByte</a>
    <a href="#LastIndexByte">¶</a></h2>
<pre>func LastIndexByte(s <a href="/builtin/#string">string</a>, c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

LastIndexByte returns the index of the last instance of c in s, or -1 if c is
not present in s.

<a id="exampleLastIndexByte"></a>
Example:

    fmt.Println(strings.LastIndexByte("Hello, world", 'l'))
    fmt.Println(strings.LastIndexByte("Hello, world", 'o'))
    fmt.Println(strings.LastIndexByte("Hello, world", 'x'))
    // Output:
    // 10
    // 8
    // -1

<h2 id="LastIndexFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L704">LastIndexFunc</a>
    <a href="#LastIndexFunc">¶</a></h2>
<pre>func LastIndexFunc(s <a href="/builtin/#string">string</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#int">int</a></pre>

LastIndexFunc returns the index into s of the last Unicode code point satisfying
f(c), or -1 if none do.

<a id="exampleLastIndexFunc"></a>
Example:

    fmt.Println(strings.LastIndexFunc("go 123", unicode.IsNumber))
    fmt.Println(strings.LastIndexFunc("123 go", unicode.IsNumber))
    fmt.Println(strings.LastIndexFunc("go", unicode.IsNumber))
    // Output:
    // 5
    // 2
    // -1

<h2 id="Map">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L447">Map</a>
    <a href="#Map">¶</a></h2>
<pre>func Map(mapping func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Map returns a copy of the string s with all its characters modified according to
the mapping function. If mapping returns a negative value, the character is
dropped from the string with no replacement.

<a id="exampleMap"></a>
Example:

    rot13 := func(r rune) rune {
        switch {
        case r >= 'A' && r <= 'Z':
            return 'A' + (r-'A'+13)%26
        case r >= 'a' && r <= 'z':
            return 'a' + (r-'a'+13)%26
        }
        return r
    }
    fmt.Println(strings.Map(rot13, "'Twas brillig and the slithy gopher..."))
    // Output: 'Gjnf oevyyvt naq gur fyvgul tbcure...

<h2 id="Repeat">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L522">Repeat</a>
    <a href="#Repeat">¶</a></h2>
<pre>func Repeat(s <a href="/builtin/#string">string</a>, count <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

Repeat returns a new string consisting of count copies of the string s.

It panics if count is negative or if the result of (len(s) * count) overflows.

<a id="exampleRepeat"></a>
Example:

    fmt.Println("ba" + strings.Repeat("na", 2))
    // Output: banana

<h2 id="Replace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L831">Replace</a>
    <a href="#Replace">¶</a></h2>
<pre>func Replace(s, old, new <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

Replace returns a copy of the string s with the first n non-overlapping
instances of old replaced by new. If old is empty, it matches at the beginning
of the string and after each UTF-8 sequence, yielding up to k+1 replacements for
a k-rune string. If n < 0, there is no limit on the number of replacements.

<a id="exampleReplace"></a>
Example:

    fmt.Println(strings.Replace("oink oink oink", "k", "ky", 2))
    fmt.Println(strings.Replace("oink oink oink", "oink", "moo", -1))
    // Output:
    // oinky oinky oink
    // moo moo moo

<h2 id="Split">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L288">Split</a>
    <a href="#Split">¶</a></h2>
<pre>func Split(s, sep <a href="/builtin/#string">string</a>) []<a href="/builtin/#string">string</a></pre>

Split slices s into all substrings separated by sep and returns a slice of the
substrings between those separators.

If s does not contain sep and sep is not empty, Split returns a slice of length
1 whose only element is s.

If sep is empty, Split splits after each UTF-8 sequence. If both s and sep are
empty, Split returns an empty slice.

It is equivalent to SplitN with a count of -1.

<a id="exampleSplit"></a>
Example:

    fmt.Printf("%q\n", strings.Split("a,b,c", ","))
    fmt.Printf("%q\n", strings.Split("a man a plan a canal panama", "a "))
    fmt.Printf("%q\n", strings.Split(" xyz ", ""))
    fmt.Printf("%q\n", strings.Split("", "Bernardo O'Higgins"))
    // Output:
    // ["a" "b" "c"]
    // ["" "man " "plan " "canal panama"]
    // [" " "x" "y" "z" " "]
    // [""]

<h2 id="SplitAfter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L300">SplitAfter</a>
    <a href="#SplitAfter">¶</a></h2>
<pre>func SplitAfter(s, sep <a href="/builtin/#string">string</a>) []<a href="/builtin/#string">string</a></pre>

SplitAfter slices s into all substrings after each instance of sep and returns a
slice of those substrings.

If s does not contain sep and sep is not empty, SplitAfter returns a slice of
length 1 whose only element is s.

If sep is empty, SplitAfter splits after each UTF-8 sequence. If both s and sep
are empty, SplitAfter returns an empty slice.

It is equivalent to SplitAfterN with a count of -1.

<a id="exampleSplitAfter"></a>
Example:

    fmt.Printf("%q\n", strings.SplitAfter("a,b,c", ","))
    // Output: ["a," "b," "c"]

<h2 id="SplitAfterN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L274">SplitAfterN</a>
    <a href="#SplitAfterN">¶</a></h2>
<pre>func SplitAfterN(s, sep <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) []<a href="/builtin/#string">string</a></pre>

SplitAfterN slices s into substrings after each instance of sep and returns a
slice of those substrings.

The count determines the number of substrings to return:

    n > 0: at most n substrings; the last substring will be the unsplit remainder.
    n == 0: the result is nil (zero substrings)
    n < 0: all substrings

Edge cases for s and sep (for example, empty strings) are handled as described
in the documentation for SplitAfter.

<a id="exampleSplitAfterN"></a>
Example:

    fmt.Printf("%q\n", strings.SplitAfterN("a,b,c", ",", 2))
    // Output: ["a," "b,c"]

<h2 id="SplitN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L262">SplitN</a>
    <a href="#SplitN">¶</a></h2>
<pre>func SplitN(s, sep <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) []<a href="/builtin/#string">string</a></pre>

SplitN slices s into substrings separated by sep and returns a slice of the
substrings between those separators.

The count determines the number of substrings to return:

    n > 0: at most n substrings; the last substring will be the unsplit remainder.
    n == 0: the result is nil (zero substrings)
    n < 0: all substrings

Edge cases for s and sep (for example, empty strings) are handled as described
in the documentation for Split.

<a id="exampleSplitN"></a>
Example:

    fmt.Printf("%q\n", strings.SplitN("a,b,c", ",", 2))
    z := strings.SplitN("a,b,c", ",", 0)
    fmt.Printf("%q (nil = %v)\n", z, z == nil)
    // Output:
    // ["a" "b,c"]
    // [] (nil = true)

<h2 id="Title">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L650">Title</a>
    <a href="#Title">¶</a></h2>
<pre>func Title(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Title returns a copy of the string s with all Unicode letters that begin words
mapped to their title case.

BUG(rsc): The rule Title uses for word boundaries does not handle Unicode
punctuation properly.

<a id="exampleTitle"></a>
Example:

    fmt.Println(strings.Title("her royal highness"))
    // Output: Her Royal Highness

<h2 id="ToLower">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L572">ToLower</a>
    <a href="#ToLower">¶</a></h2>
<pre>func ToLower(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToLower returns a copy of the string s with all Unicode letters mapped to their
lower case.

<a id="exampleToLower"></a>
Example:

    fmt.Println(strings.ToLower("Gopher"))
    // Output: gopher

<h2 id="ToLowerSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L611">ToLowerSpecial</a>
    <a href="#ToLowerSpecial">¶</a></h2>
<pre>func ToLowerSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToLowerSpecial returns a copy of the string s with all Unicode letters mapped to
their lower case, giving priority to the special casing rules.

<a id="exampleToLowerSpecial"></a>
Example:

    fmt.Println(strings.ToLowerSpecial(unicode.TurkishCase, "Önnek İş"))
    // Output: önnek iş

<h2 id="ToTitle">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L601">ToTitle</a>
    <a href="#ToTitle">¶</a></h2>
<pre>func ToTitle(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToTitle returns a copy of the string s with all Unicode letters mapped to their
title case.

<a id="exampleToTitle"></a>
Example:

    fmt.Println(strings.ToTitle("loud noises"))
    fmt.Println(strings.ToTitle("хлеб"))
    // Output:
    // LOUD NOISES
    // ХЛЕБ

<h2 id="ToTitleSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L617">ToTitleSpecial</a>
    <a href="#ToTitleSpecial">¶</a></h2>
<pre>func ToTitleSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToTitleSpecial returns a copy of the string s with all Unicode letters mapped to
their title case, giving priority to the special casing rules.

<a id="exampleToTitleSpecial"></a>
Example:

    fmt.Println(strings.ToTitleSpecial(unicode.TurkishCase, "dünyanın ilk borsa yapısı Aizonai kabul edilir"))
    // Output:
    // DÜNYANIN İLK BORSA YAPISI AİZONAİ KABUL EDİLİR

<h2 id="ToUpper">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L543">ToUpper</a>
    <a href="#ToUpper">¶</a></h2>
<pre>func ToUpper(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToUpper returns a copy of the string s with all Unicode letters mapped to their
upper case.

<a id="exampleToUpper"></a>
Example:

    fmt.Println(strings.ToUpper("Gopher"))
    // Output: GOPHER

<h2 id="ToUpperSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L605">ToUpperSpecial</a>
    <a href="#ToUpperSpecial">¶</a></h2>
<pre>func ToUpperSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToUpperSpecial returns a copy of the string s with all Unicode letters mapped to
their upper case, giving priority to the special casing rules.

<a id="exampleToUpperSpecial"></a>
Example:

    fmt.Println(strings.ToUpperSpecial(unicode.TurkishCase, "örnek iş"))
    // Output: ÖRNEK İŞ

<h2 id="Trim">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L776">Trim</a>
    <a href="#Trim">¶</a></h2>
<pre>func Trim(s <a href="/builtin/#string">string</a>, cutset <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Trim returns a slice of the string s with all leading and trailing Unicode code
points contained in cutset removed.

<a id="exampleTrim"></a>
Example:

    fmt.Print(strings.Trim("¡¡¡Hello, Gophers!!!", "!¡"))
    // Output: Hello, Gophers

<h2 id="TrimFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L692">TrimFunc</a>
    <a href="#TrimFunc">¶</a></h2>
<pre>func TrimFunc(s <a href="/builtin/#string">string</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#string">string</a></pre>

TrimFunc returns a slice of the string s with all leading and trailing Unicode
code points c satisfying f(c) removed.

<a id="exampleTrimFunc"></a>
Example:

    fmt.Print(strings.TrimFunc("¡¡¡Hello, Gophers!!!", func(r rune) bool {
        return !unicode.IsLetter(r) && !unicode.IsNumber(r)
    }))
    // Output: Hello, Gophers

<h2 id="TrimLeft">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L785">TrimLeft</a>
    <a href="#TrimLeft">¶</a></h2>
<pre>func TrimLeft(s <a href="/builtin/#string">string</a>, cutset <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimLeft returns a slice of the string s with all leading Unicode code points
contained in cutset removed.

<a id="exampleTrimLeft"></a>
Example:

    fmt.Print(strings.TrimLeft("¡¡¡Hello, Gophers!!!", "!¡"))
    // Output: Hello, Gophers!!!

<h2 id="TrimLeftFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L669">TrimLeftFunc</a>
    <a href="#TrimLeftFunc">¶</a></h2>
<pre>func TrimLeftFunc(s <a href="/builtin/#string">string</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#string">string</a></pre>

TrimLeftFunc returns a slice of the string s with all leading Unicode code
points c satisfying f(c) removed.

<a id="exampleTrimLeftFunc"></a>
Example:

    fmt.Print(strings.TrimLeftFunc("¡¡¡Hello, Gophers!!!", func(r rune) bool {
        return !unicode.IsLetter(r) && !unicode.IsNumber(r)
    }))
    // Output: Hello, Gophers!!!

<h2 id="TrimPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L809">TrimPrefix</a>
    <a href="#TrimPrefix">¶</a></h2>
<pre>func TrimPrefix(s, prefix <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimPrefix returns s without the provided leading prefix string. If s doesn't
start with prefix, s is returned unchanged.

<a id="exampleTrimPrefix"></a>
Example:

    var s = "¡¡¡Hello, Gophers!!!"
    s = strings.TrimPrefix(s, "¡¡¡Hello, ")
    s = strings.TrimPrefix(s, "¡¡¡Howdy, ")
    fmt.Print(s)
    // Output: Gophers!!!

<h2 id="TrimRight">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L794">TrimRight</a>
    <a href="#TrimRight">¶</a></h2>
<pre>func TrimRight(s <a href="/builtin/#string">string</a>, cutset <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimRight returns a slice of the string s, with all trailing Unicode code points
contained in cutset removed.

<a id="exampleTrimRight"></a>
Example:

    fmt.Print(strings.TrimRight("¡¡¡Hello, Gophers!!!", "!¡"))
    // Output: ¡¡¡Hello, Gophers

<h2 id="TrimRightFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L679">TrimRightFunc</a>
    <a href="#TrimRightFunc">¶</a></h2>
<pre>func TrimRightFunc(s <a href="/builtin/#string">string</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#string">string</a></pre>

TrimRightFunc returns a slice of the string s with all trailing Unicode code
points c satisfying f(c) removed.

<a id="exampleTrimRightFunc"></a>
Example:

    fmt.Print(strings.TrimRightFunc("¡¡¡Hello, Gophers!!!", func(r rune) bool {
        return !unicode.IsLetter(r) && !unicode.IsNumber(r)
    }))
    // Output: ¡¡¡Hello, Gophers

<h2 id="TrimSpace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L803">TrimSpace</a>
    <a href="#TrimSpace">¶</a></h2>
<pre>func TrimSpace(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimSpace returns a slice of the string s, with all leading and trailing white
space removed, as defined by Unicode.

<a id="exampleTrimSpace"></a>
Example:

    fmt.Println(strings.TrimSpace(" \t\n Hello, Gophers \n\t\r\n"))
    // Output: Hello, Gophers

<h2 id="TrimSuffix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L818">TrimSuffix</a>
    <a href="#TrimSuffix">¶</a></h2>
<pre>func TrimSuffix(s, suffix <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimSuffix returns s without the provided trailing suffix string. If s doesn't
end with suffix, s is returned unchanged.

<a id="exampleTrimSuffix"></a>
Example:

    var s = "¡¡¡Hello, Gophers!!!"
    s = strings.TrimSuffix(s, ", Gophers!!!")
    s = strings.TrimSuffix(s, ", Marmots!!!")
    fmt.Print(s)
    // Output: ¡¡¡Hello

<h2 id="Builder">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L5">Builder</a>
    <a href="#Builder">¶</a></h2>
<pre>type Builder struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Builder is used to efficiently build a string using Write methods. It
minimizes memory copying. The zero value is ready to use. Do not copy a non-zero
Builder.

<a id="exampleBuilder"></a>
Example:

    var b strings.Builder
    for i := 3; i >= 1; i-- {
        fmt.Fprintf(&b, "%d...", i)
    }
    b.WriteString("ignition")
    fmt.Println(b.String())

    // Output: 3...2...1...ignition

<h3 id="Builder.Grow">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L60">Grow</a>
    <a href="#Builder.Grow">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) Grow(n <a href="/builtin/#int">int</a>)</pre>

Grow grows b's capacity, if necessary, to guarantee space for another n bytes.
After Grow(n), at least n bytes can be written to b without another allocation.
If n is negative, Grow panics.

<h3 id="Builder.Len">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L41">Len</a>
    <a href="#Builder.Len">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) Len() <a href="/builtin/#int">int</a></pre>

Len returns the number of accumulated bytes; b.Len() == len(b.String()).

<h3 id="Builder.Reset">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L44">Reset</a>
    <a href="#Builder.Reset">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) Reset()</pre>

Reset resets the Builder to be empty.

<h3 id="Builder.String">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L36">String</a>
    <a href="#Builder.String">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) String() <a href="/builtin/#string">string</a></pre>

String returns the accumulated string.

<h3 id="Builder.Write">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L72">Write</a>
    <a href="#Builder.Write">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) Write(p []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write appends the contents of p to b's buffer. Write always returns len(p), nil.

<h3 id="Builder.WriteByte">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L80">WriteByte</a>
    <a href="#Builder.WriteByte">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) WriteByte(c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

WriteByte appends the byte c to b's buffer. The returned error is always nil.

<h3 id="Builder.WriteRune">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L88">WriteRune</a>
    <a href="#Builder.WriteRune">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) WriteRune(r <a href="/builtin/#rune">rune</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteRune appends the UTF-8 encoding of Unicode code point r to b's buffer. It
returns the length of r and a nil error.

<h3 id="Builder.WriteString">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L105">WriteString</a>
    <a href="#Builder.WriteString">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) WriteString(s <a href="/builtin/#string">string</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteString appends the contents of s to b's buffer. It returns the length of s
and a nil error.

<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L6">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Reader implements the io.Reader, io.ReaderAt, io.Seeker, io.WriterTo,
io.ByteScanner, and io.RuneScanner interfaces by reading from a string.

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L140">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(s <a href="/builtin/#string">string</a>) *<a href="#Reader">Reader</a></pre>

NewReader returns a new Reader reading from s. It is similar to
bytes.NewBufferString but more efficient and read-only.

<h3 id="Reader.Len">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L14">Len</a>
    <a href="#Reader.Len">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Len() <a href="/builtin/#int">int</a></pre>

Len returns the number of bytes of the unread portion of the string.

<h3 id="Reader.Read">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L27">Read</a>
    <a href="#Reader.Read">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Read(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="Reader.ReadAt">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L37">ReadAt</a>
    <a href="#Reader.ReadAt">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadAt(b []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="Reader.ReadByte">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L52">ReadByte</a>
    <a href="#Reader.ReadByte">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadByte() (<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>


<h3 id="Reader.ReadRune">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L71">ReadRune</a>
    <a href="#Reader.ReadRune">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadRune() (ch <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="Reader.Reset">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L136">Reset</a>
    <a href="#Reader.Reset">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Reset(s <a href="/builtin/#string">string</a>)</pre>

Reset resets the Reader to be reading from s.

<h3 id="Reader.Seek">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L96">Seek</a>
    <a href="#Reader.Seek">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Seek(offset <a href="/builtin/#int64">int64</a>, whence <a href="/builtin/#int">int</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>

Seek implements the io.Seeker interface.

<h3 id="Reader.Size">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L25">Size</a>
    <a href="#Reader.Size">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Size() <a href="/builtin/#int64">int64</a></pre>

Size returns the original length of the underlying string. Size is the number of
bytes available for reading via ReadAt. The returned value is always the same
and is not affected by calls to any other method.

<h3 id="Reader.UnreadByte">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L62">UnreadByte</a>
    <a href="#Reader.UnreadByte">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) UnreadByte() <a href="/builtin/#error">error</a></pre>


<h3 id="Reader.UnreadRune">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L86">UnreadRune</a>
    <a href="#Reader.UnreadRune">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) UnreadRune() <a href="/builtin/#error">error</a></pre>


<h3 id="Reader.WriteTo">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L117">WriteTo</a>
    <a href="#Reader.WriteTo">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteTo implements the io.WriterTo interface.

<h2 id="Replacer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/replace.go#L1">Replacer</a>
    <a href="#Replacer">¶</a></h2>
<pre>type Replacer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Replacer replaces a list of strings with replacements. It is safe for concurrent
use by multiple goroutines.

<h3 id="NewReplacer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/replace.go#L13">NewReplacer</a>
    <a href="#NewReplacer">¶</a></h3>
<pre>func NewReplacer(oldnew ...<a href="/builtin/#string">string</a>) *<a href="#Replacer">Replacer</a></pre>

NewReplacer returns a new Replacer from a list of old, new string pairs.
Replacements are performed in order, without overlapping matches.

<a id="exampleNewReplacer"></a>
Example:

    r := strings.NewReplacer("<", "&lt;", ">", "&gt;")
    fmt.Println(r.Replace("This is <b>HTML</b>!"))
    // Output: This is &lt;b&gt;HTML&lt;/b&gt;!

<h3 id="Replacer.Replace">func (*Replacer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/replace.go#L59">Replace</a>
    <a href="#Replacer.Replace">¶</a></h3>
<pre>func (r *<a href="#Replacer">Replacer</a>) Replace(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Replace returns a copy of s with all replacements performed.

<h3 id="Replacer.WriteString">func (*Replacer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/replace.go#L64">WriteString</a>
    <a href="#Replacer.WriteString">¶</a></h3>
<pre>func (r *<a href="#Replacer">Replacer</a>) WriteString(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, s <a href="/builtin/#string">string</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteString writes s to w with all replacements performed.

<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L649)  The rule Title uses for word boundaries does not handle Unicode punctuation
  properly.


