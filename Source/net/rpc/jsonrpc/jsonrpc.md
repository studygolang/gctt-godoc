version: 1.10
## package jsonrpc

  `import "net/rpc/jsonrpc"`

## Overview

Package jsonrpc implements a JSON-RPC 1.0 ClientCodec and ServerCodec for the
rpc package. For JSON-RPC 2.0 support, see https://godoc.org/?q=json-rpc+2.0

## Index

- [func Dial(network, address string) (*rpc.Client, error)](#Dial)
- [func NewClient(conn io.ReadWriteCloser) *rpc.Client](#NewClient)
- [func NewClientCodec(conn io.ReadWriteCloser) rpc.ClientCodec](#NewClientCodec)
- [func NewServerCodec(conn io.ReadWriteCloser) rpc.ServerCodec](#NewServerCodec)
- [func ServeConn(conn io.ReadWriteCloser)](#ServeConn)

### Package files
 [client.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/rpc/jsonrpc/client.go) [server.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/rpc/jsonrpc/server.go)

<h2 id="Dial">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/rpc/jsonrpc/client.go#L108">Dial</a>
    <a href="#Dial">¶</a></h2>
<pre>func Dial(network, address <a href="/builtin/#string">string</a>) (*<a href="/net/rpc/">rpc</a>.<a href="/net/rpc/#Client">Client</a>, <a href="/builtin/#error">error</a>)</pre>

Dial connects to a JSON-RPC server at the specified network address.

<h2 id="NewClient">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/rpc/jsonrpc/client.go#L103">NewClient</a>
    <a href="#NewClient">¶</a></h2>
<pre>func NewClient(conn <a href="/io/">io</a>.<a href="/io/#ReadWriteCloser">ReadWriteCloser</a>) *<a href="/net/rpc/">rpc</a>.<a href="/net/rpc/#Client">Client</a></pre>

NewClient returns a new rpc.Client to handle requests to the set of services at
the other end of the connection.

<h2 id="NewClientCodec">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/rpc/jsonrpc/client.go#L27">NewClientCodec</a>
    <a href="#NewClientCodec">¶</a></h2>
<pre>func NewClientCodec(conn <a href="/io/">io</a>.<a href="/io/#ReadWriteCloser">ReadWriteCloser</a>) <a href="/net/rpc/">rpc</a>.<a href="/net/rpc/#ClientCodec">ClientCodec</a></pre>

NewClientCodec returns a new rpc.ClientCodec using JSON-RPC on conn.

<h2 id="NewServerCodec">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/rpc/jsonrpc/server.go#L27">NewServerCodec</a>
    <a href="#NewServerCodec">¶</a></h2>
<pre>func NewServerCodec(conn <a href="/io/">io</a>.<a href="/io/#ReadWriteCloser">ReadWriteCloser</a>) <a href="/net/rpc/">rpc</a>.<a href="/net/rpc/#ServerCodec">ServerCodec</a></pre>

NewServerCodec returns a new rpc.ServerCodec using JSON-RPC on conn.

<h2 id="ServeConn">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/rpc/jsonrpc/server.go#L122">ServeConn</a>
    <a href="#ServeConn">¶</a></h2>
<pre>func ServeConn(conn <a href="/io/">io</a>.<a href="/io/#ReadWriteCloser">ReadWriteCloser</a>)</pre>

ServeConn runs the JSON-RPC server on a single connection. ServeConn blocks,
serving the connection until the client hangs up. The caller typically invokes
ServeConn in a go statement.


