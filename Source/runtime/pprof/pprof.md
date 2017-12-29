version: 1.9.2
## package pprof

  `import "runtime/pprof"`

## Overview

Package pprof writes runtime profiling data in the format expected by the pprof
visualization tool.


Profiling a Go program

The first step to profiling a Go program is to enable profiling. Support for
profiling benchmarks built with the standard testing package is built into go
test. For example, the following command runs benchmarks in the current
directory and writes the CPU and memory profiles to cpu.prof and mem.prof:

    go test -cpuprofile cpu.prof -memprofile mem.prof -bench .

To add equivalent profiling support to a standalone program, add code like the
following to your main function:

    var cpuprofile = flag.String("cpuprofile", "", "write cpu profile `file`")
    var memprofile = flag.String("memprofile", "", "write memory profile to `file`")

    func main() {
        flag.Parse()
        if *cpuprofile != "" {
            f, err := os.Create(*cpuprofile)
            if err != nil {
                log.Fatal("could not create CPU profile: ", err)
            }
            if err := pprof.StartCPUProfile(f); err != nil {
                log.Fatal("could not start CPU profile: ", err)
            }
            defer pprof.StopCPUProfile()
        }

        // ... rest of the program ...

        if *memprofile != "" {
            f, err := os.Create(*memprofile)
            if err != nil {
                log.Fatal("could not create memory profile: ", err)
            }
            runtime.GC() // get up-to-date statistics
            if err := pprof.WriteHeapProfile(f); err != nil {
                log.Fatal("could not write memory profile: ", err)
            }
            f.Close()
        }
    }

There is also a standard HTTP interface to profiling data. Adding the following
line will install handlers under the /debug/pprof/ URL to download live
profiles:

    import _ "net/http/pprof"

See the net/http/pprof package for more details.

Profiles can then be visualized with the pprof tool:

    go tool pprof cpu.prof

There are many commands available from the pprof command line. Commonly used
commands include "top", which prints a summary of the top program hot-spots, and
"web", which opens an interactive graph of hot-spots and their call graphs. Use
"help" for information on all pprof commands.

For more information about pprof, see
https://github.com/google/pprof/blob/master/doc/pprof.md.

## Index

- [func Do(ctx context.Context, labels LabelSet, f func(context.Context))](#Do)
- [func ForLabels(ctx context.Context, f func(key, value string) bool)](#ForLabels)
- [func Label(ctx context.Context, key string) (string, bool)](#Label)
- [func Profiles() []*Profile](#Profiles)
- [func SetGoroutineLabels(ctx context.Context)](#SetGoroutineLabels)
- [func StartCPUProfile(w io.Writer) error](#StartCPUProfile)
- [func StopCPUProfile()](#StopCPUProfile)
- [func WithLabels(ctx context.Context, labels LabelSet) context.Context](#WithLabels)
- [func WriteHeapProfile(w io.Writer) error](#WriteHeapProfile)
- [type LabelSet](#LabelSet)
  - [func Labels(args ...string) LabelSet](#Labels)
- [type Profile](#Profile)
  - [func Lookup(name string) *Profile](#Lookup)
  - [func NewProfile(name string) *Profile](#NewProfile)
  - [func (p *Profile) Add(value interface{}, skip int)](#Profile.Add)
  - [func (p *Profile) Count() int](#Profile.Count)
  - [func (p *Profile) Name() string](#Profile.Name)
  - [func (p *Profile) Remove(value interface{})](#Profile.Remove)
  - [func (p *Profile) WriteTo(w io.Writer, debug int) error](#Profile.WriteTo)
- [Bugs](#pkg-note-BUG)

### Package files
 [elf.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/elf.go) [label.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/label.go) [map.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/map.go) [pprof.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go) [proto.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/proto.go) [protobuf.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/protobuf.go) [protomem.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/protomem.go) [runtime.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/runtime.go)

<h2 id="Do">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/runtime.go#L21">Do</a>
    <a href="#Do">¶</a></h2>
<pre>func Do(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, labels <a href="#LabelSet">LabelSet</a>, f func(<a href="/context/">context</a>.<a href="/context/#Context">Context</a>))</pre>

Do calls f with a copy of the parent context with the given labels added to the
parent's label map. Each key/value pair in labels is inserted into the label map
in the order provided, overriding any previous value for the same key. The
augmented label map will be set for the duration of the call to f and restored
once f returns.

<h2 id="ForLabels">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/label.go#L68">ForLabels</a>
    <a href="#ForLabels">¶</a></h2>
<pre>func ForLabels(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, f func(key, value <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a>)</pre>

ForLabels invokes f with each label set on the context. The function f should
return true to continue iteration or false to stop iteration early.

<h2 id="Label">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/label.go#L60">Label</a>
    <a href="#Label">¶</a></h2>
<pre>func Label(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, key <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#bool">bool</a>)</pre>

Label returns the value of the label with the given key on ctx, and a boolean
indicating whether that label exists.

<h2 id="Profiles">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L204">Profiles</a>
    <a href="#Profiles">¶</a></h2>
<pre>func Profiles() []*<a href="#Profile">Profile</a></pre>

Profiles returns a slice of all the known profiles, sorted by name.

<h2 id="SetGoroutineLabels">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/runtime.go#L10">SetGoroutineLabels</a>
    <a href="#SetGoroutineLabels">¶</a></h2>
<pre>func SetGoroutineLabels(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>)</pre>

SetGoroutineLabels sets the current goroutine's labels to match ctx. This is a
lower-level API than Do, which should be used instead when possible.

<h2 id="StartCPUProfile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L660">StartCPUProfile</a>
    <a href="#StartCPUProfile">¶</a></h2>
<pre>func StartCPUProfile(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

StartCPUProfile enables CPU profiling for the current process. While profiling,
the profile will be buffered and written to w. StartCPUProfile returns an error
if profiling is already enabled.

On Unix-like systems, StartCPUProfile does not work by default for Go code built
with -buildmode=c-archive or -buildmode=c-shared. StartCPUProfile relies on the
SIGPROF signal, but that signal will be delivered to the main program's SIGPROF
signal handler (if any) not to the one used by Go. To make it work, call
os/signal.Notify for syscall.SIGPROF, but note that doing so may break any
profiling being done by the main program.

<h2 id="StopCPUProfile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L719">StopCPUProfile</a>
    <a href="#StopCPUProfile">¶</a></h2>
<pre>func StopCPUProfile()</pre>

StopCPUProfile stops the current CPU profile, if any. StopCPUProfile only
returns after all the writes for the profile have completed.

<h2 id="WithLabels">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/label.go#L29">WithLabels</a>
    <a href="#WithLabels">¶</a></h2>
<pre>func WithLabels(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, labels <a href="#LabelSet">LabelSet</a>) <a href="/context/">context</a>.<a href="/context/#Context">Context</a></pre>

WithLabels returns a new context.Context with the given labels added. A label
overwrites a prior label with the same key.

<h2 id="WriteHeapProfile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L457">WriteHeapProfile</a>
    <a href="#WriteHeapProfile">¶</a></h2>
<pre>func WriteHeapProfile(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

WriteHeapProfile is shorthand for Lookup("heap").WriteTo(w, 0). It is preserved
for backwards compatibility.

<h2 id="LabelSet">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/label.go#L7">LabelSet</a>
    <a href="#LabelSet">¶</a></h2>
<pre>type LabelSet struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

LabelSet is a set of labels.

<h3 id="Labels">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/label.go#L47">Labels</a>
    <a href="#Labels">¶</a></h3>
<pre>func Labels(args ...<a href="/builtin/#string">string</a>) <a href="#LabelSet">LabelSet</a></pre>

Labels takes an even number of strings representing key-value pairs and makes a
LabelSet containing them. A label overwrites a prior label with the same key.

<h2 id="Profile">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L111">Profile</a>
    <a href="#Profile">¶</a></h2>
<pre>type Profile struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Profile is a collection of stack traces showing the call sequences that led to
instances of a particular event, such as allocation. Packages can create and
maintain their own profiles; the most common use is for tracking resources that
must be explicitly closed, such as files or network connections.

A Profile's methods can be called from multiple goroutines simultaneously.

Each Profile has a unique name. A few profiles are predefined:

    goroutine    - stack traces of all current goroutines
    heap         - a sampling of all heap allocations
    threadcreate - stack traces that led to the creation of new OS threads
    block        - stack traces that led to blocking on synchronization primitives
    mutex        - stack traces of holders of contended mutexes

These predefined profiles maintain themselves and panic on an explicit Add or
Remove method call.

The heap profile reports statistics as of the most recently completed garbage
collection; it elides more recent allocation to avoid skewing the profile away
from live data and toward garbage. If there has been no garbage collection at
all, the heap profile reports all known allocations. This exception helps mainly
in programs running without garbage collection enabled, usually for debugging
purposes.

The CPU profile is not available as a Profile. It has a special API, the
StartCPUProfile and StopCPUProfile functions, because it streams output to a
writer during profiling.

<h3 id="Lookup">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L197">Lookup</a>
    <a href="#Lookup">¶</a></h3>
<pre>func Lookup(name <a href="/builtin/#string">string</a>) *<a href="#Profile">Profile</a></pre>

Lookup returns the profile with the given name, or nil if no such profile
exists.

<h3 id="NewProfile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L179">NewProfile</a>
    <a href="#NewProfile">¶</a></h3>
<pre>func NewProfile(name <a href="/builtin/#string">string</a>) *<a href="#Profile">Profile</a></pre>

NewProfile creates a new profile with the given name. If a profile with that
name already exists, NewProfile panics. The convention is to use a
'import/path.' prefix to create separate name spaces for each package. For
compatibility with various tools that read pprof data, profile names should not
contain spaces.

<h3 id="Profile.Add">func (*Profile) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L250">Add</a>
    <a href="#Profile.Add">¶</a></h3>
<pre>func (p *<a href="#Profile">Profile</a>) Add(value interface{}, skip <a href="/builtin/#int">int</a>)</pre>

Add adds the current execution stack to the profile, associated with value. Add
stores value in an internal map, so value must be suitable for use as a map key
and will not be garbage collected until the corresponding call to Remove. Add
panics if the profile already contains a stack for value.

The skip parameter has the same meaning as runtime.Caller's skip and controls
where the stack trace begins. Passing skip=0 begins the trace in the function
calling Add. For example, given this execution stack:

    Add
    called from rpc.NewClient
    called from mypkg.Run
    called from main.main

Passing skip=0 begins the stack trace at the call to Add inside rpc.NewClient.
Passing skip=1 begins the stack trace at the call to NewClient inside mypkg.Run.

<h3 id="Profile.Count">func (*Profile) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L223">Count</a>
    <a href="#Profile.Count">¶</a></h3>
<pre>func (p *<a href="#Profile">Profile</a>) Count() <a href="/builtin/#int">int</a></pre>

Count returns the number of execution stacks currently in the profile.

<h3 id="Profile.Name">func (*Profile) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L218">Name</a>
    <a href="#Profile.Name">¶</a></h3>
<pre>func (p *<a href="#Profile">Profile</a>) Name() <a href="/builtin/#string">string</a></pre>

Name returns this profile's name, which can be passed to Lookup to reobtain the
profile.

<h3 id="Profile.Remove">func (*Profile) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L276">Remove</a>
    <a href="#Profile.Remove">¶</a></h3>
<pre>func (p *<a href="#Profile">Profile</a>) Remove(value interface{})</pre>

Remove removes the execution stack associated with value from the profile. It is
a no-op if the value is not in the profile.

<h3 id="Profile.WriteTo">func (*Profile) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L295">WriteTo</a>
    <a href="#Profile.WriteTo">¶</a></h3>
<pre>func (p *<a href="#Profile">Profile</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, debug <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

WriteTo writes a pprof-formatted snapshot of the profile to w. If a write to w
returns an error, WriteTo returns that error. Otherwise, WriteTo returns nil.

The debug parameter enables additional output. Passing debug=0 prints only the
hexadecimal addresses that pprof needs. Passing debug=1 adds comments
translating addresses to function names and line numbers, so that a programmer
can read the profile without tools.

The predefined profiles may assign meaning to other debug values; for example,
when printing the "goroutine" profile, debug=2 means to print the goroutine
stacks in the same form that a Go program uses when dying due to an unrecovered
panic.

<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/pprof/pprof.go#L78)  Profiles are only as good as the kernel support used to generate them. See
  https://golang.org/issue/13841 for details about known problems.

## Subdirectories
- [..](..)
