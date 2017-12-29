version: 1.9.2
## package pem

  `import "encoding/pem"`

## Overview

Package pem implements the PEM data encoding, which originated in Privacy
Enhanced Mail. The most common use of PEM encoding today is in TLS keys and
certificates. See RFC 1421.

## Index

- [func Encode(out io.Writer, b *Block) error](#Encode)
- [func EncodeToMemory(b *Block) []byte](#EncodeToMemory)
- [type Block](#Block)
  - [func Decode(data []byte) (p *Block, rest []byte)](#Decode)

### Examples

- [Decode](#example_Decode)

### Package files
 [pem.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/pem/pem.go)

<h2 id="Encode">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/pem/pem.go#L245">Encode</a>
    <a href="#Encode">¶</a></h2>
<pre>func Encode(out <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, b *<a href="#Block">Block</a>) <a href="/builtin/#error">error</a></pre>


<h2 id="EncodeToMemory">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/pem/pem.go#L303">EncodeToMemory</a>
    <a href="#EncodeToMemory">¶</a></h2>
<pre>func EncodeToMemory(b *<a href="#Block">Block</a>) []<a href="/builtin/#byte">byte</a></pre>


<h2 id="Block">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/pem/pem.go#L17">Block</a>
    <a href="#Block">¶</a></h2>
<pre>type Block struct {
<span id="Block.Type"></span>    Type    <a href="/builtin/#string">string</a>            <span class="comment">// The type, taken from the preamble (i.e. &#34;RSA PRIVATE KEY&#34;).</span>
<span id="Block.Headers"></span>    Headers map[<a href="/builtin/#string">string</a>]<a href="/builtin/#string">string</a> <span class="comment">// Optional headers.</span>
<span id="Block.Bytes"></span>    Bytes   []<a href="/builtin/#byte">byte</a>            <span class="comment">// The decoded bytes of the contents. Typically a DER encoded ASN.1 structure.</span>
}</pre>

A Block represents a PEM encoded structure.

The encoded form is:

    -----BEGIN Type-----
    Headers
    base64-encoded Bytes
    -----END Type-----

where Headers is a possibly empty sequence of Key: Value lines.

<h3 id="Decode">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/pem/pem.go#L68">Decode</a>
    <a href="#Decode">¶</a></h3>
<pre>func Decode(data []<a href="/builtin/#byte">byte</a>) (p *<a href="#Block">Block</a>, rest []<a href="/builtin/#byte">byte</a>)</pre>

Decode will find the next PEM formatted block (certificate, private key etc) in
the input. It returns that block and the remainder of the input. If no PEM data
is found, p is nil and the whole of the input is returned in rest.

<a id="example_Decode"></a>
Example:

    var pubPEMData = []byte(`
    -----BEGIN PUBLIC KEY-----
    MIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEAlRuRnThUjU8/prwYxbty
    WPT9pURI3lbsKMiB6Fn/VHOKE13p4D8xgOCADpdRagdT6n4etr9atzDKUSvpMtR3
    CP5noNc97WiNCggBjVWhs7szEe8ugyqF23XwpHQ6uV1LKH50m92MbOWfCtjU9p/x
    qhNpQQ1AZhqNy5Gevap5k8XzRmjSldNAFZMY7Yv3Gi+nyCwGwpVtBUwhuLzgNFK/
    yDtw2WcWmUU7NuC8Q6MWvPebxVtCfVp/iQU6q60yyt6aGOBkhAX0LpKAEhKidixY
    nP9PNVBvxgu3XZ4P36gZV6+ummKdBVnc3NqwBLu5+CcdRdusmHPHd5pHf4/38Z3/
    6qU2a/fPvWzceVTEgZ47QjFMTCTmCwNt29cvi7zZeQzjtwQgn4ipN9NibRH/Ax/q
    TbIzHfrJ1xa2RteWSdFjwtxi9C20HUkjXSeI4YlzQMH0fPX6KCE7aVePTOnB69I/
    a9/q96DiXZajwlpq3wFctrs1oXqBp5DVrCIj8hU2wNgB7LtQ1mCtsYz//heai0K9
    PhE4X6hiE0YmeAZjR0uHl8M/5aW9xCoJ72+12kKpWAa0SFRWLy6FejNYCYpkupVJ
    yecLk/4L1W0l6jQQZnWErXZYe0PNFcmwGXy1Rep83kfBRNKRy5tvocalLlwXLdUk
    AIU+2GKjyT3iMuzZxxFxPFMCAwEAAQ==
    -----END PUBLIC KEY-----
    and some more`)

    block, rest := pem.Decode(pubPEMData)
    if block == nil || block.Type != "PUBLIC KEY" {
        log.Fatal("failed to decode PEM block containing public key")
    }

    pub, err := x509.ParsePKIXPublicKey(block.Bytes)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("Got a %T, with remaining data: %q", pub, rest)
    // Output: Got a *rsa.PublicKey, with remaining data: "and some more"


