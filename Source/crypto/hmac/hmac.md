version: 1.10
## package hmac

  `import "crypto/hmac"`

## Overview

Package hmac implements the Keyed-Hash Message Authentication Code (HMAC) as
defined in U.S. Federal Information Processing Standards Publication 198. An
HMAC is a cryptographic hash that uses a key to sign a message. The receiver
verifies the hash by recomputing it using the same key.

Receivers should be careful to use Equal to compare MACs in order to avoid
timing side-channels:

    // CheckMAC reports whether messageMAC is a valid HMAC tag for message.
    func CheckMAC(message, messageMAC, key []byte) bool {
    	mac := hmac.New(sha256.New, key)
    	mac.Write(message)
    	expectedMAC := mac.Sum(nil)
    	return hmac.Equal(messageMAC, expectedMAC)
    }

## Index

- [func Equal(mac1, mac2 []byte) bool](#Equal)
- [func New(h func() hash.Hash, key []byte) hash.Hash](#New)

### Package files
 [hmac.go](//github.com/golang/go/blob/release-branch.go1.10/src/crypto/hmac/hmac.go)

<h2 id="Equal">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/hmac/hmac.go#L86">Equal</a>
    <a href="#Equal">¶</a></h2>
<pre>func Equal(mac1, mac2 []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

Equal compares two MACs for equality without leaking timing information.

<h2 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/crypto/hmac/hmac.go#L60">New</a>
    <a href="#New">¶</a></h2>
<pre>func New(h func() <a href="/hash/">hash</a>.<a href="/hash/#Hash">Hash</a>, key []<a href="/builtin/#byte">byte</a>) <a href="/hash/">hash</a>.<a href="/hash/#Hash">Hash</a></pre>

New returns a new HMAC hash using the given hash.Hash type and key. Note that
unlike other hash implementations in the standard library, the returned Hash
does not implement encoding.BinaryMarshaler or encoding.BinaryUnmarshaler.


