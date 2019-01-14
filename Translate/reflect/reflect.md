version: 1.10
## package reflect

  `import "reflect"`

## 前言
Reflect包实现了GO的运行时映射机制, 使Go程序可以操作动态类型的对象. 常用的使用方式是用 interface{} 来定义一个动态类型的变量,然后通过调用Typeof方法来获取该变量的类型信息. Typeof 方法会直接返回该变量的类型

调用ValueOf方法会返回运行时数据的值.Zero方法接受reflect.Type参数,并返回该类型的zero值（初始值）


参见 下面的 “反射的规则” 文章, 来了解GO语言的反射机制.
https://golang.org/doc/articles/laws_of_reflection.html

## 目录

- [func Copy(dst, src Value) int](#Copy)
- [func DeepEqual(x, y interface{}) bool](#DeepEqual)
- [func Select(cases []SelectCase) (chosen int, recv Value, recvOK bool)](#Select)
- [func Swapper(slice interface{}) func(i, j int)](#Swapper)
- [type ChanDir](#ChanDir)
  - [func (d ChanDir) String() string](#ChanDir.String)
- [type Kind](#Kind)
  - [func (k Kind) String() string](#Kind.String)
- [type Method](#Method)
- [type SelectCase](#SelectCase)
- [type SelectDir](#SelectDir)
- [type SliceHeader](#SliceHeader)
- [type StringHeader](#StringHeader)
- [type StructField](#StructField)
- [type StructTag](#StructTag)
  - [func (tag StructTag) Get(key string) string](#StructTag.Get)
  - [func (tag StructTag) Lookup(key string) (value string, ok bool)](#StructTag.Lookup)
- [type Type](#Type)
  - [func ArrayOf(count int, elem Type) Type](#ArrayOf)
  - [func ChanOf(dir ChanDir, t Type) Type](#ChanOf)
  - [func FuncOf(in, out []Type, variadic bool) Type](#FuncOf)
  - [func MapOf(key, elem Type) Type](#MapOf)
  - [func PtrTo(t Type) Type](#PtrTo)
  - [func SliceOf(t Type) Type](#SliceOf)
  - [func StructOf(fields []StructField) Type](#StructOf)
  - [func TypeOf(i interface{}) Type](#TypeOf)
- [type Value](#Value)
  - [func Append(s Value, x ...Value) Value](#Append)
  - [func AppendSlice(s, t Value) Value](#AppendSlice)
  - [func Indirect(v Value) Value](#Indirect)
  - [func MakeChan(typ Type, buffer int) Value](#MakeChan)
  - [func MakeFunc(typ Type, fn func(args []Value) (results []Value)) Value](#MakeFunc)
  - [func MakeMap(typ Type) Value](#MakeMap)
  - [func MakeMapWithSize(typ Type, n int) Value](#MakeMapWithSize)
  - [func MakeSlice(typ Type, len, cap int) Value](#MakeSlice)
  - [func New(typ Type) Value](#New)
  - [func NewAt(typ Type, p unsafe.Pointer) Value](#NewAt)
  - [func ValueOf(i interface{}) Value](#ValueOf)
  - [func Zero(typ Type) Value](#Zero)
  - [func (v Value) Addr() Value](#Value.Addr)
  - [func (v Value) Bool() bool](#Value.Bool)
  - [func (v Value) Bytes() []byte](#Value.Bytes)
  - [func (v Value) Call(in []Value) []Value](#Value.Call)
  - [func (v Value) CallSlice(in []Value) []Value](#Value.CallSlice)
  - [func (v Value) CanAddr() bool](#Value.CanAddr)
  - [func (v Value) CanInterface() bool](#Value.CanInterface)
  - [func (v Value) CanSet() bool](#Value.CanSet)
  - [func (v Value) Cap() int](#Value.Cap)
  - [func (v Value) Close()](#Value.Close)
  - [func (v Value) Complex() complex128](#Value.Complex)
  - [func (v Value) Convert(t Type) Value](#Value.Convert)
  - [func (v Value) Elem() Value](#Value.Elem)
  - [func (v Value) Field(i int) Value](#Value.Field)
  - [func (v Value) FieldByIndex(index []int) Value](#Value.FieldByIndex)
  - [func (v Value) FieldByName(name string) Value](#Value.FieldByName)
  - [func (v Value) FieldByNameFunc(match func(string) bool) Value](#Value.FieldByNameFunc)
  - [func (v Value) Float() float64](#Value.Float)
  - [func (v Value) Index(i int) Value](#Value.Index)
  - [func (v Value) Int() int64](#Value.Int)
  - [func (v Value) Interface() (i interface{})](#Value.Interface)
  - [func (v Value) InterfaceData() [2]uintptr](#Value.InterfaceData)
  - [func (v Value) IsNil() bool](#Value.IsNil)
  - [func (v Value) IsValid() bool](#Value.IsValid)
  - [func (v Value) Kind() Kind](#Value.Kind)
  - [func (v Value) Len() int](#Value.Len)
  - [func (v Value) MapIndex(key Value) Value](#Value.MapIndex)
  - [func (v Value) MapKeys() []Value](#Value.MapKeys)
  - [func (v Value) Method(i int) Value](#Value.Method)
  - [func (v Value) MethodByName(name string) Value](#Value.MethodByName)
  - [func (v Value) NumField() int](#Value.NumField)
  - [func (v Value) NumMethod() int](#Value.NumMethod)
  - [func (v Value) OverflowComplex(x complex128) bool](#Value.OverflowComplex)
  - [func (v Value) OverflowFloat(x float64) bool](#Value.OverflowFloat)
  - [func (v Value) OverflowInt(x int64) bool](#Value.OverflowInt)
  - [func (v Value) OverflowUint(x uint64) bool](#Value.OverflowUint)
  - [func (v Value) Pointer() uintptr](#Value.Pointer)
  - [func (v Value) Recv() (x Value, ok bool)](#Value.Recv)
  - [func (v Value) Send(x Value)](#Value.Send)
  - [func (v Value) Set(x Value)](#Value.Set)
  - [func (v Value) SetBool(x bool)](#Value.SetBool)
  - [func (v Value) SetBytes(x []byte)](#Value.SetBytes)
  - [func (v Value) SetCap(n int)](#Value.SetCap)
  - [func (v Value) SetComplex(x complex128)](#Value.SetComplex)
  - [func (v Value) SetFloat(x float64)](#Value.SetFloat)
  - [func (v Value) SetInt(x int64)](#Value.SetInt)
  - [func (v Value) SetLen(n int)](#Value.SetLen)
  - [func (v Value) SetMapIndex(key, val Value)](#Value.SetMapIndex)
  - [func (v Value) SetPointer(x unsafe.Pointer)](#Value.SetPointer)
  - [func (v Value) SetString(x string)](#Value.SetString)
  - [func (v Value) SetUint(x uint64)](#Value.SetUint)
  - [func (v Value) Slice(i, j int) Value](#Value.Slice)
  - [func (v Value) Slice3(i, j, k int) Value](#Value.Slice3)
  - [func (v Value) String() string](#Value.String)
  - [func (v Value) TryRecv() (x Value, ok bool)](#Value.TryRecv)
  - [func (v Value) TrySend(x Value) bool](#Value.TrySend)
  - [func (v Value) Type() Type](#Value.Type)
  - [func (v Value) Uint() uint64](#Value.Uint)
  - [func (v Value) UnsafeAddr() uintptr](#Value.UnsafeAddr)
- [type ValueError](#ValueError)
  - [func (e *ValueError) Error() string](#ValueError.Error)
- [Bugs](#pkg-note-BUG)

### 示例

- [MakeFunc](#exampleMakeFunc)
- [StructOf](#exampleStructOf)
- [StructTag](#exampleStructTag)
- [StructTag.Lookup](#exampleStructTag_Lookup)
- [TypeOf](#exampleTypeOf)

### 包文件
 [deepequal.go](//github.com/golang/go/blob/release-branch.go1.10/src/reflect/deepequal.go) [makefunc.go](//github.com/golang/go/blob/release-branch.go1.10/src/reflect/makefunc.go) [swapper.go](//github.com/golang/go/blob/release-branch.go1.10/src/reflect/swapper.go) [type.go](//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go) [value.go](//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go)

<h2 id="Copy">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1877">Copy</a>
    <a href="#Copy">¶</a></h2>
<pre>func Copy(dst, src <a href="#Value">Value</a>) <a href="/builtin/#int">int</a></pre>
Cope方法复制src下的内容到dst,直到dst被填满或者src已经没有内容.它返回被复制的元素数目.dst和src必须是同一类型,而且必须是切片或者数组类型中的一个.
只有一个特殊情况, 当dst是Uint8类型时,src可以是String类型

<h2 id="DeepEqual">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/deepequal.go#L177">DeepEqual</a>
    <a href="#DeepEqual">¶</a></h2>
<pre>func DeepEqual(x, y interface{}) <a href="/builtin/#bool">bool</a></pre>
DeepEqual 可以检测 x,y 是否“绝对相等”
不同类型的值永远不会绝对相等,当以下情况出现时,认为两个值绝对相等

Array(数组)在每个对应的元素都相等时为绝对相等

Struct(结构体)在每一字段无论是导出还是非导出的情况下都相等,视为绝对相等

Func(函数)只有在都是nil的情况下为绝对相等

Interface(接口)在其所包含的具象值绝对相等时,为绝对相等

Map(映射)在满足下列所有条件时为绝对相等: 1.它们都为nil或者都不为nil 2.它们有相同的长度
(length) 3. 它们是同一个映射对象或者相对应的Key(Go equality匹配)都指向绝对相等的值

Pointer(指针)在指向同一对象(==操作符为true)或者指向绝对相等的值时,为绝对相等

Slice(切片)在满足下列所有条件时为绝对相等: 1.它们都为nil或者都不为nil 2.它们有相同的长度
3.它们都指向原始未经剪切的数组(&x[0] == &y[0])或者它们对应的每一项都绝对相等.注意,不为nil的
空切片和一个空的切片不为绝对相等([]byte{} 和 []byte(nil)不绝对相等)

其他值 - number, bool, string, channel 当用==比较为相等时,为绝对相等,

简单来说 DeepEqual是一个递归松弛版的GO "==" 操作符.但是这个并不足以解释所有的情况.详细来说,
比如值和自身有时是不相等的,造成这种情况,可能因为它是一个函数(无法比较),可能它是一个浮点数的NaN(在
浮点数比较时不相等), 也可能它是一个包含这样值的数组,结构体,或者接口.另一方面,指针的值不论内容,永远都和它们
自身绝对相等,即使它们指向上面描述的这些问题值,因为指针的绝对相等条件是 "==" 运算符的结果,不管指向的内容
为何,这一结果足够使我们认为它们绝对相等. DeepEqual在实际定义中,认为如果切片和映射指向的是同一块数据,无论
它们的内容如何,都为绝对相等

当DeepEqual遍历数据的时候,可能会碰到闭环.当此方法在遍历过程中,碰到已经对比过的两个对象时会退出,并
认为两参数的值绝对相等

<h2 id="Select">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1985">Select</a>
    <a href="#Select">¶</a></h2>
<pre>func Select(cases []<a href="#SelectCase">SelectCase</a>) (chosen <a href="/builtin/#int">int</a>, recv <a href="#Value">Value</a>, recvOK <a href="/builtin/#bool">bool</a>)</pre>
Select 方法执行一个参数所描述的select操作,就像go的select语句一样,在有条件可以被执行时结束阻塞,随机
选择一个可执行的条件执行.它会返回被选择条件在参数数组中的下标.如果这个条件是一个接收操作,被接收的值和一个布尔值会被
返回,这个布尔值表明了接收到的值是否是从通道接收到的(当通道关闭后会接收到zero值)

<h2 id="Swapper">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/swapper.go#L3">Swapper</a>
    <a href="#Swapper">¶</a></h2>
<pre>func Swapper(slice interface{}) func(i, j <a href="/builtin/#int">int</a>)</pre>
Swapper 传入一个Slice(切片)并返回一个函数, 这个函数可以传入两个元素的下标,然后在传入的切片中交换下标标注的两个元素

如果参数不是一个Slice(切片),Swapper 会抛出panic.

<h2 id="ChanDir">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L332">ChanDir</a>
    <a href="#ChanDir">¶</a></h2>
<pre>type ChanDir <a href="/builtin/#int">int</a></pre>
ChanDir 代表了一个通道传输的方向

<pre>const (
    <span id="RecvDir">RecvDir</span> <a href="#ChanDir">ChanDir</a>             = 1 &lt;&lt; <a href="/builtin/#iota">iota</a> <span class="comment">// &lt;-chan</span>
    <span id="SendDir">SendDir</span>                                 <span class="comment">// chan&lt;-</span>
    <span id="BothDir">BothDir</span> = <a href="#RecvDir">RecvDir</a> | <a href="#SendDir">SendDir</a>             <span class="comment">// chan</span>
)</pre>


<h3 id="ChanDir.String">func (ChanDir) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L1073">String</a>
    <a href="#ChanDir.String">¶</a></h3>
<pre>func (d <a href="#ChanDir">ChanDir</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Kind">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L216">Kind</a>
    <a href="#Kind">¶</a></h2>
<pre>type Kind <a href="/builtin/#uint">uint</a></pre>
Kind是指Tpye所表示的类型的种类,Zero种类不是有效的种类

<pre>const (
    <span id="Invalid">Invalid</span> <a href="#Kind">Kind</a> = <a href="/builtin/#iota">iota</a>
    <span id="Bool">Bool</span>
    <span id="Int">Int</span>
    <span id="Int8">Int8</span>
    <span id="Int16">Int16</span>
    <span id="Int32">Int32</span>
    <span id="Int64">Int64</span>
    <span id="Uint">Uint</span>
    <span id="Uint8">Uint8</span>
    <span id="Uint16">Uint16</span>
    <span id="Uint32">Uint32</span>
    <span id="Uint64">Uint64</span>
    <span id="Uintptr">Uintptr</span>
    <span id="Float32">Float32</span>
    <span id="Float64">Float64</span>
    <span id="Complex64">Complex64</span>
    <span id="Complex128">Complex128</span>
    <span id="Array">Array</span>
    <span id="Chan">Chan</span>
    <span id="Func">Func</span>
    <span id="Interface">Interface</span>
    <span id="Map">Map</span>
    <span id="Ptr">Ptr</span>
    <span id="Slice">Slice</span>
    <span id="String">String</span>
    <span id="Struct">Struct</span>
    <span id="UnsafePointer">UnsafePointer</span>
)</pre>


<h3 id="Kind.String">func (Kind) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L588">String</a>
    <a href="#Kind.String">¶</a></h3>
<pre>func (k <a href="#Kind">Kind</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Method">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L566">Method</a>
    <a href="#Method">¶</a></h2>
<pre>type Method struct {
<span id="Method.Name"></span>    <span class="comment">// Name 是方法的名.</span>
<span id="Method.PkgPath"></span>    <span class="comment">// PkgPath 是定义该非导出方法的包路径</span>
    <span class="comment">// 导出方法的该项为空</span>
    <span class="comment">// PkgPath和Name可以作为在方法集中的唯一识别特征 </span>
    <span class="comment">// 请查看 https://golang.org/ref/spec#Uniqueness_of_identifiers</span>
    Name    <a href="/builtin/#string">string</a>
    PkgPath <a href="/builtin/#string">string</a>

<span id="Method.Type"></span>    Type  <a href="#Type">Type</a>  <span class="comment">// 方法类型</span>
<span id="Method.Func"></span>    Func  <a href="#Value">Value</a> <span class="comment">// 接收方作为第一个参数的方法</span>
<span id="Method.Index"></span>    Index <a href="/builtin/#int">int</a>   <span class="comment">// Type.Method的下标</span>
}</pre>

Method 代表一个方法

<h2 id="SelectCase">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1972">SelectCase</a>
    <a href="#SelectCase">¶</a></h2>
<pre>type SelectCase struct {
<span id="SelectCase.Dir"></span>    Dir  <a href="#SelectDir">SelectDir</a> <span class="comment">// case的方向</span>
<span id="SelectCase.Chan"></span>    Chan <a href="#Value">Value</a>     <span class="comment">// channel通道</span>
<span id="SelectCase.Send"></span>    Send <a href="#Value">Value</a>     <span class="comment">// 发送的值</span>
}</pre>
SelectCase 代表了select操作中一个条件, 这个条件的种类取决于DIR,即通信方向

如果Dir的值是 SelectDefault, 这个条件代表了一个默认条件, 通道和Send必须是zero值

如果Dir的值是 SelectSend, 这个条件代表了一个发送操作.正常来说Chan代表的值必须是一个通道,而且发送的值必须
可以传进该通道.有一个特殊情况,如果通道是一个zero值,即使Send的值不是zero,这个操作和Send都会被忽略

如果Dir的值是 SelectSend, 这个case代表了一个接收操作.正常来说Chan代表的值必须是一个通道,如果通道
是一个zero值,这个条件会被忽略,但是Send必须还是个zero值.当一个接收操作被选择时,Select返回接收到的值

<h2 id="SelectDir">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1944">SelectDir</a>
    <a href="#SelectDir">¶</a></h2>
<pre>type SelectDir <a href="/builtin/#int">int</a></pre>
SelectDir 描述了这个SelectCase的通信方向

<pre>const (
    <span id="SelectSend">SelectSend</span>    <a href="#SelectDir">SelectDir</a> <span class="comment">// case Chan &lt;- Send</span>
    <span id="SelectRecv">SelectRecv</span>              <span class="comment">// case &lt;-Chan:</span>
    <span id="SelectDefault">SelectDefault</span>           <span class="comment">// default</span>
)</pre>


<h2 id="SliceHeader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1790">SliceHeader</a>
    <a href="#SliceHeader">¶</a></h2>
<pre>type SliceHeader struct {
<span id="SliceHeader.Data"></span>    Data <a href="/builtin/#uintptr">uintptr</a>
<span id="SliceHeader.Len"></span>    Len  <a href="/builtin/#int">int</a>
<span id="SliceHeader.Cap"></span>    Cap  <a href="/builtin/#int">int</a>
}</pre>
SliceHeader 代表了一个在运行时的Slice(切片).这个struct目前是不安全和不稳定的,可能会在之后的版本改进.
Data数据并不能保证它指向的数据不会被GC收集,所以程序必须保证有一个独立并且类型正确的指针指向其下的数据

<h2 id="StructField">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L1123">StructField</a>
    <a href="#StructField">¶</a></h2>
<pre>type StructField struct {
<span id="StructField.Name"></span>    <span class="comment">// Name 是字段名</span>
    Name <a href="/builtin/#string">string</a>
<span id="StructField.PkgPath"></span>    <span class="comment">// PkgPath 是定义该非导出方法的包路径</span>
    <span class="comment">// 导出方法的该项为空</span>
    <span class="comment">// 请查看 https://golang.org/ref/spec#Uniqueness_of_identifiers</span>
    PkgPath <a href="/builtin/#string">string</a>

<span id="StructField.Type"></span>    Type      <a href="#Type">Type</a>      <span class="comment">// 字段类型</span>
<span id="StructField.Tag"></span>    Tag       <a href="#StructTag">StructTag</a> <span class="comment">// 字段的tag</span>
<span id="StructField.Offset"></span>    Offset    <a href="/builtin/#uintptr">uintptr</a>   <span class="comment">// 结构体内的偏离值以bytes为单位</span>
<span id="StructField.Index"></span>    Index     []<a href="/builtin/#int">int</a>     <span class="comment">// Type.FieldByIndex下标</span>
<span id="StructField.Anonymous"></span>    Anonymous <a href="/builtin/#bool">bool</a>      <span class="comment">// 是否是embedded字段</span>
}</pre>
StructField 描述了结构体中的一个字段

<h2 id="StructTag">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L1146">StructTag</a>
    <a href="#StructTag">¶</a></h2>
<pre>type StructTag <a href="/builtin/#string">string</a></pre>

StructTag 是StructField中Tag属性的值

按照规约,tag是一串用key:"value"形式组成的字符串,每个Key-Value之间可以用空格作为分隔符.
每一个键是一个非空的字符串,并且不能包含空格(U+0020 ' '),引号 (U+0022 '"')和冒号(U+003A
':').每个值由U+0022 '"'字符引住,并且符合Go的字符串字面量规则


<a id="exampleStructTag"></a>
Example:

    type S struct {
        F string `species:"gopher" color:"blue"`
    }

    s := S{}
    st := reflect.TypeOf(s)
    field := st.Field(0)
    fmt.Println(field.Tag.Get("color"), field.Tag.Get("species"))

    // Output:
    // blue gopher

<h3 id="StructTag.Get">func (StructTag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L1153">Get</a>
    <a href="#StructTag.Get">¶</a></h3>
<pre>func (tag <a href="#StructTag">StructTag</a>) Get(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Get 返回tag中以key为键的值,如果所查询的键不存在,返回空字符串,如果tag不符合规约的格式,返回值未知.
判断一个tag是否由外部赋值为空string,使用Lookup.

<h3 id="StructTag.Lookup">func (StructTag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L1164">Lookup</a>
    <a href="#StructTag.Lookup">¶</a></h3>
<pre>func (tag <a href="#StructTag">StructTag</a>) Lookup(key <a href="/builtin/#string">string</a>) (value <a href="/builtin/#string">string</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

Lookup返回key在tag中对应的值,如果key包含在tag中,它返回key的值(可能为空字符串),反则返回空字符串
ok返回这个值是否被设置过.如果一个tag不符合规约的格式,返回值未知.

<a id="exampleStructTag_Lookup"></a>
Example:

    type S struct {
        F0  string `alias:"field_0"`
        F1  string `alias:""`
        F2  string
    }

    s := S{}
    st := reflect.TypeOf(s)
    for i := 0; i < st.NumField(); i++ {
        field := st.Field(i)
        if alias, ok := field.Tag.Lookup("alias"); ok {
            if alias == "" {
                fmt.Println("(blank)")
            } else {
                fmt.Println(alias)
            }
        } else {
            fmt.Println("(not specified)")
        }
    }

    // Output:
    // field_0
    // (blank)
    // (not specified)

<h2 id="Type">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L28">Type</a>
    <a href="#Type">¶</a></h2>
<pre>type Type interface {

    <span class="comment">// Align 返回这个类型在内存中所占的大小(单位byte)</span>
    Align() <a href="/builtin/#int">int</a>

    <span class="comment">// FieldAlign 返回,当作为一个结构体中的字段时,在内存中所占的大小(单位byte)</span>
    FieldAlign() <a href="/builtin/#int">int</a>

    <span class="comment">// Method返回类型的方法集中第i个方法</span>
    <span class="comment">// 如果i不在[0, NumMethod()))范围内,会发生panics.</span>
    <span class="comment">// 对于一个不是接口(Interface)的类型T和*T,返回方法的Type和Func项描述了　</span>
    <span class="comment">// 一个第一个参数是接收方的方法.</span>
    <span class="comment">// 对于一个接口(Interface)类型,返回方法的Type字段是一个没有接收方的方法签名,Func项为空.</span>
    Method(<a href="/builtin/#int">int</a>) <a href="#Method">Method</a>

    <span class="comment">// 如果方法存在,MethodByName返回类型方法集中以name命名的方法和一个布尔值</span>
    <span class="comment">// 对于一个不是接口(Interface)的类型,返回方法的Type和Func字段描述了一个第一个参数是接收方的方法</span>
    <span class="comment">// 对于一个接口(Interface)类型,返回方法的Type字段是一个没有接收方的方法签名,Func字段为空</span>
    MethodByName(<a href="/builtin/#string">string</a>) (<a href="#Method">Method</a>, <a href="/builtin/#bool">bool</a>)

    <span class="comment">// NumMethod返回类型可导出方法的数量</span>
    NumMethod() <a href="/builtin/#int">int</a>

    <span class="comment">// Name返回类型在包中的命名</span>
    Name() <a href="/builtin/#string">string</a>

    <span class="comment">// PkgPath 返回包含类型的包路径,也就是导入路径,比如&#34;encoding/base64&#34;</span>
    <span class="comment">// 如果类型是提前定义的类型(string或者error),或者是未命名的类型(*T, struct{}, []int)</span>
    <span class="comment">// 包路径为空</span>
    PkgPath() <a href="/builtin/#string">string</a>

    <span class="comment">// Size 返回需要保存该类型的byte数目,和unsafe.Sizeof类似.</span>
    Size() <a href="/builtin/#uintptr">uintptr</a>

    <span class="comment">// String返回类型的字符串表示形式.</span>
    <span class="comment">// 这个字符串可能使用缩短后的包命名</span>
    <span class="comment">// 比如返回base64,而不是&#34;encoding/base64&#34;</span>
    <span class="comment">// 不保证是唯一的</span>
    <span class="comment">// 如果比较类型的话,应该直接比较Types.　compare the Types directly.</span>
    String() <a href="/builtin/#string">string</a>

    <span class="comment">// Kind返回类型的种类</span>
    Kind() <a href="#Kind">Kind</a>

    <span class="comment">// Implements返回该类型是否实现了接口(Interface)类型u.</span>
    Implements(u <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a>

    <span class="comment">// AssignableTo返回该类型的值是否可以赋值给类型u.</span>
    AssignableTo(u <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a>

    <span class="comment">// ConvertibleTo返回该类型是否可以传化成类型u.</span>
    ConvertibleTo(u <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a>

    <span class="comment">// Comparable返回该类型的值是否可以比较.</span>
    Comparable() <a href="/builtin/#bool">bool</a>

    <span class="comment">// Bits返回该类型的大小,以bit为单位</span>
    <span class="comment">// 它会发生panic,如果类型的种类不是一个Int, Uint, Float或者Complex.</span>
    Bits() <a href="/builtin/#int">int</a>

    <span class="comment">// ChanDir返回一个通道类型的方向</span>
    <span class="comment">// 如果类型的种类不是通道(Chan),它会发生panic.</span>
    ChanDir() <a href="#ChanDir">ChanDir</a>

    <span class="comment">// IsVariadic返回方法最后的一个参数是否为可变长度,如果是t.In(t.NumIn() - 1) 返回该参数隐示的真实类型[]T</span>
    <span class="comment">// 举个例子,如果t代表了func(x int, y ... float64),那么:</span>
    <span class="comment">//</span>
    <span class="comment">//	t.NumIn() == 2</span>
    <span class="comment">//	t.In(0) is the reflect.Type for &#34;int&#34;</span>
    <span class="comment">//	t.In(1) is the reflect.Type for &#34;[]float64&#34;</span>
    <span class="comment">//	t.IsVariadic() == true</span>
    <span class="comment">//</span>
    <span class="comment">// 如果类型的种类不是一个方法,IsVariadic发生panic.</span>
    IsVariadic() <a href="/builtin/#bool">bool</a>

    <span class="comment">// Elem返回类型中元素的类型</span>
    <span class="comment">// 当类型的种类不是Array, Chan, Map, Ptr, Slice时,会发生panics.</span>
    Elem() <a href="#Type">Type</a>

    <span class="comment">// Field 返回一个结构体类型的第i个字段</span>
    <span class="comment">// 如果类型的种类不是一个结构体,发生panic.</span>
    <span class="comment">// 如果i不在[0,NumField()]范围内,发生panic</span>
    Field(i <a href="/builtin/#int">int</a>) <a href="#StructField">StructField</a>

    <span class="comment">// FieldByIndex返回和index序列相关联的StructField,相当于使用index的每一项调用Field</span>
    <span class="comment">// 如果类型的种类不是一个结构体, 发生panic.</span>
    FieldByIndex(index []<a href="/builtin/#int">int</a>) <a href="#StructField">StructField</a>

    <span class="comment">// FieldByName返回以name命名的StructField.</span>
    <span class="comment">// 布尔值表示这项是否找到.</span>
    FieldByName(name <a href="/builtin/#string">string</a>) (<a href="#StructField">StructField</a>, <a href="/builtin/#bool">bool</a>)

    <span class="comment">// FieldByNameFunc返回一个以name命名并且符合match方法的StructField</span>
    <span class="comment">// 布尔值表示这项是否找到.</span>
    <span class="comment">//</span>
    <span class="comment">// FieldByNameFunc会以广度优先的原则考虑自身以及任何匿名结构体的字段.</span>
    <span class="comment">// 它会在包含一个会多个match项最浅的一层停止,如果多项都match,返回没有match.</span>
    <span class="comment">// 这一行为和Go对含有匿名项的结构体进行命名查询的规则一致</span>
    FieldByNameFunc(match func(<a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a>) (<a href="#StructField">StructField</a>, <a href="/builtin/#bool">bool</a>)

    <span class="comment">// In 返回一个方法类型的第i个参数.</span>
    <span class="comment">// 如果这个类型的的种类不是方法,发生panic.</span>
    <span class="comment">// 如果i不在[0, NumIn())范围内, 发生panic.</span>
    In(i <a href="/builtin/#int">int</a>) <a href="#Type">Type</a>

    <span class="comment">// Key返回一个映射(Map)键的类型</span>
    <span class="comment">// 如果类型的种类不是映射(Map),发生panic.</span>
    Key() <a href="#Type">Type</a>

    <span class="comment">// Len返回一个数组(Array)的长度</span>
    <span class="comment">// 如果类型的种类不是数组(Array),发生panic.</span>
    Len() <a href="/builtin/#int">int</a>

    <span class="comment">// NumField返回一个结构体(Struct)字段的数量</span>
    <span class="comment">// 如果类型的种类不是结构体(Struct),发生panic.</span>
    NumField() <a href="/builtin/#int">int</a>

    <span class="comment">// NumIn返回一个方法(Func)的输入形参数量</span>
    <span class="comment">// 如果类型的种类不是方法(Func),panic.It panics if the type&#39;s Kind is not Func.</span>
    NumIn() <a href="/builtin/#int">int</a>

    <span class="comment">// NumOut返回一个方法类型返回值的数量.</span>
    <span class="comment">// 如果类型的种类不是方法(Func),发生panic.</span>
    NumOut() <a href="/builtin/#int">int</a>

    <span class="comment">// Out返回方法类型第i个返回值的类型.</span>
    <span class="comment">// 如果类型的种类不是方法(Func),发生panic.</span>
    <span class="comment">// 如果i不在[0, NumOut())范围内,发生panic.</span>
    Out(i <a href="/builtin/#int">int</a>) <a href="#Type">Type</a>
    <span class="comment">// 包含filtered和unexported的方法</span>
}</pre>

Type表示Go中的一个类型

不是所有的方法都对所有类型有效,如果有限制的话,文档上会标明,在调用Kind才有的方法前,使用Kind方法
来找出类型的种类,不恰当的调用一个类型的方法会引发运行时panic.

类型的值是可以比较的,比如使用==符号,这样它们就可以用来做映射(Map)的键,如果两个类型代表一样的类型,
它们也相等.

<h3 id="ArrayOf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L2849">ArrayOf</a>
    <a href="#ArrayOf">¶</a></h3>
<pre>func ArrayOf(count <a href="/builtin/#int">int</a>, elem <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>
ArrayOf返回一个参数描述的数组类型,比如,如果t表示int,ArrayOf(5,t)表示[5]int.

如果返回的类型比可用地址空间大,ArratOf发生panic.

<h3 id="ChanOf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L1804">ChanOf</a>
    <a href="#ChanOf">¶</a></h3>
<pre>func ChanOf(dir <a href="#ChanDir">ChanDir</a>, t <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>
ChanOf 返回参数表示的通道类型,比如,如果t表示int,ChanOf(RecvDir, t)表示 <-chan int.

GC运行时强制设置了通道类型的大小不能超过64KB,如果t的类型等于或者超过这个限制,ChanOf 发生panic.

<h3 id="FuncOf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L1949">FuncOf</a>
    <a href="#FuncOf">¶</a></h3>
<pre>func FuncOf(in, out []<a href="#Type">Type</a>, variadic <a href="/builtin/#bool">bool</a>) <a href="#Type">Type</a></pre>

FuncOf 返回一个参数和返回值描述的方法类型,比如,如果k表示int,e表示字符串,FuncOf([]Type{k},
[]Type{e}, false) 表示 func(int) string.

可变长度的参数控制了方法是否为可变长度.如果in[len(in)-1]不是切片(Slice),而且variadic是true的话,FuncOf发生panic.

<h3 id="MapOf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L1861">MapOf</a>
    <a href="#MapOf">¶</a></h3>
<pre>func MapOf(key, elem <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>

MapOf 返回参数描述的映射(Map)类型,比如,如果k表示int,e表示字符串,MapOf(k, e) 表示 map(int) string

如果键的类型不是一个合法的键类型(即没有实现Go的 == 操作符),MapOf发生panic.

<h3 id="PtrTo">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L1411">PtrTo</a>
    <a href="#PtrTo">¶</a></h3>
<pre>func PtrTo(t <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>

PtrTo返回元素t的指针类型,比如,如果t表示类型Foo,PtrTo(t)表示*Foo

<h3 id="SliceOf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L2257">SliceOf</a>
    <a href="#SliceOf">¶</a></h3>
<pre>func SliceOf(t <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>

PtrTo返回一个以元素t类型为类型的数组(Array)类型.如果t表示int,SliceOf(t)表示[]int

<h3 id="StructOf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L2373">StructOf</a>
    <a href="#StructOf">¶</a></h3>
<pre>func StructOf(fields []<a href="#StructField">StructField</a>) <a href="#Type">Type</a></pre>

PtrTo返回一个包含fields的结构体(Struct)类型.偏移量和下标会被忽略,因为它们是由编译器来计算的

StructOf 当前并不为嵌入的字段产生wrapper方法,这个限制可能在将来取消

<a id="exampleStructOf"></a>
Example:

    typ := reflect.StructOf([]reflect.StructField{
        {
            Name: "Height",
            Type: reflect.TypeOf(float64(0)),
            Tag:  `json:"height"`,
        },
        {
            Name: "Age",
            Type: reflect.TypeOf(int(0)),
            Tag:  `json:"age"`,
        },
    })

    v := reflect.New(typ).Elem()
    v.Field(0).SetFloat(0.4)
    v.Field(1).SetInt(2)
    s := v.Addr().Interface()

    w := new(bytes.Buffer)
    if err := json.NewEncoder(w).Encode(s); err != nil {
        panic(err)
    }

    fmt.Printf("value: %+v\n", s)
    fmt.Printf("json:  %s", w.Bytes())

    r := bytes.NewReader([]byte(`{"height":1.5,"age":10}`))
    if err := json.NewDecoder(r).Decode(s); err != nil {
        panic(err)
    }
    fmt.Printf("value: %+v\n", s)

    // Output:
    // value: &{Height:0.4 Age:2}
    // json:  {"height":0.4,"age":2}
    // value: &{Height:1.5 Age:10}

<h3 id="TypeOf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L1401">TypeOf</a>
    <a href="#TypeOf">¶</a></h3>
<pre>func TypeOf(i interface{}) <a href="#Type">Type</a></pre>

TypeOf 返回代表动态类型i的反射类型,如果i是一个空接口(Interface),TypeOf返回空值

<a id="exampleTypeOf"></a>
Example:

    // 接口(Interface)不仅仅用于静态类型
    // 一个通常的习惯,使用*Foo来找出接口Foo的反射类型
    writerType := reflect.TypeOf((*io.Writer)(nil)).Elem()

    fileType := reflect.TypeOf((*os.File)(nil))
    fmt.Println(fileType.Implements(writerType))

    // Output:
    // true

<h2 id="Value">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L26">Value</a>
    <a href="#Value">¶</a></h2>
<pre>type Value struct {
    <span class="comment">// 包含filtered或者非导出字段</span>
}</pre>

Value是Go语言值的反射接口

不是所有的方法都对所有类型的值适用,如果有限制的话,文档上会标明,在调用Kind才有的方法前,使用Kind方法
来找出类型的种类,不合理的调用一个类型的方法会引发运行时panic.

Zero值表示没有值,它的IsValid方法返回false.它的Kind方法返回Invalid,它的String方法返回
"<invalid Value>",其他方法发生panic.大多数函数和方法从不返回无效值.如果返回,文档会特别注明

Value可以被多个goroutine并发使用,因为Go语言的value可以并发使用.

为了比较两个Value,需要比较接口方法的结果,用==操作符不能比较它们表示的底层值.

<h3 id="Append">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1850">Append</a>
    <a href="#Append">¶</a></h3>
<pre>func Append(s <a href="#Value">Value</a>, x ...<a href="#Value">Value</a>) <a href="#Value">Value</a></pre>
Append把x插入进切片s,返回结果.在Go中,每一个x的值必须可以赋值给切片的类型.

<h3 id="AppendSlice">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1861">AppendSlice</a>
    <a href="#AppendSlice">¶</a></h3>
<pre>func AppendSlice(s, t <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>
Append把x插入进切片s,返回结果.s和t必须有一样的元素类型

<h3 id="Indirect">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L2131">Indirect</a>
    <a href="#Indirect">¶</a></h3>
<pre>func Indirect(v <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>
Indirect 返回v指向的值.如果v是一个空指针,Indirect返回一个zero值.如果v不是一个空指针,Indirect返回v.

<h3 id="MakeChan">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L2099">MakeChan</a>
    <a href="#MakeChan">¶</a></h3>
<pre>func MakeChan(typ <a href="#Type">Type</a>, buffer <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>
MakeChan创建一个具有类型和缓冲大小的新通道

<h3 id="MakeFunc">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/makefunc.go#L37">MakeFunc</a>
    <a href="#MakeFunc">¶</a></h3>
<pre>func MakeFunc(typ <a href="#Type">Type</a>, fn func(args []<a href="#Value">Value</a>) (results []<a href="#Value">Value</a>)) <a href="#Value">Value</a></pre>
MakeFunc根据提供的类型返回一个新的函数,这个新的函数包裹方法fn,当被调用时,这个新的方法会做一下操作:
MakeFunc returns a new function of the given Type that wraps the function fn.
When called, that new function does the following:

    - 把参数转换为一个Value切片.
    - 运行results := fn(args).
    - 返回一个Value切片作为结果,每个结果是其中一个元素

可以假设fn的实现,当实参Value切片含有typ制定的数量和类型.如果typ指代一个参数可变长度的函数,由于时在一个可变长函数的函数体中,
最终的Value是它自己:一个代表可变长实参的切片.fn返回的这个结果切片必须有跟type声明的一样的数量和类型.

Value.Call方法允许调用者触发一个有类型的函数,相反的是,MakeFunc允许调用者实现一个有类型的函数

文档的例子包含了怎么使用MakeFunc来交换不同的类型

<a id="exampleMakeFunc"></a>
Example:

    // swap是一个会被传入MakeFunc的方法实现
    // 为了在不知道类型的情况下实现代码,必须使用reflect.Value来作为类型
    swap := func(in []reflect.Value) []reflect.Value {
        return []reflect.Value{in[1], in[0]}
    }
    // makeSwap期待fptr时一个指向nil方法的指针
    // 它设置指针的值为使用MakeFunc创建的新方法
    // 当函数被触发,reflect把参数变为Value类型,调用swap.
    // 然后把swap的结果切片转换为新方法返回的值
    makeSwap := func(fptr interface{}) {
        // fptr是一个指向函数的指针
        // 获取函数值,转换为Value
        // 这样我们就能查询它的类型和设置它的值
        fn := reflect.ValueOf(fptr).Elem()

        // 创建一个该类型的方法
        v := reflect.MakeFunc(fn.Type(), swap)

        // 赋值
        fn.Set(v)
    }

    // 创建并对int调用swap函数
    var intSwap func(int, int) (int, int)
    makeSwap(&intSwap)
    fmt.Println(intSwap(0, 1))

    // 创建并对float64调用swap函数
    var floatSwap func(float64, float64) (float64, float64)
    makeSwap(&floatSwap)
    fmt.Println(floatSwap(2.72, 3.14))

    // Output:
    // 1 0
    // 3.14 2.72

<h3 id="MakeMap">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L2114">MakeMap</a>
    <a href="#MakeMap">¶</a></h3>
<pre>func MakeMap(typ <a href="#Type">Type</a>) <a href="#Value">Value</a></pre>

MakeMap创建一个相应类型的映射(Map)

<h3 id="MakeMapWithSize">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L2120">MakeMapWithSize</a>
    <a href="#MakeMapWithSize">¶</a></h3>
<pre>func MakeMapWithSize(typ <a href="#Type">Type</a>, n <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

MakeMap创建一个相应类型和初始空间接近n个元素的映射(Map)

<h3 id="MakeSlice">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L2080">MakeSlice</a>
    <a href="#MakeSlice">¶</a></h3>
<pre>func MakeSlice(typ <a href="#Type">Type</a>, len, cap <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

MakeMap创建一个新初始化的切片(Slice),新的切片的类型长度和容量和传递的参数一致

<h3 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L2173">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(typ <a href="#Type">Type</a>) <a href="#Value">Value</a></pre>

New 返回一个Value,表示一个指针,指向一个以typ为类型的空值.即返回的Value类型是PtrTo(typ)

<h3 id="NewAt">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L2184">NewAt</a>
    <a href="#NewAt">¶</a></h3>
<pre>func NewAt(typ <a href="#Type">Type</a>, p <a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>) <a href="#Value">Value</a></pre>

New 返回一个Value,表示一个指针,指向一个以typ为类型的值,p为这个指针

<h3 id="ValueOf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L2140">ValueOf</a>
    <a href="#ValueOf">¶</a></h3>
<pre>func ValueOf(i interface{}) <a href="#Value">Value</a></pre>

ValueOf返回一个新的Value, 初始化的值为i所保存的具体值,ValueOf(nil)返回zero值

<h3 id="Zero">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L2159">Zero</a>
    <a href="#Zero">¶</a></h3>
<pre>func Zero(typ <a href="#Type">Type</a>) <a href="#Value">Value</a></pre>

Zero返回一个Value表示typ类型的zero值.返回结果和Value结构体的zero值是不同的.它表示没有value.
比如,Zero(TypeOf(42))返回一个种类为Int,值为0的值.返回的值既没有地址也无法修改.

<h3 id="Value.Addr">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L233">Addr</a>
    <a href="#Value.Addr">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Addr() <a href="#Value">Value</a></pre>

Addr返回一个代表v地址的指针.如果CanAddr()返回false,发生panic.当调用一个需要指针作为接收方的方法时,
Addr特意用来获取结构体字段或者切片元素的指针,

<h3 id="Value.Bool">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L242">Bool</a>
    <a href="#Value.Bool">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Bool() <a href="/builtin/#bool">bool</a></pre>

Bool返回v底层的值,如果v的种类不是布尔值,发生panic.

<h3 id="Value.Bytes">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L249">Bytes</a>
    <a href="#Value.Bytes">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Bytes() []<a href="/builtin/#byte">byte</a></pre>

Bool返回v底层的值,如果v的种类不是bytes切片,发生panic.

<h3 id="Value.Call">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L295">Call</a>
    <a href="#Value.Call">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Call(in []<a href="#Value">Value</a>) []<a href="#Value">Value</a></pre>

Call使用in参数调用方法v.举例: 如果len(in) == 3, v.Call(in)表示Go调用v(in[0], in[1], in[2]).
如果in的种类不是函数(Func),Call发生panic.由于在Go中,每一个实参必须可以赋值给函数对应的形参,
它返回的结果是Value类型的切片..如果v是一个参数长度可变的函数.Call创建一个可变长度的切片形参,赋值相应的值

<h3 id="Value.CallSlice">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L308">CallSlice</a>
    <a href="#Value.CallSlice">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) CallSlice(in []<a href="#Value">Value</a>) []<a href="#Value">Value</a></pre>

CallSlice使用实参in调用参数可变长度的函数v,把这个切片in[len(in)-1]赋值给v的最后一个实参.例子:如果
len(in)==3,v.CallSlice(in)表示Go调用v(in[0], in[1], in[2]...).
如果v的种类不是一个Func或者v参数长度不可变,发生panic.由于在Go中,每一个实参必须可以赋值给函数对应的
形参,它返回值是一个Value类型.

<h3 id="Value.CanAddr">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L274">CanAddr</a>
    <a href="#Value.CanAddr">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) CanAddr() <a href="/builtin/#bool">bool</a></pre>

CanAddr表示value的地址是否可以使用Addr函数获取.这样的值被称为可寻址的.当值是切片的元素,
可寻址数组的元素,可寻址的结构体字段,或者指针指向的结果时,该值也是可寻址的.如果CanAddr返回false,
调用Addr会引发panic

<h3 id="Value.CanInterface">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L925">CanInterface</a>
    <a href="#Value.CanInterface">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) CanInterface() <a href="/builtin/#bool">bool</a></pre>

CanInterface返回接口是否有可能造成panic.

<h3 id="Value.CanSet">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L283">CanSet</a>
    <a href="#Value.CanSet">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) CanSet() <a href="/builtin/#bool">bool</a></pre>

CanSet 返回v的值是否改变.只有当一个Value为可寻址或者不是通过结构体的unexported字段获取的,就可以被改变.
如果CanSet返回false,调用Set或其他相应的赋值方法(比如SetBool或SetInt)时,会发生panic.

<h3 id="Value.Cap">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L694">Cap</a>
    <a href="#Value.Cap">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Cap() <a href="/builtin/#int">int</a></pre>

Cap返回v的容量.如果v的种类不是数组,通道或者切片,会发生panic.

<h3 id="Value.Close">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L710">Close</a>
    <a href="#Value.Close">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Close()</pre>

Close关闭通道v.如果v的种类不是通道,会发生panic.

<h3 id="Value.Complex">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L718">Complex</a>
    <a href="#Value.Complex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Complex() <a href="/builtin/#complex128">complex128</a></pre>

Complex返回v的底层值,返回值是一个complex128类型,如果v的种类不是Complex64或者Complex128,会发生panic.

<h3 id="Value.Convert">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L2231">Convert</a>
    <a href="#Value.Convert">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Convert(t <a href="#Type">Type</a>) <a href="#Value">Value</a></pre>

Convert返回被转换类型为t的值v.如果通常的Go转换规则不允许v被转换成t.Convert或发生panic.

<h3 id="Value.Elem">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L733">Elem</a>
    <a href="#Value.Elem">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Elem() <a href="#Value">Value</a></pre>

Elem返回接口v包含或者指针v指向的值.如果v的种类不是接口或者指针.会发生panic.如果v是nil,Elem返回zero值

<h3 id="Value.Field">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L770">Field</a>
    <a href="#Value.Field">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Field(i <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

Field返回结构体v的第i个字段.如果v的种类不是结构体,或者i不在范围内,会发生panic.

<h3 id="Value.FieldByIndex">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L802">FieldByIndex</a>
    <a href="#Value.FieldByIndex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) FieldByIndex(index []<a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

FieldByIndex返回相对于i嵌套的字段,如果v的种类不是结构体,会发生panic.

<h3 id="Value.FieldByName">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L824">FieldByName</a>
    <a href="#Value.FieldByName">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) FieldByName(name <a href="/builtin/#string">string</a>) <a href="#Value">Value</a></pre>

FieldByName返回结构体的对应name的字段,如果没有找到该字段,FieldByName返回zero值.如果v的种类不是结构体,会发生panic.

<h3 id="Value.FieldByNameFunc">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L836">FieldByNameFunc</a>
    <a href="#Value.FieldByNameFunc">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) FieldByNameFunc(match func(<a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a>) <a href="#Value">Value</a></pre>

FieldByNameFunc返回一个以name为命名,并且符合match方法的结构体字段.如果v的种类不是结构体.会发生panic.如果没有
找到字段,返回zero值.

<h3 id="Value.Float">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L845">Float</a>
    <a href="#Value.Float">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Float() <a href="/builtin/#float64">float64</a></pre>

Float返回v的底层值,返回值是一个float64类型,如果v的种类不是Float32或者Float64,会发生panic.

<h3 id="Value.Index">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L860">Index</a>
    <a href="#Value.Index">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Index(i <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

Index返回v的第i个元素.如果v的种类不是数组,切片,字符串或者i不在范围内.会发生panic.

<h3 id="Value.Int">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L906">Int</a>
    <a href="#Value.Int">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Int() <a href="/builtin/#int64">int64</a></pre>

Int返回v的底层值,返回值是一个int64类型,如果v的种类不是Int8,Int16,Int16,或者Int64,会发生panic.

<h3 id="Value.Interface">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L937">Interface</a>
    <a href="#Value.Interface">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Interface() (i interface{})</pre>

Interface返回v的当前值,返回值是inteface{}类型.它等于:

    var i interface{} = (v's underlying value)

如果Value是从一个非导出的结构体字段,会发生panic.

<h3 id="Value.InterfaceData">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L973">InterfaceData</a>
    <a href="#Value.InterfaceData">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) InterfaceData() [2]<a href="/builtin/#uintptr">uintptr</a></pre>

InterfaceData返回一个接口v,v的值是一个uintptr对,如果v的种类不是一个接口,会发生panic.

<h3 id="Value.IsNil">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L991">IsNil</a>
    <a href="#Value.IsNil">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) IsNil() <a href="/builtin/#bool">bool</a></pre>

IsNil返回它的实参v是否为nil.这个实参必须是一个通道,函数,接口,映射,指针,或者切片.如果不是的话,
IsNil会发生panic.需要注意的是,IsNil并不一直是与nil的简单比较.比如,当v是有ValueOf通过一个未
初始化的接口i创建出的,i==nil回是true,但是v.IsNil会发生panic,因为v是一个zero值的Value.

<h3 id="Value.IsValid">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1016">IsValid</a>
    <a href="#Value.IsValid">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) IsValid() <a href="/builtin/#bool">bool</a></pre>

IsValid返回v是否表示一个Value.当v是zero Value时,返回false.如果IsValid返回false.除String外,所有其他
的方法都会发生panic.大多数函数和方法不会返回一个不合法的Value.如果会的话,文档会特意注明.

<h3 id="Value.Kind">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1022">Kind</a>
    <a href="#Value.Kind">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Kind() <a href="#Kind">Kind</a></pre>

Kind返回v的种类,如果v是zero Value(IsValid返回false),Kind返回Invalid.

<h3 id="Value.Len">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1028">Len</a>
    <a href="#Value.Len">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Len() <a href="/builtin/#int">int</a></pre>

Len返回v的长度.如果v的种类不是数组,通道,映射,切片,字符串,会发生panic.

<h3 id="Value.MapIndex">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1052">MapIndex</a>
    <a href="#Value.MapIndex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) MapIndex(key <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

MapIndex返回key在映射v中对应的值.如果v的种类不是映射,会发生panic.因为在Go中键的值必须可赋值给映射的类型,如果
key在映射中不存在,或者v是空映射,会发生panic.

<h3 id="Value.MapKeys">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1092">MapKeys</a>
    <a href="#Value.MapKeys">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) MapKeys() []<a href="#Value">Value</a></pre>

MapKeys返回一个包含映射中所有key的映射.Key的顺序不固定.如果v的种类不是映射,会发生panic.如果v是一个空映射,
返回空切片.


<h3 id="Value.Method">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1133">Method</a>
    <a href="#Value.Method">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Method(i <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

Method返回v的第i个方法.调用这个函数时,实参不能包含一个接收者；返回的函数会一直用v作为它的接收者.如果i不在范围
内或者v是一个空接口,Method发生panic.

<h3 id="Value.MethodByName">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1165">MethodByName</a>
    <a href="#Value.MethodByName">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) MethodByName(name <a href="/builtin/#string">string</a>) <a href="#Value">Value</a></pre>

MethodByName根据name返回v的方法.调用这个函数时的实参不能包含一个接收者；返回的函数会一直用v作为它的接收者.当没有找到
方法时,返回zero.

<h3 id="Value.NumField">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1181">NumField</a>
    <a href="#Value.NumField">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) NumField() <a href="/builtin/#int">int</a></pre>

NumField返回结构v的字段数量.如果v的种类不是结构体,会发生panic.

<h3 id="Value.NumMethod">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1150">NumMethod</a>
    <a href="#Value.NumMethod">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) NumMethod() <a href="/builtin/#int">int</a></pre>

NumMethod返回Value的方法集中可导出的方法数量.

<h3 id="Value.OverflowComplex">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1189">OverflowComplex</a>
    <a href="#Value.OverflowComplex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) OverflowComplex(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#bool">bool</a></pre>

OverflowComplex返回complex128 x是否可以用v的类型来表示.如果v的种类不是Complex64或者Complex128会发生panic.

<h3 id="Value.OverflowFloat">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1202">OverflowFloat</a>
    <a href="#Value.OverflowFloat">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) OverflowFloat(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#bool">bool</a></pre>

OverflowFloat返回float64 x是否可以用v的类型来表示.如果v的种类不是Float32或者Float64会发生panic.

<h3 id="Value.OverflowInt">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1222">OverflowInt</a>
    <a href="#Value.OverflowInt">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) OverflowInt(x <a href="/builtin/#int64">int64</a>) <a href="/builtin/#bool">bool</a></pre>

OverflowInt返回int64 x是否可以用v的类型来表示.如果v的种类不是Int,Int8,int16,Int32,或者Int64会发生panic.

<h3 id="Value.OverflowUint">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1235">OverflowUint</a>
    <a href="#Value.OverflowUint">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) OverflowUint(x <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#bool">bool</a></pre>

OverflowUint返回uint64 x是否可以用v的类型来表示.如果v的种类不是Uint,UInt8,Uintptr,Uint16,UInt32,
或者UInt64会发生panic.

<h3 id="Value.Pointer">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1260">Pointer</a>
    <a href="#Value.Pointer">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Pointer() <a href="/builtin/#uintptr">uintptr</a></pre>

Pointer返回v的uintptr值.它返回uintptr而不是unsafe.Pointer,这样使用反射的代码就不能在没有引用unsafe包的时候
来获取unsafe.Pointer.如果v的种类不是通道,函数,映射,指针,切片或者不安全指针,或发生panic.

如果v的种类是函数,返回的指针是一个底层的胆代码指针,但这还不足以来作为一个唯一识别.只能保证,当切仅当v是一个空
函数时结果是zero.

如果v的种类是一个切片,返回的指针指向切片的第一个元素.如果这个切片是nil,返回值是0.如果这和切片是空的,但不是nil,
返回值是non-zero

<h3 id="Value.Recv">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1296">Recv</a>
    <a href="#Value.Recv">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Recv() (x <a href="#Value">Value</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

Recv接收和返回通道v中的值.如果v的种类不是通道,会发生panic.接收在值准备好之前,都是阻塞的.x是通道中发送的值,
布尔值ok是true,如果因为通道关闭接收到一个zero值,ok是false.

<h3 id="Value.Send">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1329">Send</a>
    <a href="#Value.Send">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Send(x <a href="#Value">Value</a>)</pre>

Send向通道v发送值x.如果v的种类不是通道或者x的类型和v的元素类型不同,会发生panic.这是因为在Go中,
x的值必须可以赋值给通道的类型.

<h3 id="Value.Set">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1356">Set</a>
    <a href="#Value.Set">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Set(x <a href="#Value">Value</a>)</pre>

Set把x赋值给值v.如果CanSet返回false,会发生panic.这是因为在Go中,x的值必须可以赋值给通道的类型.

<h3 id="Value.SetBool">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1373">SetBool</a>
    <a href="#Value.SetBool">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetBool(x <a href="/builtin/#bool">bool</a>)</pre>

SetBool设置v的底层值.如果v的种类不是布尔值或者CanSet()返回false,会发生panic.

<h3 id="Value.SetBytes">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1381">SetBytes</a>
    <a href="#Value.SetBytes">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetBytes(x []<a href="/builtin/#byte">byte</a>)</pre>

SetBytes设置v的底层值.如果v的种类不是bytes切片或者CanSet()返回false,会发生panic.

<h3 id="Value.SetCap">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1465">SetCap</a>
    <a href="#Value.SetCap">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetCap(n <a href="/builtin/#int">int</a>)</pre>

SetCap对v设置一个新的容量.如果v的种类不是切片或者n小于长度或者大于旧的容量时,会发生panic.

<h3 id="Value.SetComplex">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1403">SetComplex</a>
    <a href="#Value.SetComplex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetComplex(x <a href="/builtin/#complex128">complex128</a>)</pre>

SetComplex设置v的底层值.如果v的种类不是Complex64,Complex128或者CanSet()返回false,会发生panic.

<h3 id="Value.SetFloat">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1417">SetFloat</a>
    <a href="#Value.SetFloat">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetFloat(x <a href="/builtin/#float64">float64</a>)</pre>

SetFloat设置v的底层值.如果v的种类不是Float32,Float64或者CanSet()返回false,会发生panic.

<h3 id="Value.SetInt">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1431">SetInt</a>
    <a href="#Value.SetInt">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetInt(x <a href="/builtin/#int64">int64</a>)</pre>

SetInt设置v的底层值.如果v的种类不是Int,Int8,Int16,Int32,Int64或者CanSet()返回false,会发生panic.

<h3 id="Value.SetLen">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1452">SetLen</a>
    <a href="#Value.SetLen">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetLen(n <a href="/builtin/#int">int</a>)</pre>

SetCap对v设置一个新的容量.如果v的种类不是切片或者n是负数或者大于容量时,会发生panic.

<h3 id="Value.SetMapIndex">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1481">SetMapIndex</a>
    <a href="#Value.SetMapIndex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetMapIndex(key, val <a href="#Value">Value</a>)</pre>

SetMapIndex设置映射v的键值.如果v不是映射,会发生panic.由于在Go中键的值必须可以赋值给映射的键类型,键值必须可以赋值
给映射的值类型.如果val是nil,SetMapIndex会发生panic.

<h3 id="Value.SetPointer">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1532">SetPointer</a>
    <a href="#Value.SetPointer">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetPointer(x <a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>)</pre>

SetPointer设置不安全指针的值为x.如果v的种类不是UnsafePointer,会发生panic.

<h3 id="Value.SetString">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1540">SetString</a>
    <a href="#Value.SetString">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetString(x <a href="/builtin/#string">string</a>)</pre>

SetString设置v的底层值.如果v的种类不是String或者CanSet()返回false,会发生panic.

<h3 id="Value.SetUint">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1510">SetUint</a>
    <a href="#Value.SetUint">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetUint(x <a href="/builtin/#uint64">uint64</a>)</pre>

SetInt设置v的底层值.如果v的种类不是Uintptr,UInt8,UInt16,UInt32,UInt64或者CanSet()返回false,会发生panic.

<h3 id="Value.Slice">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1549">Slice</a>
    <a href="#Value.Slice">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Slice(i, j <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

Slice设置v[i:j].如果v的种类不是数组,切片,字符串,或者v是一个不可寻址的数组,或者下标超过边界,会发生panic.

<h3 id="Value.Slice3">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1611">Slice3</a>
    <a href="#Value.Slice3">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Slice3(i, j, k <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

Slice3是切片接收3个下标作为参数的操作,它返回v[i:j:k],如果v的种类不是数组或者切片,或者v是一个不可寻址的数组,
或者下标越界会发生panic.

<h3 id="Value.String">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1666">String</a>
    <a href="#Value.String">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) String() <a href="/builtin/#string">string</a></pre>

String返回字符串v的底层值.返回值是一个字符串.由于Go的字符串方法转换,String是一个特殊情况.不像其他的
获取方法.如果v的种类不是字符串,也不会发生panic.相反的是,它会返回一个"<T value>"形式的字符串.T是
v的类型.fmt包处理Value的时候比较特殊,它并不调用隐式调用String方法,而是打印出所含的具象的值.

<h3 id="Value.TryRecv">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1683">TryRecv</a>
    <a href="#Value.TryRecv">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) TryRecv() (x <a href="#Value">Value</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

TryRecv 尝试从通道v中接收值,但并不阻塞.如果v的种类不是通道,它会panic.如果接收到一个值,x是被传输的值,ok是true.
如果不阻塞但也不能结束,x是一个zero值,ok是false.如果通道关闭,x是通道元素类型的zero值,ok是false.

<h3 id="Value.TrySend">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1693">TrySend</a>
    <a href="#Value.TrySend">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) TrySend(x <a href="#Value">Value</a>) <a href="/builtin/#bool">bool</a></pre>

TrySend尝试向通道v发送x,但是不是阻塞.由于Go中x的值必须可以赋值给通道的类型,如果v的种类不是通道,会发生panic.
它返回这个值是否被发送.

<h3 id="Value.Type">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1700">Type</a>
    <a href="#Value.Type">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Type() <a href="#Type">Type</a></pre>

Type返回v的类型

<h3 id="Value.Uint">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1733">Uint</a>
    <a href="#Value.Uint">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Uint() <a href="/builtin/#uint64">uint64</a></pre>

Uint返回v的底层值,返回值的类型是uint64.如果v的种类不是Uint, Uintptr, Uint8, Uint16, Uint32, or Uint64,
会发生panic.

<h3 id="Value.UnsafeAddr">func (Value) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L1756">UnsafeAddr</a>
    <a href="#Value.UnsafeAddr">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) UnsafeAddr() <a href="/builtin/#uintptr">uintptr</a></pre>

UnsafeAddr返回一个指向v数据的指针.这是为了引用unsafe包的高级客户端准备的.当v不可寻址时,会发生panic.

<h2 id="ValueError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L147">ValueError</a>
    <a href="#ValueError">¶</a></h2>
<pre>type ValueError struct {
<span id="ValueError.Method"></span>    Method <a href="/builtin/#string">string</a>
<span id="ValueError.Kind"></span>    Kind   <a href="#Kind">Kind</a>
}</pre>
当一个Value方法在不支持它的Value上触发的时候,ValueError发生.这样的例子会在每个方法的文档上注明.

<h3 id="ValueError.Error">func (*ValueError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/reflect/value.go#L152">Error</a>
    <a href="#ValueError.Error">¶</a></h3>
<pre>func (e *<a href="#ValueError">ValueError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/release-branch.go1.10/src/reflect/type.go#L200)  如果结构体字段的名字一样,即使它们是不同包内的非导出名称,FieldByName和相应的函数会把它们视为相等.实际使用中的效果是t.FieldByName("x")的结果并不能很好的定义,如果类型为t的结构体包含多个以x命名的字段.的结果并不能很好的定义,如果类型为t的结构体包含多个以x命名的字段.FieldByName可能返回其中任意一个,并忽略其他的.更多细节参见https://golang.org/issue/4876


