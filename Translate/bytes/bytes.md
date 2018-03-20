version: 1.10
## package bytes

  `import "bytes"`

## 概述

bytes 包提供操作字节切片的函数。它类似于特殊的 strings 包。

## 索引

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

### 例子

- [Buffer](#exampleBuffer)
- [Buffer.Grow](#exampleBuffer_Grow)
- [Buffer (Reader)](#exampleBuffer_reader)
- [Compare](#exampleCompare)
- [Compare (Search)](#exampleCompare_search)
- [Contains](#exampleContains)
- [ContainsAny](#exampleContainsAny)
- [ContainsRune](#exampleContainsRune)
- [Count](#exampleCount)
- [Equal](#exampleEqual)
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
- [Reader.Len](#exampleReader_Len)
- [Repeat](#exampleRepeat)
- [Replace](#exampleReplace)
- [Runes](#exampleRunes)
- [Split](#exampleSplit)
- [SplitAfter](#exampleSplitAfter)
- [SplitAfterN](#exampleSplitAfterN)
- [SplitN](#exampleSplitN)
- [Title](#exampleTitle)
- [ToLower](#exampleToLower)
- [ToTitle](#exampleToTitle)
- [ToUpper](#exampleToUpper)
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

 [buffer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go) [bytes.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go) [bytes_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_amd64.go) [bytes_decl.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_decl.go) [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="MinRead">MinRead</span> = 512</pre>

MinRead 是 Buffer.ReadFrom 读取的最小数。如果 MinRead 个字节已经能够容纳 r 中的内容，那么就不会再增加底层缓存空间。

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrTooLarge">ErrTooLarge</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;bytes.Buffer: too large&#34;)</pre>

如果内存不能再分配缓存空间保存数据就会把 ErrTooLarge 传递给 panic。

<h2 id="Compare">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_decl.go#L14">Compare</a>
    <a href="#Compare">¶</a></h2>
<pre>func Compare(a, b []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

Compare 比较连个字节切片并根据结果返回一个整数。（`a==b` 时返回 0，`a < b` 时返回 -1，`a > b` 时返回 +1）。参数为 nil 默认为空切片。

<a id="exampleCompare"></a>
例:

    // 通过和 0 进行比较来解释流程控制语句的含义
    var a, b []byte
    if bytes.Compare(a, b) < 0 {
        // a 小于 b
    }
    if bytes.Compare(a, b) <= 0 {
        // a 小于等于 b
    }
    if bytes.Compare(a, b) > 0 {
        // a 大于 b
    }
    if bytes.Compare(a, b) >= 0 {
        // a 大于等于 b
    }
    
    // 推荐使用 Equal 来判断等于的情况
    if bytes.Equal(a, b) {
        // a equal b
    }
    if !bytes.Equal(a, b) {
        // a not equal b
    }

<a id="exampleCompare_search"></a>
<a id="exampleCompare_search"></a>
例:

    // 二进制搜索指定的字节切片
    var needle []byte
    var haystack [][]byte // 排序后
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

Contains 判断 b 中是否包含 subslice。

<a id="exampleContains"></a>
例:

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

ContainsAny 判断 b 中是否包含 chars 中的任意 UTF-8 代码点

<a id="exampleContainsAny"></a>
例:

    fmt.Println(bytes.ContainsAny([]byte("I like seafood."), "fÄo!"))
    fmt.Println(bytes.ContainsAny([]byte("I like seafood."), "去是伟大的."))
    fmt.Println(bytes.ContainsAny([]byte("I like seafood."), ""))
    fmt.Println(bytes.ContainsAny([]byte(""), ""))
    // Output:
    // true
    // true
    // false
    // false

<h2 id="ContainsRune">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L67">ContainsRune</a>
    <a href="#ContainsRune">¶</a></h2>
<pre>func ContainsRune(b []<a href="/builtin/#byte">byte</a>, r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

ContainsRune 判断 b 中是否包含 UTF-8 编码字节 rune。

<a id="exampleContainsRune"></a>
例:

    fmt.Println(bytes.ContainsRune([]byte("I like seafood."), 'f'))
    fmt.Println(bytes.ContainsRune([]byte("I like seafood."), 'ö'))
    fmt.Println(bytes.ContainsRune([]byte("去是伟大的!"), '大'))
    fmt.Println(bytes.ContainsRune([]byte("去是伟大的!"), '!'))
    fmt.Println(bytes.ContainsRune([]byte(""), '@'))
    // Output:
    // true
    // false
    // true
    // true
    // false

<h2 id="Count">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_amd64.go#L73">Count</a>
    <a href="#Count">¶</a></h2>
<pre>func Count(s, sep []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

Count 计算 s 中 sep 非重叠出现的次数。如果 sep 为空切片，Count 返回 1 + s 中 UTF-8 代码点的数量。

<a id="exampleCount"></a>
例:

    fmt.Println(bytes.Count([]byte("cheese"), []byte("e")))
    fmt.Println(bytes.Count([]byte("five"), []byte(""))) // before & after each rune
    // Output:
    // 3
    // 5

<h2 id="Equal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_decl.go#L7">Equal</a>
    <a href="#Equal">¶</a></h2>
<pre>func Equal(a, b []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

Equal 在 a 和 b 长度相同并包含相同元素时返回 true。参数为 nil 时默认为空切片。

<a id="exampleEqual"></a>
例:

    fmt.Println(bytes.Equal([]byte("Go"), []byte("Go")))
    fmt.Println(bytes.Equal([]byte("Go"), []byte("C++")))
    // Output:
    // true
    // false

<h2 id="EqualFold">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L764">EqualFold</a>
    <a href="#EqualFold">¶</a></h2>
<pre>func EqualFold(s, t []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

EqualFold 判断 s 和 t 在 UTF-8 编码下忽略大小写时是否相等。

<a id="exampleEqualFold"></a>
例:

    fmt.Println(bytes.EqualFold([]byte("Go"), []byte("go")))
    // Output: true

<h2 id="Fields">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L268">Fields</a>
    <a href="#Fields">¶</a></h2>
<pre>func Fields(s []<a href="/builtin/#byte">byte</a>) [][]<a href="/builtin/#byte">byte</a></pre>

Fields 会把 s 当作 UTF-8 代码点进行解析。它根据一个或多个空白符（unicode.IsSpace 定义）来分割 s 并返回分割结果切片，在 s 只包含空白符时返回空切片。

<a id="exampleFields"></a>
例:

    fmt.Printf("Fields are: %q", bytes.Fields([]byte("  foo bar  baz   ")))
    // Output: Fields are: ["foo" "bar" "baz"]

<h2 id="FieldsFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L324">FieldsFunc</a>
    <a href="#FieldsFunc">¶</a></h2>
<pre>func FieldsFunc(s []<a href="/builtin/#byte">byte</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) [][]<a href="/builtin/#byte">byte</a></pre>

FieldsFunc 会把 s 当作 UTF-8 代码点进行解析。他根据满足 `f(c)` 的代码点分割 s 并返回分割结果切片。如果所有的代码点都满足 `f(c)`，或者 `len(s)==0`，会返回空切片。FieldsFunc 不保证 `f(c)` 的调用顺序，如果相同 c 返回不同结果，FieldsFunc 会崩溃。

<a id="exampleFieldsFunc"></a>
例:

    f := func(c rune) bool {
        return !unicode.IsLetter(c) && !unicode.IsNumber(c)
    }
    fmt.Printf("Fields are: %q", bytes.FieldsFunc([]byte("  foo1;bar2,baz3..."), f))
    // Output: Fields are: ["foo1" "bar2" "baz3"]

<h2 id="HasPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L395">HasPrefix</a>
    <a href="#HasPrefix">¶</a></h2>
<pre>func HasPrefix(s, prefix []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

HasPrefix 判断 s 是否以 prefix 作为前缀。

<a id="exampleHasPrefix"></a>
例:

    fmt.Println(bytes.HasPrefix([]byte("Gopher"), []byte("Go")))
    fmt.Println(bytes.HasPrefix([]byte("Gopher"), []byte("C")))
    fmt.Println(bytes.HasPrefix([]byte("Gopher"), []byte("")))
    // Output:
    // true
    // false
    // true

<h2 id="HasSuffix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L400">HasSuffix</a>
    <a href="#HasSuffix">¶</a></h2>
<pre>func HasSuffix(s, suffix []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

HasSuffix 判断 s 是否以 suffix 作为后缀。

<a id="exampleHasSuffix"></a>
例:

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

Index 返回 s 中第一次出现 sep 的位置，如果不存在 sep 返回 -1。

<a id="exampleIndex"></a>
例:

    fmt.Println(bytes.Index([]byte("chicken"), []byte("ken")))
    fmt.Println(bytes.Index([]byte("chicken"), []byte("dmr")))
    // Output:
    // 4
    // -1

<h2 id="IndexAny">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L136">IndexAny</a>
    <a href="#IndexAny">¶</a></h2>
<pre>func IndexAny(s []<a href="/builtin/#byte">byte</a>, chars <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

IndexAny 会把 s 当作 UTF-8 代码点进行解析。它返回 s 中第一次出现 chars 中 Unicode 代码点的位置。如果 chars 为空或不存在返回 -1。

<a id="exampleIndexAny"></a>
例:

    fmt.Println(bytes.IndexAny([]byte("chicken"), "aeiouy"))
    fmt.Println(bytes.IndexAny([]byte("crwth"), "aeiouy"))
    // Output:
    // 2
    // -1

<h2 id="IndexByte">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes_decl.go#L1">IndexByte</a>
    <a href="#IndexByte">¶</a></h2>
<pre>func IndexByte(s []<a href="/builtin/#byte">byte</a>, c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

IndexByte 返回 s 中第一次出现 c 的位置，如果不存在返回 -1。

<a id="exampleIndexByte"></a>
例:

    fmt.Println(bytes.IndexByte([]byte("chicken"), byte('k')))
    fmt.Println(bytes.IndexByte([]byte("chicken"), byte('g')))
    // Output:
    // 4
    // -1

<h2 id="IndexFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L588">IndexFunc</a>
    <a href="#IndexFunc">¶</a></h2>
<pre>func IndexFunc(s []<a href="/builtin/#byte">byte</a>, f func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#int">int</a></pre>

IndexFunc 会把 s 当作 UTF-8 代码点进行解析。它返回 s 中第一个满足 `f(c)` 的代码点的位置，如果没有返回 -1。

<a id="exampleIndexFunc"></a>
例:

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

IndexFunc 会把 s 当作 UTF-8 代码点进行解析。它返回 s 中第一次出现 rune 的位置。如果没有返回 -1。如果 r 为 utf8.RuneError，它返回第一次出现无效 UTF-8 字符序列的位置。

<a id="exampleIndexRune"></a>
例:

    fmt.Println(bytes.IndexRune([]byte("chicken"), 'k'))
    fmt.Println(bytes.IndexRune([]byte("chicken"), 'd'))
    // Output:
    // 4
    // -1

<h2 id="Join">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L372">Join</a>
    <a href="#Join">¶</a></h2>
<pre>func Join(s [][]<a href="/builtin/#byte">byte</a>, sep []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

Join 会把 s 中的切片用 sep 连接起来并返回。sep 会出现在 s 的每个元素之间。

<a id="exampleJoin"></a>
例:

    s := [][]byte{[]byte("foo"), []byte("bar"), []byte("baz")}
    fmt.Printf("%s", bytes.Join(s, []byte(", ")))
    // Output: foo, bar, baz

<h2 id="LastIndex">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L81">LastIndex</a>
    <a href="#LastIndex">¶</a></h2>
<pre>func LastIndex(s, sep []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

LastIndex 返回 s 中最后一次出现 sep 的位置，如果不存在返回 -1。

<a id="exampleLastIndex"></a>
例:

    fmt.Println(bytes.Index([]byte("go gopher"), []byte("go")))
    fmt.Println(bytes.LastIndex([]byte("go gopher"), []byte("go")))
    fmt.Println(bytes.LastIndex([]byte("go gopher"), []byte("rodent")))
    // Output:
    // 0
    // 3
    // -1

<h2 id="LastIndexAny">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L172">LastIndexAny</a>
    <a href="#LastIndexAny">¶</a></h2>
<pre>func LastIndexAny(s []<a href="/builtin/#byte">byte</a>, chars <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

LastIndexAny 会把 s 当作 UTF-8 代码点进行解析。它返回 s 最后一次出现 chars 中 Unicode 代码点的位置。如果没有或 chars 为空返回 -1。

<a id="exampleLastIndexAny"></a>
例:

    fmt.Println(bytes.LastIndexAny([]byte("go gopher"), "MüQp"))
    fmt.Println(bytes.LastIndexAny([]byte("go 地鼠"), "地大"))
    fmt.Println(bytes.LastIndexAny([]byte("go gopher"), "z,!."))
    // Output:
    // 5
    // 3
    // -1

<h2 id="LastIndexByte">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L96">LastIndexByte</a>
    <a href="#LastIndexByte">¶</a></h2>
<pre>func LastIndexByte(s []<a href="/builtin/#byte">byte</a>, c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

LastIndexByte 返回 s 中最后一次出现 c 的位置，如果不存在返回 -1。

<a id="exampleLastIndexByte"></a>
例:

    fmt.Println(bytes.LastIndexByte([]byte("go gopher"), byte('g')))
    fmt.Println(bytes.LastIndexByte([]byte("go gopher"), byte('r')))
    fmt.Println(bytes.LastIndexByte([]byte("go gopher"), byte('z')))
    // Output:
    // 3
    // 8
    // -1

<h2 id="LastIndexFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L595">LastIndexFunc</a>
    <a href="#LastIndexFunc">¶</a></h2>
<pre>func LastIndexFunc(s []<a href="/builtin/#byte">byte</a>, f func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#int">int</a></pre>

LastIndexFunc 会把 s 当作 UTF-8 代码点进行解析。它返回最后一个满足 `f(c)` 的 Unicode 代码点的位置，如果没有返回 -1。

<a id="exampleLastIndexFunc"></a>
例:

    fmt.Println(bytes.LastIndexFunc([]byte("go gopher!"), unicode.IsLetter))
    fmt.Println(bytes.LastIndexFunc([]byte("go gopher!"), unicode.IsPunct))
    fmt.Println(bytes.LastIndexFunc([]byte("go gopher!"), unicode.IsNumber))
    // Output:
    // 8
    // 9
    // -1

<h2 id="Map">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L408">Map</a>
    <a href="#Map">¶</a></h2>
<pre>func Map(mapping func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a>, s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

Map 返回每个字节都被 mapping 处理过的 s 切片的拷贝。如果 mappding 返回负数该字符不会出现在结果切片中。s 中的字符会以 UTF-8 代码点的形式进行解析。

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
    fmt.Printf("%s", bytes.Map(rot13, []byte("'Twas brillig and the slithy gopher...")))
    // Output: 'Gjnf oevyyvt naq gur fyvgul tbcure...

<h2 id="Repeat">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L445">Repeat</a>
    <a href="#Repeat">¶</a></h2>
<pre>func Repeat(b []<a href="/builtin/#byte">byte</a>, count <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

Repeat 返回重复 count 次 b 的新切片。

如果 count 为负数或者  (len(b) * count)  溢出函数会 panic。

<a id="exampleRepeat"></a>
例:

    fmt.Printf("ba%s", bytes.Repeat([]byte("na"), 2))
    // Output: banana

<h2 id="Replace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L726">Replace</a>
    <a href="#Replace">¶</a></h2>
<pre>func Replace(s, old, new []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

Replace 将 s 中的前 n 个 old 替换成 new 并作为新切片返回。如果 old 为空，它会匹配切片de1起点和每个 UTF-8 序列的结尾，对于 k 个 rune 的切片会替换 k + 1 次。如果 n<0，不会限制替换次数。

<a id="exampleReplace"></a>
例:

    fmt.Printf("%s\n", bytes.Replace([]byte("oink oink oink"), []byte("k"), []byte("ky"), 2))
    fmt.Printf("%s\n", bytes.Replace([]byte("oink oink oink"), []byte("oink"), []byte("moo"), -1))
    // Output:
    // oinky oinky oink
    // moo moo moo

<h2 id="Runes">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L708">Runes</a>
    <a href="#Runes">¶</a></h2>
<pre>func Runes(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#rune">rune</a></pre>

Runes 会把 s 当作 UTF-8 代码点进行解析。它返回 s 对应的 rune（Unicode 代码点）切片。

<a id="exampleRunes"></a>
例:

    rs := bytes.Runes([]byte("go gopher"))
    for _, r := range rs {
        fmt.Printf("%#U\n", r)
    }
    // Output:
    // U+0067 'g'
    // U+006F 'o'
    // U+0020 ' '
    // U+0067 'g'
    // U+006F 'o'
    // U+0070 'p'
    // U+0068 'h'
    // U+0065 'e'
    // U+0072 'r'

<h2 id="Split">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L252">Split</a>
    <a href="#Split">¶</a></h2>
<pre>func Split(s, sep []<a href="/builtin/#byte">byte</a>) [][]<a href="/builtin/#byte">byte</a></pre>

Split 根据 sep 分割 s 并返回分割结果组成的切片。如果 sep 为空，Split 会分割每个 UTF-8 序列。它和 count 等于 -1 的 SplitN 相同。 

<a id="exampleSplit"></a>
例:

    fmt.Printf("%q\n", bytes.Split([]byte("a,b,c"), []byte(",")))
    fmt.Printf("%q\n", bytes.Split([]byte("a man a plan a canal panama"), []byte("a ")))
    fmt.Printf("%q\n", bytes.Split([]byte(" xyz "), []byte("")))
    fmt.Printf("%q\n", bytes.Split([]byte(""), []byte("Bernardo O'Higgins")))
    // Output:
    // ["a" "b" "c"]
    // ["" "man " "plan " "canal panama"]
    // [" " "x" "y" "z" " "]
    // [""]

<h2 id="SplitAfter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L258">SplitAfter</a>
    <a href="#SplitAfter">¶</a></h2>
<pre>func SplitAfter(s, sep []<a href="/builtin/#byte">byte</a>) [][]<a href="/builtin/#byte">byte</a></pre>

SplitAfter 从 s 中的每个 sep 后的位置分割 s 并返回分割结果组成的切片。如果 sep 为空，SplitAfter 会分割每个 UTF-8 序列。它和 count 为 -1 的 SplitAfterN 相同。

<a id="exampleSplitAfter"></a>
例:

    fmt.Printf("%q\n", bytes.SplitAfter([]byte("a,b,c"), []byte(",")))
    // Output: ["a," "b," "c"]

<h2 id="SplitAfterN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L244">SplitAfterN</a>
    <a href="#SplitAfterN">¶</a></h2>
<pre>func SplitAfterN(s, sep []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) [][]<a href="/builtin/#byte">byte</a></pre>

SplitAfterN slices s into subslices after each instance of sep and returns a
slice of those subslices. If sep is empty, SplitAfterN splits after each UTF-8
sequence. The count determines the number of subslices to return:

SplitAfterN 从 s 中的每个 sep 后的位置分割 s 并返回分割结果组成的切片。如果 sep 为空，SplitAfter 会分割每个 UTF-8 序列。count 决定返回结果元素的数量：

    n > 0: 最多 n 个元素。最后一个元素包含 s 中未分割的部分。
    n == 0: 返回 nil。
    n < 0: 不限制返回结果长度。

<a id="exampleSplitAfterN"></a>
例:

    fmt.Printf("%q\n", bytes.SplitAfterN([]byte("a,b,c"), []byte(","), 2))
    // Output: ["a," "b,c"]

<h2 id="SplitN">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L235">SplitN</a>
    <a href="#SplitN">¶</a></h2>
<pre>func SplitN(s, sep []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) [][]<a href="/builtin/#byte">byte</a></pre>

SplitN 根据 sep 分割 s 并返回分割结果组成的切片。如果 sep 为空，Split 会分割每个 UTF-8 序列。count 决定返回结果元素的数量：

    n > 0: 最多 n 个元素。最后一个元素包含 s 中未分割的部分。
    n == 0: 返回 nil。
    n < 0: 不限制返回结果长度。

<a id="exampleSplitN"></a>
例:

    fmt.Printf("%q\n", bytes.SplitN([]byte("a,b,c"), []byte(","), 2))
    z := bytes.SplitN([]byte("a,b,c"), []byte(","), 0)
    fmt.Printf("%q (nil = %v)\n", z, z == nil)
    // Output:
    // ["a" "b,c"]
    // [] (nil = true)

<h2 id="Title">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L521">Title</a>
    <a href="#Title">¶</a></h2>
<pre>func Title(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

Title 把 s 做为 UTF-8 编码字节切片并返回单词首字母为 Unicode 字符对应 Title 形式的切片。

BUG(rsc): Title 使用的规则不会处理 Unicode 的标点符号。

<a id="exampleTitle"></a>
例:

    fmt.Printf("%s", bytes.Title([]byte("her royal highness")))
    // Output: Her Royal Highness

<h2 id="ToLower">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L469">ToLower</a>
    <a href="#ToLower">¶</a></h2>
<pre>func ToLower(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToLower 把 s 做为 UTF-8 编码字节切片返回由所有 Unicode 字符对应的小写形式组成的切片。

<a id="exampleToLower"></a>
例:

    fmt.Printf("%s", bytes.ToLower([]byte("Gopher")))
    // Output: gopher

<h2 id="ToLowerSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L482">ToLowerSpecial</a>
    <a href="#ToLowerSpecial">¶</a></h2>
<pre>func ToLowerSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToLowerSpecial 把 s 做为 UTF-8 编码字节切片返回根据指定规则 c 由所有 Unicode 字符对应的小写形式组成的切片。

<h2 id="ToTitle">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L472">ToTitle</a>
    <a href="#ToTitle">¶</a></h2>
<pre>func ToTitle(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToTitle 把 s 做为 UTF-8 编码字节切片并返回所有 Unicode 字符对应的 Title 形式组成的切片。

<a id="exampleToTitle"></a>
例:

    fmt.Printf("%s\n", bytes.ToTitle([]byte("loud noises")))
    fmt.Printf("%s\n", bytes.ToTitle([]byte("хлеб")))
    // Output:
    // LOUD NOISES
    // ХЛЕБ

<h2 id="ToTitleSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L488">ToTitleSpecial</a>
    <a href="#ToTitleSpecial">¶</a></h2>
<pre>func ToTitleSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToTitleSpecial 把 s 做为 UTF-8 编码字节切片并根据指定规则 c 返回所有 Unicode 字符对应的 Title 形式组成的切片。

<h2 id="ToUpper">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L466">ToUpper</a>
    <a href="#ToUpper">¶</a></h2>
<pre>func ToUpper(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToTitle 把 s 做为 UTF-8 编码字节切片并返回所有 Unicode 字符对应的大写形式组成的切片。

<a id="exampleToUpper"></a>
例:

    fmt.Printf("%s", bytes.ToUpper([]byte("Gopher")))
    // Output: GOPHER

<h2 id="ToUpperSpecial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L476">ToUpperSpecial</a>
    <a href="#ToUpperSpecial">¶</a></h2>
<pre>func ToUpperSpecial(c <a href="/unicode/">unicode</a>.<a href="/unicode/#SpecialCase">SpecialCase</a>, s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

ToUpperSpecial 把 s 做为 UTF-8 编码字节切片并根据指定规则 c 返回所有 Unicode 字符对应的大写形式组成的切片。

<h2 id="Trim">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L684">Trim</a>
    <a href="#Trim">¶</a></h2>
<pre>func Trim(s []<a href="/builtin/#byte">byte</a>, cutset <a href="/builtin/#string">string</a>) []<a href="/builtin/#byte">byte</a></pre>

Trim 会返回 s 的一个子串，它不包含开头和结尾的 cutset。

<a id="exampleTrim"></a>
例:

    fmt.Printf("[%q]", bytes.Trim([]byte(" !!! Achtung! Achtung! !!! "), "! "))
    // Output: ["Achtung! Achtung"]

<h2 id="TrimFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L563">TrimFunc</a>
    <a href="#TrimFunc">¶</a></h2>
<pre>func TrimFunc(s []<a href="/builtin/#byte">byte</a>, f func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimFunc 去掉 s 中开头和结尾满足 `f(c)` 的 UTF-8 代码点。

<a id="exampleTrimFunc"></a>
例:

    fmt.Println(string(bytes.TrimFunc([]byte("go-gopher!"), unicode.IsLetter)))
    fmt.Println(string(bytes.TrimFunc([]byte("\"go-gopher!\""), unicode.IsLetter)))
    fmt.Println(string(bytes.TrimFunc([]byte("go-gopher!"), unicode.IsPunct)))
    fmt.Println(string(bytes.TrimFunc([]byte("1234go-gopher!567"), unicode.IsNumber)))
    // Output:
    // -gopher!
    // "go-gopher!"
    // go-gopher
    // go-gopher!

<h2 id="TrimLeft">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L690">TrimLeft</a>
    <a href="#TrimLeft">¶</a></h2>
<pre>func TrimLeft(s []<a href="/builtin/#byte">byte</a>, cutset <a href="/builtin/#string">string</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimLeft 去掉 s 开头 cutset 包含的 UTF-8 代码点。

<a id="exampleTrimLeft"></a>
例:

    fmt.Print(string(bytes.TrimLeft([]byte("453gopher8257"), "0123456789")))
    // Output:
    // gopher8257

<h2 id="TrimLeftFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L540">TrimLeftFunc</a>
    <a href="#TrimLeftFunc">¶</a></h2>
<pre>func TrimLeftFunc(s []<a href="/builtin/#byte">byte</a>, f func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimLeftFunc 会把 s 最为 UTF-8 编码字节处理并去掉开头满足 `f(c)` 的 UTF-8 代码点并返回。

<a id="exampleTrimLeftFunc"></a>
例:

    fmt.Println(string(bytes.TrimLeftFunc([]byte("go-gopher"), unicode.IsLetter)))
    fmt.Println(string(bytes.TrimLeftFunc([]byte("go-gopher!"), unicode.IsPunct)))
    fmt.Println(string(bytes.TrimLeftFunc([]byte("1234go-gopher!567"), unicode.IsNumber)))
    // Output:
    // -gopher
    // go-gopher!
    // go-gopher!567

<h2 id="TrimPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L569">TrimPrefix</a>
    <a href="#TrimPrefix">¶</a></h2>
<pre>func TrimPrefix(s, prefix []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimPrefix 去掉 s 的 prefix 前缀并返回，如果 s 没有 prefix 前缀，s 直接返回。

<a id="exampleTrimPrefix"></a>
例:

    var b = []byte("Goodbye,, world!")
    b = bytes.TrimPrefix(b, []byte("Goodbye,"))
    b = bytes.TrimPrefix(b, []byte("See ya,"))
    fmt.Printf("Hello%s", b)
    // Output: Hello, world!

<h2 id="TrimRight">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L696">TrimRight</a>
    <a href="#TrimRight">¶</a></h2>
<pre>func TrimRight(s []<a href="/builtin/#byte">byte</a>, cutset <a href="/builtin/#string">string</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimRight 去掉 s 中所有 cutset 中的 UTF-8 代码点并返回。

<a id="exampleTrimRight"></a>
例:

    fmt.Print(string(bytes.TrimRight([]byte("453gopher8257"), "0123456789")))
    // Output:
    // 453gopher

<h2 id="TrimRightFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L550">TrimRightFunc</a>
    <a href="#TrimRightFunc">¶</a></h2>
<pre>func TrimRightFunc(s []<a href="/builtin/#byte">byte</a>, f func(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimRightFunc 去掉 s 中满足 `f(c)` 的 UTF-8 代码点并返回。

<a id="exampleTrimRightFunc"></a>
例:

    fmt.Println(string(bytes.TrimRightFunc([]byte("go-gopher"), unicode.IsLetter)))
    fmt.Println(string(bytes.TrimRightFunc([]byte("go-gopher!"), unicode.IsPunct)))
    fmt.Println(string(bytes.TrimRightFunc([]byte("1234go-gopher!567"), unicode.IsNumber)))
    // Output:
    // go-
    // go-gopher
    // 1234go-gopher!

<h2 id="TrimSpace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L702">TrimSpace</a>
    <a href="#TrimSpace">¶</a></h2>
<pre>func TrimSpace(s []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimSpace 去掉 s 中开头和结尾的 Unicode 定义的空白符并返回。

<a id="exampleTrimSpace"></a>
例:

    fmt.Printf("%s", bytes.TrimSpace([]byte(" \t\n a lone gopher \n\t\r\n")))
    // Output: a lone gopher

<h2 id="TrimSuffix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L578">TrimSuffix</a>
    <a href="#TrimSuffix">¶</a></h2>
<pre>func TrimSuffix(s, suffix []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a></pre>

TrimSuffix 返回去掉 suffix 后缀的 s 。如果 s 没有 suffix 后缀，会直接返回 s。

<a id="exampleTrimSuffix"></a>
例:

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

Buffer 是一个可以读取或写入数据的动态尺寸的字节缓存区。Buffer 的零值是一个可以使用的缓冲区。

<a id="exampleBuffer"></a>
例:

    var b bytes.Buffer // Buffer 不需要初始化
    b.Write([]byte("Hello "))
    fmt.Fprintf(&b, "world!")
    b.WriteTo(os.Stdout)
    // Output: Hello world!

<a id="exampleBuffer_reader"></a>
<a id="exampleBuffer_reader"></a>
例:

    // Buffer 将字符串或者 []byte 放入 io.Reader。
    buf := bytes.NewBufferString("R29waGVycyBydWxlIQ==")
    dec := base64.NewDecoder(base64.StdEncoding, buf)
    io.Copy(os.Stdout, dec)
    // Output: Gophers rule!

<h3 id="NewBuffer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L439">NewBuffer</a>
    <a href="#NewBuffer">¶</a></h3>
<pre>func NewBuffer(buf []<a href="/builtin/#byte">byte</a>) *<a href="#Buffer">Buffer</a></pre>

NewBuffer 把 buf 做为初始内容来创建和初始化一个新的 Buffer。新的 Buffer 接管 buf，并且调用者不应该在调用后再使用 buf。NewBuffer 主要为了准备一块缓冲区读取现存数据。它也可以用来调整写入内部缓冲区的大小。如果想这么做，buf 应是一个指定容积长度为零的切片。

In most cases, new(Buffer) (or just declaring a Buffer variable) is sufficient
to initialize a Buffer.

<h3 id="NewBufferString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L447">NewBufferString</a>
    <a href="#NewBufferString">¶</a></h3>
<pre>func NewBufferString(s <a href="/builtin/#string">string</a>) *<a href="#Buffer">Buffer</a></pre>

NewBufferString 把字符串 s 作为初始内容创建并初始化一个新的 Buffer。它主要为了准备一块缓冲区来读取现在的字符串。

大多数情况下，new(Buffer) （或者只声明一个 Buffer 变量）就可以初始化一个 Buffer。

<h3 id="Buffer.Bytes">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L45">Bytes</a>
    <a href="#Buffer.Bytes">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Bytes() []<a href="/builtin/#byte">byte</a></pre>

Bytes 返回长度 b.Len() 对应的未读缓存的切片。切片在下一次缓存修改（例如：Read，Write，Reset 或 Truncate）之前都有效。切片至少在下一次缓存改变前是引用缓存数据的，所以改变切片内容会影响之后的读取操作。

<h3 id="Buffer.Cap">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L68">Cap</a>
    <a href="#Buffer.Cap">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Cap() <a href="/builtin/#int">int</a></pre>

Cap 返回缓存底层字节切片的容积。它是分配给缓存的空间大小。

<h3 id="Buffer.Grow">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L149">Grow</a>
    <a href="#Buffer.Grow">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Grow(n <a href="/builtin/#int">int</a>)</pre>

Grow 会为缓存增加 n 字节的容积。在调用 ` Grow(n)` 之后，不用分配额外空间就可以至少向缓存写入 n 字节的数据。如果 n 为负数，Grow 会 panic。如果缓存不会再增加会 panic 并携带 ErrTooLarge 错误。

<a id="exampleBuffer_Grow"></a>
例:

    var b bytes.Buffer
    b.Grow(64)
    bb := b.Bytes()
    b.Write([]byte("64 bytes or fewer"))
    fmt.Printf("%q", bb[:b.Len()])
    // Output: "64 bytes or fewer"

<h3 id="Buffer.Len">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L64">Len</a>
    <a href="#Buffer.Len">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Len() <a href="/builtin/#int">int</a></pre>

Len 返回缓存中未读的字节数；`b.Len()==len(b.Bytes())`。

<h3 id="Buffer.Next">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L309">Next</a>
    <a href="#Buffer.Next">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Next(n <a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

Next 返回一个包含缓存中下 n 字节的切片，就像 Read 返回的字节一样。如果缓存中含有不到 n 个字符，Next 会把它们全部返回。返回的切片在下一次的读写调用之前有效。

<h3 id="Buffer.Read">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L287">Read</a>
    <a href="#Buffer.Read">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read 从缓存中读取下 len(p) 个字节或者直到缓存最后。返回值 n 为读取到的字节数。如果缓存会有数据返回，err 为 io.EOF(除非 len(p) 为 0)；否则 err 为 nil。

<h3 id="Buffer.ReadByte">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L325">ReadByte</a>
    <a href="#Buffer.ReadByte">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) ReadByte() (<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadByte 读取并返回缓存的下一个字节，如果没有有效字节返回 io.EOF。

<h3 id="Buffer.ReadBytes">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L397">ReadBytes</a>
    <a href="#Buffer.ReadBytes">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) ReadBytes(delim <a href="/builtin/#byte">byte</a>) (line []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadBytes 会读取缓存中直到第一个 delim 字节的数据，返回的字节切片包括 delim 和它之前的数据。如果 ReadBytes 在找到 delim 之前发生错误，会返回发生错误之前读取的数据和错误本身（一般为 io.EOF）。只有在数据没有以 delim 结束的时候返回的 err != nil。

<h3 id="Buffer.ReadFrom">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L191">ReadFrom</a>
    <a href="#Buffer.ReadFrom">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) ReadFrom(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadFrom 从 r 中读取一直到 EOF 的数据并将数据追加到缓存中，缓存的大小会按需增加。返回值 n 为读取的字节数。除了 io.EOF 以外任何读取时发生的错误都会返回。如果缓存变得过大 Write 会 panic 并附带 ErrTooLarge 错误。

<h3 id="Buffer.ReadRune">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L342">ReadRune</a>
    <a href="#Buffer.ReadRune">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) ReadRune() (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadRune 读取并返回缓存中的下一个 UTF-8 代码点。如果没有有效字节，返回的错误为 io.EOF。如果字节为无效的 UFT-8 编码字节，返回 U+FFFD, 1。

<h3 id="Buffer.ReadString">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L425">ReadString</a>
    <a href="#Buffer.ReadString">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) ReadString(delim <a href="/builtin/#byte">byte</a>) (line <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadString 会读取缓存中直到第一个 delim 字节的数据，返回的字符串包括 delim 和它之前的数据。如果 ReadString 在找到 delim 之前发生错误，会返回发生错误之前读取的数据和错误本身（一般为 io.EOF）。只有在数据没有以 delim 结束的时候返回的 err != nil。

<h3 id="Buffer.Reset">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L88">Reset</a>
    <a href="#Buffer.Reset">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Reset()</pre>

Reset 会清空缓存但是保留底层的存储空间用来接收写入的内容。Reset 和 Truncate(0) 相同。

<h3 id="Buffer.String">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L51">String</a>
    <a href="#Buffer.String">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) String() <a href="/builtin/#string">string</a></pre>

String 把缓存的未读部分作为字符串返回。如果缓存是一个 nil 指针，它会返回 `<nil>`

更有效的构建字符串可以使用 strings.Builder 类型。

<h3 id="Buffer.Truncate">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L73">Truncate</a>
    <a href="#Buffer.Truncate">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Truncate(n <a href="/builtin/#int">int</a>)</pre>

Truncate 丢弃缓存中除前 n 个未读字节以外的其他内容并继续使用原来的存储空间。如果 n 为负数或者大于缓存长度会 panic。

<h3 id="Buffer.UnreadByte">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L380">UnreadByte</a>
    <a href="#Buffer.UnreadByte">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) UnreadByte() <a href="/builtin/#error">error</a></pre>

UnreadByte 将最近一次成功的读取操作返回的字节标记为未读。如果在最近一次读取之后发生写入操作，或者最近一次读取操作返回 error，或着读取操作读取了 0 字节，那么 UnreadByte 就会返回错误。

<h3 id="Buffer.UnreadRune">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L365">UnreadRune</a>
    <a href="#Buffer.UnreadRune">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) UnreadRune() <a href="/builtin/#error">error</a></pre>

UnreadRune 将 ReadRune 返回的最近一个 rune 标记为未读。如果针对缓存最近的读取或写入操作没有成功，UnreadRune 会返回错误（在这方面它比 UnreadByte 更加严格，UnreadByte 会把最近任何一个读取操作的字符标记为未读）。

<h3 id="Buffer.Write">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L160">Write</a>
    <a href="#Buffer.Write">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) Write(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write 将 p 追加进缓存，缓存会按需增大。返回值 n 为 p 的长度；err 一直为 nil。如果缓存变得过大 Write 会 panic 并附带 ErrTooLarge 错误。

<h3 id="Buffer.WriteByte">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L254">WriteByte</a>
    <a href="#Buffer.WriteByte">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) WriteByte(c <a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

WriteByte 把 c 追加进缓存，缓存会按需增大。返回的错误一直为 nil，它主要是为了匹配 bufio.Writer 的 WriteByte。

如果缓存变得过大 WriteByte 会 panic 并附带 ErrTooLarge 错误。

<h3 id="Buffer.WriteRune">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L268">WriteRune</a>
    <a href="#Buffer.WriteRune">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) WriteRune(r <a href="/builtin/#rune">rune</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteRune 把 UTF-8 代码点 r 追加进缓存，并返回它的长度和错误（固定为 nil 为了匹配 bufio.Writer 的 WriteRune 接口）。缓存会按需增大；如果缓存变得过大 WriteRune 会 panic 并附带 ErrTooLarge 错误。

<h3 id="Buffer.WriteString">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L172">WriteString</a>
    <a href="#Buffer.WriteString">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) WriteString(s <a href="/builtin/#string">string</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteString 会把 s 的内容追加给缓存，它会在需要的时候增加缓存大小。返回值 n 为 s 的长度；err 总是为 nil。如果缓存变得太大，WriteString 会 panic 并附带 ErrTooLarge 错误。

<h3 id="Buffer.WriteTo">func (*Buffer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/buffer.go#L227">WriteTo</a>
    <a href="#Buffer.WriteTo">¶</a></h3>
<pre>func (b *<a href="#Buffer">Buffer</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteTo 将数据写入 w 直到缓存已满或发生错误。返回值 n 为写入的字节数；它一般都是 int，不过为了满足 io.WriteTo 接口所以为 int64 类型。写入当中发生的错误都会返回给调用者。

<h2 id="Reader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L7">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Reader 实现了 io.Reader， io.ReaderAt， io.WriterTo， io.Seeker，io.ByteScanner 和 io.RuneScanner 接口来读取字节切片。与 Buffer 不同它是只读的并且支持 seeking。

<h3 id="NewReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L146">NewReader</a>
    <a href="#NewReader">¶</a></h3>
<pre>func NewReader(b []<a href="/builtin/#byte">byte</a>) *<a href="#Reader">Reader</a></pre>

NewReader 返回一个读取 b 的新 Reader。

<h3 id="Reader.Len">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L15">Len</a>
    <a href="#Reader.Len">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Len() <a href="/builtin/#int">int</a></pre>

Len 返回 Reader 还未读取的切片长度。

<a id="exampleReader_Len"></a>
例:

    fmt.Println(bytes.NewReader([]byte("Hi!")).Len())
    fmt.Println(bytes.NewReader([]byte("こんにちは!")).Len())
    // Output:
    // 3
    // 16

<h3 id="Reader.Read">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L29">Read</a>
    <a href="#Reader.Read">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Read(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read 实现了 io.Reader 接口。

<h3 id="Reader.ReadAt">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L40">ReadAt</a>
    <a href="#Reader.ReadAt">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadAt(b []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadAt 实现了 io.ReaderAt 接口。

<h3 id="Reader.ReadByte">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L56">ReadByte</a>
    <a href="#Reader.ReadByte">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadByte() (<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadByte 实现了 io.ByteReader 接口。

<h3 id="Reader.ReadRune">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L77">ReadRune</a>
    <a href="#Reader.ReadRune">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) ReadRune() (ch <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadRune 实现了 io.RuneReader 接口。

<h3 id="Reader.Reset">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L143">Reset</a>
    <a href="#Reader.Reset">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Reset(b []<a href="/builtin/#byte">byte</a>)</pre>

Reset 重置 Reader 来读取 b。

<h3 id="Reader.Seek">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L103">Seek</a>
    <a href="#Reader.Seek">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Seek(offset <a href="/builtin/#int64">int64</a>, whence <a href="/builtin/#int">int</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>

Seek 实现了 io.Seeker 接口。

<h3 id="Reader.Size">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L26">Size</a>
    <a href="#Reader.Size">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Size() <a href="/builtin/#int64">int64</a></pre>

Size 返回底层字节数组的长度。Size 是 ReadAt 能够读取的有效字节数。它的返回值是固定的不受其他方法影响

<h3 id="Reader.UnreadByte">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L67">UnreadByte</a>
    <a href="#Reader.UnreadByte">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) UnreadByte() <a href="/builtin/#error">error</a></pre>

UnreadByte 为 ReadByte 实现了 io.ByteScanner 接口。

<h3 id="Reader.UnreadRune">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L93">UnreadRune</a>
    <a href="#Reader.UnreadRune">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) UnreadRune() <a href="/builtin/#error">error</a></pre>

UnreadRune 为 ReadRune 实现了 io.RuneScanner 接口。

<h3 id="Reader.WriteTo">func (*Reader) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/reader.go#L124">WriteTo</a>
    <a href="#Reader.WriteTo">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteTo 实现了 io.WriterTo 接口。

<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/bytes/bytes.go#L520)  Title 使用的规则不会处理 Unicode 标点符号。


