version: 1.9.2
## package gif

  `import "image/gif"`

## Overview

Package gif implements a GIF image decoder and encoder.

The GIF specification is at http://www.w3.org/Graphics/GIF/spec-gif89a.txt.

## Index

- [Constants](#pkg-constants)
- [func Decode(r io.Reader) (image.Image, error)](#Decode)
- [func DecodeConfig(r io.Reader) (image.Config, error)](#DecodeConfig)
- [func Encode(w io.Writer, m image.Image, o *Options) error](#Encode)
- [func EncodeAll(w io.Writer, g *GIF) error](#EncodeAll)
- [type GIF](#GIF)
  - [func DecodeAll(r io.Reader) (*GIF, error)](#DecodeAll)
- [type Options](#Options)

### Package files
 [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/gif/reader.go) [writer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/gif/writer.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="DisposalNone">DisposalNone</span>       = 0x01
    <span id="DisposalBackground">DisposalBackground</span> = 0x02
    <span id="DisposalPrevious">DisposalPrevious</span>   = 0x03
)</pre>

Disposal Methods.

<h2 id="Decode">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/gif/reader.go#L482">Decode</a>
    <a href="#Decode">¶</a></h2>
<pre>func Decode(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/image/">image</a>.<a href="/image/#Image">Image</a>, <a href="/builtin/#error">error</a>)</pre>

Decode reads a GIF image from r and returns the first embedded image as an
image.Image.

<h2 id="DecodeConfig">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/gif/reader.go#L539">DecodeConfig</a>
    <a href="#DecodeConfig">¶</a></h2>
<pre>func DecodeConfig(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/image/">image</a>.<a href="/image/#Config">Config</a>, <a href="/builtin/#error">error</a>)</pre>

DecodeConfig returns the global color model and dimensions of a GIF image
without decoding the entire image.

<h2 id="Encode">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/gif/writer.go#L351">Encode</a>
    <a href="#Encode">¶</a></h2>
<pre>func Encode(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, m <a href="/image/">image</a>.<a href="/image/#Image">Image</a>, o *<a href="#Options">Options</a>) <a href="/builtin/#error">error</a></pre>

Encode writes the Image m to w in GIF format.

<h2 id="EncodeAll">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/gif/writer.go#L302">EncodeAll</a>
    <a href="#EncodeAll">¶</a></h2>
<pre>func EncodeAll(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, g *<a href="#GIF">GIF</a>) <a href="/builtin/#error">error</a></pre>

EncodeAll writes the images in g to w in GIF format with the given loop count
and delay between frames.

<h2 id="GIF">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/gif/reader.go#L491">GIF</a>
    <a href="#GIF">¶</a></h2>
<pre>type GIF struct {
<span id="GIF.Image"></span>    Image     []*<a href="/image/">image</a>.<a href="/image/#Paletted">Paletted</a> <span class="comment">// The successive images.</span>
<span id="GIF.Delay"></span>    Delay     []<a href="/builtin/#int">int</a>             <span class="comment">// The successive delay times, one per frame, in 100ths of a second.</span>
<span id="GIF.LoopCount"></span>    LoopCount <a href="/builtin/#int">int</a>               <span class="comment">// The loop count.</span>
<span id="GIF.Disposal"></span>    <span class="comment">// Disposal is the successive disposal methods, one per frame. For</span>
    <span class="comment">// backwards compatibility, a nil Disposal is valid to pass to EncodeAll,</span>
    <span class="comment">// and implies that each frame&#39;s disposal method is 0 (no disposal</span>
    <span class="comment">// specified).</span>
    Disposal []<a href="/builtin/#byte">byte</a>
<span id="GIF.Config"></span>    <span class="comment">// Config is the global color table (palette), width and height. A nil or</span>
    <span class="comment">// empty-color.Palette Config.ColorModel means that each frame has its own</span>
    <span class="comment">// color table and there is no global color table. Each frame&#39;s bounds must</span>
    <span class="comment">// be within the rectangle defined by the two points (0, 0) and</span>
    <span class="comment">// (Config.Width, Config.Height).</span>
    <span class="comment">//</span>
    <span class="comment">// For backwards compatibility, a zero-valued Config is valid to pass to</span>
    <span class="comment">// EncodeAll, and implies that the overall GIF&#39;s width and height equals</span>
    <span class="comment">// the first frame&#39;s bounds&#39; Rectangle.Max point.</span>
    Config <a href="/image/">image</a>.<a href="/image/#Config">Config</a>
<span id="GIF.BackgroundIndex"></span>    <span class="comment">// BackgroundIndex is the background index in the global color table, for</span>
    <span class="comment">// use with the DisposalBackground disposal method.</span>
    BackgroundIndex <a href="/builtin/#byte">byte</a>
}</pre>

GIF represents the possibly multiple images stored in a GIF file.

<h3 id="DecodeAll">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/gif/reader.go#L517">DecodeAll</a>
    <a href="#DecodeAll">¶</a></h3>
<pre>func DecodeAll(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (*<a href="#GIF">GIF</a>, <a href="/builtin/#error">error</a>)</pre>

DecodeAll reads a GIF image from r and returns the sequential frames and timing
information.

<h2 id="Options">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/gif/writer.go#L286">Options</a>
    <a href="#Options">¶</a></h2>
<pre>type Options struct {
<span id="Options.NumColors"></span>    <span class="comment">// NumColors is the maximum number of colors used in the image.</span>
    <span class="comment">// It ranges from 1 to 256.</span>
    NumColors <a href="/builtin/#int">int</a>

<span id="Options.Quantizer"></span>    <span class="comment">// Quantizer is used to produce a palette with size NumColors.</span>
    <span class="comment">// palette.Plan9 is used in place of a nil Quantizer.</span>
    Quantizer <a href="/image/draw/">draw</a>.<a href="/image/draw/#Quantizer">Quantizer</a>

<span id="Options.Drawer"></span>    <span class="comment">// Drawer is used to convert the source image to the desired palette.</span>
    <span class="comment">// draw.FloydSteinberg is used in place of a nil Drawer.</span>
    Drawer <a href="/image/draw/">draw</a>.<a href="/image/draw/#Drawer">Drawer</a>
}</pre>

Options are the encoding parameters.


