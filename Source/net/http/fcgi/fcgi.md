version: 1.10
## package fcgi

  `import "net/http/fcgi"`

## Overview

Package fcgi implements the FastCGI protocol.

The protocol is not an official standard and the original documentation is no
longer online. See the Internet Archive's mirror at:
https://web.archive.org/web/20150420080736/http://www.fastcgi.com/drupal/node/6?q=node/22

Currently only the responder role is supported.

## Index

- [Variables](#pkg-variables)
- [func ProcessEnv(r *http.Request) map[string]string](#ProcessEnv)
- [func Serve(l net.Listener, handler http.Handler) error](#Serve)

### Package files
 [child.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fcgi/child.go) [fcgi.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fcgi/fcgi.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrConnClosed">ErrConnClosed</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;fcgi: connection to web server closed&#34;)</pre>

ErrConnClosed is returned by Read when a handler attempts to read the body of a
request after the connection to the web server has been closed.

<pre>var <span id="ErrRequestAborted">ErrRequestAborted</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;fcgi: request aborted by web server&#34;)</pre>

ErrRequestAborted is returned by Read when a handler attempts to read the body
of a request that has been aborted by the web server.

<h2 id="ProcessEnv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fcgi/child.go#L348">ProcessEnv</a>
    <a href="#ProcessEnv">¶</a></h2>
<pre>func ProcessEnv(r *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>) map[<a href="/builtin/#string">string</a>]<a href="/builtin/#string">string</a></pre>

ProcessEnv returns FastCGI environment variables associated with the request r
for which no effort was made to be included in the request itself - the data is
hidden in the request's context. As an example, if REMOTE_USER is set for a
request, it will not be found anywhere in r, but it will be included in
ProcessEnv's response (via r's context).

<h2 id="Serve">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/fcgi/child.go#L321">Serve</a>
    <a href="#Serve">¶</a></h2>
<pre>func Serve(l <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>, handler <a href="/net/http/">http</a>.<a href="/net/http/#Handler">Handler</a>) <a href="/builtin/#error">error</a></pre>

Serve accepts incoming FastCGI connections on the listener l, creating a new
goroutine for each. The goroutine reads requests and then calls handler to reply
to them. If l is nil, Serve accepts connections from os.Stdin. If handler is
nil, http.DefaultServeMux is used.


