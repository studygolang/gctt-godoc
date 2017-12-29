version: 1.9.2
## package base64

  `import "encoding/base64"`

## Overview

Package base64 implements base64 encoding as specified by RFC 4648.

<a id="example_"></a>
Example:

    msg := "Hello, 世界"
    encoded := base64.StdEncoding.EncodeToString([]byte(msg))
    fmt.Println(encoded)
    decoded, err := base64.StdEncoding.DecodeString(encoded)
    if err != nil {
        fmt.Println("decode error:", err)
        return
    }
    fmt.Println(string(decoded))
    // Output:
    // SGVsbG8sIOS4lueVjA==
    // Hello, 世界

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func NewDecoder(enc *Encoding, r io.Reader) io.Reader](#NewDecoder)
- [func NewEncoder(enc *Encoding, w io.Writer) io.WriteCloser](#NewEncoder)
- [type CorruptInputError](#CorruptInputError)
  - [func (e CorruptInputError) Error() string](#CorruptInputError.Error)
- [type Encoding](#Encoding)
  - [func NewEncoding(encoder string) *Encoding](#NewEncoding)
  - [func (enc *Encoding) Decode(dst, src []byte) (n int, err error)](#Encoding.Decode)
  - [func (enc *Encoding) DecodeString(s string) ([]byte, error)](#Encoding.DecodeString)
  - [func (enc *Encoding) DecodedLen(n int) int](#Encoding.DecodedLen)
  - [func (enc *Encoding) Encode(dst, src []byte)](#Encoding.Encode)
  - [func (enc *Encoding) EncodeToString(src []byte) string](#Encoding.EncodeToString)
  - [func (enc *Encoding) EncodedLen(n int) int](#Encoding.EncodedLen)
  - [func (enc Encoding) Strict() *Encoding](#Encoding.Strict)
  - [func (enc Encoding) WithPadding(padding rune) *Encoding](#Encoding.WithPadding)

### Examples

- [Package](#example_)
- [Encoding.DecodeString](#example_Encoding_DecodeString)
- [Encoding.EncodeToString](#example_Encoding_EncodeToString)
- [NewEncoder](#example_NewEncoder)

### Package files
 [base64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="StdPadding">StdPadding</span> <a href="/builtin/#rune">rune</a> = &#39;=&#39; <span class="comment">// Standard padding character</span>
    <span id="NoPadding">NoPadding</span>  <a href="/builtin/#rune">rune</a> = -1  <span class="comment">// No padding</span>
)</pre>


<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="RawStdEncoding">RawStdEncoding</span> = <a href="#StdEncoding">StdEncoding</a>.<a href="#WithPadding">WithPadding</a>(<a href="#NoPadding">NoPadding</a>)</pre>

RawStdEncoding is the standard raw, unpadded base64 encoding, as defined in RFC
4648 section 3.2. This is the same as StdEncoding but omits padding characters.

<pre>var <span id="RawURLEncoding">RawURLEncoding</span> = <a href="#URLEncoding">URLEncoding</a>.<a href="#WithPadding">WithPadding</a>(<a href="#NoPadding">NoPadding</a>)</pre>

RawURLEncoding is the unpadded alternate base64 encoding defined in RFC 4648. It
is typically used in URLs and file names. This is the same as URLEncoding but
omits padding characters.

<pre>var <span id="StdEncoding">StdEncoding</span> = <a href="#NewEncoding">NewEncoding</a>(encodeStd)</pre>

StdEncoding is the standard base64 encoding, as defined in RFC 4648.

<pre>var <span id="URLEncoding">URLEncoding</span> = <a href="#NewEncoding">NewEncoding</a>(encodeURL)</pre>

URLEncoding is the alternate base64 encoding defined in RFC 4648. It is
typically used in URLs and file names.

<h2 id="NewDecoder">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L485">NewDecoder</a>
    <a href="#NewDecoder">¶</a></h2>
<pre>func NewDecoder(enc *<a href="#Encoding">Encoding</a>, r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

NewDecoder constructs a new base64 stream decoder.

<h2 id="NewEncoder">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L239">NewEncoder</a>
    <a href="#NewEncoder">¶</a></h2>
<pre>func NewEncoder(enc *<a href="#Encoding">Encoding</a>, w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/io/">io</a>.<a href="/io/#WriteCloser">WriteCloser</a></pre>

NewEncoder returns a new base64 stream encoder. Data written to the returned
writer will be encoded using enc and then written to w. Base64 encodings operate
in 4-byte blocks; when finished writing, the caller must Close the returned
encoder to flush any partially written blocks.

<a id="example_NewEncoder"></a>
Example:

    input := []byte("foo\x00bar")
    encoder := base64.NewEncoder(base64.StdEncoding, os.Stdout)
    encoder.Write(input)
    // Must close the encoder when finished to flush any partial blocks.
    // If you comment out the following line, the last partial block "r"
    // won't be encoded.
    encoder.Close()
    // Output:
    // Zm9vAGJhcg==

<h2 id="CorruptInputError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L256">CorruptInputError</a>
    <a href="#CorruptInputError">¶</a></h2>
<pre>type CorruptInputError <a href="/builtin/#int64">int64</a></pre>


<h3 id="CorruptInputError.Error">func (CorruptInputError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L258">Error</a>
    <a href="#CorruptInputError.Error">¶</a></h3>
<pre>func (e <a href="#CorruptInputError">CorruptInputError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Encoding">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L12">Encoding</a>
    <a href="#Encoding">¶</a></h2>
<pre>type Encoding struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

An Encoding is a radix 64 encoding/decoding scheme, defined by a 64-character
alphabet. The most common encoding is the "base64" encoding defined in RFC 4648
and used in MIME (RFC 2045) and PEM (RFC 1421). RFC 4648 also defines an
alternate encoding, which is the standard encoding with - and _ substituted for
+ and /.

<h3 id="NewEncoding">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L32">NewEncoding</a>
    <a href="#NewEncoding">¶</a></h3>
<pre>func NewEncoding(encoder <a href="/builtin/#string">string</a>) *<a href="#Encoding">Encoding</a></pre>

NewEncoding returns a new padded Encoding defined by the given alphabet, which
must be a 64-byte string that does not contain the padding character or CR / LF
('\r', '\n'). The resulting Encoding uses the default padding character ('='),
which may be changed or disabled via WithPadding.

<h3 id="Encoding.Decode">func (*Encoding) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L368">Decode</a>
    <a href="#Encoding.Decode">¶</a></h3>
<pre>func (enc *<a href="#Encoding">Encoding</a>) Decode(dst, src []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Decode decodes src using the encoding enc. It writes at most
DecodedLen(len(src)) bytes to dst and returns the number of bytes written. If
src contains invalid base64 data, it will return the number of bytes
successfully written and CorruptInputError. New line characters (\r and \n) are
ignored.

<h3 id="Encoding.DecodeString">func (*Encoding) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L374">DecodeString</a>
    <a href="#Encoding.DecodeString">¶</a></h3>
<pre>func (enc *<a href="#Encoding">Encoding</a>) DecodeString(s <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

DecodeString returns the bytes represented by the base64 string s.

<a id="example_Encoding_DecodeString"></a>
Example:

    str := "c29tZSBkYXRhIHdpdGggACBhbmQg77u/"
    data, err := base64.StdEncoding.DecodeString(str)
    if err != nil {
        fmt.Println("error:", err)
        return
    }
    fmt.Printf("%q\n", data)
    // Output:
    // "some data with \x00 and \ufeff"

<h3 id="Encoding.DecodedLen">func (*Encoding) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L491">DecodedLen</a>
    <a href="#Encoding.DecodedLen">¶</a></h3>
<pre>func (enc *<a href="#Encoding">Encoding</a>) DecodedLen(n <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

DecodedLen returns the maximum length in bytes of the decoded data corresponding
to n bytes of base64-encoded data.

<h3 id="Encoding.Encode">func (*Encoding) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L111">Encode</a>
    <a href="#Encoding.Encode">¶</a></h3>
<pre>func (enc *<a href="#Encoding">Encoding</a>) Encode(dst, src []<a href="/builtin/#byte">byte</a>)</pre>

Encode encodes src using the encoding enc, writing EncodedLen(len(src)) bytes to
dst.

The encoding pads the output to a multiple of 4 bytes, so Encode is not
appropriate for use on individual blocks of a large data stream. Use
NewEncoder() instead.

<h3 id="Encoding.EncodeToString">func (*Encoding) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L159">EncodeToString</a>
    <a href="#Encoding.EncodeToString">¶</a></h3>
<pre>func (enc *<a href="#Encoding">Encoding</a>) EncodeToString(src []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#string">string</a></pre>

EncodeToString returns the base64 encoding of src.

<a id="example_Encoding_EncodeToString"></a>
Example:

    data := []byte("any + old & data")
    str := base64.StdEncoding.EncodeToString(data)
    fmt.Println(str)
    // Output:
    // YW55ICsgb2xkICYgZGF0YQ==

<h3 id="Encoding.EncodedLen">func (*Encoding) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L245">EncodedLen</a>
    <a href="#Encoding.EncodedLen">¶</a></h3>
<pre>func (enc *<a href="#Encoding">Encoding</a>) EncodedLen(n <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

EncodedLen returns the length in bytes of the base64 encoding of an input buffer
of length n.

<h3 id="Encoding.Strict">func (Encoding) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L78">Strict</a>
    <a href="#Encoding.Strict">¶</a></h3>
<pre>func (enc <a href="#Encoding">Encoding</a>) Strict() *<a href="#Encoding">Encoding</a></pre>

Strict creates a new encoding identical to enc except with strict decoding
enabled. In this mode, the decoder requires that trailing padding bits are zero,
as described in RFC 4648 section 3.5.

<h3 id="Encoding.WithPadding">func (Encoding) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/base64/base64.go#L60">WithPadding</a>
    <a href="#Encoding.WithPadding">¶</a></h3>
<pre>func (enc <a href="#Encoding">Encoding</a>) WithPadding(padding <a href="/builtin/#rune">rune</a>) *<a href="#Encoding">Encoding</a></pre>

WithPadding creates a new encoding identical to enc except with a specified
padding character, or NoPadding to disable padding. The padding character must
not be '\r' or '\n', must not be contained in the encoding's alphabet and must
be a rune equal or below '\xff'.


