version: 1.10
## package mime

  `import "mime"`

## Overview

Package mime implements parts of the MIME spec.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func AddExtensionType(ext, typ string) error](#AddExtensionType)
- [func ExtensionsByType(typ string) ([]string, error)](#ExtensionsByType)
- [func FormatMediaType(t string, param map[string]string) string](#FormatMediaType)
- [func ParseMediaType(v string) (mediatype string, params map[string]string, err error)](#ParseMediaType)
- [func TypeByExtension(ext string) string](#TypeByExtension)
- [type WordDecoder](#WordDecoder)
  - [func (d *WordDecoder) Decode(word string) (string, error)](#WordDecoder.Decode)
  - [func (d *WordDecoder) DecodeHeader(header string) (string, error)](#WordDecoder.DecodeHeader)
- [type WordEncoder](#WordEncoder)
  - [func (e WordEncoder) Encode(charset, s string) string](#WordEncoder.Encode)

### Examples

- [WordDecoder.Decode](#exampleWordDecoder_Decode)
- [WordDecoder.DecodeHeader](#exampleWordDecoder_DecodeHeader)
- [WordEncoder.Encode](#exampleWordEncoder_Encode)

### Package files
 [encodedword.go](//github.com/golang/go/blob/release-branch.go1.10/src/mime/encodedword.go) [grammar.go](//github.com/golang/go/blob/release-branch.go1.10/src/mime/grammar.go) [mediatype.go](//github.com/golang/go/blob/release-branch.go1.10/src/mime/mediatype.go) [type.go](//github.com/golang/go/blob/release-branch.go1.10/src/mime/type.go) [type_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/mime/type_unix.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// BEncoding represents Base64 encoding scheme as defined by RFC 2045.</span>
    <span id="BEncoding">BEncoding</span> = <a href="#WordEncoder">WordEncoder</a>(&#39;b&#39;)
    <span class="comment">// QEncoding represents the Q-encoding scheme as defined by RFC 2047.</span>
    <span id="QEncoding">QEncoding</span> = <a href="#WordEncoder">WordEncoder</a>(&#39;q&#39;)
)</pre>


<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrInvalidMediaParameter">ErrInvalidMediaParameter</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;mime: invalid media parameter&#34;)</pre>

ErrInvalidMediaParameter is returned by ParseMediaType if the media type value
was found but there was an error parsing the optional parameters

<h2 id="AddExtensionType">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/mime/type.go#L146">AddExtensionType</a>
    <a href="#AddExtensionType">¶</a></h2>
<pre>func AddExtensionType(ext, typ <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

AddExtensionType sets the MIME type associated with the extension ext to typ.
The extension should begin with a leading dot, as in ".html".

<h2 id="ExtensionsByType">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/mime/type.go#L129">ExtensionsByType</a>
    <a href="#ExtensionsByType">¶</a></h2>
<pre>func ExtensionsByType(typ <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ExtensionsByType returns the extensions known to be associated with the MIME
type typ. The returned extensions will each begin with a leading dot, as in
".html". When typ has no associated extensions, ExtensionsByType returns an nil
slice.

<h2 id="FormatMediaType">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/mime/mediatype.go#L11">FormatMediaType</a>
    <a href="#FormatMediaType">¶</a></h2>
<pre>func FormatMediaType(t <a href="/builtin/#string">string</a>, param map[<a href="/builtin/#string">string</a>]<a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

FormatMediaType serializes mediatype t and the parameters param as a media type
conforming to RFC 2045 and RFC 2616. The type and parameter names are written in
lower-case. When any of the arguments result in a standard violation then
FormatMediaType returns the empty string.

<h2 id="ParseMediaType">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/mime/mediatype.go#L102">ParseMediaType</a>
    <a href="#ParseMediaType">¶</a></h2>
<pre>func ParseMediaType(v <a href="/builtin/#string">string</a>) (mediatype <a href="/builtin/#string">string</a>, params map[<a href="/builtin/#string">string</a>]<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

ParseMediaType parses a media type value and any optional parameters, per RFC
1521. Media types are the values in Content-Type and Content-Disposition headers
(RFC 2183). On success, ParseMediaType returns the media type converted to
lowercase and trimmed of white space and a non-nil map. If there is an error
parsing the optional parameter, the media type will be returned along with the
error ErrInvalidMediaParameter. The returned map, params, maps from the
lowercase attribute to the attribute value with its case preserved.

<h2 id="TypeByExtension">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/mime/type.go#L92">TypeByExtension</a>
    <a href="#TypeByExtension">¶</a></h2>
<pre>func TypeByExtension(ext <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

TypeByExtension returns the MIME type associated with the file extension ext.
The extension ext should begin with a leading dot, as in ".html". When ext has
no associated type, TypeByExtension returns "".

Extensions are looked up first case-sensitively, then case-insensitively.

The built-in table is small but on unix it is augmented by the local system's
mime.types file(s) if available under one or more of these names:

    /etc/mime.types
    /etc/apache2/mime.types
    /etc/apache/mime.types

On Windows, MIME types are extracted from the registry.

Text types have the charset parameter set to "utf-8" by default.

<h2 id="WordDecoder">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/mime/encodedword.go#L175">WordDecoder</a>
    <a href="#WordDecoder">¶</a></h2>
<pre>type WordDecoder struct {
<span id="WordDecoder.CharsetReader"></span>    <span class="comment">// CharsetReader, if non-nil, defines a function to generate</span>
    <span class="comment">// charset-conversion readers, converting from the provided</span>
    <span class="comment">// charset into UTF-8.</span>
    <span class="comment">// Charsets are always lower-case. utf-8, iso-8859-1 and us-ascii charsets</span>
    <span class="comment">// are handled by default.</span>
    <span class="comment">// One of the CharsetReader&#39;s result values must be non-nil.</span>
    CharsetReader func(charset <a href="/builtin/#string">string</a>, input <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, <a href="/builtin/#error">error</a>)
}</pre>

A WordDecoder decodes MIME headers containing RFC 2047 encoded-words.

<h3 id="WordDecoder.Decode">func (*WordDecoder) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/mime/encodedword.go#L186">Decode</a>
    <a href="#WordDecoder.Decode">¶</a></h3>
<pre>func (d *<a href="#WordDecoder">WordDecoder</a>) Decode(word <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

Decode decodes an RFC 2047 encoded-word.

<a id="exampleWordDecoder_Decode"></a>
Example:

    dec := new(mime.WordDecoder)
    header, err := dec.Decode("=?utf-8?q?=C2=A1Hola,_se=C3=B1or!?=")
    if err != nil {
        panic(err)
    }
    fmt.Println(header)

    dec.CharsetReader = func(charset string, input io.Reader) (io.Reader, error) {
        switch charset {
        case "x-case":
            // Fake character set for example.
            // Real use would integrate with packages such
            // as code.google.com/p/go-charset
            content, err := ioutil.ReadAll(input)
            if err != nil {
                return nil, err
            }
            return bytes.NewReader(bytes.ToUpper(content)), nil
        default:
            return nil, fmt.Errorf("unhandled charset %q", charset)
        }
    }
    header, err = dec.Decode("=?x-case?q?hello!?=")
    if err != nil {
        panic(err)
    }
    fmt.Println(header)
    // Output:
    // ¡Hola, señor!
    // HELLO!

<h3 id="WordDecoder.DecodeHeader">func (*WordDecoder) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/mime/encodedword.go#L229">DecodeHeader</a>
    <a href="#WordDecoder.DecodeHeader">¶</a></h3>
<pre>func (d *<a href="#WordDecoder">WordDecoder</a>) DecodeHeader(header <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

DecodeHeader decodes all encoded-words of the given string. It returns an error
if and only if CharsetReader of d returns an error.

<a id="exampleWordDecoder_DecodeHeader"></a>
Example:

    dec := new(mime.WordDecoder)
    header, err := dec.DecodeHeader("=?utf-8?q?=C3=89ric?= <eric@example.org>, =?utf-8?q?Ana=C3=AFs?= <anais@example.org>")
    if err != nil {
        panic(err)
    }
    fmt.Println(header)

    header, err = dec.DecodeHeader("=?utf-8?q?=C2=A1Hola,?= =?utf-8?q?_se=C3=B1or!?=")
    if err != nil {
        panic(err)
    }
    fmt.Println(header)

    dec.CharsetReader = func(charset string, input io.Reader) (io.Reader, error) {
        switch charset {
        case "x-case":
            // Fake character set for example.
            // Real use would integrate with packages such
            // as code.google.com/p/go-charset
            content, err := ioutil.ReadAll(input)
            if err != nil {
                return nil, err
            }
            return bytes.NewReader(bytes.ToUpper(content)), nil
        default:
            return nil, fmt.Errorf("unhandled charset %q", charset)
        }
    }
    header, err = dec.DecodeHeader("=?x-case?q?hello_?= =?x-case?q?world!?=")
    if err != nil {
        panic(err)
    }
    fmt.Println(header)
    // Output:
    // Éric <eric@example.org>, Anaïs <anais@example.org>
    // ¡Hola, señor!
    // HELLO WORLD!

<h2 id="WordEncoder">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/mime/encodedword.go#L10">WordEncoder</a>
    <a href="#WordEncoder">¶</a></h2>
<pre>type WordEncoder <a href="/builtin/#byte">byte</a></pre>

A WordEncoder is an RFC 2047 encoded-word encoder.

<h3 id="WordEncoder.Encode">func (WordEncoder) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/mime/encodedword.go#L26">Encode</a>
    <a href="#WordEncoder.Encode">¶</a></h3>
<pre>func (e <a href="#WordEncoder">WordEncoder</a>) Encode(charset, s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Encode returns the encoded-word form of s. If s is ASCII without special
characters, it is returned unchanged. The provided charset is the IANA charset
name of s. It is case insensitive.

<a id="exampleWordEncoder_Encode"></a>
Example:

    fmt.Println(mime.QEncoding.Encode("utf-8", "¡Hola, señor!"))
    fmt.Println(mime.QEncoding.Encode("utf-8", "Hello!"))
    fmt.Println(mime.BEncoding.Encode("UTF-8", "¡Hola, señor!"))
    fmt.Println(mime.QEncoding.Encode("ISO-8859-1", "Caf\xE9"))
    // Output:
    // =?utf-8?q?=C2=A1Hola,_se=C3=B1or!?=
    // Hello!
    // =?UTF-8?b?wqFIb2xhLCBzZcOxb3Ih?=
    // =?ISO-8859-1?q?Caf=E9?=

## Subdirectories
- [..](..)
- [multipart](multipart/)
- [quotedprintable](quotedprintable/)
