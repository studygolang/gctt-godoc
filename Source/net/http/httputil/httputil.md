version: 1.9.2
## package httputil

  `import "net/http/httputil"`

## Overview

Package httputil provides HTTP utility functions, complementing the more common
ones in the net/http package.

## Index

- [Variables](#pkg-variables)
- [func DumpRequest(req *http.Request, body bool) ([]byte, error)](#DumpRequest)
- [func DumpRequestOut(req *http.Request, body bool) ([]byte, error)](#DumpRequestOut)
- [func DumpResponse(resp *http.Response, body bool) ([]byte, error)](#DumpResponse)
- [func NewChunkedReader(r io.Reader) io.Reader](#NewChunkedReader)
- [func NewChunkedWriter(w io.Writer) io.WriteCloser](#NewChunkedWriter)
- [type BufferPool](#BufferPool)
- [type ClientConn](#ClientConn)
  - [func NewClientConn(c net.Conn, r *bufio.Reader) *ClientConn](#NewClientConn)
  - [func NewProxyClientConn(c net.Conn, r *bufio.Reader) *ClientConn](#NewProxyClientConn)
  - [func (cc *ClientConn) Close() error](#ClientConn.Close)
  - [func (cc *ClientConn) Do(req *http.Request) (*http.Response, error)](#ClientConn.Do)
  - [func (cc *ClientConn) Hijack() (c net.Conn, r *bufio.Reader)](#ClientConn.Hijack)
  - [func (cc *ClientConn) Pending() int](#ClientConn.Pending)
  - [func (cc *ClientConn) Read(req *http.Request) (resp *http.Response, err error)](#ClientConn.Read)
  - [func (cc *ClientConn) Write(req *http.Request) error](#ClientConn.Write)
- [type ReverseProxy](#ReverseProxy)
  - [func NewSingleHostReverseProxy(target *url.URL) *ReverseProxy](#NewSingleHostReverseProxy)
  - [func (p *ReverseProxy) ServeHTTP(rw http.ResponseWriter, req *http.Request)](#ReverseProxy.ServeHTTP)
- [type ServerConn](#ServerConn)
  - [func NewServerConn(c net.Conn, r *bufio.Reader) *ServerConn](#NewServerConn)
  - [func (sc *ServerConn) Close() error](#ServerConn.Close)
  - [func (sc *ServerConn) Hijack() (net.Conn, *bufio.Reader)](#ServerConn.Hijack)
  - [func (sc *ServerConn) Pending() int](#ServerConn.Pending)
  - [func (sc *ServerConn) Read() (*http.Request, error)](#ServerConn.Read)
  - [func (sc *ServerConn) Write(req *http.Request, resp *http.Response) error](#ServerConn.Write)

### Examples

- [DumpRequest](#exampleDumpRequest)
- [DumpRequestOut](#exampleDumpRequestOut)
- [DumpResponse](#exampleDumpResponse)
- [ReverseProxy](#exampleReverseProxy)

### Package files
 [dump.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/dump.go) [httputil.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/httputil.go) [persist.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go) [reverseproxy.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/reverseproxy.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span class="comment">// Deprecated: No longer used.</span>
    <span id="ErrPersistEOF">ErrPersistEOF</span> = &amp;<a href="/net/http/">http</a>.<a href="/net/http/#ProtocolError">ProtocolError</a>{<a href="/net/http/#ProtocolError.ErrorString">ErrorString</a>: &#34;persistent connection closed&#34;}

    <span class="comment">// Deprecated: No longer used.</span>
    <span id="ErrClosed">ErrClosed</span> = &amp;<a href="/net/http/">http</a>.<a href="/net/http/#ProtocolError">ProtocolError</a>{<a href="/net/http/#ProtocolError.ErrorString">ErrorString</a>: &#34;connection closed by user&#34;}

    <span class="comment">// Deprecated: No longer used.</span>
    <span id="ErrPipeline">ErrPipeline</span> = &amp;<a href="/net/http/">http</a>.<a href="/net/http/#ProtocolError">ProtocolError</a>{<a href="/net/http/#ProtocolError.ErrorString">ErrorString</a>: &#34;pipeline error&#34;}
)</pre>


<pre>var <span id="ErrLineTooLong">ErrLineTooLong</span> = <a href="/net/http/internal/">internal</a>.<a href="/net/http/internal/#ErrLineTooLong">ErrLineTooLong</a></pre>

ErrLineTooLong is returned when reading malformed chunked data with lines that
are too long.

<h2 id="DumpRequest">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/dump.go#L181">DumpRequest</a>
    <a href="#DumpRequest">¶</a></h2>
<pre>func DumpRequest(req *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>, body <a href="/builtin/#bool">bool</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

DumpRequest returns the given request in its HTTP/1.x wire representation. It
should only be used by servers to debug client requests. The returned
representation is an approximation only; some details of the initial request are
lost while parsing it into an http.Request. In particular, the order and case of
header field names are lost. The order of values in multi-valued headers is kept
intact. HTTP/2 requests are dumped in HTTP/1.x form, not in their original
binary representations.

If body is true, DumpRequest also returns the body. To do so, it consumes
req.Body and then replaces it with a new io.ReadCloser that yields the same
bytes. If DumpRequest returns an error, the state of req is undefined.

The documentation for http.Request.Write details which fields of req are
included in the dump.

<a id="exampleDumpRequest"></a>
Example:

    ts := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        dump, err := httputil.DumpRequest(r, true)
        if err != nil {
            http.Error(w, fmt.Sprint(err), http.StatusInternalServerError)
            return
        }

        fmt.Fprintf(w, "%q", dump)
    }))
    defer ts.Close()

    const body = "Go is a general-purpose language designed with systems programming in mind."
    req, err := http.NewRequest("POST", ts.URL, strings.NewReader(body))
    if err != nil {
        log.Fatal(err)
    }
    req.Host = "www.example.org"
    resp, err := http.DefaultClient.Do(req)
    if err != nil {
        log.Fatal(err)
    }
    defer resp.Body.Close()

    b, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s", b)

    // Output:
    // "POST / HTTP/1.1\r\nHost: www.example.org\r\nAccept-Encoding: gzip\r\nContent-Length: 75\r\nUser-Agent: Go-http-client/1.1\r\n\r\nGo is a general-purpose language designed with systems programming in mind."

<h2 id="DumpRequestOut">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/dump.go#L56">DumpRequestOut</a>
    <a href="#DumpRequestOut">¶</a></h2>
<pre>func DumpRequestOut(req *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>, body <a href="/builtin/#bool">bool</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

DumpRequestOut is like DumpRequest but for outgoing client requests. It includes
any headers that the standard http.Transport adds, such as User-Agent.

<a id="exampleDumpRequestOut"></a>
Example:

    const body = "Go is a general-purpose language designed with systems programming in mind."
    req, err := http.NewRequest("PUT", "http://www.example.org", strings.NewReader(body))
    if err != nil {
        log.Fatal(err)
    }

    dump, err := httputil.DumpRequestOut(req, true)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%q", dump)

    // Output:
    // "PUT / HTTP/1.1\r\nHost: www.example.org\r\nUser-Agent: Go-http-client/1.1\r\nContent-Length: 75\r\nAccept-Encoding: gzip\r\n\r\nGo is a general-purpose language designed with systems programming in mind."

<h2 id="DumpResponse">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/dump.go#L271">DumpResponse</a>
    <a href="#DumpResponse">¶</a></h2>
<pre>func DumpResponse(resp *<a href="/net/http/">http</a>.<a href="/net/http/#Response">Response</a>, body <a href="/builtin/#bool">bool</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

DumpResponse is like DumpRequest but dumps a response.

<a id="exampleDumpResponse"></a>
Example:

    const body = "Go is a general-purpose language designed with systems programming in mind."
    ts := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Date", "Wed, 19 Jul 1972 19:00:00 GMT")
        fmt.Fprintln(w, body)
    }))
    defer ts.Close()

    resp, err := http.Get(ts.URL)
    if err != nil {
        log.Fatal(err)
    }
    defer resp.Body.Close()

    dump, err := httputil.DumpResponse(resp, true)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%q", dump)

    // Output:
    // "HTTP/1.1 200 OK\r\nContent-Length: 76\r\nContent-Type: text/plain; charset=utf-8\r\nDate: Wed, 19 Jul 1972 19:00:00 GMT\r\n\r\nGo is a general-purpose language designed with systems programming in mind.\n"

<h2 id="NewChunkedReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/httputil.go#L10">NewChunkedReader</a>
    <a href="#NewChunkedReader">¶</a></h2>
<pre>func NewChunkedReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

NewChunkedReader returns a new chunkedReader that translates the data read from
r out of HTTP "chunked" format before returning it. The chunkedReader returns
io.EOF when the final 0-length chunk is read.

NewChunkedReader is not needed by normal applications. The http package
automatically decodes chunking when reading response bodies.

<h2 id="NewChunkedWriter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/httputil.go#L23">NewChunkedWriter</a>
    <a href="#NewChunkedWriter">¶</a></h2>
<pre>func NewChunkedWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/io/">io</a>.<a href="/io/#WriteCloser">WriteCloser</a></pre>

NewChunkedWriter returns a new chunkedWriter that translates writes into HTTP
"chunked" format before writing them to w. Closing the returned chunkedWriter
sends the final 0-length chunk that marks the end of the stream.

NewChunkedWriter is not needed by normal applications. The http package adds
chunking automatically if handlers don't set a Content-Length header. Using
NewChunkedWriter inside a handler would result in double chunking or chunking
with a Content-Length length, both of which are wrong.

<h2 id="BufferPool">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/reverseproxy.go#L56">BufferPool</a>
    <a href="#BufferPool">¶</a></h2>
<pre>type BufferPool interface {
    Get() []<a href="/builtin/#byte">byte</a>
    Put([]<a href="/builtin/#byte">byte</a>)
}</pre>

A BufferPool is an interface for getting and returning temporary byte slices for
use by io.CopyBuffer.

<h2 id="ClientConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L220">ClientConn</a>
    <a href="#ClientConn">¶</a></h2>
<pre>type ClientConn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ClientConn is an artifact of Go's early HTTP implementation. It is low-level,
old, and unused by Go's current HTTP stack. We should have deleted it before Go
1.

Deprecated: Use Client or Transport in package net/http instead.

<h3 id="NewClientConn">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L238">NewClientConn</a>
    <a href="#NewClientConn">¶</a></h3>
<pre>func NewClientConn(c <a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, r *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>) *<a href="#ClientConn">ClientConn</a></pre>

NewClientConn is an artifact of Go's early HTTP implementation. It is low-level,
old, and unused by Go's current HTTP stack. We should have deleted it before Go
1.

Deprecated: Use the Client or Transport in package net/http instead.

<h3 id="NewProxyClientConn">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L255">NewProxyClientConn</a>
    <a href="#NewProxyClientConn">¶</a></h3>
<pre>func NewProxyClientConn(c <a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, r *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>) *<a href="#ClientConn">ClientConn</a></pre>

NewProxyClientConn is an artifact of Go's early HTTP implementation. It is
low-level, old, and unused by Go's current HTTP stack. We should have deleted it
before Go 1.

Deprecated: Use the Client or Transport in package net/http instead.

<h3 id="ClientConn.Close">func (*ClientConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L276">Close</a>
    <a href="#ClientConn.Close">¶</a></h3>
<pre>func (cc *<a href="#ClientConn">ClientConn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close calls Hijack and then also closes the underlying connection.

<h3 id="ClientConn.Do">func (*ClientConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L415">Do</a>
    <a href="#ClientConn.Do">¶</a></h3>
<pre>func (cc *<a href="#ClientConn">ClientConn</a>) Do(req *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>) (*<a href="/net/http/">http</a>.<a href="/net/http/#Response">Response</a>, <a href="/builtin/#error">error</a>)</pre>

Do is convenience method that writes a request and reads a response.

<h3 id="ClientConn.Hijack">func (*ClientConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L265">Hijack</a>
    <a href="#ClientConn.Hijack">¶</a></h3>
<pre>func (cc *<a href="#ClientConn">ClientConn</a>) Hijack() (c <a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, r *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>)</pre>

Hijack detaches the ClientConn and returns the underlying connection as well as
the read-side bufio which may have some left over data. Hijack may be called
before the user or Read have signaled the end of the keep-alive logic. The user
should not call Hijack while Read or Write is in progress.

<h3 id="ClientConn.Pending">func (*ClientConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L343">Pending</a>
    <a href="#ClientConn.Pending">¶</a></h3>
<pre>func (cc *<a href="#ClientConn">ClientConn</a>) Pending() <a href="/builtin/#int">int</a></pre>

Pending returns the number of unanswered requests that have been sent on the
connection.

<h3 id="ClientConn.Read">func (*ClientConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L353">Read</a>
    <a href="#ClientConn.Read">¶</a></h3>
<pre>func (cc *<a href="#ClientConn">ClientConn</a>) Read(req *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>) (resp *<a href="/net/http/">http</a>.<a href="/net/http/#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Read reads the next response from the wire. A valid response might be returned
together with an ErrPersistEOF, which means that the remote requested that this
be the last request serviced. Read can be called concurrently with Write, but
not with another Read.

<h3 id="ClientConn.Write">func (*ClientConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L289">Write</a>
    <a href="#ClientConn.Write">¶</a></h3>
<pre>func (cc *<a href="#ClientConn">ClientConn</a>) Write(req *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>) <a href="/builtin/#error">error</a></pre>

Write writes a request. An ErrPersistEOF error is returned if the connection has
been closed in an HTTP keepalive sense. If req.Close equals true, the keepalive
connection is logically closed after this request and the opposing server is
informed. An ErrUnexpectedEOF indicates the remote closed the underlying TCP
connection, which is usually considered as graceful close.

<h2 id="ReverseProxy">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/reverseproxy.go#L18">ReverseProxy</a>
    <a href="#ReverseProxy">¶</a></h2>
<pre>type ReverseProxy struct {
<span id="ReverseProxy.Director"></span>    <span class="comment">// Director must be a function which modifies</span>
    <span class="comment">// the request into a new request to be sent</span>
    <span class="comment">// using Transport. Its response is then copied</span>
    <span class="comment">// back to the original client unmodified.</span>
    <span class="comment">// Director must not access the provided Request</span>
    <span class="comment">// after returning.</span>
    Director func(*<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>)

    <span class="comment">// The transport used to perform proxy requests.</span>
    <span class="comment">// If nil, http.DefaultTransport is used.</span>
<span id="ReverseProxy.Transport"></span>    Transport <a href="/net/http/">http</a>.<a href="/net/http/#RoundTripper">RoundTripper</a>

<span id="ReverseProxy.FlushInterval"></span>    <span class="comment">// FlushInterval specifies the flush interval</span>
    <span class="comment">// to flush to the client while copying the</span>
    <span class="comment">// response body.</span>
    <span class="comment">// If zero, no periodic flushing is done.</span>
    FlushInterval <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="ReverseProxy.ErrorLog"></span>    <span class="comment">// ErrorLog specifies an optional logger for errors</span>
    <span class="comment">// that occur when attempting to proxy the request.</span>
    <span class="comment">// If nil, logging goes to os.Stderr via the log package&#39;s</span>
    <span class="comment">// standard logger.</span>
    ErrorLog *<a href="/log/">log</a>.<a href="/log/#Logger">Logger</a>

<span id="ReverseProxy.BufferPool"></span>    <span class="comment">// BufferPool optionally specifies a buffer pool to</span>
    <span class="comment">// get byte slices for use by io.CopyBuffer when</span>
    <span class="comment">// copying HTTP response bodies.</span>
    BufferPool <a href="#BufferPool">BufferPool</a>

<span id="ReverseProxy.ModifyResponse"></span>    <span class="comment">// ModifyResponse is an optional function that</span>
    <span class="comment">// modifies the Response from the backend.</span>
    <span class="comment">// If it returns an error, the proxy returns a StatusBadGateway error.</span>
    ModifyResponse func(*<a href="/net/http/">http</a>.<a href="/net/http/#Response">Response</a>) <a href="/builtin/#error">error</a>
}</pre>

ReverseProxy is an HTTP Handler that takes an incoming request and sends it to
another server, proxying the response back to the client.

<a id="exampleReverseProxy"></a>
Example:

    backendServer := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintln(w, "this call was relayed by the reverse proxy")
    }))
    defer backendServer.Close()

    rpURL, err := url.Parse(backendServer.URL)
    if err != nil {
        log.Fatal(err)
    }
    frontendProxy := httptest.NewServer(httputil.NewSingleHostReverseProxy(rpURL))
    defer frontendProxy.Close()

    resp, err := http.Get(frontendProxy.URL)
    if err != nil {
        log.Fatal(err)
    }

    b, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s", b)

    // Output:
    // this call was relayed by the reverse proxy

<h3 id="NewSingleHostReverseProxy">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/reverseproxy.go#L80">NewSingleHostReverseProxy</a>
    <a href="#NewSingleHostReverseProxy">¶</a></h3>
<pre>func NewSingleHostReverseProxy(target *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>) *<a href="#ReverseProxy">ReverseProxy</a></pre>

NewSingleHostReverseProxy returns a new ReverseProxy that routes URLs to the
scheme, host, and base path provided in target. If the target's path is "/base"
and the incoming request was for "/dir", the target request will be for
/base/dir. NewSingleHostReverseProxy does not rewrite the Host header. To
rewrite Host headers, use ReverseProxy directly with a custom Director policy.

<h3 id="ReverseProxy.ServeHTTP">func (*ReverseProxy) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/reverseproxy.go#L131">ServeHTTP</a>
    <a href="#ReverseProxy.ServeHTTP">¶</a></h3>
<pre>func (p *<a href="#ReverseProxy">ReverseProxy</a>) ServeHTTP(rw <a href="/net/http/">http</a>.<a href="/net/http/#ResponseWriter">ResponseWriter</a>, req *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>)</pre>


<h2 id="ServerConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L27">ServerConn</a>
    <a href="#ServerConn">¶</a></h2>
<pre>type ServerConn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ServerConn is an artifact of Go's early HTTP implementation. It is low-level,
old, and unused by Go's current HTTP stack. We should have deleted it before Go
1.

Deprecated: Use the Server in package net/http instead.

<h3 id="NewServerConn">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L44">NewServerConn</a>
    <a href="#NewServerConn">¶</a></h3>
<pre>func NewServerConn(c <a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, r *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>) *<a href="#ServerConn">ServerConn</a></pre>

NewServerConn is an artifact of Go's early HTTP implementation. It is low-level,
old, and unused by Go's current HTTP stack. We should have deleted it before Go
1.

Deprecated: Use the Server in package net/http instead.

<h3 id="ServerConn.Close">func (*ServerConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L66">Close</a>
    <a href="#ServerConn.Close">¶</a></h3>
<pre>func (sc *<a href="#ServerConn">ServerConn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close calls Hijack and then also closes the underlying connection.

<h3 id="ServerConn.Hijack">func (*ServerConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L55">Hijack</a>
    <a href="#ServerConn.Hijack">¶</a></h3>
<pre>func (sc *<a href="#ServerConn">ServerConn</a>) Hijack() (<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>)</pre>

Hijack detaches the ServerConn and returns the underlying connection as well as
the read-side bufio which may have some left over data. Hijack may be called
before Read has signaled the end of the keep-alive logic. The user should not
call Hijack while Read or Write is in progress.

<h3 id="ServerConn.Pending">func (*ServerConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L156">Pending</a>
    <a href="#ServerConn.Pending">¶</a></h3>
<pre>func (sc *<a href="#ServerConn">ServerConn</a>) Pending() <a href="/builtin/#int">int</a></pre>

Pending returns the number of unanswered requests that have been received on the
connection.

<h3 id="ServerConn.Read">func (*ServerConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L78">Read</a>
    <a href="#ServerConn.Read">¶</a></h3>
<pre>func (sc *<a href="#ServerConn">ServerConn</a>) Read() (*<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>, <a href="/builtin/#error">error</a>)</pre>

Read returns the next request on the wire. An ErrPersistEOF is returned if it is
gracefully determined that there are no more requests (e.g. after the first
request on an HTTP/1.0 connection, or after a Connection:close on a HTTP/1.1
connection).

<h3 id="ServerConn.Write">func (*ServerConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httputil/persist.go#L165">Write</a>
    <a href="#ServerConn.Write">¶</a></h3>
<pre>func (sc *<a href="#ServerConn">ServerConn</a>) Write(req *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>, resp *<a href="/net/http/">http</a>.<a href="/net/http/#Response">Response</a>) <a href="/builtin/#error">error</a></pre>

Write writes resp in response to req. To close the connection gracefully, set
the Response.Close field to true. Write should be considered operational until
it returns an error, regardless of any errors returned on the Read side.


