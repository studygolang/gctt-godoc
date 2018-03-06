version: 1.10
## package suffixarray

  `import "index/suffixarray"`

## Overview

Package suffixarray implements substring search in logarithmic time using an
in-memory suffix array.

Example use:

    // create index for some data
    index := suffixarray.New(data)

    // lookup byte slice s
    offsets1 := index.Lookup(s, -1) // the list of all indices where s occurs in data
    offsets2 := index.Lookup(s, 3)  // the list of at most 3 indices where s occurs in data

## Index

- [type Index](#Index)
  - [func New(data []byte) *Index](#New)
  - [func (x *Index) Bytes() []byte](#Index.Bytes)
  - [func (x *Index) FindAllIndex(r *regexp.Regexp, n int) (result [][]int)](#Index.FindAllIndex)
  - [func (x *Index) Lookup(s []byte, n int) (result []int)](#Index.Lookup)
  - [func (x *Index) Read(r io.Reader) error](#Index.Read)
  - [func (x *Index) Write(w io.Writer) error](#Index.Write)

### Examples

- [Index.Lookup](#exampleIndex_Lookup)

### Package files
 [qsufsort.go](//github.com/golang/go/blob/release-branch.go1.10/src/index/suffixarray/qsufsort.go) [suffixarray.go](//github.com/golang/go/blob/release-branch.go1.10/src/index/suffixarray/suffixarray.go)

<h2 id="Index">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/index/suffixarray/suffixarray.go#L18">Index</a>
    <a href="#Index">¶</a></h2>
<pre>type Index struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Index implements a suffix array for fast substring search.

<h3 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/index/suffixarray/suffixarray.go#L25">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(data []<a href="/builtin/#byte">byte</a>) *<a href="#Index">Index</a></pre>

New creates a new Index for data. Index creation time is O(N*log(N)) for N =
len(data).

<h3 id="Index.Bytes">func (*Index) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/index/suffixarray/suffixarray.go#L155">Bytes</a>
    <a href="#Index.Bytes">¶</a></h3>
<pre>func (x *<a href="#Index">Index</a>) Bytes() []<a href="/builtin/#byte">byte</a></pre>

Bytes returns the data over which the index was created. It must not be
modified.

<h3 id="Index.FindAllIndex">func (*Index) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/index/suffixarray/suffixarray.go#L202">FindAllIndex</a>
    <a href="#Index.FindAllIndex">¶</a></h3>
<pre>func (x *<a href="#Index">Index</a>) FindAllIndex(r *<a href="/regexp/">regexp</a>.<a href="/regexp/#Regexp">Regexp</a>, n <a href="/builtin/#int">int</a>) (result [][]<a href="/builtin/#int">int</a>)</pre>

FindAllIndex returns a sorted list of non-overlapping matches of the regular
expression r, where a match is a pair of indices specifying the matched slice of
x.Bytes(). If n < 0, all matches are returned in successive order. Otherwise, at
most n matches are returned and they may not be successive. The result is nil if
there are no matches, or if n == 0.

<h3 id="Index.Lookup">func (*Index) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/index/suffixarray/suffixarray.go#L180">Lookup</a>
    <a href="#Index.Lookup">¶</a></h3>
<pre>func (x *<a href="#Index">Index</a>) Lookup(s []<a href="/builtin/#byte">byte</a>, n <a href="/builtin/#int">int</a>) (result []<a href="/builtin/#int">int</a>)</pre>

Lookup returns an unsorted list of at most n indices where the byte string s
occurs in the indexed data. If n < 0, all occurrences are returned. The result
is nil if s is empty, s is not found, or n == 0. Lookup time is O(log(N)*len(s)
+ len(result)) where N is the size of the indexed data.

<a id="exampleIndex_Lookup"></a>
Example:

    index := suffixarray.New([]byte("banana"))
    offsets := index.Lookup([]byte("ana"), -1)
    for _, off := range offsets {
        fmt.Println(off)
    }

    // Unordered output:
    // 1
    // 3

<h3 id="Index.Read">func (*Index) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/index/suffixarray/suffixarray.go#L88">Read</a>
    <a href="#Index.Read">¶</a></h3>
<pre>func (x *<a href="#Index">Index</a>) Read(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/builtin/#error">error</a></pre>

Read reads the index from r into x; x must not be nil.

<h3 id="Index.Write">func (*Index) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/index/suffixarray/suffixarray.go#L127">Write</a>
    <a href="#Index.Write">¶</a></h3>
<pre>func (x *<a href="#Index">Index</a>) Write(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) <a href="/builtin/#error">error</a></pre>

Write writes the index x to w.


