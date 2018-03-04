version: 1.10
## package binary

  `import "encoding/binary"`

## Overview

Package binary implements simple translation between numbers and byte sequences
and encoding and decoding of varints.

Numbers are translated by reading and writing fixed-size values. A fixed-size
value is either a fixed-size arithmetic type (bool, int8, uint8, int16, float32,
complex64, ...) or an array or struct containing only fixed-size values.

The varint functions encode and decode single integer values using a
variable-length encoding; smaller values require fewer bytes. For a
specification, see https://developers.google.com/protocol-buffers/docs/encoding.

This package favors simplicity over efficiency. Clients that require
high-performance serialization, especially for large data structures, should
look at more advanced solutions such as the encoding/gob package or protocol
buffers.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func PutUvarint(buf []byte, x uint64) int](#PutUvarint)
- [func PutVarint(buf []byte, x int64) int](#PutVarint)
- [func Read(r io.Reader, order ByteOrder, data interface{}) error](#Read)
- [func ReadUvarint(r io.ByteReader) (uint64, error)](#ReadUvarint)
- [func ReadVarint(r io.ByteReader) (int64, error)](#ReadVarint)
- [func Size(v interface{}) int](#Size)
- [func Uvarint(buf []byte) (uint64, int)](#Uvarint)
- [func Varint(buf []byte) (int64, int)](#Varint)
- [func Write(w io.Writer, order ByteOrder, data interface{}) error](#Write)
- [type ByteOrder](#ByteOrder)

### Examples

- [ByteOrder (Get)](#exampleByteOrder_get)
- [ByteOrder (Put)](#exampleByteOrder_put)
- [PutUvarint](#examplePutUvarint)
- [PutVarint](#examplePutVarint)
- [Read](#exampleRead)
- [Read (Multi)](#exampleRead_multi)
- [Uvarint](#exampleUvarint)
- [Varint](#exampleVarint)
- [Write](#exampleWrite)
- [Write (Multi)](#exampleWrite_multi)

### Package files
 [binary.go](//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/binary.go) [varint.go](//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/varint.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="MaxVarintLen16">MaxVarintLen16</span> = 3
    <span id="MaxVarintLen32">MaxVarintLen32</span> = 5
    <span id="MaxVarintLen64">MaxVarintLen64</span> = 10
)</pre>

MaxVarintLenN is the maximum length of a varint-encoded N-bit integer.

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="BigEndian">BigEndian</span> bigEndian</pre>

BigEndian is the big-endian implementation of ByteOrder.

<pre>var <span id="LittleEndian">LittleEndian</span> littleEndian</pre>

LittleEndian is the little-endian implementation of ByteOrder.

<h2 id="PutUvarint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/varint.go#L31">PutUvarint</a>
    <a href="#PutUvarint">¶</a></h2>
<pre>func PutUvarint(buf []<a href="/builtin/#byte">byte</a>, x <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#int">int</a></pre>

PutUvarint encodes a uint64 into buf and returns the number of bytes written. If
the buffer is too small, PutUvarint will panic.

<a id="examplePutUvarint"></a>
Example:

    buf := make([]byte, binary.MaxVarintLen64)

    for _, x := range []uint64{1, 2, 127, 128, 255, 256} {
        n := binary.PutUvarint(buf, x)
        fmt.Printf("%x\n", buf[:n])
    }
    // Output:
    // 01
    // 02
    // 7f
    // 8001
    // ff01
    // 8002

<h2 id="PutVarint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/varint.go#L68">PutVarint</a>
    <a href="#PutVarint">¶</a></h2>
<pre>func PutVarint(buf []<a href="/builtin/#byte">byte</a>, x <a href="/builtin/#int64">int64</a>) <a href="/builtin/#int">int</a></pre>

PutVarint encodes an int64 into buf and returns the number of bytes written. If
the buffer is too small, PutVarint will panic.

<a id="examplePutVarint"></a>
Example:

    buf := make([]byte, binary.MaxVarintLen64)

    for _, x := range []int64{-65, -64, -2, -1, 0, 1, 2, 63, 64} {
        n := binary.PutVarint(buf, x)
        fmt.Printf("%x\n", buf[:n])
    }
    // Output:
    // 8101
    // 7f
    // 03
    // 01
    // 00
    // 02
    // 04
    // 7e
    // 8001

<h2 id="Read">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/binary.go#L151">Read</a>
    <a href="#Read">¶</a></h2>
<pre>func Read(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, order <a href="#ByteOrder">ByteOrder</a>, data interface{}) <a href="/builtin/#error">error</a></pre>

Read reads structured binary data from r into data. Data must be a pointer to a
fixed-size value or a slice of fixed-size values. Bytes read from r are decoded
using the specified byte order and written to successive fields of the data.
When decoding boolean values, a zero byte is decoded as false, and any other
non-zero byte is decoded as true. When reading into structs, the field data for
fields with blank (_) field names is skipped; i.e., blank field names may be
used for padding. When reading into a struct, all non-blank fields must be
exported or Read may panic.

The error is EOF only if no bytes were read. If an EOF happens after reading
some but not all the bytes, Read returns ErrUnexpectedEOF.

<a id="exampleRead"></a>
Example:

    var pi float64
    b := []byte{0x18, 0x2d, 0x44, 0x54, 0xfb, 0x21, 0x09, 0x40}
    buf := bytes.NewReader(b)
    err := binary.Read(buf, binary.LittleEndian, &pi)
    if err != nil {
        fmt.Println("binary.Read failed:", err)
    }
    fmt.Print(pi)
    // Output: 3.141592653589793


<a id="exampleRead_multi"></a>
Example:

    b := []byte{0x18, 0x2d, 0x44, 0x54, 0xfb, 0x21, 0x09, 0x40, 0xff, 0x01, 0x02, 0x03, 0xbe, 0xef}
    r := bytes.NewReader(b)

    var data struct {
        PI   float64
        Uate uint8
        Mine [3]byte
        Too  uint16
    }

    if err := binary.Read(r, binary.LittleEndian, &data); err != nil {
        fmt.Println("binary.Read failed:", err)
    }

    fmt.Println(data.PI)
    fmt.Println(data.Uate)
    fmt.Printf("% x\n", data.Mine)
    fmt.Println(data.Too)
    // Output:
    // 3.141592653589793
    // 255
    // 01 02 03
    // 61374

<h2 id="ReadUvarint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/varint.go#L96">ReadUvarint</a>
    <a href="#ReadUvarint">¶</a></h2>
<pre>func ReadUvarint(r <a href="/io/">io</a>.<a href="/io/#ByteReader">ByteReader</a>) (<a href="/builtin/#uint64">uint64</a>, <a href="/builtin/#error">error</a>)</pre>

ReadUvarint reads an encoded unsigned integer from r and returns it as a uint64.

<h2 id="ReadVarint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/varint.go#L116">ReadVarint</a>
    <a href="#ReadVarint">¶</a></h2>
<pre>func ReadVarint(r <a href="/io/">io</a>.<a href="/io/#ByteReader">ByteReader</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>

ReadVarint reads an encoded signed integer from r and returns it as an int64.

<h2 id="Size">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/binary.go#L364">Size</a>
    <a href="#Size">¶</a></h2>
<pre>func Size(v interface{}) <a href="/builtin/#int">int</a></pre>

Size returns how many bytes Write would generate to encode the value v, which
must be a fixed-size value or a slice of fixed-size values, or a pointer to such
data. If v is neither of these, Size returns -1.

<h2 id="Uvarint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/varint.go#L50">Uvarint</a>
    <a href="#Uvarint">¶</a></h2>
<pre>func Uvarint(buf []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#uint64">uint64</a>, <a href="/builtin/#int">int</a>)</pre>

Uvarint decodes a uint64 from buf and returns that value and the number of bytes
read (> 0). If an error occurred, the value is 0 and the number of bytes n is <=
0 meaning:

    n == 0: buf too small
    n  < 0: value larger than 64 bits (overflow)
            and -n is the number of bytes read

<a id="exampleUvarint"></a>
Example:

    inputs := [][]byte{
        []byte{0x01},
        []byte{0x02},
        []byte{0x7f},
        []byte{0x80, 0x01},
        []byte{0xff, 0x01},
        []byte{0x80, 0x02},
    }
    for _, b := range inputs {
        x, n := binary.Uvarint(b)
        if n != len(b) {
            fmt.Println("Uvarint did not consume all of in")
        }
        fmt.Println(x)
    }
    // Output:
    // 1
    // 2
    // 127
    // 128
    // 255
    // 256

<h2 id="Varint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/varint.go#L84">Varint</a>
    <a href="#Varint">¶</a></h2>
<pre>func Varint(buf []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#int">int</a>)</pre>

Varint decodes an int64 from buf and returns that value and the number of bytes
read (> 0). If an error occurred, the value is 0 and the number of bytes n is <=
0 with the following meaning:

    n == 0: buf too small
    n  < 0: value larger than 64 bits (overflow)
            and -n is the number of bytes read

<a id="exampleVarint"></a>
Example:

    inputs := [][]byte{
        []byte{0x81, 0x01},
        []byte{0x7f},
        []byte{0x03},
        []byte{0x01},
        []byte{0x00},
        []byte{0x02},
        []byte{0x04},
        []byte{0x7e},
        []byte{0x80, 0x01},
    }
    for _, b := range inputs {
        x, n := binary.Varint(b)
        if n != len(b) {
            fmt.Println("Varint did not consume all of in")
        }
        fmt.Println(x)
    }
    // Output:
    // -65
    // -64
    // -2
    // -1
    // 0
    // 1
    // 2
    // 63
    // 64

<h2 id="Write">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/binary.go#L250">Write</a>
    <a href="#Write">¶</a></h2>
<pre>func Write(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, order <a href="#ByteOrder">ByteOrder</a>, data interface{}) <a href="/builtin/#error">error</a></pre>

Write writes the binary representation of data into w. Data must be a fixed-size
value or a slice of fixed-size values, or a pointer to such data. Boolean values
encode as one byte: 1 for true, and 0 for false. Bytes written to w are encoded
using the specified byte order and read from successive fields of the data. When
writing structs, zero values are written for fields with blank (_) field names.

<a id="exampleWrite"></a>
Example:

    buf := new(bytes.Buffer)
    var pi float64 = math.Pi
    err := binary.Write(buf, binary.LittleEndian, pi)
    if err != nil {
        fmt.Println("binary.Write failed:", err)
    }
    fmt.Printf("% x", buf.Bytes())
    // Output: 18 2d 44 54 fb 21 09 40


<a id="exampleWrite_multi"></a>
Example:

    buf := new(bytes.Buffer)
    var data = []interface{}{
        uint16(61374),
        int8(-54),
        uint8(254),
    }
    for _, v := range data {
        err := binary.Write(buf, binary.LittleEndian, v)
        if err != nil {
            fmt.Println("binary.Write failed:", err)
        }
    }
    fmt.Printf("%x", buf.Bytes())
    // Output: beefcafe

<h2 id="ByteOrder">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/encoding/binary/binary.go#L23">ByteOrder</a>
    <a href="#ByteOrder">¶</a></h2>
<pre>type ByteOrder interface {
    Uint16([]<a href="/builtin/#byte">byte</a>) <a href="/builtin/#uint16">uint16</a>
    Uint32([]<a href="/builtin/#byte">byte</a>) <a href="/builtin/#uint32">uint32</a>
    Uint64([]<a href="/builtin/#byte">byte</a>) <a href="/builtin/#uint64">uint64</a>
    PutUint16([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#uint16">uint16</a>)
    PutUint32([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#uint32">uint32</a>)
    PutUint64([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#uint64">uint64</a>)
    String() <a href="/builtin/#string">string</a>
}</pre>

A ByteOrder specifies how to convert byte sequences into 16-, 32-, or 64-bit
unsigned integers.

<a id="exampleByteOrder_get"></a>
Example:

    b := []byte{0xe8, 0x03, 0xd0, 0x07}
    x1 := binary.LittleEndian.Uint16(b[0:])
    x2 := binary.LittleEndian.Uint16(b[2:])
    fmt.Printf("%#04x %#04x\n", x1, x2)
    // Output:
    // 0x03e8 0x07d0


<a id="exampleByteOrder_put"></a>
Example:

    b := make([]byte, 4)
    binary.LittleEndian.PutUint16(b[0:], 0x03e8)
    binary.LittleEndian.PutUint16(b[2:], 0x07d0)
    fmt.Printf("% x\n", b)
    // Output:
    // e8 03 d0 07


