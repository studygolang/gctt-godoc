version: 1.10
## package heap

  `import "container/heap"`

## 概述

heap 包为任何实现了 heap.Interface 的类型提供对 heap 的操作。heap 是一种叫做最小树的数据结构，特点是每个结点都是子树的最小值。

树里的最小元素位于根结点，索引是0。

heap 是实现优先队列的常见方法。要建立一个优先队列，需要实现 Heap 接口，并在 Less 方法中以（负的）优先级作为排序的条件。下面的例子包含一个对应的实现，example_pq_test.go 文件包含完整的源码。

<a id="example_intHeap"></a>
例子:

    // 这个例子演示了一个使用 heap 接口构建的整数堆。
    package heap_test

    import (
        "container/heap"
        "fmt"
    )

    // IntHeap 是元素为整数的最小堆。
    type IntHeap []int

    func (h IntHeap) Len() int           { return len(h) }
    func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
    func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

    func (h *IntHeap) Push(x interface{}) {
        // Push and Pop 方法使用指针作为接收者是因为它们修改切片的长度，
        // 而不只是内容。
        *h = append(*h, x.(int))
    }

    func (h *IntHeap) Pop() interface{} {
        old := *h
        n := len(old)
        x := old[n-1]
        *h = old[0 : n-1]
        return x
    }

    // 这个例子插入若干整数到 IntHeap ，检查堆的最小值，
    // 然后按照优先级顺序删除它们。
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
例子:

    // 这个例子演示了一个使用 heap 接口构建的优先队列。
    package heap_test

    import (
        "container/heap"
        "fmt"
    )

    // Item 是优先队列里的元素。
    type Item struct {
        value    string // 元素的值，任何内容都行。
        priority int    // 本元素在队列里的优先级。
        // index 被 update 方法使用，并被 heap.Interface 的方法维护。
        index int // 元素在堆里的索引。
    }

    // PriorityQueue 类型实现了 heap.Interface 并存储数据。
    type PriorityQueue []*Item

    func (pq PriorityQueue) Len() int { return len(pq) }

    func (pq PriorityQueue) Less(i, j int) bool {
        // 我们想要 Pop 函数获得最高的，而不是最低的，优先级所以我们这里使用大于号。
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

    // update 方法修改队列里元素的优先级和值。
    func (pq *PriorityQueue) update(item *Item, value string, priority int) {
        item.value = value
        item.priority = priority
        heap.Fix(pq, item.index)
    }

    // 这个例子创建一个有若干元素的 PriorityQueue ，添加并操作一个元素，
    // 然后按照优先级顺序移除所有元素。
    func Example_priorityQueue() {
        // 若干带有优先级的元素。
        items := map[string]int{
            "banana": 3, "apple": 2, "pear": 4,
        }

        // 创建一个优先队列，把 items 放进去，然后建立优先队列（堆）的不变量。
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

        // 插入一个新的元素，然后修改它的优先级。
        item := &Item{
            value:    "orange",
            priority: 1,
        }
        heap.Push(&pq, item)
        pq.update(item, item.value, 5)

        // 取出所有元素，它们应该按照优先级递减的顺序出来。
        for pq.Len() > 0 {
            item := heap.Pop(&pq).(*Item)
            fmt.Printf("%.2d:%s ", item.priority, item.value)
        }
        // Output:
        // 05:orange 04:pear 03:banana 02:apple
    }

## 索引

- [func Fix(h Interface, i int)](#Fix)
- [func Init(h Interface)](#Init)
- [func Pop(h Interface) interface{}](#Pop)
- [func Push(h Interface, x interface{})](#Push)
- [func Remove(h Interface, i int) interface{}](#Remove)
- [type Interface](#Interface)

### 例子

- [Package (IntHeap)](#example_intHeap)
- [Package (PriorityQueue)](#example_priorityQueue)

### 文件
 [heap.go](//github.com/golang/go/blob/release-branch.go1.10/src/container/heap/heap.go)

<h2 id="Fix">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/heap/heap.go#L76">Fix</a>
    <a href="#Fix">¶</a></h2>
<pre>func Fix(h <a href="#Interface">Interface</a>, i <a href="/builtin/#int">int</a>)</pre>

Fix 函数在索引 i 的元素的值变化后重新构建堆。改变索引 i 处元素的值并调用 Fix 函数等价于调用 Remove(h, i) 然后调用 Push 函数来插入新的数据，不过代价较小。算法的时间复杂度是 O(log(n))，这里 n 等于 h.Len()。

<h2 id="Init">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/heap/heap.go#L31">Init</a>
    <a href="#Init">¶</a></h2>
<pre>func Init(h <a href="#Interface">Interface</a>)</pre>

堆必须初始化后才能使用。Init 函数会初始化堆的不变量，并应该在堆的不变量无效时调用。算法的时间复杂度是 O(n)，这里 n 等于 h.Len()。

<h2 id="Pop">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/heap/heap.go#L51">Pop</a>
    <a href="#Pop">¶</a></h2>
<pre>func Pop(h <a href="#Interface">Interface</a>) interface{}</pre>

Pop 函数从堆里移除值最小的元素（通过 Less 方法）并返回该元素。算法的时间复杂度是 O(log(n))，这里 n 等于 h.Len()。它等价于 Remove(h, 0)。

<h2 id="Push">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/heap/heap.go#L42">Push</a>
    <a href="#Push">¶</a></h2>
<pre>func Push(h <a href="#Interface">Interface</a>, x interface{})</pre>

Push 函数添加元素 x 到堆里。算法的时间复杂度是 O(log(n))，这里 n 等于 h.Len()。

<h2 id="Remove">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/heap/heap.go#L61">Remove</a>
    <a href="#Remove">¶</a></h2>
<pre>func Remove(h <a href="#Interface">Interface</a>, i <a href="/builtin/#int">int</a>) interface{}</pre>

Remove 函数从堆里移除索引 i 处的元素。算法的时间复杂度是 O(log(n))，这里 n 等于 h.Len()。

<h2 id="Interface">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/container/heap/heap.go#L20">Interface</a>
    <a href="#Interface">¶</a></h2>
<pre>type Interface interface {
    <a href="/sort/">sort</a>.<a href="/sort/#Interface">Interface</a>
    Push(x interface{}) <span class="comment">// add x as element Len()</span>
    Pop() interface{}   <span class="comment">// remove and return element Len() - 1.</span>
}</pre>

任何实现了 heap.Interface 并且拥有如下不变量（在 Init 函数被调用后或数据是空或被排序后）的类型可以用作最小堆：

    !h.Less(j, i) for 0 <= i < h.Len() and 2*i+1 <= j <= 2*i+2 and j < h.Len()

注意：这个接口里的 Push 和 Pop 方法是heap包内部实现调用。如果要向堆里添加或移除数据，请使用 heap.Push 和 heap.Pop 函数。


