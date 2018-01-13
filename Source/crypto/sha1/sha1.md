version: 1.9.2
## package sha1

  `import "crypto/sha1"`

## Overview

Package sha1 implements the SHA-1 hash algorithm as defined in RFC 3174.

SHA-1 is cryptographically broken and should not be used for secure
applications.

## Index

- [Constants](#pkg-constants)
- [func New() hash.Hash](#New)
- [func Sum(data []byte) [Size]byte](#Sum)

### Examples

- [New](#exampleNew)
- [New (File)](#exampleNew_file)
- [Sum](#exampleSum)

### Package files
 [sha1.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/sha1/sha1.go) [sha1block.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/sha1/sha1block.go) [sha1block_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/sha1/sha1block_amd64.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="BlockSize">BlockSize</span> = 64</pre>

The blocksize of SHA-1 in bytes.

<pre>const <span id="Size">Size</span> = 20</pre>

The size of a SHA-1 checksum in bytes.

<h2 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/sha1/sha1.go#L44">New</a>
    <a href="#New">¶</a></h2>
<pre>func New() <a href="/hash/">hash</a>.<a href="/hash/#Hash">Hash</a></pre>

New returns a new hash.Hash computing the SHA1 checksum.

<a id="exampleNew"></a>
Example:

    h := sha1.New()
    io.WriteString(h, "His money is twice tainted:")
    io.WriteString(h, " 'taint yours and 'taint mine.")
    fmt.Printf("% x", h.Sum(nil))
    // Output: 59 7f 6a 54 00 10 f9 4c 15 d7 18 06 a9 9a 2c 87 10 e7 47 bd


<a id="exampleNew_file"></a>
Example:

    f, err := os.Open("file.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()

    h := sha1.New()
    if _, err := io.Copy(h, f); err != nil {
        log.Fatal(err)
    }

    fmt.Printf("% x", h.Sum(nil))

<h2 id="Sum">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/sha1/sha1.go#L186">Sum</a>
    <a href="#Sum">¶</a></h2>
<pre>func Sum(data []<a href="/builtin/#byte">byte</a>) [<a href="#Size">Size</a>]<a href="/builtin/#byte">byte</a></pre>

Sum returns the SHA-1 checksum of the data.

<a id="exampleSum"></a>
Example:

    data := []byte("This page intentionally left blank.")
    fmt.Printf("% x", sha1.Sum(data))
    // Output: af 06 49 23 bb f2 30 15 96 aa c4 c2 73 ba 32 17 8e bc 4a 96


