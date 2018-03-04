version: 1.10
## package hex

  `import "encoding/hex"`

## Overview

Package hex implements hexadecimal encoding and decoding.

## Index

- [Variables](#pkg-variables)
- [func Decode(dst, src []byte) (int, error)](#Decode)
- [func DecodeString(s string) ([]byte, error)](#DecodeString)
- [func DecodedLen(x int) int](#DecodedLen)
- [func Dump(data []byte) string](#Dump)
- [func Dumper(w io.Writer) io.WriteCloser](#Dumper)
- [func Encode(dst, src []byte) int](#Encode)
- [func EncodeToString(src []byte) string](#EncodeToString)
- [func EncodedLen(n int) int](#EncodedLen)
- [func NewDecoder(r io.Reader) io.Reader](#NewDecoder)
- [func NewEncoder(w io.Writer) io.Writer](#NewEncoder)
- [type InvalidByteError](#InvalidByteError)
  - [func (e InvalidByteError) Error() string](#InvalidByteError.Error)

### Examples

- [Decode](#exampleDecode)
- [DecodeString](#exampleDecodeString)
- [Dump](#exampleDump)
- [Dumper](#exampleDumper)
- [Encode](#exampleEncode)
- [EncodeToString](#exampleEncodeToString)

### Package files
 [hex.go](//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrLength">ErrLength</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;encoding/hex: odd length hex string&#34;)</pre>

ErrLength reports an attempt to decode an odd-length input using Decode or
DecodeString. The stream-based Decoder returns io.ErrUnexpectedEOF instead of
ErrLength.

<h2 id="Decode">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L47">Decode</a>
    <a href="#Decode">¶</a></h2>
<pre>func Decode(dst, src []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Decode decodes src into DecodedLen(len(src)) bytes, returning the actual number
of bytes written to dst.

Decode expects that src contain only hexadecimal characters and that src should
have an even length. If the input is malformed, Decode returns the number of
bytes decoded before the error.

<a id="exampleDecode"></a>
Example:

    src := []byte("48656c6c6f20476f7068657221")

    dst := make([]byte, hex.DecodedLen(len(src)))
    n, err := hex.Decode(dst, src)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s\n", dst[:n])

    // Output:
    // Hello Gopher!

<h2 id="DecodeString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L98">DecodeString</a>
    <a href="#DecodeString">¶</a></h2>
<pre>func DecodeString(s <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

DecodeString returns the bytes represented by the hexadecimal string s.

DecodeString expects that src contain only hexadecimal characters and that src
should have an even length. If the input is malformed, DecodeString returns a
string containing the bytes decoded before the error.

<a id="exampleDecodeString"></a>
Example:

    const s = "48656c6c6f20476f7068657221"
    decoded, err := hex.DecodeString(s)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s\n", decoded)

    // Output:
    // Hello Gopher!

<h2 id="DecodedLen">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L38">DecodedLen</a>
    <a href="#DecodedLen">¶</a></h2>
<pre>func DecodedLen(x <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

DecodedLen returns the length of a decoding of x source bytes. Specifically, it
returns x / 2.

<h2 id="Dump">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L108">Dump</a>
    <a href="#Dump">¶</a></h2>
<pre>func Dump(data []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#string">string</a></pre>

Dump returns a string that contains a hex dump of the given data. The format of
the hex dump matches the output of `hexdump -C` on the command line.

<a id="exampleDump"></a>
Example:

    content := []byte("Go is an open source programming language.")

    fmt.Printf("%s", hex.Dump(content))

    // Output:
    // 00000000  47 6f 20 69 73 20 61 6e  20 6f 70 65 6e 20 73 6f  |Go is an open so|
    // 00000010  75 72 63 65 20 70 72 6f  67 72 61 6d 6d 69 6e 67  |urce programming|
    // 00000020  20 6c 61 6e 67 75 61 67  65 2e                    | language.|

<h2 id="Dumper">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L194">Dumper</a>
    <a href="#Dumper">¶</a></h2>
<pre>func Dumper(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/io/">io</a>.<a href="/io/#WriteCloser">WriteCloser</a></pre>

Dumper returns a WriteCloser that writes a hex dump of all written data to w.
The format of the dump matches the output of `hexdump -C` on the command line.

<a id="exampleDumper"></a>
Example:

    lines := []string{
        "Go is an open source programming language.",
        "\n",
        "We encourage all Go users to subscribe to golang-announce.",
    }

    stdoutDumper := hex.Dumper(os.Stdout)

    defer stdoutDumper.Close()

    for _, line := range lines {
        stdoutDumper.Write([]byte(line))
    }

    // Output:
    // 00000000  47 6f 20 69 73 20 61 6e  20 6f 70 65 6e 20 73 6f  |Go is an open so|
    // 00000010  75 72 63 65 20 70 72 6f  67 72 61 6d 6d 69 6e 67  |urce programming|
    // 00000020  20 6c 61 6e 67 75 61 67  65 2e 0a 57 65 20 65 6e  | language..We en|
    // 00000030  63 6f 75 72 61 67 65 20  61 6c 6c 20 47 6f 20 75  |courage all Go u|
    // 00000040  73 65 72 73 20 74 6f 20  73 75 62 73 63 72 69 62  |sers to subscrib|
    // 00000050  65 20 74 6f 20 67 6f 6c  61 6e 67 2d 61 6e 6e 6f  |e to golang-anno|
    // 00000060  75 6e 63 65 2e                                    |unce.|

<h2 id="Encode">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L15">Encode</a>
    <a href="#Encode">¶</a></h2>
<pre>func Encode(dst, src []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

Encode encodes src into EncodedLen(len(src)) bytes of dst. As a convenience, it
returns the number of bytes written to dst, but this value is always
EncodedLen(len(src)). Encode implements hexadecimal encoding.

<a id="exampleEncode"></a>
Example:

    src := []byte("Hello Gopher!")

    dst := make([]byte, hex.EncodedLen(len(src)))
    hex.Encode(dst, src)

    fmt.Printf("%s\n", dst)

    // Output:
    // 48656c6c6f20476f7068657221

<h2 id="EncodeToString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L86">EncodeToString</a>
    <a href="#EncodeToString">¶</a></h2>
<pre>func EncodeToString(src []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#string">string</a></pre>

EncodeToString returns the hexadecimal encoding of src.

<a id="exampleEncodeToString"></a>
Example:

    src := []byte("Hello")
    encodedStr := hex.EncodeToString(src)

    fmt.Printf("%s\n", encodedStr)

    // Output:
    // 48656c6c6f

<h2 id="EncodedLen">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L9">EncodedLen</a>
    <a href="#EncodedLen">¶</a></h2>
<pre>func EncodedLen(n <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

EncodedLen returns the length of an encoding of n source bytes. Specifically, it
returns n * 2.

<h2 id="NewDecoder">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L155">NewDecoder</a>
    <a href="#NewDecoder">¶</a></h2>
<pre>func NewDecoder(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

NewDecoder returns an io.Reader that decodes hexadecimal characters from r.
NewDecoder expects that r contain only an even number of hexadecimal characters.

<h2 id="NewEncoder">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L126">NewEncoder</a>
    <a href="#NewEncoder">¶</a></h2>
<pre>func NewEncoder(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a></pre>

NewEncoder returns an io.Writer that writes lowercase hexadecimal characters to
w.

<h2 id="InvalidByteError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L30">InvalidByteError</a>
    <a href="#InvalidByteError">¶</a></h2>
<pre>type InvalidByteError <a href="/builtin/#byte">byte</a></pre>

InvalidByteError values describe errors resulting from an invalid byte in a hex
string.

<h3 id="InvalidByteError.Error">func (InvalidByteError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/hex/hex.go#L32">Error</a>
    <a href="#InvalidByteError.Error">¶</a></h3>
<pre>func (e <a href="#InvalidByteError">InvalidByteError</a>) Error() <a href="/builtin/#string">string</a></pre>



