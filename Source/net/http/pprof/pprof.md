version: 1.10
## package pprof

  `import "net/http/pprof"`

## Overview

Package pprof serves via its HTTP server runtime profiling data in the format
expected by the pprof visualization tool.

The package is typically only imported for the side effect of registering its
HTTP handlers. The handled paths all begin with /debug/pprof/.

To use pprof, link this package into your program:

    import _ "net/http/pprof"

If your application is not already running an http server, you need to start
one. Add "net/http" and "log" to your imports and the following code to your
main function:

    go func() {
    	log.Println(http.ListenAndServe("localhost:6060", nil))
    }()

Then use the pprof tool to look at the heap profile:

    go tool pprof http://localhost:6060/debug/pprof/heap

Or to look at a 30-second CPU profile:

    go tool pprof http://localhost:6060/debug/pprof/profile

Or to look at the goroutine blocking profile, after calling
runtime.SetBlockProfileRate in your program:

    go tool pprof http://localhost:6060/debug/pprof/block

Or to collect a 5-second execution trace:

    wget http://localhost:6060/debug/pprof/trace?seconds=5

Or to look at the holders of contended mutexes, after calling
runtime.SetMutexProfileFraction in your program:

    go tool pprof http://localhost:6060/debug/pprof/mutex

To view all available profiles, open http://localhost:6060/debug/pprof/ in your
browser.

For a study of the facility in action, visit

    https://blog.golang.org/2011/06/profiling-go-programs.html

## Index

- [func Cmdline(w http.ResponseWriter, r *http.Request)](#Cmdline)
- [func Handler(name string) http.Handler](#Handler)
- [func Index(w http.ResponseWriter, r *http.Request)](#Index)
- [func Profile(w http.ResponseWriter, r *http.Request)](#Profile)
- [func Symbol(w http.ResponseWriter, r *http.Request)](#Symbol)
- [func Trace(w http.ResponseWriter, r *http.Request)](#Trace)

### Package files
 [pprof.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/pprof/pprof.go)

<h2 id="Cmdline">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/pprof/pprof.go#L72">Cmdline</a>
    <a href="#Cmdline">¶</a></h2>
<pre>func Cmdline(w <a href="/net/http/">http</a>.<a href="/net/http/#ResponseWriter">ResponseWriter</a>, r *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>)</pre>

Cmdline responds with the running program's command line, with arguments
separated by NUL bytes. The package initialization registers it as
/debug/pprof/cmdline.

<h2 id="Handler">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/pprof/pprof.go#L208">Handler</a>
    <a href="#Handler">¶</a></h2>
<pre>func Handler(name <a href="/builtin/#string">string</a>) <a href="/net/http/">http</a>.<a href="/net/http/#Handler">Handler</a></pre>

Handler returns an HTTP handler that serves the named profile.

<h2 id="Index">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/pprof/pprof.go#L234">Index</a>
    <a href="#Index">¶</a></h2>
<pre>func Index(w <a href="/net/http/">http</a>.<a href="/net/http/#ResponseWriter">ResponseWriter</a>, r *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>)</pre>

Index responds with the pprof-formatted profile named by the request. For
example, "/debug/pprof/heap" serves the "heap" profile. Index responds to a
request for "/debug/pprof/" with an HTML page listing the available profiles.

<h2 id="Profile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/pprof/pprof.go#L95">Profile</a>
    <a href="#Profile">¶</a></h2>
<pre>func Profile(w <a href="/net/http/">http</a>.<a href="/net/http/#ResponseWriter">ResponseWriter</a>, r *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>)</pre>

Profile responds with the pprof-formatted cpu profile. The package
initialization registers it as /debug/pprof/profile.

<h2 id="Symbol">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/pprof/pprof.go#L162">Symbol</a>
    <a href="#Symbol">¶</a></h2>
<pre>func Symbol(w <a href="/net/http/">http</a>.<a href="/net/http/#ResponseWriter">ResponseWriter</a>, r *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>)</pre>

Symbol looks up the program counters listed in the request, responding with a
table mapping program counters to function names. The package initialization
registers it as /debug/pprof/symbol.

<h2 id="Trace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/http/pprof/pprof.go#L129">Trace</a>
    <a href="#Trace">¶</a></h2>
<pre>func Trace(w <a href="/net/http/">http</a>.<a href="/net/http/#ResponseWriter">ResponseWriter</a>, r *<a href="/net/http/">http</a>.<a href="/net/http/#Request">Request</a>)</pre>

Trace responds with the execution trace in binary form. Tracing lasts for
duration specified in seconds GET parameter, or for 1 second if not specified.
The package initialization registers it as /debug/pprof/trace.


