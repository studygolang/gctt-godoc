version: 1.9.2
## package rpc

  `import "net/rpc"`

## Overview

Package rpc provides access to the exported methods of an object across a
network or other I/O connection. A server registers an object, making it visible
as a service with the name of the type of the object. After registration,
exported methods of the object will be accessible remotely. A server may
register multiple objects (services) of different types but it is an error to
register multiple objects of the same type.

Only methods that satisfy these criteria will be made available for remote
access; other methods will be ignored:

    - the method's type is exported.
    - the method is exported.
    - the method has two arguments, both exported (or builtin) types.
    - the method's second argument is a pointer.
    - the method has return type error.

In effect, the method must look schematically like

    func (t *T) MethodName(argType T1, replyType *T2) error

where T1 and T2 can be marshaled by encoding/gob. These requirements apply even
if a different codec is used. (In the future, these requirements may soften for
custom codecs.)

The method's first argument represents the arguments provided by the caller; the
second argument represents the result parameters to be returned to the caller.
The method's return value, if non-nil, is passed back as a string that the
client sees as if created by errors.New. If an error is returned, the reply
parameter will not be sent back to the client.

The server may handle requests on a single connection by calling ServeConn. More
typically it will create a network listener and call Accept or, for an HTTP
listener, HandleHTTP and http.Serve.

A client wishing to use the service establishes a connection and then invokes
NewClient on the connection. The convenience function Dial (DialHTTP) performs
both steps for a raw network connection (an HTTP connection). The resulting
Client object has two methods, Call and Go, that specify the service and method
to call, a pointer containing the arguments, and a pointer to receive the result
parameters.

The Call method waits for the remote call to complete while the Go method
launches the call asynchronously and signals completion using the Call
structure's Done channel.

Unless an explicit codec is set up, package encoding/gob is used to transport
the data.

Here is a simple example. A server wishes to export an object of type Arith:

    package server

    import "errors"

    type Args struct {
    	A, B int
    }

    type Quotient struct {
    	Quo, Rem int
    }

    type Arith int

    func (t *Arith) Multiply(args *Args, reply *int) error {
    	*reply = args.A * args.B
    	return nil
    }

    func (t *Arith) Divide(args *Args, quo *Quotient) error {
    	if args.B == 0 {
    		return errors.New("divide by zero")
    	}
    	quo.Quo = args.A / args.B
    	quo.Rem = args.A % args.B
    	return nil
    }

The server calls (for HTTP service):

    arith := new(Arith)
    rpc.Register(arith)
    rpc.HandleHTTP()
    l, e := net.Listen("tcp", ":1234")
    if e != nil {
    	log.Fatal("listen error:", e)
    }
    go http.Serve(l, nil)

At this point, clients can see a service "Arith" with methods "Arith.Multiply"
and "Arith.Divide". To invoke one, a client first dials the server:

    client, err := rpc.DialHTTP("tcp", serverAddress + ":1234")
    if err != nil {
    	log.Fatal("dialing:", err)
    }

Then it can make a remote call:

    // Synchronous call
    args := &server.Args{7,8}
    var reply int
    err = client.Call("Arith.Multiply", args, &reply)
    if err != nil {
    	log.Fatal("arith error:", err)
    }
    fmt.Printf("Arith: %d*%d=%d", args.A, args.B, reply)

or

    // Asynchronous call
    quotient := new(Quotient)
    divCall := client.Go("Arith.Divide", args, quotient, nil)
    replyCall := <-divCall.Done	// will be equal to divCall
    // check errors, print, etc.

A server implementation will often provide a simple, type-safe wrapper for the
client.

The net/rpc package is frozen and is not accepting new features.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func Accept(lis net.Listener)](#Accept)
- [func HandleHTTP()](#HandleHTTP)
- [func Register(rcvr interface{}) error](#Register)
- [func RegisterName(name string, rcvr interface{}) error](#RegisterName)
- [func ServeCodec(codec ServerCodec)](#ServeCodec)
- [func ServeConn(conn io.ReadWriteCloser)](#ServeConn)
- [func ServeRequest(codec ServerCodec) error](#ServeRequest)
- [type Call](#Call)
- [type Client](#Client)
  - [func Dial(network, address string) (*Client, error)](#Dial)
  - [func DialHTTP(network, address string) (*Client, error)](#DialHTTP)
  - [func DialHTTPPath(network, address, path string) (*Client, error)](#DialHTTPPath)
  - [func NewClient(conn io.ReadWriteCloser) *Client](#NewClient)
  - [func NewClientWithCodec(codec ClientCodec) *Client](#NewClientWithCodec)
  - [func (client *Client) Call(serviceMethod string, args interface{}, reply interface{}) error](#Client.Call)
  - [func (client *Client) Close() error](#Client.Close)
  - [func (client *Client) Go(serviceMethod string, args interface{}, reply interface{}, done chan *Call) *Call](#Client.Go)
- [type ClientCodec](#ClientCodec)
- [type Request](#Request)
- [type Response](#Response)
- [type Server](#Server)
  - [func NewServer() *Server](#NewServer)
  - [func (server *Server) Accept(lis net.Listener)](#Server.Accept)
  - [func (server *Server) HandleHTTP(rpcPath, debugPath string)](#Server.HandleHTTP)
  - [func (server *Server) Register(rcvr interface{}) error](#Server.Register)
  - [func (server *Server) RegisterName(name string, rcvr interface{}) error](#Server.RegisterName)
  - [func (server *Server) ServeCodec(codec ServerCodec)](#Server.ServeCodec)
  - [func (server *Server) ServeConn(conn io.ReadWriteCloser)](#Server.ServeConn)
  - [func (server *Server) ServeHTTP(w http.ResponseWriter, req *http.Request)](#Server.ServeHTTP)
  - [func (server *Server) ServeRequest(codec ServerCodec) error](#Server.ServeRequest)
- [type ServerCodec](#ServerCodec)
- [type ServerError](#ServerError)
  - [func (e ServerError) Error() string](#ServerError.Error)

### Package files
 [client.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go) [debug.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/debug.go) [server.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// Defaults used by HandleHTTP</span>
    <span id="DefaultRPCPath">DefaultRPCPath</span>   = &#34;/_goRPC_&#34;
    <span id="DefaultDebugPath">DefaultDebugPath</span> = &#34;/debug/rpc&#34;
)</pre>


<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="DefaultServer">DefaultServer</span> = <a href="#NewServer">NewServer</a>()</pre>

DefaultServer is the default instance of *Server.

<pre>var <span id="ErrShutdown">ErrShutdown</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;connection is shut down&#34;)</pre>


<h2 id="Accept">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L671">Accept</a>
    <a href="#Accept">¶</a></h2>
<pre>func Accept(lis <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>)</pre>

Accept accepts connections on the listener and serves requests to DefaultServer
for each incoming connection. Accept blocks; the caller typically invokes it in
a go statement.

<h2 id="HandleHTTP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L704">HandleHTTP</a>
    <a href="#HandleHTTP">¶</a></h2>
<pre>func HandleHTTP()</pre>

HandleHTTP registers an HTTP handler for RPC messages to DefaultServer on
DefaultRPCPath and a debugging handler on DefaultDebugPath. It is still
necessary to invoke http.Serve(), typically in a go statement.

<h2 id="Register">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L623">Register</a>
    <a href="#Register">¶</a></h2>
<pre>func Register(rcvr interface{}) <a href="/builtin/#error">error</a></pre>

Register publishes the receiver's methods in the DefaultServer.

<h2 id="RegisterName">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L627">RegisterName</a>
    <a href="#RegisterName">¶</a></h2>
<pre>func RegisterName(name <a href="/builtin/#string">string</a>, rcvr interface{}) <a href="/builtin/#error">error</a></pre>

RegisterName is like Register but uses the provided name for the type instead of
the receiver's concrete type.

<h2 id="ServeCodec">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L658">ServeCodec</a>
    <a href="#ServeCodec">¶</a></h2>
<pre>func ServeCodec(codec <a href="#ServerCodec">ServerCodec</a>)</pre>

ServeCodec is like ServeConn but uses the specified codec to decode requests and
encode responses.

<h2 id="ServeConn">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L652">ServeConn</a>
    <a href="#ServeConn">¶</a></h2>
<pre>func ServeConn(conn <a href="/io/">io</a>.<a href="/io/#ReadWriteCloser">ReadWriteCloser</a>)</pre>

ServeConn runs the DefaultServer on a single connection. ServeConn blocks,
serving the connection until the client hangs up. The caller typically invokes
ServeConn in a go statement. ServeConn uses the gob wire format (see package
gob) on the connection. To use an alternate codec, use ServeCodec.

<h2 id="ServeRequest">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L664">ServeRequest</a>
    <a href="#ServeRequest">¶</a></h2>
<pre>func ServeRequest(codec <a href="#ServerCodec">ServerCodec</a>) <a href="/builtin/#error">error</a></pre>

ServeRequest is like ServeCodec but synchronously serves a single request. It
does not close the codec upon completion.

<h2 id="Call">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L19">Call</a>
    <a href="#Call">¶</a></h2>
<pre>type Call struct {
<span id="Call.ServiceMethod"></span>    ServiceMethod <a href="/builtin/#string">string</a>      <span class="comment">// The name of the service and method to call.</span>
<span id="Call.Args"></span>    Args          interface{} <span class="comment">// The argument to the function (*struct).</span>
<span id="Call.Reply"></span>    Reply         interface{} <span class="comment">// The reply from the function (*struct).</span>
<span id="Call.Error"></span>    Error         <a href="/builtin/#error">error</a>       <span class="comment">// After completion, the error status.</span>
<span id="Call.Done"></span>    Done          chan *<a href="#Call">Call</a>  <span class="comment">// Strobes when call is complete.</span>
}</pre>

Call represents an active RPC.

<h2 id="Client">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L31">Client</a>
    <a href="#Client">¶</a></h2>
<pre>type Client struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Client represents an RPC Client. There may be multiple outstanding Calls
associated with a single Client, and a Client may be used by multiple goroutines
simultaneously.

<h3 id="Dial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L259">Dial</a>
    <a href="#Dial">¶</a></h3>
<pre>func Dial(network, address <a href="/builtin/#string">string</a>) (*<a href="#Client">Client</a>, <a href="/builtin/#error">error</a>)</pre>

Dial connects to an RPC server at the specified network address.

<h3 id="DialHTTP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L226">DialHTTP</a>
    <a href="#DialHTTP">¶</a></h3>
<pre>func DialHTTP(network, address <a href="/builtin/#string">string</a>) (*<a href="#Client">Client</a>, <a href="/builtin/#error">error</a>)</pre>

DialHTTP connects to an HTTP RPC server at the specified network address
listening on the default HTTP RPC path.

<h3 id="DialHTTPPath">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L232">DialHTTPPath</a>
    <a href="#DialHTTPPath">¶</a></h3>
<pre>func DialHTTPPath(network, address, path <a href="/builtin/#string">string</a>) (*<a href="#Client">Client</a>, <a href="/builtin/#error">error</a>)</pre>

DialHTTPPath connects to an HTTP RPC server at the specified network address and
path.

<h3 id="NewClient">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L178">NewClient</a>
    <a href="#NewClient">¶</a></h3>
<pre>func NewClient(conn <a href="/io/">io</a>.<a href="/io/#ReadWriteCloser">ReadWriteCloser</a>) *<a href="#Client">Client</a></pre>

NewClient returns a new Client to handle requests to the set of services at the
other end of the connection. It adds a buffer to the write side of the
connection so the header and payload are sent as a unit.

<h3 id="NewClientWithCodec">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L186">NewClientWithCodec</a>
    <a href="#NewClientWithCodec">¶</a></h3>
<pre>func NewClientWithCodec(codec <a href="#ClientCodec">ClientCodec</a>) *<a href="#Client">Client</a></pre>

NewClientWithCodec is like NewClient but uses the specified codec to encode
requests and decode responses.

<h3 id="Client.Call">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L306">Call</a>
    <a href="#Client.Call">¶</a></h3>
<pre>func (client *<a href="#Client">Client</a>) Call(serviceMethod <a href="/builtin/#string">string</a>, args interface{}, reply interface{}) <a href="/builtin/#error">error</a></pre>

Call invokes the named function, waits for it to complete, and returns its error
status.

<h3 id="Client.Close">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L269">Close</a>
    <a href="#Client.Close">¶</a></h3>
<pre>func (client *<a href="#Client">Client</a>) Close() <a href="/builtin/#error">error</a></pre>

Close calls the underlying codec's Close method. If the connection is already
shutting down, ErrShutdown is returned.

<h3 id="Client.Go">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L284">Go</a>
    <a href="#Client.Go">¶</a></h3>
<pre>func (client *<a href="#Client">Client</a>) Go(serviceMethod <a href="/builtin/#string">string</a>, args interface{}, reply interface{}, done chan *<a href="#Call">Call</a>) *<a href="#Call">Call</a></pre>

Go invokes the function asynchronously. It returns the Call structure
representing the invocation. The done channel will signal when the call is
complete by returning the same Call object. If done is nil, Go will allocate a
new channel. If non-nil, done must be buffered or Go will deliberately crash.

<h2 id="ClientCodec">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L52">ClientCodec</a>
    <a href="#ClientCodec">¶</a></h2>
<pre>type ClientCodec interface {
    <span class="comment">// WriteRequest must be safe for concurrent use by multiple goroutines.</span>
    WriteRequest(*<a href="#Request">Request</a>, interface{}) <a href="/builtin/#error">error</a>
    ReadResponseHeader(*<a href="#Response">Response</a>) <a href="/builtin/#error">error</a>
    ReadResponseBody(interface{}) <a href="/builtin/#error">error</a>

    Close() <a href="/builtin/#error">error</a>
}</pre>

A ClientCodec implements writing of RPC requests and reading of RPC responses
for the client side of an RPC session. The client calls WriteRequest to write a
request to the connection and calls ReadResponseHeader and ReadResponseBody in
pairs to read responses. The client calls Close when finished with the
connection. ReadResponseBody may be called with a nil argument to force the body
of the response to be read and then discarded.

<h2 id="Request">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L162">Request</a>
    <a href="#Request">¶</a></h2>
<pre>type Request struct {
<span id="Request.ServiceMethod"></span>    ServiceMethod <a href="/builtin/#string">string</a> <span class="comment">// format: &#34;Service.Method&#34;</span>
<span id="Request.Seq"></span>    Seq           <a href="/builtin/#uint64">uint64</a> <span class="comment">// sequence number chosen by client</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Request is a header written before every RPC call. It is used internally but
documented here as an aid to debugging, such as when analyzing network traffic.

<h2 id="Response">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L171">Response</a>
    <a href="#Response">¶</a></h2>
<pre>type Response struct {
<span id="Response.ServiceMethod"></span>    ServiceMethod <a href="/builtin/#string">string</a> <span class="comment">// echoes that of the Request</span>
<span id="Response.Seq"></span>    Seq           <a href="/builtin/#uint64">uint64</a> <span class="comment">// echoes that of the request</span>
<span id="Response.Error"></span>    Error         <a href="/builtin/#string">string</a> <span class="comment">// error, if any.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Response is a header written before every RPC return. It is used internally but
documented here as an aid to debugging, such as when analyzing network traffic.

<h2 id="Server">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L179">Server</a>
    <a href="#Server">¶</a></h2>
<pre>type Server struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Server represents an RPC Server.

<h3 id="NewServer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L188">NewServer</a>
    <a href="#NewServer">¶</a></h3>
<pre>func NewServer() *<a href="#Server">Server</a></pre>

NewServer returns a new Server.

<h3 id="Server.Accept">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L611">Accept</a>
    <a href="#Server.Accept">¶</a></h3>
<pre>func (server *<a href="#Server">Server</a>) Accept(lis <a href="/net/">net</a>.<a href="/net/#Listener">Listener</a>)</pre>

Accept accepts connections on the listener and serves requests for each incoming
connection. Accept blocks until the listener returns a non-nil error. The caller
typically invokes Accept in a go statement.

<h3 id="Server.HandleHTTP">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L696">HandleHTTP</a>
    <a href="#Server.HandleHTTP">¶</a></h3>
<pre>func (server *<a href="#Server">Server</a>) HandleHTTP(rpcPath, debugPath <a href="/builtin/#string">string</a>)</pre>

HandleHTTP registers an HTTP handler for RPC messages on rpcPath, and a
debugging handler on debugPath. It is still necessary to invoke http.Serve(),
typically in a go statement.

<h3 id="Server.Register">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L221">Register</a>
    <a href="#Server.Register">¶</a></h3>
<pre>func (server *<a href="#Server">Server</a>) Register(rcvr interface{}) <a href="/builtin/#error">error</a></pre>

Register publishes in the server the set of methods of the receiver value that
satisfy the following conditions:

    - exported method of exported type
    - two arguments, both of exported type
    - the second argument is a pointer
    - one return value, of type error

It returns an error if the receiver is not an exported type or has no suitable
methods. It also logs the error using package log. The client accesses each
method using a string of the form "Type.Method", where Type is the receiver's
concrete type.

<h3 id="Server.RegisterName">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L227">RegisterName</a>
    <a href="#Server.RegisterName">¶</a></h3>
<pre>func (server *<a href="#Server">Server</a>) RegisterName(name <a href="/builtin/#string">string</a>, rcvr interface{}) <a href="/builtin/#error">error</a></pre>

RegisterName is like Register but uses the provided name for the type instead of
the receiver's concrete type.

<h3 id="Server.ServeCodec">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L447">ServeCodec</a>
    <a href="#Server.ServeCodec">¶</a></h3>
<pre>func (server *<a href="#Server">Server</a>) ServeCodec(codec <a href="#ServerCodec">ServerCodec</a>)</pre>

ServeCodec is like ServeConn but uses the specified codec to decode requests and
encode responses.

<h3 id="Server.ServeConn">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L434">ServeConn</a>
    <a href="#Server.ServeConn">¶</a></h3>
<pre>func (server *<a href="#Server">Server</a>) ServeConn(conn <a href="/io/">io</a>.<a href="/io/#ReadWriteCloser">ReadWriteCloser</a>)</pre>

ServeConn runs the server on a single connection. ServeConn blocks, serving the
connection until the client hangs up. The caller typically invokes ServeConn in
a go statement. ServeConn uses the gob wire format (see package gob) on the
connection. To use an alternate codec, use ServeCodec.

<h3 id="Server.ServeHTTP">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L677">ServeHTTP</a>
    <a href="#Server.ServeHTTP">¶</a></h3>
<pre>func (server *<a href="#Server">Server</a>) ServeHTTP(w <a href="/net/http/">http</a>.<a href="/net/http/#ResponseWriter">ResponseWriter</a>, req *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>)</pre>

ServeHTTP implements an http.Handler that answers RPC requests.

<h3 id="Server.ServeRequest">func (*Server) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L472">ServeRequest</a>
    <a href="#Server.ServeRequest">¶</a></h3>
<pre>func (server *<a href="#Server">Server</a>) ServeRequest(codec <a href="#ServerCodec">ServerCodec</a>) <a href="/builtin/#error">error</a></pre>

ServeRequest is like ServeCodec but synchronously serves a single request. It
does not close the codec upon completion.

<h2 id="ServerCodec">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/server.go#L638">ServerCodec</a>
    <a href="#ServerCodec">¶</a></h2>
<pre>type ServerCodec interface {
    ReadRequestHeader(*<a href="#Request">Request</a>) <a href="/builtin/#error">error</a>
    ReadRequestBody(interface{}) <a href="/builtin/#error">error</a>
    <span class="comment">// WriteResponse must be safe for concurrent use by multiple goroutines.</span>
    WriteResponse(*<a href="#Response">Response</a>, interface{}) <a href="/builtin/#error">error</a>

    Close() <a href="/builtin/#error">error</a>
}</pre>

A ServerCodec implements reading of RPC requests and writing of RPC responses
for the server side of an RPC session. The server calls ReadRequestHeader and
ReadRequestBody in pairs to read requests from the connection, and it calls
WriteResponse to write a response back. The server calls Close when finished
with the connection. ReadRequestBody may be called with a nil argument to force
the body of the request to be read and discarded.

<h2 id="ServerError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L10">ServerError</a>
    <a href="#ServerError">¶</a></h2>
<pre>type ServerError <a href="/builtin/#string">string</a></pre>

ServerError represents an error that has been returned from the remote side of
the RPC connection.

<h3 id="ServerError.Error">func (ServerError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rpc/client.go#L12">Error</a>
    <a href="#ServerError.Error">¶</a></h3>
<pre>func (e <a href="#ServerError">ServerError</a>) Error() <a href="/builtin/#string">string</a></pre>


## Subdirectories
- [..](..)
- [jsonrpc](jsonrpc/)
