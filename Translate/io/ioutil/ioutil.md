version: 1.9.2
## package ioutil

  `import "io/ioutil"`

## 概述

ioutil 包实现了一些实用的 I/O 函数。

## 索引

- [Variables](#pkg-variables)
- [func NopCloser(r io.Reader) io.ReadCloser](#NopCloser)
- [func ReadAll(r io.Reader) ([]byte, error)](#ReadAll)
- [func ReadDir(dirname string) ([]os.FileInfo, error)](#ReadDir)
- [func ReadFile(filename string) ([]byte, error)](#ReadFile)
- [func TempDir(dir, prefix string) (name string, err error)](#TempDir)
- [func TempFile(dir, prefix string) (f *os.File, err error)](#TempFile)
- [func WriteFile(filename string, data []byte, perm os.FileMode) error](#WriteFile)

### 例子

- [ReadAll](#exampleReadAll)
- [ReadDir](#exampleReadDir)
- [ReadFile](#exampleReadFile)
- [TempDir](#exampleTempDir)
- [TempFile](#exampleTempFile)

### 文件
 [ioutil.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go) [tempfile.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/tempfile.go)

<h2 id="pkg-variables">变量</h2>

<pre>var <span id="Discard">Discard</span> <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a> = devNull(0)</pre>

Discard 是一个不做任何操作且永远返回成功的 io.Writer 实现。

<h2 id="NopCloser">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go#L105">NopCloser</a>
    <a href="#NopCloser">¶</a></h2>
<pre>func NopCloser(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a></pre>

NopCloser 函数把空 Close 方法和 r 封装成 ReadCloser 接口的实现返回。

<h2 id="ReadAll">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go#L31">ReadAll</a>
    <a href="#ReadAll">¶</a></h2>
<pre>func ReadAll(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadAll 函数从 r 读取数据直到遇到错误或者 EOF 并返回读取到的数据。读取成功返回 err == nil 而不是 err == EOF。因为 ReadAll 读取直到 EOF 的数据，所以它不会把 EOF 当作错误返回给用户。

<a id="exampleReadAll"></a>
例:

    r := strings.NewReader("Go is a general-purpose language designed with systems programming in mind.")

    b, err := ioutil.ReadAll(r)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s", b)

    // Output:
    // Go is a general-purpose language designed with systems programming in mind.

<h2 id="ReadDir">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go#L83">ReadDir</a>
    <a href="#ReadDir">¶</a></h2>
<pre>func ReadDir(dirname <a href="/builtin/#string">string</a>) ([]<a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)</pre>

ReadDir 函数从文件夹 dirname 中读取以文件名排序的文件信息列表。

<a id="exampleReadDir"></a>
例:

    files, err := ioutil.ReadDir(".")
    if err != nil {
        log.Fatal(err)
    }

    for _, file := range files {
        fmt.Println(file.Name())
    }

<h2 id="ReadFile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go#L39">ReadFile</a>
    <a href="#ReadFile">¶</a></h2>
<pre>func ReadFile(filename <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFile 函数读取文件 filename 中的内容。读取成功返回 err == nil 而不是 err == EOF。因为 ReadFile 会读取整个文件的内容，所以不会将 EOF 当作一个错误返回。

<a id="exampleReadFile"></a>
例:

    content, err := ioutil.ReadFile("testdata/hello")
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("File contents: %s", content)

    // Output:
    // File contents: Hello, Gophers!

<h2 id="TempDir">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/tempfile.go#L66">TempDir</a>
    <a href="#TempDir">¶</a></h2>
<pre>func TempDir(dir, prefix <a href="/builtin/#string">string</a>) (name <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

TempDir 在 dir 中创建一个名字以 prefix 作为前缀的临时文件夹并返回临时文件夹的路径。如果 dir 为空，TempDir 会使用临时文件的默认路径（详情请看 os.TempDir）。多个程序同时调用 TempDir 不会使用同一个文件夹。当用户不需要这个临时目录时需要手动删除。

<a id="exampleTempDir"></a>
例:

    content := []byte("temporary file's content")
    dir, err := ioutil.TempDir("", "example")
    if err != nil {
        log.Fatal(err)
    }

    defer os.RemoveAll(dir) // clean up

    tmpfn := filepath.Join(dir, "tmpfile")
    if err := ioutil.WriteFile(tmpfn, content, 0666); err != nil {
        log.Fatal(err)
    }

<h2 id="TempFile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/tempfile.go#L37">TempFile</a>
    <a href="#TempFile">¶</a></h2>
<pre>func TempFile(dir, prefix <a href="/builtin/#string">string</a>) (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

TempFile 函数在 dir 中创建前缀为 prefix 的临时文件，并打开该文件进行读写，返回 *os.File。如果 dir 为空，将使用默认的文件夹（详情请看 os.TempDir）。多个程序调用 TempFile 函数不会使用同一个文件。用户可以使用 f.Name() 来获取文件路径。当用户不再需要该文件时需要手动删除。 

<a id="exampleTempFile"></a>
例:

    content := []byte("temporary file's content")
    tmpfile, err := ioutil.TempFile("", "example")
    if err != nil {
        log.Fatal(err)
    }

    defer os.Remove(tmpfile.Name()) // clean up

    if _, err := tmpfile.Write(content); err != nil {
        log.Fatal(err)
    }
    if err := tmpfile.Close(); err != nil {
        log.Fatal(err)
    }

<h2 id="WriteFile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go#L66">WriteFile</a>
    <a href="#WriteFile">¶</a></h2>
<pre>func WriteFile(filename <a href="/builtin/#string">string</a>, data []<a href="/builtin/#byte">byte</a>, perm <a href="/os/">os</a>.<a href="/os/#FileMode">FileMode</a>) <a href="/builtin/#error">error</a></pre>

WriteFile 将数据写进文件 filename 中。该文件不存在时 WriteFile 会创建这个文件（需要有相应权限），如果文件已经存在 WriteFile 将会在写入之前覆盖它。


