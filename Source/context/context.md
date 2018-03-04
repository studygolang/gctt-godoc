version: 1.10
## package context

  `import "context"`

## Overview

Package context defines the Context type, which carries deadlines, cancelation
signals, and other request-scoped values across API boundaries and between
processes.

Incoming requests to a server should create a Context, and outgoing calls to
servers should accept a Context. The chain of function calls between them must
propagate the Context, optionally replacing it with a derived Context created
using WithCancel, WithDeadline, WithTimeout, or WithValue. When a Context is
canceled, all Contexts derived from it are also canceled.

The WithCancel, WithDeadline, and WithTimeout functions take a Context (the
parent) and return a derived Context (the child) and a CancelFunc. Calling the
CancelFunc cancels the child and its children, removes the parent's reference to
the child, and stops any associated timers. Failing to call the CancelFunc leaks
the child and its children until the parent is canceled or the timer fires. The
go vet tool checks that CancelFuncs are used on all control-flow paths.

Programs that use Contexts should follow these rules to keep interfaces
consistent across packages and enable static analysis tools to check context
propagation:

Do not store Contexts inside a struct type; instead, pass a Context explicitly
to each function that needs it. The Context should be the first parameter,
typically named ctx:

    func DoSomething(ctx context.Context, arg Arg) error {
    	// ... use ctx ...
    }

Do not pass a nil Context, even if a function permits it. Pass context.TODO if
you are unsure about which Context to use.

Use context Values only for request-scoped data that transits processes and
APIs, not for passing optional parameters to functions.

The same Context may be passed to functions running in different goroutines;
Contexts are safe for simultaneous use by multiple goroutines.

See https://blog.golang.org/context for example code for a server that uses
Contexts.

## Index

- [Variables](#pkg-variables)
- [type CancelFunc](#CancelFunc)
- [type Context](#Context)
  - [func Background() Context](#Background)
  - [func TODO() Context](#TODO)
  - [func WithCancel(parent Context) (ctx Context, cancel CancelFunc)](#WithCancel)
  - [func WithDeadline(parent Context, d time.Time) (Context, CancelFunc)](#WithDeadline)
  - [func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)](#WithTimeout)
  - [func WithValue(parent Context, key, val interface{}) Context](#WithValue)

### Examples

- [WithCancel](#exampleWithCancel)
- [WithDeadline](#exampleWithDeadline)
- [WithTimeout](#exampleWithTimeout)
- [WithValue](#exampleWithValue)

### Package files
 [context.go](//github.com/golang/go/blob/release-branch.go1.10/src/context/context.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="Canceled">Canceled</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;context canceled&#34;)</pre>

Canceled is the error returned by Context.Err when the context is canceled.

<pre>var <span id="DeadlineExceeded">DeadlineExceeded</span> <a href="/builtin/#error">error</a> = deadlineExceededError{}</pre>

DeadlineExceeded is the error returned by Context.Err when the context's
deadline passes.

<h2 id="CancelFunc">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/context/context.go#L212">CancelFunc</a>
    <a href="#CancelFunc">¶</a></h2>
<pre>type CancelFunc func()</pre>

A CancelFunc tells an operation to abandon its work. A CancelFunc does not wait
for the work to stop. After the first call, subsequent calls to a CancelFunc do
nothing.

<h2 id="Context">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/context/context.go#L52">Context</a>
    <a href="#Context">¶</a></h2>
<pre>type Context interface {
    <span class="comment">// Deadline returns the time when work done on behalf of this context</span>
    <span class="comment">// should be canceled. Deadline returns ok==false when no deadline is</span>
    <span class="comment">// set. Successive calls to Deadline return the same results.</span>
    Deadline() (deadline <a href="/time/">time</a>.<a href="/time/#Time">Time</a>, ok <a href="/builtin/#bool">bool</a>)

    <span class="comment">// Done returns a channel that&#39;s closed when work done on behalf of this</span>
    <span class="comment">// context should be canceled. Done may return nil if this context can</span>
    <span class="comment">// never be canceled. Successive calls to Done return the same value.</span>
    <span class="comment">//</span>
    <span class="comment">// WithCancel arranges for Done to be closed when cancel is called;</span>
    <span class="comment">// WithDeadline arranges for Done to be closed when the deadline</span>
    <span class="comment">// expires; WithTimeout arranges for Done to be closed when the timeout</span>
    <span class="comment">// elapses.</span>
    <span class="comment">//</span>
    <span class="comment">// Done is provided for use in select statements:</span>
    <span class="comment">//</span>
    <span class="comment">//  // Stream generates values with DoSomething and sends them to out</span>
    <span class="comment">//  // until DoSomething returns an error or ctx.Done is closed.</span>
    <span class="comment">//  func Stream(ctx context.Context, out chan&lt;- Value) error {</span>
    <span class="comment">//  	for {</span>
    <span class="comment">//  		v, err := DoSomething(ctx)</span>
    <span class="comment">//  		if err != nil {</span>
    <span class="comment">//  			return err</span>
    <span class="comment">//  		}</span>
    <span class="comment">//  		select {</span>
    <span class="comment">//  		case &lt;-ctx.Done():</span>
    <span class="comment">//  			return ctx.Err()</span>
    <span class="comment">//  		case out &lt;- v:</span>
    <span class="comment">//  		}</span>
    <span class="comment">//  	}</span>
    <span class="comment">//  }</span>
    <span class="comment">//</span>
    <span class="comment">// See https://blog.golang.org/pipelines for more examples of how to use</span>
    <span class="comment">// a Done channel for cancelation.</span>
    Done() &lt;-chan struct{}

    <span class="comment">// If Done is not yet closed, Err returns nil.</span>
    <span class="comment">// If Done is closed, Err returns a non-nil error explaining why:</span>
    <span class="comment">// Canceled if the context was canceled</span>
    <span class="comment">// or DeadlineExceeded if the context&#39;s deadline passed.</span>
    <span class="comment">// After Err returns a non-nil error, successive calls to Err return the same error.</span>
    Err() <a href="/builtin/#error">error</a>

    <span class="comment">// Value returns the value associated with this context for key, or nil</span>
    <span class="comment">// if no value is associated with key. Successive calls to Value with</span>
    <span class="comment">// the same key returns the same result.</span>
    <span class="comment">//</span>
    <span class="comment">// Use context values only for request-scoped data that transits</span>
    <span class="comment">// processes and API boundaries, not for passing optional parameters to</span>
    <span class="comment">// functions.</span>
    <span class="comment">//</span>
    <span class="comment">// A key identifies a specific value in a Context. Functions that wish</span>
    <span class="comment">// to store values in Context typically allocate a key in a global</span>
    <span class="comment">// variable then use that key as the argument to context.WithValue and</span>
    <span class="comment">// Context.Value. A key can be any type that supports equality;</span>
    <span class="comment">// packages should define keys as an unexported type to avoid</span>
    <span class="comment">// collisions.</span>
    <span class="comment">//</span>
    <span class="comment">// Packages that define a Context key should provide type-safe accessors</span>
    <span class="comment">// for the values stored using that key:</span>
    <span class="comment">//</span>
    <span class="comment">// 	// Package user defines a User type that&#39;s stored in Contexts.</span>
    <span class="comment">// 	package user</span>
    <span class="comment">//</span>
    <span class="comment">// 	import &#34;context&#34;</span>
    <span class="comment">//</span>
    <span class="comment">// 	// User is the type of value stored in the Contexts.</span>
    <span class="comment">// 	type User struct {...}</span>
    <span class="comment">//</span>
    <span class="comment">// 	// key is an unexported type for keys defined in this package.</span>
    <span class="comment">// 	// This prevents collisions with keys defined in other packages.</span>
    <span class="comment">// 	type key int</span>
    <span class="comment">//</span>
    <span class="comment">// 	// userKey is the key for user.User values in Contexts. It is</span>
    <span class="comment">// 	// unexported; clients use user.NewContext and user.FromContext</span>
    <span class="comment">// 	// instead of using this key directly.</span>
    <span class="comment">// 	var userKey key</span>
    <span class="comment">//</span>
    <span class="comment">// 	// NewContext returns a new Context that carries value u.</span>
    <span class="comment">// 	func NewContext(ctx context.Context, u *User) context.Context {</span>
    <span class="comment">// 		return context.WithValue(ctx, userKey, u)</span>
    <span class="comment">// 	}</span>
    <span class="comment">//</span>
    <span class="comment">// 	// FromContext returns the User value stored in ctx, if any.</span>
    <span class="comment">// 	func FromContext(ctx context.Context) (*User, bool) {</span>
    <span class="comment">// 		u, ok := ctx.Value(userKey).(*User)</span>
    <span class="comment">// 		return u, ok</span>
    <span class="comment">// 	}</span>
    Value(key interface{}) interface{}
}</pre>

A Context carries a deadline, a cancelation signal, and other values across API
boundaries.

Context's methods may be called by multiple goroutines simultaneously.

<h3 id="Background">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/context/context.go#L196">Background</a>
    <a href="#Background">¶</a></h3>
<pre>func Background() <a href="#Context">Context</a></pre>

Background returns a non-nil, empty Context. It is never canceled, has no
values, and has no deadline. It is typically used by the main function,
initialization, and tests, and as the top-level Context for incoming requests.

<h3 id="TODO">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/context/context.go#L205">TODO</a>
    <a href="#TODO">¶</a></h3>
<pre>func TODO() <a href="#Context">Context</a></pre>

TODO returns a non-nil, empty Context. Code should use context.TODO when it's
unclear which Context to use or it is not yet available (because the surrounding
function has not yet been extended to accept a Context parameter). TODO is
recognized by static analysis tools that determine whether Contexts are
propagated correctly in a program.

<h3 id="WithCancel">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/context/context.go#L220">WithCancel</a>
    <a href="#WithCancel">¶</a></h3>
<pre>func WithCancel(parent <a href="#Context">Context</a>) (ctx <a href="#Context">Context</a>, cancel <a href="#CancelFunc">CancelFunc</a>)</pre>

WithCancel returns a copy of parent with a new Done channel. The returned
context's Done channel is closed when the returned cancel function is called or
when the parent context's Done channel is closed, whichever happens first.

Canceling this context releases resources associated with it, so code should
call cancel as soon as the operations running in this Context complete.

<a id="exampleWithCancel"></a>
Example:

    // gen generates integers in a separate goroutine and
    // sends them to the returned channel.
    // The callers of gen need to cancel the context once
    // they are done consuming generated integers not to leak
    // the internal goroutine started by gen.
    gen := func(ctx context.Context) <-chan int {
        dst := make(chan int)
        n := 1
        go func() {
            for {
                select {
                case <-ctx.Done():
                    return // returning not to leak the goroutine
                case dst <- n:
                    n++
                }
            }
        }()
        return dst
    }

    ctx, cancel := context.WithCancel(context.Background())
    defer cancel() // cancel when we are finished consuming integers

    for n := range gen(ctx) {
        fmt.Println(n)
        if n == 5 {
            break
        }
    }
    // Output:
    // 1
    // 2
    // 3
    // 4
    // 5

<h3 id="WithDeadline">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/context/context.go#L373">WithDeadline</a>
    <a href="#WithDeadline">¶</a></h3>
<pre>func WithDeadline(parent <a href="#Context">Context</a>, d <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) (<a href="#Context">Context</a>, <a href="#CancelFunc">CancelFunc</a>)</pre>

WithDeadline returns a copy of the parent context with the deadline adjusted to
be no later than d. If the parent's deadline is already earlier than d,
WithDeadline(parent, d) is semantically equivalent to parent. The returned
context's Done channel is closed when the deadline expires, when the returned
cancel function is called, or when the parent context's Done channel is closed,
whichever happens first.

Canceling this context releases resources associated with it, so code should
call cancel as soon as the operations running in this Context complete.

<a id="exampleWithDeadline"></a>
Example:

    d := time.Now().Add(50 * time.Millisecond)
    ctx, cancel := context.WithDeadline(context.Background(), d)

    // Even though ctx will be expired, it is good practice to call its
    // cancelation function in any case. Failure to do so may keep the
    // context and its parent alive longer than necessary.
    defer cancel()

    select {
    case <-time.After(1 * time.Second):
        fmt.Println("overslept")
    case <-ctx.Done():
        fmt.Println(ctx.Err())
    }

    // Output:
    // context deadline exceeded

<h3 id="WithTimeout">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/context/context.go#L440">WithTimeout</a>
    <a href="#WithTimeout">¶</a></h3>
<pre>func WithTimeout(parent <a href="#Context">Context</a>, timeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>) (<a href="#Context">Context</a>, <a href="#CancelFunc">CancelFunc</a>)</pre>

WithTimeout returns WithDeadline(parent, time.Now().Add(timeout)).

Canceling this context releases resources associated with it, so code should
call cancel as soon as the operations running in this Context complete:

    func slowOperationWithTimeout(ctx context.Context) (Result, error) {
    	ctx, cancel := context.WithTimeout(ctx, 100*time.Millisecond)
    	defer cancel()  // releases resources if slowOperation completes before timeout elapses
    	return slowOperation(ctx)
    }

<a id="exampleWithTimeout"></a>
Example:

    // Pass a context with a timeout to tell a blocking function that it
    // should abandon its work after the timeout elapses.
    ctx, cancel := context.WithTimeout(context.Background(), 50*time.Millisecond)
    defer cancel()

    select {
    case <-time.After(1 * time.Second):
        fmt.Println("overslept")
    case <-ctx.Done():
        fmt.Println(ctx.Err()) // prints "context deadline exceeded"
    }

    // Output:
    // context deadline exceeded

<h3 id="WithValue">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/context/context.go#L457">WithValue</a>
    <a href="#WithValue">¶</a></h3>
<pre>func WithValue(parent <a href="#Context">Context</a>, key, val interface{}) <a href="#Context">Context</a></pre>

WithValue returns a copy of parent in which the value associated with key is
val.

Use context Values only for request-scoped data that transits processes and
APIs, not for passing optional parameters to functions.

The provided key must be comparable and should not be of type string or any
other built-in type to avoid collisions between packages using context. Users of
WithValue should define their own types for keys. To avoid allocating when
assigning to an interface{}, context keys often have concrete type struct{}.
Alternatively, exported context key variables' static type should be a pointer
or interface.

<a id="exampleWithValue"></a>
Example:

    type favContextKey string

    f := func(ctx context.Context, k favContextKey) {
        if v := ctx.Value(k); v != nil {
            fmt.Println("found value:", v)
            return
        }
        fmt.Println("key not found:", k)
    }

    k := favContextKey("language")
    ctx := context.WithValue(context.Background(), k, "Go")

    f(ctx, k)
    f(ctx, favContextKey("color"))

    // Output:
    // found value: Go
    // key not found: color


