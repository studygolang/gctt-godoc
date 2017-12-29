version: 1.9.2
## package hash

  `import "hash"`

## Overview

Package hash provides interfaces for hash functions.

## Index

- [type Hash](#Hash)
- [type Hash32](#Hash32)
- [type Hash64](#Hash64)

### Package files
 [hash.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/hash.go)

<h2 id="Hash">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/hash.go#L1">Hash</a>
    <a href="#Hash">¶</a></h2>
<pre>type Hash interface {
    <span class="comment">// Write (via the embedded io.Writer interface) adds more data to the running hash.</span>
    <span class="comment">// It never returns an error.</span>
    <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>

    <span class="comment">// Sum appends the current hash to b and returns the resulting slice.</span>
    <span class="comment">// It does not change the underlying hash state.</span>
    Sum(b []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a>

    <span class="comment">// Reset resets the Hash to its initial state.</span>
    Reset()

    <span class="comment">// Size returns the number of bytes Sum will return.</span>
    Size() <a href="/builtin/#int">int</a>

    <span class="comment">// BlockSize returns the hash&#39;s underlying block size.</span>
    <span class="comment">// The Write method must be able to accept any amount</span>
    <span class="comment">// of data, but it may operate more efficiently if all writes</span>
    <span class="comment">// are a multiple of the block size.</span>
    BlockSize() <a href="/builtin/#int">int</a>
}</pre>

Hash is the common interface implemented by all hash functions.

<h2 id="Hash32">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/hash.go#L24">Hash32</a>
    <a href="#Hash32">¶</a></h2>
<pre>type Hash32 interface {
    <a href="#Hash">Hash</a>
    Sum32() <a href="/builtin/#uint32">uint32</a>
}</pre>

Hash32 is the common interface implemented by all 32-bit hash functions.

<h2 id="Hash64">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/hash/hash.go#L30">Hash64</a>
    <a href="#Hash64">¶</a></h2>
<pre>type Hash64 interface {
    <a href="#Hash">Hash</a>
    Sum64() <a href="/builtin/#uint64">uint64</a>
}</pre>

Hash64 is the common interface implemented by all 64-bit hash functions.

## Subdirectories
- [..](..)
- [adler32](adler32/)
- [crc32](crc32/)
- [crc64](crc64/)
- [fnv](fnv/)
