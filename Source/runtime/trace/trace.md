version: 1.10
## package trace

  `import "runtime/trace"`

## Overview

Package trace contains facilities for programs to generate trace for Go
execution tracer.

The execution trace captures a wide range of execution events such as goroutine
creation/blocking/unblocking, syscall enter/exit/block, GC-related events,
changes of heap size, processor start/stop, etc. A precise nanosecond-precision
timestamp and a stack trace is captured for most events. The generated trace can
be interpreted using `go tool trace`.


Tracing a Go program

Support for tracing tests and benchmarks built with the standard testing package
is built into `go test`. For example, the following command runs the test in the
current directory and writes the trace file (trace.out).

    go test -trace=test.out

This runtime/trace package provides APIs to add equivalent tracing support to a
standalone program. See the Example that demonstrates how to use this API to
enable tracing.

There is also a standard HTTP interface to profiling data. Adding the following
line will install handlers under the /debug/pprof/trace URL to download live
profiles:

    import _ "net/http/pprof"

See the net/http/pprof package for more details.

<a id="example"></a>
Example:

    package trace_test

    import (
        "fmt"
        "log"
        "os"
        "runtime/trace"
    )

    // Example demonstrates the use of the trace package to trace
    // the execution of a Go program. The trace output will be
    // written to the file trace.out
    func Example() {
        f, err := os.Create("trace.out")
        if err != nil {
            log.Fatalf("failed to create trace output file: %v", err)
        }
        defer func() {
            if err := f.Close(); err != nil {
                log.Fatalf("failed to close trace file: %v", err)
            }
        }()

        if err := trace.Start(f); err != nil {
            log.Fatalf("failed to start trace: %v", err)
        }
        defer trace.Stop()

        // your program here
        RunMyProgram()
    }

    func RunMyProgram() {
        fmt.Printf("this function will be traced")
    }

## Index

- [func Start(w io.Writer) error](#Start)
- [func Stop()](#Stop)

### Examples

- [Package](#example)

### Package files
 [trace.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/trace/trace.go)

<h2 id="Start">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/trace/trace.go#L35">Start</a>
    <a href="#Start">¶</a></h2>
<pre>func Start(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

Start enables tracing for the current program. While tracing, the trace will be
buffered and written to w. Start returns an error if tracing is already enabled.

<h2 id="Stop">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/trace/trace.go#L53">Stop</a>
    <a href="#Stop">¶</a></h2>
<pre>func Stop()</pre>

Stop stops the current tracing, if any. Stop only returns after all the writes
for the trace have completed.


