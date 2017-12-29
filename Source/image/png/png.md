version: 1.9.2
## package png

  `import "image/png"`

## Overview

Package png implements a PNG image decoder and encoder.

The PNG specification is at http://www.w3.org/TR/PNG/.

## Index

- [func Decode(r io.Reader) (image.Image, error)](#Decode)
- [func DecodeConfig(r io.Reader) (image.Config, error)](#DecodeConfig)
- [func Encode(w io.Writer, m image.Image) error](#Encode)
- [type CompressionLevel](#CompressionLevel)
- [type Encoder](#Encoder)
  - [func (enc *Encoder) Encode(w io.Writer, m image.Image) error](#Encoder.Encode)
- [type EncoderBuffer](#EncoderBuffer)
- [type EncoderBufferPool](#EncoderBufferPool)
- [type FormatError](#FormatError)
  - [func (e FormatError) Error() string](#FormatError.Error)
- [type UnsupportedError](#UnsupportedError)
  - [func (e UnsupportedError) Error() string](#UnsupportedError.Error)

### Examples

- [Decode](#example_Decode)
- [Encode](#example_Encode)

### Package files
 [paeth.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/paeth.go) [reader.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/reader.go) [writer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/writer.go)

<h2 id="Decode">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/reader.go#L936">Decode</a>
    <a href="#Decode">¶</a></h2>
<pre>func Decode(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/image/">image</a>.<a href="/image/#Image">Image</a>, <a href="/builtin/#error">error</a>)</pre>

Decode reads a PNG image from r and returns it as an image.Image. The type of
Image returned depends on the PNG contents.

<a id="example_Decode"></a>
Example:

    // This example uses png.Decode which can only decode PNG images.
    // Consider using the general image.Decode as it can sniff and decode any registered image format.
    img, err := png.Decode(gopherPNG())
    if err != nil {
        log.Fatal(err)
    }

    levels := []string{" ", "░", "▒", "▓", "█"}

    for y := img.Bounds().Min.Y; y < img.Bounds().Max.Y; y++ {
        for x := img.Bounds().Min.X; x < img.Bounds().Max.X; x++ {
            c := color.GrayModel.Convert(img.At(x, y)).(color.Gray)
            level := c.Y / 51 // 51 * 5 = 255
            if level == 5 {
                level--
            }
            fmt.Print(levels[level])
        }
        fmt.Print("\n")
    }

<h2 id="DecodeConfig">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/reader.go#L960">DecodeConfig</a>
    <a href="#DecodeConfig">¶</a></h2>
<pre>func DecodeConfig(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/image/">image</a>.<a href="/image/#Config">Config</a>, <a href="/builtin/#error">error</a>)</pre>

DecodeConfig returns the color model and dimensions of a PNG image without
decoding the entire image.

<h2 id="Encode">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/writer.go#L513">Encode</a>
    <a href="#Encode">¶</a></h2>
<pre>func Encode(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, m <a href="/image/">image</a>.<a href="/image/#Image">Image</a>) <a href="/builtin/#error">error</a></pre>

Encode writes the Image m to w in PNG format. Any Image may be encoded, but
images that are not image.NRGBA might be encoded lossily.

<a id="example_Encode"></a>
Example:

    const width, height = 256, 256

    // Create a colored image of the given width and height.
    img := image.NewNRGBA(image.Rect(0, 0, width, height))

    for y := 0; y < height; y++ {
        for x := 0; x < width; x++ {
            img.Set(x, y, color.NRGBA{
                R:  uint8((x + y) & 255),
                G:  uint8((x + y) << 1 & 255),
                B:  uint8((x + y) << 2 & 255),
                A:  255,
            })
        }
    }

    f, err := os.Create("image.png")
    if err != nil {
        log.Fatal(err)
    }

    if err := png.Encode(f, img); err != nil {
        f.Close()
        log.Fatal(err)
    }

    if err := f.Close(); err != nil {
        log.Fatal(err)
    }

<h2 id="CompressionLevel">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/writer.go#L43">CompressionLevel</a>
    <a href="#CompressionLevel">¶</a></h2>
<pre>type CompressionLevel <a href="/builtin/#int">int</a></pre>


<pre>const (
    <span id="DefaultCompression">DefaultCompression</span> <a href="#CompressionLevel">CompressionLevel</a> = 0
    <span id="NoCompression">NoCompression</span>      <a href="#CompressionLevel">CompressionLevel</a> = -1
    <span id="BestSpeed">BestSpeed</span>          <a href="#CompressionLevel">CompressionLevel</a> = -2
    <span id="BestCompression">BestCompression</span>    <a href="#CompressionLevel">CompressionLevel</a> = -3
)</pre>


<h2 id="Encoder">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/writer.go#L8">Encoder</a>
    <a href="#Encoder">¶</a></h2>
<pre>type Encoder struct {
<span id="Encoder.CompressionLevel"></span>    CompressionLevel <a href="#CompressionLevel">CompressionLevel</a>

<span id="Encoder.BufferPool"></span>    <span class="comment">// BufferPool optionally specifies a buffer pool to get temporary</span>
    <span class="comment">// EncoderBuffers when encoding an image.</span>
    BufferPool <a href="#EncoderBufferPool">EncoderBufferPool</a>
}</pre>

Encoder configures encoding PNG images.

<h3 id="Encoder.Encode">func (*Encoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/writer.go#L519">Encode</a>
    <a href="#Encoder.Encode">¶</a></h3>
<pre>func (enc *<a href="#Encoder">Encoder</a>) Encode(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, m <a href="/image/">image</a>.<a href="/image/#Image">Image</a>) <a href="/builtin/#error">error</a></pre>

Encode writes the Image m to w in PNG format.

<h2 id="EncoderBuffer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/writer.go#L25">EncoderBuffer</a>
    <a href="#EncoderBuffer">¶</a></h2>
<pre>type EncoderBuffer encoder</pre>

EncoderBuffer holds the buffers used for encoding PNG images.

<h2 id="EncoderBufferPool">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/writer.go#L19">EncoderBufferPool</a>
    <a href="#EncoderBufferPool">¶</a></h2>
<pre>type EncoderBufferPool interface {
    Get() *<a href="#EncoderBuffer">EncoderBuffer</a>
    Put(*<a href="#EncoderBuffer">EncoderBuffer</a>)
}</pre>

EncoderBufferPool is an interface for getting and returning temporary instances
of the EncoderBuffer struct. This can be used to reuse buffers when encoding
multiple images.

<h2 id="FormatError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/reader.go#L114">FormatError</a>
    <a href="#FormatError">¶</a></h2>
<pre>type FormatError <a href="/builtin/#string">string</a></pre>

A FormatError reports that the input is not a valid PNG.

<h3 id="FormatError.Error">func (FormatError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/reader.go#L116">Error</a>
    <a href="#FormatError.Error">¶</a></h3>
<pre>func (e <a href="#FormatError">FormatError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="UnsupportedError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/reader.go#L121">UnsupportedError</a>
    <a href="#UnsupportedError">¶</a></h2>
<pre>type UnsupportedError <a href="/builtin/#string">string</a></pre>

An UnsupportedError reports that the input uses a valid but unimplemented PNG
feature.

<h3 id="UnsupportedError.Error">func (UnsupportedError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/png/reader.go#L123">Error</a>
    <a href="#UnsupportedError.Error">¶</a></h3>
<pre>func (e <a href="#UnsupportedError">UnsupportedError</a>) Error() <a href="/builtin/#string">string</a></pre>



