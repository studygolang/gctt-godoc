version: 1.10
## package strings

  `import "strings"`

## 概述

strings 包实现了一些操作 UTF-8 字符串的函数。

关于更多 Go 中的 UTF-8 字符串的信息，可以访问 https://blog.golang.org/strings.

## 索引

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

### 例子

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

### 文件

 [builder.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go) [compare.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/compare.go) [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go) [replace.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/replace.go) [search.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/search.go) [strings.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go) [strings_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings_amd64.go) [strings_decl.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings_decl.go)

<h2 id="Compare">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/compare.go#L3">Compare</a>
    <a href="#Compare">¶</a></h2>
<pre>func Compare(a, b <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

Compare 根据字典顺序比较字符串 a 和 b。a == b 时返回 0，a < b 时返回 -1，a > b 时返回 +1。

Compare 主要为了和 bytes 包对应，通常使用内置的比较运算符（==，<，>）效率更高。

<a id="exampleCompare"></a>
例:

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

Contains 判断 s 是否包含子串 substr。

<a id="exampleContains"></a>
例:

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

ContainsAny 判断 s 是否包含 chars 中的任意 <ruby>Unicode 代码点<rt>Unicode code point</rt></ruby>。

<a id="exampleContainsAny"></a>
例:

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

ContainsRune 判断 s 是否包含 Unicode 代码点 r。

<a id="exampleContainsRune"></a>
例:

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

Count 获取在 s 中非重叠出现 substr 的次数。如果 substr 为空，则返回 s 中的 Unicode 代码点数量加一。

<a id="exampleCount"></a>
例:

    fmt.Println(strings.Count("cheese", "e"))
    fmt.Println(strings.Count("five", "")) // before & after each rune
    // Output:
    // 3
    // 5

<h2 id="EqualFold">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L867">EqualFold</a>
    <a href="#EqualFold">¶</a></h2>
<pre>func EqualFold(s, t <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

EqualFold 判断 s 和 t 在忽略大小写的情况下是否相等。

<a id="exampleEqualFold"></a>
例:

    fmt.Println(strings.EqualFold("Go", "go"))
    // Output: true

<h2 id="Fields">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L309">Fields</a>
    <a href="#Fields">¶</a></h2>
<pre>func Fields(s <a href="/builtin/#string">string</a>) []<a href="/builtin/#string">string</a></pre>

Fields 根据空白字符（Go 中通过 unicode.IsSpace 判断是否为空白字符）分割字符串并将结果以切片形式返回。当 s 只包含空白字符时返回值为空。

<a id="exampleFields"></a>
例:

    fmt.Printf("Fields are: %q", strings.Fields("  foo bar  baz   "))
    // Output: Fields are: ["foo" "bar" "baz"]

<h2 id="FieldsFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L363">FieldsFunc</a>
    <a href="#FieldsFunc">¶</a></h2>
<pre>func FieldsFunc(s <a href="/builtin/#string">string</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) []<a href="/builtin/#string">string</a></pre>

FieldsFunc 根据满足函数 f(c) 的 Unicode 代码点 c 分割字符串。当 s 中所有的字符都满足 f(c) 函数或 s 为空时返回空。FieldsFunc 不保证 f(c) 的调用顺序，如果指定的 c 调用 f 返回结果不一致函数会崩溃。

<a id="exampleFieldsFunc"></a>
例:

    f := func(c rune) bool {
        return !unicode.IsLetter(c) && !unicode.IsNumber(c)
    }
    fmt.Printf("Fields are: %q", strings.FieldsFunc("  foo1;bar2,baz3...", f))
    // Output: Fields are: ["foo1" "bar2" "baz3"]

<h2 id="HasPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L435">HasPrefix</a>
    <a href="#HasPrefix">¶</a></h2>
<pre>func HasPrefix(s, prefix <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

HasPrefix 判断 s 是否以 prefix 为前缀。

<a id="exampleHasPrefix"></a>
例:

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

HasSuffix 判断 s 是否以 suffix 为后缀。

<a id="exampleHasSuffix"></a>
例:

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

Index 返回 s 中 substr 第一次出现的位置，如果没有返回 -1。

<a id="exampleIndex"></a>
例:

    fmt.Println(strings.Index("chicken", "ken"))
    fmt.Println(strings.Index("chicken", "dmr"))
    // Output:
    // 4
    // -1

<h2 id="IndexAny">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L158">IndexAny</a>
    <a href="#IndexAny">¶</a></h2>
<pre>func IndexAny(s, chars <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

IndexAny 返回 s 中 chars 中任意 Unicode 代码点第一次出现的位置，如果没有返回 -1。

<a id="exampleIndexAny"></a>
例:

    fmt.Println(strings.IndexAny("chicken", "aeiouy"))
    fmt.Println(strings.IndexAny("crwth", "aeiouy"))
    // Output:
    // 2
    // -1

<h2 id="IndexByte">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings_decl.go#L1">IndexByte</a>
    <a href="#IndexByte">¶</a></h2>
<pre>func IndexByte(s <a href="/builtin/#string">string</a>, c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

IndexByte 返回 s 中 c 字节第一次出现的位置，如果没有返回 -1。

<a id="exampleIndexByte"></a>
例:

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

IndexFunc 返回 s 中满足 f(c) 函数的 Unicode 代码点第一次出现的位置，如果没有返回 -1。

<a id="exampleIndexFunc"></a>
例:

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

IndexRune 返回 s 中 r 第一次出现的位置。如果没有返回 -1。当 r 是 utf8.RuneError 时返回无效 UTF-8 字节序列第一次出现的位置。

<a id="exampleIndexRune"></a>
例:

    fmt.Println(strings.IndexRune("chicken", 'k'))
    fmt.Println(strings.IndexRune("chicken", 'd'))
    // Output:
    // 4
    // -1

<h2 id="Join">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L405">Join</a>
    <a href="#Join">¶</a></h2>
<pre>func Join(a []<a href="/builtin/#string">string</a>, sep <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Join 将 a 中所有元素用 sep 连接并返回。

<a id="exampleJoin"></a>
例:

    s := []string{"foo", "bar", "baz"}
    fmt.Println(strings.Join(s, ", "))
    // Output: foo, bar, baz

<h2 id="LastIndex">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L98">LastIndex</a>
    <a href="#LastIndex">¶</a></h2>
<pre>func LastIndex(s, substr <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

LastIndex 返回 s 中最后一次出现 sustr 时的位置，如果没有返回 -1。

<a id="exampleLastIndex"></a>
例:

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

LastIndexAny 返回 s 中最后一次出现 chars 中 Unicode 代码点时的位置，如果没有返回 -1。

<a id="exampleLastIndexAny"></a>
例:

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

LastIndexByte 返回 s 中最后一次出现 c 字节时的位置，如果没有返回 -1。

<a id="exampleLastIndexByte"></a>
例:

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

LastIndexFunc 返回 s 中最后一个满足 f(c) 函数的 Unicode 代码点的位置，如果没有返回 -1。

<a id="exampleLastIndexFunc"></a>
例:

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

Map 为 s 中的每个字符应用回调函数 mapping 并返回处理结果。如果 mapping 返回一个负值该字符将会被丢弃。

<a id="exampleMap"></a>
例:

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

Repeat 把 s 重复 count 次并返回结果。当 count 是负数或者 len(s) * count 溢出函数会 panic。

<a id="exampleRepeat"></a>
例:

    fmt.Println("ba" + strings.Repeat("na", 2))
    // Output: banana

<h2 id="Replace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L831">Replace</a>
    <a href="#Replace">¶</a></h2>
<pre>func Replace(s, old, new <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

Replace 把 s 中的前 n 个非重叠的 old 替换为 new 并返回结果。如果 old 为空将会替换 UTF-8 代码点的前/后位置（例：长度为 k 的 UTF-8 字符串将被应用 k+1 次替换）。当 n < 0 时不会限制次数。

<a id="exampleReplace"></a>
例:

    fmt.Println(strings.Replace("oink oink oink", "k", "ky", 2))
    fmt.Println(strings.Replace("oink oink oink", "oink", "moo", -1))
    // Output:
    // oinky oinky oink
    // moo moo moo

<h2 id="Split">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L288">Split</a>
    <a href="#Split">¶</a></h2>
<pre>func Split(s, sep <a href="/builtin/#string">string</a>) []<a href="/builtin/#string">string</a></pre>

Split 将 s 根据 sep 分割并以切片形式返回。

如果 s 中不包含 sep 并且 sep 不为空 Split 会把 s 作为切片元素返回（长度为 1）。

如果 sep 为空，将分割每个 UTF-8 字符。如果 s 和 sep 都为空 Split 会返回空切片。

Split 和 SplitN(-1) 是等价的。

<a id="exampleSplit"></a>
例:

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

SplitAfter 从每个 sep 后面分割 s 并以切片形式返回。

如果 s 中不包含 sep 并且 sep 不为空 SplitAfter 会把 s 作为切片元素返回（长度为 1）。

如果 sep 为空则分割每个 UTF-8 字符。如果 s 和 sep 都为空 SplitAfter 会返回空切片。

SplitAfter 和 SplitAfterN(-1) 是等价的。

<a id="exampleSplitAfter"></a>
例:

    fmt.Printf("%q\n", strings.SplitAfter("a,b,c", ","))
    // Output: ["a," "b," "c"]

<h2 id="SplitAfterN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L274">SplitAfterN</a>
    <a href="#SplitAfterN">¶</a></h2>
<pre>func SplitAfterN(s, sep <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) []<a href="/builtin/#string">string</a></pre>

SplitAfterN 从每个 sep 后面分割 s 并以切片形式返回。

参数 n 决定函数的返回值:

    n > 0: 最多分割出 n 个子串; 最后一个子串不会再被分割
    n == 0: 返回值为 nil
    n < 0: 不限制分割后的子串数量

对于 s 和 sep 的边缘情况处理与 SplitAfter 相同。

<a id="exampleSplitAfterN"></a>
例:

    fmt.Printf("%q\n", strings.SplitAfterN("a,b,c", ",", 2))
    // Output: ["a," "b,c"]

<h2 id="SplitN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L262">SplitN</a>
    <a href="#SplitN">¶</a></h2>
<pre>func SplitN(s, sep <a href="/builtin/#string">string</a>, n <a href="/builtin/#int">int</a>) []<a href="/builtin/#string">string</a></pre>

Split 将 s 根据 sep 分割并返回。

参数 n 决定函数的返回值:

    n > 0: 最多分割出 n 个子串; 最后一个子串不会再被分割
    n == 0: 返回值为 nil
    n < 0: 不限制分割后的子串数量

对于 s 和 sep 的边缘情况处理与 Split 相同。

<a id="exampleSplitN"></a>
例:

    fmt.Printf("%q\n", strings.SplitN("a,b,c", ",", 2))
    z := strings.SplitN("a,b,c", ",", 0)
    fmt.Printf("%q (nil = %v)\n", z, z == nil)
    // Output:
    // ["a" "b,c"]
    // [] (nil = true)

<h2 id="Title">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L650">Title</a>
    <a href="#Title">¶</a></h2>
<pre>func Title(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Title 把 s 中每个单词的首字母转换成 Title 形式并返回。

BUG(rsc): Title 使用的判断字边界的规则会忽略 Unicode 标点符号。

<a id="exampleTitle"></a>
例:

    fmt.Println(strings.Title("her royal highness"))
    // Output: Her Royal Highness

<h2 id="ToLower">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L572">ToLower</a>
    <a href="#ToLower">¶</a></h2>
<pre>func ToLower(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToLower 将 s 中的所有 Unicode 字符转换成小写。

<a id="exampleToLower"></a>
例:

    fmt.Println(strings.ToLower("Gopher"))
    // Output: gopher

<h2 id="ToLowerSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L611">ToLowerSpecial</a>
    <a href="#ToLowerSpecial">¶</a></h2>
<pre>func ToLowerSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToLowerSpecial 根据 c 指定的优先规则将 s 中的所有 Unicode 代码点转换为小写。

<a id="exampleToLowerSpecial"></a>
例:

    fmt.Println(strings.ToLowerSpecial(unicode.TurkishCase, "Önnek İş"))
    // Output: önnek iş

<h2 id="ToTitle">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L601">ToTitle</a>
    <a href="#ToTitle">¶</a></h2>
<pre>func ToTitle(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToTile 将 s 中的所有 Unicode 字符转换成 Title 形式。

<a id="exampleToTitle"></a>
例:

    fmt.Println(strings.ToTitle("loud noises"))
    fmt.Println(strings.ToTitle("хлеб"))
    // Output:
    // LOUD NOISES
    // ХЛЕБ

<h2 id="ToTitleSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L617">ToTitleSpecial</a>
    <a href="#ToTitleSpecial">¶</a></h2>
<pre>func ToTitleSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToTitleSpecial 根据 c 指定的优先规则将 s 中的所有 Unicode 代码点转换成 Title 形式。

<a id="exampleToTitleSpecial"></a>
例:

    fmt.Println(strings.ToTitleSpecial(unicode.TurkishCase, "dünyanın ilk borsa yapısı Aizonai kabul edilir"))
    // Output:
    // DÜNYANIN İLK BORSA YAPISI AİZONAİ KABUL EDİLİR

<h2 id="ToUpper">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L543">ToUpper</a>
    <a href="#ToUpper">¶</a></h2>
<pre>func ToUpper(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToUpper 将 s 中所有的 Unicode 字符转换成大写。

<a id="exampleToUpper"></a>
例:

    fmt.Println(strings.ToUpper("Gopher"))
    // Output: GOPHER

<h2 id="ToUpperSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L605">ToUpperSpecial</a>
    <a href="#ToUpperSpecial">¶</a></h2>
<pre>func ToUpperSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToUpperSpecial 根据 c 指定的优先规则将 s 中的所有字符转换成大写。

<a id="exampleToUpperSpecial"></a>
例:

    fmt.Println(strings.ToUpperSpecial(unicode.TurkishCase, "örnek iş"))
    // Output: ÖRNEK İŞ

<h2 id="Trim">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L776">Trim</a>
    <a href="#Trim">¶</a></h2>
<pre>func Trim(s <a href="/builtin/#string">string</a>, cutset <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Trim 去掉 s 头部和尾部所有 cutset 中的 Unicode 代码点。

<a id="exampleTrim"></a>
例:

    fmt.Print(strings.Trim("¡¡¡Hello, Gophers!!!", "!¡"))
    // Output: Hello, Gophers

<h2 id="TrimFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L692">TrimFunc</a>
    <a href="#TrimFunc">¶</a></h2>
<pre>func TrimFunc(s <a href="/builtin/#string">string</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#string">string</a></pre>

TrimFunc 去掉 s 头部和尾部所有满足 f(c) 的 Unicode 代码点。

<a id="exampleTrimFunc"></a>
例:

    fmt.Print(strings.TrimFunc("¡¡¡Hello, Gophers!!!", func(r rune) bool {
        return !unicode.IsLetter(r) && !unicode.IsNumber(r)
    }))
    // Output: Hello, Gophers

<h2 id="TrimLeft">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L785">TrimLeft</a>
    <a href="#TrimLeft">¶</a></h2>
<pre>func TrimLeft(s <a href="/builtin/#string">string</a>, cutset <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimLeft 去掉 s 头部所有 cutset 中的 Unicode 代码点。

<a id="exampleTrimLeft"></a>
例:

    fmt.Print(strings.TrimLeft("¡¡¡Hello, Gophers!!!", "!¡"))
    // Output: Hello, Gophers!!!

<h2 id="TrimLeftFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L669">TrimLeftFunc</a>
    <a href="#TrimLeftFunc">¶</a></h2>
<pre>func TrimLeftFunc(s <a href="/builtin/#string">string</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#string">string</a></pre>

TrimLeftFunc 去掉 s 头部所有满足 f(c) 的 Unicode 代码点。

<a id="exampleTrimLeftFunc"></a>
例:

    fmt.Print(strings.TrimLeftFunc("¡¡¡Hello, Gophers!!!", func(r rune) bool {
        return !unicode.IsLetter(r) && !unicode.IsNumber(r)
    }))
    // Output: Hello, Gophers!!!

<h2 id="TrimPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L809">TrimPrefix</a>
    <a href="#TrimPrefix">¶</a></h2>
<pre>func TrimPrefix(s, prefix <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimPrefix 去掉 s 的 prefix 前缀。如果 s 不以 prefix 作为前缀则直接返回。

<a id="exampleTrimPrefix"></a>
例:

    var s = "¡¡¡Hello, Gophers!!!"
    s = strings.TrimPrefix(s, "¡¡¡Hello, ")
    s = strings.TrimPrefix(s, "¡¡¡Howdy, ")
    fmt.Print(s)
    // Output: Gophers!!!

<h2 id="TrimRight">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L794">TrimRight</a>
    <a href="#TrimRight">¶</a></h2>
<pre>func TrimRight(s <a href="/builtin/#string">string</a>, cutset <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimRignt 去掉 s 尾部所有 cutset 中的 Unicode 代码点。

<a id="exampleTrimRight"></a>
例:

    fmt.Print(strings.TrimRight("¡¡¡Hello, Gophers!!!", "!¡"))
    // Output: ¡¡¡Hello, Gophers

<h2 id="TrimRightFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L679">TrimRightFunc</a>
    <a href="#TrimRightFunc">¶</a></h2>
<pre>func TrimRightFunc(s <a href="/builtin/#string">string</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#string">string</a></pre>

TrimRightFunc 去掉字符串 s 尾部满足 f(c) 函数的 Unicode 代码点。

<a id="exampleTrimRightFunc"></a>
例:

    fmt.Print(strings.TrimRightFunc("¡¡¡Hello, Gophers!!!", func(r rune) bool {
        return !unicode.IsLetter(r) && !unicode.IsNumber(r)
    }))
    // Output: ¡¡¡Hello, Gophers

<h2 id="TrimSpace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L803">TrimSpace</a>
    <a href="#TrimSpace">¶</a></h2>
<pre>func TrimSpace(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimSpace 去掉字符串 s 两边的空格。

<a id="exampleTrimSpace"></a>
例:

    fmt.Println(strings.TrimSpace(" \t\n Hello, Gophers \n\t\r\n"))
    // Output: Hello, Gophers

<h2 id="TrimSuffix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L818">TrimSuffix</a>
    <a href="#TrimSuffix">¶</a></h2>
<pre>func TrimSuffix(s, suffix <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TrimSuffix 去掉字符串 s 中的指定后缀 suffix。如果 s 不是以 suffix 结尾返回原字符串。

<a id="exampleTrimSuffix"></a>
例:

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

Builder 可以使用 Write 高效的创建字符串。他会把内存拷贝降到最低程度。可以直接使用 Builder 的零值。不要复制一个非零的 Builder。

<a id="exampleBuilder"></a>
例:

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

Grow 会增加 b 的容量确保他有足够的空间容纳 n 字节。在调用 Grow(n) 后，至少向 b 中写入 n 字节时不会重新分配内存。如果 n 为负数，Grow 会 panic。

<h3 id="Builder.Len">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L41">Len</a>
    <a href="#Builder.Len">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) Len() <a href="/builtin/#int">int</a></pre>

Len 返回已经写入的字节；b.Len() == len(b.String())。

<h3 id="Builder.Reset">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L44">Reset</a>
    <a href="#Builder.Reset">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) Reset()</pre>

Reset 将 Builder 重置为空。

<h3 id="Builder.String">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L36">String</a>
    <a href="#Builder.String">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) String() <a href="/builtin/#string">string</a></pre>

String 返回已经写入的字符串。

<h3 id="Builder.Write">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L72">Write</a>
    <a href="#Builder.Write">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) Write(p []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write 会把 p 的内容追加到 b 的缓存中。Write 总是返回 len(p), nil。

<h3 id="Builder.WriteByte">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L80">WriteByte</a>
    <a href="#Builder.WriteByte">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) WriteByte(c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

WriteByte 将字节 c 追加到 b 的缓存中。返回的错误总是 nil。

<h3 id="Builder.WriteRune">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L88">WriteRune</a>
    <a href="#Builder.WriteRune">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) WriteRune(r <a href="/builtin/#rune">rune</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteRune 将 UTF-8 编码的 Unicode 代码点 r 追加到 b 的缓存中。它返回 r 的长度和 nil 错误。

<h3 id="Builder.WriteString">func (*Builder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/builder.go#L105">WriteString</a>
    <a href="#Builder.WriteString">¶</a></h3>
<pre>func (b *<a href="#Builder">Builder</a>) WriteString(s <a href="/builtin/#string">string</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteString 将 s 的内容追加到 b 的缓存中。它返回 s 的长度和 nil。

<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L6">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Reader 实现对字符串读取的 io.Reader、io.ReaderAt、io.Seeker、io.WriterTo、io.ByteScanner 和 io.RuneScanner 接口。

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L140">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(s <a href="/builtin/#string">string</a>) *<a href="#Reader">Reader</a></pre>

NewReader 返回一个读取 s 的 Reader。它与 bytes.NewBufferString 类似不过 Reader 是只读的并且效率更高。

<h3 id="Reader.Len">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L14">Len</a>
    <a href="#Reader.Len">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Len() <a href="/builtin/#int">int</a></pre>

Len 返回字符串未读取部分的字节数。

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

Reset 使 Reader 开始读取 s。

<h3 id="Reader.Seek">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L96">Seek</a>
    <a href="#Reader.Seek">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Seek(offset <a href="/builtin/#int64">int64</a>, whence <a href="/builtin/#int">int</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>

Seek 方法实现了 io.Seeker 接口。

<h3 id="Reader.Size">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L25">Size</a>
    <a href="#Reader.Size">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Size() <a href="/builtin/#int64">int64</a></pre>

Size 方法返回底层字符串的长度。它也是 ReadAt 能读取到的有效字节数。该返回值不受其他方法影响。

<h3 id="Reader.UnreadByte">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L62">UnreadByte</a>
    <a href="#Reader.UnreadByte">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) UnreadByte() <a href="/builtin/#error">error</a></pre>


<h3 id="Reader.UnreadRune">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L86">UnreadRune</a>
    <a href="#Reader.UnreadRune">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) UnreadRune() <a href="/builtin/#error">error</a></pre>


<h3 id="Reader.WriteTo">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/reader.go#L117">WriteTo</a>
    <a href="#Reader.WriteTo">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteTo 方法实现了 io.WriterTo 接口。

<h2 id="Replacer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/replace.go#L1">Replacer</a>
    <a href="#Replacer">¶</a></h2>
<pre>type Replacer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Replacer 根据给定的替换列表来替换字符串。它可以安全的被多个 goroutine 同时使用。

<h3 id="NewReplacer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/replace.go#L13">NewReplacer</a>
    <a href="#NewReplacer">¶</a></h3>
<pre>func NewReplacer(oldnew ...<a href="/builtin/#string">string</a>) *<a href="#Replacer">Replacer</a></pre>

NewReplacer 会配置 Replacer 的替换列表（每项都包含一个替换目标和替换值）并返回 Replacer。替换操作会按顺序进行并且不会重叠。

<a id="exampleNewReplacer"></a>
例:

    r := strings.NewReplacer("<", "&lt;", ">", "&gt;")
    fmt.Println(r.Replace("This is <b>HTML</b>!"))
    // Output: This is &lt;b&gt;HTML&lt;/b&gt;!

<h3 id="Replacer.Replace">func (*Replacer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/replace.go#L59">Replace</a>
    <a href="#Replacer.Replace">¶</a></h3>
<pre>func (r *<a href="#Replacer">Replacer</a>) Replace(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Repalce 对 s 应用替换并返回替换结果。

<h3 id="Replacer.WriteString">func (*Replacer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/replace.go#L64">WriteString</a>
    <a href="#Replacer.WriteString">¶</a></h3>
<pre>func (r *<a href="#Replacer">Replacer</a>) WriteString(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, s <a href="/builtin/#string">string</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteString 方法对 s 应用替换后将结果写入 w 中。

<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/strings/strings.go#L649)  Title 使用的判断字边界的规则会忽略 Unicode 标点符号。


