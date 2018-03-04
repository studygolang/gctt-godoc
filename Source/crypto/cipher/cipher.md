version: 1.10
## package cipher

  `import "crypto/cipher"`

## Overview

Package cipher implements standard block cipher modes that can be wrapped around
low-level block cipher implementations. See
http://csrc.nist.gov/groups/ST/toolkit/BCM/current_modes.html and NIST Special
Publication 800-38A.

## Index

- [type AEAD](#AEAD)
  - [func NewGCM(cipher Block) (AEAD, error)](#NewGCM)
  - [func NewGCMWithNonceSize(cipher Block, size int) (AEAD, error)](#NewGCMWithNonceSize)
- [type Block](#Block)
- [type BlockMode](#BlockMode)
  - [func NewCBCDecrypter(b Block, iv []byte) BlockMode](#NewCBCDecrypter)
  - [func NewCBCEncrypter(b Block, iv []byte) BlockMode](#NewCBCEncrypter)
- [type Stream](#Stream)
  - [func NewCFBDecrypter(block Block, iv []byte) Stream](#NewCFBDecrypter)
  - [func NewCFBEncrypter(block Block, iv []byte) Stream](#NewCFBEncrypter)
  - [func NewCTR(block Block, iv []byte) Stream](#NewCTR)
  - [func NewOFB(b Block, iv []byte) Stream](#NewOFB)
- [type StreamReader](#StreamReader)
  - [func (r StreamReader) Read(dst []byte) (n int, err error)](#StreamReader.Read)
- [type StreamWriter](#StreamWriter)
  - [func (w StreamWriter) Close() error](#StreamWriter.Close)
  - [func (w StreamWriter) Write(src []byte) (n int, err error)](#StreamWriter.Write)

### Examples

- [NewCBCDecrypter](#exampleNewCBCDecrypter)
- [NewCBCEncrypter](#exampleNewCBCEncrypter)
- [NewCFBDecrypter](#exampleNewCFBDecrypter)
- [NewCFBEncrypter](#exampleNewCFBEncrypter)
- [NewCTR](#exampleNewCTR)
- [NewGCM (Decrypt)](#exampleNewGCM_decrypt)
- [NewGCM (Encrypt)](#exampleNewGCM_encrypt)
- [NewOFB](#exampleNewOFB)
- [StreamReader](#exampleStreamReader)
- [StreamWriter](#exampleStreamWriter)

### Package files
 [cbc.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/cbc.go) [cfb.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/cfb.go) [cipher.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/cipher.go) [ctr.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/ctr.go) [gcm.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/gcm.go) [io.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/io.go) [ofb.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/ofb.go) [xor.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/xor.go)

<h2 id="AEAD">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/gcm.go#L5">AEAD</a>
    <a href="#AEAD">¶</a></h2>
<pre>type AEAD interface {
    <span class="comment">// NonceSize returns the size of the nonce that must be passed to Seal</span>
    <span class="comment">// and Open.</span>
    NonceSize() <a href="/builtin/#int">int</a>

    <span class="comment">// Overhead returns the maximum difference between the lengths of a</span>
    <span class="comment">// plaintext and its ciphertext.</span>
    Overhead() <a href="/builtin/#int">int</a>

    <span class="comment">// Seal encrypts and authenticates plaintext, authenticates the</span>
    <span class="comment">// additional data and appends the result to dst, returning the updated</span>
    <span class="comment">// slice. The nonce must be NonceSize() bytes long and unique for all</span>
    <span class="comment">// time, for a given key.</span>
    <span class="comment">//</span>
    <span class="comment">// The plaintext and dst must overlap exactly or not at all. To reuse</span>
    <span class="comment">// plaintext&#39;s storage for the encrypted output, use plaintext[:0] as dst.</span>
    Seal(dst, nonce, plaintext, additionalData []<a href="/builtin/#byte">byte</a>) []<a href="/builtin/#byte">byte</a>

    <span class="comment">// Open decrypts and authenticates ciphertext, authenticates the</span>
    <span class="comment">// additional data and, if successful, appends the resulting plaintext</span>
    <span class="comment">// to dst, returning the updated slice. The nonce must be NonceSize()</span>
    <span class="comment">// bytes long and both it and the additional data must match the</span>
    <span class="comment">// value passed to Seal.</span>
    <span class="comment">//</span>
    <span class="comment">// The ciphertext and dst must overlap exactly or not at all. To reuse</span>
    <span class="comment">// ciphertext&#39;s storage for the decrypted output, use ciphertext[:0] as dst.</span>
    <span class="comment">//</span>
    <span class="comment">// Even if the function fails, the contents of dst, up to its capacity,</span>
    <span class="comment">// may be overwritten.</span>
    Open(dst, nonce, ciphertext, additionalData []<a href="/builtin/#byte">byte</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)
}</pre>

AEAD is a cipher mode providing authenticated encryption with associated data.
For a description of the methodology, see

    https://en.wikipedia.org/wiki/Authenticated_encryption

<h3 id="NewGCM">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/gcm.go#L71">NewGCM</a>
    <a href="#NewGCM">¶</a></h3>
<pre>func NewGCM(cipher <a href="#Block">Block</a>) (<a href="#AEAD">AEAD</a>, <a href="/builtin/#error">error</a>)</pre>

NewGCM returns the given 128-bit, block cipher wrapped in Galois Counter Mode
with the standard nonce length.

In general, the GHASH operation performed by this implementation of GCM is not
constant-time. An exception is when the underlying Block was created by
aes.NewCipher on systems with hardware support for AES. See the crypto/aes
package documentation for details.

<a id="exampleNewGCM_decrypt"></a>
Example:

    // Load your secret key from a safe place and reuse it across multiple
    // Seal/Open calls. (Obviously don't use this example key for anything
    // real.) If you want to convert a passphrase to a key, use a suitable
    // package like bcrypt or scrypt.
    // When decoded the key should be 16 bytes (AES-128) or 32 (AES-256).
    key, _ := hex.DecodeString("6368616e676520746869732070617373776f726420746f206120736563726574")
    ciphertext, _ := hex.DecodeString("c3aaa29f002ca75870806e44086700f62ce4d43e902b3888e23ceff797a7a471")
    nonce, _ := hex.DecodeString("64a9433eae7ccceee2fc0eda")

    block, err := aes.NewCipher(key)
    if err != nil {
        panic(err.Error())
    }

    aesgcm, err := cipher.NewGCM(block)
    if err != nil {
        panic(err.Error())
    }

    plaintext, err := aesgcm.Open(nil, nonce, ciphertext, nil)
    if err != nil {
        panic(err.Error())
    }

    fmt.Printf("%s\n", plaintext)
    // Output: exampleplaintext


<a id="exampleNewGCM_encrypt"></a>
Example:

    // Load your secret key from a safe place and reuse it across multiple
    // Seal/Open calls. (Obviously don't use this example key for anything
    // real.) If you want to convert a passphrase to a key, use a suitable
    // package like bcrypt or scrypt.
    // When decoded the key should be 16 bytes (AES-128) or 32 (AES-256).
    key, _ := hex.DecodeString("6368616e676520746869732070617373776f726420746f206120736563726574")
    plaintext := []byte("exampleplaintext")

    block, err := aes.NewCipher(key)
    if err != nil {
        panic(err.Error())
    }

    // Never use more than 2^32 random nonces with a given key because of the risk of a repeat.
    nonce := make([]byte, 12)
    if _, err := io.ReadFull(rand.Reader, nonce); err != nil {
        panic(err.Error())
    }

    aesgcm, err := cipher.NewGCM(block)
    if err != nil {
        panic(err.Error())
    }

    ciphertext := aesgcm.Seal(nil, nonce, plaintext, nil)
    fmt.Printf("%x\n", ciphertext)

<h3 id="NewGCMWithNonceSize">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/gcm.go#L81">NewGCMWithNonceSize</a>
    <a href="#NewGCMWithNonceSize">¶</a></h3>
<pre>func NewGCMWithNonceSize(cipher <a href="#Block">Block</a>, size <a href="/builtin/#int">int</a>) (<a href="#AEAD">AEAD</a>, <a href="/builtin/#error">error</a>)</pre>

NewGCMWithNonceSize returns the given 128-bit, block cipher wrapped in Galois
Counter Mode, which accepts nonces of the given length.

Only use this function if you require compatibility with an existing
cryptosystem that uses non-standard nonce lengths. All other users should use
NewGCM, which is faster and more resistant to misuse.

<h2 id="Block">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/cipher.go#L5">Block</a>
    <a href="#Block">¶</a></h2>
<pre>type Block interface {
    <span class="comment">// BlockSize returns the cipher&#39;s block size.</span>
    BlockSize() <a href="/builtin/#int">int</a>

    <span class="comment">// Encrypt encrypts the first block in src into dst.</span>
    <span class="comment">// Dst and src must overlap entirely or not at all.</span>
    Encrypt(dst, src []<a href="/builtin/#byte">byte</a>)

    <span class="comment">// Decrypt decrypts the first block in src into dst.</span>
    <span class="comment">// Dst and src must overlap entirely or not at all.</span>
    Decrypt(dst, src []<a href="/builtin/#byte">byte</a>)
}</pre>

A Block represents an implementation of block cipher using a given key. It
provides the capability to encrypt or decrypt individual blocks. The mode
implementations extend that capability to streams of blocks.

<h2 id="BlockMode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/cipher.go#L35">BlockMode</a>
    <a href="#BlockMode">¶</a></h2>
<pre>type BlockMode interface {
    <span class="comment">// BlockSize returns the mode&#39;s block size.</span>
    BlockSize() <a href="/builtin/#int">int</a>

    <span class="comment">// CryptBlocks encrypts or decrypts a number of blocks. The length of</span>
    <span class="comment">// src must be a multiple of the block size. Dst and src must overlap</span>
    <span class="comment">// entirely or not at all.</span>
    <span class="comment">//</span>
    <span class="comment">// If len(dst) &lt; len(src), CryptBlocks should panic. It is acceptable</span>
    <span class="comment">// to pass a dst bigger than src, and in that case, CryptBlocks will</span>
    <span class="comment">// only update dst[:len(src)] and will not touch the rest of dst.</span>
    <span class="comment">//</span>
    <span class="comment">// Multiple calls to CryptBlocks behave as if the concatenation of</span>
    <span class="comment">// the src buffers was passed in a single run. That is, BlockMode</span>
    <span class="comment">// maintains state and does not reset at each CryptBlocks call.</span>
    CryptBlocks(dst, src []<a href="/builtin/#byte">byte</a>)
}</pre>

A BlockMode represents a block cipher running in a block-based mode (CBC, ECB
etc).

<h3 id="NewCBCDecrypter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/cbc.go#L90">NewCBCDecrypter</a>
    <a href="#NewCBCDecrypter">¶</a></h3>
<pre>func NewCBCDecrypter(b <a href="#Block">Block</a>, iv []<a href="/builtin/#byte">byte</a>) <a href="#BlockMode">BlockMode</a></pre>

NewCBCDecrypter returns a BlockMode which decrypts in cipher block chaining
mode, using the given Block. The length of iv must be the same as the Block's
block size and must match the iv used to encrypt the data.

<a id="exampleNewCBCDecrypter"></a>
Example:

    // Load your secret key from a safe place and reuse it across multiple
    // NewCipher calls. (Obviously don't use this example key for anything
    // real.) If you want to convert a passphrase to a key, use a suitable
    // package like bcrypt or scrypt.
    key, _ := hex.DecodeString("6368616e676520746869732070617373")
    ciphertext, _ := hex.DecodeString("73c86d43a9d700a253a96c85b0f6b03ac9792e0e757f869cca306bd3cba1c62b")

    block, err := aes.NewCipher(key)
    if err != nil {
        panic(err)
    }

    // The IV needs to be unique, but not secure. Therefore it's common to
    // include it at the beginning of the ciphertext.
    if len(ciphertext) < aes.BlockSize {
        panic("ciphertext too short")
    }
    iv := ciphertext[:aes.BlockSize]
    ciphertext = ciphertext[aes.BlockSize:]

    // CBC mode always works in whole blocks.
    if len(ciphertext)%aes.BlockSize != 0 {
        panic("ciphertext is not a multiple of the block size")
    }

    mode := cipher.NewCBCDecrypter(block, iv)

    // CryptBlocks can work in-place if the two arguments are the same.
    mode.CryptBlocks(ciphertext, ciphertext)

    // If the original plaintext lengths are not a multiple of the block
    // size, padding would have to be added when encrypting, which would be
    // removed at this point. For an example, see
    // https://tools.ietf.org/html/rfc5246#section-6.2.3.2. However, it's
    // critical to note that ciphertexts must be authenticated (i.e. by
    // using crypto/hmac) before being decrypted in order to avoid creating
    // a padding oracle.

    fmt.Printf("%s\n", ciphertext)
    // Output: exampleplaintext

<h3 id="NewCBCEncrypter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/cbc.go#L33">NewCBCEncrypter</a>
    <a href="#NewCBCEncrypter">¶</a></h3>
<pre>func NewCBCEncrypter(b <a href="#Block">Block</a>, iv []<a href="/builtin/#byte">byte</a>) <a href="#BlockMode">BlockMode</a></pre>

NewCBCEncrypter returns a BlockMode which encrypts in cipher block chaining
mode, using the given Block. The length of iv must be the same as the Block's
block size.

<a id="exampleNewCBCEncrypter"></a>
Example:

    // Load your secret key from a safe place and reuse it across multiple
    // NewCipher calls. (Obviously don't use this example key for anything
    // real.) If you want to convert a passphrase to a key, use a suitable
    // package like bcrypt or scrypt.
    key, _ := hex.DecodeString("6368616e676520746869732070617373")
    plaintext := []byte("exampleplaintext")

    // CBC mode works on blocks so plaintexts may need to be padded to the
    // next whole block. For an example of such padding, see
    // https://tools.ietf.org/html/rfc5246#section-6.2.3.2. Here we'll
    // assume that the plaintext is already of the correct length.
    if len(plaintext)%aes.BlockSize != 0 {
        panic("plaintext is not a multiple of the block size")
    }

    block, err := aes.NewCipher(key)
    if err != nil {
        panic(err)
    }

    // The IV needs to be unique, but not secure. Therefore it's common to
    // include it at the beginning of the ciphertext.
    ciphertext := make([]byte, aes.BlockSize+len(plaintext))
    iv := ciphertext[:aes.BlockSize]
    if _, err := io.ReadFull(rand.Reader, iv); err != nil {
        panic(err)
    }

    mode := cipher.NewCBCEncrypter(block, iv)
    mode.CryptBlocks(ciphertext[aes.BlockSize:], plaintext)

    // It's important to remember that ciphertexts must be authenticated
    // (i.e. by using crypto/hmac) as well as being encrypted in order to
    // be secure.

    fmt.Printf("%x\n", ciphertext)

<h2 id="Stream">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/cipher.go#L19">Stream</a>
    <a href="#Stream">¶</a></h2>
<pre>type Stream interface {
    <span class="comment">// XORKeyStream XORs each byte in the given slice with a byte from the</span>
    <span class="comment">// cipher&#39;s key stream. Dst and src must overlap entirely or not at all.</span>
    <span class="comment">//</span>
    <span class="comment">// If len(dst) &lt; len(src), XORKeyStream should panic. It is acceptable</span>
    <span class="comment">// to pass a dst bigger than src, and in that case, XORKeyStream will</span>
    <span class="comment">// only update dst[:len(src)] and will not touch the rest of dst.</span>
    <span class="comment">//</span>
    <span class="comment">// Multiple calls to XORKeyStream behave as if the concatenation of</span>
    <span class="comment">// the src buffers was passed in a single run. That is, Stream</span>
    <span class="comment">// maintains state and does not reset at each XORKeyStream call.</span>
    XORKeyStream(dst, src []<a href="/builtin/#byte">byte</a>)
}</pre>

A Stream represents a stream cipher.

<h3 id="NewCFBDecrypter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/cfb.go#L42">NewCFBDecrypter</a>
    <a href="#NewCFBDecrypter">¶</a></h3>
<pre>func NewCFBDecrypter(block <a href="#Block">Block</a>, iv []<a href="/builtin/#byte">byte</a>) <a href="#Stream">Stream</a></pre>

NewCFBDecrypter returns a Stream which decrypts with cipher feedback mode, using
the given Block. The iv must be the same length as the Block's block size.

<a id="exampleNewCFBDecrypter"></a>
Example:

    // Load your secret key from a safe place and reuse it across multiple
    // NewCipher calls. (Obviously don't use this example key for anything
    // real.) If you want to convert a passphrase to a key, use a suitable
    // package like bcrypt or scrypt.
    key, _ := hex.DecodeString("6368616e676520746869732070617373")
    ciphertext, _ := hex.DecodeString("7dd015f06bec7f1b8f6559dad89f4131da62261786845100056b353194ad")

    block, err := aes.NewCipher(key)
    if err != nil {
        panic(err)
    }

    // The IV needs to be unique, but not secure. Therefore it's common to
    // include it at the beginning of the ciphertext.
    if len(ciphertext) < aes.BlockSize {
        panic("ciphertext too short")
    }
    iv := ciphertext[:aes.BlockSize]
    ciphertext = ciphertext[aes.BlockSize:]

    stream := cipher.NewCFBDecrypter(block, iv)

    // XORKeyStream can work in-place if the two arguments are the same.
    stream.XORKeyStream(ciphertext, ciphertext)
    fmt.Printf("%s", ciphertext)
    // Output: some plaintext

<h3 id="NewCFBEncrypter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/cfb.go#L35">NewCFBEncrypter</a>
    <a href="#NewCFBEncrypter">¶</a></h3>
<pre>func NewCFBEncrypter(block <a href="#Block">Block</a>, iv []<a href="/builtin/#byte">byte</a>) <a href="#Stream">Stream</a></pre>

NewCFBEncrypter returns a Stream which encrypts with cipher feedback mode, using
the given Block. The iv must be the same length as the Block's block size.

<a id="exampleNewCFBEncrypter"></a>
Example:

    // Load your secret key from a safe place and reuse it across multiple
    // NewCipher calls. (Obviously don't use this example key for anything
    // real.) If you want to convert a passphrase to a key, use a suitable
    // package like bcrypt or scrypt.
    key, _ := hex.DecodeString("6368616e676520746869732070617373")
    plaintext := []byte("some plaintext")

    block, err := aes.NewCipher(key)
    if err != nil {
        panic(err)
    }

    // The IV needs to be unique, but not secure. Therefore it's common to
    // include it at the beginning of the ciphertext.
    ciphertext := make([]byte, aes.BlockSize+len(plaintext))
    iv := ciphertext[:aes.BlockSize]
    if _, err := io.ReadFull(rand.Reader, iv); err != nil {
        panic(err)
    }

    stream := cipher.NewCFBEncrypter(block, iv)
    stream.XORKeyStream(ciphertext[aes.BlockSize:], plaintext)

    // It's important to remember that ciphertexts must be authenticated
    // (i.e. by using crypto/hmac) as well as being encrypted in order to
    // be secure.
    fmt.Printf("%x\n", ciphertext)

<h3 id="NewCTR">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/ctr.go#L23">NewCTR</a>
    <a href="#NewCTR">¶</a></h3>
<pre>func NewCTR(block <a href="#Block">Block</a>, iv []<a href="/builtin/#byte">byte</a>) <a href="#Stream">Stream</a></pre>

NewCTR returns a Stream which encrypts/decrypts using the given Block in counter
mode. The length of iv must be the same as the Block's block size.

<a id="exampleNewCTR"></a>
Example:

    // Load your secret key from a safe place and reuse it across multiple
    // NewCipher calls. (Obviously don't use this example key for anything
    // real.) If you want to convert a passphrase to a key, use a suitable
    // package like bcrypt or scrypt.
    key, _ := hex.DecodeString("6368616e676520746869732070617373")
    plaintext := []byte("some plaintext")

    block, err := aes.NewCipher(key)
    if err != nil {
        panic(err)
    }

    // The IV needs to be unique, but not secure. Therefore it's common to
    // include it at the beginning of the ciphertext.
    ciphertext := make([]byte, aes.BlockSize+len(plaintext))
    iv := ciphertext[:aes.BlockSize]
    if _, err := io.ReadFull(rand.Reader, iv); err != nil {
        panic(err)
    }

    stream := cipher.NewCTR(block, iv)
    stream.XORKeyStream(ciphertext[aes.BlockSize:], plaintext)

    // It's important to remember that ciphertexts must be authenticated
    // (i.e. by using crypto/hmac) as well as being encrypted in order to
    // be secure.

    // CTR mode is the same for both encryption and decryption, so we can
    // also decrypt that ciphertext with NewCTR.

    plaintext2 := make([]byte, len(plaintext))
    stream = cipher.NewCTR(block, iv)
    stream.XORKeyStream(plaintext2, ciphertext[aes.BlockSize:])

    fmt.Printf("%s\n", plaintext2)
    // Output: some plaintext

<h3 id="NewOFB">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/ofb.go#L9">NewOFB</a>
    <a href="#NewOFB">¶</a></h3>
<pre>func NewOFB(b <a href="#Block">Block</a>, iv []<a href="/builtin/#byte">byte</a>) <a href="#Stream">Stream</a></pre>

NewOFB returns a Stream that encrypts or decrypts using the block cipher b in
output feedback mode. The initialization vector iv's length must be equal to b's
block size.

<a id="exampleNewOFB"></a>
Example:

    // Load your secret key from a safe place and reuse it across multiple
    // NewCipher calls. (Obviously don't use this example key for anything
    // real.) If you want to convert a passphrase to a key, use a suitable
    // package like bcrypt or scrypt.
    key, _ := hex.DecodeString("6368616e676520746869732070617373")
    plaintext := []byte("some plaintext")

    block, err := aes.NewCipher(key)
    if err != nil {
        panic(err)
    }

    // The IV needs to be unique, but not secure. Therefore it's common to
    // include it at the beginning of the ciphertext.
    ciphertext := make([]byte, aes.BlockSize+len(plaintext))
    iv := ciphertext[:aes.BlockSize]
    if _, err := io.ReadFull(rand.Reader, iv); err != nil {
        panic(err)
    }

    stream := cipher.NewOFB(block, iv)
    stream.XORKeyStream(ciphertext[aes.BlockSize:], plaintext)

    // It's important to remember that ciphertexts must be authenticated
    // (i.e. by using crypto/hmac) as well as being encrypted in order to
    // be secure.

    // OFB mode is the same for both encryption and decryption, so we can
    // also decrypt that ciphertext with NewOFB.

    plaintext2 := make([]byte, len(plaintext))
    stream = cipher.NewOFB(block, iv)
    stream.XORKeyStream(plaintext2, ciphertext[aes.BlockSize:])

    fmt.Printf("%s\n", plaintext2)
    // Output: some plaintext

<h2 id="StreamReader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/io.go#L4">StreamReader</a>
    <a href="#StreamReader">¶</a></h2>
<pre>type StreamReader struct {
<span id="StreamReader.S"></span>    S <a href="#Stream">Stream</a>
<span id="StreamReader.R"></span>    R <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>
}</pre>

StreamReader wraps a Stream into an io.Reader. It calls XORKeyStream to process
each slice of data which passes through.

<a id="exampleStreamReader"></a>
Example:

    // Load your secret key from a safe place and reuse it across multiple
    // NewCipher calls. (Obviously don't use this example key for anything
    // real.) If you want to convert a passphrase to a key, use a suitable
    // package like bcrypt or scrypt.
    key, _ := hex.DecodeString("6368616e676520746869732070617373")

    inFile, err := os.Open("encrypted-file")
    if err != nil {
        panic(err)
    }
    defer inFile.Close()

    block, err := aes.NewCipher(key)
    if err != nil {
        panic(err)
    }

    // If the key is unique for each ciphertext, then it's ok to use a zero
    // IV.
    var iv [aes.BlockSize]byte
    stream := cipher.NewOFB(block, iv[:])

    outFile, err := os.OpenFile("decrypted-file", os.O_WRONLY|os.O_CREATE|os.O_TRUNC, 0600)
    if err != nil {
        panic(err)
    }
    defer outFile.Close()

    reader := &cipher.StreamReader{S: stream, R: inFile}
    // Copy the input file to the output file, decrypting as we go.
    if _, err := io.Copy(outFile, reader); err != nil {
        panic(err)
    }

    // Note that this example is simplistic in that it omits any
    // authentication of the encrypted data. If you were actually to use
    // StreamReader in this manner, an attacker could flip arbitrary bits in
    // the output.

<h3 id="StreamReader.Read">func (StreamReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/io.go#L9">Read</a>
    <a href="#StreamReader.Read">¶</a></h3>
<pre>func (r <a href="#StreamReader">StreamReader</a>) Read(dst []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="StreamWriter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/io.go#L20">StreamWriter</a>
    <a href="#StreamWriter">¶</a></h2>
<pre>type StreamWriter struct {
<span id="StreamWriter.S"></span>    S   <a href="#Stream">Stream</a>
<span id="StreamWriter.W"></span>    W   <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>
<span id="StreamWriter.Err"></span>    Err <a href="/builtin/#error">error</a> <span class="comment">// unused</span>
}</pre>

StreamWriter wraps a Stream into an io.Writer. It calls XORKeyStream to process
each slice of data which passes through. If any Write call returns short then
the StreamWriter is out of sync and must be discarded. A StreamWriter has no
internal buffering; Close does not need to be called to flush write data.

<a id="exampleStreamWriter"></a>
Example:

    // Load your secret key from a safe place and reuse it across multiple
    // NewCipher calls. (Obviously don't use this example key for anything
    // real.) If you want to convert a passphrase to a key, use a suitable
    // package like bcrypt or scrypt.
    key, _ := hex.DecodeString("6368616e676520746869732070617373")

    inFile, err := os.Open("plaintext-file")
    if err != nil {
        panic(err)
    }
    defer inFile.Close()

    block, err := aes.NewCipher(key)
    if err != nil {
        panic(err)
    }

    // If the key is unique for each ciphertext, then it's ok to use a zero
    // IV.
    var iv [aes.BlockSize]byte
    stream := cipher.NewOFB(block, iv[:])

    outFile, err := os.OpenFile("encrypted-file", os.O_WRONLY|os.O_CREATE|os.O_TRUNC, 0600)
    if err != nil {
        panic(err)
    }
    defer outFile.Close()

    writer := &cipher.StreamWriter{S: stream, W: outFile}
    // Copy the input file to the output file, encrypting as we go.
    if _, err := io.Copy(writer, inFile); err != nil {
        panic(err)
    }

    // Note that this example is simplistic in that it omits any
    // authentication of the encrypted data. If you were actually to use
    // StreamReader in this manner, an attacker could flip arbitrary bits in
    // the decrypted result.

<h3 id="StreamWriter.Close">func (StreamWriter) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/io.go#L38">Close</a>
    <a href="#StreamWriter.Close">¶</a></h3>
<pre>func (w <a href="#StreamWriter">StreamWriter</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the underlying Writer and returns its Close return value, if the
Writer is also an io.Closer. Otherwise it returns nil.

<h3 id="StreamWriter.Write">func (StreamWriter) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/cipher/io.go#L26">Write</a>
    <a href="#StreamWriter.Write">¶</a></h3>
<pre>func (w <a href="#StreamWriter">StreamWriter</a>) Write(src []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>



