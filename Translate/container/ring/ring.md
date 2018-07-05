version: 1.10
## package ring

  `import "container/ring"`

## 概述

ring 包实现了循环链表的操作。

## 索引

- [type Ring](#Ring)
  - [func New(n int) *Ring](#New)
  - [func (r *Ring) Do(f func(interface{}))](#Ring.Do)
  - [func (r *Ring) Len() int](#Ring.Len)
  - [func (r *Ring) Link(s *Ring) *Ring](#Ring.Link)
  - [func (r *Ring) Move(n int) *Ring](#Ring.Move)
  - [func (r *Ring) Next() *Ring](#Ring.Next)
  - [func (r *Ring) Prev() *Ring](#Ring.Prev)
  - [func (r *Ring) Unlink(n int) *Ring](#Ring.Unlink)

### 例子

- [Ring.Do](#exampleRing_Do)
- [Ring.Len](#exampleRing_Len)
- [Ring.Link](#exampleRing_Link)
- [Ring.Move](#exampleRing_Move)
- [Ring.Next](#exampleRing_Next)
- [Ring.Prev](#exampleRing_Prev)
- [Ring.Unlink](#exampleRing_Unlink)

### 文件
 [ring.go](//github.com/golang/go/blob/release-branch.go1.10/src/container/ring/ring.go)

<h2 id="Ring">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/ring/ring.go#L4">Ring</a>
    <a href="#Ring">¶</a></h2>
<pre>type Ring struct {
<span id="Ring.Value"></span>    Value interface{} <span class="comment">// 被用户使用，本库不会接触</span>
    <span class="comment">// 包含被过滤的或未导出的字段</span>
}</pre>

Ring 是一个循环链表。循环链表没有开始和结束，通过循环链表中的任意一个元素的指针都能引用整个链表。使用值为 nil 的 Ring 指针来表示空循环链表。零值循环链表是指链表只有一个值为 nil 的元素。

<h3 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/ring/ring.go#L52">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(n <a href="/builtin/#int">int</a>) *<a href="#Ring">Ring</a></pre>

New 函数创建一个元素个数为 n 的循环链表。

<h3 id="Ring.Do">func (*Ring) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/ring/ring.go#L124">Do</a>
    <a href="#Ring.Do">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Do(f func(interface{}))</pre>

Do 方法对循环链表的每个元素都调用函数 f 。如果函数 f 改变了循环链表，Do 方法的结果将无法预测。

<a id="exampleRing_Do"></a>
例子:

    // 创建一个大小为5的循环链表
    r := ring.New(5)

    // Get the length of the ring
    // 获得循环链表的长度

    // 用若干整数来初始化循环链表
    for i := 0; i < n; i++ {
        r.Value = i
        r = r.Next()
    }

    // 迭代循环链表并打印内容
    r.Do(func(p interface{}) {
        fmt.Println(p.(int))
    })

    // Output:
    // 0
    // 1
    // 2
    // 3
    // 4

<h3 id="Ring.Len">func (*Ring) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/ring/ring.go#L111">Len</a>
    <a href="#Ring.Len">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Len() <a href="/builtin/#int">int</a></pre>

Len 方法计算循环链表 r 的元素个数。函数的执行时间与元素个数成正比。

<a id="exampleRing_Len"></a>
Example:

    // 创建一个大小为4的循环链表
    r := ring.New(4)

    // 打印循环链表的长度
    fmt.Println(r.Len())

    // Output:
    // 4

<h3 id="Ring.Link">func (*Ring) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/ring/ring.go#L83">Link</a>
    <a href="#Ring.Link">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Link(s *<a href="#Ring">Ring</a>) *<a href="#Ring">Ring</a></pre>

Link 方法连接循环链表 r 和 s ，所以 r.Next() 会将变成 s ，并且函数返回 r.Next() 的原来的值。参数 r 一定不能是空。

如果 r 和 s 指向同一个循环链表，连接它们会移除循环链表里 r 和 s 之间的元素。被移除的元素形成了一个子循环链表，函数返回的是对这个子循环链表的引用（如果没有元素被移除，返回结果是原来的 r.Next() 的值，而不是 nil）。

如果 r 和 s 指向不同的循环链表，连接它们将把 s 中的元素插入到 r 中，合成一个循环链表。函数返回的结果指向插入后的 s 的最后一个元素的下一个元素。

<a id="exampleRing_Link"></a>
例子:

    // 创建两个循环链表， r 和 s，长度都是2
    r := ring.New(2)
    s := ring.New(2)

    // 获得循环链表的长度
    lr := r.Len()
    ls := s.Len()

    // 初始化 r 中元素的值为0
    for i := 0; i < lr; i++ {
        r.Value = 0
        r = r.Next()
    }

    // 初始化 s 中元素的值为1
    for j := 0; j < ls; j++ {
        s.Value = 1
        s = s.Next()
    }

    // 连接 r 和 s
    rs := r.Link(s)

    // 迭代组合后的循环链表并打印内容
    rs.Do(func(p interface{}) {
        fmt.Println(p.(int))
    })

    // Output:
    // 0
    // 0
    // 1
    // 1

<h3 id="Ring.Move">func (*Ring) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/ring/ring.go#L34">Move</a>
    <a href="#Ring.Move">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Move(n <a href="/builtin/#int">int</a>) *<a href="#Ring">Ring</a></pre>

Move 方法把循环链表中的 n % r.Len() 个元素向后（n < 0）或向前（n >= 0）移动，函数返回这个循环链表。r 一定不能是空。

<a id="exampleRing_Move"></a>
例子:

    // 创建一个大小为5的循环链表
    r := ring.New(5)

    // 获得循环链表的长度
    n := r.Len()

    // 用若干整数初始化循环链表
    for i := 0; i < n; i++ {
        r.Value = i
        r = r.Next()
    }

    // 把循环链表向前移动3步
    r = r.Move(3)

    // 迭代循环链表并打印内容
    r.Do(func(p interface{}) {
        fmt.Println(p.(int))
    })

    // Output:
    // 3
    // 4
    // 0
    // 1
    // 2

<h3 id="Ring.Next">func (*Ring) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/ring/ring.go#L16">Next</a>
    <a href="#Ring.Next">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Next() *<a href="#Ring">Ring</a></pre>

Next 方法返回循环链表中的下一个元素。r 一定不能是空。

<a id="exampleRing_Next"></a>
例子:

    // 创建一个大小为5的循环链表
    r := ring.New(5)

    // 获得循环链表的长度
    n := r.Len()

    // 用若干整数初始化循环链表
    for i := 0; i < n; i++ {
        r.Value = i
        r = r.Next()
    }

    // 迭代循环链表并打印内容
    for j := 0; j < n; j++ {
        fmt.Println(r.Value)
        r = r.Next()
    }

    // Output:
    // 0
    // 1
    // 2
    // 3
    // 4

<h3 id="Ring.Prev">func (*Ring) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/ring/ring.go#L24">Prev</a>
    <a href="#Ring.Prev">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Prev() *<a href="#Ring">Ring</a></pre>

Prev 方法返回循环链表中的前一个元素。r 一定不能是空。

<a id="exampleRing_Prev"></a>
Example:

    // 创建一个大小为5的循环链表
    r := ring.New(5)

    // 获得循环链表的长度
    n := r.Len()

    // 用若干整数初始化循环链表
    for i := 0; i < n; i++ {
        r.Value = i
        r = r.Next()
    }

    // 迭代循环链表并打印内容
    for j := 0; j < n; j++ {
        r = r.Prev()
        fmt.Println(r.Value)
    }

    // Output:
    // 4
    // 3
    // 2
    // 1
    // 0

<h3 id="Ring.Unlink">func (*Ring) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/ring/ring.go#L101">Unlink</a>
    <a href="#Ring.Unlink">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Unlink(n <a href="/builtin/#int">int</a>) *<a href="#Ring">Ring</a></pre>

Unlink 方法以 r.Next() 为起点，从循环链表 r 中移除 n % r.Len() 个元素。如果 n
% r.Len() 等于0， r 保持不变。函数返回被移除的子循环链表。r 一定不是是空。

<a id="exampleRing_Unlink"></a>
Example:

    // 创建一个大小为6的循环链表
    r := ring.New(6)

    // 获得循环链表的长度
    n := r.Len()

    // 用若干整数初始化循环链表
    for i := 0; i < n; i++ {
        r.Value = i
        r = r.Next()
    }

    // 从 r.Next() 开始，Unlink 3个元素
    r.Unlink(3)

    // 迭代循环链表的剩余部分并打印内容
    r.Do(func(p interface{}) {
        fmt.Println(p.(int))
    })

    // Output:
    // 0
    // 4
    // 5


