version: 1.9.2
## package reflect

  `import "reflect"`

## Overview

Package reflect implements run-time reflection, allowing a program to manipulate
objects with arbitrary types. The typical use is to take a value with static
type interface{} and extract its dynamic type information by calling TypeOf,
which returns a Type.

A call to ValueOf returns a Value representing the run-time data. Zero takes a
Type and returns a Value representing a zero value for that type.

See "The Laws of Reflection" for an introduction to reflection in Go:
https://golang.org/doc/articles/laws_of_reflection.html

## Index

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

### Examples

- [MakeFunc](#exampleMakeFunc)
- [StructOf](#exampleStructOf)
- [StructTag](#exampleStructTag)
- [StructTag.Lookup](#exampleStructTag_Lookup)
- [TypeOf](#exampleTypeOf)

### Package files
 [deepequal.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/deepequal.go) [makefunc.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/makefunc.go) [swapper.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/swapper.go) [type.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go) [value.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go)

<h2 id="Copy">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1851">Copy</a>
    <a href="#Copy">¶</a></h2>
<pre>func Copy(dst, src <a href="#Value">Value</a>) <a href="/builtin/#int">int</a></pre>

Copy copies the contents of src into dst until either dst has been filled or src
has been exhausted. It returns the number of elements copied. Dst and src each
must have kind Slice or Array, and dst and src must have the same element type.

<h2 id="DeepEqual">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/deepequal.go#L177">DeepEqual</a>
    <a href="#DeepEqual">¶</a></h2>
<pre>func DeepEqual(x, y interface{}) <a href="/builtin/#bool">bool</a></pre>

DeepEqual reports whether x and y are ``deeply equal,'' defined as follows. Two
values of identical type are deeply equal if one of the following cases applies.
Values of distinct types are never deeply equal.

Array values are deeply equal when their corresponding elements are deeply
equal.

Struct values are deeply equal if their corresponding fields, both exported and
unexported, are deeply equal.

Func values are deeply equal if both are nil; otherwise they are not deeply
equal.

Interface values are deeply equal if they hold deeply equal concrete values.

Map values are deeply equal when all of the following are true: they are both
nil or both non-nil, they have the same length, and either they are the same map
object or their corresponding keys (matched using Go equality) map to deeply
equal values.

Pointer values are deeply equal if they are equal using Go's == operator or if
they point to deeply equal values.

Slice values are deeply equal when all of the following are true: they are both
nil or both non-nil, they have the same length, and either they point to the
same initial entry of the same underlying array (that is, &x[0] == &y[0]) or
their corresponding elements (up to length) are deeply equal. Note that a
non-nil empty slice and a nil slice (for example, []byte{} and []byte(nil)) are
not deeply equal.

Other values - numbers, bools, strings, and channels - are deeply equal if they
are equal using Go's == operator.

In general DeepEqual is a recursive relaxation of Go's == operator. However,
this idea is impossible to implement without some inconsistency. Specifically,
it is possible for a value to be unequal to itself, either because it is of func
type (uncomparable in general) or because it is a floating-point NaN value (not
equal to itself in floating-point comparison), or because it is an array,
struct, or interface containing such a value. On the other hand, pointer values
are always equal to themselves, even if they point at or contain such
problematic values, because they compare equal using Go's == operator, and that
is a sufficient condition to be deeply equal, regardless of content. DeepEqual
has been defined so that the same short-cut applies to slices and maps: if x and
y are the same slice or the same map, they are deeply equal regardless of
content.

As DeepEqual traverses the data values it may find a cycle. The second and
subsequent times that DeepEqual compares two pointer values that have been
compared before, it treats the values as equal rather than examining the values
to which they point. This ensures that DeepEqual terminates.

<h2 id="Select">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1948">Select</a>
    <a href="#Select">¶</a></h2>
<pre>func Select(cases []<a href="#SelectCase">SelectCase</a>) (chosen <a href="/builtin/#int">int</a>, recv <a href="#Value">Value</a>, recvOK <a href="/builtin/#bool">bool</a>)</pre>

Select executes a select operation described by the list of cases. Like the Go
select statement, it blocks until at least one of the cases can proceed, makes a
uniform pseudo-random choice, and then executes that case. It returns the index
of the chosen case and, if that case was a receive operation, the value received
and a boolean indicating whether the value corresponds to a send on the channel
(as opposed to a zero value received because the channel is closed).

<h2 id="Swapper">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/swapper.go#L3">Swapper</a>
    <a href="#Swapper">¶</a></h2>
<pre>func Swapper(slice interface{}) func(i, j <a href="/builtin/#int">int</a>)</pre>

Swapper returns a function that swaps the elements in the provided slice.

Swapper panics if the provided interface is not a slice.

<h2 id="ChanDir">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L331">ChanDir</a>
    <a href="#ChanDir">¶</a></h2>
<pre>type ChanDir <a href="/builtin/#int">int</a></pre>

ChanDir represents a channel type's direction.

<pre>const (
    <span id="RecvDir">RecvDir</span> <a href="#ChanDir">ChanDir</a>             = 1 &lt;&lt; <a href="/builtin/#iota">iota</a> <span class="comment">// &lt;-chan</span>
    <span id="SendDir">SendDir</span>                                 <span class="comment">// chan&lt;-</span>
    <span id="BothDir">BothDir</span> = <a href="#RecvDir">RecvDir</a> | <a href="#SendDir">SendDir</a>             <span class="comment">// chan</span>
)</pre>


<h3 id="ChanDir.String">func (ChanDir) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L1057">String</a>
    <a href="#ChanDir.String">¶</a></h3>
<pre>func (d <a href="#ChanDir">ChanDir</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Kind">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L215">Kind</a>
    <a href="#Kind">¶</a></h2>
<pre>type Kind <a href="/builtin/#uint">uint</a></pre>

A Kind represents the specific kind of type that a Type represents. The zero
Kind is not a valid kind.

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


<h3 id="Kind.String">func (Kind) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L592">String</a>
    <a href="#Kind.String">¶</a></h3>
<pre>func (k <a href="#Kind">Kind</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Method">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L570">Method</a>
    <a href="#Method">¶</a></h2>
<pre>type Method struct {
<span id="Method.Name"></span>    <span class="comment">// Name is the method name.</span>
<span id="Method.PkgPath"></span>    <span class="comment">// PkgPath is the package path that qualifies a lower case (unexported)</span>
    <span class="comment">// method name. It is empty for upper case (exported) method names.</span>
    <span class="comment">// The combination of PkgPath and Name uniquely identifies a method</span>
    <span class="comment">// in a method set.</span>
    <span class="comment">// See https://golang.org/ref/spec#Uniqueness_of_identifiers</span>
    Name    <a href="/builtin/#string">string</a>
    PkgPath <a href="/builtin/#string">string</a>

<span id="Method.Type"></span>    Type  <a href="#Type">Type</a>  <span class="comment">// method type</span>
<span id="Method.Func"></span>    Func  <a href="#Value">Value</a> <span class="comment">// func with receiver as first argument</span>
<span id="Method.Index"></span>    Index <a href="/builtin/#int">int</a>   <span class="comment">// index for Type.Method</span>
}</pre>

Method represents a single method.

<h2 id="SelectCase">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1935">SelectCase</a>
    <a href="#SelectCase">¶</a></h2>
<pre>type SelectCase struct {
<span id="SelectCase.Dir"></span>    Dir  <a href="#SelectDir">SelectDir</a> <span class="comment">// direction of case</span>
<span id="SelectCase.Chan"></span>    Chan <a href="#Value">Value</a>     <span class="comment">// channel to use (for send or receive)</span>
<span id="SelectCase.Send"></span>    Send <a href="#Value">Value</a>     <span class="comment">// value to send (for send)</span>
}</pre>

A SelectCase describes a single case in a select operation. The kind of case
depends on Dir, the communication direction.

If Dir is SelectDefault, the case represents a default case. Chan and Send must
be zero Values.

If Dir is SelectSend, the case represents a send operation. Normally Chan's
underlying value must be a channel, and Send's underlying value must be
assignable to the channel's element type. As a special case, if Chan is a zero
Value, then the case is ignored, and the field Send will also be ignored and may
be either zero or non-zero.

If Dir is SelectRecv, the case represents a receive operation. Normally Chan's
underlying value must be a channel and Send must be a zero Value. If Chan is a
zero Value, then the case is ignored, but Send must still be a zero Value. When
a receive operation is selected, the received Value is returned by Select.

<h2 id="SelectDir">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1907">SelectDir</a>
    <a href="#SelectDir">¶</a></h2>
<pre>type SelectDir <a href="/builtin/#int">int</a></pre>

A SelectDir describes the communication direction of a select case.

<pre>const (
    <span id="SelectSend">SelectSend</span>    <a href="#SelectDir">SelectDir</a> <span class="comment">// case Chan &lt;- Send</span>
    <span id="SelectRecv">SelectRecv</span>              <span class="comment">// case &lt;-Chan:</span>
    <span id="SelectDefault">SelectDefault</span>           <span class="comment">// default</span>
)</pre>


<h2 id="SliceHeader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1771">SliceHeader</a>
    <a href="#SliceHeader">¶</a></h2>
<pre>type SliceHeader struct {
<span id="SliceHeader.Data"></span>    Data <a href="/builtin/#uintptr">uintptr</a>
<span id="SliceHeader.Len"></span>    Len  <a href="/builtin/#int">int</a>
<span id="SliceHeader.Cap"></span>    Cap  <a href="/builtin/#int">int</a>
}</pre>

SliceHeader is the runtime representation of a slice. It cannot be used safely
or portably and its representation may change in a later release. Moreover, the
Data field is not sufficient to guarantee the data it references will not be
garbage collected, so programs must keep a separate, correctly typed pointer to
the underlying data.

<h2 id="StringHeader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1754">StringHeader</a>
    <a href="#StringHeader">¶</a></h2>
<pre>type StringHeader struct {
<span id="StringHeader.Data"></span>    Data <a href="/builtin/#uintptr">uintptr</a>
<span id="StringHeader.Len"></span>    Len  <a href="/builtin/#int">int</a>
}</pre>

StringHeader is the runtime representation of a string. It cannot be used safely
or portably and its representation may change in a later release. Moreover, the
Data field is not sufficient to guarantee the data it references will not be
garbage collected, so programs must keep a separate, correctly typed pointer to
the underlying data.

<h2 id="StructField">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L1107">StructField</a>
    <a href="#StructField">¶</a></h2>
<pre>type StructField struct {
<span id="StructField.Name"></span>    <span class="comment">// Name is the field name.</span>
    Name <a href="/builtin/#string">string</a>
<span id="StructField.PkgPath"></span>    <span class="comment">// PkgPath is the package path that qualifies a lower case (unexported)</span>
    <span class="comment">// field name. It is empty for upper case (exported) field names.</span>
    <span class="comment">// See https://golang.org/ref/spec#Uniqueness_of_identifiers</span>
    PkgPath <a href="/builtin/#string">string</a>

<span id="StructField.Type"></span>    Type      <a href="#Type">Type</a>      <span class="comment">// field type</span>
<span id="StructField.Tag"></span>    Tag       <a href="#StructTag">StructTag</a> <span class="comment">// field tag string</span>
<span id="StructField.Offset"></span>    Offset    <a href="/builtin/#uintptr">uintptr</a>   <span class="comment">// offset within struct, in bytes</span>
<span id="StructField.Index"></span>    Index     []<a href="/builtin/#int">int</a>     <span class="comment">// index sequence for Type.FieldByIndex</span>
<span id="StructField.Anonymous"></span>    Anonymous <a href="/builtin/#bool">bool</a>      <span class="comment">// is an embedded field</span>
}</pre>

A StructField describes a single field in a struct.

<h2 id="StructTag">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L1130">StructTag</a>
    <a href="#StructTag">¶</a></h2>
<pre>type StructTag <a href="/builtin/#string">string</a></pre>

A StructTag is the tag string in a struct field.

By convention, tag strings are a concatenation of optionally space-separated
key:"value" pairs. Each key is a non-empty string consisting of non-control
characters other than space (U+0020 ' '), quote (U+0022 '"'), and colon (U+003A
':'). Each value is quoted using U+0022 '"' characters and Go string literal
syntax.

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

<h3 id="StructTag.Get">func (StructTag) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L1137">Get</a>
    <a href="#StructTag.Get">¶</a></h3>
<pre>func (tag <a href="#StructTag">StructTag</a>) Get(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Get returns the value associated with key in the tag string. If there is no such
key in the tag, Get returns the empty string. If the tag does not have the
conventional format, the value returned by Get is unspecified. To determine
whether a tag is explicitly set to the empty string, use Lookup.

<h3 id="StructTag.Lookup">func (StructTag) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L1148">Lookup</a>
    <a href="#StructTag.Lookup">¶</a></h3>
<pre>func (tag <a href="#StructTag">StructTag</a>) Lookup(key <a href="/builtin/#string">string</a>) (value <a href="/builtin/#string">string</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

Lookup returns the value associated with key in the tag string. If the key is
present in the tag the value (which may be empty) is returned. Otherwise the
returned value will be the empty string. The ok return value reports whether the
value was explicitly set in the tag string. If the tag does not have the
conventional format, the value returned by Lookup is unspecified.

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

<h2 id="Type">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L27">Type</a>
    <a href="#Type">¶</a></h2>
<pre>type Type interface {

    <span class="comment">// Align returns the alignment in bytes of a value of</span>
    <span class="comment">// this type when allocated in memory.</span>
    Align() <a href="/builtin/#int">int</a>

    <span class="comment">// FieldAlign returns the alignment in bytes of a value of</span>
    <span class="comment">// this type when used as a field in a struct.</span>
    FieldAlign() <a href="/builtin/#int">int</a>

    <span class="comment">// Method returns the i&#39;th method in the type&#39;s method set.</span>
    <span class="comment">// It panics if i is not in the range [0, NumMethod()).</span>
    <span class="comment">//</span>
    <span class="comment">// For a non-interface type T or *T, the returned Method&#39;s Type and Func</span>
    <span class="comment">// fields describe a function whose first argument is the receiver.</span>
    <span class="comment">//</span>
    <span class="comment">// For an interface type, the returned Method&#39;s Type field gives the</span>
    <span class="comment">// method signature, without a receiver, and the Func field is nil.</span>
    Method(<a href="/builtin/#int">int</a>) <a href="#Method">Method</a>

    <span class="comment">// MethodByName returns the method with that name in the type&#39;s</span>
    <span class="comment">// method set and a boolean indicating if the method was found.</span>
    <span class="comment">//</span>
    <span class="comment">// For a non-interface type T or *T, the returned Method&#39;s Type and Func</span>
    <span class="comment">// fields describe a function whose first argument is the receiver.</span>
    <span class="comment">//</span>
    <span class="comment">// For an interface type, the returned Method&#39;s Type field gives the</span>
    <span class="comment">// method signature, without a receiver, and the Func field is nil.</span>
    MethodByName(<a href="/builtin/#string">string</a>) (<a href="#Method">Method</a>, <a href="/builtin/#bool">bool</a>)

    <span class="comment">// NumMethod returns the number of exported methods in the type&#39;s method set.</span>
    NumMethod() <a href="/builtin/#int">int</a>

    <span class="comment">// Name returns the type&#39;s name within its package.</span>
    <span class="comment">// It returns an empty string for unnamed types.</span>
    Name() <a href="/builtin/#string">string</a>

    <span class="comment">// PkgPath returns a named type&#39;s package path, that is, the import path</span>
    <span class="comment">// that uniquely identifies the package, such as &#34;encoding/base64&#34;.</span>
    <span class="comment">// If the type was predeclared (string, error) or unnamed (*T, struct{}, []int),</span>
    <span class="comment">// the package path will be the empty string.</span>
    PkgPath() <a href="/builtin/#string">string</a>

    <span class="comment">// Size returns the number of bytes needed to store</span>
    <span class="comment">// a value of the given type; it is analogous to unsafe.Sizeof.</span>
    Size() <a href="/builtin/#uintptr">uintptr</a>

    <span class="comment">// String returns a string representation of the type.</span>
    <span class="comment">// The string representation may use shortened package names</span>
    <span class="comment">// (e.g., base64 instead of &#34;encoding/base64&#34;) and is not</span>
    <span class="comment">// guaranteed to be unique among types. To test for type identity,</span>
    <span class="comment">// compare the Types directly.</span>
    String() <a href="/builtin/#string">string</a>

    <span class="comment">// Kind returns the specific kind of this type.</span>
    Kind() <a href="#Kind">Kind</a>

    <span class="comment">// Implements reports whether the type implements the interface type u.</span>
    Implements(u <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a>

    <span class="comment">// AssignableTo reports whether a value of the type is assignable to type u.</span>
    AssignableTo(u <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a>

    <span class="comment">// ConvertibleTo reports whether a value of the type is convertible to type u.</span>
    ConvertibleTo(u <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a>

    <span class="comment">// Comparable reports whether values of this type are comparable.</span>
    Comparable() <a href="/builtin/#bool">bool</a>

    <span class="comment">// Bits returns the size of the type in bits.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not one of the</span>
    <span class="comment">// sized or unsized Int, Uint, Float, or Complex kinds.</span>
    Bits() <a href="/builtin/#int">int</a>

    <span class="comment">// ChanDir returns a channel type&#39;s direction.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Chan.</span>
    ChanDir() <a href="#ChanDir">ChanDir</a>

    <span class="comment">// IsVariadic reports whether a function type&#39;s final input parameter</span>
    <span class="comment">// is a &#34;...&#34; parameter. If so, t.In(t.NumIn() - 1) returns the parameter&#39;s</span>
    <span class="comment">// implicit actual type []T.</span>
    <span class="comment">//</span>
    <span class="comment">// For concreteness, if t represents func(x int, y ... float64), then</span>
    <span class="comment">//</span>
    <span class="comment">//	t.NumIn() == 2</span>
    <span class="comment">//	t.In(0) is the reflect.Type for &#34;int&#34;</span>
    <span class="comment">//	t.In(1) is the reflect.Type for &#34;[]float64&#34;</span>
    <span class="comment">//	t.IsVariadic() == true</span>
    <span class="comment">//</span>
    <span class="comment">// IsVariadic panics if the type&#39;s Kind is not Func.</span>
    IsVariadic() <a href="/builtin/#bool">bool</a>

    <span class="comment">// Elem returns a type&#39;s element type.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Array, Chan, Map, Ptr, or Slice.</span>
    Elem() <a href="#Type">Type</a>

    <span class="comment">// Field returns a struct type&#39;s i&#39;th field.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Struct.</span>
    <span class="comment">// It panics if i is not in the range [0, NumField()).</span>
    Field(i <a href="/builtin/#int">int</a>) <a href="#StructField">StructField</a>

    <span class="comment">// FieldByIndex returns the nested field corresponding</span>
    <span class="comment">// to the index sequence. It is equivalent to calling Field</span>
    <span class="comment">// successively for each index i.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Struct.</span>
    FieldByIndex(index []<a href="/builtin/#int">int</a>) <a href="#StructField">StructField</a>

    <span class="comment">// FieldByName returns the struct field with the given name</span>
    <span class="comment">// and a boolean indicating if the field was found.</span>
    FieldByName(name <a href="/builtin/#string">string</a>) (<a href="#StructField">StructField</a>, <a href="/builtin/#bool">bool</a>)

    <span class="comment">// FieldByNameFunc returns the struct field with a name</span>
    <span class="comment">// that satisfies the match function and a boolean indicating if</span>
    <span class="comment">// the field was found.</span>
    <span class="comment">//</span>
    <span class="comment">// FieldByNameFunc considers the fields in the struct itself</span>
    <span class="comment">// and then the fields in any anonymous structs, in breadth first order,</span>
    <span class="comment">// stopping at the shallowest nesting depth containing one or more</span>
    <span class="comment">// fields satisfying the match function. If multiple fields at that depth</span>
    <span class="comment">// satisfy the match function, they cancel each other</span>
    <span class="comment">// and FieldByNameFunc returns no match.</span>
    <span class="comment">// This behavior mirrors Go&#39;s handling of name lookup in</span>
    <span class="comment">// structs containing anonymous fields.</span>
    FieldByNameFunc(match func(<a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a>) (<a href="#StructField">StructField</a>, <a href="/builtin/#bool">bool</a>)

    <span class="comment">// In returns the type of a function type&#39;s i&#39;th input parameter.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Func.</span>
    <span class="comment">// It panics if i is not in the range [0, NumIn()).</span>
    In(i <a href="/builtin/#int">int</a>) <a href="#Type">Type</a>

    <span class="comment">// Key returns a map type&#39;s key type.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Map.</span>
    Key() <a href="#Type">Type</a>

    <span class="comment">// Len returns an array type&#39;s length.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Array.</span>
    Len() <a href="/builtin/#int">int</a>

    <span class="comment">// NumField returns a struct type&#39;s field count.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Struct.</span>
    NumField() <a href="/builtin/#int">int</a>

    <span class="comment">// NumIn returns a function type&#39;s input parameter count.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Func.</span>
    NumIn() <a href="/builtin/#int">int</a>

    <span class="comment">// NumOut returns a function type&#39;s output parameter count.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Func.</span>
    NumOut() <a href="/builtin/#int">int</a>

    <span class="comment">// Out returns the type of a function type&#39;s i&#39;th output parameter.</span>
    <span class="comment">// It panics if the type&#39;s Kind is not Func.</span>
    <span class="comment">// It panics if i is not in the range [0, NumOut()).</span>
    Out(i <a href="/builtin/#int">int</a>) <a href="#Type">Type</a>
    <span class="comment">// contains filtered or unexported methods</span>
}</pre>

Type is the representation of a Go type.

Not all methods apply to all kinds of types. Restrictions, if any, are noted in
the documentation for each method. Use the Kind method to find out the kind of
type before calling kind-specific methods. Calling a method inappropriate to the
kind of type causes a run-time panic.

Type values are comparable, such as with the == operator. Two Type values are
equal if they represent identical types.

<h3 id="ArrayOf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L2846">ArrayOf</a>
    <a href="#ArrayOf">¶</a></h3>
<pre>func ArrayOf(count <a href="/builtin/#int">int</a>, elem <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>

ArrayOf returns the array type with the given count and element type. For
example, if t represents int, ArrayOf(5, t) represents [5]int.

If the resulting type would be larger than the available address space, ArrayOf
panics.

<h3 id="ChanOf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L1801">ChanOf</a>
    <a href="#ChanOf">¶</a></h3>
<pre>func ChanOf(dir <a href="#ChanDir">ChanDir</a>, t <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>

ChanOf returns the channel type with the given direction and element type. For
example, if t represents int, ChanOf(RecvDir, t) represents <-chan int.

The gc runtime imposes a limit of 64 kB on channel element types. If t's size is
equal to or exceeds this limit, ChanOf panics.

<h3 id="FuncOf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L1946">FuncOf</a>
    <a href="#FuncOf">¶</a></h3>
<pre>func FuncOf(in, out []<a href="#Type">Type</a>, variadic <a href="/builtin/#bool">bool</a>) <a href="#Type">Type</a></pre>

FuncOf returns the function type with the given argument and result types. For
example if k represents int and e represents string, FuncOf([]Type{k},
[]Type{e}, false) represents func(int) string.

The variadic argument controls whether the function is variadic. FuncOf panics
if the in[len(in)-1] does not represent a slice and variadic is true.

<h3 id="MapOf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L1858">MapOf</a>
    <a href="#MapOf">¶</a></h3>
<pre>func MapOf(key, elem <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>

MapOf returns the map type with the given key and element types. For example, if
k represents int and e represents string, MapOf(k, e) represents map[int]string.

If the key type is not a valid map key type (that is, if it does not implement
Go's == operator), MapOf panics.

<h3 id="PtrTo">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L1398">PtrTo</a>
    <a href="#PtrTo">¶</a></h3>
<pre>func PtrTo(t <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>

PtrTo returns the pointer type with element t. For example, if t represents type
Foo, PtrTo(t) represents *Foo.

<h3 id="SliceOf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L2254">SliceOf</a>
    <a href="#SliceOf">¶</a></h3>
<pre>func SliceOf(t <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>

SliceOf returns the slice type with element type t. For example, if t represents
int, SliceOf(t) represents []int.

<h3 id="StructOf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L2370">StructOf</a>
    <a href="#StructOf">¶</a></h3>
<pre>func StructOf(fields []<a href="#StructField">StructField</a>) <a href="#Type">Type</a></pre>

StructOf returns the struct type containing fields. The Offset and Index fields
are ignored and computed as they would be by the compiler.

StructOf currently does not generate wrapper methods for embedded fields. This
limitation may be lifted in a future version.

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

<h3 id="TypeOf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L1388">TypeOf</a>
    <a href="#TypeOf">¶</a></h3>
<pre>func TypeOf(i interface{}) <a href="#Type">Type</a></pre>

TypeOf returns the reflection Type that represents the dynamic type of i. If i
is a nil interface value, TypeOf returns nil.

<a id="exampleTypeOf"></a>
Example:

    // As interface types are only used for static typing, a
    // common idiom to find the reflection Type for an interface
    // type Foo is to use a *Foo value.
    writerType := reflect.TypeOf((*io.Writer)(nil)).Elem()

    fileType := reflect.TypeOf((*os.File)(nil))
    fmt.Println(fileType.Implements(writerType))

    // Output:
    // true

<h2 id="Value">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L26">Value</a>
    <a href="#Value">¶</a></h2>
<pre>type Value struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Value is the reflection interface to a Go value.

Not all methods apply to all kinds of values. Restrictions, if any, are noted in
the documentation for each method. Use the Kind method to find out the kind of
value before calling kind-specific methods. Calling a method inappropriate to
the kind of type causes a run time panic.

The zero Value represents no value. Its IsValid method returns false, its Kind
method returns Invalid, its String method returns "<invalid Value>", and all
other methods panic. Most functions and methods never return an invalid value.
If one does, its documentation states the conditions explicitly.

A Value can be used concurrently by multiple goroutines provided that the
underlying Go value can be used concurrently for the equivalent direct
operations.

To compare two Values, compare the results of the Interface method. Using == on
two Values does not compare the underlying values they represent.

<h3 id="Append">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1826">Append</a>
    <a href="#Append">¶</a></h3>
<pre>func Append(s <a href="#Value">Value</a>, x ...<a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

Append appends the values x to a slice s and returns the resulting slice. As in
Go, each x's value must be assignable to the slice's element type.

<h3 id="AppendSlice">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1837">AppendSlice</a>
    <a href="#AppendSlice">¶</a></h3>
<pre>func AppendSlice(s, t <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

AppendSlice appends a slice t to a slice s and returns the resulting slice. The
slices s and t must have the same element type.

<h3 id="Indirect">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L2094">Indirect</a>
    <a href="#Indirect">¶</a></h3>
<pre>func Indirect(v <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

Indirect returns the value that v points to. If v is a nil pointer, Indirect
returns a zero Value. If v is not a pointer, Indirect returns v.

<h3 id="MakeChan">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L2062">MakeChan</a>
    <a href="#MakeChan">¶</a></h3>
<pre>func MakeChan(typ <a href="#Type">Type</a>, buffer <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

MakeChan creates a new channel with the specified type and buffer size.

<h3 id="MakeFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/makefunc.go#L37">MakeFunc</a>
    <a href="#MakeFunc">¶</a></h3>
<pre>func MakeFunc(typ <a href="#Type">Type</a>, fn func(args []<a href="#Value">Value</a>) (results []<a href="#Value">Value</a>)) <a href="#Value">Value</a></pre>

MakeFunc returns a new function of the given Type that wraps the function fn.
When called, that new function does the following:

    - converts its arguments to a slice of Values.
    - runs results := fn(args).
    - returns the results as a slice of Values, one per formal result.

The implementation fn can assume that the argument Value slice has the number
and type of arguments given by typ. If typ describes a variadic function, the
final Value is itself a slice representing the variadic arguments, as in the
body of a variadic function. The result Value slice returned by fn must have the
number and type of results given by typ.

The Value.Call method allows the caller to invoke a typed function in terms of
Values; in contrast, MakeFunc allows the caller to implement a typed function in
terms of Values.

The Examples section of the documentation includes an illustration of how to use
MakeFunc to build a swap function for different types.

<a id="exampleMakeFunc"></a>
Example:

    // swap is the implementation passed to MakeFunc.
    // It must work in terms of reflect.Values so that it is possible
    // to write code without knowing beforehand what the types
    // will be.
    swap := func(in []reflect.Value) []reflect.Value {
        return []reflect.Value{in[1], in[0]}
    }

    // makeSwap expects fptr to be a pointer to a nil function.
    // It sets that pointer to a new function created with MakeFunc.
    // When the function is invoked, reflect turns the arguments
    // into Values, calls swap, and then turns swap's result slice
    // into the values returned by the new function.
    makeSwap := func(fptr interface{}) {
        // fptr is a pointer to a function.
        // Obtain the function value itself (likely nil) as a reflect.Value
        // so that we can query its type and then set the value.
        fn := reflect.ValueOf(fptr).Elem()

        // Make a function of the right type.
        v := reflect.MakeFunc(fn.Type(), swap)

        // Assign it to the value fn represents.
        fn.Set(v)
    }

    // Make and call a swap function for ints.
    var intSwap func(int, int) (int, int)
    makeSwap(&intSwap)
    fmt.Println(intSwap(0, 1))

    // Make and call a swap function for float64s.
    var floatSwap func(float64, float64) (float64, float64)
    makeSwap(&floatSwap)
    fmt.Println(floatSwap(2.72, 3.14))

    // Output:
    // 1 0
    // 3.14 2.72

<h3 id="MakeMap">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L2077">MakeMap</a>
    <a href="#MakeMap">¶</a></h3>
<pre>func MakeMap(typ <a href="#Type">Type</a>) <a href="#Value">Value</a></pre>

MakeMap creates a new map with the specified type.

<h3 id="MakeMapWithSize">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L2083">MakeMapWithSize</a>
    <a href="#MakeMapWithSize">¶</a></h3>
<pre>func MakeMapWithSize(typ <a href="#Type">Type</a>, n <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

MakeMapWithSize creates a new map with the specified type and initial space for
approximately n elements.

<h3 id="MakeSlice">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L2043">MakeSlice</a>
    <a href="#MakeSlice">¶</a></h3>
<pre>func MakeSlice(typ <a href="#Type">Type</a>, len, cap <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

MakeSlice creates a new zero-initialized slice value for the specified slice
type, length, and capacity.

<h3 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L2136">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(typ <a href="#Type">Type</a>) <a href="#Value">Value</a></pre>

New returns a Value representing a pointer to a new zero value for the specified
type. That is, the returned Value's Type is PtrTo(typ).

<h3 id="NewAt">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L2147">NewAt</a>
    <a href="#NewAt">¶</a></h3>
<pre>func NewAt(typ <a href="#Type">Type</a>, p <a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>) <a href="#Value">Value</a></pre>

NewAt returns a Value representing a pointer to a value of the specified type,
using p as that pointer.

<h3 id="ValueOf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L2103">ValueOf</a>
    <a href="#ValueOf">¶</a></h3>
<pre>func ValueOf(i interface{}) <a href="#Value">Value</a></pre>

ValueOf returns a new Value initialized to the concrete value stored in the
interface i. ValueOf(nil) returns the zero Value.

<h3 id="Zero">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L2122">Zero</a>
    <a href="#Zero">¶</a></h3>
<pre>func Zero(typ <a href="#Type">Type</a>) <a href="#Value">Value</a></pre>

Zero returns a Value representing the zero value for the specified type. The
result is different from the zero value of the Value struct, which represents no
value at all. For example, Zero(TypeOf(42)) returns a Value with Kind Int and
value 0. The returned value is neither addressable nor settable.

<h3 id="Value.Addr">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L227">Addr</a>
    <a href="#Value.Addr">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Addr() <a href="#Value">Value</a></pre>

Addr returns a pointer value representing the address of v. It panics if
CanAddr() returns false. Addr is typically used to obtain a pointer to a struct
field or slice element in order to call a method that requires a pointer
receiver.

<h3 id="Value.Bool">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L236">Bool</a>
    <a href="#Value.Bool">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Bool() <a href="/builtin/#bool">bool</a></pre>

Bool returns v's underlying value. It panics if v's kind is not Bool.

<h3 id="Value.Bytes">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L243">Bytes</a>
    <a href="#Value.Bytes">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Bytes() []<a href="/builtin/#byte">byte</a></pre>

Bytes returns v's underlying value. It panics if v's underlying value is not a
slice of bytes.

<h3 id="Value.Call">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L289">Call</a>
    <a href="#Value.Call">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Call(in []<a href="#Value">Value</a>) []<a href="#Value">Value</a></pre>

Call calls the function v with the input arguments in. For example, if len(in)
== 3, v.Call(in) represents the Go call v(in[0], in[1], in[2]). Call panics if
v's Kind is not Func. It returns the output results as Values. As in Go, each
input argument must be assignable to the type of the function's corresponding
input parameter. If v is a variadic function, Call creates the variadic slice
parameter itself, copying in the corresponding values.

<h3 id="Value.CallSlice">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L302">CallSlice</a>
    <a href="#Value.CallSlice">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) CallSlice(in []<a href="#Value">Value</a>) []<a href="#Value">Value</a></pre>

CallSlice calls the variadic function v with the input arguments in, assigning
the slice in[len(in)-1] to v's final variadic argument. For example, if len(in)
== 3, v.CallSlice(in) represents the Go call v(in[0], in[1], in[2]...).
CallSlice panics if v's Kind is not Func or if v is not variadic. It returns the
output results as Values. As in Go, each input argument must be assignable to
the type of the function's corresponding input parameter.

<h3 id="Value.CanAddr">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L268">CanAddr</a>
    <a href="#Value.CanAddr">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) CanAddr() <a href="/builtin/#bool">bool</a></pre>

CanAddr reports whether the value's address can be obtained with Addr. Such
values are called addressable. A value is addressable if it is an element of a
slice, an element of an addressable array, a field of an addressable struct, or
the result of dereferencing a pointer. If CanAddr returns false, calling Addr
will panic.

<h3 id="Value.CanInterface">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L908">CanInterface</a>
    <a href="#Value.CanInterface">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) CanInterface() <a href="/builtin/#bool">bool</a></pre>

CanInterface reports whether Interface can be used without panicking.

<h3 id="Value.CanSet">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L277">CanSet</a>
    <a href="#Value.CanSet">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) CanSet() <a href="/builtin/#bool">bool</a></pre>

CanSet reports whether the value of v can be changed. A Value can be changed
only if it is addressable and was not obtained by the use of unexported struct
fields. If CanSet returns false, calling Set or any type-specific setter (e.g.,
SetBool, SetInt) will panic.

<h3 id="Value.Cap">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L677">Cap</a>
    <a href="#Value.Cap">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Cap() <a href="/builtin/#int">int</a></pre>

Cap returns v's capacity. It panics if v's Kind is not Array, Chan, or Slice.

<h3 id="Value.Close">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L693">Close</a>
    <a href="#Value.Close">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Close()</pre>

Close closes the channel v. It panics if v's Kind is not Chan.

<h3 id="Value.Complex">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L701">Complex</a>
    <a href="#Value.Complex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Complex() <a href="/builtin/#complex128">complex128</a></pre>

Complex returns v's underlying value, as a complex128. It panics if v's Kind is
not Complex64 or Complex128

<h3 id="Value.Convert">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L2188">Convert</a>
    <a href="#Value.Convert">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Convert(t <a href="#Type">Type</a>) <a href="#Value">Value</a></pre>

Convert returns the value v converted to type t. If the usual Go conversion
rules do not allow conversion of the value v to type t, Convert panics.

<h3 id="Value.Elem">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L716">Elem</a>
    <a href="#Value.Elem">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Elem() <a href="#Value">Value</a></pre>

Elem returns the value that the interface v contains or that the pointer v
points to. It panics if v's Kind is not Interface or Ptr. It returns the zero
Value if v is nil.

<h3 id="Value.Field">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L753">Field</a>
    <a href="#Value.Field">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Field(i <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

Field returns the i'th field of the struct v. It panics if v's Kind is not
Struct or i is out of range.

<h3 id="Value.FieldByIndex">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L785">FieldByIndex</a>
    <a href="#Value.FieldByIndex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) FieldByIndex(index []<a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

FieldByIndex returns the nested field corresponding to index. It panics if v's
Kind is not struct.

<h3 id="Value.FieldByName">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L807">FieldByName</a>
    <a href="#Value.FieldByName">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) FieldByName(name <a href="/builtin/#string">string</a>) <a href="#Value">Value</a></pre>

FieldByName returns the struct field with the given name. It returns the zero
Value if no field was found. It panics if v's Kind is not struct.

<h3 id="Value.FieldByNameFunc">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L819">FieldByNameFunc</a>
    <a href="#Value.FieldByNameFunc">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) FieldByNameFunc(match func(<a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a>) <a href="#Value">Value</a></pre>

FieldByNameFunc returns the struct field with a name that satisfies the match
function. It panics if v's Kind is not struct. It returns the zero Value if no
field was found.

<h3 id="Value.Float">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L828">Float</a>
    <a href="#Value.Float">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Float() <a href="/builtin/#float64">float64</a></pre>

Float returns v's underlying value, as a float64. It panics if v's Kind is not
Float32 or Float64

<h3 id="Value.Index">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L843">Index</a>
    <a href="#Value.Index">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Index(i <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

Index returns v's i'th element. It panics if v's Kind is not Array, Slice, or
String or i is out of range.

<h3 id="Value.Int">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L889">Int</a>
    <a href="#Value.Int">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Int() <a href="/builtin/#int64">int64</a></pre>

Int returns v's underlying value, as an int64. It panics if v's Kind is not Int,
Int8, Int16, Int32, or Int64.

<h3 id="Value.Interface">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L920">Interface</a>
    <a href="#Value.Interface">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Interface() (i interface{})</pre>

Interface returns v's current value as an interface{}. It is equivalent to:

    var i interface{} = (v's underlying value)

It panics if the Value was obtained by accessing unexported struct fields.

<h3 id="Value.InterfaceData">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L956">InterfaceData</a>
    <a href="#Value.InterfaceData">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) InterfaceData() [2]<a href="/builtin/#uintptr">uintptr</a></pre>

InterfaceData returns the interface v's value as a uintptr pair. It panics if
v's Kind is not Interface.

<h3 id="Value.IsNil">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L974">IsNil</a>
    <a href="#Value.IsNil">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) IsNil() <a href="/builtin/#bool">bool</a></pre>

IsNil reports whether its argument v is nil. The argument must be a chan, func,
interface, map, pointer, or slice value; if it is not, IsNil panics. Note that
IsNil is not always equivalent to a regular comparison with nil in Go. For
example, if v was created by calling ValueOf with an uninitialized interface
variable i, i==nil will be true but v.IsNil will panic as v will be the zero
Value.

<h3 id="Value.IsValid">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L999">IsValid</a>
    <a href="#Value.IsValid">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) IsValid() <a href="/builtin/#bool">bool</a></pre>

IsValid reports whether v represents a value. It returns false if v is the zero
Value. If IsValid returns false, all other methods except String panic. Most
functions and methods never return an invalid value. If one does, its
documentation states the conditions explicitly.

<h3 id="Value.Kind">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1005">Kind</a>
    <a href="#Value.Kind">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Kind() <a href="#Kind">Kind</a></pre>

Kind returns v's Kind. If v is the zero Value (IsValid returns false), Kind
returns Invalid.

<h3 id="Value.Len">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1011">Len</a>
    <a href="#Value.Len">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Len() <a href="/builtin/#int">int</a></pre>

Len returns v's length. It panics if v's Kind is not Array, Chan, Map, Slice, or
String.

<h3 id="Value.MapIndex">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1035">MapIndex</a>
    <a href="#Value.MapIndex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) MapIndex(key <a href="#Value">Value</a>) <a href="#Value">Value</a></pre>

MapIndex returns the value associated with key in the map v. It panics if v's
Kind is not Map. It returns the zero Value if key is not found in the map or if
v represents a nil map. As in Go, the key's value must be assignable to the
map's key type.

<h3 id="Value.MapKeys">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1076">MapKeys</a>
    <a href="#Value.MapKeys">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) MapKeys() []<a href="#Value">Value</a></pre>

MapKeys returns a slice containing all the keys present in the map, in
unspecified order. It panics if v's Kind is not Map. It returns an empty slice
if v represents a nil map.

<h3 id="Value.Method">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1117">Method</a>
    <a href="#Value.Method">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Method(i <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

Method returns a function value corresponding to v's i'th method. The arguments
to a Call on the returned function should not include a receiver; the returned
function will always use v as the receiver. Method panics if i is out of range
or if v is a nil interface value.

<h3 id="Value.MethodByName">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1149">MethodByName</a>
    <a href="#Value.MethodByName">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) MethodByName(name <a href="/builtin/#string">string</a>) <a href="#Value">Value</a></pre>

MethodByName returns a function value corresponding to the method of v with the
given name. The arguments to a Call on the returned function should not include
a receiver; the returned function will always use v as the receiver. It returns
the zero Value if no method was found.

<h3 id="Value.NumField">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1165">NumField</a>
    <a href="#Value.NumField">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) NumField() <a href="/builtin/#int">int</a></pre>

NumField returns the number of fields in the struct v. It panics if v's Kind is
not Struct.

<h3 id="Value.NumMethod">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1134">NumMethod</a>
    <a href="#Value.NumMethod">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) NumMethod() <a href="/builtin/#int">int</a></pre>

NumMethod returns the number of exported methods in the value's method set.

<h3 id="Value.OverflowComplex">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1173">OverflowComplex</a>
    <a href="#Value.OverflowComplex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) OverflowComplex(x <a href="/builtin/#complex128">complex128</a>) <a href="/builtin/#bool">bool</a></pre>

OverflowComplex reports whether the complex128 x cannot be represented by v's
type. It panics if v's Kind is not Complex64 or Complex128.

<h3 id="Value.OverflowFloat">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1186">OverflowFloat</a>
    <a href="#Value.OverflowFloat">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) OverflowFloat(x <a href="/builtin/#float64">float64</a>) <a href="/builtin/#bool">bool</a></pre>

OverflowFloat reports whether the float64 x cannot be represented by v's type.
It panics if v's Kind is not Float32 or Float64.

<h3 id="Value.OverflowInt">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1206">OverflowInt</a>
    <a href="#Value.OverflowInt">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) OverflowInt(x <a href="/builtin/#int64">int64</a>) <a href="/builtin/#bool">bool</a></pre>

OverflowInt reports whether the int64 x cannot be represented by v's type. It
panics if v's Kind is not Int, Int8, int16, Int32, or Int64.

<h3 id="Value.OverflowUint">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1219">OverflowUint</a>
    <a href="#Value.OverflowUint">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) OverflowUint(x <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#bool">bool</a></pre>

OverflowUint reports whether the uint64 x cannot be represented by v's type. It
panics if v's Kind is not Uint, Uintptr, Uint8, Uint16, Uint32, or Uint64.

<h3 id="Value.Pointer">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1244">Pointer</a>
    <a href="#Value.Pointer">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Pointer() <a href="/builtin/#uintptr">uintptr</a></pre>

Pointer returns v's value as a uintptr. It returns uintptr instead of
unsafe.Pointer so that code using reflect cannot obtain unsafe.Pointers without
importing the unsafe package explicitly. It panics if v's Kind is not Chan,
Func, Map, Ptr, Slice, or UnsafePointer.

If v's Kind is Func, the returned pointer is an underlying code pointer, but not
necessarily enough to identify a single function uniquely. The only guarantee is
that the result is zero if and only if v is a nil func Value.

If v's Kind is Slice, the returned pointer is to the first element of the slice.
If the slice is nil the returned value is 0. If the slice is empty but non-nil
the return value is non-zero.

<h3 id="Value.Recv">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1280">Recv</a>
    <a href="#Value.Recv">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Recv() (x <a href="#Value">Value</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

Recv receives and returns a value from the channel v. It panics if v's Kind is
not Chan. The receive blocks until a value is ready. The boolean value ok is
true if the value x corresponds to a send on the channel, false if it is a zero
value received because the channel is closed.

<h3 id="Value.Send">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1313">Send</a>
    <a href="#Value.Send">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Send(x <a href="#Value">Value</a>)</pre>

Send sends x on the channel v. It panics if v's kind is not Chan or if x's type
is not the same type as v's element type. As in Go, x's value must be assignable
to the channel's element type.

<h3 id="Value.Set">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1340">Set</a>
    <a href="#Value.Set">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Set(x <a href="#Value">Value</a>)</pre>

Set assigns x to the value v. It panics if CanSet returns false. As in Go, x's
value must be assignable to v's type.

<h3 id="Value.SetBool">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1357">SetBool</a>
    <a href="#Value.SetBool">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetBool(x <a href="/builtin/#bool">bool</a>)</pre>

SetBool sets v's underlying value. It panics if v's Kind is not Bool or if
CanSet() is false.

<h3 id="Value.SetBytes">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1365">SetBytes</a>
    <a href="#Value.SetBytes">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetBytes(x []<a href="/builtin/#byte">byte</a>)</pre>

SetBytes sets v's underlying value. It panics if v's underlying value is not a
slice of bytes.

<h3 id="Value.SetCap">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1449">SetCap</a>
    <a href="#Value.SetCap">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetCap(n <a href="/builtin/#int">int</a>)</pre>

SetCap sets v's capacity to n. It panics if v's Kind is not Slice or if n is
smaller than the length or greater than the capacity of the slice.

<h3 id="Value.SetComplex">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1387">SetComplex</a>
    <a href="#Value.SetComplex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetComplex(x <a href="/builtin/#complex128">complex128</a>)</pre>

SetComplex sets v's underlying value to x. It panics if v's Kind is not
Complex64 or Complex128, or if CanSet() is false.

<h3 id="Value.SetFloat">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1401">SetFloat</a>
    <a href="#Value.SetFloat">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetFloat(x <a href="/builtin/#float64">float64</a>)</pre>

SetFloat sets v's underlying value to x. It panics if v's Kind is not Float32 or
Float64, or if CanSet() is false.

<h3 id="Value.SetInt">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1415">SetInt</a>
    <a href="#Value.SetInt">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetInt(x <a href="/builtin/#int64">int64</a>)</pre>

SetInt sets v's underlying value to x. It panics if v's Kind is not Int, Int8,
Int16, Int32, or Int64, or if CanSet() is false.

<h3 id="Value.SetLen">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1436">SetLen</a>
    <a href="#Value.SetLen">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetLen(n <a href="/builtin/#int">int</a>)</pre>

SetLen sets v's length to n. It panics if v's Kind is not Slice or if n is
negative or greater than the capacity of the slice.

<h3 id="Value.SetMapIndex">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1465">SetMapIndex</a>
    <a href="#Value.SetMapIndex">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetMapIndex(key, val <a href="#Value">Value</a>)</pre>

SetMapIndex sets the value associated with key in the map v to val. It panics if
v's Kind is not Map. If val is the zero Value, SetMapIndex deletes the key from
the map. Otherwise if v holds a nil map, SetMapIndex will panic. As in Go, key's
value must be assignable to the map's key type, and val's value must be
assignable to the map's value type.

<h3 id="Value.SetPointer">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1516">SetPointer</a>
    <a href="#Value.SetPointer">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetPointer(x <a href="/unsafe/">unsafe</a>.<a href="/unsafe/#Pointer">Pointer</a>)</pre>

SetPointer sets the unsafe.Pointer value v to x. It panics if v's Kind is not
UnsafePointer.

<h3 id="Value.SetString">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1524">SetString</a>
    <a href="#Value.SetString">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetString(x <a href="/builtin/#string">string</a>)</pre>

SetString sets v's underlying value to x. It panics if v's Kind is not String or
if CanSet() is false.

<h3 id="Value.SetUint">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1494">SetUint</a>
    <a href="#Value.SetUint">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) SetUint(x <a href="/builtin/#uint64">uint64</a>)</pre>

SetUint sets v's underlying value to x. It panics if v's Kind is not Uint,
Uintptr, Uint8, Uint16, Uint32, or Uint64, or if CanSet() is false.

<h3 id="Value.Slice">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1533">Slice</a>
    <a href="#Value.Slice">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Slice(i, j <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

Slice returns v[i:j]. It panics if v's Kind is not Array, Slice or String, or if
v is an unaddressable array, or if the indexes are out of bounds.

<h3 id="Value.Slice3">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1592">Slice3</a>
    <a href="#Value.Slice3">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Slice3(i, j, k <a href="/builtin/#int">int</a>) <a href="#Value">Value</a></pre>

Slice3 is the 3-index form of the slice operation: it returns v[i:j:k]. It
panics if v's Kind is not Array or Slice, or if v is an unaddressable array, or
if the indexes are out of bounds.

<h3 id="Value.String">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1647">String</a>
    <a href="#Value.String">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) String() <a href="/builtin/#string">string</a></pre>

String returns the string v's underlying value, as a string. String is a special
case because of Go's String method convention. Unlike the other getters, it does
not panic if v's Kind is not String. Instead, it returns a string of the form
"<T value>" where T is v's type. The fmt package treats Values specially. It
does not call their String method implicitly but instead prints the concrete
values they hold.

<h3 id="Value.TryRecv">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1664">TryRecv</a>
    <a href="#Value.TryRecv">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) TryRecv() (x <a href="#Value">Value</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

TryRecv attempts to receive a value from the channel v but will not block. It
panics if v's Kind is not Chan. If the receive delivers a value, x is the
transferred value and ok is true. If the receive cannot finish without blocking,
x is the zero Value and ok is false. If the channel is closed, x is the zero
value for the channel's element type and ok is false.

<h3 id="Value.TrySend">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1674">TrySend</a>
    <a href="#Value.TrySend">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) TrySend(x <a href="#Value">Value</a>) <a href="/builtin/#bool">bool</a></pre>

TrySend attempts to send x on the channel v but will not block. It panics if v's
Kind is not Chan. It reports whether the value was sent. As in Go, x's value
must be assignable to the channel's element type.

<h3 id="Value.Type">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1681">Type</a>
    <a href="#Value.Type">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Type() <a href="#Type">Type</a></pre>

Type returns v's type.

<h3 id="Value.Uint">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1714">Uint</a>
    <a href="#Value.Uint">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) Uint() <a href="/builtin/#uint64">uint64</a></pre>

Uint returns v's underlying value, as a uint64. It panics if v's Kind is not
Uint, Uintptr, Uint8, Uint16, Uint32, or Uint64.

<h3 id="Value.UnsafeAddr">func (Value) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L1737">UnsafeAddr</a>
    <a href="#Value.UnsafeAddr">¶</a></h3>
<pre>func (v <a href="#Value">Value</a>) UnsafeAddr() <a href="/builtin/#uintptr">uintptr</a></pre>

UnsafeAddr returns a pointer to v's data. It is for advanced clients that also
import the "unsafe" package. It panics if v is not addressable.

<h2 id="ValueError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L140">ValueError</a>
    <a href="#ValueError">¶</a></h2>
<pre>type ValueError struct {
<span id="ValueError.Method"></span>    Method <a href="/builtin/#string">string</a>
<span id="ValueError.Kind"></span>    Kind   <a href="#Kind">Kind</a>
}</pre>

A ValueError occurs when a Value method is invoked on a Value that does not
support it. Such cases are documented in the description of each method.

<h3 id="ValueError.Error">func (*ValueError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/value.go#L145">Error</a>
    <a href="#ValueError.Error">¶</a></h3>
<pre>func (e *<a href="#ValueError">ValueError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/reflect/type.go#L199)  FieldByName and related functions consider struct field names to be equal if
  the names are equal, even if they are unexported names originating in
  different packages. The practical effect of this is that the result of
  t.FieldByName("x") is not well defined if the struct type t contains
  multiple fields named x (embedded from different packages). FieldByName may
  return one of the fields named x or may report that there are none. See
  golang.org/issue/4876 for more details.


