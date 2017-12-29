version: 1.9.2
## package tls

  `import "crypto/tls"`

## Overview

Package tls partially implements TLS 1.2, as specified in RFC 5246.

## Index

- [Constants](#pkg-constants)
- [func Listen(network, laddr string, config *Config) (net.Listener, error)](#Listen)
- [func NewListener(inner net.Listener, config *Config) net.Listener](#NewListener)
- [type Certificate](#Certificate)
  - [func LoadX509KeyPair(certFile, keyFile string) (Certificate, error)](#LoadX509KeyPair)
  - [func X509KeyPair(certPEMBlock, keyPEMBlock []byte) (Certificate, error)](#X509KeyPair)
- [type CertificateRequestInfo](#CertificateRequestInfo)
- [type ClientAuthType](#ClientAuthType)
- [type ClientHelloInfo](#ClientHelloInfo)
- [type ClientSessionCache](#ClientSessionCache)
  - [func NewLRUClientSessionCache(capacity int) ClientSessionCache](#NewLRUClientSessionCache)
- [type ClientSessionState](#ClientSessionState)
- [type Config](#Config)
  - [func (c *Config) BuildNameToCertificate()](#Config.BuildNameToCertificate)
  - [func (c *Config) Clone() *Config](#Config.Clone)
  - [func (c *Config) SetSessionTicketKeys(keys [][32]byte)](#Config.SetSessionTicketKeys)
- [type Conn](#Conn)
  - [func Client(conn net.Conn, config *Config) *Conn](#Client)
  - [func Dial(network, addr string, config *Config) (*Conn, error)](#Dial)
  - [func DialWithDialer(dialer *net.Dialer, network, addr string, config *Config) (*Conn, error)](#DialWithDialer)
  - [func Server(conn net.Conn, config *Config) *Conn](#Server)
  - [func (c *Conn) Close() error](#Conn.Close)
  - [func (c *Conn) CloseWrite() error](#Conn.CloseWrite)
  - [func (c *Conn) ConnectionState() ConnectionState](#Conn.ConnectionState)
  - [func (c *Conn) Handshake() error](#Conn.Handshake)
  - [func (c *Conn) LocalAddr() net.Addr](#Conn.LocalAddr)
  - [func (c *Conn) OCSPResponse() []byte](#Conn.OCSPResponse)
  - [func (c *Conn) Read(b []byte) (n int, err error)](#Conn.Read)
  - [func (c *Conn) RemoteAddr() net.Addr](#Conn.RemoteAddr)
  - [func (c *Conn) SetDeadline(t time.Time) error](#Conn.SetDeadline)
  - [func (c *Conn) SetReadDeadline(t time.Time) error](#Conn.SetReadDeadline)
  - [func (c *Conn) SetWriteDeadline(t time.Time) error](#Conn.SetWriteDeadline)
  - [func (c *Conn) VerifyHostname(host string) error](#Conn.VerifyHostname)
  - [func (c *Conn) Write(b []byte) (int, error)](#Conn.Write)
- [type ConnectionState](#ConnectionState)
- [type CurveID](#CurveID)
- [type RecordHeaderError](#RecordHeaderError)
  - [func (e RecordHeaderError) Error() string](#RecordHeaderError.Error)
- [type RenegotiationSupport](#RenegotiationSupport)
- [type SignatureScheme](#SignatureScheme)
- [Bugs](#pkg-note-BUG)

### Examples

- [Config (KeyLogWriter)](#example_Config_keyLogWriter)
- [Dial](#example_Dial)

### Package files
 [alert.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/alert.go) [cipher_suites.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/cipher_suites.go) [common.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go) [conn.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go) [handshake_client.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/handshake_client.go) [handshake_messages.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/handshake_messages.go) [handshake_server.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/handshake_server.go) [key_agreement.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/key_agreement.go) [prf.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/prf.go) [ticket.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/ticket.go) [tls.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/tls.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="TLS_RSA_WITH_RC4_128_SHA">TLS_RSA_WITH_RC4_128_SHA</span>                <a href="/builtin/#uint16">uint16</a> = 0x0005
    <span id="TLS_RSA_WITH_3DES_EDE_CBC_SHA">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span>           <a href="/builtin/#uint16">uint16</a> = 0x000a
    <span id="TLS_RSA_WITH_AES_128_CBC_SHA">TLS_RSA_WITH_AES_128_CBC_SHA</span>            <a href="/builtin/#uint16">uint16</a> = 0x002f
    <span id="TLS_RSA_WITH_AES_256_CBC_SHA">TLS_RSA_WITH_AES_256_CBC_SHA</span>            <a href="/builtin/#uint16">uint16</a> = 0x0035
    <span id="TLS_RSA_WITH_AES_128_CBC_SHA256">TLS_RSA_WITH_AES_128_CBC_SHA256</span>         <a href="/builtin/#uint16">uint16</a> = 0x003c
    <span id="TLS_RSA_WITH_AES_128_GCM_SHA256">TLS_RSA_WITH_AES_128_GCM_SHA256</span>         <a href="/builtin/#uint16">uint16</a> = 0x009c
    <span id="TLS_RSA_WITH_AES_256_GCM_SHA384">TLS_RSA_WITH_AES_256_GCM_SHA384</span>         <a href="/builtin/#uint16">uint16</a> = 0x009d
    <span id="TLS_ECDHE_ECDSA_WITH_RC4_128_SHA">TLS_ECDHE_ECDSA_WITH_RC4_128_SHA</span>        <a href="/builtin/#uint16">uint16</a> = 0xc007
    <span id="TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span>    <a href="/builtin/#uint16">uint16</a> = 0xc009
    <span id="TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span>    <a href="/builtin/#uint16">uint16</a> = 0xc00a
    <span id="TLS_ECDHE_RSA_WITH_RC4_128_SHA">TLS_ECDHE_RSA_WITH_RC4_128_SHA</span>          <a href="/builtin/#uint16">uint16</a> = 0xc011
    <span id="TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA">TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA</span>     <a href="/builtin/#uint16">uint16</a> = 0xc012
    <span id="TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span>      <a href="/builtin/#uint16">uint16</a> = 0xc013
    <span id="TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span>      <a href="/builtin/#uint16">uint16</a> = 0xc014
    <span id="TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span> <a href="/builtin/#uint16">uint16</a> = 0xc023
    <span id="TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span>   <a href="/builtin/#uint16">uint16</a> = 0xc027
    <span id="TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span>   <a href="/builtin/#uint16">uint16</a> = 0xc02f
    <span id="TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span> <a href="/builtin/#uint16">uint16</a> = 0xc02b
    <span id="TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384">TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384</span>   <a href="/builtin/#uint16">uint16</a> = 0xc030
    <span id="TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span> <a href="/builtin/#uint16">uint16</a> = 0xc02c
    <span id="TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305">TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305</span>    <a href="/builtin/#uint16">uint16</a> = 0xcca8
    <span id="TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305">TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305</span>  <a href="/builtin/#uint16">uint16</a> = 0xcca9

    <span class="comment">// TLS_FALLBACK_SCSV isn&#39;t a standard cipher suite but an indicator</span>
    <span class="comment">// that the client is doing version fallback. See</span>
    <span class="comment">// https://tools.ietf.org/html/rfc7507.</span>
    <span id="TLS_FALLBACK_SCSV">TLS_FALLBACK_SCSV</span> <a href="/builtin/#uint16">uint16</a> = 0x5600
)</pre>

A list of cipher suite IDs that are, or have been, implemented by this package.

Taken from http://www.iana.org/assignments/tls-parameters/tls-parameters.xml

<pre>const (
    <span id="VersionSSL30">VersionSSL30</span> = 0x0300
    <span id="VersionTLS10">VersionTLS10</span> = 0x0301
    <span id="VersionTLS11">VersionTLS11</span> = 0x0302
    <span id="VersionTLS12">VersionTLS12</span> = 0x0303
)</pre>


<h2 id="Listen">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/tls.go#L64">Listen</a>
    <a href="#Listen">¶</a></h2>
<pre>func Listen(network, laddr <a href="/builtin/#string">string</a>, config *<a href="#Config">Config</a>) (<a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>, <a href="/builtin/#error">error</a>)</pre>

Listen creates a TLS listener accepting connections on the given network address
using net.Listen. The configuration config must be non-nil and must include at
least one certificate or else set GetCertificate.

<h2 id="NewListener">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/tls.go#L53">NewListener</a>
    <a href="#NewListener">¶</a></h2>
<pre>func NewListener(inner <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>, config *<a href="#Config">Config</a>) <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a></pre>

NewListener creates a Listener which accepts connections from an inner Listener
and wraps each connection with Server. The configuration config must be non-nil
and must include at least one certificate or else set GetCertificate.

<h2 id="Certificate">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L787">Certificate</a>
    <a href="#Certificate">¶</a></h2>
<pre>type Certificate struct {
<span id="Certificate.Certificate"></span>    Certificate [][]<a href="/builtin/#byte">byte</a>
<span id="Certificate.PrivateKey"></span>    <span class="comment">// PrivateKey contains the private key corresponding to the public key</span>
    <span class="comment">// in Leaf. For a server, this must implement crypto.Signer and/or</span>
    <span class="comment">// crypto.Decrypter, with an RSA or ECDSA PublicKey. For a client</span>
    <span class="comment">// (performing client authentication), this must be a crypto.Signer</span>
    <span class="comment">// with an RSA or ECDSA PublicKey.</span>
    PrivateKey <a href="/crypto/">crypto</a>.<a href="/crypto/#PrivateKey">PrivateKey</a>
<span id="Certificate.OCSPStaple"></span>    <span class="comment">// OCSPStaple contains an optional OCSP response which will be served</span>
    <span class="comment">// to clients that request it.</span>
    OCSPStaple []<a href="/builtin/#byte">byte</a>
<span id="Certificate.SignedCertificateTimestamps"></span>    <span class="comment">// SignedCertificateTimestamps contains an optional list of Signed</span>
    <span class="comment">// Certificate Timestamps which will be served to clients that request it.</span>
    SignedCertificateTimestamps [][]<a href="/builtin/#byte">byte</a>
<span id="Certificate.Leaf"></span>    <span class="comment">// Leaf is the parsed form of the leaf certificate, which may be</span>
    <span class="comment">// initialized using x509.ParseCertificate to reduce per-handshake</span>
    <span class="comment">// processing for TLS clients doing client authentication. If nil, the</span>
    <span class="comment">// leaf certificate will be parsed as needed.</span>
    Leaf *<a href="/crypto/x509/">x509</a>.<a href="/crypto/x509/#Certificate">Certificate</a>
}</pre>

A Certificate is a chain of one or more certificates, leaf first.

<h3 id="LoadX509KeyPair">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/tls.go#L168">LoadX509KeyPair</a>
    <a href="#LoadX509KeyPair">¶</a></h3>
<pre>func LoadX509KeyPair(certFile, keyFile <a href="/builtin/#string">string</a>) (<a href="#Certificate">Certificate</a>, <a href="/builtin/#error">error</a>)</pre>

LoadX509KeyPair reads and parses a public/private key pair from a pair of files.
The files must contain PEM encoded data. The certificate file may contain
intermediate certificates following the leaf certificate to form a certificate
chain. On successful return, Certificate.Leaf will be nil because the parsed
form of the certificate is not retained.

<h3 id="X509KeyPair">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/tls.go#L183">X509KeyPair</a>
    <a href="#X509KeyPair">¶</a></h3>
<pre>func X509KeyPair(certPEMBlock, keyPEMBlock []<a href="/builtin/#byte">byte</a>) (<a href="#Certificate">Certificate</a>, <a href="/builtin/#error">error</a>)</pre>

X509KeyPair parses a public/private key pair from a pair of PEM encoded data. On
successful return, Certificate.Leaf will be nil because the parsed form of the
certificate is not retained.

<h2 id="CertificateRequestInfo">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L284">CertificateRequestInfo</a>
    <a href="#CertificateRequestInfo">¶</a></h2>
<pre>type CertificateRequestInfo struct {
<span id="CertificateRequestInfo.AcceptableCAs"></span>    <span class="comment">// AcceptableCAs contains zero or more, DER-encoded, X.501</span>
    <span class="comment">// Distinguished Names. These are the names of root or intermediate CAs</span>
    <span class="comment">// that the server wishes the returned certificate to be signed by. An</span>
    <span class="comment">// empty slice indicates that the server has no preference.</span>
    AcceptableCAs [][]<a href="/builtin/#byte">byte</a>

<span id="CertificateRequestInfo.SignatureSchemes"></span>    <span class="comment">// SignatureSchemes lists the signature schemes that the server is</span>
    <span class="comment">// willing to verify.</span>
    SignatureSchemes []<a href="#SignatureScheme">SignatureScheme</a>
}</pre>

CertificateRequestInfo contains information from a server's CertificateRequest
message, which is used to demand a certificate and proof of control from a
client.

<h2 id="ClientAuthType">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L175">ClientAuthType</a>
    <a href="#ClientAuthType">¶</a></h2>
<pre>type ClientAuthType <a href="/builtin/#int">int</a></pre>

ClientAuthType declares the policy the server will follow for TLS Client
Authentication.

<pre>const (
    <span id="NoClientCert">NoClientCert</span> <a href="#ClientAuthType">ClientAuthType</a> = <a href="/builtin/#iota">iota</a>
    <span id="RequestClientCert">RequestClientCert</span>
    <span id="RequireAnyClientCert">RequireAnyClientCert</span>
    <span id="VerifyClientCertIfGiven">VerifyClientCertIfGiven</span>
    <span id="RequireAndVerifyClientCert">RequireAndVerifyClientCert</span>
)</pre>


<h2 id="ClientHelloInfo">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L231">ClientHelloInfo</a>
    <a href="#ClientHelloInfo">¶</a></h2>
<pre>type ClientHelloInfo struct {
<span id="ClientHelloInfo.CipherSuites"></span>    <span class="comment">// CipherSuites lists the CipherSuites supported by the client (e.g.</span>
    <span class="comment">// TLS_RSA_WITH_RC4_128_SHA).</span>
    CipherSuites []<a href="/builtin/#uint16">uint16</a>

<span id="ClientHelloInfo.ServerName"></span>    <span class="comment">// ServerName indicates the name of the server requested by the client</span>
    <span class="comment">// in order to support virtual hosting. ServerName is only set if the</span>
    <span class="comment">// client is using SNI (see</span>
    <span class="comment">// http://tools.ietf.org/html/rfc4366#section-3.1).</span>
    ServerName <a href="/builtin/#string">string</a>

<span id="ClientHelloInfo.SupportedCurves"></span>    <span class="comment">// SupportedCurves lists the elliptic curves supported by the client.</span>
    <span class="comment">// SupportedCurves is set only if the Supported Elliptic Curves</span>
    <span class="comment">// Extension is being used (see</span>
    <span class="comment">// http://tools.ietf.org/html/rfc4492#section-5.1.1).</span>
    SupportedCurves []<a href="#CurveID">CurveID</a>

<span id="ClientHelloInfo.SupportedPoints"></span>    <span class="comment">// SupportedPoints lists the point formats supported by the client.</span>
    <span class="comment">// SupportedPoints is set only if the Supported Point Formats Extension</span>
    <span class="comment">// is being used (see</span>
    <span class="comment">// http://tools.ietf.org/html/rfc4492#section-5.1.2).</span>
    SupportedPoints []<a href="/builtin/#uint8">uint8</a>

<span id="ClientHelloInfo.SignatureSchemes"></span>    <span class="comment">// SignatureSchemes lists the signature and hash schemes that the client</span>
    <span class="comment">// is willing to verify. SignatureSchemes is set only if the Signature</span>
    <span class="comment">// Algorithms Extension is being used (see</span>
    <span class="comment">// https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1).</span>
    SignatureSchemes []<a href="#SignatureScheme">SignatureScheme</a>

<span id="ClientHelloInfo.SupportedProtos"></span>    <span class="comment">// SupportedProtos lists the application protocols supported by the client.</span>
    <span class="comment">// SupportedProtos is set only if the Application-Layer Protocol</span>
    <span class="comment">// Negotiation Extension is being used (see</span>
    <span class="comment">// https://tools.ietf.org/html/rfc7301#section-3.1).</span>
    <span class="comment">//</span>
    <span class="comment">// Servers can select a protocol by setting Config.NextProtos in a</span>
    <span class="comment">// GetConfigForClient return value.</span>
    SupportedProtos []<a href="/builtin/#string">string</a>

<span id="ClientHelloInfo.SupportedVersions"></span>    <span class="comment">// SupportedVersions lists the TLS versions supported by the client.</span>
    <span class="comment">// For TLS versions less than 1.3, this is extrapolated from the max</span>
    <span class="comment">// version advertised by the client, so values other than the greatest</span>
    <span class="comment">// might be rejected if used.</span>
    SupportedVersions []<a href="/builtin/#uint16">uint16</a>

<span id="ClientHelloInfo.Conn"></span>    <span class="comment">// Conn is the underlying net.Conn for the connection. Do not read</span>
    <span class="comment">// from, or write to, this connection; that will cause the TLS</span>
    <span class="comment">// connection to fail.</span>
    Conn <a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>
}</pre>

ClientHelloInfo contains information from a ClientHello message in order to
guide certificate selection in the GetCertificate callback.

<h2 id="ClientSessionCache">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L201">ClientSessionCache</a>
    <a href="#ClientSessionCache">¶</a></h2>
<pre>type ClientSessionCache interface {
    <span class="comment">// Get searches for a ClientSessionState associated with the given key.</span>
    <span class="comment">// On return, ok is true if one was found.</span>
    Get(sessionKey <a href="/builtin/#string">string</a>) (session *<a href="#ClientSessionState">ClientSessionState</a>, ok <a href="/builtin/#bool">bool</a>)

    <span class="comment">// Put adds the ClientSessionState to the cache with the given key.</span>
    Put(sessionKey <a href="/builtin/#string">string</a>, cs *<a href="#ClientSessionState">ClientSessionState</a>)
}</pre>

ClientSessionCache is a cache of ClientSessionState objects that can be used by
a client to resume a TLS session with a given server. ClientSessionCache
implementations should expect to be called concurrently from different
goroutines. Only ticket-based resumption is supported, not SessionID-based
resumption.

<h3 id="NewLRUClientSessionCache">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L831">NewLRUClientSessionCache</a>
    <a href="#NewLRUClientSessionCache">¶</a></h3>
<pre>func NewLRUClientSessionCache(capacity <a href="/builtin/#int">int</a>) <a href="#ClientSessionCache">ClientSessionCache</a></pre>

NewLRUClientSessionCache returns a ClientSessionCache with the given capacity
that uses an LRU strategy. If capacity is < 1, a default capacity is used
instead.

<h2 id="ClientSessionState">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L187">ClientSessionState</a>
    <a href="#ClientSessionState">¶</a></h2>
<pre>type ClientSessionState struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ClientSessionState contains the state needed by clients to resume TLS sessions.

<h2 id="Config">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L327">Config</a>
    <a href="#Config">¶</a></h2>
<pre>type Config struct {
<span id="Config.Rand"></span>    <span class="comment">// Rand provides the source of entropy for nonces and RSA blinding.</span>
    <span class="comment">// If Rand is nil, TLS uses the cryptographic random reader in package</span>
    <span class="comment">// crypto/rand.</span>
    <span class="comment">// The Reader must be safe for use by multiple goroutines.</span>
    Rand <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>

<span id="Config.Time"></span>    <span class="comment">// Time returns the current time as the number of seconds since the epoch.</span>
    <span class="comment">// If Time is nil, TLS uses time.Now.</span>
    Time func() <a href="/time/">time</a>.<a href="/time/#Time">Time</a>

<span id="Config.Certificates"></span>    <span class="comment">// Certificates contains one or more certificate chains to present to</span>
    <span class="comment">// the other side of the connection. Server configurations must include</span>
    <span class="comment">// at least one certificate or else set GetCertificate. Clients doing</span>
    <span class="comment">// client-authentication may set either Certificates or</span>
<span id="Config.GetClientCertificate"></span>    <span class="comment">// GetClientCertificate.</span>
    Certificates []<a href="#Certificate">Certificate</a>

<span id="Config.NameToCertificate"></span>    <span class="comment">// NameToCertificate maps from a certificate name to an element of</span>
    <span class="comment">// Certificates. Note that a certificate name can be of the form</span>
    <span class="comment">// &#39;*.example.com&#39; and so doesn&#39;t have to be a domain name as such.</span>
    <span class="comment">// See Config.BuildNameToCertificate</span>
    <span class="comment">// The nil value causes the first element of Certificates to be used</span>
    <span class="comment">// for all connections.</span>
    NameToCertificate map[<a href="/builtin/#string">string</a>]*<a href="#Certificate">Certificate</a>

<span id="Config.GetCertificate"></span>    <span class="comment">// GetCertificate returns a Certificate based on the given</span>
    <span class="comment">// ClientHelloInfo. It will only be called if the client supplies SNI</span>
    <span class="comment">// information or if Certificates is empty.</span>
    <span class="comment">//</span>
    <span class="comment">// If GetCertificate is nil or returns nil, then the certificate is</span>
    <span class="comment">// retrieved from NameToCertificate. If NameToCertificate is nil, the</span>
    <span class="comment">// first element of Certificates will be used.</span>
    GetCertificate func(*<a href="#ClientHelloInfo">ClientHelloInfo</a>) (*<a href="#Certificate">Certificate</a>, <a href="/builtin/#error">error</a>)

    <span class="comment">// GetClientCertificate, if not nil, is called when a server requests a</span>
    <span class="comment">// certificate from a client. If set, the contents of Certificates will</span>
    <span class="comment">// be ignored.</span>
    <span class="comment">//</span>
    <span class="comment">// If GetClientCertificate returns an error, the handshake will be</span>
    <span class="comment">// aborted and that error will be returned. Otherwise</span>
    <span class="comment">// GetClientCertificate must return a non-nil Certificate. If</span>
    <span class="comment">// Certificate.Certificate is empty then no certificate will be sent to</span>
    <span class="comment">// the server. If this is unacceptable to the server then it may abort</span>
    <span class="comment">// the handshake.</span>
    <span class="comment">//</span>
    <span class="comment">// GetClientCertificate may be called multiple times for the same</span>
    <span class="comment">// connection if renegotiation occurs or if TLS 1.3 is in use.</span>
    GetClientCertificate func(*<a href="#CertificateRequestInfo">CertificateRequestInfo</a>) (*<a href="#Certificate">Certificate</a>, <a href="/builtin/#error">error</a>)

<span id="Config.GetConfigForClient"></span>    <span class="comment">// GetConfigForClient, if not nil, is called after a ClientHello is</span>
    <span class="comment">// received from a client. It may return a non-nil Config in order to</span>
    <span class="comment">// change the Config that will be used to handle this connection. If</span>
    <span class="comment">// the returned Config is nil, the original Config will be used. The</span>
    <span class="comment">// Config returned by this callback may not be subsequently modified.</span>
    <span class="comment">//</span>
    <span class="comment">// If GetConfigForClient is nil, the Config passed to Server() will be</span>
    <span class="comment">// used for all connections.</span>
    <span class="comment">//</span>
    <span class="comment">// Uniquely for the fields in the returned Config, session ticket keys</span>
    <span class="comment">// will be duplicated from the original Config if not set.</span>
    <span class="comment">// Specifically, if SetSessionTicketKeys was called on the original</span>
    <span class="comment">// config but not on the returned config then the ticket keys from the</span>
    <span class="comment">// original config will be copied into the new config before use.</span>
    <span class="comment">// Otherwise, if SessionTicketKey was set in the original config but</span>
    <span class="comment">// not in the returned config then it will be copied into the returned</span>
    <span class="comment">// config before use. If neither of those cases applies then the key</span>
    <span class="comment">// material from the returned config will be used for session tickets.</span>
    GetConfigForClient func(*<a href="#ClientHelloInfo">ClientHelloInfo</a>) (*<a href="#Config">Config</a>, <a href="/builtin/#error">error</a>)

<span id="Config.VerifyPeerCertificate"></span>    <span class="comment">// VerifyPeerCertificate, if not nil, is called after normal</span>
    <span class="comment">// certificate verification by either a TLS client or server. It</span>
    <span class="comment">// receives the raw ASN.1 certificates provided by the peer and also</span>
    <span class="comment">// any verified chains that normal processing found. If it returns a</span>
    <span class="comment">// non-nil error, the handshake is aborted and that error results.</span>
    <span class="comment">//</span>
    <span class="comment">// If normal verification fails then the handshake will abort before</span>
    <span class="comment">// considering this callback. If normal verification is disabled by</span>
    <span class="comment">// setting InsecureSkipVerify then this callback will be considered but</span>
    <span class="comment">// the verifiedChains argument will always be nil.</span>
    VerifyPeerCertificate func(rawCerts [][]<a href="/builtin/#byte">byte</a>, verifiedChains [][]*<a href="/crypto/x509/">x509</a>.<a href="/crypto/x509/#Certificate">Certificate</a>) <a href="/builtin/#error">error</a>

<span id="Config.RootCAs"></span>    <span class="comment">// RootCAs defines the set of root certificate authorities</span>
    <span class="comment">// that clients use when verifying server certificates.</span>
    <span class="comment">// If RootCAs is nil, TLS uses the host&#39;s root CA set.</span>
    RootCAs *<a href="/crypto/x509/">x509</a>.<a href="/crypto/x509/#CertPool">CertPool</a>

<span id="Config.NextProtos"></span>    <span class="comment">// NextProtos is a list of supported, application level protocols.</span>
    NextProtos []<a href="/builtin/#string">string</a>

<span id="Config.ServerName"></span>    <span class="comment">// ServerName is used to verify the hostname on the returned</span>
    <span class="comment">// certificates unless InsecureSkipVerify is given. It is also included</span>
    <span class="comment">// in the client&#39;s handshake to support virtual hosting unless it is</span>
    <span class="comment">// an IP address.</span>
    ServerName <a href="/builtin/#string">string</a>

<span id="Config.ClientAuth"></span>    <span class="comment">// ClientAuth determines the server&#39;s policy for</span>
    <span class="comment">// TLS Client Authentication. The default is NoClientCert.</span>
    ClientAuth <a href="#ClientAuthType">ClientAuthType</a>

<span id="Config.ClientCAs"></span>    <span class="comment">// ClientCAs defines the set of root certificate authorities</span>
    <span class="comment">// that servers use if required to verify a client certificate</span>
    <span class="comment">// by the policy in ClientAuth.</span>
    ClientCAs *<a href="/crypto/x509/">x509</a>.<a href="/crypto/x509/#CertPool">CertPool</a>

<span id="Config.InsecureSkipVerify"></span>    <span class="comment">// InsecureSkipVerify controls whether a client verifies the</span>
    <span class="comment">// server&#39;s certificate chain and host name.</span>
    <span class="comment">// If InsecureSkipVerify is true, TLS accepts any certificate</span>
    <span class="comment">// presented by the server and any host name in that certificate.</span>
    <span class="comment">// In this mode, TLS is susceptible to man-in-the-middle attacks.</span>
    <span class="comment">// This should be used only for testing.</span>
    InsecureSkipVerify <a href="/builtin/#bool">bool</a>

<span id="Config.CipherSuites"></span>    <span class="comment">// CipherSuites is a list of supported cipher suites. If CipherSuites</span>
    <span class="comment">// is nil, TLS uses a list of suites supported by the implementation.</span>
    CipherSuites []<a href="/builtin/#uint16">uint16</a>

<span id="Config.PreferServerCipherSuites"></span>    <span class="comment">// PreferServerCipherSuites controls whether the server selects the</span>
    <span class="comment">// client&#39;s most preferred ciphersuite, or the server&#39;s most preferred</span>
    <span class="comment">// ciphersuite. If true then the server&#39;s preference, as expressed in</span>
    <span class="comment">// the order of elements in CipherSuites, is used.</span>
    PreferServerCipherSuites <a href="/builtin/#bool">bool</a>

<span id="Config.SessionTicketsDisabled"></span>    <span class="comment">// SessionTicketsDisabled may be set to true to disable session ticket</span>
    <span class="comment">// (resumption) support.</span>
    SessionTicketsDisabled <a href="/builtin/#bool">bool</a>

<span id="Config.SessionTicketKey"></span>    <span class="comment">// SessionTicketKey is used by TLS servers to provide session</span>
    <span class="comment">// resumption. See RFC 5077. If zero, it will be filled with</span>
    <span class="comment">// random data before the first server handshake.</span>
    <span class="comment">//</span>
    <span class="comment">// If multiple servers are terminating connections for the same host</span>
    <span class="comment">// they should all have the same SessionTicketKey. If the</span>
    <span class="comment">// SessionTicketKey leaks, previously recorded and future TLS</span>
    <span class="comment">// connections using that key are compromised.</span>
    SessionTicketKey [32]<a href="/builtin/#byte">byte</a>

    <span class="comment">// SessionCache is a cache of ClientSessionState entries for TLS session</span>
    <span class="comment">// resumption.</span>
<span id="Config.ClientSessionCache"></span>    ClientSessionCache <a href="#ClientSessionCache">ClientSessionCache</a>

<span id="Config.MinVersion"></span>    <span class="comment">// MinVersion contains the minimum SSL/TLS version that is acceptable.</span>
    <span class="comment">// If zero, then TLS 1.0 is taken as the minimum.</span>
    MinVersion <a href="/builtin/#uint16">uint16</a>

<span id="Config.MaxVersion"></span>    <span class="comment">// MaxVersion contains the maximum SSL/TLS version that is acceptable.</span>
    <span class="comment">// If zero, then the maximum version supported by this package is used,</span>
    <span class="comment">// which is currently TLS 1.2.</span>
    MaxVersion <a href="/builtin/#uint16">uint16</a>

<span id="Config.CurvePreferences"></span>    <span class="comment">// CurvePreferences contains the elliptic curves that will be used in</span>
    <span class="comment">// an ECDHE handshake, in preference order. If empty, the default will</span>
    <span class="comment">// be used.</span>
    CurvePreferences []<a href="#CurveID">CurveID</a>

<span id="Config.DynamicRecordSizingDisabled"></span>    <span class="comment">// DynamicRecordSizingDisabled disables adaptive sizing of TLS records.</span>
    <span class="comment">// When true, the largest possible TLS record size is always used. When</span>
    <span class="comment">// false, the size of TLS records may be adjusted in an attempt to</span>
    <span class="comment">// improve latency.</span>
    DynamicRecordSizingDisabled <a href="/builtin/#bool">bool</a>

<span id="Config.Renegotiation"></span>    <span class="comment">// Renegotiation controls what types of renegotiation are supported.</span>
    <span class="comment">// The default, none, is correct for the vast majority of applications.</span>
    Renegotiation <a href="#RenegotiationSupport">RenegotiationSupport</a>

<span id="Config.KeyLogWriter"></span>    <span class="comment">// KeyLogWriter optionally specifies a destination for TLS master secrets</span>
    <span class="comment">// in NSS key log format that can be used to allow external programs</span>
    <span class="comment">// such as Wireshark to decrypt TLS connections.</span>
    <span class="comment">// See https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format.</span>
    <span class="comment">// Use of KeyLogWriter compromises security and should only be</span>
    <span class="comment">// used for debugging.</span>
    KeyLogWriter <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Config structure is used to configure a TLS client or server. After one has
been passed to a TLS function it must not be modified. A Config may be reused;
the tls package will also not modify it.

<a id="example_Config_keyLogWriter"></a>
Example:

    // Debugging TLS applications by decrypting a network traffic capture.

    // WARNING: Use of KeyLogWriter compromises security and should only be
    // used for debugging.

    // Dummy test HTTP server for the example with insecure random so output is
    // reproducible.
    server := httptest.NewUnstartedServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {}))
    server.TLS = &tls.Config{
        Rand: zeroSource{}, // for example only; don't do this.
    }
    server.StartTLS()
    defer server.Close()

    // Typically the log would go to an open file:
    // w, err := os.OpenFile("tls-secrets.txt", os.O_WRONLY|os.O_CREATE|os.O_TRUNC, 0600)
    w := os.Stdout

    client := &http.Client{
        Transport: &http.Transport{
            TLSClientConfig: &tls.Config{
                KeyLogWriter: w,

                Rand:               zeroSource{}, // for reproducible output; don't do this.
                InsecureSkipVerify: true,         // test server certificate is not trusted.
            },
        },
    }
    resp, err := client.Get(server.URL)
    if err != nil {
        log.Fatalf("Failed to get URL: %v", err)
    }
    resp.Body.Close()

    // The resulting file can be used with Wireshark to decrypt the TLS
    // connection by setting (Pre)-Master-Secret log filename in SSL Protocol
    // preferences.

    // Output:
    // CLIENT_RANDOM 0000000000000000000000000000000000000000000000000000000000000000 baca0df460a688e44ce018b025183cc2353ae01f89755ef766eedd3ecc302888ee3b3a22962e45f48c20df15a98c0e80

<h3 id="Config.BuildNameToCertificate">func (*Config) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L749">BuildNameToCertificate</a>
    <a href="#Config.BuildNameToCertificate">¶</a></h3>
<pre>func (c *<a href="#Config">Config</a>) BuildNameToCertificate()</pre>

BuildNameToCertificate parses c.Certificates and builds c.NameToCertificate from
the CommonName and SubjectAlternateName fields of each of the leaf certificates.

<h3 id="Config.Clone">func (*Config) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L537">Clone</a>
    <a href="#Config.Clone">¶</a></h3>
<pre>func (c *<a href="#Config">Config</a>) Clone() *<a href="#Config">Config</a></pre>

Clone returns a shallow clone of c. It is safe to clone a Config that is being
used concurrently by a TLS client or server.

<h3 id="Config.SetSessionTicketKeys">func (*Config) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L625">SetSessionTicketKeys</a>
    <a href="#Config.SetSessionTicketKeys">¶</a></h3>
<pre>func (c *<a href="#Config">Config</a>) SetSessionTicketKeys(keys [][32]<a href="/builtin/#byte">byte</a>)</pre>

SetSessionTicketKeys updates the session ticket keys for a server. The first key
will be used when creating new tickets, while all keys can be used for
decrypting tickets. It is safe to call this function while the server is running
in order to rotate the session ticket keys. The function will panic if keys is
empty.

<h2 id="Conn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L15">Conn</a>
    <a href="#Conn">¶</a></h2>
<pre>type Conn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Conn represents a secured connection. It implements the net.Conn interface.

<h3 id="Client">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/tls.go#L29">Client</a>
    <a href="#Client">¶</a></h3>
<pre>func Client(conn <a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, config *<a href="#Config">Config</a>) *<a href="#Conn">Conn</a></pre>

Client returns a new TLS client side connection using conn as the underlying
transport. The config cannot be nil: users must set either ServerName or
InsecureSkipVerify in the config.

<h3 id="Dial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/tls.go#L159">Dial</a>
    <a href="#Dial">¶</a></h3>
<pre>func Dial(network, addr <a href="/builtin/#string">string</a>, config *<a href="#Config">Config</a>) (*<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Dial connects to the given network address using net.Dial and then initiates a
TLS handshake, returning the resulting TLS connection. Dial interprets a nil
configuration as equivalent to the zero configuration; see the documentation of
Config for the defaults.

<a id="example_Dial"></a>
Example:

    // Connecting with a custom root-certificate set.

    const rootPEM = `
    -----BEGIN CERTIFICATE-----
    MIIEBDCCAuygAwIBAgIDAjppMA0GCSqGSIb3DQEBBQUAMEIxCzAJBgNVBAYTAlVT
    MRYwFAYDVQQKEw1HZW9UcnVzdCBJbmMuMRswGQYDVQQDExJHZW9UcnVzdCBHbG9i
    YWwgQ0EwHhcNMTMwNDA1MTUxNTU1WhcNMTUwNDA0MTUxNTU1WjBJMQswCQYDVQQG
    EwJVUzETMBEGA1UEChMKR29vZ2xlIEluYzElMCMGA1UEAxMcR29vZ2xlIEludGVy
    bmV0IEF1dGhvcml0eSBHMjCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
    AJwqBHdc2FCROgajguDYUEi8iT/xGXAaiEZ+4I/F8YnOIe5a/mENtzJEiaB0C1NP
    VaTOgmKV7utZX8bhBYASxF6UP7xbSDj0U/ck5vuR6RXEz/RTDfRK/J9U3n2+oGtv
    h8DQUB8oMANA2ghzUWx//zo8pzcGjr1LEQTrfSTe5vn8MXH7lNVg8y5Kr0LSy+rE
    ahqyzFPdFUuLH8gZYR/Nnag+YyuENWllhMgZxUYi+FOVvuOAShDGKuy6lyARxzmZ
    EASg8GF6lSWMTlJ14rbtCMoU/M4iarNOz0YDl5cDfsCx3nuvRTPPuj5xt970JSXC
    DTWJnZ37DhF5iR43xa+OcmkCAwEAAaOB+zCB+DAfBgNVHSMEGDAWgBTAephojYn7
    qwVkDBF9qn1luMrMTjAdBgNVHQ4EFgQUSt0GFhu89mi1dvWBtrtiGrpagS8wEgYD
    VR0TAQH/BAgwBgEB/wIBADAOBgNVHQ8BAf8EBAMCAQYwOgYDVR0fBDMwMTAvoC2g
    K4YpaHR0cDovL2NybC5nZW90cnVzdC5jb20vY3Jscy9ndGdsb2JhbC5jcmwwPQYI
    KwYBBQUHAQEEMTAvMC0GCCsGAQUFBzABhiFodHRwOi8vZ3RnbG9iYWwtb2NzcC5n
    ZW90cnVzdC5jb20wFwYDVR0gBBAwDjAMBgorBgEEAdZ5AgUBMA0GCSqGSIb3DQEB
    BQUAA4IBAQA21waAESetKhSbOHezI6B1WLuxfoNCunLaHtiONgaX4PCVOzf9G0JY
    /iLIa704XtE7JW4S615ndkZAkNoUyHgN7ZVm2o6Gb4ChulYylYbc3GrKBIxbf/a/
    zG+FA1jDaFETzf3I93k9mTXwVqO94FntT0QJo544evZG0R0SnU++0ED8Vf4GXjza
    HFa9llF7b1cq26KqltyMdMKVvvBulRP/F/A8rLIQjcxz++iPAsbw+zOzlTvjwsto
    WHPbqCRiOwY1nQ2pM714A5AuTHhdUDqB1O6gyHA43LL5Z/qHQF1hwFGPa4NrzQU6
    yuGnBXj8ytqU0CwIPX4WecigUCAkVDNx
    -----END CERTIFICATE-----`

    // First, create the set of root certificates. For this example we only
    // have one. It's also possible to omit this in order to use the
    // default root set of the current operating system.
    roots := x509.NewCertPool()
    ok := roots.AppendCertsFromPEM([]byte(rootPEM))
    if !ok {
        panic("failed to parse root certificate")
    }

    conn, err := tls.Dial("tcp", "mail.google.com:443", &tls.Config{
        RootCAs: roots,
    })
    if err != nil {
        panic("failed to connect: " + err.Error())
    }
    conn.Close()

<h3 id="DialWithDialer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/tls.go#L88">DialWithDialer</a>
    <a href="#DialWithDialer">¶</a></h3>
<pre>func DialWithDialer(dialer *<a href="/net/">net</a>.<a href="/net/#Dialer">Dialer</a>, network, addr <a href="/builtin/#string">string</a>, config *<a href="#Config">Config</a>) (*<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

DialWithDialer connects to the given network address using dialer.Dial and then
initiates a TLS handshake, returning the resulting TLS connection. Any timeout
or deadline given in the dialer apply to connection and TLS handshake as a
whole.

DialWithDialer interprets a nil configuration as equivalent to the zero
configuration; see the documentation of Config for the defaults.

<h3 id="Server">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/tls.go#L21">Server</a>
    <a href="#Server">¶</a></h3>
<pre>func Server(conn <a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, config *<a href="#Config">Config</a>) *<a href="#Conn">Conn</a></pre>

Server returns a new TLS server side connection using conn as the underlying
transport. The configuration config must be non-nil and must include at least
one certificate or else set GetCertificate.

<h3 id="Conn.Close">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L1174">Close</a>
    <a href="#Conn.Close">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the connection.

<h3 id="Conn.CloseWrite">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L1215">CloseWrite</a>
    <a href="#Conn.CloseWrite">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) CloseWrite() <a href="/builtin/#error">error</a></pre>

CloseWrite shuts down the writing side of the connection. It should only be
called once the handshake has completed and does not call CloseWrite on the
underlying connection. Most callers should just use Close.

<h3 id="Conn.ConnectionState">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L1322">ConnectionState</a>
    <a href="#Conn.ConnectionState">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) ConnectionState() <a href="#ConnectionState">ConnectionState</a></pre>

ConnectionState returns basic TLS details about the connection.

<h3 id="Conn.Handshake">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L1240">Handshake</a>
    <a href="#Conn.Handshake">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) Handshake() <a href="/builtin/#error">error</a></pre>

Handshake runs the client or server handshake protocol if it has not yet been
run. Most uses of this package need not call Handshake explicitly: the first
Read or Write will call it automatically.

<h3 id="Conn.LocalAddr">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L100">LocalAddr</a>
    <a href="#Conn.LocalAddr">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) LocalAddr() <a href="/net/">net</a>.<a href="/net/#Addr">Addr</a></pre>

LocalAddr returns the local network address.

<h3 id="Conn.OCSPResponse">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L1354">OCSPResponse</a>
    <a href="#Conn.OCSPResponse">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) OCSPResponse() []<a href="/builtin/#byte">byte</a></pre>

OCSPResponse returns the stapled OCSP response from the TLS server, if any.
(Only valid for client connections.)

<h3 id="Conn.Read">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L1106">Read</a>
    <a href="#Conn.Read">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) Read(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read can be made to time out and return a net.Error with Timeout() == true after
a fixed time limit; see SetDeadline and SetReadDeadline.

<h3 id="Conn.RemoteAddr">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L105">RemoteAddr</a>
    <a href="#Conn.RemoteAddr">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) RemoteAddr() <a href="/net/">net</a>.<a href="/net/#Addr">Addr</a></pre>

RemoteAddr returns the remote network address.

<h3 id="Conn.SetDeadline">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L112">SetDeadline</a>
    <a href="#Conn.SetDeadline">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline sets the read and write deadlines associated with the connection. A
zero value for t means Read and Write will not time out. After a Write has timed
out, the TLS state is corrupt and all future writes will return the same error.

<h3 id="Conn.SetReadDeadline">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L118">SetReadDeadline</a>
    <a href="#Conn.SetReadDeadline">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetReadDeadline sets the read deadline on the underlying connection. A zero
value for t means Read will not time out.

<h3 id="Conn.SetWriteDeadline">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L125">SetWriteDeadline</a>
    <a href="#Conn.SetWriteDeadline">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetWriteDeadline sets the write deadline on the underlying connection. A zero
value for t means Write will not time out. After a Write has timed out, the TLS
state is corrupt and all future writes will return the same error.

<h3 id="Conn.VerifyHostname">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L1364">VerifyHostname</a>
    <a href="#Conn.VerifyHostname">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) VerifyHostname(host <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

VerifyHostname checks that the peer certificate chain is valid for connecting to
host. If so, it returns nil; if not, it returns an error describing the problem.

<h3 id="Conn.Write">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L1006">Write</a>
    <a href="#Conn.Write">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write writes data to the connection.

<h2 id="ConnectionState">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L151">ConnectionState</a>
    <a href="#ConnectionState">¶</a></h2>
<pre>type ConnectionState struct {
<span id="ConnectionState.Version"></span>    Version                     <a href="/builtin/#uint16">uint16</a>                <span class="comment">// TLS version used by the connection (e.g. VersionTLS12)</span>
<span id="ConnectionState.HandshakeComplete"></span>    HandshakeComplete           <a href="/builtin/#bool">bool</a>                  <span class="comment">// TLS handshake is complete</span>
<span id="ConnectionState.DidResume"></span>    DidResume                   <a href="/builtin/#bool">bool</a>                  <span class="comment">// connection resumes a previous TLS connection</span>
<span id="ConnectionState.CipherSuite"></span>    CipherSuite                 <a href="/builtin/#uint16">uint16</a>                <span class="comment">// cipher suite in use (TLS_RSA_WITH_RC4_128_SHA, ...)</span>
<span id="ConnectionState.NegotiatedProtocol"></span>    NegotiatedProtocol          <a href="/builtin/#string">string</a>                <span class="comment">// negotiated next protocol (not guaranteed to be from Config.NextProtos)</span>
<span id="ConnectionState.NegotiatedProtocolIsMutual"></span>    NegotiatedProtocolIsMutual  <a href="/builtin/#bool">bool</a>                  <span class="comment">// negotiated protocol was advertised by server (client side only)</span>
<span id="ConnectionState.ServerName"></span>    ServerName                  <a href="/builtin/#string">string</a>                <span class="comment">// server name requested by client, if any (server side only)</span>
<span id="ConnectionState.PeerCertificates"></span>    PeerCertificates            []*<a href="/crypto/x509/">x509</a>.<a href="/crypto/x509/#Certificate">Certificate</a>   <span class="comment">// certificate chain presented by remote peer</span>
<span id="ConnectionState.VerifiedChains"></span>    VerifiedChains              [][]*<a href="/crypto/x509/">x509</a>.<a href="/crypto/x509/#Certificate">Certificate</a> <span class="comment">// verified chains built from PeerCertificates</span>
<span id="ConnectionState.SignedCertificateTimestamps"></span>    SignedCertificateTimestamps [][]<a href="/builtin/#byte">byte</a>              <span class="comment">// SCTs from the server, if any</span>
<span id="ConnectionState.OCSPResponse"></span>    OCSPResponse                []<a href="/builtin/#byte">byte</a>                <span class="comment">// stapled OCSP response from server, if any</span>

<span id="ConnectionState.TLSUnique"></span>    <span class="comment">// TLSUnique contains the &#34;tls-unique&#34; channel binding value (see RFC</span>
    <span class="comment">// 5929, section 3). For resumed sessions this value will be nil</span>
    <span class="comment">// because resumption does not include enough context (see</span>
    <span class="comment">// https://mitls.org/pages/attacks/3SHAKE#channelbindings). This will</span>
    <span class="comment">// change in future versions of Go once the TLS master-secret fix has</span>
    <span class="comment">// been standardized and implemented.</span>
    TLSUnique []<a href="/builtin/#byte">byte</a>
}</pre>

ConnectionState records basic TLS details about the connection.

<h2 id="CurveID">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L84">CurveID</a>
    <a href="#CurveID">¶</a></h2>
<pre>type CurveID <a href="/builtin/#uint16">uint16</a></pre>

CurveID is the type of a TLS identifier for an elliptic curve. See
http://www.iana.org/assignments/tls-parameters/tls-parameters.xml#tls-parameters-8

<pre>const (
    <span id="CurveP256">CurveP256</span> <a href="#CurveID">CurveID</a> = 23
    <span id="CurveP384">CurveP384</span> <a href="#CurveID">CurveID</a> = 24
    <span id="CurveP521">CurveP521</span> <a href="#CurveID">CurveID</a> = 25
    <span id="X25519">X25519</span>    <a href="#CurveID">CurveID</a> = 29
)</pre>


<h2 id="RecordHeaderError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L534">RecordHeaderError</a>
    <a href="#RecordHeaderError">¶</a></h2>
<pre>type RecordHeaderError struct {
<span id="RecordHeaderError.Msg"></span>    <span class="comment">// Msg contains a human readable string that describes the error.</span>
    Msg <a href="/builtin/#string">string</a>
<span id="RecordHeaderError.RecordHeader"></span>    <span class="comment">// RecordHeader contains the five bytes of TLS record header that</span>
    <span class="comment">// triggered the error.</span>
    RecordHeader [5]<a href="/builtin/#byte">byte</a>
}</pre>

RecordHeaderError results when a TLS record header is invalid.

<h3 id="RecordHeaderError.Error">func (RecordHeaderError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/conn.go#L542">Error</a>
    <a href="#RecordHeaderError.Error">¶</a></h3>
<pre>func (e <a href="#RecordHeaderError">RecordHeaderError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="RenegotiationSupport">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L308">RenegotiationSupport</a>
    <a href="#RenegotiationSupport">¶</a></h2>
<pre>type RenegotiationSupport <a href="/builtin/#int">int</a></pre>

RenegotiationSupport enumerates the different levels of support for TLS
renegotiation. TLS renegotiation is the act of performing subsequent handshakes
on a connection after the first. This significantly complicates the state
machine and has been the source of numerous, subtle security issues. Initiating
a renegotiation is not supported, but support for accepting renegotiation
requests may be enabled.

Even when enabled, the server may not change its identity between handshakes
(i.e. the leaf certificate must be the same). Additionally, concurrent handshake
and application data flow is not permitted so renegotiation can only be used
with protocols that synchronise with the renegotiation, such as HTTPS.

<pre>const (
    <span class="comment">// RenegotiateNever disables renegotiation.</span>
    <span id="RenegotiateNever">RenegotiateNever</span> <a href="#RenegotiationSupport">RenegotiationSupport</a> = <a href="/builtin/#iota">iota</a>

    <span class="comment">// RenegotiateOnceAsClient allows a remote server to request</span>
    <span class="comment">// renegotiation once per connection.</span>
    <span id="RenegotiateOnceAsClient">RenegotiateOnceAsClient</span>

    <span class="comment">// RenegotiateFreelyAsClient allows a remote server to repeatedly</span>
    <span class="comment">// request renegotiation.</span>
    <span id="RenegotiateFreelyAsClient">RenegotiateFreelyAsClient</span>
)</pre>


<h2 id="SignatureScheme">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/common.go#L212">SignatureScheme</a>
    <a href="#SignatureScheme">¶</a></h2>
<pre>type SignatureScheme <a href="/builtin/#uint16">uint16</a></pre>

SignatureScheme identifies a signature algorithm supported by TLS. See
https://tools.ietf.org/html/draft-ietf-tls-tls13-18#section-4.2.3.

<pre>const (
    <span id="PKCS1WithSHA1">PKCS1WithSHA1</span>   <a href="#SignatureScheme">SignatureScheme</a> = 0x0201
    <span id="PKCS1WithSHA256">PKCS1WithSHA256</span> <a href="#SignatureScheme">SignatureScheme</a> = 0x0401
    <span id="PKCS1WithSHA384">PKCS1WithSHA384</span> <a href="#SignatureScheme">SignatureScheme</a> = 0x0501
    <span id="PKCS1WithSHA512">PKCS1WithSHA512</span> <a href="#SignatureScheme">SignatureScheme</a> = 0x0601

    <span id="PSSWithSHA256">PSSWithSHA256</span> <a href="#SignatureScheme">SignatureScheme</a> = 0x0804
    <span id="PSSWithSHA384">PSSWithSHA384</span> <a href="#SignatureScheme">SignatureScheme</a> = 0x0805
    <span id="PSSWithSHA512">PSSWithSHA512</span> <a href="#SignatureScheme">SignatureScheme</a> = 0x0806

    <span id="ECDSAWithP256AndSHA256">ECDSAWithP256AndSHA256</span> <a href="#SignatureScheme">SignatureScheme</a> = 0x0403
    <span id="ECDSAWithP384AndSHA384">ECDSAWithP384AndSHA384</span> <a href="#SignatureScheme">SignatureScheme</a> = 0x0503
    <span id="ECDSAWithP521AndSHA512">ECDSAWithP521AndSHA512</span> <a href="#SignatureScheme">SignatureScheme</a> = 0x0603
)</pre>


<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/tls/tls.go#L1)  The crypto/tls package only implements some countermeasures against Lucky13
  attacks on CBC-mode encryption, and only on SHA1 variants. See
  http://www.isg.rhul.ac.uk/tls/TLStiming.pdf and
  https://www.imperialviolet.org/2013/02/04/luckythirteen.html.


