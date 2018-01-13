version: 1.9.2
## package bytes

  `import "bytes"`

## Overview

Package bytes implements functions for the manipulation of byte slices. It is
analogous to the facilities of the strings package.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func Compare(a, b []byte) int](#Compare)
- [func Contains(b, subslice []byte) bool](#Contains)
- [func ContainsAny(b []byte, chars string) bool](#ContainsAny)
- [func ContainsRune(b []byte, r rune) bool](#ContainsRune)
- [func Count(s, sep []byte) int](#Count)
- [func Equal(a, b []byte) bool](#Equal)
- [func EqualFold(s, t []byte) bool](#EqualFold)
- [func Fields(s []byte) [][]byte](#Fields)
- [func FieldsFunc(s []byte, f func(rune) bool) [][]byte](#FieldsFunc)
- [func HasPrefix(s, prefix []byte) bool](#HasPrefix)
- [func HasSuffix(s, suffix []byte) bool](#HasSuffix)
- [func Index(s, sep []byte) int](#Index)
- [func IndexAny(s []byte, chars string) int](#IndexAny)
- [func IndexByte(s []byte, c byte) int](#IndexByte)
- [func IndexFunc(s []byte, f func(r rune) bool) int](#IndexFunc)
- [func IndexRune(s []byte, r rune) int](#IndexRune)
- [func Join(s [][]byte, sep []byte) []byte](#Join)
- [func LastIndex(s, sep []byte) int](#LastIndex)
- [func LastIndexAny(s []byte, chars string) int](#LastIndexAny)
- [func LastIndexByte(s []byte, c byte) int](#LastIndexByte)
- [func LastIndexFunc(s []byte, f func(r rune) bool) int](#LastIndexFunc)
- [func Map(mapping func(r rune) rune, s []byte) []byte](#Map)
- [func Repeat(b []byte, count int) []byte](#Repeat)
- [func Replace(s, old, new []byte, n int) []byte](#Replace)
- [func Runes(s []byte) []rune](#Runes)
- [func Split(s, sep []byte) [][]byte](#Split)
- [func SplitAfter(s, sep []byte) [][]byte](#SplitAfter)
- [func SplitAfterN(s, sep []byte, n int) [][]byte](#SplitAfterN)
- [func SplitN(s, sep []byte, n int) [][]byte](#SplitN)
- [func Title(s []byte) []byte](#Title)
- [func ToLower(s []byte) []byte](#ToLower)
- [func ToLowerSpecial(c unicode.SpecialCase, s []byte) []byte](#ToLowerSpecial)
- [func ToTitle(s []byte) []byte](#ToTitle)
- [func ToTitleSpecial(c unicode.SpecialCase, s []byte) []byte](#ToTitleSpecial)
- [func ToUpper(s []byte) []byte](#ToUpper)
- [func ToUpperSpecial(c unicode.SpecialCase, s []byte) []byte](#ToUpperSpecial)
- [func Trim(s []byte, cutset string) []byte](#Trim)
- [func TrimFunc(s []byte, f func(r rune) bool) []byte](#TrimFunc)
- [func TrimLeft(s []byte, cutset string) []byte](#TrimLeft)
- [func TrimLeftFunc(s []byte, f func(r rune) bool) []byte](#TrimLeftFunc)
- [func TrimPrefix(s, prefix []byte) []byte](#TrimPrefix)
- [func TrimRight(s []byte, cutset string) []byte](#TrimRight)
- [func TrimRightFunc(s []byte, f func(r rune) bool) []byte](#TrimRightFunc)
- [func TrimSpace(s []byte) []byte](#TrimSpace)
- [func TrimSuffix(s, suffix []byte) []byte](#TrimSuffix)
- [type Buffer](#Buffer)
  - [func NewBuffer(buf []byte) *Buffer](#NewBuffer)
  - [func NewBufferString(s string) *Buffer](#NewBufferString)
  - [func (b *Buffer) Bytes() []byte](#Buffer.Bytes)
  - [func (b *Buffer) Cap() int](#Buffer.Cap)
  - [func (b *Buffer) Grow(n int)](#Buffer.Grow)
  - [func (b *Buffer) Len() int](#Buffer.Len)
  - [func (b *Buffer) Next(n int) []byte](#Buffer.Next)
  - [func (b *Buffer) Read(p []byte) (n int, err error)](#Buffer.Read)
  - [func (b *Buffer) ReadByte() (byte, error)](#Buffer.ReadByte)
  - [func (b *Buffer) ReadBytes(delim byte) (line []byte, err error)](#Buffer.ReadBytes)
  - [func (b *Buffer) ReadFrom(r io.Reader) (n int64, err error)](#Buffer.ReadFrom)
  - [func (b *Buffer) ReadRune() (r rune, size int, err error)](#Buffer.ReadRune)
  - [func (b *Buffer) ReadString(delim byte) (line string, err error)](#Buffer.ReadString)
  - [func (b *Buffer) Reset()](#Buffer.Reset)
  - [func (b *Buffer) String() string](#Buffer.String)
  - [func (b *Buffer) Truncate(n int)](#Buffer.Truncate)
  - [func (b *Buffer) UnreadByte() error](#Buffer.UnreadByte)
  - [func (b *Buffer) UnreadRune() error](#Buffer.UnreadRune)
  - [func (b *Buffer) Write(p []byte) (n int, err error)](#Buffer.Write)
  - [func (b *Buffer) WriteByte(c byte) error](#Buffer.WriteByte)
  - [func (b *Buffer) WriteRune(r rune) (n int, err error)](#Buffer.WriteRune)
  - [func (b *Buffer) WriteString(s string) (n int, err error)](#Buffer.WriteString)
  - [func (b *Buffer) WriteTo(w io.Writer) (n int64, err error)](#Buffer.WriteTo)
- [type Reader](#Reader)
  - [func NewReader(b []byte) *Reader](#NewReader)
  - [func (r *Reader) Len() int](#Reader.Len)
  - [func (r *Reader) Read(b []byte) (n int, err error)](#Reader.Read)
  - [func (r *Reader) ReadAt(b []byte, off int64) (n int, err error)](#Reader.ReadAt)
  - [func (r *Reader) ReadByte() (byte, error)](#Reader.ReadByte)
  - [func (r *Reader) ReadRune() (ch rune, size int, err error)](#Reader.ReadRune)
  - [func (r *Reader) Reset(b []byte)](#Reader.Reset)
  - [func (r *Reader) Seek(offset int64, whence int) (int64, error)](#Reader.Seek)
  - [func (r *Reader) Size() int64](#Reader.Size)
  - [func (r *Reader) UnreadByte() error](#Reader.UnreadByte)
  - [func (r *Reader) UnreadRune() error](#Reader.UnreadRune)
  - [func (r *Reader) WriteTo(w io.Writer) (n int64, err error)](#Reader.WriteTo)
- [Bugs](#pkg-note-BUG)

### Examples

- [Buffer](#exampleBuffer)
- [Buffer.Grow](#exampleBuffer_Grow)
- [Buffer (Reader)](#exampleBuffer_reader)
- [Compare](#exampleCompare)
- [Compare (Search)](#exampleCompare_search)
- [Contains](#exampleContains)
- [Count](#exampleCount)
- [EqualFold](#exampleEqualFold)
- [Fields](#exampleFields)
- [FieldsFunc](#exampleFieldsFunc)
- [HasPrefix](#exampleHasPrefix)
- [HasSuffix](#exampleHasSuffix)
- [Index](#exampleIndex)
- [IndexAny](#exampleIndexAny)
- [IndexFunc](#exampleIndexFunc)
- [IndexRune](#exampleIndexRune)
- [Join](#exampleJoin)
- [LastIndex](#exampleLastIndex)
- [Map](#exampleMap)
- [Repeat](#exampleRepeat)
- [Replace](#exampleReplace)
- [Split](#exampleSplit)
- [SplitAfter](#exampleSplitAfter)
- [SplitAfterN](#exampleSplitAfterN)
- [SplitN](#exampleSplitN)
- [Title](#exampleTitle)
- [ToLower](#exampleToLower)
- [ToTitle](#exampleToTitle)
- [ToUpper](#exampleToUpper)
- [Trim](#exampleTrim)
- [TrimPrefix](#exampleTrimPrefix)
- [TrimSpace](#exampleTrimSpace)
- [TrimSuffix](#exampleTrimSuffix)

### Package files
 [buffer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go) [bytes.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go) [bytes_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_amd64.go) [bytes_decl.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_decl.go) [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="MinRead">MinRead</span> = 512</pre>

MinRead is the minimum slice size passed to a Read call by Buffer.ReadFrom. As
long as the Buffer has at least MinRead bytes beyond what is required to hold
the contents of r, ReadFrom will not grow the underlying buffer.

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrTooLarge">ErrTooLarge</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;bytes.Buffer: too large&#34;)</pre>

ErrTooLarge is passed to panic if memory cannot be allocated to store data in a
buffer.

<h2 id="Compare">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_decl.go#L14">Compare</a>
    <a href="#Compare">¶</a></h2>
<pre>func Compare(a, b []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

Compare returns an integer comparing two byte slices lexicographically. The
result will be 0 if a==b, -1 if a < b, and +1 if a > b. A nil argument is
equivalent to an empty slice.

<a id="exampleCompare"></a>
Example:

    // Interpret Compare's result by comparing it to zero.
    var a, b []byte
    if bytes.Compare(a, b) < 0 {
        // a less b
    }
    if bytes.Compare(a, b) <= 0 {
        // a less or equal b
    }
    if bytes.Compare(a, b) > 0 {
        // a greater b
    }
    if bytes.Compare(a, b) >= 0 {
        // a greater or equal b
    }

    // Prefer Equal to Compare for equality comparisons.
    if bytes.Equal(a, b) {
        // a equal b
    }
    if !bytes.Equal(a, b) {
        // a not equal b
    }


<a id="exampleCompare_search"></a>
Example:

    // Binary search to find a matching byte slice.
    var needle []byte
    var haystack [][]byte // Assume sorted
    i := sort.Search(len(haystack), func(i int) bool {
        // Return haystack[i] >= needle.
        return bytes.Compare(haystack[i], needle) >= 0
    })
    if i < len(haystack) && bytes.Equal(haystack[i], needle) {
        // Found it!
    }

<h2 id="Contains">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L57">Contains</a>
    <a href="#Contains">¶</a></h2>
<pre>func Contains(b, subslice []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

Contains reports whether subslice is within b.

<a id="exampleContains"></a>
Example:

    fmt.Println(bytes.Contains([]byte("seafood"), []byte("foo")))
    fmt.Println(bytes.Contains([]byte("seafood"), []byte("bar")))
    fmt.Println(bytes.Contains([]byte("seafood"), []byte("")))
    fmt.Println(bytes.Contains([]byte(""), []byte("")))
    // Output:
    // true
    // false
    // true
    // true

<h2 id="ContainsAny">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L62">ContainsAny</a>
    <a href="#ContainsAny">¶</a></h2>
<pre>func ContainsAny(b []<a href="/builtin/#byte">byte</a>, chars <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

ContainsAny reports whether any of the UTF-8-encoded Unicode code points in
chars are within b.

<h2 id="ContainsRune">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L67">ContainsRune</a>
    <a href="#ContainsRune">¶</a></h2>
<pre>func ContainsRune(b []<a href="/builtin/#byte">byte</a>, r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

ContainsRune reports whether the Unicode code point r is within b.

<h2 id="Count">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_amd64.go#L91">Count</a>
    <a href="#Count">¶</a></h2>
<pre>func Count(s, sep []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

Count counts the number of non-overlapping instances of sep in s. If sep is an
empty slice, Count returns 1 + the number of Unicode code points in s.

<a id="exampleCount"></a>
Example:

    fmt.Println(bytes.Count([]byte("cheese"), []byte("e")))
    fmt.Println(bytes.Count([]byte("five"), []byte(""))) // before & after each rune
    // Output:
    // 3
    // 5

<h2 id="Equal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_decl.go#L7">Equal</a>
    <a href="#Equal">¶</a></h2>
<pre>func Equal(a, b []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

Equal returns a boolean reporting whether a and b are the same length and
contain the same bytes. A nil argument is equivalent to an empty slice.

<h2 id="EqualFold">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L699">EqualFold</a>
    <a href="#EqualFold">¶</a></h2>
<pre>func EqualFold(s, t []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

EqualFold reports whether s and t, interpreted as UTF-8 strings, are equal under
Unicode case-folding.

<a id="exampleEqualFold"></a>
Example:

    fmt.Println(bytes.EqualFold([]byte("Go"), []byte("go")))
    // Output: true

<h2 id="Fields">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L260">Fields</a>
    <a href="#Fields">¶</a></h2>
<pre>func Fields(s []<a href="/builtin/#byte">byte</a>) [][]<a href="/builtin/#byte">byte</a></pre>

Fields splits the slice s around each instance of one or more consecutive white
space characters, returning a slice of subslices of s or an empty list if s
contains only white space.

<a id="exampleFields"></a>
Example:

    fmt.Printf("Fields are: %q", bytes.Fields([]byte("  foo bar  baz   ")))
    // Output: Fields are: ["foo" "bar" "baz"]

<h2 id="FieldsFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L270">FieldsFunc</a>
    <a href="#FieldsFunc">¶</a></h2>
<pre>func FieldsFunc(s []<a href="/builtin/#byte">byte</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) [][]<a href="/builtin/#byte">byte</a></pre>

FieldsFunc interprets s as a sequence of UTF-8-encoded Unicode code points. It
splits the slice s at each run of code points c satisfying f(c) and returns a
slice of subslices of s. If all code points in s satisfy f(c), or len(s) == 0,
an empty slice is returned. FieldsFunc makes no guarantees about the order in
which it calls f(c). If f does not return consistent results for a given c,
FieldsFunc may crash.

<a id="exampleFieldsFunc"></a>
Example:

    f := func(c rune) bool {
        return !unicode.IsLetter(c) && !unicode.IsNumber(c)
    }
    fmt.Printf("Fields are: %q", bytes.FieldsFunc([]byte("  foo1;bar2,baz3..."), f))
    // Output: Fields are: ["foo1" "bar2" "baz3"]

<h2 id="HasPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L331">HasPrefix</a>
    <a href="#HasPrefix">¶</a></h2>
<pre>func HasPrefix(s, prefix []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

HasPrefix tests whether the byte slice s begins with prefix.

<a id="exampleHasPrefix"></a>
Example:

    fmt.Println(bytes.HasPrefix([]byte("Gopher"), []byte("Go")))
    fmt.Println(bytes.HasPrefix([]byte("Gopher"), []byte("C")))
    fmt.Println(bytes.HasPrefix([]byte("Gopher"), []byte("")))
    // Output:
    // true
    // false
    // true

<h2 id="HasSuffix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L336">HasSuffix</a>
    <a href="#HasSuffix">¶</a></h2>
<pre>func HasSuffix(s, suffix []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

HasSuffix tests whether the byte slice s ends with suffix.

<a id="exampleHasSuffix"></a>
Example:

    fmt.Println(bytes.HasSuffix([]byte("Amigo"), []byte("go")))
    fmt.Println(bytes.HasSuffix([]byte("Amigo"), []byte("O")))
    fmt.Println(bytes.HasSuffix([]byte("Amigo"), []byte("Ami")))
    fmt.Println(bytes.HasSuffix([]byte("Amigo"), []byte("")))
    // Output:
    // true
    // false
    // false
    // true

<h2 id="Index">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_amd64.go#L17">Index</a>
    <a href="#Index">¶</a></h2>
<pre>func Index(s, sep []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

Index returns the index of the first instance of sep in s, or -1 if sep is not
present in s.

<a id="exampleIndex"></a>
Example:

    fmt.Println(bytes.Index([]byte("chicken"), []byte("ken")))
    fmt.Println(bytes.Index([]byte("chicken"), []byte("dmr")))
    // Output:
    // 4
    // -1

<h2 id="IndexAny">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L136">IndexAny</a>
    <a href="#IndexAny">¶</a></h2>
<pre>func IndexAny(s []<a href="/builtin/#byte">byte</a>, chars <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

IndexAny interprets s as a sequence of UTF-8-encoded Unicode code points. It
returns the byte index of the first occurrence in s of any of the Unicode code
points in chars. It returns -1 if chars is empty or if there is no code point in
common.

<a id="exampleIndexAny"></a>
Example:

    fmt.Println(bytes.IndexAny([]byte("chicken"), "aeiouy"))
    fmt.Println(bytes.IndexAny([]byte("crwth"), "aeiouy"))
    // Output:
    // 2
    // -1

<h2 id="IndexByte">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_decl.go#L1">IndexByte</a>
    <a href="#IndexByte">¶</a></h2>
<pre>func IndexByte(s []<a href="/builtin/#byte">byte</a>, c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

IndexByte returns the index of the first instance of c in s, or -1 if c is not
present in s.

<h2 id="IndexFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L524">IndexFunc</a>
    <a href="#IndexFunc">¶</a></h2>
<pre>func IndexFunc(s []<a href="/builtin/#byte">byte</a>, f func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#int">int</a></pre>

IndexFunc interprets s as a sequence of UTF-8-encoded Unicode code points. It
returns the byte index in s of the first Unicode code point satisfying f(c), or
-1 if none do.

<a id="exampleIndexFunc"></a>
Example:

    f := func(c rune) bool {
        return unicode.Is(unicode.Han, c)
    }
    fmt.Println(bytes.IndexFunc([]byte("Hello, 世界"), f))
    fmt.Println(bytes.IndexFunc([]byte("Hello, world"), f))
    // Output:
    // 7
    // -1

<h2 id="IndexRune">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L110">IndexRune</a>
    <a href="#IndexRune">¶</a></h2>
<pre>func IndexRune(s []<a href="/builtin/#byte">byte</a>, r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#int">int</a></pre>

IndexRune interprets s as a sequence of UTF-8-encoded Unicode code points. It
returns the byte index of the first occurrence in s of the given rune. It
returns -1 if rune is not present in s. If r is utf8.RuneError, it returns the
first instance of any invalid UTF-8 byte sequence.

<a id="exampleIndexRune"></a>
Example:

    fmt.Println(bytes.IndexRune([]byte("chicken"), 'k'))
    fmt.Println(bytes.IndexRune([]byte("chicken"), 'd'))
    // Output:
    // 4
    // -1

<h2 id="Join">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L308">Join</a>
    <a href="#Join">¶</a></h2>
<pre>func Join(s [][]<a href="/builtin/#byte">byte</a>, sep []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

Join concatenates the elements of s to create a new byte slice. The separator
sep is placed between elements in the resulting slice.

<a id="exampleJoin"></a>
Example:

    s := [][]byte{[]byte("foo"), []byte("bar"), []byte("baz")}
    fmt.Printf("%s", bytes.Join(s, []byte(", ")))
    // Output: foo, bar, baz

<h2 id="LastIndex">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L81">LastIndex</a>
    <a href="#LastIndex">¶</a></h2>
<pre>func LastIndex(s, sep []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

LastIndex returns the index of the last instance of sep in s, or -1 if sep is
not present in s.

<a id="exampleLastIndex"></a>
Example:

    fmt.Println(bytes.Index([]byte("go gopher"), []byte("go")))
    fmt.Println(bytes.LastIndex([]byte("go gopher"), []byte("go")))
    fmt.Println(bytes.LastIndex([]byte("go gopher"), []byte("rodent")))
    // Output:
    // 0
    // 3
    // -1

<h2 id="LastIndexAny">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L170">LastIndexAny</a>
    <a href="#LastIndexAny">¶</a></h2>
<pre>func LastIndexAny(s []<a href="/builtin/#byte">byte</a>, chars <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

LastIndexAny interprets s as a sequence of UTF-8-encoded Unicode code points. It
returns the byte index of the last occurrence in s of any of the Unicode code
points in chars. It returns -1 if chars is empty or if there is no code point in
common.

<h2 id="LastIndexByte">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L96">LastIndexByte</a>
    <a href="#LastIndexByte">¶</a></h2>
<pre>func LastIndexByte(s []<a href="/builtin/#byte">byte</a>, c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

LastIndexByte returns the index of the last instance of c in s, or -1 if c is
not present in s.

<h2 id="LastIndexFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L531">LastIndexFunc</a>
    <a href="#LastIndexFunc">¶</a></h2>
<pre>func LastIndexFunc(s []<a href="/builtin/#byte">byte</a>, f func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#int">int</a></pre>

LastIndexFunc interprets s as a sequence of UTF-8-encoded Unicode code points.
It returns the byte index in s of the last Unicode code point satisfying f(c),
or -1 if none do.

<h2 id="Map">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L344">Map</a>
    <a href="#Map">¶</a></h2>
<pre>func Map(mapping func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a>, s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

Map returns a copy of the byte slice s with all its characters modified
according to the mapping function. If mapping returns a negative value, the
character is dropped from the string with no replacement. The characters in s
and the output are interpreted as UTF-8-encoded Unicode code points.

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
    fmt.Printf("%s", bytes.Map(rot13, []byte("'Twas brillig and the slithy gopher...")))
    // Output: 'Gjnf oevyyvt naq gur fyvgul tbcure...

<h2 id="Repeat">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L381">Repeat</a>
    <a href="#Repeat">¶</a></h2>
<pre>func Repeat(b []<a href="/builtin/#byte">byte</a>, count <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

Repeat returns a new byte slice consisting of count copies of b.

It panics if count is negative or if the result of (len(b) * count) overflows.

<a id="exampleRepeat"></a>
Example:

    fmt.Printf("ba%s", bytes.Repeat([]byte("na"), 2))
    // Output: banana

<h2 id="Replace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L661">Replace</a>
    <a href="#Replace">¶</a></h2>
<pre>func Replace(s, old, new []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

Replace returns a copy of the slice s with the first n non-overlapping instances
of old replaced by new. If old is empty, it matches at the beginning of the
slice and after each UTF-8 sequence, yielding up to k+1 replacements for a
k-rune slice. If n < 0, there is no limit on the number of replacements.

<a id="exampleReplace"></a>
Example:

    fmt.Printf("%s\n", bytes.Replace([]byte("oink oink oink"), []byte("k"), []byte("ky"), 2))
    fmt.Printf("%s\n", bytes.Replace([]byte("oink oink oink"), []byte("oink"), []byte("moo"), -1))
    // Output:
    // oinky oinky oink
    // moo moo moo

<h2 id="Runes">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L643">Runes</a>
    <a href="#Runes">¶</a></h2>
<pre>func Runes(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#rune">rune</a></pre>

Runes returns a slice of runes (Unicode code points) equivalent to s.

<h2 id="Split">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L248">Split</a>
    <a href="#Split">¶</a></h2>
<pre>func Split(s, sep []<a href="/builtin/#byte">byte</a>) [][]<a href="/builtin/#byte">byte</a></pre>

Split slices s into all subslices separated by sep and returns a slice of the
subslices between those separators. If sep is empty, Split splits after each
UTF-8 sequence. It is equivalent to SplitN with a count of -1.

<a id="exampleSplit"></a>
Example:

    fmt.Printf("%q\n", bytes.Split([]byte("a,b,c"), []byte(",")))
    fmt.Printf("%q\n", bytes.Split([]byte("a man a plan a canal panama"), []byte("a ")))
    fmt.Printf("%q\n", bytes.Split([]byte(" xyz "), []byte("")))
    fmt.Printf("%q\n", bytes.Split([]byte(""), []byte("Bernardo O'Higgins")))
    // Output:
    // ["a" "b" "c"]
    // ["" "man " "plan " "canal panama"]
    // [" " "x" "y" "z" " "]
    // [""]

<h2 id="SplitAfter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L254">SplitAfter</a>
    <a href="#SplitAfter">¶</a></h2>
<pre>func SplitAfter(s, sep []<a href="/builtin/#byte">byte</a>) [][]<a href="/builtin/#byte">byte</a></pre>

SplitAfter slices s into all subslices after each instance of sep and returns a
slice of those subslices. If sep is empty, SplitAfter splits after each UTF-8
sequence. It is equivalent to SplitAfterN with a count of -1.

<a id="exampleSplitAfter"></a>
Example:

    fmt.Printf("%q\n", bytes.SplitAfter([]byte("a,b,c"), []byte(",")))
    // Output: ["a," "b," "c"]

<h2 id="SplitAfterN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L240">SplitAfterN</a>
    <a href="#SplitAfterN">¶</a></h2>
<pre>func SplitAfterN(s, sep []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) [][]<a href="/builtin/#byte">byte</a></pre>

SplitAfterN slices s into subslices after each instance of sep and returns a
slice of those subslices. If sep is empty, SplitAfterN splits after each UTF-8
sequence. The count determines the number of subslices to return:

    n > 0: at most n subslices; the last subslice will be the unsplit remainder.
    n == 0: the result is nil (zero subslices)
    n < 0: all subslices

<a id="exampleSplitAfterN"></a>
Example:

    fmt.Printf("%q\n", bytes.SplitAfterN([]byte("a,b,c"), []byte(","), 2))
    // Output: ["a," "b,c"]

<h2 id="SplitN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L231">SplitN</a>
    <a href="#SplitN">¶</a></h2>
<pre>func SplitN(s, sep []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) [][]<a href="/builtin/#byte">byte</a></pre>

SplitN slices s into subslices separated by sep and returns a slice of the
subslices between those separators. If sep is empty, SplitN splits after each
UTF-8 sequence. The count determines the number of subslices to return:

    n > 0: at most n subslices; the last subslice will be the unsplit remainder.
    n == 0: the result is nil (zero subslices)
    n < 0: all subslices

<a id="exampleSplitN"></a>
Example:

    fmt.Printf("%q\n", bytes.SplitN([]byte("a,b,c"), []byte(","), 2))
    z := bytes.SplitN([]byte("a,b,c"), []byte(","), 0)
    fmt.Printf("%q (nil = %v)\n", z, z == nil)
    // Output:
    // ["a" "b,c"]
    // [] (nil = true)

<h2 id="Title">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L457">Title</a>
    <a href="#Title">¶</a></h2>
<pre>func Title(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

Title returns a copy of s with all Unicode letters that begin words mapped to
their title case.

BUG(rsc): The rule Title uses for word boundaries does not handle Unicode
punctuation properly.

<a id="exampleTitle"></a>
Example:

    fmt.Printf("%s", bytes.Title([]byte("her royal highness")))
    // Output: Her Royal Highness

<h2 id="ToLower">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L405">ToLower</a>
    <a href="#ToLower">¶</a></h2>
<pre>func ToLower(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToLower returns a copy of the byte slice s with all Unicode letters mapped to
their lower case.

<a id="exampleToLower"></a>
Example:

    fmt.Printf("%s", bytes.ToLower([]byte("Gopher")))
    // Output: gopher

<h2 id="ToLowerSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L418">ToLowerSpecial</a>
    <a href="#ToLowerSpecial">¶</a></h2>
<pre>func ToLowerSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToLowerSpecial returns a copy of the byte slice s with all Unicode letters
mapped to their lower case, giving priority to the special casing rules.

<h2 id="ToTitle">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L408">ToTitle</a>
    <a href="#ToTitle">¶</a></h2>
<pre>func ToTitle(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToTitle returns a copy of the byte slice s with all Unicode letters mapped to
their title case.

<a id="exampleToTitle"></a>
Example:

    fmt.Printf("%s\n", bytes.ToTitle([]byte("loud noises")))
    fmt.Printf("%s\n", bytes.ToTitle([]byte("хлеб")))
    // Output:
    // LOUD NOISES
    // ХЛЕБ

<h2 id="ToTitleSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L424">ToTitleSpecial</a>
    <a href="#ToTitleSpecial">¶</a></h2>
<pre>func ToTitleSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToTitleSpecial returns a copy of the byte slice s with all Unicode letters
mapped to their title case, giving priority to the special casing rules.

<h2 id="ToUpper">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L402">ToUpper</a>
    <a href="#ToUpper">¶</a></h2>
<pre>func ToUpper(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToUpper returns a copy of the byte slice s with all Unicode letters mapped to
their upper case.

<a id="exampleToUpper"></a>
Example:

    fmt.Printf("%s", bytes.ToUpper([]byte("Gopher")))
    // Output: GOPHER

<h2 id="ToUpperSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L412">ToUpperSpecial</a>
    <a href="#ToUpperSpecial">¶</a></h2>
<pre>func ToUpperSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToUpperSpecial returns a copy of the byte slice s with all Unicode letters
mapped to their upper case, giving priority to the special casing rules.

<h2 id="Trim">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L620">Trim</a>
    <a href="#Trim">¶</a></h2>
<pre>func Trim(s []<a href="/builtin/#byte">byte</a>, cutset <a href="/builtin/#string">string</a>) []<a href="/builtin/#byte">byte</a></pre>

Trim returns a subslice of s by slicing off all leading and trailing
UTF-8-encoded Unicode code points contained in cutset.

<a id="exampleTrim"></a>
Example:

    fmt.Printf("[%q]", bytes.Trim([]byte(" !!! Achtung! Achtung! !!! "), "! "))
    // Output: ["Achtung! Achtung"]

<h2 id="TrimFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L499">TrimFunc</a>
    <a href="#TrimFunc">¶</a></h2>
<pre>func TrimFunc(s []<a href="/builtin/#byte">byte</a>, f func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimFunc returns a subslice of s by slicing off all leading and trailing
UTF-8-encoded Unicode code points c that satisfy f(c).

<h2 id="TrimLeft">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L626">TrimLeft</a>
    <a href="#TrimLeft">¶</a></h2>
<pre>func TrimLeft(s []<a href="/builtin/#byte">byte</a>, cutset <a href="/builtin/#string">string</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimLeft returns a subslice of s by slicing off all leading UTF-8-encoded
Unicode code points contained in cutset.

<h2 id="TrimLeftFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L476">TrimLeftFunc</a>
    <a href="#TrimLeftFunc">¶</a></h2>
<pre>func TrimLeftFunc(s []<a href="/builtin/#byte">byte</a>, f func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimLeftFunc returns a subslice of s by slicing off all leading UTF-8-encoded
Unicode code points c that satisfy f(c).

<h2 id="TrimPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L505">TrimPrefix</a>
    <a href="#TrimPrefix">¶</a></h2>
<pre>func TrimPrefix(s, prefix []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimPrefix returns s without the provided leading prefix string. If s doesn't
start with prefix, s is returned unchanged.

<a id="exampleTrimPrefix"></a>
Example:

    var b = []byte("Goodbye,, world!")
    b = bytes.TrimPrefix(b, []byte("Goodbye,"))
    b = bytes.TrimPrefix(b, []byte("See ya,"))
    fmt.Printf("Hello%s", b)
    // Output: Hello, world!

<h2 id="TrimRight">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L632">TrimRight</a>
    <a href="#TrimRight">¶</a></h2>
<pre>func TrimRight(s []<a href="/builtin/#byte">byte</a>, cutset <a href="/builtin/#string">string</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimRight returns a subslice of s by slicing off all trailing UTF-8-encoded
Unicode code points that are contained in cutset.

<h2 id="TrimRightFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L486">TrimRightFunc</a>
    <a href="#TrimRightFunc">¶</a></h2>
<pre>func TrimRightFunc(s []<a href="/builtin/#byte">byte</a>, f func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimRightFunc returns a subslice of s by slicing off all trailing UTF-8 encoded
Unicode code points c that satisfy f(c).

<h2 id="TrimSpace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L638">TrimSpace</a>
    <a href="#TrimSpace">¶</a></h2>
<pre>func TrimSpace(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimSpace returns a subslice of s by slicing off all leading and trailing white
space, as defined by Unicode.

<a id="exampleTrimSpace"></a>
Example:

    fmt.Printf("%s", bytes.TrimSpace([]byte(" \t\n a lone gopher \n\t\r\n")))
    // Output: a lone gopher

<h2 id="TrimSuffix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L514">TrimSuffix</a>
    <a href="#TrimSuffix">¶</a></h2>
<pre>func TrimSuffix(s, suffix []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimSuffix returns s without the provided trailing suffix string. If s doesn't
end with suffix, s is returned unchanged.

<a id="exampleTrimSuffix"></a>
Example:

    var b = []byte("Hello, goodbye, etc!")
    b = bytes.TrimSuffix(b, []byte("goodbye, etc!"))
    b = bytes.TrimSuffix(b, []byte("gopher"))
    b = append(b, bytes.TrimSuffix([]byte("world!"), []byte("x!"))...)
    os.Stdout.Write(b)
    // Output: Hello, world!

<h2 id="Buffer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L7">Buffer</a>
    <a href="#Buffer">¶</a></h2>
<pre>type Buffer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Buffer is a variable-sized buffer of bytes with Read and Write methods. The
zero value for Buffer is an empty buffer ready to use.

<a id="exampleBuffer"></a>
Example:

    var b bytes.Buffer // A Buffer needs no initialization.
    b.Write([]byte("Hello "))
    fmt.Fprintf(&b, "world!")
    b.WriteTo(os.Stdout)
    // Output: Hello world!


<a id="exampleBuffer_reader"></a>
Example:

    // A Buffer can turn a string or a []byte into an io.Reader.
    buf := bytes.NewBufferString("R29waGVycyBydWxlIQ==")
    dec := base64.NewDecoder(base64.StdEncoding, buf)
    io.Copy(os.Stdout, dec)
    // Output: Gophers rule!

<h3 id="NewBuffer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L443">NewBuffer</a>
    <a href="#NewBuffer">¶</a></h3>
<pre>func NewBuffer(buf []<a href="/builtin/#byte">byte</a>) *<a href="#Buffer">Buffer</a></pre>

NewBuffer creates and initializes a new Buffer using buf as its initial
contents. The new Buffer takes ownership of buf, and the caller should not use
buf after this call. NewBuffer is intended to prepare a Buffer to read existing
data. It can also be used to size the internal buffer for writing. To do that,
buf should have the desired capacity but a length of zero.

In most cases, new(Buffer) (or just declaring a Buffer variable) is sufficient
to initialize a Buffer.

<h3 id="NewBufferString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L451">NewBufferString</a>
    <a href="#NewBufferString">¶</a></h3>
<pre>func NewBufferString(s <a href="/builtin/#string">string</a>) *<a href="#Buffer">Buffer</a></pre>

NewBufferString creates and initializes a new Buffer using string s as its
initial contents. It is intended to prepare a buffer to read an existing string.

In most cases, new(Buffer) (or just declaring a Buffer variable) is sufficient
to initialize a Buffer.

<h3 id="Buffer.Bytes">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L42">Bytes</a>
    <a href="#Buffer.Bytes">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Bytes() []<a href="/builtin/#byte">byte</a></pre>

Bytes returns a slice of length b.Len() holding the unread portion of the
buffer. The slice is valid for use only until the next buffer modification (that
is, only until the next call to a method like Read, Write, Reset, or Truncate).
The slice aliases the buffer content at least until the next buffer
modification, so immediate changes to the slice will affect the result of future
reads.

<h3 id="Buffer.Cap">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L60">Cap</a>
    <a href="#Buffer.Cap">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Cap() <a href="/builtin/#int">int</a></pre>

Cap returns the capacity of the buffer's underlying byte slice, that is, the
total space allocated for the buffer's data.

<h3 id="Buffer.Grow">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L138">Grow</a>
    <a href="#Buffer.Grow">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Grow(n <a href="/builtin/#int">int</a>)</pre>

Grow grows the buffer's capacity, if necessary, to guarantee space for another n
bytes. After Grow(n), at least n bytes can be written to the buffer without
another allocation. If n is negative, Grow will panic. If the buffer can't grow
it will panic with ErrTooLarge.

<a id="exampleBuffer_Grow"></a>
Example:

    var b bytes.Buffer
    b.Grow(64)
    bb := b.Bytes()
    b.Write([]byte("64 bytes or fewer"))
    fmt.Printf("%q", bb[:b.Len()])
    // Output: "64 bytes or fewer"

<h3 id="Buffer.Len">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L56">Len</a>
    <a href="#Buffer.Len">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Len() <a href="/builtin/#int">int</a></pre>

Len returns the number of bytes of the unread portion of the buffer; b.Len() ==
len(b.Bytes()).

<h3 id="Buffer.Next">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L311">Next</a>
    <a href="#Buffer.Next">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Next(n <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

Next returns a slice containing the next n bytes from the buffer, advancing the
buffer as if the bytes had been returned by Read. If there are fewer than n
bytes in the buffer, Next returns the entire buffer. The slice is only valid
until the next call to a read or write method.

<h3 id="Buffer.Read">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L289">Read</a>
    <a href="#Buffer.Read">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read reads the next len(p) bytes from the buffer or until the buffer is drained.
The return value n is the number of bytes read. If the buffer has no data to
return, err is io.EOF (unless len(p) is zero); otherwise it is nil.

<h3 id="Buffer.ReadByte">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L327">ReadByte</a>
    <a href="#Buffer.ReadByte">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) ReadByte() (<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadByte reads and returns the next byte from the buffer. If no byte is
available, it returns error io.EOF.

<h3 id="Buffer.ReadBytes">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L401">ReadBytes</a>
    <a href="#Buffer.ReadBytes">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) ReadBytes(delim <a href="/builtin/#byte">byte</a>) (line []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadBytes reads until the first occurrence of delim in the input, returning a
slice containing the data up to and including the delimiter. If ReadBytes
encounters an error before finding a delimiter, it returns the data read before
the error and the error itself (often io.EOF). ReadBytes returns err != nil if
and only if the returned data does not end in delim.

<h3 id="Buffer.ReadFrom">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L180">ReadFrom</a>
    <a href="#Buffer.ReadFrom">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) ReadFrom(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadFrom reads data from r until EOF and appends it to the buffer, growing the
buffer as needed. The return value n is the number of bytes read. Any error
except io.EOF encountered during the read is also returned. If the buffer
becomes too large, ReadFrom will panic with ErrTooLarge.

<h3 id="Buffer.ReadRune">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L345">ReadRune</a>
    <a href="#Buffer.ReadRune">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) ReadRune() (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadRune reads and returns the next UTF-8-encoded Unicode code point from the
buffer. If no bytes are available, the error returned is io.EOF. If the bytes
are an erroneous UTF-8 encoding, it consumes one byte and returns U+FFFD, 1.

<h3 id="Buffer.ReadString">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L429">ReadString</a>
    <a href="#Buffer.ReadString">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) ReadString(delim <a href="/builtin/#byte">byte</a>) (line <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadString reads until the first occurrence of delim in the input, returning a
string containing the data up to and including the delimiter. If ReadString
encounters an error before finding a delimiter, it returns the data read before
the error and the error itself (often io.EOF). ReadString returns err != nil if
and only if the returned data does not end in delim.

<h3 id="Buffer.Reset">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L80">Reset</a>
    <a href="#Buffer.Reset">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Reset()</pre>

Reset resets the buffer to be empty, but it retains the underlying storage for
use by future writes. Reset is the same as Truncate(0).

<h3 id="Buffer.String">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L46">String</a>
    <a href="#Buffer.String">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) String() <a href="/builtin/#string">string</a></pre>

String returns the contents of the unread portion of the buffer as a string. If
the Buffer is a nil pointer, it returns "<nil>".

<h3 id="Buffer.Truncate">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L65">Truncate</a>
    <a href="#Buffer.Truncate">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Truncate(n <a href="/builtin/#int">int</a>)</pre>

Truncate discards all but the first n unread bytes from the buffer but continues
to use the same allocated storage. It panics if n is negative or greater than
the length of the buffer.

<h3 id="Buffer.UnreadByte">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L384">UnreadByte</a>
    <a href="#Buffer.UnreadByte">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) UnreadByte() <a href="/builtin/#error">error</a></pre>

UnreadByte unreads the last byte returned by the most recent successful read
operation that read at least one byte. If a write has happened since the last
read, if the last read returned an error, or if the read read zero bytes,
UnreadByte returns an error.

<h3 id="Buffer.UnreadRune">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L369">UnreadRune</a>
    <a href="#Buffer.UnreadRune">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) UnreadRune() <a href="/builtin/#error">error</a></pre>

UnreadRune unreads the last rune returned by ReadRune. If the most recent read
or write operation on the buffer was not a successful ReadRune, UnreadRune
returns an error. (In this regard it is stricter than UnreadByte, which will
unread the last byte from any read operation.)

<h3 id="Buffer.Write">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L149">Write</a>
    <a href="#Buffer.Write">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Write(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write appends the contents of p to the buffer, growing the buffer as needed. The
return value n is the length of p; err is always nil. If the buffer becomes too
large, Write will panic with ErrTooLarge.

<h3 id="Buffer.WriteByte">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L256">WriteByte</a>
    <a href="#Buffer.WriteByte">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) WriteByte(c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

WriteByte appends the byte c to the buffer, growing the buffer as needed. The
returned error is always nil, but is included to match bufio.Writer's WriteByte.
If the buffer becomes too large, WriteByte will panic with ErrTooLarge.

<h3 id="Buffer.WriteRune">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L270">WriteRune</a>
    <a href="#Buffer.WriteRune">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) WriteRune(r <a href="/builtin/#rune">rune</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteRune appends the UTF-8 encoding of Unicode code point r to the buffer,
returning its length and an error, which is always nil but is included to match
bufio.Writer's WriteRune. The buffer is grown as needed; if it becomes too
large, WriteRune will panic with ErrTooLarge.

<h3 id="Buffer.WriteString">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L161">WriteString</a>
    <a href="#Buffer.WriteString">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) WriteString(s <a href="/builtin/#string">string</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteString appends the contents of s to the buffer, growing the buffer as
needed. The return value n is the length of s; err is always nil. If the buffer
becomes too large, WriteString will panic with ErrTooLarge.

<h3 id="Buffer.WriteTo">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L228">WriteTo</a>
    <a href="#Buffer.WriteTo">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteTo writes data to w until the buffer is drained or an error occurs. The
return value n is the number of bytes written; it always fits into an int, but
it is int64 to match the io.WriterTo interface. Any error encountered during the
write is also returned.

<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L7">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Reader implements the io.Reader, io.ReaderAt, io.WriterTo, io.Seeker,
io.ByteScanner, and io.RuneScanner interfaces by reading from a byte slice.
Unlike a Buffer, a Reader is read-only and supports seeking.

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L140">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(b []<a href="/builtin/#byte">byte</a>) *<a href="#Reader">Reader</a></pre>

NewReader returns a new Reader reading from b.

<h3 id="Reader.Len">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L15">Len</a>
    <a href="#Reader.Len">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Len() <a href="/builtin/#int">int</a></pre>

Len returns the number of bytes of the unread portion of the slice.

<h3 id="Reader.Read">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L28">Read</a>
    <a href="#Reader.Read">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Read(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="Reader.ReadAt">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L38">ReadAt</a>
    <a href="#Reader.ReadAt">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadAt(b []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="Reader.ReadByte">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L53">ReadByte</a>
    <a href="#Reader.ReadByte">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadByte() (<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>


<h3 id="Reader.ReadRune">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L72">ReadRune</a>
    <a href="#Reader.ReadRune">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadRune() (ch <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="Reader.Reset">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L137">Reset</a>
    <a href="#Reader.Reset">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Reset(b []<a href="/builtin/#byte">byte</a>)</pre>

Reset resets the Reader to be reading from b.

<h3 id="Reader.Seek">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L97">Seek</a>
    <a href="#Reader.Seek">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Seek(offset <a href="/builtin/#int64">int64</a>, whence <a href="/builtin/#int">int</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>

Seek implements the io.Seeker interface.

<h3 id="Reader.Size">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L26">Size</a>
    <a href="#Reader.Size">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Size() <a href="/builtin/#int64">int64</a></pre>

Size returns the original length of the underlying byte slice. Size is the
number of bytes available for reading via ReadAt. The returned value is always
the same and is not affected by calls to any other method.

<h3 id="Reader.UnreadByte">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L63">UnreadByte</a>
    <a href="#Reader.UnreadByte">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) UnreadByte() <a href="/builtin/#error">error</a></pre>


<h3 id="Reader.UnreadRune">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L87">UnreadRune</a>
    <a href="#Reader.UnreadRune">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) UnreadRune() <a href="/builtin/#error">error</a></pre>


<h3 id="Reader.WriteTo">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L118">WriteTo</a>
    <a href="#Reader.WriteTo">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteTo implements the io.WriterTo interface.

<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L456)  The rule Title uses for word boundaries does not handle Unicode punctuation
  properly.


