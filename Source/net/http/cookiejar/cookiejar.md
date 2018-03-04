version: 1.10
## package cookiejar

  `import "net/http/cookiejar"`

## Overview

Package cookiejar implements an in-memory RFC 6265-compliant http.CookieJar.

## Index

- [type Jar](#Jar)
  - [func New(o *Options) (*Jar, error)](#New)
  - [func (j *Jar) Cookies(u *url.URL) (cookies []*http.Cookie)](#Jar.Cookies)
  - [func (j *Jar) SetCookies(u *url.URL, cookies []*http.Cookie)](#Jar.SetCookies)
- [type Options](#Options)
- [type PublicSuffixList](#PublicSuffixList)

### Examples

- [New](#exampleNew)

### Package files
 [jar.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookiejar/jar.go) [punycode.go](//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookiejar/punycode.go)

<h2 id="Jar">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookiejar/jar.go#L50">Jar</a>
    <a href="#Jar">¶</a></h2>
<pre>type Jar struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Jar implements the http.CookieJar interface from the net/http package.

<h3 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookiejar/jar.go#L67">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(o *<a href="#Options">Options</a>) (*<a href="#Jar">Jar</a>, <a href="/builtin/#error">error</a>)</pre>

New returns a new cookie jar. A nil *Options is equivalent to a zero Options.

<a id="exampleNew"></a>
Example:

    // Start a server to give us cookies.
    ts := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        if cookie, err := r.Cookie("Flavor"); err != nil {
            http.SetCookie(w, &http.Cookie{Name: "Flavor", Value: "Chocolate Chip"})
        } else {
            cookie.Value = "Oatmeal Raisin"
            http.SetCookie(w, cookie)
        }
    }))
    defer ts.Close()

    u, err := url.Parse(ts.URL)
    if err != nil {
        log.Fatal(err)
    }

    // All users of cookiejar should import "golang.org/x/net/publicsuffix"
    jar, err := cookiejar.New(&cookiejar.Options{PublicSuffixList: publicsuffix.List})
    if err != nil {
        log.Fatal(err)
    }

    client := &http.Client{
        Jar: jar,
    }

    if _, err = client.Get(u.String()); err != nil {
        log.Fatal(err)
    }

    fmt.Println("After 1st request:")
    for _, cookie := range jar.Cookies(u) {
        fmt.Printf("  %s: %s\n", cookie.Name, cookie.Value)
    }

    if _, err = client.Get(u.String()); err != nil {
        log.Fatal(err)
    }

    fmt.Println("After 2nd request:")
    for _, cookie := range jar.Cookies(u) {
        fmt.Printf("  %s: %s\n", cookie.Name, cookie.Value)
    }
    // Output:
    // After 1st request:
    //   Flavor: Chocolate Chip
    // After 2nd request:
    //   Flavor: Oatmeal Raisin

<h3 id="Jar.Cookies">func (*Jar) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookiejar/jar.go#L143">Cookies</a>
    <a href="#Jar.Cookies">¶</a></h3>
<pre>func (j *<a href="#Jar">Jar</a>) Cookies(u *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>) (cookies []*<a href="/net/http/">http</a>.<a href="/net/http/#Cookie">Cookie</a>)</pre>

Cookies implements the Cookies method of the http.CookieJar interface.

It returns an empty slice if the URL's scheme is not HTTP or HTTPS.

<h3 id="Jar.SetCookies">func (*Jar) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookiejar/jar.go#L218">SetCookies</a>
    <a href="#Jar.SetCookies">¶</a></h3>
<pre>func (j *<a href="#Jar">Jar</a>) SetCookies(u *<a href="/net/url/">url</a>.<a href="/net/url/#URL">URL</a>, cookies []*<a href="/net/http/">http</a>.<a href="/net/http/#Cookie">Cookie</a>)</pre>

SetCookies implements the SetCookies method of the http.CookieJar interface.

It does nothing if the URL's scheme is not HTTP or HTTPS.

<h2 id="Options">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookiejar/jar.go#L39">Options</a>
    <a href="#Options">¶</a></h2>
<pre>type Options struct {
<span id="Options.PublicSuffixList"></span>    <span class="comment">// PublicSuffixList is the public suffix list that determines whether</span>
    <span class="comment">// an HTTP server can set a cookie for a domain.</span>
    <span class="comment">//</span>
    <span class="comment">// A nil value is valid and may be useful for testing but it is not</span>
    <span class="comment">// secure: it means that the HTTP server for foo.co.uk can set a cookie</span>
    <span class="comment">// for bar.co.uk.</span>
    PublicSuffixList <a href="#PublicSuffixList">PublicSuffixList</a>
}</pre>

Options are the options for creating a new Jar.

<h2 id="PublicSuffixList">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/net/http/cookiejar/jar.go#L24">PublicSuffixList</a>
    <a href="#PublicSuffixList">¶</a></h2>
<pre>type PublicSuffixList interface {
    <span class="comment">// PublicSuffix returns the public suffix of domain.</span>
    <span class="comment">//</span>
    <span class="comment">// TODO: specify which of the caller and callee is responsible for IP</span>
    <span class="comment">// addresses, for leading and trailing dots, for case sensitivity, and</span>
    <span class="comment">// for IDN/Punycode.</span>
    PublicSuffix(domain <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a>

    <span class="comment">// String returns a description of the source of this public suffix</span>
    <span class="comment">// list. The description will typically contain something like a time</span>
    <span class="comment">// stamp or version number.</span>
    String() <a href="/builtin/#string">string</a>
}</pre>

PublicSuffixList provides the public suffix of a domain. For example:

    - the public suffix of "example.com" is "com",
    - the public suffix of "foo1.foo2.foo3.co.uk" is "co.uk", and
    - the public suffix of "bar.pvt.k12.ma.us" is "pvt.k12.ma.us".

Implementations of PublicSuffixList must be safe for concurrent use by multiple
goroutines.

An implementation that always returns "" is valid and may be useful for testing
but it is not secure: it means that the HTTP server for foo.com can set a cookie
for bar.com.

A public suffix list implementation is in the package
golang.org/x/net/publicsuffix.


