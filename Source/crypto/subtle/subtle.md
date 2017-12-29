version: 1.9.2
## package subtle

  `import "crypto/subtle"`

## Overview

Package subtle implements functions that are often useful in cryptographic code
but require careful thought to use correctly.

## Index

- [func ConstantTimeByteEq(x, y uint8) int](#ConstantTimeByteEq)
- [func ConstantTimeCompare(x, y []byte) int](#ConstantTimeCompare)
- [func ConstantTimeCopy(v int, x, y []byte)](#ConstantTimeCopy)
- [func ConstantTimeEq(x, y int32) int](#ConstantTimeEq)
- [func ConstantTimeLessOrEq(x, y int) int](#ConstantTimeLessOrEq)
- [func ConstantTimeSelect(v, x, y int) int](#ConstantTimeSelect)

### Package files
 [constant_time.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/subtle/constant_time.go)

<h2 id="ConstantTimeByteEq">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/subtle/constant_time.go#L21">ConstantTimeByteEq</a>
    <a href="#ConstantTimeByteEq">¶</a></h2>
<pre>func ConstantTimeByteEq(x, y <a href="/builtin/#uint8">uint8</a>) <a href="/builtin/#int">int</a></pre>

ConstantTimeByteEq returns 1 if x == y and 0 otherwise.

<h2 id="ConstantTimeCompare">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/subtle/constant_time.go#L2">ConstantTimeCompare</a>
    <a href="#ConstantTimeCompare">¶</a></h2>
<pre>func ConstantTimeCompare(x, y []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#int">int</a></pre>

ConstantTimeCompare returns 1 if and only if the two slices, x and y, have equal
contents. The time taken is a function of the length of the slices and is
independent of the contents.

<h2 id="ConstantTimeCopy">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/subtle/constant_time.go#L45">ConstantTimeCopy</a>
    <a href="#ConstantTimeCopy">¶</a></h2>
<pre>func ConstantTimeCopy(v <a href="/builtin/#int">int</a>, x, y []<a href="/builtin/#byte">byte</a>)</pre>

ConstantTimeCopy copies the contents of y into x (a slice of equal length) if v
== 1. If v == 0, x is left unchanged. Its behavior is undefined if v takes any
other value.

<h2 id="ConstantTimeEq">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/subtle/constant_time.go#L31">ConstantTimeEq</a>
    <a href="#ConstantTimeEq">¶</a></h2>
<pre>func ConstantTimeEq(x, y <a href="/builtin/#int32">int32</a>) <a href="/builtin/#int">int</a></pre>

ConstantTimeEq returns 1 if x == y and 0 otherwise.

<h2 id="ConstantTimeLessOrEq">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/subtle/constant_time.go#L59">ConstantTimeLessOrEq</a>
    <a href="#ConstantTimeLessOrEq">¶</a></h2>
<pre>func ConstantTimeLessOrEq(x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

ConstantTimeLessOrEq returns 1 if x <= y and 0 otherwise. Its behavior is
undefined if x or y are negative or > 2**31 - 1.

<h2 id="ConstantTimeSelect">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/subtle/constant_time.go#L18">ConstantTimeSelect</a>
    <a href="#ConstantTimeSelect">¶</a></h2>
<pre>func ConstantTimeSelect(v, x, y <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

ConstantTimeSelect returns x if v is 1 and y if v is 0. Its behavior is
undefined if v takes any other value.


