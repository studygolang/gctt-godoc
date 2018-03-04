version: 1.10
## package dwarf

  `import "debug/dwarf"`

## Overview

Package dwarf provides access to DWARF debugging information loaded from
executable files, as defined in the DWARF 2.0 Standard at
http://dwarfstd.org/doc/dwarf-2.0.0.pdf

## Index

- [Variables](#pkg-variables)
- [type AddrType](#AddrType)
- [type ArrayType](#ArrayType)
  - [func (t *ArrayType) Size() int64](#ArrayType.Size)
  - [func (t *ArrayType) String() string](#ArrayType.String)
- [type Attr](#Attr)
  - [func (a Attr) GoString() string](#Attr.GoString)
  - [func (a Attr) String() string](#Attr.String)
- [type BasicType](#BasicType)
  - [func (b *BasicType) Basic() *BasicType](#BasicType.Basic)
  - [func (t *BasicType) String() string](#BasicType.String)
- [type BoolType](#BoolType)
- [type CharType](#CharType)
- [type Class](#Class)
  - [func (i Class) GoString() string](#Class.GoString)
  - [func (i Class) String() string](#Class.String)
- [type CommonType](#CommonType)
  - [func (c *CommonType) Common() *CommonType](#CommonType.Common)
  - [func (c *CommonType) Size() int64](#CommonType.Size)
- [type ComplexType](#ComplexType)
- [type Data](#Data)
  - [func New(abbrev, aranges, frame, info, line, pubnames, ranges, str []byte) (*Data, error)](#New)
  - [func (d *Data) AddTypes(name string, types []byte) error](#Data.AddTypes)
  - [func (d *Data) LineReader(cu *Entry) (*LineReader, error)](#Data.LineReader)
  - [func (d *Data) Ranges(e *Entry) ([][2]uint64, error)](#Data.Ranges)
  - [func (d *Data) Reader() *Reader](#Data.Reader)
  - [func (d *Data) Type(off Offset) (Type, error)](#Data.Type)
- [type DecodeError](#DecodeError)
  - [func (e DecodeError) Error() string](#DecodeError.Error)
- [type DotDotDotType](#DotDotDotType)
  - [func (t *DotDotDotType) String() string](#DotDotDotType.String)
- [type Entry](#Entry)
  - [func (e *Entry) AttrField(a Attr) *Field](#Entry.AttrField)
  - [func (e *Entry) Val(a Attr) interface{}](#Entry.Val)
- [type EnumType](#EnumType)
  - [func (t *EnumType) String() string](#EnumType.String)
- [type EnumValue](#EnumValue)
- [type Field](#Field)
- [type FloatType](#FloatType)
- [type FuncType](#FuncType)
  - [func (t *FuncType) String() string](#FuncType.String)
- [type IntType](#IntType)
- [type LineEntry](#LineEntry)
- [type LineFile](#LineFile)
- [type LineReader](#LineReader)
  - [func (r *LineReader) Next(entry *LineEntry) error](#LineReader.Next)
  - [func (r *LineReader) Reset()](#LineReader.Reset)
  - [func (r *LineReader) Seek(pos LineReaderPos)](#LineReader.Seek)
  - [func (r *LineReader) SeekPC(pc uint64, entry *LineEntry) error](#LineReader.SeekPC)
  - [func (r *LineReader) Tell() LineReaderPos](#LineReader.Tell)
- [type LineReaderPos](#LineReaderPos)
- [type Offset](#Offset)
- [type PtrType](#PtrType)
  - [func (t *PtrType) String() string](#PtrType.String)
- [type QualType](#QualType)
  - [func (t *QualType) Size() int64](#QualType.Size)
  - [func (t *QualType) String() string](#QualType.String)
- [type Reader](#Reader)
  - [func (r *Reader) AddressSize() int](#Reader.AddressSize)
  - [func (r *Reader) Next() (*Entry, error)](#Reader.Next)
  - [func (r *Reader) Seek(off Offset)](#Reader.Seek)
  - [func (r *Reader) SeekPC(pc uint64) (*Entry, error)](#Reader.SeekPC)
  - [func (r *Reader) SkipChildren()](#Reader.SkipChildren)
- [type StructField](#StructField)
- [type StructType](#StructType)
  - [func (t *StructType) Defn() string](#StructType.Defn)
  - [func (t *StructType) String() string](#StructType.String)
- [type Tag](#Tag)
  - [func (t Tag) GoString() string](#Tag.GoString)
  - [func (t Tag) String() string](#Tag.String)
- [type Type](#Type)
- [type TypedefType](#TypedefType)
  - [func (t *TypedefType) Size() int64](#TypedefType.Size)
  - [func (t *TypedefType) String() string](#TypedefType.String)
- [type UcharType](#UcharType)
- [type UintType](#UintType)
- [type UnspecifiedType](#UnspecifiedType)
- [type VoidType](#VoidType)
  - [func (t *VoidType) String() string](#VoidType.String)

### Package files
 [buf.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/buf.go) [class_string.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/class_string.go) [const.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/const.go) [entry.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go) [line.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go) [open.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/open.go) [type.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go) [typeunit.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/typeunit.go) [unit.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/unit.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrUnknownPC">ErrUnknownPC</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;ErrUnknownPC&#34;)</pre>

ErrUnknownPC is the error returned by LineReader.ScanPC when the seek PC is not
covered by any entry in the line table.

<h2 id="AddrType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L77">AddrType</a>
    <a href="#AddrType">¶</a></h2>
<pre>type AddrType struct {
    <a href="#BasicType">BasicType</a>
}</pre>

An AddrType represents a machine address type.

<h2 id="ArrayType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L100">ArrayType</a>
    <a href="#ArrayType">¶</a></h2>
<pre>type ArrayType struct {
    <a href="#CommonType">CommonType</a>
<span id="ArrayType.Type"></span>    Type          <a href="#Type">Type</a>
<span id="ArrayType.StrideBitSize"></span>    StrideBitSize <a href="/builtin/#int64">int64</a> <span class="comment">// if &gt; 0, number of bits to hold each element</span>
<span id="ArrayType.Count"></span>    Count         <a href="/builtin/#int64">int64</a> <span class="comment">// if == -1, an incomplete array, like char x[].</span>
}</pre>

An ArrayType represents a fixed size array type.

<h3 id="ArrayType.Size">func (*ArrayType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L111">Size</a>
    <a href="#ArrayType.Size">¶</a></h3>
<pre>func (t *<a href="#ArrayType">ArrayType</a>) Size() <a href="/builtin/#int64">int64</a></pre>


<h3 id="ArrayType.String">func (*ArrayType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L107">String</a>
    <a href="#ArrayType.String">¶</a></h3>
<pre>func (t *<a href="#ArrayType">ArrayType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Attr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/const.go#L2">Attr</a>
    <a href="#Attr">¶</a></h2>
<pre>type Attr <a href="/builtin/#uint32">uint32</a></pre>

An Attr identifies the attribute type in a DWARF Entry's Field.

<pre>const (
    <span id="AttrSibling">AttrSibling</span>        <a href="#Attr">Attr</a> = 0x01
    <span id="AttrLocation">AttrLocation</span>       <a href="#Attr">Attr</a> = 0x02
    <span id="AttrName">AttrName</span>           <a href="#Attr">Attr</a> = 0x03
    <span id="AttrOrdering">AttrOrdering</span>       <a href="#Attr">Attr</a> = 0x09
    <span id="AttrByteSize">AttrByteSize</span>       <a href="#Attr">Attr</a> = 0x0B
    <span id="AttrBitOffset">AttrBitOffset</span>      <a href="#Attr">Attr</a> = 0x0C
    <span id="AttrBitSize">AttrBitSize</span>        <a href="#Attr">Attr</a> = 0x0D
    <span id="AttrStmtList">AttrStmtList</span>       <a href="#Attr">Attr</a> = 0x10
    <span id="AttrLowpc">AttrLowpc</span>          <a href="#Attr">Attr</a> = 0x11
    <span id="AttrHighpc">AttrHighpc</span>         <a href="#Attr">Attr</a> = 0x12
    <span id="AttrLanguage">AttrLanguage</span>       <a href="#Attr">Attr</a> = 0x13
    <span id="AttrDiscr">AttrDiscr</span>          <a href="#Attr">Attr</a> = 0x15
    <span id="AttrDiscrValue">AttrDiscrValue</span>     <a href="#Attr">Attr</a> = 0x16
    <span id="AttrVisibility">AttrVisibility</span>     <a href="#Attr">Attr</a> = 0x17
    <span id="AttrImport">AttrImport</span>         <a href="#Attr">Attr</a> = 0x18
    <span id="AttrStringLength">AttrStringLength</span>   <a href="#Attr">Attr</a> = 0x19
    <span id="AttrCommonRef">AttrCommonRef</span>      <a href="#Attr">Attr</a> = 0x1A
    <span id="AttrCompDir">AttrCompDir</span>        <a href="#Attr">Attr</a> = 0x1B
    <span id="AttrConstValue">AttrConstValue</span>     <a href="#Attr">Attr</a> = 0x1C
    <span id="AttrContainingType">AttrContainingType</span> <a href="#Attr">Attr</a> = 0x1D
    <span id="AttrDefaultValue">AttrDefaultValue</span>   <a href="#Attr">Attr</a> = 0x1E
    <span id="AttrInline">AttrInline</span>         <a href="#Attr">Attr</a> = 0x20
    <span id="AttrIsOptional">AttrIsOptional</span>     <a href="#Attr">Attr</a> = 0x21
    <span id="AttrLowerBound">AttrLowerBound</span>     <a href="#Attr">Attr</a> = 0x22
    <span id="AttrProducer">AttrProducer</span>       <a href="#Attr">Attr</a> = 0x25
    <span id="AttrPrototyped">AttrPrototyped</span>     <a href="#Attr">Attr</a> = 0x27
    <span id="AttrReturnAddr">AttrReturnAddr</span>     <a href="#Attr">Attr</a> = 0x2A
    <span id="AttrStartScope">AttrStartScope</span>     <a href="#Attr">Attr</a> = 0x2C
    <span id="AttrStrideSize">AttrStrideSize</span>     <a href="#Attr">Attr</a> = 0x2E
    <span id="AttrUpperBound">AttrUpperBound</span>     <a href="#Attr">Attr</a> = 0x2F
    <span id="AttrAbstractOrigin">AttrAbstractOrigin</span> <a href="#Attr">Attr</a> = 0x31
    <span id="AttrAccessibility">AttrAccessibility</span>  <a href="#Attr">Attr</a> = 0x32
    <span id="AttrAddrClass">AttrAddrClass</span>      <a href="#Attr">Attr</a> = 0x33
    <span id="AttrArtificial">AttrArtificial</span>     <a href="#Attr">Attr</a> = 0x34
    <span id="AttrBaseTypes">AttrBaseTypes</span>      <a href="#Attr">Attr</a> = 0x35
    <span id="AttrCalling">AttrCalling</span>        <a href="#Attr">Attr</a> = 0x36
    <span id="AttrCount">AttrCount</span>          <a href="#Attr">Attr</a> = 0x37
    <span id="AttrDataMemberLoc">AttrDataMemberLoc</span>  <a href="#Attr">Attr</a> = 0x38
    <span id="AttrDeclColumn">AttrDeclColumn</span>     <a href="#Attr">Attr</a> = 0x39
    <span id="AttrDeclFile">AttrDeclFile</span>       <a href="#Attr">Attr</a> = 0x3A
    <span id="AttrDeclLine">AttrDeclLine</span>       <a href="#Attr">Attr</a> = 0x3B
    <span id="AttrDeclaration">AttrDeclaration</span>    <a href="#Attr">Attr</a> = 0x3C
    <span id="AttrDiscrList">AttrDiscrList</span>      <a href="#Attr">Attr</a> = 0x3D
    <span id="AttrEncoding">AttrEncoding</span>       <a href="#Attr">Attr</a> = 0x3E
    <span id="AttrExternal">AttrExternal</span>       <a href="#Attr">Attr</a> = 0x3F
    <span id="AttrFrameBase">AttrFrameBase</span>      <a href="#Attr">Attr</a> = 0x40
    <span id="AttrFriend">AttrFriend</span>         <a href="#Attr">Attr</a> = 0x41
    <span id="AttrIdentifierCase">AttrIdentifierCase</span> <a href="#Attr">Attr</a> = 0x42
    <span id="AttrMacroInfo">AttrMacroInfo</span>      <a href="#Attr">Attr</a> = 0x43
    <span id="AttrNamelistItem">AttrNamelistItem</span>   <a href="#Attr">Attr</a> = 0x44
    <span id="AttrPriority">AttrPriority</span>       <a href="#Attr">Attr</a> = 0x45
    <span id="AttrSegment">AttrSegment</span>        <a href="#Attr">Attr</a> = 0x46
    <span id="AttrSpecification">AttrSpecification</span>  <a href="#Attr">Attr</a> = 0x47
    <span id="AttrStaticLink">AttrStaticLink</span>     <a href="#Attr">Attr</a> = 0x48
    <span id="AttrType">AttrType</span>           <a href="#Attr">Attr</a> = 0x49
    <span id="AttrUseLocation">AttrUseLocation</span>    <a href="#Attr">Attr</a> = 0x4A
    <span id="AttrVarParam">AttrVarParam</span>       <a href="#Attr">Attr</a> = 0x4B
    <span id="AttrVirtuality">AttrVirtuality</span>     <a href="#Attr">Attr</a> = 0x4C
    <span id="AttrVtableElemLoc">AttrVtableElemLoc</span>  <a href="#Attr">Attr</a> = 0x4D
    <span id="AttrAllocated">AttrAllocated</span>      <a href="#Attr">Attr</a> = 0x4E
    <span id="AttrAssociated">AttrAssociated</span>     <a href="#Attr">Attr</a> = 0x4F
    <span id="AttrDataLocation">AttrDataLocation</span>   <a href="#Attr">Attr</a> = 0x50
    <span id="AttrStride">AttrStride</span>         <a href="#Attr">Attr</a> = 0x51
    <span id="AttrEntrypc">AttrEntrypc</span>        <a href="#Attr">Attr</a> = 0x52
    <span id="AttrUseUTF8">AttrUseUTF8</span>        <a href="#Attr">Attr</a> = 0x53
    <span id="AttrExtension">AttrExtension</span>      <a href="#Attr">Attr</a> = 0x54
    <span id="AttrRanges">AttrRanges</span>         <a href="#Attr">Attr</a> = 0x55
    <span id="AttrTrampoline">AttrTrampoline</span>     <a href="#Attr">Attr</a> = 0x56
    <span id="AttrCallColumn">AttrCallColumn</span>     <a href="#Attr">Attr</a> = 0x57
    <span id="AttrCallFile">AttrCallFile</span>       <a href="#Attr">Attr</a> = 0x58
    <span id="AttrCallLine">AttrCallLine</span>       <a href="#Attr">Attr</a> = 0x59
    <span id="AttrDescription">AttrDescription</span>    <a href="#Attr">Attr</a> = 0x5A
)</pre>


<h3 id="Attr.GoString">func (Attr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/const.go#L164">GoString</a>
    <a href="#Attr.GoString">¶</a></h3>
<pre>func (a <a href="#Attr">Attr</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="Attr.String">func (Attr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/const.go#L154">String</a>
    <a href="#Attr.String">¶</a></h3>
<pre>func (a <a href="#Attr">Attr</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="BasicType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L26">BasicType</a>
    <a href="#BasicType">¶</a></h2>
<pre>type BasicType struct {
    <a href="#CommonType">CommonType</a>
<span id="BasicType.BitSize"></span>    BitSize   <a href="/builtin/#int64">int64</a>
<span id="BasicType.BitOffset"></span>    BitOffset <a href="/builtin/#int64">int64</a>
}</pre>

A BasicType holds fields common to all basic types.

<h3 id="BasicType.Basic">func (*BasicType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L32">Basic</a>
    <a href="#BasicType.Basic">¶</a></h3>
<pre>func (b *<a href="#BasicType">BasicType</a>) Basic() *<a href="#BasicType">BasicType</a></pre>


<h3 id="BasicType.String">func (*BasicType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L34">String</a>
    <a href="#BasicType.String">¶</a></h3>
<pre>func (t *<a href="#BasicType">BasicType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="BoolType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L72">BoolType</a>
    <a href="#BoolType">¶</a></h2>
<pre>type BoolType struct {
    <a href="#BasicType">BasicType</a>
}</pre>

A BoolType represents a boolean type.

<h2 id="CharType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L42">CharType</a>
    <a href="#CharType">¶</a></h2>
<pre>type CharType struct {
    <a href="#BasicType">BasicType</a>
}</pre>

A CharType represents a signed character type.

<h2 id="Class">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L250">Class</a>
    <a href="#Class">¶</a></h2>
<pre>type Class <a href="/builtin/#int">int</a></pre>

A Class is the DWARF 4 class of an attribute value.

In general, a given attribute's value may take on one of several possible
classes defined by DWARF, each of which leads to a slightly different
interpretation of the attribute.

DWARF version 4 distinguishes attribute value classes more finely than previous
versions of DWARF. The reader will disambiguate coarser classes from earlier
versions of DWARF into the appropriate DWARF 4 class. For example, DWARF 2 uses
"constant" for constants as well as all types of section offsets, but the reader
will canonicalize attributes in DWARF 2 files that refer to section offsets to
one of the Class*Ptr classes, even though these classes were only defined in
DWARF 3.

<pre>const (
    <span class="comment">// ClassUnknown represents values of unknown DWARF class.</span>
    <span id="ClassUnknown">ClassUnknown</span> <a href="#Class">Class</a> = <a href="/builtin/#iota">iota</a>

    <span class="comment">// ClassAddress represents values of type uint64 that are</span>
    <span class="comment">// addresses on the target machine.</span>
    <span id="ClassAddress">ClassAddress</span>

    <span class="comment">// ClassBlock represents values of type []byte whose</span>
    <span class="comment">// interpretation depends on the attribute.</span>
    <span id="ClassBlock">ClassBlock</span>

    <span class="comment">// ClassConstant represents values of type int64 that are</span>
    <span class="comment">// constants. The interpretation of this constant depends on</span>
    <span class="comment">// the attribute.</span>
    <span id="ClassConstant">ClassConstant</span>

    <span class="comment">// ClassExprLoc represents values of type []byte that contain</span>
    <span class="comment">// an encoded DWARF expression or location description.</span>
    <span id="ClassExprLoc">ClassExprLoc</span>

    <span class="comment">// ClassFlag represents values of type bool.</span>
    <span id="ClassFlag">ClassFlag</span>

    <span class="comment">// ClassLinePtr represents values that are an int64 offset</span>
    <span class="comment">// into the &#34;line&#34; section.</span>
    <span id="ClassLinePtr">ClassLinePtr</span>

    <span class="comment">// ClassLocListPtr represents values that are an int64 offset</span>
    <span class="comment">// into the &#34;loclist&#34; section.</span>
    <span id="ClassLocListPtr">ClassLocListPtr</span>

    <span class="comment">// ClassMacPtr represents values that are an int64 offset into</span>
    <span class="comment">// the &#34;mac&#34; section.</span>
    <span id="ClassMacPtr">ClassMacPtr</span>

    <span class="comment">// ClassMacPtr represents values that are an int64 offset into</span>
    <span class="comment">// the &#34;rangelist&#34; section.</span>
    <span id="ClassRangeListPtr">ClassRangeListPtr</span>

    <span class="comment">// ClassReference represents values that are an Offset offset</span>
    <span class="comment">// of an Entry in the info section (for use with Reader.Seek).</span>
    <span class="comment">// The DWARF specification combines ClassReference and</span>
    <span class="comment">// ClassReferenceSig into class &#34;reference&#34;.</span>
    <span id="ClassReference">ClassReference</span>

    <span class="comment">// ClassReferenceSig represents values that are a uint64 type</span>
    <span class="comment">// signature referencing a type Entry.</span>
    <span id="ClassReferenceSig">ClassReferenceSig</span>

    <span class="comment">// ClassString represents values that are strings. If the</span>
    <span class="comment">// compilation unit specifies the AttrUseUTF8 flag (strongly</span>
    <span class="comment">// recommended), the string value will be encoded in UTF-8.</span>
    <span class="comment">// Otherwise, the encoding is unspecified.</span>
    <span id="ClassString">ClassString</span>

    <span class="comment">// ClassReferenceAlt represents values of type int64 that are</span>
    <span class="comment">// an offset into the DWARF &#34;info&#34; section of an alternate</span>
    <span class="comment">// object file.</span>
    <span id="ClassReferenceAlt">ClassReferenceAlt</span>

    <span class="comment">// ClassStringAlt represents values of type int64 that are an</span>
    <span class="comment">// offset into the DWARF string section of an alternate object</span>
    <span class="comment">// file.</span>
    <span id="ClassStringAlt">ClassStringAlt</span>
)</pre>


<h3 id="Class.GoString">func (Class) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L321">GoString</a>
    <a href="#Class.GoString">¶</a></h3>
<pre>func (i <a href="#Class">Class</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="Class.String">func (Class) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/class_string.go#L1">String</a>
    <a href="#Class.String">¶</a></h3>
<pre>func (i <a href="#Class">Class</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="CommonType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L14">CommonType</a>
    <a href="#CommonType">¶</a></h2>
<pre>type CommonType struct {
<span id="CommonType.ByteSize"></span>    ByteSize <a href="/builtin/#int64">int64</a>  <span class="comment">// size of value of this type, in bytes</span>
<span id="CommonType.Name"></span>    Name     <a href="/builtin/#string">string</a> <span class="comment">// name that can be used to refer to type</span>
}</pre>

A CommonType holds fields common to multiple types. If a field is not known or
not applicable for a given type, the zero value is used.

<h3 id="CommonType.Common">func (*CommonType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L19">Common</a>
    <a href="#CommonType.Common">¶</a></h3>
<pre>func (c *<a href="#CommonType">CommonType</a>) Common() *<a href="#CommonType">CommonType</a></pre>


<h3 id="CommonType.Size">func (*CommonType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L21">Size</a>
    <a href="#CommonType.Size">¶</a></h3>
<pre>func (c *<a href="#CommonType">CommonType</a>) Size() <a href="/builtin/#int64">int64</a></pre>


<h2 id="ComplexType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L67">ComplexType</a>
    <a href="#ComplexType">¶</a></h2>
<pre>type ComplexType struct {
    <a href="#BasicType">BasicType</a>
}</pre>

A ComplexType represents a complex floating point type.

<h2 id="Data">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/open.go#L4">Data</a>
    <a href="#Data">¶</a></h2>
<pre>type Data struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Data represents the DWARF debugging information loaded from an executable file
(for example, an ELF or Mach-O executable).

<h3 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/open.go#L31">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(abbrev, aranges, frame, info, line, pubnames, ranges, str []<a href="/builtin/#byte">byte</a>) (*<a href="#Data">Data</a>, <a href="/builtin/#error">error</a>)</pre>

New returns a new Data object initialized from the given parameters. Rather than
calling this function directly, clients should typically use the DWARF method of
the File type of the appropriate package debug/elf, debug/macho, or debug/pe.

The []byte arguments are the data from the corresponding debug section in the
object file; for example, for an ELF object, abbrev is the contents of the
".debug_abbrev" section.

<h3 id="Data.AddTypes">func (*Data) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/open.go#L84">AddTypes</a>
    <a href="#Data.AddTypes">¶</a></h3>
<pre>func (d *<a href="#Data">Data</a>) AddTypes(name <a href="/builtin/#string">string</a>, types []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

AddTypes will add one .debug_types section to the DWARF data. A typical object
with DWARF version 4 debug info will have multiple .debug_types sections. The
name is used for error reporting only, and serves to distinguish one
.debug_types section from another.

<h3 id="Data.LineReader">func (*Data) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go#L128">LineReader</a>
    <a href="#Data.LineReader">¶</a></h3>
<pre>func (d *<a href="#Data">Data</a>) LineReader(cu *<a href="#Entry">Entry</a>) (*<a href="#LineReader">LineReader</a>, <a href="/builtin/#error">error</a>)</pre>

LineReader returns a new reader for the line table of compilation unit cu, which
must be an Entry with tag TagCompileUnit.

If this compilation unit has no line table, it returns nil, nil.

<h3 id="Data.Ranges">func (*Data) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L674">Ranges</a>
    <a href="#Data.Ranges">¶</a></h3>
<pre>func (d *<a href="#Data">Data</a>) Ranges(e *<a href="#Entry">Entry</a>) ([][2]<a href="/builtin/#uint64">uint64</a>, <a href="/builtin/#error">error</a>)</pre>

Ranges returns the PC ranges covered by e, a slice of [low,high) pairs. Only
some entry types, such as TagCompileUnit or TagSubprogram, have PC ranges; for
others, this will return nil with no error.

<h3 id="Data.Reader">func (*Data) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L514">Reader</a>
    <a href="#Data.Reader">¶</a></h3>
<pre>func (d *<a href="#Data">Data</a>) Reader() *<a href="#Reader">Reader</a></pre>

Reader returns a new Reader for Data. The reader is positioned at byte offset 0
in the DWARF ``info'' section.

<h3 id="Data.Type">func (*Data) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L267">Type</a>
    <a href="#Data.Type">¶</a></h3>
<pre>func (d *<a href="#Data">Data</a>) Type(off <a href="#Offset">Offset</a>) (<a href="#Type">Type</a>, <a href="/builtin/#error">error</a>)</pre>

Type reads the type at off in the DWARF ``info'' section.

<h2 id="DecodeError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/buf.go#L174">DecodeError</a>
    <a href="#DecodeError">¶</a></h2>
<pre>type DecodeError struct {
<span id="DecodeError.Name"></span>    Name   <a href="/builtin/#string">string</a>
<span id="DecodeError.Offset"></span>    Offset <a href="#Offset">Offset</a>
<span id="DecodeError.Err"></span>    Err    <a href="/builtin/#string">string</a>
}</pre>


<h3 id="DecodeError.Error">func (DecodeError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/buf.go#L180">Error</a>
    <a href="#DecodeError.Error">¶</a></h3>
<pre>func (e <a href="#DecodeError">DecodeError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="DotDotDotType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L238">DotDotDotType</a>
    <a href="#DotDotDotType">¶</a></h2>
<pre>type DotDotDotType struct {
    <a href="#CommonType">CommonType</a>
}</pre>

A DotDotDotType represents the variadic ... function parameter.

<h3 id="DotDotDotType.String">func (*DotDotDotType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L242">String</a>
    <a href="#DotDotDotType.String">¶</a></h3>
<pre>func (t *<a href="#DotDotDotType">DotDotDotType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Entry">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L200">Entry</a>
    <a href="#Entry">¶</a></h2>
<pre>type Entry struct {
<span id="Entry.Offset"></span>    Offset   <a href="#Offset">Offset</a> <span class="comment">// offset of Entry in DWARF info</span>
<span id="Entry.Tag"></span>    Tag      <a href="#Tag">Tag</a>    <span class="comment">// tag (kind of Entry)</span>
<span id="Entry.Children"></span>    Children <a href="/builtin/#bool">bool</a>   <span class="comment">// whether Entry is followed by children</span>
<span id="Entry.Field"></span>    Field    []<a href="#Field">Field</a>
}</pre>

An entry is a sequence of attribute/value pairs.

<h3 id="Entry.AttrField">func (*Entry) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L341">AttrField</a>
    <a href="#Entry.AttrField">¶</a></h3>
<pre>func (e *<a href="#Entry">Entry</a>) AttrField(a <a href="#Attr">Attr</a>) *<a href="#Field">Field</a></pre>

AttrField returns the Field associated with attribute Attr in Entry, or nil if
there is no such attribute.

<h3 id="Entry.Val">func (*Entry) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L332">Val</a>
    <a href="#Entry.Val">¶</a></h3>
<pre>func (e *<a href="#Entry">Entry</a>) Val(a <a href="#Attr">Attr</a>) interface{}</pre>

Val returns the value associated with attribute Attr in Entry, or nil if there
is no such attribute.

A common idiom is to merge the check for nil return with the check that the
value has the expected dynamic type, as in:

    v, ok := e.Val(AttrSibling).(int64)

<h2 id="EnumType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L187">EnumType</a>
    <a href="#EnumType">¶</a></h2>
<pre>type EnumType struct {
    <a href="#CommonType">CommonType</a>
<span id="EnumType.EnumName"></span>    EnumName <a href="/builtin/#string">string</a>
<span id="EnumType.Val"></span>    Val      []*<a href="#EnumValue">EnumValue</a>
}</pre>

An EnumType represents an enumerated type. The only indication of its native
integer type is its ByteSize (inside CommonType).

<h3 id="EnumType.String">func (*EnumType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L199">String</a>
    <a href="#EnumType.String">¶</a></h3>
<pre>func (t *<a href="#EnumType">EnumType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="EnumValue">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L194">EnumValue</a>
    <a href="#EnumValue">¶</a></h2>
<pre>type EnumValue struct {
<span id="EnumValue.Name"></span>    Name <a href="/builtin/#string">string</a>
<span id="EnumValue.Val"></span>    Val  <a href="/builtin/#int64">int64</a>
}</pre>

An EnumValue represents a single enumeration value.

<h2 id="Field">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L230">Field</a>
    <a href="#Field">¶</a></h2>
<pre>type Field struct {
<span id="Field.Attr"></span>    Attr  <a href="#Attr">Attr</a>
<span id="Field.Val"></span>    Val   interface{}
<span id="Field.Class"></span>    Class <a href="#Class">Class</a>
}</pre>

A Field is a single attribute/value pair in an Entry.

A value can be one of several "attribute classes" defined by DWARF. The Go types
corresponding to each class are:

    DWARF class       Go type        Class
    -----------       -------        -----
    address           uint64         ClassAddress
    block             []byte         ClassBlock
    constant          int64          ClassConstant
    flag              bool           ClassFlag
    reference
      to info         dwarf.Offset   ClassReference
      to type unit    uint64         ClassReferenceSig
    string            string         ClassString
    exprloc           []byte         ClassExprLoc
    lineptr           int64          ClassLinePtr
    loclistptr        int64          ClassLocListPtr
    macptr            int64          ClassMacPtr
    rangelistptr      int64          ClassRangeListPtr

For unrecognized or vendor-defined attributes, Class may be ClassUnknown.

<h2 id="FloatType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L62">FloatType</a>
    <a href="#FloatType">¶</a></h2>
<pre>type FloatType struct {
    <a href="#BasicType">BasicType</a>
}</pre>

A FloatType represents a floating point type.

<h2 id="FuncType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L216">FuncType</a>
    <a href="#FuncType">¶</a></h2>
<pre>type FuncType struct {
    <a href="#CommonType">CommonType</a>
<span id="FuncType.ReturnType"></span>    ReturnType <a href="#Type">Type</a>
<span id="FuncType.ParamType"></span>    ParamType  []<a href="#Type">Type</a>
}</pre>

A FuncType represents a function type.

<h3 id="FuncType.String">func (*FuncType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L222">String</a>
    <a href="#FuncType.String">¶</a></h3>
<pre>func (t *<a href="#FuncType">FuncType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="IntType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L52">IntType</a>
    <a href="#IntType">¶</a></h2>
<pre>type IntType struct {
    <a href="#BasicType">BasicType</a>
}</pre>

An IntType represents a signed integer type.

<h2 id="LineEntry">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go#L39">LineEntry</a>
    <a href="#LineEntry">¶</a></h2>
<pre>type LineEntry struct {
<span id="LineEntry.Address"></span>    <span class="comment">// Address is the program-counter value of a machine</span>
    <span class="comment">// instruction generated by the compiler. This LineEntry</span>
    <span class="comment">// applies to each instruction from Address to just before the</span>
    <span class="comment">// Address of the next LineEntry.</span>
    Address <a href="/builtin/#uint64">uint64</a>

<span id="LineEntry.OpIndex"></span>    <span class="comment">// OpIndex is the index of an operation within a VLIW</span>
    <span class="comment">// instruction. The index of the first operation is 0. For</span>
    <span class="comment">// non-VLIW architectures, it will always be 0. Address and</span>
    <span class="comment">// OpIndex together form an operation pointer that can</span>
    <span class="comment">// reference any individual operation within the instruction</span>
    <span class="comment">// stream.</span>
    OpIndex <a href="/builtin/#int">int</a>

<span id="LineEntry.File"></span>    <span class="comment">// File is the source file corresponding to these</span>
    <span class="comment">// instructions.</span>
    File *<a href="#LineFile">LineFile</a>

<span id="LineEntry.Line"></span>    <span class="comment">// Line is the source code line number corresponding to these</span>
    <span class="comment">// instructions. Lines are numbered beginning at 1. It may be</span>
    <span class="comment">// 0 if these instructions cannot be attributed to any source</span>
    <span class="comment">// line.</span>
    Line <a href="/builtin/#int">int</a>

<span id="LineEntry.Column"></span>    <span class="comment">// Column is the column number within the source line of these</span>
    <span class="comment">// instructions. Columns are numbered beginning at 1. It may</span>
    <span class="comment">// be 0 to indicate the &#34;left edge&#34; of the line.</span>
    Column <a href="/builtin/#int">int</a>

<span id="LineEntry.IsStmt"></span>    <span class="comment">// IsStmt indicates that Address is a recommended breakpoint</span>
    <span class="comment">// location, such as the beginning of a line, statement, or a</span>
    <span class="comment">// distinct subpart of a statement.</span>
    IsStmt <a href="/builtin/#bool">bool</a>

<span id="LineEntry.BasicBlock"></span>    <span class="comment">// BasicBlock indicates that Address is the beginning of a</span>
    <span class="comment">// basic block.</span>
    BasicBlock <a href="/builtin/#bool">bool</a>

<span id="LineEntry.PrologueEnd"></span>    <span class="comment">// PrologueEnd indicates that Address is one (of possibly</span>
    <span class="comment">// many) PCs where execution should be suspended for a</span>
    <span class="comment">// breakpoint on entry to the containing function.</span>
    <span class="comment">//</span>
    <span class="comment">// Added in DWARF 3.</span>
    PrologueEnd <a href="/builtin/#bool">bool</a>

<span id="LineEntry.EpilogueBegin"></span>    <span class="comment">// EpilogueBegin indicates that Address is one (of possibly</span>
    <span class="comment">// many) PCs where execution should be suspended for a</span>
    <span class="comment">// breakpoint on exit from this function.</span>
    <span class="comment">//</span>
    <span class="comment">// Added in DWARF 3.</span>
    EpilogueBegin <a href="/builtin/#bool">bool</a>

<span id="LineEntry.ISA"></span>    <span class="comment">// ISA is the instruction set architecture for these</span>
    <span class="comment">// instructions. Possible ISA values should be defined by the</span>
    <span class="comment">// applicable ABI specification.</span>
    <span class="comment">//</span>
    <span class="comment">// Added in DWARF 3.</span>
    ISA <a href="/builtin/#int">int</a>

<span id="LineEntry.Discriminator"></span>    <span class="comment">// Discriminator is an arbitrary integer indicating the block</span>
    <span class="comment">// to which these instructions belong. It serves to</span>
    <span class="comment">// distinguish among multiple blocks that may all have with</span>
    <span class="comment">// the same source file, line, and column. Where only one</span>
    <span class="comment">// block exists for a given source position, it should be 0.</span>
    <span class="comment">//</span>
    <span class="comment">// Added in DWARF 3.</span>
    Discriminator <a href="/builtin/#int">int</a>

<span id="LineEntry.EndSequence"></span>    <span class="comment">// EndSequence indicates that Address is the first byte after</span>
    <span class="comment">// the end of a sequence of target machine instructions. If it</span>
    <span class="comment">// is set, only this and the Address field are meaningful. A</span>
    <span class="comment">// line number table may contain information for multiple</span>
    <span class="comment">// potentially disjoint instruction sequences. The last entry</span>
    <span class="comment">// in a line table should always have EndSequence set.</span>
    EndSequence <a href="/builtin/#bool">bool</a>
}</pre>

A LineEntry is a row in a DWARF line table.

<h2 id="LineFile">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go#L118">LineFile</a>
    <a href="#LineFile">¶</a></h2>
<pre>type LineFile struct {
<span id="LineFile.Name"></span>    Name   <a href="/builtin/#string">string</a>
<span id="LineFile.Mtime"></span>    Mtime  <a href="/builtin/#uint64">uint64</a> <span class="comment">// Implementation defined modification time, or 0 if unknown</span>
<span id="LineFile.Length"></span>    Length <a href="/builtin/#int">int</a>    <span class="comment">// File length, or 0 if unknown</span>
}</pre>

A LineFile is a source file referenced by a DWARF line table entry.

<h2 id="LineReader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go#L10">LineReader</a>
    <a href="#LineReader">¶</a></h2>
<pre>type LineReader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A LineReader reads a sequence of LineEntry structures from a DWARF "line"
section for a single compilation unit. LineEntries occur in order of increasing
PC and each LineEntry gives metadata for the instructions from that LineEntry's
PC to just before the next LineEntry's PC. The last entry will have its
EndSequence field set.

<h3 id="LineReader.Next">func (*LineReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go#L306">Next</a>
    <a href="#LineReader.Next">¶</a></h3>
<pre>func (r *<a href="#LineReader">LineReader</a>) Next(entry *<a href="#LineEntry">LineEntry</a>) <a href="/builtin/#error">error</a></pre>

Next sets *entry to the next row in this line table and moves to the next row.
If there are no more entries and the line table is properly terminated, it
returns io.EOF.

Rows are always in order of increasing entry.Address, but entry.Line may go
forward or backward.

<h3 id="LineReader.Reset">func (*LineReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go#L493">Reset</a>
    <a href="#LineReader.Reset">¶</a></h3>
<pre>func (r *<a href="#LineReader">LineReader</a>) Reset()</pre>

Reset repositions the line table reader at the beginning of the line table.

<h3 id="LineReader.Seek">func (*LineReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go#L483">Seek</a>
    <a href="#LineReader.Seek">¶</a></h3>
<pre>func (r *<a href="#LineReader">LineReader</a>) Seek(pos <a href="#LineReaderPos">LineReaderPos</a>)</pre>

Seek restores the line table reader to a position returned by Tell.

The argument pos must have been returned by a call to Tell on this line table.

<h3 id="LineReader.SeekPC">func (*LineReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go#L542">SeekPC</a>
    <a href="#LineReader.SeekPC">¶</a></h3>
<pre>func (r *<a href="#LineReader">LineReader</a>) SeekPC(pc <a href="/builtin/#uint64">uint64</a>, entry *<a href="#LineEntry">LineEntry</a>) <a href="/builtin/#error">error</a></pre>

SeekPC sets *entry to the LineEntry that includes pc and positions the reader on
the next entry in the line table. If necessary, this will seek backwards to find
pc.

If pc is not covered by any entry in this line table, SeekPC returns
ErrUnknownPC. In this case, *entry and the final seek position are unspecified.

Note that DWARF line tables only permit sequential, forward scans. Hence, in the
worst case, this takes time linear in the size of the line table. If the caller
wishes to do repeated fast PC lookups, it should build an appropriate index of
the line table.

<h3 id="LineReader.Tell">func (*LineReader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go#L475">Tell</a>
    <a href="#LineReader.Tell">¶</a></h3>
<pre>func (r *<a href="#LineReader">LineReader</a>) Tell() <a href="#LineReaderPos">LineReaderPos</a></pre>

Tell returns the current position in the line table.

<h2 id="LineReaderPos">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/line.go#L463">LineReaderPos</a>
    <a href="#LineReaderPos">¶</a></h2>
<pre>type LineReaderPos struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A LineReaderPos represents a position in a line table.

<h2 id="Offset">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L352">Offset</a>
    <a href="#Offset">¶</a></h2>
<pre>type Offset <a href="/builtin/#uint32">uint32</a></pre>

An Offset represents the location of an Entry within the DWARF info. (See
Reader.Seek.)

<h2 id="PtrType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L126">PtrType</a>
    <a href="#PtrType">¶</a></h2>
<pre>type PtrType struct {
    <a href="#CommonType">CommonType</a>
<span id="PtrType.Type"></span>    Type <a href="#Type">Type</a>
}</pre>

A PtrType represents a pointer type.

<h3 id="PtrType.String">func (*PtrType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L131">String</a>
    <a href="#PtrType.String">¶</a></h3>
<pre>func (t *<a href="#PtrType">PtrType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="QualType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L89">QualType</a>
    <a href="#QualType">¶</a></h2>
<pre>type QualType struct {
    <a href="#CommonType">CommonType</a>
<span id="QualType.Qual"></span>    Qual <a href="/builtin/#string">string</a>
<span id="QualType.Type"></span>    Type <a href="#Type">Type</a>
}</pre>

A QualType represents a type that has the C/C++ "const", "restrict", or
"volatile" qualifier.

<h3 id="QualType.Size">func (*QualType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L97">Size</a>
    <a href="#QualType.Size">¶</a></h3>
<pre>func (t *<a href="#QualType">QualType</a>) Size() <a href="/builtin/#int64">int64</a></pre>


<h3 id="QualType.String">func (*QualType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L95">String</a>
    <a href="#QualType.String">¶</a></h3>
<pre>func (t *<a href="#QualType">QualType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Reader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L503">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Reader allows reading Entry structures from a DWARF ``info'' section. The
Entry structures are arranged in a tree. The Reader's Next function return
successive entries from a pre-order traversal of the tree. If an entry has
children, its Children field will be true, and the children follow, terminated
by an Entry with Tag 0.

<h3 id="Reader.AddressSize">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L522">AddressSize</a>
    <a href="#Reader.AddressSize">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) AddressSize() <a href="/builtin/#int">int</a></pre>

AddressSize returns the size in bytes of addresses in the current compilation
unit.

<h3 id="Reader.Next">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L565">Next</a>
    <a href="#Reader.Next">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Next() (*<a href="#Entry">Entry</a>, <a href="/builtin/#error">error</a>)</pre>

Next reads the next entry from the encoded entry stream. It returns nil, nil
when it reaches the end of the section. It returns an error if the current
offset is invalid or the data at the offset cannot be decoded as a valid Entry.

<h3 id="Reader.Seek">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L528">Seek</a>
    <a href="#Reader.Seek">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) Seek(off <a href="#Offset">Offset</a>)</pre>

Seek positions the Reader at offset off in the encoded entry stream. Offset 0
can be used to denote the first entry.

<h3 id="Reader.SeekPC">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L642">SeekPC</a>
    <a href="#Reader.SeekPC">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) SeekPC(pc <a href="/builtin/#uint64">uint64</a>) (*<a href="#Entry">Entry</a>, <a href="/builtin/#error">error</a>)</pre>

SeekPC returns the Entry for the compilation unit that includes pc, and
positions the reader to read the children of that unit. If pc is not covered by
any unit, SeekPC returns ErrUnknownPC and the position of the reader is
undefined.

Because compilation units can describe multiple regions of the executable, in
the worst case SeekPC must search through all the ranges in all the compilation
units. Each call to SeekPC starts the search at the compilation unit of the last
call, so in general looking up a series of PCs will be faster if they are
sorted. If the caller wishes to do repeated fast PC lookups, it should build an
appropriate index using the Ranges method.

<h3 id="Reader.SkipChildren">func (*Reader) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/entry.go#L593">SkipChildren</a>
    <a href="#Reader.SkipChildren">¶</a></h3>
<pre>func (r *<a href="#Reader">Reader</a>) SkipChildren()</pre>

SkipChildren skips over the child entries associated with the last Entry
returned by Next. If that Entry did not have children or Next has not been
called, SkipChildren is a no-op.

<h2 id="StructField">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L143">StructField</a>
    <a href="#StructField">¶</a></h2>
<pre>type StructField struct {
<span id="StructField.Name"></span>    Name       <a href="/builtin/#string">string</a>
<span id="StructField.Type"></span>    Type       <a href="#Type">Type</a>
<span id="StructField.ByteOffset"></span>    ByteOffset <a href="/builtin/#int64">int64</a>
<span id="StructField.ByteSize"></span>    ByteSize   <a href="/builtin/#int64">int64</a> <span class="comment">// usually zero; use Type.Size() for normal fields</span>
<span id="StructField.BitOffset"></span>    BitOffset  <a href="/builtin/#int64">int64</a> <span class="comment">// within the ByteSize bytes at ByteOffset</span>
<span id="StructField.BitSize"></span>    BitSize    <a href="/builtin/#int64">int64</a> <span class="comment">// zero if not a bit field</span>
}</pre>

A StructField represents a field in a struct, union, or C++ class type.

<h2 id="StructType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L134">StructType</a>
    <a href="#StructType">¶</a></h2>
<pre>type StructType struct {
    <a href="#CommonType">CommonType</a>
<span id="StructType.StructName"></span>    StructName <a href="/builtin/#string">string</a>
<span id="StructType.Kind"></span>    Kind       <a href="/builtin/#string">string</a> <span class="comment">// &#34;struct&#34;, &#34;union&#34;, or &#34;class&#34;.</span>
<span id="StructType.Field"></span>    Field      []*<a href="#StructField">StructField</a>
<span id="StructType.Incomplete"></span>    Incomplete <a href="/builtin/#bool">bool</a> <span class="comment">// if true, struct, union, class is declared but not defined</span>
}</pre>

A StructType represents a struct, union, or C++ class type.

<h3 id="StructType.Defn">func (*StructType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L159">Defn</a>
    <a href="#StructType.Defn">¶</a></h3>
<pre>func (t *<a href="#StructType">StructType</a>) Defn() <a href="/builtin/#string">string</a></pre>


<h3 id="StructType.String">func (*StructType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L152">String</a>
    <a href="#StructType.String">¶</a></h3>
<pre>func (t *<a href="#StructType">StructType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Tag">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/const.go#L212">Tag</a>
    <a href="#Tag">¶</a></h2>
<pre>type Tag <a href="/builtin/#uint32">uint32</a></pre>

A Tag is the classification (the type) of an Entry.

<pre>const (
    <span id="TagArrayType">TagArrayType</span>              <a href="#Tag">Tag</a> = 0x01
    <span id="TagClassType">TagClassType</span>              <a href="#Tag">Tag</a> = 0x02
    <span id="TagEntryPoint">TagEntryPoint</span>             <a href="#Tag">Tag</a> = 0x03
    <span id="TagEnumerationType">TagEnumerationType</span>        <a href="#Tag">Tag</a> = 0x04
    <span id="TagFormalParameter">TagFormalParameter</span>        <a href="#Tag">Tag</a> = 0x05
    <span id="TagImportedDeclaration">TagImportedDeclaration</span>    <a href="#Tag">Tag</a> = 0x08
    <span id="TagLabel">TagLabel</span>                  <a href="#Tag">Tag</a> = 0x0A
    <span id="TagLexDwarfBlock">TagLexDwarfBlock</span>          <a href="#Tag">Tag</a> = 0x0B
    <span id="TagMember">TagMember</span>                 <a href="#Tag">Tag</a> = 0x0D
    <span id="TagPointerType">TagPointerType</span>            <a href="#Tag">Tag</a> = 0x0F
    <span id="TagReferenceType">TagReferenceType</span>          <a href="#Tag">Tag</a> = 0x10
    <span id="TagCompileUnit">TagCompileUnit</span>            <a href="#Tag">Tag</a> = 0x11
    <span id="TagStringType">TagStringType</span>             <a href="#Tag">Tag</a> = 0x12
    <span id="TagStructType">TagStructType</span>             <a href="#Tag">Tag</a> = 0x13
    <span id="TagSubroutineType">TagSubroutineType</span>         <a href="#Tag">Tag</a> = 0x15
    <span id="TagTypedef">TagTypedef</span>                <a href="#Tag">Tag</a> = 0x16
    <span id="TagUnionType">TagUnionType</span>              <a href="#Tag">Tag</a> = 0x17
    <span id="TagUnspecifiedParameters">TagUnspecifiedParameters</span>  <a href="#Tag">Tag</a> = 0x18
    <span id="TagVariant">TagVariant</span>                <a href="#Tag">Tag</a> = 0x19
    <span id="TagCommonDwarfBlock">TagCommonDwarfBlock</span>       <a href="#Tag">Tag</a> = 0x1A
    <span id="TagCommonInclusion">TagCommonInclusion</span>        <a href="#Tag">Tag</a> = 0x1B
    <span id="TagInheritance">TagInheritance</span>            <a href="#Tag">Tag</a> = 0x1C
    <span id="TagInlinedSubroutine">TagInlinedSubroutine</span>      <a href="#Tag">Tag</a> = 0x1D
    <span id="TagModule">TagModule</span>                 <a href="#Tag">Tag</a> = 0x1E
    <span id="TagPtrToMemberType">TagPtrToMemberType</span>        <a href="#Tag">Tag</a> = 0x1F
    <span id="TagSetType">TagSetType</span>                <a href="#Tag">Tag</a> = 0x20
    <span id="TagSubrangeType">TagSubrangeType</span>           <a href="#Tag">Tag</a> = 0x21
    <span id="TagWithStmt">TagWithStmt</span>               <a href="#Tag">Tag</a> = 0x22
    <span id="TagAccessDeclaration">TagAccessDeclaration</span>      <a href="#Tag">Tag</a> = 0x23
    <span id="TagBaseType">TagBaseType</span>               <a href="#Tag">Tag</a> = 0x24
    <span id="TagCatchDwarfBlock">TagCatchDwarfBlock</span>        <a href="#Tag">Tag</a> = 0x25
    <span id="TagConstType">TagConstType</span>              <a href="#Tag">Tag</a> = 0x26
    <span id="TagConstant">TagConstant</span>               <a href="#Tag">Tag</a> = 0x27
    <span id="TagEnumerator">TagEnumerator</span>             <a href="#Tag">Tag</a> = 0x28
    <span id="TagFileType">TagFileType</span>               <a href="#Tag">Tag</a> = 0x29
    <span id="TagFriend">TagFriend</span>                 <a href="#Tag">Tag</a> = 0x2A
    <span id="TagNamelist">TagNamelist</span>               <a href="#Tag">Tag</a> = 0x2B
    <span id="TagNamelistItem">TagNamelistItem</span>           <a href="#Tag">Tag</a> = 0x2C
    <span id="TagPackedType">TagPackedType</span>             <a href="#Tag">Tag</a> = 0x2D
    <span id="TagSubprogram">TagSubprogram</span>             <a href="#Tag">Tag</a> = 0x2E
    <span id="TagTemplateTypeParameter">TagTemplateTypeParameter</span>  <a href="#Tag">Tag</a> = 0x2F
    <span id="TagTemplateValueParameter">TagTemplateValueParameter</span> <a href="#Tag">Tag</a> = 0x30
    <span id="TagThrownType">TagThrownType</span>             <a href="#Tag">Tag</a> = 0x31
    <span id="TagTryDwarfBlock">TagTryDwarfBlock</span>          <a href="#Tag">Tag</a> = 0x32
    <span id="TagVariantPart">TagVariantPart</span>            <a href="#Tag">Tag</a> = 0x33
    <span id="TagVariable">TagVariable</span>               <a href="#Tag">Tag</a> = 0x34
    <span id="TagVolatileType">TagVolatileType</span>           <a href="#Tag">Tag</a> = 0x35
    <span class="comment">// The following are new in DWARF 3.</span>
    <span id="TagDwarfProcedure">TagDwarfProcedure</span>  <a href="#Tag">Tag</a> = 0x36
    <span id="TagRestrictType">TagRestrictType</span>    <a href="#Tag">Tag</a> = 0x37
    <span id="TagInterfaceType">TagInterfaceType</span>   <a href="#Tag">Tag</a> = 0x38
    <span id="TagNamespace">TagNamespace</span>       <a href="#Tag">Tag</a> = 0x39
    <span id="TagImportedModule">TagImportedModule</span>  <a href="#Tag">Tag</a> = 0x3A
    <span id="TagUnspecifiedType">TagUnspecifiedType</span> <a href="#Tag">Tag</a> = 0x3B
    <span id="TagPartialUnit">TagPartialUnit</span>     <a href="#Tag">Tag</a> = 0x3C
    <span id="TagImportedUnit">TagImportedUnit</span>    <a href="#Tag">Tag</a> = 0x3D
    <span id="TagMutableType">TagMutableType</span>     <a href="#Tag">Tag</a> = 0x3E <span class="comment">// Later removed from DWARF.</span>
    <span id="TagCondition">TagCondition</span>       <a href="#Tag">Tag</a> = 0x3F
    <span id="TagSharedType">TagSharedType</span>      <a href="#Tag">Tag</a> = 0x40
    <span class="comment">// The following are new in DWARF 4.</span>
    <span id="TagTypeUnit">TagTypeUnit</span>            <a href="#Tag">Tag</a> = 0x41
    <span id="TagRvalueReferenceType">TagRvalueReferenceType</span> <a href="#Tag">Tag</a> = 0x42
    <span id="TagTemplateAlias">TagTemplateAlias</span>       <a href="#Tag">Tag</a> = 0x43
)</pre>


<h3 id="Tag.GoString">func (Tag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/const.go#L354">GoString</a>
    <a href="#Tag.GoString">¶</a></h3>
<pre>func (t <a href="#Tag">Tag</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="Tag.String">func (Tag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/const.go#L344">String</a>
    <a href="#Tag.String">¶</a></h3>
<pre>func (t <a href="#Tag">Tag</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Type">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L5">Type</a>
    <a href="#Type">¶</a></h2>
<pre>type Type interface {
    Common() *<a href="#CommonType">CommonType</a>
    String() <a href="/builtin/#string">string</a>
    Size() <a href="/builtin/#int64">int64</a>
}</pre>

A Type conventionally represents a pointer to any of the specific Type
structures (CharType, StructType, etc.).

<h2 id="TypedefType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L245">TypedefType</a>
    <a href="#TypedefType">¶</a></h2>
<pre>type TypedefType struct {
    <a href="#CommonType">CommonType</a>
<span id="TypedefType.Type"></span>    Type <a href="#Type">Type</a>
}</pre>

A TypedefType represents a named type.

<h3 id="TypedefType.Size">func (*TypedefType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L252">Size</a>
    <a href="#TypedefType.Size">¶</a></h3>
<pre>func (t *<a href="#TypedefType">TypedefType</a>) Size() <a href="/builtin/#int64">int64</a></pre>


<h3 id="TypedefType.String">func (*TypedefType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L250">String</a>
    <a href="#TypedefType.String">¶</a></h3>
<pre>func (t *<a href="#TypedefType">TypedefType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="UcharType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L47">UcharType</a>
    <a href="#UcharType">¶</a></h2>
<pre>type UcharType struct {
    <a href="#BasicType">BasicType</a>
}</pre>

A UcharType represents an unsigned character type.

<h2 id="UintType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L57">UintType</a>
    <a href="#UintType">¶</a></h2>
<pre>type UintType struct {
    <a href="#BasicType">BasicType</a>
}</pre>

A UintType represents an unsigned integer type.

<h2 id="UnspecifiedType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L82">UnspecifiedType</a>
    <a href="#UnspecifiedType">¶</a></h2>
<pre>type UnspecifiedType struct {
    <a href="#BasicType">BasicType</a>
}</pre>

An UnspecifiedType represents an implicit, unknown, ambiguous or nonexistent
type.

<h2 id="VoidType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L119">VoidType</a>
    <a href="#VoidType">¶</a></h2>
<pre>type VoidType struct {
    <a href="#CommonType">CommonType</a>
}</pre>

A VoidType represents the C void type.

<h3 id="VoidType.String">func (*VoidType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/dwarf/type.go#L123">String</a>
    <a href="#VoidType.String">¶</a></h3>
<pre>func (t *<a href="#VoidType">VoidType</a>) String() <a href="/builtin/#string">string</a></pre>



