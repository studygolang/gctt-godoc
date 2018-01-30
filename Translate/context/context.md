version: 1.9.2
## package context

  `import "context"`

## 概述

context 包定义了 Context 类型。它在 API 边界和进程之间传递截止时间，取消信号，和其他在请求声明周期中的值。

请求到达服务器后需创建 Context 实例，并且也应该接受 Context 。在他们之间的函数调用链必须传递这个 Context，也可以派生新的 Context 替换它（使用 WithCancel，WithDeadline，WithTimeout 或 WithValue 派生 Context）。当 Context 注销时，所有由它派生出的 Context 也会被注销。

WithCancel，WithDeadline，WithTimeout 函数接收一个 Context（作为父级 Context）并返回一个派生 Context（最为子级 Context）和 CancelFunc。调用 CancelFunc 会注销子级 Context 和由它派生的 Context ，删除父级 Context 对它的引用，并关闭所有关联的定时器。如果没有调用 CancelFunc 会导致它和由它派生的 Context 在注销父级 Context 或父级 Context 到达截止日期前一直存在。go vet 工具可以检查是否所有的流程控制语句都使用了 CancelFunc。

为了能让使用 Context 的程序保持接口的一致性并且能够使用静态工具进行检查，我们需要遵循以下规则：

在函数中按需传递 Context 而不要将 Context 储存在结构体中。Context 应该作为函数的第一个参数，一般叫 ctx：

```go
func DoSomething(ctx context.Context, arg Arg) error {
	// ... use ctx ...
}
```

即使函数允许也不要使用 nil  Context。如果你不知道使用哪个 Context 可以使用 context.TODO 。

仅在 API 或者进程的请求生命周期的数据上使用 context 的值，而不作为可选参数传递给函数 。

相同的 Context 可以传递进不同的 goroutines。Context 可以被多个 goroutine 同时使用。

更多详情与示例见 https://blog.golang.org/context 。

## 索引

- [Variables](#pkg-variables)
- [type CancelFunc](#CancelFunc)
- [type Context](#Context)
  - [func Background() Context](#Background)
  - [func TODO() Context](#TODO)
  - [func WithCancel(parent Context) (ctx Context, cancel CancelFunc)](#WithCancel)
  - [func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)](#WithDeadline)
  - [func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)](#WithTimeout)
  - [func WithValue(parent Context, key, val interface{}) Context](#WithValue)

### 例子

- [WithCancel](#exampleWithCancel)
- [WithDeadline](#exampleWithDeadline)
- [WithTimeout](#exampleWithTimeout)
- [WithValue](#exampleWithValue)

### 文件

 [context.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="Canceled">Canceled</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;context canceled&#34;)</pre>

Context.Err 在 context 被注销时返回 Canceled。

<pre>var <span id="DeadlineExceeded">DeadlineExceeded</span> <a href="/builtin/#error">error</a> = deadlineExceededError{}</pre>

Context.Err 当截止时间到达时返回 DeadlineExceeded。

<h2 id="CancelFunc">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L212">CancelFunc</a>
    <a href="#CancelFunc">¶</a></h2>
<pre>type CancelFunc func()</pre>

CancelFunc 会取消本次请求。 调用 CancelFunc 立即终止本次请求，随后再次调用 CancelFunc 不做任何事。

<h2 id="Context">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L52">Context</a>
    <a href="#Context">¶</a></h2>
<pre>type Context interface {
    <span class="comment">// 当请求处理完成时 Deadline 返回一个时间，这时我们需要手动取消 context。</span>
    <span class="comment">// 如果没有设置 deadline 那么 ok 为 false。</span>
    <span class="comment">// Deadline 的多次成功调用返回相同结果。</span>
    Deadline() (deadline <a href="/time/">time</a>.<a href="/time/#Time">Time</a>, ok <a href="/builtin/#bool">bool</a>)
    
    <span class="comment">// Done 返回一个 channel，当 channel 关闭表示请求完成，需要手动取消 context。</span>
    <span class="comment">// 如果 context 不能被取消，那么 Done 将会返回 nil。</span>
    <span class="comment">// Done 的多次调用返回相同值。</span>
    <span class="comment">//</span>
    <span class="comment">// WithCancel 让 Done 返回的 channel 在被取消的时候关闭。</span>
    <span class="comment">// WithDeadline 让 Done 返回的 channel 在到达截止时间的时候关闭。</span>
    <span class="comment">// WithTimeout 让 Done 返回的 channel 在超时的时候关闭。</span>
    <span class="comment">// </span>
    <span class="comment">//</span>
    <span class="comment">// Done 和 select 搭配使用:</span>
    <span class="comment">//</span>
    <span class="comment">//  // Stream 调用 DoSomething 初始化一个值并输出。</span>
    <span class="comment">//  // 当 Done 返回的 channel 被关闭或者 DoSomething 函数返回错误 Stream 函数才会返回。</span>
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
    <span class="comment">// 在 https://blog.golang.org/pipelines 可以获取更多示例</span>
    <span class="comment">// 一个用来获取取消状态的 channel</span>
    Done() &lt;-chan struct{}
    
    <span class="comment">// 如果 Done 返回的 channel 没有被关闭，那么 Err 返回 nil</span>
    <span class="comment">// 如果 Done 返回的 channel 被关闭，那么 Err 的返回值告诉我们为什么关闭:</span>
    <span class="comment">// 当 context 被取消时返回 Canceled。 </span>
    <span class="comment">// 当 context 达到截止时间时返回 DeadlineExceeded。</span>
    <span class="comment">// 在 Err 开始返回非 nil 错误后，每次调用都返回相同的值。</span>
    Err() <a href="/builtin/#error">error</a>
    
    <span class="comment">// Value 返回 context 中 key 关联的值，如果没有返回 nil。</span>
    <span class="comment">// 使用相同的 key 一定能取到相同的 value。</span>
    <span class="comment">//</span>
    <span class="comment">// 保证只在请求生命周期内的数据上使用 value，而不是把它作为函数的可选参数。</span>
    <span class="comment">//</span>
    <span class="comment">// 一个 key 在 Context 中对应一个 value。</span>
    <span class="comment">// 如果想在 context 中保存键值对。</span>
    <span class="comment">// 我们可以把这个键设置成一个全局变量，并使用 Context.WithValue 和 Context.Value 来设置和访问他的值。</span>
    <span class="comment">// key 可以是任何可以比较相等的类型。</span>
    <span class="comment">// 我们定义的 Context 的 key 的类型必须是非导出的，这样可以避免不同包间的命名冲突。</span>
    <span class="comment">//</span>
    <span class="comment">// 定义 Context 的 key 的包需要保证对应值的类型安全。</span>
    <span class="comment">//</span>
    <span class="comment">// 	// user 包定义了一个 User 类型并将它保存在 Contexts 中。</span>
    <span class="comment">// 	package user</span>
    <span class="comment">//</span>
    <span class="comment">// 	import &#34;context&#34;</span>
    <span class="comment">//</span>
    <span class="comment">// 	// User 是保存在 Contexts 中的值的类型。</span>
    <span class="comment">// 	type User struct {...}</span>
    <span class="comment">//</span>
    <span class="comment">// 	// key 是一个在包中没有导出的类型。</span>
    <span class="comment">// 	// 这样可以防止与其他包产生命名冲突。</span>
    <span class="comment">// 	type key int</span>
    <span class="comment">//</span>
    <span class="comment">// 	// userKey 是 Contexts 中保存 User 类型值的 key 类型。它是没有导出的。客户端使用 user.NewContext 或者 user.FromContext 来避免直接使用这个 key。</span>
    <span class="comment">// 	var userKey key = 0</span>
    <span class="comment">//</span>
    <span class="comment">// 	// NewContext 返回一个包含 u 的 Context</span>
    <span class="comment">// 	func NewContext(ctx context.Context, u *User) context.Context {</span>
    <span class="comment">// 		return context.WithValue(ctx, userKey, u)</span>
    <span class="comment">// 	}</span>
    <span class="comment">//</span>
    <span class="comment">// 	// FromContext 在 ctx 中有 User 值的时候将其返回。</span>
    <span class="comment">// 	func FromContext(ctx context.Context) (*User, bool) {</span>
    <span class="comment">// 		u, ok := ctx.Value(userKey).(*User)</span>
    <span class="comment">// 		return u, ok</span>
    <span class="comment">// 	}</span>
    Value(key interface{}) interface{}
}</pre>

Context 穿过 API 边界传递截止时间，取消信号和其他值。

Context 的方法都能在多个 goroutines 中并发使用。

<h3 id="Background">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L196">Background</a>
    <a href="#Background">¶</a></h3>
<pre>func Background() <a href="#Context">Context</a></pre>

Background 返回一个非 nil 的空 Context，它不能被取消，没有值，没有截止时间。它的典型应该用场景包括：main 函数，初始化，测试，或者作为所有请求 context 的顶级 Context。

<h3 id="TODO">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L205">TODO</a>
    <a href="#TODO">¶</a></h3>
<pre>func TODO() <a href="#Context">Context</a></pre>

TODO 返回一个非 nil 的空 Context。当不知道该使用哪个 Context 或者没有可用的 Context 的时候（因为函数不接受 Context 参数）的时候可以使用它， TODO 可以被静态的分析工具（检测 Context 在程序中被正确传递）识别。

<h3 id="WithCancel">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L220">WithCancel</a>
    <a href="#WithCancel">¶</a></h3>
<pre>func WithCancel(parent <a href="#Context">Context</a>) (ctx <a href="#Context">Context</a>, cancel <a href="#CancelFunc">CancelFunc</a>)</pre>

WithCancel 返回拥有 Done channel 的 parent 副本。不管调用 CancelFunc 和关闭 parent 的 Done channel 哪个先发生，Done channel 都会关闭。

注销 context 会释放和他关联的资源，所以我们应该在 Context 完成后立即注销它。

<a id="exampleWithCancel"></a>
例:

```go
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
```

<h3 id="WithDeadline">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L373">WithDeadline</a>
    <a href="#WithDeadline">¶</a></h3>
<pre>func WithDeadline(parent <a href="#Context">Context</a>, deadline <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) (<a href="#Context">Context</a>, <a href="#CancelFunc">CancelFunc</a>)</pre>

WithDeadline 返回拥有截止时间的 parent 副本。如果 parent 的截止时间早于 d 将使用 parent 的截止时间。不管到达截止时间，调用注销函数，关闭 parent 的 Done channel 哪个先发生，Done channel 都会被关闭。 

注销 context 会释放和他关联的资源，所以我们应该在 Context 完成后立即注销它。

<a id="exampleWithDeadline"></a>
例:

```go
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
```

<h3 id="WithTimeout">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L440">WithTimeout</a>
    <a href="#WithTimeout">¶</a></h3>
<pre>func WithTimeout(parent <a href="#Context">Context</a>, timeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>) (<a href="#Context">Context</a>, <a href="#CancelFunc">CancelFunc</a>)</pre>

withTimeout 也可以用 WithDeadline(parent, time.Now().Add(timeout)) 代替。

context 的取消操作会释放相关的资源，所以在 Context 完成以后就要调用 cancel：

```go
func slowOperationWithTimeout(ctx context.Context) (Result, error) {
	ctx, cancel := context.WithTimeout(ctx, 100*time.Millisecond)
	defer cancel()  // releases resources if slowOperation completes before timeout elapses
	return slowOperation(ctx)
}
```

<a id="exampleWithTimeout"></a>
例:

```go
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
```

<h3 id="WithValue">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L457">WithValue</a>
    <a href="#WithValue">¶</a></h3>
<pre>func WithValue(parent <a href="#Context">Context</a>, key, val interface{}) <a href="#Context">Context</a></pre>

WithValue 以 key 为键将 value 保存在 parent 的副本中并返回。

仅在请求声明周期中使用 Values，而不是将其作为函数的可选参数。

为了避免不同的包使用时出现相同的键，key 必须是可比较的并且不能是内置的 string 等类型。 WithValue 的用户应该定义自己的 key 类型。为了避免分配成 interface{]，context 的 key 一般都为具体类型。或者导出的 context 的 key 变量的静态类型应该是一个指针或者接口。

<a id="exampleWithValue"></a>
例:

    type favContextKey string

```go
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
```


