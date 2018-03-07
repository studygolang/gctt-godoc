version: 1.10
## package http

  `import "net/http"`

##概述

http 包提供 HTTP 客户端和服务器的实现。

使用 Get、Head、Post 和 PostForm 发起 HTTP（或 HTTPS）请求：

    resp, err := http.Get("http://example.com/")
    ...
    resp, err := http.Post("http://example.com/upload", "image/jpeg", &buf)
    ...
    resp, err := http.PostForm("http://example.com/form",
    	url.Values{"key": {"Value"}, "id": {"123"}})

客户端必须在使用后手动关闭响应的 body：

    resp, err := http.Get("http://example.com/")
    if err != nil {
    	// handle error
    }
    defer resp.Body.Close()
    body, err := ioutil.ReadAll(resp.Body)
    // ...

手动创建 Client 可以自定义 HTTP 请求头，重定向策略和其他设置：

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

手动创建 Transport 可以自定义代理、TLS 、keep-alive、压缩和其他配置：

    tr := &http.Transport{
    	MaxIdleConns:       10,
    	IdleConnTimeout:    30 * time.Second,
    	DisableCompression: true,
    }
    client := &http.Client{Transport: tr}
    resp, err := client.Get("https://example.com")

Client 和 Transport 都可以安全的被多个 goroutine 同时使用，所以我们应该只创建一个实例并多次复用。

ListenAndServe 启动指定地址和处理函数的 HTTP 服务器。处理函数通常为 nil（这时默认使用 DefaultServeMux）。Handle 和 HandleFunc 为 DefaultServeMux 添加处理函数：

    http.Handle("/foo", fooHandler)

    http.HandleFunc("/bar", func(w http.ResponseWriter, r *http.Request) {
    	fmt.Fprintf(w, "Hello, %q", html.EscapeString(r.URL.Path))
    })
    
    log.Fatal(http.ListenAndServe(":8080", nil))

通过创建自定义 Server 可以更好的控制服务器的行为：

    s := &http.Server{
    	Addr:           ":8080",
    	Handler:        myHandler,
    	ReadTimeout:    10 * time.Second,
    	WriteTimeout:   10 * time.Second,
    	MaxHeaderBytes: 1 << 20,
    }
    log.Fatal(s.ListenAndServe())

从 Go 1.6 开始 http 包在使用 HTTPS 时支持 HTTP/2 协议。如果程序需要禁用 HTTPS 可以设置 Transport.TLSNextProto（客户端）或 Server.TLSNextProto（服务端）为一个空 map（非 nil）。或者使用目前支持的以下 GODEBUG 环境变量：

    GODEBUG=http2client=0  # 禁用 HTTP/2 客户端支持
    GODEBUG=http2server=0  # 禁用 HTTP/2 服务端支持
    GODEBUG=http2debug=1   # 开启 HTTP/2 调试日志
    GODEBUG=http2debug=2   # 更多参数

不是所有的 Go API 都兼容 GODEBUG 环境变量。如果在使用前有任何问题请在 https://golang.org/s/http2bug 提出。

http 包的 Transport 和 Server 都可以通过简单配置自动支持 HTTP/2。如果需要配置更复杂的 HTTP/2 属性、使用 HTTP/2 的底层功能或想用 Go 的 http2 较新版本的包可以直接使用 "golang.org/x/net/http2" 包中的 ConfigureTransport 和 ConfigureServer。通过 golang.org/x/net/http2 进行手动配置会覆盖 net/http 包的内建 HTTP/2 支持。

## 索引

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

### 例子

- [FileServer](#exampleFileServer)
- [FileServer (StripPrefix)](#exampleFileServer_stripPrefix)
- [Get](#exampleGet)
- [Hijacker](#exampleHijacker)
- [ResponseWriter (Trailers)](#exampleResponseWriter_trailers)
- [ServeMux.Handle](#exampleServeMux_Handle)
- [Server.Shutdown](#exampleServer_Shutdown)
- [StripPrefix](#exampleStripPrefix)

### 文件

 [client.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go) [cookie.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/cookie.go) [doc.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/doc.go) [filetransport.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/filetransport.go) [fs.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/fs.go) [h2_bundle.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/h2_bundle.go) [header.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/header.go) [http.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/http.go) [jar.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/jar.go) [method.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/method.go) [request.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go) [response.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/response.go) [server.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go) [sniff.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/sniff.go) [status.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/status.go) [transfer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/transfer.go) [transport.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/transport.go)

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

常见的 HTTP 方法。

除非有特别声明否则所有方法都定义在 RFC 7231 第 4.3 章。

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

HTTP 状态码依据 IANA 注册。请看：http://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml

<pre>const <span id="DefaultMaxHeaderBytes">DefaultMaxHeaderBytes</span> = 1 &lt;&lt; 20 <span class="comment">// 1 MB</span>
</pre>

DefaultMaxHeaderBytes 代表 HTTP 请求头允许的最大字节数。该属性可以用 Server.MaxHeaderBytes 覆盖。

<pre>const <span id="DefaultMaxIdleConnsPerHost">DefaultMaxIdleConnsPerHost</span> = 2</pre>

DefaultMaxIdleConnsPerHost 是 Transport.MaxIdleConnsPerHost 的默认值。

<pre>const <span id="TimeFormat">TimeFormat</span> = &#34;Mon, 02 Jan 2006 15:04:05 GMT&#34;</pre>

TimeFormat 是 HTTP 头里面的时间格式。它和 time.RFC1123 相似只不过固定使用 GMT 时区。被格式化的时间必须是 UTC 格式才能保证产生正确的格式化结果。

关于解析这种时间格式，请看 ParseTime。

<pre>const <span id="TrailerPrefix">TrailerPrefix</span> = &#34;Trailer:&#34;</pre>

TrailerPrefix 是 ResponseWriter.Header map 的键的魔法前缀，如果存在，就意味着这个 map 实体其实是响应 trailer 而不是响应头。ServeHTTP 调用完成后会去掉这个前缀并把送入 trailer。

这个机制仅在写入头部时还不知道 trailer 的相关信息的情况下使用。如果 trailer 是固定的或者在写入头部时就可以确定，推荐使用正常的 Go trailer 机制：

    https://golang.org/pkg/net/http/#ResponseWriter
    https://golang.org/pkg/net/http/#example_ResponseWriter_trailers

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span class="comment">// 当 Pusher 的实现提示不支持 HTTP/2 的 Push 方法时返回 ErrNotSupported/span>
    <span id="ErrNotSupported">ErrNotSupported</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;feature not supported&#34;}
    
    <span class="comment">// 当服务器返回了一个 Trailer 头但是没有分块数据的时候，Transport 返回  ErrUnexpectedTrailer</span>
    <span id="ErrUnexpectedTrailer">ErrUnexpectedTrailer</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;trailer header without chunked transfer encoding&#34;}
    
    <span class="comment">// 当请求的 Content-Type 不包含 boundary 参数时 Request.MultipartReader 返回 ErrMissingBoundary</span>
    <span id="ErrMissingBoundary">ErrMissingBoundary</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;no multipart boundary param in Content-Type&#34;}
    
    <span class="comment">// 当请求的 Content-Type 不是 multipart/form-data 时 Request.MultipartReader 返回 ErrNotMultipart</span>
    <span id="ErrNotMultipart">ErrNotMultipart</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;request Content-Type isn&#39;t multipart/form-data&#34;}
    
    <span class="comment">// 弃用: ErrHeaderTooLong 不再使用。</span>
    <span id="ErrHeaderTooLong">ErrHeaderTooLong</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;header too long&#34;}
    <span class="comment">// 弃用: ErrShortBody 不再使用。</span>
    <span id="ErrShortBody">ErrShortBody</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;entity body too short&#34;}
    <span class="comment">// 弃用: ErrMissingContentLength 不再使用。</span>
    <span id="ErrMissingContentLength">ErrMissingContentLength</span> = &amp;<a href="#ProtocolError">ProtocolError</a>{&#34;missing ContentLength in HEAD response&#34;}
)</pre>


<pre>var (
    <span class="comment">// 当 HTTP 方法或响应码不包含请求体时，ResponseWriter.Write 返回 ErrBodyNotAllowed</span>
    <span id="ErrBodyNotAllowed">ErrBodyNotAllowed</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: request method or response status code does not allow body&#34;)
    
    <span class="comment">// 当底层链接被 Hijacker 劫持的时候 ResponseWriter.Write 返回 ErrHijacked。</span>
    <span class="comment">// 在劫持链接中写入0字节也会返回 ErrHijacked。</span>
    <span id="ErrHijacked">ErrHijacked</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: connection has been hijacked&#34;)
    
    <span class="comment">// 当处理函数写入了 Content-Length 响应头却企图发送字节大于该值的内容时返回 ErrContentLength。</span>
    <span id="ErrContentLength">ErrContentLength</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: wrote more than the declared Content-Length&#34;)
    
    <span class="comment">// 弃用: ErrWriteAfterFlush 不再使用。</span>
    <span id="ErrWriteAfterFlush">ErrWriteAfterFlush</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;unused&#34;)
)</pre>

HTTP 服务器使用的错误。

<pre>var (
    <span class="comment">// ServerContextKey 是一个 context key。它能在 HTTP 处理函数中获取其对应的服务器实例。对应值类型为 *Server。</span>
    <span id="ServerContextKey">ServerContextKey</span> = &amp;contextKey{&#34;http-server&#34;}
    
    <span class="comment">// LocalAddrContextKey 是一个 context key。它能在 HTTP 处理函数中获取链接到达的本地地址。</span>
    <span class="comment">// 对应值类型为 net.Addr。</span>
    <span id="LocalAddrContextKey">LocalAddrContextKey</span> = &amp;contextKey{&#34;local-addr&#34;}
)</pre>


<pre>var <span id="DefaultClient">DefaultClient</span> = &amp;<a href="#Client">Client</a>{}</pre>

DefaultClient 是 http 包中的默认 Client， Get、Head 和 Post 方法都使用这个 Client。

<pre>var <span id="DefaultServeMux">DefaultServeMux</span> = &amp;defaultServeMux</pre>

DefaultServeMux 是 Serve 默认使用的 ServeMux。

<pre>var <span id="ErrAbortHandler">ErrAbortHandler</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;net/http: abort Handler&#34;)</pre>

ErrAbortHandler 会监听处理函数中的 panic 事件。当 ServeHTTP 方法发生 panic 并中止响应的时候 ErrAbortHandler 会记录栈信息到服务器的错误日志。

<pre>var <span id="ErrBodyReadAfterClose">ErrBodyReadAfterClose</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: invalid Read on closed Body&#34;)</pre>

ErrBodyReadAfterClose 会在读取一个已经关闭的 Request 或 Response 的 Body 时返回。比如在 HTTP 处理函数在调用 ResponseWriter 的 WriteHeader 或 Write 后读取 Body。

<pre>var <span id="ErrHandlerTimeout">ErrHandlerTimeout</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: Handler timeout&#34;)</pre>

在超时以后调用 ResponseWriter 的 Write 返回 ErrHandlerTimeout。

<pre>var <span id="ErrLineTooLong">ErrLineTooLong</span> = <a href="/net/http/internal/">internal</a>.<a href="/net/http/internal/#ErrLineTooLong">ErrLineTooLong</a></pre>

ErrLineTooLong 读取请求体或响应体遇到不正确的分块信息时返回 ErrLineTooLong。

<pre>var <span id="ErrMissingFile">ErrMissingFile</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: no such file&#34;)</pre>

FromFile 在文件名不存在于请求中或不是文件名时返回 ErrMissingFile。

<pre>var <span id="ErrNoCookie">ErrNoCookie</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: named cookie not present&#34;)</pre>

请求的 Cookie 方法没有找到 cookie 时返回 ErrNoCookie。

<pre>var <span id="ErrNoLocation">ErrNoLocation</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: no Location header in response&#34;)</pre>

没有 Location 头部的时候调用响应的 Location 方法返回 ErrNoLocation。

<pre>var <span id="ErrServerClosed">ErrServerClosed</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;http: Server closed&#34;)</pre>

在服务器 Close 或者 Shutdown 以后调用 Serve，ServeTLS，ListenAndServe 和 ListenAndServeTLS 方法返回 ErrServerClosed。

<pre>var <span id="ErrSkipAltProtocol">ErrSkipAltProtocol</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;net/http: skip alternate protocol&#34;)</pre>

ErrSkipAltProtocol 是一个被 Transport.RegisterProtocol 定义的监听错误。

<pre>var <span id="ErrUseLastResponse">ErrUseLastResponse</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;net/http: use last response&#34;)</pre>

Client.CheckRedirect 返回 ErrUseLastResponse 和控制重定向过程有关。如果返回 ErrUseLastResponse，就不会发送下一个请求，并返回最近一次的响应（不关闭其 body）。

<pre>var <span id="NoBody">NoBody</span> = noBody{}</pre>

NoBody 是一个 0 字节的 io.ReadCloser。Read 一直返回 EOF，Close 一直返回 nil。他可以被用来明确表示一个 0 字节的外部请求。另一种相对简单的方法是直接把 Body 设置为 nil。

<h2 id="CanonicalHeaderKey">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/header.go#L166">CanonicalHeaderKey</a>
    <a href="#CanonicalHeaderKey">¶</a></h2>
<pre>func CanonicalHeaderKey(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

CanonicalHeaderKey 返回头部键 s 的权威格式。它会把 s 的首字母和连字符后的字母转换成大写，其他部分小写。例如 "accept-encoding" 会变成 "Accept-Encoding"。如果含有空格或者非法字节不对 s 做变动。

<h2 id="DetectContentType">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/sniff.go#L11">DetectContentType</a>
    <a href="#DetectContentType">¶</a></h2>
<pre>func DetectContentType(data []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#string">string</a></pre>

DetectContentType 实现了 http://mimesniff.spec.whatwg.org/ 所述判断数据 Content-Type 类型的算法。该算法会分析数据的前 512 个字节。DetectContentType 一直返回有效的 MIME 类型。如果找不到一个适合的类型会返回 `application/octet-stream`。

<h2 id="Error">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L1946">Error</a>
    <a href="#Error">¶</a></h2>
<pre>func Error(w <a href="#ResponseWriter">ResponseWriter</a>, error <a href="/builtin/#string">string</a>, code <a href="/builtin/#int">int</a>)</pre>

Error 以指定错误信息和 HTTP 状态码响应客户端请求。它不会以其他方式结束请求。在此之后不能再向 w 中写入信息。错误信息必须是纯文本。

<h2 id="Handle">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2364">Handle</a>
    <a href="#Handle">¶</a></h2>
<pre>func Handle(pattern <a href="/builtin/#string">string</a>, handler <a href="#Handler">Handler</a>)</pre>

Handle 将指定 URI pattern 的处理函数 handler 注册进 DefaultServeMux。ServeMux 的文档会说明 patterns 的匹配方式。

<h2 id="HandleFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2369">HandleFunc</a>
    <a href="#HandleFunc">¶</a></h2>
<pre>func HandleFunc(pattern <a href="/builtin/#string">string</a>, handler func(<a href="#ResponseWriter">ResponseWriter</a>, *<a href="#Request">Request</a>))</pre>

HandleFunc 将指定 URI pattern 的处理函数 handler 注册进 DefaultServeMux。ServeMux 的文档会说明 patterns 的匹配方式。

<h2 id="ListenAndServe">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2957">ListenAndServe</a>
    <a href="#ListenAndServe">¶</a></h2>
<pre>func ListenAndServe(addr <a href="/builtin/#string">string</a>, handler <a href="#Handler">Handler</a>) <a href="/builtin/#error">error</a></pre>

ListenAndServe 开始在 TCP 网络地址 addr 上进行监听，然后会在接收到的链接上调用处理函数的 Serve 处理请求。接收到的链接会启用 keep-alives。在 Handler 为 nil 的时候使用  DefaultServeMux。

一个简单的例子：

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

ListenAndServe 总会返回一个非 nil 的错误。

<h2 id="ListenAndServeTLS">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2990">ListenAndServeTLS</a>
    <a href="#ListenAndServeTLS">¶</a></h2>
<pre>func ListenAndServeTLS(addr, certFile, keyFile <a href="/builtin/#string">string</a>, handler <a href="#Handler">Handler</a>) <a href="/builtin/#error">error</a></pre>

ListenAndServeTLS 与 ListenAndServe 相同除了它接受的是 HTTPS 的链接。此外，必须提供服务器的证书文件和私钥。如果证书由证书颁发机构签署，certFile 就应该是包含服务器证书，任何中间证书和 CA 证书的串。

一个简单的例子:

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

可以使用 crypto/tls 中的 generate_cert.go 来创建 cert.pem 和 key.pem。

ListenAndServeTLS 总会返回非 nil 错误。

<h2 id="MaxBytesReader">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L1017">MaxBytesReader</a>
    <a href="#MaxBytesReader">¶</a></h2>
<pre>func MaxBytesReader(w <a href="#ResponseWriter">ResponseWriter</a>, r <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>, n <a href="/builtin/#int64">int64</a>) <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a></pre>

MaxBytesReader 和 io.LimitReader 相似，不过它是用来限制请求体的大小。和 io.LimitReader 的差异主要体现在 MaxBytesReader 的返回值是一个 ReadCloser，超过上限会返回一个非 EOF 的错误。并且在调用 Close 的时候关闭底层 Reader。

MaxBytesReader 会拒绝发送过大请求数据的客户端来避免浪费服务器资源。

<h2 id="NotFound">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L1954">NotFound</a>
    <a href="#NotFound">¶</a></h2>
<pre>func NotFound(w <a href="#ResponseWriter">ResponseWriter</a>, r *<a href="#Request">Request</a>)</pre>

NotFound 以 404 错误响应客户端请求。

<h2 id="ParseHTTPVersion">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L698">ParseHTTPVersion</a>
    <a href="#ParseHTTPVersion">¶</a></h2>
<pre>func ParseHTTPVersion(vers <a href="/builtin/#string">string</a>) (major, minor <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

ParseHTTPVersion 解析 HTTP 版本字符串。"HTTP/1.0" 返回 (1, 0, true)。

<h2 id="ParseTime">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/header.go#L71">ParseTime</a>
    <a href="#ParseTime">¶</a></h2>
<pre>func ParseTime(text <a href="/builtin/#string">string</a>) (t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>, err <a href="/builtin/#error">error</a>)</pre>

ParseTime 尝试使用 HTTP/1.1 允许的三种时间格式： TimeFormat、time.RFC850、和 time.ANSIC 来解析时间头部（例如：Date 头）。

<h2 id="ProxyFromEnvironment">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/transport.go#L265">ProxyFromEnvironment</a>
    <a href="#ProxyFromEnvironment">¶</a></h2>
<pre>func ProxyFromEnvironment(req *<a href="#Request">Request</a>) (*<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, <a href="/builtin/#error">error</a>)</pre>

ProxyFromEnvironment 返回该请求需要使用的代理 URL。可以设置环境变量 HTTP_PROXY、HTTPS_PROXY 和 NO_PROXY（或者其的第几版本）来使用代理。HTTPS_PROXY 的优先级高于 HTTP_PROXY 请求。

环境变量的值可以为完整的 URL 或假定使用 http 的 "host[:port]" 形式。如果不是以上格式将会返回错误。

如果没有定义代理环境变量将会返回 nil，nil。如果请求不想使用代理应该定义 NO_PROXY。

作为特例，如果 req.URL.Host 是 `localhost` (不带端口号)。那么返回 nil，nil。

<h2 id="ProxyURL">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/transport.go#L303">ProxyURL</a>
    <a href="#ProxyURL">¶</a></h2>
<pre>func ProxyURL(fixedURL *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>) func(*<a href="#Request">Request</a>) (*<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, <a href="/builtin/#error">error</a>)</pre>

ProxyURL 返回一个供 Transport 使用的代理函数，它一直返回相同 URL。

<h2 id="Redirect">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L1988">Redirect</a>
    <a href="#Redirect">¶</a></h2>
<pre>func Redirect(w <a href="#ResponseWriter">ResponseWriter</a>, r *<a href="#Request">Request</a>, url <a href="/builtin/#string">string</a>, code <a href="/builtin/#int">int</a>)</pre>

Redirect 以一个重定向的 url（可能是一个当前请求路径的相对路径）响应客户端请求。

返回的状态码在 3xx 的范围内一般为 StatusMovedPermanently、StatusFound 或 StatusSeeOther。

<h2 id="Serve">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2377">Serve</a>
    <a href="#Serve">¶</a></h2>
<pre>func Serve(l <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>, handler <a href="#Handler">Handler</a>) <a href="/builtin/#error">error</a></pre>

Serve 接受即将到达 l 上的 HTTP 链接。并为每个链接创建 goroutine。在 goroutine 中会读取请求并写入响应。当 Handler 为 nil 时默认使用 DefaultServeMux。

<h2 id="ServeContent">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/fs.go#L141">ServeContent</a>
    <a href="#ServeContent">¶</a></h2>
<pre>func ServeContent(w <a href="#ResponseWriter">ResponseWriter</a>, req *<a href="#Request">Request</a>, name <a href="/builtin/#string">string</a>, modtime <a href="/time/">time</a>.<a href="/time/#Time">Time</a>, content <a href="/io/">io</a>.<a href="/io/#ReadSeeker">ReadSeeker</a>)</pre>

ServeContent 以 ReadSeeker 中的内容响应请求。ServeContent 相对 io.Copy 的主要优势是它能正确处理不同种类的请求，设置 MIME 类型和处理 If-Match、If-Unmodified-Since、If-None-Match、If-Modified-Since、If-Range 请求。

如果响应没有设置 Content-Type，ServeContent 会先尝试使用文件名的后缀处判断类型。如果失败会读取第一块数据并使用 DetectContentType 解析它。名字在其他时候是没有用的；如果他为空就不会写入响应。

如果 modtime 不为 0 或 Unix 时间起点。ServeContent 会将其设置为 Last-Modified 响应头信息。如果请求头中包含 If-Modified-Since，ServeContent 会使用 modtime 来判断是否需要发送数据。

Seek 方法必须能够找到数据的结尾，这样 ServerContent 才能知道数据的大小。

如果调用者设置了 w 的 RFC 7232 第 2.3 章格式 ETag 头部。ServeContent 会使用它处理使用  If-Match、If-None-Match 或 If-Range 的请求。

注意：*os.File 实现了 io.ReadSeeker 接口。

<h2 id="ServeFile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/fs.go#L652">ServeFile</a>
    <a href="#ServeFile">¶</a></h2>
<pre>func ServeFile(w <a href="#ResponseWriter">ResponseWriter</a>, r *<a href="#Request">Request</a>, name <a href="/builtin/#string">string</a>)</pre>

ServeFile 以文件或者文件夹的内容响应请求。

如果给定的文件/文件夹为相对路径，那么将会以当前目录作为参考目录并可能访问父级目录。如果给定的名字是用户的输入，那么在调用 ServeFile 前需要对其预处理。出于安全考虑，ServeFile 会拒绝 `r.URL.Path` 中带有 `..` 的请求。

特殊说明：ServeFile 会将请求重定向在  r.URL.Path 的 "/index.html" 里。如果不想这样可以改变路径或者使用 ServeContent。

<h2 id="ServeTLS">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2392">ServeTLS</a>
    <a href="#ServeTLS">¶</a></h2>
<pre>func ServeTLS(l <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>, handler <a href="#Handler">Handler</a>, certFile, keyFile <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

ServeTLS 接受即将到达 l 的 HTTPS 链接，并为每个链接创建 goroutine。goroutine 会读取请求然后处理并响应请求。

如果 Handler 为 nil，会默认使用 DefaultServeMux。

此外，必须提供服务器的证书文件和私钥。如果证书由证书颁发机构签署，certFile 就应该是包含服务器证书，任何中间证书和 CA 证书的串。

<h2 id="SetCookie">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/cookie.go#L122">SetCookie</a>
    <a href="#SetCookie">¶</a></h2>
<pre>func SetCookie(w <a href="#ResponseWriter">ResponseWriter</a>, cookie *<a href="#Cookie">Cookie</a>)</pre>

SetCookie 在 ResponseWriter 头部添加一个 Set-Cookie 的头信息。cookie 必须包含一个有效名称。无效的 cookies 可能会被忽略。

<h2 id="StatusText">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/status.go#L134">StatusText</a>
    <a href="#StatusText">¶</a></h2>
<pre>func StatusText(code <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

StatusText 返回 HTTP 状态码对应的文字。如果返回未知状态码会返回空字符串。

<h2 id="Client">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L46">Client</a>
    <a href="#Client">¶</a></h2>
<pre>type Client struct {
<span id="Client.Transport"></span>    <span class="comment">// Transport 指定 HTTP 请求的生成机制，如果是 nil， 默认使用 DefaultTransport 。</span>
    Transport <a href="#RoundTripper">RoundTripper</a>

<span id="Client.CheckRedirect"></span>    <span class="comment">// CheckRedirect 指定处理重定向的策略。</span>
    <span class="comment">// 如果 CheckRedirect 不为 nil, 客户端会在重定向之前调用它。</span>
    <span class="comment">// req 是即将到达的请求，via 是已经转发的请求（按时间先后排序）。</span>
    <span class="comment">// 如果 CheckRedirect 错误，Client 的 Get 方法会返回之前的响应（Body 为关闭状态）和 CheckRedirect 发生的错误。</span>
    <span class="comment">// 如果 CheckRedirect 返回 ErrUseLastResponse，那么将会返回最近一次响应（Body 为未关闭状态）和 nil。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果 CheckRedirect 为 nil,调用者会使用默认的策略（支持最大 10 次连续重定向）。/span>
    CheckRedirect func(req *<a href="#Request">Request</a>, via []*<a href="#Request">Request</a>) <a href="/builtin/#error">error</a>

<span id="Client.Jar"></span>    <span class="comment">//  Jar 代表 cookie jar。</span>
    <span class="comment">//</span>
    <span class="comment">// Jar 用来保存每个请求的相关 cookie 并更新每个响应的 cookie。</span>
    <span class="comment">// Jar 会被重定向的客户端读取。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果 Jar 为 nil, cookie 只会在被显式设置的时候才会发送。</span>
    Jar <a href="#CookieJar">CookieJar</a>

<span id="Client.Timeout"></span>    <span class="comment">//Timeout 代表请求的这个客户端请求的时间限制。</span>
    <span class="comment">// 它包括连接时间，重定向时间和读取响应的时间。</span>
    <span class="comment">// 定时器会在 Get、Head、Post 或 Do 返回时停止并中止 Response.Body 的读取。</span>
    <span class="comment">//</span>
    <span class="comment">// 零值说明没有超时时间。</span>
    <span class="comment">//</span>
    <span class="comment">// Client 可以通过底层 Request.Cancel 来取消本次对底层 Transport 的请求。</span>
    <span class="comment">// 在 Client.Do 中也可以设置 Request.Cancel；两种方式都可以取消请求。</span>
    <span class="comment">//</span>
    <span class="comment">// 出于兼容性考虑，Client 可以在 Transport 中找到 CancelRequest 并使用它。新的 RoundTripper 实现需要使用 Request.Cancel 而不是实现 CancelRequest</span>
    Timeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>
}</pre>

Client 是 HTTP 客户端。零值的 Client 使用了 DefaultTransport。

Client 的 Transport 含有有内部状态（用来缓存 TCP 链接），所以 Client 应该多次重用而不是

按需创建。 Client 可以安全的被多个 goroutine 使用。

Client 比 RoundTripper 更高级并且还会处理 HTTP 的细节，例如 cookies 和重定向。

当重定向的时候，Client 使用之前请求的请求头除了：

• 当请求头中包含 "Authorization"、"WWW-Authenticate" 和 "Cookie" 敏感信息。这些头部会在重定向地址不是原域名或原域名的子域名时忽略。例如 "foo.com" 在重定向到 "foo.com" 或者 "sub.foo.com" 时会带有上述头信息。而重定向到 "bar.com" 却不会。 

• 当请求带有一个非 nil 的 cookie。因为每次重定向 cookie 的状态都可能会改变，所以每次重定向都有可能改变 cookie。每次重定向都会把发生变化的 cookie 数据删除并插入新值。如果 Jar 为 nil，那么将会保持原来的 cookie。

<h3 id="Client.Do">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L483">Do</a>
    <a href="#Client.Do">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Do(req *<a href="#Request">Request</a>) (*<a href="#Response">Response</a>, <a href="/builtin/#error">error</a>)</pre>

Do 会根据客户端的配置发送 HTTP 请求并返回响应。

如果是因为客户端配置或者网络连接原因会返回 error，一个非 2xx 响应不会返回错误。

如果返回的 error 是 nil，响应就会包含一个非 nil 的 Body（需要用户手动 Close）。如果 Body 没有关闭那么底层  RoundTripper 不会在随后的请求中重用 TCP 链接。

请求的 Body 会被底层 Transport 关闭（即使运行出错也会关闭）。

发生错误时任何响应都能被忽略。非 nil 的 error 和非 nil 的 Response 只有在检查重定向失败的时候才会同时发生。这时返回的 Response.Body 已经关闭。

一般情况我们可以使用 Get、Post 或 PostForm 而不是 Do。

如果服务器把重定向作为响应，Client 首先使用 CheckRedirect 判断是否要遵循重定向策略。如果允许重定向，状态码为 301、302 或 303 时，随后的请求使用 Get 方法（如果之前是 HEAD 就使用 HEAD 方法）。如果定义了 Request.GetBody 方法， 307 或 308 会使用原来的方法和请求体。NewRequest 函数会自动为标准的库的 Body 类型设置 GetBody 方法。

<h3 id="Client.Get">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L381">Get</a>
    <a href="#Client.Get">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Get(url <a href="/builtin/#string">string</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Get 向指定的 URL 发起 GET 请求。如果响应是以下几种重定向状态码之一，Get 会在调用 CheckRedirect 函数后进行重定向 ：

    301 (Moved Permanently)
    302 (Found)
    303 (See Other)
    307 (Temporary Redirect)
    308 (Permanent Redirect)

如果 Client 的 CheckRedirect 返回失败或发生 HTTP 协议错误会返回错误。一个非 2xx 响应不会返回错误。

如果 err 为 nil，resp 总是一个非 nil 的 resp.Body。调用者应该在读取完数据后关闭 resp.Body。

如果想使用自定义头部，可以使用 NewRequest 和 Client.Do。

<h3 id="Client.Head">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L791">Head</a>
    <a href="#Client.Head">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Head(url <a href="/builtin/#string">string</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Head 向指定 URL 发起一个 HEAD 请求。如果响应是以下几种重定向状态码之一，Get 会在调用 CheckRedirect 函数后进行重定向 ：

    301 (Moved Permanently)
    302 (Found)
    303 (See Other)
    307 (Temporary Redirect)
    308 (Permanent Redirect)

<h3 id="Client.Post">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L726">Post</a>
    <a href="#Client.Post">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Post(url <a href="/builtin/#string">string</a>, contentType <a href="/builtin/#string">string</a>, body <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Post 会向指定 URL 发起 POST 请求。

用户需要在读取完数据以后关闭 resp.Body。

如果 body 是一个 io.Closer，它会在请求完成后关闭。

如果想使用自定义头部，可以使用 NewRequest 和 Client.Do。

在 Client.Do 文档中具体说明如何处理重定向。

<h3 id="Client.PostForm">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L763">PostForm</a>
    <a href="#Client.PostForm">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) PostForm(url <a href="/builtin/#string">string</a>, data <a href="/net/url/">url</a>.<a href="/net/url/#Values">Values</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

PostForm 会向指定 URL 发起 POST 请求，他的请求体是经过 URL 编码的键值对。

Content-Type 会设置成 application/x-www-form-urlencoded。如果想设置其他头部，可以使用 NewRequest 和 DefaultClient.Do。

如果 err 为 nil，resp 总会返回非 nil 的 resp.Body。用户应该在读取完数据以后关闭 resp.Body。

在 Client.Do 文档中具体说明如何处理重定向。

<h2 id="CloseNotifier">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L190">CloseNotifier</a>
    <a href="#CloseNotifier">¶</a></h2>
<pre>type CloseNotifier interface {
    <span class="comment">// 当客户端关闭的时候，CloseNotify 会返回最多接收到一个值的 channel。</span>
    <span class="comment">//</span>
    <span class="comment">// CloseNotify 会一直等待直到 Request.Body 读取完成。</span>
    <span class="comment">//</span>
    <span class="comment">// 在处理函数执行完成以后, 不保证 channel 中是否有值返回。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果是 HTTP/1.1 协议并且 CloseNotify 在一个幂等请求执行时等待，那么在下次请求到达的时候可能导致 channel 中有值返回。在 HTTP/1.1 的管道实现中没有启用观察功能而且很多时候是观察不到的。如果对于你来说这是一个问题，可以使用 HTTP/2 协议或只在 POST 请求上使用 CloseNotify。</span>
    CloseNotify() &lt;-chan <a href="/builtin/#bool">bool</a>
}</pre>

ResponseWriter 实现了 CloseNotifier 接口，它赋予程序感知底层链接断开的能力。

这个机制能在客户端连接断开后取消那些服务器上的慢操作。

<h2 id="ConnState">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2620">ConnState</a>
    <a href="#ConnState">¶</a></h2>
<pre>type ConnState <a href="/builtin/#int">int</a></pre>

ConnState 代表客户端的连接状态，Server.ConnState 使用它。

<pre>const (
    <span class="comment">// StateNew 代表一个立刻发送请求的新链接。</span>
    <span class="comment">// 链接都以这个状态开始然后转换成 StateActive 或 StateClosed。</span>
    <span id="StateNew">StateNew</span> <a href="#ConnState">ConnState</a> = <a href="/builtin/#iota">iota</a>
    
    <span class="comment">// StateActive 代表一个已经读取过 1 个或更多字节请求的链接。</span>
    <span class="comment">// The Server.ConnState 会在请求进入处理函数前触发 StateActive 状态。在处理请求的过程中都不会再触发。</span>
    <span class="comment">// 在请求处理以后，状态会变成 StateClosed、StateHijacked 或 StateIdle。</span>
    <span class="comment">// 对于 HTTP/2，StateActive 会在活跃请求从 0 变为 1 时触发，并且只在所有活跃请求中触发一次。</span>
    <span class="comment">// 这意味着 ConState 不会在每个请求之前使用；ConnState 只表示链接的总体状态。</span>
    <span id="StateActive">StateActive</span>
    
    <span class="comment">// StateIdle 代表一个已经处理完请求并处于 keep-alive 状态等待新请求的链接。</span>
    <span class="comment">// 链接可以从 StateIdle 状态变为 StateActive 或 StateClosed 状态。</span>
    <span id="StateIdle">StateIdle</span>
    
    <span class="comment">// StateHijacked 代表一个劫持链接。</span>
    <span class="comment">// 这是一个链接的最终状态。它不会转换成 StateClosed.</span>
    <span id="StateHijacked">StateHijacked</span>
    
    <span class="comment">// StateClosed 代表一个关闭的链接。</span>
    <span class="comment">// 这是一个最终状态，劫持链接不会变成这个状态。</span>
    <span id="StateClosed">StateClosed</span>
    
    
)</pre>


<h3 id="ConnState.String">func (ConnState) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2666">String</a>
    <a href="#ConnState.String">¶</a></h3>
<pre>func (c <a href="#ConnState">ConnState</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Cookie">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/cookie.go#L10">Cookie</a>
    <a href="#Cookie">¶</a></h2>
<pre>type Cookie struct {
<span id="Cookie.Name"></span>    Name  <a href="/builtin/#string">string</a>
<span id="Cookie.Value"></span>    Value <a href="/builtin/#string">string</a>

<span id="Cookie.Path"></span>    Path       <a href="/builtin/#string">string</a>    <span class="comment">// optional</span>
<span id="Cookie.Domain"></span>    Domain     <a href="/builtin/#string">string</a>    <span class="comment">// optional</span>
<span id="Cookie.Expires"></span>    Expires    <a href="/time/">time</a>.<a href="/time/#Time">Time</a> <span class="comment">// optional</span>
<span id="Cookie.RawExpires"></span>    RawExpires <a href="/builtin/#string">string</a>    <span class="comment">// 只针对 cookie 的读取。</span>

<span id="Cookie.MaxAge"></span>    <span class="comment">// MaxAge 等于 0 代表没有 `Max-Age` 属性。</span>

    <span class="comment">// MaxAge 小于 0 代表现在删除 cookie，与 `Max-Age: 0` 相同。</span>
    <span class="comment">// MaxAge 大于 0 代表 `Max-Age` 在规定时间内存在。</span>
    MaxAge   <a href="/builtin/#int">int</a>
<span id="Cookie.Secure"></span>    Secure   <a href="/builtin/#bool">bool</a>
<span id="Cookie.HttpOnly"></span>    HttpOnly <a href="/builtin/#bool">bool</a>
<span id="Cookie.Raw"></span>    Raw      <a href="/builtin/#string">string</a>
<span id="Cookie.Unparsed"></span>    Unparsed []<a href="/builtin/#string">string</a> <span class="comment">// 键值对的原生字符串文本。</span>
}</pre>

Cookie 代表 HTTP 响应中的 Set-Cookie 头信息和 HTTP 请求中的 Cookie 头信息。

更多详情请看http://tools.ietf.org/html/rfc6265。

<h3 id="Cookie.String">func (*Cookie) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/cookie.go#L132">String</a>
    <a href="#Cookie.String">¶</a></h3>
<pre>func (c *<a href="#Cookie">Cookie</a>) String() <a href="/builtin/#string">string</a></pre>

String 返回 cookie 序列化后的字符串 （用于请求中的 Cookie 或者响应中的 Set-Cookie 头信息；只有 Name 和 Value 都存在才会被返回）。如果 c 为 nil 或者 c.Name 是无效的返回空字符串。

<h2 id="CookieJar">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/jar.go#L7">CookieJar</a>
    <a href="#CookieJar">¶</a></h2>
<pre>type CookieJar interface {
    <span class="comment">// SetCookies 处理指定 URL 返回的 cookie 信息。</span>
    <span class="comment">// 它可以选择保存或者不保存 cookie。这依赖具体的实现。</span>
    SetCookies(u *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, cookies []*<a href="#Cookie">Cookie</a>)
    
    <span class="comment">// Cookies 返回请求指定 URL 的 cookie。</span>
    <span class="comment">// 它严格遵循的 cookie 标准使用规范（RFC 6265）。</span>
    Cookies(u *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>) []*<a href="#Cookie">Cookie</a>
    
}</pre>

CookieJar 会管理 cookie 的储存并在 HTTP 请求中使用。

CookieJar 的实现必须保证能安全的被多个 goroutine 使用。

net/http/cookiejar 包提供了 CookieJar 的实现。

<h2 id="Dir">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/fs.go#L30">Dir</a>
    <a href="#Dir">¶</a></h2>
<pre>type Dir <a href="/builtin/#string">string</a></pre>

Dir 使用原生的文件系统实现了指定文件目录树的 FileSystem。

FileSystem.Open 传入 '/' 开头的路径时，Dir 的值是它在文件系统中的文件名而不是 URL，所以他的分隔符为 filepath.Separator 而不一定是 '/'。

注意 Dir 可以在一段时间内读取到一些敏感文件（例如 .git 目录或 .htpasswd）。为了排除这些文件，可以把这些文件移出目录或创建自定义的 FileSystem 实现。

一个空的 Dir 用 . 表示。

<h3 id="Dir.Open">func (Dir) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/fs.go#L56">Open</a>
    <a href="#Dir.Open">¶</a></h3>
<pre>func (d <a href="#Dir">Dir</a>) Open(name <a href="/builtin/#string">string</a>) (<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>


<h2 id="File">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/fs.go#L83">File</a>
    <a href="#File">¶</a></h2>
<pre>type File interface {
    <a href="/io/">io</a>.<a href="/io/#Closer">Closer</a>
    <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>
    <a href="/io/">io</a>.<a href="/io/#Seeker">Seeker</a>
    Readdir(count <a href="/builtin/#int">int</a>) ([]<a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)
    Stat() (<a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)
}</pre>

FileSystem 的 Open 返回 File 作为 FileServer 的实现。

这些方法的行为应该与 *os.File 保持一致。 

<h2 id="FileSystem">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/fs.go#L75">FileSystem</a>
    <a href="#FileSystem">¶</a></h2>
<pre>type FileSystem interface {
    Open(name <a href="/builtin/#string">string</a>) (<a href="#File">File</a>, <a href="/builtin/#error">error</a>)
}</pre>

FileSystem 实现了对一组指定文件的访问。不管是什么操作系统文件分隔符都是下划线（'/'，U+002F）字符。

<h2 id="Flusher">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L152">Flusher</a>
    <a href="#Flusher">¶</a></h2>
<pre>type Flusher interface {
    <span class="comment">// Flush 将所有缓存数据发送到客户端。</span>
    Flush()
}</pre>

ResponseWriters 实现了 Flusher 接口，它允许 HTTP 处理函数将缓存数据刷新进客户端。

默认的 HTTP/1.x 和 HTTP/2 ResponseWriter 实现都支持 Flusher。但是被封装过的 ResponseWriter 不一定实现了该接口。处理函数应该在使用之前测试是否实现了该接口。

注意：如果客户端是通过 HTTP 代理连接，即使 ResponseWriters 支持 Flush缓存数据也有可能在响应结束前不会直接到达客户端。

<h2 id="Handler">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L72">Handler</a>
    <a href="#Handler">¶</a></h2>
<pre>type Handler interface {
    ServeHTTP(<a href="#ResponseWriter">ResponseWriter</a>, *<a href="#Request">Request</a>)
}</pre>

Handler 负责对 HTTP 请求作出响应。

ServeHTTP 应该将响应头和响应数据写入 ResponseWriter 后返回。函数返回代表请求结束；在调用 ServeHTTP 的同时或之后再向 ResponseWriter 写入数据，或读取请求体都是无效的。

根据不同的 HTTP 客户端软件、HTTP 协议版本、所有客户端到服务端的中间环节都不可能在写入 ResponseWriter 之后读取 Request.Body。严谨的处理函数应该先读取 Request.Body 再响应请求。

除了读取请求，处理函数不能对请求做任何修改。

如果 ServeHTTP panic，服务器会认为 panic 就是中断此次请求，它会 recover 这个 panic，在错误日志中记录调用栈，

并关闭连接或发送 RST_STREAM（如果是 HTTP/2），这依赖 HTTP 协议的实现。中断请求函数客户端会得到一个中断的响应，但是他并不是服务器出错，所以不会有错误日志或 panic，而进入 ErrAbortHandler 函数。

<h3 id="FileServer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/fs.go#L695">FileServer</a>
    <a href="#FileServer">¶</a></h3>
<pre>func FileServer(root <a href="#FileSystem">FileSystem</a>) <a href="#Handler">Handler</a></pre>

FileServer 返回一个 HTTP 处理函数，它用 root 里文件的内容作为 HTTP 请求的响应来提供服务。

使用 http.Dir 来获取操作系统的文件系统实现：

    http.Handle("/", http.FileServer(http.Dir("/tmp")))

如果在请求路径有的尾部像 `/index.html` 这样，会把请求定向到同名文件路径，如果没有会自动定向到 `index.html`。

<a id="exampleFileServer"></a>
例:

    // Simple static webserver:
    log.Fatal(http.ListenAndServe(":8080", http.FileServer(http.Dir("/usr/share/doc"))))


<a id="exampleFileServer_stripPrefix"></a>
例:

    // 为了在一个 URL 路径下（/tmpfiles/）提供（/tmp）文件目录服务，可以使用 StripPrefix 在到达文件服务器之前修改请求的 URL 路径：
    http.Handle("/tmpfiles/", http.StripPrefix("/tmpfiles/", http.FileServer(http.Dir("/tmp"))))

<h3 id="NotFoundHandler">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L1958">NotFoundHandler</a>
    <a href="#NotFoundHandler">¶</a></h3>
<pre>func NotFoundHandler() <a href="#Handler">Handler</a></pre>

NotFoundHandler 返回一个将  `404 page not found` 作为请求响应的处理函数。

<h3 id="RedirectHandler">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2084">RedirectHandler</a>
    <a href="#RedirectHandler">¶</a></h3>
<pre>func RedirectHandler(url <a href="/builtin/#string">string</a>, code <a href="/builtin/#int">int</a>) <a href="#Handler">Handler</a></pre>

RedirectHandler 返回一个将请求以指定状态码重定向 url 作为响应的处理函数。

重定向状态码应该在 3xx 范围内，一般都为 StatusMovedPermanently、StatusFound 或 StatusSeeOther。

<h3 id="StripPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L1965">StripPrefix</a>
    <a href="#StripPrefix">¶</a></h3>
<pre>func StripPrefix(prefix <a href="/builtin/#string">string</a>, h <a href="#Handler">Handler</a>) <a href="#Handler">Handler</a></pre>

StripPrefix 返回一个将 URL 中的指定前缀去掉后调用 h 的处理函数。

如果请求不是以指定前缀开头，返回 HTTP 的 ` 404 not found` 错误。

<a id="exampleStripPrefix"></a>
例:

    // 为了在一个 URL 路径下（/tmpfiles/）提供（/tmp）文件目录服务，可以使用 StripPrefix 在到达文件服务器之前修改请求的 URL 路径：
    http.Handle("/tmpfiles/", http.StripPrefix("/tmpfiles/", http.FileServer(http.Dir("/tmp"))))

<h3 id="TimeoutHandler">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L3080">TimeoutHandler</a>
    <a href="#TimeoutHandler">¶</a></h3>
<pre>func TimeoutHandler(h <a href="#Handler">Handler</a>, dt <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>, msg <a href="/builtin/#string">string</a>) <a href="#Handler">Handler</a></pre>

TimeoutHandler 返回一个必须在规定时间内返回的处理函数。

Handler 会为每个请求调用 h.ServeHTTP，但是如果函数执行时间比规定时限更长，就会返回 503 错误和给定的错误信息（如果 msg 为空，会使用默认信息）。在超时以后 h 中 ResponseWriter 的写操作返回 ErrHandlerTimout。

TimeoutHandler 会将所有的处理函数写入内存而且不支持 Hijacker 和 Flusher 接口。

<h2 id="HandlerFunc">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L1933">HandlerFunc</a>
    <a href="#HandlerFunc">¶</a></h2>
<pre>type HandlerFunc func(<a href="#ResponseWriter">ResponseWriter</a>, *<a href="#Request">Request</a>)</pre>

HandlerFunc 类型是一个允许使用普通的函数作为 HTTP 的处理函数的适配器。如果一个函数拥有合适的函数签名 HandlerFunc(f) 会是一个调用 f 的 Handler。

<h3 id="HandlerFunc.ServeHTTP">func (HandlerFunc) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L1936">ServeHTTP</a>
    <a href="#HandlerFunc.ServeHTTP">¶</a></h3>
<pre>func (f <a href="#HandlerFunc">HandlerFunc</a>) ServeHTTP(w <a href="#ResponseWriter">ResponseWriter</a>, r *<a href="#Request">Request</a>)</pre>

ServeHTTP 调用  f(w, r)。

<h2 id="Header">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/header.go#L9">Header</a>
    <a href="#Header">¶</a></h2>
<pre>type Header map[<a href="/builtin/#string">string</a>][]

Header 代表 HTTP 头部的键值对信息。

<h3 id="Header.Add">func (Header) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/header.go#L13">Add</a>
    <a href="#Header.Add">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) Add(key, value <a href="/builtin/#string">string</a>)</pre>

Add 为 Header 添加一个键值对，他会将值追加到 key 对应的值中。

<h3 id="Header.Del">func (Header) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/header.go#L43">Del</a>
    <a href="#Header.Del">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) Del(key <a href="/builtin/#string">string</a>)</pre>

Del 删除 key 对应的值。

<h3 id="Header.Get">func (Header) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/header.go#L30">Get</a>
    <a href="#Header.Get">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) Get(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Get 会返回 key 对应的第一个值。它不区分大小写。textproto.CanonicalMIMEHeaderKey 用来提供规范化的 key。

如果 key 没有对应的值。Get 返回空字符串。如果想获取 key 的多个值，或使用非规范化的 key，可以直接访问 map。

<h3 id="Header.Set">func (Header) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/header.go#L20">Set</a>
    <a href="#Header.Set">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) Set(key, value <a href="/builtin/#string">string</a>)</pre>

Set 设置 key 的值。它会替换已经存在键的值。

<h3 id="Header.Write">func (Header) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/header.go#L48">Write</a>
    <a href="#Header.Write">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) Write(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

Write 以线性格式写入头信息。

<h3 id="Header.WriteSubset">func (Header) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/header.go#L137">WriteSubset</a>
    <a href="#Header.WriteSubset">¶</a></h3>
<pre>func (h <a href="#Header">Header</a>) WriteSubset(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, exclude map[<a href="/builtin/#string">string</a>]<a href="/builtin/#bool">bool</a>) <a href="/builtin/#error">error</a></pre>

WriteSubset 以线性格式写入头部，如果 exclude 不是 nil， 不会写入 `exclude[key] == true` 的键。

<h2 id="Hijacker">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L164">Hijacker</a>
    <a href="#Hijacker">¶</a></h2>
<pre>type Hijacker interface {
    <span class="comment">// Hijack 让调用者接管链接。</span>
    <span class="comment">// 调用 Hijack 后 http 服务器不会再对链接做任何操作。</span>
    <span class="comment">//</span>
    <span class="comment">// 由调用者管理和关闭这些链接。</span>
    <span class="comment">//</span>
    <span class="comment">// 返回的 net.Conn 可能有读或写的截止时间，这依赖服务器的配置。</span>
    <span class="comment">// 由调用者负责按需设置或者清除截止时间。</span>
    <span class="comment">//</span>
    <span class="comment">// 返回的 bufio.Reader 可能包含来自客户端还没处理的数据。</span>
    <span class="comment">//</span>
    <span class="comment">// 在调用 Hijack 后，原来的 Request.Body 不应该再被使用。</span>
    Hijack() (<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, *<a href="/bufio/">bufio</a>.<a href="/bufio/#ReadWriter">ReadWriter</a>, <a href="/builtin/#error">error</a>)
}</pre>

ResponseWriters 实现了 Hijacker 接口，它允许 HTTP 处理器接管链接。

默认的 HTTP/1.x  ResponseWriter 链接支持 Hijacker，但是 HTTP/2 不支持。封装的 ResponseWriter 可能不支持 Hijacker，处理器应该在使用前测试它是否有这种能力。

<a id="exampleHijacker"></a>
例:

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

<h2 id="ProtocolError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L34">ProtocolError</a>
    <a href="#ProtocolError">¶</a></h2>
<pre>type ProtocolError struct {
<span id="ProtocolError.ErrorString"></span>    ErrorString <a href="/builtin/#string">string</a>
}</pre>

ProtocolError 代表 HTTP 协议错误。

弃用：不是所有 http 包协议相关的错误都是 ProtocolError 类型。

<h3 id="ProtocolError.Error">func (*ProtocolError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L38">Error</a>
    <a href="#ProtocolError.Error">¶</a></h3>
<pre>func (pe *<a href="#ProtocolError">ProtocolError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="PushOptions">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/http.go#L95">PushOptions</a>
    <a href="#PushOptions">¶</a></h2>
<pre>type PushOptions struct {
<span id="PushOptions.Method"></span>    <span class="comment">//  Method 代表请求承诺的 HTTP 方法。</span>
    <span class="comment">// 它的值只能是 "GET" 或 "HEAD"。为空默认使用 "GET"。</span>
    Method <a href="/builtin/#string">string</a>

<span id="PushOptions.Header"></span>    <span class="comment">// Header 代表请求额承诺的请求头。</span>
    <span class="comment">// 它不能是 HTTP/2 中像 ":path" 和 ":scheme" 这些会自动添加的请求头字段。</span>
    Header <a href="#Header">Header</a>
}</pre>

PushOptions 描述了 Pusher.Push 的可选项。

<h2 id="Pusher">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/http.go#L109">Pusher</a>
    <a href="#Pusher">¶</a></h2>
<pre>type Pusher interface {
    <span class="comment">// Push 初始化一个 HTTP/2 服务器推送。这会构造一个给定选项和目标的请求。</span>
    <span class="comment">// 把这个请求序列化进 PUSH_PROMISE 框架。然后分发进请求处理函数。</span>
    <span class="comment">//如果 opts 为空，使用默认选项。</span>
    <span class="comment">//</span>
    <span class="comment">// target 必须是绝对路径或完整 URL（包含有效主机名称和父请求相同的协议方案）。</span>
    <span class="comment">// 如果 target 是绝对路径那么继承父请求的主机名和协议方案。</span>
    <span class="comment">//</span>
    <span class="comment">// HTTP/2 不允许递归推送和越权推送。</span>
    <span class="comment">// Push 不一定能检测到这些无效推送，不过无效的推送和取消操作可以被客户端侦测到。</span>
    <span class="comment">//</span>
    <span class="comment">// 想要向 URL X 推送的处理函数应该在任何可能触发向 URL X 请求之前调用 Push 方法。这样可以避免客户端在 X 接收到 PUSH_PROMISE 之前发起请求而引起的冲突。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果客户端禁用了推送或底层链接不支持推送会返回 ErrNotSupported。</span>
    Push(target <a href="/builtin/#string">string</a>, opts *<a href="#PushOptions">PushOptions</a>) <a href="/builtin/#error">error</a>
}</pre>

ResponseWriters 实现了Pusher 接口用来支持 HTTP/2 服务器的推送功能。更多信息请看https://tools.ietf.org/html/rfc7540#section-8.2。

<h2 id="Request">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L88">Request</a>
    <a href="#Request">¶</a></h2>
<pre>type Request struct {
<span id="Request.Method"></span>    <span class="comment">// Method 代表 HTTP 请求的方法 (GET、POST、PUT、等)。</span>
    <span class="comment">// 对于客户端空 Method 代表 GET 请求。</span>
    <span class="comment">//</span>
    <span class="comment">// Go 的 HTTP 客户端不支持使用 CONNECT 方法发送请求。Transport 有文档具体说明。</span>
    Method <a href="/builtin/#string">string</a>

<span id="Request.URL"></span>    <span class="comment">//  URL 对于服务端代表被请求的地址。</span>
    <span class="comment">// 对于客户端代表请求的 URL。</span>
    <span class="comment">//</span>
    <span class="comment">// 对于服务端来讲 URL 是从 RequestURI 中解析出来的。对于大多数请求，Path 和 RawQuery 都为空。（详情请看 RFC 2616 第 5.1.2 章节）</span>
    <span class="comment">//</span>
    <span class="comment">// 对于客户端请求，URL 的 Host 代表连接的服务器地址。而请求的 Host 字段可选地指定要在 HTTP 请求中发送的 Host 值。</span>
    URL *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>
    
    <span class="comment">// 请求的协议版本。</span>
    <span class="comment">//</span>
    <span class="comment">// 客户端忽略这些字段。HTTP 客户端总是使用 HTTP/1.1 或 HTTP/2。</span>
    <span class="comment">// 详情请看 Transport 文档。</span>
<span id="Request.Proto"></span>    Proto      <a href="/builtin/#string">string</a> <span class="comment">// &#34;HTTP/1.0&#34;</span>
<span id="Request.ProtoMajor"></span>    ProtoMajor <a href="/builtin/#int">int</a>    <span class="comment">// 1</span>
<span id="Request.ProtoMinor"></span>    ProtoMinor <a href="/builtin/#int">int</a>    <span class="comment">// 0</span>

<span id="Request.Header"></span>    <span class="comment">// Header 包含服务器接收或请求发送的请求头信息。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果服务器接收到如下头信息,</span>
    <span class="comment">//</span>
    <span class="comment">//	Host: example.com</span>
    <span class="comment">//	accept-encoding: gzip, deflate</span>
    <span class="comment">//	Accept-Language: en-us</span>
    <span class="comment">//	fOO: Bar</span>
    <span class="comment">//	foo: two</span>
    <span class="comment">//</span>
    <span class="comment">// 会转换为：</span>
    <span class="comment">//</span>
    <span class="comment">//	Header = map[string][]string{</span>
    <span class="comment">//		&#34;Accept-Encoding&#34;: {&#34;gzip, deflate&#34;},</span>
    <span class="comment">//		&#34;Accept-Language&#34;: {&#34;en-us&#34;},</span>
    <span class="comment">//		&#34;Foo&#34;: {&#34;Bar&#34;, &#34;two&#34;},</span>
    <span class="comment">//	}</span>
    <span class="comment">//</span>
    <span class="comment">// 对于即将到达的请求，Host 头信息会晋升为 Request.Host 字段并在 Header 的 map 中移除。</span>
    <span class="comment">//</span>
    <span class="comment">// HTTP 定义的头信息不区分大小写。请求解析器使用 CanonicalHeaderKey 实现忽略头信息的大小写，将首字符和连字符之后的字母大写，其他字母小写。</span>
    <span class="comment">//</span>
    <span class="comment">// 对于客户端请求，固定的头部例如 Content-Length 和 Connection 会自动按需设置已经存在的值会被忽略。详情请看 Request.Write 文档。</span>
    Header <a href="#Header">Header</a>

<span id="Request.Body"></span>    <span class="comment">//  Body 代表请求体。</span>
    <span class="comment">//</span>
    <span class="comment">// 客户端请求的请求体为 nil 说明请求没有请求体（例如 GET 请求）。客户端的 Transport 负责调用 Close 方法。</span>
    <span class="comment">//</span>
    <span class="comment">// 对于服务端请求的请求体一直为非 nil，但是在没有请求体存在的时候会立即返回 EOF。服务器会自动关闭请求体。ServeHTTP 不需要做关闭操作。</span>
    Body <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>

<span id="Request.GetBody"></span>    <span class="comment">// GetBody 定义了一个可选的函数用来返回请求体的拷贝。</span>
    <span class="comment">// 一般会用在在重定向时需要多次读取请求的时候。使用 GetBody 时仍然需要设置 Body。</span>
    <span class="comment">//</span>
    <span class="comment">// 对于服务端请求它是没用的。</span>
    GetBody func() (<a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>, <a href="/builtin/#error">error</a>)

<span id="Request.ContentLength"></span>    <span class="comment">// ContentLength 记录对应内容的长度。</span>
    <span class="comment">// -1 表示未知长度。</span>
    <span class="comment">// Values &gt;= 0 表示可以读取的最大字节数。</span>
    <span class="comment">// 对于客户端请求，一个非 nil 的 Body 值为 0 与未知长度是一样的。</span>
    ContentLength <a href="/builtin/#int64">int64</a>

<span id="Request.TransferEncoding"></span>    <span class="comment">// TransferEncoding 表示请求体由外至内的传输编码。空列表代表未编码。</span>
    <span class="comment">// TransferEncoding 总能被忽略；chunked 会根据收发请求的情况自动添加删除。</span>
    TransferEncoding []<a href="/builtin/#string">string</a>

<span id="Request.Close"></span>    <span class="comment">// Close 表示是否要在响应请求（服务端）或在读取响应（客户端）之后关闭链接。</span>
    <span class="comment">//</span>
    <span class="comment">// HTTP 服务器会自动处理并且处理函数中不需要这个字段。</span>
    <span class="comment">//</span>
    <span class="comment">// 客户端通过设置 Close 来阻止在设置 Transport.DisableKeepAlives 时请求相同主机时复用 TCP 链接。</span>
    Close <a href="/builtin/#bool">bool</a>
    
    <span id="Request.Host"></span><span class="comment">// 服务端的 Host 表示请求 URL 中的主机名。依据 RFC 2616，这个值可以是 Host 请求头的值也可以是 URL 主机名本身。</span>
    <span class="comment">// 它可能是 `host:port` 格式。对于国际域名，他可能是 Punycode 或 Unicode 格式。使用 `golang.org/x/net/idna` 转换成你需要的格式。</span>
    <span class="comment">//</span>
    <span class="comment">// 客户端请求可以选择在发送时重写 Host 请求头。Request.Write 方法使用的是 URL.Host 的值。Host 可以是一个国际域名。</span>
<span id="Request.Host"></span>    Host <a href="/builtin/#string">string</a>

<span id="Request.Form"></span>    <span class="comment">// Form 保存从 URL 参数和 POST 或 PUT 请求体中解析的数据。</span>
    <span class="comment">// 这个字段只有在调用 ParseForm 后才有效。</span>
    <span class="comment">// HTTP 客户端会忽略 Form 而直接使用 Body。</span>
    Form <a href="/net/url/">url</a>.<a href="/net/url/#Values">Values</a>

<span id="Request.PostForm"></span>    <span class="comment">// PostForm 包含从 POST、PATCH 或 PUT 请求体中解析出的数据。</span>
    <span class="comment">//</span>
    <span class="comment">// 这个字段只在 ParseForm 调用后有效。</span>
    <span class="comment">// HTTP 客户端忽略 PostForm 直接使用 Body。</span>
    PostForm <a href="/net/url/">url</a>.<a href="/net/url/#Values">Values</a>

<span id="Request.MultipartForm"></span>    <span class="comment">// MultipartForm 解析包括上传文件的 multipart 表单。</span>
    <span class="comment">// 这个字段只在 ParseForm 调用后有效。</span>
    <span class="comment">// HTTP 客户端忽略 MultipartForm 直接使用 Body。</span>
    MultipartForm *<a href="/mime/multipart/">multipart</a>.<a href="/mime/multipart/#Form">Form</a>

<span id="Request.Trailer"></span>    <span class="comment">// Trailer 指定一些在请求体之后发送的额外头部。</span>
    <span class="comment">//</span>
    <span class="comment">// 服务端请求的 Trailer 最初只包含请求 trailer 的键，它们的值都为 nil
        （客户端定义的 trailer 会在之后发送）。当处理函数正在读取请求体时，不能使用 Trailer。在请求体返回 EOF 后，Trailer 会被再次读取，如果客户端发送了 trailer 的值，那么 Trailer 就不会是 nil 值。</span>
    <span class="comment">//</span>
    <span class="comment">// 客户端的 Trailer 必须包括需要发送 trailer 的键。值可以为 nil 或者最终值。ContentLength 必须是 0 或 -1，表示发送一个分段的请求。</span>
    <span class="comment">// 在 HTTP 请求发送以后在请求体被读取前 map 中的值都是可以修改的。一旦 Body 返回 EOF，调用者就不能再修改 Trailer。</span>
    <span class="comment">//</span>
    <span class="comment">// 只有少数 HTTP 客户端，服务器，或代理支持 HTTP trailers。</span>
    Trailer <a href="#Header">Header</a>

<span id="Request.RemoteAddr"></span>    <span class="comment">// RemoteAddr 允许 HTTP 服务器和其他软件记录请求的发送地址，一般是为了日志需要。这个字段不会被 ReadRequest 填充并且没有定义格式。在调用处理函数之前，HTTP 服务器会将 RemoteAddr 设置为 `IP:port`；</span>
    <span class="comment">// 这个字段会被 HTTP 客户端忽略。</span>
    RemoteAddr <a href="/builtin/#string">string</a>

<span id="Request.RequestURI"></span>    <span class="comment">// RequestURI 保存客户端发给服务端的 Request-Line 的 Request-URI 原始数据。一般使用 URL 字段，在 HTTP 客户端请求中手动设置该字段是错误的。</span>
    RequestURI <a href="/builtin/#string">string</a>

<span id="Request.TLS"></span>    <span class="comment">// TLS 允许 HTTP 服务器和其他软件记录接收请求的 TLS 链接的相关信息。这个字段不会被 ReadRequest 填充。</span>
    <span class="comment">// 在这个包中的 HTTP 服务器会在调用处理函数前为开启 TLS 的链接设置该字段（如果未开启值为 nil）。</span>
    <span class="comment">// 这个字段会被 HTTP 客户端忽略。</span>
    TLS *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#ConnectionState">ConnectionState</a>

<span id="Request.Cancel"></span>    <span class="comment">// Cancel 是一个可选的 channel，它的关闭意味着请求被取消。不是所有的 RoundTripper 实现都支持 Cancel。</span>
    <span class="comment">//</span>
    <span class="comment">// 在服务端这个字段不适用。</span>
    <span class="comment">//</span>
    <span class="comment">// 弃用：使用 Context 和 WithContext 代替。如果请求的 Cancel 和 context 一起存在，不保证它们的优先级。</span>
    Cancel &lt;-chan struct{}

<span id="Request.Response"></span>    <span class="comment">// Response 是一个重定向的响应，它是创建该请求的原因。这个字段只有在客户端重定向时才会用到。</span>
    Response *<a href="#Response">Response</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Request 代表一个服务器接收或向客户端发送的 HTTP 请求。

客户端和服务端使用的时候语义上略有不同。为了了解这些我们可以查看 Request.Write 和 RoundTripper 的文档。

<h3 id="NewRequest">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L759">NewRequest</a>
    <a href="#NewRequest">¶</a></h3>
<pre>func NewRequest(method, url <a href="/builtin/#string">string</a>, body <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (*<a href="#Request">Request</a>, <a href="/builtin/#error">error</a>)</pre>

NewRequest 返回指定方法、URL 和请求体（可选）的新 Request。

如果 body 实现了 io.Closer，那么 Request.Body 会使用它并由 Do、Post、PostForm 和 Transport.RoundTrip 负责关闭。

NewRequest 返回一个适用于 Client.Do 和 Transport.RoundTrip 的 Request。如果想创建一个测试处理函数使用的请求，

可以使用 net/http/httptest 中的 NewRequest，ReadRequest，或手动修改请求的字段。

如果 body 是 bytes.Buffer、bytes.Reader 或 *strings.Reader 类型，返回请求的 ContentLength 是一个确切的值（而不是 -1），GetBody 会被填充（所以重定向的 307 和 308 能够重新读取body），并且 Body 会在 ContentLength 等于 0 的时候设置成 NoBody。

<h3 id="ReadRequest">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L903">ReadRequest</a>
    <a href="#ReadRequest">¶</a></h3>
<pre>func ReadRequest(b *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>) (*<a href="#Request">Request</a>, <a href="/builtin/#error">error</a>)</pre>

ReadRequest 读取并解析 b 中的请求数据。

<h3 id="Request.AddCookie">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L374">AddCookie</a>
    <a href="#Request.AddCookie">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) AddCookie(c *<a href="#Cookie">Cookie</a>)</pre>

AddCookie 为请求添加 cookie。依据 RFC 6265 的第 5.4 章节，AddCookie 不会添加多个相同 Cookie 头。这意味着所有的 cookie都会由分号分割写在同一行里面。

<h3 id="Request.BasicAuth">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L839">BasicAuth</a>
    <a href="#Request.BasicAuth">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) BasicAuth() (username, password <a href="/builtin/#string">string</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

如果请求使用的是 HTTP 基础验证 ，BasicAuth 返回 Authorization 头用于验证的用户名和密码。详情请看 RFC 2617 第 2 章。

<h3 id="Request.Context">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L310">Context</a>
    <a href="#Request.Context">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) Context() <a href="/context/">context</a>.<a href="/context/#Context">Context</a></pre>

Context 返回请求的 context，可以使用 WithContext 来更改 context。

context 总是非 nil 的；它默认是 background context。

对于服务器接收到的请求，context 会在客户端链接关闭或 ServeHTTP 方法返回时取消请求。

<h3 id="Request.Cookie">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L363">Cookie</a>
    <a href="#Request.Cookie">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) Cookie(name <a href="/builtin/#string">string</a>) (*<a href="#Cookie">Cookie</a>, <a href="/builtin/#error">error</a>)</pre>

Cookie 返回名字为 name 的 cookie ，如果没有找到返回 ErrNoCookie。如果找到多个同名 cookie，只有一个会返回。

<h3 id="Request.Cookies">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L352">Cookies</a>
    <a href="#Request.Cookies">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) Cookies() []*<a href="#Cookie">Cookie</a></pre>

Cookies 解析并返回 HTTP 请求的 cookie。

<h3 id="Request.FormFile">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L1258">FormFile</a>
    <a href="#Request.FormFile">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) FormFile(key <a href="/builtin/#string">string</a>) (<a href="/mime/multipart/">multipart</a>.<a href="/mime/multipart/#File">File</a>, *<a href="/mime/multipart/">multipart</a>.<a href="/mime/multipart/#FileHeader">FileHeader</a>, <a href="/builtin/#error">error</a>)</pre>

FormFile 返回 key 对应的第一个文件。FormFile 会在需要时调用 ParseMultipartForm 和 ParseForm。

<h3 id="Request.FormValue">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L1231">FormValue</a>
    <a href="#Request.FormValue">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) FormValue(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

FormValue 返回请求查询部分 key 对应的第一个值。POST 和 PUT body 请求体参数优先级高于 URL 查询部分。FormValue 会在需要的时候调用 ParseMultipartForm 和 ParseForm 并会忽略这些函数返回的错误。如果 key 不存在，FormValue 返回空字符串。如果同一个键对应多个值，会调用 ParseForm 然后直接在 Request.Form 取值。

<h3 id="Request.MultipartReader">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L407">MultipartReader</a>
    <a href="#Request.MultipartReader">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) MultipartReader() (*<a href="/mime/multipart/">multipart</a>.<a href="/mime/multipart/#Reader">Reader</a>, <a href="/builtin/#error">error</a>)</pre>

如果是 multipart/form-data 的 POST 请求，MultipartReader 返回一个 MIME multipart reader ，否则返回 nil 和 error。使用这个函数代替 ParseMultipartForm 把请求体最为流进行处理。

<h3 id="Request.ParseForm">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L1145">ParseForm</a>
    <a href="#Request.ParseForm">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) ParseForm() <a href="/builtin/#error">error</a></pre>

ParseForm 填充了 r.Form 和 r.PostForm。

对于所有请求，ParseForm 会解析 URL 中的查询部分并更新到 r.Form 中。

对于 POST、PUT 和 PATCH 请求，他还解析请求体并将结果保存在 r.PostForm 和 r.Form 中。请求体参数优先级高于 URL 查询参数。

对于其他的 HTTP 方法，如果请求的 Content-Type 不是 application/x-www-form-urlencoded，就不会读取请求体，这时 r.PostForm 是一个非 nil 的空值。

如果不使用 MaxBytesReader 限制请求体的尺寸。会取默认值 10MB。

ParseMultipartForm 会自动调用 ParseForm。ParseForm 是幂等的。

<h3 id="Request.ParseMultipartForm">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L1186">ParseMultipartForm</a>
    <a href="#Request.ParseMultipartForm">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) ParseMultipartForm(maxMemory <a href="/builtin/#int64">int64</a>) <a href="/builtin/#error">error</a></pre>

ParseMultipartForm 把请求体当作 multipart/form-data 类型并解析。它会解析整个请求体和最大 maxMemory 字节的文件并保存在内存中，其余部分会保存在硬盘或临时文件中。ParseMultipartForm 会在需要时调用 ParseForm。ParseMultipartForm 只需调用一次，多次调用没有效果。

<h3 id="Request.PostFormValue">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L1246">PostFormValue</a>
    <a href="#Request.PostFormValue">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) PostFormValue(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

PostFormValue 返回 POST 或 PUT 请求体参数中 key 对应的第一个值。它会忽略 URL 查询参数。PostFormValue

在需要时调用 ParseMultipartForm 和 ParseForm 并会忽略这些函数的错误。如果 key 不存在返回空字符串。

<h3 id="Request.ProtoAtLeast">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L341">ProtoAtLeast</a>
    <a href="#Request.ProtoAtLeast">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) ProtoAtLeast(major, minor <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>

ProtoAtLeast 验证 HTTP 协议版本至少为 major.minor。

<h3 id="Request.Referer">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L391">Referer</a>
    <a href="#Request.Referer">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) Referer() <a href="/builtin/#string">string</a></pre>

如果请求发送了上一个请求的 URL，Referer 会返回它。

Referer 其实是在 HTTP 早期请求中的拼写错误。这个值可以通过 Header["Referer"] 获取。

将它作为函数提供的好处是编译器可以诊断 req.Referer() 的拼写错误，Header["Referrer"] 却不能。

<h3 id="Request.SetBasicAuth">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L871">SetBasicAuth</a>
    <a href="#Request.SetBasicAuth">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) SetBasicAuth(username, password <a href="/builtin/#string">string</a>)</pre>

SetBasicAuth 设置请求的 Authorization 头信息供 HTTP 基础认证。

基础认证提供的用户名和密码是没有加密的。

<h3 id="Request.UserAgent">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L347">UserAgent</a>
    <a href="#Request.UserAgent">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) UserAgent() <a href="/builtin/#string">string</a></pre>

如果请求发送了 User-Agent 头信息，UserAgent 会返回它。

<h3 id="Request.WithContext">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L319">WithContext</a>
    <a href="#Request.WithContext">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) WithContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>) *<a href="#Request">Request</a></pre>

WithContext 返回一个浅拷贝的 context 来替换 ctx。ctx 不能是 nil。

<h3 id="Request.Write">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L467">Write</a>
    <a href="#Request.Write">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) Write(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

Write 以线性格式将 HTTP/1.1 请求头和请求体写入 w，这个方法会访问以下字段：

    Host
    URL
    Method (defaults to "GET")
    Header
    ContentLength
    TransferEncoding
    Body

如果 Body 存在，Content-Length 小于 0 并且 TransferEncoding 没有设置成 identity，Write 会把 "Transfer-Encoding: chunked" 写入请求头。Body 会在请求发送完成后关闭。

<h3 id="Request.WriteProxy">func (*Request) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/request.go#L477">WriteProxy</a>
    <a href="#Request.WriteProxy">¶</a></h3>
<pre>func (r *<a href="#Request">Request</a>) WriteProxy(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

WriteProxy 和 Write 相似只是以 HTTP 代理的格式写入。特指：WriteProxy 会根据 RFC 2616 的 5.1.2 章把绝对路径的 URI、协议和主机写入 Request-URI 行。WriteProxy 还会将 r.Host 或 r.URL.Host 的值写入 Host 头信息。

<h2 id="Response">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/response.go#L23">Response</a>
    <a href="#Response">¶</a></h2>
<pre>type Response struct {
<span id="Response.Status"></span>    Status     <a href="/builtin/#string">string</a> <span class="comment">// e.g. &#34;200 OK&#34;</span>
<span id="Response.StatusCode"></span>    StatusCode <a href="/builtin/#int">int</a>    <span class="comment">// e.g. 200</span>
<span id="Response.Proto"></span>    Proto      <a href="/builtin/#string">string</a> <span class="comment">// e.g. &#34;HTTP/1.0&#34;</span>
<span id="Response.ProtoMajor"></span>    ProtoMajor <a href="/builtin/#int">int</a>    <span class="comment">// e.g. 1</span>
<span id="Response.ProtoMinor"></span>    ProtoMinor <a href="/builtin/#int">int</a>    <span class="comment">// e.g. 0</span>

<span id="Response.Header"></span>    <span class="comment">// Header 保存响应头的键值对信息。如果响应头包含多个相同的键，会把他们用分号分割（RFC 2616 第 4.2 章需要把多个相同头部看作一个分号序列）。</span>
    <span class="comment">// 响应头的值复制到这个结构的其他字段（例如：ContentLength，TransferEncoding，Trailer）。</span>
    
    <span class="comment">//</span>
    <span class="comment">// 响应头的中的键都是规范化的 (请看 CanonicalHeaderKey).</span>
    Header <a href="#Header">Header</a>
    

<span id="Response.Body"></span>    <span class="comment">// Body 代表响应体。</span>
    <span class="comment">//</span>
    <span class="comment">// 响应体可以从 Body 属性按需读取。如果网络链接断开或者服务器关闭了响应，Body.Read 会返回错误。</span>
    <span class="comment">//</span>
    <span class="comment">// Client 和 Transport 确保 Body 即使在没有响应体或者响应长度为 0 的时候也不为 nil。由调用者负责关闭它。</span>
    <span class="comment">// 如果 Body 读取完成后没有关闭，默认的 HTTP 客户端的 Transport 不会企图重用 HTTP/1.0 或 HTTP/1.1 的 TCP 链接。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果服务器响应头中包含 Transfer-Encoding 且为 chunked，Body 会自动的分段传输。</span>
    Body <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>
    
    <span id="Response.ContentLength"></span> <span class="comment">// ContentLength 记录对应响应内容的长度，-1 表示响应内容长度未知。如果在响应头中设置它大于 0，代表可以在 Body 中最多读取 ContentLength 个字节的数据。</span>
    ContentLength <a href="/builtin/#int64">int64</a>
    
     <span class="comment">// 包含请求体从外到内的传输编码。如果为 nil，说明未编码。</span>
<span id="Response.TransferEncoding"></span>    TransferEncoding []<a href="/builtin/#string">string</a>

<span id="Response.Close"></span>    <span class="comment">// Close 记录响应头指向的链接在读取 Body后是否关闭。</span>
    <span class="comment">// 这个值只是给客户端作为参考：ReadResponse 和 Response.Write 都会关闭链接。</span>
    Close <a href="/builtin/#bool">bool</a>

<span id="Response.Uncompressed"></span>    <span class="comment">// Uncompressed 是否需要 http 解压已经被压缩的数据。如果为 true，会从 Body 中读取数据并解压缩内容而不是直接返回在服务端压缩过的数据，ContentLength 会置为 -1，Content-Length 和 Content-Encoding 字段会从 responseHeader 中删除。如果想获取服务器原本的响应数据，需要把 Transport.DisableCompression 置为 true。</span>
    Uncompressed <a href="/builtin/#bool">bool</a>

<span id="Response.Trailer"></span>    <span class="comment">// Trailer 保存的键值对格式与响应头相同。</span>
    <span class="comment">//</span>
    <span class="comment">// Trailer 最初由服务器指定的 `Trailer` 头的键都为 nil。它们不会在添加在响应头中。</span>
    <span class="comment">//</span>
    <span class="comment">// 不能在读取响应的时候访问 Trailer。</span>
    <span class="comment">//</span>
    <span class="comment">// 当响应体返回 io.EOF 时，Trailer 才会获取到服务器发送的值。</span>
    Trailer <a href="#Header">Header</a>

<span id="Response.Request"></span>    <span class="comment">// Request 代表获取到该响应的请求。</span>
    <span class="comment">// Request 的 Body 为 nil (因为已经被消费)。</span>
    <span class="comment">// 它只会在客户端请求中才会有值。</span>
    Request *<a href="#Request">Request</a>

<span id="Response.TLS"></span>    <span class="comment">// TLS 包含响应收到的 TLS 链接信息。对于非加密链接它为 nil。</span>
    <span class="comment">// 这个指针对应的值是响应之间共享的所以不能改变它。</span>
    TLS *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#ConnectionState">ConnectionState</a>
}</pre>

Response 表示来自 HTTP 请求的响应。

一旦收到响应头，Client 和 Transport 就会从服务器返回 Response。响应体在读取 Body 字段时按需传输。

<h3 id="Get">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L359">Get</a>
    <a href="#Get">¶</a></h3>
<pre>func Get(url <a href="/builtin/#string">string</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Get 向指定 URL 发起 Get 请求。如果响应的状态码是下面几种重定向状态码，Get 会自动进行最多 10 次的重定向：

    301 (Moved Permanently)
    302 (Found)
    303 (See Other)
    307 (Temporary Redirect)
    308 (Permanent Redirect)

如果有太多的重定向或者发生 HTTP 协议错误会返回错误。一个非 2xx 的响应不会导致错误。

当 err 为 nil，resp 的 Body 一定不是 nil。用户应该是读取之后关闭 resp.Body。

Get 是 DefaultClient.Get 的封装。

如果想自定义请求头，可以使用 NewRequest 和 DefaultClient.Do。

<a id="exampleGet"></a>
例:

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

<h3 id="Head">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L778">Head</a>
    <a href="#Head">¶</a></h3>
<pre>func Head(url <a href="/builtin/#string">string</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Head 向指定 URL 发起 HEAD 请求。如果响应的状态码是下面几种重定向状态码，Get 会自动进行最多 10 次的重定向：

    301 (Moved Permanently)
    302 (Found)
    303 (See Other)
    307 (Temporary Redirect)
    308 (Permanent Redirect)

Head 是 DefaultClient.Head 的封装。

<h3 id="Post">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L711">Post</a>
    <a href="#Post">¶</a></h3>
<pre>func Post(url <a href="/builtin/#string">string</a>, contentType <a href="/builtin/#string">string</a>, body <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

Post 向指定 URL 发起 POST 请求。

用户应该在读取完 resp.Body 后关闭它。

如果 Body 是一个 io.Closer 接口实现。它会在请求结束后关闭。

Post 是 DefaultClient.Post 的封装。

如果想自定义请求头，可以使用 NewRequest 和 DefaultClient.Do。

关于更多关于重定向的处理请看 Client.Do 文档。

<h3 id="PostForm">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L748">PostForm</a>
    <a href="#PostForm">¶</a></h3>
<pre>func PostForm(url <a href="/builtin/#string">string</a>, data <a href="/net/url/">url</a>.<a href="/net/url/#Values">Values</a>) (resp *<a href="#Response">Response</a>, err <a href="/builtin/#error">error</a>)</pre>

PostForm 向指定 URL 发起 POST 请求，他会在请求体附带 URL 编码过的键值对。

Content-Type 头会被设置为 application/x-www-form-urlencoded。如果想设置其他请求头，可以使用 NewRequest 和 DefaultClient.Do。

当 err 为 nil。resp 总是包含一个非 nil Body。用户应该在读取完 resp.Body 后关闭它。

PostForm 是 DefaultClient.PostForm 的封装。

关于更多关于重定向的处理请看 Client.Do 文档。

<h3 id="ReadResponse">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/response.go#L138">ReadResponse</a>
    <a href="#ReadResponse">¶</a></h3>
<pre>func ReadResponse(r *<a href="/bufio/">bufio</a>.<a href="/bufio/#Reader">Reader</a>, req *<a href="#Request">Request</a>) (*<a href="#Response">Response</a>, <a href="/builtin/#error">error</a>)</pre>

ReadResponse 从 r 中读取并返回 HTTP 响应。req 可以选择指定响应对应的参数。如果为 nil ，假设它是一个 GET 请求。用户必须在读取完 resp.Body 后关闭它。在这之后客户端可以在 resp.Trailer 中获取 trailer。

<h3 id="Response.Cookies">func (*Response) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/response.go#L109">Cookies</a>
    <a href="#Response.Cookies">¶</a></h3>
<pre>func (r *<a href="#Response">Response</a>) Cookies() []*<a href="#Cookie">Cookie</a></pre>

Cookies 解析并返回 Set-Cookie 头中的 cookie。

<h3 id="Response.Location">func (*Response) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/response.go#L121">Location</a>
    <a href="#Response.Location">¶</a></h3>
<pre>func (r *<a href="#Response">Response</a>) Location() (*<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, <a href="/builtin/#error">error</a>)</pre>

响应头中如果存在 Location，Location函数返回它对应的 URL。如果是相对路径会选择响应对应的请求作为参考。如果没有 Location 头信息会返回 ErrNoLocation 错误。

<h3 id="Response.ProtoAtLeast">func (*Response) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/response.go#L208">ProtoAtLeast</a>
    <a href="#Response.ProtoAtLeast">¶</a></h3>
<pre>func (r *<a href="#Response">Response</a>) ProtoAtLeast(major, minor <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>

ProtoAtLeast 判断在响应中的 HTTP 协议至少为 major.minor。

<h3 id="Response.Write">func (*Response) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/response.go#L229">Write</a>
    <a href="#Response.Write">¶</a></h3>
<pre>func (r *<a href="#Response">Response</a>) Write(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

Write 以 HTTP/1.x 服务器响应格式将 r 写入 w。包括状态行，头部，数据体和 trailer（可选）。

这个方法将会访问 r 的以下属性：

    StatusCode
    ProtoMajor
    ProtoMinor
    Request.Method
    TransferEncoding
    Trailer
    Body
    ContentLength
    Header, values for non-canonical keys will have unpredictable behavior

Response 的 Body 会在发送后关闭。

<h2 id="ResponseWriter">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L81">ResponseWriter</a>
    <a href="#ResponseWriter">¶</a></h2>
<pre>type ResponseWriter interface {
    <span class="comment">// Header 返回通过 WriteHeader 发送的头信息 map。这个机制也能用来设置 HTTP trailer。</span>
    <span class="comment">//</span>
    <span class="comment">// 在调用 WriteHeader 后再修改头信息 map 不会影响响应的头信息，除非它是 trailer。</span>
    <span class="comment">//</span>
    <span class="comment">// 有两种方式来设置 Trailer。推荐的方式是在头信息中预先声明你要返回的 trailers 键名。这些键会被处理成 trailer。第二种方式在第一次写入 trailer 前都不知道 trailer 的键信息，它为 trailer 信息设置一个常量值的前缀 TrailerPrefix。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果不想使用隐式的头部（例如：Date），可以把他的值设为 nil。</span>
    Header() <a href="#Header">Header</a>
    
    <span class="comment">// Write 会把 HTTP 的响应写入链接。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果没有调用过 WriteHeader，Write 会在写入数据前调用 WriteHeader（http.StatusOK）。如果响应头中没有 Content-Type 信息，会把写入数据的前 512 字节传入 DetectContentType 并把结果作为 Content-Type 的值。</span>
    <span class="comment">//</span>
    <span class="comment">// 根据 HTTP 协议的版本和客户端，调用 Write 可能会阻止再次读取 Request.Body。对于 HTTP/1.x 请求，处理函数需要在写入请求前读取请求体数据。一旦头部被刷新（用于显式调用 Flusher.Flush 或写入的数据触发了刷新），请求体就会无效。而对于 HTTP/2 请求，Go 的 HTTP 服务器允许处理函数读取请求的同时写入响应。但是这种行为不是所有的 HTTP/2 客户端都支持。如果可能，处理函数应该在写入前完成读取请求以保证最大的兼容性。</span>
    Write([]<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)
    
    <span class="comment">// WriteHeader 发送状态码和响应头。如果 WriteHeader 没有被显式调用，那么第一次调用 Write 时会隐式触发 WriteHeader(http.StatusOK)。因此它主要用于返回错误状态码的情况。</span>
    <span class="comment">//</span>
    <span class="comment">// 状态码必须是有效的 HTTP 1xx-5xx 状态码。只可以写入一回头部。Go 目前不支持发送用户自定义的 1xx 信息头，除了 100-continue 会在请求体被读取的时候自动发送。</span>
    WriteHeader(statusCode <a href="/builtin/#int">int</a>)
}</pre>

HTTP 处理函数使用 ResponseWriter 接口初始化 HTTP 响应。

ResponseWriter 不能在 Handler.ServeHTTP 方法返回后使用。

<a id="exampleResponseWriter_trailers"></a>
例:

    mux := http.NewServeMux()
    mux.HandleFunc("/sendstrailers", func(w http.ResponseWriter, req *http.Request) {
        //在调用 WriteHeader 或 Write 之前，声明你之后将要发送的 trailer。
        //这三个头其实会送入 trailer。
        w.Header().Set("Trailer", "AtEnd1, AtEnd2")
        w.Header().Add("Trailer", "AtEnd3")
    
        w.Header().Set("Content-Type", "text/plain; charset=utf-8") // normal header
        w.WriteHeader(http.StatusOK)
    
        w.Header().Set("AtEnd1", "value 1")
        io.WriteString(w, "This HTTP response has both headers before this text and trailers at the end.\n")
        w.Header().Set("AtEnd2", "value 2")
        w.Header().Set("AtEnd3", "value 3") // These will appear as trailers.
    })

<h2 id="RoundTripper">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/client.go#L107">RoundTripper</a>
    <a href="#RoundTripper">¶</a></h2>
<pre>type RoundTripper interface {
    <span class="comment">// RoundTrip 执行一个单独的 HTTP 事务，并响应指定请求。</span>
    <span class="comment">//</span>
    <span class="comment">// RoundTrip 不应该企图中断响应。尤其是在它获取到了一个响应时，必须返回 err == nil，不管 HTTP 状态码是什么。</span>
    <span class="comment">// 一个非 nil 错误应该当作获取响应失败处理。相似的，RoundTrip 不应该获取高级的协议信息例如重定向，身份验证或 cookie。</span>
    <span class="comment">//</span>
    <span class="comment">// RoundTrip 不能改变请求，除了读取和关闭请求体。</span>
    <span class="comment">// been closed.</span>
    <span class="comment">//</span>
    <span class="comment">// RoundTrip 必须关闭请求体，包括发生错误的时候，不过可以根据实现在不同的 goroutine 中关闭，甚至是在 RoundTrip 返回以后。这意味着调用者如果想要让接下来的请求复用请求体必须等到 Close 函数调用以后。</span>
    <span class="comment">//</span>
    <span class="comment">// 必须初始化请求的 URL 和 Header 字段。</span>
    RoundTrip(*<a href="#Request">Request</a>) (*<a href="#Response">Response</a>, <a href="/builtin/#error">error</a>)
}</pre>

RoundTripper 接口代表执行 HTTP 单个事务，根据请求取得响应的能力。

RoundTripper 实现必须能够安全的被多个 goroutine 同时使用。

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

DefaultTransport 是 Transport 的默认实现，DefaultClient 使用它。它按需建立网络连接并且缓存它们来在随后的请求中复用。它会使用环境变量中的 HTTP_PROXY 和 NO_PROXY （或  http_proxy 和 no_proxy）作为 HTTP 代理。

<h3 id="NewFileTransport">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/filetransport.go#L20">NewFileTransport</a>
    <a href="#NewFileTransport">¶</a></h3>
<pre>func NewFileTransport(fs <a href="#FileSystem">FileSystem</a>) <a href="#RoundTripper">RoundTripper</a></pre>

NewFileTransport 返回一个新的 RoundTripper，他服务于文件系统。RoundTripper 的返回值忽略 URL 的主机参数。

一个使用 NewFileTransport 的例子就是注册 file 协议的 Transport：

    t := &http.Transport{}
    t.RegisterProtocol("file", http.NewFileTransport(http.Dir("/")))
    c := &http.Client{Transport: t}
    res, err := c.Get("file:///etc/passwd")
    ...

<h2 id="ServeMux">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2123">ServeMux</a>
    <a href="#ServeMux">¶</a></h2>
<pre>type ServeMux struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ServeMux 是一个 HTTP 请求多路复用器。他根据已经注册的 pattern 匹配每个请求的 URL 并调用最匹配的处理函数。

Patterns 名称固定为绝对路径（例如 /favicon.ico），或者以下划线结尾（例如 /images/）。

长 patterns 的优先级高于短 patterns。所以如果注册了 2 个 patterns /images/ 和 /images/thumbnails/

第二个的处理函数将会处理 /images/thumbnails/ 而前者会接收其他在 /images/ 子树下的请求。

注意：因为以斜线结尾的 pattern 被命名为一个子树，pattern 的 / 会匹配所有其他 pattern 没有匹配的请求，不仅仅是 /。

如果一个子树已经注册并且一个请求访问了这个子树路径但没有以斜线结尾，ServeMux 会自动重定向到子树根节点（添加结尾的下划线）。这个行为可以通过注册一个不以斜线结尾的 pattern 覆盖。例如：注册 /images/ 导致 ServeMux 重定向 /images 到 /images/，除非已经注册 /images 。

Patterns 可以选择以主机名称开头来严格匹配 URL。指定主机的 patterns 的优先级高于普通 patterns，所以注册 /codesearch 和 codesearch.google.com/ 的处理函数不会对 http://www.google.com/ 请求做出响应。

ServeMux 也关心 URL 路径的形式。他会自动过滤 . 、 .. 和多余的斜线。


<h3 id="NewServeMux">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2135">NewServeMux</a>
    <a href="#NewServeMux">¶</a></h3>
<pre>func NewServeMux() *<a href="#ServeMux">ServeMux</a></pre>

NewServeMux 创建并返回一个新的 ServeMux。

<h3 id="ServeMux.Handle">func (*ServeMux) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2332">Handle</a>
    <a href="#ServeMux.Handle">¶</a></h3>
<pre>func (mux *<a href="#ServeMux">ServeMux</a>) Handle(pattern <a href="/builtin/#string">string</a>, handler <a href="#Handler">Handler</a>)</pre>

Handle 注册指定 pattern 的处理函数。如果给定的 pattern 已经拥有处理函数将会 panic。

<a id="exampleServeMux_Handle"></a>
例:

    mux := http.NewServeMux()
    mux.Handle("/api/", apiHandler{})
    mux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
        // "/" 会匹配所有请求，所以我们要确保我们在 root 下。
        if req.URL.Path != "/" {
            http.NotFound(w, req)
            return
        }
        fmt.Fprintf(w, "Welcome to the home page!")
    })

<h3 id="ServeMux.HandleFunc">func (*ServeMux) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2357">HandleFunc</a>
    <a href="#ServeMux.HandleFunc">¶</a></h3>
<pre>func (mux *<a href="#ServeMux">ServeMux</a>) HandleFunc(pattern <a href="/builtin/#string">string</a>, handler func(<a href="#ResponseWriter">ResponseWriter</a>, *<a href="#Request">Request</a>))</pre>

HandleFunc 注册指定 pattern 的处理函数。

<h3 id="ServeMux.Handler">func (*ServeMux) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2262">Handler</a>
    <a href="#ServeMux.Handler">¶</a></h3>
<pre>func (mux *<a href="#ServeMux">ServeMux</a>) Handler(r *<a href="#Request">Request</a>) (h <a href="#Handler">Handler</a>, pattern <a href="/builtin/#string">string</a>)</pre>

Handler 根据  r.Method、r.Host 和 r.URL.Path 返回请求所用的处理函数，它总是返回非 nil 的处理函数。如果不存在这个路径。处理函数会是一个内部产生的重定向到指定页面的处理函数。如果主机包含端口，会在匹配的时候忽略。

对于 CONNECT 请求路径和主机名的使用没有变化。

处理函数也返回匹配到的路径，如果是一个内部生成的重定向路径会返回重定向页面的路径。

如果请求没有匹配到处理函数。处理函数会是 `page not found` 处理函数和一个空 parttern。

<h3 id="ServeMux.ServeHTTP">func (*ServeMux) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2318">ServeHTTP</a>
    <a href="#ServeMux.ServeHTTP">¶</a></h3>
<pre>func (mux *<a href="#ServeMux">ServeMux</a>) ServeHTTP(w <a href="#ResponseWriter">ResponseWriter</a>, r *<a href="#Request">Request</a>)</pre>

ServeHTTP 将请求分发到它最匹配的 pattern 对应的处理函数上。

<h2 id="Server">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2399">Server</a>
    <a href="#Server">¶</a></h2>
<pre>type Server struct {
<span id="Server.Addr"></span>    Addr    <a href="/builtin/#string">string</a>  <span class="comment">// TCP 的监听地址, 如果为空使用 ":http"</span>
<span id="Server.Handler"></span>    Handler <a href="#Handler">Handler</a> <span class="comment">// 调用的处理函数, 如果为 nil 使用 http.DefaultServeMux</span>

<span id="Server.TLSConfig"></span>    <span class="comment">// 可选的 TLS 配置,  ServeTLS 和 ListenAndServeTLS 使用。</span>
    <span class="comment">// 它的值会被 ServeTLS 和 ListenAndServeTLS 拷贝，所以不能使用像 tls.Config.SetSessionTicketKeys 修改它的配置信息。如果需要使用 SetSessionTicketKeys，可以在 TLS 监听器上使用 Server.Serve。</span>
    TLSConfig *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#Config">Config</a>

<span id="Server.ReadTimeout"></span>    <span class="comment">// ReadTimeout 读取的请求实体的最大时间，包括请求体。</span>
    <span class="comment">//</span>
    <span class="comment">// 因为 ReadTimeout 不能让处理函数自己决定每个请求体的有效时间和上传频率，所以大多数人更推荐使用 ReadHeaderTimeout。它们两个是可以一起使用的。</span>
    ReadTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>
<span id="Server.ReadHeaderTimeout"></span>    <span class="comment">// ReadHeaderTimeout 是允许读取请求头的时间。链接的读取时限会在读取头部信息后重置。处理函数可以慢慢考虑如何使用请求体。</span>

    ReadHeaderTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Server.WriteTimeout"></span>    <span class="comment">// WriteTimeout 是写入响应的超时时间。它会在读取到新请求的请求头时重置。和 ReadTimeout 一样，它不能让处理函数决定写入请求体的时间。</span>
    WriteTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Server.IdleTimeout"></span>    <span class="comment">//IdleTimeout 是在 keep-alives 开启时保持连接状态等待下次请求的最大时间。如果 IdleTimeout 为 0，会使用 ReadTimeout 的值。如果它们都是 0，会使用 ReadHeaderTimeout。</span>
    IdleTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Server.MaxHeaderBytes"></span>    <span class="comment">// MaxHeaderBytes 控制服务器从请求头中读取的键值对的最大字节数（包括请求行）。它不会限制请求体的大小。</span>
    <span class="comment">// 如果值为 0，会使用 DefaultMaxHeaderBytes。</span>
    <span class="comment">// </span>
    MaxHeaderBytes <a href="/builtin/#int">int</a>

<span id="Server.TLSNextProto"></span>    <span class="comment">// TLSNextProto 可选的在发生 NPN/ALPN 协议升级的时候指定一个函数来接管 TLS 链接的所有权。</span>
    <span class="comment">// map 的键是协议名。处理函数的参数用来处理 HTTP 请求并且会在没有设置的时候生成请求的 TLS 和 RemoteAddr。链接会在函数返回的时候自动关闭。</span>
    <span class="comment">// 如果 TLSNextProto 不为 nil, 不会自动启用对 HTTP/2 的支持。</span>
    TLSNextProto map[<a href="/builtin/#string">string</a>]func(*<a href="#Server">Server</a>, *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#Conn">Conn</a>, <a href="#Handler">Handler</a>)

<span id="Server.ConnState"></span>    <span class="comment">// ConnState 可选的指定一个函数，它会在哭护短连接状态改变的时候调用。ConnState 类型文档会具体说明。</span>
    ConnState func(<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, <a href="#ConnState">ConnState</a>)

<span id="Server.ErrorLog"></span>    <span class="comment">// ErrorLog 一个 logger 用来接收链接的错误，未定义处理器行为和底层文件系统错误。</span>
    <span class="comment">// 如果为 nil，日志会通过 log 包输出错误信息到 os.Stderr。</span>
    ErrorLog *<a href="/log/">log</a>.<a href="/log/#Logger">Logger</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Server 定义了 HTTP 服务器运行时的参数。零值的 Server 是一个有效的配置。

<h3 id="Server.Close">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2514">Close</a>
    <a href="#Server.Close">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) Close() <a href="/builtin/#error">error</a></pre>

Close 会立即关闭所有正在监听的 net.Listener 或任何状态为 StateNew、StateActive 或 StateIdle 的链接。如果想优雅的关闭，请使用 Shutdown。

Close 不会关闭被劫持的链接，例如 WebSocket。

Close 返回在关闭底层监听时的任何错误。

<h3 id="Server.ListenAndServe">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2692">ListenAndServe</a>
    <a href="#Server.ListenAndServe">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) ListenAndServe() <a href="/builtin/#error">error</a></pre>

ListenAndServe 监听一个 TCP 地址 srv.Addr 并调用请求处理函数来处理来自 TLS 链接的请求。接收到的链接会启用 TCP keep-alives。如果 srv.Addr 为空， 会使用 ":http"。ListenAndServe 总是返回一个非 nil 错误。

<h3 id="Server.ListenAndServeTLS">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L3009">ListenAndServeTLS</a>
    <a href="#Server.ListenAndServeTLS">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) ListenAndServeTLS(certFile, keyFile <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

ListenAndServeTLS 监听一个 TCP 地址 srv.Addr 并且点用请求处理函数来处理 TLS 链接。接收到的链接会被启用 TCP keep-alive。

如果 TLSConfig.Certificates 和 TLSConfig.GetCertificate 都没有被填充那么必须提供包含服务器的证书和对应私钥的文件名。如果证书是被证书颁发机构签名，certFile 应该是服务器证书、中间器和 CA 证书串。

如果 srv.Addr 为空，使用 `:https`。

ListenAndServeTLS r 一直返回非 nil 错误。

<h3 id="Server.RegisterOnShutdown">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2583">RegisterOnShutdown</a>
    <a href="#Server.RegisterOnShutdown">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) RegisterOnShutdown(f func())</pre>

RegisterOnShutdown 注册一个在服务器关闭时执行的函数。它对于关闭那些接受 NPN/ALPN 协议升级或已经被劫持的链接非常有用。这个函数应该根据协议关闭链接，但不等待关闭完成。

<h3 id="Server.Serve">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2743">Serve</a>
    <a href="#Server.Serve">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) Serve(l <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>) <a href="/builtin/#error">error</a></pre>

Serve 在 l 中接受链接。并为每个链接创建一个新的 goroutine。goroutine 会读取请求然后调用 srv.Handler 进行响应。

为了支持 HTTP/2 在调用 Serve 之前 srv.TLSConfig 应该根据 TLS 配置初始化。如果 srv.TLSConfig 非空并且在 Config.NextProtos 中不包含 "h2"，说明不支持 HTTP/2。

Serve 总是返回一个非 nil 错误。在 Close 或者 Shutdown 之后，返回 ErrServerClosed 错误。

<h3 id="Server.ServeTLS">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2806">ServeTLS</a>
    <a href="#Server.ServeTLS">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) ServeTLS(l <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>, certFile, keyFile <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

ServeTLS 在 l 中接受请求链接。并为每个链接创建一个新的 goroutine。goroutine 会读取请求然后调用 srv.Handler 进行响应。

如果 TLSConfig.Certificates 和 TLSConfig.GetCertificate 都没有被填充那么必须提供包含服务器的证书和对应私钥的文件名。如果证书是被证书颁发机构签名的，certFile 应该是服务器证书、中间证书和 CA 证书组成的串。

为了支持 HTTP/2 在调用 Serve 之前 srv.TLSConfig 应该根据 TLS 配置初始化。如果 srv.TLSConfig 非空并且在 Config.NextProtos 中不包含 "h2"，说明不支持 HTTP/2。

ServeTLS 总是返回一个非 nil 错误。在 Close 或者 Shutdown 之后，返回 ErrServerClosed 错误。

<h3 id="Server.SetKeepAlivesEnabled">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2889">SetKeepAlivesEnabled</a>
    <a href="#Server.SetKeepAlivesEnabled">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) SetKeepAlivesEnabled(v <a href="/builtin/#bool">bool</a>)</pre>

SetKeepAlivesEnabled 控制 HTTP keep-alives 是否开启。keep-alives 默认是开启的。只有一些资源限制严重或者服务器正在关闭才会禁用它。

<h3 id="Server.Shutdown">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/server.go#L2552">Shutdown</a>
    <a href="#Server.Shutdown">¶</a></h3>
<pre>func (srv *<a href="#Server">Server</a>) Shutdown(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>) <a href="/builtin/#error">error</a></pre>

Shutdown 不会立即关闭还在活跃的链接。Shutdown 先关闭所有的监听端口，然后关闭空闲的链接，然后等待活跃的链接变成空闲链接然后关闭它。如果 context 在关闭之前过期，Shutdown 会返回 context 的错误。否则返回关闭服务器监听端口发生的错误。

一旦调用 Shutdown，Serve、ListenAndServe 和 ListenAndServeTLS 会立即返回 ErrServerClosed。确保程序不会退出等待 Shutdown 执行完成。

Shutdown 不会企图关闭或等待劫持链接例如 WebSockets。Shutdown 的调用者应该分别通知这些长链接关闭并等待它们关闭完成。

<a id="exampleServer_Shutdown"></a>
例:

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

<h2 id="Transport">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/transport.go#L75">Transport</a>
    <a href="#Transport">¶</a></h2>
<pre>type Transport struct {

<span id="Transport.Proxy"></span>    <span class="comment">// Proxy 是一个返回指定请求代理的函数。如果该函数返回一个非 nil 错误。请求会因为错误中断。</span>
    <span class="comment">//</span>
    <span class="comment">// 代理的类型由 URL 的协议决定。如果协议部分为空，默认为 http 代理。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果 Proxy 是 nil 或者返回一个 nil 的 *URL，不会使用代理。</span>
    Proxy func(*<a href="#Request">Request</a>) (*<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, <a href="/builtin/#error">error</a>)

<span id="Transport.DialContext"></span>    <span class="comment">// DialContext 指定创建非加密链接的函数。</span>
    <span class="comment">// 如果 DialContext 为 nil (并且弃用的 Dial 也为 nil), transport 会默认使用 net 包。</span>
    DialContext func(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, network, addr <a href="/builtin/#string">string</a>) (<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, <a href="/builtin/#error">error</a>)

<span id="Transport.Dial"></span>    <span class="comment">// Dial 创建非加密链接的函数。</span>
    <span class="comment">//</span>
    <span class="comment">// 弃用：请使用 DialContext，它允许 transport 在不需要的时候取消链接。</span>
    <span class="comment">// 如果都设置了，会优先使用 DialContext。</span>
    Dial func(network, addr <a href="/builtin/#string">string</a>) (<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, <a href="/builtin/#error">error</a>)

<span id="Transport.DialTLS"></span>    <span class="comment">// DialTLS 指定一个可选函数用来为没有代理 HTTPS 的请求创建 TLS 链接。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果 DialTLS 为 nil，会使用 Dial 和 TLSClientConfig。</span>
    <span class="comment">//</span>
    <span class="comment">// 如果设置了 DialTLS，HTTPS 请求就不会使用 Dial 并忽略 TLSClientConfig 和 TLSHandshakeTimeout。我们返回的 net.Conn 链接认为已经完成了 TLS 握手。</span>
    DialTLS func(network, addr <a href="/builtin/#string">string</a>) (<a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, <a href="/builtin/#error">error</a>)

<span id="Transport.TLSClientConfig"></span>    <span class="comment">// TLSClientConfig 指定 tls.Client 使用的 TLS 协议。</span>
    <span class="comment">// 如果为 nil，会使用默认配置。</span>
    <span class="comment">// 如果不为 nil，HTTP/2 支持可能默认不是启用状态。</span>
    TLSClientConfig *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#Config">Config</a>

<span id="Transport.TLSHandshakeTimeout"></span>    <span class="comment">// TLSHandshakeTimeout 指定最大等待 TLS 握手的时间。零值代表没有超时时间。</span>
    TLSHandshakeTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Transport.DisableKeepAlives"></span>    <span class="comment">// DisableKeepAlives，如果为 true，不同请求不会复用 TCP 链接。</span>
    DisableKeepAlives <a href="/builtin/#bool">bool</a>

<span id="Transport.DisableCompression"></span>    <span class="comment">// DisableCompression 如果为 true，Transport 即使使用 Accept-Encoding: gzip 请求头也不会压缩请求。 </span>
    <span class="comment">// 如果 Transport 自己发送了一个经过 gzip 的请求，并获取到了 gzip 的响应，这显然需要对 Response.Body 进行解码。然而如果用户明确的请求压缩它它并不会自动的压缩。</span>
    DisableCompression <a href="/builtin/#bool">bool</a>

<span id="Transport.MaxIdleConns"></span>    <span class="comment">// MaxIdleConns 控制包括所有主机的最大空闲（keep-alive）链接数。0 代表不限制。</span>
    MaxIdleConns <a href="/builtin/#int">int</a>

<span id="Transport.MaxIdleConnsPerHost"></span>    <span class="comment">// MaxIdleConnsPerHost, 如果不为 0，会控制每个主机的最大空闲(keep-alive) 链接数。如果为零值，会使用 DefaultMaxIdleConnsPerHost。</span>
    MaxIdleConnsPerHost <a href="/builtin/#int">int</a>

<span id="Transport.IdleConnTimeout"></span>    <span class="comment">// IdleConnTimeout 是空闲链接在关闭前的最大有效时间。</span>
    <span class="comment">// 零值代表不限制。</span>
    IdleConnTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Transport.ResponseHeaderTimeout"></span>    <span class="comment">// ResponseHeaderTimeout 如果非零，限制在写入请求成功后等待响应头的时间。</span>
    <span class="comment">// 这个时间不包括读取响应体的时间。</span>
    ResponseHeaderTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Transport.ExpectContinueTimeout"></span>    <span class="comment">// ExpectContinueTimeout 代表在请求头中包含 Expect: 100-continue 时，写入请求头后第一次获取响应头的超时时间。零值意味着请求体会被立刻发送而不等待服务器批准。</span>

    <span class="comment">// 这个时间不包括发送请求头的时间。</span>
    ExpectContinueTimeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Transport.TLSNextProto"></span>    <span class="comment">// TLSNextProto 指定 Transport 在 NPN/ALPN 协议协商以后如何在备用协议间切换。如果 Transport 使用非空协议名发起一个 TLS 链接并且在 TLSNextProto 中包含该协议的键，那么请求根据权限（例如："example.com" 或者 "example.com:1234"）调用这个函数。这个函数必须返回一个 RoundTripper 来处理请求。</span>
    <span class="comment">// 如果 TLSNextProto 不为 nil，不会自动启用对 HTTP/2 的支持。</span>
    TLSNextProto map[<a href="/builtin/#string">string</a>]func(authority <a href="/builtin/#string">string</a>, c *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#Conn">Conn</a>) <a href="#RoundTripper">RoundTripper</a>

<span id="Transport.ProxyConnectHeader"></span>    <span class="comment">// ProxyConnectHeader 可选的指定发起 CONNECT 请求时的请求头。</span>
    ProxyConnectHeader <a href="#Header">Header</a>

<span id="Transport.MaxResponseHeaderBytes"></span>    <span class="comment">// MaxResponseHeaderBytes 限制服务器读取响应头的最大字节数。</span>
    <span class="comment">// 零值意味着默认限制。</span>
    MaxResponseHeaderBytes <a href="/builtin/#int64">int64</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Transport 是一个支持 HTTP，HTTPS 和 HTTP 代理（HTTP 或 HTTPS 的 CONNECT）的 RoundTripper 实现。

默认情况下，Transport 会缓存链接进行复用。这可能导致在访问多个主机的时候导致打开过多的链接。

这个行为可以通过调用 CloseIdleConnections 方法，设置 MaxIdleConnsPerHost 和 DisableKeepAlives 来改变。

Transports 应该使用重用策略来代替按需创建的策略。Transports 可以安全的被多个 goroutine 同时使用。

Transport 用来创建比较原始的  HTTP 和 HTTPS 请求，对于更加实用的高级功能，例如 cookies 或者重定向，请看 Client。

对于 HTTP 的 URL Transport 使用 HTTP/1.1，对于 HTTPS 的 URL Transport 可以使用 HTTP/1.1 或 HTTP/2，这依赖服务器是否支持 HTTP/2 和 Transport 是如何配置的。DefaultTransport 支持 HTTP/2。为了让 transport 显式的启用 HTTP/2，可以使用 golang.org/x/net/http2 并且调用 ConfigureTransport。关于更多的关于 HTTP/2 可以查看包文档。



Transport 会在处理 HTTPS 请求时发送 CONNECT 请求到一个代理来供自己使用，但是 Transport 一般不会发送一个 CONNECT 请求。因此经过 RoundTrip 的请求中不应该有一个叫 "CONNECT" 的方法，作为 Go 的 HTTP/1.x 实现不支持全双工链接的请求体（可以获取响应的时候发送请求）。Go 的 HTTP/2 实现支持全双工链接，但是一些 CONNET 代理都是 HTTP/1.x。


<h3 id="Transport.CancelRequest">func (*Transport) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/transport.go#L548">CancelRequest</a>
    <a href="#Transport.CancelRequest">¶</a></h3>
<pre>func (t *<a href="#Transport">Transport</a>) CancelRequest(req *<a href="#Request">Request</a>)</pre>

CancelRequest 会通过关闭链接来取消请求。

CancelRequest 只应该在 RoundTrip 返回以后调用。

弃用：使用 Request.WithContext 来创建一个带可以取消 context 的请求。CancelRequest 不能取消 HTTP/2 请求。

<h3 id="Transport.CloseIdleConnections">func (*Transport) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/transport.go#L523">CloseIdleConnections</a>
    <a href="#Transport.CloseIdleConnections">¶</a></h3>
<pre>func (t *<a href="#Transport">Transport</a>) CloseIdleConnections()</pre>

CloseIdleConnections 关闭之前所有请求的链接。不包括 "keep-alive" 状态的链接。他不会中断任何当前正在用的链接。

<h3 id="Transport.RegisterProtocol">func (*Transport) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/transport.go#L504">RegisterProtocol</a>
    <a href="#Transport.RegisterProtocol">¶</a></h3>
<pre>func (t *<a href="#Transport">Transport</a>) RegisterProtocol(scheme <a href="/builtin/#string">string</a>, rt <a href="#RoundTripper">RoundTripper</a>)</pre>

RegisterProtocol 注册一个新的协议。Transport 将会把请求转发给 rt。

RegisterProtocol 可以使用其他包的协议实现例如："ftp" 或 "file"。

如果 rt.RoundTrip 返回 ErrSkipAltProtocol，Transport 将会处理 RoundTrip 自己，就像协议没有注册一样。

<h3 id="Transport.RoundTrip">func (*Transport) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/transport.go#L340">RoundTrip</a>
    <a href="#Transport.RoundTrip">¶</a></h3>
<pre>func (t *<a href="#Transport">Transport</a>) RoundTrip(req *<a href="#Request">Request</a>) (*<a href="#Response">Response</a>, <a href="/builtin/#error">error</a>)</pre>

RoundTrip 实现了 RoundTripper 接口。

对于高级 HTTP 客户端的支持请看 Get、Post 和 Client 类型。

## Subdirectories
- [..](..)
- [cgi](cgi/)
- [cookiejar](cookiejar/)
- [fcgi](fcgi/)
- [httptest](httptest/)
- [httptrace](httptrace/)
- [httputil](httputil/)
- [pprof](pprof/)
