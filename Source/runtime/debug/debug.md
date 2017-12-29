version: 1.9.2
## package debug

  `import "runtime/debug"`

## Overview

Package debug contains facilities for programs to debug themselves while they
are running.

## Index

- [func FreeOSMemory()](#FreeOSMemory)
- [func PrintStack()](#PrintStack)
- [func ReadGCStats(stats *GCStats)](#ReadGCStats)
- [func SetGCPercent(percent int) int](#SetGCPercent)
- [func SetMaxStack(bytes int) int](#SetMaxStack)
- [func SetMaxThreads(threads int) int](#SetMaxThreads)
- [func SetPanicOnFault(enabled bool) bool](#SetPanicOnFault)
- [func SetTraceback(level string)](#SetTraceback)
- [func Stack() []byte](#Stack)
- [func WriteHeapDump(fd uintptr)](#WriteHeapDump)
- [type GCStats](#GCStats)

### Package files
 [garbage.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/garbage.go) [stack.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/stack.go) [stubs.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/stubs.go)

<h2 id="FreeOSMemory">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/garbage.go#L89">FreeOSMemory</a>
    <a href="#FreeOSMemory">¶</a></h2>
<pre>func FreeOSMemory()</pre>

FreeOSMemory forces a garbage collection followed by an attempt to return as
much memory to the operating system as possible. (Even if this is not called,
the runtime gradually returns memory to the operating system in a background
task.)

<h2 id="PrintStack">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/stack.go#L5">PrintStack</a>
    <a href="#PrintStack">¶</a></h2>
<pre>func PrintStack()</pre>

PrintStack prints to standard error the stack trace returned by runtime.Stack.

<h2 id="ReadGCStats">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/garbage.go#L21">ReadGCStats</a>
    <a href="#ReadGCStats">¶</a></h2>
<pre>func ReadGCStats(stats *<a href="#GCStats">GCStats</a>)</pre>

ReadGCStats reads statistics about garbage collection into stats. The number of
entries in the pause history is system-dependent; stats.Pause slice will be
reused if large enough, reallocated otherwise. ReadGCStats may use the full
capacity of the stats.Pause slice. If stats.PauseQuantiles is non-empty,
ReadGCStats fills it with quantiles summarizing the distribution of pause time.
For example, if len(stats.PauseQuantiles) is 5, it will be filled with the
minimum, 25%, 50%, 75%, and maximum pause times.

<h2 id="SetGCPercent">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/garbage.go#L81">SetGCPercent</a>
    <a href="#SetGCPercent">¶</a></h2>
<pre>func SetGCPercent(percent <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

SetGCPercent sets the garbage collection target percentage: a collection is
triggered when the ratio of freshly allocated data to live data remaining after
the previous collection reaches this percentage. SetGCPercent returns the
previous setting. The initial setting is the value of the GOGC environment
variable at startup, or 100 if the variable is not set. A negative percentage
disables garbage collection.

<h2 id="SetMaxStack">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/garbage.go#L103">SetMaxStack</a>
    <a href="#SetMaxStack">¶</a></h2>
<pre>func SetMaxStack(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

SetMaxStack sets the maximum amount of memory that can be used by a single
goroutine stack. If any goroutine exceeds this limit while growing its stack,
the program crashes. SetMaxStack returns the previous setting. The initial
setting is 1 GB on 64-bit systems, 250 MB on 32-bit systems.

SetMaxStack is useful mainly for limiting the damage done by goroutines that
enter an infinite recursion. It only limits future stack growth.

<h2 id="SetMaxThreads">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/garbage.go#L121">SetMaxThreads</a>
    <a href="#SetMaxThreads">¶</a></h2>
<pre>func SetMaxThreads(threads <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

SetMaxThreads sets the maximum number of operating system threads that the Go
program can use. If it attempts to use more than this many, the program crashes.
SetMaxThreads returns the previous setting. The initial setting is 10,000
threads.

The limit controls the number of operating system threads, not the number of
goroutines. A Go program creates a new thread only when a goroutine is ready to
run but all the existing threads are blocked in system calls, cgo calls, or are
locked to other goroutines due to use of runtime.LockOSThread.

SetMaxThreads is useful mainly for limiting the damage done by programs that
create an unbounded number of threads. The idea is to take down the program
before it takes down the operating system.

<h2 id="SetPanicOnFault">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/garbage.go#L134">SetPanicOnFault</a>
    <a href="#SetPanicOnFault">¶</a></h2>
<pre>func SetPanicOnFault(enabled <a href="/builtin/#bool">bool</a>) <a href="/builtin/#bool">bool</a></pre>

SetPanicOnFault controls the runtime's behavior when a program faults at an
unexpected (non-nil) address. Such faults are typically caused by bugs such as
runtime memory corruption, so the default response is to crash the program.
Programs working with memory-mapped files or unsafe manipulation of memory may
cause faults at non-nil addresses in less dramatic situations; SetPanicOnFault
allows such programs to request that the runtime trigger only a panic, not a
crash. SetPanicOnFault applies only to the current goroutine. It returns the
previous setting.

<h2 id="SetTraceback">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/garbage.go#L158">SetTraceback</a>
    <a href="#SetTraceback">¶</a></h2>
<pre>func SetTraceback(level <a href="/builtin/#string">string</a>)</pre>

SetTraceback sets the amount of detail printed by the runtime in the traceback
it prints before exiting due to an unrecovered panic or an internal runtime
error. The level argument takes the same values as the GOTRACEBACK environment
variable. For example, SetTraceback("all") ensure that the program prints all
goroutines when it crashes. See the package runtime documentation for details.
If SetTraceback is called with a level lower than that of the environment
variable, the call is ignored.

<h2 id="Stack">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/stack.go#L11">Stack</a>
    <a href="#Stack">¶</a></h2>
<pre>func Stack() []<a href="/builtin/#byte">byte</a></pre>

Stack returns a formatted stack trace of the goroutine that calls it. It calls
runtime.Stack with a large enough buffer to capture the entire trace.

<h2 id="WriteHeapDump">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/garbage.go#L147">WriteHeapDump</a>
    <a href="#WriteHeapDump">¶</a></h2>
<pre>func WriteHeapDump(fd <a href="/builtin/#uintptr">uintptr</a>)</pre>

WriteHeapDump writes a description of the heap and the objects in it to the
given file descriptor.

WriteHeapDump suspends the execution of all goroutines until the heap dump is
completely written. Thus, the file descriptor must not be connected to a pipe or
socket whose other end is in the same Go process; instead, use a temporary file
or network socket.

The heap dump format is defined at https://golang.org/s/go15heapdump.

<h2 id="GCStats">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug/garbage.go#L4">GCStats</a>
    <a href="#GCStats">¶</a></h2>
<pre>type GCStats struct {
<span id="GCStats.LastGC"></span>    LastGC         <a href="/time/">time</a>.<a href="/time/#Time">Time</a>       <span class="comment">// time of last collection</span>
<span id="GCStats.NumGC"></span>    NumGC          <a href="/builtin/#int64">int64</a>           <span class="comment">// number of garbage collections</span>
<span id="GCStats.PauseTotal"></span>    PauseTotal     <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>   <span class="comment">// total pause for all collections</span>
<span id="GCStats.Pause"></span>    Pause          []<a href="/time/">time</a>.<a href="/time/#Duration">Duration</a> <span class="comment">// pause history, most recent first</span>
<span id="GCStats.PauseEnd"></span>    PauseEnd       []<a href="/time/">time</a>.<a href="/time/#Time">Time</a>     <span class="comment">// pause end times history, most recent first</span>
<span id="GCStats.PauseQuantiles"></span>    PauseQuantiles []<a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>
}</pre>

GCStats collect information about recent garbage collections.


