version: 1.9.2
## package list

  `import "container/list"`

## Overview

Package list implements a doubly linked list.

To iterate over a list (where l is a *List):

    for e := l.Front(); e != nil; e = e.Next() {
    	// do something with e.Value
    }

<a id="example_"></a>
Example:

    // Create a new list and put some numbers in it.
    l := list.New()
    e4 := l.PushBack(4)
    e1 := l.PushFront(1)
    l.InsertBefore(3, e4)
    l.InsertAfter(2, e1)

    // Iterate through list and print its contents.
    for e := l.Front(); e != nil; e = e.Next() {
        fmt.Println(e.Value)
    }

    // Output:
    // 1
    // 2
    // 3
    // 4

## Index

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

### Examples

- [Package](#example_)

### Package files
 [list.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go)

<h2 id="Element">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L5">Element</a>
    <a href="#Element">¶</a></h2>
<pre>type Element struct {

    <span class="comment">// The value stored with this element.</span>
<span id="Element.Value"></span>    Value interface{}
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Element is an element of a linked list.

<h3 id="Element.Next">func (*Element) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L21">Next</a>
    <a href="#Element.Next">¶</a></h3>
<pre>func (e *<a href="#Element">Element</a>) Next() *<a href="#Element">Element</a></pre>

Next returns the next list element or nil.

<h3 id="Element.Prev">func (*Element) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L29">Prev</a>
    <a href="#Element.Prev">¶</a></h3>
<pre>func (e *<a href="#Element">Element</a>) Prev() *<a href="#Element">Element</a></pre>

Prev returns the previous list element or nil.

<h2 id="List">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L38">List</a>
    <a href="#List">¶</a></h2>
<pre>type List struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

List represents a doubly linked list. The zero value for List is an empty list
ready to use.

<h3 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L52">New</a>
    <a href="#New">¶</a></h3>
<pre>func New() *<a href="#List">List</a></pre>

New returns an initialized list.

<h3 id="List.Back">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L67">Back</a>
    <a href="#List.Back">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) Back() *<a href="#Element">Element</a></pre>

Back returns the last element of list l or nil.

<h3 id="List.Front">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L59">Front</a>
    <a href="#List.Front">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) Front() *<a href="#Element">Element</a></pre>

Front returns the first element of list l or nil.

<h3 id="List.Init">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L44">Init</a>
    <a href="#List.Init">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) Init() *<a href="#List">List</a></pre>

Init initializes or clears list l.

<h3 id="List.InsertAfter">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L144">InsertAfter</a>
    <a href="#List.InsertAfter">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) InsertAfter(v interface{}, mark *<a href="#Element">Element</a>) *<a href="#Element">Element</a></pre>

InsertAfter inserts a new element e with value v immediately after mark and
returns e. If mark is not an element of l, the list is not modified.

<h3 id="List.InsertBefore">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L134">InsertBefore</a>
    <a href="#List.InsertBefore">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) InsertBefore(v interface{}, mark *<a href="#Element">Element</a>) *<a href="#Element">Element</a></pre>

InsertBefore inserts a new element e with value v immediately before mark and
returns e. If mark is not an element of l, the list is not modified.

<h3 id="List.Len">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L56">Len</a>
    <a href="#List.Len">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) Len() <a href="/builtin/#int">int</a></pre>

Len returns the number of elements of list l. The complexity is O(1).

<h3 id="List.MoveAfter">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L183">MoveAfter</a>
    <a href="#List.MoveAfter">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) MoveAfter(e, mark *<a href="#Element">Element</a>)</pre>

MoveAfter moves element e to its new position after mark. If e or mark is not an
element of l, or e == mark, the list is not modified.

<h3 id="List.MoveBefore">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L174">MoveBefore</a>
    <a href="#List.MoveBefore">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) MoveBefore(e, mark *<a href="#Element">Element</a>)</pre>

MoveBefore moves element e to its new position before mark. If e or mark is not
an element of l, or e == mark, the list is not modified.

<h3 id="List.MoveToBack">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L164">MoveToBack</a>
    <a href="#List.MoveToBack">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) MoveToBack(e *<a href="#Element">Element</a>)</pre>

MoveToBack moves element e to the back of list l. If e is not an element of l,
the list is not modified.

<h3 id="List.MoveToFront">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L154">MoveToFront</a>
    <a href="#List.MoveToFront">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) MoveToFront(e *<a href="#Element">Element</a>)</pre>

MoveToFront moves element e to the front of list l. If e is not an element of l,
the list is not modified.

<h3 id="List.PushBack">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L127">PushBack</a>
    <a href="#List.PushBack">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) PushBack(v interface{}) *<a href="#Element">Element</a></pre>

PushBack inserts a new element e with value v at the back of list l and returns
e.

<h3 id="List.PushBackList">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L192">PushBackList</a>
    <a href="#List.PushBackList">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) PushBackList(other *<a href="#List">List</a>)</pre>

PushBackList inserts a copy of an other list at the back of list l. The lists l
and other may be the same.

<h3 id="List.PushFront">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L121">PushFront</a>
    <a href="#List.PushFront">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) PushFront(v interface{}) *<a href="#Element">Element</a></pre>

PushFront inserts a new element e with value v at the front of list l and
returns e.

<h3 id="List.PushFrontList">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L201">PushFrontList</a>
    <a href="#List.PushFrontList">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) PushFrontList(other *<a href="#List">List</a>)</pre>

PushFrontList inserts a copy of an other list at the front of list l. The lists
l and other may be the same.

<h3 id="List.Remove">func (*List) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/list/list.go#L111">Remove</a>
    <a href="#List.Remove">¶</a></h3>
<pre>func (l *<a href="#List">List</a>) Remove(e *<a href="#Element">Element</a>) interface{}</pre>

Remove removes e from l if e is an element of list l. It returns the element
value e.Value.


