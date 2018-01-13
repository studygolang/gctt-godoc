version: 1.9.2
## package sort

  `import "sort"`

## Overview

Package sort provides primitives for sorting slices and user-defined
collections.

<a id="example"></a>
Example:

    package sort_test

    import (
        "fmt"
        "sort"
    )

    type Person struct {
        Name string
        Age  int
    }

    func (p Person) String() string {
        return fmt.Sprintf("%s: %d", p.Name, p.Age)
    }

    // ByAge implements sort.Interface for []Person based on
    // the Age field.
    type ByAge []Person

    func (a ByAge) Len() int           { return len(a) }
    func (a ByAge) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
    func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age }

    func Example() {
        people := []Person{
            {"Bob", 31},
            {"John", 42},
            {"Michael", 17},
            {"Jenny", 26},
        }

        fmt.Println(people)
        sort.Sort(ByAge(people))
        fmt.Println(people)

        // Output:
        // [Bob: 31 John: 42 Michael: 17 Jenny: 26]
        // [Michael: 17 Jenny: 26 Bob: 31 John: 42]
    }


<a id="example_sortKeys"></a>
Example:

    package sort_test

    import (
        "fmt"
        "sort"
    )

    // A couple of type definitions to make the units clear.
    type earthMass float64
    type au float64

    // A Planet defines the properties of a solar system object.
    type Planet struct {
        name     string
        mass     earthMass
        distance au
    }

    // By is the type of a "less" function that defines the ordering of its Planet arguments.
    type By func(p1, p2 *Planet) bool

    // Sort is a method on the function type, By, that sorts the argument slice according to the function.
    func (by By) Sort(planets []Planet) {
        ps := &planetSorter{
            planets: planets,
            by:      by, // The Sort method's receiver is the function (closure) that defines the sort order.
        }
        sort.Sort(ps)
    }

    // planetSorter joins a By function and a slice of Planets to be sorted.
    type planetSorter struct {
        planets []Planet
        by      func(p1, p2 *Planet) bool // Closure used in the Less method.
    }

    // Len is part of sort.Interface.
    func (s *planetSorter) Len() int {
        return len(s.planets)
    }

    // Swap is part of sort.Interface.
    func (s *planetSorter) Swap(i, j int) {
        s.planets[i], s.planets[j] = s.planets[j], s.planets[i]
    }

    // Less is part of sort.Interface. It is implemented by calling the "by" closure in the sorter.
    func (s *planetSorter) Less(i, j int) bool {
        return s.by(&s.planets[i], &s.planets[j])
    }

    var planets = []Planet{
        {"Mercury", 0.055, 0.4},
        {"Venus", 0.815, 0.7},
        {"Earth", 1.0, 1.0},
        {"Mars", 0.107, 1.5},
    }

    // ExampleSortKeys demonstrates a technique for sorting a struct type using programmable sort criteria.
    func Example_sortKeys() {
        // Closures that order the Planet structure.
        name := func(p1, p2 *Planet) bool {
            return p1.name < p2.name
        }
        mass := func(p1, p2 *Planet) bool {
            return p1.mass < p2.mass
        }
        distance := func(p1, p2 *Planet) bool {
            return p1.distance < p2.distance
        }
        decreasingDistance := func(p1, p2 *Planet) bool {
            return !distance(p1, p2)
        }

        // Sort the planets by the various criteria.
        By(name).Sort(planets)
        fmt.Println("By name:", planets)

        By(mass).Sort(planets)
        fmt.Println("By mass:", planets)

        By(distance).Sort(planets)
        fmt.Println("By distance:", planets)

        By(decreasingDistance).Sort(planets)
        fmt.Println("By decreasing distance:", planets)

        // Output: By name: [{Earth 1 1} {Mars 0.107 1.5} {Mercury 0.055 0.4} {Venus 0.815 0.7}]
        // By mass: [{Mercury 0.055 0.4} {Mars 0.107 1.5} {Venus 0.815 0.7} {Earth 1 1}]
        // By distance: [{Mercury 0.055 0.4} {Venus 0.815 0.7} {Earth 1 1} {Mars 0.107 1.5}]
        // By decreasing distance: [{Mars 0.107 1.5} {Earth 1 1} {Venus 0.815 0.7} {Mercury 0.055 0.4}]
    }


<a id="example_sortMultiKeys"></a>
Example:

    package sort_test

    import (
        "fmt"
        "sort"
    )

    // A Change is a record of source code changes, recording user, language, and delta size.
    type Change struct {
        user     string
        language string
        lines    int
    }

    type lessFunc func(p1, p2 *Change) bool

    // multiSorter implements the Sort interface, sorting the changes within.
    type multiSorter struct {
        changes []Change
        less    []lessFunc
    }

    // Sort sorts the argument slice according to the less functions passed to OrderedBy.
    func (ms *multiSorter) Sort(changes []Change) {
        ms.changes = changes
        sort.Sort(ms)
    }

    // OrderedBy returns a Sorter that sorts using the less functions, in order.
    // Call its Sort method to sort the data.
    func OrderedBy(less ...lessFunc) *multiSorter {
        return &multiSorter{
            less: less,
        }
    }

    // Len is part of sort.Interface.
    func (ms *multiSorter) Len() int {
        return len(ms.changes)
    }

    // Swap is part of sort.Interface.
    func (ms *multiSorter) Swap(i, j int) {
        ms.changes[i], ms.changes[j] = ms.changes[j], ms.changes[i]
    }

    // Less is part of sort.Interface. It is implemented by looping along the
    // less functions until it finds a comparison that is either Less or
    // !Less. Note that it can call the less functions twice per call. We
    // could change the functions to return -1, 0, 1 and reduce the
    // number of calls for greater efficiency: an exercise for the reader.
    func (ms *multiSorter) Less(i, j int) bool {
        p, q := &ms.changes[i], &ms.changes[j]
        // Try all but the last comparison.
        var k int
        for k = 0; k < len(ms.less)-1; k++ {
            less := ms.less[k]
            switch {
            case less(p, q):
                // p < q, so we have a decision.
                return true
            case less(q, p):
                // p > q, so we have a decision.
                return false
            }
            // p == q; try the next comparison.
        }
        // All comparisons to here said "equal", so just return whatever
        // the final comparison reports.
        return ms.less[k](p, q)
    }

    var changes = []Change{
        {"gri", "Go", 100},
        {"ken", "C", 150},
        {"glenda", "Go", 200},
        {"rsc", "Go", 200},
        {"r", "Go", 100},
        {"ken", "Go", 200},
        {"dmr", "C", 100},
        {"r", "C", 150},
        {"gri", "Smalltalk", 80},
    }

    // ExampleMultiKeys demonstrates a technique for sorting a struct type using different
    // sets of multiple fields in the comparison. We chain together "Less" functions, each of
    // which compares a single field.
    func Example_sortMultiKeys() {
        // Closures that order the Change structure.
        user := func(c1, c2 *Change) bool {
            return c1.user < c2.user
        }
        language := func(c1, c2 *Change) bool {
            return c1.language < c2.language
        }
        increasingLines := func(c1, c2 *Change) bool {
            return c1.lines < c2.lines
        }
        decreasingLines := func(c1, c2 *Change) bool {
            return c1.lines > c2.lines // Note: > orders downwards.
        }

        // Simple use: Sort by user.
        OrderedBy(user).Sort(changes)
        fmt.Println("By user:", changes)

        // More examples.
        OrderedBy(user, increasingLines).Sort(changes)
        fmt.Println("By user,<lines:", changes)

        OrderedBy(user, decreasingLines).Sort(changes)
        fmt.Println("By user,>lines:", changes)

        OrderedBy(language, increasingLines).Sort(changes)
        fmt.Println("By language,<lines:", changes)

        OrderedBy(language, increasingLines, user).Sort(changes)
        fmt.Println("By language,<lines,user:", changes)

        // Output:
        // By user: [{dmr C 100} {glenda Go 200} {gri Go 100} {gri Smalltalk 80} {ken C 150} {ken Go 200} {r Go 100} {r C 150} {rsc Go 200}]
        // By user,<lines: [{dmr C 100} {glenda Go 200} {gri Smalltalk 80} {gri Go 100} {ken C 150} {ken Go 200} {r Go 100} {r C 150} {rsc Go 200}]
        // By user,>lines: [{dmr C 100} {glenda Go 200} {gri Go 100} {gri Smalltalk 80} {ken Go 200} {ken C 150} {r C 150} {r Go 100} {rsc Go 200}]
        // By language,<lines: [{dmr C 100} {ken C 150} {r C 150} {r Go 100} {gri Go 100} {ken Go 200} {glenda Go 200} {rsc Go 200} {gri Smalltalk 80}]
        // By language,<lines,user: [{dmr C 100} {ken C 150} {r C 150} {gri Go 100} {r Go 100} {glenda Go 200} {ken Go 200} {rsc Go 200} {gri Smalltalk 80}]

    }


<a id="example_sortWrapper"></a>
Example:

    package sort_test

    import (
        "fmt"
        "sort"
    )

    type Grams int

    func (g Grams) String() string { return fmt.Sprintf("%dg", int(g)) }

    type Organ struct {
        Name   string
        Weight Grams
    }

    type Organs []*Organ

    func (s Organs) Len() int      { return len(s) }
    func (s Organs) Swap(i, j int) { s[i], s[j] = s[j], s[i] }

    // ByName implements sort.Interface by providing Less and using the Len and
    // Swap methods of the embedded Organs value.
    type ByName struct{ Organs }

    func (s ByName) Less(i, j int) bool { return s.Organs[i].Name < s.Organs[j].Name }

    // ByWeight implements sort.Interface by providing Less and using the Len and
    // Swap methods of the embedded Organs value.
    type ByWeight struct{ Organs }

    func (s ByWeight) Less(i, j int) bool { return s.Organs[i].Weight < s.Organs[j].Weight }

    func Example_sortWrapper() {
        s := []*Organ{
            {"brain", 1340},
            {"heart", 290},
            {"liver", 1494},
            {"pancreas", 131},
            {"prostate", 62},
            {"spleen", 162},
        }

        sort.Sort(ByWeight{s})
        fmt.Println("Organs by weight:")
        printOrgans(s)

        sort.Sort(ByName{s})
        fmt.Println("Organs by name:")
        printOrgans(s)

        // Output:
        // Organs by weight:
        // prostate (62g)
        // pancreas (131g)
        // spleen   (162g)
        // heart    (290g)
        // brain    (1340g)
        // liver    (1494g)
        // Organs by name:
        // brain    (1340g)
        // heart    (290g)
        // liver    (1494g)
        // pancreas (131g)
        // prostate (62g)
        // spleen   (162g)
    }

    func printOrgans(s []*Organ) {
        for _, o := range s {
            fmt.Printf("%-8s (%v)\n", o.Name, o.Weight)
        }
    }

## Index

- [func Float64s(a []float64)](#Float64s)
- [func Float64sAreSorted(a []float64) bool](#Float64sAreSorted)
- [func Ints(a []int)](#Ints)
- [func IntsAreSorted(a []int) bool](#IntsAreSorted)
- [func IsSorted(data Interface) bool](#IsSorted)
- [func Search(n int, f func(int) bool) int](#Search)
- [func SearchFloat64s(a []float64, x float64) int](#SearchFloat64s)
- [func SearchInts(a []int, x int) int](#SearchInts)
- [func SearchStrings(a []string, x string) int](#SearchStrings)
- [func Slice(slice interface{}, less func(i, j int) bool)](#Slice)
- [func SliceIsSorted(slice interface{}, less func(i, j int) bool) bool](#SliceIsSorted)
- [func SliceStable(slice interface{}, less func(i, j int) bool)](#SliceStable)
- [func Sort(data Interface)](#Sort)
- [func Stable(data Interface)](#Stable)
- [func Strings(a []string)](#Strings)
- [func StringsAreSorted(a []string) bool](#StringsAreSorted)
- [type Float64Slice](#Float64Slice)
  - [func (p Float64Slice) Len() int](#Float64Slice.Len)
  - [func (p Float64Slice) Less(i, j int) bool](#Float64Slice.Less)
  - [func (p Float64Slice) Search(x float64) int](#Float64Slice.Search)
  - [func (p Float64Slice) Sort()](#Float64Slice.Sort)
  - [func (p Float64Slice) Swap(i, j int)](#Float64Slice.Swap)
- [type IntSlice](#IntSlice)
  - [func (p IntSlice) Len() int](#IntSlice.Len)
  - [func (p IntSlice) Less(i, j int) bool](#IntSlice.Less)
  - [func (p IntSlice) Search(x int) int](#IntSlice.Search)
  - [func (p IntSlice) Sort()](#IntSlice.Sort)
  - [func (p IntSlice) Swap(i, j int)](#IntSlice.Swap)
- [type Interface](#Interface)
  - [func Reverse(data Interface) Interface](#Reverse)
- [type StringSlice](#StringSlice)
  - [func (p StringSlice) Len() int](#StringSlice.Len)
  - [func (p StringSlice) Less(i, j int) bool](#StringSlice.Less)
  - [func (p StringSlice) Search(x string) int](#StringSlice.Search)
  - [func (p StringSlice) Sort()](#StringSlice.Sort)
  - [func (p StringSlice) Swap(i, j int)](#StringSlice.Swap)

### Examples

- [Package](#example)
- [Ints](#exampleInts)
- [Reverse](#exampleReverse)
- [Search](#exampleSearch)
- [Search (DescendingOrder)](#exampleSearch_descendingOrder)
- [Slice](#exampleSlice)
- [SliceStable](#exampleSliceStable)
- [Strings](#exampleStrings)
- [Package (SortKeys)](#example_sortKeys)
- [Package (SortMultiKeys)](#example_sortMultiKeys)
- [Package (SortWrapper)](#example_sortWrapper)

### Package files
 [search.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/search.go) [sort.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go) [zfuncversion.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/zfuncversion.go)

<h2 id="Float64s">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L340">Float64s</a>
    <a href="#Float64s">¶</a></h2>
<pre>func Float64s(a []<a href="/builtin/#float64">float64</a>)</pre>

Float64s sorts a slice of float64s in increasing order (not-a-number values are
treated as less than other values).

<h2 id="Float64sAreSorted">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L350">Float64sAreSorted</a>
    <a href="#Float64sAreSorted">¶</a></h2>
<pre>func Float64sAreSorted(a []<a href="/builtin/#float64">float64</a>) <a href="/builtin/#bool">bool</a></pre>

Float64sAreSorted tests whether a slice of float64s is sorted in increasing
order (not-a-number values are treated as less than other values).

<h2 id="Ints">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L336">Ints</a>
    <a href="#Ints">¶</a></h2>
<pre>func Ints(a []<a href="/builtin/#int">int</a>)</pre>

Ints sorts a slice of ints in increasing order.

<a id="exampleInts"></a>
Example:

    s := []int{5, 2, 6, 3, 1, 4} // unsorted
    sort.Ints(s)
    fmt.Println(s)
    // Output: [1 2 3 4 5 6]

<h2 id="IntsAreSorted">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L346">IntsAreSorted</a>
    <a href="#IntsAreSorted">¶</a></h2>
<pre>func IntsAreSorted(a []<a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>

IntsAreSorted tests whether a slice of ints is sorted in increasing order.

<h2 id="IsSorted">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L285">IsSorted</a>
    <a href="#IsSorted">¶</a></h2>
<pre>func IsSorted(data <a href="#Interface">Interface</a>) <a href="/builtin/#bool">bool</a></pre>

IsSorted reports whether data is sorted.

<h2 id="Search">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/search.go#L49">Search</a>
    <a href="#Search">¶</a></h2>
<pre>func Search(n <a href="/builtin/#int">int</a>, f func(<a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#int">int</a></pre>

Search uses binary search to find and return the smallest index i in [0, n) at
which f(i) is true, assuming that on the range [0, n), f(i) == true implies
f(i+1) == true. That is, Search requires that f is false for some (possibly
empty) prefix of the input range [0, n) and then true for the (possibly empty)
remainder; Search returns the first true index. If there is no such index,
Search returns n. (Note that the "not found" return value is not -1 as in, for
instance, strings.Index.) Search calls f(i) only for i in the range [0, n).

A common use of Search is to find the index i for a value x in a sorted,
indexable data structure such as an array or slice. In this case, the argument
f, typically a closure, captures the value to be searched for, and how the data
structure is indexed and ordered.

For instance, given a slice data sorted in ascending order, the call
Search(len(data), func(i int) bool { return data[i] >= 23 }) returns the
smallest index i such that data[i] >= 23. If the caller wants to find whether 23
is in the slice, it must test data[i] == 23 separately.

Searching data sorted in descending order would use the <= operator instead of
the >= operator.

To complete the example above, the following code tries to find the value x in
an integer slice data sorted in ascending order:

    x := 23
    i := sort.Search(len(data), func(i int) bool { return data[i] >= x })
    if i < len(data) && data[i] == x {
    	// x is present at data[i]
    } else {
    	// x is not present in data,
    	// but i is the index where it would be inserted.
    }

As a more whimsical example, this program guesses your number:

    func GuessingGame() {
    	var s string
    	fmt.Printf("Pick an integer from 0 to 100.\n")
    	answer := sort.Search(100, func(i int) bool {
    		fmt.Printf("Is your number <= %d? ", i)
    		fmt.Scanf("%s", &s)
    		return s != "" && s[0] == 'y'
    	})
    	fmt.Printf("Your number is %d.\n", answer)
    }

<a id="exampleSearch"></a>
Example:

    a := []int{1, 3, 6, 10, 15, 21, 28, 36, 45, 55}
    x := 6

    i := sort.Search(len(a), func(i int) bool { return a[i] >= x })
    if i < len(a) && a[i] == x {
        fmt.Printf("found %d at index %d in %v\n", x, i, a)
    } else {
        fmt.Printf("%d not found in %v\n", x, a)
    }
    // Output:
    // found 6 at index 2 in [1 3 6 10 15 21 28 36 45 55]


<a id="exampleSearch_descendingOrder"></a>
Example:

    a := []int{55, 45, 36, 28, 21, 15, 10, 6, 3, 1}
    x := 6

    i := sort.Search(len(a), func(i int) bool { return a[i] <= x })
    if i < len(a) && a[i] == x {
        fmt.Printf("found %d at index %d in %v\n", x, i, a)
    } else {
        fmt.Printf("%d not found in %v\n", x, a)
    }
    // Output:
    // found 6 at index 7 in [55 45 36 28 21 15 10 6 3 1]

<h2 id="SearchFloat64s">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/search.go#L82">SearchFloat64s</a>
    <a href="#SearchFloat64s">¶</a></h2>
<pre>func SearchFloat64s(a []<a href="/builtin/#float64">float64</a>, x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#int">int</a></pre>

SearchFloat64s searches for x in a sorted slice of float64s and returns the
index as specified by Search. The return value is the index to insert x if x is
not present (it could be len(a)). The slice must be sorted in ascending order.

<h2 id="SearchInts">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/search.go#L73">SearchInts</a>
    <a href="#SearchInts">¶</a></h2>
<pre>func SearchInts(a []<a href="/builtin/#int">int</a>, x <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

SearchInts searches for x in a sorted slice of ints and returns the index as
specified by Search. The return value is the index to insert x if x is not
present (it could be len(a)). The slice must be sorted in ascending order.

<h2 id="SearchStrings">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/search.go#L91">SearchStrings</a>
    <a href="#SearchStrings">¶</a></h2>
<pre>func SearchStrings(a []<a href="/builtin/#string">string</a>, x <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

SearchStrings searches for x in a sorted slice of strings and returns the index
as specified by Search. The return value is the index to insert x if x is not
present (it could be len(a)). The slice must be sorted in ascending order.

<h2 id="Slice">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L237">Slice</a>
    <a href="#Slice">¶</a></h2>
<pre>func Slice(slice interface{}, less func(i, j <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a>)</pre>

Slice sorts the provided slice given the provided less function.

The sort is not guaranteed to be stable. For a stable sort, use SliceStable.

The function panics if the provided interface is not a slice.

<a id="exampleSlice"></a>
Example:

    people := []struct {
        Name string
        Age  int
    }{
        {"Gopher", 7},
        {"Alice", 55},
        {"Vera", 24},
        {"Bob", 75},
    }
    sort.Slice(people, func(i, j int) bool { return people[i].Name < people[j].Name })
    fmt.Println("By name:", people)

    sort.Slice(people, func(i, j int) bool { return people[i].Age < people[j].Age })
    fmt.Println("By age:", people)
    // Output: By name: [{Alice 55} {Bob 75} {Gopher 7} {Vera 24}]
    // By age: [{Gopher 7} {Vera 24} {Alice 55} {Bob 75}]

<h2 id="SliceIsSorted">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L257">SliceIsSorted</a>
    <a href="#SliceIsSorted">¶</a></h2>
<pre>func SliceIsSorted(slice interface{}, less func(i, j <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a>) <a href="/builtin/#bool">bool</a></pre>

SliceIsSorted tests whether a slice is sorted.

The function panics if the provided interface is not a slice.

<h2 id="SliceStable">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L248">SliceStable</a>
    <a href="#SliceStable">¶</a></h2>
<pre>func SliceStable(slice interface{}, less func(i, j <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a>)</pre>

SliceStable sorts the provided slice given the provided less function while
keeping the original order of equal elements.

The function panics if the provided interface is not a slice.

<a id="exampleSliceStable"></a>
Example:

    people := []struct {
        Name string
        Age  int
    }{
        {"Alice", 25},
        {"Elizabeth", 75},
        {"Alice", 75},
        {"Bob", 75},
        {"Alice", 75},
        {"Bob", 25},
        {"Colin", 25},
        {"Elizabeth", 25},
    }

    // Sort by name, preserving original order
    sort.SliceStable(people, func(i, j int) bool { return people[i].Name < people[j].Name })
    fmt.Println("By name:", people)

    // Sort by age preserving name order
    sort.SliceStable(people, func(i, j int) bool { return people[i].Age < people[j].Age })
    fmt.Println("By age,name:", people)

    // Output: By name: [{Alice 25} {Alice 75} {Alice 75} {Bob 75} {Bob 25} {Colin 25} {Elizabeth 75} {Elizabeth 25}]
    // By age,name: [{Alice 25} {Bob 25} {Colin 25} {Elizabeth 25} {Alice 75} {Alice 75} {Bob 75} {Elizabeth 75}]

<h2 id="Sort">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L208">Sort</a>
    <a href="#Sort">¶</a></h2>
<pre>func Sort(data <a href="#Interface">Interface</a>)</pre>

Sort sorts data. It makes one call to data.Len to determine n, and O(n*log(n))
calls to data.Less and data.Swap. The sort is not guaranteed to be stable.

<h2 id="Stable">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L385">Stable</a>
    <a href="#Stable">¶</a></h2>
<pre>func Stable(data <a href="#Interface">Interface</a>)</pre>

Stable sorts data while keeping the original order of equal elements.

It makes one call to data.Len to determine n, O(n*log(n)) calls to data.Less and
O(n*log(n)*log(n)) calls to data.Swap.

<h2 id="Strings">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L343">Strings</a>
    <a href="#Strings">¶</a></h2>
<pre>func Strings(a []<a href="/builtin/#string">string</a>)</pre>

Strings sorts a slice of strings in increasing order.

<a id="exampleStrings"></a>
Example:

    s := []string{"Go", "Bravo", "Gopher", "Alpha", "Grin", "Delta"}
    sort.Strings(s)
    fmt.Println(s)
    // Output: [Alpha Bravo Delta Go Gopher Grin]

<h2 id="StringsAreSorted">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L353">StringsAreSorted</a>
    <a href="#StringsAreSorted">¶</a></h2>
<pre>func StringsAreSorted(a []<a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

StringsAreSorted tests whether a slice of strings is sorted in increasing order.

<h2 id="Float64Slice">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L309">Float64Slice</a>
    <a href="#Float64Slice">¶</a></h2>
<pre>type Float64Slice []<a href="/builtin/#float64">float64</a></pre>

Float64Slice attaches the methods of Interface to []float64, sorting in
increasing order (not-a-number values are treated as less than other values).

<h3 id="Float64Slice.Len">func (Float64Slice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L311">Len</a>
    <a href="#Float64Slice.Len">¶</a></h3>
<pre>func (p <a href="#Float64Slice">Float64Slice</a>) Len() <a href="/builtin/#int">int</a></pre>


<h3 id="Float64Slice.Less">func (Float64Slice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L312">Less</a>
    <a href="#Float64Slice.Less">¶</a></h3>
<pre>func (p <a href="#Float64Slice">Float64Slice</a>) Less(i, j <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>


<h3 id="Float64Slice.Search">func (Float64Slice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/search.go#L99">Search</a>
    <a href="#Float64Slice.Search">¶</a></h3>
<pre>func (p <a href="#Float64Slice">Float64Slice</a>) Search(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#int">int</a></pre>

Search returns the result of applying SearchFloat64s to the receiver and x.

<h3 id="Float64Slice.Sort">func (Float64Slice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L321">Sort</a>
    <a href="#Float64Slice.Sort">¶</a></h3>
<pre>func (p <a href="#Float64Slice">Float64Slice</a>) Sort()</pre>

Sort is a convenience method.

<h3 id="Float64Slice.Swap">func (Float64Slice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L313">Swap</a>
    <a href="#Float64Slice.Swap">¶</a></h3>
<pre>func (p <a href="#Float64Slice">Float64Slice</a>) Swap(i, j <a href="/builtin/#int">int</a>)</pre>


<h2 id="IntSlice">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L298">IntSlice</a>
    <a href="#IntSlice">¶</a></h2>
<pre>type IntSlice []<a href="/builtin/#int">int</a></pre>

IntSlice attaches the methods of Interface to []int, sorting in increasing
order.

<h3 id="IntSlice.Len">func (IntSlice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L300">Len</a>
    <a href="#IntSlice.Len">¶</a></h3>
<pre>func (p <a href="#IntSlice">IntSlice</a>) Len() <a href="/builtin/#int">int</a></pre>


<h3 id="IntSlice.Less">func (IntSlice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L301">Less</a>
    <a href="#IntSlice.Less">¶</a></h3>
<pre>func (p <a href="#IntSlice">IntSlice</a>) Less(i, j <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>


<h3 id="IntSlice.Search">func (IntSlice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/search.go#L96">Search</a>
    <a href="#IntSlice.Search">¶</a></h3>
<pre>func (p <a href="#IntSlice">IntSlice</a>) Search(x <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

Search returns the result of applying SearchInts to the receiver and x.

<h3 id="IntSlice.Sort">func (IntSlice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L305">Sort</a>
    <a href="#IntSlice.Sort">¶</a></h3>
<pre>func (p <a href="#IntSlice">IntSlice</a>) Sort()</pre>

Sort is a convenience method.

<h3 id="IntSlice.Swap">func (IntSlice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L302">Swap</a>
    <a href="#IntSlice.Swap">¶</a></h3>
<pre>func (p <a href="#IntSlice">IntSlice</a>) Swap(i, j <a href="/builtin/#int">int</a>)</pre>


<h2 id="Interface">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L6">Interface</a>
    <a href="#Interface">¶</a></h2>
<pre>type Interface interface {
    <span class="comment">// Len is the number of elements in the collection.</span>
    Len() <a href="/builtin/#int">int</a>
    <span class="comment">// Less reports whether the element with</span>
    <span class="comment">// index i should sort before the element with index j.</span>
    Less(i, j <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a>
    <span class="comment">// Swap swaps the elements with indexes i and j.</span>
    Swap(i, j <a href="/builtin/#int">int</a>)
}</pre>

A type, typically a collection, that satisfies sort.Interface can be sorted by
the routines in this package. The methods require that the elements of the
collection be enumerated by an integer index.

<h3 id="Reverse">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L280">Reverse</a>
    <a href="#Reverse">¶</a></h3>
<pre>func Reverse(data <a href="#Interface">Interface</a>) <a href="#Interface">Interface</a></pre>

Reverse returns the reverse order for data.

<a id="exampleReverse"></a>
Example:

    s := []int{5, 2, 6, 3, 1, 4} // unsorted
    sort.Sort(sort.Reverse(sort.IntSlice(s)))
    fmt.Println(s)
    // Output: [6 5 4 3 2 1]

<h2 id="StringSlice">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L324">StringSlice</a>
    <a href="#StringSlice">¶</a></h2>
<pre>type StringSlice []<a href="/builtin/#string">string</a></pre>

StringSlice attaches the methods of Interface to []string, sorting in increasing
order.

<h3 id="StringSlice.Len">func (StringSlice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L326">Len</a>
    <a href="#StringSlice.Len">¶</a></h3>
<pre>func (p <a href="#StringSlice">StringSlice</a>) Len() <a href="/builtin/#int">int</a></pre>


<h3 id="StringSlice.Less">func (StringSlice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L327">Less</a>
    <a href="#StringSlice.Less">¶</a></h3>
<pre>func (p <a href="#StringSlice">StringSlice</a>) Less(i, j <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a></pre>


<h3 id="StringSlice.Search">func (StringSlice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/search.go#L102">Search</a>
    <a href="#StringSlice.Search">¶</a></h3>
<pre>func (p <a href="#StringSlice">StringSlice</a>) Search(x <a href="/builtin/#string">string</a>) <a href="/builtin/#int">int</a></pre>

Search returns the result of applying SearchStrings to the receiver and x.

<h3 id="StringSlice.Sort">func (StringSlice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L331">Sort</a>
    <a href="#StringSlice.Sort">¶</a></h3>
<pre>func (p <a href="#StringSlice">StringSlice</a>) Sort()</pre>

Sort is a convenience method.

<h3 id="StringSlice.Swap">func (StringSlice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/sort/sort.go#L328">Swap</a>
    <a href="#StringSlice.Swap">¶</a></h3>
<pre>func (p <a href="#StringSlice">StringSlice</a>) Swap(i, j <a href="/builtin/#int">int</a>)</pre>



