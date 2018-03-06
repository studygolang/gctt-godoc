version: 1.10
## package utf8

  `import "unicode/utf8"`

## Overview

Package utf8 implements functions and constants to support text encoded in
UTF-8. It includes functions to translate between runes and UTF-8 byte
sequences.

## Index

- [Constants](#pkg-constants)
- [func DecodeLastRune(p []byte) (r rune, size int)](#DecodeLastRune)
- [func DecodeLastRuneInString(s string) (r rune, size int)](#DecodeLastRuneInString)
- [func DecodeRune(p []byte) (r rune, size int)](#DecodeRune)
- [func DecodeRuneInString(s string) (r rune, size int)](#DecodeRuneInString)
- [func EncodeRune(p []byte, r rune) int](#EncodeRune)
- [func FullRune(p []byte) bool](#FullRune)
- [func FullRuneInString(s string) bool](#FullRuneInString)
- [func RuneCount(p []byte) int](#RuneCount)
- [func RuneCountInString(s string) (n int)](#RuneCountInString)
- [func RuneLen(r rune) int](#RuneLen)
- [func RuneStart(b byte) bool](#RuneStart)
- [func Valid(p []byte) bool](#Valid)
- [func ValidRune(r rune) bool](#ValidRune)
- [func ValidString(s string) bool](#ValidString)

### Examples

- [DecodeLastRune](#exampleDecodeLastRune)
- [DecodeLastRuneInString](#exampleDecodeLastRuneInString)
- [DecodeRune](#exampleDecodeRune)
- [DecodeRuneInString](#exampleDecodeRuneInString)
- [EncodeRune](#exampleEncodeRune)
- [FullRune](#exampleFullRune)
- [FullRuneInString](#exampleFullRuneInString)
- [RuneCount](#exampleRuneCount)
- [RuneCountInString](#exampleRuneCountInString)
- [RuneLen](#exampleRuneLen)
- [RuneStart](#exampleRuneStart)
- [Valid](#exampleValid)
- [ValidRune](#exampleValidRune)
- [ValidString](#exampleValidString)

### Package files
 [utf8.go](//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="RuneError">RuneError</span> = &#39;\uFFFD&#39;     <span class="comment">// the &#34;error&#34; Rune or &#34;Unicode replacement character&#34;</span>
    <span id="RuneSelf">RuneSelf</span>  = 0x80         <span class="comment">// characters below Runeself are represented as themselves in a single byte.</span>
    <span id="MaxRune">MaxRune</span>   = &#39;\U0010FFFF&#39; <span class="comment">// Maximum valid Unicode code point.</span>
    <span id="UTFMax">UTFMax</span>    = 4            <span class="comment">// maximum number of bytes of a UTF-8 encoded Unicode character.</span>
)</pre>

Numbers fundamental to the encoding.

<h2 id="DecodeLastRune">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L235">DecodeLastRune</a>
    <a href="#DecodeLastRune">¶</a></h2>
<pre>func DecodeLastRune(p []<a href="/builtin/#byte">byte</a>) (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>)</pre>

DecodeLastRune unpacks the last UTF-8 encoding in p and returns the rune and its
width in bytes. If p is empty it returns (RuneError, 0). Otherwise, if the
encoding is invalid, it returns (RuneError, 1). Both are impossible results for
correct, non-empty UTF-8.

An encoding is invalid if it is incorrect UTF-8, encodes a rune that is out of
range, or is not the shortest possible UTF-8 encoding for the value. No other
validation is performed.

<a id="exampleDecodeLastRune"></a>
Example:

    b := []byte("Hello, 世界")

    for len(b) > 0 {
        r, size := utf8.DecodeLastRune(b)
        fmt.Printf("%c %v\n", r, size)

        b = b[:len(b)-size]
    }
    // Output:
    // 界 3
    // 世 3
    //   1
    // , 1
    // o 1
    // l 1
    // l 1
    // e 1
    // H 1

<h2 id="DecodeLastRuneInString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L275">DecodeLastRuneInString</a>
    <a href="#DecodeLastRuneInString">¶</a></h2>
<pre>func DecodeLastRuneInString(s <a href="/builtin/#string">string</a>) (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>)</pre>

DecodeLastRuneInString is like DecodeLastRune but its input is a string. If s is
empty it returns (RuneError, 0). Otherwise, if the encoding is invalid, it
returns (RuneError, 1). Both are impossible results for correct, non-empty
UTF-8.

An encoding is invalid if it is incorrect UTF-8, encodes a rune that is out of
range, or is not the shortest possible UTF-8 encoding for the value. No other
validation is performed.

<a id="exampleDecodeLastRuneInString"></a>
Example:

    str := "Hello, 世界"

    for len(str) > 0 {
        r, size := utf8.DecodeLastRuneInString(str)
        fmt.Printf("%c %v\n", r, size)

        str = str[:len(str)-size]
    }
    // Output:
    // 界 3
    // 世 3
    //   1
    // , 1
    // o 1
    // l 1
    // l 1
    // e 1
    // H 1

<h2 id="DecodeRune">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L139">DecodeRune</a>
    <a href="#DecodeRune">¶</a></h2>
<pre>func DecodeRune(p []<a href="/builtin/#byte">byte</a>) (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>)</pre>

DecodeRune unpacks the first UTF-8 encoding in p and returns the rune and its
width in bytes. If p is empty it returns (RuneError, 0). Otherwise, if the
encoding is invalid, it returns (RuneError, 1). Both are impossible results for
correct, non-empty UTF-8.

An encoding is invalid if it is incorrect UTF-8, encodes a rune that is out of
range, or is not the shortest possible UTF-8 encoding for the value. No other
validation is performed.

<a id="exampleDecodeRune"></a>
Example:

    b := []byte("Hello, 世界")

    for len(b) > 0 {
        r, size := utf8.DecodeRune(b)
        fmt.Printf("%c %v\n", r, size)

        b = b[size:]
    }
    // Output:
    // H 1
    // e 1
    // l 1
    // l 1
    // o 1
    // , 1
    //   1
    // 世 3
    // 界 3

<h2 id="DecodeRuneInString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L187">DecodeRuneInString</a>
    <a href="#DecodeRuneInString">¶</a></h2>
<pre>func DecodeRuneInString(s <a href="/builtin/#string">string</a>) (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>)</pre>

DecodeRuneInString is like DecodeRune but its input is a string. If s is empty
it returns (RuneError, 0). Otherwise, if the encoding is invalid, it returns
(RuneError, 1). Both are impossible results for correct, non-empty UTF-8.

An encoding is invalid if it is incorrect UTF-8, encodes a rune that is out of
range, or is not the shortest possible UTF-8 encoding for the value. No other
validation is performed.

<a id="exampleDecodeRuneInString"></a>
Example:

    str := "Hello, 世界"

    for len(str) > 0 {
        r, size := utf8.DecodeRuneInString(str)
        fmt.Printf("%c %v\n", r, size)

        str = str[size:]
    }
    // Output:
    // H 1
    // e 1
    // l 1
    // l 1
    // o 1
    // , 1
    //   1
    // 世 3
    // 界 3

<h2 id="EncodeRune">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L329">EncodeRune</a>
    <a href="#EncodeRune">¶</a></h2>
<pre>func EncodeRune(p []<a href="/builtin/#byte">byte</a>, r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#int">int</a></pre>

EncodeRune writes into p (which must be large enough) the UTF-8 encoding of the
rune. It returns the number of bytes written.

<a id="exampleEncodeRune"></a>
Example:

    r := '世'
    buf := make([]byte, 3)

    n := utf8.EncodeRune(buf, r)

    fmt.Println(buf)
    fmt.Println(n)
    // Output:
    // [228 184 150]
    // 3

<h2 id="FullRune">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L92">FullRune</a>
    <a href="#FullRune">¶</a></h2>
<pre>func FullRune(p []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

FullRune reports whether the bytes in p begin with a full UTF-8 encoding of a
rune. An invalid encoding is considered a full Rune since it will convert as a
width-1 error rune.

<a id="exampleFullRune"></a>
Example:

    buf := []byte{228, 184, 150} // 世
    fmt.Println(utf8.FullRune(buf))
    fmt.Println(utf8.FullRune(buf[:2]))
    // Output:
    // true
    // false

<h2 id="FullRuneInString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L112">FullRuneInString</a>
    <a href="#FullRuneInString">¶</a></h2>
<pre>func FullRuneInString(s <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

FullRuneInString is like FullRune but its input is a string.

<a id="exampleFullRuneInString"></a>
Example:

    str := "世"
    fmt.Println(utf8.FullRuneInString(str))
    fmt.Println(utf8.FullRuneInString(str[:2]))
    // Output:
    // true
    // false

<h2 id="RuneCount">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L361">RuneCount</a>
    <a href="#RuneCount">¶</a></h2>
<pre>func RuneCount(p []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

RuneCount returns the number of runes in p. Erroneous and short encodings are
treated as single runes of width 1 byte.

<a id="exampleRuneCount"></a>
Example:

    buf := []byte("Hello, 世界")
    fmt.Println("bytes =", len(buf))
    fmt.Println("runes =", utf8.RuneCount(buf))
    // Output:
    // bytes = 13
    // runes = 9

<h2 id="RuneCountInString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L398">RuneCountInString</a>
    <a href="#RuneCountInString">¶</a></h2>
<pre>func RuneCountInString(s <a href="/builtin/#string">string</a>) (n <a href="/builtin/#int">int</a>)</pre>

RuneCountInString is like RuneCount but its input is a string.

<a id="exampleRuneCountInString"></a>
Example:

    str := "Hello, 世界"
    fmt.Println("bytes =", len(str))
    fmt.Println("runes =", utf8.RuneCountInString(str))
    // Output:
    // bytes = 13
    // runes = 9

<h2 id="RuneLen">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L309">RuneLen</a>
    <a href="#RuneLen">¶</a></h2>
<pre>func RuneLen(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#int">int</a></pre>

RuneLen returns the number of bytes required to encode the rune. It returns -1
if the rune is not a valid value to encode in UTF-8.

<a id="exampleRuneLen"></a>
Example:

    fmt.Println(utf8.RuneLen('a'))
    fmt.Println(utf8.RuneLen('界'))
    // Output:
    // 1
    // 3

<h2 id="RuneStart">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L435">RuneStart</a>
    <a href="#RuneStart">¶</a></h2>
<pre>func RuneStart(b <a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

RuneStart reports whether the byte could be the first byte of an encoded,
possibly invalid rune. Second and subsequent bytes always have the top two bits
set to 10.

<a id="exampleRuneStart"></a>
Example:

    buf := []byte("a界")
    fmt.Println(utf8.RuneStart(buf[0]))
    fmt.Println(utf8.RuneStart(buf[1]))
    fmt.Println(utf8.RuneStart(buf[2]))
    // Output:
    // true
    // true
    // false

<h2 id="Valid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L438">Valid</a>
    <a href="#Valid">¶</a></h2>
<pre>func Valid(p []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

Valid reports whether p consists entirely of valid UTF-8-encoded runes.

<a id="exampleValid"></a>
Example:

    valid := []byte("Hello, 世界")
    invalid := []byte{0xff, 0xfe, 0xfd}

    fmt.Println(utf8.Valid(valid))
    fmt.Println(utf8.Valid(invalid))
    // Output:
    // true
    // false

<h2 id="ValidRune">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L503">ValidRune</a>
    <a href="#ValidRune">¶</a></h2>
<pre>func ValidRune(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

ValidRune reports whether r can be legally encoded as UTF-8. Code points that
are out of range or a surrogate half are illegal.

<a id="exampleValidRune"></a>
Example:

    valid := 'a'
    invalid := rune(0xfffffff)

    fmt.Println(utf8.ValidRune(valid))
    fmt.Println(utf8.ValidRune(invalid))
    // Output:
    // true
    // false

<h2 id="ValidString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf8/utf8.go#L470">ValidString</a>
    <a href="#ValidString">¶</a></h2>
<pre>func ValidString(s <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

ValidString reports whether s consists entirely of valid UTF-8-encoded runes.

<a id="exampleValidString"></a>
Example:

    valid := "Hello, 世界"
    invalid := string([]byte{0xff, 0xfe, 0xfd})

    fmt.Println(utf8.ValidString(valid))
    fmt.Println(utf8.ValidString(invalid))
    // Output:
    // true
    // false


