version: 1.10
## package path

  `import "path"`

## Overview

Package path implements utility routines for manipulating slash-separated paths.

The path package should only be used for paths separated by forward slashes,
such as the paths in URLs. This package does not deal with Windows paths with
drive letters or backslashes; to manipulate operating system paths, use the
path/filepath package.

## Index

- [Variables](#pkg-variables)
- [func Base(path string) string](#Base)
- [func Clean(path string) string](#Clean)
- [func Dir(path string) string](#Dir)
- [func Ext(path string) string](#Ext)
- [func IsAbs(path string) bool](#IsAbs)
- [func Join(elem ...string) string](#Join)
- [func Match(pattern, name string) (matched bool, err error)](#Match)
- [func Split(path string) (dir, file string)](#Split)

### Examples

- [Base](#exampleBase)
- [Clean](#exampleClean)
- [Dir](#exampleDir)
- [Ext](#exampleExt)
- [IsAbs](#exampleIsAbs)
- [Join](#exampleJoin)
- [Split](#exampleSplit)

### Package files
 [match.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/match.go) [path.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/path.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrBadPattern">ErrBadPattern</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;syntax error in pattern&#34;)</pre>

ErrBadPattern indicates a globbing pattern was malformed.

<h2 id="Base">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/path.go#L171">Base</a>
    <a href="#Base">¶</a></h2>
<pre>func Base(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Base returns the last element of path. Trailing slashes are removed before
extracting the last element. If the path is empty, Base returns ".". If the path
consists entirely of slashes, Base returns "/".

<a id="exampleBase"></a>
Example:

    fmt.Println(path.Base("/a/b"))
    fmt.Println(path.Base("/"))
    fmt.Println(path.Base(""))
    // Output:
    // b
    // /
    // .

<h2 id="Clean">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/path.go#L64">Clean</a>
    <a href="#Clean">¶</a></h2>
<pre>func Clean(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Clean returns the shortest path name equivalent to path by purely lexical
processing. It applies the following rules iteratively until no further
processing can be done:

    1. Replace multiple slashes with a single slash.
    2. Eliminate each . path name element (the current directory).
    3. Eliminate each inner .. path name element (the parent directory)
       along with the non-.. element that precedes it.
    4. Eliminate .. elements that begin a rooted path:
       that is, replace "/.." by "/" at the beginning of a path.

The returned path ends in a slash only if it is the root "/".

If the result of this process is an empty string, Clean returns the string ".".

See also Rob Pike, ``Lexical File Names in Plan 9 or Getting Dot-Dot Right,''
https://9p.io/sys/doc/lexnames.html

<a id="exampleClean"></a>
Example:

    paths := []string{
        "a/c",
        "a//c",
        "a/c/.",
        "a/c/b/..",
        "/../a/c",
        "/../a/b/../././/c",
        "",
    }

    for _, p := range paths {
        fmt.Printf("Clean(%q) = %q\n", p, path.Clean(p))
    }

    // Output:
    // Clean("a/c") = "a/c"
    // Clean("a//c") = "a/c"
    // Clean("a/c/.") = "a/c"
    // Clean("a/c/b/..") = "a/c"
    // Clean("/../a/c") = "/a/c"
    // Clean("/../a/b/../././/c") = "/a/c"
    // Clean("") = "."

<h2 id="Dir">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/path.go#L202">Dir</a>
    <a href="#Dir">¶</a></h2>
<pre>func Dir(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Dir returns all but the last element of path, typically the path's directory.
After dropping the final element using Split, the path is Cleaned and trailing
slashes are removed. If the path is empty, Dir returns ".". If the path consists
entirely of slashes followed by non-slash bytes, Dir returns a single slash. In
any other case, the returned path does not end in a slash.

<a id="exampleDir"></a>
Example:

    fmt.Println(path.Dir("/a/b/c"))
    fmt.Println(path.Dir("a/b/c"))
    fmt.Println(path.Dir("/a/"))
    fmt.Println(path.Dir("a/"))
    fmt.Println(path.Dir("/"))
    fmt.Println(path.Dir(""))
    // Output:
    // /a/b
    // a/b
    // /a
    // a
    // /
    // .

<h2 id="Ext">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/path.go#L158">Ext</a>
    <a href="#Ext">¶</a></h2>
<pre>func Ext(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Ext returns the file name extension used by path. The extension is the suffix
beginning at the final dot in the final slash-separated element of path; it is
empty if there is no dot.

<a id="exampleExt"></a>
Example:

    fmt.Println(path.Ext("/a/b/c/bar.css"))
    fmt.Println(path.Ext("/"))
    fmt.Println(path.Ext(""))
    // Output:
    // .css
    //
    //

<h2 id="IsAbs">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/path.go#L191">IsAbs</a>
    <a href="#IsAbs">¶</a></h2>
<pre>func IsAbs(path <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

IsAbs reports whether the path is absolute.

<a id="exampleIsAbs"></a>
Example:

    fmt.Println(path.IsAbs("/dev/null"))
    // Output: true

<h2 id="Join">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/path.go#L145">Join</a>
    <a href="#Join">¶</a></h2>
<pre>func Join(elem ...<a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Join joins any number of path elements into a single path, adding a separating
slash if necessary. The result is Cleaned; in particular, all empty strings are
ignored.

<a id="exampleJoin"></a>
Example:

    fmt.Println(path.Join("a", "b", "c"))
    fmt.Println(path.Join("a", "b/c"))
    fmt.Println(path.Join("a/b", "c"))
    fmt.Println(path.Join("", ""))
    fmt.Println(path.Join("a", ""))
    fmt.Println(path.Join("", "a"))
    // Output:
    // a/b/c
    // a/b/c
    // a/b/c
    //
    // a
    // a

<h2 id="Match">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/match.go#L28">Match</a>
    <a href="#Match">¶</a></h2>
<pre>func Match(pattern, name <a href="/builtin/#string">string</a>) (matched <a href="/builtin/#bool">bool</a>, err <a href="/builtin/#error">error</a>)</pre>

Match reports whether name matches the shell file name pattern. The pattern
syntax is:

    pattern:
    	{ term }
    term:
    	'*'         matches any sequence of non-/ characters
    	'?'         matches any single non-/ character
    	'[' [ '^' ] { character-range } ']'
    	            character class (must be non-empty)
    	c           matches character c (c != '*', '?', '\\', '[')
    	'\\' c      matches character c

    character-range:
    	c           matches character c (c != '\\', '-', ']')
    	'\\' c      matches character c
    	lo '-' hi   matches character c for lo <= c <= hi

Match requires pattern to match all of name, not just a substring. The only
possible returned error is ErrBadPattern, when pattern is malformed.

<h2 id="Split">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/path.go#L137">Split</a>
    <a href="#Split">¶</a></h2>
<pre>func Split(path <a href="/builtin/#string">string</a>) (dir, file <a href="/builtin/#string">string</a>)</pre>

Split splits path immediately following the final slash, separating it into a
directory and file name component. If there is no slash in path, Split returns
an empty dir and file set to path. The returned values have the property that
path = dir+file.

<a id="exampleSplit"></a>
Example:

    fmt.Println(path.Split("static/myfile.css"))
    fmt.Println(path.Split("myfile.css"))
    fmt.Println(path.Split(""))
    // Output:
    // static/ myfile.css
    //  myfile.css
    //

## Subdirectories
- [..](..)
- [filepath](filepath/)
