version: 1.10
## package color

  `import "image/color"`

## Overview

Package color implements a basic color library.

## Index

- [Variables](#pkg-variables)
- [func CMYKToRGB(c, m, y, k uint8) (uint8, uint8, uint8)](#CMYKToRGB)
- [func RGBToCMYK(r, g, b uint8) (uint8, uint8, uint8, uint8)](#RGBToCMYK)
- [func RGBToYCbCr(r, g, b uint8) (uint8, uint8, uint8)](#RGBToYCbCr)
- [func YCbCrToRGB(y, cb, cr uint8) (uint8, uint8, uint8)](#YCbCrToRGB)
- [type Alpha](#Alpha)
  - [func (c Alpha) RGBA() (r, g, b, a uint32)](#Alpha.RGBA)
- [type Alpha16](#Alpha16)
  - [func (c Alpha16) RGBA() (r, g, b, a uint32)](#Alpha16.RGBA)
- [type CMYK](#CMYK)
  - [func (c CMYK) RGBA() (uint32, uint32, uint32, uint32)](#CMYK.RGBA)
- [type Color](#Color)
- [type Gray](#Gray)
  - [func (c Gray) RGBA() (r, g, b, a uint32)](#Gray.RGBA)
- [type Gray16](#Gray16)
  - [func (c Gray16) RGBA() (r, g, b, a uint32)](#Gray16.RGBA)
- [type Model](#Model)
  - [func ModelFunc(f func(Color) Color) Model](#ModelFunc)
- [type NRGBA](#NRGBA)
  - [func (c NRGBA) RGBA() (r, g, b, a uint32)](#NRGBA.RGBA)
- [type NRGBA64](#NRGBA64)
  - [func (c NRGBA64) RGBA() (r, g, b, a uint32)](#NRGBA64.RGBA)
- [type NYCbCrA](#NYCbCrA)
  - [func (c NYCbCrA) RGBA() (uint32, uint32, uint32, uint32)](#NYCbCrA.RGBA)
- [type Palette](#Palette)
  - [func (p Palette) Convert(c Color) Color](#Palette.Convert)
  - [func (p Palette) Index(c Color) int](#Palette.Index)
- [type RGBA](#RGBA)
  - [func (c RGBA) RGBA() (r, g, b, a uint32)](#RGBA.RGBA)
- [type RGBA64](#RGBA64)
  - [func (c RGBA64) RGBA() (r, g, b, a uint32)](#RGBA64.RGBA)
- [type YCbCr](#YCbCr)
  - [func (c YCbCr) RGBA() (uint32, uint32, uint32, uint32)](#YCbCr.RGBA)

### Package files
 [color.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go) [ycbcr.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="Black">Black</span>       = <a href="#Gray16">Gray16</a>{0}
    <span id="White">White</span>       = <a href="#Gray16">Gray16</a>{0xffff}
    <span id="Transparent">Transparent</span> = <a href="#Alpha16">Alpha16</a>{0}
    <span id="Opaque">Opaque</span>      = <a href="#Alpha16">Alpha16</a>{0xffff}
)</pre>

Standard colors.

<h2 id="CMYKToRGB">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go#L326">CMYKToRGB</a>
    <a href="#CMYKToRGB">¶</a></h2>
<pre>func CMYKToRGB(c, m, y, k <a href="/builtin/#uint8">uint8</a>) (<a href="/builtin/#uint8">uint8</a>, <a href="/builtin/#uint8">uint8</a>, <a href="/builtin/#uint8">uint8</a>)</pre>

CMYKToRGB converts a CMYK quadruple to an RGB triple.

<h2 id="RGBToCMYK">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go#L305">RGBToCMYK</a>
    <a href="#RGBToCMYK">¶</a></h2>
<pre>func RGBToCMYK(r, g, b <a href="/builtin/#uint8">uint8</a>) (<a href="/builtin/#uint8">uint8</a>, <a href="/builtin/#uint8">uint8</a>, <a href="/builtin/#uint8">uint8</a>, <a href="/builtin/#uint8">uint8</a>)</pre>

RGBToCMYK converts an RGB triple to a CMYK quadruple.

<h2 id="RGBToYCbCr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go#L1">RGBToYCbCr</a>
    <a href="#RGBToYCbCr">¶</a></h2>
<pre>func RGBToYCbCr(r, g, b <a href="/builtin/#uint8">uint8</a>) (<a href="/builtin/#uint8">uint8</a>, <a href="/builtin/#uint8">uint8</a>, <a href="/builtin/#uint8">uint8</a>)</pre>

RGBToYCbCr converts an RGB triple to a Y'CbCr triple.

<h2 id="YCbCrToRGB">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go#L48">YCbCrToRGB</a>
    <a href="#YCbCrToRGB">¶</a></h2>
<pre>func YCbCrToRGB(y, cb, cr <a href="/builtin/#uint8">uint8</a>) (<a href="/builtin/#uint8">uint8</a>, <a href="/builtin/#uint8">uint8</a>, <a href="/builtin/#uint8">uint8</a>)</pre>

YCbCrToRGB converts a Y'CbCr triple to an RGB triple.

<h2 id="Alpha">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L89">Alpha</a>
    <a href="#Alpha">¶</a></h2>
<pre>type Alpha struct {
<span id="Alpha.A"></span>    A <a href="/builtin/#uint8">uint8</a>
}</pre>

Alpha represents an 8-bit alpha color.

<h3 id="Alpha.RGBA">func (Alpha) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L93">RGBA</a>
    <a href="#Alpha.RGBA">¶</a></h3>
<pre>func (c <a href="#Alpha">Alpha</a>) RGBA() (r, g, b, a <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="Alpha16">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L100">Alpha16</a>
    <a href="#Alpha16">¶</a></h2>
<pre>type Alpha16 struct {
<span id="Alpha16.A"></span>    A <a href="/builtin/#uint16">uint16</a>
}</pre>

Alpha16 represents a 16-bit alpha color.

<h3 id="Alpha16.RGBA">func (Alpha16) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L104">RGBA</a>
    <a href="#Alpha16.RGBA">¶</a></h3>
<pre>func (c <a href="#Alpha16">Alpha16</a>) RGBA() (r, g, b, a <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="CMYK">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go#L338">CMYK</a>
    <a href="#CMYK">¶</a></h2>
<pre>type CMYK struct {
<span id="CMYK.C"></span>    C, M, Y, K <a href="/builtin/#uint8">uint8</a>
}</pre>

CMYK represents a fully opaque CMYK color, having 8 bits for each of cyan,
magenta, yellow and black.

It is not associated with any particular color profile.

<h3 id="CMYK.RGBA">func (CMYK) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go#L342">RGBA</a>
    <a href="#CMYK.RGBA">¶</a></h3>
<pre>func (c <a href="#CMYK">CMYK</a>) RGBA() (<a href="/builtin/#uint32">uint32</a>, <a href="/builtin/#uint32">uint32</a>, <a href="/builtin/#uint32">uint32</a>, <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="Color">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L1">Color</a>
    <a href="#Color">¶</a></h2>
<pre>type Color interface {
    <span class="comment">// RGBA returns the alpha-premultiplied red, green, blue and alpha values</span>
    <span class="comment">// for the color. Each value ranges within [0, 0xffff], but is represented</span>
    <span class="comment">// by a uint32 so that multiplying by a blend factor up to 0xffff will not</span>
    <span class="comment">// overflow.</span>
    <span class="comment">//</span>
    <span class="comment">// An alpha-premultiplied color component c has been scaled by alpha (a),</span>
    <span class="comment">// so has valid values 0 &lt;= c &lt;= a.</span>
    RGBA() (r, g, b, a <a href="/builtin/#uint32">uint32</a>)
}</pre>

Color can convert itself to alpha-premultiplied 16-bits per channel RGBA. The
conversion may be lossy.

<h2 id="Gray">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L110">Gray</a>
    <a href="#Gray">¶</a></h2>
<pre>type Gray struct {
<span id="Gray.Y"></span>    Y <a href="/builtin/#uint8">uint8</a>
}</pre>

Gray represents an 8-bit grayscale color.

<h3 id="Gray.RGBA">func (Gray) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L114">RGBA</a>
    <a href="#Gray.RGBA">¶</a></h3>
<pre>func (c <a href="#Gray">Gray</a>) RGBA() (r, g, b, a <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="Gray16">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L121">Gray16</a>
    <a href="#Gray16">¶</a></h2>
<pre>type Gray16 struct {
<span id="Gray16.Y"></span>    Y <a href="/builtin/#uint16">uint16</a>
}</pre>

Gray16 represents a 16-bit grayscale color.

<h3 id="Gray16.RGBA">func (Gray16) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L125">RGBA</a>
    <a href="#Gray16.RGBA">¶</a></h3>
<pre>func (c <a href="#Gray16">Gray16</a>) RGBA() (r, g, b, a <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="Model">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L132">Model</a>
    <a href="#Model">¶</a></h2>
<pre>type Model interface {
    Convert(c <a href="#Color">Color</a>) <a href="#Color">Color</a>
}</pre>

Model can convert any Color to one from its own color model. The conversion may
be lossy.

<pre>var (
    <span id="RGBAModel">RGBAModel</span>    <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(rgbaModel)
    <span id="RGBA64Model">RGBA64Model</span>  <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(rgba64Model)
    <span id="NRGBAModel">NRGBAModel</span>   <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(nrgbaModel)
    <span id="NRGBA64Model">NRGBA64Model</span> <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(nrgba64Model)
    <span id="AlphaModel">AlphaModel</span>   <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(alphaModel)
    <span id="Alpha16Model">Alpha16Model</span> <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(alpha16Model)
    <span id="GrayModel">GrayModel</span>    <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(grayModel)
    <span id="Gray16Model">Gray16Model</span>  <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(gray16Model)
)</pre>

Models for the standard color types.

<pre>var <span id="CMYKModel">CMYKModel</span> <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(cmykModel)</pre>

CMYKModel is the Model for CMYK colors.

<pre>var <span id="NYCbCrAModel">NYCbCrAModel</span> <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(nYCbCrAModel)</pre>

NYCbCrAModel is the Model for non-alpha-premultiplied Y'CbCr-with-alpha colors.

<pre>var <span id="YCbCrModel">YCbCrModel</span> <a href="#Model">Model</a> = <a href="#ModelFunc">ModelFunc</a>(yCbCrModel)</pre>

YCbCrModel is the Model for Y'CbCr colors.

<h3 id="ModelFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L137">ModelFunc</a>
    <a href="#ModelFunc">¶</a></h3>
<pre>func ModelFunc(f func(<a href="#Color">Color</a>) <a href="#Color">Color</a>) <a href="#Model">Model</a></pre>

ModelFunc returns a Model that invokes f to implement the conversion.

<h2 id="NRGBA">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L46">NRGBA</a>
    <a href="#NRGBA">¶</a></h2>
<pre>type NRGBA struct {
<span id="NRGBA.R"></span>    R, G, B, A <a href="/builtin/#uint8">uint8</a>
}</pre>

NRGBA represents a non-alpha-premultiplied 32-bit color.

<h3 id="NRGBA.RGBA">func (NRGBA) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L50">RGBA</a>
    <a href="#NRGBA.RGBA">¶</a></h3>
<pre>func (c <a href="#NRGBA">NRGBA</a>) RGBA() (r, g, b, a <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="NRGBA64">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L70">NRGBA64</a>
    <a href="#NRGBA64">¶</a></h2>
<pre>type NRGBA64 struct {
<span id="NRGBA64.R"></span>    R, G, B, A <a href="/builtin/#uint16">uint16</a>
}</pre>

NRGBA64 represents a non-alpha-premultiplied 64-bit color, having 16 bits for
each of red, green, blue and alpha.

<h3 id="NRGBA64.RGBA">func (NRGBA64) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L74">RGBA</a>
    <a href="#NRGBA64.RGBA">¶</a></h3>
<pre>func (c <a href="#NRGBA64">NRGBA64</a>) RGBA() (r, g, b, a <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="NYCbCrA">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go#L232">NYCbCrA</a>
    <a href="#NYCbCrA">¶</a></h2>
<pre>type NYCbCrA struct {
    <a href="#YCbCr">YCbCr</a>
<span id="NYCbCrA.A"></span>    A <a href="/builtin/#uint8">uint8</a>
}</pre>

NYCbCrA represents a non-alpha-premultiplied Y'CbCr-with-alpha color, having 8
bits each for one luma, two chroma and one alpha component.

<h3 id="NYCbCrA.RGBA">func (NYCbCrA) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go#L237">RGBA</a>
    <a href="#NYCbCrA.RGBA">¶</a></h3>
<pre>func (c <a href="#NYCbCrA">NYCbCrA</a>) RGBA() (<a href="/builtin/#uint32">uint32</a>, <a href="/builtin/#uint32">uint32</a>, <a href="/builtin/#uint32">uint32</a>, <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="Palette">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L270">Palette</a>
    <a href="#Palette">¶</a></h2>
<pre>type Palette []<a href="#Color">Color</a></pre>

Palette is a palette of colors.

<h3 id="Palette.Convert">func (Palette) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L273">Convert</a>
    <a href="#Palette.Convert">¶</a></h3>
<pre>func (p <a href="#Palette">Palette</a>) Convert(c <a href="#Color">Color</a>) <a href="#Color">Color</a></pre>

Convert returns the palette color closest to c in Euclidean R,G,B space.

<h3 id="Palette.Index">func (Palette) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L282">Index</a>
    <a href="#Palette.Index">¶</a></h3>
<pre>func (p <a href="#Palette">Palette</a>) Index(c <a href="#Color">Color</a>) <a href="/builtin/#int">int</a></pre>

Index returns the index of the palette color closest to c in Euclidean R,G,B,A
space.

<h2 id="RGBA">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L16">RGBA</a>
    <a href="#RGBA">¶</a></h2>
<pre>type RGBA struct {
<span id="RGBA.R"></span>    R, G, B, A <a href="/builtin/#uint8">uint8</a>
}</pre>

RGBA represents a traditional 32-bit alpha-premultiplied color, having 8 bits
for each of red, green, blue and alpha.

An alpha-premultiplied color component C has been scaled by alpha (A), so has
valid values 0 <= C <= A.

<h3 id="RGBA.RGBA">func (RGBA) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L20">RGBA</a>
    <a href="#RGBA.RGBA">¶</a></h3>
<pre>func (c <a href="#RGBA">RGBA</a>) RGBA() (r, g, b, a <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="RGBA64">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L37">RGBA64</a>
    <a href="#RGBA64">¶</a></h2>
<pre>type RGBA64 struct {
<span id="RGBA64.R"></span>    R, G, B, A <a href="/builtin/#uint16">uint16</a>
}</pre>

RGBA64 represents a 64-bit alpha-premultiplied color, having 16 bits for each of
red, green, blue and alpha.

An alpha-premultiplied color component C has been scaled by alpha (A), so has
valid values 0 <= C <= A.

<h3 id="RGBA64.RGBA">func (RGBA64) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/color.go#L41">RGBA</a>
    <a href="#RGBA64.RGBA">¶</a></h3>
<pre>func (c <a href="#RGBA64">RGBA64</a>) RGBA() (r, g, b, a <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="YCbCr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go#L157">YCbCr</a>
    <a href="#YCbCr">¶</a></h2>
<pre>type YCbCr struct {
<span id="YCbCr.Y"></span>    Y, Cb, Cr <a href="/builtin/#uint8">uint8</a>
}</pre>

YCbCr represents a fully opaque 24-bit Y'CbCr color, having 8 bits each for one
luma and two chroma components.

JPEG, VP8, the MPEG family and other codecs use this color model. Such codecs
often use the terms YUV and Y'CbCr interchangeably, but strictly speaking, the
term YUV applies only to analog video signals, and Y' (luma) is Y (luminance)
after applying gamma correction.

Conversion between RGB and Y'CbCr is lossy and there are multiple, slightly
different formulae for converting between the two. This package follows the JFIF
specification at http://www.w3.org/Graphics/JPEG/jfif3.pdf.

<h3 id="YCbCr.RGBA">func (YCbCr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/color/ycbcr.go#L161">RGBA</a>
    <a href="#YCbCr.RGBA">¶</a></h3>
<pre>func (c <a href="#YCbCr">YCbCr</a>) RGBA() (<a href="/builtin/#uint32">uint32</a>, <a href="/builtin/#uint32">uint32</a>, <a href="/builtin/#uint32">uint32</a>, <a href="/builtin/#uint32">uint32</a>)</pre>


## Subdirectories
- [..](..)
- [palette](palette/)
