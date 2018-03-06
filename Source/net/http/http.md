version: 1.10
## package http

  `import "net/http"`

## Overview

Package http provides HTTP client and server implementations.

Get, Head, Post, and PostForm make HTTP (or HTTPS) requests:

    resp, err := http.Get("http://example.com/")
    ...
    resp, err := http.Post("http://example.com/upload", "image/jpeg", &buf)
    ...
    resp, err := http.PostForm("http://example.com/form",
    	url.Values{"key": {"Value"}, "id": {"123"}})

The client must close the response body when finished with it:

    resp, err := http.Get("http://example.com/")
    if err != nil {
    	// handle error
    }
    defer resp.Body.Close()
    body, err := ioutil.ReadAll(resp.Body)
    // ...

For control over HTTP client headers, redirect policy, and other settings,
create a Client:

    client := &http.Client{
    	CheckRedirect: redirectPolicyFunc,
    }

    resp, err := client.Get("http://example.com")
    // ...

    req, err := http.NewRequest("GET", "http://example.com", nil)
    // ...
    req.Header.Add("If-None-Match", `W/"wyzzy"`)
    resp, err := client.Do(req)
    // ...

For control over proxies, TLS configuration, keep-alives, compression, and other
settings, create a Transport:

    tr := &http.Transport{
    	MaxIdleConns:       10,
    	IdleConnTimeout:    30 * time.Second,
    	DisableCompression: true,
    }
    client := &http.Client{Transport: tr}
    resp, err := client.Get("https://example.com")

Clients and Transports are safe for concurrent use by multiple goroutines and
for efficiency should only be created once and re-used.

ListenAndServe starts an HTTP server with a given address and handler. The
handler is usually nil, which means to use DefaultServeMux. Handle and
HandleFunc add handlers to DefaultServeMux:

    http.Handle("/foo", fooHandler)

    http.HandleFunc("/bar", func(w http.ResponseWriter, r *http.Request) {
    	fmt.Fprintf(w, "Hello, %q", html.EscapeString(r.URL.Path))
    })

    log.Fatal(http.ListenAndServe(":8080", nil))

More control over the server's behavior is available by creating a custom
Server:

    s := &http.Server{
    	Addr:           ":8080",
    	Handler:        myHandler,
    	ReadTimeout:    10 * time.Second,
    	WriteTimeout:   10 * time.Second,
    	MaxHeaderBytes: 1 << 20,
    }
    log.Fatal(s.ListenAndServe())

Starting with Go 1.6, the http package has transparent support for the HTTP/2
protocol when using HTTPS. Programs that must disable HTTP/2 can do so by
setting Transport.TLSNextProto (for clients) or Server.TLSNextProto (for
servers) to a non-nil, empty map. Alternatively, the following GODEBUG
environment variables are currently supported:

    GODEBUG=http2client=0  # disable HTTP/2 client support
    GODEBUG=http2server=0  # disable HTTP/2 server support
    GODEBUG=http2debug=1   # enable verbose HTTP/2 debug logs
    GODEBUG=http2debug=2   # ... even more verbose, with frame dumps

The GODEBUG variables are not covered by Go's API compatibility promise. Please
report any issues before disabling HTTP/2 support: https://golang.org/s/http2bug

The http package's Transport and Server both automatically enable HTTP/2 support
for simple configurations. To enable HTTP/2 for more complex configurations, to
use lower-level HTTP/2 features, or to use a newer version of Go's http2
package, import "golang.org/x/net/http2" directly and use its ConfigureTransport
and/or ConfigureServer functions. Manually configuring HTTP/2 via the
golang.org/x/net/http2 package takes precedence over the net/http package's
built-in HTTP/2 support.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func CanonicalHeaderKey(s string) string](#CanonicalHeaderKey)
- [func DetectContentType(data []byte) string](#DetectContentType)
- [func Error(w ResponseWriter, error string, code int)](#Error)
- [func Handle(pattern string, handler Handler)](#Handle)
- [func HandleFunc(pattern string, handler func(ResponseWriter, *Request))](#HandleFunc)
- [func ListenAndServe(addr string, handler Handler) error](#ListenAndServe)
- [func ListenAndServeTLS(addr, certFile, keyFile string, handler Handler) error](#ListenAndServeTLS)
- [func MaxBytesReader(w ResponseWriter, r io.ReadCloser, n int64) io.ReadCloser](#MaxBytesReader)
- [func NotFound(w ResponseWriter, r *Request)](#NotFound)
- [func ParseHTTPVersion(vers string) (major, minor int, ok bool)](#ParseHTTPVersion)
- [func ParseTime(text string) (t time.Time, err error)](#ParseTime)
- [func ProxyFromEnvironment(req *Request) (*url.URL, error)](#ProxyFromEnvironment)
- [func ProxyURL(fixedURL *url.URL) func(*Request) (*url.URL, error)](#ProxyURL)
- [func Redirect(w ResponseWriter, r *Request, url string, code int)](#Redirect)
- [func Serve(l net.Listener, handler Handler) error](#Serve)
- [func ServeContent(w ResponseWriter, req *Request, name string, modtime time.Time, content io.ReadSeeker)](#ServeContent)
- [func ServeFile(w ResponseWriter, r *Request, name string)](#ServeFile)
- [func ServeTLS(l net.Listener, handler Handler, certFile, keyFile string) error](#ServeTLS)
- [func SetCookie(w ResponseWriter, cookie *Cookie)](#SetCookie)
- [func StatusText(code int) string](#StatusText)
- [type Client](#Client)
  - [func (c *Client) Do(req *Request) (*Response, error)](#Client.Do)
  - [func (c *Client) Get(url string) (resp *Response, err error)](#Client.Get)
  - [func (c *Client) Head(url string) (resp *Response, err error)](#Client.Head)
  - [func (c *Client) Post(url string, contentType string, body io.Reader) (resp *Response, err error)](#Client.Post)
  - [func (c *Client) PostForm(url string, data url.Values) (resp *Response, err error)](#Client.PostForm)
- [type CloseNotifier](#CloseNotifier)
- [type ConnState](#ConnState)
  - [func (c ConnState) String() string](#ConnState.String)
- [type Cookie](#Cookie)
  - [func (c *Cookie) String() string](#Cookie.String)
- [type CookieJar](#CookieJar)
- [type Dir](#Dir)
  - [func (d Dir) Open(name string) (File, error)](#Dir.Open)
- [type File](#File)
- [type FileSystem](#FileSystem)
- [type Flusher](#Flusher)
- [type Handler](#Handler)
  - [func FileServer(root FileSystem) Handler](#FileServer)
  - [func NotFoundHandler() Handler](#NotFoundHandler)
  - [func RedirectHandler(url string, code int) Handler](#RedirectHandler)
  - [func StripPrefix(prefix string, h Handler) Handler](#StripPrefix)
  - [func TimeoutHandler(h Handler, dt time.Duration, msg string) Handler](#TimeoutHandler)
- [type HandlerFunc](#HandlerFunc)
  - [func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request)](#HandlerFunc.ServeHTTP)
- [type Header](#Header)
  - [func (h Header) Add(key, value string)](#Header.Add)
  - [func (h Header) Del(key string)](#Header.Del)
  - [func (h Header) Get(key string) string](#Header.Get)
  - [func (h Header) Set(key, value string)](#Header.Set)
  - [func (h Header) Write(w io.Writer) error](#Header.Write)
  - [func (h Header) WriteSubset(w io.Writer, exclude map[string]bool) error](#Header.WriteSubset)
- [type Hijacker](#Hijacker)
- [type ProtocolError](#ProtocolError)
  - [func (pe *ProtocolError) Error() string](#ProtocolError.Error)
- [type PushOptions](#PushOptions)
- [type Pusher](#Pusher)
- [type Request](#Request)
  - [func NewRequest(method, url string, body io.Reader) (*Request, error)](#NewRequest)
  - [func ReadRequest(b *bufio.Reader) (*Request, error)](#ReadRequest)
  - [func (r *Request) AddCookie(c *Cookie)](#Request.AddCookie)
  - [func (r *Request) BasicAuth() (username, password string, ok bool)](#Request.BasicAuth)
  - [func (r *Request) Context() context.Context](#Request.Context)
  - [func (r *Request) Cookie(name string) (*Cookie, error)](#Request.Cookie)
  - [func (r *Request) Cookies() []*Cookie](#Request.Cookies)
  - [func (r *Request) FormFile(key string) (multipart.File, *multipart.FileHeader, error)](#Request.FormFile)
  - [func (r *Request) FormValue(key string) string](#Request.FormValue)
  - [func (r *Request) MultipartReader() (*multipart.Reader, error)](#Request.MultipartReader)
  - [func (r *Request) ParseForm() error](#Request.ParseForm)
  - [func (r *Request) ParseMultipartForm(maxMemory int64) error](#Request.ParseMultipartForm)
  - [func (r *Request) PostFormValue(key string) string](#Request.PostFormValue)
  - [func (r *Request) ProtoAtLeast(major, minor int) bool](#Request.ProtoAtLeast)
  - [func (r *Request) Referer() string](#Request.Referer)
  - [func (r *Request) SetBasicAuth(username, password string)](#Request.SetBasicAuth)
  - [func (r *Request) UserAgent() string](#Request.UserAgent)
  - [func (r *Request) WithContext(ctx context.Context) *Request](#Request.WithContext)
  - [func (r *Request) Write(w io.Writer) error](#Request.Write)
  - [func (r *Request) WriteProxy(w io.Writer) error](#Request.WriteProxy)
- [type Response](#Response)
  - [func Get(url string) (resp *Response, err error)](#Get)
  - [func Head(url string) (resp *Response, err error)](#Head)
  - [func Post(url string, contentType string, body io.Reader) (resp *Response, err error)](#Post)
  - [func PostForm(url string, data url.Values) (resp *Response, err error)](#PostForm)
  - [func ReadResponse(r *bufio.Reader, req *Request) (*Response, error)](#ReadResponse)
  - [func (r *Response) Cookies() []*Cookie](#Response.Cookies)
  - [func (r *Response) Location() (*url.URL, error)](#Response.Location)
  - [func (r *Response) ProtoAtLeast(major, minor int) bool](#Response.ProtoAtLeast)
  - [func (r *Response) Write(w io.Writer) error](#Response.Write)
- [type ResponseWriter](#ResponseWriter)
- [type RoundTripper](#RoundTripper)
  - [func NewFileTransport(fs FileSystem) RoundTripper](#NewFileTransport)
- [type ServeMux](#ServeMux)
  - [func NewServeMux() *ServeMux](#NewServeMux)
  - [func (mux *ServeMux) Handle(pattern string, handler Handler)](#ServeMux.Handle)
  - [func (mux *ServeMux) HandleFunc(pattern string, handler func(ResponseWriter, *Request))](#ServeMux.HandleFunc)
  - [func (mux *ServeMux) Handler(r *Request) (h Handler, pattern string)](#ServeMux.Handler)
  - [func (mux *ServeMux) ServeHTTP(w ResponseWriter, r *Request)](#ServeMux.ServeHTTP)
- [type Server](#Server)
  - [func (srv *Server) Close() error](#Server.Close)
  - [func (srv *Server) ListenAndServe() error](#Server.ListenAndServe)
  - [func (srv *Server) ListenAndServeTLS(certFile, keyFile string) error](#Server.ListenAndServeTLS)
  - [func (srv *Server) RegisterOnShutdown(f func())](#Server.RegisterOnShutdown)
  - [func (srv *Server) Serve(l net.Listener) error](#Server.Serve)
  - [func (srv *Server) ServeTLS(l net.Listener, certFile, keyFile string) error](#Server.ServeTLS)
  - [func (srv *Server) SetKeepAlivesEnabled(v bool)](#Server.SetKeepAlivesEnabled)
  - [func (srv *Server) Shutdown(ctx context.Context) error](#Server.Shutdown)
- [type Transport](#Transport)
  - [func (t *Transport) CancelRequest(req *Request)](#Transport.CancelRequest)
  - [func (t *Transport) CloseIdleConnections()](#Transport.CloseIdleConnections)
  - [func (t *Transport) RegisterProtocol(scheme string, rt RoundTripper)](#Transport.RegisterProtocol)
  - [func (t *Transport) RoundTrip(req *Request) (*Response, error)](#Transport.RoundTrip)

### Examples

- [FileServer](#exampleFileServer)
- [FileServer (StripPrefix)](#exampleFileServer_stripPrefix)
- [Get](#exampleGet)
- [Hijacker](#exampleHijacker)
- [ResponseWriter (Trailers)](#exampleResponseWriter_trailers)
- [ServeMux.Handle](#exampleServeMux_Handle)
- [Server.Shutdown](#exampleServer_Shutdown)
- [StripPrefix](#exampleStripPrefix)

### Package files
 [client.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go) [cookie.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookie.go) [doc.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/doc.go) [filetransport.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/filetransport.go) [fs.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fs.go) [h2_bundle.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/h2_bundle.go) [header.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/header.go) [http.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/http.go) [jar.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/jar.go) [method.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/method.go) [request.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go) [response.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/response.go) [server.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go) [sniff.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/sniff.go) [status.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/status.go) [transfer.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/transfer.go) [transport.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/transport.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="MethodGet">MethodGet</span>     = &#34;GET&#34;
    <span id="MethodHead">MethodHead</span>    = &#34;HEAD&#34;
    <span id="MethodPost">MethodPost</span>    = &#34;POST&#34;
    <span id="MethodPut">MethodPut</span>     = &#34;PUT&#34;
    <span id="MethodPatch">MethodPatch</span>   = &#34;PATCH&#34; <span class="comment">// RFC 5789</span>
    <span id="MethodDelete">MethodDelete</span>  = &#34;DELETE&#34;
    <span id="MethodConnect">MethodConnect</span> = &#34;CONNECT&#34;
    <span id="MethodOptions">MethodOptions</span> = &#34;OPTIONS&#34;
    <span id="MethodTrace">MethodTrace</span>   = &#34;TRACE&#34;
)</pre>

Common HTTP methods.

Unless otherwise noted, these are defined in RFC 7231 section 4.3.

<pre>const (
    <span id="StatusContinue">StatusContinue</span>           = 100 <span class="comment">// RFC 7231, 6.2.1</span>
    <span id="StatusSwitchingProtocols">StatusSwitchingProtocols</span> = 101 <span class="comment">// RFC 7231, 6.2.2</span>
    <span id="StatusProcessing">StatusProcessing</span>         = 102 <span class="comment">// RFC 2518, 10.1</span>

    <span id="StatusOK">StatusOK</span>                   = 200 <span class="comment">// RFC 7231, 6.3.1</span>
    <span id="StatusCreated">StatusCreated</span>              = 201 <span class="comment">// RFC 7231, 6.3.2</span>
    <span id="StatusAccepted">StatusAccepted</span>             = 202 <span class="comment">// RFC 7231, 6.3.3</span>
    <span id="StatusNonAuthoritativeInfo">StatusNonAuthoritativeInfo</span> = 203 <span class="comment">// RFC 7231, 6.3.4</span>
    <span id="StatusNoContent">StatusNoContent</span>            = 204 <span class="comment">// RFC 7231, 6.3.5</span>
    <span id="StatusResetContent">StatusResetContent</span>         = 205 <span class="comment">// RFC 7231, 6.3.6</span>
    <span id="StatusPartialContent">StatusPartialContent</span>       = 206 <span class="comment">// RFC 7233, 4.1</span>
    <span id="StatusMultiStatus">StatusMultiStatus</span>          = 207 <span class="comment">// RFC 4918, 11.1</span>
    <span id="StatusAlreadyReported">StatusAlreadyReported</span>      = 208 <span class="comment">// RFC 5842, 7.1</span>
    <span id="StatusIMUsed">StatusIMUsed</span>               = 226 <span class="comment">// RFC 3229, 10.4.1</span>

    <span id="StatusMultipleChoices">StatusMultipleChoices</span>  = 300 <span class="comment">// RFC 7231, 6.4.1</span>
    <span id="StatusMovedPermanently">StatusMovedPermanently</span> = 301 <span class="comment">// RFC 7231, 6.4.2</span>
    <span id="StatusFound">StatusFound</span>            = 302 <span class="comment">// RFC 7231, 6.4.3</span>
    <span id="StatusSeeOther">StatusSeeOther</span>         = 303 <span class="comment">// RFC 7231, 6.4.4</span>
    <span id="StatusNotModified">StatusNotModified</span>      = 304 <span class="comment">// RFC 7232, 4.1</span>
    <span id="StatusUseProxy">StatusUseProxy</span>         = 305 <span class="comment">// RFC 7231, 6.4.5</span>

    <span id="StatusTemporaryRedirect">StatusTemporaryRedirect</span> = 307 <span class="comment">// RFC 7231, 6.4.7</span>
    <span id="StatusPermanentRedirect">StatusPermanentRedirect</span> = 308 <span class="comment">// RFC 7538, 3</span>

    <span id="StatusBadRequest">StatusBadRequest</span>                   = 400 <span class="comment">// RFC 7231, 6.5.1</span>
    <span id="StatusUnauthorized">StatusUnauthorized</span>                 = 401 <span class="comment">// RFC 7235, 3.1</span>
    <span id="StatusPaymentRequired">StatusPaymentRequired</span>              = 402 <span class="comment">// RFC 7231, 6.5.2</span>
    <span id="StatusForbidden">StatusForbidden</span>                    = 403 <span class="comment">// RFC 7231, 6.5.3</span>
    <span id="StatusNotFound">StatusNotFound</span>                     = 404 <span class="comment">// RFC 7231, 6.5.4</span>
    <span id="StatusMethodNotAllowed">StatusMethodNotAllowed</span>             = 405 <span class="comment">// RFC 7231, 6.5.5</span>
    <span id="StatusNotAcceptable">StatusNotAcceptable</span>                = 406 <span class="comment">// RFC 7231, 6.5.6</span>
    <span id="StatusProxyAuthRequired">StatusProxyAuthRequired</span>            = 407 <span class="comment">// RFC 7235, 3.2</span>
    <span id="StatusRequestTimeout">StatusRequestTimeout</span>               = 408 <span class="comment">// RFC 7231, 6.5.7</span>
    <span id="StatusConflict">StatusConflict</span>                     = 409 <span class="comment">// RFC 7231, 6.5.8</span>
    <span id="StatusGone">StatusGone</span>                         = 410 <span class="comment">// RFC 7231, 6.5.9</span>
    <span id="StatusLengthRequired">StatusLengthRequired</span>               = 411 <span class="comment">// RFC 7231, 6.5.10</span>
    <span id="StatusPreconditionFailed">StatusPreconditionFailed</span>           = 412 <span class="comment">// RFC 7232, 4.2</span>
    <span id="StatusRequestEntityTooLarge">StatusRequestEntityTooLarge</span>        = 413 <span class="comment">// RFC 7231, 6.5.11</span>
    <span id="StatusRequestURITooLong">StatusRequestURITooLong</span>            = 414 <span class="comment">// RFC 7231, 6.5.12</span>
    <span id="StatusUnsupportedMediaType">StatusUnsupportedMediaType</span>         = 415 <span class="comment">// RFC 7231, 6.5.13</span>
    <span id="StatusRequestedRangeNotSatisfiable">StatusRequestedRangeNotSatisfiable</span> = 416 <span class="comment">// RFC 7233, 4.4</span>
    <span id="StatusExpectationFailed">StatusExpectationFailed</span>            = 417 <span class="comment">// RFC 7231, 6.5.14</span>
    <span id="StatusTeapot">StatusTeapot</span>                       = 418 <span class="comment">// RFC 7168, 2.3.3</span>
    <span id="StatusUnprocessableEntity">StatusUnprocessableEntity</span>          = 422 <span class="comment">// RFC 4918, 11.2</span>
    <span id="StatusLocked">StatusLocked</span>                       = 423 <span class="comment">// RFC 4918, 11.3</span>
    <span id="StatusFailedDependency">StatusFailedDependency</span>             = 424 <span class="comment">// RFC 4918, 11.4</span>
    <span id="StatusUpgradeRequired">StatusUpgradeRequired</span>              = 426 <span class="comment">// RFC 7231, 6.5.15</span>
    <span id="StatusPreconditionRequired">StatusPreconditionRequired</span>         = 428 <span class="comment">// RFC 6585, 3</span>
    <span id="StatusTooManyRequests">StatusTooManyRequests</span>              = 429 <span class="comment">// RFC 6585, 4</span>
    <span id="StatusRequestHeaderFieldsTooLarge">StatusRequestHeaderFieldsTooLarge</span>  = 431 <span class="comment">// RFC 6585, 5</span>
    <span id="StatusUnavailableForLegalReasons">StatusUnavailableForLegalReasons</span>   = 451 <span class="comment">// RFC 7725, 3</span>

    <span id="StatusInternalServerError">StatusInternalServerError</span>           = 500 <span class="comment">// RFC 7231, 6.6.1</span>
    <span id="StatusNotImplemented">StatusNotImplemented</span>                = 501 <span class="comment">// RFC 7231, 6.6.2</span>
    <span id="StatusBadGateway">StatusBadGateway</span>                    = 502 <span class="comment">// RFC 7231, 6.6.3</span>
    <span id="StatusServiceUnavailable">StatusServiceUnavailable</span>            = 503 <span class="comment">// RFC 7231, 6.6.4</span>
    <span id="StatusGatewayTimeout">StatusGatewayTimeout</span>                = 504 <span class="comment">// RFC 7231, 6.6.5</span>
    <span id="StatusHTTPVersionNotSupported">StatusHTTPVersionNotSupported</span>       = 505 <span class="comment">// RFC 7231, 6.6.6</span>
    <span id="StatusVariantAlsoNegotiates">StatusVariantAlsoNegotiates</span>         = 506 <span class="comment">// RFC 2295, 8.1</span>
    <span id="StatusInsufficientStorage">StatusInsufficientStorage</span>           = 507 <span class="comment">// RFC 4918, 11.5</span>
    <span id="StatusLoopDetected">StatusLoopDetected</span>                  = 508 <span class="comment">// RFC 5842, 7.2</span>
    <span id="StatusNotExtended">StatusNotExtended</span>                   = 510 <span class="comment">// RFC 2774, 7</span>
    <span id="StatusNetworkAuthenticationRequired">StatusNetworkAuthenticationRequired</span> = 511 <span class="comment">// RFC 6585, 6</span>
)</pre>

HTTP status codes as registered with IANA. See:
http://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml

<pre>const <span id="DefaultMaxHeaderBytes">DefaultMaxHeaderBytes</span> = 1 &lt;&lt; 20 <span class="comment">// 1 MB</span>
</pre>

DefaultMaxHeaderBytes is the maximum permitted size of the headers in an HTTP
request. This can be overridden by setting Server.MaxHeaderBytes.

<pre>const <span id="DefaultMaxIdleConnsPerHost">DefaultMaxIdleConnsPerHost</span> = 2</pre>

DefaultMaxIdleConnsPerHost is the default value of Transport's
MaxIdleConnsPerHost.

<pre>const <span id="TimeFormat">TimeFormat</span> = &#34;Mon, 02 Jan 2006 15:04:05 GMT&#34;</pre>

TimeFormat is the time format to use when generating times in HTTP headers. It
is like time.RFC1123 but hard-codes GMT as the time zone. The time being
formatted must be in UTC for Format to generate the correct format.

For parsing this time format, see ParseTime.

<pre>const <span id="TrailerPrefix">TrailerPrefix</span> = &#34;Trailer:&#34;</pre>

TrailerPrefix is a magic prefix for ResponseWriter.Header map keys that, if
present, signals that the map entry is actually for the response trailers, and
not the response headers. The prefix is stripped after the ServeHTTP call
finishes and the values are sent in the trailers.

This mechanism is intended only for trailers that are not known prior to the
headers being written. If the set of trailers is fixed or known before the
header is written, the normal Go trailers mechanism is preferred:

    https://golang.org/pkg/net/http/#ResponseWriter
    https://golang.org/pkg/net/http/#example_ResponseWriter_trailers

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span class="comment">// ErrNotSupported is returned by the Push method of Pusher</span>
    <span class="comment">// implementations to indicate that HTTP/2 Push support is not</span>
    <span class="comment">// available.</span>
    <span id="ErrNotSupported">ErrNotSupported</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;feature not supported&#34;}

    <span class="comment">// ErrUnexpectedTrailer is returned by the Transport when a server</span>
    <span class="comment">// replies with a Trailer header, but without a chunked reply.</span>
    <span id="ErrUnexpectedTrailer">ErrUnexpectedTrailer</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;trailer header without chunked transfer encoding&#34;}

    <span class="comment">// ErrMissingBoundary is returned by Request.MultipartReader when the</span>
    <span class="comment">// request&#39;s Content-Type does not include a &#34;boundary&#34; parameter.</span>
    <span id="ErrMissingBoundary">ErrMissingBoundary</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;no multipart boundary param in Content-Type&#34;}

    <span class="comment">// ErrNotMultipart is returned by Request.MultipartReader when the</span>
    <span class="comment">// request&#39;s Content-Type is not multipart/form-data.</span>
    <span id="ErrNotMultipart">ErrNotMultipart</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;request Content-Type isn&#39;t multipart/form-data&#34;}

    <span class="comment">// Deprecated: ErrHeaderTooLong is not used.</span>
    <span id="ErrHeaderTooLong">ErrHeaderTooLong</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;header too long&#34;}
    <span class="comment">// Deprecated: ErrShortBody is not used.</span>
    <span id="ErrShortBody">ErrShortBody</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;entity body too short&#34;}
    <span class="comment">// Deprecated: ErrMissingContentLength is not used.</span>
    <span id="ErrMissingContentLength">ErrMissingContentLength</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;missing ContentLength in HEAD response&#34;}
)</pre>


<pre>var (
    <span class="comment">// ErrBodyNotAllowed is returned by ResponseWriter.Write calls</span>
    <span class="comment">// when the HTTP method or response code does not permit a</span>
    <span class="comment">// body.</span>
    <span id="ErrBodyNotAllowed">ErrBodyNotAllowed</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: request method or response status code does not allow body&#34;)

    <span class="comment">// ErrHijacked is returned by ResponseWriter.Write calls when</span>
    <span class="comment">// the underlying connection has been hijacked using the</span>
    <span class="comment">// Hijacker interface. A zero-byte write on a hijacked</span>
    <span class="comment">// connection will return ErrHijacked without any other side</span>
    <span class="comment">// effects.</span>
    <span id="ErrHijacked">ErrHijacked</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: connection has been hijacked&#34;)

    <span class="comment">// ErrContentLength is returned by ResponseWriter.Write calls</span>
    <span class="comment">// when a Handler set a Content-Length response header with a</span>
    <span class="comment">// declared size and then attempted to write more bytes than</span>
    <span class="comment">// declared.</span>
    <span id="ErrContentLength">ErrContentLength</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: wrote more than the declared Content-Length&#34;)

    <span class="comment">// Deprecated: ErrWriteAfterFlush is no longer used.</span>
    <span id="ErrWriteAfterFlush">ErrWriteAfterFlush</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;unused&#34;)
)</pre>

Errors used by the HTTP server.

<pre>var (
    <span class="comment">// ServerContextKey is a context key. It can be used in HTTP</span>
    <span class="comment">// handlers with context.WithValue to access the server that</span>
    <span class="comment">// started the handler. The associated value will be of</span>
    <span class="comment">// type *Server.</span>
    <span id="ServerContextKey">ServerContextKey</span> = &amp;contextKey{&#34;http-server&#34;}

    <span class="comment">// LocalAddrContextKey is a context key. It can be used in</span>
    <span class="comment">// HTTP handlers with context.WithValue to access the address</span>
    <span class="comment">// the local address the connection arrived on.</span>
    <span class="comment">// The associated value will be of type net.Addr.</span>
    <span id="LocalAddrContextKey">LocalAddrContextKey</span> = &amp;contextKey{&#34;local-addr&#34;}
)</pre>


<pre>var <span id="DefaultClient">DefaultClient</span> = &amp;<a href="#Client">Client</a>{}</pre>

DefaultClient is the default Client and is used by Get, Head, and Post.

<pre>var <span id="DefaultServeMux">DefaultServeMux</span> = &amp;defaultServeMux</pre>

DefaultServeMux is the default ServeMux used by Serve.

<pre>var <span id="ErrAbortHandler">ErrAbortHandler</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;net/http: abort Handler&#34;)</pre>

ErrAbortHandler is a sentinel panic value to abort a handler. While any panic
from ServeHTTP aborts the response to the client, panicking with ErrAbortHandler
also suppresses logging of a stack trace to the server's error log.

<pre>var <span id="ErrBodyReadAfterClose">ErrBodyReadAfterClose</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: invalid Read on closed Body&#34;)</pre>

ErrBodyReadAfterClose is returned when reading a Request or Response Body after
the body has been closed. This typically happens when the body is read after an
HTTP Handler calls WriteHeader or Write on its ResponseWriter.

<pre>var <span id="ErrHandlerTimeout">ErrHandlerTimeout</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: Handler timeout&#34;)</pre>

ErrHandlerTimeout is returned on ResponseWriter Write calls in handlers which
have timed out.

<pre>var <span id="ErrLineTooLong">ErrLineTooLong</span> = <a href="/net/http/internal/">internal</a>.<a href="/net/http/internal/#ErrLineTooLong">ErrLineTooLong</a></pre>

ErrLineTooLong is returned when reading request or response bodies with
malformed chunked encoding.

<pre>var <span id="ErrMissingFile">ErrMissingFile</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: no such file&#34;)</pre>

ErrMissingFile is returned by FormFile when the provided file field name is
either not present in the request or not a file field.

<pre>var <span id="ErrNoCookie">ErrNoCookie</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: named cookie not present&#34;)</pre>

ErrNoCookie is returned by Request's Cookie method when a cookie is not found.

<pre>var <span id="ErrNoLocation">ErrNoLocation</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: no Location header in response&#34;)</pre>

ErrNoLocation is returned by Response's Location method when no Location header
is present.

<pre>var <span id="ErrServerClosed">ErrServerClosed</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: Server closed&#34;)</pre>

ErrServerClosed is returned by the Server's Serve, ServeTLS, ListenAndServe, and
ListenAndServeTLS methods after a call to Shutdown or Close.

<pre>var <span id="ErrSkipAltProtocol">ErrSkipAltProtocol</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;net/http: skip alternate protocol&#34;)</pre>

ErrSkipAltProtocol is a sentinel error value defined by
Transport.RegisterProtocol.

<pre>var <span id="ErrUseLastResponse">ErrUseLastResponse</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;net/http: use last response&#34;)</pre>

ErrUseLastResponse can be returned by Client.CheckRedirect hooks to control how
redirects are processed. If returned, the next request is not sent and the most
recent response is returned with its body unclosed.

<pre>var <span id="NoBody">NoBody</span> = noBody{}</pre>

NoBody is an io.ReadCloser with no bytes. Read always returns EOF and Close
always returns nil. It can be used in an outgoing client request to explicitly
signal that a request has zero bytes. An alternative, however, is to simply set
Request.Body to nil.

<h2 id="CanonicalHeaderKey">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/header.go#L166">CanonicalHeaderKey</a>
    <a href="#CanonicalHeaderKey">¶</a></h2>
<pre>func CanonicalHeaderKey(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

CanonicalHeaderKey returns the canonical format of the header key s. The
canonicalization converts the first letter and any letter following a hyphen to
upper case; the rest are converted to lowercase. For example, the canonical key
for "accept-encoding" is "Accept-Encoding". If s contains a space or invalid
header field bytes, it is returned without modifications.

<h2 id="DetectContentType">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/sniff.go#L11">DetectContentType</a>
    <a href="#DetectContentType">¶</a></h2>
<pre>func DetectContentType(data []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#string">string</a></pre>

DetectContentType implements the algorithm described at
http://mimesniff.spec.whatwg.org/ to determine the Content-Type of the given
data. It considers at most the first 512 bytes of data. DetectContentType always
returns a valid MIME type: if it cannot determine a more specific one, it
returns "application/octet-stream".

<h2 id="Error">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L1946">Error</a>
    <a href="#Error">¶</a></h2>
<pre>func Error(w <a href="#ResponseWriter">ResponseWriter</a>, error <a href="/builtin/#string">string</a>, code <a href="/builtin/#int">int</a>)</pre>

Error replies to the request with the specified error message and HTTP code. It
does not otherwise end the request; the caller should ensure no further writes
are done to w. The error message should be plain text.

<h2 id="Handle">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2364">Handle</a>
    <a href="#Handle">¶</a></h2>
<pre>func Handle(pattern <a href="/builtin/#string">string</a>, handler <a href="#Handler">Handler</a>)</pre>

Handle registers the handler for the given pattern in the DefaultServeMux. The
documentation for ServeMux explains how patterns are matched.

<h2 id="HandleFunc">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2369">HandleFunc</a>
    <a href="#HandleFunc">¶</a></h2>
<pre>func HandleFunc(pattern <a href="/builtin/#string">string</a>, handler func(<a href="#ResponseWriter">ResponseWriter</a>, *<a href="#Request">Request</a>))</pre>

HandleFunc registers the handler function for the given pattern in the
DefaultServeMux. The documentation for ServeMux explains how patterns are
matched.

<h2 id="ListenAndServe">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2957">ListenAndServe</a>
    <a href="#ListenAndServe">¶</a></h2>
<pre>func ListenAndServe(addr <a href="/builtin/#string">string</a>, handler <a href="#Handler">Handler</a>) <a href="/builtin/#error">error</a></pre>

ListenAndServe listens on the TCP network address addr and then calls Serve with
handler to handle requests on incoming connections. Accepted connections are
configured to enable TCP keep-alives. Handler is typically nil, in which case
the DefaultServeMux is used.

A trivial example server is:

    package main

    import (
    	"io"
    	"net/http"
    	"log"
    )

    // hello world, the web server
    func HelloServer(w http.ResponseWriter, req *http.Request) {
    	io.WriteString(w, "hello, world!\n")
    }

    func main() {
    	http.HandleFunc("/hello", HelloServer)
    	log.Fatal(http.ListenAndServe(":12345", nil))
    }

ListenAndServe always returns a non-nil error.

<h2 id="ListenAndServeTLS">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2990">ListenAndServeTLS</a>
    <a href="#ListenAndServeTLS">¶</a></h2>
<pre>func ListenAndServeTLS(addr, certFile, keyFile <a href="/builtin/#string">string</a>, handler <a href="#Handler">Handler</a>) <a href="/builtin/#error">error</a></pre>

ListenAndServeTLS acts identically to ListenAndServe, except that it expects
HTTPS connections. Additionally, files containing a certificate and matching
private key for the server must be provided. If the certificate is signed by a
certificate authority, the certFile should be the concatenation of the server's
certificate, any intermediates, and the CA's certificate.

A trivial example server is:

    import (
    	"log"
    	"net/http"
    )

    func handler(w http.ResponseWriter, req *http.Request) {
    	w.Header().Set("Content-Type", "text/plain")
    	w.Write([]byte("This is an example server.\n"))
    }

    func main() {
    	http.HandleFunc("/", handler)
    	log.Printf("About to listen on 10443. Go to https://127.0.0.1:10443/")
    	err := http.ListenAndServeTLS(":10443", "cert.pem", "key.pem", nil)
    	log.Fatal(err)
    }

One can use generate_cert.go in crypto/tls to generate cert.pem and key.pem.

ListenAndServeTLS always returns a non-nil error.

<h2 id="MaxBytesReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L1017">MaxBytesReader</a>
    <a href="#MaxBytesReader">¶</a></h2>
<pre>func MaxBytesReader(w <a href="#ResponseWriter">ResponseWriter</a>, r <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>, n <a href="/builtin/#int64">int64</a>) <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a></pre>

MaxBytesReader is similar to io.LimitReader but is intended for limiting the
size of incoming request bodies. In contrast to io.LimitReader, MaxBytesReader's
result is a ReadCloser, returns a non-EOF error for a Read beyond the limit, and
closes the underlying reader when its Close method is called.

MaxBytesReader prevents clients from accidentally or maliciously sending a large
request and wasting server resources.

<h2 id="NotFound">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L1954">NotFound</a>
    <a href="#NotFound">¶</a></h2>
<pre>func NotFound(w <a href="#ResponseWriter">ResponseWriter</a>, r *<a href="#Request">Request</a>)</pre>

NotFound replies to the request with an HTTP 404 not found error.

<h2 id="ParseHTTPVersion">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L698">ParseHTTPVersion</a>
    <a href="#ParseHTTPVersion">¶</a></h2>
<pre>func ParseHTTPVersion(vers <a href="/builtin/#string">string</a>) (major, minor <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

ParseHTTPVersion parses a HTTP version string. "HTTP/1.0" returns (1, 0, true).

<h2 id="ParseTime">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/header.go#L71">ParseTime</a>
    <a href="#ParseTime">¶</a></h2>
<pre>func ParseTime(text <a href="/builtin/#string">string</a>) (t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>, err <a href="/builtin/#error">error</a>)</pre>

ParseTime parses a time header (such as the Date: header), trying each of the
three formats allowed by HTTP/1.1: TimeFormat, time.RFC850, and time.ANSIC.

<h2 id="ProxyFromEnvironment">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/transport.go#L265">ProxyFromEnvironment</a>
    <a href="#ProxyFromEnvironment">¶</a></h2>
<pre>func ProxyFromEnvironment(req *<a href="#Request">Request</a>) (*<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, <a href="/builtin/#error">error</a>)</pre>

ProxyFromEnvironment returns the URL of the proxy to use for a given request, as
indicated by the environment variables HTTP_PROXY, HTTPS_PROXY and NO_PROXY (or
the lowercase versions thereof). HTTPS_PROXY takes precedence over HTTP_PROXY
for https requests.

The environment values may be either a complete URL or a "host[:port]", in which
case the "http" scheme is assumed. An error is returned if the value is a
different form.

A nil URL and nil error are returned if no proxy is defined in the environment,
or a proxy should not be used for the given request, as defined by NO_PROXY.

As a special case, if req.URL.Host is "localhost" (with or without a port
number), then a nil URL and nil error will be returned.

<h2 id="ProxyURL">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/transport.go#L303">ProxyURL</a>
    <a href="#ProxyURL">¶</a></h2>
<pre>func ProxyURL(fixedURL *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>) func(*<a href="#Request">Request</a>) (*<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, <a href="/builtin/#error">error</a>)</pre>

ProxyURL returns a proxy function (for use in a Transport) that always returns
the same URL.

<h2 id="Redirect">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L1988">Redirect</a>
    <a href="#Redirect">¶</a></h2>
<pre>func Redirect(w <a href="#ResponseWriter">ResponseWriter</a>, r *<a href="#Request">Request</a>, url <a href="/builtin/#string">string</a>, code <a href="/builtin/#int">int</a>)</pre>

Redirect replies to the request with a redirect to url, which may be a path
relative to the request path.

The provided code should be in the 3xx range and is usually
StatusMovedPermanently, StatusFound or StatusSeeOther.

<h2 id="Serve">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2377">Serve</a>
    <a href="#Serve">¶</a></h2>
<pre>func Serve(l <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>, handler <a href="#Handler">Handler</a>) <a href="/builtin/#error">error</a></pre>

Serve accepts incoming HTTP connections on the listener l, creating a new
service goroutine for each. The service goroutines read requests and then call
handler to reply to them. Handler is typically nil, in which case the
DefaultServeMux is used.

<h2 id="ServeContent">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fs.go#L141">ServeContent</a>
    <a href="#ServeContent">¶</a></h2>
<pre>func ServeContent(w <a href="#ResponseWriter">ResponseWriter</a>, req *<a href="#Request">Request</a>, name <a href="/builtin/#string">string</a>, modtime <a href="/time/">time</a>.<a href="/time/#Time">Time</a>, content <a href="/io/">io</a>.<a href="/io/#ReadSeeker">ReadSeeker</a>)</pre>

ServeContent replies to the request using the content in the provided
ReadSeeker. The main benefit of ServeContent over io.Copy is that it handles
Range requests properly, sets the MIME type, and handles If-Match,
If-Unmodified-Since, If-None-Match, If-Modified-Since, and If-Range requests.

If the response's Content-Type header is not set, ServeContent first tries to
deduce the type from name's file extension and, if that fails, falls back to
reading the first block of the content and passing it to DetectContentType. The
name is otherwise unused; in particular it can be empty and is never sent in the
response.

If modtime is not the zero time or Unix epoch, ServeContent includes it in a
Last-Modified header in the response. If the request includes an
If-Modified-Since header, ServeContent uses modtime to decide whether the
content needs to be sent at all.

The content's Seek method must work: ServeContent uses a seek to the end of the
content to determine its size.

If the caller has set w's ETag header formatted per RFC 7232, section 2.3,
ServeContent uses it to handle requests using If-Match, If-None-Match, or
If-Range.

Note that *os.File implements the io.ReadSeeker interface.

<h2 id="ServeFile">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fs.go#L652">ServeFile</a>
    <a href="#ServeFile">¶</a></h2>
<pre>func ServeFile(w <a href="#ResponseWriter">ResponseWriter</a>, r *<a href="#Request">Request</a>, name <a href="/builtin/#string">string</a>)</pre>

ServeFile replies to the request with the contents of the named file or
directory.

If the provided file or directory name is a relative path, it is interpreted
relative to the current directory and may ascend to parent directories. If the
provided name is constructed from user input, it should be sanitized before
calling ServeFile. As a precaution, ServeFile will reject requests where
r.URL.Path contains a ".." path element.

As a special case, ServeFile redirects any request where r.URL.Path ends in
"/index.html" to the same path, without the final "index.html". To avoid such
redirects either modify the path or use ServeContent.

<h2 id="ServeTLS">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2392">ServeTLS</a>
    <a href="#ServeTLS">¶</a></h2>
<pre>func ServeTLS(l <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>, handler <a href="#Handler">Handler</a>, certFile, keyFile <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

ServeTLS accepts incoming HTTPS connections on the listener l, creating a new
service goroutine for each. The service goroutines read requests and then call
handler to reply to them.

Handler is typically nil, in which case the DefaultServeMux is used.

Additionally, files containing a certificate and matching private key for the
server must be provided. If the certificate is signed by a certificate
authority, the certFile should be the concatenation of the server's certificate,
any intermediates, and the CA's certificate.

<h2 id="SetCookie">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookie.go#L122">SetCookie</a>
    <a href="#SetCookie">¶</a></h2>
<pre>func SetCookie(w <a href="#ResponseWriter">ResponseWriter</a>, cookie *<a href="#Cookie">Cookie</a>)</pre>

SetCookie adds a Set-Cookie header to the provided ResponseWriter's headers. The
provided cookie must have a valid Name. Invalid cookies may be silently dropped.

<h2 id="StatusText">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/status.go#L134">StatusText</a>
    <a href="#StatusText">¶</a></h2>
<pre>func StatusText(code <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

StatusText returns a text for the HTTP status code. It returns the empty string
if the code is unknown.

<h2 id="Client">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L46">Client</a>
    <a href="#Client">¶</a></h2>
<pre>type Client struct {
<span id="Client.Transport"></span>    <span class="comment">// Transport specifies the mechanism by which individual</span>
    <span class="comment">// HTTP requests are made.</span>
    <span class="comment">// If nil, DefaultTransport is used.</span>
    Transport <a href="#RoundTripper">RoundTripper</a>

<span id="Client.CheckRedirect"></span>    <span class="comment">// CheckRedirect specifies the policy for handling redirects.</span>
    <span class="comment">// If CheckRedirect is not nil, the client calls it before</span>
    <span class="comment">// following an HTTP redirect. The arguments req and via are</span>
    <span class="comment">// the upcoming request and the requests made already, oldest</span>
    <span class="comment">// first. If CheckRedirect returns an error, the Client&#39;s Get</span>
    <span class="comment">// method returns both the previous Response (with its Body</span>
    <span class="comment">// closed) and CheckRedirect&#39;s error (wrapped in a url.Error)</span>
    <span class="comment">// instead of issuing the Request req.</span>
    <span class="comment">// As a special case, if CheckRedirect returns ErrUseLastResponse,</span>
    <span class="comment">// then the most recent response is returned with its body</span>
    <span class="comment">// unclosed, along with a nil error.</span>
    <span class="comment">//</span>
    <span class="comment">// If CheckRedirect is nil, the Client uses its default policy,</span>
    <span class="comment">// which is to stop after 10 consecutive requests.</span>
    CheckRedirect func(req *<a href="#Request">Request</a>, via []*<a href="#Request">Request</a>) <a href="/builtin/#error">error</a>

<span id="Client.Jar"></span>    <span class="comment">// Jar specifies the cookie jar.</span>
    <span class="comment">//</span>
    <span class="comment">// The Jar is used to insert relevant cookies into every</span>
    <span class="comment">// outbound Request and is updated with the cookie values</span>
    <span class="comment">// of every inbound Response. The Jar is consulted for every</span>
    <span class="comment">// redirect that the Client follows.</span>
    <span class="comment">//</span>
    <span class="comment">// If Jar is nil, cookies are only sent if they are explicitly</span>
    <span class="comment">// set on the Request.</span>
    Jar <a href="#CookieJar">CookieJar</a>

<span id="Client.Timeout"></span>    <span class="comment">// Timeout specifies a time limit for requests made by this</span>
    <span class="comment">// Client. The timeout includes connection time, any</span>
    <span class="comment">// redirects, and reading the response body. The timer remains</span>
    <span class="comment">// running after Get, Head, Post, or Do return and will</span>
    <span class="comment">// interrupt reading of the Response.Body.</span>
    <span class="comment">//</span>
    <span class="comment">// A Timeout of zero means no timeout.</span>
    <span class="comment">//</span>
    <span class="comment">// The Client cancels requests to the underlying Transport</span>
    <span class="comment">// using the Request.Cancel mechanism. Requests passed</span>
    <span class="comment">// to Client.Do may still set Request.Cancel; both will</span>
    <span class="comment">// cancel the request.</span>
    <span class="comment">//</span>
    <span class="comment">// For compatibility, the Client will also use the deprecated</span>
    <span class="comment">// CancelRequest method on Transport if found. New</span>
    <span class="comment">// RoundTripper implementations should use Request.Cancel</span>
    <span class="comment">// instead of implementing CancelRequest.</span>
    Timeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>
}</pre>

A Client is an HTTP client. Its zero value (DefaultClient) is a usable client
that uses DefaultTransport.

The Client's Transport typically has internal state (cached TCP connections), so
Clients should be reused instead of created as needed. Clients are safe for
concurrent use by multiple goroutines.

A Client is higher-level than a RoundTripper (such as Transport) and
additionally handles HTTP details such as cookies and redirects.

When following redirects, the Client will forward all headers set on the initial
Request except:

• when forwarding sensitive headers like "Authorization", "WWW-Authenticate",
and "Cookie" to untrusted targets. These headers will be ignored when following
a redirect to a domain that is not a subdomain match or exact match of the
initial domain. For example, a redirect from "foo.com" to either "foo.com" or
"sub.foo.com" will forward the sensitive headers, but a redirect to "bar.com"
will not.

• when forwarding the "Cookie" header with a non-nil cookie Jar. Since each
redirect may mutate the state of the cookie jar, a redirect may possibly alter a
cookie set in the initial request. When forwarding the "Cookie" header, any
mutated cookies will be omitted, with the expectation that the Jar will insert
those mutated cookies with the updated values (assuming the origin matches). If
Jar is nil, the initial cookies are forwarded without change.

<h3 id="Client.Do">func (*Client) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L483">Do</a>
    <a href="#Client.Do">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Do(req *<a href="#Request">Request</a>) (*<a href="#Response">Response</a>, <a href="/builtin/#error">error</a>)</pre>

Do sends an HTTP request and returns an HTTP response, following policy (such as
redirects, cookies, auth) as configured on the client.

An error is returned if caused by client policy (such as CheckRedirect), or
failure to speak HTTP (such as a network connectivity problem). A non-2xx status
code doesn't cause an error.

If the returned error is nil, the Response will contain a non-nil Body which the
user is expected to close. If the Body is not closed, the Client's underlying
RoundTripper (typically Transport) may not be able to re-use a persistent TCP
connection to the server for a subsequent "keep-alive" request.

The request Body, if non-nil, will be closed by the underlying Transport, even
on errors.

On error, any Response can be ignored. A non-nil Response with a non-nil error
only occurs when CheckRedirect fails, and even then the returned Response.Body
is already closed.

Generally Get, Post, or PostForm will be used instead of Do.

If the server replies with a redirect, the Client first uses the CheckRedirect
function to determine whether the redirect should be followed. If permitted, a
301, 302, or 303 redirect causes subsequent requests to use HTTP method GET (or
HEAD if the original request was HEAD), with no body. A 307 or 308 redirect
preserves the original HTTP method and body, provided that the Request.GetBody
function is defined. The NewRequest function automatically sets GetBody for
common standard library body types.

<h3 id="Client.Get">func (*Client) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L381">Get</a>
    <a href="#Client.Get">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Get(url <a href="/builtin/#string">string</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Get issues a GET to the specified URL. If the response is one of the following
redirect codes, Get follows the redirect after calling the Client's
CheckRedirect function:

    301 (Moved Permanently)
    302 (Found)
    303 (See Other)
    307 (Temporary Redirect)
    308 (Permanent Redirect)

An error is returned if the Client's CheckRedirect function fails or if there
was an HTTP protocol error. A non-2xx response doesn't cause an error.

When err is nil, resp always contains a non-nil resp.Body. Caller should close
resp.Body when done reading from it.

To make a request with custom headers, use NewRequest and Client.Do.

<h3 id="Client.Head">func (*Client) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L791">Head</a>
    <a href="#Client.Head">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Head(url <a href="/builtin/#string">string</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Head issues a HEAD to the specified URL. If the response is one of the following
redirect codes, Head follows the redirect after calling the Client's
CheckRedirect function:

    301 (Moved Permanently)
    302 (Found)
    303 (See Other)
    307 (Temporary Redirect)
    308 (Permanent Redirect)

<h3 id="Client.Post">func (*Client) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L726">Post</a>
    <a href="#Client.Post">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Post(url <a href="/builtin/#string">string</a>, contentType <a href="/builtin/#string">string</a>, body <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Post issues a POST to the specified URL.

Caller should close resp.Body when done reading from it.

If the provided body is an io.Closer, it is closed after the request.

To set custom headers, use NewRequest and Client.Do.

See the Client.Do method documentation for details on how redirects are handled.

<h3 id="Client.PostForm">func (*Client) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L763">PostForm</a>
    <a href="#Client.PostForm">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) PostForm(url <a href="/builtin/#string">string</a>, data <a href="/net/url/">url</a>.<a href="/net/url/#Values">Values</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

PostForm issues a POST to the specified URL, with data's keys and values
URL-encoded as the request body.

The Content-Type header is set to application/x-www-form-urlencoded. To set
other headers, use NewRequest and Client.Do.

When err is nil, resp always contains a non-nil resp.Body. Caller should close
resp.Body when done reading from it.

See the Client.Do method documentation for details on how redirects are handled.

<h2 id="CloseNotifier">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L190">CloseNotifier</a>
    <a href="#CloseNotifier">¶</a></h2>
<pre>type CloseNotifier interface {
    <span class="comment">// CloseNotify returns a channel that receives at most a</span>
    <span class="comment">// single value (true) when the client connection has gone</span>
    <span class="comment">// away.</span>
    <span class="comment">//</span>
    <span class="comment">// CloseNotify may wait to notify until Request.Body has been</span>
    <span class="comment">// fully read.</span>
    <span class="comment">//</span>
    <span class="comment">// After the Handler has returned, there is no guarantee</span>
    <span class="comment">// that the channel receives a value.</span>
    <span class="comment">//</span>
    <span class="comment">// If the protocol is HTTP/1.1 and CloseNotify is called while</span>
    <span class="comment">// processing an idempotent request (such a GET) while</span>
    <span class="comment">// HTTP/1.1 pipelining is in use, the arrival of a subsequent</span>
    <span class="comment">// pipelined request may cause a value to be sent on the</span>
    <span class="comment">// returned channel. In practice HTTP/1.1 pipelining is not</span>
    <span class="comment">// enabled in browsers and not seen often in the wild. If this</span>
    <span class="comment">// is a problem, use HTTP/2 or only use CloseNotify on methods</span>
    <span class="comment">// such as POST.</span>
    CloseNotify() &lt;-chan <a href="/builtin/#bool">bool</a>
}</pre>

The CloseNotifier interface is implemented by ResponseWriters which allow
detecting when the underlying connection has gone away.

This mechanism can be used to cancel long operations on the server if the client
has disconnected before the response is ready.

<h2 id="ConnState">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2620">ConnState</a>
    <a href="#ConnState">¶</a></h2>
<pre>type ConnState <a href="/builtin/#int">int</a></pre>

A ConnState represents the state of a client connection to a server. It's used
by the optional Server.ConnState hook.

<pre>const (
    <span class="comment">// StateNew represents a new connection that is expected to</span>
    <span class="comment">// send a request immediately. Connections begin at this</span>
    <span class="comment">// state and then transition to either StateActive or</span>
    <span class="comment">// StateClosed.</span>
    <span id="StateNew">StateNew</span> <a href="#ConnState">ConnState</a> = <a href="/builtin/#iota">iota</a>

    <span class="comment">// StateActive represents a connection that has read 1 or more</span>
    <span class="comment">// bytes of a request. The Server.ConnState hook for</span>
    <span class="comment">// StateActive fires before the request has entered a handler</span>
    <span class="comment">// and doesn&#39;t fire again until the request has been</span>
    <span class="comment">// handled. After the request is handled, the state</span>
    <span class="comment">// transitions to StateClosed, StateHijacked, or StateIdle.</span>
    <span class="comment">// For HTTP/2, StateActive fires on the transition from zero</span>
    <span class="comment">// to one active request, and only transitions away once all</span>
    <span class="comment">// active requests are complete. That means that ConnState</span>
    <span class="comment">// cannot be used to do per-request work; ConnState only notes</span>
    <span class="comment">// the overall state of the connection.</span>
    <span id="StateActive">StateActive</span>

    <span class="comment">// StateIdle represents a connection that has finished</span>
    <span class="comment">// handling a request and is in the keep-alive state, waiting</span>
    <span class="comment">// for a new request. Connections transition from StateIdle</span>
    <span class="comment">// to either StateActive or StateClosed.</span>
    <span id="StateIdle">StateIdle</span>

    <span class="comment">// StateHijacked represents a hijacked connection.</span>
    <span class="comment">// This is a terminal state. It does not transition to StateClosed.</span>
    <span id="StateHijacked">StateHijacked</span>

    <span class="comment">// StateClosed represents a closed connection.</span>
    <span class="comment">// This is a terminal state. Hijacked connections do not</span>
    <span class="comment">// transition to StateClosed.</span>
    <span id="StateClosed">StateClosed</span>
)</pre>


<h3 id="ConnState.String">func (ConnState) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2666">String</a>
    <a href="#ConnState.String">¶</a></h3>
<pre>func (c <a href="#ConnState">ConnState</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Cookie">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookie.go#L10">Cookie</a>
    <a href="#Cookie">¶</a></h2>
<pre>type Cookie struct {
<span id="Cookie.Name"></span>    Name  <a href="/builtin/#string">string</a>
<span id="Cookie.Value"></span>    Value <a href="/builtin/#string">string</a>

<span id="Cookie.Path"></span>    Path       <a href="/builtin/#string">string</a>    <span class="comment">// optional</span>
<span id="Cookie.Domain"></span>    Domain     <a href="/builtin/#string">string</a>    <span class="comment">// optional</span>
<span id="Cookie.Expires"></span>    Expires    <a href="/time/">time</a>.<a href="/time/#Time">Time</a> <span class="comment">// optional</span>
<span id="Cookie.RawExpires"></span>    RawExpires <a href="/builtin/#string">string</a>    <span class="comment">// for reading cookies only</span>

<span id="Cookie.MaxAge"></span>    <span class="comment">// MaxAge=0 means no &#39;Max-Age&#39; attribute specified.</span>
    <span class="comment">// MaxAge&lt;0 means delete cookie now, equivalently &#39;Max-Age: 0&#39;</span>
    <span class="comment">// MaxAge&gt;0 means Max-Age attribute present and given in seconds</span>
    MaxAge   <a href="/builtin/#int">int</a>
<span id="Cookie.Secure"></span>    Secure   <a href="/builtin/#bool">bool</a>
<span id="Cookie.HttpOnly"></span>    HttpOnly <a href="/builtin/#bool">bool</a>
<span id="Cookie.Raw"></span>    Raw      <a href="/builtin/#string">string</a>
<span id="Cookie.Unparsed"></span>    Unparsed []<a href="/builtin/#string">string</a> <span class="comment">// Raw text of unparsed attribute-value pairs</span>
}</pre>

A Cookie represents an HTTP cookie as sent in the Set-Cookie header of an HTTP
response or the Cookie header of an HTTP request.

See http://tools.ietf.org/html/rfc6265 for details.

<h3 id="Cookie.String">func (*Cookie) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookie.go#L132">String</a>
    <a href="#Cookie.String">¶</a></h3>
<pre>func (c *<a href="#Cookie">Cookie</a>) String() <a href="/builtin/#string">string</a></pre>

String returns the serialization of the cookie for use in a Cookie header (if
only Name and Value are set) or a Set-Cookie response header (if other fields
are set). If c is nil or c.Name is invalid, the empty string is returned.

<h2 id="CookieJar">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/jar.go#L7">CookieJar</a>
    <a href="#CookieJar">¶</a></h2>
<pre>type CookieJar interface {
    <span class="comment">// SetCookies handles the receipt of the cookies in a reply for the</span>
    <span class="comment">// given URL.  It may or may not choose to save the cookies, depending</span>
    <span class="comment">// on the jar&#39;s policy and implementation.</span>
    SetCookies(u *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, cookies []*<a href="#Cookie">Cookie</a>)

    <span class="comment">// Cookies returns the cookies to send in a request for the given URL.</span>
    <span class="comment">// It is up to the implementation to honor the standard cookie use</span>
    <span class="comment">// restrictions such as in RFC 6265.</span>
    Cookies(u *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>) []*<a href="#Cookie">Cookie</a>
}</pre>

A CookieJar manages storage and use of cookies in HTTP requests.

Implementations of CookieJar must be safe for concurrent use by multiple
goroutines.

The net/http/cookiejar package provides a CookieJar implementation.

<h2 id="Dir">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fs.go#L30">Dir</a>
    <a href="#Dir">¶</a></h2>
<pre>type Dir <a href="/builtin/#string">string</a></pre>

A Dir implements FileSystem using the native file system restricted to a
specific directory tree.

While the FileSystem.Open method takes '/'-separated paths, a Dir's string value
is a filename on the native file system, not a URL, so it is separated by
filepath.Separator, which isn't necessarily '/'.

Note that Dir will allow access to files and directories starting with a period,
which could expose sensitive directories like a .git directory or sensitive
files like .htpasswd. To exclude files with a leading period, remove the
files/directories from the server or create a custom FileSystem implementation.

An empty Dir is treated as ".".

<h3 id="Dir.Open">func (Dir) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fs.go#L56">Open</a>
    <a href="#Dir.Open">¶</a></h3>
<pre>func (d <a href="#Dir">Dir</a>) Open(name <a href="/builtin/#string">string</a>) (<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>


<h2 id="File">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fs.go#L83">File</a>
    <a href="#File">¶</a></h2>
<pre>type File interface {
    <a href="/io/">io</a>.<a href="/io/#Closer">Closer</a>
    <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>
    <a href="/io/">io</a>.<a href="/io/#Seeker">Seeker</a>
    Readdir(count <a href="/builtin/#int">int</a>) ([]<a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)
    Stat() (<a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)
}</pre>

A File is returned by a FileSystem's Open method and can be served by the
FileServer implementation.

The methods should behave the same as those on an *os.File.

<h2 id="FileSystem">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fs.go#L75">FileSystem</a>
    <a href="#FileSystem">¶</a></h2>
<pre>type FileSystem interface {
    Open(name <a href="/builtin/#string">string</a>) (<a href="#File">File</a>, <a href="/builtin/#error">error</a>)
}</pre>

A FileSystem implements access to a collection of named files. The elements in a
file path are separated by slash ('/', U+002F) characters, regardless of host
operating system convention.

<h2 id="Flusher">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L152">Flusher</a>
    <a href="#Flusher">¶</a></h2>
<pre>type Flusher interface {
    <span class="comment">// Flush sends any buffered data to the client.</span>
    Flush()
}</pre>

The Flusher interface is implemented by ResponseWriters that allow an HTTP
handler to flush buffered data to the client.

The default HTTP/1.x and HTTP/2 ResponseWriter implementations support Flusher,
but ResponseWriter wrappers may not. Handlers should always test for this
ability at runtime.

Note that even for ResponseWriters that support Flush, if the client is
connected through an HTTP proxy, the buffered data may not reach the client
until the response completes.

<h2 id="Handler">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L72">Handler</a>
    <a href="#Handler">¶</a></h2>
<pre>type Handler interface {
    ServeHTTP(<a href="#ResponseWriter">ResponseWriter</a>, *<a href="#Request">Request</a>)
}</pre>

A Handler responds to an HTTP request.

ServeHTTP should write reply headers and data to the ResponseWriter and then
return. Returning signals that the request is finished; it is not valid to use
the ResponseWriter or read from the Request.Body after or concurrently with the
completion of the ServeHTTP call.

Depending on the HTTP client software, HTTP protocol version, and any
intermediaries between the client and the Go server, it may not be possible to
read from the Request.Body after writing to the ResponseWriter. Cautious
handlers should read the Request.Body first, and then reply.

Except for reading the body, handlers should not modify the provided Request.

If ServeHTTP panics, the server (the caller of ServeHTTP) assumes that the
effect of the panic was isolated to the active request. It recovers the panic,
logs a stack trace to the server error log, and either closes the network
connection or sends an HTTP/2 RST_STREAM, depending on the HTTP protocol. To
abort a handler so the client sees an interrupted response but the server
doesn't log an error, panic with the value ErrAbortHandler.

<h3 id="FileServer">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fs.go#L695">FileServer</a>
    <a href="#FileServer">¶</a></h3>
<pre>func FileServer(root <a href="#FileSystem">FileSystem</a>) <a href="#Handler">Handler</a></pre>

FileServer returns a handler that serves HTTP requests with the contents of the
file system rooted at root.

To use the operating system's file system implementation, use http.Dir:

    http.Handle("/", http.FileServer(http.Dir("/tmp")))

As a special case, the returned file server redirects any request ending in
"/index.html" to the same path, without the final "index.html".

<a id="exampleFileServer"></a>
Example:

    // Simple static webserver:
    log.Fatal(http.ListenAndServe(":8080", http.FileServer(http.Dir("/usr/share/doc"))))


<a id="exampleFileServer_stripPrefix"></a>
Example:

    // To serve a directory on disk (/tmp) under an alternate URL
    // path (/tmpfiles/), use StripPrefix to modify the request
    // URL's path before the FileServer sees it:
    http.Handle("/tmpfiles/", http.StripPrefix("/tmpfiles/", http.FileServer(http.Dir("/tmp"))))

<h3 id="NotFoundHandler">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L1958">NotFoundHandler</a>
    <a href="#NotFoundHandler">¶</a></h3>
<pre>func NotFoundHandler() <a href="#Handler">Handler</a></pre>

NotFoundHandler returns a simple request handler that replies to each request
with a ``404 page not found'' reply.

<h3 id="RedirectHandler">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2084">RedirectHandler</a>
    <a href="#RedirectHandler">¶</a></h3>
<pre>func RedirectHandler(url <a href="/builtin/#string">string</a>, code <a href="/builtin/#int">int</a>) <a href="#Handler">Handler</a></pre>

RedirectHandler returns a request handler that redirects each request it
receives to the given url using the given status code.

The provided code should be in the 3xx range and is usually
StatusMovedPermanently, StatusFound or StatusSeeOther.

<h3 id="StripPrefix">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L1965">StripPrefix</a>
    <a href="#StripPrefix">¶</a></h3>
<pre>func StripPrefix(prefix <a href="/builtin/#string">string</a>, h <a href="#Handler">Handler</a>) <a href="#Handler">Handler</a></pre>

StripPrefix returns a handler that serves HTTP requests by removing the given
prefix from the request URL's Path and invoking the handler h. StripPrefix
handles a request for a path that doesn't begin with prefix by replying with an
HTTP 404 not found error.

<a id="exampleStripPrefix"></a>
Example:

    // To serve a directory on disk (/tmp) under an alternate URL
    // path (/tmpfiles/), use StripPrefix to modify the request
    // URL's path before the FileServer sees it:
    http.Handle("/tmpfiles/", http.StripPrefix("/tmpfiles/", http.FileServer(http.Dir("/tmp"))))

<h3 id="TimeoutHandler">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L3080">TimeoutHandler</a>
    <a href="#TimeoutHandler">¶</a></h3>
<pre>func TimeoutHandler(h <a href="#Handler">Handler</a>, dt <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>, msg <a href="/builtin/#string">string</a>) <a href="#Handler">Handler</a></pre>

TimeoutHandler returns a Handler that runs h with the given time limit.

The new Handler calls h.ServeHTTP to handle each request, but if a call runs for
longer than its time limit, the handler responds with a 503 Service Unavailable
error and the given message in its body. (If msg is empty, a suitable default
message will be sent.) After such a timeout, writes by h to its ResponseWriter
will return ErrHandlerTimeout.

TimeoutHandler buffers all Handler writes to memory and does not support the
Hijacker or Flusher interfaces.

<h2 id="HandlerFunc">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L1933">HandlerFunc</a>
    <a href="#HandlerFunc">¶</a></h2>
<pre>type HandlerFunc func(<a href="#ResponseWriter">ResponseWriter</a>, *<a href="#Request">Request</a>)</pre>

The HandlerFunc type is an adapter to allow the use of ordinary functions as
HTTP handlers. If f is a function with the appropriate signature, HandlerFunc(f)
is a Handler that calls f.

<h3 id="HandlerFunc.ServeHTTP">func (HandlerFunc) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L1936">ServeHTTP</a>
    <a href="#HandlerFunc.ServeHTTP">¶</a></h3>
<pre>func (f <a href="#HandlerFunc">HandlerFunc</a>) ServeHTTP(w <a href="#ResponseWriter">ResponseWriter</a>, r *<a href="#Request">Request</a>)</pre>

ServeHTTP calls f(w, r).

<h2 id="Header">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/header.go#L9">Header</a>
    <a href="#Header">¶</a></h2>
<pre>type Header map[<a href="/builtin/#string">string</a>][]<a href="/builtin/#string">string</a></pre>

A Header represents the key-value pairs in an HTTP header.

<h3 id="Header.Add">func (Header) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/header.go#L13">Add</a>
    <a href="#Header.Add">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) Add(key, value <a href="/builtin/#string">string</a>)</pre>

Add adds the key, value pair to the header. It appends to any existing values
associated with key.

<h3 id="Header.Del">func (Header) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/header.go#L43">Del</a>
    <a href="#Header.Del">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) Del(key <a href="/builtin/#string">string</a>)</pre>

Del deletes the values associated with key.

<h3 id="Header.Get">func (Header) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/header.go#L30">Get</a>
    <a href="#Header.Get">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) Get(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Get gets the first value associated with the given key. It is case insensitive;
textproto.CanonicalMIMEHeaderKey is used to canonicalize the provided key. If
there are no values associated with the key, Get returns "". To access multiple
values of a key, or to use non-canonical keys, access the map directly.

<h3 id="Header.Set">func (Header) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/header.go#L20">Set</a>
    <a href="#Header.Set">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) Set(key, value <a href="/builtin/#string">string</a>)</pre>

Set sets the header entries associated with key to the single element value. It
replaces any existing values associated with key.

<h3 id="Header.Write">func (Header) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/header.go#L48">Write</a>
    <a href="#Header.Write">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) Write(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

Write writes a header in wire format.

<h3 id="Header.WriteSubset">func (Header) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/header.go#L137">WriteSubset</a>
    <a href="#Header.WriteSubset">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) WriteSubset(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, exclude map[<a href="/builtin/#string">string</a>]<a href="/builtin/#bool">bool</a>) <a href="/builtin/#error">error</a></pre>

WriteSubset writes a header in wire format. If exclude is not nil, keys where
exclude[key] == true are not written.

<h2 id="Hijacker">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L164">Hijacker</a>
    <a href="#Hijacker">¶</a></h2>
<pre>type Hijacker interface {
    <span class="comment">// Hijack lets the caller take over the connection.</span>
    <span class="comment">// After a call to Hijack the HTTP server library</span>
    <span class="comment">// will not do anything else with the connection.</span>
    <span class="comment">//</span>
    <span class="comment">// It becomes the caller&#39;s responsibility to manage</span>
    <span class="comment">// and close the connection.</span>
    <span class="comment">//</span>
    <span class="comment">// The returned net.Conn may have read or write deadlines</span>
    <span class="comment">// already set, depending on the configuration of the</span>
    <span class="comment">// Server. It is the caller&#39;s responsibility to set</span>
    <span class="comment">// or clear those deadlines as needed.</span>
    <span class="comment">//</span>
    <span class="comment">// The returned bufio.Reader may contain unprocessed buffered</span>
    <span class="comment">// data from the client.</span>
    <span class="comment">//</span>
    <span class="comment">// After a call to Hijack, the original Request.Body must</span>
    <span class="comment">// not be used.</span>
    Hijack() (<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, *<a href="/bufio/">bufio</a>.<a href="/bufio/#ReadWriter">ReadWriter</a>, <a href="/builtin/#error">error</a>)
}</pre>

The Hijacker interface is implemented by ResponseWriters that allow an HTTP
handler to take over the connection.

The default ResponseWriter for HTTP/1.x connections supports Hijacker, but
HTTP/2 connections intentionally do not. ResponseWriter wrappers may also not
support Hijacker. Handlers should always test for this ability at runtime.

<a id="exampleHijacker"></a>
Example:

    http.HandleFunc("/hijack", func(w http.ResponseWriter, r *http.Request) {
        hj, ok := w.(http.Hijacker)
        if !ok {
            http.Error(w, "webserver doesn't support hijacking", http.StatusInternalServerError)
            return
        }
        conn, bufrw, err := hj.Hijack()
        if err != nil {
            http.Error(w, err.Error(), http.StatusInternalServerError)
            return
        }
        // Don't forget to close the connection:
        defer conn.Close()
        bufrw.WriteString("Now we're speaking raw TCP. Say hi: ")
        bufrw.Flush()
        s, err := bufrw.ReadString('\n')
        if err != nil {
            log.Printf("error reading string: %v", err)
            return
        }
        fmt.Fprintf(bufrw, "You said: %q\nBye.\n", s)
        bufrw.Flush()
    })

<h2 id="ProtocolError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L34">ProtocolError</a>
    <a href="#ProtocolError">¶</a></h2>
<pre>type ProtocolError struct {
<span id="ProtocolError.ErrorString"></span>    ErrorString <a href="/builtin/#string">string</a>
}</pre>

ProtocolError represents an HTTP protocol error.

Deprecated: Not all errors in the http package related to protocol errors are of
type ProtocolError.

<h3 id="ProtocolError.Error">func (*ProtocolError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L38">Error</a>
    <a href="#ProtocolError.Error">¶</a></h3>
<pre>func (pe *<a href="#ProtocolError">ProtocolError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="PushOptions">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/http.go#L95">PushOptions</a>
    <a href="#PushOptions">¶</a></h2>
<pre>type PushOptions struct {
<span id="PushOptions.Method"></span>    <span class="comment">// Method specifies the HTTP method for the promised request.</span>
    <span class="comment">// If set, it must be &#34;GET&#34; or &#34;HEAD&#34;. Empty means &#34;GET&#34;.</span>
    Method <a href="/builtin/#string">string</a>

<span id="PushOptions.Header"></span>    <span class="comment">// Header specifies additional promised request headers. This cannot</span>
    <span class="comment">// include HTTP/2 pseudo header fields like &#34;:path&#34; and &#34;:scheme&#34;,</span>
    <span class="comment">// which will be added automatically.</span>
    Header <a href="#Header">Header</a>
}</pre>

PushOptions describes options for Pusher.Push.

<h2 id="Pusher">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/http.go#L109">Pusher</a>
    <a href="#Pusher">¶</a></h2>
<pre>type Pusher interface {
    <span class="comment">// Push initiates an HTTP/2 server push. This constructs a synthetic</span>
    <span class="comment">// request using the given target and options, serializes that request</span>
    <span class="comment">// into a PUSH_PROMISE frame, then dispatches that request using the</span>
    <span class="comment">// server&#39;s request handler. If opts is nil, default options are used.</span>
    <span class="comment">//</span>
    <span class="comment">// The target must either be an absolute path (like &#34;/path&#34;) or an absolute</span>
    <span class="comment">// URL that contains a valid host and the same scheme as the parent request.</span>
    <span class="comment">// If the target is a path, it will inherit the scheme and host of the</span>
    <span class="comment">// parent request.</span>
    <span class="comment">//</span>
    <span class="comment">// The HTTP/2 spec disallows recursive pushes and cross-authority pushes.</span>
    <span class="comment">// Push may or may not detect these invalid pushes; however, invalid</span>
    <span class="comment">// pushes will be detected and canceled by conforming clients.</span>
    <span class="comment">//</span>
    <span class="comment">// Handlers that wish to push URL X should call Push before sending any</span>
    <span class="comment">// data that may trigger a request for URL X. This avoids a race where the</span>
    <span class="comment">// client issues requests for X before receiving the PUSH_PROMISE for X.</span>
    <span class="comment">//</span>
    <span class="comment">// Push returns ErrNotSupported if the client has disabled push or if push</span>
    <span class="comment">// is not supported on the underlying connection.</span>
    Push(target <a href="/builtin/#string">string</a>, opts *<a href="#PushOptions">PushOptions</a>) <a href="/builtin/#error">error</a>
}</pre>

Pusher is the interface implemented by ResponseWriters that support HTTP/2
server push. For more background, see
https://tools.ietf.org/html/rfc7540#section-8.2.

<h2 id="Request">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L88">Request</a>
    <a href="#Request">¶</a></h2>
<pre>type Request struct {
<span id="Request.Method"></span>    <span class="comment">// Method specifies the HTTP method (GET, POST, PUT, etc.).</span>
    <span class="comment">// For client requests an empty string means GET.</span>
    <span class="comment">//</span>
    <span class="comment">// Go&#39;s HTTP client does not support sending a request with</span>
    <span class="comment">// the CONNECT method. See the documentation on Transport for</span>
    <span class="comment">// details.</span>
    Method <a href="/builtin/#string">string</a>

<span id="Request.URL"></span>    <span class="comment">// URL specifies either the URI being requested (for server</span>
    <span class="comment">// requests) or the URL to access (for client requests).</span>
    <span class="comment">//</span>
    <span class="comment">// For server requests the URL is parsed from the URI</span>
    <span class="comment">// supplied on the Request-Line as stored in RequestURI.  For</span>
    <span class="comment">// most requests, fields other than Path and RawQuery will be</span>
    <span class="comment">// empty. (See RFC 2616, Section 5.1.2)</span>
    <span class="comment">//</span>
    <span class="comment">// For client requests, the URL&#39;s Host specifies the server to</span>
    <span class="comment">// connect to, while the Request&#39;s Host field optionally</span>
    <span class="comment">// specifies the Host header value to send in the HTTP</span>
    <span class="comment">// request.</span>
    URL *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>

    <span class="comment">// The protocol version for incoming server requests.</span>
    <span class="comment">//</span>
    <span class="comment">// For client requests these fields are ignored. The HTTP</span>
    <span class="comment">// client code always uses either HTTP/1.1 or HTTP/2.</span>
    <span class="comment">// See the docs on Transport for details.</span>
<span id="Request.Proto"></span>    Proto      <a href="/builtin/#string">string</a> <span class="comment">// &#34;HTTP/1.0&#34;</span>
<span id="Request.ProtoMajor"></span>    ProtoMajor <a href="/builtin/#int">int</a>    <span class="comment">// 1</span>
<span id="Request.ProtoMinor"></span>    ProtoMinor <a href="/builtin/#int">int</a>    <span class="comment">// 0</span>

<span id="Request.Header"></span>    <span class="comment">// Header contains the request header fields either received</span>
    <span class="comment">// by the server or to be sent by the client.</span>
    <span class="comment">//</span>
    <span class="comment">// If a server received a request with header lines,</span>
    <span class="comment">//</span>
    <span class="comment">//	Host: example.com</span>
    <span class="comment">//	accept-encoding: gzip, deflate</span>
    <span class="comment">//	Accept-Language: en-us</span>
    <span class="comment">//	fOO: Bar</span>
    <span class="comment">//	foo: two</span>
    <span class="comment">//</span>
    <span class="comment">// then</span>
    <span class="comment">//</span>
    <span class="comment">//	Header = map[string][]string{</span>
    <span class="comment">//		&#34;Accept-Encoding&#34;: {&#34;gzip, deflate&#34;},</span>
    <span class="comment">//		&#34;Accept-Language&#34;: {&#34;en-us&#34;},</span>
    <span class="comment">//		&#34;Foo&#34;: {&#34;Bar&#34;, &#34;two&#34;},</span>
    <span class="comment">//	}</span>
    <span class="comment">//</span>
    <span class="comment">// For incoming requests, the Host header is promoted to the</span>
    <span class="comment">// Request.Host field and removed from the Header map.</span>
    <span class="comment">//</span>
    <span class="comment">// HTTP defines that header names are case-insensitive. The</span>
    <span class="comment">// request parser implements this by using CanonicalHeaderKey,</span>
    <span class="comment">// making the first character and any characters following a</span>
    <span class="comment">// hyphen uppercase and the rest lowercase.</span>
    <span class="comment">//</span>
    <span class="comment">// For client requests, certain headers such as Content-Length</span>
    <span class="comment">// and Connection are automatically written when needed and</span>
    <span class="comment">// values in Header may be ignored. See the documentation</span>
    <span class="comment">// for the Request.Write method.</span>
    Header <a href="#Header">Header</a>

<span id="Request.Body"></span>    <span class="comment">// Body is the request&#39;s body.</span>
    <span class="comment">//</span>
    <span class="comment">// For client requests a nil body means the request has no</span>
    <span class="comment">// body, such as a GET request. The HTTP Client&#39;s Transport</span>
    <span class="comment">// is responsible for calling the Close method.</span>
    <span class="comment">//</span>
    <span class="comment">// For server requests the Request Body is always non-nil</span>
    <span class="comment">// but will return EOF immediately when no body is present.</span>
    <span class="comment">// The Server will close the request body. The ServeHTTP</span>
    <span class="comment">// Handler does not need to.</span>
    Body <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>

<span id="Request.GetBody"></span>    <span class="comment">// GetBody defines an optional func to return a new copy of</span>
    <span class="comment">// Body. It is used for client requests when a redirect requires</span>
    <span class="comment">// reading the body more than once. Use of GetBody still</span>
    <span class="comment">// requires setting Body.</span>
    <span class="comment">//</span>
    <span class="comment">// For server requests it is unused.</span>
    GetBody func() (<a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>, <a href="/builtin/#error">error</a>)

<span id="Request.ContentLength"></span>    <span class="comment">// ContentLength records the length of the associated content.</span>
    <span class="comment">// The value -1 indicates that the length is unknown.</span>
    <span class="comment">// Values &gt;= 0 indicate that the given number of bytes may</span>
    <span class="comment">// be read from Body.</span>
    <span class="comment">// For client requests, a value of 0 with a non-nil Body is</span>
    <span class="comment">// also treated as unknown.</span>
    ContentLength <a href="/builtin/#int64">int64</a>

<span id="Request.TransferEncoding"></span>    <span class="comment">// TransferEncoding lists the transfer encodings from outermost to</span>
    <span class="comment">// innermost. An empty list denotes the &#34;identity&#34; encoding.</span>
    <span class="comment">// TransferEncoding can usually be ignored; chunked encoding is</span>
    <span class="comment">// automatically added and removed as necessary when sending and</span>
    <span class="comment">// receiving requests.</span>
    TransferEncoding []<a href="/builtin/#string">string</a>

<span id="Request.Close"></span>    <span class="comment">// Close indicates whether to close the connection after</span>
    <span class="comment">// replying to this request (for servers) or after sending this</span>
    <span class="comment">// request and reading its response (for clients).</span>
    <span class="comment">//</span>
    <span class="comment">// For server requests, the HTTP server handles this automatically</span>
    <span class="comment">// and this field is not needed by Handlers.</span>
    <span class="comment">//</span>
    <span class="comment">// For client requests, setting this field prevents re-use of</span>
    <span class="comment">// TCP connections between requests to the same hosts, as if</span>
    <span class="comment">// Transport.DisableKeepAlives were set.</span>
    Close <a href="/builtin/#bool">bool</a>

    <span class="comment">// For server requests Host specifies the host on which the</span>
    <span class="comment">// URL is sought. Per RFC 2616, this is either the value of</span>
    <span class="comment">// the &#34;Host&#34; header or the host name given in the URL itself.</span>
    <span class="comment">// It may be of the form &#34;host:port&#34;. For international domain</span>
    <span class="comment">// names, Host may be in Punycode or Unicode form. Use</span>
    <span class="comment">// golang.org/x/net/idna to convert it to either format if</span>
    <span class="comment">// needed.</span>
    <span class="comment">//</span>
    <span class="comment">// For client requests Host optionally overrides the Host</span>
    <span class="comment">// header to send. If empty, the Request.Write method uses</span>
    <span class="comment">// the value of URL.Host. Host may contain an international</span>
    <span class="comment">// domain name.</span>
<span id="Request.Host"></span>    Host <a href="/builtin/#string">string</a>

<span id="Request.Form"></span>    <span class="comment">// Form contains the parsed form data, including both the URL</span>
    <span class="comment">// field&#39;s query parameters and the POST or PUT form data.</span>
    <span class="comment">// This field is only available after ParseForm is called.</span>
    <span class="comment">// The HTTP client ignores Form and uses Body instead.</span>
    Form <a href="/net/url/">url</a>.<a href="/net/url/#Values">Values</a>

<span id="Request.PostForm"></span>    <span class="comment">// PostForm contains the parsed form data from POST, PATCH,</span>
    <span class="comment">// or PUT body parameters.</span>
    <span class="comment">//</span>
    <span class="comment">// This field is only available after ParseForm is called.</span>
    <span class="comment">// The HTTP client ignores PostForm and uses Body instead.</span>
    PostForm <a href="/net/url/">url</a>.<a href="/net/url/#Values">Values</a>

<span id="Request.MultipartForm"></span>    <span class="comment">// MultipartForm is the parsed multipart form, including file uploads.</span>
    <span class="comment">// This field is only available after ParseMultipartForm is called.</span>
    <span class="comment">// The HTTP client ignores MultipartForm and uses Body instead.</span>
    MultipartForm *<a href="/mime/multipart/">multipart</a>.<a href="/mime/multipart/#Form">Form</a>

<span id="Request.Trailer"></span>    <span class="comment">// Trailer specifies additional headers that are sent after the request</span>
    <span class="comment">// body.</span>
    <span class="comment">//</span>
    <span class="comment">// For server requests the Trailer map initially contains only the</span>
    <span class="comment">// trailer keys, with nil values. (The client declares which trailers it</span>
    <span class="comment">// will later send.)  While the handler is reading from Body, it must</span>
    <span class="comment">// not reference Trailer. After reading from Body returns EOF, Trailer</span>
    <span class="comment">// can be read again and will contain non-nil values, if they were sent</span>
    <span class="comment">// by the client.</span>
    <span class="comment">//</span>
    <span class="comment">// For client requests Trailer must be initialized to a map containing</span>
    <span class="comment">// the trailer keys to later send. The values may be nil or their final</span>
    <span class="comment">// values. The ContentLength must be 0 or -1, to send a chunked request.</span>
    <span class="comment">// After the HTTP request is sent the map values can be updated while</span>
    <span class="comment">// the request body is read. Once the body returns EOF, the caller must</span>
    <span class="comment">// not mutate Trailer.</span>
    <span class="comment">//</span>
    <span class="comment">// Few HTTP clients, servers, or proxies support HTTP trailers.</span>
    Trailer <a href="#Header">Header</a>

<span id="Request.RemoteAddr"></span>    <span class="comment">// RemoteAddr allows HTTP servers and other software to record</span>
    <span class="comment">// the network address that sent the request, usually for</span>
    <span class="comment">// logging. This field is not filled in by ReadRequest and</span>
    <span class="comment">// has no defined format. The HTTP server in this package</span>
    <span class="comment">// sets RemoteAddr to an &#34;IP:port&#34; address before invoking a</span>
    <span class="comment">// handler.</span>
    <span class="comment">// This field is ignored by the HTTP client.</span>
    RemoteAddr <a href="/builtin/#string">string</a>

<span id="Request.RequestURI"></span>    <span class="comment">// RequestURI is the unmodified Request-URI of the</span>
    <span class="comment">// Request-Line (RFC 2616, Section 5.1) as sent by the client</span>
    <span class="comment">// to a server. Usually the URL field should be used instead.</span>
    <span class="comment">// It is an error to set this field in an HTTP client request.</span>
    RequestURI <a href="/builtin/#string">string</a>

<span id="Request.TLS"></span>    <span class="comment">// TLS allows HTTP servers and other software to record</span>
    <span class="comment">// information about the TLS connection on which the request</span>
    <span class="comment">// was received. This field is not filled in by ReadRequest.</span>
    <span class="comment">// The HTTP server in this package sets the field for</span>
    <span class="comment">// TLS-enabled connections before invoking a handler;</span>
    <span class="comment">// otherwise it leaves the field nil.</span>
    <span class="comment">// This field is ignored by the HTTP client.</span>
    TLS *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#ConnectionState">ConnectionState</a>

<span id="Request.Cancel"></span>    <span class="comment">// Cancel is an optional channel whose closure indicates that the client</span>
    <span class="comment">// request should be regarded as canceled. Not all implementations of</span>
    <span class="comment">// RoundTripper may support Cancel.</span>
    <span class="comment">//</span>
    <span class="comment">// For server requests, this field is not applicable.</span>
    <span class="comment">//</span>
    <span class="comment">// Deprecated: Use the Context and WithContext methods</span>
    <span class="comment">// instead. If a Request&#39;s Cancel field and context are both</span>
    <span class="comment">// set, it is undefined whether Cancel is respected.</span>
    Cancel &lt;-chan struct{}

<span id="Request.Response"></span>    <span class="comment">// Response is the redirect response which caused this request</span>
    <span class="comment">// to be created. This field is only populated during client</span>
    <span class="comment">// redirects.</span>
    Response *<a href="#Response">Response</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Request represents an HTTP request received by a server or to be sent by a
client.

The field semantics differ slightly between client and server usage. In addition
to the notes on the fields below, see the documentation for Request.Write and
RoundTripper.

<h3 id="NewRequest">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L759">NewRequest</a>
    <a href="#NewRequest">¶</a></h3>
<pre>func NewRequest(method, url <a href="/builtin/#string">string</a>, body <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (*<a href="#Request">Request</a>, <a href="/builtin/#error">error</a>)</pre>

NewRequest returns a new Request given a method, URL, and optional body.

If the provided body is also an io.Closer, the returned Request.Body is set to
body and will be closed by the Client methods Do, Post, and PostForm, and
Transport.RoundTrip.

NewRequest returns a Request suitable for use with Client.Do or
Transport.RoundTrip. To create a request for use with testing a Server Handler,
either use the NewRequest function in the net/http/httptest package, use
ReadRequest, or manually update the Request fields. See the Request type's
documentation for the difference between inbound and outbound request fields.

If body is of type *bytes.Buffer, *bytes.Reader, or *strings.Reader, the
returned request's ContentLength is set to its exact value (instead of -1),
GetBody is populated (so 307 and 308 redirects can replay the body), and Body is
set to NoBody if the ContentLength is 0.

<h3 id="ReadRequest">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L903">ReadRequest</a>
    <a href="#ReadRequest">¶</a></h3>
<pre>func ReadRequest(b *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>) (*<a href="#Request">Request</a>, <a href="/builtin/#error">error</a>)</pre>

ReadRequest reads and parses an incoming request from b.

<h3 id="Request.AddCookie">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L374">AddCookie</a>
    <a href="#Request.AddCookie">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) AddCookie(c *<a href="#Cookie">Cookie</a>)</pre>

AddCookie adds a cookie to the request. Per RFC 6265 section 5.4, AddCookie does
not attach more than one Cookie header field. That means all cookies, if any,
are written into the same line, separated by semicolon.

<h3 id="Request.BasicAuth">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L839">BasicAuth</a>
    <a href="#Request.BasicAuth">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) BasicAuth() (username, password <a href="/builtin/#string">string</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

BasicAuth returns the username and password provided in the request's
Authorization header, if the request uses HTTP Basic Authentication. See RFC
2617, Section 2.

<h3 id="Request.Context">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L310">Context</a>
    <a href="#Request.Context">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) Context() <a href="/context/">context</a>.<a href="/context/#Context">Context</a></pre>

Context returns the request's context. To change the context, use WithContext.

The returned context is always non-nil; it defaults to the background context.

For outgoing client requests, the context controls cancelation.

For incoming server requests, the context is canceled when the client's
connection closes, the request is canceled (with HTTP/2), or when the ServeHTTP
method returns.

<h3 id="Request.Cookie">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L363">Cookie</a>
    <a href="#Request.Cookie">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) Cookie(name <a href="/builtin/#string">string</a>) (*<a href="#Cookie">Cookie</a>, <a href="/builtin/#error">error</a>)</pre>

Cookie returns the named cookie provided in the request or ErrNoCookie if not
found. If multiple cookies match the given name, only one cookie will be
returned.

<h3 id="Request.Cookies">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L352">Cookies</a>
    <a href="#Request.Cookies">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) Cookies() []*<a href="#Cookie">Cookie</a></pre>

Cookies parses and returns the HTTP cookies sent with the request.

<h3 id="Request.FormFile">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L1258">FormFile</a>
    <a href="#Request.FormFile">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) FormFile(key <a href="/builtin/#string">string</a>) (<a href="/mime/multipart/">multipart</a>.<a href="/mime/multipart/#File">File</a>, *<a href="/mime/multipart/">multipart</a>.<a href="/mime/multipart/#FileHeader">FileHeader</a>, <a href="/builtin/#error">error</a>)</pre>

FormFile returns the first file for the provided form key. FormFile calls
ParseMultipartForm and ParseForm if necessary.

<h3 id="Request.FormValue">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L1231">FormValue</a>
    <a href="#Request.FormValue">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) FormValue(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

FormValue returns the first value for the named component of the query. POST and
PUT body parameters take precedence over URL query string values. FormValue
calls ParseMultipartForm and ParseForm if necessary and ignores any errors
returned by these functions. If key is not present, FormValue returns the empty
string. To access multiple values of the same key, call ParseForm and then
inspect Request.Form directly.

<h3 id="Request.MultipartReader">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L407">MultipartReader</a>
    <a href="#Request.MultipartReader">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) MultipartReader() (*<a href="/mime/multipart/">multipart</a>.<a href="/mime/multipart/#Reader">Reader</a>, <a href="/builtin/#error">error</a>)</pre>

MultipartReader returns a MIME multipart reader if this is a multipart/form-data
POST request, else returns nil and an error. Use this function instead of
ParseMultipartForm to process the request body as a stream.

<h3 id="Request.ParseForm">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L1145">ParseForm</a>
    <a href="#Request.ParseForm">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) ParseForm() <a href="/builtin/#error">error</a></pre>

ParseForm populates r.Form and r.PostForm.

For all requests, ParseForm parses the raw query from the URL and updates
r.Form.

For POST, PUT, and PATCH requests, it also parses the request body as a form and
puts the results into both r.PostForm and r.Form. Request body parameters take
precedence over URL query string values in r.Form.

For other HTTP methods, or when the Content-Type is not
application/x-www-form-urlencoded, the request Body is not read, and r.PostForm
is initialized to a non-nil, empty value.

If the request Body's size has not already been limited by MaxBytesReader, the
size is capped at 10MB.

ParseMultipartForm calls ParseForm automatically. ParseForm is idempotent.

<h3 id="Request.ParseMultipartForm">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L1186">ParseMultipartForm</a>
    <a href="#Request.ParseMultipartForm">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) ParseMultipartForm(maxMemory <a href="/builtin/#int64">int64</a>) <a href="/builtin/#error">error</a></pre>

ParseMultipartForm parses a request body as multipart/form-data. The whole
request body is parsed and up to a total of maxMemory bytes of its file parts
are stored in memory, with the remainder stored on disk in temporary files.
ParseMultipartForm calls ParseForm if necessary. After one call to
ParseMultipartForm, subsequent calls have no effect.

<h3 id="Request.PostFormValue">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L1246">PostFormValue</a>
    <a href="#Request.PostFormValue">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) PostFormValue(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

PostFormValue returns the first value for the named component of the POST or PUT
request body. URL query parameters are ignored. PostFormValue calls
ParseMultipartForm and ParseForm if necessary and ignores any errors returned by
these functions. If key is not present, PostFormValue returns the empty string.

<h3 id="Request.ProtoAtLeast">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L341">ProtoAtLeast</a>
    <a href="#Request.ProtoAtLeast">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) ProtoAtLeast(major, minor <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>

ProtoAtLeast reports whether the HTTP protocol used in the request is at least
major.minor.

<h3 id="Request.Referer">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L391">Referer</a>
    <a href="#Request.Referer">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) Referer() <a href="/builtin/#string">string</a></pre>

Referer returns the referring URL, if sent in the request.

Referer is misspelled as in the request itself, a mistake from the earliest days
of HTTP. This value can also be fetched from the Header map as
Header["Referer"]; the benefit of making it available as a method is that the
compiler can diagnose programs that use the alternate (correct English) spelling
req.Referrer() but cannot diagnose programs that use Header["Referrer"].

<h3 id="Request.SetBasicAuth">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L871">SetBasicAuth</a>
    <a href="#Request.SetBasicAuth">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) SetBasicAuth(username, password <a href="/builtin/#string">string</a>)</pre>

SetBasicAuth sets the request's Authorization header to use HTTP Basic
Authentication with the provided username and password.

With HTTP Basic Authentication the provided username and password are not
encrypted.

<h3 id="Request.UserAgent">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L347">UserAgent</a>
    <a href="#Request.UserAgent">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) UserAgent() <a href="/builtin/#string">string</a></pre>

UserAgent returns the client's User-Agent, if sent in the request.

<h3 id="Request.WithContext">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L319">WithContext</a>
    <a href="#Request.WithContext">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) WithContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>) *<a href="#Request">Request</a></pre>

WithContext returns a shallow copy of r with its context changed to ctx. The
provided ctx must be non-nil.

<h3 id="Request.Write">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L467">Write</a>
    <a href="#Request.Write">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) Write(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

Write writes an HTTP/1.1 request, which is the header and body, in wire format.
This method consults the following fields of the request:

    Host
    URL
    Method (defaults to "GET")
    Header
    ContentLength
    TransferEncoding
    Body

If Body is present, Content-Length is <= 0 and TransferEncoding hasn't been set
to "identity", Write adds "Transfer-Encoding: chunked" to the header. Body is
closed after it is sent.

<h3 id="Request.WriteProxy">func (*Request) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/request.go#L477">WriteProxy</a>
    <a href="#Request.WriteProxy">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) WriteProxy(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

WriteProxy is like Write but writes the request in the form expected by an HTTP
proxy. In particular, WriteProxy writes the initial Request-URI line of the
request with an absolute URI, per section 5.1.2 of RFC 2616, including the
scheme and host. In either case, WriteProxy also writes a Host header, using
either r.Host or r.URL.Host.

<h2 id="Response">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/response.go#L23">Response</a>
    <a href="#Response">¶</a></h2>
<pre>type Response struct {
<span id="Response.Status"></span>    Status     <a href="/builtin/#string">string</a> <span class="comment">// e.g. &#34;200 OK&#34;</span>
<span id="Response.StatusCode"></span>    StatusCode <a href="/builtin/#int">int</a>    <span class="comment">// e.g. 200</span>
<span id="Response.Proto"></span>    Proto      <a href="/builtin/#string">string</a> <span class="comment">// e.g. &#34;HTTP/1.0&#34;</span>
<span id="Response.ProtoMajor"></span>    ProtoMajor <a href="/builtin/#int">int</a>    <span class="comment">// e.g. 1</span>
<span id="Response.ProtoMinor"></span>    ProtoMinor <a href="/builtin/#int">int</a>    <span class="comment">// e.g. 0</span>

<span id="Response.Header"></span>    <span class="comment">// Header maps header keys to values. If the response had multiple</span>
    <span class="comment">// headers with the same key, they may be concatenated, with comma</span>
    <span class="comment">// delimiters.  (Section 4.2 of RFC 2616 requires that multiple headers</span>
    <span class="comment">// be semantically equivalent to a comma-delimited sequence.) When</span>
    <span class="comment">// Header values are duplicated by other fields in this struct (e.g.,</span>
<span id="Response.ContentLength"></span>    <span class="comment">// ContentLength, TransferEncoding, Trailer), the field values are</span>
    <span class="comment">// authoritative.</span>
    <span class="comment">//</span>
    <span class="comment">// Keys in the map are canonicalized (see CanonicalHeaderKey).</span>
    Header <a href="#Header">Header</a>

<span id="Response.Body"></span>    <span class="comment">// Body represents the response body.</span>
    <span class="comment">//</span>
    <span class="comment">// The response body is streamed on demand as the Body field</span>
    <span class="comment">// is read. If the network connection fails or the server</span>
    <span class="comment">// terminates the response, Body.Read calls return an error.</span>
    <span class="comment">//</span>
    <span class="comment">// The http Client and Transport guarantee that Body is always</span>
    <span class="comment">// non-nil, even on responses without a body or responses with</span>
    <span class="comment">// a zero-length body. It is the caller&#39;s responsibility to</span>
    <span class="comment">// close Body. The default HTTP client&#39;s Transport may not</span>
    <span class="comment">// reuse HTTP/1.x &#34;keep-alive&#34; TCP connections if the Body is</span>
    <span class="comment">// not read to completion and closed.</span>
    <span class="comment">//</span>
    <span class="comment">// The Body is automatically dechunked if the server replied</span>
    <span class="comment">// with a &#34;chunked&#34; Transfer-Encoding.</span>
    Body <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>

    <span class="comment">// ContentLength records the length of the associated content. The</span>
    <span class="comment">// value -1 indicates that the length is unknown. Unless Request.Method</span>
    <span class="comment">// is &#34;HEAD&#34;, values &gt;= 0 indicate that the given number of bytes may</span>
    <span class="comment">// be read from Body.</span>
    ContentLength <a href="/builtin/#int64">int64</a>

    <span class="comment">// Contains transfer encodings from outer-most to inner-most. Value is</span>
    <span class="comment">// nil, means that &#34;identity&#34; encoding is used.</span>
<span id="Response.TransferEncoding"></span>    TransferEncoding []<a href="/builtin/#string">string</a>

<span id="Response.Close"></span>    <span class="comment">// Close records whether the header directed that the connection be</span>
    <span class="comment">// closed after reading Body. The value is advice for clients: neither</span>
    <span class="comment">// ReadResponse nor Response.Write ever closes a connection.</span>
    Close <a href="/builtin/#bool">bool</a>

<span id="Response.Uncompressed"></span>    <span class="comment">// Uncompressed reports whether the response was sent compressed but</span>
    <span class="comment">// was decompressed by the http package. When true, reading from</span>
    <span class="comment">// Body yields the uncompressed content instead of the compressed</span>
    <span class="comment">// content actually set from the server, ContentLength is set to -1,</span>
    <span class="comment">// and the &#34;Content-Length&#34; and &#34;Content-Encoding&#34; fields are deleted</span>
    <span class="comment">// from the responseHeader. To get the original response from</span>
    <span class="comment">// the server, set Transport.DisableCompression to true.</span>
    Uncompressed <a href="/builtin/#bool">bool</a>

<span id="Response.Trailer"></span>    <span class="comment">// Trailer maps trailer keys to values in the same</span>
    <span class="comment">// format as Header.</span>
    <span class="comment">//</span>
    <span class="comment">// The Trailer initially contains only nil values, one for</span>
    <span class="comment">// each key specified in the server&#39;s &#34;Trailer&#34; header</span>
    <span class="comment">// value. Those values are not added to Header.</span>
    <span class="comment">//</span>
    <span class="comment">// Trailer must not be accessed concurrently with Read calls</span>
    <span class="comment">// on the Body.</span>
    <span class="comment">//</span>
    <span class="comment">// After Body.Read has returned io.EOF, Trailer will contain</span>
    <span class="comment">// any trailer values sent by the server.</span>
    Trailer <a href="#Header">Header</a>

<span id="Response.Request"></span>    <span class="comment">// Request is the request that was sent to obtain this Response.</span>
    <span class="comment">// Request&#39;s Body is nil (having already been consumed).</span>
    <span class="comment">// This is only populated for Client requests.</span>
    Request *<a href="#Request">Request</a>

<span id="Response.TLS"></span>    <span class="comment">// TLS contains information about the TLS connection on which the</span>
    <span class="comment">// response was received. It is nil for unencrypted responses.</span>
    <span class="comment">// The pointer is shared between responses and should not be</span>
    <span class="comment">// modified.</span>
    TLS *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#ConnectionState">ConnectionState</a>
}</pre>

Response represents the response from an HTTP request.

The Client and Transport return Responses from servers once the response headers
have been received. The response body is streamed on demand as the Body field is
read.

<h3 id="Get">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L359">Get</a>
    <a href="#Get">¶</a></h3>
<pre>func Get(url <a href="/builtin/#string">string</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Get issues a GET to the specified URL. If the response is one of the following
redirect codes, Get follows the redirect, up to a maximum of 10 redirects:

    301 (Moved Permanently)
    302 (Found)
    303 (See Other)
    307 (Temporary Redirect)
    308 (Permanent Redirect)

An error is returned if there were too many redirects or if there was an HTTP
protocol error. A non-2xx response doesn't cause an error.

When err is nil, resp always contains a non-nil resp.Body. Caller should close
resp.Body when done reading from it.

Get is a wrapper around DefaultClient.Get.

To make a request with custom headers, use NewRequest and DefaultClient.Do.

<a id="exampleGet"></a>
Example:

    res, err := http.Get("http://www.google.com/robots.txt")
    if err != nil {
        log.Fatal(err)
    }
    robots, err := ioutil.ReadAll(res.Body)
    res.Body.Close()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%s", robots)

<h3 id="Head">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L778">Head</a>
    <a href="#Head">¶</a></h3>
<pre>func Head(url <a href="/builtin/#string">string</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Head issues a HEAD to the specified URL. If the response is one of the following
redirect codes, Head follows the redirect, up to a maximum of 10 redirects:

    301 (Moved Permanently)
    302 (Found)
    303 (See Other)
    307 (Temporary Redirect)
    308 (Permanent Redirect)

Head is a wrapper around DefaultClient.Head

<h3 id="Post">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L711">Post</a>
    <a href="#Post">¶</a></h3>
<pre>func Post(url <a href="/builtin/#string">string</a>, contentType <a href="/builtin/#string">string</a>, body <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Post issues a POST to the specified URL.

Caller should close resp.Body when done reading from it.

If the provided body is an io.Closer, it is closed after the request.

Post is a wrapper around DefaultClient.Post.

To set custom headers, use NewRequest and DefaultClient.Do.

See the Client.Do method documentation for details on how redirects are handled.

<h3 id="PostForm">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L748">PostForm</a>
    <a href="#PostForm">¶</a></h3>
<pre>func PostForm(url <a href="/builtin/#string">string</a>, data <a href="/net/url/">url</a>.<a href="/net/url/#Values">Values</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

PostForm issues a POST to the specified URL, with data's keys and values
URL-encoded as the request body.

The Content-Type header is set to application/x-www-form-urlencoded. To set
other headers, use NewRequest and DefaultClient.Do.

When err is nil, resp always contains a non-nil resp.Body. Caller should close
resp.Body when done reading from it.

PostForm is a wrapper around DefaultClient.PostForm.

See the Client.Do method documentation for details on how redirects are handled.

<h3 id="ReadResponse">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/response.go#L138">ReadResponse</a>
    <a href="#ReadResponse">¶</a></h3>
<pre>func ReadResponse(r *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>, req *<a href="#Request">Request</a>) (*<a href="#Response">Response</a>, <a href="/builtin/#error">error</a>)</pre>

ReadResponse reads and returns an HTTP response from r. The req parameter
optionally specifies the Request that corresponds to this Response. If nil, a
GET request is assumed. Clients must call resp.Body.Close when finished reading
resp.Body. After that call, clients can inspect resp.Trailer to find key/value
pairs included in the response trailer.

<h3 id="Response.Cookies">func (*Response) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/response.go#L109">Cookies</a>
    <a href="#Response.Cookies">¶</a></h3>
<pre>func (r *<a href="#Response">Response</a>) Cookies() []*<a href="#Cookie">Cookie</a></pre>

Cookies parses and returns the cookies set in the Set-Cookie headers.

<h3 id="Response.Location">func (*Response) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/response.go#L121">Location</a>
    <a href="#Response.Location">¶</a></h3>
<pre>func (r *<a href="#Response">Response</a>) Location() (*<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, <a href="/builtin/#error">error</a>)</pre>

Location returns the URL of the response's "Location" header, if present.
Relative redirects are resolved relative to the Response's Request.
ErrNoLocation is returned if no Location header is present.

<h3 id="Response.ProtoAtLeast">func (*Response) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/response.go#L208">ProtoAtLeast</a>
    <a href="#Response.ProtoAtLeast">¶</a></h3>
<pre>func (r *<a href="#Response">Response</a>) ProtoAtLeast(major, minor <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>

ProtoAtLeast reports whether the HTTP protocol used in the response is at least
major.minor.

<h3 id="Response.Write">func (*Response) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/response.go#L229">Write</a>
    <a href="#Response.Write">¶</a></h3>
<pre>func (r *<a href="#Response">Response</a>) Write(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

Write writes r to w in the HTTP/1.x server response format, including the status
line, headers, body, and optional trailer.

This method consults the following fields of the response r:

    StatusCode
    ProtoMajor
    ProtoMinor
    Request.Method
    TransferEncoding
    Trailer
    Body
    ContentLength
    Header, values for non-canonical keys will have unpredictable behavior

The Response Body is closed after it is sent.

<h2 id="ResponseWriter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L81">ResponseWriter</a>
    <a href="#ResponseWriter">¶</a></h2>
<pre>type ResponseWriter interface {
    <span class="comment">// Header returns the header map that will be sent by</span>
    <span class="comment">// WriteHeader. The Header map also is the mechanism with which</span>
    <span class="comment">// Handlers can set HTTP trailers.</span>
    <span class="comment">//</span>
    <span class="comment">// Changing the header map after a call to WriteHeader (or</span>
    <span class="comment">// Write) has no effect unless the modified headers are</span>
    <span class="comment">// trailers.</span>
    <span class="comment">//</span>
    <span class="comment">// There are two ways to set Trailers. The preferred way is to</span>
    <span class="comment">// predeclare in the headers which trailers you will later</span>
    <span class="comment">// send by setting the &#34;Trailer&#34; header to the names of the</span>
    <span class="comment">// trailer keys which will come later. In this case, those</span>
    <span class="comment">// keys of the Header map are treated as if they were</span>
    <span class="comment">// trailers. See the example. The second way, for trailer</span>
    <span class="comment">// keys not known to the Handler until after the first Write,</span>
    <span class="comment">// is to prefix the Header map keys with the TrailerPrefix</span>
    <span class="comment">// constant value. See TrailerPrefix.</span>
    <span class="comment">//</span>
    <span class="comment">// To suppress implicit response headers (such as &#34;Date&#34;), set</span>
    <span class="comment">// their value to nil.</span>
    Header() <a href="#Header">Header</a>

    <span class="comment">// Write writes the data to the connection as part of an HTTP reply.</span>
    <span class="comment">//</span>
    <span class="comment">// If WriteHeader has not yet been called, Write calls</span>
    <span class="comment">// WriteHeader(http.StatusOK) before writing the data. If the Header</span>
    <span class="comment">// does not contain a Content-Type line, Write adds a Content-Type set</span>
    <span class="comment">// to the result of passing the initial 512 bytes of written data to</span>
    <span class="comment">// DetectContentType.</span>
    <span class="comment">//</span>
    <span class="comment">// Depending on the HTTP protocol version and the client, calling</span>
    <span class="comment">// Write or WriteHeader may prevent future reads on the</span>
    <span class="comment">// Request.Body. For HTTP/1.x requests, handlers should read any</span>
    <span class="comment">// needed request body data before writing the response. Once the</span>
    <span class="comment">// headers have been flushed (due to either an explicit Flusher.Flush</span>
    <span class="comment">// call or writing enough data to trigger a flush), the request body</span>
    <span class="comment">// may be unavailable. For HTTP/2 requests, the Go HTTP server permits</span>
    <span class="comment">// handlers to continue to read the request body while concurrently</span>
    <span class="comment">// writing the response. However, such behavior may not be supported</span>
    <span class="comment">// by all HTTP/2 clients. Handlers should read before writing if</span>
    <span class="comment">// possible to maximize compatibility.</span>
    Write([]<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)

    <span class="comment">// WriteHeader sends an HTTP response header with the provided</span>
    <span class="comment">// status code.</span>
    <span class="comment">//</span>
    <span class="comment">// If WriteHeader is not called explicitly, the first call to Write</span>
    <span class="comment">// will trigger an implicit WriteHeader(http.StatusOK).</span>
    <span class="comment">// Thus explicit calls to WriteHeader are mainly used to</span>
    <span class="comment">// send error codes.</span>
    <span class="comment">//</span>
    <span class="comment">// The provided code must be a valid HTTP 1xx-5xx status code.</span>
    <span class="comment">// Only one header may be written. Go does not currently</span>
    <span class="comment">// support sending user-defined 1xx informational headers,</span>
    <span class="comment">// with the exception of 100-continue response header that the</span>
    <span class="comment">// Server sends automatically when the Request.Body is read.</span>
    WriteHeader(statusCode <a href="/builtin/#int">int</a>)
}</pre>

A ResponseWriter interface is used by an HTTP handler to construct an HTTP
response.

A ResponseWriter may not be used after the Handler.ServeHTTP method has
returned.

<a id="exampleResponseWriter_trailers"></a>
Example:

    mux := http.NewServeMux()
    mux.HandleFunc("/sendstrailers", func(w http.ResponseWriter, req *http.Request) {
        // Before any call to WriteHeader or Write, declare
        // the trailers you will set during the HTTP
        // response. These three headers are actually sent in
        // the trailer.
        w.Header().Set("Trailer", "AtEnd1, AtEnd2")
        w.Header().Add("Trailer", "AtEnd3")

        w.Header().Set("Content-Type", "text/plain; charset=utf-8") // normal header
        w.WriteHeader(http.StatusOK)

        w.Header().Set("AtEnd1", "value 1")
        io.WriteString(w, "This HTTP response has both headers before this text and trailers at the end.\n")
        w.Header().Set("AtEnd2", "value 2")
        w.Header().Set("AtEnd3", "value 3") // These will appear as trailers.
    })

<h2 id="RoundTripper">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/client.go#L107">RoundTripper</a>
    <a href="#RoundTripper">¶</a></h2>
<pre>type RoundTripper interface {
    <span class="comment">// RoundTrip executes a single HTTP transaction, returning</span>
    <span class="comment">// a Response for the provided Request.</span>
    <span class="comment">//</span>
    <span class="comment">// RoundTrip should not attempt to interpret the response. In</span>
    <span class="comment">// particular, RoundTrip must return err == nil if it obtained</span>
    <span class="comment">// a response, regardless of the response&#39;s HTTP status code.</span>
    <span class="comment">// A non-nil err should be reserved for failure to obtain a</span>
    <span class="comment">// response. Similarly, RoundTrip should not attempt to</span>
    <span class="comment">// handle higher-level protocol details such as redirects,</span>
    <span class="comment">// authentication, or cookies.</span>
    <span class="comment">//</span>
    <span class="comment">// RoundTrip should not modify the request, except for</span>
    <span class="comment">// consuming and closing the Request&#39;s Body. RoundTrip may</span>
    <span class="comment">// read fields of the request in a separate goroutine. Callers</span>
    <span class="comment">// should not mutate the request until the Response&#39;s Body has</span>
    <span class="comment">// been closed.</span>
    <span class="comment">//</span>
    <span class="comment">// RoundTrip must always close the body, including on errors,</span>
    <span class="comment">// but depending on the implementation may do so in a separate</span>
    <span class="comment">// goroutine even after RoundTrip returns. This means that</span>
    <span class="comment">// callers wanting to reuse the body for subsequent requests</span>
    <span class="comment">// must arrange to wait for the Close call before doing so.</span>
    <span class="comment">//</span>
    <span class="comment">// The Request&#39;s URL and Header fields must be initialized.</span>
    RoundTrip(*<a href="#Request">Request</a>) (*<a href="#Response">Response</a>, <a href="/builtin/#error">error</a>)
}</pre>

RoundTripper is an interface representing the ability to execute a single HTTP
transaction, obtaining the Response for a given Request.

A RoundTripper must be safe for concurrent use by multiple goroutines.

<pre>var <span id="DefaultTransport">DefaultTransport</span> <a href="#RoundTripper">RoundTripper</a> = &amp;<a href="#Transport">Transport</a>{
    <a href="#Transport.Proxy">Proxy</a>: <a href="#ProxyFromEnvironment">ProxyFromEnvironment</a>,
    <a href="#Transport.DialContext">DialContext</a>: (&amp;<a href="/net/">net</a>.<a href="/net/#Dialer">Dialer</a>{
        <a href="/net/#Dialer.Timeout">Timeout</a>:   30 * <a href="/time/">time</a>.<a href="/time/#Second">Second</a>,
        <a href="/net/#Dialer.KeepAlive">KeepAlive</a>: 30 * <a href="/time/">time</a>.<a href="/time/#Second">Second</a>,
        <a href="/net/#Dialer.DualStack">DualStack</a>: <a href="/builtin/#true">true</a>,
    }).<a href="#DialContext">DialContext</a>,
    <a href="#Transport.MaxIdleConns">MaxIdleConns</a>:          100,
    <a href="#Transport.IdleConnTimeout">IdleConnTimeout</a>:       90 * <a href="/time/">time</a>.<a href="/time/#Second">Second</a>,
    <a href="#Transport.TLSHandshakeTimeout">TLSHandshakeTimeout</a>:   10 * <a href="/time/">time</a>.<a href="/time/#Second">Second</a>,
    <a href="#Transport.ExpectContinueTimeout">ExpectContinueTimeout</a>: 1 * <a href="/time/">time</a>.<a href="/time/#Second">Second</a>,
}</pre>

DefaultTransport is the default implementation of Transport and is used by
DefaultClient. It establishes network connections as needed and caches them for
reuse by subsequent calls. It uses HTTP proxies as directed by the $HTTP_PROXY
and $NO_PROXY (or $http_proxy and $no_proxy) environment variables.

<h3 id="NewFileTransport">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/filetransport.go#L20">NewFileTransport</a>
    <a href="#NewFileTransport">¶</a></h3>
<pre>func NewFileTransport(fs <a href="#FileSystem">FileSystem</a>) <a href="#RoundTripper">RoundTripper</a></pre>

NewFileTransport returns a new RoundTripper, serving the provided FileSystem.
The returned RoundTripper ignores the URL host in its incoming requests, as well
as most other properties of the request.

The typical use case for NewFileTransport is to register the "file" protocol
with a Transport, as in:

    t := &http.Transport{}
    t.RegisterProtocol("file", http.NewFileTransport(http.Dir("/")))
    c := &http.Client{Transport: t}
    res, err := c.Get("file:///etc/passwd")
    ...

<h2 id="ServeMux">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2123">ServeMux</a>
    <a href="#ServeMux">¶</a></h2>
<pre>type ServeMux struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ServeMux is an HTTP request multiplexer. It matches the URL of each incoming
request against a list of registered patterns and calls the handler for the
pattern that most closely matches the URL.

Patterns name fixed, rooted paths, like "/favicon.ico", or rooted subtrees, like
"/images/" (note the trailing slash). Longer patterns take precedence over
shorter ones, so that if there are handlers registered for both "/images/" and
"/images/thumbnails/", the latter handler will be called for paths beginning
"/images/thumbnails/" and the former will receive requests for any other paths
in the "/images/" subtree.

Note that since a pattern ending in a slash names a rooted subtree, the pattern
"/" matches all paths not matched by other registered patterns, not just the URL
with Path == "/".

If a subtree has been registered and a request is received naming the subtree
root without its trailing slash, ServeMux redirects that request to the subtree
root (adding the trailing slash). This behavior can be overridden with a
separate registration for the path without the trailing slash. For example,
registering "/images/" causes ServeMux to redirect a request for "/images" to
"/images/", unless "/images" has been registered separately.

Patterns may optionally begin with a host name, restricting matches to URLs on
that host only. Host-specific patterns take precedence over general patterns, so
that a handler might register for the two patterns "/codesearch" and
"codesearch.google.com/" without also taking over requests for
"http://www.google.com/".

ServeMux also takes care of sanitizing the URL request path, redirecting any
request containing . or .. elements or repeated slashes to an equivalent,
cleaner URL.

<h3 id="NewServeMux">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2135">NewServeMux</a>
    <a href="#NewServeMux">¶</a></h3>
<pre>func NewServeMux() *<a href="#ServeMux">ServeMux</a></pre>

NewServeMux allocates and returns a new ServeMux.

<h3 id="ServeMux.Handle">func (*ServeMux) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2332">Handle</a>
    <a href="#ServeMux.Handle">¶</a></h3>
<pre>func (mux *<a href="#ServeMux">ServeMux</a>) Handle(pattern <a href="/builtin/#string">string</a>, handler <a href="#Handler">Handler</a>)</pre>

Handle registers the handler for the given pattern. If a handler already exists
for pattern, Handle panics.

<a id="exampleServeMux_Handle"></a>
Example:

    mux := http.NewServeMux()
    mux.Handle("/api/", apiHandler{})
    mux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
        // The "/" pattern matches everything, so we need to check
        // that we're at the root here.
        if req.URL.Path != "/" {
            http.NotFound(w, req)
            return
        }
        fmt.Fprintf(w, "Welcome to the home page!")
    })

<h3 id="ServeMux.HandleFunc">func (*ServeMux) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2357">HandleFunc</a>
    <a href="#ServeMux.HandleFunc">¶</a></h3>
<pre>func (mux *<a href="#ServeMux">ServeMux</a>) HandleFunc(pattern <a href="/builtin/#string">string</a>, handler func(<a href="#ResponseWriter">ResponseWriter</a>, *<a href="#Request">Request</a>))</pre>

HandleFunc registers the handler function for the given pattern.

<h3 id="ServeMux.Handler">func (*ServeMux) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2262">Handler</a>
    <a href="#ServeMux.Handler">¶</a></h3>
<pre>func (mux *<a href="#ServeMux">ServeMux</a>) Handler(r *<a href="#Request">Request</a>) (h <a href="#Handler">Handler</a>, pattern <a href="/builtin/#string">string</a>)</pre>

Handler returns the handler to use for the given request, consulting r.Method,
r.Host, and r.URL.Path. It always returns a non-nil handler. If the path is not
in its canonical form, the handler will be an internally-generated handler that
redirects to the canonical path. If the host contains a port, it is ignored when
matching handlers.

The path and host are used unchanged for CONNECT requests.

Handler also returns the registered pattern that matches the request or, in the
case of internally-generated redirects, the pattern that will match after
following the redirect.

If there is no registered handler that applies to the request, Handler returns a
``page not found'' handler and an empty pattern.

<h3 id="ServeMux.ServeHTTP">func (*ServeMux) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2318">ServeHTTP</a>
    <a href="#ServeMux.ServeHTTP">¶</a></h3>
<pre>func (mux *<a href="#ServeMux">ServeMux</a>) ServeHTTP(w <a href="#ResponseWriter">ResponseWriter</a>, r *<a href="#Request">Request</a>)</pre>

ServeHTTP dispatches the request to the handler whose pattern most closely
matches the request URL.

<h2 id="Server">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2399">Server</a>
    <a href="#Server">¶</a></h2>
<pre>type Server struct {
<span id="Server.Addr"></span>    Addr    <a href="/builtin/#string">string</a>  <span class="comment">// TCP address to listen on, &#34;:http&#34; if empty</span>
<span id="Server.Handler"></span>    Handler <a href="#Handler">Handler</a> <span class="comment">// handler to invoke, http.DefaultServeMux if nil</span>

<span id="Server.TLSConfig"></span>    <span class="comment">// TLSConfig optionally provides a TLS configuration for use</span>
    <span class="comment">// by ServeTLS and ListenAndServeTLS. Note that this value is</span>
    <span class="comment">// cloned by ServeTLS and ListenAndServeTLS, so it&#39;s not</span>
    <span class="comment">// possible to modify the configuration with methods like</span>
    <span class="comment">// tls.Config.SetSessionTicketKeys. To use</span>
    <span class="comment">// SetSessionTicketKeys, use Server.Serve with a TLS Listener</span>
    <span class="comment">// instead.</span>
    TLSConfig *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#Config">Config</a>

<span id="Server.ReadTimeout"></span>    <span class="comment">// ReadTimeout is the maximum duration for reading the entire</span>
    <span class="comment">// request, including the body.</span>
    <span class="comment">//</span>
    <span class="comment">// Because ReadTimeout does not let Handlers make per-request</span>
    <span class="comment">// decisions on each request body&#39;s acceptable deadline or</span>
    <span class="comment">// upload rate, most users will prefer to use</span>
<span id="Server.ReadHeaderTimeout"></span>    <span class="comment">// ReadHeaderTimeout. It is valid to use them both.</span>
    ReadTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

    <span class="comment">// ReadHeaderTimeout is the amount of time allowed to read</span>
    <span class="comment">// request headers. The connection&#39;s read deadline is reset</span>
    <span class="comment">// after reading the headers and the Handler can decide what</span>
    <span class="comment">// is considered too slow for the body.</span>
    ReadHeaderTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Server.WriteTimeout"></span>    <span class="comment">// WriteTimeout is the maximum duration before timing out</span>
    <span class="comment">// writes of the response. It is reset whenever a new</span>
    <span class="comment">// request&#39;s header is read. Like ReadTimeout, it does not</span>
    <span class="comment">// let Handlers make decisions on a per-request basis.</span>
    WriteTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Server.IdleTimeout"></span>    <span class="comment">// IdleTimeout is the maximum amount of time to wait for the</span>
    <span class="comment">// next request when keep-alives are enabled. If IdleTimeout</span>
    <span class="comment">// is zero, the value of ReadTimeout is used. If both are</span>
    <span class="comment">// zero, ReadHeaderTimeout is used.</span>
    IdleTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Server.MaxHeaderBytes"></span>    <span class="comment">// MaxHeaderBytes controls the maximum number of bytes the</span>
    <span class="comment">// server will read parsing the request header&#39;s keys and</span>
    <span class="comment">// values, including the request line. It does not limit the</span>
    <span class="comment">// size of the request body.</span>
    <span class="comment">// If zero, DefaultMaxHeaderBytes is used.</span>
    MaxHeaderBytes <a href="/builtin/#int">int</a>

<span id="Server.TLSNextProto"></span>    <span class="comment">// TLSNextProto optionally specifies a function to take over</span>
    <span class="comment">// ownership of the provided TLS connection when an NPN/ALPN</span>
    <span class="comment">// protocol upgrade has occurred. The map key is the protocol</span>
    <span class="comment">// name negotiated. The Handler argument should be used to</span>
    <span class="comment">// handle HTTP requests and will initialize the Request&#39;s TLS</span>
    <span class="comment">// and RemoteAddr if not already set. The connection is</span>
    <span class="comment">// automatically closed when the function returns.</span>
    <span class="comment">// If TLSNextProto is not nil, HTTP/2 support is not enabled</span>
    <span class="comment">// automatically.</span>
    TLSNextProto map[<a href="/builtin/#string">string</a>]func(*<a href="#Server">Server</a>, *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#Conn">Conn</a>, <a href="#Handler">Handler</a>)

<span id="Server.ConnState"></span>    <span class="comment">// ConnState specifies an optional callback function that is</span>
    <span class="comment">// called when a client connection changes state. See the</span>
    <span class="comment">// ConnState type and associated constants for details.</span>
    ConnState func(<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, <a href="#ConnState">ConnState</a>)

<span id="Server.ErrorLog"></span>    <span class="comment">// ErrorLog specifies an optional logger for errors accepting</span>
    <span class="comment">// connections, unexpected behavior from handlers, and</span>
    <span class="comment">// underlying FileSystem errors.</span>
    <span class="comment">// If nil, logging is done via the log package&#39;s standard logger.</span>
    ErrorLog *<a href="/log/">log</a>.<a href="/log/#Logger">Logger</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Server defines parameters for running an HTTP server. The zero value for
Server is a valid configuration.

<h3 id="Server.Close">func (*Server) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2514">Close</a>
    <a href="#Server.Close">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) Close() <a href="/builtin/#error">error</a></pre>

Close immediately closes all active net.Listeners and any connections in state
StateNew, StateActive, or StateIdle. For a graceful shutdown, use Shutdown.

Close does not attempt to close (and does not even know about) any hijacked
connections, such as WebSockets.

Close returns any error returned from closing the Server's underlying
Listener(s).

<h3 id="Server.ListenAndServe">func (*Server) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2692">ListenAndServe</a>
    <a href="#Server.ListenAndServe">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) ListenAndServe() <a href="/builtin/#error">error</a></pre>

ListenAndServe listens on the TCP network address srv.Addr and then calls Serve
to handle requests on incoming connections. Accepted connections are configured
to enable TCP keep-alives. If srv.Addr is blank, ":http" is used. ListenAndServe
always returns a non-nil error.

<h3 id="Server.ListenAndServeTLS">func (*Server) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L3009">ListenAndServeTLS</a>
    <a href="#Server.ListenAndServeTLS">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) ListenAndServeTLS(certFile, keyFile <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

ListenAndServeTLS listens on the TCP network address srv.Addr and then calls
Serve to handle requests on incoming TLS connections. Accepted connections are
configured to enable TCP keep-alives.

Filenames containing a certificate and matching private key for the server must
be provided if neither the Server's TLSConfig.Certificates nor
TLSConfig.GetCertificate are populated. If the certificate is signed by a
certificate authority, the certFile should be the concatenation of the server's
certificate, any intermediates, and the CA's certificate.

If srv.Addr is blank, ":https" is used.

ListenAndServeTLS always returns a non-nil error.

<h3 id="Server.RegisterOnShutdown">func (*Server) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2583">RegisterOnShutdown</a>
    <a href="#Server.RegisterOnShutdown">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) RegisterOnShutdown(f func())</pre>

RegisterOnShutdown registers a function to call on Shutdown. This can be used to
gracefully shutdown connections that have undergone NPN/ALPN protocol upgrade or
that have been hijacked. This function should start protocol-specific graceful
shutdown, but should not wait for shutdown to complete.

<h3 id="Server.Serve">func (*Server) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2743">Serve</a>
    <a href="#Server.Serve">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) Serve(l <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>) <a href="/builtin/#error">error</a></pre>

Serve accepts incoming connections on the Listener l, creating a new service
goroutine for each. The service goroutines read requests and then call
srv.Handler to reply to them.

For HTTP/2 support, srv.TLSConfig should be initialized to the provided
listener's TLS Config before calling Serve. If srv.TLSConfig is non-nil and
doesn't include the string "h2" in Config.NextProtos, HTTP/2 support is not
enabled.

Serve always returns a non-nil error. After Shutdown or Close, the returned
error is ErrServerClosed.

<h3 id="Server.ServeTLS">func (*Server) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2806">ServeTLS</a>
    <a href="#Server.ServeTLS">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) ServeTLS(l <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>, certFile, keyFile <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

ServeTLS accepts incoming connections on the Listener l, creating a new service
goroutine for each. The service goroutines read requests and then call
srv.Handler to reply to them.

Additionally, files containing a certificate and matching private key for the
server must be provided if neither the Server's TLSConfig.Certificates nor
TLSConfig.GetCertificate are populated.. If the certificate is signed by a
certificate authority, the certFile should be the concatenation of the server's
certificate, any intermediates, and the CA's certificate.

For HTTP/2 support, srv.TLSConfig should be initialized to the provided
listener's TLS Config before calling ServeTLS. If srv.TLSConfig is non-nil and
doesn't include the string "h2" in Config.NextProtos, HTTP/2 support is not
enabled.

ServeTLS always returns a non-nil error. After Shutdown or Close, the returned
error is ErrServerClosed.

<h3 id="Server.SetKeepAlivesEnabled">func (*Server) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2889">SetKeepAlivesEnabled</a>
    <a href="#Server.SetKeepAlivesEnabled">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) SetKeepAlivesEnabled(v <a href="/builtin/#bool">bool</a>)</pre>

SetKeepAlivesEnabled controls whether HTTP keep-alives are enabled. By default,
keep-alives are always enabled. Only very resource-constrained environments or
servers in the process of shutting down should disable them.

<h3 id="Server.Shutdown">func (*Server) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/server.go#L2552">Shutdown</a>
    <a href="#Server.Shutdown">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) Shutdown(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>) <a href="/builtin/#error">error</a></pre>

Shutdown gracefully shuts down the server without interrupting any active
connections. Shutdown works by first closing all open listeners, then closing
all idle connections, and then waiting indefinitely for connections to return to
idle and then shut down. If the provided context expires before the shutdown is
complete, Shutdown returns the context's error, otherwise it returns any error
returned from closing the Server's underlying Listener(s).

When Shutdown is called, Serve, ListenAndServe, and ListenAndServeTLS
immediately return ErrServerClosed. Make sure the program doesn't exit and waits
instead for Shutdown to return.

Shutdown does not attempt to close nor wait for hijacked connections such as
WebSockets. The caller of Shutdown should separately notify such long-lived
connections of shutdown and wait for them to close, if desired. See
RegisterOnShutdown for a way to register shutdown notification functions.

<a id="exampleServer_Shutdown"></a>
Example:

    var srv http.Server

    idleConnsClosed := make(chan struct{})
    go func() {
        sigint := make(chan os.Signal, 1)
        signal.Notify(sigint, os.Interrupt)
        <-sigint

        // We received an interrupt signal, shut down.
        if err := srv.Shutdown(context.Background()); err != nil {
            // Error from closing listeners, or context timeout:
            log.Printf("HTTP server Shutdown: %v", err)
        }
        close(idleConnsClosed)
    }()

    if err := srv.ListenAndServe(); err != http.ErrServerClosed {
        // Error starting or closing listener:
        log.Printf("HTTP server ListenAndServe: %v", err)
    }

    <-idleConnsClosed

<h2 id="Transport">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/transport.go#L75">Transport</a>
    <a href="#Transport">¶</a></h2>
<pre>type Transport struct {

<span id="Transport.Proxy"></span>    <span class="comment">// Proxy specifies a function to return a proxy for a given</span>
    <span class="comment">// Request. If the function returns a non-nil error, the</span>
    <span class="comment">// request is aborted with the provided error.</span>
    <span class="comment">//</span>
    <span class="comment">// The proxy type is determined by the URL scheme. &#34;http&#34;</span>
    <span class="comment">// and &#34;socks5&#34; are supported. If the scheme is empty,</span>
    <span class="comment">// &#34;http&#34; is assumed.</span>
    <span class="comment">//</span>
    <span class="comment">// If Proxy is nil or returns a nil *URL, no proxy is used.</span>
    Proxy func(*<a href="#Request">Request</a>) (*<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, <a href="/builtin/#error">error</a>)

<span id="Transport.DialContext"></span>    <span class="comment">// DialContext specifies the dial function for creating unencrypted TCP connections.</span>
    <span class="comment">// If DialContext is nil (and the deprecated Dial below is also nil),</span>
    <span class="comment">// then the transport dials using package net.</span>
    DialContext func(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, network, addr <a href="/builtin/#string">string</a>) (<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, <a href="/builtin/#error">error</a>)

<span id="Transport.Dial"></span>    <span class="comment">// Dial specifies the dial function for creating unencrypted TCP connections.</span>
    <span class="comment">//</span>
    <span class="comment">// Deprecated: Use DialContext instead, which allows the transport</span>
    <span class="comment">// to cancel dials as soon as they are no longer needed.</span>
    <span class="comment">// If both are set, DialContext takes priority.</span>
    Dial func(network, addr <a href="/builtin/#string">string</a>) (<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, <a href="/builtin/#error">error</a>)

<span id="Transport.DialTLS"></span>    <span class="comment">// DialTLS specifies an optional dial function for creating</span>
    <span class="comment">// TLS connections for non-proxied HTTPS requests.</span>
    <span class="comment">//</span>
    <span class="comment">// If DialTLS is nil, Dial and TLSClientConfig are used.</span>
    <span class="comment">//</span>
    <span class="comment">// If DialTLS is set, the Dial hook is not used for HTTPS</span>
    <span class="comment">// requests and the TLSClientConfig and TLSHandshakeTimeout</span>
    <span class="comment">// are ignored. The returned net.Conn is assumed to already be</span>
    <span class="comment">// past the TLS handshake.</span>
    DialTLS func(network, addr <a href="/builtin/#string">string</a>) (<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, <a href="/builtin/#error">error</a>)

<span id="Transport.TLSClientConfig"></span>    <span class="comment">// TLSClientConfig specifies the TLS configuration to use with</span>
    <span class="comment">// tls.Client.</span>
    <span class="comment">// If nil, the default configuration is used.</span>
    <span class="comment">// If non-nil, HTTP/2 support may not be enabled by default.</span>
    TLSClientConfig *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#Config">Config</a>

<span id="Transport.TLSHandshakeTimeout"></span>    <span class="comment">// TLSHandshakeTimeout specifies the maximum amount of time waiting to</span>
    <span class="comment">// wait for a TLS handshake. Zero means no timeout.</span>
    TLSHandshakeTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Transport.DisableKeepAlives"></span>    <span class="comment">// DisableKeepAlives, if true, prevents re-use of TCP connections</span>
    <span class="comment">// between different HTTP requests.</span>
    DisableKeepAlives <a href="/builtin/#bool">bool</a>

<span id="Transport.DisableCompression"></span>    <span class="comment">// DisableCompression, if true, prevents the Transport from</span>
    <span class="comment">// requesting compression with an &#34;Accept-Encoding: gzip&#34;</span>
    <span class="comment">// request header when the Request contains no existing</span>
    <span class="comment">// Accept-Encoding value. If the Transport requests gzip on</span>
    <span class="comment">// its own and gets a gzipped response, it&#39;s transparently</span>
    <span class="comment">// decoded in the Response.Body. However, if the user</span>
    <span class="comment">// explicitly requested gzip it is not automatically</span>
    <span class="comment">// uncompressed.</span>
    DisableCompression <a href="/builtin/#bool">bool</a>

<span id="Transport.MaxIdleConns"></span>    <span class="comment">// MaxIdleConns controls the maximum number of idle (keep-alive)</span>
    <span class="comment">// connections across all hosts. Zero means no limit.</span>
    MaxIdleConns <a href="/builtin/#int">int</a>

<span id="Transport.MaxIdleConnsPerHost"></span>    <span class="comment">// MaxIdleConnsPerHost, if non-zero, controls the maximum idle</span>
    <span class="comment">// (keep-alive) connections to keep per-host. If zero,</span>
    <span class="comment">// DefaultMaxIdleConnsPerHost is used.</span>
    MaxIdleConnsPerHost <a href="/builtin/#int">int</a>

<span id="Transport.IdleConnTimeout"></span>    <span class="comment">// IdleConnTimeout is the maximum amount of time an idle</span>
    <span class="comment">// (keep-alive) connection will remain idle before closing</span>
    <span class="comment">// itself.</span>
    <span class="comment">// Zero means no limit.</span>
    IdleConnTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Transport.ResponseHeaderTimeout"></span>    <span class="comment">// ResponseHeaderTimeout, if non-zero, specifies the amount of</span>
    <span class="comment">// time to wait for a server&#39;s response headers after fully</span>
    <span class="comment">// writing the request (including its body, if any). This</span>
    <span class="comment">// time does not include the time to read the response body.</span>
    ResponseHeaderTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Transport.ExpectContinueTimeout"></span>    <span class="comment">// ExpectContinueTimeout, if non-zero, specifies the amount of</span>
    <span class="comment">// time to wait for a server&#39;s first response headers after fully</span>
    <span class="comment">// writing the request headers if the request has an</span>
    <span class="comment">// &#34;Expect: 100-continue&#34; header. Zero means no timeout and</span>
    <span class="comment">// causes the body to be sent immediately, without</span>
    <span class="comment">// waiting for the server to approve.</span>
    <span class="comment">// This time does not include the time to send the request header.</span>
    ExpectContinueTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Transport.TLSNextProto"></span>    <span class="comment">// TLSNextProto specifies how the Transport switches to an</span>
    <span class="comment">// alternate protocol (such as HTTP/2) after a TLS NPN/ALPN</span>
    <span class="comment">// protocol negotiation. If Transport dials an TLS connection</span>
    <span class="comment">// with a non-empty protocol name and TLSNextProto contains a</span>
    <span class="comment">// map entry for that key (such as &#34;h2&#34;), then the func is</span>
    <span class="comment">// called with the request&#39;s authority (such as &#34;example.com&#34;</span>
    <span class="comment">// or &#34;example.com:1234&#34;) and the TLS connection. The function</span>
    <span class="comment">// must return a RoundTripper that then handles the request.</span>
    <span class="comment">// If TLSNextProto is not nil, HTTP/2 support is not enabled</span>
    <span class="comment">// automatically.</span>
    TLSNextProto map[<a href="/builtin/#string">string</a>]func(authority <a href="/builtin/#string">string</a>, c *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#Conn">Conn</a>) <a href="#RoundTripper">RoundTripper</a>

<span id="Transport.ProxyConnectHeader"></span>    <span class="comment">// ProxyConnectHeader optionally specifies headers to send to</span>
    <span class="comment">// proxies during CONNECT requests.</span>
    ProxyConnectHeader <a href="#Header">Header</a>

<span id="Transport.MaxResponseHeaderBytes"></span>    <span class="comment">// MaxResponseHeaderBytes specifies a limit on how many</span>
    <span class="comment">// response bytes are allowed in the server&#39;s response</span>
    <span class="comment">// header.</span>
    <span class="comment">//</span>
    <span class="comment">// Zero means to use a default limit.</span>
    MaxResponseHeaderBytes <a href="/builtin/#int64">int64</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Transport is an implementation of RoundTripper that supports HTTP, HTTPS, and
HTTP proxies (for either HTTP or HTTPS with CONNECT).

By default, Transport caches connections for future re-use. This may leave many
open connections when accessing many hosts. This behavior can be managed using
Transport's CloseIdleConnections method and the MaxIdleConnsPerHost and
DisableKeepAlives fields.

Transports should be reused instead of created as needed. Transports are safe
for concurrent use by multiple goroutines.

A Transport is a low-level primitive for making HTTP and HTTPS requests. For
high-level functionality, such as cookies and redirects, see Client.

Transport uses HTTP/1.1 for HTTP URLs and either HTTP/1.1 or HTTP/2 for HTTPS
URLs, depending on whether the server supports HTTP/2, and how the Transport is
configured. The DefaultTransport supports HTTP/2. To explicitly enable HTTP/2 on
a transport, use golang.org/x/net/http2 and call ConfigureTransport. See the
package docs for more about HTTP/2.

The Transport will send CONNECT requests to a proxy for its own use when
processing HTTPS requests, but Transport should generally not be used to send a
CONNECT request. That is, the Request passed to the RoundTrip method should not
have a Method of "CONNECT", as Go's HTTP/1.x implementation does not support
full-duplex request bodies being written while the response body is streamed.
Go's HTTP/2 implementation does support full duplex, but many CONNECT proxies
speak HTTP/1.x.

<h3 id="Transport.CancelRequest">func (*Transport) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/transport.go#L548">CancelRequest</a>
    <a href="#Transport.CancelRequest">¶</a></h3>
<pre>func (t *<a href="#Transport">Transport</a>) CancelRequest(req *<a href="#Request">Request</a>)</pre>

CancelRequest cancels an in-flight request by closing its connection.
CancelRequest should only be called after RoundTrip has returned.

Deprecated: Use Request.WithContext to create a request with a cancelable
context instead. CancelRequest cannot cancel HTTP/2 requests.

<h3 id="Transport.CloseIdleConnections">func (*Transport) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/transport.go#L523">CloseIdleConnections</a>
    <a href="#Transport.CloseIdleConnections">¶</a></h3>
<pre>func (t *<a href="#Transport">Transport</a>) CloseIdleConnections()</pre>

CloseIdleConnections closes any connections which were previously connected from
previous requests but are now sitting idle in a "keep-alive" state. It does not
interrupt any connections currently in use.

<h3 id="Transport.RegisterProtocol">func (*Transport) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/transport.go#L504">RegisterProtocol</a>
    <a href="#Transport.RegisterProtocol">¶</a></h3>
<pre>func (t *<a href="#Transport">Transport</a>) RegisterProtocol(scheme <a href="/builtin/#string">string</a>, rt <a href="#RoundTripper">RoundTripper</a>)</pre>

RegisterProtocol registers a new protocol with scheme. The Transport will pass
requests using the given scheme to rt. It is rt's responsibility to simulate
HTTP request semantics.

RegisterProtocol can be used by other packages to provide implementations of
protocol schemes like "ftp" or "file".

If rt.RoundTrip returns ErrSkipAltProtocol, the Transport will handle the
RoundTrip itself for that one request, as if the protocol were not registered.

<h3 id="Transport.RoundTrip">func (*Transport) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/transport.go#L340">RoundTrip</a>
    <a href="#Transport.RoundTrip">¶</a></h3>
<pre>func (t *<a href="#Transport">Transport</a>) RoundTrip(req *<a href="#Request">Request</a>) (*<a href="#Response">Response</a>, <a href="/builtin/#error">error</a>)</pre>

RoundTrip implements the RoundTripper interface.

For higher-level HTTP client support (such as handling of cookies and
redirects), see Get, Post, and the Client type.

## Subdirectories
- [..](..)
- [cgi](cgi/)
- [cookiejar](cookiejar/)
- [fcgi](fcgi/)
- [httptest](httptest/)
- [httptrace](httptrace/)
- [httputil](httputil/)
- [pprof](pprof/)
