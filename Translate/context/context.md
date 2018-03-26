version: 1.10
## package context

  `import "context"`

## 概述

context 包定义了 Context 类型，它在 API 边界和进程之间传递截止时间，取消信号，和其他请求生命周期中的值。

请求到达服务器后需创建 Context 实例，服务器的响应也应该接受 Context。在它们之间的函数调用链必须传递这个 Context 实例，也可以调用 WithCancel, WithDeadline, WithTimeout, 或 WithValue 来衍生出新 Context 实例。当取消一个 Context 实例时，所有由它衍生出的 Context 实例都会取消。

函数 WithCancel，WithDeadline，WithTimeout 接受一个 Context 实例（父级）并返回一个衍生 Context（子级） 和一个 CancelFunc。调用 CancelFunc 会取消它（子级 Context）和由它衍生的 Context 实例，删除父级与它的联系并停止所有关联的定时器。不调用 CancelFunc 会导致它和它的衍生体的生命周期和父级一样长。go vet 工具可以检查是否所有的流程控制语句都使用了 CancelFunc。

为了能让使用 Context 的程序保持包之间的接口一致性并且能够使用静态工具进行检查，我们需要遵循以下规则：

在函数中按需传递 Context 而不要将 Context 储存在结构体中。Context 应该作为函数的第一个参数，一般叫 ctx：

    func DoSomething(ctx context.Context, arg Arg) error {
    	// ... use ctx ...
    }

即使函数允许也不要传递值为 nil 的 Context。如果你不知道使用哪个 Context 可以使用 context.TODO 。

只在 API 或者进程的生命周期中的数据使用 context 的值，不将其作为函数的可选参数 。

相同的 Context 可以传递进不同的 goroutines。Context 可以安全的被多个 goroutine 同时使用。

更多详情与示例见 https://blog.golang.org/context 。

## 索引

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
 [context.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="Canceled">Canceled</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;context canceled&#34;)</pre>

Context.Err 在 context 取消后返回 Canceled。

<pre>var <span id="DeadlineExceeded">DeadlineExceeded</span> <a href="/builtin/#error">error</a> = deadlineExceededError{}</pre>

Context.Err 在 context 超过截止时间后返回 DeadlineExceeded。

<h2 id="CancelFunc">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L212">CancelFunc</a>
    <a href="#CancelFunc">¶</a></h2>
<pre>type CancelFunc func()</pre>

CancelFunc 会通知一个作业终止它的工作。CancelFunc 不会等待工作完成而立即终止它，在首次调用 CancelFunc 后，再调用 CancelFunc 不做任何事。

<h2 id="Context">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L52">Context</a>
    <a href="#Context">¶</a></h2>
<pre>type Context interface {
    <span class="comment">// 当请求处理完成时 Deadline 返回一个时间，这时我们需要手动取消 context。</span>
    <span class="comment">// 如果没有设置 deadline 那么 ok 为 false。</span>
    <span class="comment">// Deadline 的多次成功调用返回相同结果。</span>
    Deadline() (deadline <a href="/time/">time</a>.<a href="/time/#Time">Time</a>, ok <a href="/builtin/#bool">bool</a>)
    
    <span class="comment">// Done 返回一个 channel，当 channel 关闭表示请求完成，需要手取消 context。</span>
    <span class="comment">// 如果 context 不能取消，那么 Done 将会返回 nil。</span>
    <span class="comment">// Done 的多次调用返回相同值。</span>
    <span class="comment">//</span>
    <span class="comment">// WithCancel 使 Done 返回的 channel 在 context 取消的时候关闭。</span>
    <span class="comment">// WithDeadline 使 Done 返回的 channel 在 context 到达截止时间的时候关闭。</span>
    <span class="comment">// WithTimeout 使 Done 返回的 channel 在超时的时候关闭。</span>
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
    <span class="comment">// 在 https://blog.golang.org/pipelines 有更多用法示例。</span>
    <span class="comment">// Done channel 用来获取取消状态。</span>
    Done() &lt;-chan struct{}
    
    <span class="comment">// 如果 Done 没有关闭，Err 返回 nil。</span>
    <span class="comment">// 如果 Done 已经关闭，Err 回返回非 nil 值来告知 Done 因为什么关闭:</span>
    <span class="comment">// 如果 context 取消返回 Canceled。</span>
    <span class="comment">// 如果 context 到达截止时间返回 DeadlineExceeded。</span>
    <span class="comment">// 在 Err 返回非 nil 值后，每次调用 Err 都返回相同结果。</span>
    Err() <a href="/builtin/#error">error</a>
    
    <span class="comment">// Value 返回 key 对应的值如果没有对应的值返回 nil。对相同键的多次成功调用返回相同的值。</span>
    <span class="comment">//</span>
    <span class="comment">// 只在 API 或者进程的生命周期中的数据使用 context 的值，不将其作为函数的可选参数 。</span>
    <span class="comment">//</span>
    <span class="comment">// 在 Context 中使用 key 的区分具体值。</span>
    <span class="comment">// 希望在 context 保存值的函数一般都会在全局分配一个键，然后使用这个键作为 context.WithValue 和 Context.Value 的参数。</span>
    <span class="comment">// key 可以是任何支持相等比较的类型，使用 key 的包应该定义非导出类型避免与其他 key 冲突。</span>
    <span class="comment">//</span>
    <span class="comment">// 定义 Context 的 key 的包需要为相应的值提供类型安全的访问器：</span>
    <span class="comment">//</span>
    <span class="comment">// 	// user 包定义了一个保存在 Context 中的 User 类型。</span>
    <span class="comment">// 	package user</span>
    <span class="comment">//</span>
    <span class="comment">// 	import &#34;context&#34;</span>
    <span class="comment">//</span>
    <span class="comment">// 	// User 是保存在 Context 中的类型。</span>
    <span class="comment">// 	type User struct {...}</span>
    <span class="comment">//</span>
    <span class="comment">// 	// key 是包中的一个非导出类型。</span>
    <span class="comment">// 	// 它可以避免与其他包中定义的 key 类型产生冲突。</span>
    <span class="comment">// 	type key int</span>
    <span class="comment">//</span>
    <span class="comment">// 	// userKey 是 Context 中保存的 user.User 所对应的 key。</span>
    <span class="comment">// 	// 它是非导出的类型; 用户可以使用 user.NewContext 和 user.FromContext 从而避免直接使用 key。</span>
    <span class="comment">// 	var userKey key</span>
    <span class="comment">//</span>
    <span class="comment">// 	// NewContext 返回一个保存 u 的 Context。</span>
    <span class="comment">// 	func NewContext(ctx context.Context, u *User) context.Context {</span>
    <span class="comment">// 		return context.WithValue(ctx, userKey, u)</span>
    <span class="comment">// 	}</span>
    <span class="comment">//</span>
    <span class="comment">// 	// 如果存在， FromContext 返回 ctx 中保存的 User 类型值。</span>
    <span class="comment">// 	func FromContext(ctx context.Context) (*User, bool) {</span>
    <span class="comment">// 		u, ok := ctx.Value(userKey).(*User)</span>
    <span class="comment">// 		return u, ok</span>
    <span class="comment">// 	}</span>
    Value(key interface{}) interface{}
}</pre>

Context 穿过 API 边界传递截止时间，取消信号和其他值。

Context 的方法都能在多个 goroutines 中同时使用。

<h3 id="Background">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L196">Background</a>
    <a href="#Background">¶</a></h3>
<pre>func Background() <a href="#Context">Context</a></pre>

Background 返回一个非 nil 的空 Context，它不能取消，不包含值，没有截止时间。它的典型应该用场景包括：main 函数，初始化，测试，或者作为所有请求的顶级 Context。

<h3 id="TODO">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L205">TODO</a>
    <a href="#TODO">¶</a></h3>
<pre>func TODO() <a href="#Context">Context</a></pre>

TODO 返回一个非 nil 的空 Context。当不知道该使用哪个 Context 或者没有可用的 Context 的时候（因为函数不接受 Context 参数）的时候可以使用它， TODO 可以被静态的分析工具（检测 Context 在程序中正确传递）识别。

<h3 id="WithCancel">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L220">WithCancel</a>
    <a href="#WithCancel">¶</a></h3>
<pre>func WithCancel(parent <a href="#Context">Context</a>) (ctx <a href="#Context">Context</a>, cancel <a href="#CancelFunc">CancelFunc</a>)</pre>

WithCancel 返回拥有 Done channel 的 parent 副本。不管调用 CancelFunc 和关闭 parent 的 Done channel 哪个先发生，Done channel 都会关闭。

取消 context 会释放和他关联的资源，所以我们应该在 Context 完成后立即取消它。

<a id="exampleWithCancel"></a>
例:

    // gen 在单独的 goroutine 中生成数字并将数字发送进 channel。
    // gen 的调用者一旦取消 context 就不会再消费生成的数字也不会导致 gen 中的 goroutine 泄漏。
    gen := func(ctx context.Context) <-chan int {
        dst := make(chan int)
        n := 1
        go func() {
            for {
                select {
                case <-ctx.Done():
                    return // 在 ctx 取消时返回，这样不会导致 goroutine 泄漏。
                case dst <- n:
                    n++
                }
            }
        }()
        return dst
    }

    ctx, cancel := context.WithCancel(context.Background())
    defer cancel() // 在我们消费完数字取消 ctx。

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

<h3 id="WithDeadline">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L373">WithDeadline</a>
    <a href="#WithDeadline">¶</a></h3>
<pre>func WithDeadline(parent <a href="#Context">Context</a>, d <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) (<a href="#Context">Context</a>, <a href="#CancelFunc">CancelFunc</a>)</pre>

WithDeadline 返回拥有截止时间的 parent 副本。如果 parent 的截止时间早于 d 将使用 parent 的截止时间。不管到达截止时间，调用取消函数，关闭 parent 的 Done channel 哪个先发生，Done channel 都会关闭。

取消 context 会释放和他关联的资源，所以我们应该在 Context 完成后立即取消它。

<a id="exampleWithDeadline"></a>
例:

    d := time.Now().Add(50 * time.Millisecond)
    ctx, cancel := context.WithDeadline(context.Background(), d)

    // 即使 ctx 将要过期，在最后调用取消函数也是很好的做法。不这么做会导致 context 和它的父级在不必要的时候存在。
    defer cancel()

    select {
    case <-time.After(1 * time.Second):
        fmt.Println("overslept")
    case <-ctx.Done():
        fmt.Println(ctx.Err())
    }

    // Output:
    // context deadline exceeded

<h3 id="WithTimeout">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L440">WithTimeout</a>
    <a href="#WithTimeout">¶</a></h3>
<pre>func WithTimeout(parent <a href="#Context">Context</a>, timeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>) (<a href="#Context">Context</a>, <a href="#CancelFunc">CancelFunc</a>)</pre>

withTimeout 也可以用 WithDeadline(parent, time.Now().Add(timeout)) 代替。

context 的取消操作会释放相关的资源，所以在 Context 完成以后就要调用 cancel：

    func slowOperationWithTimeout(ctx context.Context) (Result, error) {
    	ctx, cancel := context.WithTimeout(ctx, 100*time.Millisecond)
    	defer cancel()  // 如果在超时之前完成就释放资源。
    	return slowOperation(ctx)
    }

<a id="exampleWithTimeout"></a>
例:

    // 通过一个带超时的 context 可以通知阻塞函数应该在到达超时时间后放弃当前工作。
    ctx, cancel := context.WithTimeout(context.Background(), 50*time.Millisecond)
    defer cancel()

    select {
    case <-time.After(1 * time.Second):
        fmt.Println("overslept")
    case <-ctx.Done():
        fmt.Println(ctx.Err()) // 打印 "context deadline exceeded"
    }

    // Output:
    // context deadline exceeded

<h3 id="WithValue">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/context/context.go#L457">WithValue</a>
    <a href="#WithValue">¶</a></h3>
<pre>func WithValue(parent <a href="#Context">Context</a>, key, val interface{}) <a href="#Context">Context</a></pre>

WithValue 以 key 为键将 value 保存在 parent 的副本中并返回。

仅在请求声明周期中使用 Values，而不是将其作为函数的可选参数。

为了避免不同的包使用时出现相同的键，key 必须是可比较的并且不能是内置的 string 等类型。 WithValue 的用户应该定义自己的 key 类型。为了避免分配成 interface{}，context 的 key 一般都为具体类型。或者导出的 context 的 key 变量的静态类型应该是一个指针或者接口。

<a id="exampleWithValue"></a>
例:

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


