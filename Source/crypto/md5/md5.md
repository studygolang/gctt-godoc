version: 1.10
## package md5

  `import "crypto/md5"`

## Overview

Package md5 implements the MD5 hash algorithm as defined in RFC 1321.

MD5 is cryptographically broken and should not be used for secure applications.

## Index

- [Constants](#pkg-constants)
- [func New() hash.Hash](#New)
- [func Sum(data []byte) [Size]byte](#Sum)

### Examples

- [New](#exampleNew)
- [New (File)](#exampleNew_file)
- [Sum](#exampleSum)

### Package files
 [md5.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/md5/md5.go) [md5block.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/md5/md5block.go) [md5block_decl.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/md5/md5block_decl.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="BlockSize">BlockSize</span> = 64</pre>

The blocksize of MD5 in bytes.

<pre>const <span id="Size">Size</span> = 16</pre>

The size of an MD5 checksum in bytes.

<h2 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/md5/md5.go#L120">New</a>
    <a href="#New">¶</a></h2>
<pre>func New() <a href="/hash/">hash</a>.<a href="/hash/#Hash">Hash</a></pre>

New returns a new hash.Hash computing the MD5 checksum. The Hash also implements
encoding.BinaryMarshaler and encoding.BinaryUnmarshaler to marshal and unmarshal
the internal state of the hash.

<a id="exampleNew"></a>
Example:

    h := md5.New()
    io.WriteString(h, "The fog is getting thicker!")
    io.WriteString(h, "And Leon's getting laaarger!")
    fmt.Printf("%x", h.Sum(nil))
    // Output: e2c569be17396eca2a2e3c11578123ed


<a id="exampleNew_file"></a>
Example:

    f, err := os.Open("file.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()

    h := md5.New()
    if _, err := io.Copy(h, f); err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%x", h.Sum(nil))

<h2 id="Sum">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/md5/md5.go#L194">Sum</a>
    <a href="#Sum">¶</a></h2>
<pre>func Sum(data []<a href="/builtin/#byte">byte</a>) [<a href="#Size">Size</a>]<a href="/builtin/#byte">byte</a></pre>

Sum returns the MD5 checksum of the data.

<a id="exampleSum"></a>
Example:

    data := []byte("These pretzels are making me thirsty.")
    fmt.Printf("%x", md5.Sum(data))
    // Output: b0804ec967f48520697662a204f5fe72


