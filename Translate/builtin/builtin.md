version: 1.10
## package builtin

  `import "builtin"`

## 概述

builtin 包为 Go 预定义的标识符提供说明。这个文档的内容其实并不在 builtin 包中，但是把它们写在这里方便 godoc 生成这些特殊标识符的文档。

## 索引

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func append(slice []Type, elems ...Type) []Type](#append)
- [func cap(v Type) int](#cap)
- [func close(c chan&lt;- Type)](#close)
- [func complex(r, i FloatType) ComplexType](#complex)
- [func copy(dst, src []Type) int](#copy)
- [func delete(m map[Type]Type1, key Type)](#delete)
- [func imag(c ComplexType) FloatType](#imag)
- [func len(v Type) int](#len)
- [func make(t Type, size ...IntegerType) Type](#make)
- [func new(Type) *Type](#new)
- [func panic(v interface{})](#panic)
- [func print(args ...Type)](#print)
- [func println(args ...Type)](#println)
- [func real(c ComplexType) FloatType](#real)
- [func recover() interface{}](#recover)
- [type ComplexType](#ComplexType)
- [type FloatType](#FloatType)
- [type IntegerType](#IntegerType)
- [type Type](#Type)
- [type Type1](#Type1)
- [type bool](#bool)
- [type byte](#byte)
- [type complex128](#complex128)
- [type complex64](#complex64)
- [type error](#error)
- [type float32](#float32)
- [type float64](#float64)
- [type int](#int)
- [type int16](#int16)
- [type int32](#int32)
- [type int64](#int64)
- [type int8](#int8)
- [type rune](#rune)
- [type string](#string)
- [type uint](#uint)
- [type uint16](#uint16)
- [type uint32](#uint32)
- [type uint64](#uint64)
- [type uint8](#uint8)
- [type uintptr](#uintptr)

### 文件

 [builtin.go](//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="true">true</span>  = 0 == 0 <span class="comment">// 无类型布尔值</span>
    <span id="false">false</span> = 0 != 0 <span class="comment">// 无类型布尔值</span>
)</pre>

true 和 false 是两个无类型布尔值。

<pre>const <span id="iota">iota</span> = 0 <span class="comment">// 无类型整数</span>
</pre>

iota 是一个预定义的标识符，它负责为当前的一组常量声明（一般在括号内）生成无类型的整数序号，序号从 0 开始。

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="nil">nil</span> <a href="#Type">Type</a> <span class="comment">// Type 必须是一个指针，channel，func，interface，map 或 slice 类型。</span>
</pre>

nil 是预定义的标识符，它可以表示指针，channel，func，interface，map 或 slice 类型的零值。

<h2 id="append">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L124">append</a>
    <a href="#append">¶</a></h2>
<pre>func append(slice []<a href="#Type">Type</a>, elems ...<a href="#Type">Type</a>) []<a href="#Type">Type</a></pre>

内置的 append 函数在切片的结尾追加元素。如果它有足够的空间，那么会直接给新元素分配空间。如果没有足够的空间，会为切片重新分配一个新的底层数组。append 函数返回操作后的切片。因此如果需要储存操作后的结果，一般会把值再赋给原来的切片：

```go
slice = append(slice, elem1, elem2)
slice = append(slice, anotherSlice...)
```

作为特例，像这样为字节切片追加一个字符串是合法的：

```go
slice = append([]byte("hello "), "world"...)
```

<h2 id="cap">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L154">cap</a>
    <a href="#cap">¶</a></h2>
<pre>func cap(v <a href="#Type">Type</a>) int</pre>

内置的 cap 函数根据 v 的类型返回它的容量：

    Array: v 中元素的数量
    Pointer to array: *v 中的元素数量
    Slice: 不用重新分配底层数组的最大长度。
    如果 v 为 nil 返回 0
    Channel: channel 缓存的容积。 
    如果 v 为 nil 返回 0

<h2 id="close">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L204">close</a>
    <a href="#close">¶</a></h2>
<pre>func close(c chan&lt;- <a href="#Type">Type</a>)</pre>

内置函数 close 用来关闭一个 channel（双向或者只写），只应该由发送端关闭 channel，接收端永远不需要关闭，接收到最后一个发送的值后会获取到关闭状态。在接收端从关闭的 channel 中接收到最后一个值以后，任何对 c 的接收操作都不会阻塞而是直接返回，返回值为 channel 元素的零值。下面格式的 ok 会在 channel 被关闭的时候被置为 false。

    x, ok := <-c

<h2 id="complex">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L185">complex</a>
    <a href="#complex">¶</a></h2>
<pre>func complex(r, i <a href="#FloatType">FloatType</a>) <a href="#ComplexType">ComplexType</a></pre>

内置函数 complex 通过两个浮点数来构造一个复数。实数和虚数部分必须为相同类型，可以是 float32 或者 float64（或者能转换成它们的类型），并且返回值是对应的虚数类型（float32 对应 complex64，float64 对应 complex128）。

<h2 id="copy">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L131">copy</a>
    <a href="#copy">¶</a></h2>
<pre>func copy(dst, src []<a href="#Type">Type</a>) int</pre>

内置函数 copy 从源切片把元素拷贝到指定切片（作为特例，它也可以从字符串把元素拷贝到切片）。源切片和指定切片可以重叠。copy 函数返回拷贝元素的数量，它是 `len(src)` 和 `len(dst)` 中的较小值。

<h2 id="delete">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L136">delete</a>
    <a href="#delete">¶</a></h2>
<pre>func delete(m map[<a href="#Type">Type</a>]<a href="#Type1">Type1</a>, key <a href="#Type">Type</a>)</pre>

内置函数 delete 删除 map 中指定索引的元素。如果 m 为 nil 或者没有这个元素，delete 什么也不做。

<h2 id="imag">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L194">imag</a>
    <a href="#imag">¶</a></h2>
<pre>func imag(c <a href="#ComplexType">ComplexType</a>) <a href="#FloatType">FloatType</a></pre>

内置函数 imag 返回复数 c 的虚数部分。返回值为 c 对应的浮点数类型。

<h2 id="len">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L145">len</a>
    <a href="#len">¶</a></h2>
<pre>func len(v <a href="#Type">Type</a>) int</pre>

内置函数 len 根据 v 的类型返回 v 的长度：

    Array: v 中元素的数量
    Pointer to array: *v 中元素的数量
    Slice, or map: v 中元素的数量，如果 v 为 nil 返回 0
    String: v 中的字节数
    Channel: channel 缓存中排队元素的数量
    如果 v 为 nil 返回 0

<h2 id="make">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L173">make</a>
    <a href="#make">¶</a></h2>
<pre>func make(t <a href="#Type">Type</a>, size ...<a href="#IntegerType">IntegerType</a>) <a href="#Type">Type</a></pre>

内置函数 make 分配并初始化 slice，map 或者 chan。和 new 类似，第一个参数为参数的类型，而不是一个值。和 new 不同的是 make 返回的类型和参数类型相同，而不是一个指向该类型的指针。具体的返回值依赖于具体类型：

    Slice: size 指定切片的长度，切片的长度和容积相同。第二个整型参数可以用来指定自定义的容积。
    它不能小于切片的长度。例如：`make([]int,0,10)` 分配一个大小为 10 的底层数组并返回一个
    基于该底层数组的长度为 0 容积为 10 的切片。
    Map: 程序会分配足够的空间给一个空的 map 来保存指定数量的元素。这时我们可以不用为其设置具体大小。
    Channel: 可以通过指定容积来初始化 channel 缓存的大小。如果为零，或者没有设置缓存大小，那么
    会返回一个没有缓存的 channel。

<h2 id="new">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L178">new</a>
    <a href="#new">¶</a></h2>
<pre>func new(<a href="#Type">Type</a>) *<a href="#Type">Type</a></pre>

内置函数 new 会分配内存。第一个参数是一个类型，而不是值，并且返回值是一个指向该类型零值的指针。

<h2 id="panic">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L217">panic</a>
    <a href="#panic">¶</a></h2>
<pre>func panic(v interface{})</pre>

内置函数 panic 停止当前执行的 goroutine。如果函数 F 调用了 panic，F 的执行会立马终止。但是通过 defer 注册的函数还会正常执行，并且 F 会返回到它的调用者。对于调用者 G，对 F 的调用行为就像调用了 panic 一样，会中断 G 的执行并执行 defer 注册的函数。就这样把所有调用链上的函数按照相反的顺序全部停止。这样程序就会以附带错误条件（panic 的参数值）的方式中断。这个中断的序列叫做恐慌并且可以通过内置函数 recover 进行控制。

<h2 id="print">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L234">print</a>
    <a href="#print">¶</a></h2>
<pre>func print(args ...<a href="#Type">Type</a>)</pre>

内置函数 print 把它的参数格式化成特定的形式然后写入标准错误输出。Print 对于初始化程序和调试十分有用；不承诺它在语言中一直存在。

<h2 id="println">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L241">println</a>
    <a href="#println">¶</a></h2>
<pre>func println(args ...<a href="#Type">Type</a>)</pre>

内置函数 println 以指定方式格式化参数并将结果写入标准错误输出。参数之间和行间一直都会有空白。

Println 对于初始化和调试十分有用。不承诺它在语言中一直存在。

<h2 id="real">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L189">real</a>
    <a href="#real">¶</a></h2>
<pre>func real(c <a href="#ComplexType">ComplexType</a>) <a href="#FloatType">FloatType</a></pre>

real 内置函数返回复数 c 的实数部分。返回值为 c 对应的浮点数类型。

<h2 id="recover">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L228">recover</a>
    <a href="#recover">¶</a></h2>
<pre>func recover() interface{}</pre>

内置函数 recover 允许程序管理一个恐慌 goroutine 的行为。在 defer 注册的函数中使用 recover（不是所有的函数都调用它）会中断恐慌序列并重新执行程序然后取回 panic 传递的错误信息。如果 recover 在 defer 注册的函数外调用，它不会中断恐慌序列。当 goroutine 没有发生恐慌或者 panic 的参数为 nil 的时候，recover 会返回 nil。因此 recover 的返回值可以用来判断是否发生了恐慌。

<h2 id="ComplexType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L113">ComplexType</a>
    <a href="#ComplexType">¶</a></h2>
<pre>type ComplexType complex64</pre>

ComplexType 类型在这里只是为了说明。它可以代表的两种复数类型（complex64 和 complex128）。

<h2 id="FloatType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L109">FloatType</a>
    <a href="#FloatType">¶</a></h2>
<pre>type FloatType float32</pre>

FloatType 类型在这里只是为了说明。它可以代表的两种浮点数类型（float32 和 float64）。

<h2 id="IntegerType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L105">IntegerType</a>
    <a href="#IntegerType">¶</a></h2>
<pre>type IntegerType int</pre>

IntegerType 类型在这里只是为了说明。它可以代表任何整型：int，uint，int8 等等。

<h2 id="Type">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L96">Type</a>
    <a href="#Type">¶</a></h2>
<pre>type Type int</pre>

Type 类型在这里只是为了说明。它代表任何的 Go 类型。但是对于任何给定的函数调用表示相同的类型。

<h2 id="Type1">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L101">Type1</a>
    <a href="#Type1">¶</a></h2>
<pre>type Type1 int</pre>

Type1 类型在这里只是为了说明。它代表任何 Go 类型。但是对于任何给定的函数调用表示相同的类型。

<h2 id="bool">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L4">bool</a>
    <a href="#bool">¶</a></h2>
<pre>type bool bool</pre>

b

<h2 id="byte">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L78">byte</a>
    <a href="#byte">¶</a></h2>
<pre>type byte = uint8</pre>

byte 是 uint8 的别名，它和 uint8 是等价的。它是用来区分字节值和 8 位无符号整数值。

<h2 id="complex128">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L56">complex128</a>
    <a href="#complex128">¶</a></h2>
<pre>type complex128 complex128</pre>

complex128 代表复数的 float64 的实数部分和 float64 的虚数部分。

<h2 id="complex64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L52">complex64</a>
    <a href="#complex64">¶</a></h2>
<pre>type complex64 complex64</pre>

complex64 代表复数的 float32 的实数部分和 float32 的虚数部分。

<h2 id="error">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L245">error</a>
    <a href="#error">¶</a></h2>
<pre>type error interface {
    Error() string
}</pre>

内置接口 error 用来简单表示错误的条件，nil 代表没有错误。

<h2 id="float32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L45">float32</a>
    <a href="#float32">¶</a></h2>
<pre>type float32 float32</pre>

float32 表示所有 IEEE-754 的 32 位浮点数。

<h2 id="float64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L48">float64</a>
    <a href="#float64">¶</a></h2>
<pre>type float64 float64</pre>

float64 表示所有 IEEE-754 的 64 位浮点数。

<h2 id="int">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L65">int</a>
    <a href="#int">¶</a></h2>
<pre>type int int</pre>

int 是有符号的整型，它最少为 32 位。它和 int32 表示不同的类型。

<h2 id="int16">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L34">int16</a>
    <a href="#int16">¶</a></h2>
<pre>type int16 int16</pre>

int16 代表所有有符号的 16 位整数。区间是 -32768 到 32767。

<h2 id="int32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L38">int32</a>
    <a href="#int32">¶</a></h2>
<pre>type int32 int32</pre>

int32 代表所有有符号的整数类型。区间是 -2147483648 到 2147483647。

<h2 id="int64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L42">int64</a>
    <a href="#int64">¶</a></h2>
<pre>type int64 int64</pre>

int64 代表所有的有符号类型的 64 位整数。区间是 -9223372036854775808 到 9223372036854775807。

<h2 id="int8">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L30">int8</a>
    <a href="#int8">¶</a></h2>
<pre>type int8 int8</pre>

int8 代表所有有符号 8 位整数。区间是 -128 到 127。

<h2 id="rune">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L82">rune</a>
    <a href="#rune">¶</a></h2>
<pre>type rune = int32</pre>

rune 是 int32 的别名，它和 int32 是等价的。它主要用来区分字符值和整型值。

<h2 id="string">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L61">string</a>
    <a href="#string">¶</a></h2>
<pre>type string string</pre>

string 表示字符串 8 位字节的集合。一般情况先都是 UTF-8 文本。字符串可能为空，但是不能为 nil。

string 的值不可改变的。

<h2 id="uint">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L69">uint</a>
    <a href="#uint">¶</a></h2>
<pre>type uint uint</pre>

uint 是无符号整型，它至少有 32 位。它和 uint32 表示不同类型。

<h2 id="uint16">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L18">uint16</a>
    <a href="#uint16">¶</a></h2>
<pre>type uint16 uint16</pre>

uint16 代表所有无符号 16 位整数。区间是 0 到 65535。

<h2 id="uint32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L22">uint32</a>
    <a href="#uint32">¶</a></h2>
<pre>type uint32 uint32</pre>

uint32 表示所有无符号 32 位 整数。区间是 0 到 4294967295。

<h2 id="uint64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L26">uint64</a>
    <a href="#uint64">¶</a></h2>
<pre>type uint64 uint64</pre>

uint64 表示所有无符号 64 位整数。区间是 0 到 18446744073709551615。

<h2 id="uint8">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L14">uint8</a>
    <a href="#uint8">¶</a></h2>
<pre>type uint8 uint8</pre>

uint8 表示所有无符号的 8 位整数。区间是 0 到 255。

<h2 id="uintptr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L73">uintptr</a>
    <a href="#uintptr">¶</a></h2>
<pre>type uintptr uintptr</pre>

uintptr 是一个整型，它的大小足够保存任何的指针。


