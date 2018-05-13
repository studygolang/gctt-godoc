version: 1.10
## package list

  `import "container/list"`

## 概述

list 包实现一个双向链表。

迭代一个链表（这里 l 是一个 List 指针）：

    for e := l.Front(); e != nil; e = e.Next() {
    	// do something with e.Value
    }

<a id="example"></a>
例子:

    // 创建一个新的链表，然后放入若干数字。
    l := list.New()
    e4 := l.PushBack(4)
    e1 := l.PushFront(1)
    l.InsertBefore(3, e4)
    l.InsertAfter(2, e1)

    // 迭代链表并打印内容。
    for e := l.Front(); e != nil; e = e.Next() {
        fmt.Println(e.Value)
    }

    // Output:
    // 1
    // 2
    // 3
    // 4

## 索引

- [type Element](#Element)
  - [func (e *Element) Next() *Element](#Element.Next)
  - [func (e *Element) Prev() *Element](#Element.Prev)
- [type List](#List)
  - [func New() *List](#New)
  - [func (l *List) Back() *Element](#List.Back)
  - [func (l *List) Front() *Element](#List.Front)
  - [func (l *List) Init() *List](#List.Init)
  - [func (l *List) InsertAfter(v interface{}, mark *Element) *Element](#List.InsertAfter)
  - [func (l *List) InsertBefore(v interface{}, mark *Element) *Element](#List.InsertBefore)
  - [func (l *List) Len() int](#List.Len)
  - [func (l *List) MoveAfter(e, mark *Element)](#List.MoveAfter)
  - [func (l *List) MoveBefore(e, mark *Element)](#List.MoveBefore)
  - [func (l *List) MoveToBack(e *Element)](#List.MoveToBack)
  - [func (l *List) MoveToFront(e *Element)](#List.MoveToFront)
  - [func (l *List) PushBack(v interface{}) *Element](#List.PushBack)
  - [func (l *List) PushBackList(other *List)](#List.PushBackList)
  - [func (l *List) PushFront(v interface{}) *Element](#List.PushFront)
  - [func (l *List) PushFrontList(other *List)](#List.PushFrontList)
  - [func (l *List) Remove(e *Element) interface{}](#List.Remove)

### 例子

- [Package](#example)

### 文件
 [list.go](//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go)

<h2 id="Element">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L5">Element</a>
    <a href="#Element">¶</a></h2>
<pre>type Element struct {

    <span class="comment">// 该元素中存储的值。</span>
<span id="Element.Value"></span>    Value interface{}
    <span class="comment">// 包含被过滤的或未导出的字段</span>
}</pre>

Element 是链表里的一个元素。

<h3 id="Element.Next">func (*Element) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L21">Next</a>
    <a href="#Element.Next">¶</a></h3>
<pre>func (e *<a href="#Element">Element</a>) Next() *<a href="#Element">Element</a></pre>

Next 方法返回链表里的下一个元素或 nil 。

<h3 id="Element.Prev">func (*Element) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L29">Prev</a>
    <a href="#Element.Prev">¶</a></h3>
<pre>func (e *<a href="#Element">Element</a>) Prev() *<a href="#Element">Element</a></pre>

Prev 方法返回链表里的前一个元素或 nil 。

<h2 id="List">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L38">List</a>
    <a href="#List">¶</a></h2>
<pre>type List struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

List 类型表示一个双向链表。List 的值为0是一个可用的空链表。

<h3 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L52">New</a>
    <a href="#New">¶</a></h3>
<pre>func New() *<a href="#List">List</a></pre>

New 函数返回一个初始化过的链表。

<h3 id="List.Back">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L67">Back</a>
    <a href="#List.Back">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) Back() *<a href="#Element">Element</a></pre>

Back 方法返回链表的最后一个元素，如果链表是空的则返回 nil 。

<h3 id="List.Front">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L59">Front</a>
    <a href="#List.Front">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) Front() *<a href="#Element">Element</a></pre>

Front 方法返回链表的第一个元素，如果链表是空的则返回 nil 。

<h3 id="List.Init">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L44">Init</a>
    <a href="#List.Init">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) Init() *<a href="#List">List</a></pre>

Init 方法初始化或清空链表 l 。

<h3 id="List.InsertAfter">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L147">InsertAfter</a>
    <a href="#List.InsertAfter">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) InsertAfter(v interface{}, mark *<a href="#Element">Element</a>) *<a href="#Element">Element</a></pre>

InsertAfter 方法在参数 mark 后面插入一个值为 v 的新元素 e 并返回 e 。如果参数 mark 不是 l 的元素，链表不会被修改。参数 mark 一定不能为 nil 。

<h3 id="List.InsertBefore">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L136">InsertBefore</a>
    <a href="#List.InsertBefore">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) InsertBefore(v interface{}, mark *<a href="#Element">Element</a>) *<a href="#Element">Element</a></pre>

InsertBefore 方法在参数 mark 前面插入一个值为 v 的新元素 e 并返回 e 。如果参数 mark 不是 l 的元素，链表不会被修改。参数 mark 一定不能为 nil 。

<h3 id="List.Len">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L56">Len</a>
    <a href="#List.Len">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) Len() <a href="/builtin/#int">int</a></pre>

Len 方法返回链表 l 中元素的个数。函数复杂度是 O(1)。

<h3 id="List.MoveAfter">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L190">MoveAfter</a>
    <a href="#List.MoveAfter">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) MoveAfter(e, mark *<a href="#Element">Element</a>)</pre>

MoveAfter 方法移动元素 e 到参数 mark 后面的新位置。如果 e 或 mark 不是链表 l 的元素，或 e 等于 mark，链表不会被修改。e 和 mark 一定不能为 nil 。

<h3 id="List.MoveBefore">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L180">MoveBefore</a>
    <a href="#List.MoveBefore">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) MoveBefore(e, mark *<a href="#Element">Element</a>)</pre>

MoveBefore 方法移动元素 e 到参数 mark 前面的新位置。如果 e 或 mark 不是链表 l 的元素，或 e 等于 mark，链表不会被修改。e 和 mark 一定不能为 nil 。

<h3 id="List.MoveToBack">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L169">MoveToBack</a>
    <a href="#List.MoveToBack">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) MoveToBack(e *<a href="#Element">Element</a>)</pre>

MoveToBack 方法移动元素 e 到链表的末尾。如果 e 不是链表 l 的元素，链表不会被修改。e 一定不能为 nil 。

<h3 id="List.MoveToFront">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L158">MoveToFront</a>
    <a href="#List.MoveToFront">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) MoveToFront(e *<a href="#Element">Element</a>)</pre>

MoveToFront 方法移动元素 e 到链表的开头。如果 e 不是链表 l 的元素，链表不会被修改。e 一定不能为 nil 。

<h3 id="List.PushBack">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L128">PushBack</a>
    <a href="#List.PushBack">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) PushBack(v interface{}) *<a href="#Element">Element</a></pre>

PushBack 方法插入值为 v 的新元素 e 到链表 l 的末尾，并返回 e 。

<h3 id="List.PushBackList">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L199">PushBackList</a>
    <a href="#List.PushBackList">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) PushBackList(other *<a href="#List">List</a>)</pre>

PushBackList 方法插入链表 other 的一份拷贝到链表 l 的末尾。链表 l 和 other 可能一样，但它们一定不能为 nil 。

<h3 id="List.PushFront">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L122">PushFront</a>
    <a href="#List.PushFront">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) PushFront(v interface{}) *<a href="#Element">Element</a></pre>

PushFront 方法插入值为 v 的新元素 e 到链表 l 的开头，并返回 e 。

<h3 id="List.PushFrontList">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L208">PushFrontList</a>
    <a href="#List.PushFrontList">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) PushFrontList(other *<a href="#List">List</a>)</pre>

PushFrontList 方法插入链表 other 的一份拷贝到链表 l 的开头。链表 l 和 other 可能一样，但它们一定不能为 nil 。

<h3 id="List.Remove">func (*List) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/list/list.go#L112">Remove</a>
    <a href="#List.Remove">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) Remove(e *<a href="#Element">Element</a>) interface{}</pre>

Remove 方法从链表 l 中移除元素 e 。这个函数返回元素 e 的值，即 e.Value 。元素 e 一定不能为 nil 。


