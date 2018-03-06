version: 1.10
## package jpeg

  `import "image/jpeg"`

## Overview

Package jpeg implements a JPEG image decoder and encoder.

JPEG is defined in ITU-T T.81: http://www.w3.org/Graphics/JPEG/itu-t81.pdf.

## Index

- [Constants](#pkg-constants)
- [func Decode(r io.Reader) (image.Image, error)](#Decode)
- [func DecodeConfig(r io.Reader) (image.Config, error)](#DecodeConfig)
- [func Encode(w io.Writer, m image.Image, o *Options) error](#Encode)
- [type FormatError](#FormatError)
  - [func (e FormatError) Error() string](#FormatError.Error)
- [type Options](#Options)
- [type Reader](#Reader)
- [type UnsupportedError](#UnsupportedError)
  - [func (e UnsupportedError) Error() string](#UnsupportedError.Error)

### Package files
 [fdct.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/fdct.go) [huffman.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/huffman.go) [idct.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/idct.go) [reader.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/reader.go) [scan.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/scan.go) [writer.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/writer.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="DefaultQuality">DefaultQuality</span> = 75</pre>

DefaultQuality is the default quality encoding parameter.

<h2 id="Decode">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/reader.go#L766">Decode</a>
    <a href="#Decode">¶</a></h2>
<pre>func Decode(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/image/">image</a>.<a href="/image/#Image">Image</a>, <a href="/builtin/#error">error</a>)</pre>

Decode reads a JPEG image from r and returns it as an image.Image.

<h2 id="DecodeConfig">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/reader.go#L773">DecodeConfig</a>
    <a href="#DecodeConfig">¶</a></h2>
<pre>func DecodeConfig(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/image/">image</a>.<a href="/image/#Config">Config</a>, <a href="/builtin/#error">error</a>)</pre>

DecodeConfig returns the color model and dimensions of a JPEG image without
decoding the entire image.

<h2 id="Encode">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/writer.go#L565">Encode</a>
    <a href="#Encode">¶</a></h2>
<pre>func Encode(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, m <a href="/image/">image</a>.<a href="/image/#Image">Image</a>, o *<a href="#Options">Options</a>) <a href="/builtin/#error">error</a></pre>

Encode writes the Image m to w in JPEG 4:2:0 baseline format with the given
options. Default parameters are used if a nil *Options is passed.

<h2 id="FormatError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/reader.go#L11">FormatError</a>
    <a href="#FormatError">¶</a></h2>
<pre>type FormatError <a href="/builtin/#string">string</a></pre>

A FormatError reports that the input is not a valid JPEG.

<h3 id="FormatError.Error">func (FormatError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/reader.go#L13">Error</a>
    <a href="#FormatError.Error">¶</a></h3>
<pre>func (e <a href="#FormatError">FormatError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Options">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/writer.go#L559">Options</a>
    <a href="#Options">¶</a></h2>
<pre>type Options struct {
<span id="Options.Quality"></span>    Quality <a href="/builtin/#int">int</a>
}</pre>

Options are the encoding parameters. Quality ranges from 1 to 100 inclusive,
higher is better.

<h2 id="Reader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/reader.go#L83">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader interface {
    <a href="/io/">io</a>.<a href="/io/#ByteReader">ByteReader</a>
    <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>
}</pre>

Deprecated: Reader is deprecated.

<h2 id="UnsupportedError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/reader.go#L16">UnsupportedError</a>
    <a href="#UnsupportedError">¶</a></h2>
<pre>type UnsupportedError <a href="/builtin/#string">string</a></pre>

An UnsupportedError reports that the input uses a valid but unimplemented JPEG
feature.

<h3 id="UnsupportedError.Error">func (UnsupportedError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/jpeg/reader.go#L18">Error</a>
    <a href="#UnsupportedError.Error">¶</a></h3>
<pre>func (e <a href="#UnsupportedError">UnsupportedError</a>) Error() <a href="/builtin/#string">string</a></pre>



