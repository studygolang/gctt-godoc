version: 1.10
## package bits

  `import "math/bits"`

## Overview

Package bits implements bit counting and manipulation functions for the
predeclared unsigned integer types.

## Index

- [Constants](#pkg-constants)
- [func LeadingZeros(x uint) int](#LeadingZeros)
- [func LeadingZeros16(x uint16) int](#LeadingZeros16)
- [func LeadingZeros32(x uint32) int](#LeadingZeros32)
- [func LeadingZeros64(x uint64) int](#LeadingZeros64)
- [func LeadingZeros8(x uint8) int](#LeadingZeros8)
- [func Len(x uint) int](#Len)
- [func Len16(x uint16) (n int)](#Len16)
- [func Len32(x uint32) (n int)](#Len32)
- [func Len64(x uint64) (n int)](#Len64)
- [func Len8(x uint8) int](#Len8)
- [func OnesCount(x uint) int](#OnesCount)
- [func OnesCount16(x uint16) int](#OnesCount16)
- [func OnesCount32(x uint32) int](#OnesCount32)
- [func OnesCount64(x uint64) int](#OnesCount64)
- [func OnesCount8(x uint8) int](#OnesCount8)
- [func Reverse(x uint) uint](#Reverse)
- [func Reverse16(x uint16) uint16](#Reverse16)
- [func Reverse32(x uint32) uint32](#Reverse32)
- [func Reverse64(x uint64) uint64](#Reverse64)
- [func Reverse8(x uint8) uint8](#Reverse8)
- [func ReverseBytes(x uint) uint](#ReverseBytes)
- [func ReverseBytes16(x uint16) uint16](#ReverseBytes16)
- [func ReverseBytes32(x uint32) uint32](#ReverseBytes32)
- [func ReverseBytes64(x uint64) uint64](#ReverseBytes64)
- [func RotateLeft(x uint, k int) uint](#RotateLeft)
- [func RotateLeft16(x uint16, k int) uint16](#RotateLeft16)
- [func RotateLeft32(x uint32, k int) uint32](#RotateLeft32)
- [func RotateLeft64(x uint64, k int) uint64](#RotateLeft64)
- [func RotateLeft8(x uint8, k int) uint8](#RotateLeft8)
- [func TrailingZeros(x uint) int](#TrailingZeros)
- [func TrailingZeros16(x uint16) (n int)](#TrailingZeros16)
- [func TrailingZeros32(x uint32) int](#TrailingZeros32)
- [func TrailingZeros64(x uint64) int](#TrailingZeros64)
- [func TrailingZeros8(x uint8) int](#TrailingZeros8)

### Examples

- [LeadingZeros16](#exampleLeadingZeros16)
- [LeadingZeros32](#exampleLeadingZeros32)
- [LeadingZeros64](#exampleLeadingZeros64)
- [LeadingZeros8](#exampleLeadingZeros8)
- [Len16](#exampleLen16)
- [Len32](#exampleLen32)
- [Len64](#exampleLen64)
- [Len8](#exampleLen8)
- [OnesCount16](#exampleOnesCount16)
- [OnesCount32](#exampleOnesCount32)
- [OnesCount64](#exampleOnesCount64)
- [OnesCount8](#exampleOnesCount8)
- [Reverse16](#exampleReverse16)
- [Reverse32](#exampleReverse32)
- [Reverse64](#exampleReverse64)
- [Reverse8](#exampleReverse8)
- [ReverseBytes16](#exampleReverseBytes16)
- [ReverseBytes32](#exampleReverseBytes32)
- [ReverseBytes64](#exampleReverseBytes64)
- [RotateLeft16](#exampleRotateLeft16)
- [RotateLeft32](#exampleRotateLeft32)
- [RotateLeft64](#exampleRotateLeft64)
- [RotateLeft8](#exampleRotateLeft8)
- [TrailingZeros16](#exampleTrailingZeros16)
- [TrailingZeros32](#exampleTrailingZeros32)
- [TrailingZeros64](#exampleTrailingZeros64)
- [TrailingZeros8](#exampleTrailingZeros8)

### Package files
 [bits.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go) [bits_tables.go](//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits_tables.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="UintSize">UintSize</span> = uintSize</pre>

UintSize is the size of a uint in bits.

<h2 id="LeadingZeros">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L9">LeadingZeros</a>
    <a href="#LeadingZeros">¶</a></h2>
<pre>func LeadingZeros(x <a href="/builtin/#uint">uint</a>) <a href="/builtin/#int">int</a></pre>

LeadingZeros returns the number of leading zero bits in x; the result is
UintSize for x == 0.

<h2 id="LeadingZeros16">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L15">LeadingZeros16</a>
    <a href="#LeadingZeros16">¶</a></h2>
<pre>func LeadingZeros16(x <a href="/builtin/#uint16">uint16</a>) <a href="/builtin/#int">int</a></pre>

LeadingZeros16 returns the number of leading zero bits in x; the result is 16
for x == 0.

<a id="exampleLeadingZeros16"></a>
Example:

    fmt.Printf("LeadingZeros16(%016b) = %d\n", 1, bits.LeadingZeros16(1))
    // Output:
    // LeadingZeros16(0000000000000001) = 15

<h2 id="LeadingZeros32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L18">LeadingZeros32</a>
    <a href="#LeadingZeros32">¶</a></h2>
<pre>func LeadingZeros32(x <a href="/builtin/#uint32">uint32</a>) <a href="/builtin/#int">int</a></pre>

LeadingZeros32 returns the number of leading zero bits in x; the result is 32
for x == 0.

<a id="exampleLeadingZeros32"></a>
Example:

    fmt.Printf("LeadingZeros32(%032b) = %d\n", 1, bits.LeadingZeros32(1))
    // Output:
    // LeadingZeros32(00000000000000000000000000000001) = 31

<h2 id="LeadingZeros64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L21">LeadingZeros64</a>
    <a href="#LeadingZeros64">¶</a></h2>
<pre>func LeadingZeros64(x <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#int">int</a></pre>

LeadingZeros64 returns the number of leading zero bits in x; the result is 64
for x == 0.

<a id="exampleLeadingZeros64"></a>
Example:

    fmt.Printf("LeadingZeros64(%064b) = %d\n", 1, bits.LeadingZeros64(1))
    // Output:
    // LeadingZeros64(0000000000000000000000000000000000000000000000000000000000000001) = 63

<h2 id="LeadingZeros8">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L12">LeadingZeros8</a>
    <a href="#LeadingZeros8">¶</a></h2>
<pre>func LeadingZeros8(x <a href="/builtin/#uint8">uint8</a>) <a href="/builtin/#int">int</a></pre>

LeadingZeros8 returns the number of leading zero bits in x; the result is 8 for
x == 0.

<a id="exampleLeadingZeros8"></a>
Example:

    fmt.Printf("LeadingZeros8(%08b) = %d\n", 1, bits.LeadingZeros8(1))
    // Output:
    // LeadingZeros8(00000001) = 7

<h2 id="Len">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L271">Len</a>
    <a href="#Len">¶</a></h2>
<pre>func Len(x <a href="/builtin/#uint">uint</a>) <a href="/builtin/#int">int</a></pre>

Len returns the minimum number of bits required to represent x; the result is 0
for x == 0.

<h2 id="Len16">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L284">Len16</a>
    <a href="#Len16">¶</a></h2>
<pre>func Len16(x <a href="/builtin/#uint16">uint16</a>) (n <a href="/builtin/#int">int</a>)</pre>

Len16 returns the minimum number of bits required to represent x; the result is
0 for x == 0.

<a id="exampleLen16"></a>
Example:

    fmt.Printf("Len16(%016b) = %d\n", 8, bits.Len16(8))
    // Output:
    // Len16(0000000000001000) = 4

<h2 id="Len32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L293">Len32</a>
    <a href="#Len32">¶</a></h2>
<pre>func Len32(x <a href="/builtin/#uint32">uint32</a>) (n <a href="/builtin/#int">int</a>)</pre>

Len32 returns the minimum number of bits required to represent x; the result is
0 for x == 0.

<a id="exampleLen32"></a>
Example:

    fmt.Printf("Len32(%032b) = %d\n", 8, bits.Len32(8))
    // Output:
    // Len32(00000000000000000000000000001000) = 4

<h2 id="Len64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L306">Len64</a>
    <a href="#Len64">¶</a></h2>
<pre>func Len64(x <a href="/builtin/#uint64">uint64</a>) (n <a href="/builtin/#int">int</a>)</pre>

Len64 returns the minimum number of bits required to represent x; the result is
0 for x == 0.

<a id="exampleLen64"></a>
Example:

    fmt.Printf("Len64(%064b) = %d\n", 8, bits.Len64(8))
    // Output:
    // Len64(0000000000000000000000000000000000000000000000000000000000001000) = 4

<h2 id="Len8">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L279">Len8</a>
    <a href="#Len8">¶</a></h2>
<pre>func Len8(x <a href="/builtin/#uint8">uint8</a>) <a href="/builtin/#int">int</a></pre>

Len8 returns the minimum number of bits required to represent x; the result is 0
for x == 0.

<a id="exampleLen8"></a>
Example:

    fmt.Printf("Len8(%08b) = %d\n", 8, bits.Len8(8))
    // Output:
    // Len8(00001000) = 4

<h2 id="OnesCount">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L101">OnesCount</a>
    <a href="#OnesCount">¶</a></h2>
<pre>func OnesCount(x <a href="/builtin/#uint">uint</a>) <a href="/builtin/#int">int</a></pre>

OnesCount returns the number of one bits ("population count") in x.

<h2 id="OnesCount16">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L114">OnesCount16</a>
    <a href="#OnesCount16">¶</a></h2>
<pre>func OnesCount16(x <a href="/builtin/#uint16">uint16</a>) <a href="/builtin/#int">int</a></pre>

OnesCount16 returns the number of one bits ("population count") in x.

<a id="exampleOnesCount16"></a>
Example:

    fmt.Printf("OnesCount16(%016b) = %d\n", 14, bits.OnesCount16(14))
    // Output:
    // OnesCount16(0000000000001110) = 3

<h2 id="OnesCount32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L119">OnesCount32</a>
    <a href="#OnesCount32">¶</a></h2>
<pre>func OnesCount32(x <a href="/builtin/#uint32">uint32</a>) <a href="/builtin/#int">int</a></pre>

OnesCount32 returns the number of one bits ("population count") in x.

<a id="exampleOnesCount32"></a>
Example:

    fmt.Printf("OnesCount32(%032b) = %d\n", 14, bits.OnesCount32(14))
    // Output:
    // OnesCount32(00000000000000000000000000001110) = 3

<h2 id="OnesCount64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L124">OnesCount64</a>
    <a href="#OnesCount64">¶</a></h2>
<pre>func OnesCount64(x <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#int">int</a></pre>

OnesCount64 returns the number of one bits ("population count") in x.

<a id="exampleOnesCount64"></a>
Example:

    fmt.Printf("OnesCount64(%064b) = %d\n", 14, bits.OnesCount64(14))
    // Output:
    // OnesCount64(0000000000000000000000000000000000000000000000000000000000001110) = 3

<h2 id="OnesCount8">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L109">OnesCount8</a>
    <a href="#OnesCount8">¶</a></h2>
<pre>func OnesCount8(x <a href="/builtin/#uint8">uint8</a>) <a href="/builtin/#int">int</a></pre>

OnesCount8 returns the number of one bits ("population count") in x.

<a id="exampleOnesCount8"></a>
Example:

    fmt.Printf("OnesCount8(%08b) = %d\n", 14, bits.OnesCount8(14))
    // Output:
    // OnesCount8(00001110) = 3

<h2 id="Reverse">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L200">Reverse</a>
    <a href="#Reverse">¶</a></h2>
<pre>func Reverse(x <a href="/builtin/#uint">uint</a>) <a href="/builtin/#uint">uint</a></pre>

Reverse returns the value of x with its bits in reversed order.

<h2 id="Reverse16">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L213">Reverse16</a>
    <a href="#Reverse16">¶</a></h2>
<pre>func Reverse16(x <a href="/builtin/#uint16">uint16</a>) <a href="/builtin/#uint16">uint16</a></pre>

Reverse16 returns the value of x with its bits in reversed order.

<a id="exampleReverse16"></a>
Example:

    fmt.Printf("%016b\n", 19)
    fmt.Printf("%016b\n", bits.Reverse16(19))
    // Output:
    // 0000000000010011
    // 1100100000000000

<h2 id="Reverse32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L218">Reverse32</a>
    <a href="#Reverse32">¶</a></h2>
<pre>func Reverse32(x <a href="/builtin/#uint32">uint32</a>) <a href="/builtin/#uint32">uint32</a></pre>

Reverse32 returns the value of x with its bits in reversed order.

<a id="exampleReverse32"></a>
Example:

    fmt.Printf("%032b\n", 19)
    fmt.Printf("%032b\n", bits.Reverse32(19))
    // Output:
    // 00000000000000000000000000010011
    // 11001000000000000000000000000000

<h2 id="Reverse64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L228">Reverse64</a>
    <a href="#Reverse64">¶</a></h2>
<pre>func Reverse64(x <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#uint64">uint64</a></pre>

Reverse64 returns the value of x with its bits in reversed order.

<a id="exampleReverse64"></a>
Example:

    fmt.Printf("%064b\n", 19)
    fmt.Printf("%064b\n", bits.Reverse64(19))
    // Output:
    // 0000000000000000000000000000000000000000000000000000000000010011
    // 1100100000000000000000000000000000000000000000000000000000000000

<h2 id="Reverse8">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L208">Reverse8</a>
    <a href="#Reverse8">¶</a></h2>
<pre>func Reverse8(x <a href="/builtin/#uint8">uint8</a>) <a href="/builtin/#uint8">uint8</a></pre>

Reverse8 returns the value of x with its bits in reversed order.

<a id="exampleReverse8"></a>
Example:

    fmt.Printf("%08b\n", 19)
    fmt.Printf("%08b\n", bits.Reverse8(19))
    // Output:
    // 00010011
    // 11001000

<h2 id="ReverseBytes">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L241">ReverseBytes</a>
    <a href="#ReverseBytes">¶</a></h2>
<pre>func ReverseBytes(x <a href="/builtin/#uint">uint</a>) <a href="/builtin/#uint">uint</a></pre>

ReverseBytes returns the value of x with its bytes in reversed order.

<h2 id="ReverseBytes16">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L249">ReverseBytes16</a>
    <a href="#ReverseBytes16">¶</a></h2>
<pre>func ReverseBytes16(x <a href="/builtin/#uint16">uint16</a>) <a href="/builtin/#uint16">uint16</a></pre>

ReverseBytes16 returns the value of x with its bytes in reversed order.

<a id="exampleReverseBytes16"></a>
Example:

    fmt.Printf("%016b\n", 15)
    fmt.Printf("%016b\n", bits.ReverseBytes16(15))
    // Output:
    // 0000000000001111
    // 0000111100000000

<h2 id="ReverseBytes32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L254">ReverseBytes32</a>
    <a href="#ReverseBytes32">¶</a></h2>
<pre>func ReverseBytes32(x <a href="/builtin/#uint32">uint32</a>) <a href="/builtin/#uint32">uint32</a></pre>

ReverseBytes32 returns the value of x with its bytes in reversed order.

<a id="exampleReverseBytes32"></a>
Example:

    fmt.Printf("%032b\n", 15)
    fmt.Printf("%032b\n", bits.ReverseBytes32(15))
    // Output:
    // 00000000000000000000000000001111
    // 00001111000000000000000000000000

<h2 id="ReverseBytes64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L261">ReverseBytes64</a>
    <a href="#ReverseBytes64">¶</a></h2>
<pre>func ReverseBytes64(x <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#uint64">uint64</a></pre>

ReverseBytes64 returns the value of x with its bytes in reversed order.

<a id="exampleReverseBytes64"></a>
Example:

    fmt.Printf("%064b\n", 15)
    fmt.Printf("%064b\n", bits.ReverseBytes64(15))
    // Output:
    // 0000000000000000000000000000000000000000000000000000000000001111
    // 0000111100000000000000000000000000000000000000000000000000000000

<h2 id="RotateLeft">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L158">RotateLeft</a>
    <a href="#RotateLeft">¶</a></h2>
<pre>func RotateLeft(x <a href="/builtin/#uint">uint</a>, k <a href="/builtin/#int">int</a>) <a href="/builtin/#uint">uint</a></pre>

RotateLeft returns the value of x rotated left by (k mod UintSize) bits. To
rotate x right by k bits, call RotateLeft(x, -k).

<h2 id="RotateLeft16">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L175">RotateLeft16</a>
    <a href="#RotateLeft16">¶</a></h2>
<pre>func RotateLeft16(x <a href="/builtin/#uint16">uint16</a>, k <a href="/builtin/#int">int</a>) <a href="/builtin/#uint16">uint16</a></pre>

RotateLeft16 returns the value of x rotated left by (k mod 16) bits. To rotate x
right by k bits, call RotateLeft16(x, -k).

<a id="exampleRotateLeft16"></a>
Example:

    fmt.Printf("%016b\n", 15)
    fmt.Printf("%016b\n", bits.RotateLeft16(15, 2))
    fmt.Printf("%016b\n", bits.RotateLeft16(15, -2))
    // Output:
    // 0000000000001111
    // 0000000000111100
    // 1100000000000011

<h2 id="RotateLeft32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L183">RotateLeft32</a>
    <a href="#RotateLeft32">¶</a></h2>
<pre>func RotateLeft32(x <a href="/builtin/#uint32">uint32</a>, k <a href="/builtin/#int">int</a>) <a href="/builtin/#uint32">uint32</a></pre>

RotateLeft32 returns the value of x rotated left by (k mod 32) bits. To rotate x
right by k bits, call RotateLeft32(x, -k).

<a id="exampleRotateLeft32"></a>
Example:

    fmt.Printf("%032b\n", 15)
    fmt.Printf("%032b\n", bits.RotateLeft32(15, 2))
    fmt.Printf("%032b\n", bits.RotateLeft32(15, -2))
    // Output:
    // 00000000000000000000000000001111
    // 00000000000000000000000000111100
    // 11000000000000000000000000000011

<h2 id="RotateLeft64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L191">RotateLeft64</a>
    <a href="#RotateLeft64">¶</a></h2>
<pre>func RotateLeft64(x <a href="/builtin/#uint64">uint64</a>, k <a href="/builtin/#int">int</a>) <a href="/builtin/#uint64">uint64</a></pre>

RotateLeft64 returns the value of x rotated left by (k mod 64) bits. To rotate x
right by k bits, call RotateLeft64(x, -k).

<a id="exampleRotateLeft64"></a>
Example:

    fmt.Printf("%064b\n", 15)
    fmt.Printf("%064b\n", bits.RotateLeft64(15, 2))
    fmt.Printf("%064b\n", bits.RotateLeft64(15, -2))
    // Output:
    // 0000000000000000000000000000000000000000000000000000000000001111
    // 0000000000000000000000000000000000000000000000000000000000111100
    // 1100000000000000000000000000000000000000000000000000000000000011

<h2 id="RotateLeft8">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L167">RotateLeft8</a>
    <a href="#RotateLeft8">¶</a></h2>
<pre>func RotateLeft8(x <a href="/builtin/#uint8">uint8</a>, k <a href="/builtin/#int">int</a>) <a href="/builtin/#uint8">uint8</a></pre>

RotateLeft8 returns the value of x rotated left by (k mod 8) bits. To rotate x
right by k bits, call RotateLeft8(x, -k).

<a id="exampleRotateLeft8"></a>
Example:

    fmt.Printf("%08b\n", 15)
    fmt.Printf("%08b\n", bits.RotateLeft8(15, 2))
    fmt.Printf("%08b\n", bits.RotateLeft8(15, -2))
    // Output:
    // 00001111
    // 00111100
    // 11000011

<h2 id="TrailingZeros">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L43">TrailingZeros</a>
    <a href="#TrailingZeros">¶</a></h2>
<pre>func TrailingZeros(x <a href="/builtin/#uint">uint</a>) <a href="/builtin/#int">int</a></pre>

TrailingZeros returns the number of trailing zero bits in x; the result is
UintSize for x == 0.

<h2 id="TrailingZeros16">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L56">TrailingZeros16</a>
    <a href="#TrailingZeros16">¶</a></h2>
<pre>func TrailingZeros16(x <a href="/builtin/#uint16">uint16</a>) (n <a href="/builtin/#int">int</a>)</pre>

TrailingZeros16 returns the number of trailing zero bits in x; the result is 16
for x == 0.

<a id="exampleTrailingZeros16"></a>
Example:

    fmt.Printf("TrailingZeros16(%016b) = %d\n", 14, bits.TrailingZeros16(14))
    // Output:
    // TrailingZeros16(0000000000001110) = 1

<h2 id="TrailingZeros32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L65">TrailingZeros32</a>
    <a href="#TrailingZeros32">¶</a></h2>
<pre>func TrailingZeros32(x <a href="/builtin/#uint32">uint32</a>) <a href="/builtin/#int">int</a></pre>

TrailingZeros32 returns the number of trailing zero bits in x; the result is 32
for x == 0.

<a id="exampleTrailingZeros32"></a>
Example:

    fmt.Printf("TrailingZeros32(%032b) = %d\n", 14, bits.TrailingZeros32(14))
    // Output:
    // TrailingZeros32(00000000000000000000000000001110) = 1

<h2 id="TrailingZeros64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L74">TrailingZeros64</a>
    <a href="#TrailingZeros64">¶</a></h2>
<pre>func TrailingZeros64(x <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#int">int</a></pre>

TrailingZeros64 returns the number of trailing zero bits in x; the result is 64
for x == 0.

<a id="exampleTrailingZeros64"></a>
Example:

    fmt.Printf("TrailingZeros64(%064b) = %d\n", 14, bits.TrailingZeros64(14))
    // Output:
    // TrailingZeros64(0000000000000000000000000000000000000000000000000000000000001110) = 1

<h2 id="TrailingZeros8">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/math/bits/bits.go#L51">TrailingZeros8</a>
    <a href="#TrailingZeros8">¶</a></h2>
<pre>func TrailingZeros8(x <a href="/builtin/#uint8">uint8</a>) <a href="/builtin/#int">int</a></pre>

TrailingZeros8 returns the number of trailing zero bits in x; the result is 8
for x == 0.

<a id="exampleTrailingZeros8"></a>
Example:

    fmt.Printf("TrailingZeros8(%08b) = %d\n", 14, bits.TrailingZeros8(14))
    // Output:
    // TrailingZeros8(00001110) = 1


