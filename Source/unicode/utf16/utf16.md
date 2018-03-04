version: 1.10
## package utf16

  `import "unicode/utf16"`

## Overview

Package utf16 implements encoding and decoding of UTF-16 sequences.

## Index

- [func Decode(s []uint16) []rune](#Decode)
- [func DecodeRune(r1, r2 rune) rune](#DecodeRune)
- [func Encode(s []rune) []uint16](#Encode)
- [func EncodeRune(r rune) (r1, r2 rune)](#EncodeRune)
- [func IsSurrogate(r rune) bool](#IsSurrogate)

### Package files
 [utf16.go](//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf16/utf16.go)

<h2 id="Decode">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf16/utf16.go#L78">Decode</a>
    <a href="#Decode">¶</a></h2>
<pre>func Decode(s []<a href="/builtin/#uint16">uint16</a>) []<a href="/builtin/#rune">rune</a></pre>

Decode returns the Unicode code point sequence represented by the UTF-16
encoding s.

<h2 id="DecodeRune">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf16/utf16.go#L27">DecodeRune</a>
    <a href="#DecodeRune">¶</a></h2>
<pre>func DecodeRune(r1, r2 <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a></pre>

DecodeRune returns the UTF-16 decoding of a surrogate pair. If the pair is not a
valid UTF-16 surrogate pair, DecodeRune returns the Unicode replacement code
point U+FFFD.

<h2 id="Encode">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf16/utf16.go#L46">Encode</a>
    <a href="#Encode">¶</a></h2>
<pre>func Encode(s []<a href="/builtin/#rune">rune</a>) []<a href="/builtin/#uint16">uint16</a></pre>

Encode returns the UTF-16 encoding of the Unicode code point sequence s.

<h2 id="EncodeRune">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf16/utf16.go#L37">EncodeRune</a>
    <a href="#EncodeRune">¶</a></h2>
<pre>func EncodeRune(r <a href="/builtin/#rune">rune</a>) (r1, r2 <a href="/builtin/#rune">rune</a>)</pre>

EncodeRune returns the UTF-16 surrogate pair r1, r2 for the given rune. If the
rune is not a valid Unicode code point or does not need encoding, EncodeRune
returns U+FFFD, U+FFFD.

<h2 id="IsSurrogate">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/unicode/utf16/utf16.go#L20">IsSurrogate</a>
    <a href="#IsSurrogate">¶</a></h2>
<pre>func IsSurrogate(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsSurrogate reports whether the specified Unicode code point can appear in a
surrogate pair.


