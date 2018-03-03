version: 1.10
## package heap

  `import "container/heap"`

## Overview

Package heap provides heap operations for any type that implements
heap.Interface. A heap is a tree with the property that each node is the
minimum-valued node in its subtree.

The minimum element in the tree is the root, at index 0.

A heap is a common way to implement a priority queue. To build a priority queue,
implement the Heap interface with the (negative) priority as the ordering for
the Less method, so Push adds items while Pop removes the highest-priority item
from the queue. The Examples include such an implementation; the file
example_pq_test.go has the complete source.

<a id="example_intHeap"></a>
Example:

    // This example demonstrates an integer heap built using the heap interface.
    package heap_test

    import (
        "container/heap"
        "fmt"
    )

    // An IntHeap is a min-heap of ints.
    type IntHeap []int

    func (h IntHeap) Len() int           { return len(h) }
    func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
    func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

    func (h *IntHeap) Push(x interface{}) {
        // Push and Pop use pointer receivers because they modify the slice's length,
        // not just its contents.
        *h = append(*h, x.(int))
    }

    func (h *IntHeap) Pop() interface{} {
        old := *h
        n := len(old)
        x := old[n-1]
        *h = old[0 : n-1]
        return x
    }

    // This example inserts several ints into an IntHeap, checks the minimum,
    // and removes them in order of priority.
    func Example_intHeap() {
        h := &IntHeap{2, 1, 5}
        heap.Init(h)
        heap.Push(h, 3)
        fmt.Printf("minimum: %d\n", (*h)[0])
        for h.Len() > 0 {
            fmt.Printf("%d ", heap.Pop(h))
        }
        // Output:
        // minimum: 1
        // 1 2 3 5
    }


<a id="example_priorityQueue"></a>
Example:

    // This example demonstrates a priority queue built using the heap interface.
    package heap_test

    import (
        "container/heap"
        "fmt"
    )

    // An Item is something we manage in a priority queue.
    type Item struct {
        value    string // The value of the item; arbitrary.
        priority int    // The priority of the item in the queue.
        // The index is needed by update and is maintained by the heap.Interface methods.
        index int // The index of the item in the heap.
    }

    // A PriorityQueue implements heap.Interface and holds Items.
    type PriorityQueue []*Item

    func (pq PriorityQueue) Len() int { return len(pq) }

    func (pq PriorityQueue) Less(i, j int) bool {
        // We want Pop to give us the highest, not lowest, priority so we use greater than here.
        return pq[i].priority > pq[j].priority
    }

    func (pq PriorityQueue) Swap(i, j int) {
        pq[i], pq[j] = pq[j], pq[i]
        pq[i].index = i
        pq[j].index = j
    }

    func (pq *PriorityQueue) Push(x interface{}) {
        n := len(*pq)
        item := x.(*Item)
        item.index = n
        *pq = append(*pq, item)
    }

    func (pq *PriorityQueue) Pop() interface{} {
        old := *pq
        n := len(old)
        item := old[n-1]
        item.index = -1 // for safety
        *pq = old[0 : n-1]
        return item
    }

    // update modifies the priority and value of an Item in the queue.
    func (pq *PriorityQueue) update(item *Item, value string, priority int) {
        item.value = value
        item.priority = priority
        heap.Fix(pq, item.index)
    }

    // This example creates a PriorityQueue with some items, adds and manipulates an item,
    // and then removes the items in priority order.
    func Example_priorityQueue() {
        // Some items and their priorities.
        items := map[string]int{
            "banana": 3, "apple": 2, "pear": 4,
        }

        // Create a priority queue, put the items in it, and
        // establish the priority queue (heap) invariants.
        pq := make(PriorityQueue, len(items))
        i := 0
        for value, priority := range items {
            pq[i] = &Item{
                value:    value,
                priority: priority,
                index:    i,
            }
            i++
        }
        heap.Init(&pq)

        // Insert a new item and then modify its priority.
        item := &Item{
            value:    "orange",
            priority: 1,
        }
        heap.Push(&pq, item)
        pq.update(item, item.value, 5)

        // Take the items out; they arrive in decreasing priority order.
        for pq.Len() > 0 {
            item := heap.Pop(&pq).(*Item)
            fmt.Printf("%.2d:%s ", item.priority, item.value)
        }
        // Output:
        // 05:orange 04:pear 03:banana 02:apple
    }

## Index

- [func Fix(h Interface, i int)](#Fix)
- [func Init(h Interface)](#Init)
- [func Pop(h Interface) interface{}](#Pop)
- [func Push(h Interface, x interface{})](#Push)
- [func Remove(h Interface, i int) interface{}](#Remove)
- [type Interface](#Interface)

### Examples

- [Package (IntHeap)](#example_intHeap)
- [Package (PriorityQueue)](#example_priorityQueue)

### Package files
 [heap.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/heap/heap.go)

<h2 id="Fix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/heap/heap.go#L76">Fix</a>
    <a href="#Fix">¶</a></h2>
<pre>func Fix(h <a href="#Interface">Interface</a>, i <a href="/builtin/#int">int</a>)</pre>

Fix re-establishes the heap ordering after the element at index i has changed
its value. Changing the value of the element at index i and then calling Fix is
equivalent to, but less expensive than, calling Remove(h, i) followed by a Push
of the new value. The complexity is O(log(n)) where n = h.Len().

<h2 id="Init">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/heap/heap.go#L31">Init</a>
    <a href="#Init">¶</a></h2>
<pre>func Init(h <a href="#Interface">Interface</a>)</pre>

A heap must be initialized before any of the heap operations can be used. Init
is idempotent with respect to the heap invariants and may be called whenever the
heap invariants may have been invalidated. Its complexity is O(n) where n =
h.Len().

<h2 id="Pop">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/heap/heap.go#L51">Pop</a>
    <a href="#Pop">¶</a></h2>
<pre>func Pop(h <a href="#Interface">Interface</a>) interface{}</pre>

Pop removes the minimum element (according to Less) from the heap and returns
it. The complexity is O(log(n)) where n = h.Len(). It is equivalent to Remove(h,
0).

<h2 id="Push">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/heap/heap.go#L42">Push</a>
    <a href="#Push">¶</a></h2>
<pre>func Push(h <a href="#Interface">Interface</a>, x interface{})</pre>

Push pushes the element x onto the heap. The complexity is O(log(n)) where n =
h.Len().

<h2 id="Remove">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/heap/heap.go#L61">Remove</a>
    <a href="#Remove">¶</a></h2>
<pre>func Remove(h <a href="#Interface">Interface</a>, i <a href="/builtin/#int">int</a>) interface{}</pre>

Remove removes the element at index i from the heap. The complexity is O(log(n))
where n = h.Len().

<h2 id="Interface">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/container/heap/heap.go#L20">Interface</a>
    <a href="#Interface">¶</a></h2>
<pre>type Interface interface {
    <a href="/sort/">sort</a>.<a href="/sort/#Interface">Interface</a>
    Push(x interface{}) <span class="comment">// add x as element Len()</span>
    Pop() interface{}   <span class="comment">// remove and return element Len() - 1.</span>
}</pre>

Any type that implements heap.Interface may be used as a min-heap with the
following invariants (established after Init has been called or if the data is
empty or sorted):

    !h.Less(j, i) for 0 <= i < h.Len() and 2*i+1 <= j <= 2*i+2 and j < h.Len()

Note that Push and Pop in this interface are for package heap's implementation
to call. To add and remove things from the heap, use heap.Push and heap.Pop.


