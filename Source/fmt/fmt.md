version: 1.10
## package fmt

  `import "fmt"`

## Overview

Package fmt implements formatted I/O with functions analogous to C's printf and
scanf. The format 'verbs' are derived from C's but are simpler.


Printing

The verbs:

General:

    %v	the value in a default format
    	when printing structs, the plus flag (%+v) adds field names
    %#v	a Go-syntax representation of the value
    %T	a Go-syntax representation of the type of the value
    %%	a literal percent sign; consumes no value

Boolean:

    %t	the word true or false

Integer:

    %b	base 2
    %c	the character represented by the corresponding Unicode code point
    %d	base 10
    %o	base 8
    %q	a single-quoted character literal safely escaped with Go syntax.
    %x	base 16, with lower-case letters for a-f
    %X	base 16, with upper-case letters for A-F
    %U	Unicode format: U+1234; same as "U+%04X"

Floating-point and complex constituents:

    %b	decimalless scientific notation with exponent a power of two,
    	in the manner of strconv.FormatFloat with the 'b' format,
    	e.g. -123456p-78
    %e	scientific notation, e.g. -1.234456e+78
    %E	scientific notation, e.g. -1.234456E+78
    %f	decimal point but no exponent, e.g. 123.456
    %F	synonym for %f
    %g	%e for large exponents, %f otherwise. Precision is discussed below.
    %G	%E for large exponents, %F otherwise

String and slice of bytes (treated equivalently with these verbs):

    %s	the uninterpreted bytes of the string or slice
    %q	a double-quoted string safely escaped with Go syntax
    %x	base 16, lower-case, two characters per byte
    %X	base 16, upper-case, two characters per byte

Pointer:

    %p	base 16 notation, with leading 0x
    The %b, %d, %o, %x and %X verbs also work with pointers,
    formatting the value exactly as if it were an integer.

The default format for %v is:

    bool:                    %t
    int, int8 etc.:          %d
    uint, uint8 etc.:        %d, %#x if printed with %#v
    float32, complex64, etc: %g
    string:                  %s
    chan:                    %p
    pointer:                 %p

For compound objects, the elements are printed using these rules, recursively,
laid out like this:

    struct:             {field0 field1 ...}
    array, slice:       [elem0 elem1 ...]
    maps:               map[key1:value1 key2:value2]
    pointer to above:   &{}, &[], &map[]

Width is specified by an optional decimal number immediately preceding the verb.
If absent, the width is whatever is necessary to represent the value. Precision
is specified after the (optional) width by a period followed by a decimal
number. If no period is present, a default precision is used. A period with no
following number specifies a precision of zero. Examples:

    %f     default width, default precision
    %9f    width 9, default precision
    %.2f   default width, precision 2
    %9.2f  width 9, precision 2
    %9.f   width 9, precision 0

Width and precision are measured in units of Unicode code points, that is,
runes. (This differs from C's printf where the units are always measured in
bytes.) Either or both of the flags may be replaced with the character '*',
causing their values to be obtained from the next operand (preceding the one to
format), which must be of type int.

For most values, width is the minimum number of runes to output, padding the
formatted form with spaces if necessary.

For strings, byte slices and byte arrays, however, precision limits the length
of the input to be formatted (not the size of the output), truncating if
necessary. Normally it is measured in runes, but for these types when formatted
with the %x or %X format it is measured in bytes.

For floating-point values, width sets the minimum width of the field and
precision sets the number of places after the decimal, if appropriate, except
that for %g/%G precision sets the total number of significant digits. For
example, given 12.345 the format %6.3f prints 12.345 while %.3g prints 12.3. The
default precision for %e, %f and %#g is 6; for %g it is the smallest number of
digits necessary to identify the value uniquely.

For complex numbers, the width and precision apply to the two components
independently and the result is parenthesized, so %f applied to 1.2+3.4i
produces (1.200000+3.400000i).

Other flags:

    +	always print a sign for numeric values;
    	guarantee ASCII-only output for %q (%+q)
    -	pad with spaces on the right rather than the left (left-justify the field)
    #	alternate format: add leading 0 for octal (%#o), 0x for hex (%#x);
    	0X for hex (%#X); suppress 0x for %p (%#p);
    	for %q, print a raw (backquoted) string if strconv.CanBackquote
    	returns true;
    	always print a decimal point for %e, %E, %f, %F, %g and %G;
    	do not remove trailing zeros for %g and %G;
    	write e.g. U+0078 'x' if the character is printable for %U (%#U).
    ' '	(space) leave a space for elided sign in numbers (% d);
    	put spaces between bytes printing strings or slices in hex (% x, % X)
    0	pad with leading zeros rather than spaces;
    	for numbers, this moves the padding after the sign

Flags are ignored by verbs that do not expect them. For example there is no
alternate decimal format, so %#d and %d behave identically.

For each Printf-like function, there is also a Print function that takes no
format and is equivalent to saying %v for every operand. Another variant Println
inserts blanks between operands and appends a newline.

Regardless of the verb, if an operand is an interface value, the internal
concrete value is used, not the interface itself. Thus:

    var i interface{} = 23
    fmt.Printf("%v\n", i)

will print 23.

Except when printed using the verbs %T and %p, special formatting considerations
apply for operands that implement certain interfaces. In order of application:

1. If the operand is a reflect.Value, the operand is replaced by the concrete
value that it holds, and printing continues with the next rule.

2. If an operand implements the Formatter interface, it will be invoked.
Formatter provides fine control of formatting.

3. If the %v verb is used with the # flag (%#v) and the operand implements the
GoStringer interface, that will be invoked.

If the format (which is implicitly %v for Println etc.) is valid for a string
(%s %q %v %x %X), the following two rules apply:

4. If an operand implements the error interface, the Error method will be
invoked to convert the object to a string, which will then be formatted as
required by the verb (if any).

5. If an operand implements method String() string, that method will be invoked
to convert the object to a string, which will then be formatted as required by
the verb (if any).

For compound operands such as slices and structs, the format applies to the
elements of each operand, recursively, not to the operand as a whole. Thus %q
will quote each element of a slice of strings, and %6.2f will control formatting
for each element of a floating-point array.

However, when printing a byte slice with a string-like verb (%s %q %x %X), it is
treated identically to a string, as a single item.

To avoid recursion in cases such as

    type X string
    func (x X) String() string { return Sprintf("<%s>", x) }

convert the value before recurring:

    func (x X) String() string { return Sprintf("<%s>", string(x)) }

Infinite recursion can also be triggered by self-referential data structures,
such as a slice that contains itself as an element, if that type has a String
method. Such pathologies are rare, however, and the package does not protect
against them.

When printing a struct, fmt cannot and therefore does not invoke formatting
methods such as Error or String on unexported fields.

Explicit argument indexes:

In Printf, Sprintf, and Fprintf, the default behavior is for each formatting
verb to format successive arguments passed in the call. However, the notation
[n] immediately before the verb indicates that the nth one-indexed argument is
to be formatted instead. The same notation before a '*' for a width or precision
selects the argument index holding the value. After processing a bracketed
expression [n], subsequent verbs will use arguments n+1, n+2, etc. unless
otherwise directed.

For example,

    fmt.Sprintf("%[2]d %[1]d\n", 11, 22)

will yield "22 11", while

    fmt.Sprintf("%[3]*.[2]*[1]f", 12.0, 2, 6)

equivalent to

    fmt.Sprintf("%6.2f", 12.0)

will yield " 12.00". Because an explicit index affects subsequent verbs, this
notation can be used to print the same values multiple times by resetting the
index for the first argument to be repeated:

    fmt.Sprintf("%d %d %#[1]x %#x", 16, 17)

will yield "16 17 0x10 0x11".

Format errors:

If an invalid argument is given for a verb, such as providing a string to %d,
the generated string will contain a description of the problem, as in these
examples:

    Wrong type or unknown verb: %!verb(type=value)
    	Printf("%d", hi):          %!d(string=hi)
    Too many arguments: %!(EXTRA type=value)
    	Printf("hi", "guys"):      hi%!(EXTRA string=guys)
    Too few arguments: %!verb(MISSING)
    	Printf("hi%d"):            hi%!d(MISSING)
    Non-int for width or precision: %!(BADWIDTH) or %!(BADPREC)
    	Printf("%*s", 4.5, "hi"):  %!(BADWIDTH)hi
    	Printf("%.*s", 4.5, "hi"): %!(BADPREC)hi
    Invalid or invalid use of argument index: %!(BADINDEX)
    	Printf("%*[2]d", 7):       %!d(BADINDEX)
    	Printf("%.[2]d", 7):       %!d(BADINDEX)

All errors begin with the string "%!" followed sometimes by a single character
(the verb) and end with a parenthesized description.

If an Error or String method triggers a panic when called by a print routine,
the fmt package reformats the error message from the panic, decorating it with
an indication that it came through the fmt package. For example, if a String
method calls panic("bad"), the resulting formatted message will look like

    %!s(PANIC=bad)

The %!s just shows the print verb in use when the failure occurred. If the panic
is caused by a nil receiver to an Error or String method, however, the output is
the undecorated string, "<nil>".


Scanning

An analogous set of functions scans formatted text to yield values. Scan, Scanf
and Scanln read from os.Stdin; Fscan, Fscanf and Fscanln read from a specified
io.Reader; Sscan, Sscanf and Sscanln read from an argument string.

Scan, Fscan, Sscan treat newlines in the input as spaces.

Scanln, Fscanln and Sscanln stop scanning at a newline and require that the
items be followed by a newline or EOF.

Scanf, Fscanf, and Sscanf parse the arguments according to a format string,
analogous to that of Printf. In the text that follows, 'space' means any Unicode
whitespace character except newline.

In the format string, a verb introduced by the % character consumes and parses
input; these verbs are described in more detail below. A character other than %,
space, or newline in the format consumes exactly that input character, which
must be present. A newline with zero or more spaces before it in the format
string consumes zero or more spaces in the input followed by a single newline or
the end of the input. A space following a newline in the format string consumes
zero or more spaces in the input. Otherwise, any run of one or more spaces in
the format string consumes as many spaces as possible in the input. Unless the
run of spaces in the format string appears adjacent to a newline, the run must
consume at least one space from the input or find the end of the input.

The handling of spaces and newlines differs from that of C's scanf family: in C,
newlines are treated as any other space, and it is never an error when a run of
spaces in the format string finds no spaces to consume in the input.

The verbs behave analogously to those of Printf. For example, %x will scan an
integer as a hexadecimal number, and %v will scan the default representation
format for the value. The Printf verbs %p and %T and the flags # and + are not
implemented, and the verbs %e %E %f %F %g and %G are all equivalent and scan any
floating-point or complex value.

Input processed by verbs is implicitly space-delimited: the implementation of
every verb except %c starts by discarding leading spaces from the remaining
input, and the %s verb (and %v reading into a string) stops consuming input at
the first space or newline character.

The familiar base-setting prefixes 0 (octal) and 0x (hexadecimal) are accepted
when scanning integers without a format or with the %v verb.

Width is interpreted in the input text but there is no syntax for scanning with
a precision (no %5.2f, just %5f). If width is provided, it applies after leading
spaces are trimmed and specifies the maximum number of runes to read to satisfy
the verb. For example,

    Sscanf(" 1234567 ", "%5s%d", &s, &i)

will set s to "12345" and i to 67 while

    Sscanf(" 12 34 567 ", "%5s%d", &s, &i)

will set s to "12" and i to 34.

In all the scanning functions, a carriage return followed immediately by a
newline is treated as a plain newline (\r\n means the same as \n).

In all the scanning functions, if an operand implements method Scan (that is, it
implements the Scanner interface) that method will be used to scan the text for
that operand. Also, if the number of arguments scanned is less than the number
of arguments provided, an error is returned.

All arguments to be scanned must be either pointers to basic types or
implementations of the Scanner interface.

Like Scanf and Fscanf, Sscanf need not consume its entire input. There is no way
to recover how much of the input string Sscanf used.

Note: Fscan etc. can read one character (rune) past the input they return, which
means that a loop calling a scan routine may skip some of the input. This is
usually a problem only when there is no space between input values. If the
reader provided to Fscan implements ReadRune, that method will be used to read
characters. If the reader also implements UnreadRune, that method will be used
to save the character and successive calls will not lose data. To attach
ReadRune and UnreadRune methods to a reader without that capability, use
bufio.NewReader.

## Index

- [func Errorf(format string, a ...interface{}) error](#Errorf)
- [func Fprint(w io.Writer, a ...interface{}) (n int, err error)](#Fprint)
- [func Fprintf(w io.Writer, format string, a ...interface{}) (n int, err error)](#Fprintf)
- [func Fprintln(w io.Writer, a ...interface{}) (n int, err error)](#Fprintln)
- [func Fscan(r io.Reader, a ...interface{}) (n int, err error)](#Fscan)
- [func Fscanf(r io.Reader, format string, a ...interface{}) (n int, err error)](#Fscanf)
- [func Fscanln(r io.Reader, a ...interface{}) (n int, err error)](#Fscanln)
- [func Print(a ...interface{}) (n int, err error)](#Print)
- [func Printf(format string, a ...interface{}) (n int, err error)](#Printf)
- [func Println(a ...interface{}) (n int, err error)](#Println)
- [func Scan(a ...interface{}) (n int, err error)](#Scan)
- [func Scanf(format string, a ...interface{}) (n int, err error)](#Scanf)
- [func Scanln(a ...interface{}) (n int, err error)](#Scanln)
- [func Sprint(a ...interface{}) string](#Sprint)
- [func Sprintf(format string, a ...interface{}) string](#Sprintf)
- [func Sprintln(a ...interface{}) string](#Sprintln)
- [func Sscan(str string, a ...interface{}) (n int, err error)](#Sscan)
- [func Sscanf(str string, format string, a ...interface{}) (n int, err error)](#Sscanf)
- [func Sscanln(str string, a ...interface{}) (n int, err error)](#Sscanln)
- [type Formatter](#Formatter)
- [type GoStringer](#GoStringer)
- [type ScanState](#ScanState)
- [type Scanner](#Scanner)
- [type State](#State)
- [type Stringer](#Stringer)

### Examples

- [Stringer](#exampleStringer)

### Package files
 [doc.go](//github.com/golang/go/blob/release-branch.go1.10/src/fmt/doc.go) [format.go](//github.com/golang/go/blob/release-branch.go1.10/src/fmt/format.go) [print.go](//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go) [scan.go](//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go)

<h2 id="Errorf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L201">Errorf</a>
    <a href="#Errorf">¶</a></h2>
<pre>func Errorf(format <a href="/builtin/#string">string</a>, a ...interface{}) <a href="/builtin/#error">error</a></pre>

Errorf formats according to a format specifier and returns the string as a value
that satisfies error.

<h2 id="Fprint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L210">Fprint</a>
    <a href="#Fprint">¶</a></h2>
<pre>func Fprint(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Fprint formats using the default formats for its operands and writes to w.
Spaces are added between operands when neither is a string. It returns the
number of bytes written and any write error encountered.

<h2 id="Fprintf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L176">Fprintf</a>
    <a href="#Fprintf">¶</a></h2>
<pre>func Fprintf(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, format <a href="/builtin/#string">string</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Fprintf formats according to a format specifier and writes to w. It returns the
number of bytes written and any write error encountered.

<h2 id="Fprintln">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L242">Fprintln</a>
    <a href="#Fprintln">¶</a></h2>
<pre>func Fprintln(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Fprintln formats using the default formats for its operands and writes to w.
Spaces are always added between operands and a newline is appended. It returns
the number of bytes written and any write error encountered.

<h2 id="Fscan">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L111">Fscan</a>
    <a href="#Fscan">¶</a></h2>
<pre>func Fscan(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Fscan scans text read from r, storing successive space-separated values into
successive arguments. Newlines count as space. It returns the number of items
successfully scanned. If that is less than the number of arguments, err will
report why.

<h2 id="Fscanf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L131">Fscanf</a>
    <a href="#Fscanf">¶</a></h2>
<pre>func Fscanf(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, format <a href="/builtin/#string">string</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Fscanf scans text read from r, storing successive space-separated values into
successive arguments as determined by the format. It returns the number of items
successfully parsed. Newlines in the input must match newlines in the format.

<h2 id="Fscanln">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L120">Fscanln</a>
    <a href="#Fscanln">¶</a></h2>
<pre>func Fscanln(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Fscanln is similar to Fscan, but stops scanning at a newline and after the final
item there must be a newline or EOF.

<h2 id="Print">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L221">Print</a>
    <a href="#Print">¶</a></h2>
<pre>func Print(a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Print formats using the default formats for its operands and writes to standard
output. Spaces are added between operands when neither is a string. It returns
the number of bytes written and any write error encountered.

<h2 id="Printf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L186">Printf</a>
    <a href="#Printf">¶</a></h2>
<pre>func Printf(format <a href="/builtin/#string">string</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Printf formats according to a format specifier and writes to standard output. It
returns the number of bytes written and any write error encountered.

<h2 id="Println">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L253">Println</a>
    <a href="#Println">¶</a></h2>
<pre>func Println(a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Println formats using the default formats for its operands and writes to
standard output. Spaces are always added between operands and a newline is
appended. It returns the number of bytes written and any write error
encountered.

<h2 id="Scan">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L53">Scan</a>
    <a href="#Scan">¶</a></h2>
<pre>func Scan(a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Scan scans text read from standard input, storing successive space-separated
values into successive arguments. Newlines count as space. It returns the number
of items successfully scanned. If that is less than the number of arguments, err
will report why.

<h2 id="Scanf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L70">Scanf</a>
    <a href="#Scanf">¶</a></h2>
<pre>func Scanf(format <a href="/builtin/#string">string</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Scanf scans text read from standard input, storing successive space-separated
values into successive arguments as determined by the format. It returns the
number of items successfully scanned. If that is less than the number of
arguments, err will report why. Newlines in the input must match newlines in the
format. The one exception: the verb %c always scans the next rune in the input,
even if it is a space (or tab etc.) or newline.

<h2 id="Scanln">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L59">Scanln</a>
    <a href="#Scanln">¶</a></h2>
<pre>func Scanln(a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Scanln is similar to Scan, but stops scanning at a newline and after the final
item there must be a newline or EOF.

<h2 id="Sprint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L227">Sprint</a>
    <a href="#Sprint">¶</a></h2>
<pre>func Sprint(a ...interface{}) <a href="/builtin/#string">string</a></pre>

Sprint formats using the default formats for its operands and returns the
resulting string. Spaces are added between operands when neither is a string.

<h2 id="Sprintf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L191">Sprintf</a>
    <a href="#Sprintf">¶</a></h2>
<pre>func Sprintf(format <a href="/builtin/#string">string</a>, a ...interface{}) <a href="/builtin/#string">string</a></pre>

Sprintf formats according to a format specifier and returns the resulting
string.

<h2 id="Sprintln">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L259">Sprintln</a>
    <a href="#Sprintln">¶</a></h2>
<pre>func Sprintln(a ...interface{}) <a href="/builtin/#string">string</a></pre>

Sprintln formats using the default formats for its operands and returns the
resulting string. Spaces are always added between operands and a newline is
appended.

<h2 id="Sscan">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L89">Sscan</a>
    <a href="#Sscan">¶</a></h2>
<pre>func Sscan(str <a href="/builtin/#string">string</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Sscan scans the argument string, storing successive space-separated values into
successive arguments. Newlines count as space. It returns the number of items
successfully scanned. If that is less than the number of arguments, err will
report why.

<h2 id="Sscanf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L103">Sscanf</a>
    <a href="#Sscanf">¶</a></h2>
<pre>func Sscanf(str <a href="/builtin/#string">string</a>, format <a href="/builtin/#string">string</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Sscanf scans the argument string, storing successive space-separated values into
successive arguments as determined by the format. It returns the number of items
successfully parsed. Newlines in the input must match newlines in the format.

<h2 id="Sscanln">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L95">Sscanln</a>
    <a href="#Sscanln">¶</a></h2>
<pre>func Sscanln(str <a href="/builtin/#string">string</a>, a ...interface{}) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Sscanln is similar to Sscan, but stops scanning at a newline and after the final
item there must be a newline or EOF.

<h2 id="Formatter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L43">Formatter</a>
    <a href="#Formatter">¶</a></h2>
<pre>type Formatter interface {
    Format(f <a href="#State">State</a>, c <a href="/builtin/#rune">rune</a>)
}</pre>

Formatter is the interface implemented by values with a custom formatter. The
implementation of Format may call Sprint(f) or Fprint(f) etc. to generate its
output.

<h2 id="GoStringer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L60">GoStringer</a>
    <a href="#GoStringer">¶</a></h2>
<pre>type GoStringer interface {
    GoString() <a href="/builtin/#string">string</a>
}</pre>

GoStringer is implemented by any value that has a GoString method, which defines
the Go syntax for that value. The GoString method is used to print values passed
as an operand to a %#v format.

<h2 id="ScanState">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L11">ScanState</a>
    <a href="#ScanState">¶</a></h2>
<pre>type ScanState interface {
    <span class="comment">// ReadRune reads the next rune (Unicode code point) from the input.</span>
    <span class="comment">// If invoked during Scanln, Fscanln, or Sscanln, ReadRune() will</span>
    <span class="comment">// return EOF after returning the first &#39;\n&#39; or when reading beyond</span>
    <span class="comment">// the specified width.</span>
    ReadRune() (r <a href="/builtin/#rune">rune</a>, size <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
    <span class="comment">// UnreadRune causes the next call to ReadRune to return the same rune.</span>
    UnreadRune() <a href="/builtin/#error">error</a>
    <span class="comment">// SkipSpace skips space in the input. Newlines are treated appropriately</span>
    <span class="comment">// for the operation being performed; see the package documentation</span>
    <span class="comment">// for more information.</span>
    SkipSpace()
    <span class="comment">// Token skips space in the input if skipSpace is true, then returns the</span>
    <span class="comment">// run of Unicode code points c satisfying f(c).  If f is nil,</span>
    <span class="comment">// !unicode.IsSpace(c) is used; that is, the token will hold non-space</span>
    <span class="comment">// characters. Newlines are treated appropriately for the operation being</span>
    <span class="comment">// performed; see the package documentation for more information.</span>
    <span class="comment">// The returned slice points to shared data that may be overwritten</span>
    <span class="comment">// by the next call to Token, a call to a Scan function using the ScanState</span>
    <span class="comment">// as input, or when the calling Scan method returns.</span>
    Token(skipSpace <a href="/builtin/#bool">bool</a>, f func(<a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a>) (token []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)
    <span class="comment">// Width returns the value of the width option and whether it has been set.</span>
    <span class="comment">// The unit is Unicode code points.</span>
    Width() (wid <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)
    <span class="comment">// Because ReadRune is implemented by the interface, Read should never be</span>
    <span class="comment">// called by the scanning routines and a valid implementation of</span>
    <span class="comment">// ScanState may choose always to return an error from Read.</span>
    Read(buf []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
}</pre>

ScanState represents the scanner state passed to custom scanners. Scanners may
do rune-at-a-time scanning or ask the ScanState to discover the next
space-delimited token.

<h2 id="Scanner">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/scan.go#L45">Scanner</a>
    <a href="#Scanner">¶</a></h2>
<pre>type Scanner interface {
    Scan(state <a href="#ScanState">ScanState</a>, verb <a href="/builtin/#rune">rune</a>) <a href="/builtin/#error">error</a>
}</pre>

Scanner is implemented by any value that has a Scan method, which scans the
input for the representation of a value and stores the result in the receiver,
which must be a pointer to be useful. The Scan method is called for any argument
to Scan, Scanf, or Scanln that implements it.

<h2 id="State">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L28">State</a>
    <a href="#State">¶</a></h2>
<pre>type State interface {
    <span class="comment">// Write is the function to call to emit formatted output to be printed.</span>
    Write(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
    <span class="comment">// Width returns the value of the width option and whether it has been set.</span>
    Width() (wid <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)
    <span class="comment">// Precision returns the value of the precision option and whether it has been set.</span>
    Precision() (prec <a href="/builtin/#int">int</a>, ok <a href="/builtin/#bool">bool</a>)

    <span class="comment">// Flag reports whether the flag c, a character, has been set.</span>
    Flag(c <a href="/builtin/#int">int</a>) <a href="/builtin/#bool">bool</a>
}</pre>

State represents the printer state passed to custom formatters. It provides
access to the io.Writer interface plus information about the flags and options
for the operand's format specifier.

<h2 id="Stringer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/fmt/print.go#L52">Stringer</a>
    <a href="#Stringer">¶</a></h2>
<pre>type Stringer interface {
    String() <a href="/builtin/#string">string</a>
}</pre>

Stringer is implemented by any value that has a String method, which defines the
``native'' format for that value. The String method is used to print values
passed as an operand to any format that accepts a string or to an unformatted
printer such as Print.

<a id="exampleStringer"></a>
Example:

    package fmt_test

    import (
        "fmt"
    )

    // Animal has a Name and an Age to represent an animal.
    type Animal struct {
        Name string
        Age  uint
    }

    // String makes Animal satisfy the Stringer interface.
    func (a Animal) String() string {
        return fmt.Sprintf("%v (%d)", a.Name, a.Age)
    }

    func ExampleStringer() {
        a := Animal{
            Name: "Gopher",
            Age:  2,
        }
        fmt.Println(a)
        // Output: Gopher (2)
    }


