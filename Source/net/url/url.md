version: 1.9.2
## package url

  `import "net/url"`

## Overview

Package url parses URLs and implements query escaping.

## Index

- [func PathEscape(s string) string](#PathEscape)
- [func PathUnescape(s string) (string, error)](#PathUnescape)
- [func QueryEscape(s string) string](#QueryEscape)
- [func QueryUnescape(s string) (string, error)](#QueryUnescape)
- [type Error](#Error)
  - [func (e *Error) Error() string](#Error.Error)
  - [func (e *Error) Temporary() bool](#Error.Temporary)
  - [func (e *Error) Timeout() bool](#Error.Timeout)
- [type EscapeError](#EscapeError)
  - [func (e EscapeError) Error() string](#EscapeError.Error)
- [type InvalidHostError](#InvalidHostError)
  - [func (e InvalidHostError) Error() string](#InvalidHostError.Error)
- [type URL](#URL)
  - [func Parse(rawurl string) (*URL, error)](#Parse)
  - [func ParseRequestURI(rawurl string) (*URL, error)](#ParseRequestURI)
  - [func (u *URL) EscapedPath() string](#URL.EscapedPath)
  - [func (u *URL) Hostname() string](#URL.Hostname)
  - [func (u *URL) IsAbs() bool](#URL.IsAbs)
  - [func (u *URL) MarshalBinary() (text []byte, err error)](#URL.MarshalBinary)
  - [func (u *URL) Parse(ref string) (*URL, error)](#URL.Parse)
  - [func (u *URL) Port() string](#URL.Port)
  - [func (u *URL) Query() Values](#URL.Query)
  - [func (u *URL) RequestURI() string](#URL.RequestURI)
  - [func (u *URL) ResolveReference(ref *URL) *URL](#URL.ResolveReference)
  - [func (u *URL) String() string](#URL.String)
  - [func (u *URL) UnmarshalBinary(text []byte) error](#URL.UnmarshalBinary)
- [type Userinfo](#Userinfo)
  - [func User(username string) *Userinfo](#User)
  - [func UserPassword(username, password string) *Userinfo](#UserPassword)
  - [func (u *Userinfo) Password() (string, bool)](#Userinfo.Password)
  - [func (u *Userinfo) String() string](#Userinfo.String)
  - [func (u *Userinfo) Username() string](#Userinfo.Username)
- [type Values](#Values)
  - [func ParseQuery(query string) (Values, error)](#ParseQuery)
  - [func (v Values) Add(key, value string)](#Values.Add)
  - [func (v Values) Del(key string)](#Values.Del)
  - [func (v Values) Encode() string](#Values.Encode)
  - [func (v Values) Get(key string) string](#Values.Get)
  - [func (v Values) Set(key, value string)](#Values.Set)

### Examples

- [ParseQuery](#exampleParseQuery)
- [URL](#exampleURL)
- [URL.Hostname](#exampleURL_Hostname)
- [URL.RequestURI](#exampleURL_RequestURI)
- [URL.ResolveReference](#exampleURL_ResolveReference)
- [URL (Roundtrip)](#exampleURL_roundtrip)
- [Values](#exampleValues)

### Package files
 [url.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go)

<h2 id="PathEscape">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L260">PathEscape</a>
    <a href="#PathEscape">¶</a></h2>
<pre>func PathEscape(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

PathEscape escapes the string so it can be safely placed inside a URL path
segment.

<h2 id="PathUnescape">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L168">PathUnescape</a>
    <a href="#PathUnescape">¶</a></h2>
<pre>func PathUnescape(s <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

PathUnescape does the inverse transformation of PathEscape, converting %AB into
the byte 0xAB. It returns an error if any % is not followed by two hexadecimal
digits.

PathUnescape is identical to QueryUnescape except that it does not unescape '+'
to ' ' (space).

<h2 id="QueryEscape">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L254">QueryEscape</a>
    <a href="#QueryEscape">¶</a></h2>
<pre>func QueryEscape(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

QueryEscape escapes the string so it can be safely placed inside a URL query.

<h2 id="QueryUnescape">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L159">QueryUnescape</a>
    <a href="#QueryUnescape">¶</a></h2>
<pre>func QueryUnescape(s <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

QueryUnescape does the inverse transformation of QueryEscape, converting %AB
into the byte 0xAB and '+' into ' ' (space). It returns an error if any % is not
followed by two hexadecimal digits.

<h2 id="Error">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L13">Error</a>
    <a href="#Error">¶</a></h2>
<pre>type Error struct {
<span id="Error.Op"></span>    Op  <a href="/builtin/#string">string</a>
<span id="Error.URL"></span>    URL <a href="/builtin/#string">string</a>
<span id="Error.Err"></span>    Err <a href="/builtin/#error">error</a>
}</pre>

Error reports an error and the operation and URL that caused it.

<h3 id="Error.Error">func (*Error) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L19">Error</a>
    <a href="#Error.Error">¶</a></h3>
<pre>func (e *<a href="#Error">Error</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="Error.Temporary">func (*Error) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L34">Temporary</a>
    <a href="#Error.Temporary">¶</a></h3>
<pre>func (e *<a href="#Error">Error</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="Error.Timeout">func (*Error) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L25">Timeout</a>
    <a href="#Error.Timeout">¶</a></h3>
<pre>func (e *<a href="#Error">Error</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="EscapeError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L75">EscapeError</a>
    <a href="#EscapeError">¶</a></h2>
<pre>type EscapeError <a href="/builtin/#string">string</a></pre>


<h3 id="EscapeError.Error">func (EscapeError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L77">Error</a>
    <a href="#EscapeError.Error">¶</a></h3>
<pre>func (e <a href="#EscapeError">EscapeError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="InvalidHostError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L81">InvalidHostError</a>
    <a href="#InvalidHostError">¶</a></h2>
<pre>type InvalidHostError <a href="/builtin/#string">string</a></pre>


<h3 id="InvalidHostError.Error">func (InvalidHostError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L83">Error</a>
    <a href="#InvalidHostError.Error">¶</a></h3>
<pre>func (e <a href="#InvalidHostError">InvalidHostError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="URL">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L318">URL</a>
    <a href="#URL">¶</a></h2>
<pre>type URL struct {
<span id="URL.Scheme"></span>    Scheme     <a href="/builtin/#string">string</a>
<span id="URL.Opaque"></span>    Opaque     <a href="/builtin/#string">string</a>    <span class="comment">// encoded opaque data</span>
<span id="URL.User"></span>    User       *<a href="#Userinfo">Userinfo</a> <span class="comment">// username and password information</span>
<span id="URL.Host"></span>    Host       <a href="/builtin/#string">string</a>    <span class="comment">// host or host:port</span>
<span id="URL.Path"></span>    Path       <a href="/builtin/#string">string</a>    <span class="comment">// path (relative paths may omit leading slash)</span>
<span id="URL.RawPath"></span>    RawPath    <a href="/builtin/#string">string</a>    <span class="comment">// encoded path hint (see EscapedPath method)</span>
<span id="URL.ForceQuery"></span>    ForceQuery <a href="/builtin/#bool">bool</a>      <span class="comment">// append a query (&#39;?&#39;) even if RawQuery is empty</span>
<span id="URL.RawQuery"></span>    RawQuery   <a href="/builtin/#string">string</a>    <span class="comment">// encoded query values, without &#39;?&#39;</span>
<span id="URL.Fragment"></span>    Fragment   <a href="/builtin/#string">string</a>    <span class="comment">// fragment for references, without &#39;#&#39;</span>
}</pre>

A URL represents a parsed URL (technically, a URI reference).

The general form represented is:

    [scheme:][//[userinfo@]host][/]path[?query][#fragment]

URLs that do not start with a slash after the scheme are interpreted as:

    scheme:opaque[?query][#fragment]

Note that the Path field is stored in decoded form: /%47%6f%2f becomes /Go/. A
consequence is that it is impossible to tell which slashes in the Path were
slashes in the raw URL and which were %2f. This distinction is rarely important,
but when it is, code must not use Path directly. The Parse function sets both
Path and RawPath in the URL it returns, and URL's String method uses RawPath if
it is a valid encoding of Path, by calling the EscapedPath method.

<a id="exampleURL"></a>
Example:

    u, err := url.Parse("http://bing.com/search?q=dotnet")
    if err != nil {
        log.Fatal(err)
    }
    u.Scheme = "https"
    u.Host = "google.com"
    q := u.Query()
    q.Set("q", "golang")
    u.RawQuery = q.Encode()
    fmt.Println(u)
    // Output: https://google.com/search?q=golang


<a id="exampleURL_roundtrip"></a>
Example:

    // Parse + String preserve the original encoding.
    u, err := url.Parse("https://example.com/foo%2fbar")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(u.Path)
    fmt.Println(u.RawPath)
    fmt.Println(u.String())
    // Output:
    // /foo/bar
    // /foo%2fbar
    // https://example.com/foo%2fbar

<h3 id="Parse">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L421">Parse</a>
    <a href="#Parse">¶</a></h3>
<pre>func Parse(rawurl <a href="/builtin/#string">string</a>) (*<a href="#URL">URL</a>, <a href="/builtin/#error">error</a>)</pre>

Parse parses rawurl into a URL structure. The rawurl may be relative or
absolute.

<h3 id="ParseRequestURI">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L442">ParseRequestURI</a>
    <a href="#ParseRequestURI">¶</a></h3>
<pre>func ParseRequestURI(rawurl <a href="/builtin/#string">string</a>) (*<a href="#URL">URL</a>, <a href="/builtin/#error">error</a>)</pre>

ParseRequestURI parses rawurl into a URL structure. It assumes that rawurl was
received in an HTTP request, so the rawurl is interpreted only as an absolute
URI or an absolute path. The string rawurl is assumed not to have a #fragment
suffix. (Web browsers strip #fragment before sending the URL to a web server.)

<h3 id="URL.EscapedPath">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L634">EscapedPath</a>
    <a href="#URL.EscapedPath">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) EscapedPath() <a href="/builtin/#string">string</a></pre>

EscapedPath returns the escaped form of u.Path. In general there are multiple
possible escaped forms of any path. EscapedPath returns u.RawPath when it is a
valid escaping of u.Path. Otherwise EscapedPath ignores u.RawPath and computes
an escaped form on its own. The String and RequestURI methods use EscapedPath to
construct their results. In general, code should call EscapedPath instead of
reading u.RawPath directly.

<h3 id="URL.Hostname">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L994">Hostname</a>
    <a href="#URL.Hostname">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) Hostname() <a href="/builtin/#string">string</a></pre>

Hostname returns u.Host, without any port number.

If Host is an IPv6 literal with a port number, Hostname returns the IPv6 literal
without the square brackets. IPv6 literals may include a zone identifier.

<a id="exampleURL_Hostname"></a>
Example:

    u, err := url.Parse("https://example.org:8000/path")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(u.Hostname())
    u, err = url.Parse("https://[2001:0db8:85a3:0000:0000:8a2e:0370:7334]:17000")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(u.Hostname())
    // Output:
    // example.org
    // 2001:0db8:85a3:0000:0000:8a2e:0370:7334

<h3 id="URL.IsAbs">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L907">IsAbs</a>
    <a href="#URL.IsAbs">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) IsAbs() <a href="/builtin/#bool">bool</a></pre>

IsAbs reports whether the URL is absolute. Absolute means that it has a
non-empty scheme.

<h3 id="URL.MarshalBinary">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L1032">MarshalBinary</a>
    <a href="#URL.MarshalBinary">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) MarshalBinary() (text []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="URL.Parse">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L914">Parse</a>
    <a href="#URL.Parse">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) Parse(ref <a href="/builtin/#string">string</a>) (*<a href="#URL">URL</a>, <a href="/builtin/#error">error</a>)</pre>

Parse parses a URL in the context of the receiver. The provided URL may be
relative or absolute. Parse returns nil, err on parse failure, otherwise its
return value is the same as ResolveReference.

<h3 id="URL.Port">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L1000">Port</a>
    <a href="#URL.Port">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) Port() <a href="/builtin/#string">string</a></pre>

Port returns the port part of u.Host, without the leading colon. If u.Host
doesn't contain a port, Port returns an empty string.

<h3 id="URL.Query">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L964">Query</a>
    <a href="#URL.Query">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) Query() <a href="#Values">Values</a></pre>

Query parses RawQuery and returns the corresponding values. It silently discards
malformed value pairs. To check errors use ParseQuery.

<h3 id="URL.RequestURI">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L971">RequestURI</a>
    <a href="#URL.RequestURI">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) RequestURI() <a href="/builtin/#string">string</a></pre>

RequestURI returns the encoded path?query or opaque?query string that would be
used in an HTTP request for u.

<a id="exampleURL_RequestURI"></a>
Example:

    u, err := url.Parse("https://example.org/path?foo=bar")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(u.RequestURI())
    // Output: /path?foo=bar

<h3 id="URL.ResolveReference">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L928">ResolveReference</a>
    <a href="#URL.ResolveReference">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) ResolveReference(ref *<a href="#URL">URL</a>) *<a href="#URL">URL</a></pre>

ResolveReference resolves a URI reference to an absolute URI from an absolute
base URI, per RFC 3986 Section 5.2. The URI reference may be relative or
absolute. ResolveReference always returns a new URL instance, even if the
returned URL is identical to either the base or reference. If ref is an absolute
URL, then ResolveReference ignores base and returns a copy of ref.

<a id="exampleURL_ResolveReference"></a>
Example:

    u, err := url.Parse("../../..//search?q=dotnet")
    if err != nil {
        log.Fatal(err)
    }
    base, err := url.Parse("http://example.com/directory/")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(base.ResolveReference(u))
    // Output:
    // http://example.com/search?q=dotnet

<h3 id="URL.String">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L709">String</a>
    <a href="#URL.String">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) String() <a href="/builtin/#string">string</a></pre>

String reassembles the URL into a valid URL string. The general form of the
result is one of:

    scheme:opaque?query#fragment
    scheme://userinfo@host/path?query#fragment

If u.Opaque is non-empty, String uses the first form; otherwise it uses the
second form. To obtain the path, String uses u.EscapedPath().

In the second form, the following rules apply:

    - if u.Scheme is empty, scheme: is omitted.
    - if u.User is nil, userinfo@ is omitted.
    - if u.Host is empty, host/ is omitted.
    - if u.Scheme and u.Host are empty and u.User is nil,
       the entire scheme://userinfo@host/ is omitted.
    - if u.Host is non-empty and u.Path begins with a /,
       the form host/path does not add its own /.
    - if u.RawQuery is empty, ?query is omitted.
    - if u.Fragment is empty, #fragment is omitted.

<h3 id="URL.UnmarshalBinary">func (*URL) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L1036">UnmarshalBinary</a>
    <a href="#URL.UnmarshalBinary">¶</a></h3>
<pre>func (u *<a href="#URL">URL</a>) UnmarshalBinary(text []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>


<h2 id="Userinfo">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L352">Userinfo</a>
    <a href="#Userinfo">¶</a></h2>
<pre>type Userinfo struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

The Userinfo type is an immutable encapsulation of username and password details
for a URL. An existing Userinfo value is guaranteed to have a username set
(potentially empty, as allowed by RFC 2396), and optionally a password.

<h3 id="User">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L332">User</a>
    <a href="#User">¶</a></h3>
<pre>func User(username <a href="/builtin/#string">string</a>) *<a href="#Userinfo">Userinfo</a></pre>

User returns a Userinfo containing the provided username and no password set.

<h3 id="UserPassword">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L344">UserPassword</a>
    <a href="#UserPassword">¶</a></h3>
<pre>func UserPassword(username, password <a href="/builtin/#string">string</a>) *<a href="#Userinfo">Userinfo</a></pre>

UserPassword returns a Userinfo containing the provided username and password.

This functionality should only be used with legacy web sites. RFC 2396 warns
that interpreting Userinfo this way ``is NOT RECOMMENDED, because the passing of
authentication information in clear text (such as URI) has proven to be a
security risk in almost every case where it has been used.''

<h3 id="Userinfo.Password">func (*Userinfo) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L364">Password</a>
    <a href="#Userinfo.Password">¶</a></h3>
<pre>func (u *<a href="#Userinfo">Userinfo</a>) Password() (<a href="/builtin/#string">string</a>, <a href="/builtin/#bool">bool</a>)</pre>

Password returns the password in case it is set, and whether it is set.

<h3 id="Userinfo.String">func (*Userinfo) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L370">String</a>
    <a href="#Userinfo.String">¶</a></h3>
<pre>func (u *<a href="#Userinfo">Userinfo</a>) String() <a href="/builtin/#string">string</a></pre>

String returns the encoded userinfo information in the standard form of
"username[:password]".

<h3 id="Userinfo.Username">func (*Userinfo) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L359">Username</a>
    <a href="#Userinfo.Username">¶</a></h3>
<pre>func (u *<a href="#Userinfo">Userinfo</a>) Username() <a href="/builtin/#string">string</a></pre>

Username returns the username.

<h2 id="Values">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L760">Values</a>
    <a href="#Values">¶</a></h2>
<pre>type Values map[<a href="/builtin/#string">string</a>][]<a href="/builtin/#string">string</a></pre>

Values maps a string key to a list of values. It is typically used for query
parameters and form values. Unlike in the http.Header map, the keys in a Values
map are case-sensitive.

<a id="exampleValues"></a>
Example:

    v := url.Values{}
    v.Set("name", "Ava")
    v.Add("friend", "Jess")
    v.Add("friend", "Sarah")
    v.Add("friend", "Zoe")
    // v.Encode() == "name=Ava&friend=Jess&friend=Sarah&friend=Zoe"
    fmt.Println(v.Get("name"))
    fmt.Println(v.Get("friend"))
    fmt.Println(v["friend"])
    // Output:
    // Ava
    // Jess
    // [Jess Sarah Zoe]

<h3 id="ParseQuery">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L803">ParseQuery</a>
    <a href="#ParseQuery">¶</a></h3>
<pre>func ParseQuery(query <a href="/builtin/#string">string</a>) (<a href="#Values">Values</a>, <a href="/builtin/#error">error</a>)</pre>

ParseQuery parses the URL-encoded query string and returns a map listing the
values specified for each key. ParseQuery always returns a non-nil map
containing all the valid query parameters found; err describes the first
decoding error encountered, if any.

Query is expected to be a list of key=value settings separated by ampersands or
semicolons. A setting without an equals sign is interpreted as a key set to an
empty value.

<a id="exampleParseQuery"></a>
Example:

    m, err := url.ParseQuery(`x=1&y=2&y=3;z`)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(toJSON(m))
    // Output:
    // {"x":["1"], "y":["2", "3"], "z":[""]}

<h3 id="Values.Add">func (Values) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L785">Add</a>
    <a href="#Values.Add">¶</a></h3>
<pre>func (v <a href="#Values">Values</a>) Add(key, value <a href="/builtin/#string">string</a>)</pre>

Add adds the value to key. It appends to any existing values associated with
key.

<h3 id="Values.Del">func (Values) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L790">Del</a>
    <a href="#Values.Del">¶</a></h3>
<pre>func (v <a href="#Values">Values</a>) Del(key <a href="/builtin/#string">string</a>)</pre>

Del deletes the values associated with key.

<h3 id="Values.Encode">func (Values) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L845">Encode</a>
    <a href="#Values.Encode">¶</a></h3>
<pre>func (v <a href="#Values">Values</a>) Encode() <a href="/builtin/#string">string</a></pre>

Encode encodes the values into ``URL encoded'' form ("bar=baz&foo=quux") sorted
by key.

<h3 id="Values.Get">func (Values) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L766">Get</a>
    <a href="#Values.Get">¶</a></h3>
<pre>func (v <a href="#Values">Values</a>) Get(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Get gets the first value associated with the given key. If there are no values
associated with the key, Get returns the empty string. To access multiple
values, use the map directly.

<h3 id="Values.Set">func (Values) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/url/url.go#L779">Set</a>
    <a href="#Values.Set">¶</a></h3>
<pre>func (v <a href="#Values">Values</a>) Set(key, value <a href="/builtin/#string">string</a>)</pre>

Set sets the key to value. It replaces any existing values.


