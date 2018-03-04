version: 1.10
## package atomic

  `import "sync/atomic"`

## Overview

Package atomic provides low-level atomic memory primitives useful for
implementing synchronization algorithms.

These functions require great care to be used correctly. Except for special,
low-level applications, synchronization is better done with channels or the
facilities of the sync package. Share memory by communicating; don't communicate
by sharing memory.

The swap operation, implemented by the SwapT functions, is the atomic equivalent
of:

    old = *addr
    *addr = new
    return old

The compare-and-swap operation, implemented by the CompareAndSwapT functions, is
the atomic equivalent of:

    if *addr == old {
    	*addr = new
    	return true
    }
    return false

The add operation, implemented by the AddT functions, is the atomic equivalent
of:

    *addr += delta
    return *addr

The load and store operations, implemented by the LoadT and StoreT functions,
are the atomic equivalents of "return *addr" and "*addr = val".

## Index

- [func AddInt32(addr *int32, delta int32) (new int32)](#AddInt32)
- [func AddInt64(addr *int64, delta int64) (new int64)](#AddInt64)
- [func AddUint32(addr *uint32, delta uint32) (new uint32)](#AddUint32)
- [func AddUint64(addr *uint64, delta uint64) (new uint64)](#AddUint64)
- [func AddUintptr(addr *uintptr, delta uintptr) (new uintptr)](#AddUintptr)
- [func CompareAndSwapInt32(addr *int32, old, new int32) (swapped bool)](#CompareAndSwapInt32)
- [func CompareAndSwapInt64(addr *int64, old, new int64) (swapped bool)](#CompareAndSwapInt64)
- [func CompareAndSwapPointer(addr *unsafe.Pointer, old, new unsafe.Pointer) (swapped bool)](#CompareAndSwapPointer)
- [func CompareAndSwapUint32(addr *uint32, old, new uint32) (swapped bool)](#CompareAndSwapUint32)
- [func CompareAndSwapUint64(addr *uint64, old, new uint64) (swapped bool)](#CompareAndSwapUint64)
- [func CompareAndSwapUintptr(addr *uintptr, old, new uintptr) (swapped bool)](#CompareAndSwapUintptr)
- [func LoadInt32(addr *int32) (val int32)](#LoadInt32)
- [func LoadInt64(addr *int64) (val int64)](#LoadInt64)
- [func LoadPointer(addr *unsafe.Pointer) (val unsafe.Pointer)](#LoadPointer)
- [func LoadUint32(addr *uint32) (val uint32)](#LoadUint32)
- [func LoadUint64(addr *uint64) (val uint64)](#LoadUint64)
- [func LoadUintptr(addr *uintptr) (val uintptr)](#LoadUintptr)
- [func StoreInt32(addr *int32, val int32)](#StoreInt32)
- [func StoreInt64(addr *int64, val int64)](#StoreInt64)
- [func StorePointer(addr *unsafe.Pointer, val unsafe.Pointer)](#StorePointer)
- [func StoreUint32(addr *uint32, val uint32)](#StoreUint32)
- [func StoreUint64(addr *uint64, val uint64)](#StoreUint64)
- [func StoreUintptr(addr *uintptr, val uintptr)](#StoreUintptr)
- [func SwapInt32(addr *int32, new int32) (old int32)](#SwapInt32)
- [func SwapInt64(addr *int64, new int64) (old int64)](#SwapInt64)
- [func SwapPointer(addr *unsafe.Pointer, new unsafe.Pointer) (old unsafe.Pointer)](#SwapPointer)
- [func SwapUint32(addr *uint32, new uint32) (old uint32)](#SwapUint32)
- [func SwapUint64(addr *uint64, new uint64) (old uint64)](#SwapUint64)
- [func SwapUintptr(addr *uintptr, new uintptr) (old uintptr)](#SwapUintptr)
- [type Value](#Value)
  - [func (v *Value) Load() (x interface{})](#Value.Load)
  - [func (v *Value) Store(x interface{})](#Value.Store)
- [Bugs](#pkg-note-BUG)

### Examples

- [Value (Config)](#exampleValue_config)
- [Value (ReadMostly)](#exampleValue_readMostly)

### Package files
 [doc.go](//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go) [value.go](//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/value.go)

<h2 id="AddInt32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L82">AddInt32</a>
    <a href="#AddInt32">¶</a></h2>
<pre>func AddInt32(addr *<a href="/builtin/#int32">int32</a>, delta <a href="/builtin/#int32">int32</a>) (new <a href="/builtin/#int32">int32</a>)</pre>

AddInt32 atomically adds delta to *addr and returns the new value.

<h2 id="AddInt64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L90">AddInt64</a>
    <a href="#AddInt64">¶</a></h2>
<pre>func AddInt64(addr *<a href="/builtin/#int64">int64</a>, delta <a href="/builtin/#int64">int64</a>) (new <a href="/builtin/#int64">int64</a>)</pre>

AddInt64 atomically adds delta to *addr and returns the new value.

<h2 id="AddUint32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L87">AddUint32</a>
    <a href="#AddUint32">¶</a></h2>
<pre>func AddUint32(addr *<a href="/builtin/#uint32">uint32</a>, delta <a href="/builtin/#uint32">uint32</a>) (new <a href="/builtin/#uint32">uint32</a>)</pre>

AddUint32 atomically adds delta to *addr and returns the new value. To subtract
a signed positive constant value c from x, do AddUint32(&x, ^uint32(c-1)). In
particular, to decrement x, do AddUint32(&x, ^uint32(0)).

<h2 id="AddUint64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L95">AddUint64</a>
    <a href="#AddUint64">¶</a></h2>
<pre>func AddUint64(addr *<a href="/builtin/#uint64">uint64</a>, delta <a href="/builtin/#uint64">uint64</a>) (new <a href="/builtin/#uint64">uint64</a>)</pre>

AddUint64 atomically adds delta to *addr and returns the new value. To subtract
a signed positive constant value c from x, do AddUint64(&x, ^uint64(c-1)). In
particular, to decrement x, do AddUint64(&x, ^uint64(0)).

<h2 id="AddUintptr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L98">AddUintptr</a>
    <a href="#AddUintptr">¶</a></h2>
<pre>func AddUintptr(addr *<a href="/builtin/#uintptr">uintptr</a>, delta <a href="/builtin/#uintptr">uintptr</a>) (new <a href="/builtin/#uintptr">uintptr</a>)</pre>

AddUintptr atomically adds delta to *addr and returns the new value.

<h2 id="CompareAndSwapInt32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L64">CompareAndSwapInt32</a>
    <a href="#CompareAndSwapInt32">¶</a></h2>
<pre>func CompareAndSwapInt32(addr *<a href="/builtin/#int32">int32</a>, old, new <a href="/builtin/#int32">int32</a>) (swapped <a href="/builtin/#bool">bool</a>)</pre>

CompareAndSwapInt32 executes the compare-and-swap operation for an int32 value.

<h2 id="CompareAndSwapInt64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L67">CompareAndSwapInt64</a>
    <a href="#CompareAndSwapInt64">¶</a></h2>
<pre>func CompareAndSwapInt64(addr *<a href="/builtin/#int64">int64</a>, old, new <a href="/builtin/#int64">int64</a>) (swapped <a href="/builtin/#bool">bool</a>)</pre>

CompareAndSwapInt64 executes the compare-and-swap operation for an int64 value.

<h2 id="CompareAndSwapPointer">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L79">CompareAndSwapPointer</a>
    <a href="#CompareAndSwapPointer">¶</a></h2>
<pre>func CompareAndSwapPointer(addr *<a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>, old, new <a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>) (swapped <a href="/builtin/#bool">bool</a>)</pre>

CompareAndSwapPointer executes the compare-and-swap operation for a
unsafe.Pointer value.

<h2 id="CompareAndSwapUint32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L70">CompareAndSwapUint32</a>
    <a href="#CompareAndSwapUint32">¶</a></h2>
<pre>func CompareAndSwapUint32(addr *<a href="/builtin/#uint32">uint32</a>, old, new <a href="/builtin/#uint32">uint32</a>) (swapped <a href="/builtin/#bool">bool</a>)</pre>

CompareAndSwapUint32 executes the compare-and-swap operation for a uint32 value.

<h2 id="CompareAndSwapUint64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L73">CompareAndSwapUint64</a>
    <a href="#CompareAndSwapUint64">¶</a></h2>
<pre>func CompareAndSwapUint64(addr *<a href="/builtin/#uint64">uint64</a>, old, new <a href="/builtin/#uint64">uint64</a>) (swapped <a href="/builtin/#bool">bool</a>)</pre>

CompareAndSwapUint64 executes the compare-and-swap operation for a uint64 value.

<h2 id="CompareAndSwapUintptr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L76">CompareAndSwapUintptr</a>
    <a href="#CompareAndSwapUintptr">¶</a></h2>
<pre>func CompareAndSwapUintptr(addr *<a href="/builtin/#uintptr">uintptr</a>, old, new <a href="/builtin/#uintptr">uintptr</a>) (swapped <a href="/builtin/#bool">bool</a>)</pre>

CompareAndSwapUintptr executes the compare-and-swap operation for a uintptr
value.

<h2 id="LoadInt32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L101">LoadInt32</a>
    <a href="#LoadInt32">¶</a></h2>
<pre>func LoadInt32(addr *<a href="/builtin/#int32">int32</a>) (val <a href="/builtin/#int32">int32</a>)</pre>

LoadInt32 atomically loads *addr.

<h2 id="LoadInt64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L104">LoadInt64</a>
    <a href="#LoadInt64">¶</a></h2>
<pre>func LoadInt64(addr *<a href="/builtin/#int64">int64</a>) (val <a href="/builtin/#int64">int64</a>)</pre>

LoadInt64 atomically loads *addr.

<h2 id="LoadPointer">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L116">LoadPointer</a>
    <a href="#LoadPointer">¶</a></h2>
<pre>func LoadPointer(addr *<a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>) (val <a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>)</pre>

LoadPointer atomically loads *addr.

<h2 id="LoadUint32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L107">LoadUint32</a>
    <a href="#LoadUint32">¶</a></h2>
<pre>func LoadUint32(addr *<a href="/builtin/#uint32">uint32</a>) (val <a href="/builtin/#uint32">uint32</a>)</pre>

LoadUint32 atomically loads *addr.

<h2 id="LoadUint64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L110">LoadUint64</a>
    <a href="#LoadUint64">¶</a></h2>
<pre>func LoadUint64(addr *<a href="/builtin/#uint64">uint64</a>) (val <a href="/builtin/#uint64">uint64</a>)</pre>

LoadUint64 atomically loads *addr.

<h2 id="LoadUintptr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L113">LoadUintptr</a>
    <a href="#LoadUintptr">¶</a></h2>
<pre>func LoadUintptr(addr *<a href="/builtin/#uintptr">uintptr</a>) (val <a href="/builtin/#uintptr">uintptr</a>)</pre>

LoadUintptr atomically loads *addr.

<h2 id="StoreInt32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L119">StoreInt32</a>
    <a href="#StoreInt32">¶</a></h2>
<pre>func StoreInt32(addr *<a href="/builtin/#int32">int32</a>, val <a href="/builtin/#int32">int32</a>)</pre>

StoreInt32 atomically stores val into *addr.

<h2 id="StoreInt64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L122">StoreInt64</a>
    <a href="#StoreInt64">¶</a></h2>
<pre>func StoreInt64(addr *<a href="/builtin/#int64">int64</a>, val <a href="/builtin/#int64">int64</a>)</pre>

StoreInt64 atomically stores val into *addr.

<h2 id="StorePointer">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L134">StorePointer</a>
    <a href="#StorePointer">¶</a></h2>
<pre>func StorePointer(addr *<a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>, val <a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>)</pre>

StorePointer atomically stores val into *addr.

<h2 id="StoreUint32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L125">StoreUint32</a>
    <a href="#StoreUint32">¶</a></h2>
<pre>func StoreUint32(addr *<a href="/builtin/#uint32">uint32</a>, val <a href="/builtin/#uint32">uint32</a>)</pre>

StoreUint32 atomically stores val into *addr.

<h2 id="StoreUint64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L128">StoreUint64</a>
    <a href="#StoreUint64">¶</a></h2>
<pre>func StoreUint64(addr *<a href="/builtin/#uint64">uint64</a>, val <a href="/builtin/#uint64">uint64</a>)</pre>

StoreUint64 atomically stores val into *addr.

<h2 id="StoreUintptr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L131">StoreUintptr</a>
    <a href="#StoreUintptr">¶</a></h2>
<pre>func StoreUintptr(addr *<a href="/builtin/#uintptr">uintptr</a>, val <a href="/builtin/#uintptr">uintptr</a>)</pre>

StoreUintptr atomically stores val into *addr.

<h2 id="SwapInt32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L46">SwapInt32</a>
    <a href="#SwapInt32">¶</a></h2>
<pre>func SwapInt32(addr *<a href="/builtin/#int32">int32</a>, new <a href="/builtin/#int32">int32</a>) (old <a href="/builtin/#int32">int32</a>)</pre>

SwapInt32 atomically stores new into *addr and returns the previous *addr value.

<h2 id="SwapInt64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L49">SwapInt64</a>
    <a href="#SwapInt64">¶</a></h2>
<pre>func SwapInt64(addr *<a href="/builtin/#int64">int64</a>, new <a href="/builtin/#int64">int64</a>) (old <a href="/builtin/#int64">int64</a>)</pre>

SwapInt64 atomically stores new into *addr and returns the previous *addr value.

<h2 id="SwapPointer">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L61">SwapPointer</a>
    <a href="#SwapPointer">¶</a></h2>
<pre>func SwapPointer(addr *<a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>, new <a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>) (old <a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>)</pre>

SwapPointer atomically stores new into *addr and returns the previous *addr
value.

<h2 id="SwapUint32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L52">SwapUint32</a>
    <a href="#SwapUint32">¶</a></h2>
<pre>func SwapUint32(addr *<a href="/builtin/#uint32">uint32</a>, new <a href="/builtin/#uint32">uint32</a>) (old <a href="/builtin/#uint32">uint32</a>)</pre>

SwapUint32 atomically stores new into *addr and returns the previous *addr
value.

<h2 id="SwapUint64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L55">SwapUint64</a>
    <a href="#SwapUint64">¶</a></h2>
<pre>func SwapUint64(addr *<a href="/builtin/#uint64">uint64</a>, new <a href="/builtin/#uint64">uint64</a>) (old <a href="/builtin/#uint64">uint64</a>)</pre>

SwapUint64 atomically stores new into *addr and returns the previous *addr
value.

<h2 id="SwapUintptr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L58">SwapUintptr</a>
    <a href="#SwapUintptr">¶</a></h2>
<pre>func SwapUintptr(addr *<a href="/builtin/#uintptr">uintptr</a>, new <a href="/builtin/#uintptr">uintptr</a>) (old <a href="/builtin/#uintptr">uintptr</a>)</pre>

SwapUintptr atomically stores new into *addr and returns the previous *addr
value.

<h2 id="Value">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/value.go#L6">Value</a>
    <a href="#Value">¶</a></h2>
<pre>type Value struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Value provides an atomic load and store of a consistently typed value. The
zero value for a Value returns nil from Load. Once Store has been called, a
Value must not be copied.

A Value must not be copied after first use.

<a id="exampleValue_config"></a>
Example:

    var config Value // holds current server configuration
    // Create initial config value and store into config.
    config.Store(loadConfig())
    go func() {
        // Reload config every 10 seconds
        // and update config value with the new version.
        for {
            time.Sleep(10 * time.Second)
            config.Store(loadConfig())
        }
    }()
    // Create worker goroutines that handle incoming requests
    // using the latest config value.
    for i := 0; i < 10; i++ {
        go func() {
            for r := range requests() {
                c := config.Load()
                // Handle request r using config c.
                _, _ = r, c
            }
        }()
    }


<a id="exampleValue_readMostly"></a>
Example:

    type Map map[string]string
    var m Value
    m.Store(make(Map))
    var mu sync.Mutex // used only by writers
    // read function can be used to read the data without further synchronization
    read := func(key string) (val string) {
        m1 := m.Load().(Map)
        return m1[key]
    }
    // insert function can be used to update the data without further synchronization
    insert := func(key, val string) {
        mu.Lock() // synchronize with other potential writers
        defer mu.Unlock()
        m1 := m.Load().(Map) // load current value of the data structure
        m2 := make(Map)      // create a new value
        for k, v := range m1 {
            m2[k] = v // copy all data from the current object to the new one
        }
        m2[key] = val // do the update that we need
        m.Store(m2)   // atomically replace the current object with the new one
        // At this point all new readers start working with the new version.
        // The old version will be garbage collected once the existing readers
        // (if any) are done with it.
    }
    _, _ = read, insert

<h3 id="Value.Load">func (*Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/value.go#L18">Load</a>
    <a href="#Value.Load">¶</a></h3>
<pre>func (v *<a href="#Value">Value</a>) Load() (x interface{})</pre>

Load returns the value set by the most recent Store. It returns nil if there has
been no call to Store for this Value.

<h3 id="Value.Store">func (*Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/value.go#L35">Store</a>
    <a href="#Value.Store">¶</a></h3>
<pre>func (v *<a href="#Value">Value</a>) Store(x interface{})</pre>

Store sets the value of the Value to x. All calls to Store for a given Value
must use values of the same concrete type. Store of an inconsistent type panics,
as does Store(nil).

<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/release-branch.go1.10/src/sync/atomic/doc.go#L36)  On x86-32, the 64-bit functions use instructions unavailable before the
  Pentium MMX.

  On non-Linux ARM, the 64-bit functions use instructions unavailable before
  the ARMv6k core.

  On both ARM and x86-32, it is the caller's responsibility to arrange for
  64-bit alignment of 64-bit words accessed atomically. The first word in a
  variable or in an allocated struct, array, or slice can be relied upon to be
  64-bit aligned.


