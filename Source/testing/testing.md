version: 1.10
## package testing

  `import "testing"`

## Overview

Package testing provides support for automated testing of Go packages. It is
intended to be used in concert with the ``go test'' command, which automates
execution of any function of the form

    func TestXxx(*testing.T)

where Xxx does not start with a lowercase letter. The function name serves to
identify the test routine.

Within these functions, use the Error, Fail or related methods to signal
failure.

To write a new test suite, create a file whose name ends _test.go that contains
the TestXxx functions as described here. Put the file in the same package as the
one being tested. The file will be excluded from regular package builds but will
be included when the ``go test'' command is run. For more detail, run ``go help
test'' and ``go help testflag''.

Tests and benchmarks may be skipped if not applicable with a call to the Skip
method of *T and *B:

    func TestTimeConsuming(t *testing.T) {
        if testing.Short() {
            t.Skip("skipping test in short mode.")
        }
        ...
    }


Benchmarks

Functions of the form

    func BenchmarkXxx(*testing.B)

are considered benchmarks, and are executed by the "go test" command when its
-bench flag is provided. Benchmarks are run sequentially.

For a description of the testing flags, see
https://golang.org/cmd/go/#hdr-Description_of_testing_flags.

A sample benchmark function looks like this:

    func BenchmarkHello(b *testing.B) {
        for i := 0; i < b.N; i++ {
            fmt.Sprintf("hello")
        }
    }

The benchmark function must run the target code b.N times. During benchmark
execution, b.N is adjusted until the benchmark function lasts long enough to be
timed reliably. The output

    BenchmarkHello    10000000    282 ns/op

means that the loop ran 10000000 times at a speed of 282 ns per loop.

If a benchmark needs some expensive setup before running, the timer may be
reset:

    func BenchmarkBigLen(b *testing.B) {
        big := NewBig()
        b.ResetTimer()
        for i := 0; i < b.N; i++ {
            big.Len()
        }
    }

If a benchmark needs to test performance in a parallel setting, it may use the
RunParallel helper function; such benchmarks are intended to be used with the go
test -cpu flag:

    func BenchmarkTemplateParallel(b *testing.B) {
        templ := template.Must(template.New("test").Parse("Hello, {{.}}!"))
        b.RunParallel(func(pb *testing.PB) {
            var buf bytes.Buffer
            for pb.Next() {
                buf.Reset()
                templ.Execute(&buf, "World")
            }
        })
    }


Examples

The package also runs and verifies example code. Example functions may include a
concluding line comment that begins with "Output:" and is compared with the
standard output of the function when the tests are run. (The comparison ignores
leading and trailing space.) These are examples of an example:

    func ExampleHello() {
        fmt.Println("hello")
        // Output: hello
    }

    func ExampleSalutations() {
        fmt.Println("hello, and")
        fmt.Println("goodbye")
        // Output:
        // hello, and
        // goodbye
    }

The comment prefix "Unordered output:" is like "Output:", but matches any line
order:

    func ExamplePerm() {
        for _, value := range Perm(4) {
            fmt.Println(value)
        }
        // Unordered output: 4
        // 2
        // 1
        // 3
        // 0
    }

Example functions without output comments are compiled but not executed.

The naming convention to declare examples for the package, a function F, a type
T and method M on type T are:

    func Example() { ... }
    func ExampleF() { ... }
    func ExampleT() { ... }
    func ExampleT_M() { ... }

Multiple example functions for a package/type/function/method may be provided by
appending a distinct suffix to the name. The suffix must start with a lower-case
letter.

    func Example_suffix() { ... }
    func ExampleF_suffix() { ... }
    func ExampleT_suffix() { ... }
    func ExampleT_M_suffix() { ... }

The entire test file is presented as the example when it contains a single
example function, at least one other function, type, variable, or constant
declaration, and no test or benchmark functions.


Subtests and Sub-benchmarks

The Run methods of T and B allow defining subtests and sub-benchmarks, without
having to define separate functions for each. This enables uses like
table-driven benchmarks and creating hierarchical tests. It also provides a way
to share common setup and tear-down code:

    func TestFoo(t *testing.T) {
        // <setup code>
        t.Run("A=1", func(t *testing.T) { ... })
        t.Run("A=2", func(t *testing.T) { ... })
        t.Run("B=1", func(t *testing.T) { ... })
        // <tear-down code>
    }

Each subtest and sub-benchmark has a unique name: the combination of the name of
the top-level test and the sequence of names passed to Run, separated by
slashes, with an optional trailing sequence number for disambiguation.

The argument to the -run and -bench command-line flags is an unanchored regular
expression that matches the test's name. For tests with multiple slash-separated
elements, such as subtests, the argument is itself slash-separated, with
expressions matching each name element in turn. Because it is unanchored, an
empty expression matches any string. For example, using "matching" to mean
"whose name contains":

    go test -run ''      # Run all tests.
    go test -run Foo     # Run top-level tests matching "Foo", such as "TestFooBar".
    go test -run Foo/A=  # For top-level tests matching "Foo", run subtests matching "A=".
    go test -run /A=1    # For all top-level tests, run subtests matching "A=1".

Subtests can also be used to control parallelism. A parent test will only
complete once all of its subtests complete. In this example, all tests are run
in parallel with each other, and only with each other, regardless of other
top-level tests that may be defined:

    func TestGroupedParallel(t *testing.T) {
        for _, tc := range tests {
            tc := tc // capture range variable
            t.Run(tc.Name, func(t *testing.T) {
                t.Parallel()
                ...
            })
        }
    }

Run does not return until parallel subtests have completed, providing a way to
clean up after a group of parallel tests:

    func TestTeardownParallel(t *testing.T) {
        // This Run will not return until the parallel tests finish.
        t.Run("group", func(t *testing.T) {
            t.Run("Test1", parallelTest1)
            t.Run("Test2", parallelTest2)
            t.Run("Test3", parallelTest3)
        })
        // <tear-down code>
    }


Main

It is sometimes necessary for a test program to do extra setup or teardown
before or after testing. It is also sometimes necessary for a test to control
which code runs on the main thread. To support these and other cases, if a test
file contains a function:

    func TestMain(m *testing.M)

then the generated test will call TestMain(m) instead of running the tests
directly. TestMain runs in the main goroutine and can do whatever setup and
teardown is necessary around a call to m.Run. It should then call os.Exit with
the result of m.Run. When TestMain is called, flag.Parse has not been run. If
TestMain depends on command-line flags, including those of the testing package,
it should call flag.Parse explicitly.

A simple implementation of TestMain is:

    func TestMain(m *testing.M) {
    	// call flag.Parse() here if TestMain uses flags
    	os.Exit(m.Run())
    }

## Index

- [func AllocsPerRun(runs int, f func()) (avg float64)](#AllocsPerRun)
- [func CoverMode() string](#CoverMode)
- [func Coverage() float64](#Coverage)
- [func Main(matchString func(pat, str string) (bool, error), tests []InternalTest, benchmarks []InternalBenchmark, examples []InternalExample)](#Main)
- [func RegisterCover(c Cover)](#RegisterCover)
- [func RunBenchmarks(matchString func(pat, str string) (bool, error), benchmarks []InternalBenchmark)](#RunBenchmarks)
- [func RunExamples(matchString func(pat, str string) (bool, error), examples []InternalExample) (ok bool)](#RunExamples)
- [func RunTests(matchString func(pat, str string) (bool, error), tests []InternalTest) (ok bool)](#RunTests)
- [func Short() bool](#Short)
- [func Verbose() bool](#Verbose)
- [type B](#B)
  - [func (c *B) Error(args ...interface{})](#B.Error)
  - [func (c *B) Errorf(format string, args ...interface{})](#B.Errorf)
  - [func (c *B) Fail()](#B.Fail)
  - [func (c *B) FailNow()](#B.FailNow)
  - [func (c *B) Failed() bool](#B.Failed)
  - [func (c *B) Fatal(args ...interface{})](#B.Fatal)
  - [func (c *B) Fatalf(format string, args ...interface{})](#B.Fatalf)
  - [func (c *B) Helper()](#B.Helper)
  - [func (c *B) Log(args ...interface{})](#B.Log)
  - [func (c *B) Logf(format string, args ...interface{})](#B.Logf)
  - [func (c *B) Name() string](#B.Name)
  - [func (b *B) ReportAllocs()](#B.ReportAllocs)
  - [func (b *B) ResetTimer()](#B.ResetTimer)
  - [func (b *B) Run(name string, f func(b *B)) bool](#B.Run)
  - [func (b *B) RunParallel(body func(*PB))](#B.RunParallel)
  - [func (b *B) SetBytes(n int64)](#B.SetBytes)
  - [func (b *B) SetParallelism(p int)](#B.SetParallelism)
  - [func (c *B) Skip(args ...interface{})](#B.Skip)
  - [func (c *B) SkipNow()](#B.SkipNow)
  - [func (c *B) Skipf(format string, args ...interface{})](#B.Skipf)
  - [func (c *B) Skipped() bool](#B.Skipped)
  - [func (b *B) StartTimer()](#B.StartTimer)
  - [func (b *B) StopTimer()](#B.StopTimer)
- [type BenchmarkResult](#BenchmarkResult)
  - [func Benchmark(f func(b *B)) BenchmarkResult](#Benchmark)
  - [func (r BenchmarkResult) AllocedBytesPerOp() int64](#BenchmarkResult.AllocedBytesPerOp)
  - [func (r BenchmarkResult) AllocsPerOp() int64](#BenchmarkResult.AllocsPerOp)
  - [func (r BenchmarkResult) MemString() string](#BenchmarkResult.MemString)
  - [func (r BenchmarkResult) NsPerOp() int64](#BenchmarkResult.NsPerOp)
  - [func (r BenchmarkResult) String() string](#BenchmarkResult.String)
- [type Cover](#Cover)
- [type CoverBlock](#CoverBlock)
- [type InternalBenchmark](#InternalBenchmark)
- [type InternalExample](#InternalExample)
- [type InternalTest](#InternalTest)
- [type M](#M)
  - [func MainStart(deps testDeps, tests []InternalTest, benchmarks []InternalBenchmark, examples []InternalExample) *M](#MainStart)
  - [func (m *M) Run() int](#M.Run)
- [type PB](#PB)
  - [func (pb *PB) Next() bool](#PB.Next)
- [type T](#T)
  - [func (c *T) Error(args ...interface{})](#T.Error)
  - [func (c *T) Errorf(format string, args ...interface{})](#T.Errorf)
  - [func (c *T) Fail()](#T.Fail)
  - [func (c *T) FailNow()](#T.FailNow)
  - [func (c *T) Failed() bool](#T.Failed)
  - [func (c *T) Fatal(args ...interface{})](#T.Fatal)
  - [func (c *T) Fatalf(format string, args ...interface{})](#T.Fatalf)
  - [func (c *T) Helper()](#T.Helper)
  - [func (c *T) Log(args ...interface{})](#T.Log)
  - [func (c *T) Logf(format string, args ...interface{})](#T.Logf)
  - [func (c *T) Name() string](#T.Name)
  - [func (t *T) Parallel()](#T.Parallel)
  - [func (t *T) Run(name string, f func(t *T)) bool](#T.Run)
  - [func (c *T) Skip(args ...interface{})](#T.Skip)
  - [func (c *T) SkipNow()](#T.SkipNow)
  - [func (c *T) Skipf(format string, args ...interface{})](#T.Skipf)
  - [func (c *T) Skipped() bool](#T.Skipped)
- [type TB](#TB)

### Examples

- [B.RunParallel](#exampleB_RunParallel)

### Package files
 [allocs.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/allocs.go) [benchmark.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go) [cover.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/cover.go) [example.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/example.go) [match.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/match.go) [testing.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go)

<h2 id="AllocsPerRun">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/allocs.go#L10">AllocsPerRun</a>
    <a href="#AllocsPerRun">¶</a></h2>
<pre>func AllocsPerRun(runs <a href="/builtin/#int">int</a>, f func()) (avg <a href="/builtin/#float64">float64</a>)</pre>

AllocsPerRun returns the average number of allocations during calls to f.
Although the return value has type float64, it will always be an integral value.

To compute the number of allocations, the function will first be run once as a
warm-up. The average number of allocations over the specified number of runs
will then be measured and returned.

AllocsPerRun sets GOMAXPROCS to 1 during its measurement and will restore it
before returning.

<h2 id="CoverMode">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L307">CoverMode</a>
    <a href="#CoverMode">¶</a></h2>
<pre>func CoverMode() <a href="/builtin/#string">string</a></pre>

CoverMode reports what the test coverage mode is set to. The values are "set",
"count", or "atomic". The return value will be empty if test coverage is not
enabled.

<h2 id="Coverage">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/cover.go#L35">Coverage</a>
    <a href="#Coverage">¶</a></h2>
<pre>func Coverage() <a href="/builtin/#float64">float64</a></pre>

Coverage reports the current code coverage as a fraction in the range [0, 1]. If
coverage is not enabled, Coverage returns 0.

When running a large set of sequential test cases, checking Coverage after each
one can be useful for identifying which test cases exercise new code paths. It
is not a replacement for the reports generated by 'go test -cover' and 'go tool
cover'.

<h2 id="Main">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L894">Main</a>
    <a href="#Main">¶</a></h2>
<pre>func Main(matchString func(pat, str <a href="/builtin/#string">string</a>) (<a href="/builtin/#bool">bool</a>, <a href="/builtin/#error">error</a>), tests []<a href="#InternalTest">InternalTest</a>, benchmarks []<a href="#InternalBenchmark">InternalBenchmark</a>, examples []<a href="#InternalExample">InternalExample</a>)</pre>

Main is an internal function, part of the implementation of the "go test"
command. It was exported because it is cross-package and predates "internal"
packages. It is no longer used by "go test" but preserved, as much as possible,
for other systems that simulate "go test" using Main, but Main sometimes cannot
be updated as new functionality is added to the testing package. Systems
simulating "go test" should be updated to use MainStart.

<h2 id="RegisterCover">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/cover.go#L54">RegisterCover</a>
    <a href="#RegisterCover">¶</a></h2>
<pre>func RegisterCover(c <a href="#Cover">Cover</a>)</pre>

RegisterCover records the coverage data accumulators for the tests. NOTE: This
function is internal to the testing infrastructure and may change. It is not
covered (yet) by the Go 1 compatibility guidelines.

<h2 id="RunBenchmarks">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L367">RunBenchmarks</a>
    <a href="#RunBenchmarks">¶</a></h2>
<pre>func RunBenchmarks(matchString func(pat, str <a href="/builtin/#string">string</a>) (<a href="/builtin/#bool">bool</a>, <a href="/builtin/#error">error</a>), benchmarks []<a href="#InternalBenchmark">InternalBenchmark</a>)</pre>

An internal function but exported because it is cross-package; part of the
implementation of the "go test" command.

<h2 id="RunExamples">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/example.go#L16">RunExamples</a>
    <a href="#RunExamples">¶</a></h2>
<pre>func RunExamples(matchString func(pat, str <a href="/builtin/#string">string</a>) (<a href="/builtin/#bool">bool</a>, <a href="/builtin/#error">error</a>), examples []<a href="#InternalExample">InternalExample</a>) (ok <a href="/builtin/#bool">bool</a>)</pre>

An internal function but exported because it is cross-package; part of the
implementation of the "go test" command.

<h2 id="RunTests">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L1025">RunTests</a>
    <a href="#RunTests">¶</a></h2>
<pre>func RunTests(matchString func(pat, str <a href="/builtin/#string">string</a>) (<a href="/builtin/#bool">bool</a>, <a href="/builtin/#error">error</a>), tests []<a href="#InternalTest">InternalTest</a>) (ok <a href="/builtin/#bool">bool</a>)</pre>

An internal function but exported because it is cross-package; part of the
implementation of the "go test" command.

<h2 id="Short">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L300">Short</a>
    <a href="#Short">¶</a></h2>
<pre>func Short() <a href="/builtin/#bool">bool</a></pre>

Short reports whether the -test.short flag is set.

<h2 id="Verbose">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L312">Verbose</a>
    <a href="#Verbose">¶</a></h2>
<pre>func Verbose() <a href="/builtin/#bool">bool</a></pre>

Verbose reports whether the -test.v flag is set.

<h2 id="B">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L38">B</a>
    <a href="#B">¶</a></h2>
<pre>type B struct {
<span id="B.N"></span>    N <a href="/builtin/#int">int</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

B is a type passed to Benchmark functions to manage benchmark timing and to
specify the number of iterations to run.

A benchmark ends when its Benchmark function returns or calls any of the methods
FailNow, Fatal, Fatalf, SkipNow, Skip, or Skipf. Those methods must be called
only from the goroutine running the Benchmark function. The other reporting
methods, such as the variations of Log and Error, may be called simultaneously
from multiple goroutines.

Like in tests, benchmark logs are accumulated during execution and dumped to
standard error when done. Unlike in tests, benchmark logs are always printed, so
as not to hide output whose existence may be affecting benchmark results.

<h3 id="B.Error">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L567">Error</a>
    <a href="#B.Error">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Error(args ...interface{})</pre>

Error is equivalent to Log followed by Fail.

<h3 id="B.Errorf">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L573">Errorf</a>
    <a href="#B.Errorf">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Errorf(format <a href="/builtin/#string">string</a>, args ...interface{})</pre>

Errorf is equivalent to Logf followed by Fail.

<h3 id="B.Fail">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L491">Fail</a>
    <a href="#B.Fail">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Fail()</pre>

Fail marks the function as having failed but continues execution.

<h3 id="B.FailNow">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L520">FailNow</a>
    <a href="#B.FailNow">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) FailNow()</pre>

FailNow marks the function as having failed and stops its execution by calling
runtime.Goexit (which then runs all deferred calls in the current goroutine).
Execution will continue at the next test or benchmark. FailNow must be called
from the goroutine running the test or benchmark function, not from other
goroutines created during the test. Calling FailNow does not stop those other
goroutines.

<h3 id="B.Failed">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L505">Failed</a>
    <a href="#B.Failed">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Failed() <a href="/builtin/#bool">bool</a></pre>

Failed reports whether the function has failed.

<h3 id="B.Fatal">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L579">Fatal</a>
    <a href="#B.Fatal">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Fatal(args ...interface{})</pre>

Fatal is equivalent to Log followed by FailNow.

<h3 id="B.Fatalf">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L585">Fatalf</a>
    <a href="#B.Fatalf">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Fatalf(format <a href="/builtin/#string">string</a>, args ...interface{})</pre>

Fatalf is equivalent to Logf followed by FailNow.

<h3 id="B.Helper">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L634">Helper</a>
    <a href="#B.Helper">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Helper()</pre>

Helper marks the calling function as a test helper function. When printing file
and line information, that function will be skipped. Helper may be called
simultaneously from multiple goroutines. Helper has no effect if it is called
directly from a TestXxx/BenchmarkXxx function or a subtest/sub-benchmark
function.

<h3 id="B.Log">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L557">Log</a>
    <a href="#B.Log">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Log(args ...interface{})</pre>

Log formats its arguments using default formatting, analogous to Println, and
records the text in the error log. For tests, the text will be printed only if
the test fails or the -test.v flag is set. For benchmarks, the text is always
printed to avoid having performance depend on the value of the -test.v flag.

<h3 id="B.Logf">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L564">Logf</a>
    <a href="#B.Logf">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Logf(format <a href="/builtin/#string">string</a>, args ...interface{})</pre>

Logf formats its arguments according to the format, analogous to Printf, and
records the text in the error log. A final newline is added if not provided. For
tests, the text will be printed only if the test fails or the -test.v flag is
set. For benchmarks, the text is always printed to avoid having performance
depend on the value of the -test.v flag.

<h3 id="B.Name">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L477">Name</a>
    <a href="#B.Name">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Name() <a href="/builtin/#string">string</a></pre>

Name returns the name of the running test or benchmark.

<h3 id="B.ReportAllocs">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L108">ReportAllocs</a>
    <a href="#B.ReportAllocs">¶</a></h3>
<pre>func (b *<a href="#B">B</a>) ReportAllocs()</pre>

ReportAllocs enables malloc statistics for this benchmark. It is equivalent to
setting -test.benchmem, but it only affects the benchmark function that calls
ReportAllocs.

<h3 id="B.ResetTimer">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L89">ResetTimer</a>
    <a href="#B.ResetTimer">¶</a></h3>
<pre>func (b *<a href="#B">B</a>) ResetTimer()</pre>

ResetTimer zeros the elapsed benchmark time and memory allocation counters. It
does not affect whether the timer is running.

<h3 id="B.Run">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L468">Run</a>
    <a href="#B.Run">¶</a></h3>
<pre>func (b *<a href="#B">B</a>) Run(name <a href="/builtin/#string">string</a>, f func(b *<a href="#B">B</a>)) <a href="/builtin/#bool">bool</a></pre>

Run benchmarks f as a subbenchmark with the given name. It reports whether there
were any failures.

A subbenchmark is like any other benchmark. A benchmark that calls Run at least
once will not be measured itself and will be called once with N=1.

<h3 id="B.RunParallel">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L581">RunParallel</a>
    <a href="#B.RunParallel">¶</a></h3>
<pre>func (b *<a href="#B">B</a>) RunParallel(body func(*<a href="#PB">PB</a>))</pre>

RunParallel runs a benchmark in parallel. It creates multiple goroutines and
distributes b.N iterations among them. The number of goroutines defaults to
GOMAXPROCS. To increase parallelism for non-CPU-bound benchmarks, call
SetParallelism before RunParallel. RunParallel is usually used with the go test
-cpu flag.

The body function will be run in each goroutine. It should set up any
goroutine-local state and then iterate until pb.Next returns false. It should
not use the StartTimer, StopTimer, or ResetTimer functions, because they have
global effect. It should also not call Run.

<a id="exampleB_RunParallel"></a>
Example:

    // Parallel benchmark for text/template.Template.Execute on a single object.
    testing.Benchmark(func(b *testing.B) {
        templ := template.Must(template.New("test").Parse("Hello, {{.}}!"))
        // RunParallel will create GOMAXPROCS goroutines
        // and distribute work among them.
        b.RunParallel(func(pb *testing.PB) {
            // Each goroutine has its own bytes.Buffer.
            var buf bytes.Buffer
            for pb.Next() {
                // The loop body is executed b.N times total across all goroutines.
                buf.Reset()
                templ.Execute(&buf, "World")
            }
        })
    })

<h3 id="B.SetBytes">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L103">SetBytes</a>
    <a href="#B.SetBytes">¶</a></h3>
<pre>func (b *<a href="#B">B</a>) SetBytes(n <a href="/builtin/#int64">int64</a>)</pre>

SetBytes records the number of bytes processed in a single operation. If this is
called, the benchmark will report ns/op and MB/s.

<h3 id="B.SetParallelism">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L625">SetParallelism</a>
    <a href="#B.SetParallelism">¶</a></h3>
<pre>func (b *<a href="#B">B</a>) SetParallelism(p <a href="/builtin/#int">int</a>)</pre>

SetParallelism sets the number of goroutines used by RunParallel to
p*GOMAXPROCS. There is usually no need to call SetParallelism for CPU-bound
benchmarks. If p is less than 1, this call will have no effect.

<h3 id="B.Skip">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L591">Skip</a>
    <a href="#B.Skip">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Skip(args ...interface{})</pre>

Skip is equivalent to Log followed by SkipNow.

<h3 id="B.SkipNow">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L610">SkipNow</a>
    <a href="#B.SkipNow">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) SkipNow()</pre>

SkipNow marks the test as having been skipped and stops its execution by calling
runtime.Goexit. If a test fails (see Error, Errorf, Fail) and is then skipped,
it is still considered to have failed. Execution will continue at the next test
or benchmark. See also FailNow. SkipNow must be called from the goroutine
running the test, not from other goroutines created during the test. Calling
SkipNow does not stop those other goroutines.

<h3 id="B.Skipf">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L597">Skipf</a>
    <a href="#B.Skipf">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Skipf(format <a href="/builtin/#string">string</a>, args ...interface{})</pre>

Skipf is equivalent to Logf followed by SkipNow.

<h3 id="B.Skipped">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L623">Skipped</a>
    <a href="#B.Skipped">¶</a></h3>
<pre>func (c *<a href="#B">B</a>) Skipped() <a href="/builtin/#bool">bool</a></pre>

Skipped reports whether the test was skipped.

<h3 id="B.StartTimer">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L64">StartTimer</a>
    <a href="#B.StartTimer">¶</a></h3>
<pre>func (b *<a href="#B">B</a>) StartTimer()</pre>

StartTimer starts timing a test. This function is called automatically before a
benchmark starts, but it can also used to resume timing after a call to
StopTimer.

<h3 id="B.StopTimer">func (*B) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L77">StopTimer</a>
    <a href="#B.StopTimer">¶</a></h3>
<pre>func (b *<a href="#B">B</a>) StopTimer()</pre>

StopTimer stops timing a test. This can be used to pause the timer while
performing complex initialization that you don't want to measure.

<h2 id="BenchmarkResult">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L286">BenchmarkResult</a>
    <a href="#BenchmarkResult">¶</a></h2>
<pre>type BenchmarkResult struct {
<span id="BenchmarkResult.N"></span>    N         <a href="/builtin/#int">int</a>           <span class="comment">// The number of iterations.</span>
<span id="BenchmarkResult.T"></span>    T         <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a> <span class="comment">// The total time taken.</span>
<span id="BenchmarkResult.Bytes"></span>    Bytes     <a href="/builtin/#int64">int64</a>         <span class="comment">// Bytes processed in one iteration.</span>
<span id="BenchmarkResult.MemAllocs"></span>    MemAllocs <a href="/builtin/#uint64">uint64</a>        <span class="comment">// The total number of memory allocations.</span>
<span id="BenchmarkResult.MemBytes"></span>    MemBytes  <a href="/builtin/#uint64">uint64</a>        <span class="comment">// The total number of bytes allocated.</span>
}</pre>

The results of a benchmark run.

<h3 id="Benchmark">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L636">Benchmark</a>
    <a href="#Benchmark">¶</a></h3>
<pre>func Benchmark(f func(b *<a href="#B">B</a>)) <a href="#BenchmarkResult">BenchmarkResult</a></pre>

Benchmark benchmarks a single function. Useful for creating custom benchmarks
that do not use the "go test" command.

If f calls Run, the result will be an estimate of running all its subbenchmarks
that don't call Run in sequence in a single benchmark.

<h3 id="BenchmarkResult.AllocedBytesPerOp">func (BenchmarkResult) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L317">AllocedBytesPerOp</a>
    <a href="#BenchmarkResult.AllocedBytesPerOp">¶</a></h3>
<pre>func (r <a href="#BenchmarkResult">BenchmarkResult</a>) AllocedBytesPerOp() <a href="/builtin/#int64">int64</a></pre>

AllocedBytesPerOp returns r.MemBytes / r.N.

<h3 id="BenchmarkResult.AllocsPerOp">func (BenchmarkResult) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L309">AllocsPerOp</a>
    <a href="#BenchmarkResult.AllocsPerOp">¶</a></h3>
<pre>func (r <a href="#BenchmarkResult">BenchmarkResult</a>) AllocsPerOp() <a href="/builtin/#int64">int64</a></pre>

AllocsPerOp returns r.MemAllocs / r.N.

<h3 id="BenchmarkResult.MemString">func (BenchmarkResult) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L345">MemString</a>
    <a href="#BenchmarkResult.MemString">¶</a></h3>
<pre>func (r <a href="#BenchmarkResult">BenchmarkResult</a>) MemString() <a href="/builtin/#string">string</a></pre>

MemString returns r.AllocedBytesPerOp and r.AllocsPerOp in the same format as
'go test'.

<h3 id="BenchmarkResult.NsPerOp">func (BenchmarkResult) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L294">NsPerOp</a>
    <a href="#BenchmarkResult.NsPerOp">¶</a></h3>
<pre>func (r <a href="#BenchmarkResult">BenchmarkResult</a>) NsPerOp() <a href="/builtin/#int64">int64</a></pre>


<h3 id="BenchmarkResult.String">func (BenchmarkResult) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L324">String</a>
    <a href="#BenchmarkResult.String">¶</a></h3>
<pre>func (r <a href="#BenchmarkResult">BenchmarkResult</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Cover">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/cover.go#L21">Cover</a>
    <a href="#Cover">¶</a></h2>
<pre>type Cover struct {
<span id="Cover.Mode"></span>    Mode            <a href="/builtin/#string">string</a>
<span id="Cover.Counters"></span>    Counters        map[<a href="/builtin/#string">string</a>][]<a href="/builtin/#uint32">uint32</a>
<span id="Cover.Blocks"></span>    Blocks          map[<a href="/builtin/#string">string</a>][]<a href="#CoverBlock">CoverBlock</a>
<span id="Cover.CoveredPackages"></span>    CoveredPackages <a href="/builtin/#string">string</a>
}</pre>

Cover records information about test coverage checking. NOTE: This struct is
internal to the testing infrastructure and may change. It is not covered (yet)
by the Go 1 compatibility guidelines.

<h2 id="CoverBlock">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/cover.go#L8">CoverBlock</a>
    <a href="#CoverBlock">¶</a></h2>
<pre>type CoverBlock struct {
<span id="CoverBlock.Line0"></span>    Line0 <a href="/builtin/#uint32">uint32</a>
<span id="CoverBlock.Col0"></span>    Col0  <a href="/builtin/#uint16">uint16</a>
<span id="CoverBlock.Line1"></span>    Line1 <a href="/builtin/#uint32">uint32</a>
<span id="CoverBlock.Col1"></span>    Col1  <a href="/builtin/#uint16">uint16</a>
<span id="CoverBlock.Stmts"></span>    Stmts <a href="/builtin/#uint16">uint16</a>
}</pre>

CoverBlock records the coverage data for a single basic block. NOTE: This struct
is internal to the testing infrastructure and may change. It is not covered
(yet) by the Go 1 compatibility guidelines.

<h2 id="InternalBenchmark">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L20">InternalBenchmark</a>
    <a href="#InternalBenchmark">¶</a></h2>
<pre>type InternalBenchmark struct {
<span id="InternalBenchmark.Name"></span>    Name <a href="/builtin/#string">string</a>
<span id="InternalBenchmark.F"></span>    F    func(b *<a href="#B">B</a>)
}</pre>

An internal type but exported because it is cross-package; part of the
implementation of the "go test" command.

<h2 id="InternalExample">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/example.go#L7">InternalExample</a>
    <a href="#InternalExample">¶</a></h2>
<pre>type InternalExample struct {
<span id="InternalExample.Name"></span>    Name      <a href="/builtin/#string">string</a>
<span id="InternalExample.F"></span>    F         func()
<span id="InternalExample.Output"></span>    Output    <a href="/builtin/#string">string</a>
<span id="InternalExample.Unordered"></span>    Unordered <a href="/builtin/#bool">bool</a>
}</pre>


<h2 id="InternalTest">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L706">InternalTest</a>
    <a href="#InternalTest">¶</a></h2>
<pre>type InternalTest struct {
<span id="InternalTest.Name"></span>    Name <a href="/builtin/#string">string</a>
<span id="InternalTest.F"></span>    F    func(*<a href="#T">T</a>)
}</pre>

An internal type but exported because it is cross-package; part of the
implementation of the "go test" command.

<h2 id="M">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L899">M</a>
    <a href="#M">¶</a></h2>
<pre>type M struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

M is a type passed to a TestMain function to run the actual tests.

<h3 id="MainStart">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L929">MainStart</a>
    <a href="#MainStart">¶</a></h3>
<pre>func MainStart(deps testDeps, tests []<a href="#InternalTest">InternalTest</a>, benchmarks []<a href="#InternalBenchmark">InternalBenchmark</a>, examples []<a href="#InternalExample">InternalExample</a>) *<a href="#M">M</a></pre>

MainStart is meant for use by tests generated by 'go test'. It is not meant to
be called directly and is not subject to the Go 1 compatibility document. It may
change signature from release to release.

<h3 id="M.Run">func (*M) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L939">Run</a>
    <a href="#M.Run">¶</a></h3>
<pre>func (m *<a href="#M">M</a>) Run() <a href="/builtin/#int">int</a></pre>

Run runs the tests. It returns an exit code to pass to os.Exit.

<h2 id="PB">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L548">PB</a>
    <a href="#PB">¶</a></h2>
<pre>type PB struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A PB is used by RunParallel for running parallel benchmarks.

<h3 id="PB.Next">func (*PB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/benchmark.go#L556">Next</a>
    <a href="#PB.Next">¶</a></h3>
<pre>func (pb *<a href="#PB">PB</a>) Next() <a href="/builtin/#bool">bool</a></pre>

Next reports whether there are more iterations to execute.

<h2 id="T">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L468">T</a>
    <a href="#T">¶</a></h2>
<pre>type T struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

T is a type passed to Test functions to manage test state and support formatted
test logs. Logs are accumulated during execution and dumped to standard output
when done.

A test ends when its Test function returns or calls any of the methods FailNow,
Fatal, Fatalf, SkipNow, Skip, or Skipf. Those methods, as well as the Parallel
method, must be called only from the goroutine running the Test function.

The other reporting methods, such as the variations of Log and Error, may be
called simultaneously from multiple goroutines.

<h3 id="T.Error">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L567">Error</a>
    <a href="#T.Error">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Error(args ...interface{})</pre>

Error is equivalent to Log followed by Fail.

<h3 id="T.Errorf">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L573">Errorf</a>
    <a href="#T.Errorf">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Errorf(format <a href="/builtin/#string">string</a>, args ...interface{})</pre>

Errorf is equivalent to Logf followed by Fail.

<h3 id="T.Fail">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L491">Fail</a>
    <a href="#T.Fail">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Fail()</pre>

Fail marks the function as having failed but continues execution.

<h3 id="T.FailNow">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L520">FailNow</a>
    <a href="#T.FailNow">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) FailNow()</pre>

FailNow marks the function as having failed and stops its execution by calling
runtime.Goexit (which then runs all deferred calls in the current goroutine).
Execution will continue at the next test or benchmark. FailNow must be called
from the goroutine running the test or benchmark function, not from other
goroutines created during the test. Calling FailNow does not stop those other
goroutines.

<h3 id="T.Failed">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L505">Failed</a>
    <a href="#T.Failed">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Failed() <a href="/builtin/#bool">bool</a></pre>

Failed reports whether the function has failed.

<h3 id="T.Fatal">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L579">Fatal</a>
    <a href="#T.Fatal">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Fatal(args ...interface{})</pre>

Fatal is equivalent to Log followed by FailNow.

<h3 id="T.Fatalf">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L585">Fatalf</a>
    <a href="#T.Fatalf">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Fatalf(format <a href="/builtin/#string">string</a>, args ...interface{})</pre>

Fatalf is equivalent to Logf followed by FailNow.

<h3 id="T.Helper">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L634">Helper</a>
    <a href="#T.Helper">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Helper()</pre>

Helper marks the calling function as a test helper function. When printing file
and line information, that function will be skipped. Helper may be called
simultaneously from multiple goroutines. Helper has no effect if it is called
directly from a TestXxx/BenchmarkXxx function or a subtest/sub-benchmark
function.

<h3 id="T.Log">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L557">Log</a>
    <a href="#T.Log">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Log(args ...interface{})</pre>

Log formats its arguments using default formatting, analogous to Println, and
records the text in the error log. For tests, the text will be printed only if
the test fails or the -test.v flag is set. For benchmarks, the text is always
printed to avoid having performance depend on the value of the -test.v flag.

<h3 id="T.Logf">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L564">Logf</a>
    <a href="#T.Logf">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Logf(format <a href="/builtin/#string">string</a>, args ...interface{})</pre>

Logf formats its arguments according to the format, analogous to Printf, and
records the text in the error log. A final newline is added if not provided. For
tests, the text will be printed only if the test fails or the -test.v flag is
set. For benchmarks, the text is always printed to avoid having performance
depend on the value of the -test.v flag.

<h3 id="T.Name">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L477">Name</a>
    <a href="#T.Name">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Name() <a href="/builtin/#string">string</a></pre>

Name returns the name of the running test or benchmark.

<h3 id="T.Parallel">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L661">Parallel</a>
    <a href="#T.Parallel">¶</a></h3>
<pre>func (t *<a href="#T">T</a>) Parallel()</pre>

Parallel signals that this test is to be run in parallel with (and only with)
other parallel tests. When a test is run multiple times due to use of
-test.count or -test.cpu, multiple instances of a single test never run in
parallel with each other.

<h3 id="T.Run">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L781">Run</a>
    <a href="#T.Run">¶</a></h3>
<pre>func (t *<a href="#T">T</a>) Run(name <a href="/builtin/#string">string</a>, f func(t *<a href="#T">T</a>)) <a href="/builtin/#bool">bool</a></pre>

Run runs f as a subtest of t called name. It runs f in a separate goroutine and
blocks until f returns or calls t.Parallel to become a parallel test. Run
reports whether f succeeded (or at least did not fail before calling
t.Parallel).

Run may be called simultaneously from multiple goroutines, but all such calls
must return before the outer test function for t returns.

<h3 id="T.Skip">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L591">Skip</a>
    <a href="#T.Skip">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Skip(args ...interface{})</pre>

Skip is equivalent to Log followed by SkipNow.

<h3 id="T.SkipNow">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L610">SkipNow</a>
    <a href="#T.SkipNow">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) SkipNow()</pre>

SkipNow marks the test as having been skipped and stops its execution by calling
runtime.Goexit. If a test fails (see Error, Errorf, Fail) and is then skipped,
it is still considered to have failed. Execution will continue at the next test
or benchmark. See also FailNow. SkipNow must be called from the goroutine
running the test, not from other goroutines created during the test. Calling
SkipNow does not stop those other goroutines.

<h3 id="T.Skipf">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L597">Skipf</a>
    <a href="#T.Skipf">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Skipf(format <a href="/builtin/#string">string</a>, args ...interface{})</pre>

Skipf is equivalent to Logf followed by SkipNow.

<h3 id="T.Skipped">func (*T) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L623">Skipped</a>
    <a href="#T.Skipped">¶</a></h3>
<pre>func (c *<a href="#T">T</a>) Skipped() <a href="/builtin/#bool">bool</a></pre>

Skipped reports whether the test was skipped.

<h2 id="TB">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/testing/testing.go#L432">TB</a>
    <a href="#TB">¶</a></h2>
<pre>type TB interface {
    Error(args ...interface{})
    Errorf(format <a href="/builtin/#string">string</a>, args ...interface{})
    Fail()
    FailNow()
    Failed() <a href="/builtin/#bool">bool</a>
    Fatal(args ...interface{})
    Fatalf(format <a href="/builtin/#string">string</a>, args ...interface{})
    Log(args ...interface{})
    Logf(format <a href="/builtin/#string">string</a>, args ...interface{})
    Name() <a href="/builtin/#string">string</a>
    Skip(args ...interface{})
    SkipNow()
    Skipf(format <a href="/builtin/#string">string</a>, args ...interface{})
    Skipped() <a href="/builtin/#bool">bool</a>
    Helper()
    <span class="comment">// contains filtered or unexported methods</span>
}</pre>

TB is the interface common to T and B.

## Subdirectories
- [..](..)
- [iotest](iotest/)
- [quick](quick/)
