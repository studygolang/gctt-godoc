version: 1.10
## package hash

  `import "hash"`

## Overview

Package hash provides interfaces for hash functions.

<a id="example_binaryMarshaler"></a>
Example:

    const (
        input1 = "The tunneling gopher digs downwards, "
        input2 = "unaware of what he will find."
    )

    first := sha256.New()
    first.Write([]byte(input1))

    marshaler, ok := first.(encoding.BinaryMarshaler)
    if !ok {
        log.Fatal("first does not implement encoding.BinaryMarshaler")
    }
    state, err := marshaler.MarshalBinary()
    if err != nil {
        log.Fatal("unable to marshal hash:", err)
    }

    second := sha256.New()

    unmarshaler, ok := second.(encoding.BinaryUnmarshaler)
    if !ok {
        log.Fatal("second does not implement encoding.BinaryUnmarshaler")
    }
    if err := unmarshaler.UnmarshalBinary(state); err != nil {
        log.Fatal("unable to unmarshal hash:", err)
    }

    first.Write([]byte(input2))
    second.Write([]byte(input2))

    fmt.Printf("%x\n", first.Sum(nil))
    fmt.Println(bytes.Equal(first.Sum(nil), second.Sum(nil)))
    // Output:
    // 57d51a066f3a39942649cd9a76c77e97ceab246756ff3888659e6aa5a07f4a52
    // true

## Index

- [type Hash](#Hash)
- [type Hash32](#Hash32)
- [type Hash64](#Hash64)

### Examples

- [Package (BinaryMarshaler)](#example_binaryMarshaler)

### Package files
 [hash.go](//github.com/golang/go/blob/release-branch.go1.10/src/hash/hash.go)

<h2 id="Hash">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/hash/hash.go#L16">Hash</a>
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

Hash implementations in the standard library (e.g. hash/crc32 and crypto/sha256)
implement the encoding.BinaryMarshaler and encoding.BinaryUnmarshaler
interfaces. Marshaling a hash implementation allows its internal state to be
saved and used for additional processing later, without having to re-write the
data previously written to the hash. The hash state may contain portions of the
input in its original form, which users are expected to handle for any possible
security implications.

Compatibility: Any future changes to hash or crypto packages will endeavor to
maintain compatibility with state encoded using previous versions. That is, any
released versions of the packages should be able to decode data written with any
previously released version, subject to issues such as security fixes. See the
Go compatibility document for background: https://golang.org/doc/go1compat

<h2 id="Hash32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/hash/hash.go#L39">Hash32</a>
    <a href="#Hash32">¶</a></h2>
<pre>type Hash32 interface {
    <a href="#Hash">Hash</a>
    Sum32() <a href="/builtin/#uint32">uint32</a>
}</pre>

Hash32 is the common interface implemented by all 32-bit hash functions.

<h2 id="Hash64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/hash/hash.go#L45">Hash64</a>
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
