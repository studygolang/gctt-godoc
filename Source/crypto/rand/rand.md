version: 1.9.2
## package rand

  `import "crypto/rand"`

## Overview

Package rand implements a cryptographically secure pseudorandom number
generator.

## Index

- [Variables](#pkg-variables)
- [func Int(rand io.Reader, max *big.Int) (n *big.Int, err error)](#Int)
- [func Prime(rand io.Reader, bits int) (p *big.Int, err error)](#Prime)
- [func Read(b []byte) (n int, err error)](#Read)

### Examples

- [Read](#exampleRead)

### Package files
 [eagain.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rand/eagain.go) [rand.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rand/rand.go) [rand_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rand/rand_linux.go) [rand_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rand/rand_unix.go) [util.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rand/util.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="Reader">Reader</span> <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

Reader is a global, shared instance of a cryptographically strong pseudo-random
generator.

On Linux, Reader uses getrandom(2) if available, /dev/urandom otherwise. On
OpenBSD, Reader uses getentropy(2). On other Unix-like systems, Reader reads
from /dev/urandom. On Windows systems, Reader uses the CryptGenRandom API.

<h2 id="Int">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rand/util.go#L96">Int</a>
    <a href="#Int">¶</a></h2>
<pre>func Int(rand <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, max *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>) (n *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>, err <a href="/builtin/#error">error</a>)</pre>

Int returns a uniform random value in [0, max). It panics if max <= 0.

<h2 id="Prime">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rand/util.go#L21">Prime</a>
    <a href="#Prime">¶</a></h2>
<pre>func Prime(rand <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, bits <a href="/builtin/#int">int</a>) (p *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>, err <a href="/builtin/#error">error</a>)</pre>

Prime returns a number, p, of the given size, such that p is prime with high
probability. Prime will return error for any error returned by rand.Read or if
bits < 2.

<h2 id="Read">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/rand/rand.go#L12">Read</a>
    <a href="#Read">¶</a></h2>
<pre>func Read(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read is a helper function that calls Reader.Read using io.ReadFull. On return, n
== len(b) if and only if err == nil.

<a id="exampleRead"></a>
Example:

    c := 10
    b := make([]byte, c)
    _, err := rand.Read(b)
    if err != nil {
        fmt.Println("error:", err)
        return
    }
    // The slice should now contain random bytes instead of only zeroes.
    fmt.Println(bytes.Equal(b, make([]byte, c)))

    // Output:
    // false


