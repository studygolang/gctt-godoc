version: 1.10
## package flag

  `import "flag"`

## Overview

Package flag implements command-line flag parsing.

Usage:

Define flags using flag.String(), Bool(), Int(), etc.

This declares an integer flag, -flagname, stored in the pointer ip, with type
*int.

    import "flag"
    var ip = flag.Int("flagname", 1234, "help message for flagname")

If you like, you can bind the flag to a variable using the Var() functions.

    var flagvar int
    func init() {
    	flag.IntVar(&flagvar, "flagname", 1234, "help message for flagname")
    }

Or you can create custom flags that satisfy the Value interface (with pointer
receivers) and couple them to flag parsing by

    flag.Var(&flagVal, "name", "help message for flagname")

For such flags, the default value is just the initial value of the variable.

After all flags are defined, call

    flag.Parse()

to parse the command line into the defined flags.

Flags may then be used directly. If you're using the flags themselves, they are
all pointers; if you bind to variables, they're values.

    fmt.Println("ip has value ", *ip)
    fmt.Println("flagvar has value ", flagvar)

After parsing, the arguments following the flags are available as the slice
flag.Args() or individually as flag.Arg(i). The arguments are indexed from 0
through flag.NArg()-1.

Command line flag syntax:

    -flag
    -flag=x
    -flag x  // non-boolean flags only

One or two minus signs may be used; they are equivalent. The last form is not
permitted for boolean flags because the meaning of the command

    cmd -x *

where * is a Unix shell wildcard, will change if there is a file called 0,
false, etc. You must use the -flag=false form to turn off a boolean flag.

Flag parsing stops just before the first non-flag argument ("-" is a non-flag
argument) or after the terminator "--".

Integer flags accept 1234, 0664, 0x1234 and may be negative. Boolean flags may
be:

    1, 0, t, f, T, F, true, false, TRUE, FALSE, True, False

Duration flags accept any input valid for time.ParseDuration.

The default set of command-line flags is controlled by top-level functions. The
FlagSet type allows one to define independent sets of flags, such as to
implement subcommands in a command-line interface. The methods of FlagSet are
analogous to the top-level functions for the command-line flag set.

<a id="example"></a>
Example:

    // These examples demonstrate more intricate uses of the flag package.
    package flag_test

    import (
        "errors"
        "flag"
        "fmt"
        "strings"
        "time"
    )

    // Example 1: A single string flag called "species" with default value "gopher".
    var species = flag.String("species", "gopher", "the species we are studying")

    // Example 2: Two flags sharing a variable, so we can have a shorthand.
    // The order of initialization is undefined, so make sure both use the
    // same default value. They must be set up with an init function.
    var gopherType string

    func init() {
        const (
            defaultGopher = "pocket"
            usage         = "the variety of gopher"
        )
        flag.StringVar(&gopherType, "gopher_type", defaultGopher, usage)
        flag.StringVar(&gopherType, "g", defaultGopher, usage+" (shorthand)")
    }

    // Example 3: A user-defined flag type, a slice of durations.
    type interval []time.Duration

    // String is the method to format the flag's value, part of the flag.Value interface.
    // The String method's output will be used in diagnostics.
    func (i *interval) String() string {
        return fmt.Sprint(*i)
    }

    // Set is the method to set the flag value, part of the flag.Value interface.
    // Set's argument is a string to be parsed to set the flag.
    // It's a comma-separated list, so we split it.
    func (i *interval) Set(value string) error {
        // If we wanted to allow the flag to be set multiple times,
        // accumulating values, we would delete this if statement.
        // That would permit usages such as
        //	-deltaT 10s -deltaT 15s
        // and other combinations.
        if len(*i) > 0 {
            return errors.New("interval flag already set")
        }
        for _, dt := range strings.Split(value, ",") {
            duration, err := time.ParseDuration(dt)
            if err != nil {
                return err
            }
            *i = append(*i, duration)
        }
        return nil
    }

    // Define a flag to accumulate durations. Because it has a special type,
    // we need to use the Var function and therefore create the flag during
    // init.

    var intervalFlag interval

    func init() {
        // Tie the command-line flag to the intervalFlag variable and
        // set a usage message.
        flag.Var(&intervalFlag, "deltaT", "comma-separated list of intervals to use between events")
    }

    func Example() {
        // All the interesting pieces are with the variables declared above, but
        // to enable the flag package to see the flags defined there, one must
        // execute, typically at the start of main (not init!):
        //	flag.Parse()
        // We don't run it here because this is not a main function and
        // the testing suite has already parsed the flags.
    }

## Index

- [Variables](#pkg-variables)
- [func Arg(i int) string](#Arg)
- [func Args() []string](#Args)
- [func Bool(name string, value bool, usage string) *bool](#Bool)
- [func BoolVar(p *bool, name string, value bool, usage string)](#BoolVar)
- [func Duration(name string, value time.Duration, usage string) *time.Duration](#Duration)
- [func DurationVar(p *time.Duration, name string, value time.Duration, usage string)](#DurationVar)
- [func Float64(name string, value float64, usage string) *float64](#Float64)
- [func Float64Var(p *float64, name string, value float64, usage string)](#Float64Var)
- [func Int(name string, value int, usage string) *int](#Int)
- [func Int64(name string, value int64, usage string) *int64](#Int64)
- [func Int64Var(p *int64, name string, value int64, usage string)](#Int64Var)
- [func IntVar(p *int, name string, value int, usage string)](#IntVar)
- [func NArg() int](#NArg)
- [func NFlag() int](#NFlag)
- [func Parse()](#Parse)
- [func Parsed() bool](#Parsed)
- [func PrintDefaults()](#PrintDefaults)
- [func Set(name, value string) error](#Set)
- [func String(name string, value string, usage string) *string](#String)
- [func StringVar(p *string, name string, value string, usage string)](#StringVar)
- [func Uint(name string, value uint, usage string) *uint](#Uint)
- [func Uint64(name string, value uint64, usage string) *uint64](#Uint64)
- [func Uint64Var(p *uint64, name string, value uint64, usage string)](#Uint64Var)
- [func UintVar(p *uint, name string, value uint, usage string)](#UintVar)
- [func UnquoteUsage(flag *Flag) (name string, usage string)](#UnquoteUsage)
- [func Var(value Value, name string, usage string)](#Var)
- [func Visit(fn func(*Flag))](#Visit)
- [func VisitAll(fn func(*Flag))](#VisitAll)
- [type ErrorHandling](#ErrorHandling)
- [type Flag](#Flag)
  - [func Lookup(name string) *Flag](#Lookup)
- [type FlagSet](#FlagSet)
  - [func NewFlagSet(name string, errorHandling ErrorHandling) *FlagSet](#NewFlagSet)
  - [func (f *FlagSet) Arg(i int) string](#FlagSet.Arg)
  - [func (f *FlagSet) Args() []string](#FlagSet.Args)
  - [func (f *FlagSet) Bool(name string, value bool, usage string) *bool](#FlagSet.Bool)
  - [func (f *FlagSet) BoolVar(p *bool, name string, value bool, usage string)](#FlagSet.BoolVar)
  - [func (f *FlagSet) Duration(name string, value time.Duration, usage string) *time.Duration](#FlagSet.Duration)
  - [func (f *FlagSet) DurationVar(p *time.Duration, name string, value time.Duration, usage string)](#FlagSet.DurationVar)
  - [func (f *FlagSet) ErrorHandling() ErrorHandling](#FlagSet.ErrorHandling)
  - [func (f *FlagSet) Float64(name string, value float64, usage string) *float64](#FlagSet.Float64)
  - [func (f *FlagSet) Float64Var(p *float64, name string, value float64, usage string)](#FlagSet.Float64Var)
  - [func (f *FlagSet) Init(name string, errorHandling ErrorHandling)](#FlagSet.Init)
  - [func (f *FlagSet) Int(name string, value int, usage string) *int](#FlagSet.Int)
  - [func (f *FlagSet) Int64(name string, value int64, usage string) *int64](#FlagSet.Int64)
  - [func (f *FlagSet) Int64Var(p *int64, name string, value int64, usage string)](#FlagSet.Int64Var)
  - [func (f *FlagSet) IntVar(p *int, name string, value int, usage string)](#FlagSet.IntVar)
  - [func (f *FlagSet) Lookup(name string) *Flag](#FlagSet.Lookup)
  - [func (f *FlagSet) NArg() int](#FlagSet.NArg)
  - [func (f *FlagSet) NFlag() int](#FlagSet.NFlag)
  - [func (f *FlagSet) Name() string](#FlagSet.Name)
  - [func (f *FlagSet) Output() io.Writer](#FlagSet.Output)
  - [func (f *FlagSet) Parse(arguments []string) error](#FlagSet.Parse)
  - [func (f *FlagSet) Parsed() bool](#FlagSet.Parsed)
  - [func (f *FlagSet) PrintDefaults()](#FlagSet.PrintDefaults)
  - [func (f *FlagSet) Set(name, value string) error](#FlagSet.Set)
  - [func (f *FlagSet) SetOutput(output io.Writer)](#FlagSet.SetOutput)
  - [func (f *FlagSet) String(name string, value string, usage string) *string](#FlagSet.String)
  - [func (f *FlagSet) StringVar(p *string, name string, value string, usage string)](#FlagSet.StringVar)
  - [func (f *FlagSet) Uint(name string, value uint, usage string) *uint](#FlagSet.Uint)
  - [func (f *FlagSet) Uint64(name string, value uint64, usage string) *uint64](#FlagSet.Uint64)
  - [func (f *FlagSet) Uint64Var(p *uint64, name string, value uint64, usage string)](#FlagSet.Uint64Var)
  - [func (f *FlagSet) UintVar(p *uint, name string, value uint, usage string)](#FlagSet.UintVar)
  - [func (f *FlagSet) Var(value Value, name string, usage string)](#FlagSet.Var)
  - [func (f *FlagSet) Visit(fn func(*Flag))](#FlagSet.Visit)
  - [func (f *FlagSet) VisitAll(fn func(*Flag))](#FlagSet.VisitAll)
- [type Getter](#Getter)
- [type Value](#Value)

### Examples

- [Package](#example)

### Package files
 [flag.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="CommandLine">CommandLine</span> = <a href="#NewFlagSet">NewFlagSet</a>(<a href="/os/">os</a>.<a href="/os/#Args">Args</a>[0], <a href="#ExitOnError">ExitOnError</a>)</pre>

CommandLine is the default set of command-line flags, parsed from os.Args. The
top-level functions such as BoolVar, Arg, and so on are wrappers for the methods
of CommandLine.

<pre>var <span id="ErrHelp">ErrHelp</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;flag: help requested&#34;)</pre>

ErrHelp is the error returned if the -help or -h flag is invoked but no such
flag is defined.

<pre>var <span id="Usage">Usage</span> = func() {
    <a href="/fmt/">fmt</a>.<a href="/fmt/#Fprintf">Fprintf</a>(<a href="#CommandLine">CommandLine</a>.<a href="#Output">Output</a>(), &#34;Usage of %s:\n&#34;, <a href="/os/">os</a>.<a href="/os/#Args">Args</a>[0])
    <a href="#PrintDefaults">PrintDefaults</a>()
}</pre>

Usage prints a usage message documenting all defined command-line flags to
CommandLine's output, which by default is os.Stderr. It is called when an error
occurs while parsing flags. The function is a variable that may be changed to
point to a custom function. By default it prints a simple header and calls
PrintDefaults; for details about the format of the output and how to control it,
see the documentation for PrintDefaults. Custom usage functions may choose to
exit the program; by default exiting happens anyway as the command line's error
handling strategy is set to ExitOnError.

<h2 id="Arg">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L554">Arg</a>
    <a href="#Arg">¶</a></h2>
<pre>func Arg(i <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

Arg returns the i'th command-line argument. Arg(0) is the first remaining
argument after flags have been processed. Arg returns an empty string if the
requested element does not exist.

<h2 id="Args">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L568">Args</a>
    <a href="#Args">¶</a></h2>
<pre>func Args() []<a href="/builtin/#string">string</a></pre>

Args returns the non-flag command-line arguments.

<h2 id="Bool">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L592">Bool</a>
    <a href="#Bool">¶</a></h2>
<pre>func Bool(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#bool">bool</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#bool">bool</a></pre>

Bool defines a bool flag with specified name, default value, and usage string.
The return value is the address of a bool variable that stores the value of the
flag.

<h2 id="BoolVar">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L578">BoolVar</a>
    <a href="#BoolVar">¶</a></h2>
<pre>func BoolVar(p *<a href="/builtin/#bool">bool</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#bool">bool</a>, usage <a href="/builtin/#string">string</a>)</pre>

BoolVar defines a bool flag with specified name, default value, and usage
string. The argument p points to a bool variable in which to store the value of
the flag.

<h2 id="Duration">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L778">Duration</a>
    <a href="#Duration">¶</a></h2>
<pre>func Duration(name <a href="/builtin/#string">string</a>, value <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>, usage <a href="/builtin/#string">string</a>) *<a href="/time/">time</a>.<a href="/time/#Duration">Duration</a></pre>

Duration defines a time.Duration flag with specified name, default value, and
usage string. The return value is the address of a time.Duration variable that
stores the value of the flag. The flag accepts a value acceptable to
time.ParseDuration.

<h2 id="DurationVar">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L762">DurationVar</a>
    <a href="#DurationVar">¶</a></h2>
<pre>func DurationVar(p *<a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>, name <a href="/builtin/#string">string</a>, value <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>, usage <a href="/builtin/#string">string</a>)</pre>

DurationVar defines a time.Duration flag with specified name, default value, and
usage string. The argument p points to a time.Duration variable in which to
store the value of the flag. The flag accepts a value acceptable to
time.ParseDuration.

<h2 id="Float64">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L748">Float64</a>
    <a href="#Float64">¶</a></h2>
<pre>func Float64(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#float64">float64</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#float64">float64</a></pre>

Float64 defines a float64 flag with specified name, default value, and usage
string. The return value is the address of a float64 variable that stores the
value of the flag.

<h2 id="Float64Var">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L734">Float64Var</a>
    <a href="#Float64Var">¶</a></h2>
<pre>func Float64Var(p *<a href="/builtin/#float64">float64</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#float64">float64</a>, usage <a href="/builtin/#string">string</a>)</pre>

Float64Var defines a float64 flag with specified name, default value, and usage
string. The argument p points to a float64 variable in which to store the value
of the flag.

<h2 id="Int">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L618">Int</a>
    <a href="#Int">¶</a></h2>
<pre>func Int(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#int">int</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#int">int</a></pre>

Int defines an int flag with specified name, default value, and usage string.
The return value is the address of an int variable that stores the value of the
flag.

<h2 id="Int64">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L644">Int64</a>
    <a href="#Int64">¶</a></h2>
<pre>func Int64(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#int64">int64</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#int64">int64</a></pre>

Int64 defines an int64 flag with specified name, default value, and usage
string. The return value is the address of an int64 variable that stores the
value of the flag.

<h2 id="Int64Var">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L630">Int64Var</a>
    <a href="#Int64Var">¶</a></h2>
<pre>func Int64Var(p *<a href="/builtin/#int64">int64</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#int64">int64</a>, usage <a href="/builtin/#string">string</a>)</pre>

Int64Var defines an int64 flag with specified name, default value, and usage
string. The argument p points to an int64 variable in which to store the value
of the flag.

<h2 id="IntVar">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L604">IntVar</a>
    <a href="#IntVar">¶</a></h2>
<pre>func IntVar(p *<a href="/builtin/#int">int</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#int">int</a>, usage <a href="/builtin/#string">string</a>)</pre>

IntVar defines an int flag with specified name, default value, and usage string.
The argument p points to an int variable in which to store the value of the
flag.

<h2 id="NArg">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L562">NArg</a>
    <a href="#NArg">¶</a></h2>
<pre>func NArg() <a href="/builtin/#int">int</a></pre>

NArg is the number of arguments remaining after flags have been processed.

<h2 id="NFlag">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L539">NFlag</a>
    <a href="#NFlag">¶</a></h2>
<pre>func NFlag() <a href="/builtin/#int">int</a></pre>

NFlag returns the number of command-line flags that have been set.

<h2 id="Parse">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L946">Parse</a>
    <a href="#Parse">¶</a></h2>
<pre>func Parse()</pre>

Parse parses the command-line flags from os.Args[1:]. Must be called after all
flags are defined and before flags are accessed by the program.

<h2 id="Parsed">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L952">Parsed</a>
    <a href="#Parsed">¶</a></h2>
<pre>func Parsed() <a href="/builtin/#bool">bool</a></pre>

Parsed reports whether the command-line flags have been parsed.

<h2 id="PrintDefaults">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L503">PrintDefaults</a>
    <a href="#PrintDefaults">¶</a></h2>
<pre>func PrintDefaults()</pre>

PrintDefaults prints, to standard error unless configured otherwise, a usage
message showing the default settings of all defined command-line flags. For an
integer valued flag x, the default output has the form

    -x int
    	usage-message-for-x (default 7)

The usage message will appear on a separate line for anything but a bool flag
with a one-byte name. For bool flags, the type is omitted and if the flag name
is one byte the usage message appears on the same line. The parenthetical
default is omitted if the default is the zero value for the type. The listed
type, here int, can be changed by placing a back-quoted name in the flag's usage
string; the first such item in the message is taken to be a parameter name to
show in the message and the back quotes are stripped from the message when
displayed. For instance, given

    flag.String("I", "", "search `directory` for include files")

the output will be

    -I directory
    	search directory for include files.

<h2 id="Set">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L384">Set</a>
    <a href="#Set">¶</a></h2>
<pre>func Set(name, value <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Set sets the value of the named command-line flag.

<h2 id="String">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L722">String</a>
    <a href="#String">¶</a></h2>
<pre>func String(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#string">string</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#string">string</a></pre>

String defines a string flag with specified name, default value, and usage
string. The return value is the address of a string variable that stores the
value of the flag.

<h2 id="StringVar">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L708">StringVar</a>
    <a href="#StringVar">¶</a></h2>
<pre>func StringVar(p *<a href="/builtin/#string">string</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#string">string</a>, usage <a href="/builtin/#string">string</a>)</pre>

StringVar defines a string flag with specified name, default value, and usage
string. The argument p points to a string variable in which to store the value
of the flag.

<h2 id="Uint">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L670">Uint</a>
    <a href="#Uint">¶</a></h2>
<pre>func Uint(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#uint">uint</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#uint">uint</a></pre>

Uint defines a uint flag with specified name, default value, and usage string.
The return value is the address of a uint variable that stores the value of the
flag.

<h2 id="Uint64">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L696">Uint64</a>
    <a href="#Uint64">¶</a></h2>
<pre>func Uint64(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#uint64">uint64</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#uint64">uint64</a></pre>

Uint64 defines a uint64 flag with specified name, default value, and usage
string. The return value is the address of a uint64 variable that stores the
value of the flag.

<h2 id="Uint64Var">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L682">Uint64Var</a>
    <a href="#Uint64Var">¶</a></h2>
<pre>func Uint64Var(p *<a href="/builtin/#uint64">uint64</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#uint64">uint64</a>, usage <a href="/builtin/#string">string</a>)</pre>

Uint64Var defines a uint64 flag with specified name, default value, and usage
string. The argument p points to a uint64 variable in which to store the value
of the flag.

<h2 id="UintVar">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L656">UintVar</a>
    <a href="#UintVar">¶</a></h2>
<pre>func UintVar(p *<a href="/builtin/#uint">uint</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#uint">uint</a>, usage <a href="/builtin/#string">string</a>)</pre>

UintVar defines a uint flag with specified name, default value, and usage
string. The argument p points to a uint variable in which to store the value of
the flag.

<h2 id="UnquoteUsage">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L417">UnquoteUsage</a>
    <a href="#UnquoteUsage">¶</a></h2>
<pre>func UnquoteUsage(flag *<a href="#Flag">Flag</a>) (name <a href="/builtin/#string">string</a>, usage <a href="/builtin/#string">string</a>)</pre>

UnquoteUsage extracts a back-quoted name from the usage string for a flag and
returns it and the un-quoted usage. Given "a `name` to show" it returns ("name",
"a name to show"). If there are no back quotes, the name is an educated guess of
the type of the flag's value, or the empty string if the flag is boolean.

<h2 id="Var">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L814">Var</a>
    <a href="#Var">¶</a></h2>
<pre>func Var(value <a href="#Value">Value</a>, name <a href="/builtin/#string">string</a>, usage <a href="/builtin/#string">string</a>)</pre>

Var defines a flag with the specified name and usage string. The type and value
of the flag are represented by the first argument, of type Value, which
typically holds a user-defined implementation of Value. For instance, the caller
could create a flag that turns a comma-separated string into a slice of strings
by giving the slice the methods of Value; in particular, Set would decompose the
comma-separated string into the slice.

<h2 id="Visit">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L351">Visit</a>
    <a href="#Visit">¶</a></h2>
<pre>func Visit(fn func(*<a href="#Flag">Flag</a>))</pre>

Visit visits the command-line flags in lexicographical order, calling fn for
each. It visits only those flags that have been set.

<h2 id="VisitAll">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L337">VisitAll</a>
    <a href="#VisitAll">¶</a></h2>
<pre>func VisitAll(fn func(*<a href="#Flag">Flag</a>))</pre>

VisitAll visits the command-line flags in lexicographical order, calling fn for
each. It visits all flags, even those not set.

<h2 id="ErrorHandling">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L250">ErrorHandling</a>
    <a href="#ErrorHandling">¶</a></h2>
<pre>type ErrorHandling <a href="/builtin/#int">int</a></pre>

ErrorHandling defines how FlagSet.Parse behaves if the parse fails.

<pre>const (
    <span id="ContinueOnError">ContinueOnError</span> <a href="#ErrorHandling">ErrorHandling</a> = <a href="/builtin/#iota">iota</a> <span class="comment">// Return a descriptive error.</span>
    <span id="ExitOnError">ExitOnError</span>                          <span class="comment">// Call os.Exit(2).</span>
    <span id="PanicOnError">PanicOnError</span>                         <span class="comment">// Call panic with a descriptive error.</span>
)</pre>

These constants cause FlagSet.Parse to behave as described if the parse fails.

<h2 id="Flag">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L279">Flag</a>
    <a href="#Flag">¶</a></h2>
<pre>type Flag struct {
<span id="Flag.Name"></span>    Name     <a href="/builtin/#string">string</a> <span class="comment">// name as it appears on command line</span>
<span id="Flag.Usage"></span>    Usage    <a href="/builtin/#string">string</a> <span class="comment">// help message</span>
<span id="Flag.Value"></span>    Value    <a href="#Value">Value</a>  <span class="comment">// value as set</span>
<span id="Flag.DefValue"></span>    DefValue <a href="/builtin/#string">string</a> <span class="comment">// default value (as text); for usage message</span>
}</pre>

A Flag represents the state of a flag.

<h3 id="Lookup">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L362">Lookup</a>
    <a href="#Lookup">¶</a></h3>
<pre>func Lookup(name <a href="/builtin/#string">string</a>) *<a href="#Flag">Flag</a></pre>

Lookup returns the Flag structure of the named command-line flag, returning nil
if none exists.

<h2 id="FlagSet">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L261">FlagSet</a>
    <a href="#FlagSet">¶</a></h2>
<pre>type FlagSet struct {
<span id="FlagSet.Usage"></span>    <span class="comment">// Usage is the function called when an error occurs while parsing flags.</span>
    <span class="comment">// The field is a function (not a method) that may be changed to point to</span>
    <span class="comment">// a custom error handler. What happens after Usage is called depends</span>
    <span class="comment">// on the ErrorHandling setting; for the command line, this defaults</span>
    <span class="comment">// to ExitOnError, which exits the program after calling Usage.</span>
    Usage func()
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A FlagSet represents a set of defined flags. The zero value of a FlagSet has no
name and has ContinueOnError error handling.

<h3 id="NewFlagSet">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L975">NewFlagSet</a>
    <a href="#NewFlagSet">¶</a></h3>
<pre>func NewFlagSet(name <a href="/builtin/#string">string</a>, errorHandling <a href="#ErrorHandling">ErrorHandling</a>) *<a href="#FlagSet">FlagSet</a></pre>

NewFlagSet returns a new, empty flag set with the specified name and error
handling property.

<h3 id="FlagSet.Arg">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L544">Arg</a>
    <a href="#FlagSet.Arg">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Arg(i <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

Arg returns the i'th argument. Arg(0) is the first remaining argument after
flags have been processed. Arg returns an empty string if the requested element
does not exist.

<h3 id="FlagSet.Args">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L565">Args</a>
    <a href="#FlagSet.Args">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Args() []<a href="/builtin/#string">string</a></pre>

Args returns the non-flag arguments.

<h3 id="FlagSet.Bool">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L584">Bool</a>
    <a href="#FlagSet.Bool">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Bool(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#bool">bool</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#bool">bool</a></pre>

Bool defines a bool flag with specified name, default value, and usage string.
The return value is the address of a bool variable that stores the value of the
flag.

<h3 id="FlagSet.BoolVar">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L572">BoolVar</a>
    <a href="#FlagSet.BoolVar">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) BoolVar(p *<a href="/builtin/#bool">bool</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#bool">bool</a>, usage <a href="/builtin/#string">string</a>)</pre>

BoolVar defines a bool flag with specified name, default value, and usage
string. The argument p points to a bool variable in which to store the value of
the flag.

<h3 id="FlagSet.Duration">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L769">Duration</a>
    <a href="#FlagSet.Duration">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Duration(name <a href="/builtin/#string">string</a>, value <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>, usage <a href="/builtin/#string">string</a>) *<a href="/time/">time</a>.<a href="/time/#Duration">Duration</a></pre>

Duration defines a time.Duration flag with specified name, default value, and
usage string. The return value is the address of a time.Duration variable that
stores the value of the flag. The flag accepts a value acceptable to
time.ParseDuration.

<h3 id="FlagSet.DurationVar">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L755">DurationVar</a>
    <a href="#FlagSet.DurationVar">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) DurationVar(p *<a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>, name <a href="/builtin/#string">string</a>, value <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>, usage <a href="/builtin/#string">string</a>)</pre>

DurationVar defines a time.Duration flag with specified name, default value, and
usage string. The argument p points to a time.Duration variable in which to
store the value of the flag. The flag accepts a value acceptable to
time.ParseDuration.

<h3 id="FlagSet.ErrorHandling">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L317">ErrorHandling</a>
    <a href="#FlagSet.ErrorHandling">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) ErrorHandling() <a href="#ErrorHandling">ErrorHandling</a></pre>

ErrorHandling returns the error handling behavior of the flag set.

<h3 id="FlagSet.Float64">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L740">Float64</a>
    <a href="#FlagSet.Float64">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Float64(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#float64">float64</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#float64">float64</a></pre>

Float64 defines a float64 flag with specified name, default value, and usage
string. The return value is the address of a float64 variable that stores the
value of the flag.

<h3 id="FlagSet.Float64Var">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L728">Float64Var</a>
    <a href="#FlagSet.Float64Var">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Float64Var(p *<a href="/builtin/#float64">float64</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#float64">float64</a>, usage <a href="/builtin/#string">string</a>)</pre>

Float64Var defines a float64 flag with specified name, default value, and usage
string. The argument p points to a float64 variable in which to store the value
of the flag.

<h3 id="FlagSet.Init">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L987">Init</a>
    <a href="#FlagSet.Init">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Init(name <a href="/builtin/#string">string</a>, errorHandling <a href="#ErrorHandling">ErrorHandling</a>)</pre>

Init sets the name and error handling property for a flag set. By default, the
zero FlagSet uses an empty name and the ContinueOnError error handling policy.

<h3 id="FlagSet.Int">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L610">Int</a>
    <a href="#FlagSet.Int">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Int(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#int">int</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#int">int</a></pre>

Int defines an int flag with specified name, default value, and usage string.
The return value is the address of an int variable that stores the value of the
flag.

<h3 id="FlagSet.Int64">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L636">Int64</a>
    <a href="#FlagSet.Int64">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Int64(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#int64">int64</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#int64">int64</a></pre>

Int64 defines an int64 flag with specified name, default value, and usage
string. The return value is the address of an int64 variable that stores the
value of the flag.

<h3 id="FlagSet.Int64Var">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L624">Int64Var</a>
    <a href="#FlagSet.Int64Var">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Int64Var(p *<a href="/builtin/#int64">int64</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#int64">int64</a>, usage <a href="/builtin/#string">string</a>)</pre>

Int64Var defines an int64 flag with specified name, default value, and usage
string. The argument p points to an int64 variable in which to store the value
of the flag.

<h3 id="FlagSet.IntVar">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L598">IntVar</a>
    <a href="#FlagSet.IntVar">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) IntVar(p *<a href="/builtin/#int">int</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#int">int</a>, usage <a href="/builtin/#string">string</a>)</pre>

IntVar defines an int flag with specified name, default value, and usage string.
The argument p points to an int variable in which to store the value of the
flag.

<h3 id="FlagSet.Lookup">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L356">Lookup</a>
    <a href="#FlagSet.Lookup">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Lookup(name <a href="/builtin/#string">string</a>) *<a href="#Flag">Flag</a></pre>

Lookup returns the Flag structure of the named flag, returning nil if none
exists.

<h3 id="FlagSet.NArg">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L559">NArg</a>
    <a href="#FlagSet.NArg">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) NArg() <a href="/builtin/#int">int</a></pre>

NArg is the number of arguments remaining after flags have been processed.

<h3 id="FlagSet.NFlag">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L536">NFlag</a>
    <a href="#FlagSet.NFlag">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) NFlag() <a href="/builtin/#int">int</a></pre>

NFlag returns the number of flags that have been set.

<h3 id="FlagSet.Name">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L312">Name</a>
    <a href="#FlagSet.Name">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Name() <a href="/builtin/#string">string</a></pre>

Name returns the name of the flag set.

<h3 id="FlagSet.Output">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L304">Output</a>
    <a href="#FlagSet.Output">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Output() <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a></pre>

Output returns the destination for usage and error messages. os.Stderr is
returned if output was not set or was set to nil.

<h3 id="FlagSet.Parse">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L916">Parse</a>
    <a href="#FlagSet.Parse">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Parse(arguments []<a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Parse parses flag definitions from the argument list, which should not include
the command name. Must be called after all flags in the FlagSet are defined and
before flags are accessed by the program. The return value will be ErrHelp if
-help or -h were set but not defined.

<h3 id="FlagSet.Parsed">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L940">Parsed</a>
    <a href="#FlagSet.Parsed">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Parsed() <a href="/builtin/#bool">bool</a></pre>

Parsed reports whether f.Parse has been called.

<h3 id="FlagSet.PrintDefaults">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L454">PrintDefaults</a>
    <a href="#FlagSet.PrintDefaults">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) PrintDefaults()</pre>

PrintDefaults prints, to standard error unless configured otherwise, the default
values of all defined command-line flags in the set. See the documentation for
the global function PrintDefaults for more information.

<h3 id="FlagSet.Set">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L367">Set</a>
    <a href="#FlagSet.Set">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Set(name, value <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Set sets the value of the named flag.

<h3 id="FlagSet.SetOutput">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L323">SetOutput</a>
    <a href="#FlagSet.SetOutput">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) SetOutput(output <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>)</pre>

SetOutput sets the destination for usage and error messages. If output is nil,
os.Stderr is used.

<h3 id="FlagSet.String">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L714">String</a>
    <a href="#FlagSet.String">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) String(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#string">string</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#string">string</a></pre>

String defines a string flag with specified name, default value, and usage
string. The return value is the address of a string variable that stores the
value of the flag.

<h3 id="FlagSet.StringVar">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L702">StringVar</a>
    <a href="#FlagSet.StringVar">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) StringVar(p *<a href="/builtin/#string">string</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#string">string</a>, usage <a href="/builtin/#string">string</a>)</pre>

StringVar defines a string flag with specified name, default value, and usage
string. The argument p points to a string variable in which to store the value
of the flag.

<h3 id="FlagSet.Uint">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L662">Uint</a>
    <a href="#FlagSet.Uint">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Uint(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#uint">uint</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#uint">uint</a></pre>

Uint defines a uint flag with specified name, default value, and usage string.
The return value is the address of a uint variable that stores the value of the
flag.

<h3 id="FlagSet.Uint64">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L688">Uint64</a>
    <a href="#FlagSet.Uint64">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Uint64(name <a href="/builtin/#string">string</a>, value <a href="/builtin/#uint64">uint64</a>, usage <a href="/builtin/#string">string</a>) *<a href="/builtin/#uint64">uint64</a></pre>

Uint64 defines a uint64 flag with specified name, default value, and usage
string. The return value is the address of a uint64 variable that stores the
value of the flag.

<h3 id="FlagSet.Uint64Var">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L676">Uint64Var</a>
    <a href="#FlagSet.Uint64Var">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Uint64Var(p *<a href="/builtin/#uint64">uint64</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#uint64">uint64</a>, usage <a href="/builtin/#string">string</a>)</pre>

Uint64Var defines a uint64 flag with specified name, default value, and usage
string. The argument p points to a uint64 variable in which to store the value
of the flag.

<h3 id="FlagSet.UintVar">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L650">UintVar</a>
    <a href="#FlagSet.UintVar">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) UintVar(p *<a href="/builtin/#uint">uint</a>, name <a href="/builtin/#string">string</a>, value <a href="/builtin/#uint">uint</a>, usage <a href="/builtin/#string">string</a>)</pre>

UintVar defines a uint flag with specified name, default value, and usage
string. The argument p points to a uint variable in which to store the value of
the flag.

<h3 id="FlagSet.Var">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L788">Var</a>
    <a href="#FlagSet.Var">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Var(value <a href="#Value">Value</a>, name <a href="/builtin/#string">string</a>, usage <a href="/builtin/#string">string</a>)</pre>

Var defines a flag with the specified name and usage string. The type and value
of the flag are represented by the first argument, of type Value, which
typically holds a user-defined implementation of Value. For instance, the caller
could create a flag that turns a comma-separated string into a slice of strings
by giving the slice the methods of Value; in particular, Set would decompose the
comma-separated string into the slice.

<h3 id="FlagSet.Visit">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L343">Visit</a>
    <a href="#FlagSet.Visit">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) Visit(fn func(*<a href="#Flag">Flag</a>))</pre>

Visit visits the flags in lexicographical order, calling fn for each. It visits
only those flags that have been set.

<h3 id="FlagSet.VisitAll">func (*FlagSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L329">VisitAll</a>
    <a href="#FlagSet.VisitAll">¶</a></h3>
<pre>func (f *<a href="#FlagSet">FlagSet</a>) VisitAll(fn func(*<a href="#Flag">Flag</a>))</pre>

VisitAll visits the flags in lexicographical order, calling fn for each. It
visits all flags, even those not set.

<h2 id="Getter">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L244">Getter</a>
    <a href="#Getter">¶</a></h2>
<pre>type Getter interface {
    <a href="#Value">Value</a>
    Get() interface{}
}</pre>

Getter is an interface that allows the contents of a Value to be retrieved. It
wraps the Value interface, rather than being part of it, because it appeared
after Go 1 and its compatibility rules. All Value types provided by this package
satisfy the Getter interface.

<h2 id="Value">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/flag/flag.go#L235">Value</a>
    <a href="#Value">¶</a></h2>
<pre>type Value interface {
    String() <a href="/builtin/#string">string</a>
    Set(<a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a>
}</pre>

Value is the interface to the dynamic value stored in a flag. (The default value
is represented as a string.)

If a Value has an IsBoolFlag() bool method returning true, the command-line
parser makes -name equivalent to -name=true rather than using the next
command-line argument.

Set is called once, in command line order, for each flag present. The flag
package may call the String method with a zero-valued receiver, such as a nil
pointer.


