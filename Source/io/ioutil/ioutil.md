version: 1.10
## package ioutil

  `import "io/ioutil"`

## Overview

Package ioutil implements some I/O utility functions.

## Index

- [Variables](#pkg-variables)
- [func NopCloser(r io.Reader) io.ReadCloser](#NopCloser)
- [func ReadAll(r io.Reader) ([]byte, error)](#ReadAll)
- [func ReadDir(dirname string) ([]os.FileInfo, error)](#ReadDir)
- [func ReadFile(filename string) ([]byte, error)](#ReadFile)
- [func TempDir(dir, prefix string) (name string, err error)](#TempDir)
- [func TempFile(dir, prefix string) (f *os.File, err error)](#TempFile)
- [func WriteFile(filename string, data []byte, perm os.FileMode) error](#WriteFile)

### Examples

- [ReadAll](#exampleReadAll)
- [ReadDir](#exampleReadDir)
- [ReadFile](#exampleReadFile)
- [TempDir](#exampleTempDir)
- [TempFile](#exampleTempFile)

### Package files
 [ioutil.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go) [tempfile.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/tempfile.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="Discard">Discard</span> <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a> = devNull(0)</pre>

Discard is an io.Writer on which all Write calls succeed without doing anything.

<h2 id="NopCloser">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go#L108">NopCloser</a>
    <a href="#NopCloser">¶</a></h2>
<pre>func NopCloser(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a></pre>

NopCloser returns a ReadCloser with a no-op Close method wrapping the provided
Reader r.

<h2 id="ReadAll">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go#L34">ReadAll</a>
    <a href="#ReadAll">¶</a></h2>
<pre>func ReadAll(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadAll reads from r until an error or EOF and returns the data it read. A
successful call returns err == nil, not err == EOF. Because ReadAll is defined
to read from src until EOF, it does not treat an EOF from Read as an error to be
reported.

<a id="exampleReadAll"></a>
Example:

    r := strings.NewReader("Go is a general-purpose language designed with systems programming in mind.")

    b, err := ioutil.ReadAll(r)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s", b)

    // Output:
    // Go is a general-purpose language designed with systems programming in mind.

<h2 id="ReadDir">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go#L86">ReadDir</a>
    <a href="#ReadDir">¶</a></h2>
<pre>func ReadDir(dirname <a href="/builtin/#string">string</a>) ([]<a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)</pre>

ReadDir reads the directory named by dirname and returns a list of directory
entries sorted by filename.

<a id="exampleReadDir"></a>
Example:

    files, err := ioutil.ReadDir(".")
    if err != nil {
        log.Fatal(err)
    }

    for _, file := range files {
        fmt.Println(file.Name())
    }

<h2 id="ReadFile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go#L42">ReadFile</a>
    <a href="#ReadFile">¶</a></h2>
<pre>func ReadFile(filename <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFile reads the file named by filename and returns the contents. A successful
call returns err == nil, not err == EOF. Because ReadFile reads the whole file,
it does not treat an EOF from Read as an error to be reported.

<a id="exampleReadFile"></a>
Example:

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

TempDir creates a new temporary directory in the directory dir with a name
beginning with prefix and returns the path of the new directory. If dir is the
empty string, TempDir uses the default directory for temporary files (see
os.TempDir). Multiple programs calling TempDir simultaneously will not choose
the same directory. It is the caller's responsibility to remove the directory
when no longer needed.

<a id="exampleTempDir"></a>
Example:

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

TempFile creates a new temporary file in the directory dir with a name beginning
with prefix, opens the file for reading and writing, and returns the resulting
*os.File. If dir is the empty string, TempFile uses the default directory for
temporary files (see os.TempDir). Multiple programs calling TempFile
simultaneously will not choose the same file. The caller can use f.Name() to
find the pathname of the file. It is the caller's responsibility to remove the
file when no longer needed.

<a id="exampleTempFile"></a>
Example:

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

<h2 id="WriteFile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/io/ioutil/ioutil.go#L69">WriteFile</a>
    <a href="#WriteFile">¶</a></h2>
<pre>func WriteFile(filename <a href="/builtin/#string">string</a>, data []<a href="/builtin/#byte">byte</a>, perm <a href="/os/">os</a>.<a href="/os/#FileMode">FileMode</a>) <a href="/builtin/#error">error</a></pre>

WriteFile writes data to a file named by filename. If the file does not exist,
WriteFile creates it with permissions perm; otherwise WriteFile truncates it
before writing.


