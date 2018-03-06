version: 1.10
## package builtin

  `import "builtin"`

## Overview

Package builtin provides documentation for Go's predeclared identifiers. The
items documented here are not actually in package builtin but their descriptions
here allow godoc to present documentation for the language's special
identifiers.

## Index

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

### Package files
 [builtin.go](//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="true">true</span>  = 0 == 0 <span class="comment">// Untyped bool.</span>
    <span id="false">false</span> = 0 != 0 <span class="comment">// Untyped bool.</span>
)</pre>

true and false are the two untyped boolean values.

<pre>const <span id="iota">iota</span> = 0 <span class="comment">// Untyped int.</span>
</pre>

iota is a predeclared identifier representing the untyped integer ordinal number
of the current const specification in a (usually parenthesized) const
declaration. It is zero-indexed.

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="nil">nil</span> <a href="#Type">Type</a> <span class="comment">// Type must be a pointer, channel, func, interface, map, or slice type</span>
</pre>

nil is a predeclared identifier representing the zero value for a pointer,
channel, func, interface, map, or slice type.

<h2 id="append">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L124">append</a>
    <a href="#append">¶</a></h2>
<pre>func append(slice []<a href="#Type">Type</a>, elems ...<a href="#Type">Type</a>) []<a href="#Type">Type</a></pre>

The append built-in function appends elements to the end of a slice. If it has
sufficient capacity, the destination is resliced to accommodate the new
elements. If it does not, a new underlying array will be allocated. Append
returns the updated slice. It is therefore necessary to store the result of
append, often in the variable holding the slice itself:

    slice = append(slice, elem1, elem2)
    slice = append(slice, anotherSlice...)

As a special case, it is legal to append a string to a byte slice, like this:

    slice = append([]byte("hello "), "world"...)

<h2 id="cap">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L154">cap</a>
    <a href="#cap">¶</a></h2>
<pre>func cap(v <a href="#Type">Type</a>) int</pre>

The cap built-in function returns the capacity of v, according to its type:

    Array: the number of elements in v (same as len(v)).
    Pointer to array: the number of elements in *v (same as len(v)).
    Slice: the maximum length the slice can reach when resliced;
    if v is nil, cap(v) is zero.
    Channel: the channel buffer capacity, in units of elements;
    if v is nil, cap(v) is zero.

<h2 id="close">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L204">close</a>
    <a href="#close">¶</a></h2>
<pre>func close(c chan&lt;- <a href="#Type">Type</a>)</pre>

The close built-in function closes a channel, which must be either bidirectional
or send-only. It should be executed only by the sender, never the receiver, and
has the effect of shutting down the channel after the last sent value is
received. After the last value has been received from a closed channel c, any
receive from c will succeed without blocking, returning the zero value for the
channel element. The form

    x, ok := <-c

will also set ok to false for a closed channel.

<h2 id="complex">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L185">complex</a>
    <a href="#complex">¶</a></h2>
<pre>func complex(r, i <a href="#FloatType">FloatType</a>) <a href="#ComplexType">ComplexType</a></pre>

The complex built-in function constructs a complex value from two floating-point
values. The real and imaginary parts must be of the same size, either float32 or
float64 (or assignable to them), and the return value will be the corresponding
complex type (complex64 for float32, complex128 for float64).

<h2 id="copy">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L131">copy</a>
    <a href="#copy">¶</a></h2>
<pre>func copy(dst, src []<a href="#Type">Type</a>) int</pre>

The copy built-in function copies elements from a source slice into a
destination slice. (As a special case, it also will copy bytes from a string to
a slice of bytes.) The source and destination may overlap. Copy returns the
number of elements copied, which will be the minimum of len(src) and len(dst).

<h2 id="delete">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L136">delete</a>
    <a href="#delete">¶</a></h2>
<pre>func delete(m map[<a href="#Type">Type</a>]<a href="#Type1">Type1</a>, key <a href="#Type">Type</a>)</pre>

The delete built-in function deletes the element with the specified key (m[key])
from the map. If m is nil or there is no such element, delete is a no-op.

<h2 id="imag">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L194">imag</a>
    <a href="#imag">¶</a></h2>
<pre>func imag(c <a href="#ComplexType">ComplexType</a>) <a href="#FloatType">FloatType</a></pre>

The imag built-in function returns the imaginary part of the complex number c.
The return value will be floating point type corresponding to the type of c.

<h2 id="len">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L145">len</a>
    <a href="#len">¶</a></h2>
<pre>func len(v <a href="#Type">Type</a>) int</pre>

The len built-in function returns the length of v, according to its type:

    Array: the number of elements in v.
    Pointer to array: the number of elements in *v (even if v is nil).
    Slice, or map: the number of elements in v; if v is nil, len(v) is zero.
    String: the number of bytes in v.
    Channel: the number of elements queued (unread) in the channel buffer;
    if v is nil, len(v) is zero.

<h2 id="make">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L173">make</a>
    <a href="#make">¶</a></h2>
<pre>func make(t <a href="#Type">Type</a>, size ...<a href="#IntegerType">IntegerType</a>) <a href="#Type">Type</a></pre>

The make built-in function allocates and initializes an object of type slice,
map, or chan (only). Like new, the first argument is a type, not a value. Unlike
new, make's return type is the same as the type of its argument, not a pointer
to it. The specification of the result depends on the type:

    Slice: The size specifies the length. The capacity of the slice is
    equal to its length. A second integer argument may be provided to
    specify a different capacity; it must be no smaller than the
    length. For example, make([]int, 0, 10) allocates an underlying array
    of size 10 and returns a slice of length 0 and capacity 10 that is
    backed by this underlying array.
    Map: An empty map is allocated with enough space to hold the
    specified number of elements. The size may be omitted, in which case
    a small starting size is allocated.
    Channel: The channel's buffer is initialized with the specified
    buffer capacity. If zero, or the size is omitted, the channel is
    unbuffered.

<h2 id="new">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L178">new</a>
    <a href="#new">¶</a></h2>
<pre>func new(<a href="#Type">Type</a>) *<a href="#Type">Type</a></pre>

The new built-in function allocates memory. The first argument is a type, not a
value, and the value returned is a pointer to a newly allocated zero value of
that type.

<h2 id="panic">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L217">panic</a>
    <a href="#panic">¶</a></h2>
<pre>func panic(v interface{})</pre>

The panic built-in function stops normal execution of the current goroutine.
When a function F calls panic, normal execution of F stops immediately. Any
functions whose execution was deferred by F are run in the usual way, and then F
returns to its caller. To the caller G, the invocation of F then behaves like a
call to panic, terminating G's execution and running any deferred functions.
This continues until all functions in the executing goroutine have stopped, in
reverse order. At that point, the program is terminated and the error condition
is reported, including the value of the argument to panic. This termination
sequence is called panicking and can be controlled by the built-in function
recover.

<h2 id="print">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L234">print</a>
    <a href="#print">¶</a></h2>
<pre>func print(args ...<a href="#Type">Type</a>)</pre>

The print built-in function formats its arguments in an implementation-specific
way and writes the result to standard error. Print is useful for bootstrapping
and debugging; it is not guaranteed to stay in the language.

<h2 id="println">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L241">println</a>
    <a href="#println">¶</a></h2>
<pre>func println(args ...<a href="#Type">Type</a>)</pre>

The println built-in function formats its arguments in an
implementation-specific way and writes the result to standard error. Spaces are
always added between arguments and a newline is appended. Println is useful for
bootstrapping and debugging; it is not guaranteed to stay in the language.

<h2 id="real">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L189">real</a>
    <a href="#real">¶</a></h2>
<pre>func real(c <a href="#ComplexType">ComplexType</a>) <a href="#FloatType">FloatType</a></pre>

The real built-in function returns the real part of the complex number c. The
return value will be floating point type corresponding to the type of c.

<h2 id="recover">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L228">recover</a>
    <a href="#recover">¶</a></h2>
<pre>func recover() interface{}</pre>

The recover built-in function allows a program to manage behavior of a panicking
goroutine. Executing a call to recover inside a deferred function (but not any
function called by it) stops the panicking sequence by restoring normal
execution and retrieves the error value passed to the call of panic. If recover
is called outside the deferred function it will not stop a panicking sequence.
In this case, or when the goroutine is not panicking, or if the argument
supplied to panic was nil, recover returns nil. Thus the return value from
recover reports whether the goroutine is panicking.

<h2 id="ComplexType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L113">ComplexType</a>
    <a href="#ComplexType">¶</a></h2>
<pre>type ComplexType complex64</pre>

ComplexType is here for the purposes of documentation only. It is a stand-in for
either complex type: complex64 or complex128.

<h2 id="FloatType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L109">FloatType</a>
    <a href="#FloatType">¶</a></h2>
<pre>type FloatType float32</pre>

FloatType is here for the purposes of documentation only. It is a stand-in for
either float type: float32 or float64.

<h2 id="IntegerType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L105">IntegerType</a>
    <a href="#IntegerType">¶</a></h2>
<pre>type IntegerType int</pre>

IntegerType is here for the purposes of documentation only. It is a stand-in for
any integer type: int, uint, int8 etc.

<h2 id="Type">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L96">Type</a>
    <a href="#Type">¶</a></h2>
<pre>type Type int</pre>

Type is here for the purposes of documentation only. It is a stand-in for any Go
type, but represents the same type for any given function invocation.

<h2 id="Type1">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L101">Type1</a>
    <a href="#Type1">¶</a></h2>
<pre>type Type1 int</pre>

Type1 is here for the purposes of documentation only. It is a stand-in for any
Go type, but represents the same type for any given function invocation.

<h2 id="bool">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L4">bool</a>
    <a href="#bool">¶</a></h2>
<pre>type bool bool</pre>

bool is the set of boolean values, true and false.

<h2 id="byte">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L78">byte</a>
    <a href="#byte">¶</a></h2>
<pre>type byte = uint8</pre>

byte is an alias for uint8 and is equivalent to uint8 in all ways. It is used,
by convention, to distinguish byte values from 8-bit unsigned integer values.

<h2 id="complex128">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L56">complex128</a>
    <a href="#complex128">¶</a></h2>
<pre>type complex128 complex128</pre>

complex128 is the set of all complex numbers with float64 real and imaginary
parts.

<h2 id="complex64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L52">complex64</a>
    <a href="#complex64">¶</a></h2>
<pre>type complex64 complex64</pre>

complex64 is the set of all complex numbers with float32 real and imaginary
parts.

<h2 id="error">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L245">error</a>
    <a href="#error">¶</a></h2>
<pre>type error interface {
    Error() string
}</pre>

The error built-in interface type is the conventional interface for representing
an error condition, with the nil value representing no error.

<h2 id="float32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L45">float32</a>
    <a href="#float32">¶</a></h2>
<pre>type float32 float32</pre>

float32 is the set of all IEEE-754 32-bit floating-point numbers.

<h2 id="float64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L48">float64</a>
    <a href="#float64">¶</a></h2>
<pre>type float64 float64</pre>

float64 is the set of all IEEE-754 64-bit floating-point numbers.

<h2 id="int">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L65">int</a>
    <a href="#int">¶</a></h2>
<pre>type int int</pre>

int is a signed integer type that is at least 32 bits in size. It is a distinct
type, however, and not an alias for, say, int32.

<h2 id="int16">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L34">int16</a>
    <a href="#int16">¶</a></h2>
<pre>type int16 int16</pre>

int16 is the set of all signed 16-bit integers. Range: -32768 through 32767.

<h2 id="int32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L38">int32</a>
    <a href="#int32">¶</a></h2>
<pre>type int32 int32</pre>

int32 is the set of all signed 32-bit integers. Range: -2147483648 through
2147483647.

<h2 id="int64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L42">int64</a>
    <a href="#int64">¶</a></h2>
<pre>type int64 int64</pre>

int64 is the set of all signed 64-bit integers. Range: -9223372036854775808
through 9223372036854775807.

<h2 id="int8">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L30">int8</a>
    <a href="#int8">¶</a></h2>
<pre>type int8 int8</pre>

int8 is the set of all signed 8-bit integers. Range: -128 through 127.

<h2 id="rune">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L82">rune</a>
    <a href="#rune">¶</a></h2>
<pre>type rune = int32</pre>

rune is an alias for int32 and is equivalent to int32 in all ways. It is used,
by convention, to distinguish character values from integer values.

<h2 id="string">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L61">string</a>
    <a href="#string">¶</a></h2>
<pre>type string string</pre>

string is the set of all strings of 8-bit bytes, conventionally but not
necessarily representing UTF-8-encoded text. A string may be empty, but not nil.
Values of string type are immutable.

<h2 id="uint">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L69">uint</a>
    <a href="#uint">¶</a></h2>
<pre>type uint uint</pre>

uint is an unsigned integer type that is at least 32 bits in size. It is a
distinct type, however, and not an alias for, say, uint32.

<h2 id="uint16">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L18">uint16</a>
    <a href="#uint16">¶</a></h2>
<pre>type uint16 uint16</pre>

uint16 is the set of all unsigned 16-bit integers. Range: 0 through 65535.

<h2 id="uint32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L22">uint32</a>
    <a href="#uint32">¶</a></h2>
<pre>type uint32 uint32</pre>

uint32 is the set of all unsigned 32-bit integers. Range: 0 through 4294967295.

<h2 id="uint64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L26">uint64</a>
    <a href="#uint64">¶</a></h2>
<pre>type uint64 uint64</pre>

uint64 is the set of all unsigned 64-bit integers. Range: 0 through
18446744073709551615.

<h2 id="uint8">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L14">uint8</a>
    <a href="#uint8">¶</a></h2>
<pre>type uint8 uint8</pre>

uint8 is the set of all unsigned 8-bit integers. Range: 0 through 255.

<h2 id="uintptr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/builtin/builtin.go#L73">uintptr</a>
    <a href="#uintptr">¶</a></h2>
<pre>type uintptr uintptr</pre>

uintptr is an integer type that is large enough to hold the bit pattern of any
pointer.


