version: 1.10
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

### Examples

- [Ring.Do](#exampleRing_Do)
- [Ring.Len](#exampleRing_Len)
- [Ring.Link](#exampleRing_Link)
- [Ring.Move](#exampleRing_Move)
- [Ring.Next](#exampleRing_Next)
- [Ring.Prev](#exampleRing_Prev)
- [Ring.Unlink](#exampleRing_Unlink)

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

<a id="exampleRing_Do"></a>
Example:

    // Create a new ring of size 5
    r := ring.New(5)

    // Get the length of the ring
    n := r.Len()

    // Initialize the ring with some integer values
    for i := 0; i < n; i++ {
        r.Value = i
        r = r.Next()
    }

    // Iterate through the ring and print its contents
    r.Do(func(p interface{}) {
        fmt.Println(p.(int))
    })

    // Output:
    // 0
    // 1
    // 2
    // 3
    // 4

<h3 id="Ring.Len">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L111">Len</a>
    <a href="#Ring.Len">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Len() <a href="/builtin/#int">int</a></pre>

Len computes the number of elements in ring r. It executes in time proportional
to the number of elements.

<a id="exampleRing_Len"></a>
Example:

    // Create a new ring of size 4
    r := ring.New(4)

    // Print out its length
    fmt.Println(r.Len())

    // Output:
    // 4

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

<a id="exampleRing_Link"></a>
Example:

    // Create two rings, r and s, of size 2
    r := ring.New(2)
    s := ring.New(2)

    // Get the length of the ring
    lr := r.Len()
    ls := s.Len()

    // Initialize r with 0s
    for i := 0; i < lr; i++ {
        r.Value = 0
        r = r.Next()
    }

    // Initialize s with 1s
    for j := 0; j < ls; j++ {
        s.Value = 1
        s = s.Next()
    }

    // Link ring r and ring s
    rs := r.Link(s)

    // Iterate through the combined ring and print its contents
    rs.Do(func(p interface{}) {
        fmt.Println(p.(int))
    })

    // Output:
    // 0
    // 0
    // 1
    // 1

<h3 id="Ring.Move">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L34">Move</a>
    <a href="#Ring.Move">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Move(n <a href="/builtin/#int">int</a>) *<a href="#Ring">Ring</a></pre>

Move moves n % r.Len() elements backward (n < 0) or forward (n >= 0) in the ring
and returns that ring element. r must not be empty.

<a id="exampleRing_Move"></a>
Example:

    // Create a new ring of size 5
    r := ring.New(5)

    // Get the length of the ring
    n := r.Len()

    // Initialize the ring with some integer values
    for i := 0; i < n; i++ {
        r.Value = i
        r = r.Next()
    }

    // Move the pointer forward by three steps
    r = r.Move(3)

    // Iterate through the ring and print its contents
    r.Do(func(p interface{}) {
        fmt.Println(p.(int))
    })

    // Output:
    // 3
    // 4
    // 0
    // 1
    // 2

<h3 id="Ring.Next">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L16">Next</a>
    <a href="#Ring.Next">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Next() *<a href="#Ring">Ring</a></pre>

Next returns the next ring element. r must not be empty.

<a id="exampleRing_Next"></a>
Example:

    // Create a new ring of size 5
    r := ring.New(5)

    // Get the length of the ring
    n := r.Len()

    // Initialize the ring with some integer values
    for i := 0; i < n; i++ {
        r.Value = i
        r = r.Next()
    }

    // Iterate through the ring and print its contents
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

<h3 id="Ring.Prev">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L24">Prev</a>
    <a href="#Ring.Prev">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Prev() *<a href="#Ring">Ring</a></pre>

Prev returns the previous ring element. r must not be empty.

<a id="exampleRing_Prev"></a>
Example:

    // Create a new ring of size 5
    r := ring.New(5)

    // Get the length of the ring
    n := r.Len()

    // Initialize the ring with some integer values
    for i := 0; i < n; i++ {
        r.Value = i
        r = r.Next()
    }

    // Iterate through the ring backwards and print its contents
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

<h3 id="Ring.Unlink">func (*Ring) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/ring/ring.go#L101">Unlink</a>
    <a href="#Ring.Unlink">¶</a></h3>
<pre>func (r *<a href="#Ring">Ring</a>) Unlink(n <a href="/builtin/#int">int</a>) *<a href="#Ring">Ring</a></pre>

Unlink removes n % r.Len() elements from the ring r, starting at r.Next(). If n
% r.Len() == 0, r remains unchanged. The result is the removed subring. r must
not be empty.

<a id="exampleRing_Unlink"></a>
Example:

    // Create a new ring of size 6
    r := ring.New(6)

    // Get the length of the ring
    n := r.Len()

    // Initialize the ring with some integer values
    for i := 0; i < n; i++ {
        r.Value = i
        r = r.Next()
    }

    // Unlink three elements from r, starting from r.Next()
    r.Unlink(3)

    // Iterate through the remaining ring and print its contents
    r.Do(func(p interface{}) {
        fmt.Println(p.(int))
    })

    // Output:
    // 0
    // 4
    // 5


