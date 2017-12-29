version: 1.9.2
## package trace

  `import "runtime/trace"`

## Overview

Go execution tracer. The tracer captures a wide range of execution events like
goroutine creation/blocking/unblocking, syscall enter/exit/block, GC-related
events, changes of heap size, processor start/stop, etc and writes them to an
io.Writer in a compact form. A precise nanosecond-precision timestamp and a
stack trace is captured for most events. A trace can be analyzed later with 'go
tool trace' command.

## Index

- [func Start(w io.Writer) error](#Start)
- [func Stop()](#Stop)

### Package files
 [trace.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/trace/trace.go)

<h2 id="Start">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/trace/trace.go#L12">Start</a>
    <a href="#Start">¶</a></h2>
<pre>func Start(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

Start enables tracing for the current program. While tracing, the trace will be
buffered and written to w. Start returns an error if tracing is already enabled.

<h2 id="Stop">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/trace/trace.go#L30">Stop</a>
    <a href="#Stop">¶</a></h2>
<pre>func Stop()</pre>

Stop stops the current tracing, if any. Stop only returns after all the writes
for the trace have completed.


