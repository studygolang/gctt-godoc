version: 1.9.2
## package quick

  `import "testing/quick"`

## Overview

Package quick implements utility functions to help with black box testing.

The testing/quick package is frozen and is not accepting new features.

## Index

- [func Check(f interface{}, config *Config) error](#Check)
- [func CheckEqual(f, g interface{}, config *Config) error](#CheckEqual)
- [func Value(t reflect.Type, rand *rand.Rand) (value reflect.Value, ok bool)](#Value)
- [type CheckEqualError](#CheckEqualError)
  - [func (s *CheckEqualError) Error() string](#CheckEqualError.Error)
- [type CheckError](#CheckError)
  - [func (s *CheckError) Error() string](#CheckError.Error)
- [type Config](#Config)
- [type Generator](#Generator)
- [type SetupError](#SetupError)
  - [func (s SetupError) Error() string](#SetupError.Error)

### Package files
 [quick.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go)

<h2 id="Check">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L252">Check</a>
    <a href="#Check">¶</a></h2>
<pre>func Check(f interface{}, config *<a href="#Config">Config</a>) <a href="/builtin/#error">error</a></pre>

Check looks for an input to f, any function that returns bool, such that f
returns false. It calls f repeatedly, with arbitrary values for each argument.
If f returns false on a given input, Check returns that input as a *CheckError.
For example:

    func TestOddMultipleOfThree(t *testing.T) {
    	f := func(x int) bool {
    		y := OddMultipleOfThree(x)
    		return y%2 == 1 && y%3 == 0
    	}
    	if err := quick.Check(f, nil); err != nil {
    		t.Error(err)
    	}
    }

<h2 id="CheckEqual">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L291">CheckEqual</a>
    <a href="#CheckEqual">¶</a></h2>
<pre>func CheckEqual(f, g interface{}, config *<a href="#Config">Config</a>) <a href="/builtin/#error">error</a></pre>

CheckEqual looks for an input on which f and g return different results. It
calls f and g repeatedly with arbitrary values for each argument. If f and g
return different answers, CheckEqual returns a *CheckEqualError describing the
input and the outputs.

<h2 id="Value">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L49">Value</a>
    <a href="#Value">¶</a></h2>
<pre>func Value(t <a href="/reflect/">reflect</a>.<a href="/reflect/#Type">Type</a>, rand *<a href="/math/rand/">rand</a>.<a href="/math/rand/#Rand">Rand</a>) (value <a href="/reflect/">reflect</a>.<a href="/reflect/#Value">Value</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

Value returns an arbitrary value of the given type. If the type implements the
Generator interface, that will be used. Note: To create arbitrary values for
structs, all the fields must be exported.

<h2 id="CheckEqualError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L227">CheckEqualError</a>
    <a href="#CheckEqualError">¶</a></h2>
<pre>type CheckEqualError struct {
    <a href="#CheckError">CheckError</a>
<span id="CheckEqualError.Out1"></span>    Out1 []interface{}
<span id="CheckEqualError.Out2"></span>    Out2 []interface{}
}</pre>

A CheckEqualError is the result CheckEqual finding an error.

<h3 id="CheckEqualError.Error">func (*CheckEqualError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L233">Error</a>
    <a href="#CheckEqualError.Error">¶</a></h3>
<pre>func (s *<a href="#CheckEqualError">CheckEqualError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="CheckError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L217">CheckError</a>
    <a href="#CheckError">¶</a></h2>
<pre>type CheckError struct {
<span id="CheckError.Count"></span>    Count <a href="/builtin/#int">int</a>
<span id="CheckError.In"></span>    In    []interface{}
}</pre>

A CheckError is the result of Check finding an error.

<h3 id="CheckError.Error">func (*CheckError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L222">Error</a>
    <a href="#CheckError.Error">¶</a></h3>
<pre>func (s *<a href="#CheckError">CheckError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Config">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L167">Config</a>
    <a href="#Config">¶</a></h2>
<pre>type Config struct {
<span id="Config.MaxCount"></span>    <span class="comment">// MaxCount sets the maximum number of iterations.</span>
    <span class="comment">// If zero, MaxCountScale is used.</span>
    MaxCount <a href="/builtin/#int">int</a>
<span id="Config.MaxCountScale"></span>    <span class="comment">// MaxCountScale is a non-negative scale factor applied to the</span>
    <span class="comment">// default maximum.</span>
    <span class="comment">// If zero, the default is unchanged.</span>
    MaxCountScale <a href="/builtin/#float64">float64</a>
<span id="Config.Rand"></span>    <span class="comment">// Rand specifies a source of random numbers.</span>
    <span class="comment">// If nil, a default pseudo-random source will be used.</span>
    Rand *<a href="/math/rand/">rand</a>.<a href="/math/rand/#Rand">Rand</a>
<span id="Config.Values"></span>    <span class="comment">// Values specifies a function to generate a slice of</span>
    <span class="comment">// arbitrary reflect.Values that are congruent with the</span>
    <span class="comment">// arguments to the function being tested.</span>
    <span class="comment">// If nil, the top-level Value function is used to generate them.</span>
    Values func([]<a href="/reflect/">reflect</a>.<a href="/reflect/#Value">Value</a>, *<a href="/math/rand/">rand</a>.<a href="/math/rand/#Rand">Rand</a>)
}</pre>

A Config structure contains options for running a test.

<h2 id="Generator">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L13">Generator</a>
    <a href="#Generator">¶</a></h2>
<pre>type Generator interface {
    <span class="comment">// Generate returns a random instance of the type on which it is a</span>
    <span class="comment">// method using the size as a size hint.</span>
    Generate(rand *<a href="/math/rand/">rand</a>.<a href="/math/rand/#Rand">Rand</a>, size <a href="/builtin/#int">int</a>) <a href="/reflect/">reflect</a>.<a href="/reflect/#Value">Value</a>
}</pre>

A Generator can generate random values of its own type.

<h2 id="SetupError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L212">SetupError</a>
    <a href="#SetupError">¶</a></h2>
<pre>type SetupError <a href="/builtin/#string">string</a></pre>

A SetupError is the result of an error in the way that check is being used,
independent of the functions being tested.

<h3 id="SetupError.Error">func (SetupError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/quick/quick.go#L214">Error</a>
    <a href="#SetupError.Error">¶</a></h3>
<pre>func (s <a href="#SetupError">SetupError</a>) Error() <a href="/builtin/#string">string</a></pre>



