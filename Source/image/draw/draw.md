version: 1.9.2
## package draw

  `import "image/draw"`

## Overview

Package draw provides image composition functions.

See "The Go image/draw package" for an introduction to this package:
https://golang.org/doc/articles/image_draw.html

## Index

- [func Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point, op Op)](#Draw)
- [func DrawMask(dst Image, r image.Rectangle, src image.Image, sp image.Point, mask image.Image, mp image.Point, op Op)](#DrawMask)
- [type Drawer](#Drawer)
- [type Image](#Image)
- [type Op](#Op)
  - [func (op Op) Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point)](#Op.Draw)
- [type Quantizer](#Quantizer)

### Examples

- [Drawer (FloydSteinberg)](#example_Drawer_floydSteinberg)

### Package files
 [draw.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/draw/draw.go)

<h2 id="Draw">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/draw/draw.go#L90">Draw</a>
    <a href="#Draw">¶</a></h2>
<pre>func Draw(dst <a href="#Image">Image</a>, r <a href="/image/">image</a>.<a href="/image/#Rectangle">Rectangle</a>, src <a href="/image/">image</a>.<a href="/image/#Image">Image</a>, sp <a href="/image/">image</a>.<a href="/image/#Point">Point</a>, op <a href="#Op">Op</a>)</pre>

Draw calls DrawMask with a nil mask.

<h2 id="DrawMask">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/draw/draw.go#L96">DrawMask</a>
    <a href="#DrawMask">¶</a></h2>
<pre>func DrawMask(dst <a href="#Image">Image</a>, r <a href="/image/">image</a>.<a href="/image/#Rectangle">Rectangle</a>, src <a href="/image/">image</a>.<a href="/image/#Image">Image</a>, sp <a href="/image/">image</a>.<a href="/image/#Point">Point</a>, mask <a href="/image/">image</a>.<a href="/image/#Image">Image</a>, mp <a href="/image/">image</a>.<a href="/image/#Point">Point</a>, op <a href="#Op">Op</a>)</pre>

DrawMask aligns r.Min in dst with sp in src and mp in mask and then replaces the
rectangle r in dst with the result of a Porter-Duff composition. A nil mask is
treated as opaque.

<h2 id="Drawer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/draw/draw.go#L40">Drawer</a>
    <a href="#Drawer">¶</a></h2>
<pre>type Drawer interface {
    <span class="comment">// Draw aligns r.Min in dst with sp in src and then replaces the</span>
    <span class="comment">// rectangle r in dst with the result of drawing src on dst.</span>
    Draw(dst <a href="#Image">Image</a>, r <a href="/image/">image</a>.<a href="/image/#Rectangle">Rectangle</a>, src <a href="/image/">image</a>.<a href="/image/#Image">Image</a>, sp <a href="/image/">image</a>.<a href="/image/#Point">Point</a>)
}</pre>

Drawer contains the Draw method.

<pre>var <span id="FloydSteinberg">FloydSteinberg</span> <a href="#Drawer">Drawer</a> = floydSteinberg{}</pre>

FloydSteinberg is a Drawer that is the Src Op with Floyd-Steinberg error
diffusion.

<a id="example_Drawer_floydSteinberg"></a>
Example:

    const width = 130
    const height = 50

    im := image.NewGray(image.Rectangle{Max: image.Point{X: width, Y: height}})
    for x := 0; x < width; x++ {
        for y := 0; y < height; y++ {
            dist := math.Sqrt(math.Pow(float64(x-width/2), 2)/3+math.Pow(float64(y-height/2), 2)) / (height / 1.5) * 255
            var gray uint8
            if dist > 255 {
                gray = 255
            } else {
                gray = uint8(dist)
            }
            im.SetGray(x, y, color.Gray{Y: 255 - gray})
        }
    }
    pi := image.NewPaletted(im.Bounds(), []color.Color{
        color.Gray{Y: 255},
        color.Gray{Y: 160},
        color.Gray{Y: 70},
        color.Gray{Y: 35},
        color.Gray{Y: 0},
    })

    draw.FloydSteinberg.Draw(pi, im.Bounds(), im, image.ZP)
    shade := []string{" ", "░", "▒", "▓", "█"}
    for i, p := range pi.Pix {
        fmt.Print(shade[p])
        if (i+1)%width == 0 {
            fmt.Print("\n")
        }
    }

<h2 id="Image">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/draw/draw.go#L11">Image</a>
    <a href="#Image">¶</a></h2>
<pre>type Image interface {
    <a href="/image/">image</a>.<a href="/image/#Image">Image</a>
    Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)
}</pre>

Image is an image.Image with a Set method to change a single pixel.

<h2 id="Op">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/draw/draw.go#L24">Op</a>
    <a href="#Op">¶</a></h2>
<pre>type Op <a href="/builtin/#int">int</a></pre>

Op is a Porter-Duff compositing operator.

<pre>const (
    <span class="comment">// Over specifies ``(src in mask) over dst&#39;&#39;.</span>
    <span id="Over">Over</span> <a href="#Op">Op</a> = <a href="/builtin/#iota">iota</a>
    <span class="comment">// Src specifies ``src in mask&#39;&#39;.</span>
    <span id="Src">Src</span>
)</pre>


<h3 id="Op.Draw">func (Op) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/draw/draw.go#L35">Draw</a>
    <a href="#Op.Draw">¶</a></h3>
<pre>func (op <a href="#Op">Op</a>) Draw(dst <a href="#Image">Image</a>, r <a href="/image/">image</a>.<a href="/image/#Rectangle">Rectangle</a>, src <a href="/image/">image</a>.<a href="/image/#Image">Image</a>, sp <a href="/image/">image</a>.<a href="/image/#Point">Point</a>)</pre>

Draw implements the Drawer interface by calling the Draw function with this Op.

<h2 id="Quantizer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/image/draw/draw.go#L17">Quantizer</a>
    <a href="#Quantizer">¶</a></h2>
<pre>type Quantizer interface {
    <span class="comment">// Quantize appends up to cap(p) - len(p) colors to p and returns the</span>
    <span class="comment">// updated palette suitable for converting m to a paletted image.</span>
    Quantize(p <a href="/image/color/">color</a>.<a href="/image/color/#Palette">Palette</a>, m <a href="/image/">image</a>.<a href="/image/#Image">Image</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Palette">Palette</a>
}</pre>

Quantizer produces a palette for an image.


