version: 1.10
## package html

  `import "html"`

## Overview

Package html provides functions for escaping and unescaping HTML text.

## Index

- [func EscapeString(s string) string](#EscapeString)
- [func UnescapeString(s string) string](#UnescapeString)

### Examples

- [EscapeString](#exampleEscapeString)
- [UnescapeString](#exampleUnescapeString)

### Package files
 [entity.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/html/entity.go) [escape.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/html/escape.go)

<h2 id="EscapeString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/html/escape.go#L168">EscapeString</a>
    <a href="#EscapeString">¶</a></h2>
<pre>func EscapeString(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

EscapeString escapes special characters like "<" to become "&lt;". It escapes
only five such characters: <, >, &, ' and ". UnescapeString(EscapeString(s)) ==
s always holds, but the converse isn't always true.

<a id="exampleEscapeString"></a>
Example:

    const s = `"Fran & Freddie's Diner" <tasty@example.com>`
    fmt.Println(html.EscapeString(s))
    // Output: &#34;Fran &amp; Freddie&#39;s Diner&#34; &lt;tasty@example.com&gt;

<h2 id="UnescapeString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/html/escape.go#L177">UnescapeString</a>
    <a href="#UnescapeString">¶</a></h2>
<pre>func UnescapeString(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

UnescapeString unescapes entities like "&lt;" to become "<". It unescapes a
larger range of entities than EscapeString escapes. For example, "&aacute;"
unescapes to "á", as does "&#225;" and "&#xE1;". UnescapeString(EscapeString(s))
== s always holds, but the converse isn't always true.

<a id="exampleUnescapeString"></a>
Example:

    const s = `&quot;Fran &amp; Freddie&#39;s Diner&quot; &lt;tasty@example.com&gt;`
    fmt.Println(html.UnescapeString(s))
    // Output: "Fran & Freddie's Diner" <tasty@example.com>

## Subdirectories
- [..](..)
- [template](template/)
