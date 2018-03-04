version: 1.10
## package plugin

  `import "plugin"`

## Overview

Package plugin implements loading and symbol resolution of Go plugins.

A plugin is a Go main package with exported functions and variables that has
been built with:

    go build -buildmode=plugin

When a plugin is first opened, the init functions of all packages not already
part of the program are called. The main function is not run. A plugin is only
initialized once, and cannot be closed.

Currently plugins are only supported on Linux and macOS. Please report any
issues.

## Index

- [type Plugin](#Plugin)
  - [func Open(path string) (*Plugin, error)](#Open)
  - [func (p *Plugin) Lookup(symName string) (Symbol, error)](#Plugin.Lookup)
- [type Symbol](#Symbol)

### Package files
 [plugin.go](//github.com/golang/go/blob/release-branch.go1.10/src/plugin/plugin.go) [plugin_dlopen.go](//github.com/golang/go/blob/release-branch.go1.10/src/plugin/plugin_dlopen.go)

<h2 id="Plugin">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/plugin/plugin.go#L11">Plugin</a>
    <a href="#Plugin">¶</a></h2>
<pre>type Plugin struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Plugin is a loaded Go plugin.

<h3 id="Open">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/plugin/plugin.go#L21">Open</a>
    <a href="#Open">¶</a></h3>
<pre>func Open(path <a href="/builtin/#string">string</a>) (*<a href="#Plugin">Plugin</a>, <a href="/builtin/#error">error</a>)</pre>

Open opens a Go plugin. If a path has already been opened, then the existing
*Plugin is returned. It is safe for concurrent use by multiple goroutines.

<h3 id="Plugin.Lookup">func (*Plugin) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/plugin/plugin.go#L29">Lookup</a>
    <a href="#Plugin.Lookup">¶</a></h3>
<pre>func (p *<a href="#Plugin">Plugin</a>) Lookup(symName <a href="/builtin/#string">string</a>) (<a href="#Symbol">Symbol</a>, <a href="/builtin/#error">error</a>)</pre>

Lookup searches for a symbol named symName in plugin p. A symbol is any exported
variable or function. It reports an error if the symbol is not found. It is safe
for concurrent use by multiple goroutines.

<h2 id="Symbol">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/plugin/plugin.go#L62">Symbol</a>
    <a href="#Symbol">¶</a></h2>
<pre>type Symbol interface{}</pre>

A Symbol is a pointer to a variable or function.

For example, a plugin defined as

    package main

    import "fmt"

    var V int

    func F() { fmt.Printf("Hello, number %d\n", V) }

may be loaded with the Open function and then the exported package symbols V and
F can be accessed

    p, err := plugin.Open("plugin_name.so")
    if err != nil {
    	panic(err)
    }
    v, err := p.Lookup("V")
    if err != nil {
    	panic(err)
    }
    f, err := p.Lookup("F")
    if err != nil {
    	panic(err)
    }
    *v.(*int) = 7
    f.(func())() // prints "Hello, number 7"


