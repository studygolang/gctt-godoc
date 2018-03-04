version: 1.10
## package cgi

  `import "net/http/cgi"`

## Overview

Package cgi implements CGI (Common Gateway Interface) as specified in RFC 3875.

Note that using CGI means starting a new process to handle each request, which
is typically less efficient than using a long-running server. This package is
intended primarily for compatibility with existing systems.

## Index

- [func Request() (*http.Request, error)](#Request)
- [func RequestFromMap(params map[string]string) (*http.Request, error)](#RequestFromMap)
- [func Serve(handler http.Handler) error](#Serve)
- [type Handler](#Handler)
  - [func (h *Handler) ServeHTTP(rw http.ResponseWriter, req *http.Request)](#Handler.ServeHTTP)

### Package files
 [child.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cgi/child.go) [host.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cgi/host.go)

<h2 id="Request">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cgi/child.go#L19">Request</a>
    <a href="#Request">¶</a></h2>
<pre>func Request() (*<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>, <a href="/builtin/#error">error</a>)</pre>

Request returns the HTTP request as represented in the current environment. This
assumes the current program is being run by a web server in a CGI environment.
The returned Request's Body is populated, if applicable.

<h2 id="RequestFromMap">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cgi/child.go#L42">RequestFromMap</a>
    <a href="#RequestFromMap">¶</a></h2>
<pre>func RequestFromMap(params map[<a href="/builtin/#string">string</a>]<a href="/builtin/#string">string</a>) (*<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>, <a href="/builtin/#error">error</a>)</pre>

RequestFromMap creates an http.Request from CGI variables. The returned
Request's Body field is not populated.

<h2 id="Serve">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cgi/child.go#L136">Serve</a>
    <a href="#Serve">¶</a></h2>
<pre>func Serve(handler <a href="/net/http/">http</a>.<a href="/net/http/#Handler">Handler</a>) <a href="/builtin/#error">error</a></pre>

Serve executes the provided Handler on the currently active CGI request, if any.
If there's no current CGI environment an error is returned. The provided handler
may be nil to use http.DefaultServeMux.

<h2 id="Handler">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cgi/host.go#L37">Handler</a>
    <a href="#Handler">¶</a></h2>
<pre>type Handler struct {
<span id="Handler.Path"></span>    Path <a href="/builtin/#string">string</a> <span class="comment">// path to the CGI executable</span>
<span id="Handler.Root"></span>    Root <a href="/builtin/#string">string</a> <span class="comment">// root URI prefix of handler or empty for &#34;/&#34;</span>

<span id="Handler.Dir"></span>    <span class="comment">// Dir specifies the CGI executable&#39;s working directory.</span>
    <span class="comment">// If Dir is empty, the base directory of Path is used.</span>
    <span class="comment">// If Path has no base directory, the current working</span>
    <span class="comment">// directory is used.</span>
    Dir <a href="/builtin/#string">string</a>

<span id="Handler.Env"></span>    Env        []<a href="/builtin/#string">string</a>    <span class="comment">// extra environment variables to set, if any, as &#34;key=value&#34;</span>
<span id="Handler.InheritEnv"></span>    InheritEnv []<a href="/builtin/#string">string</a>    <span class="comment">// environment variables to inherit from host, as &#34;key&#34;</span>
<span id="Handler.Logger"></span>    Logger     *<a href="/log/">log</a>.<a href="/log/#Logger">Logger</a> <span class="comment">// optional log for errors or nil to use log.Print</span>
<span id="Handler.Args"></span>    Args       []<a href="/builtin/#string">string</a>    <span class="comment">// optional arguments to pass to child process</span>
<span id="Handler.Stderr"></span>    Stderr     <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>   <span class="comment">// optional stderr for the child process; nil means os.Stderr</span>

<span id="Handler.PathLocationHandler"></span>    <span class="comment">// PathLocationHandler specifies the root http Handler that</span>
    <span class="comment">// should handle internal redirects when the CGI process</span>
    <span class="comment">// returns a Location header value starting with a &#34;/&#34;, as</span>
    <span class="comment">// specified in RFC 3875 § 6.3.2. This will likely be</span>
    <span class="comment">// http.DefaultServeMux.</span>
    <span class="comment">//</span>
    <span class="comment">// If nil, a CGI response with a local URI path is instead sent</span>
    <span class="comment">// back to the client and not redirected internally.</span>
    PathLocationHandler <a href="/net/http/">http</a>.<a href="/net/http/#Handler">Handler</a>
}</pre>

Handler runs an executable in a subprocess with a CGI environment.

<h3 id="Handler.ServeHTTP">func (*Handler) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cgi/host.go#L96">ServeHTTP</a>
    <a href="#Handler.ServeHTTP">¶</a></h3>
<pre>func (h *<a href="#Handler">Handler</a>) ServeHTTP(rw <a href="/net/http/">http</a>.<a href="/net/http/#ResponseWriter">ResponseWriter</a>, req *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>)</pre>



