version: 1.9.2
## package errors

  `import "errors"`

## 概述

errors 包实现了处理错误的方法。

<a id="example"></a>
示例:

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

## 索引

- [func New(text string) error](#New)

### 例子

- [Package](#example)
- [New](#exampleNew)
- [New (Errorf)](#exampleNew_errorf)

### 文件
 [errors.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/errors/errors.go)

<h2 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/errors/errors.go#L1">New</a>
    <a href="#New">¶</a></h2>
<pre>func New(text <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

New 方法返回一个以指定文本格式化的 error 对象。

<a id="exampleNew"></a>
示例:

    err := errors.New("emit macho dwarf: elf header corrupted")
    if err != nil {
        fmt.Print(err)
    }
    // Output: emit macho dwarf: elf header corrupted


<a id="exampleNew_errorf"></a>
示例:

    const name, id = "bimmler", 17
    err := fmt.Errorf("user %q (id %d) not found", name, id)
    if err != nil {
        fmt.Print(err)
    }
    // Output: user "bimmler" (id 17) not found


