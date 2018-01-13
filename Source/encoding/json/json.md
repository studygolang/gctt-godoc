version: 1.9.2
## package json

  `import "encoding/json"`

## Overview

Package json implements encoding and decoding of JSON as defined in RFC 4627.
The mapping between JSON and Go values is described in the documentation for the
Marshal and Unmarshal functions.

See "JSON and Go" for an introduction to this package:
https://golang.org/doc/articles/json_and_go.html

<a id="example_customMarshalJSON"></a>
Example:

    package json_test

    import (
        "encoding/json"
        "fmt"
        "log"
        "strings"
    )

    type Animal int

    const (
        Unknown Animal = iota
        Gopher
        Zebra
    )

    func (a *Animal) UnmarshalJSON(b []byte) error {
        var s string
        if err := json.Unmarshal(b, &s); err != nil {
            return err
        }
        switch strings.ToLower(s) {
        default:
            *a = Unknown
        case "gopher":
            *a = Gopher
        case "zebra":
            *a = Zebra
        }

        return nil
    }

    func (a Animal) MarshalJSON() ([]byte, error) {
        var s string
        switch a {
        default:
            s = "unknown"
        case Gopher:
            s = "gopher"
        case Zebra:
            s = "zebra"
        }

        return json.Marshal(s)
    }

    func Example_customMarshalJSON() {
        blob := `["gopher","armadillo","zebra","unknown","gopher","bee","gopher","zebra"]`
        var zoo []Animal
        if err := json.Unmarshal([]byte(blob), &zoo); err != nil {
            log.Fatal(err)
        }

        census := make(map[Animal]int)
        for _, animal := range zoo {
            census[animal] += 1
        }

        fmt.Printf("Zoo Census:\n* Gophers: %d\n* Zebras:  %d\n* Unknown: %d\n",
            census[Gopher], census[Zebra], census[Unknown])

        // Output:
        // Zoo Census:
        // * Gophers: 3
        // * Zebras:  2
        // * Unknown: 3
    }

## Index

- [func Compact(dst *bytes.Buffer, src []byte) error](#Compact)
- [func HTMLEscape(dst *bytes.Buffer, src []byte)](#HTMLEscape)
- [func Indent(dst *bytes.Buffer, src []byte, prefix, indent string) error](#Indent)
- [func Marshal(v interface{}) ([]byte, error)](#Marshal)
- [func MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)](#MarshalIndent)
- [func Unmarshal(data []byte, v interface{}) error](#Unmarshal)
- [func Valid(data []byte) bool](#Valid)
- [type Decoder](#Decoder)
  - [func NewDecoder(r io.Reader) *Decoder](#NewDecoder)
  - [func (dec *Decoder) Buffered() io.Reader](#Decoder.Buffered)
  - [func (dec *Decoder) Decode(v interface{}) error](#Decoder.Decode)
  - [func (dec *Decoder) More() bool](#Decoder.More)
  - [func (dec *Decoder) Token() (Token, error)](#Decoder.Token)
  - [func (dec *Decoder) UseNumber()](#Decoder.UseNumber)
- [type Delim](#Delim)
  - [func (d Delim) String() string](#Delim.String)
- [type Encoder](#Encoder)
  - [func NewEncoder(w io.Writer) *Encoder](#NewEncoder)
  - [func (enc *Encoder) Encode(v interface{}) error](#Encoder.Encode)
  - [func (enc *Encoder) SetEscapeHTML(on bool)](#Encoder.SetEscapeHTML)
  - [func (enc *Encoder) SetIndent(prefix, indent string)](#Encoder.SetIndent)
- [type InvalidUTF8Error](#InvalidUTF8Error)
  - [func (e *InvalidUTF8Error) Error() string](#InvalidUTF8Error.Error)
- [type InvalidUnmarshalError](#InvalidUnmarshalError)
  - [func (e *InvalidUnmarshalError) Error() string](#InvalidUnmarshalError.Error)
- [type Marshaler](#Marshaler)
- [type MarshalerError](#MarshalerError)
  - [func (e *MarshalerError) Error() string](#MarshalerError.Error)
- [type Number](#Number)
  - [func (n Number) Float64() (float64, error)](#Number.Float64)
  - [func (n Number) Int64() (int64, error)](#Number.Int64)
  - [func (n Number) String() string](#Number.String)
- [type RawMessage](#RawMessage)
  - [func (m RawMessage) MarshalJSON() ([]byte, error)](#RawMessage.MarshalJSON)
  - [func (m *RawMessage) UnmarshalJSON(data []byte) error](#RawMessage.UnmarshalJSON)
- [type SyntaxError](#SyntaxError)
  - [func (e *SyntaxError) Error() string](#SyntaxError.Error)
- [type Token](#Token)
- [type UnmarshalFieldError](#UnmarshalFieldError)
  - [func (e *UnmarshalFieldError) Error() string](#UnmarshalFieldError.Error)
- [type UnmarshalTypeError](#UnmarshalTypeError)
  - [func (e *UnmarshalTypeError) Error() string](#UnmarshalTypeError.Error)
- [type Unmarshaler](#Unmarshaler)
- [type UnsupportedTypeError](#UnsupportedTypeError)
  - [func (e *UnsupportedTypeError) Error() string](#UnsupportedTypeError.Error)
- [type UnsupportedValueError](#UnsupportedValueError)
  - [func (e *UnsupportedValueError) Error() string](#UnsupportedValueError.Error)

### Examples

- [Decoder](#exampleDecoder)
- [Decoder.Decode (Stream)](#exampleDecoder_Decode_stream)
- [Decoder.Token](#exampleDecoder_Token)
- [Indent](#exampleIndent)
- [Marshal](#exampleMarshal)
- [RawMessage (Marshal)](#exampleRawMessage_marshal)
- [RawMessage (Unmarshal)](#exampleRawMessage_unmarshal)
- [Unmarshal](#exampleUnmarshal)
- [Package (CustomMarshalJSON)](#example_customMarshalJSON)

### Package files
 [decode.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go) [encode.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go) [fold.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/fold.go) [indent.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/indent.go) [scanner.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/scanner.go) [stream.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go) [tables.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/tables.go) [tags.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/tags.go)

<h2 id="Compact">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/indent.go#L1">Compact</a>
    <a href="#Compact">¶</a></h2>
<pre>func Compact(dst *<a href="/bytes/">bytes</a>.<a href="/bytes/#Buffer">Buffer</a>, src []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

Compact appends to dst the JSON-encoded src with insignificant space characters
elided.

<h2 id="HTMLEscape">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L178">HTMLEscape</a>
    <a href="#HTMLEscape">¶</a></h2>
<pre>func HTMLEscape(dst *<a href="/bytes/">bytes</a>.<a href="/bytes/#Buffer">Buffer</a>, src []<a href="/builtin/#byte">byte</a>)</pre>

HTMLEscape appends to dst the JSON-encoded src with <, >, &, U+2028 and U+2029
characters inside string literals changed to \u003c, \u003e, \u0026, \u2028,
\u2029 so that the JSON will be safe to embed inside HTML <script> tags. For
historical reasons, web browsers don't honor standard HTML escaping within
<script> tags, so an alternative JSON encoding must be used.

<h2 id="Indent">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/indent.go#L69">Indent</a>
    <a href="#Indent">¶</a></h2>
<pre>func Indent(dst *<a href="/bytes/">bytes</a>.<a href="/bytes/#Buffer">Buffer</a>, src []<a href="/builtin/#byte">byte</a>, prefix, indent <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Indent appends to dst an indented form of the JSON-encoded src. Each element in
a JSON object or array begins on a new, indented line beginning with prefix
followed by one or more copies of indent according to the indentation nesting.
The data appended to dst does not begin with the prefix nor any indentation, to
make it easier to embed inside other formatted JSON data. Although leading space
characters (space, tab, carriage return, newline) at the beginning of src are
dropped, trailing space characters at the end of src are preserved and copied to
dst. For example, if src has no trailing spaces, neither will dst; if src ends
in a trailing newline, so will dst.

<a id="exampleIndent"></a>
Example:

    type Road struct {
        Name   string
        Number int
    }
    roads := []Road{
        {"Diamond Fork", 29},
        {"Sheep Creek", 51},
    }

    b, err := json.Marshal(roads)
    if err != nil {
        log.Fatal(err)
    }

    var out bytes.Buffer
    json.Indent(&out, b, "=", "\t")
    out.WriteTo(os.Stdout)
    // Output:
    // [
    // =	{
    // =		"Name": "Diamond Fork",
    // =		"Number": 29
    // =	},
    // =	{
    // =		"Name": "Sheep Creek",
    // =		"Number": 51
    // =	}
    // =]

<h2 id="Marshal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L149">Marshal</a>
    <a href="#Marshal">¶</a></h2>
<pre>func Marshal(v interface{}) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Marshal returns the JSON encoding of v.

Marshal traverses the value v recursively. If an encountered value implements
the Marshaler interface and is not a nil pointer, Marshal calls its MarshalJSON
method to produce JSON. If no MarshalJSON method is present but the value
implements encoding.TextMarshaler instead, Marshal calls its MarshalText method
and encodes the result as a JSON string. The nil pointer exception is not
strictly necessary but mimics a similar, necessary exception in the behavior of
UnmarshalJSON.

Otherwise, Marshal uses the following type-dependent default encodings:

Boolean values encode as JSON booleans.

Floating point, integer, and Number values encode as JSON numbers.

String values encode as JSON strings coerced to valid UTF-8, replacing invalid
bytes with the Unicode replacement rune. The angle brackets "<" and ">" are
escaped to "\u003c" and "\u003e" to keep some browsers from misinterpreting JSON
output as HTML. Ampersand "&" is also escaped to "\u0026" for the same reason.
This escaping can be disabled using an Encoder that had SetEscapeHTML(false)
called on it.

Array and slice values encode as JSON arrays, except that []byte encodes as a
base64-encoded string, and a nil slice encodes as the null JSON value.

Struct values encode as JSON objects. Each exported struct field becomes a
member of the object, using the field name as the object key, unless the field
is omitted for one of the reasons given below.

The encoding of each struct field can be customized by the format string stored
under the "json" key in the struct field's tag. The format string gives the name
of the field, possibly followed by a comma-separated list of options. The name
may be empty in order to specify options without overriding the default field
name.

The "omitempty" option specifies that the field should be omitted from the
encoding if the field has an empty value, defined as false, 0, a nil pointer, a
nil interface value, and any empty array, slice, map, or string.

As a special case, if the field tag is "-", the field is always omitted. Note
that a field with name "-" can still be generated using the tag "-,".

Examples of struct field tags and their meanings:

    // Field appears in JSON as key "myName".
    Field int `json:"myName"`

    // Field appears in JSON as key "myName" and
    // the field is omitted from the object if its value is empty,
    // as defined above.
    Field int `json:"myName,omitempty"`

    // Field appears in JSON as key "Field" (the default), but
    // the field is skipped if empty.
    // Note the leading comma.
    Field int `json:",omitempty"`

    // Field is ignored by this package.
    Field int `json:"-"`

    // Field appears in JSON as key "-".
    Field int `json:"-,"`

The "string" option signals that a field is stored as JSON inside a JSON-encoded
string. It applies only to fields of string, floating point, integer, or boolean
types. This extra level of encoding is sometimes used when communicating with
JavaScript programs:

    Int64String int64 `json:",string"`

The key name will be used if it's a non-empty string consisting of only Unicode
letters, digits, and ASCII punctuation except quotation marks, backslash, and
comma.

Anonymous struct fields are usually marshaled as if their inner exported fields
were fields in the outer struct, subject to the usual Go visibility rules
amended as described in the next paragraph. An anonymous struct field with a
name given in its JSON tag is treated as having that name, rather than being
anonymous. An anonymous struct field of interface type is treated the same as
having that type as its name, rather than being anonymous.

The Go visibility rules for struct fields are amended for JSON when deciding
which field to marshal or unmarshal. If there are multiple fields at the same
level, and that level is the least nested (and would therefore be the nesting
level selected by the usual Go rules), the following extra rules apply:

1) Of those fields, if any are JSON-tagged, only tagged fields are considered,
even if there are multiple untagged fields that would otherwise conflict.

2) If there is exactly one field (tagged or not according to the first rule),
that is selected.

3) Otherwise there are multiple fields, and all are ignored; no error occurs.

Handling of anonymous struct fields is new in Go 1.1. Prior to Go 1.1, anonymous
struct fields were ignored. To force ignoring of an anonymous struct field in
both current and earlier versions, give the field a JSON tag of "-".

Map values encode as JSON objects. The map's key type must either be a string,
an integer type, or implement encoding.TextMarshaler. The map keys are sorted
and used as JSON object keys by applying the following rules, subject to the
UTF-8 coercion described for string values above:

    - string keys are used directly
    - encoding.TextMarshalers are marshaled
    - integer keys are converted to strings

Pointer values encode as the value pointed to. A nil pointer encodes as the null
JSON value.

Interface values encode as the value contained in the interface. A nil interface
value encodes as the null JSON value.

Channel, complex, and function values cannot be encoded in JSON. Attempting to
encode such a value causes Marshal to return an UnsupportedTypeError.

JSON cannot represent cyclic data structures and Marshal does not handle them.
Passing cyclic structures to Marshal will result in an infinite recursion.

<a id="exampleMarshal"></a>
Example:

    type ColorGroup struct {
        ID     int
        Name   string
        Colors []string
    }
    group := ColorGroup{
        ID:     1,
        Name:   "Reds",
        Colors: []string{"Crimson", "Red", "Ruby", "Maroon"},
    }
    b, err := json.Marshal(group)
    if err != nil {
        fmt.Println("error:", err)
    }
    os.Stdout.Write(b)
    // Output:
    // {"ID":1,"Name":"Reds","Colors":["Crimson","Red","Ruby","Maroon"]}

<h2 id="MarshalIndent">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L159">MarshalIndent</a>
    <a href="#MarshalIndent">¶</a></h2>
<pre>func MarshalIndent(v interface{}, prefix, indent <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

MarshalIndent is like Marshal but applies Indent to format the output.

<h2 id="Unmarshal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L86">Unmarshal</a>
    <a href="#Unmarshal">¶</a></h2>
<pre>func Unmarshal(data []<a href="/builtin/#byte">byte</a>, v interface{}) <a href="/builtin/#error">error</a></pre>

Unmarshal parses the JSON-encoded data and stores the result in the value
pointed to by v. If v is nil or not a pointer, Unmarshal returns an
InvalidUnmarshalError.

Unmarshal uses the inverse of the encodings that Marshal uses, allocating maps,
slices, and pointers as necessary, with the following additional rules:

To unmarshal JSON into a pointer, Unmarshal first handles the case of the JSON
being the JSON literal null. In that case, Unmarshal sets the pointer to nil.
Otherwise, Unmarshal unmarshals the JSON into the value pointed at by the
pointer. If the pointer is nil, Unmarshal allocates a new value for it to point
to.

To unmarshal JSON into a value implementing the Unmarshaler interface, Unmarshal
calls that value's UnmarshalJSON method, including when the input is a JSON
null. Otherwise, if the value implements encoding.TextUnmarshaler and the input
is a JSON quoted string, Unmarshal calls that value's UnmarshalText method with
the unquoted form of the string.

To unmarshal JSON into a struct, Unmarshal matches incoming object keys to the
keys used by Marshal (either the struct field name or its tag), preferring an
exact match but also accepting a case-insensitive match. Unmarshal will only set
exported fields of the struct.

To unmarshal JSON into an interface value, Unmarshal stores one of these in the
interface value:

    bool, for JSON booleans
    float64, for JSON numbers
    string, for JSON strings
    []interface{}, for JSON arrays
    map[string]interface{}, for JSON objects
    nil for JSON null

To unmarshal a JSON array into a slice, Unmarshal resets the slice length to
zero and then appends each element to the slice. As a special case, to unmarshal
an empty JSON array into a slice, Unmarshal replaces the slice with a new empty
slice.

To unmarshal a JSON array into a Go array, Unmarshal decodes JSON array elements
into corresponding Go array elements. If the Go array is smaller than the JSON
array, the additional JSON array elements are discarded. If the JSON array is
smaller than the Go array, the additional Go array elements are set to zero
values.

To unmarshal a JSON object into a map, Unmarshal first establishes a map to use.
If the map is nil, Unmarshal allocates a new map. Otherwise Unmarshal reuses the
existing map, keeping existing entries. Unmarshal then stores key-value pairs
from the JSON object into the map. The map's key type must either be a string,
an integer, or implement encoding.TextUnmarshaler.

If a JSON value is not appropriate for a given target type, or if a JSON number
overflows the target type, Unmarshal skips that field and completes the
unmarshaling as best it can. If no more serious errors are encountered,
Unmarshal returns an UnmarshalTypeError describing the earliest such error. In
any case, it's not guaranteed that all the remaining fields following the
problematic one will be unmarshaled into the target object.

The JSON null value unmarshals into an interface, map, pointer, or slice by
setting that Go value to nil. Because null is often used in JSON to mean ``not
present,'' unmarshaling a JSON null into any other Go type has no effect on the
value and produces no error.

When unmarshaling quoted strings, invalid UTF-8 or invalid UTF-16 surrogate
pairs are not treated as an error. Instead, they are replaced by the Unicode
replacement character U+FFFD.

<a id="exampleUnmarshal"></a>
Example:

    var jsonBlob = []byte(`[
    		{"Name": "Platypus", "Order": "Monotremata"},
    		{"Name": "Quoll",    "Order": "Dasyuromorphia"}
    	]`)
    type Animal struct {
        Name  string
        Order string
    }
    var animals []Animal
    err := json.Unmarshal(jsonBlob, &animals)
    if err != nil {
        fmt.Println("error:", err)
    }
    fmt.Printf("%+v", animals)
    // Output:
    // [{Name:Platypus Order:Monotremata} {Name:Quoll Order:Dasyuromorphia}]

<h2 id="Valid">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/scanner.go#L9">Valid</a>
    <a href="#Valid">¶</a></h2>
<pre>func Valid(data []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#bool">bool</a></pre>

Valid reports whether data is a valid JSON encoding.

<h2 id="Decoder">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L4">Decoder</a>
    <a href="#Decoder">¶</a></h2>
<pre>type Decoder struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Decoder reads and decodes JSON values from an input stream.

<a id="exampleDecoder"></a>
Example:

    const jsonStream = `
    		{"Name": "Ed", "Text": "Knock knock."}
    		{"Name": "Sam", "Text": "Who's there?"}
    		{"Name": "Ed", "Text": "Go fmt."}
    		{"Name": "Sam", "Text": "Go fmt who?"}
    		{"Name": "Ed", "Text": "Go fmt yourself!"}
    	`
    type Message struct {
        Name, Text string
    }
    dec := json.NewDecoder(strings.NewReader(jsonStream))
    for {
        var m Message
        if err := dec.Decode(&m); err == io.EOF {
            break
        } else if err != nil {
            log.Fatal(err)
        }
        fmt.Printf("%s: %s\n", m.Name, m.Text)
    }
    // Output:
    // Ed: Knock knock.
    // Sam: Who's there?
    // Ed: Go fmt.
    // Sam: Go fmt who?
    // Ed: Go fmt yourself!

<h3 id="NewDecoder">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L20">NewDecoder</a>
    <a href="#NewDecoder">¶</a></h3>
<pre>func NewDecoder(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Decoder">Decoder</a></pre>

NewDecoder returns a new decoder that reads from r.

The decoder introduces its own buffering and may read data from r beyond the
JSON values requested.

<h3 id="Decoder.Buffered">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L67">Buffered</a>
    <a href="#Decoder.Buffered">¶</a></h3>
<pre>func (dec *<a href="#Decoder">Decoder</a>) Buffered() <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a></pre>

Buffered returns a reader of the data remaining in the Decoder's buffer. The
reader is valid until the next call to Decode.

<h3 id="Decoder.Decode">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L33">Decode</a>
    <a href="#Decoder.Decode">¶</a></h3>
<pre>func (dec *<a href="#Decoder">Decoder</a>) Decode(v interface{}) <a href="/builtin/#error">error</a></pre>

Decode reads the next JSON-encoded value from its input and stores it in the
value pointed to by v.

See the documentation for Unmarshal for details about the conversion of JSON
into a Go value.

<a id="exampleDecoder_Decode_stream"></a>
Example:

    const jsonStream = `
    	[
    		{"Name": "Ed", "Text": "Knock knock."},
    		{"Name": "Sam", "Text": "Who's there?"},
    		{"Name": "Ed", "Text": "Go fmt."},
    		{"Name": "Sam", "Text": "Go fmt who?"},
    		{"Name": "Ed", "Text": "Go fmt yourself!"}
    	]
    `
    type Message struct {
        Name, Text string
    }
    dec := json.NewDecoder(strings.NewReader(jsonStream))

    // read open bracket
    t, err := dec.Token()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%T: %v\n", t, t)

    // while the array contains values
    for dec.More() {
        var m Message
        // decode an array value (Message)
        err := dec.Decode(&m)
        if err != nil {
            log.Fatal(err)
        }

        fmt.Printf("%v: %v\n", m.Name, m.Text)
    }

    // read closing bracket
    t, err = dec.Token()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%T: %v\n", t, t)

    // Output:
    // json.Delim: [
    // Ed: Knock knock.
    // Sam: Who's there?
    // Ed: Go fmt.
    // Sam: Go fmt who?
    // Ed: Go fmt yourself!
    // json.Delim: ]

<h3 id="Decoder.More">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L470">More</a>
    <a href="#Decoder.More">¶</a></h3>
<pre>func (dec *<a href="#Decoder">Decoder</a>) More() <a href="/builtin/#bool">bool</a></pre>

More reports whether there is another element in the current array or object
being parsed.

<h3 id="Decoder.Token">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L347">Token</a>
    <a href="#Decoder.Token">¶</a></h3>
<pre>func (dec *<a href="#Decoder">Decoder</a>) Token() (<a href="#Token">Token</a>, <a href="/builtin/#error">error</a>)</pre>

Token returns the next JSON token in the input stream. At the end of the input
stream, Token returns nil, io.EOF.

Token guarantees that the delimiters [ ] { } it returns are properly nested and
matched: if Token encounters an unexpected delimiter in the input, it will
return an error.

The input stream consists of basic JSON values—bool, string, number, and
null—along with delimiters [ ] { } of type Delim to mark the start and end of
arrays and objects. Commas and colons are elided.

<a id="exampleDecoder_Token"></a>
Example:

    const jsonStream = `
    		{"Message": "Hello", "Array": [1, 2, 3], "Null": null, "Number": 1.234}
    	`
    dec := json.NewDecoder(strings.NewReader(jsonStream))
    for {
        t, err := dec.Token()
        if err == io.EOF {
            break
        }
        if err != nil {
            log.Fatal(err)
        }
        fmt.Printf("%T: %v", t, t)
        if dec.More() {
            fmt.Printf(" (more)")
        }
        fmt.Printf("\n")
    }
    // Output:
    // json.Delim: { (more)
    // string: Message (more)
    // string: Hello (more)
    // string: Array (more)
    // json.Delim: [ (more)
    // float64: 1 (more)
    // float64: 2 (more)
    // float64: 3
    // json.Delim: ] (more)
    // string: Null (more)
    // <nil>: <nil> (more)
    // string: Number (more)
    // float64: 1.234
    // json.Delim: }

<h3 id="Decoder.UseNumber">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L26">UseNumber</a>
    <a href="#Decoder.UseNumber">¶</a></h3>
<pre>func (dec *<a href="#Decoder">Decoder</a>) UseNumber()</pre>

UseNumber causes the Decoder to unmarshal a number into an interface{} as a
Number instead of as a float64.

<h2 id="Delim">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L330">Delim</a>
    <a href="#Delim">¶</a></h2>
<pre>type Delim <a href="/builtin/#rune">rune</a></pre>

A Delim is a JSON array or object delimiter, one of [ ] { or }.

<h3 id="Delim.String">func (Delim) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L332">String</a>
    <a href="#Delim.String">¶</a></h3>
<pre>func (d <a href="#Delim">Delim</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Encoder">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L158">Encoder</a>
    <a href="#Encoder">¶</a></h2>
<pre>type Encoder struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

An Encoder writes JSON values to an output stream.

<h3 id="NewEncoder">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L169">NewEncoder</a>
    <a href="#NewEncoder">¶</a></h3>
<pre>func NewEncoder(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) *<a href="#Encoder">Encoder</a></pre>

NewEncoder returns a new encoder that writes to w.

<h3 id="Encoder.Encode">func (*Encoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L178">Encode</a>
    <a href="#Encoder.Encode">¶</a></h3>
<pre>func (enc *<a href="#Encoder">Encoder</a>) Encode(v interface{}) <a href="/builtin/#error">error</a></pre>

Encode writes the JSON encoding of v to the stream, followed by a newline
character.

See the documentation for Marshal for details about the conversion of Go values
to JSON.

<h3 id="Encoder.SetEscapeHTML">func (*Encoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L230">SetEscapeHTML</a>
    <a href="#Encoder.SetEscapeHTML">¶</a></h3>
<pre>func (enc *<a href="#Encoder">Encoder</a>) SetEscapeHTML(on <a href="/builtin/#bool">bool</a>)</pre>

SetEscapeHTML specifies whether problematic HTML characters should be escaped
inside JSON quoted strings. The default behavior is to escape &, <, and > to
\u0026, \u003c, and \u003e to avoid certain safety problems that can arise when
embedding JSON in HTML.

In non-HTML settings where the escaping interferes with the readability of the
output, SetEscapeHTML(false) disables this behavior.

<h3 id="Encoder.SetIndent">func (*Encoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L218">SetIndent</a>
    <a href="#Encoder.SetIndent">¶</a></h3>
<pre>func (enc *<a href="#Encoder">Encoder</a>) SetIndent(prefix, indent <a href="/builtin/#string">string</a>)</pre>

SetIndent instructs the encoder to format each subsequent encoded value as if
indented by the package-level function Indent(dst, src, prefix, indent). Calling
SetIndent("", "") disables indentation.

<h2 id="InvalidUTF8Error">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L238">InvalidUTF8Error</a>
    <a href="#InvalidUTF8Error">¶</a></h2>
<pre>type InvalidUTF8Error struct {
<span id="InvalidUTF8Error.S"></span>    S <a href="/builtin/#string">string</a> <span class="comment">// the whole string value that caused the error</span>
}</pre>

Before Go 1.2, an InvalidUTF8Error was returned by Marshal when attempting to
encode a string value with invalid UTF-8 sequences. As of Go 1.2, Marshal
instead coerces the string to valid UTF-8 by replacing invalid bytes with the
Unicode replacement rune U+FFFD. This error is no longer generated but is kept
for backwards compatibility with programs that might mention it.

<h3 id="InvalidUTF8Error.Error">func (*InvalidUTF8Error) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L242">Error</a>
    <a href="#InvalidUTF8Error.Error">¶</a></h3>
<pre>func (e *<a href="#InvalidUTF8Error">InvalidUTF8Error</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="InvalidUnmarshalError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L144">InvalidUnmarshalError</a>
    <a href="#InvalidUnmarshalError">¶</a></h2>
<pre>type InvalidUnmarshalError struct {
<span id="InvalidUnmarshalError.Type"></span>    Type <a href="/reflect/">reflect</a>.<a href="/reflect/#Type">Type</a>
}</pre>

An InvalidUnmarshalError describes an invalid argument passed to Unmarshal. (The
argument to Unmarshal must be a non-nil pointer.)

<h3 id="InvalidUnmarshalError.Error">func (*InvalidUnmarshalError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L148">Error</a>
    <a href="#InvalidUnmarshalError.Error">¶</a></h3>
<pre>func (e *<a href="#InvalidUnmarshalError">InvalidUnmarshalError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Marshaler">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L209">Marshaler</a>
    <a href="#Marshaler">¶</a></h2>
<pre>type Marshaler interface {
    MarshalJSON() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)
}</pre>

Marshaler is the interface implemented by types that can marshal themselves into
valid JSON.

<h2 id="MarshalerError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L246">MarshalerError</a>
    <a href="#MarshalerError">¶</a></h2>
<pre>type MarshalerError struct {
<span id="MarshalerError.Type"></span>    Type <a href="/reflect/">reflect</a>.<a href="/reflect/#Type">Type</a>
<span id="MarshalerError.Err"></span>    Err  <a href="/builtin/#error">error</a>
}</pre>


<h3 id="MarshalerError.Error">func (*MarshalerError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L251">Error</a>
    <a href="#MarshalerError.Error">¶</a></h3>
<pre>func (e *<a href="#MarshalerError">MarshalerError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Number">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L182">Number</a>
    <a href="#Number">¶</a></h2>
<pre>type Number <a href="/builtin/#string">string</a></pre>

A Number represents a JSON number literal.

<h3 id="Number.Float64">func (Number) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L188">Float64</a>
    <a href="#Number.Float64">¶</a></h3>
<pre>func (n <a href="#Number">Number</a>) Float64() (<a href="/builtin/#float64">float64</a>, <a href="/builtin/#error">error</a>)</pre>

Float64 returns the number as a float64.

<h3 id="Number.Int64">func (Number) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L193">Int64</a>
    <a href="#Number.Int64">¶</a></h3>
<pre>func (n <a href="#Number">Number</a>) Int64() (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>

Int64 returns the number as an int64.

<h3 id="Number.String">func (Number) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L185">String</a>
    <a href="#Number.String">¶</a></h3>
<pre>func (n <a href="#Number">Number</a>) String() <a href="/builtin/#string">string</a></pre>

String returns the literal text of the number.

<h2 id="RawMessage">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L237">RawMessage</a>
    <a href="#RawMessage">¶</a></h2>
<pre>type RawMessage []<a href="/builtin/#byte">byte</a></pre>

RawMessage is a raw encoded JSON value. It implements Marshaler and Unmarshaler
and can be used to delay JSON decoding or precompute a JSON encoding.

<a id="exampleRawMessage_marshal"></a>
Example:

    h := json.RawMessage(`{"precomputed": true}`)

    c := struct {
        Header *json.RawMessage `json:"header"`
        Body   string           `json:"body"`
    }{Header: &h, Body: "Hello Gophers!"}

    b, err := json.MarshalIndent(&c, "", "\t")
    if err != nil {
        fmt.Println("error:", err)
    }
    os.Stdout.Write(b)

    // Output:
    // {
    // 	"header": {
    // 		"precomputed": true
    // 	},
    // 	"body": "Hello Gophers!"
    // }


<a id="exampleRawMessage_unmarshal"></a>
Example:

    type Color struct {
        Space string
        Point json.RawMessage // delay parsing until we know the color space
    }
    type RGB struct {
        R   uint8
        G   uint8
        B   uint8
    }
    type YCbCr struct {
        Y   uint8
        Cb  int8
        Cr  int8
    }

    var j = []byte(`[
    		{"Space": "YCbCr", "Point": {"Y": 255, "Cb": 0, "Cr": -10}},
    		{"Space": "RGB",   "Point": {"R": 98, "G": 218, "B": 255}}
    	]`)
    var colors []Color
    err := json.Unmarshal(j, &colors)
    if err != nil {
        log.Fatalln("error:", err)
    }

    for _, c := range colors {
        var dst interface{}
        switch c.Space {
        case "RGB":
            dst = new(RGB)
        case "YCbCr":
            dst = new(YCbCr)
        }
        err := json.Unmarshal(c.Point, dst)
        if err != nil {
            log.Fatalln("error:", err)
        }
        fmt.Println(c.Space, dst)
    }
    // Output:
    // YCbCr &{255 0 -10}
    // RGB &{98 218 255}

<h3 id="RawMessage.MarshalJSON">func (RawMessage) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L240">MarshalJSON</a>
    <a href="#RawMessage.MarshalJSON">¶</a></h3>
<pre>func (m <a href="#RawMessage">RawMessage</a>) MarshalJSON() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

MarshalJSON returns m as the JSON encoding of m.

<h3 id="RawMessage.UnmarshalJSON">func (*RawMessage) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L248">UnmarshalJSON</a>
    <a href="#RawMessage.UnmarshalJSON">¶</a></h3>
<pre>func (m *<a href="#RawMessage">RawMessage</a>) UnmarshalJSON(data []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

UnmarshalJSON sets *m to a copy of data.

<h2 id="SyntaxError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/scanner.go#L59">SyntaxError</a>
    <a href="#SyntaxError">¶</a></h2>
<pre>type SyntaxError struct {
<span id="SyntaxError.Offset"></span>    Offset <a href="/builtin/#int64">int64</a> <span class="comment">// error occurred after reading Offset bytes</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A SyntaxError is a description of a JSON syntax error.

<h3 id="SyntaxError.Error">func (*SyntaxError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/scanner.go#L64">Error</a>
    <a href="#SyntaxError.Error">¶</a></h3>
<pre>func (e *<a href="#SyntaxError">SyntaxError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Token">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/stream.go#L268">Token</a>
    <a href="#Token">¶</a></h2>
<pre>type Token interface{}</pre>

A Token holds a value of one of these types:

    Delim, for the four JSON delimiters [ ] { }
    bool, for JSON booleans
    float64, for JSON numbers
    Number, for JSON numbers
    string, for JSON string literals
    nil, for JSON null

<h2 id="UnmarshalFieldError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L132">UnmarshalFieldError</a>
    <a href="#UnmarshalFieldError">¶</a></h2>
<pre>type UnmarshalFieldError struct {
<span id="UnmarshalFieldError.Key"></span>    Key   <a href="/builtin/#string">string</a>
<span id="UnmarshalFieldError.Type"></span>    Type  <a href="/reflect/">reflect</a>.<a href="/reflect/#Type">Type</a>
<span id="UnmarshalFieldError.Field"></span>    Field <a href="/reflect/">reflect</a>.<a href="/reflect/#StructField">StructField</a>
}</pre>

An UnmarshalFieldError describes a JSON object key that led to an unexported
(and therefore unwritable) struct field. (No longer used; kept for
compatibility.)

<h3 id="UnmarshalFieldError.Error">func (*UnmarshalFieldError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L138">Error</a>
    <a href="#UnmarshalFieldError.Error">¶</a></h3>
<pre>func (e *<a href="#UnmarshalFieldError">UnmarshalFieldError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="UnmarshalTypeError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L114">UnmarshalTypeError</a>
    <a href="#UnmarshalTypeError">¶</a></h2>
<pre>type UnmarshalTypeError struct {
<span id="UnmarshalTypeError.Value"></span>    Value  <a href="/builtin/#string">string</a>       <span class="comment">// description of JSON value - &#34;bool&#34;, &#34;array&#34;, &#34;number -5&#34;</span>
<span id="UnmarshalTypeError.Type"></span>    Type   <a href="/reflect/">reflect</a>.<a href="/reflect/#Type">Type</a> <span class="comment">// type of Go value it could not be assigned to</span>
<span id="UnmarshalTypeError.Offset"></span>    Offset <a href="/builtin/#int64">int64</a>        <span class="comment">// error occurred after reading Offset bytes</span>
<span id="UnmarshalTypeError.Struct"></span>    Struct <a href="/builtin/#string">string</a>       <span class="comment">// name of the struct type containing the field</span>
<span id="UnmarshalTypeError.Field"></span>    Field  <a href="/builtin/#string">string</a>       <span class="comment">// name of the field holding the Go value</span>
}</pre>

An UnmarshalTypeError describes a JSON value that was not appropriate for a
value of a specific Go type.

<h3 id="UnmarshalTypeError.Error">func (*UnmarshalTypeError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L122">Error</a>
    <a href="#UnmarshalTypeError.Error">¶</a></h3>
<pre>func (e *<a href="#UnmarshalTypeError">UnmarshalTypeError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Unmarshaler">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/decode.go#L108">Unmarshaler</a>
    <a href="#Unmarshaler">¶</a></h2>
<pre>type Unmarshaler interface {
    UnmarshalJSON([]<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a>
}</pre>

Unmarshaler is the interface implemented by types that can unmarshal a JSON
description of themselves. The input can be assumed to be a valid encoding of a
JSON value. UnmarshalJSON must copy the JSON data if it wishes to retain the
data after returning.

By convention, to approximate the behavior of Unmarshal itself, Unmarshalers
implement UnmarshalJSON([]byte("null")) as a no-op.

<h2 id="UnsupportedTypeError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L215">UnsupportedTypeError</a>
    <a href="#UnsupportedTypeError">¶</a></h2>
<pre>type UnsupportedTypeError struct {
<span id="UnsupportedTypeError.Type"></span>    Type <a href="/reflect/">reflect</a>.<a href="/reflect/#Type">Type</a>
}</pre>

An UnsupportedTypeError is returned by Marshal when attempting to encode an
unsupported value type.

<h3 id="UnsupportedTypeError.Error">func (*UnsupportedTypeError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L219">Error</a>
    <a href="#UnsupportedTypeError.Error">¶</a></h3>
<pre>func (e *<a href="#UnsupportedTypeError">UnsupportedTypeError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="UnsupportedValueError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L223">UnsupportedValueError</a>
    <a href="#UnsupportedValueError">¶</a></h2>
<pre>type UnsupportedValueError struct {
<span id="UnsupportedValueError.Value"></span>    Value <a href="/reflect/">reflect</a>.<a href="/reflect/#Value">Value</a>
<span id="UnsupportedValueError.Str"></span>    Str   <a href="/builtin/#string">string</a>
}</pre>


<h3 id="UnsupportedValueError.Error">func (*UnsupportedValueError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/json/encode.go#L228">Error</a>
    <a href="#UnsupportedValueError.Error">¶</a></h3>
<pre>func (e *<a href="#UnsupportedValueError">UnsupportedValueError</a>) Error() <a href="/builtin/#string">string</a></pre>



