version: 1.9.2
## package runtime

  `import "runtime"`

## Overview

Package runtime contains operations that interact with Go's runtime system, such
as functions to control goroutines. It also includes the low-level type
information used by the reflect package; see reflect's documentation for the
programmable interface to the run-time type system.


Environment Variables

The following environment variables ($name or %name%, depending on the host
operating system) control the run-time behavior of Go programs. The meanings and
use may change from release to release.

The GOGC variable sets the initial garbage collection target percentage. A
collection is triggered when the ratio of freshly allocated data to live data
remaining after the previous collection reaches this percentage. The default is
GOGC=100. Setting GOGC=off disables the garbage collector entirely. The
runtime/debug package's SetGCPercent function allows changing this percentage at
run time. See https://golang.org/pkg/runtime/debug/#SetGCPercent.

The GODEBUG variable controls debugging variables within the runtime. It is a
comma-separated list of name=val pairs setting these named variables:

    allocfreetrace: setting allocfreetrace=1 causes every allocation to be
    profiled and a stack trace printed on each object's allocation and free.

    cgocheck: setting cgocheck=0 disables all checks for packages
    using cgo to incorrectly pass Go pointers to non-Go code.
    Setting cgocheck=1 (the default) enables relatively cheap
    checks that may miss some errors.  Setting cgocheck=2 enables
    expensive checks that should not miss any errors, but will
    cause your program to run slower.

    efence: setting efence=1 causes the allocator to run in a mode
    where each object is allocated on a unique page and addresses are
    never recycled.

    gccheckmark: setting gccheckmark=1 enables verification of the
    garbage collector's concurrent mark phase by performing a
    second mark pass while the world is stopped.  If the second
    pass finds a reachable object that was not found by concurrent
    mark, the garbage collector will panic.

    gcpacertrace: setting gcpacertrace=1 causes the garbage collector to
    print information about the internal state of the concurrent pacer.

    gcshrinkstackoff: setting gcshrinkstackoff=1 disables moving goroutines
    onto smaller stacks. In this mode, a goroutine's stack can only grow.

    gcrescanstacks: setting gcrescanstacks=1 enables stack
    re-scanning during the STW mark termination phase. This is
    helpful for debugging if objects are being prematurely
    garbage collected.

    gcstoptheworld: setting gcstoptheworld=1 disables concurrent garbage collection,
    making every garbage collection a stop-the-world event. Setting gcstoptheworld=2
    also disables concurrent sweeping after the garbage collection finishes.

    gctrace: setting gctrace=1 causes the garbage collector to emit a single line to standard
    error at each collection, summarizing the amount of memory collected and the
    length of the pause. Setting gctrace=2 emits the same summary but also
    repeats each collection. The format of this line is subject to change.
    Currently, it is:
    	gc # @#s #%: #+#+# ms clock, #+#/#/#+# ms cpu, #->#-># MB, # MB goal, # P
    where the fields are as follows:
    	gc #        the GC number, incremented at each GC
    	@#s         time in seconds since program start
    	#%          percentage of time spent in GC since program start
    	#+...+#     wall-clock/CPU times for the phases of the GC
    	#->#-># MB  heap size at GC start, at GC end, and live heap
    	# MB goal   goal heap size
    	# P         number of processors used
    The phases are stop-the-world (STW) sweep termination, concurrent
    mark and scan, and STW mark termination. The CPU times
    for mark/scan are broken down in to assist time (GC performed in
    line with allocation), background GC time, and idle GC time.
    If the line ends with "(forced)", this GC was forced by a
    runtime.GC() call.

    Setting gctrace to any value > 0 also causes the garbage collector
    to emit a summary when memory is released back to the system.
    This process of returning memory to the system is called scavenging.
    The format of this summary is subject to change.
    Currently it is:
    	scvg#: # MB released  printed only if non-zero
    	scvg#: inuse: # idle: # sys: # released: # consumed: # (MB)
    where the fields are as follows:
    	scvg#        the scavenge cycle number, incremented at each scavenge
    	inuse: #     MB used or partially used spans
    	idle: #      MB spans pending scavenging
    	sys: #       MB mapped from the system
    	released: #  MB released to the system
    	consumed: #  MB allocated from the system

    memprofilerate: setting memprofilerate=X will update the value of runtime.MemProfileRate.
    When set to 0 memory profiling is disabled.  Refer to the description of
    MemProfileRate for the default value.

    invalidptr: defaults to invalidptr=1, causing the garbage collector and stack
    copier to crash the program if an invalid pointer value (for example, 1)
    is found in a pointer-typed location. Setting invalidptr=0 disables this check.
    This should only be used as a temporary workaround to diagnose buggy code.
    The real fix is to not store integers in pointer-typed locations.

    sbrk: setting sbrk=1 replaces the memory allocator and garbage collector
    with a trivial allocator that obtains memory from the operating system and
    never reclaims any memory.

    scavenge: scavenge=1 enables debugging mode of heap scavenger.

    scheddetail: setting schedtrace=X and scheddetail=1 causes the scheduler to emit
    detailed multiline info every X milliseconds, describing state of the scheduler,
    processors, threads and goroutines.

    schedtrace: setting schedtrace=X causes the scheduler to emit a single line to standard
    error every X milliseconds, summarizing the scheduler state.

The net and net/http packages also refer to debugging variables in GODEBUG. See
the documentation for those packages for details.

The GOMAXPROCS variable limits the number of operating system threads that can
execute user-level Go code simultaneously. There is no limit to the number of
threads that can be blocked in system calls on behalf of Go code; those do not
count against the GOMAXPROCS limit. This package's GOMAXPROCS function queries
and changes the limit.

The GOTRACEBACK variable controls the amount of output generated when a Go
program fails due to an unrecovered panic or an unexpected runtime condition. By
default, a failure prints a stack trace for the current goroutine, eliding
functions internal to the run-time system, and then exits with exit code 2. The
failure prints stack traces for all goroutines if there is no current goroutine
or the failure is internal to the run-time. GOTRACEBACK=none omits the goroutine
stack traces entirely. GOTRACEBACK=single (the default) behaves as described
above. GOTRACEBACK=all adds stack traces for all user-created goroutines.
GOTRACEBACK=system is like ``all'' but adds stack frames for run-time functions
and shows goroutines created internally by the run-time. GOTRACEBACK=crash is
like ``system'' but crashes in an operating system-specific manner instead of
exiting. For example, on Unix systems, the crash raises SIGABRT to trigger a
core dump. For historical reasons, the GOTRACEBACK settings 0, 1, and 2 are
synonyms for none, all, and system, respectively. The runtime/debug package's
SetTraceback function allows increasing the amount of output at run time, but it
cannot reduce the amount below that specified by the environment variable. See
https://golang.org/pkg/runtime/debug/#SetTraceback.

The GOARCH, GOOS, GOPATH, and GOROOT environment variables complete the set of
Go environment variables. They influence the building of Go programs (see
https://golang.org/cmd/go and https://golang.org/pkg/go/build). GOARCH, GOOS,
and GOROOT are recorded at compile time and made available by constants or
functions in this package, but they do not influence the execution of the
run-time system.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func BlockProfile(p []BlockProfileRecord) (n int, ok bool)](#BlockProfile)
- [func Breakpoint()](#Breakpoint)
- [func CPUProfile() []byte](#CPUProfile)
- [func Caller(skip int) (pc uintptr, file string, line int, ok bool)](#Caller)
- [func Callers(skip int, pc []uintptr) int](#Callers)
- [func GC()](#GC)
- [func GOMAXPROCS(n int) int](#GOMAXPROCS)
- [func GOROOT() string](#GOROOT)
- [func Goexit()](#Goexit)
- [func GoroutineProfile(p []StackRecord) (n int, ok bool)](#GoroutineProfile)
- [func Gosched()](#Gosched)
- [func KeepAlive(interface{})](#KeepAlive)
- [func LockOSThread()](#LockOSThread)
- [func MemProfile(p []MemProfileRecord, inuseZero bool) (n int, ok bool)](#MemProfile)
- [func MutexProfile(p []BlockProfileRecord) (n int, ok bool)](#MutexProfile)
- [func NumCPU() int](#NumCPU)
- [func NumCgoCall() int64](#NumCgoCall)
- [func NumGoroutine() int](#NumGoroutine)
- [func ReadMemStats(m *MemStats)](#ReadMemStats)
- [func ReadTrace() []byte](#ReadTrace)
- [func SetBlockProfileRate(rate int)](#SetBlockProfileRate)
- [func SetCPUProfileRate(hz int)](#SetCPUProfileRate)
- [func SetCgoTraceback(version int, traceback, context, symbolizer unsafe.Pointer)](#SetCgoTraceback)
- [func SetFinalizer(obj interface{}, finalizer interface{})](#SetFinalizer)
- [func SetMutexProfileFraction(rate int) int](#SetMutexProfileFraction)
- [func Stack(buf []byte, all bool) int](#Stack)
- [func StartTrace() error](#StartTrace)
- [func StopTrace()](#StopTrace)
- [func ThreadCreateProfile(p []StackRecord) (n int, ok bool)](#ThreadCreateProfile)
- [func UnlockOSThread()](#UnlockOSThread)
- [func Version() string](#Version)
- [type BlockProfileRecord](#BlockProfileRecord)
- [type Error](#Error)
- [type Frame](#Frame)
- [type Frames](#Frames)
  - [func CallersFrames(callers []uintptr) *Frames](#CallersFrames)
  - [func (ci *Frames) Next() (frame Frame, more bool)](#Frames.Next)
- [type Func](#Func)
  - [func FuncForPC(pc uintptr) *Func](#FuncForPC)
  - [func (f *Func) Entry() uintptr](#Func.Entry)
  - [func (f *Func) FileLine(pc uintptr) (file string, line int)](#Func.FileLine)
  - [func (f *Func) Name() string](#Func.Name)
- [type MemProfileRecord](#MemProfileRecord)
  - [func (r *MemProfileRecord) InUseBytes() int64](#MemProfileRecord.InUseBytes)
  - [func (r *MemProfileRecord) InUseObjects() int64](#MemProfileRecord.InUseObjects)
  - [func (r *MemProfileRecord) Stack() []uintptr](#MemProfileRecord.Stack)
- [type MemStats](#MemStats)
- [type StackRecord](#StackRecord)
  - [func (r *StackRecord) Stack() []uintptr](#StackRecord.Stack)
- [type TypeAssertionError](#TypeAssertionError)
  - [func (e *TypeAssertionError) Error() string](#TypeAssertionError.Error)
  - [func (*TypeAssertionError) RuntimeError()](#TypeAssertionError.RuntimeError)

### Examples

- [Frames](#example_Frames)

### Package files
 [alg.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/alg.go) [atomic_pointer.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/atomic_pointer.go) [cgo.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cgo.go) [cgo_mmap.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cgo_mmap.go) [cgo_sigaction.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cgo_sigaction.go) [cgocall.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cgocall.go) [cgocallback.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cgocallback.go) [cgocheck.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cgocheck.go) [chan.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/chan.go) [compiler.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/compiler.go) [complex.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/complex.go) [cpuflags_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cpuflags_amd64.go) [cpuprof.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cpuprof.go) [cputicks.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cputicks.go) [debug.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug.go) [defs_linux_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/defs_linux_amd64.go) [env_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/env_posix.go) [error.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/error.go) [extern.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/extern.go) [fastlog2.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/fastlog2.go) [fastlog2table.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/fastlog2table.go) [float.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/float.go) [hash64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/hash64.go) [hashmap.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/hashmap.go) [hashmap_fast.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/hashmap_fast.go) [heapdump.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/heapdump.go) [iface.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/iface.go) [lfstack.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/lfstack.go) [lfstack_64bit.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/lfstack_64bit.go) [lock_futex.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/lock_futex.go) [malloc.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/malloc.go) [mbarrier.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mbarrier.go) [mbitmap.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mbitmap.go) [mcache.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mcache.go) [mcentral.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mcentral.go) [mem_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mem_linux.go) [mfinal.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mfinal.go) [mfixalloc.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mfixalloc.go) [mgc.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mgc.go) [mgclarge.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mgclarge.go) [mgcmark.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mgcmark.go) [mgcsweep.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mgcsweep.go) [mgcsweepbuf.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mgcsweepbuf.go) [mgcwork.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mgcwork.go) [mheap.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mheap.go) [mprof.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go) [msan0.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/msan0.go) [msize.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/msize.go) [mstats.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mstats.go) [netpoll.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/netpoll.go) [netpoll_epoll.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/netpoll_epoll.go) [os_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/os_linux.go) [os_linux_generic.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/os_linux_generic.go) [panic.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/panic.go) [plugin.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/plugin.go) [print.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/print.go) [proc.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/proc.go) [profbuf.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/profbuf.go) [proflabel.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/proflabel.go) [race0.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/race0.go) [rdebug.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/rdebug.go) [relax_stub.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/relax_stub.go) [runtime.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/runtime.go) [runtime1.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/runtime1.go) [runtime2.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/runtime2.go) [rwmutex.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/rwmutex.go) [select.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/select.go) [sema.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/sema.go) [signal_amd64x.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/signal_amd64x.go) [signal_linux_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/signal_linux_amd64.go) [signal_sighandler.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/signal_sighandler.go) [signal_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/signal_unix.go) [sigqueue.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/sigqueue.go) [sigtab_linux_generic.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/sigtab_linux_generic.go) [sizeclasses.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/sizeclasses.go) [slice.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/slice.go) [softfloat64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/softfloat64.go) [sqrt.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/sqrt.go) [stack.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/stack.go) [string.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/string.go) [stubs.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/stubs.go) [stubs2.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/stubs2.go) [stubs_asm.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/stubs_asm.go) [stubs_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/stubs_linux.go) [symtab.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/symtab.go) [sys_nonppc64x.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/sys_nonppc64x.go) [sys_x86.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/sys_x86.go) [time.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/time.go) [timestub.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/timestub.go) [trace.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/trace.go) [traceback.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/traceback.go) [type.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/type.go) [typekind.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/typekind.go) [unaligned1.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/unaligned1.go) [utf8.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/utf8.go) [vdso_linux_amd64.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/vdso_linux_amd64.go) [write_err.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/write_err.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const <span id="Compiler">Compiler</span> = &#34;gc&#34;</pre>

Compiler is the name of the compiler toolchain that built the running binary.
Known toolchains are:

    gc      Also known as cmd/compile.
    gccgo   The gccgo front end, part of the GCC compiler suite.

<pre>const <span id="GOARCH">GOARCH</span> <a href="/builtin/#string">string</a> = <a href="/runtime/internal/sys/">sys</a>.<a href="/runtime/internal/sys/#GOARCH">GOARCH</a></pre>

GOARCH is the running program's architecture target: one of 386, amd64, arm,
s390x, and so on.

<pre>const <span id="GOOS">GOOS</span> <a href="/builtin/#string">string</a> = <a href="/runtime/internal/sys/">sys</a>.<a href="/runtime/internal/sys/#GOOS">GOOS</a></pre>

GOOS is the running program's operating system target: one of darwin, freebsd,
linux, and so on.

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="MemProfileRate">MemProfileRate</span> <a href="/builtin/#int">int</a> = 512 * 1024</pre>

MemProfileRate controls the fraction of memory allocations that are recorded and
reported in the memory profile. The profiler aims to sample an average of one
allocation per MemProfileRate bytes allocated.

To include every allocated block in the profile, set MemProfileRate to 1. To
turn off profiling entirely, set MemProfileRate to 0.

The tools that process the memory profiles assume that the profile rate is
constant across the lifetime of the program and equal to the current value.
Programs that change the memory profiling rate should do so just once, as early
as possible in the execution of the program (for example, at the beginning of
main).

<h2 id="BlockProfile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L624">BlockProfile</a>
    <a href="#BlockProfile">¶</a></h2>
<pre>func BlockProfile(p []<a href="#BlockProfileRecord">BlockProfileRecord</a>) (n <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

BlockProfile returns n, the number of records in the current blocking profile.
If len(p) >= n, BlockProfile copies the profile into p and returns n, true. If
len(p) < n, BlockProfile does not change p and returns n, false.

Most clients should use the runtime/pprof package or the testing package's
-test.blockprofile flag instead of calling BlockProfile directly.

<h2 id="Breakpoint">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/proc.go#L3143">Breakpoint</a>
    <a href="#Breakpoint">¶</a></h2>
<pre>func Breakpoint()</pre>

Breakpoint executes a breakpoint trap.

<h2 id="CPUProfile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cpuprof.go#L175">CPUProfile</a>
    <a href="#CPUProfile">¶</a></h2>
<pre>func CPUProfile() []<a href="/builtin/#byte">byte</a></pre>

CPUProfile panics. It formerly provided raw access to chunks of a pprof-format
profile generated by the runtime. The details of generating that format have
changed, so this functionality has been removed.

Deprecated: use the runtime/pprof package, or the handlers in the net/http/pprof
package, or the testing package's -test.cpuprofile flag instead.

<h2 id="Caller">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/extern.go#L158">Caller</a>
    <a href="#Caller">¶</a></h2>
<pre>func Caller(skip <a href="/builtin/#int">int</a>) (pc <a href="/builtin/#uintptr">uintptr</a>, file <a href="/builtin/#string">string</a>, line <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

Caller reports file and line number information about function invocations on
the calling goroutine's stack. The argument skip is the number of stack frames
to ascend, with 0 identifying the caller of Caller. (For historical reasons the
meaning of skip differs between Caller and Callers.) The return values report
the program counter, file name, and line number within the file of the
corresponding call. The boolean ok is false if it was not possible to recover
the information.

<h2 id="Callers">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/extern.go#L195">Callers</a>
    <a href="#Callers">¶</a></h2>
<pre>func Callers(skip <a href="/builtin/#int">int</a>, pc []<a href="/builtin/#uintptr">uintptr</a>) <a href="/builtin/#int">int</a></pre>

Callers fills the slice pc with the return program counters of function
invocations on the calling goroutine's stack. The argument skip is the number of
stack frames to skip before recording in pc, with 0 identifying the frame for
Callers itself and 1 identifying the caller of Callers. It returns the number of
entries written to pc.

To translate these PCs into symbolic information such as function names and line
numbers, use CallersFrames. CallersFrames accounts for inlined functions and
adjusts the return program counters into call program counters. Iterating over
the returned slice of PCs directly is discouraged, as is using FuncForPC on any
of the returned PCs, since these cannot account for inlining or return program
counter adjustment.

<h2 id="GC">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mgc.go#L1009">GC</a>
    <a href="#GC">¶</a></h2>
<pre>func GC()</pre>

GC runs a garbage collection and blocks the caller until the garbage collection
is complete. It may also block the entire program.

<h2 id="GOMAXPROCS">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug.go#L7">GOMAXPROCS</a>
    <a href="#GOMAXPROCS">¶</a></h2>
<pre>func GOMAXPROCS(n <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

GOMAXPROCS sets the maximum number of CPUs that can be executing simultaneously
and returns the previous setting. If n < 1, it does not change the current
setting. The number of logical CPUs on the local machine can be queried with
NumCPU. This call will go away when the scheduler improves.

<h2 id="GOROOT">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/extern.go#L208">GOROOT</a>
    <a href="#GOROOT">¶</a></h2>
<pre>func GOROOT() <a href="/builtin/#string">string</a></pre>

GOROOT returns the root of the Go tree. It uses the GOROOT environment variable,
if set, or else the root used during the Go build.

<h2 id="Goexit">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/panic.go#L335">Goexit</a>
    <a href="#Goexit">¶</a></h2>
<pre>func Goexit()</pre>

Goexit terminates the goroutine that calls it. No other goroutine is affected.
Goexit runs all deferred calls before terminating the goroutine. Because Goexit
is not panic, however, any recover calls in those deferred functions will return
nil.

Calling Goexit from the main goroutine terminates that goroutine without func
main returning. Since func main has not returned, the program continues
execution of other goroutines. If all other goroutines exit, the program
crashes.

<h2 id="GoroutineProfile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L710">GoroutineProfile</a>
    <a href="#GoroutineProfile">¶</a></h2>
<pre>func GoroutineProfile(p []<a href="#StackRecord">StackRecord</a>) (n <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

GoroutineProfile returns n, the number of records in the active goroutine stack
profile. If len(p) >= n, GoroutineProfile copies the profile into p and returns
n, true. If len(p) < n, GoroutineProfile does not change p and returns n, false.

Most clients should use the runtime/pprof package instead of calling
GoroutineProfile directly.

<h2 id="Gosched">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/proc.go#L248">Gosched</a>
    <a href="#Gosched">¶</a></h2>
<pre>func Gosched()</pre>

Gosched yields the processor, allowing other goroutines to run. It does not
suspend the current goroutine, so execution resumes automatically. go:nosplit

<h2 id="KeepAlive">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mfinal.go#L480">KeepAlive</a>
    <a href="#KeepAlive">¶</a></h2>
<pre>func KeepAlive(interface{})</pre>

KeepAlive marks its argument as currently reachable. This ensures that the
object is not freed, and its finalizer is not run, before the point in the
program where KeepAlive is called.

A very simplified example showing where KeepAlive is required:

    type File struct { d int }
    d, err := syscall.Open("/file/path", syscall.O_RDONLY, 0)
    // ... do something if err != nil ...
    p := &File{d}
    runtime.SetFinalizer(p, func(p *File) { syscall.Close(p.d) })
    var buf [10]byte
    n, err := syscall.Read(p.d, buf[:])
    // Ensure p is not finalized until Read returns.
    runtime.KeepAlive(p)
    // No more uses of p after this point.

Without the KeepAlive call, the finalizer could run at the start of
syscall.Read, closing the file descriptor before syscall.Read makes the actual
system call.

<h2 id="LockOSThread">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/proc.go#L3162">LockOSThread</a>
    <a href="#LockOSThread">¶</a></h2>
<pre>func LockOSThread()</pre>

LockOSThread wires the calling goroutine to its current operating system thread.
Until the calling goroutine exits or calls UnlockOSThread, it will always
execute in that thread, and no other goroutine can.

<h2 id="MemProfile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L533">MemProfile</a>
    <a href="#MemProfile">¶</a></h2>
<pre>func MemProfile(p []<a href="#MemProfileRecord">MemProfileRecord</a>, inuseZero <a href="/builtin/#bool">bool</a>) (n <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

MemProfile returns a profile of memory allocated and freed per allocation site.

MemProfile returns n, the number of records in the current memory profile. If
len(p) >= n, MemProfile copies the profile into p and returns n, true. If len(p)
< n, MemProfile does not change p and returns n, false.

If inuseZero is true, the profile includes allocation records where r.AllocBytes
> 0 but r.AllocBytes == r.FreeBytes. These are sites where memory was allocated,
but it has all been released back to the runtime.

The returned profile may be up to two garbage collection cycles old. This is to
avoid skewing the profile toward allocations; because allocations happen in real
time but frees are delayed until the garbage collector performs sweeping, the
profile only accounts for allocations that have had a chance to be freed by the
garbage collector.

Most clients should use the runtime/pprof package or the testing package's
-test.memprofile flag instead of calling MemProfile directly.

<h2 id="MutexProfile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L659">MutexProfile</a>
    <a href="#MutexProfile">¶</a></h2>
<pre>func MutexProfile(p []<a href="#BlockProfileRecord">BlockProfileRecord</a>) (n <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

MutexProfile returns n, the number of records in the current mutex profile. If
len(p) >= n, MutexProfile copies the profile into p and returns n, true.
Otherwise, MutexProfile does not change p, and returns n, false.

Most clients should use the runtime/pprof package instead of calling
MutexProfile directly.

<h2 id="NumCPU">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug.go#L32">NumCPU</a>
    <a href="#NumCPU">¶</a></h2>
<pre>func NumCPU() <a href="/builtin/#int">int</a></pre>

NumCPU returns the number of logical CPUs usable by the current process.

The set of available CPUs is checked by querying the operating system at process
startup. Changes to operating system CPU allocation after process startup are
not reflected.

<h2 id="NumCgoCall">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug.go#L37">NumCgoCall</a>
    <a href="#NumCgoCall">¶</a></h2>
<pre>func NumCgoCall() <a href="/builtin/#int64">int64</a></pre>

NumCgoCall returns the number of cgo calls made by the current process.

<h2 id="NumGoroutine">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/debug.go#L46">NumGoroutine</a>
    <a href="#NumGoroutine">¶</a></h2>
<pre>func NumGoroutine() <a href="/builtin/#int">int</a></pre>

NumGoroutine returns the number of goroutines that currently exist.

<h2 id="ReadMemStats">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mstats.go#L447">ReadMemStats</a>
    <a href="#ReadMemStats">¶</a></h2>
<pre>func ReadMemStats(m *<a href="#MemStats">MemStats</a>)</pre>

ReadMemStats populates m with memory allocator statistics.

The returned memory allocator statistics are up to date as of the call to
ReadMemStats. This is in contrast with a heap profile, which is a snapshot as of
the most recently completed garbage collection cycle.

<h2 id="ReadTrace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/trace.go#L345">ReadTrace</a>
    <a href="#ReadTrace">¶</a></h2>
<pre>func ReadTrace() []<a href="/builtin/#byte">byte</a></pre>

ReadTrace returns the next chunk of binary tracing data, blocking until data is
available. If tracing is turned off and all the data accumulated while it was on
has been returned, ReadTrace returns nil. The caller must copy the returned data
before calling ReadTrace again. ReadTrace must be called from one goroutine at a
time.

<h2 id="SetBlockProfileRate">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L370">SetBlockProfileRate</a>
    <a href="#SetBlockProfileRate">¶</a></h2>
<pre>func SetBlockProfileRate(rate <a href="/builtin/#int">int</a>)</pre>

SetBlockProfileRate controls the fraction of goroutine blocking events that are
reported in the blocking profile. The profiler aims to sample an average of one
blocking event per rate nanoseconds spent blocked.

To include every blocking event in the profile, pass rate = 1. To turn off
profiling entirely, pass rate <= 0.

<h2 id="SetCPUProfileRate">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/cpuprof.go#L43">SetCPUProfileRate</a>
    <a href="#SetCPUProfileRate">¶</a></h2>
<pre>func SetCPUProfileRate(hz <a href="/builtin/#int">int</a>)</pre>

SetCPUProfileRate sets the CPU profiling rate to hz samples per second. If hz <=
0, SetCPUProfileRate turns off profiling. If the profiler is on, the rate cannot
be changed without first turning it off.

Most clients should use the runtime/pprof package or the testing package's
-test.cpuprofile flag instead of calling SetCPUProfileRate directly.

<h2 id="SetCgoTraceback">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/traceback.go#L1003">SetCgoTraceback</a>
    <a href="#SetCgoTraceback">¶</a></h2>
<pre>func SetCgoTraceback(version <a href="/builtin/#int">int</a>, traceback, context, symbolizer <a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>)</pre>

SetCgoTraceback records three C functions to use to gather traceback information
from C code and to convert that traceback information into symbolic information.
These are used when printing stack traces for a program that uses cgo.

The traceback and context functions may be called from a signal handler, and
must therefore use only async-signal safe functions. The symbolizer function may
be called while the program is crashing, and so must be cautious about using
memory. None of the functions may call back into Go.

The context function will be called with a single argument, a pointer to a
struct:

    struct {
    	Context uintptr
    }

In C syntax, this struct will be

    struct {
    	uintptr_t Context;
    };

If the Context field is 0, the context function is being called to record the
current traceback context. It should record in the Context field whatever
information is needed about the current point of execution to later produce a
stack trace, probably the stack pointer and PC. In this case the context
function will be called from C code.

If the Context field is not 0, then it is a value returned by a previous call to
the context function. This case is called when the context is no longer needed;
that is, when the Go code is returning to its C code caller. This permits the
context function to release any associated resources.

While it would be correct for the context function to record a complete a stack
trace whenever it is called, and simply copy that out in the traceback function,
in a typical program the context function will be called many times without ever
recording a traceback for that context. Recording a complete stack trace in a
call to the context function is likely to be inefficient.

The traceback function will be called with a single argument, a pointer to a
struct:

    struct {
    	Context    uintptr
    	SigContext uintptr
    	Buf        *uintptr
    	Max        uintptr
    }

In C syntax, this struct will be

    struct {
    	uintptr_t  Context;
    	uintptr_t  SigContext;
    	uintptr_t* Buf;
    	uintptr_t  Max;
    };

The Context field will be zero to gather a traceback from the current program
execution point. In this case, the traceback function will be called from C
code.

Otherwise Context will be a value previously returned by a call to the context
function. The traceback function should gather a stack trace from that saved
point in the program execution. The traceback function may be called from an
execution thread other than the one that recorded the context, but only when the
context is known to be valid and unchanging. The traceback function may also be
called deeper in the call stack on the same thread that recorded the context.
The traceback function may be called multiple times with the same Context value;
it will usually be appropriate to cache the result, if possible, the first time
this is called for a specific context value.

If the traceback function is called from a signal handler on a Unix system,
SigContext will be the signal context argument passed to the signal handler (a C
ucontext_t* cast to uintptr_t). This may be used to start tracing at the point
where the signal occurred. If the traceback function is not called from a signal
handler, SigContext will be zero.

Buf is where the traceback information should be stored. It should be PC values,
such that Buf[0] is the PC of the caller, Buf[1] is the PC of that function's
caller, and so on. Max is the maximum number of entries to store. The function
should store a zero to indicate the top of the stack, or that the caller is on a
different stack, presumably a Go stack.

Unlike runtime.Callers, the PC values returned should, when passed to the
symbolizer function, return the file/line of the call instruction. No additional
subtraction is required or appropriate.

The symbolizer function will be called with a single argument, a pointer to a
struct:

    struct {
    	PC      uintptr // program counter to fetch information for
    	File    *byte   // file name (NUL terminated)
    	Lineno  uintptr // line number
    	Func    *byte   // function name (NUL terminated)
    	Entry   uintptr // function entry point
    	More    uintptr // set non-zero if more info for this PC
    	Data    uintptr // unused by runtime, available for function
    }

In C syntax, this struct will be

    struct {
    	uintptr_t PC;
    	char*     File;
    	uintptr_t Lineno;
    	char*     Func;
    	uintptr_t Entry;
    	uintptr_t More;
    	uintptr_t Data;
    };

The PC field will be a value returned by a call to the traceback function.

The first time the function is called for a particular traceback, all the fields
except PC will be 0. The function should fill in the other fields if possible,
setting them to 0/nil if the information is not available. The Data field may be
used to store any useful information across calls. The More field should be set
to non-zero if there is more information for this PC, zero otherwise. If More is
set non-zero, the function will be called again with the same PC, and may return
different information (this is intended for use with inlined functions). If More
is zero, the function will be called with the next PC value in the traceback.
When the traceback is complete, the function will be called once more with PC
set to zero; this may be used to free any information. Each call will leave the
fields of the struct set to the same values they had upon return, except for the
PC field when the More field is zero. The function must not keep a copy of the
struct pointer between calls.

When calling SetCgoTraceback, the version argument is the version number of the
structs that the functions expect to receive. Currently this must be zero.

The symbolizer function may be nil, in which case the results of the traceback
function will be displayed as numbers. If the traceback function is nil, the
symbolizer function will never be called. The context function may be nil, in
which case the traceback function will only be called with the context field set
to zero. If the context function is nil, then calls from Go to C to Go will not
show a traceback for the C portion of the call stack.

SetCgoTraceback should be called only once, ideally from an init function.

<h2 id="SetFinalizer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mfinal.go#L299">SetFinalizer</a>
    <a href="#SetFinalizer">¶</a></h2>
<pre>func SetFinalizer(obj interface{}, finalizer interface{})</pre>

SetFinalizer sets the finalizer associated with obj to the provided finalizer
function. When the garbage collector finds an unreachable block with an
associated finalizer, it clears the association and runs finalizer(obj) in a
separate goroutine. This makes obj reachable again, but now without an
associated finalizer. Assuming that SetFinalizer is not called again, the next
time the garbage collector sees that obj is unreachable, it will free obj.

SetFinalizer(obj, nil) clears any finalizer associated with obj.

The argument obj must be a pointer to an object allocated by calling new, by
taking the address of a composite literal, or by taking the address of a local
variable. The argument finalizer must be a function that takes a single argument
to which obj's type can be assigned, and can have arbitrary ignored return
values. If either of these is not true, SetFinalizer may abort the program.

Finalizers are run in dependency order: if A points at B, both have finalizers,
and they are otherwise unreachable, only the finalizer for A runs; once A is
freed, the finalizer for B can run. If a cyclic structure includes a block with
a finalizer, that cycle is not guaranteed to be garbage collected and the
finalizer is not guaranteed to run, because there is no ordering that respects
the dependencies.

The finalizer for obj is scheduled to run at some arbitrary time after obj
becomes unreachable. There is no guarantee that finalizers will run before a
program exits, so typically they are useful only for releasing non-memory
resources associated with an object during a long-running program. For example,
an os.File object could use a finalizer to close the associated operating system
file descriptor when a program discards an os.File without calling Close, but it
would be a mistake to depend on a finalizer to flush an in-memory I/O buffer
such as a bufio.Writer, because the buffer would not be flushed at program exit.

It is not guaranteed that a finalizer will run if the size of *obj is zero
bytes.

It is not guaranteed that a finalizer will run for objects allocated in
initializers for package-level variables. Such objects may be linker-allocated,
not heap-allocated.

A finalizer may run as soon as an object becomes unreachable. In order to use
finalizers correctly, the program must ensure that the object is reachable until
it is no longer required. Objects stored in global variables, or that can be
found by tracing pointers from a global variable, are reachable. For other
objects, pass the object to a call of the KeepAlive function to mark the last
point in the function where the object must be reachable.

For example, if p points to a struct that contains a file descriptor d, and p
has a finalizer that closes that file descriptor, and if the last use of p in a
function is a call to syscall.Write(p.d, buf, size), then p may be unreachable
as soon as the program enters syscall.Write. The finalizer may run at that
moment, closing p.d, causing syscall.Write to fail because it is writing to a
closed file descriptor (or, worse, to an entirely different file descriptor
opened by a different goroutine). To avoid this problem, call
runtime.KeepAlive(p) after the call to syscall.Write.

A single goroutine runs all finalizers for a program, sequentially. If a
finalizer must run for a long time, it should do so by starting a new goroutine.

<h2 id="SetMutexProfileFraction">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L429">SetMutexProfileFraction</a>
    <a href="#SetMutexProfileFraction">¶</a></h2>
<pre>func SetMutexProfileFraction(rate <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

SetMutexProfileFraction controls the fraction of mutex contention events that
are reported in the mutex profile. On average 1/rate events are reported. The
previous rate is returned.

To turn off profiling entirely, pass rate 0. To just read the current rate, pass
rate -1. (For n>1 the details of sampling may change.)

<h2 id="Stack">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L770">Stack</a>
    <a href="#Stack">¶</a></h2>
<pre>func Stack(buf []<a href="/builtin/#byte">byte</a>, all <a href="/builtin/#bool">bool</a>) <a href="/builtin/#int">int</a></pre>

Stack formats a stack trace of the calling goroutine into buf and returns the
number of bytes written to buf. If all is true, Stack formats stack traces of
all other goroutines into buf after the trace for the current goroutine.

<h2 id="StartTrace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/trace.go#L163">StartTrace</a>
    <a href="#StartTrace">¶</a></h2>
<pre>func StartTrace() <a href="/builtin/#error">error</a></pre>

StartTrace enables tracing for the current process. While tracing, the data will
be buffered and available via ReadTrace. StartTrace returns an error if tracing
is already enabled. Most clients should use the runtime/trace package or the
testing package's -test.trace flag instead of calling StartTrace directly.

<h2 id="StopTrace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/trace.go#L254">StopTrace</a>
    <a href="#StopTrace">¶</a></h2>
<pre>func StopTrace()</pre>

StopTrace stops tracing, if it was previously enabled. StopTrace only returns
after all the reads for the trace have completed.

<h2 id="ThreadCreateProfile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L688">ThreadCreateProfile</a>
    <a href="#ThreadCreateProfile">¶</a></h2>
<pre>func ThreadCreateProfile(p []<a href="#StackRecord">StackRecord</a>) (n <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

ThreadCreateProfile returns n, the number of records in the thread creation
profile. If len(p) >= n, ThreadCreateProfile copies the profile into p and
returns n, true. If len(p) < n, ThreadCreateProfile does not change p and
returns n, false.

Most clients should use the runtime/pprof package instead of calling
ThreadCreateProfile directly.

<h2 id="UnlockOSThread">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/proc.go#L3190">UnlockOSThread</a>
    <a href="#UnlockOSThread">¶</a></h2>
<pre>func UnlockOSThread()</pre>

UnlockOSThread unwires the calling goroutine from its fixed operating system
thread. If the calling goroutine has not called LockOSThread, UnlockOSThread is
a no-op.

<h2 id="Version">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/extern.go#L219">Version</a>
    <a href="#Version">¶</a></h2>
<pre>func Version() <a href="/builtin/#string">string</a></pre>

Version returns the Go tree's version string. It is either the commit hash and
date at the time of the build or, when possible, a release tag like "go1.3".

<h2 id="BlockProfileRecord">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L611">BlockProfileRecord</a>
    <a href="#BlockProfileRecord">¶</a></h2>
<pre>type BlockProfileRecord struct {
<span id="BlockProfileRecord.Count"></span>    Count  <a href="/builtin/#int64">int64</a>
<span id="BlockProfileRecord.Cycles"></span>    Cycles <a href="/builtin/#int64">int64</a>
    <a href="#StackRecord">StackRecord</a>
}</pre>

BlockProfileRecord describes blocking events originated at a particular call
sequence (stack trace).

<h2 id="Error">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/error.go#L1">Error</a>
    <a href="#Error">¶</a></h2>
<pre>type Error interface {
    <a href="/builtin/#error">error</a>

    <span class="comment">// RuntimeError is a no-op function but</span>
    <span class="comment">// serves to distinguish types that are run time</span>
    <span class="comment">// errors from ordinary errors: a type is a</span>
    <span class="comment">// run time error if it has a RuntimeError method.</span>
    RuntimeError()
}</pre>

The Error interface identifies a run time error.

<h2 id="Frame">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/symtab.go#L15">Frame</a>
    <a href="#Frame">¶</a></h2>
<pre>type Frame struct {
<span id="Frame.PC"></span>    <span class="comment">// PC is the program counter for the location in this frame.</span>
    <span class="comment">// For a frame that calls another frame, this will be the</span>
    <span class="comment">// program counter of a call instruction. Because of inlining,</span>
    <span class="comment">// multiple frames may have the same PC value, but different</span>
    <span class="comment">// symbolic information.</span>
    PC <a href="/builtin/#uintptr">uintptr</a>

<span id="Frame.Func"></span>    <span class="comment">// Func is the Func value of this call frame. This may be nil</span>
    <span class="comment">// for non-Go code or fully inlined functions.</span>
    Func *<a href="#Func">Func</a>

<span id="Frame.Function"></span>    <span class="comment">// Function is the package path-qualified function name of</span>
    <span class="comment">// this call frame. If non-empty, this string uniquely</span>
    <span class="comment">// identifies a single function in the program.</span>
    <span class="comment">// This may be the empty string if not known.</span>
    <span class="comment">// If Func is not nil then Function == Func.Name().</span>
    Function <a href="/builtin/#string">string</a>

<span id="Frame.File"></span>    <span class="comment">// File and Line are the file name and line number of the</span>
    <span class="comment">// location in this frame. For non-leaf frames, this will be</span>
    <span class="comment">// the location of a call. These may be the empty string and</span>
    <span class="comment">// zero, respectively, if not known.</span>
    File <a href="/builtin/#string">string</a>
<span id="Frame.Line"></span>    Line <a href="/builtin/#int">int</a>

<span id="Frame.Entry"></span>    <span class="comment">// Entry point program counter for the function; may be zero</span>
    <span class="comment">// if not known. If Func is not nil then Entry ==</span>
    <span class="comment">// Func.Entry().</span>
    Entry <a href="/builtin/#uintptr">uintptr</a>
}</pre>

Frame is the information returned by Frames for each call frame.

<h2 id="Frames">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/symtab.go#L5">Frames</a>
    <a href="#Frames">¶</a></h2>
<pre>type Frames struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Frames may be used to get function/file/line information for a slice of PC
values returned by Callers.

<a id="example_Frames"></a>
Example:

    c := func() {
        // Ask runtime.Callers for up to 10 pcs, including runtime.Callers itself.
        pc := make([]uintptr, 10)
        n := runtime.Callers(0, pc)
        if n == 0 {
            // No pcs available. Stop now.
            // This can happen if the first argument to runtime.Callers is large.
            return
        }

        pc = pc[:n] // pass only valid pcs to runtime.CallersFrames
        frames := runtime.CallersFrames(pc)

        // Loop to get frames.
        // A fixed number of pcs can expand to an indefinite number of Frames.
        for {
            frame, more := frames.Next()
            // To keep this example's output stable
            // even if there are changes in the testing package,
            // stop unwinding when we leave package runtime.
            if !strings.Contains(frame.File, "runtime/") {
                break
            }
            fmt.Printf("- more:%v | %s\n", more, frame.Function)
            if !more {
                break
            }
        }
    }

    b := func() { c() }
    a := func() { b() }

    a()
    // Output:
    // - more:true | runtime.Callers
    // - more:true | runtime_test.ExampleFrames.func1
    // - more:true | runtime_test.ExampleFrames.func2
    // - more:true | runtime_test.ExampleFrames.func3
    // - more:true | runtime_test.ExampleFrames

<h3 id="CallersFrames">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/symtab.go#L76">CallersFrames</a>
    <a href="#CallersFrames">¶</a></h3>
<pre>func CallersFrames(callers []<a href="/builtin/#uintptr">uintptr</a>) *<a href="#Frames">Frames</a></pre>

CallersFrames takes a slice of PC values returned by Callers and prepares to
return function/file/line information. Do not change the slice until you are
done with the Frames.

<h3 id="Frames.Next">func (*Frames) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/symtab.go#L104">Next</a>
    <a href="#Frames.Next">¶</a></h3>
<pre>func (ci *<a href="#Frames">Frames</a>) Next() (frame <a href="#Frame">Frame</a>, more <a href="/builtin/#bool">bool</a>)</pre>

Next returns frame information for the next caller. If more is false, there are
no more callers (the Frame value is valid).

<h2 id="Func">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/symtab.go#L303">Func</a>
    <a href="#Func">¶</a></h2>
<pre>type Func struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Func represents a Go function in the running binary.

<h3 id="FuncForPC">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/symtab.go#L574">FuncForPC</a>
    <a href="#FuncForPC">¶</a></h3>
<pre>func FuncForPC(pc <a href="/builtin/#uintptr">uintptr</a>) *<a href="#Func">Func</a></pre>

FuncForPC returns a *Func describing the function that contains the given
program counter address, or else nil.

If pc represents multiple functions because of inlining, it returns the *Func
describing the outermost function.

<h3 id="Func.Entry">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/symtab.go#L587">Entry</a>
    <a href="#Func.Entry">¶</a></h3>
<pre>func (f *<a href="#Func">Func</a>) Entry() <a href="/builtin/#uintptr">uintptr</a></pre>

Entry returns the entry address of the function.

<h3 id="Func.FileLine">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/symtab.go#L595">FileLine</a>
    <a href="#Func.FileLine">¶</a></h3>
<pre>func (f *<a href="#Func">Func</a>) FileLine(pc <a href="/builtin/#uintptr">uintptr</a>) (file <a href="/builtin/#string">string</a>, line <a href="/builtin/#int">int</a>)</pre>

FileLine returns the file name and line number of the source code corresponding
to the program counter pc. The result will not be accurate if pc is not a
program counter within f.

<h3 id="Func.Name">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/symtab.go#L579">Name</a>
    <a href="#Func.Name">¶</a></h3>
<pre>func (f *<a href="#Func">Func</a>) Name() <a href="/builtin/#string">string</a></pre>

Name returns the name of the function.

<h2 id="MemProfileRecord">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L487">MemProfileRecord</a>
    <a href="#MemProfileRecord">¶</a></h2>
<pre>type MemProfileRecord struct {
<span id="MemProfileRecord.AllocBytes"></span>    AllocBytes, FreeBytes     <a href="/builtin/#int64">int64</a>       <span class="comment">// number of bytes allocated, freed</span>
<span id="MemProfileRecord.AllocObjects"></span>    AllocObjects, FreeObjects <a href="/builtin/#int64">int64</a>       <span class="comment">// number of objects allocated, freed</span>
<span id="MemProfileRecord.Stack0"></span>    Stack0                    [32]<a href="/builtin/#uintptr">uintptr</a> <span class="comment">// stack trace for this record; ends at first 0 entry</span>
}</pre>

A MemProfileRecord describes the live objects allocated by a particular call
sequence (stack trace).

<h3 id="MemProfileRecord.InUseBytes">func (*MemProfileRecord) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L494">InUseBytes</a>
    <a href="#MemProfileRecord.InUseBytes">¶</a></h3>
<pre>func (r *<a href="#MemProfileRecord">MemProfileRecord</a>) InUseBytes() <a href="/builtin/#int64">int64</a></pre>

InUseBytes returns the number of bytes in use (AllocBytes - FreeBytes).

<h3 id="MemProfileRecord.InUseObjects">func (*MemProfileRecord) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L497">InUseObjects</a>
    <a href="#MemProfileRecord.InUseObjects">¶</a></h3>
<pre>func (r *<a href="#MemProfileRecord">MemProfileRecord</a>) InUseObjects() <a href="/builtin/#int64">int64</a></pre>

InUseObjects returns the number of objects in use (AllocObjects - FreeObjects).

<h3 id="MemProfileRecord.Stack">func (*MemProfileRecord) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L503">Stack</a>
    <a href="#MemProfileRecord.Stack">¶</a></h3>
<pre>func (r *<a href="#MemProfileRecord">MemProfileRecord</a>) Stack() []<a href="/builtin/#uintptr">uintptr</a></pre>

Stack returns the stack trace associated with the record, a prefix of r.Stack0.

<h2 id="MemStats">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mstats.go#L149">MemStats</a>
    <a href="#MemStats">¶</a></h2>
<pre>type MemStats struct {

<span id="MemStats.Alloc"></span>    <span class="comment">// Alloc is bytes of allocated heap objects.</span>
    <span class="comment">//</span>
    <span class="comment">// This is the same as HeapAlloc (see below).</span>
    Alloc <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.TotalAlloc"></span>    <span class="comment">// TotalAlloc is cumulative bytes allocated for heap objects.</span>
    <span class="comment">//</span>
    <span class="comment">// TotalAlloc increases as heap objects are allocated, but</span>
    <span class="comment">// unlike Alloc and HeapAlloc, it does not decrease when</span>
    <span class="comment">// objects are freed.</span>
    TotalAlloc <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.Sys"></span>    <span class="comment">// Sys is the total bytes of memory obtained from the OS.</span>
    <span class="comment">//</span>
    <span class="comment">// Sys is the sum of the XSys fields below. Sys measures the</span>
    <span class="comment">// virtual address space reserved by the Go runtime for the</span>
    <span class="comment">// heap, stacks, and other internal data structures. It&#39;s</span>
    <span class="comment">// likely that not all of the virtual address space is backed</span>
    <span class="comment">// by physical memory at any given moment, though in general</span>
    <span class="comment">// it all was at some point.</span>
    Sys <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.Lookups"></span>    <span class="comment">// Lookups is the number of pointer lookups performed by the</span>
    <span class="comment">// runtime.</span>
    <span class="comment">//</span>
    <span class="comment">// This is primarily useful for debugging runtime internals.</span>
    Lookups <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.Mallocs"></span>    <span class="comment">// Mallocs is the cumulative count of heap objects allocated.</span>
    <span class="comment">// The number of live objects is Mallocs - Frees.</span>
    Mallocs <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.Frees"></span>    <span class="comment">// Frees is the cumulative count of heap objects freed.</span>
    Frees <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.HeapAlloc"></span>    <span class="comment">// HeapAlloc is bytes of allocated heap objects.</span>
    <span class="comment">//</span>
    <span class="comment">// &#34;Allocated&#34; heap objects include all reachable objects, as</span>
    <span class="comment">// well as unreachable objects that the garbage collector has</span>
    <span class="comment">// not yet freed. Specifically, HeapAlloc increases as heap</span>
    <span class="comment">// objects are allocated and decreases as the heap is swept</span>
    <span class="comment">// and unreachable objects are freed. Sweeping occurs</span>
    <span class="comment">// incrementally between GC cycles, so these two processes</span>
    <span class="comment">// occur simultaneously, and as a result HeapAlloc tends to</span>
    <span class="comment">// change smoothly (in contrast with the sawtooth that is</span>
    <span class="comment">// typical of stop-the-world garbage collectors).</span>
    HeapAlloc <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.HeapSys"></span>    <span class="comment">// HeapSys is bytes of heap memory obtained from the OS.</span>
    <span class="comment">//</span>
    <span class="comment">// HeapSys measures the amount of virtual address space</span>
    <span class="comment">// reserved for the heap. This includes virtual address space</span>
    <span class="comment">// that has been reserved but not yet used, which consumes no</span>
    <span class="comment">// physical memory, but tends to be small, as well as virtual</span>
    <span class="comment">// address space for which the physical memory has been</span>
    <span class="comment">// returned to the OS after it became unused (see HeapReleased</span>
    <span class="comment">// for a measure of the latter).</span>
    <span class="comment">//</span>
    <span class="comment">// HeapSys estimates the largest size the heap has had.</span>
    HeapSys <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.HeapIdle"></span>    <span class="comment">// HeapIdle is bytes in idle (unused) spans.</span>
    <span class="comment">//</span>
    <span class="comment">// Idle spans have no objects in them. These spans could be</span>
    <span class="comment">// (and may already have been) returned to the OS, or they can</span>
    <span class="comment">// be reused for heap allocations, or they can be reused as</span>
    <span class="comment">// stack memory.</span>
    <span class="comment">//</span>
    <span class="comment">// HeapIdle minus HeapReleased estimates the amount of memory</span>
    <span class="comment">// that could be returned to the OS, but is being retained by</span>
    <span class="comment">// the runtime so it can grow the heap without requesting more</span>
    <span class="comment">// memory from the OS. If this difference is significantly</span>
    <span class="comment">// larger than the heap size, it indicates there was a recent</span>
    <span class="comment">// transient spike in live heap size.</span>
    HeapIdle <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.HeapInuse"></span>    <span class="comment">// HeapInuse is bytes in in-use spans.</span>
    <span class="comment">//</span>
    <span class="comment">// In-use spans have at least one object in them. These spans</span>
    <span class="comment">// can only be used for other objects of roughly the same</span>
    <span class="comment">// size.</span>
    <span class="comment">//</span>
    <span class="comment">// HeapInuse minus HeapAlloc esimates the amount of memory</span>
    <span class="comment">// that has been dedicated to particular size classes, but is</span>
    <span class="comment">// not currently being used. This is an upper bound on</span>
    <span class="comment">// fragmentation, but in general this memory can be reused</span>
    <span class="comment">// efficiently.</span>
    HeapInuse <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.HeapReleased"></span>    <span class="comment">// HeapReleased is bytes of physical memory returned to the OS.</span>
    <span class="comment">//</span>
    <span class="comment">// This counts heap memory from idle spans that was returned</span>
    <span class="comment">// to the OS and has not yet been reacquired for the heap.</span>
    HeapReleased <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.HeapObjects"></span>    <span class="comment">// HeapObjects is the number of allocated heap objects.</span>
    <span class="comment">//</span>
    <span class="comment">// Like HeapAlloc, this increases as objects are allocated and</span>
    <span class="comment">// decreases as the heap is swept and unreachable objects are</span>
    <span class="comment">// freed.</span>
    HeapObjects <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.StackInuse"></span>    <span class="comment">// StackInuse is bytes in stack spans.</span>
    <span class="comment">//</span>
    <span class="comment">// In-use stack spans have at least one stack in them. These</span>
    <span class="comment">// spans can only be used for other stacks of the same size.</span>
    <span class="comment">//</span>
    <span class="comment">// There is no StackIdle because unused stack spans are</span>
    <span class="comment">// returned to the heap (and hence counted toward HeapIdle).</span>
    StackInuse <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.StackSys"></span>    <span class="comment">// StackSys is bytes of stack memory obtained from the OS.</span>
    <span class="comment">//</span>
    <span class="comment">// StackSys is StackInuse, plus any memory obtained directly</span>
    <span class="comment">// from the OS for OS thread stacks (which should be minimal).</span>
    StackSys <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.MSpanInuse"></span>    <span class="comment">// MSpanInuse is bytes of allocated mspan structures.</span>
    MSpanInuse <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.MSpanSys"></span>    <span class="comment">// MSpanSys is bytes of memory obtained from the OS for mspan</span>
    <span class="comment">// structures.</span>
    MSpanSys <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.MCacheInuse"></span>    <span class="comment">// MCacheInuse is bytes of allocated mcache structures.</span>
    MCacheInuse <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.MCacheSys"></span>    <span class="comment">// MCacheSys is bytes of memory obtained from the OS for</span>
    <span class="comment">// mcache structures.</span>
    MCacheSys <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.BuckHashSys"></span>    <span class="comment">// BuckHashSys is bytes of memory in profiling bucket hash tables.</span>
    BuckHashSys <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.GCSys"></span>    <span class="comment">// GCSys is bytes of memory in garbage collection metadata.</span>
    GCSys <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.OtherSys"></span>    <span class="comment">// OtherSys is bytes of memory in miscellaneous off-heap</span>
    <span class="comment">// runtime allocations.</span>
    OtherSys <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.NextGC"></span>    <span class="comment">// NextGC is the target heap size of the next GC cycle.</span>
    <span class="comment">//</span>
    <span class="comment">// The garbage collector&#39;s goal is to keep HeapAlloc ≤ NextGC.</span>
    <span class="comment">// At the end of each GC cycle, the target for the next cycle</span>
    <span class="comment">// is computed based on the amount of reachable data and the</span>
    <span class="comment">// value of GOGC.</span>
    NextGC <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.LastGC"></span>    <span class="comment">// LastGC is the time the last garbage collection finished, as</span>
    <span class="comment">// nanoseconds since 1970 (the UNIX epoch).</span>
    LastGC <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.PauseTotalNs"></span>    <span class="comment">// PauseTotalNs is the cumulative nanoseconds in GC</span>
    <span class="comment">// stop-the-world pauses since the program started.</span>
    <span class="comment">//</span>
    <span class="comment">// During a stop-the-world pause, all goroutines are paused</span>
    <span class="comment">// and only the garbage collector can run.</span>
    PauseTotalNs <a href="/builtin/#uint64">uint64</a>

<span id="MemStats.PauseNs"></span>    <span class="comment">// PauseNs is a circular buffer of recent GC stop-the-world</span>
    <span class="comment">// pause times in nanoseconds.</span>
    <span class="comment">//</span>
    <span class="comment">// The most recent pause is at PauseNs[(NumGC+255)%256]. In</span>
    <span class="comment">// general, PauseNs[N%256] records the time paused in the most</span>
    <span class="comment">// recent N%256th GC cycle. There may be multiple pauses per</span>
    <span class="comment">// GC cycle; this is the sum of all pauses during a cycle.</span>
    PauseNs [256]<a href="/builtin/#uint64">uint64</a>

<span id="MemStats.PauseEnd"></span>    <span class="comment">// PauseEnd is a circular buffer of recent GC pause end times,</span>
    <span class="comment">// as nanoseconds since 1970 (the UNIX epoch).</span>
    <span class="comment">//</span>
    <span class="comment">// This buffer is filled the same way as PauseNs. There may be</span>
    <span class="comment">// multiple pauses per GC cycle; this records the end of the</span>
    <span class="comment">// last pause in a cycle.</span>
    PauseEnd [256]<a href="/builtin/#uint64">uint64</a>

<span id="MemStats.NumGC"></span>    <span class="comment">// NumGC is the number of completed GC cycles.</span>
    NumGC <a href="/builtin/#uint32">uint32</a>

<span id="MemStats.NumForcedGC"></span>    <span class="comment">// NumForcedGC is the number of GC cycles that were forced by</span>
    <span class="comment">// the application calling the GC function.</span>
    NumForcedGC <a href="/builtin/#uint32">uint32</a>

<span id="MemStats.GCCPUFraction"></span>    <span class="comment">// GCCPUFraction is the fraction of this program&#39;s available</span>
    <span class="comment">// CPU time used by the GC since the program started.</span>
    <span class="comment">//</span>
    <span class="comment">// GCCPUFraction is expressed as a number between 0 and 1,</span>
    <span class="comment">// where 0 means GC has consumed none of this program&#39;s CPU. A</span>
    <span class="comment">// program&#39;s available CPU time is defined as the integral of</span>
    <span class="comment">// GOMAXPROCS since the program started. That is, if</span>
    <span class="comment">// GOMAXPROCS is 2 and a program has been running for 10</span>
    <span class="comment">// seconds, its &#34;available CPU&#34; is 20 seconds. GCCPUFraction</span>
    <span class="comment">// does not include CPU time used for write barrier activity.</span>
    <span class="comment">//</span>
    <span class="comment">// This is the same as the fraction of CPU reported by</span>
    <span class="comment">// GODEBUG=gctrace=1.</span>
    GCCPUFraction <a href="/builtin/#float64">float64</a>

<span id="MemStats.EnableGC"></span>    <span class="comment">// EnableGC indicates that GC is enabled. It is always true,</span>
    <span class="comment">// even if GOGC=off.</span>
    EnableGC <a href="/builtin/#bool">bool</a>

<span id="MemStats.DebugGC"></span>    <span class="comment">// DebugGC is currently unused.</span>
    DebugGC <a href="/builtin/#bool">bool</a>

<span id="MemStats.BySize"></span>    <span class="comment">// BySize reports per-size class allocation statistics.</span>
    <span class="comment">//</span>
    <span class="comment">// BySize[N] gives statistics for allocations of size S where</span>
    <span class="comment">// BySize[N-1].Size &lt; S ≤ BySize[N].Size.</span>
    <span class="comment">//</span>
    <span class="comment">// This does not report allocations larger than BySize[60].Size.</span>
    BySize [61]struct {
        <span class="comment">// Size is the maximum byte size of an object in this</span>
        <span class="comment">// size class.</span>
        Size <a href="/builtin/#uint32">uint32</a>

        <span class="comment">// Mallocs is the cumulative count of heap objects</span>
        <span class="comment">// allocated in this size class. The cumulative bytes</span>
        <span class="comment">// of allocation is Size*Mallocs. The number of live</span>
        <span class="comment">// objects in this size class is Mallocs - Frees.</span>
        Mallocs <a href="/builtin/#uint64">uint64</a>

        <span class="comment">// Frees is the cumulative count of heap objects freed</span>
        <span class="comment">// in this size class.</span>
        Frees <a href="/builtin/#uint64">uint64</a>
    }
}</pre>

A MemStats records statistics about the memory allocator.

<h2 id="StackRecord">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L454">StackRecord</a>
    <a href="#StackRecord">¶</a></h2>
<pre>type StackRecord struct {
<span id="StackRecord.Stack0"></span>    Stack0 [32]<a href="/builtin/#uintptr">uintptr</a> <span class="comment">// stack trace for this record; ends at first 0 entry</span>
}</pre>

A StackRecord describes a single execution stack.

<h3 id="StackRecord.Stack">func (*StackRecord) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/mprof.go#L460">Stack</a>
    <a href="#StackRecord.Stack">¶</a></h3>
<pre>func (r *<a href="#StackRecord">StackRecord</a>) Stack() []<a href="/builtin/#uintptr">uintptr</a></pre>

Stack returns the stack trace associated with the record, a prefix of r.Stack0.

<h2 id="TypeAssertionError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/error.go#L11">TypeAssertionError</a>
    <a href="#TypeAssertionError">¶</a></h2>
<pre>type TypeAssertionError struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A TypeAssertionError explains a failed type assertion.

<h3 id="TypeAssertionError.Error">func (*TypeAssertionError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/error.go#L20">Error</a>
    <a href="#TypeAssertionError.Error">¶</a></h3>
<pre>func (e *<a href="#TypeAssertionError">TypeAssertionError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="TypeAssertionError.RuntimeError">func (*TypeAssertionError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/runtime/error.go#L18">RuntimeError</a>
    <a href="#TypeAssertionError.RuntimeError">¶</a></h3>
<pre>func (*<a href="#TypeAssertionError">TypeAssertionError</a>) RuntimeError()</pre>


## Subdirectories
- [..](..)
- [cgo](cgo/)
- [debug](debug/)
- [msan](msan/)
- [pprof](pprof/)
- [race](race/)
- [trace](trace/)
