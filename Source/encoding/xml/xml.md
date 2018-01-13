version: 1.9.2
## package xml

  `import "encoding/xml"`

## Overview

Package xml implements a simple XML 1.0 parser that understands XML name spaces.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func Escape(w io.Writer, s []byte)](#Escape)
- [func EscapeText(w io.Writer, s []byte) error](#EscapeText)
- [func Marshal(v interface{}) ([]byte, error)](#Marshal)
- [func MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)](#MarshalIndent)
- [func Unmarshal(data []byte, v interface{}) error](#Unmarshal)
- [type Attr](#Attr)
- [type CharData](#CharData)
  - [func (c CharData) Copy() CharData](#CharData.Copy)
- [type Comment](#Comment)
  - [func (c Comment) Copy() Comment](#Comment.Copy)
- [type Decoder](#Decoder)
  - [func NewDecoder(r io.Reader) *Decoder](#NewDecoder)
  - [func (d *Decoder) Decode(v interface{}) error](#Decoder.Decode)
  - [func (d *Decoder) DecodeElement(v interface{}, start *StartElement) error](#Decoder.DecodeElement)
  - [func (d *Decoder) InputOffset() int64](#Decoder.InputOffset)
  - [func (d *Decoder) RawToken() (Token, error)](#Decoder.RawToken)
  - [func (d *Decoder) Skip() error](#Decoder.Skip)
  - [func (d *Decoder) Token() (Token, error)](#Decoder.Token)
- [type Directive](#Directive)
  - [func (d Directive) Copy() Directive](#Directive.Copy)
- [type Encoder](#Encoder)
  - [func NewEncoder(w io.Writer) *Encoder](#NewEncoder)
  - [func (enc *Encoder) Encode(v interface{}) error](#Encoder.Encode)
  - [func (enc *Encoder) EncodeElement(v interface{}, start StartElement) error](#Encoder.EncodeElement)
  - [func (enc *Encoder) EncodeToken(t Token) error](#Encoder.EncodeToken)
  - [func (enc *Encoder) Flush() error](#Encoder.Flush)
  - [func (enc *Encoder) Indent(prefix, indent string)](#Encoder.Indent)
- [type EndElement](#EndElement)
- [type Marshaler](#Marshaler)
- [type MarshalerAttr](#MarshalerAttr)
- [type Name](#Name)
- [type ProcInst](#ProcInst)
  - [func (p ProcInst) Copy() ProcInst](#ProcInst.Copy)
- [type StartElement](#StartElement)
  - [func (e StartElement) Copy() StartElement](#StartElement.Copy)
  - [func (e StartElement) End() EndElement](#StartElement.End)
- [type SyntaxError](#SyntaxError)
  - [func (e *SyntaxError) Error() string](#SyntaxError.Error)
- [type TagPathError](#TagPathError)
  - [func (e *TagPathError) Error() string](#TagPathError.Error)
- [type Token](#Token)
  - [func CopyToken(t Token) Token](#CopyToken)
- [type UnmarshalError](#UnmarshalError)
  - [func (e UnmarshalError) Error() string](#UnmarshalError.Error)
- [type Unmarshaler](#Unmarshaler)
- [type UnmarshalerAttr](#UnmarshalerAttr)
- [type UnsupportedTypeError](#UnsupportedTypeError)
  - [func (e *UnsupportedTypeError) Error() string](#UnsupportedTypeError.Error)
- [Bugs](#pkg-note-BUG)

### Examples

- [Encoder](#exampleEncoder)
- [MarshalIndent](#exampleMarshalIndent)
- [Unmarshal](#exampleUnmarshal)

### Package files
 [marshal.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go) [read.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/read.go) [typeinfo.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/typeinfo.go) [xml.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// A generic XML header suitable for use with the output of Marshal.</span>
    <span class="comment">// This is not automatically added to any output of this package,</span>
    <span class="comment">// it is provided as a convenience.</span>
    <span id="Header">Header</span> = `&lt;?xml version=&#34;1.0&#34; encoding=&#34;UTF-8&#34;?&gt;` + &#34;\n&#34;
)</pre>


<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="HTMLAutoClose">HTMLAutoClose</span> = htmlAutoClose</pre>

HTMLAutoClose is the set of HTML elements that should be considered to close
automatically.

<pre>var <span id="HTMLEntity">HTMLEntity</span> = htmlEntity</pre>

HTMLEntity is an entity map containing translations for the standard HTML entity
characters.

<h2 id="Escape">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L1939">Escape</a>
    <a href="#Escape">¶</a></h2>
<pre>func Escape(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, s []<a href="/builtin/#byte">byte</a>)</pre>

Escape is like EscapeText but omits the error return value. It is provided for
backwards compatibility with Go 1.0. Code targeting Go 1.1 or later should use
EscapeText.

<h2 id="EscapeText">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L1843">EscapeText</a>
    <a href="#EscapeText">¶</a></h2>
<pre>func EscapeText(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, s []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

EscapeText writes to w the properly escaped XML equivalent of the plain text
data s.

<h2 id="Marshal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L62">Marshal</a>
    <a href="#Marshal">¶</a></h2>
<pre>func Marshal(v interface{}) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Marshal returns the XML encoding of v.

Marshal handles an array or slice by marshaling each of the elements. Marshal
handles a pointer by marshaling the value it points at or, if the pointer is
nil, by writing nothing. Marshal handles an interface value by marshaling the
value it contains or, if the interface value is nil, by writing nothing. Marshal
handles all other data by writing one or more XML elements containing the data.

The name for the XML elements is taken from, in order of preference:

    - the tag on the XMLName field, if the data is a struct
    - the value of the XMLName field of type Name
    - the tag of the struct field used to obtain the data
    - the name of the struct field used to obtain the data
    - the name of the marshaled type

The XML element for a struct contains marshaled elements for each of the
exported fields of the struct, with these exceptions:

    - the XMLName field, described above, is omitted.
    - a field with tag "-" is omitted.
    - a field with tag "name,attr" becomes an attribute with
      the given name in the XML element.
    - a field with tag ",attr" becomes an attribute with the
      field name in the XML element.
    - a field with tag ",chardata" is written as character data,
      not as an XML element.
    - a field with tag ",cdata" is written as character data
      wrapped in one or more <![CDATA[ ... ]]> tags, not as an XML element.
    - a field with tag ",innerxml" is written verbatim, not subject
      to the usual marshaling procedure.
    - a field with tag ",comment" is written as an XML comment, not
      subject to the usual marshaling procedure. It must not contain
      the "--" string within it.
    - a field with a tag including the "omitempty" option is omitted
      if the field value is empty. The empty values are false, 0, any
      nil pointer or interface value, and any array, slice, map, or
      string of length zero.
    - an anonymous struct field is handled as if the fields of its
      value were part of the outer struct.

If a field uses a tag "a>b>c", then the element c will be nested inside parent
elements a and b. Fields that appear next to each other that name the same
parent will be enclosed in one XML element.

See MarshalIndent for an example.

Marshal will return an error if asked to marshal a channel, function, or map.

<h2 id="MarshalIndent">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L108">MarshalIndent</a>
    <a href="#MarshalIndent">¶</a></h2>
<pre>func MarshalIndent(v interface{}, prefix, indent <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

MarshalIndent works like Marshal, but each XML element begins on a new indented
line that starts with prefix and is followed by one or more copies of indent
according to the nesting depth.

<a id="exampleMarshalIndent"></a>
Example:

    type Address struct {
        City, State string
    }
    type Person struct {
        XMLName   xml.Name `xml:"person"`
        Id        int      `xml:"id,attr"`
        FirstName string   `xml:"name>first"`
        LastName  string   `xml:"name>last"`
        Age       int      `xml:"age"`
        Height    float32  `xml:"height,omitempty"`
        Married   bool
        Address
        Comment string `xml:",comment"`
    }

    v := &Person{Id: 13, FirstName: "John", LastName: "Doe", Age: 42}
    v.Comment = " Need more details. "
    v.Address = Address{"Hanga Roa", "Easter Island"}

    output, err := xml.MarshalIndent(v, "  ", "    ")
    if err != nil {
        fmt.Printf("error: %v\n", err)
    }

    os.Stdout.Write(output)
    // Output:
    //   <person id="13">
    //       <name>
    //           <first>John</first>
    //           <last>Doe</last>
    //       </name>
    //       <age>42</age>
    //       <Married>false</Married>
    //       <City>Hanga Roa</City>
    //       <State>Easter Island</State>
    //       <!-- Need more details. -->
    //   </person>

<h2 id="Unmarshal">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/read.go#L116">Unmarshal</a>
    <a href="#Unmarshal">¶</a></h2>
<pre>func Unmarshal(data []<a href="/builtin/#byte">byte</a>, v interface{}) <a href="/builtin/#error">error</a></pre>

Unmarshal parses the XML-encoded data and stores the result in the value pointed
to by v, which must be an arbitrary struct, slice, or string. Well-formed data
that does not fit into v is discarded.

Because Unmarshal uses the reflect package, it can only assign to exported
(upper case) fields. Unmarshal uses a case-sensitive comparison to match XML
element names to tag values and struct field names.

Unmarshal maps an XML element to a struct using the following rules. In the
rules, the tag of a field refers to the value associated with the key 'xml' in
the struct field's tag (see the example above).

    * If the struct has a field of type []byte or string with tag
       ",innerxml", Unmarshal accumulates the raw XML nested inside the
       element in that field. The rest of the rules still apply.

    * If the struct has a field named XMLName of type Name,
       Unmarshal records the element name in that field.

    * If the XMLName field has an associated tag of the form
       "name" or "namespace-URL name", the XML element must have
       the given name (and, optionally, name space) or else Unmarshal
       returns an error.

    * If the XML element has an attribute whose name matches a
       struct field name with an associated tag containing ",attr" or
       the explicit name in a struct field tag of the form "name,attr",
       Unmarshal records the attribute value in that field.

    * If the XML element has an attribute not handled by the previous
       rule and the struct has a field with an associated tag containing
       ",any,attr", Unmarshal records the attribute value in the first
       such field.

    * If the XML element contains character data, that data is
       accumulated in the first struct field that has tag ",chardata".
       The struct field may have type []byte or string.
       If there is no such field, the character data is discarded.

    * If the XML element contains comments, they are accumulated in
       the first struct field that has tag ",comment".  The struct
       field may have type []byte or string. If there is no such
       field, the comments are discarded.

    * If the XML element contains a sub-element whose name matches
       the prefix of a tag formatted as "a" or "a>b>c", unmarshal
       will descend into the XML structure looking for elements with the
       given names, and will map the innermost elements to that struct
       field. A tag starting with ">" is equivalent to one starting
       with the field name followed by ">".

    * If the XML element contains a sub-element whose name matches
       a struct field's XMLName tag and the struct field has no
       explicit name tag as per the previous rule, unmarshal maps
       the sub-element to that struct field.

    * If the XML element contains a sub-element whose name matches a
       field without any mode flags (",attr", ",chardata", etc), Unmarshal
       maps the sub-element to that struct field.

    * If the XML element contains a sub-element that hasn't matched any
       of the above rules and the struct has a field with tag ",any",
       unmarshal maps the sub-element to that struct field.

    * An anonymous struct field is handled as if the fields of its
       value were part of the outer struct.

    * A struct field with tag "-" is never unmarshaled into.

Unmarshal maps an XML element to a string or []byte by saving the concatenation
of that element's character data in the string or []byte. The saved []byte is
never nil.

Unmarshal maps an attribute value to a string or []byte by saving the value in
the string or slice.

Unmarshal maps an attribute value to an Attr by saving the attribute, including
its name, in the Attr.

Unmarshal maps an XML element or attribute value to a slice by extending the
length of the slice and mapping the element or attribute to the newly created
value.

Unmarshal maps an XML element or attribute value to a bool by setting it to the
boolean value represented by the string.

Unmarshal maps an XML element or attribute value to an integer or floating-point
field by setting the field to the result of interpreting the string value in
decimal. There is no check for overflow.

Unmarshal maps an XML element to a Name by recording the element name.

Unmarshal maps an XML element to a pointer by setting the pointer to a freshly
allocated value and then mapping the element to that value.

A missing element or empty attribute value will be unmarshaled as a zero value.
If the field is a slice, a zero value will be appended to the field. Otherwise,
the field will be set to its zero value.

<a id="exampleUnmarshal"></a>
Example:

    type Email struct {
        Where string `xml:"where,attr"`
        Addr  string
    }
    type Address struct {
        City, State string
    }
    type Result struct {
        XMLName xml.Name `xml:"Person"`
        Name    string   `xml:"FullName"`
        Phone   string
        Email   []Email
        Groups  []string `xml:"Group>Value"`
        Address
    }
    v := Result{Name: "none", Phone: "none"}

    data := `
    		<Person>
    			<FullName>Grace R. Emlin</FullName>
    			<Company>Example Inc.</Company>
    			<Email where="home">
    				<Addr>gre@example.com</Addr>
    			</Email>
    			<Email where='work'>
    				<Addr>gre@work.com</Addr>
    			</Email>
    			<Group>
    				<Value>Friends</Value>
    				<Value>Squash</Value>
    			</Group>
    			<City>Hanga Roa</City>
    			<State>Easter Island</State>
    		</Person>
    	`
    err := xml.Unmarshal([]byte(data), &v)
    if err != nil {
        fmt.Printf("error: %v", err)
        return
    }
    fmt.Printf("XMLName: %#v\n", v.XMLName)
    fmt.Printf("Name: %q\n", v.Name)
    fmt.Printf("Phone: %q\n", v.Phone)
    fmt.Printf("Email: %v\n", v.Email)
    fmt.Printf("Groups: %v\n", v.Groups)
    fmt.Printf("Address: %v\n", v.Address)
    // Output:
    // XMLName: xml.Name{Space:"", Local:"Person"}
    // Name: "Grace R. Emlin"
    // Phone: "none"
    // Email: [{home gre@example.com} {work gre@work.com}]
    // Groups: [Friends Squash]
    // Address: {Hanga Roa Easter Island}

<h2 id="Attr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L38">Attr</a>
    <a href="#Attr">¶</a></h2>
<pre>type Attr struct {
<span id="Attr.Name"></span>    Name  <a href="#Name">Name</a>
<span id="Attr.Value"></span>    Value <a href="/builtin/#string">string</a>
}</pre>

An Attr represents an attribute in an XML element (Name=Value).

<h2 id="CharData">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L73">CharData</a>
    <a href="#CharData">¶</a></h2>
<pre>type CharData []<a href="/builtin/#byte">byte</a></pre>

A CharData represents XML character data (raw text), in which XML escape
sequences have been replaced by the characters they represent.

<h3 id="CharData.Copy">func (CharData) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L81">Copy</a>
    <a href="#CharData.Copy">¶</a></h3>
<pre>func (c <a href="#CharData">CharData</a>) Copy() <a href="#CharData">CharData</a></pre>


<h2 id="Comment">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L85">Comment</a>
    <a href="#Comment">¶</a></h2>
<pre>type Comment []<a href="/builtin/#byte">byte</a></pre>

A Comment represents an XML comment of the form <!--comment-->. The bytes do not
include the <!-- and --> comment markers.

<h3 id="Comment.Copy">func (Comment) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L87">Copy</a>
    <a href="#Comment.Copy">¶</a></h3>
<pre>func (c <a href="#Comment">Comment</a>) Copy() <a href="#Comment">Comment</a></pre>


<h2 id="Decoder">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L125">Decoder</a>
    <a href="#Decoder">¶</a></h2>
<pre>type Decoder struct {
<span id="Decoder.Strict"></span>    <span class="comment">// Strict defaults to true, enforcing the requirements</span>
    <span class="comment">// of the XML specification.</span>
    <span class="comment">// If set to false, the parser allows input containing common</span>
    <span class="comment">// mistakes:</span>
    <span class="comment">//	* If an element is missing an end tag, the parser invents</span>
    <span class="comment">//	  end tags as necessary to keep the return values from Token</span>
    <span class="comment">//	  properly balanced.</span>
    <span class="comment">//	* In attribute values and character data, unknown or malformed</span>
    <span class="comment">//	  character entities (sequences beginning with &amp;) are left alone.</span>
    <span class="comment">//</span>
    <span class="comment">// Setting:</span>
    <span class="comment">//</span>
    <span class="comment">//	d.Strict = false;</span>
    <span class="comment">//	d.AutoClose = HTMLAutoClose;</span>
    <span class="comment">//	d.Entity = HTMLEntity</span>
    <span class="comment">//</span>
    <span class="comment">// creates a parser that can handle typical HTML.</span>
    <span class="comment">//</span>
    <span class="comment">// Strict mode does not enforce the requirements of the XML name spaces TR.</span>
    <span class="comment">// In particular it does not reject name space tags using undefined prefixes.</span>
    <span class="comment">// Such tags are recorded with the unknown prefix as the name space URL.</span>
    Strict <a href="/builtin/#bool">bool</a>

    <span class="comment">// When Strict == false, AutoClose indicates a set of elements to</span>
    <span class="comment">// consider closed immediately after they are opened, regardless</span>
    <span class="comment">// of whether an end element is present.</span>
<span id="Decoder.AutoClose"></span>    AutoClose []<a href="/builtin/#string">string</a>

<span id="Decoder.Entity"></span>    <span class="comment">// Entity can be used to map non-standard entity names to string replacements.</span>
    <span class="comment">// The parser behaves as if these standard mappings are present in the map,</span>
    <span class="comment">// regardless of the actual map content:</span>
    <span class="comment">//</span>
    <span class="comment">//	&#34;lt&#34;: &#34;&lt;&#34;,</span>
    <span class="comment">//	&#34;gt&#34;: &#34;&gt;&#34;,</span>
    <span class="comment">//	&#34;amp&#34;: &#34;&amp;&#34;,</span>
    <span class="comment">//	&#34;apos&#34;: &#34;&#39;&#34;,</span>
    <span class="comment">//	&#34;quot&#34;: `&#34;`,</span>
    Entity map[<a href="/builtin/#string">string</a>]<a href="/builtin/#string">string</a>

<span id="Decoder.CharsetReader"></span>    <span class="comment">// CharsetReader, if non-nil, defines a function to generate</span>
    <span class="comment">// charset-conversion readers, converting from the provided</span>
    <span class="comment">// non-UTF-8 charset into UTF-8. If CharsetReader is nil or</span>
    <span class="comment">// returns an error, parsing stops with an error. One of the</span>
    <span class="comment">// the CharsetReader&#39;s result values must be non-nil.</span>
    CharsetReader func(charset <a href="/builtin/#string">string</a>, input <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, <a href="/builtin/#error">error</a>)

<span id="Decoder.DefaultSpace"></span>    <span class="comment">// DefaultSpace sets the default name space used for unadorned tags,</span>
    <span class="comment">// as if the entire XML stream were wrapped in an element containing</span>
    <span class="comment">// the attribute xmlns=&#34;DefaultSpace&#34;.</span>
    DefaultSpace <a href="/builtin/#string">string</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Decoder represents an XML parser reading a particular input stream. The parser
assumes that its input is encoded in UTF-8.

<h3 id="NewDecoder">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L196">NewDecoder</a>
    <a href="#NewDecoder">¶</a></h3>
<pre>func NewDecoder(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) *<a href="#Decoder">Decoder</a></pre>

NewDecoder creates a new XML parser reading from r. If r does not implement
io.ByteReader, NewDecoder will do its own buffering.

<h3 id="Decoder.Decode">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/read.go#L122">Decode</a>
    <a href="#Decoder.Decode">¶</a></h3>
<pre>func (d *<a href="#Decoder">Decoder</a>) Decode(v interface{}) <a href="/builtin/#error">error</a></pre>

Decode works like Unmarshal, except it reads the decoder stream to find the
start element.

<h3 id="Decoder.DecodeElement">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/read.go#L130">DecodeElement</a>
    <a href="#Decoder.DecodeElement">¶</a></h3>
<pre>func (d *<a href="#Decoder">Decoder</a>) DecodeElement(v interface{}, start *<a href="#StartElement">StartElement</a>) <a href="/builtin/#error">error</a></pre>

DecodeElement works like Unmarshal except that it takes a pointer to the start
XML element to decode into v. It is useful when a client reads some raw XML
tokens itself but also wants to defer to Unmarshal for some elements.

<h3 id="Decoder.InputOffset">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L879">InputOffset</a>
    <a href="#Decoder.InputOffset">¶</a></h3>
<pre>func (d *<a href="#Decoder">Decoder</a>) InputOffset() <a href="/builtin/#int64">int64</a></pre>

InputOffset returns the input stream byte offset of the current decoder
position. The offset gives the location of the end of the most recently returned
token and the beginning of the next token.

<h3 id="Decoder.RawToken">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L488">RawToken</a>
    <a href="#Decoder.RawToken">¶</a></h3>
<pre>func (d *<a href="#Decoder">Decoder</a>) RawToken() (<a href="#Token">Token</a>, <a href="/builtin/#error">error</a>)</pre>

RawToken is like Token but does not verify that start and end elements match and
does not translate name space prefixes to their corresponding URLs.

<h3 id="Decoder.Skip">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/read.go#L724">Skip</a>
    <a href="#Decoder.Skip">¶</a></h3>
<pre>func (d *<a href="#Decoder">Decoder</a>) Skip() <a href="/builtin/#error">error</a></pre>

Skip reads tokens until it has consumed the end element matching the most recent
start element already consumed. It recurs if it encounters a start element, so
it can be used to skip nested structures. It returns nil if it finds an end
element matching the start element; otherwise it returns an error describing the
problem.

<h3 id="Decoder.Token">func (*Decoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L230">Token</a>
    <a href="#Decoder.Token">¶</a></h3>
<pre>func (d *<a href="#Decoder">Decoder</a>) Token() (<a href="#Token">Token</a>, <a href="/builtin/#error">error</a>)</pre>

Token returns the next XML token in the input stream. At the end of the input
stream, Token returns nil, io.EOF.

Slices of bytes in the returned token data refer to the parser's internal buffer
and remain valid only until the next call to Token. To acquire a copy of the
bytes, call CopyToken or the token's Copy method.

Token expands self-closing elements such as <br/> into separate start and end
elements returned by successive calls.

Token guarantees that the StartElement and EndElement tokens it returns are
properly nested and matched: if Token encounters an unexpected end element or
EOF before all expected end elements, it will return an error.

Token implements XML name spaces as described by
http://www.w3.org/TR/REC-xml-names/. Each of the Name structures contained in
the Token has the Space set to the URL identifying its name space when known. If
Token encounters an unrecognized name space prefix, it uses the prefix as the
Space rather than report an error.

<h2 id="Directive">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L102">Directive</a>
    <a href="#Directive">¶</a></h2>
<pre>type Directive []<a href="/builtin/#byte">byte</a></pre>

A Directive represents an XML directive of the form <!text>. The bytes do not
include the <! and > markers.

<h3 id="Directive.Copy">func (Directive) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L104">Copy</a>
    <a href="#Directive.Copy">¶</a></h3>
<pre>func (d <a href="#Directive">Directive</a>) Copy() <a href="#Directive">Directive</a></pre>


<h2 id="Encoder">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L119">Encoder</a>
    <a href="#Encoder">¶</a></h2>
<pre>type Encoder struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

An Encoder writes XML data to an output stream.

<a id="exampleEncoder"></a>
Example:

    type Address struct {
        City, State string
    }
    type Person struct {
        XMLName   xml.Name `xml:"person"`
        Id        int      `xml:"id,attr"`
        FirstName string   `xml:"name>first"`
        LastName  string   `xml:"name>last"`
        Age       int      `xml:"age"`
        Height    float32  `xml:"height,omitempty"`
        Married   bool
        Address
        Comment string `xml:",comment"`
    }

    v := &Person{Id: 13, FirstName: "John", LastName: "Doe", Age: 42}
    v.Comment = " Need more details. "
    v.Address = Address{"Hanga Roa", "Easter Island"}

    enc := xml.NewEncoder(os.Stdout)
    enc.Indent("  ", "    ")
    if err := enc.Encode(v); err != nil {
        fmt.Printf("error: %v\n", err)
    }

    // Output:
    //   <person id="13">
    //       <name>
    //           <first>John</first>
    //           <last>Doe</last>
    //       </name>
    //       <age>42</age>
    //       <Married>false</Married>
    //       <City>Hanga Roa</City>
    //       <State>Easter Island</State>
    //       <!-- Need more details. -->
    //   </person>

<h3 id="NewEncoder">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L124">NewEncoder</a>
    <a href="#NewEncoder">¶</a></h3>
<pre>func NewEncoder(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) *<a href="#Encoder">Encoder</a></pre>

NewEncoder returns a new encoder that writes to w.

<h3 id="Encoder.Encode">func (*Encoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L144">Encode</a>
    <a href="#Encoder.Encode">¶</a></h3>
<pre>func (enc *<a href="#Encoder">Encoder</a>) Encode(v interface{}) <a href="/builtin/#error">error</a></pre>

Encode writes the XML encoding of v to the stream.

See the documentation for Marshal for details about the conversion of Go values
to XML.

Encode calls Flush before returning.

<h3 id="Encoder.EncodeElement">func (*Encoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L159">EncodeElement</a>
    <a href="#Encoder.EncodeElement">¶</a></h3>
<pre>func (enc *<a href="#Encoder">Encoder</a>) EncodeElement(v interface{}, start <a href="#StartElement">StartElement</a>) <a href="/builtin/#error">error</a></pre>

EncodeElement writes the XML encoding of v to the stream, using start as the
outermost tag in the encoding.

See the documentation for Marshal for details about the conversion of Go values
to XML.

EncodeElement calls Flush before returning.

<h3 id="Encoder.EncodeToken">func (*Encoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L185">EncodeToken</a>
    <a href="#Encoder.EncodeToken">¶</a></h3>
<pre>func (enc *<a href="#Encoder">Encoder</a>) EncodeToken(t <a href="#Token">Token</a>) <a href="/builtin/#error">error</a></pre>

EncodeToken writes the given XML token to the stream. It returns an error if
StartElement and EndElement tokens are not properly matched.

EncodeToken does not call Flush, because usually it is part of a larger
operation such as Encode or EncodeElement (or a custom Marshaler's MarshalXML
invoked during those), and those will call Flush when finished. Callers that
create an Encoder and then invoke EncodeToken directly, without using Encode or
EncodeElement, need to call Flush when finished to ensure that the XML is
written to the underlying writer.

EncodeToken allows writing a ProcInst with Target set to "xml" only as the first
token in the stream.

<h3 id="Encoder.Flush">func (*Encoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L282">Flush</a>
    <a href="#Encoder.Flush">¶</a></h3>
<pre>func (enc *<a href="#Encoder">Encoder</a>) Flush() <a href="/builtin/#error">error</a></pre>

Flush flushes any buffered XML to the underlying writer. See the EncodeToken
documentation for details about when it is necessary.

<h3 id="Encoder.Indent">func (*Encoder) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L133">Indent</a>
    <a href="#Encoder.Indent">¶</a></h3>
<pre>func (enc *<a href="#Encoder">Encoder</a>) Indent(prefix, indent <a href="/builtin/#string">string</a>)</pre>

Indent sets the encoder to generate XML in which each element begins on a new
indented line that starts with prefix and is followed by one or more copies of
indent according to the nesting depth.

<h2 id="EndElement">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L66">EndElement</a>
    <a href="#EndElement">¶</a></h2>
<pre>type EndElement struct {
<span id="EndElement.Name"></span>    Name <a href="#Name">Name</a>
}</pre>

An EndElement represents an XML end element.

<h2 id="Marshaler">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L86">Marshaler</a>
    <a href="#Marshaler">¶</a></h2>
<pre>type Marshaler interface {
    MarshalXML(e *<a href="#Encoder">Encoder</a>, start <a href="#StartElement">StartElement</a>) <a href="/builtin/#error">error</a>
}</pre>

Marshaler is the interface implemented by objects that can marshal themselves
into valid XML elements.

MarshalXML encodes the receiver as zero or more XML elements. By convention,
arrays or slices are typically encoded as a sequence of elements, one per entry.
Using start as the element tag is not required, but doing so will enable
Unmarshal to match the XML elements to the correct struct field. One common
implementation strategy is to construct a separate value with a layout
corresponding to the desired XML and then to encode it using e.EncodeElement.
Another common strategy is to use repeated calls to e.EncodeToken to generate
the XML output one token at a time. The sequence of encoded tokens must make up
zero or more valid XML elements.

<h2 id="MarshalerAttr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L101">MarshalerAttr</a>
    <a href="#MarshalerAttr">¶</a></h2>
<pre>type MarshalerAttr interface {
    MarshalXMLAttr(name <a href="#Name">Name</a>) (<a href="#Attr">Attr</a>, <a href="/builtin/#error">error</a>)
}</pre>

MarshalerAttr is the interface implemented by objects that can marshal
themselves into valid XML attributes.

MarshalXMLAttr returns an XML attribute with the encoded value of the receiver.
Using name as the attribute name is not required, but doing so will enable
Unmarshal to match the attribute to the correct struct field. If MarshalXMLAttr
returns the zero attribute Attr{}, no attribute will be generated in the output.
MarshalXMLAttr is used only for struct fields with the "attr" option in the
field tag.

<h2 id="Name">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L33">Name</a>
    <a href="#Name">¶</a></h2>
<pre>type Name struct {
<span id="Name.Space"></span>    Space, Local <a href="/builtin/#string">string</a>
}</pre>

A Name represents an XML name (Local) annotated with a name space identifier
(Space). In tokens returned by Decoder.Token, the Space identifier is given as a
canonical URL, not the short prefix used in the document being parsed.

<h2 id="ProcInst">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L90">ProcInst</a>
    <a href="#ProcInst">¶</a></h2>
<pre>type ProcInst struct {
<span id="ProcInst.Target"></span>    Target <a href="/builtin/#string">string</a>
<span id="ProcInst.Inst"></span>    Inst   []<a href="/builtin/#byte">byte</a>
}</pre>

A ProcInst represents an XML processing instruction of the form <?target inst?>

<h3 id="ProcInst.Copy">func (ProcInst) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L95">Copy</a>
    <a href="#ProcInst.Copy">¶</a></h3>
<pre>func (p <a href="#ProcInst">ProcInst</a>) Copy() <a href="#ProcInst">ProcInst</a></pre>


<h2 id="StartElement">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L48">StartElement</a>
    <a href="#StartElement">¶</a></h2>
<pre>type StartElement struct {
<span id="StartElement.Name"></span>    Name <a href="#Name">Name</a>
<span id="StartElement.Attr"></span>    Attr []<a href="#Attr">Attr</a>
}</pre>

A StartElement represents an XML start element.

<h3 id="StartElement.Copy">func (StartElement) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L53">Copy</a>
    <a href="#StartElement.Copy">¶</a></h3>
<pre>func (e <a href="#StartElement">StartElement</a>) Copy() <a href="#StartElement">StartElement</a></pre>


<h3 id="StartElement.End">func (StartElement) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L61">End</a>
    <a href="#StartElement.End">¶</a></h3>
<pre>func (e <a href="#StartElement">StartElement</a>) End() <a href="#EndElement">EndElement</a></pre>

End returns the corresponding XML end element.

<h2 id="SyntaxError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L19">SyntaxError</a>
    <a href="#SyntaxError">¶</a></h2>
<pre>type SyntaxError struct {
<span id="SyntaxError.Msg"></span>    Msg  <a href="/builtin/#string">string</a>
<span id="SyntaxError.Line"></span>    Line <a href="/builtin/#int">int</a>
}</pre>

A SyntaxError represents a syntax error in the XML input stream.

<h3 id="SyntaxError.Error">func (*SyntaxError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L24">Error</a>
    <a href="#SyntaxError.Error">¶</a></h3>
<pre>func (e *<a href="#SyntaxError">SyntaxError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="TagPathError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/typeinfo.go#L325">TagPathError</a>
    <a href="#TagPathError">¶</a></h2>
<pre>type TagPathError struct {
<span id="TagPathError.Struct"></span>    Struct       <a href="/reflect/">reflect</a>.<a href="/reflect/#Type">Type</a>
<span id="TagPathError.Field1"></span>    Field1, Tag1 <a href="/builtin/#string">string</a>
<span id="TagPathError.Field2"></span>    Field2, Tag2 <a href="/builtin/#string">string</a>
}</pre>

A TagPathError represents an error in the unmarshaling process caused by the use
of field tags with conflicting paths.

<h3 id="TagPathError.Error">func (*TagPathError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/typeinfo.go#L331">Error</a>
    <a href="#TagPathError.Error">¶</a></h3>
<pre>func (e *<a href="#TagPathError">TagPathError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Token">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L45">Token</a>
    <a href="#Token">¶</a></h2>
<pre>type Token interface{}</pre>

A Token is an interface holding one of the token types: StartElement,
EndElement, CharData, Comment, ProcInst, or Directive.

<h3 id="CopyToken">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/xml.go#L107">CopyToken</a>
    <a href="#CopyToken">¶</a></h3>
<pre>func CopyToken(t <a href="#Token">Token</a>) <a href="#Token">Token</a></pre>

CopyToken returns a copy of a Token.

<h2 id="UnmarshalError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/read.go#L139">UnmarshalError</a>
    <a href="#UnmarshalError">¶</a></h2>
<pre>type UnmarshalError <a href="/builtin/#string">string</a></pre>

An UnmarshalError represents an error in the unmarshaling process.

<h3 id="UnmarshalError.Error">func (UnmarshalError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/read.go#L141">Error</a>
    <a href="#UnmarshalError.Error">¶</a></h3>
<pre>func (e <a href="#UnmarshalError">UnmarshalError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Unmarshaler">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/read.go#L158">Unmarshaler</a>
    <a href="#Unmarshaler">¶</a></h2>
<pre>type Unmarshaler interface {
    UnmarshalXML(d *<a href="#Decoder">Decoder</a>, start <a href="#StartElement">StartElement</a>) <a href="/builtin/#error">error</a>
}</pre>

Unmarshaler is the interface implemented by objects that can unmarshal an XML
element description of themselves.

UnmarshalXML decodes a single XML element beginning with the given start
element. If it returns an error, the outer call to Unmarshal stops and returns
that error. UnmarshalXML must consume exactly one XML element. One common
implementation strategy is to unmarshal into a separate value with a layout
matching the expected XML using d.DecodeElement, and then to copy the data from
that value into the receiver. Another common strategy is to use d.Token to
process the XML object one token at a time. UnmarshalXML may not use d.RawToken.

<h2 id="UnmarshalerAttr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/read.go#L170">UnmarshalerAttr</a>
    <a href="#UnmarshalerAttr">¶</a></h2>
<pre>type UnmarshalerAttr interface {
    UnmarshalXMLAttr(attr <a href="#Attr">Attr</a>) <a href="/builtin/#error">error</a>
}</pre>

UnmarshalerAttr is the interface implemented by objects that can unmarshal an
XML attribute description of themselves.

UnmarshalXMLAttr decodes a single XML attribute. If it returns an error, the
outer call to Unmarshal stops and returns that error. UnmarshalXMLAttr is used
only for struct fields with the "attr" option in the field tag.

<h2 id="UnsupportedTypeError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L1006">UnsupportedTypeError</a>
    <a href="#UnsupportedTypeError">¶</a></h2>
<pre>type UnsupportedTypeError struct {
<span id="UnsupportedTypeError.Type"></span>    Type <a href="/reflect/">reflect</a>.<a href="/reflect/#Type">Type</a>
}</pre>

A MarshalXMLError is returned when Marshal encounters a type that cannot be
converted into XML.

<h3 id="UnsupportedTypeError.Error">func (*UnsupportedTypeError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/marshal.go#L1010">Error</a>
    <a href="#UnsupportedTypeError.Error">¶</a></h3>
<pre>func (e *<a href="#UnsupportedTypeError">UnsupportedTypeError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/encoding/xml/read.go#L7)  Mapping between XML elements and data structures is inherently flawed: an
  XML element is an order-dependent collection of anonymous values, while a
  data structure is an order-independent collection of named values. See
  package json for a textual representation more suitable to data structures.


