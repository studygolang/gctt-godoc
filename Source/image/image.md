version: 1.10
## package image

  `import "image"`

## Overview

Package image implements a basic 2-D image library.

The fundamental interface is called Image. An Image contains colors, which are
described in the image/color package.

Values of the Image interface are created either by calling functions such as
NewRGBA and NewPaletted, or by calling Decode on an io.Reader containing image
data in a format such as GIF, JPEG or PNG. Decoding any particular image format
requires the prior registration of a decoder function. Registration is typically
automatic as a side effect of initializing that format's package so that, to
decode a PNG image, it suffices to have

    import _ "image/png"

in a program's main package. The _ means to import a package purely for its
initialization side effects.

See "The Go image package" for more details:
https://golang.org/doc/articles/image_package.html

<a id="example"></a>
Example:

    // This example demonstrates decoding a JPEG image and examining its pixels.
    package image_test

    import (
        "encoding/base64"
        "fmt"
        "image"
        "log"
        "strings"

        // Package image/jpeg is not used explicitly in the code below,
        // but is imported for its initialization side-effect, which allows
        // image.Decode to understand JPEG formatted images. Uncomment these
        // two lines to also understand GIF and PNG images:
        // _ "image/gif"
        // _ "image/png"
        _ "image/jpeg"
    )

    func Example() {
        // Decode the JPEG data. If reading from file, create a reader with
        //
        // reader, err := os.Open("testdata/video-001.q50.420.jpeg")
        // if err != nil {
        //     log.Fatal(err)
        // }
        // defer reader.Close()
        reader := base64.NewDecoder(base64.StdEncoding, strings.NewReader(data))
        m, _, err := image.Decode(reader)
        if err != nil {
            log.Fatal(err)
        }
        bounds := m.Bounds()

        // Calculate a 16-bin histogram for m's red, green, blue and alpha components.
        //
        // An image's bounds do not necessarily start at (0, 0), so the two loops start
        // at bounds.Min.Y and bounds.Min.X. Looping over Y first and X second is more
        // likely to result in better memory access patterns than X first and Y second.
        var histogram [16][4]int
        for y := bounds.Min.Y; y < bounds.Max.Y; y++ {
            for x := bounds.Min.X; x < bounds.Max.X; x++ {
                r, g, b, a := m.At(x, y).RGBA()
                // A color's RGBA method returns values in the range [0, 65535].
                // Shifting by 12 reduces this to the range [0, 15].
                histogram[r>>12][0]++
                histogram[g>>12][1]++
                histogram[b>>12][2]++
                histogram[a>>12][3]++
            }
        }

        // Print the results.
        fmt.Printf("%-14s %6s %6s %6s %6s\n", "bin", "red", "green", "blue", "alpha")
        for i, x := range histogram {
            fmt.Printf("0x%04x-0x%04x: %6d %6d %6d %6d\n", i<<12, (i+1)<<12-1, x[0], x[1], x[2], x[3])
        }
        // Output:
        // bin               red  green   blue  alpha
        // 0x0000-0x0fff:    364    790   7242      0
        // 0x1000-0x1fff:    645   2967   1039      0
        // 0x2000-0x2fff:   1072   2299    979      0
        // 0x3000-0x3fff:    820   2266    980      0
        // 0x4000-0x4fff:    537   1305    541      0
        // 0x5000-0x5fff:    319    962    261      0
        // 0x6000-0x6fff:    322    375    177      0
        // 0x7000-0x7fff:    601    279    214      0
        // 0x8000-0x8fff:   3478    227    273      0
        // 0x9000-0x9fff:   2260    234    329      0
        // 0xa000-0xafff:    921    282    373      0
        // 0xb000-0xbfff:    321    335    397      0
        // 0xc000-0xcfff:    229    388    298      0
        // 0xd000-0xdfff:    260    414    277      0
        // 0xe000-0xefff:    516    428    298      0
        // 0xf000-0xffff:   2785   1899   1772  15450
    }

    const data = `
    /9j/4AAQSkZJRgABAQIAHAAcAAD/2wBDABALDA4MChAODQ4SERATGCgaGBYWGDEjJR0oOjM9PDkzODdA
    SFxOQERXRTc4UG1RV19iZ2hnPk1xeXBkeFxlZ2P/2wBDARESEhgVGC8aGi9jQjhCY2NjY2NjY2NjY2Nj
    Y2NjY2NjY2NjY2NjY2NjY2NjY2NjY2NjY2NjY2NjY2NjY2NjY2P/wAARCABnAJYDASIAAhEBAxEB/8QA
    HwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIh
    MUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVW
    V1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXG
    x8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQF
    BgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAV
    YnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOE
    hYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq
    8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDlwKMD0pwzSiuK57QzGDxS7D6in8Y5ximnAPUfSlcq4m3ilUYp
    2OKXHvRcVxnTtS7c07HNFK4DQPakC4PNOA+tOx70XAjK/So5gBGP94fzqfvUVx/qxx/EP51UXqRP4WSE
    cmgjilP3jSEZqS0IO/NGDnpUiocDg/McDjvV6HTPOdVWYgsM5KcfzzQ2JySM2jp6VYu7SWzmMUwG4cgj
    kMPUVBjjtTGtRu0Zopw+lFFxhinrGzuqqMsxAA9yaXFSRv5cqSEcIwYj6GpuZ30O30fSLKzhUpbpNMv3
    5XGTn29BV28jt7pPLuIVljPBBFVreYx+VbqAjycgt3x14zRcNOxGyVFHQkIc/wA61exyKLbuzjdZ046d
    ftEuTEw3Rk9SPT8P8Kpbea3tchbyVae4JkjbbGpGdwOM89Af6ViFTWUtGdcXoM2+woK1JtpNtTcoZt+l
    Jt7ZqTbRtouFyPFRXI/c9D94fzqzioLsfuD/ALw/nVReqIn8LJCOTSY+tSMOTmkIpXLRu+F0t5pJxPHG
    wjjUAuBjJJz1+laD6Pai+WaK9SBX6puzn6ZP+NV/Dkdtc6ZNbyAFwxLAHDYPv6VoQ21nPNEEiQGEFRtk
    Gf0NaWTOeW7Of8QwGG4MRZnEbYXPJwRnOR0zWNXW+KrqBLUWi5EjbWCgcAA9c/gRXKYqZaGlK/LqMH0F
    FLtHvRSNiYD2pSDTgpp6p0ywUHoTULXYxcktzrdCf7Xo8LP/AKyEmMNjJ46dfbFWJ5TDGNwB9lFUvDV9
    YrbfYGbyrjcWG88S57g+vtV26ZIvMlumKwwjLZ6V0WfU54yTvYwtbubea2WNWbzg4bYQeBgj8OtYeKhj
    u4y2HQxqxOD1xzxmrWAQCCGB6EGsaikndmsJxeiYzBo280/Z7UbayuaXGY5oIp+2lx9KLjIsVDeD/Rj/
    ALy/zq1t96r3y4tT/vL/ADq4P3kRP4WSleTSFKkkKoCW4GaqNcMxIjXj1pxjKT0FKrGC1Nrw3vGrKkYz
    5kTAr6455/HH510UdwPtRgWCbzF5+YYUf4Vwun39xpmoR3qASMmQUJwGU9Rnt/8AWrpbrxhb8/ZdOmaQ
    gAGZwFH5ZJrpVKVlY5ZYhN6kXiu2eO/ikZlIljAAB5yM549OawSOOlPuLqe+umuLqTfM4OSOAo7ADsKh
    hl/cRsTuJHPv7mlKi3sVTxNtGP20VJhThgSQaK52mnZnUqsWrpkyeUrr5pABOAPU1AGaXUCWJISHGPfP
    P8qL7BiKnsMg46H3qrbzupbj5mPTPTpXVSglG551SpzSsXJ4/MBUgYIxyKpySyGBYJriV1D7kRpCVH4V
    bSeNJ4xchni3DeqnBI+td7F4b0mKIRjT45VbktJlzk455+n6VtYzv2PNwFZWBHBGKVJDGVC54/nXQeMN
    NttLNkba1jgWVWDmM8bhg4/nzXLSSbXVj6fyNKUdNRp21RtIRJGrjuM0u3FQ2DbodvcEkfQmrW2vLqLl
    k0ejCXNFMj2/jQV9qkxSYNRcsZiq2oI32N2CkhWXJxwOe9XMcVt6hoPn6dFaW0wgRpNzvKDlz6+/0rai
    ryv2Jm9LHJai+ZRGCBjnr71ErdAxAY9B611t1Y2cunbbaOQ3FvKZI3UqGlZMbiWwfcfhV231iwvLSM3U
    lt5Uq52TuZG+hGMA12xXJGxxzjzybOQtNOvb5j9ktZJhnBIHyg+5PFX38JayqK/2eLJIBUTgkDA9q7ex
    itrSHFpGsUbndhRgc+g7VNIyfZJAoJZUbb3I46CtFJMylBo8sdWhmYMuCnylc9wef5VUT7+1chc5NS7h
    sUZO5RtIPUH3pkBDOxxxmqM9TQtn+WilhHfHaik43KTG3Z4IyPyrNVjGCsZ+dmwv6V3cXhSG8sYpJLud
    JJIwxChdoJGcYx/Wkg8DafA4knvLiQr/ALqj+VQpKw3FtnFFfvbiSMgZJ6/jXp2n3d9cQRBTFsKD96EP
    oOxPU/8A68VVtbbRtMVntbePKDLTSHJH/Aj/AEqHTvE66rq72VugMMcbSGTnL4wMAfjT5n0HyW3L+s6b
    baxaJBdzN+7bcrxkAhun0rz3VNCv7e7lgigknWI43xLu6jjIHTjtXqfkpPGVYsBkghTikgsYIN/lhgXb
    cxLkknp/ShczQ7xtY8vtEmhkj8yGRBuCnehUcnHcVtmwfJ/fQ8e7f/E12txZW91C0U6b42xlST2OR/Ko
    Bo1gM/uW55/1jf41nOipu7LhV5FZHIGzI6zwj/vr/Ck+yr3uYf8Ax7/CutbQdMb71tn/ALaN/jSf8I/p
    X/PoP++2/wAan6rAr6wzkWt0II+1Rc/7Lf4Vd1eeCSKBbdZDdShYoiZNoyfY10P/AAj2lf8APmP++2/x
    oPh/SjKspsozIuNrZORjp3qo0FHYPb3OZt7ae3SzjuItsiRSAgnccl/UA+3Q1yNjKLR4ZZYY5VD7tkv3
    WwO/+e1evPp9nI257aJm6bioz1z1+tY+s6Hplnot9PbWMMcqwOFcLyOO1bJWMZSTOPHi+9w3mosrlyd2
    9lCj02g9P/1e9a3hzxAbl2ikZRcdQueHHt7j864Y8Z4I4oRzG6urFWU5BHBB7HNJxTFGbR6he6Vpmtgm
    eLy5zwZI/lb8fX8azIvBUUTHdfSFP4QsYB/HNZ+k+KEnRY75hHOvAk6K/v7H9K6yyvlnQBmDZ6GsnzR0
    N0oy1RzOtaN/Y1tHNFO06u+zYy4I4Jzx9KKveJblXuordSGES5b6n/62PzorKVdp2LjQTVyWz8UWEWlq
    jSgyxfJt6EgdDzWTdeLIZGO7zHI/hVajGmWWP+PWL8qwlAIURrhpMAHHJA71pRcZrToZzcoEuo6heakA
    GHk245CZ6/X1qPTLq40q+W5t2QybSpDAkEEc55/zilk5k2r91eKhLDzWz2rpsczbbuemeD76fUNG865I
    MiysmQMZAAwa3a5j4ftu0ByP+fh/5CulkLLG7INzhSVHqe1Fh3uOoqn9qQQxyhndmHIxwOmSR2xQ13KD
    KoiBZOV9JBnt707MVy5RWdNdy7wRGf3bfMinnO1jg+vY03WXLaJO3mhQ20b0zwpYf0qlG7S7icrJs08U
    VwumgC+YiQyeVtZH567hzj8aSL949oGhE/2v5pJCDkksQwBHC4/+vXQ8LZ2uYxxCavY7us/xCcaBfn0h
    b+VP0bnSrb94ZMJgOecj1rl/GfidUE2k2gy5+SeQjgA/wj3rlas2jdao48qrjLAGkSKPk4Gc1WMj92I+
    lIJnU8OfxPWo5inBokmtQTmM4OOh71b0q6vbFmWCbaxHyqQGAP0PT8KhSTzVyo5ocSKA5VfTOTmqsmRd
    pl99XjPzThzK3zOeOSeveirNmkgg/fIpYsTkYORxRXmzlTjJqx6EVUcU7mhkKCzdAK59QI9zYxtG1fYU
    UVtgtmY4nZEa8Ak9aqFv3rfSiiu1nMeifDv/AJF+T/r4f+QrqqKKQwzQenNFFMCOKFIgNuThdoJ5OPSk
    ubeK6t3gnXdG4wwziiii/UTKMOg6dbzJLFE4dSCP3rEdeOM8805tDsGMvySgSsS6rM6gk9eAcUUVftZt
    3uyVGNthuq3Eei6DK8H7sRR7YuMgHtXkc8rzTNLM26RyWY+p70UVnLY0iEsUipG7rhZBlDkc1HgYoorM
    0HwyBXGeRjmrcUhMg2ghezd//rUUVcTKW5s2jZtY/QDaOKKKK8ip8bPRj8KP/9k=
    `

## Index

- [Variables](#pkg-variables)
- [func RegisterFormat(name, magic string, decode func(io.Reader) (Image, error), decodeConfig func(io.Reader) (Config, error))](#RegisterFormat)
- [type Alpha](#Alpha)
  - [func NewAlpha(r Rectangle) *Alpha](#NewAlpha)
  - [func (p *Alpha) AlphaAt(x, y int) color.Alpha](#Alpha.AlphaAt)
  - [func (p *Alpha) At(x, y int) color.Color](#Alpha.At)
  - [func (p *Alpha) Bounds() Rectangle](#Alpha.Bounds)
  - [func (p *Alpha) ColorModel() color.Model](#Alpha.ColorModel)
  - [func (p *Alpha) Opaque() bool](#Alpha.Opaque)
  - [func (p *Alpha) PixOffset(x, y int) int](#Alpha.PixOffset)
  - [func (p *Alpha) Set(x, y int, c color.Color)](#Alpha.Set)
  - [func (p *Alpha) SetAlpha(x, y int, c color.Alpha)](#Alpha.SetAlpha)
  - [func (p *Alpha) SubImage(r Rectangle) Image](#Alpha.SubImage)
- [type Alpha16](#Alpha16)
  - [func NewAlpha16(r Rectangle) *Alpha16](#NewAlpha16)
  - [func (p *Alpha16) Alpha16At(x, y int) color.Alpha16](#Alpha16.Alpha16At)
  - [func (p *Alpha16) At(x, y int) color.Color](#Alpha16.At)
  - [func (p *Alpha16) Bounds() Rectangle](#Alpha16.Bounds)
  - [func (p *Alpha16) ColorModel() color.Model](#Alpha16.ColorModel)
  - [func (p *Alpha16) Opaque() bool](#Alpha16.Opaque)
  - [func (p *Alpha16) PixOffset(x, y int) int](#Alpha16.PixOffset)
  - [func (p *Alpha16) Set(x, y int, c color.Color)](#Alpha16.Set)
  - [func (p *Alpha16) SetAlpha16(x, y int, c color.Alpha16)](#Alpha16.SetAlpha16)
  - [func (p *Alpha16) SubImage(r Rectangle) Image](#Alpha16.SubImage)
- [type CMYK](#CMYK)
  - [func NewCMYK(r Rectangle) *CMYK](#NewCMYK)
  - [func (p *CMYK) At(x, y int) color.Color](#CMYK.At)
  - [func (p *CMYK) Bounds() Rectangle](#CMYK.Bounds)
  - [func (p *CMYK) CMYKAt(x, y int) color.CMYK](#CMYK.CMYKAt)
  - [func (p *CMYK) ColorModel() color.Model](#CMYK.ColorModel)
  - [func (p *CMYK) Opaque() bool](#CMYK.Opaque)
  - [func (p *CMYK) PixOffset(x, y int) int](#CMYK.PixOffset)
  - [func (p *CMYK) Set(x, y int, c color.Color)](#CMYK.Set)
  - [func (p *CMYK) SetCMYK(x, y int, c color.CMYK)](#CMYK.SetCMYK)
  - [func (p *CMYK) SubImage(r Rectangle) Image](#CMYK.SubImage)
- [type Config](#Config)
  - [func DecodeConfig(r io.Reader) (Config, string, error)](#DecodeConfig)
- [type Gray](#Gray)
  - [func NewGray(r Rectangle) *Gray](#NewGray)
  - [func (p *Gray) At(x, y int) color.Color](#Gray.At)
  - [func (p *Gray) Bounds() Rectangle](#Gray.Bounds)
  - [func (p *Gray) ColorModel() color.Model](#Gray.ColorModel)
  - [func (p *Gray) GrayAt(x, y int) color.Gray](#Gray.GrayAt)
  - [func (p *Gray) Opaque() bool](#Gray.Opaque)
  - [func (p *Gray) PixOffset(x, y int) int](#Gray.PixOffset)
  - [func (p *Gray) Set(x, y int, c color.Color)](#Gray.Set)
  - [func (p *Gray) SetGray(x, y int, c color.Gray)](#Gray.SetGray)
  - [func (p *Gray) SubImage(r Rectangle) Image](#Gray.SubImage)
- [type Gray16](#Gray16)
  - [func NewGray16(r Rectangle) *Gray16](#NewGray16)
  - [func (p *Gray16) At(x, y int) color.Color](#Gray16.At)
  - [func (p *Gray16) Bounds() Rectangle](#Gray16.Bounds)
  - [func (p *Gray16) ColorModel() color.Model](#Gray16.ColorModel)
  - [func (p *Gray16) Gray16At(x, y int) color.Gray16](#Gray16.Gray16At)
  - [func (p *Gray16) Opaque() bool](#Gray16.Opaque)
  - [func (p *Gray16) PixOffset(x, y int) int](#Gray16.PixOffset)
  - [func (p *Gray16) Set(x, y int, c color.Color)](#Gray16.Set)
  - [func (p *Gray16) SetGray16(x, y int, c color.Gray16)](#Gray16.SetGray16)
  - [func (p *Gray16) SubImage(r Rectangle) Image](#Gray16.SubImage)
- [type Image](#Image)
  - [func Decode(r io.Reader) (Image, string, error)](#Decode)
- [type NRGBA](#NRGBA)
  - [func NewNRGBA(r Rectangle) *NRGBA](#NewNRGBA)
  - [func (p *NRGBA) At(x, y int) color.Color](#NRGBA.At)
  - [func (p *NRGBA) Bounds() Rectangle](#NRGBA.Bounds)
  - [func (p *NRGBA) ColorModel() color.Model](#NRGBA.ColorModel)
  - [func (p *NRGBA) NRGBAAt(x, y int) color.NRGBA](#NRGBA.NRGBAAt)
  - [func (p *NRGBA) Opaque() bool](#NRGBA.Opaque)
  - [func (p *NRGBA) PixOffset(x, y int) int](#NRGBA.PixOffset)
  - [func (p *NRGBA) Set(x, y int, c color.Color)](#NRGBA.Set)
  - [func (p *NRGBA) SetNRGBA(x, y int, c color.NRGBA)](#NRGBA.SetNRGBA)
  - [func (p *NRGBA) SubImage(r Rectangle) Image](#NRGBA.SubImage)
- [type NRGBA64](#NRGBA64)
  - [func NewNRGBA64(r Rectangle) *NRGBA64](#NewNRGBA64)
  - [func (p *NRGBA64) At(x, y int) color.Color](#NRGBA64.At)
  - [func (p *NRGBA64) Bounds() Rectangle](#NRGBA64.Bounds)
  - [func (p *NRGBA64) ColorModel() color.Model](#NRGBA64.ColorModel)
  - [func (p *NRGBA64) NRGBA64At(x, y int) color.NRGBA64](#NRGBA64.NRGBA64At)
  - [func (p *NRGBA64) Opaque() bool](#NRGBA64.Opaque)
  - [func (p *NRGBA64) PixOffset(x, y int) int](#NRGBA64.PixOffset)
  - [func (p *NRGBA64) Set(x, y int, c color.Color)](#NRGBA64.Set)
  - [func (p *NRGBA64) SetNRGBA64(x, y int, c color.NRGBA64)](#NRGBA64.SetNRGBA64)
  - [func (p *NRGBA64) SubImage(r Rectangle) Image](#NRGBA64.SubImage)
- [type NYCbCrA](#NYCbCrA)
  - [func NewNYCbCrA(r Rectangle, subsampleRatio YCbCrSubsampleRatio) *NYCbCrA](#NewNYCbCrA)
  - [func (p *NYCbCrA) AOffset(x, y int) int](#NYCbCrA.AOffset)
  - [func (p *NYCbCrA) At(x, y int) color.Color](#NYCbCrA.At)
  - [func (p *NYCbCrA) ColorModel() color.Model](#NYCbCrA.ColorModel)
  - [func (p *NYCbCrA) NYCbCrAAt(x, y int) color.NYCbCrA](#NYCbCrA.NYCbCrAAt)
  - [func (p *NYCbCrA) Opaque() bool](#NYCbCrA.Opaque)
  - [func (p *NYCbCrA) SubImage(r Rectangle) Image](#NYCbCrA.SubImage)
- [type Paletted](#Paletted)
  - [func NewPaletted(r Rectangle, p color.Palette) *Paletted](#NewPaletted)
  - [func (p *Paletted) At(x, y int) color.Color](#Paletted.At)
  - [func (p *Paletted) Bounds() Rectangle](#Paletted.Bounds)
  - [func (p *Paletted) ColorIndexAt(x, y int) uint8](#Paletted.ColorIndexAt)
  - [func (p *Paletted) ColorModel() color.Model](#Paletted.ColorModel)
  - [func (p *Paletted) Opaque() bool](#Paletted.Opaque)
  - [func (p *Paletted) PixOffset(x, y int) int](#Paletted.PixOffset)
  - [func (p *Paletted) Set(x, y int, c color.Color)](#Paletted.Set)
  - [func (p *Paletted) SetColorIndex(x, y int, index uint8)](#Paletted.SetColorIndex)
  - [func (p *Paletted) SubImage(r Rectangle) Image](#Paletted.SubImage)
- [type PalettedImage](#PalettedImage)
- [type Point](#Point)
  - [func Pt(X, Y int) Point](#Pt)
  - [func (p Point) Add(q Point) Point](#Point.Add)
  - [func (p Point) Div(k int) Point](#Point.Div)
  - [func (p Point) Eq(q Point) bool](#Point.Eq)
  - [func (p Point) In(r Rectangle) bool](#Point.In)
  - [func (p Point) Mod(r Rectangle) Point](#Point.Mod)
  - [func (p Point) Mul(k int) Point](#Point.Mul)
  - [func (p Point) String() string](#Point.String)
  - [func (p Point) Sub(q Point) Point](#Point.Sub)
- [type RGBA](#RGBA)
  - [func NewRGBA(r Rectangle) *RGBA](#NewRGBA)
  - [func (p *RGBA) At(x, y int) color.Color](#RGBA.At)
  - [func (p *RGBA) Bounds() Rectangle](#RGBA.Bounds)
  - [func (p *RGBA) ColorModel() color.Model](#RGBA.ColorModel)
  - [func (p *RGBA) Opaque() bool](#RGBA.Opaque)
  - [func (p *RGBA) PixOffset(x, y int) int](#RGBA.PixOffset)
  - [func (p *RGBA) RGBAAt(x, y int) color.RGBA](#RGBA.RGBAAt)
  - [func (p *RGBA) Set(x, y int, c color.Color)](#RGBA.Set)
  - [func (p *RGBA) SetRGBA(x, y int, c color.RGBA)](#RGBA.SetRGBA)
  - [func (p *RGBA) SubImage(r Rectangle) Image](#RGBA.SubImage)
- [type RGBA64](#RGBA64)
  - [func NewRGBA64(r Rectangle) *RGBA64](#NewRGBA64)
  - [func (p *RGBA64) At(x, y int) color.Color](#RGBA64.At)
  - [func (p *RGBA64) Bounds() Rectangle](#RGBA64.Bounds)
  - [func (p *RGBA64) ColorModel() color.Model](#RGBA64.ColorModel)
  - [func (p *RGBA64) Opaque() bool](#RGBA64.Opaque)
  - [func (p *RGBA64) PixOffset(x, y int) int](#RGBA64.PixOffset)
  - [func (p *RGBA64) RGBA64At(x, y int) color.RGBA64](#RGBA64.RGBA64At)
  - [func (p *RGBA64) Set(x, y int, c color.Color)](#RGBA64.Set)
  - [func (p *RGBA64) SetRGBA64(x, y int, c color.RGBA64)](#RGBA64.SetRGBA64)
  - [func (p *RGBA64) SubImage(r Rectangle) Image](#RGBA64.SubImage)
- [type Rectangle](#Rectangle)
  - [func Rect(x0, y0, x1, y1 int) Rectangle](#Rect)
  - [func (r Rectangle) Add(p Point) Rectangle](#Rectangle.Add)
  - [func (r Rectangle) At(x, y int) color.Color](#Rectangle.At)
  - [func (r Rectangle) Bounds() Rectangle](#Rectangle.Bounds)
  - [func (r Rectangle) Canon() Rectangle](#Rectangle.Canon)
  - [func (r Rectangle) ColorModel() color.Model](#Rectangle.ColorModel)
  - [func (r Rectangle) Dx() int](#Rectangle.Dx)
  - [func (r Rectangle) Dy() int](#Rectangle.Dy)
  - [func (r Rectangle) Empty() bool](#Rectangle.Empty)
  - [func (r Rectangle) Eq(s Rectangle) bool](#Rectangle.Eq)
  - [func (r Rectangle) In(s Rectangle) bool](#Rectangle.In)
  - [func (r Rectangle) Inset(n int) Rectangle](#Rectangle.Inset)
  - [func (r Rectangle) Intersect(s Rectangle) Rectangle](#Rectangle.Intersect)
  - [func (r Rectangle) Overlaps(s Rectangle) bool](#Rectangle.Overlaps)
  - [func (r Rectangle) Size() Point](#Rectangle.Size)
  - [func (r Rectangle) String() string](#Rectangle.String)
  - [func (r Rectangle) Sub(p Point) Rectangle](#Rectangle.Sub)
  - [func (r Rectangle) Union(s Rectangle) Rectangle](#Rectangle.Union)
- [type Uniform](#Uniform)
  - [func NewUniform(c color.Color) *Uniform](#NewUniform)
  - [func (c *Uniform) At(x, y int) color.Color](#Uniform.At)
  - [func (c *Uniform) Bounds() Rectangle](#Uniform.Bounds)
  - [func (c *Uniform) ColorModel() color.Model](#Uniform.ColorModel)
  - [func (c *Uniform) Convert(color.Color) color.Color](#Uniform.Convert)
  - [func (c *Uniform) Opaque() bool](#Uniform.Opaque)
  - [func (c *Uniform) RGBA() (r, g, b, a uint32)](#Uniform.RGBA)
- [type YCbCr](#YCbCr)
  - [func NewYCbCr(r Rectangle, subsampleRatio YCbCrSubsampleRatio) *YCbCr](#NewYCbCr)
  - [func (p *YCbCr) At(x, y int) color.Color](#YCbCr.At)
  - [func (p *YCbCr) Bounds() Rectangle](#YCbCr.Bounds)
  - [func (p *YCbCr) COffset(x, y int) int](#YCbCr.COffset)
  - [func (p *YCbCr) ColorModel() color.Model](#YCbCr.ColorModel)
  - [func (p *YCbCr) Opaque() bool](#YCbCr.Opaque)
  - [func (p *YCbCr) SubImage(r Rectangle) Image](#YCbCr.SubImage)
  - [func (p *YCbCr) YCbCrAt(x, y int) color.YCbCr](#YCbCr.YCbCrAt)
  - [func (p *YCbCr) YOffset(x, y int) int](#YCbCr.YOffset)
- [type YCbCrSubsampleRatio](#YCbCrSubsampleRatio)
  - [func (s YCbCrSubsampleRatio) String() string](#YCbCrSubsampleRatio.String)

### Examples

- [Package](#example)

### Package files
 [format.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/format.go) [geom.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go) [image.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go) [names.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/names.go) [ycbcr.go](//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span class="comment">// Black is an opaque black uniform image.</span>
    <span id="Black">Black</span> = <a href="#NewUniform">NewUniform</a>(<a href="/image/color/">color</a>.<a href="/image/color/#Black">Black</a>)
    <span class="comment">// White is an opaque white uniform image.</span>
    <span id="White">White</span> = <a href="#NewUniform">NewUniform</a>(<a href="/image/color/">color</a>.<a href="/image/color/#White">White</a>)
    <span class="comment">// Transparent is a fully transparent uniform image.</span>
    <span id="Transparent">Transparent</span> = <a href="#NewUniform">NewUniform</a>(<a href="/image/color/">color</a>.<a href="/image/color/#Transparent">Transparent</a>)
    <span class="comment">// Opaque is a fully opaque uniform image.</span>
    <span id="Opaque">Opaque</span> = <a href="#NewUniform">NewUniform</a>(<a href="/image/color/">color</a>.<a href="/image/color/#Opaque">Opaque</a>)
)</pre>


<pre>var <span id="ErrFormat">ErrFormat</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;image: unknown format&#34;)</pre>

ErrFormat indicates that decoding encountered an unknown format.

<h2 id="RegisterFormat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/format.go#L22">RegisterFormat</a>
    <a href="#RegisterFormat">¶</a></h2>
<pre>func RegisterFormat(name, magic <a href="/builtin/#string">string</a>, decode func(<a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="#Image">Image</a>, <a href="/builtin/#error">error</a>), decodeConfig func(<a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="#Config">Config</a>, <a href="/builtin/#error">error</a>))</pre>

RegisterFormat registers an image format for use by Decode. Name is the name of
the format, like "jpeg" or "png". Magic is the magic prefix that identifies the
format's encoding. The magic string can contain "?" wildcards that each match
any one byte. Decode is the function that decodes the encoded image.
DecodeConfig is the function that decodes just its configuration.

<h2 id="Alpha">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L472">Alpha</a>
    <a href="#Alpha">¶</a></h2>
<pre>type Alpha struct {
<span id="Alpha.Pix"></span>    <span class="comment">// Pix holds the image&#39;s pixels, as alpha values. The pixel at</span>
    <span class="comment">// (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*1].</span>
    Pix []<a href="/builtin/#uint8">uint8</a>
<span id="Alpha.Stride"></span>    <span class="comment">// Stride is the Pix stride (in bytes) between vertically adjacent pixels.</span>
    Stride <a href="/builtin/#int">int</a>
<span id="Alpha.Rect"></span>    <span class="comment">// Rect is the image&#39;s bounds.</span>
    Rect <a href="#Rectangle">Rectangle</a>
}</pre>

Alpha is an in-memory image whose At method returns color.Alpha values.

<h3 id="NewAlpha">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L557">NewAlpha</a>
    <a href="#NewAlpha">¶</a></h3>
<pre>func NewAlpha(r <a href="#Rectangle">Rectangle</a>) *<a href="#Alpha">Alpha</a></pre>

NewAlpha returns a new Alpha image with the given bounds.

<h3 id="Alpha.AlphaAt">func (*Alpha) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L490">AlphaAt</a>
    <a href="#Alpha.AlphaAt">¶</a></h3>
<pre>func (p *<a href="#Alpha">Alpha</a>) AlphaAt(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Alpha">Alpha</a></pre>


<h3 id="Alpha.At">func (*Alpha) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L486">At</a>
    <a href="#Alpha.At">¶</a></h3>
<pre>func (p *<a href="#Alpha">Alpha</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="Alpha.Bounds">func (*Alpha) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L484">Bounds</a>
    <a href="#Alpha.Bounds">¶</a></h3>
<pre>func (p *<a href="#Alpha">Alpha</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="Alpha.ColorModel">func (*Alpha) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L482">ColorModel</a>
    <a href="#Alpha.ColorModel">¶</a></h3>
<pre>func (p *<a href="#Alpha">Alpha</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="Alpha.Opaque">func (*Alpha) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L539">Opaque</a>
    <a href="#Alpha.Opaque">¶</a></h3>
<pre>func (p *<a href="#Alpha">Alpha</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="Alpha.PixOffset">func (*Alpha) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L500">PixOffset</a>
    <a href="#Alpha.PixOffset">¶</a></h3>
<pre>func (p *<a href="#Alpha">Alpha</a>) PixOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

PixOffset returns the index of the first element of Pix that corresponds to the
pixel at (x, y).

<h3 id="Alpha.Set">func (*Alpha) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L504">Set</a>
    <a href="#Alpha.Set">¶</a></h3>
<pre>func (p *<a href="#Alpha">Alpha</a>) Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)</pre>


<h3 id="Alpha.SetAlpha">func (*Alpha) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L512">SetAlpha</a>
    <a href="#Alpha.SetAlpha">¶</a></h3>
<pre>func (p *<a href="#Alpha">Alpha</a>) SetAlpha(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Alpha">Alpha</a>)</pre>


<h3 id="Alpha.SubImage">func (*Alpha) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L522">SubImage</a>
    <a href="#Alpha.SubImage">¶</a></h3>
<pre>func (p *<a href="#Alpha">Alpha</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="Alpha16">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L564">Alpha16</a>
    <a href="#Alpha16">¶</a></h2>
<pre>type Alpha16 struct {
<span id="Alpha16.Pix"></span>    <span class="comment">// Pix holds the image&#39;s pixels, as alpha values in big-endian format. The pixel at</span>
    <span class="comment">// (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*2].</span>
    Pix []<a href="/builtin/#uint8">uint8</a>
<span id="Alpha16.Stride"></span>    <span class="comment">// Stride is the Pix stride (in bytes) between vertically adjacent pixels.</span>
    Stride <a href="/builtin/#int">int</a>
<span id="Alpha16.Rect"></span>    <span class="comment">// Rect is the image&#39;s bounds.</span>
    Rect <a href="#Rectangle">Rectangle</a>
}</pre>

Alpha16 is an in-memory image whose At method returns color.Alpha16 values.

<h3 id="NewAlpha16">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L652">NewAlpha16</a>
    <a href="#NewAlpha16">¶</a></h3>
<pre>func NewAlpha16(r <a href="#Rectangle">Rectangle</a>) *<a href="#Alpha16">Alpha16</a></pre>

NewAlpha16 returns a new Alpha16 image with the given bounds.

<h3 id="Alpha16.Alpha16At">func (*Alpha16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L582">Alpha16At</a>
    <a href="#Alpha16.Alpha16At">¶</a></h3>
<pre>func (p *<a href="#Alpha16">Alpha16</a>) Alpha16At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Alpha16">Alpha16</a></pre>


<h3 id="Alpha16.At">func (*Alpha16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L578">At</a>
    <a href="#Alpha16.At">¶</a></h3>
<pre>func (p *<a href="#Alpha16">Alpha16</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="Alpha16.Bounds">func (*Alpha16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L576">Bounds</a>
    <a href="#Alpha16.Bounds">¶</a></h3>
<pre>func (p *<a href="#Alpha16">Alpha16</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="Alpha16.ColorModel">func (*Alpha16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L574">ColorModel</a>
    <a href="#Alpha16.ColorModel">¶</a></h3>
<pre>func (p *<a href="#Alpha16">Alpha16</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="Alpha16.Opaque">func (*Alpha16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L634">Opaque</a>
    <a href="#Alpha16.Opaque">¶</a></h3>
<pre>func (p *<a href="#Alpha16">Alpha16</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="Alpha16.PixOffset">func (*Alpha16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L592">PixOffset</a>
    <a href="#Alpha16.PixOffset">¶</a></h3>
<pre>func (p *<a href="#Alpha16">Alpha16</a>) PixOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

PixOffset returns the index of the first element of Pix that corresponds to the
pixel at (x, y).

<h3 id="Alpha16.Set">func (*Alpha16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L596">Set</a>
    <a href="#Alpha16.Set">¶</a></h3>
<pre>func (p *<a href="#Alpha16">Alpha16</a>) Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)</pre>


<h3 id="Alpha16.SetAlpha16">func (*Alpha16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L606">SetAlpha16</a>
    <a href="#Alpha16.SetAlpha16">¶</a></h3>
<pre>func (p *<a href="#Alpha16">Alpha16</a>) SetAlpha16(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Alpha16">Alpha16</a>)</pre>


<h3 id="Alpha16.SubImage">func (*Alpha16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L617">SubImage</a>
    <a href="#Alpha16.SubImage">¶</a></h3>
<pre>func (p *<a href="#Alpha16">Alpha16</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="CMYK">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L820">CMYK</a>
    <a href="#CMYK">¶</a></h2>
<pre>type CMYK struct {
<span id="CMYK.Pix"></span>    <span class="comment">// Pix holds the image&#39;s pixels, in C, M, Y, K order. The pixel at</span>
    <span class="comment">// (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*4].</span>
    Pix []<a href="/builtin/#uint8">uint8</a>
<span id="CMYK.Stride"></span>    <span class="comment">// Stride is the Pix stride (in bytes) between vertically adjacent pixels.</span>
    Stride <a href="/builtin/#int">int</a>
<span id="CMYK.Rect"></span>    <span class="comment">// Rect is the image&#39;s bounds.</span>
    Rect <a href="#Rectangle">Rectangle</a>
}</pre>

CMYK is an in-memory image whose At method returns color.CMYK values.

<h3 id="NewCMYK">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L899">NewCMYK</a>
    <a href="#NewCMYK">¶</a></h3>
<pre>func NewCMYK(r <a href="#Rectangle">Rectangle</a>) *<a href="#CMYK">CMYK</a></pre>

NewCMYK returns a new CMYK image with the given bounds.

<h3 id="CMYK.At">func (*CMYK) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L834">At</a>
    <a href="#CMYK.At">¶</a></h3>
<pre>func (p *<a href="#CMYK">CMYK</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="CMYK.Bounds">func (*CMYK) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L832">Bounds</a>
    <a href="#CMYK.Bounds">¶</a></h3>
<pre>func (p *<a href="#CMYK">CMYK</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="CMYK.CMYKAt">func (*CMYK) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L838">CMYKAt</a>
    <a href="#CMYK.CMYKAt">¶</a></h3>
<pre>func (p *<a href="#CMYK">CMYK</a>) CMYKAt(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#CMYK">CMYK</a></pre>


<h3 id="CMYK.ColorModel">func (*CMYK) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L830">ColorModel</a>
    <a href="#CMYK.ColorModel">¶</a></h3>
<pre>func (p *<a href="#CMYK">CMYK</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="CMYK.Opaque">func (*CMYK) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L894">Opaque</a>
    <a href="#CMYK.Opaque">¶</a></h3>
<pre>func (p *<a href="#CMYK">CMYK</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="CMYK.PixOffset">func (*CMYK) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L848">PixOffset</a>
    <a href="#CMYK.PixOffset">¶</a></h3>
<pre>func (p *<a href="#CMYK">CMYK</a>) PixOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

PixOffset returns the index of the first element of Pix that corresponds to the
pixel at (x, y).

<h3 id="CMYK.Set">func (*CMYK) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L852">Set</a>
    <a href="#CMYK.Set">¶</a></h3>
<pre>func (p *<a href="#CMYK">CMYK</a>) Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)</pre>


<h3 id="CMYK.SetCMYK">func (*CMYK) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L864">SetCMYK</a>
    <a href="#CMYK.SetCMYK">¶</a></h3>
<pre>func (p *<a href="#CMYK">CMYK</a>) SetCMYK(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#CMYK">CMYK</a>)</pre>


<h3 id="CMYK.SubImage">func (*CMYK) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L877">SubImage</a>
    <a href="#CMYK.SubImage">¶</a></h3>
<pre>func (p *<a href="#CMYK">CMYK</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="Config">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L19">Config</a>
    <a href="#Config">¶</a></h2>
<pre>type Config struct {
<span id="Config.ColorModel"></span>    ColorModel    <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a>
<span id="Config.Width"></span>    Width, Height <a href="/builtin/#int">int</a>
}</pre>

Config holds an image's color model and dimensions.

<h3 id="DecodeConfig">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/format.go#L82">DecodeConfig</a>
    <a href="#DecodeConfig">¶</a></h3>
<pre>func DecodeConfig(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="#Config">Config</a>, <a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

DecodeConfig decodes the color model and dimensions of an image that has been
encoded in a registered format. The string returned is the format name used
during format registration. Format registration is typically done by an init
function in the codec-specific package.

<h2 id="Gray">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L659">Gray</a>
    <a href="#Gray">¶</a></h2>
<pre>type Gray struct {
<span id="Gray.Pix"></span>    <span class="comment">// Pix holds the image&#39;s pixels, as gray values. The pixel at</span>
    <span class="comment">// (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*1].</span>
    Pix []<a href="/builtin/#uint8">uint8</a>
<span id="Gray.Stride"></span>    <span class="comment">// Stride is the Pix stride (in bytes) between vertically adjacent pixels.</span>
    Stride <a href="/builtin/#int">int</a>
<span id="Gray.Rect"></span>    <span class="comment">// Rect is the image&#39;s bounds.</span>
    Rect <a href="#Rectangle">Rectangle</a>
}</pre>

Gray is an in-memory image whose At method returns color.Gray values.

<h3 id="NewGray">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L731">NewGray</a>
    <a href="#NewGray">¶</a></h3>
<pre>func NewGray(r <a href="#Rectangle">Rectangle</a>) *<a href="#Gray">Gray</a></pre>

NewGray returns a new Gray image with the given bounds.

<h3 id="Gray.At">func (*Gray) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L673">At</a>
    <a href="#Gray.At">¶</a></h3>
<pre>func (p *<a href="#Gray">Gray</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="Gray.Bounds">func (*Gray) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L671">Bounds</a>
    <a href="#Gray.Bounds">¶</a></h3>
<pre>func (p *<a href="#Gray">Gray</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="Gray.ColorModel">func (*Gray) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L669">ColorModel</a>
    <a href="#Gray.ColorModel">¶</a></h3>
<pre>func (p *<a href="#Gray">Gray</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="Gray.GrayAt">func (*Gray) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L677">GrayAt</a>
    <a href="#Gray.GrayAt">¶</a></h3>
<pre>func (p *<a href="#Gray">Gray</a>) GrayAt(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Gray">Gray</a></pre>


<h3 id="Gray.Opaque">func (*Gray) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L726">Opaque</a>
    <a href="#Gray.Opaque">¶</a></h3>
<pre>func (p *<a href="#Gray">Gray</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="Gray.PixOffset">func (*Gray) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L687">PixOffset</a>
    <a href="#Gray.PixOffset">¶</a></h3>
<pre>func (p *<a href="#Gray">Gray</a>) PixOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

PixOffset returns the index of the first element of Pix that corresponds to the
pixel at (x, y).

<h3 id="Gray.Set">func (*Gray) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L691">Set</a>
    <a href="#Gray.Set">¶</a></h3>
<pre>func (p *<a href="#Gray">Gray</a>) Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)</pre>


<h3 id="Gray.SetGray">func (*Gray) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L699">SetGray</a>
    <a href="#Gray.SetGray">¶</a></h3>
<pre>func (p *<a href="#Gray">Gray</a>) SetGray(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Gray">Gray</a>)</pre>


<h3 id="Gray.SubImage">func (*Gray) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L709">SubImage</a>
    <a href="#Gray.SubImage">¶</a></h3>
<pre>func (p *<a href="#Gray">Gray</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="Gray16">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L738">Gray16</a>
    <a href="#Gray16">¶</a></h2>
<pre>type Gray16 struct {
<span id="Gray16.Pix"></span>    <span class="comment">// Pix holds the image&#39;s pixels, as gray values in big-endian format. The pixel at</span>
    <span class="comment">// (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*2].</span>
    Pix []<a href="/builtin/#uint8">uint8</a>
<span id="Gray16.Stride"></span>    <span class="comment">// Stride is the Pix stride (in bytes) between vertically adjacent pixels.</span>
    Stride <a href="/builtin/#int">int</a>
<span id="Gray16.Rect"></span>    <span class="comment">// Rect is the image&#39;s bounds.</span>
    Rect <a href="#Rectangle">Rectangle</a>
}</pre>

Gray16 is an in-memory image whose At method returns color.Gray16 values.

<h3 id="NewGray16">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L813">NewGray16</a>
    <a href="#NewGray16">¶</a></h3>
<pre>func NewGray16(r <a href="#Rectangle">Rectangle</a>) *<a href="#Gray16">Gray16</a></pre>

NewGray16 returns a new Gray16 image with the given bounds.

<h3 id="Gray16.At">func (*Gray16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L752">At</a>
    <a href="#Gray16.At">¶</a></h3>
<pre>func (p *<a href="#Gray16">Gray16</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="Gray16.Bounds">func (*Gray16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L750">Bounds</a>
    <a href="#Gray16.Bounds">¶</a></h3>
<pre>func (p *<a href="#Gray16">Gray16</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="Gray16.ColorModel">func (*Gray16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L748">ColorModel</a>
    <a href="#Gray16.ColorModel">¶</a></h3>
<pre>func (p *<a href="#Gray16">Gray16</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="Gray16.Gray16At">func (*Gray16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L756">Gray16At</a>
    <a href="#Gray16.Gray16At">¶</a></h3>
<pre>func (p *<a href="#Gray16">Gray16</a>) Gray16At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Gray16">Gray16</a></pre>


<h3 id="Gray16.Opaque">func (*Gray16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L808">Opaque</a>
    <a href="#Gray16.Opaque">¶</a></h3>
<pre>func (p *<a href="#Gray16">Gray16</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="Gray16.PixOffset">func (*Gray16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L766">PixOffset</a>
    <a href="#Gray16.PixOffset">¶</a></h3>
<pre>func (p *<a href="#Gray16">Gray16</a>) PixOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

PixOffset returns the index of the first element of Pix that corresponds to the
pixel at (x, y).

<h3 id="Gray16.Set">func (*Gray16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L770">Set</a>
    <a href="#Gray16.Set">¶</a></h3>
<pre>func (p *<a href="#Gray16">Gray16</a>) Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)</pre>


<h3 id="Gray16.SetGray16">func (*Gray16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L780">SetGray16</a>
    <a href="#Gray16.SetGray16">¶</a></h3>
<pre>func (p *<a href="#Gray16">Gray16</a>) SetGray16(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Gray16">Gray16</a>)</pre>


<h3 id="Gray16.SubImage">func (*Gray16) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L791">SubImage</a>
    <a href="#Gray16.SubImage">¶</a></h3>
<pre>func (p *<a href="#Gray16">Gray16</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="Image">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L26">Image</a>
    <a href="#Image">¶</a></h2>
<pre>type Image interface {
    <span class="comment">// ColorModel returns the Image&#39;s color model.</span>
    ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a>
    <span class="comment">// Bounds returns the domain for which At can return non-zero color.</span>
    <span class="comment">// The bounds do not necessarily contain the point (0, 0).</span>
    Bounds() <a href="#Rectangle">Rectangle</a>
    <span class="comment">// At returns the color of the pixel at (x, y).</span>
    <span class="comment">// At(Bounds().Min.X, Bounds().Min.Y) returns the upper-left pixel of the grid.</span>
    <span class="comment">// At(Bounds().Max.X-1, Bounds().Max.Y-1) returns the lower-right one.</span>
    At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>
}</pre>

Image is a finite rectangular grid of color.Color values taken from a color
model.

<h3 id="Decode">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/format.go#L68">Decode</a>
    <a href="#Decode">¶</a></h3>
<pre>func Decode(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="#Image">Image</a>, <a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

Decode decodes an image that has been encoded in a registered format. The string
returned is the format name used during format registration. Format registration
is typically done by an init function in the codec- specific package.

<h2 id="NRGBA">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L261">NRGBA</a>
    <a href="#NRGBA">¶</a></h2>
<pre>type NRGBA struct {
<span id="NRGBA.Pix"></span>    <span class="comment">// Pix holds the image&#39;s pixels, in R, G, B, A order. The pixel at</span>
    <span class="comment">// (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*4].</span>
    Pix []<a href="/builtin/#uint8">uint8</a>
<span id="NRGBA.Stride"></span>    <span class="comment">// Stride is the Pix stride (in bytes) between vertically adjacent pixels.</span>
    Stride <a href="/builtin/#int">int</a>
<span id="NRGBA.Rect"></span>    <span class="comment">// Rect is the image&#39;s bounds.</span>
    Rect <a href="#Rectangle">Rectangle</a>
}</pre>

NRGBA is an in-memory image whose At method returns color.NRGBA values.

<h3 id="NewNRGBA">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L353">NewNRGBA</a>
    <a href="#NewNRGBA">¶</a></h3>
<pre>func NewNRGBA(r <a href="#Rectangle">Rectangle</a>) *<a href="#NRGBA">NRGBA</a></pre>

NewNRGBA returns a new NRGBA image with the given bounds.

<h3 id="NRGBA.At">func (*NRGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L275">At</a>
    <a href="#NRGBA.At">¶</a></h3>
<pre>func (p *<a href="#NRGBA">NRGBA</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="NRGBA.Bounds">func (*NRGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L273">Bounds</a>
    <a href="#NRGBA.Bounds">¶</a></h3>
<pre>func (p *<a href="#NRGBA">NRGBA</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="NRGBA.ColorModel">func (*NRGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L271">ColorModel</a>
    <a href="#NRGBA.ColorModel">¶</a></h3>
<pre>func (p *<a href="#NRGBA">NRGBA</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="NRGBA.NRGBAAt">func (*NRGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L279">NRGBAAt</a>
    <a href="#NRGBA.NRGBAAt">¶</a></h3>
<pre>func (p *<a href="#NRGBA">NRGBA</a>) NRGBAAt(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#NRGBA">NRGBA</a></pre>


<h3 id="NRGBA.Opaque">func (*NRGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L335">Opaque</a>
    <a href="#NRGBA.Opaque">¶</a></h3>
<pre>func (p *<a href="#NRGBA">NRGBA</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="NRGBA.PixOffset">func (*NRGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L289">PixOffset</a>
    <a href="#NRGBA.PixOffset">¶</a></h3>
<pre>func (p *<a href="#NRGBA">NRGBA</a>) PixOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

PixOffset returns the index of the first element of Pix that corresponds to the
pixel at (x, y).

<h3 id="NRGBA.Set">func (*NRGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L293">Set</a>
    <a href="#NRGBA.Set">¶</a></h3>
<pre>func (p *<a href="#NRGBA">NRGBA</a>) Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)</pre>


<h3 id="NRGBA.SetNRGBA">func (*NRGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L305">SetNRGBA</a>
    <a href="#NRGBA.SetNRGBA">¶</a></h3>
<pre>func (p *<a href="#NRGBA">NRGBA</a>) SetNRGBA(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#NRGBA">NRGBA</a>)</pre>


<h3 id="NRGBA.SubImage">func (*NRGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L318">SubImage</a>
    <a href="#NRGBA.SubImage">¶</a></h3>
<pre>func (p *<a href="#NRGBA">NRGBA</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="NRGBA64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L360">NRGBA64</a>
    <a href="#NRGBA64">¶</a></h2>
<pre>type NRGBA64 struct {
<span id="NRGBA64.Pix"></span>    <span class="comment">// Pix holds the image&#39;s pixels, in R, G, B, A order and big-endian format. The pixel at</span>
    <span class="comment">// (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*8].</span>
    Pix []<a href="/builtin/#uint8">uint8</a>
<span id="NRGBA64.Stride"></span>    <span class="comment">// Stride is the Pix stride (in bytes) between vertically adjacent pixels.</span>
    Stride <a href="/builtin/#int">int</a>
<span id="NRGBA64.Rect"></span>    <span class="comment">// Rect is the image&#39;s bounds.</span>
    Rect <a href="#Rectangle">Rectangle</a>
}</pre>

NRGBA64 is an in-memory image whose At method returns color.NRGBA64 values.

<h3 id="NewNRGBA64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L465">NewNRGBA64</a>
    <a href="#NewNRGBA64">¶</a></h3>
<pre>func NewNRGBA64(r <a href="#Rectangle">Rectangle</a>) *<a href="#NRGBA64">NRGBA64</a></pre>

NewNRGBA64 returns a new NRGBA64 image with the given bounds.

<h3 id="NRGBA64.At">func (*NRGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L374">At</a>
    <a href="#NRGBA64.At">¶</a></h3>
<pre>func (p *<a href="#NRGBA64">NRGBA64</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="NRGBA64.Bounds">func (*NRGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L372">Bounds</a>
    <a href="#NRGBA64.Bounds">¶</a></h3>
<pre>func (p *<a href="#NRGBA64">NRGBA64</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="NRGBA64.ColorModel">func (*NRGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L370">ColorModel</a>
    <a href="#NRGBA64.ColorModel">¶</a></h3>
<pre>func (p *<a href="#NRGBA64">NRGBA64</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="NRGBA64.NRGBA64At">func (*NRGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L378">NRGBA64At</a>
    <a href="#NRGBA64.NRGBA64At">¶</a></h3>
<pre>func (p *<a href="#NRGBA64">NRGBA64</a>) NRGBA64At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#NRGBA64">NRGBA64</a></pre>


<h3 id="NRGBA64.Opaque">func (*NRGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L447">Opaque</a>
    <a href="#NRGBA64.Opaque">¶</a></h3>
<pre>func (p *<a href="#NRGBA64">NRGBA64</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="NRGBA64.PixOffset">func (*NRGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L393">PixOffset</a>
    <a href="#NRGBA64.PixOffset">¶</a></h3>
<pre>func (p *<a href="#NRGBA64">NRGBA64</a>) PixOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

PixOffset returns the index of the first element of Pix that corresponds to the
pixel at (x, y).

<h3 id="NRGBA64.Set">func (*NRGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L397">Set</a>
    <a href="#NRGBA64.Set">¶</a></h3>
<pre>func (p *<a href="#NRGBA64">NRGBA64</a>) Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)</pre>


<h3 id="NRGBA64.SetNRGBA64">func (*NRGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L413">SetNRGBA64</a>
    <a href="#NRGBA64.SetNRGBA64">¶</a></h3>
<pre>func (p *<a href="#NRGBA64">NRGBA64</a>) SetNRGBA64(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#NRGBA64">NRGBA64</a>)</pre>


<h3 id="NRGBA64.SubImage">func (*NRGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L430">SubImage</a>
    <a href="#NRGBA64.SubImage">¶</a></h3>
<pre>func (p *<a href="#NRGBA64">NRGBA64</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="NYCbCrA">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L179">NYCbCrA</a>
    <a href="#NYCbCrA">¶</a></h2>
<pre>type NYCbCrA struct {
    <a href="#YCbCr">YCbCr</a>
<span id="NYCbCrA.A"></span>    A       []<a href="/builtin/#uint8">uint8</a>
<span id="NYCbCrA.AStride"></span>    AStride <a href="/builtin/#int">int</a>
}</pre>

NYCbCrA is an in-memory image of non-alpha-premultiplied Y'CbCr-with-alpha
colors. A and AStride are analogous to the Y and YStride fields of the embedded
YCbCr.

<h3 id="NewNYCbCrA">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L268">NewNYCbCrA</a>
    <a href="#NewNYCbCrA">¶</a></h3>
<pre>func NewNYCbCrA(r <a href="#Rectangle">Rectangle</a>, subsampleRatio <a href="#YCbCrSubsampleRatio">YCbCrSubsampleRatio</a>) *<a href="#NYCbCrA">NYCbCrA</a></pre>

NewNYCbCrA returns a new NYCbCrA image with the given bounds and subsample
ratio.

<h3 id="NYCbCrA.AOffset">func (*NYCbCrA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L212">AOffset</a>
    <a href="#NYCbCrA.AOffset">¶</a></h3>
<pre>func (p *<a href="#NYCbCrA">NYCbCrA</a>) AOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

AOffset returns the index of the first element of A that corresponds to the
pixel at (x, y).

<h3 id="NYCbCrA.At">func (*NYCbCrA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L189">At</a>
    <a href="#NYCbCrA.At">¶</a></h3>
<pre>func (p *<a href="#NYCbCrA">NYCbCrA</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="NYCbCrA.ColorModel">func (*NYCbCrA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L185">ColorModel</a>
    <a href="#NYCbCrA.ColorModel">¶</a></h3>
<pre>func (p *<a href="#NYCbCrA">NYCbCrA</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="NYCbCrA.NYCbCrAAt">func (*NYCbCrA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L193">NYCbCrAAt</a>
    <a href="#NYCbCrA.NYCbCrAAt">¶</a></h3>
<pre>func (p *<a href="#NYCbCrA">NYCbCrA</a>) NYCbCrAAt(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#NYCbCrA">NYCbCrA</a></pre>


<h3 id="NYCbCrA.Opaque">func (*NYCbCrA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L249">Opaque</a>
    <a href="#NYCbCrA.Opaque">¶</a></h3>
<pre>func (p *<a href="#NYCbCrA">NYCbCrA</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="NYCbCrA.SubImage">func (*NYCbCrA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L218">SubImage</a>
    <a href="#NYCbCrA.SubImage">¶</a></h3>
<pre>func (p *<a href="#NYCbCrA">NYCbCrA</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="Paletted">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L906">Paletted</a>
    <a href="#Paletted">¶</a></h2>
<pre>type Paletted struct {
<span id="Paletted.Pix"></span>    <span class="comment">// Pix holds the image&#39;s pixels, as palette indices. The pixel at</span>
    <span class="comment">// (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*1].</span>
    Pix []<a href="/builtin/#uint8">uint8</a>
<span id="Paletted.Stride"></span>    <span class="comment">// Stride is the Pix stride (in bytes) between vertically adjacent pixels.</span>
    Stride <a href="/builtin/#int">int</a>
<span id="Paletted.Rect"></span>    <span class="comment">// Rect is the image&#39;s bounds.</span>
    Rect <a href="#Rectangle">Rectangle</a>
<span id="Paletted.Palette"></span>    <span class="comment">// Palette is the image&#39;s palette.</span>
    Palette <a href="/image/color/">color</a>.<a href="/image/color/#Palette">Palette</a>
}</pre>

Paletted is an in-memory image of uint8 indices into a given palette.

<h3 id="NewPaletted">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L1009">NewPaletted</a>
    <a href="#NewPaletted">¶</a></h3>
<pre>func NewPaletted(r <a href="#Rectangle">Rectangle</a>, p <a href="/image/color/">color</a>.<a href="/image/color/#Palette">Palette</a>) *<a href="#Paletted">Paletted</a></pre>

NewPaletted returns a new Paletted image with the given width, height and
palette.

<h3 id="Paletted.At">func (*Paletted) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L922">At</a>
    <a href="#Paletted.At">¶</a></h3>
<pre>func (p *<a href="#Paletted">Paletted</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="Paletted.Bounds">func (*Paletted) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L920">Bounds</a>
    <a href="#Paletted.Bounds">¶</a></h3>
<pre>func (p *<a href="#Paletted">Paletted</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="Paletted.ColorIndexAt">func (*Paletted) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L947">ColorIndexAt</a>
    <a href="#Paletted.ColorIndexAt">¶</a></h3>
<pre>func (p *<a href="#Paletted">Paletted</a>) ColorIndexAt(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#uint8">uint8</a></pre>


<h3 id="Paletted.ColorModel">func (*Paletted) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L918">ColorModel</a>
    <a href="#Paletted.ColorModel">¶</a></h3>
<pre>func (p *<a href="#Paletted">Paletted</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="Paletted.Opaque">func (*Paletted) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L985">Opaque</a>
    <a href="#Paletted.Opaque">¶</a></h3>
<pre>func (p *<a href="#Paletted">Paletted</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="Paletted.PixOffset">func (*Paletted) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L935">PixOffset</a>
    <a href="#Paletted.PixOffset">¶</a></h3>
<pre>func (p *<a href="#Paletted">Paletted</a>) PixOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

PixOffset returns the index of the first element of Pix that corresponds to the
pixel at (x, y).

<h3 id="Paletted.Set">func (*Paletted) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L939">Set</a>
    <a href="#Paletted.Set">¶</a></h3>
<pre>func (p *<a href="#Paletted">Paletted</a>) Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)</pre>


<h3 id="Paletted.SetColorIndex">func (*Paletted) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L955">SetColorIndex</a>
    <a href="#Paletted.SetColorIndex">¶</a></h3>
<pre>func (p *<a href="#Paletted">Paletted</a>) SetColorIndex(x, y <a href="/builtin/#int">int</a>, index <a href="/builtin/#uint8">uint8</a>)</pre>


<h3 id="Paletted.SubImage">func (*Paletted) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L965">SubImage</a>
    <a href="#Paletted.SubImage">¶</a></h3>
<pre>func (p *<a href="#Paletted">Paletted</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="PalettedImage">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L43">PalettedImage</a>
    <a href="#PalettedImage">¶</a></h2>
<pre>type PalettedImage interface {
    <span class="comment">// ColorIndexAt returns the palette index of the pixel at (x, y).</span>
    ColorIndexAt(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#uint8">uint8</a>
    <a href="#Image">Image</a>
}</pre>

PalettedImage is an image whose colors may come from a limited palette. If m is
a PalettedImage and m.ColorModel() returns a color.Palette p, then m.At(x, y)
should be equivalent to p[m.ColorIndexAt(x, y)]. If m's color model is not a
color.Palette, then ColorIndexAt's behavior is undefined.

<h2 id="Point">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L3">Point</a>
    <a href="#Point">¶</a></h2>
<pre>type Point struct {
<span id="Point.X"></span>    X, Y <a href="/builtin/#int">int</a>
}</pre>

A Point is an X, Y coordinate pair. The axes increase right and down.

<pre>var <span id="ZP">ZP</span> <a href="#Point">Point</a></pre>

ZP is the zero Point.

<h3 id="Pt">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L63">Pt</a>
    <a href="#Pt">¶</a></h3>
<pre>func Pt(X, Y <a href="/builtin/#int">int</a>) <a href="#Point">Point</a></pre>

Pt is shorthand for Point{X, Y}.

<h3 id="Point.Add">func (Point) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L13">Add</a>
    <a href="#Point.Add">¶</a></h3>
<pre>func (p <a href="#Point">Point</a>) Add(q <a href="#Point">Point</a>) <a href="#Point">Point</a></pre>

Add returns the vector p+q.

<h3 id="Point.Div">func (Point) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L28">Div</a>
    <a href="#Point.Div">¶</a></h3>
<pre>func (p <a href="#Point">Point</a>) Div(k <a href="/builtin/#int">int</a>) <a href="#Point">Point</a></pre>

Div returns the vector p/k.

<h3 id="Point.Eq">func (Point) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L55">Eq</a>
    <a href="#Point.Eq">¶</a></h3>
<pre>func (p <a href="#Point">Point</a>) Eq(q <a href="#Point">Point</a>) <a href="/builtin/#bool">bool</a></pre>

Eq reports whether p and q are equal.

<h3 id="Point.In">func (Point) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L33">In</a>
    <a href="#Point.In">¶</a></h3>
<pre>func (p <a href="#Point">Point</a>) In(r <a href="#Rectangle">Rectangle</a>) <a href="/builtin/#bool">bool</a></pre>

In reports whether p is in r.

<h3 id="Point.Mod">func (Point) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L40">Mod</a>
    <a href="#Point.Mod">¶</a></h3>
<pre>func (p <a href="#Point">Point</a>) Mod(r <a href="#Rectangle">Rectangle</a>) <a href="#Point">Point</a></pre>

Mod returns the point q in r such that p.X-q.X is a multiple of r's width and
p.Y-q.Y is a multiple of r's height.

<h3 id="Point.Mul">func (Point) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L23">Mul</a>
    <a href="#Point.Mul">¶</a></h3>
<pre>func (p <a href="#Point">Point</a>) Mul(k <a href="/builtin/#int">int</a>) <a href="#Point">Point</a></pre>

Mul returns the vector p*k.

<h3 id="Point.String">func (Point) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L8">String</a>
    <a href="#Point.String">¶</a></h3>
<pre>func (p <a href="#Point">Point</a>) String() <a href="/builtin/#string">string</a></pre>

String returns a string representation of p like "(3,4)".

<h3 id="Point.Sub">func (Point) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L18">Sub</a>
    <a href="#Point.Sub">¶</a></h3>
<pre>func (p <a href="#Point">Point</a>) Sub(q <a href="#Point">Point</a>) <a href="#Point">Point</a></pre>

Sub returns the vector p-q.

<h2 id="RGBA">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L50">RGBA</a>
    <a href="#RGBA">¶</a></h2>
<pre>type RGBA struct {
<span id="RGBA.Pix"></span>    <span class="comment">// Pix holds the image&#39;s pixels, in R, G, B, A order. The pixel at</span>
    <span class="comment">// (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*4].</span>
    Pix []<a href="/builtin/#uint8">uint8</a>
<span id="RGBA.Stride"></span>    <span class="comment">// Stride is the Pix stride (in bytes) between vertically adjacent pixels.</span>
    Stride <a href="/builtin/#int">int</a>
<span id="RGBA.Rect"></span>    <span class="comment">// Rect is the image&#39;s bounds.</span>
    Rect <a href="#Rectangle">Rectangle</a>
}</pre>

RGBA is an in-memory image whose At method returns color.RGBA values.

<h3 id="NewRGBA">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L142">NewRGBA</a>
    <a href="#NewRGBA">¶</a></h3>
<pre>func NewRGBA(r <a href="#Rectangle">Rectangle</a>) *<a href="#RGBA">RGBA</a></pre>

NewRGBA returns a new RGBA image with the given bounds.

<h3 id="RGBA.At">func (*RGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L64">At</a>
    <a href="#RGBA.At">¶</a></h3>
<pre>func (p *<a href="#RGBA">RGBA</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="RGBA.Bounds">func (*RGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L62">Bounds</a>
    <a href="#RGBA.Bounds">¶</a></h3>
<pre>func (p *<a href="#RGBA">RGBA</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="RGBA.ColorModel">func (*RGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L60">ColorModel</a>
    <a href="#RGBA.ColorModel">¶</a></h3>
<pre>func (p *<a href="#RGBA">RGBA</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="RGBA.Opaque">func (*RGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L124">Opaque</a>
    <a href="#RGBA.Opaque">¶</a></h3>
<pre>func (p *<a href="#RGBA">RGBA</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="RGBA.PixOffset">func (*RGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L78">PixOffset</a>
    <a href="#RGBA.PixOffset">¶</a></h3>
<pre>func (p *<a href="#RGBA">RGBA</a>) PixOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

PixOffset returns the index of the first element of Pix that corresponds to the
pixel at (x, y).

<h3 id="RGBA.RGBAAt">func (*RGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L68">RGBAAt</a>
    <a href="#RGBA.RGBAAt">¶</a></h3>
<pre>func (p *<a href="#RGBA">RGBA</a>) RGBAAt(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#RGBA">RGBA</a></pre>


<h3 id="RGBA.Set">func (*RGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L82">Set</a>
    <a href="#RGBA.Set">¶</a></h3>
<pre>func (p *<a href="#RGBA">RGBA</a>) Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)</pre>


<h3 id="RGBA.SetRGBA">func (*RGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L94">SetRGBA</a>
    <a href="#RGBA.SetRGBA">¶</a></h3>
<pre>func (p *<a href="#RGBA">RGBA</a>) SetRGBA(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#RGBA">RGBA</a>)</pre>


<h3 id="RGBA.SubImage">func (*RGBA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L107">SubImage</a>
    <a href="#RGBA.SubImage">¶</a></h3>
<pre>func (p *<a href="#RGBA">RGBA</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="RGBA64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L149">RGBA64</a>
    <a href="#RGBA64">¶</a></h2>
<pre>type RGBA64 struct {
<span id="RGBA64.Pix"></span>    <span class="comment">// Pix holds the image&#39;s pixels, in R, G, B, A order and big-endian format. The pixel at</span>
    <span class="comment">// (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*8].</span>
    Pix []<a href="/builtin/#uint8">uint8</a>
<span id="RGBA64.Stride"></span>    <span class="comment">// Stride is the Pix stride (in bytes) between vertically adjacent pixels.</span>
    Stride <a href="/builtin/#int">int</a>
<span id="RGBA64.Rect"></span>    <span class="comment">// Rect is the image&#39;s bounds.</span>
    Rect <a href="#Rectangle">Rectangle</a>
}</pre>

RGBA64 is an in-memory image whose At method returns color.RGBA64 values.

<h3 id="NewRGBA64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L254">NewRGBA64</a>
    <a href="#NewRGBA64">¶</a></h3>
<pre>func NewRGBA64(r <a href="#Rectangle">Rectangle</a>) *<a href="#RGBA64">RGBA64</a></pre>

NewRGBA64 returns a new RGBA64 image with the given bounds.

<h3 id="RGBA64.At">func (*RGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L163">At</a>
    <a href="#RGBA64.At">¶</a></h3>
<pre>func (p *<a href="#RGBA64">RGBA64</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="RGBA64.Bounds">func (*RGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L161">Bounds</a>
    <a href="#RGBA64.Bounds">¶</a></h3>
<pre>func (p *<a href="#RGBA64">RGBA64</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="RGBA64.ColorModel">func (*RGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L159">ColorModel</a>
    <a href="#RGBA64.ColorModel">¶</a></h3>
<pre>func (p *<a href="#RGBA64">RGBA64</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="RGBA64.Opaque">func (*RGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L236">Opaque</a>
    <a href="#RGBA64.Opaque">¶</a></h3>
<pre>func (p *<a href="#RGBA64">RGBA64</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="RGBA64.PixOffset">func (*RGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L182">PixOffset</a>
    <a href="#RGBA64.PixOffset">¶</a></h3>
<pre>func (p *<a href="#RGBA64">RGBA64</a>) PixOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

PixOffset returns the index of the first element of Pix that corresponds to the
pixel at (x, y).

<h3 id="RGBA64.RGBA64At">func (*RGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L167">RGBA64At</a>
    <a href="#RGBA64.RGBA64At">¶</a></h3>
<pre>func (p *<a href="#RGBA64">RGBA64</a>) RGBA64At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#RGBA64">RGBA64</a></pre>


<h3 id="RGBA64.Set">func (*RGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L186">Set</a>
    <a href="#RGBA64.Set">¶</a></h3>
<pre>func (p *<a href="#RGBA64">RGBA64</a>) Set(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>)</pre>


<h3 id="RGBA64.SetRGBA64">func (*RGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L202">SetRGBA64</a>
    <a href="#RGBA64.SetRGBA64">¶</a></h3>
<pre>func (p *<a href="#RGBA64">RGBA64</a>) SetRGBA64(x, y <a href="/builtin/#int">int</a>, c <a href="/image/color/">color</a>.<a href="/image/color/#RGBA64">RGBA64</a>)</pre>


<h3 id="RGBA64.SubImage">func (*RGBA64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/image.go#L219">SubImage</a>
    <a href="#RGBA64.SubImage">¶</a></h3>
<pre>func (p *<a href="#RGBA64">RGBA64</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h2 id="Rectangle">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L75">Rectangle</a>
    <a href="#Rectangle">¶</a></h2>
<pre>type Rectangle struct {
<span id="Rectangle.Min"></span>    Min, Max <a href="#Point">Point</a>
}</pre>

A Rectangle contains the points with Min.X <= X < Max.X, Min.Y <= Y < Max.Y. It
is well-formed if Min.X <= Max.X and likewise for Y. Points are always
well-formed. A rectangle's methods always return well-formed outputs for
well-formed inputs.

A Rectangle is also an Image whose bounds are the rectangle itself. At returns
color.Opaque for points in the rectangle and color.Transparent otherwise.

<pre>var <span id="ZR">ZR</span> <a href="#Rectangle">Rectangle</a></pre>

ZR is the zero Rectangle.

<h3 id="Rect">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L252">Rect</a>
    <a href="#Rect">¶</a></h3>
<pre>func Rect(x0, y0, x1, y1 <a href="/builtin/#int">int</a>) <a href="#Rectangle">Rectangle</a></pre>

Rect is shorthand for Rectangle{Pt(x0, y0), Pt(x1, y1)}. The returned rectangle
has minimum and maximum coordinates swapped if necessary so that it is
well-formed.

<h3 id="Rectangle.Add">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L103">Add</a>
    <a href="#Rectangle.Add">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Add(p <a href="#Point">Point</a>) <a href="#Rectangle">Rectangle</a></pre>

Add returns the rectangle r translated by p.

<h3 id="Rectangle.At">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L229">At</a>
    <a href="#Rectangle.At">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>

At implements the Image interface.

<h3 id="Rectangle.Bounds">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L237">Bounds</a>
    <a href="#Rectangle.Bounds">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>

Bounds implements the Image interface.

<h3 id="Rectangle.Canon">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L218">Canon</a>
    <a href="#Rectangle.Canon">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Canon() <a href="#Rectangle">Rectangle</a></pre>

Canon returns the canonical version of r. The returned rectangle has minimum and
maximum coordinates swapped if necessary so that it is well-formed.

<h3 id="Rectangle.ColorModel">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L242">ColorModel</a>
    <a href="#Rectangle.ColorModel">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>

ColorModel implements the Image interface.

<h3 id="Rectangle.Dx">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L85">Dx</a>
    <a href="#Rectangle.Dx">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Dx() <a href="/builtin/#int">int</a></pre>

Dx returns r's width.

<h3 id="Rectangle.Dy">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L90">Dy</a>
    <a href="#Rectangle.Dy">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Dy() <a href="/builtin/#int">int</a></pre>

Dy returns r's height.

<h3 id="Rectangle.Empty">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L188">Empty</a>
    <a href="#Rectangle.Empty">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Empty() <a href="/builtin/#bool">bool</a></pre>

Empty reports whether the rectangle contains no points.

<h3 id="Rectangle.Eq">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L194">Eq</a>
    <a href="#Rectangle.Eq">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Eq(s <a href="#Rectangle">Rectangle</a>) <a href="/builtin/#bool">bool</a></pre>

Eq reports whether r and s contain the same set of points. All empty rectangles
are considered equal.

<h3 id="Rectangle.In">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L206">In</a>
    <a href="#Rectangle.In">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) In(s <a href="#Rectangle">Rectangle</a>) <a href="/builtin/#bool">bool</a></pre>

In reports whether every point in r is in s.

<h3 id="Rectangle.Inset">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L121">Inset</a>
    <a href="#Rectangle.Inset">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Inset(n <a href="/builtin/#int">int</a>) <a href="#Rectangle">Rectangle</a></pre>

Inset returns the rectangle r inset by n, which may be negative. If either of
r's dimensions is less than 2*n then an empty rectangle near the center of r
will be returned.

<h3 id="Rectangle.Intersect">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L141">Intersect</a>
    <a href="#Rectangle.Intersect">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Intersect(s <a href="#Rectangle">Rectangle</a>) <a href="#Rectangle">Rectangle</a></pre>

Intersect returns the largest rectangle contained by both r and s. If the two
rectangles do not overlap then the zero rectangle will be returned.

<h3 id="Rectangle.Overlaps">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L199">Overlaps</a>
    <a href="#Rectangle.Overlaps">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Overlaps(s <a href="#Rectangle">Rectangle</a>) <a href="/builtin/#bool">bool</a></pre>

Overlaps reports whether r and s have a non-empty intersection.

<h3 id="Rectangle.Size">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L95">Size</a>
    <a href="#Rectangle.Size">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Size() <a href="#Point">Point</a></pre>

Size returns r's width and height.

<h3 id="Rectangle.String">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L80">String</a>
    <a href="#Rectangle.String">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) String() <a href="/builtin/#string">string</a></pre>

String returns a string representation of r like "(3,4)-(6,5)".

<h3 id="Rectangle.Sub">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L111">Sub</a>
    <a href="#Rectangle.Sub">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Sub(p <a href="#Point">Point</a>) <a href="#Rectangle">Rectangle</a></pre>

Sub returns the rectangle r translated by -p.

<h3 id="Rectangle.Union">func (Rectangle) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/geom.go#L165">Union</a>
    <a href="#Rectangle.Union">¶</a></h3>
<pre>func (r <a href="#Rectangle">Rectangle</a>) Union(s <a href="#Rectangle">Rectangle</a>) <a href="#Rectangle">Rectangle</a></pre>

Union returns the smallest rectangle that contains both r and s.

<h2 id="Uniform">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/names.go#L14">Uniform</a>
    <a href="#Uniform">¶</a></h2>
<pre>type Uniform struct {
<span id="Uniform.C"></span>    C <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>
}</pre>

Uniform is an infinite-sized Image of uniform color. It implements the
color.Color, color.Model, and Image interfaces.

<h3 id="NewUniform">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/names.go#L40">NewUniform</a>
    <a href="#NewUniform">¶</a></h3>
<pre>func NewUniform(c <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>) *<a href="#Uniform">Uniform</a></pre>


<h3 id="Uniform.At">func (*Uniform) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/names.go#L32">At</a>
    <a href="#Uniform.At">¶</a></h3>
<pre>func (c *<a href="#Uniform">Uniform</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="Uniform.Bounds">func (*Uniform) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/names.go#L30">Bounds</a>
    <a href="#Uniform.Bounds">¶</a></h3>
<pre>func (c *<a href="#Uniform">Uniform</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="Uniform.ColorModel">func (*Uniform) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/names.go#L22">ColorModel</a>
    <a href="#Uniform.ColorModel">¶</a></h3>
<pre>func (c *<a href="#Uniform">Uniform</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="Uniform.Convert">func (*Uniform) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/names.go#L26">Convert</a>
    <a href="#Uniform.Convert">¶</a></h3>
<pre>func (c *<a href="#Uniform">Uniform</a>) Convert(<a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="Uniform.Opaque">func (*Uniform) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/names.go#L35">Opaque</a>
    <a href="#Uniform.Opaque">¶</a></h3>
<pre>func (c *<a href="#Uniform">Uniform</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>

Opaque scans the entire image and reports whether it is fully opaque.

<h3 id="Uniform.RGBA">func (*Uniform) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/names.go#L18">RGBA</a>
    <a href="#Uniform.RGBA">¶</a></h3>
<pre>func (c *<a href="#Uniform">Uniform</a>) RGBA() (r, g, b, a <a href="/builtin/#uint32">uint32</a>)</pre>


<h2 id="YCbCr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L44">YCbCr</a>
    <a href="#YCbCr">¶</a></h2>
<pre>type YCbCr struct {
<span id="YCbCr.Y"></span>    Y, Cb, Cr      []<a href="/builtin/#uint8">uint8</a>
<span id="YCbCr.YStride"></span>    YStride        <a href="/builtin/#int">int</a>
<span id="YCbCr.CStride"></span>    CStride        <a href="/builtin/#int">int</a>
<span id="YCbCr.SubsampleRatio"></span>    SubsampleRatio <a href="#YCbCrSubsampleRatio">YCbCrSubsampleRatio</a>
<span id="YCbCr.Rect"></span>    Rect           <a href="#Rectangle">Rectangle</a>
}</pre>

YCbCr is an in-memory image of Y'CbCr colors. There is one Y sample per pixel,
but each Cb and Cr sample can span one or more pixels. YStride is the Y slice
index delta between vertically adjacent pixels. CStride is the Cb and Cr slice
index delta between vertically adjacent pixels that map to separate chroma
samples. It is not an absolute requirement, but YStride and len(Y) are typically
multiples of 8, and:

    For 4:4:4, CStride == YStride/1 && len(Cb) == len(Cr) == len(Y)/1.
    For 4:2:2, CStride == YStride/2 && len(Cb) == len(Cr) == len(Y)/2.
    For 4:2:0, CStride == YStride/2 && len(Cb) == len(Cr) == len(Y)/4.
    For 4:4:0, CStride == YStride/1 && len(Cb) == len(Cr) == len(Y)/2.
    For 4:1:1, CStride == YStride/4 && len(Cb) == len(Cr) == len(Y)/4.
    For 4:1:0, CStride == YStride/4 && len(Cb) == len(Cr) == len(Y)/8.

<h3 id="NewYCbCr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L159">NewYCbCr</a>
    <a href="#NewYCbCr">¶</a></h3>
<pre>func NewYCbCr(r <a href="#Rectangle">Rectangle</a>, subsampleRatio <a href="#YCbCrSubsampleRatio">YCbCrSubsampleRatio</a>) *<a href="#YCbCr">YCbCr</a></pre>

NewYCbCr returns a new YCbCr image with the given bounds and subsample ratio.

<h3 id="YCbCr.At">func (*YCbCr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L60">At</a>
    <a href="#YCbCr.At">¶</a></h3>
<pre>func (p *<a href="#YCbCr">YCbCr</a>) At(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#Color">Color</a></pre>


<h3 id="YCbCr.Bounds">func (*YCbCr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L56">Bounds</a>
    <a href="#YCbCr.Bounds">¶</a></h3>
<pre>func (p *<a href="#YCbCr">YCbCr</a>) Bounds() <a href="#Rectangle">Rectangle</a></pre>


<h3 id="YCbCr.COffset">func (*YCbCr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L85">COffset</a>
    <a href="#YCbCr.COffset">¶</a></h3>
<pre>func (p *<a href="#YCbCr">YCbCr</a>) COffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

COffset returns the index of the first element of Cb or Cr that corresponds to
the pixel at (x, y).

<h3 id="YCbCr.ColorModel">func (*YCbCr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L52">ColorModel</a>
    <a href="#YCbCr.ColorModel">¶</a></h3>
<pre>func (p *<a href="#YCbCr">YCbCr</a>) ColorModel() <a href="/image/color/">color</a>.<a href="/image/color/#Model">Model</a></pre>


<h3 id="YCbCr.Opaque">func (*YCbCr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L127">Opaque</a>
    <a href="#YCbCr.Opaque">¶</a></h3>
<pre>func (p *<a href="#YCbCr">YCbCr</a>) Opaque() <a href="/builtin/#bool">bool</a></pre>


<h3 id="YCbCr.SubImage">func (*YCbCr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L104">SubImage</a>
    <a href="#YCbCr.SubImage">¶</a></h3>
<pre>func (p *<a href="#YCbCr">YCbCr</a>) SubImage(r <a href="#Rectangle">Rectangle</a>) <a href="#Image">Image</a></pre>

SubImage returns an image representing the portion of the image p visible
through r. The returned value shares pixels with the original image.

<h3 id="YCbCr.YCbCrAt">func (*YCbCr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L64">YCbCrAt</a>
    <a href="#YCbCr.YCbCrAt">¶</a></h3>
<pre>func (p *<a href="#YCbCr">YCbCr</a>) YCbCrAt(x, y <a href="/builtin/#int">int</a>) <a href="/image/color/">color</a>.<a href="/image/color/#YCbCr">YCbCr</a></pre>


<h3 id="YCbCr.YOffset">func (*YCbCr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L79">YOffset</a>
    <a href="#YCbCr.YOffset">¶</a></h3>
<pre>func (p *<a href="#YCbCr">YCbCr</a>) YOffset(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

YOffset returns the index of the first element of Y that corresponds to the
pixel at (x, y).

<h2 id="YCbCrSubsampleRatio">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L2">YCbCrSubsampleRatio</a>
    <a href="#YCbCrSubsampleRatio">¶</a></h2>
<pre>type YCbCrSubsampleRatio <a href="/builtin/#int">int</a></pre>

YCbCrSubsampleRatio is the chroma subsample ratio used in a YCbCr image.

<pre>const (
    <span id="YCbCrSubsampleRatio444">YCbCrSubsampleRatio444</span> <a href="#YCbCrSubsampleRatio">YCbCrSubsampleRatio</a> = <a href="/builtin/#iota">iota</a>
    <span id="YCbCrSubsampleRatio422">YCbCrSubsampleRatio422</span>
    <span id="YCbCrSubsampleRatio420">YCbCrSubsampleRatio420</span>
    <span id="YCbCrSubsampleRatio440">YCbCrSubsampleRatio440</span>
    <span id="YCbCrSubsampleRatio411">YCbCrSubsampleRatio411</span>
    <span id="YCbCrSubsampleRatio410">YCbCrSubsampleRatio410</span>
)</pre>


<h3 id="YCbCrSubsampleRatio.String">func (YCbCrSubsampleRatio) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/image/ycbcr.go#L13">String</a>
    <a href="#YCbCrSubsampleRatio.String">¶</a></h3>
<pre>func (s <a href="#YCbCrSubsampleRatio">YCbCrSubsampleRatio</a>) String() <a href="/builtin/#string">string</a></pre>


## Subdirectories
- [..](..)
- [color](color/)
- [draw](draw/)
- [gif](gif/)
- [jpeg](jpeg/)
- [png](png/)
