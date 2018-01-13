version: 1.9.2
## package httptest

  `import "net/http/httptest"`

## Overview

Package httptest provides utilities for HTTP testing.

## Index

- [Constants](#pkg-constants)
- [func NewRequest(method, target string, body io.Reader) *http.Request](#NewRequest)
- [type ResponseRecorder](#ResponseRecorder)
  - [func NewRecorder() *ResponseRecorder](#NewRecorder)
  - [func (rw *ResponseRecorder) Flush()](#ResponseRecorder.Flush)
  - [func (rw *ResponseRecorder) Header() http.Header](#ResponseRecorder.Header)
  - [func (rw *ResponseRecorder) Result() *http.Response](#ResponseRecorder.Result)
  - [func (rw *ResponseRecorder) Write(buf []byte) (int, error)](#ResponseRecorder.Write)
  - [func (rw *ResponseRecorder) WriteHeader(code int)](#ResponseRecorder.WriteHeader)
  - [func (rw *ResponseRecorder) WriteString(str string) (int, error)](#ResponseRecorder.WriteString)
- [type Server](#Server)
  - [func NewServer(handler http.Handler) *Server](#NewServer)
  - [func NewTLSServer(handler http.Handler) *Server](#NewTLSServer)
  - [func NewUnstartedServer(handler http.Handler) *Server](#NewUnstartedServer)
  - [func (s *Server) Certificate() *x509.Certificate](#Server.Certificate)
  - [func (s *Server) Client() *http.Client](#Server.Client)
  - [func (s *Server) Close()](#Server.Close)
  - [func (s *Server) CloseClientConnections()](#Server.CloseClientConnections)
  - [func (s *Server) Start()](#Server.Start)
  - [func (s *Server) StartTLS()](#Server.StartTLS)

### Examples

- [NewTLSServer](#exampleNewTLSServer)
- [ResponseRecorder](#exampleResponseRecorder)
- [Server](#exampleServer)

### Package files
 [httptest.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/httptest.go) [recorder.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/recorder.go) [server.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="DefaultRemoteAddr">DefaultRemoteAddr</span> = &#34;1.2.3.4&#34;</pre>

DefaultRemoteAddr is the default remote address to return in RemoteAddr if an
explicit DefaultRemoteAddr isn't set on ResponseRecorder.

<h2 id="NewRequest">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/httptest.go#L31">NewRequest</a>
    <a href="#NewRequest">¶</a></h2>
<pre>func NewRequest(method, target <a href="/builtin/#string">string</a>, body <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a></pre>

NewRequest returns a new incoming server Request, suitable for passing to an
http.Handler for testing.

The target is the RFC 7230 "request-target": it may be either a path or an
absolute URL. If target is an absolute URL, the host name from the URL is used.
Otherwise, "example.com" is used.

The TLS field is set to a non-nil dummy value if target has scheme "https".

The Request.Proto is always HTTP/1.1.

An empty method means "GET".

The provided body may be nil. If the body is of type *bytes.Reader,
*strings.Reader, or *bytes.Buffer, the Request.ContentLength is set.

NewRequest panics on error for ease of use in testing, where a panic is
acceptable.

To generate a client HTTP request instead of a server request, see the
NewRequest function in the net/http package.

<h2 id="ResponseRecorder">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/recorder.go#L8">ResponseRecorder</a>
    <a href="#ResponseRecorder">¶</a></h2>
<pre>type ResponseRecorder struct {
<span id="ResponseRecorder.Code"></span>    <span class="comment">// Code is the HTTP response code set by WriteHeader.</span>
    <span class="comment">//</span>
    <span class="comment">// Note that if a Handler never calls WriteHeader or Write,</span>
    <span class="comment">// this might end up being 0, rather than the implicit</span>
    <span class="comment">// http.StatusOK. To get the implicit value, use the Result</span>
    <span class="comment">// method.</span>
    Code <a href="/builtin/#int">int</a>

<span id="ResponseRecorder.HeaderMap"></span>    <span class="comment">// HeaderMap contains the headers explicitly set by the Handler.</span>
    <span class="comment">//</span>
    <span class="comment">// To get the implicit headers set by the server (such as</span>
    <span class="comment">// automatic Content-Type), use the Result method.</span>
    HeaderMap <a href="/net/http/">http</a>.<a href="/net/http/#Header">Header</a>

<span id="ResponseRecorder.Body"></span>    <span class="comment">// Body is the buffer to which the Handler&#39;s Write calls are sent.</span>
    <span class="comment">// If nil, the Writes are silently discarded.</span>
    Body *<a href="/bytes/">bytes</a>.<a href="/bytes/#Buffer">Buffer</a>

<span id="ResponseRecorder.Flushed"></span>    <span class="comment">// Flushed is whether the Handler called Flush.</span>
    Flushed <a href="/builtin/#bool">bool</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ResponseRecorder is an implementation of http.ResponseWriter that records its
mutations for later inspection in tests.

<a id="exampleResponseRecorder"></a>
Example:

    handler := func(w http.ResponseWriter, r *http.Request) {
        io.WriteString(w, "<html><body>Hello World!</body></html>")
    }

    req := httptest.NewRequest("GET", "http://example.com/foo", nil)
    w := httptest.NewRecorder()
    handler(w, req)

    resp := w.Result()
    body, _ := ioutil.ReadAll(resp.Body)

    fmt.Println(resp.StatusCode)
    fmt.Println(resp.Header.Get("Content-Type"))
    fmt.Println(string(body))

    // Output:
    // 200
    // text/html; charset=utf-8
    // <html><body>Hello World!</body></html>

<h3 id="NewRecorder">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/recorder.go#L36">NewRecorder</a>
    <a href="#NewRecorder">¶</a></h3>
<pre>func NewRecorder() *<a href="#ResponseRecorder">ResponseRecorder</a></pre>

NewRecorder returns an initialized ResponseRecorder.

<h3 id="ResponseRecorder.Flush">func (*ResponseRecorder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/recorder.go#L130">Flush</a>
    <a href="#ResponseRecorder.Flush">¶</a></h3>
<pre>func (rw *<a href="#ResponseRecorder">ResponseRecorder</a>) Flush()</pre>

Flush sets rw.Flushed to true.

<h3 id="ResponseRecorder.Header">func (*ResponseRecorder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/recorder.go#L49">Header</a>
    <a href="#ResponseRecorder.Header">¶</a></h3>
<pre>func (rw *<a href="#ResponseRecorder">ResponseRecorder</a>) Header() <a href="/net/http/">http</a>.<a href="/net/http/#Header">Header</a></pre>

Header returns the response headers.

<h3 id="ResponseRecorder.Result">func (*ResponseRecorder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/recorder.go#L152">Result</a>
    <a href="#ResponseRecorder.Result">¶</a></h3>
<pre>func (rw *<a href="#ResponseRecorder">ResponseRecorder</a>) Result() *<a href="/net/http/">http</a>.<a href="/net/http/#Response">Response</a></pre>

Result returns the response generated by the handler.

The returned Response will have at least its StatusCode, Header, Body, and
optionally Trailer populated. More fields may be populated in the future, so
callers should not DeepEqual the result in tests.

The Response.Header is a snapshot of the headers at the time of the first write
call, or at the time of this call, if the handler never did a write.

The Response.Body is guaranteed to be non-nil and Body.Read call is guaranteed
to not return any error other than io.EOF.

Result must only be called after the handler has finished running.

<h3 id="ResponseRecorder.Write">func (*ResponseRecorder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/recorder.go#L88">Write</a>
    <a href="#ResponseRecorder.Write">¶</a></h3>
<pre>func (rw *<a href="#ResponseRecorder">ResponseRecorder</a>) Write(buf []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write always succeeds and writes to rw.Body, if not nil.

<h3 id="ResponseRecorder.WriteHeader">func (*ResponseRecorder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/recorder.go#L107">WriteHeader</a>
    <a href="#ResponseRecorder.WriteHeader">¶</a></h3>
<pre>func (rw *<a href="#ResponseRecorder">ResponseRecorder</a>) WriteHeader(code <a href="/builtin/#int">int</a>)</pre>

WriteHeader sets rw.Code. After it is called, changing rw.Header will not affect
rw.HeaderMap.

<h3 id="ResponseRecorder.WriteString">func (*ResponseRecorder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/recorder.go#L97">WriteString</a>
    <a href="#ResponseRecorder.WriteString">¶</a></h3>
<pre>func (rw *<a href="#ResponseRecorder">ResponseRecorder</a>) WriteString(str <a href="/builtin/#string">string</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteString always succeeds and writes to rw.Body, if not nil.

<h2 id="Server">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go#L16">Server</a>
    <a href="#Server">¶</a></h2>
<pre>type Server struct {
<span id="Server.URL"></span>    URL      <a href="/builtin/#string">string</a> <span class="comment">// base URL of form http://ipaddr:port with no trailing slash</span>
<span id="Server.Listener"></span>    Listener <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>

<span id="Server.TLS"></span>    <span class="comment">// TLS is the optional TLS configuration, populated with a new config</span>
    <span class="comment">// after TLS is started. If set on an unstarted server before StartTLS</span>
    <span class="comment">// is called, existing fields are copied into the new config.</span>
    TLS *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#Config">Config</a>

<span id="Server.Config"></span>    <span class="comment">// Config may be changed after calling NewUnstartedServer and</span>
    <span class="comment">// before Start or StartTLS.</span>
    Config *<a href="/net/http/">http</a>.<a href="/net/http/#Server">Server</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Server is an HTTP server listening on a system-chosen port on the local
loopback interface, for use in end-to-end HTTP tests.

<a id="exampleServer"></a>
Example:

    ts := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintln(w, "Hello, client")
    }))
    defer ts.Close()

    res, err := http.Get(ts.URL)
    if err != nil {
        log.Fatal(err)
    }
    greeting, err := ioutil.ReadAll(res.Body)
    res.Body.Close()
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s", greeting)
    // Output: Hello, client

<h3 id="NewServer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go#L70">NewServer</a>
    <a href="#NewServer">¶</a></h3>
<pre>func NewServer(handler <a href="/net/http/">http</a>.<a href="/net/http/#Handler">Handler</a>) *<a href="#Server">Server</a></pre>

NewServer starts and returns a new Server. The caller should call Close when
finished, to shut it down.

<h3 id="NewTLSServer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go#L150">NewTLSServer</a>
    <a href="#NewTLSServer">¶</a></h3>
<pre>func NewTLSServer(handler <a href="/net/http/">http</a>.<a href="/net/http/#Handler">Handler</a>) *<a href="#Server">Server</a></pre>

NewTLSServer starts and returns a new Server using TLS. The caller should call
Close when finished, to shut it down.

<a id="exampleNewTLSServer"></a>
Example:

    ts := httptest.NewTLSServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintln(w, "Hello, client")
    }))
    defer ts.Close()

    client := ts.Client()
    res, err := client.Get(ts.URL)
    if err != nil {
        log.Fatal(err)
    }

    greeting, err := ioutil.ReadAll(res.Body)
    res.Body.Close()
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s", greeting)
    // Output: Hello, client

<h3 id="NewUnstartedServer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go#L82">NewUnstartedServer</a>
    <a href="#NewUnstartedServer">¶</a></h3>
<pre>func NewUnstartedServer(handler <a href="/net/http/">http</a>.<a href="/net/http/#Handler">Handler</a>) *<a href="#Server">Server</a></pre>

NewUnstartedServer returns a new Server but doesn't start it.

After changing its configuration, the caller should call Start or StartTLS.

The caller should call Close when finished, to shut it down.

<h3 id="Server.Certificate">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go#L255">Certificate</a>
    <a href="#Server.Certificate">¶</a></h3>
<pre>func (s *<a href="#Server">Server</a>) Certificate() *<a href="/crypto/x509/">x509</a>.<a href="/crypto/x509/#Certificate">Certificate</a></pre>

Certificate returns the certificate used by the server, or nil if the server
doesn't use TLS.

<h3 id="Server.Client">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go#L262">Client</a>
    <a href="#Server.Client">¶</a></h3>
<pre>func (s *<a href="#Server">Server</a>) Client() *<a href="/net/http/">http</a>.<a href="/net/http/#Client">Client</a></pre>

Client returns an HTTP client configured for making requests to the server. It
is configured to trust the server's TLS test certificate and will close its idle
connections on Server.Close.

<h3 id="Server.Close">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go#L162">Close</a>
    <a href="#Server.Close">¶</a></h3>
<pre>func (s *<a href="#Server">Server</a>) Close()</pre>

Close shuts down the server and blocks until all outstanding requests on this
server have completed.

<h3 id="Server.CloseClientConnections">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go#L226">CloseClientConnections</a>
    <a href="#Server.CloseClientConnections">¶</a></h3>
<pre>func (s *<a href="#Server">Server</a>) CloseClientConnections()</pre>

CloseClientConnections closes any open HTTP connections to the test Server.

<h3 id="Server.Start">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go#L90">Start</a>
    <a href="#Server.Start">¶</a></h3>
<pre>func (s *<a href="#Server">Server</a>) Start()</pre>

Start starts a server from NewUnstartedServer.

<h3 id="Server.StartTLS">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/httptest/server.go#L107">StartTLS</a>
    <a href="#Server.StartTLS">¶</a></h3>
<pre>func (s *<a href="#Server">Server</a>) StartTLS()</pre>

StartTLS starts TLS on a server from NewUnstartedServer.


