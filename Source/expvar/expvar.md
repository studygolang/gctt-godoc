version: 1.10
## package expvar

  `import "expvar"`

## Overview

Package expvar provides a standardized interface to public variables, such as
operation counters in servers. It exposes these variables via HTTP at
/debug/vars in JSON format.

Operations to set or modify these public variables are atomic.

In addition to adding the HTTP handler, this package registers the following
variables:

    cmdline   os.Args
    memstats  runtime.Memstats

The package is sometimes only imported for the side effect of registering its
HTTP handler and the above variables. To use it this way, link this package into
your program:

    import _ "expvar"

## Index

- [func Do(f func(KeyValue))](#Do)
- [func Handler() http.Handler](#Handler)
- [func Publish(name string, v Var)](#Publish)
- [type Float](#Float)
  - [func NewFloat(name string) *Float](#NewFloat)
  - [func (v *Float) Add(delta float64)](#Float.Add)
  - [func (v *Float) Set(value float64)](#Float.Set)
  - [func (v *Float) String() string](#Float.String)
  - [func (v *Float) Value() float64](#Float.Value)
- [type Func](#Func)
  - [func (f Func) String() string](#Func.String)
  - [func (f Func) Value() interface{}](#Func.Value)
- [type Int](#Int)
  - [func NewInt(name string) *Int](#NewInt)
  - [func (v *Int) Add(delta int64)](#Int.Add)
  - [func (v *Int) Set(value int64)](#Int.Set)
  - [func (v *Int) String() string](#Int.String)
  - [func (v *Int) Value() int64](#Int.Value)
- [type KeyValue](#KeyValue)
- [type Map](#Map)
  - [func NewMap(name string) *Map](#NewMap)
  - [func (v *Map) Add(key string, delta int64)](#Map.Add)
  - [func (v *Map) AddFloat(key string, delta float64)](#Map.AddFloat)
  - [func (v *Map) Do(f func(KeyValue))](#Map.Do)
  - [func (v *Map) Get(key string) Var](#Map.Get)
  - [func (v *Map) Init() *Map](#Map.Init)
  - [func (v *Map) Set(key string, av Var)](#Map.Set)
  - [func (v *Map) String() string](#Map.String)
- [type String](#String)
  - [func NewString(name string) *String](#NewString)
  - [func (v *String) Set(value string)](#String.Set)
  - [func (v *String) String() string](#String.String)
  - [func (v *String) Value() string](#String.Value)
- [type Var](#Var)
  - [func Get(name string) Var](#Get)

### Package files
 [expvar.go](//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go)

<h2 id="Do">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L296">Do</a>
    <a href="#Do">¶</a></h2>
<pre>func Do(f func(<a href="#KeyValue">KeyValue</a>))</pre>

Do calls f for each exported variable. The global variable map is locked during
the iteration, but existing entries may be concurrently updated.

<h2 id="Handler">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L322">Handler</a>
    <a href="#Handler">¶</a></h2>
<pre>func Handler() <a href="/net/http/">http</a>.<a href="/net/http/#Handler">Handler</a></pre>

Handler returns the expvar HTTP Handler.

This is only needed to install the handler in a non-standard location.

<h2 id="Publish">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L249">Publish</a>
    <a href="#Publish">¶</a></h2>
<pre>func Publish(name <a href="/builtin/#string">string</a>, v <a href="#Var">Var</a>)</pre>

Publish declares a named exported variable. This should be called from a
package's init function when it creates its Vars. If the name is already
registered then this will log.Panic.

<h2 id="Float">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L59">Float</a>
    <a href="#Float">¶</a></h2>
<pre>type Float struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Float is a 64-bit float variable that satisfies the Var interface.

<h3 id="NewFloat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L275">NewFloat</a>
    <a href="#NewFloat">¶</a></h3>
<pre>func NewFloat(name <a href="/builtin/#string">string</a>) *<a href="#Float">Float</a></pre>


<h3 id="Float.Add">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L73">Add</a>
    <a href="#Float.Add">¶</a></h3>
<pre>func (v *<a href="#Float">Float</a>) Add(delta <a href="/builtin/#float64">float64</a>)</pre>

Add adds delta to v.

<h3 id="Float.Set">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L86">Set</a>
    <a href="#Float.Set">¶</a></h3>
<pre>func (v *<a href="#Float">Float</a>) Set(value <a href="/builtin/#float64">float64</a>)</pre>

Set sets v to value.

<h3 id="Float.String">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L67">String</a>
    <a href="#Float.String">¶</a></h3>
<pre>func (v *<a href="#Float">Float</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Float.Value">func (*Float) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L63">Value</a>
    <a href="#Float.Value">¶</a></h3>
<pre>func (v *<a href="#Float">Float</a>) Value() <a href="/builtin/#float64">float64</a></pre>


<h2 id="Func">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L228">Func</a>
    <a href="#Func">¶</a></h2>
<pre>type Func func() interface{}</pre>

Func implements Var by calling the function and formatting the returned value
using JSON.

<h3 id="Func.String">func (Func) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L234">String</a>
    <a href="#Func.String">¶</a></h3>
<pre>func (f <a href="#Func">Func</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Func.Value">func (Func) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L230">Value</a>
    <a href="#Func.Value">¶</a></h3>
<pre>func (f <a href="#Func">Func</a>) Value() interface{}</pre>


<h2 id="Int">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L38">Int</a>
    <a href="#Int">¶</a></h2>
<pre>type Int struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Int is a 64-bit integer variable that satisfies the Var interface.

<h3 id="NewInt">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L269">NewInt</a>
    <a href="#NewInt">¶</a></h3>
<pre>func NewInt(name <a href="/builtin/#string">string</a>) *<a href="#Int">Int</a></pre>


<h3 id="Int.Add">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L50">Add</a>
    <a href="#Int.Add">¶</a></h3>
<pre>func (v *<a href="#Int">Int</a>) Add(delta <a href="/builtin/#int64">int64</a>)</pre>


<h3 id="Int.Set">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L54">Set</a>
    <a href="#Int.Set">¶</a></h3>
<pre>func (v *<a href="#Int">Int</a>) Set(value <a href="/builtin/#int64">int64</a>)</pre>


<h3 id="Int.String">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L46">String</a>
    <a href="#Int.String">¶</a></h3>
<pre>func (v *<a href="#Int">Int</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Int.Value">func (*Int) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L42">Value</a>
    <a href="#Int.Value">¶</a></h3>
<pre>func (v *<a href="#Int">Int</a>) Value() <a href="/builtin/#int64">int64</a></pre>


<h2 id="KeyValue">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L98">KeyValue</a>
    <a href="#KeyValue">¶</a></h2>
<pre>type KeyValue struct {
<span id="KeyValue.Key"></span>    Key   <a href="/builtin/#string">string</a>
<span id="KeyValue.Value"></span>    Value <a href="#Var">Var</a>
}</pre>

KeyValue represents a single entry in a Map.

<h2 id="Map">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L91">Map</a>
    <a href="#Map">¶</a></h2>
<pre>type Map struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Map is a string-to-Var map variable that satisfies the Var interface.

<h3 id="NewMap">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L281">NewMap</a>
    <a href="#NewMap">¶</a></h3>
<pre>func NewMap(name <a href="/builtin/#string">string</a>) *<a href="#Map">Map</a></pre>


<h3 id="Map.Add">func (*Map) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L159">Add</a>
    <a href="#Map.Add">¶</a></h3>
<pre>func (v *<a href="#Map">Map</a>) Add(key <a href="/builtin/#string">string</a>, delta <a href="/builtin/#int64">int64</a>)</pre>

Add adds delta to the *Int value stored under the given map key.

<h3 id="Map.AddFloat">func (*Map) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L176">AddFloat</a>
    <a href="#Map.AddFloat">¶</a></h3>
<pre>func (v *<a href="#Map">Map</a>) AddFloat(key <a href="/builtin/#string">string</a>, delta <a href="/builtin/#float64">float64</a>)</pre>

AddFloat adds delta to the *Float value stored under the given map key.

<h3 id="Map.Do">func (*Map) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L195">Do</a>
    <a href="#Map.Do">¶</a></h3>
<pre>func (v *<a href="#Map">Map</a>) Do(f func(<a href="#KeyValue">KeyValue</a>))</pre>

Do calls f for each entry in the map. The map is locked during the iteration,
but existing entries may be concurrently updated.

<h3 id="Map.Get">func (*Map) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L138">Get</a>
    <a href="#Map.Get">¶</a></h3>
<pre>func (v *<a href="#Map">Map</a>) Get(key <a href="/builtin/#string">string</a>) <a href="#Var">Var</a></pre>


<h3 id="Map.Init">func (*Map) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L119">Init</a>
    <a href="#Map.Init">¶</a></h3>
<pre>func (v *<a href="#Map">Map</a>) Init() *<a href="#Map">Map</a></pre>

Init removes all keys from the map.

<h3 id="Map.Set">func (*Map) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L144">Set</a>
    <a href="#Map.Set">¶</a></h3>
<pre>func (v *<a href="#Map">Map</a>) Set(key <a href="/builtin/#string">string</a>, av <a href="#Var">Var</a>)</pre>


<h3 id="Map.String">func (*Map) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L103">String</a>
    <a href="#Map.String">¶</a></h3>
<pre>func (v *<a href="#Map">Map</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="String">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L205">String</a>
    <a href="#String">¶</a></h2>
<pre>type String struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

String is a string variable, and satisfies the Var interface.

<h3 id="NewString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L287">NewString</a>
    <a href="#NewString">¶</a></h3>
<pre>func NewString(name <a href="/builtin/#string">string</a>) *<a href="#String">String</a></pre>


<h3 id="String.Set">func (*String) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L222">Set</a>
    <a href="#String.Set">¶</a></h3>
<pre>func (v *<a href="#String">String</a>) Set(value <a href="/builtin/#string">string</a>)</pre>


<h3 id="String.String">func (*String) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L216">String</a>
    <a href="#String.String">¶</a></h3>
<pre>func (v *<a href="#String">String</a>) String() <a href="/builtin/#string">string</a></pre>

String implements the Val interface. To get the unquoted string use Value.

<h3 id="String.Value">func (*String) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L209">Value</a>
    <a href="#String.Value">¶</a></h3>
<pre>func (v *<a href="#String">String</a>) Value() <a href="/builtin/#string">string</a></pre>


<h2 id="Var">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L30">Var</a>
    <a href="#Var">¶</a></h2>
<pre>type Var interface {
    <span class="comment">// String returns a valid JSON value for the variable.</span>
    <span class="comment">// Types with String methods that do not return valid JSON</span>
    <span class="comment">// (such as time.Time) must not be used as a Var.</span>
    String() <a href="/builtin/#string">string</a>
}</pre>

Var is an abstract type for all exported variables.

<h3 id="Get">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/expvar/expvar.go#L261">Get</a>
    <a href="#Get">¶</a></h3>
<pre>func Get(name <a href="/builtin/#string">string</a>) <a href="#Var">Var</a></pre>

Get retrieves a named exported variable. It returns nil if the name has not been
registered.


