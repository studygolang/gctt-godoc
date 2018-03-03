version: 1.10
## package errors

  `import "errors"`

## Overview

Package errors implements functions to manipulate errors.

<a id="example"></a>
Example:

    package errors_test

    import (
        "fmt"
        "time"
    )

    // MyError is an error implementation that includes a time and message.
    type MyError struct {
        When time.Time
        What string
    }

    func (e MyError) Error() string {
        return fmt.Sprintf("%v: %v", e.When, e.What)
    }

    func oops() error {
        return MyError{
            time.Date(1989, 3, 15, 22, 30, 0, 0, time.UTC),
            "the file system has gone away",
        }
    }

    func Example() {
        if err := oops(); err != nil {
            fmt.Println(err)
        }
        // Output: 1989-03-15 22:30:00 +0000 UTC: the file system has gone away
    }

## Index

- [func New(text string) error](#New)

### Examples

- [Package](#example)
- [New](#exampleNew)
- [New (Errorf)](#exampleNew_errorf)

### Package files
 [errors.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/errors/errors.go)

<h2 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/errors/errors.go#L1">New</a>
    <a href="#New">¶</a></h2>
<pre>func New(text <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

New returns an error that formats as the given text.

<a id="exampleNew"></a>
Example:

    err := errors.New("emit macho dwarf: elf header corrupted")
    if err != nil {
        fmt.Print(err)
    }
    // Output: emit macho dwarf: elf header corrupted


<a id="exampleNew_errorf"></a>
Example:

    const name, id = "bimmler", 17
    err := fmt.Errorf("user %q (id %d) not found", name, id)
    if err != nil {
        fmt.Print(err)
    }
    // Output: user "bimmler" (id 17) not found


