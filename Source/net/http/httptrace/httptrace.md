version: 1.9.2
## package httptrace

  `import "net/http/httptrace"`

## Overview

Package httptrace provides mechanisms to trace the events within HTTP client
requests.

<a id="example"></a>
Example:

    req, _ := http.NewRequest("GET", "http://example.com", nil)
    trace := &httptrace.ClientTrace{
        GotConn: func(connInfo httptrace.GotConnInfo) {
            fmt.Printf("Got Conn: %+v\n", connInfo)
        },
        DNSDone: func(dnsInfo httptrace.DNSDoneInfo) {
            fmt.Printf("DNS Info: %+v\n", dnsInfo)
        },
    }
    req = req.WithContext(httptrace.WithClientTrace(req.Context(), trace))
    _, err := http.DefaultTransport.RoundTrip(req)
    if err != nil {
        log.Fatal(err)
    }

## Index

- [func WithClientTrace(ctx context.Context, trace *ClientTrace) context.Context](#WithClientTrace)
- [type ClientTrace](#ClientTrace)
  - [func ContextClientTrace(ctx context.Context) *ClientTrace](#ContextClientTrace)
- [type DNSDoneInfo](#DNSDoneInfo)
- [type DNSStartInfo](#DNSStartInfo)
- [type GotConnInfo](#GotConnInfo)
- [type WroteRequestInfo](#WroteRequestInfo)

### Examples

- [Package](#example)

### Package files
 [trace.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptrace/trace.go)

<h2 id="WithClientTrace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptrace/trace.go#L23">WithClientTrace</a>
    <a href="#WithClientTrace">¶</a></h2>
<pre>func WithClientTrace(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, trace *<a href="#ClientTrace">ClientTrace</a>) <a href="/context/">context</a>.<a href="/context/#Context">Context</a></pre>

WithClientTrace returns a new context based on the provided parent ctx. HTTP
client requests made with the returned context will use the provided trace
hooks, in addition to any previous hooks registered with ctx. Any hooks defined
in the provided trace will be called first.

<h2 id="ClientTrace">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptrace/trace.go#L69">ClientTrace</a>
    <a href="#ClientTrace">¶</a></h2>
<pre>type ClientTrace struct {
<span id="ClientTrace.GetConn"></span>    <span class="comment">// GetConn is called before a connection is created or</span>
    <span class="comment">// retrieved from an idle pool. The hostPort is the</span>
    <span class="comment">// &#34;host:port&#34; of the target or proxy. GetConn is called even</span>
    <span class="comment">// if there&#39;s already an idle cached connection available.</span>
    GetConn func(hostPort <a href="/builtin/#string">string</a>)

<span id="ClientTrace.GotConn"></span>    <span class="comment">// GotConn is called after a successful connection is</span>
    <span class="comment">// obtained. There is no hook for failure to obtain a</span>
    <span class="comment">// connection; instead, use the error from</span>
    <span class="comment">// Transport.RoundTrip.</span>
    GotConn func(<a href="#GotConnInfo">GotConnInfo</a>)

<span id="ClientTrace.PutIdleConn"></span>    <span class="comment">// PutIdleConn is called when the connection is returned to</span>
    <span class="comment">// the idle pool. If err is nil, the connection was</span>
    <span class="comment">// successfully returned to the idle pool. If err is non-nil,</span>
    <span class="comment">// it describes why not. PutIdleConn is not called if</span>
    <span class="comment">// connection reuse is disabled via Transport.DisableKeepAlives.</span>
    <span class="comment">// PutIdleConn is called before the caller&#39;s Response.Body.Close</span>
    <span class="comment">// call returns.</span>
    <span class="comment">// For HTTP/2, this hook is not currently used.</span>
    PutIdleConn func(err <a href="/builtin/#error">error</a>)

<span id="ClientTrace.GotFirstResponseByte"></span>    <span class="comment">// GotFirstResponseByte is called when the first byte of the response</span>
    <span class="comment">// headers is available.</span>
    GotFirstResponseByte func()

<span id="ClientTrace.Got100Continue"></span>    <span class="comment">// Got100Continue is called if the server replies with a &#34;100</span>
    <span class="comment">// Continue&#34; response.</span>
    Got100Continue func()

<span id="ClientTrace.DNSStart"></span>    <span class="comment">// DNSStart is called when a DNS lookup begins.</span>
    DNSStart func(<a href="#DNSStartInfo">DNSStartInfo</a>)

<span id="ClientTrace.DNSDone"></span>    <span class="comment">// DNSDone is called when a DNS lookup ends.</span>
    DNSDone func(<a href="#DNSDoneInfo">DNSDoneInfo</a>)

<span id="ClientTrace.ConnectStart"></span>    <span class="comment">// ConnectStart is called when a new connection&#39;s Dial begins.</span>
    <span class="comment">// If net.Dialer.DualStack (IPv6 &#34;Happy Eyeballs&#34;) support is</span>
    <span class="comment">// enabled, this may be called multiple times.</span>
    ConnectStart func(network, addr <a href="/builtin/#string">string</a>)

<span id="ClientTrace.ConnectDone"></span>    <span class="comment">// ConnectDone is called when a new connection&#39;s Dial</span>
    <span class="comment">// completes. The provided err indicates whether the</span>
    <span class="comment">// connection completedly successfully.</span>
    <span class="comment">// If net.Dialer.DualStack (&#34;Happy Eyeballs&#34;) support is</span>
    <span class="comment">// enabled, this may be called multiple times.</span>
    ConnectDone func(network, addr <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)

<span id="ClientTrace.TLSHandshakeStart"></span>    <span class="comment">// TLSHandshakeStart is called when the TLS handshake is started. When</span>
    <span class="comment">// connecting to a HTTPS site via a HTTP proxy, the handshake happens after</span>
    <span class="comment">// the CONNECT request is processed by the proxy.</span>
    TLSHandshakeStart func()

<span id="ClientTrace.TLSHandshakeDone"></span>    <span class="comment">// TLSHandshakeDone is called after the TLS handshake with either the</span>
    <span class="comment">// successful handshake&#39;s connection state, or a non-nil error on handshake</span>
    <span class="comment">// failure.</span>
    TLSHandshakeDone func(<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#ConnectionState">ConnectionState</a>, <a href="/builtin/#error">error</a>)

<span id="ClientTrace.WroteHeaders"></span>    <span class="comment">// WroteHeaders is called after the Transport has written</span>
    <span class="comment">// the request headers.</span>
    WroteHeaders func()

<span id="ClientTrace.Wait100Continue"></span>    <span class="comment">// Wait100Continue is called if the Request specified</span>
    <span class="comment">// &#34;Expected: 100-continue&#34; and the Transport has written the</span>
    <span class="comment">// request headers but is waiting for &#34;100 Continue&#34; from the</span>
    <span class="comment">// server before writing the request body.</span>
    Wait100Continue func()

<span id="ClientTrace.WroteRequest"></span>    <span class="comment">// WroteRequest is called with the result of writing the</span>
    <span class="comment">// request and any body. It may be called multiple times</span>
    <span class="comment">// in the case of retried requests.</span>
    WroteRequest func(<a href="#WroteRequestInfo">WroteRequestInfo</a>)
}</pre>

ClientTrace is a set of hooks to run at various stages of an outgoing HTTP
request. Any particular hook may be nil. Functions may be called concurrently
from different goroutines and some may be called after the request has completed
or failed.

ClientTrace currently traces a single HTTP request & response during a single
round trip and has no hooks that span a series of redirected requests.

See https://blog.golang.org/http-tracing for more.

<h3 id="ContextClientTrace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptrace/trace.go#L13">ContextClientTrace</a>
    <a href="#ContextClientTrace">¶</a></h3>
<pre>func ContextClientTrace(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>) *<a href="#ClientTrace">ClientTrace</a></pre>

ContextClientTrace returns the ClientTrace associated with the provided context.
If none, it returns nil.

<h2 id="DNSDoneInfo">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptrace/trace.go#L194">DNSDoneInfo</a>
    <a href="#DNSDoneInfo">¶</a></h2>
<pre>type DNSDoneInfo struct {
<span id="DNSDoneInfo.Addrs"></span>    <span class="comment">// Addrs are the IPv4 and/or IPv6 addresses found in the DNS</span>
    <span class="comment">// lookup. The contents of the slice should not be mutated.</span>
    Addrs []<a href="/net/">net</a>.<a href="/net/#IPAddr">IPAddr</a>

<span id="DNSDoneInfo.Err"></span>    <span class="comment">// Err is any error that occurred during the DNS lookup.</span>
    Err <a href="/builtin/#error">error</a>

<span id="DNSDoneInfo.Coalesced"></span>    <span class="comment">// Coalesced is whether the Addrs were shared with another</span>
    <span class="comment">// caller who was doing the same DNS lookup concurrently.</span>
    Coalesced <a href="/builtin/#bool">bool</a>
}</pre>

DNSDoneInfo contains information about the results of a DNS lookup.

<h2 id="DNSStartInfo">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptrace/trace.go#L189">DNSStartInfo</a>
    <a href="#DNSStartInfo">¶</a></h2>
<pre>type DNSStartInfo struct {
<span id="DNSStartInfo.Host"></span>    Host <a href="/builtin/#string">string</a>
}</pre>

DNSStartInfo contains information about a DNS request.

<h2 id="GotConnInfo">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptrace/trace.go#L216">GotConnInfo</a>
    <a href="#GotConnInfo">¶</a></h2>
<pre>type GotConnInfo struct {
<span id="GotConnInfo.Conn"></span>    <span class="comment">// Conn is the connection that was obtained. It is owned by</span>
    <span class="comment">// the http.Transport and should not be read, written or</span>
    <span class="comment">// closed by users of ClientTrace.</span>
    Conn <a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>

<span id="GotConnInfo.Reused"></span>    <span class="comment">// Reused is whether this connection has been previously</span>
    <span class="comment">// used for another HTTP request.</span>
    Reused <a href="/builtin/#bool">bool</a>

<span id="GotConnInfo.WasIdle"></span>    <span class="comment">// WasIdle is whether this connection was obtained from an</span>
    <span class="comment">// idle pool.</span>
    WasIdle <a href="/builtin/#bool">bool</a>

<span id="GotConnInfo.IdleTime"></span>    <span class="comment">// IdleTime reports how long the connection was previously</span>
    <span class="comment">// idle, if WasIdle is true.</span>
    IdleTime <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>
}</pre>

GotConnInfo is the argument to the ClientTrace.GotConn function and contains
information about the obtained connection.

<h2 id="WroteRequestInfo">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptrace/trace.go#L146">WroteRequestInfo</a>
    <a href="#WroteRequestInfo">¶</a></h2>
<pre>type WroteRequestInfo struct {
<span id="WroteRequestInfo.Err"></span>    <span class="comment">// Err is any error encountered while writing the Request.</span>
    Err <a href="/builtin/#error">error</a>
}</pre>

WroteRequestInfo contains information provided to the WroteRequest hook.


