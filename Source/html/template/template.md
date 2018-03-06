version: 1.10
## package template

  `import "html/template"`

## Overview

Package template (html/template) implements data-driven templates for generating
HTML output safe against code injection. It provides the same interface as
package text/template and should be used instead of text/template whenever the
output is HTML.

The documentation here focuses on the security features of the package. For
information about how to program the templates themselves, see the documentation
for text/template.


Introduction

This package wraps package text/template so you can share its template API to
parse and execute HTML templates safely.

    tmpl, err := template.New("name").Parse(...)
    // Error checking elided
    err = tmpl.Execute(out, data)

If successful, tmpl will now be injection-safe. Otherwise, err is an error
defined in the docs for ErrorCode.

HTML templates treat data values as plain text which should be encoded so they
can be safely embedded in an HTML document. The escaping is contextual, so
actions can appear within JavaScript, CSS, and URI contexts.

The security model used by this package assumes that template authors are
trusted, while Execute's data parameter is not. More details are provided below.

Example

    import "text/template"
    ...
    t, err := template.New("foo").Parse(`{{define "T"}}Hello, {{.}}!{{end}}`)
    err = t.ExecuteTemplate(out, "T", "<script>alert('you have been pwned')</script>")

produces

    Hello, <script>alert('you have been pwned')</script>!

but the contextual autoescaping in html/template

    import "html/template"
    ...
    t, err := template.New("foo").Parse(`{{define "T"}}Hello, {{.}}!{{end}}`)
    err = t.ExecuteTemplate(out, "T", "<script>alert('you have been pwned')</script>")

produces safe, escaped HTML output

    Hello, &lt;script&gt;alert(&#39;you have been pwned&#39;)&lt;/script&gt;!


Contexts

This package understands HTML, CSS, JavaScript, and URIs. It adds sanitizing
functions to each simple action pipeline, so given the excerpt

    <a href="/search?q={{.}}">{{.}}</a>

At parse time each {{.}} is overwritten to add escaping functions as necessary.
In this case it becomes

    <a href="/search?q={{. | urlescaper | attrescaper}}">{{. | htmlescaper}}</a>

where urlescaper, attrescaper, and htmlescaper are aliases for internal escaping
functions.


Errors

See the documentation of ErrorCode for details.


A fuller picture

The rest of this package comment may be skipped on first reading; it includes
details necessary to understand escaping contexts and error messages. Most users
will not need to understand these details.


Contexts

Assuming {{.}} is `O'Reilly: How are <i>you</i>?`, the table below shows how
{{.}} appears when used in the context to the left.

    Context                          {{.}} After
    {{.}}                            O'Reilly: How are &lt;i&gt;you&lt;/i&gt;?
    <a title='{{.}}'>                O&#39;Reilly: How are you?
    <a href="/{{.}}">                O&#39;Reilly: How are %3ci%3eyou%3c/i%3e?
    <a href="?q={{.}}">              O&#39;Reilly%3a%20How%20are%3ci%3e...%3f
    <a onx='f("{{.}}")'>             O\x27Reilly: How are \x3ci\x3eyou...?
    <a onx='f({{.}})'>               "O\x27Reilly: How are \x3ci\x3eyou...?"
    <a onx='pattern = /{{.}}/;'>     O\x27Reilly: How are \x3ci\x3eyou...\x3f

If used in an unsafe context, then the value might be filtered out:

    Context                          {{.}} After
    <a href="{{.}}">                 #ZgotmplZ

since "O'Reilly:" is not an allowed protocol like "http:".

If {{.}} is the innocuous word, `left`, then it can appear more widely,

    Context                              {{.}} After
    {{.}}                                left
    <a title='{{.}}'>                    left
    <a href='{{.}}'>                     left
    <a href='/{{.}}'>                    left
    <a href='?dir={{.}}'>                left
    <a style="border-{{.}}: 4px">        left
    <a style="align: {{.}}">             left
    <a style="background: '{{.}}'>       left
    <a style="background: url('{{.}}')>  left
    <style>p.{{.}} {color:red}</style>   left

Non-string values can be used in JavaScript contexts. If {{.}} is

    struct{A,B string}{ "foo", "bar" }

in the escaped template

    <script>var pair = {{.}};</script>

then the template output is

    <script>var pair = {"A": "foo", "B": "bar"};</script>

See package json to understand how non-string content is marshaled for embedding
in JavaScript contexts.


Typed Strings

By default, this package assumes that all pipelines produce a plain text string.
It adds escaping pipeline stages necessary to correctly and safely embed that
plain text string in the appropriate context.

When a data value is not plain text, you can make sure it is not over-escaped by
marking it with its type.

Types HTML, JS, URL, and others from content.go can carry safe content that is
exempted from escaping.

The template

    Hello, {{.}}!

can be invoked with

    tmpl.Execute(out, template.HTML(`<b>World</b>`))

to produce

    Hello, <b>World</b>!

instead of the

    Hello, &lt;b&gt;World&lt;b&gt;!

that would have been produced if {{.}} was a regular string.


Security Model

https://rawgit.com/mikesamuel/sanitized-jquery-templates/trunk/safetemplate.html#problem_definition
defines "safe" as used by this package.

This package assumes that template authors are trusted, that Execute's data
parameter is not, and seeks to preserve the properties below in the face of
untrusted data:

Structure Preservation Property: "... when a template author writes an HTML tag
in a safe templating language, the browser will interpret the corresponding
portion of the output as a tag regardless of the values of untrusted data, and
similarly for other structures such as attribute boundaries and JS and CSS
string boundaries."

Code Effect Property: "... only code specified by the template author should run
as a result of injecting the template output into a page and all code specified
by the template author should run as a result of the same."

Least Surprise Property: "A developer (or code reviewer) familiar with HTML,
CSS, and JavaScript, who knows that contextual autoescaping happens should be
able to look at a {{.}} and correctly infer what sanitization happens."

<a id="example"></a>
Example:

    const tpl = `
    <!DOCTYPE html>
    <html>
    	<head>
    		<meta charset="UTF-8">
    		<title>{{.Title}}</title>
    	</head>
    	<body>
    		{{range .Items}}<div>{{ . }}</div>{{else}}<div><strong>no rows</strong></div>{{end}}
    	</body>
    </html>`

    check := func(err error) {
        if err != nil {
            log.Fatal(err)
        }
    }
    t, err := template.New("webpage").Parse(tpl)
    check(err)

    data := struct {
        Title string
        Items []string
    }{
        Title: "My page",
        Items: []string{
            "My photos",
            "My blog",
        },
    }

    err = t.Execute(os.Stdout, data)
    check(err)

    noItems := struct {
        Title string
        Items []string
    }{
        Title: "My another page",
        Items: []string{},
    }

    err = t.Execute(os.Stdout, noItems)
    check(err)

    // Output:
    // <!DOCTYPE html>
    // <html>
    // 	<head>
    // 		<meta charset="UTF-8">
    // 		<title>My page</title>
    // 	</head>
    // 	<body>
    // 		<div>My photos</div><div>My blog</div>
    // 	</body>
    // </html>
    // <!DOCTYPE html>
    // <html>
    // 	<head>
    // 		<meta charset="UTF-8">
    // 		<title>My another page</title>
    // 	</head>
    // 	<body>
    // 		<div><strong>no rows</strong></div>
    // 	</body>
    // </html>


<a id="example_autoescaping"></a>
Example:

    check := func(err error) {
        if err != nil {
            log.Fatal(err)
        }
    }
    t, err := template.New("foo").Parse(`{{define "T"}}Hello, {{.}}!{{end}}`)
    check(err)
    err = t.ExecuteTemplate(os.Stdout, "T", "<script>alert('you have been pwned')</script>")
    check(err)
    // Output:
    // Hello, &lt;script&gt;alert(&#39;you have been pwned&#39;)&lt;/script&gt;!


<a id="example_escape"></a>
Example:

    const s = `"Fran & Freddie's Diner" <tasty@example.com>`
    v := []interface{}{`"Fran & Freddie's Diner"`, ' ', `<tasty@example.com>`}

    fmt.Println(template.HTMLEscapeString(s))
    template.HTMLEscape(os.Stdout, []byte(s))
    fmt.Fprintln(os.Stdout, "")
    fmt.Println(template.HTMLEscaper(v...))

    fmt.Println(template.JSEscapeString(s))
    template.JSEscape(os.Stdout, []byte(s))
    fmt.Fprintln(os.Stdout, "")
    fmt.Println(template.JSEscaper(v...))

    fmt.Println(template.URLQueryEscaper(v...))

    // Output:
    // &#34;Fran &amp; Freddie&#39;s Diner&#34; &lt;tasty@example.com&gt;
    // &#34;Fran &amp; Freddie&#39;s Diner&#34; &lt;tasty@example.com&gt;
    // &#34;Fran &amp; Freddie&#39;s Diner&#34;32&lt;tasty@example.com&gt;
    // \"Fran & Freddie\'s Diner\" \x3Ctasty@example.com\x3E
    // \"Fran & Freddie\'s Diner\" \x3Ctasty@example.com\x3E
    // \"Fran & Freddie\'s Diner\"32\x3Ctasty@example.com\x3E
    // %22Fran+%26+Freddie%27s+Diner%2232%3Ctasty%40example.com%3E

## Index

- [func HTMLEscape(w io.Writer, b []byte)](#HTMLEscape)
- [func HTMLEscapeString(s string) string](#HTMLEscapeString)
- [func HTMLEscaper(args ...interface{}) string](#HTMLEscaper)
- [func IsTrue(val interface{}) (truth, ok bool)](#IsTrue)
- [func JSEscape(w io.Writer, b []byte)](#JSEscape)
- [func JSEscapeString(s string) string](#JSEscapeString)
- [func JSEscaper(args ...interface{}) string](#JSEscaper)
- [func URLQueryEscaper(args ...interface{}) string](#URLQueryEscaper)
- [type CSS](#CSS)
- [type Error](#Error)
  - [func (e *Error) Error() string](#Error.Error)
- [type ErrorCode](#ErrorCode)
- [type FuncMap](#FuncMap)
- [type HTML](#HTML)
- [type HTMLAttr](#HTMLAttr)
- [type JS](#JS)
- [type JSStr](#JSStr)
- [type Srcset](#Srcset)
- [type Template](#Template)
  - [func Must(t *Template, err error) *Template](#Must)
  - [func New(name string) *Template](#New)
  - [func ParseFiles(filenames ...string) (*Template, error)](#ParseFiles)
  - [func ParseGlob(pattern string) (*Template, error)](#ParseGlob)
  - [func (t *Template) AddParseTree(name string, tree *parse.Tree) (*Template, error)](#Template.AddParseTree)
  - [func (t *Template) Clone() (*Template, error)](#Template.Clone)
  - [func (t *Template) DefinedTemplates() string](#Template.DefinedTemplates)
  - [func (t *Template) Delims(left, right string) *Template](#Template.Delims)
  - [func (t *Template) Execute(wr io.Writer, data interface{}) error](#Template.Execute)
  - [func (t *Template) ExecuteTemplate(wr io.Writer, name string, data interface{}) error](#Template.ExecuteTemplate)
  - [func (t *Template) Funcs(funcMap FuncMap) *Template](#Template.Funcs)
  - [func (t *Template) Lookup(name string) *Template](#Template.Lookup)
  - [func (t *Template) Name() string](#Template.Name)
  - [func (t *Template) New(name string) *Template](#Template.New)
  - [func (t *Template) Option(opt ...string) *Template](#Template.Option)
  - [func (t *Template) Parse(text string) (*Template, error)](#Template.Parse)
  - [func (t *Template) ParseFiles(filenames ...string) (*Template, error)](#Template.ParseFiles)
  - [func (t *Template) ParseGlob(pattern string) (*Template, error)](#Template.ParseGlob)
  - [func (t *Template) Templates() []*Template](#Template.Templates)
- [type URL](#URL)

### Examples

- [Package](#example)
- [Template (Block)](#exampleTemplate_block)
- [Template (Glob)](#exampleTemplate_glob)
- [Template (Helpers)](#exampleTemplate_helpers)
- [Template (Parsefiles)](#exampleTemplate_parsefiles)
- [Template (Share)](#exampleTemplate_share)
- [Package (Autoescaping)](#example_autoescaping)
- [Package (Escape)](#example_escape)

### Package files
 [attr.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/attr.go) [content.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/content.go) [context.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/context.go) [css.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/css.go) [doc.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/doc.go) [error.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/error.go) [escape.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/escape.go) [html.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/html.go) [js.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/js.go) [template.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go) [transition.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/transition.go) [url.go](//github.com/golang/go/blob/release-branch.go1.10/src/html/template/url.go)

<h2 id="HTMLEscape">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/escape.go#L855">HTMLEscape</a>
    <a href="#HTMLEscape">¶</a></h2>
<pre>func HTMLEscape(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, b []<a href="/builtin/#byte">byte</a>)</pre>

HTMLEscape writes to w the escaped HTML equivalent of the plain text data b.

<h2 id="HTMLEscapeString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/escape.go#L860">HTMLEscapeString</a>
    <a href="#HTMLEscapeString">¶</a></h2>
<pre>func HTMLEscapeString(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

HTMLEscapeString returns the escaped HTML equivalent of the plain text data s.

<h2 id="HTMLEscaper">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/escape.go#L866">HTMLEscaper</a>
    <a href="#HTMLEscaper">¶</a></h2>
<pre>func HTMLEscaper(args ...interface{}) <a href="/builtin/#string">string</a></pre>

HTMLEscaper returns the escaped HTML equivalent of the textual representation of
its arguments.

<h2 id="IsTrue">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L477">IsTrue</a>
    <a href="#IsTrue">¶</a></h2>
<pre>func IsTrue(val interface{}) (truth, ok <a href="/builtin/#bool">bool</a>)</pre>

IsTrue reports whether the value is 'true', in the sense of not the zero of its
type, and whether the value has a meaningful truth value. This is the definition
of truth used by if and other such actions.

<h2 id="JSEscape">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/escape.go#L871">JSEscape</a>
    <a href="#JSEscape">¶</a></h2>
<pre>func JSEscape(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, b []<a href="/builtin/#byte">byte</a>)</pre>

JSEscape writes to w the escaped JavaScript equivalent of the plain text data b.

<h2 id="JSEscapeString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/escape.go#L876">JSEscapeString</a>
    <a href="#JSEscapeString">¶</a></h2>
<pre>func JSEscapeString(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

JSEscapeString returns the escaped JavaScript equivalent of the plain text data
s.

<h2 id="JSEscaper">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/escape.go#L882">JSEscaper</a>
    <a href="#JSEscaper">¶</a></h2>
<pre>func JSEscaper(args ...interface{}) <a href="/builtin/#string">string</a></pre>

JSEscaper returns the escaped JavaScript equivalent of the textual
representation of its arguments.

<h2 id="URLQueryEscaper">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/escape.go#L888">URLQueryEscaper</a>
    <a href="#URLQueryEscaper">¶</a></h2>
<pre>func URLQueryEscaper(args ...interface{}) <a href="/builtin/#string">string</a></pre>

URLQueryEscaper returns the escaped value of the textual representation of its
arguments in a form suitable for embedding in a URL query.

<h2 id="CSS">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/content.go#L15">CSS</a>
    <a href="#CSS">¶</a></h2>
<pre>type CSS <a href="/builtin/#string">string</a></pre>

CSS encapsulates known safe content that matches any of:

    1. The CSS3 stylesheet production, such as `p { color: purple }`.
    2. The CSS3 rule production, such as `a[href=~"https:"].foo#bar`.
    3. CSS3 declaration productions, such as `color: red; margin: 2px`.
    4. The CSS3 value production, such as `rgba(0, 0, 255, 127)`.

See http://www.w3.org/TR/css3-syntax/#parsing and
https://web.archive.org/web/20090211114933/http://w3.org/TR/css3-syntax#style

Use of this type presents a security risk: the encapsulated content should come
from a trusted source, as it will be included verbatim in the template output.

<h2 id="Error">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/error.go#L3">Error</a>
    <a href="#Error">¶</a></h2>
<pre>type Error struct {
<span id="Error.ErrorCode"></span>    <span class="comment">// ErrorCode describes the kind of error.</span>
    ErrorCode <a href="#ErrorCode">ErrorCode</a>
<span id="Error.Node"></span>    <span class="comment">// Node is the node that caused the problem, if known.</span>
    <span class="comment">// If not nil, it overrides Name and Line.</span>
    Node <a href="/text/template/parse/">parse</a>.<a href="/text/template/parse/#Node">Node</a>
<span id="Error.Name"></span>    <span class="comment">// Name is the name of the template in which the error was encountered.</span>
    Name <a href="/builtin/#string">string</a>
<span id="Error.Line"></span>    <span class="comment">// Line is the line number of the error in the template source or 0.</span>
    Line <a href="/builtin/#int">int</a>
<span id="Error.Description"></span>    <span class="comment">// Description is a human-readable description of the problem.</span>
    Description <a href="/builtin/#string">string</a>
}</pre>

Error describes a problem encountered during template Escaping.

<h3 id="Error.Error">func (*Error) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/error.go#L206">Error</a>
    <a href="#Error.Error">¶</a></h3>
<pre>func (e *<a href="#Error">Error</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="ErrorCode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/error.go#L18">ErrorCode</a>
    <a href="#ErrorCode">¶</a></h2>
<pre>type ErrorCode <a href="/builtin/#int">int</a></pre>

ErrorCode is a code for a kind of error.

<pre>const (
    <span class="comment">// OK indicates the lack of an error.</span>
    <span id="OK">OK</span> <a href="#ErrorCode">ErrorCode</a> = <a href="/builtin/#iota">iota</a>

    <span class="comment">// ErrAmbigContext: &#34;... appears in an ambiguous context within a URL&#34;</span>
    <span class="comment">// Example:</span>
    <span class="comment">//   &lt;a href=&#34;</span>
    <span class="comment">//      {{if .C}}</span>
    <span class="comment">//        /path/</span>
    <span class="comment">//      {{else}}</span>
    <span class="comment">//        /search?q=</span>
    <span class="comment">//      {{end}}</span>
    <span class="comment">//      {{.X}}</span>
    <span class="comment">//   &#34;&gt;</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   {{.X}} is in an ambiguous URL context since, depending on {{.C}},</span>
    <span class="comment">//  it may be either a URL suffix or a query parameter.</span>
    <span class="comment">//   Moving {{.X}} into the condition removes the ambiguity:</span>
    <span class="comment">//   &lt;a href=&#34;{{if .C}}/path/{{.X}}{{else}}/search?q={{.X}}&#34;&gt;</span>
    <span id="ErrAmbigContext">ErrAmbigContext</span>

    <span class="comment">// ErrBadHTML: &#34;expected space, attr name, or end of tag, but got ...&#34;,</span>
    <span class="comment">//   &#34;... in unquoted attr&#34;, &#34;... in attribute name&#34;</span>
    <span class="comment">// Example:</span>
    <span class="comment">//   &lt;a href = /search?q=foo&gt;</span>
    <span class="comment">//   &lt;href=foo&gt;</span>
    <span class="comment">//   &lt;form na&lt;e=...&gt;</span>
    <span class="comment">//   &lt;option selected&lt;</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   This is often due to a typo in an HTML element, but some runes</span>
    <span class="comment">//   are banned in tag names, attribute names, and unquoted attribute</span>
    <span class="comment">//   values because they can tickle parser ambiguities.</span>
    <span class="comment">//   Quoting all attributes is the best policy.</span>
    <span id="ErrBadHTML">ErrBadHTML</span>

    <span class="comment">// ErrBranchEnd: &#34;{{if}} branches end in different contexts&#34;</span>
    <span class="comment">// Example:</span>
    <span class="comment">//   {{if .C}}&lt;a href=&#34;{{end}}{{.X}}</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   Package html/template statically examines each path through an</span>
    <span class="comment">//   {{if}}, {{range}}, or {{with}} to escape any following pipelines.</span>
    <span class="comment">//   The example is ambiguous since {{.X}} might be an HTML text node,</span>
    <span class="comment">//   or a URL prefix in an HTML attribute. The context of {{.X}} is</span>
    <span class="comment">//   used to figure out how to escape it, but that context depends on</span>
    <span class="comment">//   the run-time value of {{.C}} which is not statically known.</span>
    <span class="comment">//</span>
    <span class="comment">//   The problem is usually something like missing quotes or angle</span>
    <span class="comment">//   brackets, or can be avoided by refactoring to put the two contexts</span>
    <span class="comment">//   into different branches of an if, range or with. If the problem</span>
    <span class="comment">//   is in a {{range}} over a collection that should never be empty,</span>
    <span class="comment">//   adding a dummy {{else}} can help.</span>
    <span id="ErrBranchEnd">ErrBranchEnd</span>

    <span class="comment">// ErrEndContext: &#34;... ends in a non-text context: ...&#34;</span>
    <span class="comment">// Examples:</span>
    <span class="comment">//   &lt;div</span>
    <span class="comment">//   &lt;div title=&#34;no close quote&gt;</span>
    <span class="comment">//   &lt;script&gt;f()</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   Executed templates should produce a DocumentFragment of HTML.</span>
    <span class="comment">//   Templates that end without closing tags will trigger this error.</span>
    <span class="comment">//   Templates that should not be used in an HTML context or that</span>
    <span class="comment">//   produce incomplete Fragments should not be executed directly.</span>
    <span class="comment">//</span>
    <span class="comment">//   {{define &#34;main&#34;}} &lt;script&gt;{{template &#34;helper&#34;}}&lt;/script&gt; {{end}}</span>
    <span class="comment">//   {{define &#34;helper&#34;}} document.write(&#39; &lt;div title=&#34; &#39;) {{end}}</span>
    <span class="comment">//</span>
    <span class="comment">//   &#34;helper&#34; does not produce a valid document fragment, so should</span>
    <span class="comment">//   not be Executed directly.</span>
    <span id="ErrEndContext">ErrEndContext</span>

    <span class="comment">// ErrNoSuchTemplate: &#34;no such template ...&#34;</span>
    <span class="comment">// Examples:</span>
    <span class="comment">//   {{define &#34;main&#34;}}&lt;div {{template &#34;attrs&#34;}}&gt;{{end}}</span>
    <span class="comment">//   {{define &#34;attrs&#34;}}href=&#34;{{.URL}}&#34;{{end}}</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   Package html/template looks through template calls to compute the</span>
    <span class="comment">//   context.</span>
    <span class="comment">//   Here the {{.URL}} in &#34;attrs&#34; must be treated as a URL when called</span>
    <span class="comment">//   from &#34;main&#34;, but you will get this error if &#34;attrs&#34; is not defined</span>
    <span class="comment">//   when &#34;main&#34; is parsed.</span>
    <span id="ErrNoSuchTemplate">ErrNoSuchTemplate</span>

    <span class="comment">// ErrOutputContext: &#34;cannot compute output context for template ...&#34;</span>
    <span class="comment">// Examples:</span>
    <span class="comment">//   {{define &#34;t&#34;}}{{if .T}}{{template &#34;t&#34; .T}}{{end}}{{.H}}&#34;,{{end}}</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   A recursive template does not end in the same context in which it</span>
    <span class="comment">//   starts, and a reliable output context cannot be computed.</span>
    <span class="comment">//   Look for typos in the named template.</span>
    <span class="comment">//   If the template should not be called in the named start context,</span>
    <span class="comment">//   look for calls to that template in unexpected contexts.</span>
    <span class="comment">//   Maybe refactor recursive templates to not be recursive.</span>
    <span id="ErrOutputContext">ErrOutputContext</span>

    <span class="comment">// ErrPartialCharset: &#34;unfinished JS regexp charset in ...&#34;</span>
    <span class="comment">// Example:</span>
    <span class="comment">//     &lt;script&gt;var pattern = /foo[{{.Chars}}]/&lt;/script&gt;</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   Package html/template does not support interpolation into regular</span>
    <span class="comment">//   expression literal character sets.</span>
    <span id="ErrPartialCharset">ErrPartialCharset</span>

    <span class="comment">// ErrPartialEscape: &#34;unfinished escape sequence in ...&#34;</span>
    <span class="comment">// Example:</span>
    <span class="comment">//   &lt;script&gt;alert(&#34;\{{.X}}&#34;)&lt;/script&gt;</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   Package html/template does not support actions following a</span>
    <span class="comment">//   backslash.</span>
    <span class="comment">//   This is usually an error and there are better solutions; for</span>
    <span class="comment">//   example</span>
    <span class="comment">//     &lt;script&gt;alert(&#34;{{.X}}&#34;)&lt;/script&gt;</span>
    <span class="comment">//   should work, and if {{.X}} is a partial escape sequence such as</span>
    <span class="comment">//   &#34;xA0&#34;, mark the whole sequence as safe content: JSStr(`\xA0`)</span>
    <span id="ErrPartialEscape">ErrPartialEscape</span>

    <span class="comment">// ErrRangeLoopReentry: &#34;on range loop re-entry: ...&#34;</span>
    <span class="comment">// Example:</span>
    <span class="comment">//   &lt;script&gt;var x = [{{range .}}&#39;{{.}},{{end}}]&lt;/script&gt;</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   If an iteration through a range would cause it to end in a</span>
    <span class="comment">//   different context than an earlier pass, there is no single context.</span>
    <span class="comment">//   In the example, there is missing a quote, so it is not clear</span>
    <span class="comment">//   whether {{.}} is meant to be inside a JS string or in a JS value</span>
    <span class="comment">//   context. The second iteration would produce something like</span>
    <span class="comment">//</span>
    <span class="comment">//     &lt;script&gt;var x = [&#39;firstValue,&#39;secondValue]&lt;/script&gt;</span>
    <span id="ErrRangeLoopReentry">ErrRangeLoopReentry</span>

    <span class="comment">// ErrSlashAmbig: &#39;/&#39; could start a division or regexp.</span>
    <span class="comment">// Example:</span>
    <span class="comment">//   &lt;script&gt;</span>
    <span class="comment">//     {{if .C}}var x = 1{{end}}</span>
    <span class="comment">//     /-{{.N}}/i.test(x) ? doThis : doThat();</span>
    <span class="comment">//   &lt;/script&gt;</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   The example above could produce `var x = 1/-2/i.test(s)...`</span>
    <span class="comment">//   in which the first &#39;/&#39; is a mathematical division operator or it</span>
    <span class="comment">//   could produce `/-2/i.test(s)` in which the first &#39;/&#39; starts a</span>
    <span class="comment">//   regexp literal.</span>
    <span class="comment">//   Look for missing semicolons inside branches, and maybe add</span>
    <span class="comment">//   parentheses to make it clear which interpretation you intend.</span>
    <span id="ErrSlashAmbig">ErrSlashAmbig</span>

    <span class="comment">// ErrPredefinedEscaper: &#34;predefined escaper ... disallowed in template&#34;</span>
    <span class="comment">// Example:</span>
    <span class="comment">//   &lt;div class={{. | html}}&gt;Hello&lt;div&gt;</span>
    <span class="comment">// Discussion:</span>
    <span class="comment">//   Package html/template already contextually escapes all pipelines to</span>
    <span class="comment">//   produce HTML output safe against code injection. Manually escaping</span>
    <span class="comment">//   pipeline output using the predefined escapers &#34;html&#34; or &#34;urlquery&#34; is</span>
    <span class="comment">//   unnecessary, and may affect the correctness or safety of the escaped</span>
    <span class="comment">//   pipeline output in Go 1.8 and earlier.</span>
    <span class="comment">//</span>
    <span class="comment">//   In most cases, such as the given example, this error can be resolved by</span>
    <span class="comment">//   simply removing the predefined escaper from the pipeline and letting the</span>
    <span class="comment">//   contextual autoescaper handle the escaping of the pipeline. In other</span>
    <span class="comment">//   instances, where the predefined escaper occurs in the middle of a</span>
    <span class="comment">//   pipeline where subsequent commands expect escaped input, e.g.</span>
    <span class="comment">//     {{.X | html | makeALink}}</span>
    <span class="comment">//   where makeALink does</span>
    <span class="comment">//     return `&lt;a href=&#34;`+input+`&#34;&gt;link&lt;/a&gt;`</span>
    <span class="comment">//   consider refactoring the surrounding template to make use of the</span>
    <span class="comment">//   contextual autoescaper, i.e.</span>
    <span class="comment">//     &lt;a href=&#34;{{.X}}&#34;&gt;link&lt;/a&gt;</span>
    <span class="comment">//</span>
    <span class="comment">//   To ease migration to Go 1.9 and beyond, &#34;html&#34; and &#34;urlquery&#34; will</span>
    <span class="comment">//   continue to be allowed as the last command in a pipeline. However, if the</span>
    <span class="comment">//   pipeline occurs in an unquoted attribute value context, &#34;html&#34; is</span>
    <span class="comment">//   disallowed. Avoid using &#34;html&#34; and &#34;urlquery&#34; entirely in new templates.</span>
    <span id="ErrPredefinedEscaper">ErrPredefinedEscaper</span>
)</pre>

We define codes for each error that manifests while escaping templates, but
escaped templates may also fail at runtime.

Output: "ZgotmplZ" Example:

    <img src="{{.X}}">
    where {{.X}} evaluates to `javascript:...`

Discussion:

    "ZgotmplZ" is a special value that indicates that unsafe content reached a
    CSS or URL context at runtime. The output of the example will be
      <img src="#ZgotmplZ">
    If the data comes from a trusted source, use content types to exempt it
    from filtering: URL(`javascript:...`).

<h2 id="FuncMap">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L326">FuncMap</a>
    <a href="#FuncMap">¶</a></h2>
<pre>type FuncMap map[<a href="/builtin/#string">string</a>]interface{}</pre>

FuncMap is the type of the map defining the mapping from names to functions.
Each function must have either a single return value, or two return values of
which the second has type error. In that case, if the second (error) argument
evaluates to non-nil during execution, execution terminates and Execute returns
that error. FuncMap has the same base type as FuncMap in "text/template", copied
here so clients need not import "text/template".

<h2 id="HTML">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/content.go#L25">HTML</a>
    <a href="#HTML">¶</a></h2>
<pre>type HTML <a href="/builtin/#string">string</a></pre>

HTML encapsulates a known safe HTML document fragment. It should not be used for
HTML from a third-party, or HTML with unclosed tags or comments. The outputs of
a sound HTML sanitizer and a template escaped by this package are fine for use
with HTML.

Use of this type presents a security risk: the encapsulated content should come
from a trusted source, as it will be included verbatim in the template output.

<h2 id="HTMLAttr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/content.go#L33">HTMLAttr</a>
    <a href="#HTMLAttr">¶</a></h2>
<pre>type HTMLAttr <a href="/builtin/#string">string</a></pre>

HTMLAttr encapsulates an HTML attribute from a trusted source, for example, `
dir="ltr"`.

Use of this type presents a security risk: the encapsulated content should come
from a trusted source, as it will be included verbatim in the template output.

<h2 id="JS">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/content.go#L51">JS</a>
    <a href="#JS">¶</a></h2>
<pre>type JS <a href="/builtin/#string">string</a></pre>

JS encapsulates a known safe EcmaScript5 Expression, for example, `(x + y *
z())`. Template authors are responsible for ensuring that typed expressions do
not break the intended precedence and that there is no statement/expression
ambiguity as when passing an expression like "{ foo: bar() }\n['foo']()", which
is both a valid Expression and a valid Program with a very different meaning.

Use of this type presents a security risk: the encapsulated content should come
from a trusted source, as it will be included verbatim in the template output.

Using JS to include valid but untrusted JSON is not safe. A safe alternative is
to parse the JSON with json.Unmarshal and then pass the resultant object into
the template, where it will be converted to sanitized JSON when presented in a
JavaScript context.

<h2 id="JSStr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/content.go#L64">JSStr</a>
    <a href="#JSStr">¶</a></h2>
<pre>type JSStr <a href="/builtin/#string">string</a></pre>

JSStr encapsulates a sequence of characters meant to be embedded between quotes
in a JavaScript expression. The string must match a series of StringCharacters:

    StringCharacter :: SourceCharacter but not `\` or LineTerminator
                     | EscapeSequence

Note that LineContinuations are not allowed. JSStr("foo\\nbar") is fine, but
JSStr("foo\\\nbar") is not.

Use of this type presents a security risk: the encapsulated content should come
from a trusted source, as it will be included verbatim in the template output.

<h2 id="Srcset">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/content.go#L83">Srcset</a>
    <a href="#Srcset">¶</a></h2>
<pre>type Srcset <a href="/builtin/#string">string</a></pre>

Srcset encapsulates a known safe srcset attribute (see
http://w3c.github.io/html/semantics-embedded-content.html#element-attrdef-img-srcset).

Use of this type presents a security risk: the encapsulated content should come
from a trusted source, as it will be included verbatim in the template output.

<h2 id="Template">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L9">Template</a>
    <a href="#Template">¶</a></h2>
<pre>type Template struct {

    <span class="comment">// The underlying template&#39;s parse tree, updated to be HTML-safe.</span>
<span id="Template.Tree"></span>    Tree *<a href="/text/template/parse/">parse</a>.<a href="/text/template/parse/#Tree">Tree</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Template is a specialized Template from "text/template" that produces a safe
HTML document fragment.

<a id="exampleTemplate_block"></a>
Example:

    const (
        master  = `Names:{{block "list" .}}{{"\n"}}{{range .}}{{println "-" .}}{{end}}{{end}}`
        overlay = `{{define "list"}} {{join . ", "}}{{end}} `
    )
    var (
        funcs     = template.FuncMap{"join": strings.Join}
        guardians = []string{"Gamora", "Groot", "Nebula", "Rocket", "Star-Lord"}
    )
    masterTmpl, err := template.New("master").Funcs(funcs).Parse(master)
    if err != nil {
        log.Fatal(err)
    }
    overlayTmpl, err := template.Must(masterTmpl.Clone()).Parse(overlay)
    if err != nil {
        log.Fatal(err)
    }
    if err := masterTmpl.Execute(os.Stdout, guardians); err != nil {
        log.Fatal(err)
    }
    if err := overlayTmpl.Execute(os.Stdout, guardians); err != nil {
        log.Fatal(err)
    }
    // Output:
    // Names:
    // - Gamora
    // - Groot
    // - Nebula
    // - Rocket
    // - Star-Lord
    // Names: Gamora, Groot, Nebula, Rocket, Star-Lord


<a id="exampleTemplate_glob"></a>
Example:

    // Here we create a temporary directory and populate it with our sample
    // template definition files; usually the template files would already
    // exist in some location known to the program.
    dir := createTestDir([]templateFile{
        // T0.tmpl is a plain template file that just invokes T1.
        {"T0.tmpl", `T0 invokes T1: ({{template "T1"}})`},
        // T1.tmpl defines a template, T1 that invokes T2.
        {"T1.tmpl", `{{define "T1"}}T1 invokes T2: ({{template "T2"}}){{end}}`},
        // T2.tmpl defines a template T2.
        {"T2.tmpl", `{{define "T2"}}This is T2{{end}}`},
    })
    // Clean up after the test; another quirk of running as an example.
    defer os.RemoveAll(dir)

    // pattern is the glob pattern used to find all the template files.
    pattern := filepath.Join(dir, "*.tmpl")

    // Here starts the example proper.
    // T0.tmpl is the first name matched, so it becomes the starting template,
    // the value returned by ParseGlob.
    tmpl := template.Must(template.ParseGlob(pattern))

    err := tmpl.Execute(os.Stdout, nil)
    if err != nil {
        log.Fatalf("template execution: %s", err)
    }
    // Output:
    // T0 invokes T1: (T1 invokes T2: (This is T2))


<a id="exampleTemplate_helpers"></a>
Example:

    // Here we create a temporary directory and populate it with our sample
    // template definition files; usually the template files would already
    // exist in some location known to the program.
    dir := createTestDir([]templateFile{
        // T1.tmpl defines a template, T1 that invokes T2.
        {"T1.tmpl", `{{define "T1"}}T1 invokes T2: ({{template "T2"}}){{end}}`},
        // T2.tmpl defines a template T2.
        {"T2.tmpl", `{{define "T2"}}This is T2{{end}}`},
    })
    // Clean up after the test; another quirk of running as an example.
    defer os.RemoveAll(dir)

    // pattern is the glob pattern used to find all the template files.
    pattern := filepath.Join(dir, "*.tmpl")

    // Here starts the example proper.
    // Load the helpers.
    templates := template.Must(template.ParseGlob(pattern))
    // Add one driver template to the bunch; we do this with an explicit template definition.
    _, err := templates.Parse("{{define `driver1`}}Driver 1 calls T1: ({{template `T1`}})\n{{end}}")
    if err != nil {
        log.Fatal("parsing driver1: ", err)
    }
    // Add another driver template.
    _, err = templates.Parse("{{define `driver2`}}Driver 2 calls T2: ({{template `T2`}})\n{{end}}")
    if err != nil {
        log.Fatal("parsing driver2: ", err)
    }
    // We load all the templates before execution. This package does not require
    // that behavior but html/template's escaping does, so it's a good habit.
    err = templates.ExecuteTemplate(os.Stdout, "driver1", nil)
    if err != nil {
        log.Fatalf("driver1 execution: %s", err)
    }
    err = templates.ExecuteTemplate(os.Stdout, "driver2", nil)
    if err != nil {
        log.Fatalf("driver2 execution: %s", err)
    }
    // Output:
    // Driver 1 calls T1: (T1 invokes T2: (This is T2))
    // Driver 2 calls T2: (This is T2)


<a id="exampleTemplate_parsefiles"></a>
Example:

    // Here we create different temporary directories and populate them with our sample
    // template definition files; usually the template files would already
    // exist in some location known to the program.
    dir1 := createTestDir([]templateFile{
        // T1.tmpl is a plain template file that just invokes T2.
        {"T1.tmpl", `T1 invokes T2: ({{template "T2"}})`},
    })

    dir2 := createTestDir([]templateFile{
        // T2.tmpl defines a template T2.
        {"T2.tmpl", `{{define "T2"}}This is T2{{end}}`},
    })

    // Clean up after the test; another quirk of running as an example.
    defer func(dirs ...string) {
        for _, dir := range dirs {
            os.RemoveAll(dir)
        }
    }(dir1, dir2)

    // Here starts the example proper.
    // Let's just parse only dir1/T0 and dir2/T2
    paths := []string{
        filepath.Join(dir1, "T1.tmpl"),
        filepath.Join(dir2, "T2.tmpl"),
    }
    tmpl := template.Must(template.ParseFiles(paths...))

    err := tmpl.Execute(os.Stdout, nil)
    if err != nil {
        log.Fatalf("template execution: %s", err)
    }
    // Output:
    // T1 invokes T2: (This is T2)


<a id="exampleTemplate_share"></a>
Example:

    // Here we create a temporary directory and populate it with our sample
    // template definition files; usually the template files would already
    // exist in some location known to the program.
    dir := createTestDir([]templateFile{
        // T0.tmpl is a plain template file that just invokes T1.
        {"T0.tmpl", "T0 ({{.}} version) invokes T1: ({{template `T1`}})\n"},
        // T1.tmpl defines a template, T1 that invokes T2. Note T2 is not defined
        {"T1.tmpl", `{{define "T1"}}T1 invokes T2: ({{template "T2"}}){{end}}`},
    })
    // Clean up after the test; another quirk of running as an example.
    defer os.RemoveAll(dir)

    // pattern is the glob pattern used to find all the template files.
    pattern := filepath.Join(dir, "*.tmpl")

    // Here starts the example proper.
    // Load the drivers.
    drivers := template.Must(template.ParseGlob(pattern))

    // We must define an implementation of the T2 template. First we clone
    // the drivers, then add a definition of T2 to the template name space.

    // 1. Clone the helper set to create a new name space from which to run them.
    first, err := drivers.Clone()
    if err != nil {
        log.Fatal("cloning helpers: ", err)
    }
    // 2. Define T2, version A, and parse it.
    _, err = first.Parse("{{define `T2`}}T2, version A{{end}}")
    if err != nil {
        log.Fatal("parsing T2: ", err)
    }

    // Now repeat the whole thing, using a different version of T2.
    // 1. Clone the drivers.
    second, err := drivers.Clone()
    if err != nil {
        log.Fatal("cloning drivers: ", err)
    }
    // 2. Define T2, version B, and parse it.
    _, err = second.Parse("{{define `T2`}}T2, version B{{end}}")
    if err != nil {
        log.Fatal("parsing T2: ", err)
    }

    // Execute the templates in the reverse order to verify the
    // first is unaffected by the second.
    err = second.ExecuteTemplate(os.Stdout, "T0.tmpl", "second")
    if err != nil {
        log.Fatalf("second execution: %s", err)
    }
    err = first.ExecuteTemplate(os.Stdout, "T0.tmpl", "first")
    if err != nil {
        log.Fatalf("first: execution: %s", err)
    }

    // Output:
    // T0 (second version) invokes T1: (T1 invokes T2: (T2, version B))
    // T0 (first version) invokes T1: (T1 invokes T2: (T2, version A))

<h3 id="Must">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L360">Must</a>
    <a href="#Must">¶</a></h3>
<pre>func Must(t *<a href="#Template">Template</a>, err <a href="/builtin/#error">error</a>) *<a href="#Template">Template</a></pre>

Must is a helper that wraps a call to a function returning (*Template, error)
and panics if the error is non-nil. It is intended for use in variable
initializations such as

    var t = template.Must(template.New("name").Parse("html"))

<h3 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L272">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(name <a href="/builtin/#string">string</a>) *<a href="#Template">Template</a></pre>

New allocates a new HTML template with the given name.

<h3 id="ParseFiles">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L376">ParseFiles</a>
    <a href="#ParseFiles">¶</a></h3>
<pre>func ParseFiles(filenames ...<a href="/builtin/#string">string</a>) (*<a href="#Template">Template</a>, <a href="/builtin/#error">error</a>)</pre>

ParseFiles creates a new Template and parses the template definitions from the
named files. The returned template's name will have the (base) name and (parsed)
contents of the first file. There must be at least one file. If an error occurs,
parsing stops and the returned *Template is nil.

When parsing multiple files with the same name in different directories, the
last one mentioned will be the one that results. For instance,
ParseFiles("a/foo", "b/foo") stores "b/foo" as the template named "foo", while
"a/foo" is unavailable.

<h3 id="ParseGlob">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L441">ParseGlob</a>
    <a href="#ParseGlob">¶</a></h3>
<pre>func ParseGlob(pattern <a href="/builtin/#string">string</a>) (*<a href="#Template">Template</a>, <a href="/builtin/#error">error</a>)</pre>

ParseGlob creates a new Template and parses the template definitions from the
files identified by the pattern, which must match at least one file. The
returned template will have the (base) name and (parsed) contents of the first
file matched by the pattern. ParseGlob is equivalent to calling ParseFiles with
the list of files matched by the pattern.

When parsing multiple files with the same name in different directories, the
last one mentioned will be the one that results.

<h3 id="Template.AddParseTree">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L205">AddParseTree</a>
    <a href="#Template.AddParseTree">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) AddParseTree(name <a href="/builtin/#string">string</a>, tree *<a href="/text/template/parse/">parse</a>.<a href="/text/template/parse/#Tree">Tree</a>) (*<a href="#Template">Template</a>, <a href="/builtin/#error">error</a>)</pre>

AddParseTree creates a new template with the name and parse tree and associates
it with t.

It returns an error if t or any associated template has already been executed.

<h3 id="Template.Clone">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L234">Clone</a>
    <a href="#Template.Clone">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) Clone() (*<a href="#Template">Template</a>, <a href="/builtin/#error">error</a>)</pre>

Clone returns a duplicate of the template, including all associated templates.
The actual representation is not copied, but the name space of associated
templates is, so further calls to Parse in the copy will add templates to the
copy but not to the original. Clone can be used to prepare common templates and
use them with variant definitions for other templates by adding the variants
after the clone is made.

It returns an error if t has already been executed.

<h3 id="Template.DefinedTemplates">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L159">DefinedTemplates</a>
    <a href="#Template.DefinedTemplates">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) DefinedTemplates() <a href="/builtin/#string">string</a></pre>

DefinedTemplates returns a string listing the defined templates, prefixed by the
string "; defined templates are: ". If there are none, it returns the empty
string. Used to generate an error message.

<h3 id="Template.Delims">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L343">Delims</a>
    <a href="#Template.Delims">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) Delims(left, right <a href="/builtin/#string">string</a>) *<a href="#Template">Template</a></pre>

Delims sets the action delimiters to the specified strings, to be used in
subsequent calls to Parse, ParseFiles, or ParseGlob. Nested template definitions
will inherit the settings. An empty delimiter stands for the corresponding
default: {{ or }}. The return value is the template, so calls can be chained.

<h3 id="Template.Execute">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L108">Execute</a>
    <a href="#Template.Execute">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) Execute(wr <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, data interface{}) <a href="/builtin/#error">error</a></pre>

Execute applies a parsed template to the specified data object, writing the
output to wr. If an error occurs executing the template or writing its output,
execution stops, but partial results may already have been written to the output
writer. A template may be executed safely in parallel, although if parallel
executions share a Writer the output may be interleaved.

<h3 id="Template.ExecuteTemplate">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L122">ExecuteTemplate</a>
    <a href="#Template.ExecuteTemplate">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) ExecuteTemplate(wr <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, name <a href="/builtin/#string">string</a>, data interface{}) <a href="/builtin/#error">error</a></pre>

ExecuteTemplate applies the template associated with t that has the given name
to the specified data object and writes the output to wr. If an error occurs
executing the template or writing its output, execution stops, but partial
results may already have been written to the output writer. A template may be
executed safely in parallel, although if parallel executions share a Writer the
output may be interleaved.

<h3 id="Template.Funcs">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L333">Funcs</a>
    <a href="#Template.Funcs">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) Funcs(funcMap <a href="#FuncMap">FuncMap</a>) *<a href="#Template">Template</a></pre>

Funcs adds the elements of the argument map to the template's function map. It
must be called before the template is parsed. It panics if a value in the map is
not a function with appropriate return type. However, it is legal to overwrite
elements of the map. The return value is the template, so calls can be chained.

<h3 id="Template.Lookup">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L350">Lookup</a>
    <a href="#Template.Lookup">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) Lookup(name <a href="/builtin/#string">string</a>) *<a href="#Template">Template</a></pre>

Lookup returns the template with the given name that is associated with t, or
nil if there is no such template.

<h3 id="Template.Name">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L315">Name</a>
    <a href="#Template.Name">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) Name() <a href="/builtin/#string">string</a></pre>

Name returns the name of the template.

<h3 id="Template.New">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L292">New</a>
    <a href="#Template.New">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) New(name <a href="/builtin/#string">string</a>) *<a href="#Template">Template</a></pre>

New allocates a new HTML template associated with the given one and with the
same delimiters. The association, which is transitive, allows one template to
invoke another with a {{template}} action.

If a template with the given name already exists, the new HTML template will
replace it. The existing template will be reset and disassociated with t.

<h3 id="Template.Option">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L64">Option</a>
    <a href="#Template.Option">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) Option(opt ...<a href="/builtin/#string">string</a>) *<a href="#Template">Template</a></pre>

Option sets options for the template. Options are described by strings, either a
simple string or "key=value". There can be at most one equals sign in an option
string. If the option string is unrecognized or otherwise invalid, Option
panics.

Known options:

missingkey: Control the behavior during execution if a map is indexed with a key
that is not present in the map.

    "missingkey=default" or "missingkey=invalid"
    	The default behavior: Do nothing and continue execution.
    	If printed, the result of the index operation is the string
    	"<no value>".
    "missingkey=zero"
    	The operation returns the zero value for the map type's element.
    "missingkey=error"
    	Execution stops immediately with an error.

<h3 id="Template.Parse">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L174">Parse</a>
    <a href="#Template.Parse">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) Parse(text <a href="/builtin/#string">string</a>) (*<a href="#Template">Template</a>, <a href="/builtin/#error">error</a>)</pre>

Parse parses text as a template body for t. Named template definitions ({{define
...}} or {{block ...}} statements) in text define additional templates
associated with t and are removed from the definition of t itself.

Templates can be redefined in successive calls to Parse, before the first use of
Execute on t or any associated template. A template definition with a body
containing only white space and comments is considered empty and will not
replace an existing template's body. This allows using Parse to add new named
template definitions without overwriting the main template body.

<h3 id="Template.ParseFiles">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L388">ParseFiles</a>
    <a href="#Template.ParseFiles">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) ParseFiles(filenames ...<a href="/builtin/#string">string</a>) (*<a href="#Template">Template</a>, <a href="/builtin/#error">error</a>)</pre>

ParseFiles parses the named files and associates the resulting templates with t.
If an error occurs, parsing stops and the returned template is nil; otherwise it
is t. There must be at least one file.

When parsing multiple files with the same name in different directories, the
last one mentioned will be the one that results.

ParseFiles returns an error if t or any associated template has already been
executed.

<h3 id="Template.ParseGlob">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L455">ParseGlob</a>
    <a href="#Template.ParseGlob">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) ParseGlob(pattern <a href="/builtin/#string">string</a>) (*<a href="#Template">Template</a>, <a href="/builtin/#error">error</a>)</pre>

ParseGlob parses the template definitions in the files identified by the pattern
and associates the resulting templates with t. The pattern is processed by
filepath.Glob and must match at least one file. ParseGlob is equivalent to
calling t.ParseFiles with the list of files matched by the pattern.

When parsing multiple files with the same name in different directories, the
last one mentioned will be the one that results.

ParseGlob returns an error if t or any associated template has already been
executed.

<h3 id="Template.Templates">func (*Template) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/template.go#L34">Templates</a>
    <a href="#Template.Templates">¶</a></h3>
<pre>func (t *<a href="#Template">Template</a>) Templates() []*<a href="#Template">Template</a></pre>

Templates returns a slice of the templates associated with t, including t
itself.

<h2 id="URL">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/html/template/content.go#L75">URL</a>
    <a href="#URL">¶</a></h2>
<pre>type URL <a href="/builtin/#string">string</a></pre>

URL encapsulates a known safe URL or URL substring (see RFC 3986). A URL like
`javascript:checkThatFormNotEditedBeforeLeavingPage()` from a trusted source
should go in the page, but by default dynamic `javascript:` URLs are filtered
out since they are a frequently exploited injection vector.

Use of this type presents a security risk: the encapsulated content should come
from a trusted source, as it will be included verbatim in the template output.


