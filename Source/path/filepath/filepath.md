version: 1.9.2
## package filepath

  `import "path/filepath"`

## Overview

Package filepath implements utility routines for manipulating filename paths in
a way compatible with the target operating system-defined file paths.

The filepath package uses either forward slashes or backslashes, depending on
the operating system. To process paths such as URLs that always use forward
slashes regardless of the operating system, see the path package.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func Abs(path string) (string, error)](#Abs)
- [func Base(path string) string](#Base)
- [func Clean(path string) string](#Clean)
- [func Dir(path string) string](#Dir)
- [func EvalSymlinks(path string) (string, error)](#EvalSymlinks)
- [func Ext(path string) string](#Ext)
- [func FromSlash(path string) string](#FromSlash)
- [func Glob(pattern string) (matches []string, err error)](#Glob)
- [func HasPrefix(p, prefix string) bool](#HasPrefix)
- [func IsAbs(path string) bool](#IsAbs)
- [func Join(elem ...string) string](#Join)
- [func Match(pattern, name string) (matched bool, err error)](#Match)
- [func Rel(basepath, targpath string) (string, error)](#Rel)
- [func Split(path string) (dir, file string)](#Split)
- [func SplitList(path string) []string](#SplitList)
- [func ToSlash(path string) string](#ToSlash)
- [func VolumeName(path string) string](#VolumeName)
- [func Walk(root string, walkFn WalkFunc) error](#Walk)
- [type WalkFunc](#WalkFunc)

### Examples

- [Join](#example_Join)
- [Rel](#example_Rel)
- [Split](#example_Split)
- [SplitList](#example_SplitList)

### Package files
 [match.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/match.go) [path.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go) [path_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path_unix.go) [symlink.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/symlink.go) [symlink_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/symlink_unix.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="Separator">Separator</span>     = <a href="/os/">os</a>.<a href="/os/#PathSeparator">PathSeparator</a>
    <span id="ListSeparator">ListSeparator</span> = <a href="/os/">os</a>.<a href="/os/#PathListSeparator">PathListSeparator</a>
)</pre>


<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrBadPattern">ErrBadPattern</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;syntax error in pattern&#34;)</pre>

ErrBadPattern indicates a globbing pattern was malformed.

<pre>var <span id="SkipDir">SkipDir</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;skip this directory&#34;)</pre>

SkipDir is used as a return value from WalkFuncs to indicate that the directory
named in the call is to be skipped. It is not returned as an error by any
function.

<h2 id="Abs">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L230">Abs</a>
    <a href="#Abs">¶</a></h2>
<pre>func Abs(path <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

Abs returns an absolute representation of path. If the path is not absolute it
will be joined with the current working directory to turn it into an absolute
path. The absolute path name for a given file is not guaranteed to be unique.
Abs calls Clean on the result.

<h2 id="Base">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L421">Base</a>
    <a href="#Base">¶</a></h2>
<pre>func Base(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Base returns the last element of path. Trailing path separators are removed
before extracting the last element. If the path is empty, Base returns ".". If
the path consists entirely of separators, Base returns a single separator.

<h2 id="Clean">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L78">Clean</a>
    <a href="#Clean">¶</a></h2>
<pre>func Clean(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Clean returns the shortest path name equivalent to path by purely lexical
processing. It applies the following rules iteratively until no further
processing can be done:

    1. Replace multiple Separator elements with a single one.
    2. Eliminate each . path name element (the current directory).
    3. Eliminate each inner .. path name element (the parent directory)
       along with the non-.. element that precedes it.
    4. Eliminate .. elements that begin a rooted path:
       that is, replace "/.." by "/" at the beginning of a path,
       assuming Separator is '/'.

The returned path ends in a slash only if it represents a root directory, such
as "/" on Unix or `C:\` on Windows.

Finally, any occurrences of slash are replaced by Separator.

If the result of this process is an empty string, Clean returns the string ".".

See also Rob Pike, ``Lexical File Names in Plan 9 or Getting Dot-Dot Right,''
https://9p.io/sys/doc/lexnames.html

<h2 id="Dir">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L452">Dir</a>
    <a href="#Dir">¶</a></h2>
<pre>func Dir(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Dir returns all but the last element of path, typically the path's directory.
After dropping the final element, Dir calls Clean on the path and trailing
slashes are removed. If the path is empty, Dir returns ".". If the path consists
entirely of separators, Dir returns a single separator. The returned path does
not end in a separator unless it is the root directory.

<h2 id="EvalSymlinks">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L221">EvalSymlinks</a>
    <a href="#EvalSymlinks">¶</a></h2>
<pre>func EvalSymlinks(path <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

EvalSymlinks returns the path name after the evaluation of any symbolic links.
If path is relative the result will be relative to the current directory, unless
one of the components is an absolute symbolic link. EvalSymlinks calls Clean on
the result.

<h2 id="Ext">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L207">Ext</a>
    <a href="#Ext">¶</a></h2>
<pre>func Ext(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Ext returns the file name extension used by path. The extension is the suffix
beginning at the final dot in the final element of path; it is empty if there is
no dot.

<h2 id="FromSlash">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L165">FromSlash</a>
    <a href="#FromSlash">¶</a></h2>
<pre>func FromSlash(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

FromSlash returns the result of replacing each slash ('/') character in path
with a separator character. Multiple slashes are replaced by multiple
separators.

<h2 id="Glob">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/match.go#L224">Glob</a>
    <a href="#Glob">¶</a></h2>
<pre>func Glob(pattern <a href="/builtin/#string">string</a>) (matches []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

Glob returns the names of all files matching pattern or nil if there is no
matching file. The syntax of patterns is the same as in Match. The pattern may
describe hierarchical names such as /usr/*/bin/ed (assuming the Separator is
'/').

Glob ignores file system errors such as I/O errors reading directories. The only
possible returned error is ErrBadPattern, when pattern is malformed.

<h2 id="HasPrefix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path_unix.go#L16">HasPrefix</a>
    <a href="#HasPrefix">¶</a></h2>
<pre>func HasPrefix(p, prefix <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

HasPrefix exists for historical compatibility and should not be used.

Deprecated: HasPrefix does not respect path boundaries and does not ignore case
when required.

<h2 id="IsAbs">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path_unix.go#L2">IsAbs</a>
    <a href="#IsAbs">¶</a></h2>
<pre>func IsAbs(path <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

IsAbs reports whether the path is absolute.

<h2 id="Join">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L199">Join</a>
    <a href="#Join">¶</a></h2>
<pre>func Join(elem ...<a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Join joins any number of path elements into a single path, adding a Separator if
necessary. Join calls Clean on the result; in particular, all empty strings are
ignored. On Windows, the result is a UNC path if and only if the first path
element is a UNC path.

<a id="example_Join"></a>
Example:

    fmt.Println("On Unix:")
    fmt.Println(filepath.Join("a", "b", "c"))
    fmt.Println(filepath.Join("a", "b/c"))
    fmt.Println(filepath.Join("a/b", "c"))
    fmt.Println(filepath.Join("a/b", "/c"))
    // Output:
    // On Unix:
    // a/b/c
    // a/b/c
    // a/b/c
    // a/b/c

<h2 id="Match">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/match.go#L34">Match</a>
    <a href="#Match">¶</a></h2>
<pre>func Match(pattern, name <a href="/builtin/#string">string</a>) (matched <a href="/builtin/#bool">bool</a>, err <a href="/builtin/#error">error</a>)</pre>

Match reports whether name matches the shell file name pattern. The pattern
syntax is:

    pattern:
    	{ term }
    term:
    	'*'         matches any sequence of non-Separator characters
    	'?'         matches any single non-Separator character
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

On Windows, escaping is disabled. Instead, '\\' is treated as path separator.

<h2 id="Rel">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L253">Rel</a>
    <a href="#Rel">¶</a></h2>
<pre>func Rel(basepath, targpath <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

Rel returns a relative path that is lexically equivalent to targpath when joined
to basepath with an intervening separator. That is, Join(basepath, Rel(basepath,
targpath)) is equivalent to targpath itself. On success, the returned path will
always be relative to basepath, even if basepath and targpath share no elements.
An error is returned if targpath can't be made relative to basepath or if
knowing the current working directory would be necessary to compute it. Rel
calls Clean on the result.

<a id="example_Rel"></a>
Example:

    paths := []string{
        "/a/b/c",
        "/b/c",
        "./b/c",
    }
    base := "/a"

    fmt.Println("On Unix:")
    for _, p := range paths {
        rel, err := filepath.Rel(base, p)
        fmt.Printf("%q: %q %v\n", p, rel, err)
    }

    // Output:
    // On Unix:
    // "/a/b/c": "b/c" <nil>
    // "/b/c": "../b/c" <nil>
    // "./b/c": "" Rel: can't make ./b/c relative to /a

<h2 id="Split">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L185">Split</a>
    <a href="#Split">¶</a></h2>
<pre>func Split(path <a href="/builtin/#string">string</a>) (dir, file <a href="/builtin/#string">string</a>)</pre>

Split splits path immediately following the final Separator, separating it into
a directory and file name component. If there is no Separator in path, Split
returns an empty dir and file set to path. The returned values have the property
that path = dir+file.

<a id="example_Split"></a>
Example:

    paths := []string{
        "/home/arnie/amelia.jpg",
        "/mnt/photos/",
        "rabbit.jpg",
        "/usr/local//go",
    }
    fmt.Println("On Unix:")
    for _, p := range paths {
        dir, file := filepath.Split(p)
        fmt.Printf("input: %q\n\tdir: %q\n\tfile: %q\n", p, dir, file)
    }
    // Output:
    // On Unix:
    // input: "/home/arnie/amelia.jpg"
    // 	dir: "/home/arnie/"
    // 	file: "amelia.jpg"
    // input: "/mnt/photos/"
    // 	dir: "/mnt/photos/"
    // 	file: ""
    // input: "rabbit.jpg"
    // 	dir: ""
    // 	file: "rabbit.jpg"
    // input: "/usr/local//go"
    // 	dir: "/usr/local//"
    // 	file: "go"

<h2 id="SplitList">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L176">SplitList</a>
    <a href="#SplitList">¶</a></h2>
<pre>func SplitList(path <a href="/builtin/#string">string</a>) []<a href="/builtin/#string">string</a></pre>

SplitList splits a list of paths joined by the OS-specific ListSeparator,
usually found in PATH or GOPATH environment variables. Unlike strings.Split,
SplitList returns an empty slice when passed an empty string.

<a id="example_SplitList"></a>
Example:

    fmt.Println("On Unix:", filepath.SplitList("/a/b/c:/usr/bin"))
    // Output:
    // On Unix: [/a/b/c /usr/bin]

<h2 id="ToSlash">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L155">ToSlash</a>
    <a href="#ToSlash">¶</a></h2>
<pre>func ToSlash(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ToSlash returns the result of replacing each separator character in path with a
slash ('/') character. Multiple separators are replaced by multiple slashes.

<h2 id="VolumeName">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L470">VolumeName</a>
    <a href="#VolumeName">¶</a></h2>
<pre>func VolumeName(path <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

VolumeName returns leading volume name. Given "C:\foo\bar" it returns "C:" on
Windows. Given "\\host\share\foo" it returns "\\host\share". On other platforms
it returns "".

<h2 id="Walk">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L388">Walk</a>
    <a href="#Walk">¶</a></h2>
<pre>func Walk(root <a href="/builtin/#string">string</a>, walkFn <a href="#WalkFunc">WalkFunc</a>) <a href="/builtin/#error">error</a></pre>

Walk walks the file tree rooted at root, calling walkFn for each file or
directory in the tree, including root. All errors that arise visiting files and
directories are filtered by walkFn. The files are walked in lexical order, which
makes the output deterministic but means that for very large directories Walk
can be inefficient. Walk does not follow symbolic links.

<h2 id="WalkFunc">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/path/filepath/path.go#L340">WalkFunc</a>
    <a href="#WalkFunc">¶</a></h2>
<pre>type WalkFunc func(path <a href="/builtin/#string">string</a>, info <a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a>, err <a href="/builtin/#error">error</a>) <a href="/builtin/#error">error</a></pre>

WalkFunc is the type of the function called for each file or directory visited
by Walk. The path argument contains the argument to Walk as a prefix; that is,
if Walk is called with "dir", which is a directory containing the file "a", the
walk function will be called with argument "dir/a". The info argument is the
os.FileInfo for the named path.

If there was a problem walking to the file or directory named by path, the
incoming error will describe the problem and the function can decide how to
handle that error (and Walk will not descend into that directory). If an error
is returned, processing stops. The sole exception is when the function returns
the special value SkipDir. If the function returns SkipDir when invoked on a
directory, Walk skips the directory's contents entirely. If the function returns
SkipDir when invoked on a non-directory file, Walk skips the remaining files in
the containing directory.


