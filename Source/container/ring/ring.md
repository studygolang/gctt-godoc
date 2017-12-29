version: 1.9.2
## package ring

  `import "container/ring"`

## Overview

Package ring implements operations on circular lists.

## Index

- [type Ring](#Ring)
  - [func New(n int) *Ring](#New)
  - [func (r *Ring) Do(f func(interface{}))](#Ring.Do)
  - [func (r *Ring) Len() int](#Ring.Len)
  - [func (r *Ring) Link(s *Ring) *Ring](#Ring.Link)
  - [func (r *Ring) Move(n int) *Ring](#Ring.Move)
  - [func (r *Ring) Next() *Ring](#Ring.Next)
  - [func (r *Ring) Prev() *Ring](#Ring.Prev)
  - [func (r *Ring) Unlink(n int) *Ring](#Ring.Unlink)

### Package files
 [ring.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go)

<h2 id="Ring">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L4">Ring</a>
    <a href="#Ring">¶</a></h2>
<pre>type Ring struct {
<span id="Ring.Value"></span>    Value interface{} <span class="comment">// for use by client; untouched by this library</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Ring is an element of a circular list, or ring. Rings do not have a beginning
or end; a pointer to any ring element serves as reference to the entire ring.
Empty rings are represented as nil Ring pointers. The zero value for a Ring is a
one-element ring with a nil Value.

<h3 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L52">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(n <a href="/builtin/#int">int</a>) *<a href="#Ring">Ring</a></pre>

New creates a ring of n elements.

<h3 id="Ring.Do">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L124">Do</a>
    <a href="#Ring.Do">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Do(f func(interface{}))</pre>

Do calls function f on each element of the ring, in forward order. The behavior
of Do is undefined if f changes *r.

<h3 id="Ring.Len">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L111">Len</a>
    <a href="#Ring.Len">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Len() <a href="/builtin/#int">int</a></pre>

Len computes the number of elements in ring r. It executes in time proportional
to the number of elements.

<h3 id="Ring.Link">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L83">Link</a>
    <a href="#Ring.Link">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Link(s *<a href="#Ring">Ring</a>) *<a href="#Ring">Ring</a></pre>

Link connects ring r with ring s such that r.Next() becomes s and returns the
original value for r.Next(). r must not be empty.

If r and s point to the same ring, linking them removes the elements between r
and s from the ring. The removed elements form a subring and the result is a
reference to that subring (if no elements were removed, the result is still the
original value for r.Next(), and not nil).

If r and s point to different rings, linking them creates a single ring with the
elements of s inserted after r. The result points to the element following the
last element of s after insertion.

<h3 id="Ring.Move">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L34">Move</a>
    <a href="#Ring.Move">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Move(n <a href="/builtin/#int">int</a>) *<a href="#Ring">Ring</a></pre>

Move moves n % r.Len() elements backward (n < 0) or forward (n >= 0) in the ring
and returns that ring element. r must not be empty.

<h3 id="Ring.Next">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L16">Next</a>
    <a href="#Ring.Next">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Next() *<a href="#Ring">Ring</a></pre>

Next returns the next ring element. r must not be empty.

<h3 id="Ring.Prev">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L24">Prev</a>
    <a href="#Ring.Prev">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Prev() *<a href="#Ring">Ring</a></pre>

Prev returns the previous ring element. r must not be empty.

<h3 id="Ring.Unlink">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L101">Unlink</a>
    <a href="#Ring.Unlink">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Unlink(n <a href="/builtin/#int">int</a>) *<a href="#Ring">Ring</a></pre>

Unlink removes n % r.Len() elements from the ring r, starting at r.Next(). If n
% r.Len() == 0, r remains unchanged. The result is the removed subring. r must
not be empty.


