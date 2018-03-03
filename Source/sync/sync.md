version: 1.10
## package sync

  `import "sync"`

## Overview

Package sync provides basic synchronization primitives such as mutual exclusion
locks. Other than the Once and WaitGroup types, most are intended for use by
low-level library routines. Higher-level synchronization is better done via
channels and communication.

Values containing the types defined in this package should not be copied.

## Index

- [type Cond](#Cond)
  - [func NewCond(l Locker) *Cond](#NewCond)
  - [func (c *Cond) Broadcast()](#Cond.Broadcast)
  - [func (c *Cond) Signal()](#Cond.Signal)
  - [func (c *Cond) Wait()](#Cond.Wait)
- [type Locker](#Locker)
- [type Map](#Map)
  - [func (m *Map) Delete(key interface{})](#Map.Delete)
  - [func (m *Map) Load(key interface{}) (value interface{}, ok bool)](#Map.Load)
  - [func (m *Map) LoadOrStore(key, value interface{}) (actual interface{}, loaded bool)](#Map.LoadOrStore)
  - [func (m *Map) Range(f func(key, value interface{}) bool)](#Map.Range)
  - [func (m *Map) Store(key, value interface{})](#Map.Store)
- [type Mutex](#Mutex)
  - [func (m *Mutex) Lock()](#Mutex.Lock)
  - [func (m *Mutex) Unlock()](#Mutex.Unlock)
- [type Once](#Once)
  - [func (o *Once) Do(f func())](#Once.Do)
- [type Pool](#Pool)
  - [func (p *Pool) Get() interface{}](#Pool.Get)
  - [func (p *Pool) Put(x interface{})](#Pool.Put)
- [type RWMutex](#RWMutex)
  - [func (rw *RWMutex) Lock()](#RWMutex.Lock)
  - [func (rw *RWMutex) RLock()](#RWMutex.RLock)
  - [func (rw *RWMutex) RLocker() Locker](#RWMutex.RLocker)
  - [func (rw *RWMutex) RUnlock()](#RWMutex.RUnlock)
  - [func (rw *RWMutex) Unlock()](#RWMutex.Unlock)
- [type WaitGroup](#WaitGroup)
  - [func (wg *WaitGroup) Add(delta int)](#WaitGroup.Add)
  - [func (wg *WaitGroup) Done()](#WaitGroup.Done)
  - [func (wg *WaitGroup) Wait()](#WaitGroup.Wait)

### Examples

- [Once](#exampleOnce)
- [Pool](#examplePool)
- [WaitGroup](#exampleWaitGroup)

### Package files
 [cond.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/cond.go) [map.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/map.go) [mutex.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/mutex.go) [once.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/once.go) [pool.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/pool.go) [runtime.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/runtime.go) [rwmutex.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/rwmutex.go) [waitgroup.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/waitgroup.go)

<h2 id="Cond">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/cond.go#L11">Cond</a>
    <a href="#Cond">¶</a></h2>
<pre>type Cond struct {

<span id="Cond.L"></span>    <span class="comment">// L is held while observing or changing the condition</span>
    L <a href="#Locker">Locker</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Cond implements a condition variable, a rendezvous point for goroutines waiting
for or announcing the occurrence of an event.

Each Cond has an associated Locker L (often a *Mutex or *RWMutex), which must be
held when changing the condition and when calling the Wait method.

A Cond must not be copied after first use.

<h3 id="NewCond">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/cond.go#L22">NewCond</a>
    <a href="#NewCond">¶</a></h3>
<pre>func NewCond(l <a href="#Locker">Locker</a>) *<a href="#Cond">Cond</a></pre>

NewCond returns a new Cond with Locker l.

<h3 id="Cond.Broadcast">func (*Cond) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/cond.go#L63">Broadcast</a>
    <a href="#Cond.Broadcast">¶</a></h3>
<pre>func (c *<a href="#Cond">Cond</a>) Broadcast()</pre>

Broadcast wakes all goroutines waiting on c.

It is allowed but not required for the caller to hold c.L during the call.

<h3 id="Cond.Signal">func (*Cond) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/cond.go#L54">Signal</a>
    <a href="#Cond.Signal">¶</a></h3>
<pre>func (c *<a href="#Cond">Cond</a>) Signal()</pre>

Signal wakes one goroutine waiting on c, if there is any.

It is allowed but not required for the caller to hold c.L during the call.

<h3 id="Cond.Wait">func (*Cond) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/cond.go#L42">Wait</a>
    <a href="#Cond.Wait">¶</a></h3>
<pre>func (c *<a href="#Cond">Cond</a>) Wait()</pre>

Wait atomically unlocks c.L and suspends execution of the calling goroutine.
After later resuming execution, Wait locks c.L before returning. Unlike in other
systems, Wait cannot return unless awoken by Broadcast or Signal.

Because c.L is not locked when Wait first resumes, the caller typically cannot
assume that the condition is true when Wait returns. Instead, the caller should
Wait in a loop:

    c.L.Lock()
    for !condition() {
        c.Wait()
    }
    ... make use of condition ...
    c.L.Unlock()

<h2 id="Locker">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/mutex.go#L21">Locker</a>
    <a href="#Locker">¶</a></h2>
<pre>type Locker interface {
    Lock()
    Unlock()
}</pre>

A Locker represents an object that can be locked and unlocked.

<h2 id="Map">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/map.go#L17">Map</a>
    <a href="#Map">¶</a></h2>
<pre>type Map struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Map is like a Go map[interface{}]interface{} but is safe for concurrent use by
multiple goroutines without additional locking or coordination. Loads, stores,
and deletes run in amortized constant time.

The Map type is specialized. Most code should use a plain Go map instead, with
separate locking or coordination, for better type safety and to make it easier
to maintain other invariants along with the map content.

The Map type is optimized for two common use cases: (1) when the entry for a
given key is only ever written once but read many times, as in caches that only
grow, or (2) when multiple goroutines read, write, and overwrite entries for
disjoint sets of keys. In these two cases, use of a Map may significantly reduce
lock contention compared to a Go map paired with a separate Mutex or RWMutex.

The zero Map is empty and ready for use. A Map must not be copied after first
use.

<h3 id="Map.Delete">func (*Map) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/map.go#L261">Delete</a>
    <a href="#Map.Delete">¶</a></h3>
<pre>func (m *<a href="#Map">Map</a>) Delete(key interface{})</pre>

Delete deletes the value for a key.

<h3 id="Map.Load">func (*Map) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/map.go#L92">Load</a>
    <a href="#Map.Load">¶</a></h3>
<pre>func (m *<a href="#Map">Map</a>) Load(key interface{}) (value interface{}, ok <a href="/builtin/#bool">bool</a>)</pre>

Load returns the value stored in the map for a key, or nil if no value is
present. The ok result indicates whether value was found in the map.

<h3 id="Map.LoadOrStore">func (*Map) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/map.go#L193">LoadOrStore</a>
    <a href="#Map.LoadOrStore">¶</a></h3>
<pre>func (m *<a href="#Map">Map</a>) LoadOrStore(key, value interface{}) (actual interface{}, loaded <a href="/builtin/#bool">bool</a>)</pre>

LoadOrStore returns the existing value for the key if present. Otherwise, it
stores and returns the given value. The loaded result is true if the value was
loaded, false if stored.

<h3 id="Map.Range">func (*Map) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/map.go#L300">Range</a>
    <a href="#Map.Range">¶</a></h3>
<pre>func (m *<a href="#Map">Map</a>) Range(f func(key, value interface{}) <a href="/builtin/#bool">bool</a>)</pre>

Range calls f sequentially for each key and value present in the map. If f
returns false, range stops the iteration.

Range does not necessarily correspond to any consistent snapshot of the Map's
contents: no key will be visited more than once, but if the value for any key is
stored or deleted concurrently, Range may reflect any mapping for that key from
any point during the Range call.

Range may be O(N) with the number of elements in the map even if f returns false
after a constant number of calls.

<h3 id="Map.Store">func (*Map) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/map.go#L126">Store</a>
    <a href="#Map.Store">¶</a></h3>
<pre>func (m *<a href="#Map">Map</a>) Store(key, value interface{})</pre>

Store sets the value for a key.

<h2 id="Mutex">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/mutex.go#L15">Mutex</a>
    <a href="#Mutex">¶</a></h2>
<pre>type Mutex struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Mutex is a mutual exclusion lock. The zero value for a Mutex is an unlocked
mutex.

A Mutex must not be copied after first use.

<h3 id="Mutex.Lock">func (*Mutex) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/mutex.go#L62">Lock</a>
    <a href="#Mutex.Lock">¶</a></h3>
<pre>func (m *<a href="#Mutex">Mutex</a>) Lock()</pre>

Lock locks m. If the lock is already in use, the calling goroutine blocks until
the mutex is available.

<h3 id="Mutex.Unlock">func (*Mutex) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/mutex.go#L165">Unlock</a>
    <a href="#Mutex.Unlock">¶</a></h3>
<pre>func (m *<a href="#Mutex">Mutex</a>) Unlock()</pre>

Unlock unlocks m. It is a run-time error if m is not locked on entry to Unlock.

A locked Mutex is not associated with a particular goroutine. It is allowed for
one goroutine to lock a Mutex and then arrange for another goroutine to unlock
it.

<h2 id="Once">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/once.go#L2">Once</a>
    <a href="#Once">¶</a></h2>
<pre>type Once struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Once is an object that will perform exactly one action.

<a id="exampleOnce"></a>
Example:

    var once sync.Once
    onceBody := func() {
        fmt.Println("Only once")
    }
    done := make(chan bool)
    for i := 0; i < 10; i++ {
        go func() {
            once.Do(onceBody)
            done <- true
        }()
    }
    for i := 0; i < 10; i++ {
        <-done
    }
    // Output:
    // Only once

<h3 id="Once.Do">func (*Once) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/once.go#L25">Do</a>
    <a href="#Once.Do">¶</a></h3>
<pre>func (o *<a href="#Once">Once</a>) Do(f func())</pre>

Do calls the function f if and only if Do is being called for the first time for
this instance of Once. In other words, given

    var once Once

if once.Do(f) is called multiple times, only the first call will invoke f, even
if f has a different value in each invocation. A new instance of Once is
required for each function to execute.

Do is intended for initialization that must be run exactly once. Since f is
niladic, it may be necessary to use a function literal to capture the arguments
to a function to be invoked by Do:

    config.once.Do(func() { config.init(filename) })

Because no call to Do returns until the one call to f returns, if f causes Do to
be called, it will deadlock.

If f panics, Do considers it to have returned; future calls of Do return without
calling f.

<h2 id="Pool">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/pool.go#L34">Pool</a>
    <a href="#Pool">¶</a></h2>
<pre>type Pool struct {

<span id="Pool.New"></span>    <span class="comment">// New optionally specifies a function to generate</span>
    <span class="comment">// a value when Get would otherwise return nil.</span>
    <span class="comment">// It may not be changed concurrently with calls to Get.</span>
    New func() interface{}
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Pool is a set of temporary objects that may be individually saved and
retrieved.

Any item stored in the Pool may be removed automatically at any time without
notification. If the Pool holds the only reference when this happens, the item
might be deallocated.

A Pool is safe for use by multiple goroutines simultaneously.

Pool's purpose is to cache allocated but unused items for later reuse, relieving
pressure on the garbage collector. That is, it makes it easy to build efficient,
thread-safe free lists. However, it is not suitable for all free lists.

An appropriate use of a Pool is to manage a group of temporary items silently
shared among and potentially reused by concurrent independent clients of a
package. Pool provides a way to amortize allocation overhead across many
clients.

An example of good use of a Pool is in the fmt package, which maintains a
dynamically-sized store of temporary output buffers. The store scales under load
(when many goroutines are actively printing) and shrinks when quiescent.

On the other hand, a free list maintained as part of a short-lived object is not
a suitable use for a Pool, since the overhead does not amortize well in that
scenario. It is more efficient to have such objects implement their own free
list.

A Pool must not be copied after first use.

<a id="examplePool"></a>
Example:

    package sync_test

    import (
        "bytes"
        "io"
        "os"
        "sync"
        "time"
    )

    var bufPool = sync.Pool{
        New: func() interface{} {
            // The Pool's New function should generally only return pointer
            // types, since a pointer can be put into the return interface
            // value without an allocation:
            return new(bytes.Buffer)
        },
    }

    // timeNow is a fake version of time.Now for tests.
    func timeNow() time.Time {
        return time.Unix(1136214245, 0)
    }

    func Log(w io.Writer, key, val string) {
        b := bufPool.Get().(*bytes.Buffer)
        b.Reset()
        // Replace this with time.Now() in a real logger.
        b.WriteString(timeNow().UTC().Format(time.RFC3339))
        b.WriteByte(' ')
        b.WriteString(key)
        b.WriteByte('=')
        b.WriteString(val)
        w.Write(b.Bytes())
        bufPool.Put(b)
    }

    func ExamplePool() {
        Log(os.Stdout, "path", "/search?q=flowers")
        // Output: 2006-01-02T15:04:05Z path=/search?q=flowers
    }

<h3 id="Pool.Get">func (*Pool) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/pool.go#L114">Get</a>
    <a href="#Pool.Get">¶</a></h3>
<pre>func (p *<a href="#Pool">Pool</a>) Get() interface{}</pre>

Get selects an arbitrary item from the Pool, removes it from the Pool, and
returns it to the caller. Get may choose to ignore the pool and treat it as
empty. Callers should not assume any relation between values passed to Put and
the values returned by Get.

If Get would otherwise return nil and p.New is non-nil, Get returns the result
of calling p.New.

<h3 id="Pool.Put">func (*Pool) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/pool.go#L78">Put</a>
    <a href="#Pool.Put">¶</a></h3>
<pre>func (p *<a href="#Pool">Pool</a>) Put(x interface{})</pre>

Put adds x to the pool.

<h2 id="RWMutex">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/rwmutex.go#L18">RWMutex</a>
    <a href="#RWMutex">¶</a></h2>
<pre>type RWMutex struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A RWMutex is a reader/writer mutual exclusion lock. The lock can be held by an
arbitrary number of readers or a single writer. The zero value for a RWMutex is
an unlocked mutex.

A RWMutex must not be copied after first use.

If a goroutine holds a RWMutex for reading and another goroutine might call
Lock, no goroutine should expect to be able to acquire a read lock until the
initial read lock is released. In particular, this prohibits recursive read
locking. This is to ensure that the lock eventually becomes available; a blocked
Lock call excludes new readers from acquiring the lock.

<h3 id="RWMutex.Lock">func (*RWMutex) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/rwmutex.go#L77">Lock</a>
    <a href="#RWMutex.Lock">¶</a></h3>
<pre>func (rw *<a href="#RWMutex">RWMutex</a>) Lock()</pre>

Lock locks rw for writing. If the lock is already locked for reading or writing,
Lock blocks until the lock is available.

<h3 id="RWMutex.RLock">func (*RWMutex) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/rwmutex.go#L33">RLock</a>
    <a href="#RWMutex.RLock">¶</a></h3>
<pre>func (rw *<a href="#RWMutex">RWMutex</a>) RLock()</pre>

RLock locks rw for reading.

It should not be used for recursive read locking; a blocked Lock call excludes
new readers from acquiring the lock. See the documentation on the RWMutex type.

<h3 id="RWMutex.RLocker">func (*RWMutex) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/rwmutex.go#L130">RLocker</a>
    <a href="#RWMutex.RLocker">¶</a></h3>
<pre>func (rw *<a href="#RWMutex">RWMutex</a>) RLocker() <a href="#Locker">Locker</a></pre>

RLocker returns a Locker interface that implements the Lock and Unlock methods
by calling rw.RLock and rw.RUnlock.

<h3 id="RWMutex.RUnlock">func (*RWMutex) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/rwmutex.go#L52">RUnlock</a>
    <a href="#RWMutex.RUnlock">¶</a></h3>
<pre>func (rw *<a href="#RWMutex">RWMutex</a>) RUnlock()</pre>

RUnlock undoes a single RLock call; it does not affect other simultaneous
readers. It is a run-time error if rw is not locked for reading on entry to
RUnlock.

<h3 id="RWMutex.Unlock">func (*RWMutex) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/rwmutex.go#L103">Unlock</a>
    <a href="#RWMutex.Unlock">¶</a></h3>
<pre>func (rw *<a href="#RWMutex">RWMutex</a>) Unlock()</pre>

Unlock unlocks rw for writing. It is a run-time error if rw is not locked for
writing on entry to Unlock.

As with Mutexes, a locked RWMutex is not associated with a particular goroutine.
One goroutine may RLock (Lock) a RWMutex and then arrange for another goroutine
to RUnlock (Unlock) it.

<h2 id="WaitGroup">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/waitgroup.go#L10">WaitGroup</a>
    <a href="#WaitGroup">¶</a></h2>
<pre>type WaitGroup struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A WaitGroup waits for a collection of goroutines to finish. The main goroutine
calls Add to set the number of goroutines to wait for. Then each of the
goroutines runs and calls Done when finished. At the same time, Wait can be used
to block until all goroutines have finished.

A WaitGroup must not be copied after first use.

<a id="exampleWaitGroup"></a>
Example:

    var wg sync.WaitGroup
    var urls = []string{
        "http://www.golang.org/",
        "http://www.google.com/",
        "http://www.somestupidname.com/",
    }
    for _, url := range urls {
        // Increment the WaitGroup counter.
        wg.Add(1)
        // Launch a goroutine to fetch the URL.
        go func(url string) {
            // Decrement the counter when the goroutine completes.
            defer wg.Done()
            // Fetch the URL.
            http.Get(url)
        }(url)
    }
    // Wait for all HTTP fetches to complete.
    wg.Wait()

<h3 id="WaitGroup.Add">func (*WaitGroup) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/waitgroup.go#L42">Add</a>
    <a href="#WaitGroup.Add">¶</a></h3>
<pre>func (wg *<a href="#WaitGroup">WaitGroup</a>) Add(delta <a href="/builtin/#int">int</a>)</pre>

Add adds delta, which may be negative, to the WaitGroup counter. If the counter
becomes zero, all goroutines blocked on Wait are released. If the counter goes
negative, Add panics.

Note that calls with a positive delta that occur when the counter is zero must
happen before a Wait. Calls with a negative delta, or calls with a positive
delta that start when the counter is greater than zero, may happen at any time.
Typically this means the calls to Add should execute before the statement
creating the goroutine or other event to be waited for. If a WaitGroup is reused
to wait for several independent sets of events, new Add calls must happen after
all previous Wait calls have returned. See the WaitGroup example.

<h3 id="WaitGroup.Done">func (*WaitGroup) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/waitgroup.go#L87">Done</a>
    <a href="#WaitGroup.Done">¶</a></h3>
<pre>func (wg *<a href="#WaitGroup">WaitGroup</a>) Done()</pre>

Done decrements the WaitGroup counter by one.

<h3 id="WaitGroup.Wait">func (*WaitGroup) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sync/waitgroup.go#L92">Wait</a>
    <a href="#WaitGroup.Wait">¶</a></h3>
<pre>func (wg *<a href="#WaitGroup">WaitGroup</a>) Wait()</pre>

Wait blocks until the WaitGroup counter is zero.

## Subdirectories
- [..](..)
- [atomic](atomic/)
