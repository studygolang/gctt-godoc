version: 1.9.2
## package asn1

  `import "encoding/asn1"`

## Overview

Package asn1 implements parsing of DER-encoded ASN.1 data structures, as defined
in ITU-T Rec X.690.

See also ``A Layman's Guide to a Subset of ASN.1, BER, and DER,''
http://luca.ntop.org/Teaching/Appunti/asn1.html.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func Marshal(val interface{}) ([]byte, error)](#Marshal)
- [func Unmarshal(b []byte, val interface{}) (rest []byte, err error)](#Unmarshal)
- [func UnmarshalWithParams(b []byte, val interface{}, params string) (rest []byte, err error)](#UnmarshalWithParams)
- [type BitString](#BitString)
  - [func (b BitString) At(i int) int](#BitString.At)
  - [func (b BitString) RightAlign() []byte](#BitString.RightAlign)
- [type Enumerated](#Enumerated)
- [type Flag](#Flag)
- [type ObjectIdentifier](#ObjectIdentifier)
  - [func (oi ObjectIdentifier) Equal(other ObjectIdentifier) bool](#ObjectIdentifier.Equal)
  - [func (oi ObjectIdentifier) String() string](#ObjectIdentifier.String)
- [type RawContent](#RawContent)
- [type RawValue](#RawValue)
- [type StructuralError](#StructuralError)
  - [func (e StructuralError) Error() string](#StructuralError.Error)
- [type SyntaxError](#SyntaxError)
  - [func (e SyntaxError) Error() string](#SyntaxError.Error)

### Package files
 [asn1.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go) [common.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/common.go) [marshal.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/marshal.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="TagBoolean">TagBoolean</span>         = 1
    <span id="TagInteger">TagInteger</span>         = 2
    <span id="TagBitString">TagBitString</span>       = 3
    <span id="TagOctetString">TagOctetString</span>     = 4
    <span id="TagNull">TagNull</span>            = 5
    <span id="TagOID">TagOID</span>             = 6
    <span id="TagEnum">TagEnum</span>            = 10
    <span id="TagUTF8String">TagUTF8String</span>      = 12
    <span id="TagSequence">TagSequence</span>        = 16
    <span id="TagSet">TagSet</span>             = 17
    <span id="TagPrintableString">TagPrintableString</span> = 19
    <span id="TagT61String">TagT61String</span>       = 20
    <span id="TagIA5String">TagIA5String</span>       = 22
    <span id="TagUTCTime">TagUTCTime</span>         = 23
    <span id="TagGeneralizedTime">TagGeneralizedTime</span> = 24
    <span id="TagGeneralString">TagGeneralString</span>   = 27
)</pre>

ASN.1 tags represent the type of the following object.

<pre>const (
    <span id="ClassUniversal">ClassUniversal</span>       = 0
    <span id="ClassApplication">ClassApplication</span>     = 1
    <span id="ClassContextSpecific">ClassContextSpecific</span> = 2
    <span id="ClassPrivate">ClassPrivate</span>         = 3
)</pre>

ASN.1 class types represent the namespace of the tag.

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="NullBytes">NullBytes</span> = []<a href="/builtin/#byte">byte</a>{<a href="#TagNull">TagNull</a>, 0}</pre>

NullBytes contains bytes representing the DER-encoded ASN.1 NULL type.

<pre>var <span id="NullRawValue">NullRawValue</span> = <a href="#RawValue">RawValue</a>{<a href="#RawValue.Tag">Tag</a>: <a href="#TagNull">TagNull</a>}</pre>

NullRawValue is a RawValue with its Tag set to the ASN.1 NULL type tag (5).

<h2 id="Marshal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/marshal.go#L642">Marshal</a>
    <a href="#Marshal">¶</a></h2>
<pre>func Marshal(val interface{}) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Marshal returns the ASN.1 encoding of val.

In addition to the struct tags recognised by Unmarshal, the following can be
used:

    ia5:         causes strings to be marshaled as ASN.1, IA5String values
    omitempty:   causes empty slices to be skipped
    printable:   causes strings to be marshaled as ASN.1, PrintableString values
    utf8:        causes strings to be marshaled as ASN.1, UTF8String values
    utc:         causes time.Time to be marshaled as ASN.1, UTCTime values
    generalized: causes time.Time to be marshaled as ASN.1, GeneralizedTime values

<h2 id="Unmarshal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L1001">Unmarshal</a>
    <a href="#Unmarshal">¶</a></h2>
<pre>func Unmarshal(b []<a href="/builtin/#byte">byte</a>, val interface{}) (rest []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

Unmarshal parses the DER-encoded ASN.1 data structure b and uses the reflect
package to fill in an arbitrary value pointed at by val. Because Unmarshal uses
the reflect package, the structs being written to must use upper case field
names.

An ASN.1 INTEGER can be written to an int, int32, int64, or *big.Int (from the
math/big package). If the encoded value does not fit in the Go type, Unmarshal
returns a parse error.

An ASN.1 BIT STRING can be written to a BitString.

An ASN.1 OCTET STRING can be written to a []byte.

An ASN.1 OBJECT IDENTIFIER can be written to an ObjectIdentifier.

An ASN.1 ENUMERATED can be written to an Enumerated.

An ASN.1 UTCTIME or GENERALIZEDTIME can be written to a time.Time.

An ASN.1 PrintableString or IA5String can be written to a string.

Any of the above ASN.1 values can be written to an interface{}. The value stored
in the interface has the corresponding Go type. For integers, that type is
int64.

An ASN.1 SEQUENCE OF x or SET OF x can be written to a slice if an x can be
written to the slice's element type.

An ASN.1 SEQUENCE or SET can be written to a struct if each of the elements in
the sequence can be written to the corresponding element in the struct.

The following tags on struct fields have special meaning to Unmarshal:

    application specifies that a APPLICATION tag is used
    default:x   sets the default value for optional integer fields (only used if optional is also present)
    explicit    specifies that an additional, explicit tag wraps the implicit one
    optional    marks the field as ASN.1 OPTIONAL
    set         causes a SET, rather than a SEQUENCE type to be expected
    tag:x       specifies the ASN.1 tag number; implies ASN.1 CONTEXT SPECIFIC

If the type of the first field of a structure is RawContent then the raw ASN1
contents of the struct will be stored in it.

If the type name of a slice element ends with "SET" then it's treated as if the
"set" tag was set on it. This can be used with nested slices where a struct tag
cannot be given.

Other ASN.1 types are not supported; if it encounters them, Unmarshal returns a
parse error.

<h2 id="UnmarshalWithParams">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L1007">UnmarshalWithParams</a>
    <a href="#UnmarshalWithParams">¶</a></h2>
<pre>func UnmarshalWithParams(b []<a href="/builtin/#byte">byte</a>, val interface{}, params <a href="/builtin/#string">string</a>) (rest []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>

UnmarshalWithParams allows field parameters to be specified for the top-level
element. The form of the params is the same as the field tags.

<h2 id="BitString">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L148">BitString</a>
    <a href="#BitString">¶</a></h2>
<pre>type BitString struct {
<span id="BitString.Bytes"></span>    Bytes     []<a href="/builtin/#byte">byte</a> <span class="comment">// bits packed into bytes.</span>
<span id="BitString.BitLength"></span>    BitLength <a href="/builtin/#int">int</a>    <span class="comment">// length in bits.</span>
}</pre>

BitString is the structure to use when you want an ASN.1 BIT STRING type. A bit
string is padded up to the nearest byte in memory and the number of valid bits
is recorded. Padding bits will be zero.

<h3 id="BitString.At">func (BitString) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L155">At</a>
    <a href="#BitString.At">¶</a></h3>
<pre>func (b <a href="#BitString">BitString</a>) At(i <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

At returns the bit at the given index. If the index is out of range it returns
false.

<h3 id="BitString.RightAlign">func (BitString) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L166">RightAlign</a>
    <a href="#BitString.RightAlign">¶</a></h3>
<pre>func (b <a href="#BitString">BitString</a>) RightAlign() []<a href="/builtin/#byte">byte</a></pre>

RightAlign returns a slice where the padding bits are at the beginning. The
slice may share memory with the BitString.

<h2 id="Enumerated">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L284">Enumerated</a>
    <a href="#Enumerated">¶</a></h2>
<pre>type Enumerated <a href="/builtin/#int">int</a></pre>

An Enumerated is represented as a plain int.

<h2 id="Flag">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L289">Flag</a>
    <a href="#Flag">¶</a></h2>
<pre>type Flag <a href="/builtin/#bool">bool</a></pre>

A Flag accepts any data and is set to true if present.

<h2 id="ObjectIdentifier">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L211">ObjectIdentifier</a>
    <a href="#ObjectIdentifier">¶</a></h2>
<pre>type ObjectIdentifier []<a href="/builtin/#int">int</a></pre>

An ObjectIdentifier represents an ASN.1 OBJECT IDENTIFIER.

<h3 id="ObjectIdentifier.Equal">func (ObjectIdentifier) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L214">Equal</a>
    <a href="#ObjectIdentifier.Equal">¶</a></h3>
<pre>func (oi <a href="#ObjectIdentifier">ObjectIdentifier</a>) Equal(other <a href="#ObjectIdentifier">ObjectIdentifier</a>) <a href="/builtin/#bool">bool</a></pre>

Equal reports whether oi and other represent the same identifier.

<h3 id="ObjectIdentifier.String">func (ObjectIdentifier) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L227">String</a>
    <a href="#ObjectIdentifier.String">¶</a></h3>
<pre>func (oi <a href="#ObjectIdentifier">ObjectIdentifier</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="RawContent">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L442">RawContent</a>
    <a href="#RawContent">¶</a></h2>
<pre>type RawContent []<a href="/builtin/#byte">byte</a></pre>

RawContent is used to signal that the undecoded, DER data needs to be preserved
for a struct. To use it, the first field of the struct must have this type. It's
an error for any of the other fields to have this type.

<h2 id="RawValue">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L432">RawValue</a>
    <a href="#RawValue">¶</a></h2>
<pre>type RawValue struct {
<span id="RawValue.Class"></span>    Class, Tag <a href="/builtin/#int">int</a>
<span id="RawValue.IsCompound"></span>    IsCompound <a href="/builtin/#bool">bool</a>
<span id="RawValue.Bytes"></span>    Bytes      []<a href="/builtin/#byte">byte</a>
<span id="RawValue.FullBytes"></span>    FullBytes  []<a href="/builtin/#byte">byte</a> <span class="comment">// includes the tag and length</span>
}</pre>

A RawValue represents an undecoded ASN.1 object.

<h2 id="StructuralError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L25">StructuralError</a>
    <a href="#StructuralError">¶</a></h2>
<pre>type StructuralError struct {
<span id="StructuralError.Msg"></span>    Msg <a href="/builtin/#string">string</a>
}</pre>

A StructuralError suggests that the ASN.1 data is valid, but the Go type which
is receiving it doesn't match.

<h3 id="StructuralError.Error">func (StructuralError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L29">Error</a>
    <a href="#StructuralError.Error">¶</a></h3>
<pre>func (e <a href="#StructuralError">StructuralError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="SyntaxError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L32">SyntaxError</a>
    <a href="#SyntaxError">¶</a></h2>
<pre>type SyntaxError struct {
<span id="SyntaxError.Msg"></span>    Msg <a href="/builtin/#string">string</a>
}</pre>

A SyntaxError suggests that the ASN.1 data is invalid.

<h3 id="SyntaxError.Error">func (SyntaxError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/asn1/asn1.go#L36">Error</a>
    <a href="#SyntaxError.Error">¶</a></h3>
<pre>func (e <a href="#SyntaxError">SyntaxError</a>) Error() <a href="/builtin/#string">string</a></pre>



