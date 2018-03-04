version: 1.10
## package build

  `import "go/build"`

## Overview

Package build gathers information about Go packages.


Go Path

The Go path is a list of directory trees containing Go source code. It is
consulted to resolve imports that cannot be found in the standard Go tree. The
default path is the value of the GOPATH environment variable, interpreted as a
path list appropriate to the operating system (on Unix, the variable is a
colon-separated string; on Windows, a semicolon-separated string; on Plan 9, a
list).

Each directory listed in the Go path must have a prescribed structure:

The src/ directory holds source code. The path below 'src' determines the import
path or executable name.

The pkg/ directory holds installed package objects. As in the Go tree, each
target operating system and architecture pair has its own subdirectory of pkg
(pkg/GOOS_GOARCH).

If DIR is a directory listed in the Go path, a package with source in
DIR/src/foo/bar can be imported as "foo/bar" and has its compiled form installed
to "DIR/pkg/GOOS_GOARCH/foo/bar.a" (or, for gccgo,
"DIR/pkg/gccgo/foo/libbar.a").

The bin/ directory holds compiled commands. Each command is named for its source
directory, but only using the final element, not the entire path. That is, the
command with source in DIR/src/foo/quux is installed into DIR/bin/quux, not
DIR/bin/foo/quux. The foo/ is stripped so that you can add DIR/bin to your PATH
to get at the installed commands.

Here's an example directory layout:

    GOPATH=/home/user/gocode

    /home/user/gocode/
        src/
            foo/
                bar/               (go code in package bar)
                    x.go
                quux/              (go code in package main)
                    y.go
        bin/
            quux                   (installed command)
        pkg/
            linux_amd64/
                foo/
                    bar.a          (installed package object)


Build Constraints

A build constraint, also known as a build tag, is a line comment that begins

    // +build

that lists the conditions under which a file should be included in the package.
Constraints may appear in any kind of source file (not just Go), but they must
appear near the top of the file, preceded only by blank lines and other line
comments. These rules mean that in Go files a build constraint must appear
before the package clause.

To distinguish build constraints from package documentation, a series of build
constraints must be followed by a blank line.

A build constraint is evaluated as the OR of space-separated options; each
option evaluates as the AND of its comma-separated terms; and each term is an
alphanumeric word or, preceded by !, its negation. That is, the build
constraint:

    // +build linux,386 darwin,!cgo

corresponds to the boolean formula:

    (linux AND 386) OR (darwin AND (NOT cgo))

A file may have multiple build constraints. The overall constraint is the AND of
the individual constraints. That is, the build constraints:

    // +build linux darwin
    // +build 386

corresponds to the boolean formula:

    (linux OR darwin) AND 386

During a particular build, the following words are satisfied:

    - the target operating system, as spelled by runtime.GOOS
    - the target architecture, as spelled by runtime.GOARCH
    - the compiler being used, either "gc" or "gccgo"
    - "cgo", if ctxt.CgoEnabled is true
    - "go1.1", from Go version 1.1 onward
    - "go1.2", from Go version 1.2 onward
    - "go1.3", from Go version 1.3 onward
    - "go1.4", from Go version 1.4 onward
    - "go1.5", from Go version 1.5 onward
    - "go1.6", from Go version 1.6 onward
    - "go1.7", from Go version 1.7 onward
    - "go1.8", from Go version 1.8 onward
    - "go1.9", from Go version 1.9 onward
    - "go1.10", from Go version 1.10 onward
    - any additional words listed in ctxt.BuildTags

If a file's name, after stripping the extension and a possible _test suffix,
matches any of the following patterns:

    *_GOOS
    *_GOARCH
    *_GOOS_GOARCH

(example: source_windows_amd64.go) where GOOS and GOARCH represent any known
operating system and architecture values respectively, then the file is
considered to have an implicit build constraint requiring those terms (in
addition to any explicit constraints in the file).

To keep a file from being considered for the build:

    // +build ignore

(any other unsatisfied word will work as well, but ``ignore'' is conventional.)

To build a file only when using cgo, and only on Linux and OS X:

    // +build linux,cgo darwin,cgo

Such a file is usually paired with another file implementing the default
functionality for other systems, which in this case would carry the constraint:

    // +build !linux,!darwin !cgo

Naming a file dns_windows.go will cause it to be included only when building the
package for Windows; similarly, math_386.s will be included only when building
the package for 32-bit x86.

Using GOOS=android matches build tags and files as for GOOS=linux in addition to
android tags and files.


Binary-Only Packages

It is possible to distribute packages in binary form without including the
source code used for compiling the package. To do this, the package must be
distributed with a source file not excluded by build constraints and containing
a "//go:binary-only-package" comment. Like a build constraint, this comment must
appear near the top of the file, preceded only by blank lines and other line
comments and with a blank line following the comment, to separate it from the
package documentation. Unlike build constraints, this comment is only recognized
in non-test Go source files.

The minimal source code for a binary-only package is therefore:

    //go:binary-only-package

    package mypkg

The source code may include additional Go code. That code is never compiled but
will be processed by tools like godoc and might be useful as end-user
documentation.

## Index

- [Variables](#pkg-variables)
- [func ArchChar(goarch string) (string, error)](#ArchChar)
- [func IsLocalImport(path string) bool](#IsLocalImport)
- [type Context](#Context)
  - [func (ctxt *Context) Import(path string, srcDir string, mode ImportMode) (*Package, error)](#Context.Import)
  - [func (ctxt *Context) ImportDir(dir string, mode ImportMode) (*Package, error)](#Context.ImportDir)
  - [func (ctxt *Context) MatchFile(dir, name string) (match bool, err error)](#Context.MatchFile)
  - [func (ctxt *Context) SrcDirs() []string](#Context.SrcDirs)
- [type ImportMode](#ImportMode)
- [type MultiplePackageError](#MultiplePackageError)
  - [func (e *MultiplePackageError) Error() string](#MultiplePackageError.Error)
- [type NoGoError](#NoGoError)
  - [func (e *NoGoError) Error() string](#NoGoError.Error)
- [type Package](#Package)
  - [func Import(path, srcDir string, mode ImportMode) (*Package, error)](#Import)
  - [func ImportDir(dir string, mode ImportMode) (*Package, error)](#ImportDir)
  - [func (p *Package) IsCommand() bool](#Package.IsCommand)

### Package files
 [build.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go) [doc.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/build/doc.go) [read.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/build/read.go) [syslist.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/build/syslist.go) [zcgo.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/build/zcgo.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ToolDir">ToolDir</span> = <a href="/path/filepath/">filepath</a>.<a href="/path/filepath/#Join">Join</a>(<a href="/runtime/">runtime</a>.<a href="/runtime/#GOROOT">GOROOT</a>(), &#34;pkg/tool/&#34;+<a href="/runtime/">runtime</a>.<a href="/runtime/#GOOS">GOOS</a>+&#34;_&#34;+<a href="/runtime/">runtime</a>.<a href="/runtime/#GOARCH">GOARCH</a>)</pre>

ToolDir is the directory containing build tools.

<h2 id="ArchChar">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L1601">ArchChar</a>
    <a href="#ArchChar">¶</a></h2>
<pre>func ArchChar(goarch <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ArchChar returns "?" and an error. In earlier versions of Go, the returned
string was used to derive the compiler and linker tool names, the default object
file suffix, and the default linker output name. As of Go 1.5, those strings no
longer vary by architecture; they are compile, link, .o, and a.out,
respectively.

<h2 id="IsLocalImport">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L1591">IsLocalImport</a>
    <a href="#IsLocalImport">¶</a></h2>
<pre>func IsLocalImport(path <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

IsLocalImport reports whether the import path is a local import path, like ".",
"..", "./foo", or "../foo".

<h2 id="Context">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L20">Context</a>
    <a href="#Context">¶</a></h2>
<pre>type Context struct {
<span id="Context.GOARCH"></span>    GOARCH      <a href="/builtin/#string">string</a> <span class="comment">// target architecture</span>
<span id="Context.GOOS"></span>    GOOS        <a href="/builtin/#string">string</a> <span class="comment">// target operating system</span>
<span id="Context.GOROOT"></span>    GOROOT      <a href="/builtin/#string">string</a> <span class="comment">// Go root</span>
<span id="Context.GOPATH"></span>    GOPATH      <a href="/builtin/#string">string</a> <span class="comment">// Go path</span>
<span id="Context.CgoEnabled"></span>    CgoEnabled  <a href="/builtin/#bool">bool</a>   <span class="comment">// whether cgo can be used</span>
<span id="Context.UseAllFiles"></span>    UseAllFiles <a href="/builtin/#bool">bool</a>   <span class="comment">// use files regardless of +build lines, file names</span>
<span id="Context.Compiler"></span>    Compiler    <a href="/builtin/#string">string</a> <span class="comment">// compiler to assume when computing target paths</span>

    <span class="comment">// The build and release tags specify build constraints</span>
    <span class="comment">// that should be considered satisfied when processing +build lines.</span>
    <span class="comment">// Clients creating a new context may customize BuildTags, which</span>
    <span class="comment">// defaults to empty, but it is usually an error to customize ReleaseTags,</span>
    <span class="comment">// which defaults to the list of Go releases the current release is compatible with.</span>
    <span class="comment">// In addition to the BuildTags and ReleaseTags, build constraints</span>
    <span class="comment">// consider the values of GOARCH and GOOS as satisfied tags.</span>
<span id="Context.BuildTags"></span>    BuildTags   []<a href="/builtin/#string">string</a>
<span id="Context.ReleaseTags"></span>    ReleaseTags []<a href="/builtin/#string">string</a>

    <span class="comment">// The install suffix specifies a suffix to use in the name of the installation</span>
    <span class="comment">// directory. By default it is empty, but custom builds that need to keep</span>
    <span class="comment">// their outputs separate can set InstallSuffix to do so. For example, when</span>
    <span class="comment">// using the race detector, the go command uses InstallSuffix = &#34;race&#34;, so</span>
    <span class="comment">// that on a Linux/386 system, packages are written to a directory named</span>
    <span class="comment">// &#34;linux_386_race&#34; instead of the usual &#34;linux_386&#34;.</span>
<span id="Context.InstallSuffix"></span>    InstallSuffix <a href="/builtin/#string">string</a>

<span id="Context.JoinPath"></span>    <span class="comment">// JoinPath joins the sequence of path fragments into a single path.</span>
    <span class="comment">// If JoinPath is nil, Import uses filepath.Join.</span>
    JoinPath func(elem ...<a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a>

<span id="Context.SplitPathList"></span>    <span class="comment">// SplitPathList splits the path list into a slice of individual paths.</span>
    <span class="comment">// If SplitPathList is nil, Import uses filepath.SplitList.</span>
    SplitPathList func(list <a href="/builtin/#string">string</a>) []<a href="/builtin/#string">string</a>

<span id="Context.IsAbsPath"></span>    <span class="comment">// IsAbsPath reports whether path is an absolute path.</span>
    <span class="comment">// If IsAbsPath is nil, Import uses filepath.IsAbs.</span>
    IsAbsPath func(path <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a>

<span id="Context.IsDir"></span>    <span class="comment">// IsDir reports whether the path names a directory.</span>
    <span class="comment">// If IsDir is nil, Import calls os.Stat and uses the result&#39;s IsDir method.</span>
    IsDir func(path <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a>

<span id="Context.HasSubdir"></span>    <span class="comment">// HasSubdir reports whether dir is lexically a subdirectory of</span>
    <span class="comment">// root, perhaps multiple levels below. It does not try to check</span>
    <span class="comment">// whether dir exists.</span>
    <span class="comment">// If so, HasSubdir sets rel to a slash-separated path that</span>
    <span class="comment">// can be joined to root to produce a path equivalent to dir.</span>
    <span class="comment">// If HasSubdir is nil, Import uses an implementation built on</span>
    <span class="comment">// filepath.EvalSymlinks.</span>
    HasSubdir func(root, dir <a href="/builtin/#string">string</a>) (rel <a href="/builtin/#string">string</a>, ok <a href="/builtin/#bool">bool</a>)

<span id="Context.ReadDir"></span>    <span class="comment">// ReadDir returns a slice of os.FileInfo, sorted by Name,</span>
    <span class="comment">// describing the content of the named directory.</span>
    <span class="comment">// If ReadDir is nil, Import uses ioutil.ReadDir.</span>
    ReadDir func(dir <a href="/builtin/#string">string</a>) ([]<a href="/os/">os</a>.<a href="/os/#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)

<span id="Context.OpenFile"></span>    <span class="comment">// OpenFile opens a file (not a directory) for reading.</span>
    <span class="comment">// If OpenFile is nil, Import uses os.Open.</span>
    OpenFile func(path <a href="/builtin/#string">string</a>) (<a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>, <a href="/builtin/#error">error</a>)
}</pre>

A Context specifies the supporting context for a build.

<pre>var <span id="Default">Default</span> <a href="#Context">Context</a> = defaultContext()</pre>

Default is the default Context for builds. It uses the GOARCH, GOOS, GOROOT, and
GOPATH environment variables if set, or else the compiled code's GOARCH, GOOS,
and GOROOT.

<h3 id="Context.Import">func (*Context) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L481">Import</a>
    <a href="#Context.Import">¶</a></h3>
<pre>func (ctxt *<a href="#Context">Context</a>) Import(path <a href="/builtin/#string">string</a>, srcDir <a href="/builtin/#string">string</a>, mode <a href="#ImportMode">ImportMode</a>) (*<a href="#Package">Package</a>, <a href="/builtin/#error">error</a>)</pre>

Import returns details about the Go package named by the import path,
interpreting local import paths relative to the srcDir directory. If the path is
a local import path naming a package that can be imported using a standard
import path, the returned package will set p.ImportPath to that path.

In the directory containing the package, .go, .c, .h, and .s files are
considered part of the package except for:

    - .go files in package documentation
    - files starting with _ or . (likely editor temporary files)
    - files with build constraints not satisfied by the context

If an error occurs, Import returns a non-nil error and a non-nil *Package
containing partial information.

<h3 id="Context.ImportDir">func (*Context) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L429">ImportDir</a>
    <a href="#Context.ImportDir">¶</a></h3>
<pre>func (ctxt *<a href="#Context">Context</a>) ImportDir(dir <a href="/builtin/#string">string</a>, mode <a href="#ImportMode">ImportMode</a>) (*<a href="#Package">Package</a>, <a href="/builtin/#error">error</a>)</pre>

ImportDir is like Import but processes the Go package found in the named
directory.

<h3 id="Context.MatchFile">func (*Context) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L1040">MatchFile</a>
    <a href="#Context.MatchFile">¶</a></h3>
<pre>func (ctxt *<a href="#Context">Context</a>) MatchFile(dir, name <a href="/builtin/#string">string</a>) (match <a href="/builtin/#bool">bool</a>, err <a href="/builtin/#error">error</a>)</pre>

MatchFile reports whether the file with the given name in the given directory
matches the context and would be included in a Package created by ImportDir of
that directory.

MatchFile considers the name of the file and may use ctxt.OpenFile to read some
or all of the file's content.

<h3 id="Context.SrcDirs">func (*Context) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L229">SrcDirs</a>
    <a href="#Context.SrcDirs">¶</a></h3>
<pre>func (ctxt *<a href="#Context">Context</a>) SrcDirs() []<a href="/builtin/#string">string</a></pre>

SrcDirs returns a list of package source root directories. It draws from the
current Go root and Go path but omits directories that do not exist.

<h2 id="ImportMode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L320">ImportMode</a>
    <a href="#ImportMode">¶</a></h2>
<pre>type ImportMode <a href="/builtin/#uint">uint</a></pre>

An ImportMode controls the behavior of the Import method.

<pre>const (
    <span class="comment">// If FindOnly is set, Import stops after locating the directory</span>
    <span class="comment">// that should contain the sources for a package. It does not</span>
    <span class="comment">// read any files in the directory.</span>
    <span id="FindOnly">FindOnly</span> <a href="#ImportMode">ImportMode</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a>

    <span class="comment">// If AllowBinary is set, Import can be satisfied by a compiled</span>
    <span class="comment">// package object without corresponding sources.</span>
    <span class="comment">//</span>
    <span class="comment">// Deprecated:</span>
    <span class="comment">// The supported way to create a compiled-only package is to</span>
    <span class="comment">// write source code containing a //go:binary-only-package comment at</span>
    <span class="comment">// the top of the file. Such a package will be recognized</span>
    <span class="comment">// regardless of this flag setting (because it has source code)</span>
    <span class="comment">// and will have BinaryOnly set to true in the returned Package.</span>
    <span id="AllowBinary">AllowBinary</span>

    <span class="comment">// If ImportComment is set, parse import comments on package statements.</span>
    <span class="comment">// Import returns an error if it finds a comment it cannot understand</span>
    <span class="comment">// or finds conflicting comments in multiple source files.</span>
    <span class="comment">// See golang.org/s/go14customimport for more information.</span>
    <span id="ImportComment">ImportComment</span>

    <span class="comment">// By default, Import searches vendor directories</span>
    <span class="comment">// that apply in the given source directory before searching</span>
    <span class="comment">// the GOROOT and GOPATH roots.</span>
    <span class="comment">// If an Import finds and returns a package using a vendor</span>
    <span class="comment">// directory, the resulting ImportPath is the complete path</span>
    <span class="comment">// to the package, including the path elements leading up</span>
    <span class="comment">// to and including &#34;vendor&#34;.</span>
    <span class="comment">// For example, if Import(&#34;y&#34;, &#34;x/subdir&#34;, 0) finds</span>
    <span class="comment">// &#34;x/vendor/y&#34;, the returned package&#39;s ImportPath is &#34;x/vendor/y&#34;,</span>
    <span class="comment">// not plain &#34;y&#34;.</span>
    <span class="comment">// See golang.org/s/go15vendor for more information.</span>
    <span class="comment">//</span>
    <span class="comment">// Setting IgnoreVendor ignores vendor directories.</span>
    <span class="comment">//</span>
    <span class="comment">// In contrast to the package&#39;s ImportPath,</span>
    <span class="comment">// the returned package&#39;s Imports, TestImports, and XTestImports</span>
    <span class="comment">// are always the exact import paths from the source files:</span>
    <span class="comment">// Import makes no attempt to resolve or check those paths.</span>
    <span id="IgnoreVendor">IgnoreVendor</span>
)</pre>


<h2 id="MultiplePackageError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L446">MultiplePackageError</a>
    <a href="#MultiplePackageError">¶</a></h2>
<pre>type MultiplePackageError struct {
<span id="MultiplePackageError.Dir"></span>    Dir      <a href="/builtin/#string">string</a>   <span class="comment">// directory containing files</span>
<span id="MultiplePackageError.Packages"></span>    Packages []<a href="/builtin/#string">string</a> <span class="comment">// package names found</span>
<span id="MultiplePackageError.Files"></span>    Files    []<a href="/builtin/#string">string</a> <span class="comment">// corresponding files: Files[i] declares package Packages[i]</span>
}</pre>

MultiplePackageError describes a directory containing multiple buildable Go
source files for multiple packages.

<h3 id="MultiplePackageError.Error">func (*MultiplePackageError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L452">Error</a>
    <a href="#MultiplePackageError.Error">¶</a></h3>
<pre>func (e *<a href="#MultiplePackageError">MultiplePackageError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="NoGoError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L436">NoGoError</a>
    <a href="#NoGoError">¶</a></h2>
<pre>type NoGoError struct {
<span id="NoGoError.Dir"></span>    Dir <a href="/builtin/#string">string</a>
}</pre>

NoGoError is the error used by Import to describe a directory containing no
buildable Go source files. (It may still contain test files, files hidden by
build tags, and so on.)

<h3 id="NoGoError.Error">func (*NoGoError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L440">Error</a>
    <a href="#NoGoError.Error">¶</a></h3>
<pre>func (e *<a href="#NoGoError">NoGoError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Package">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L367">Package</a>
    <a href="#Package">¶</a></h2>
<pre>type Package struct {
<span id="Package.Dir"></span>    Dir           <a href="/builtin/#string">string</a>   <span class="comment">// directory containing package sources</span>
<span id="Package.Name"></span>    Name          <a href="/builtin/#string">string</a>   <span class="comment">// package name</span>
<span id="Package.ImportComment"></span>    ImportComment <a href="/builtin/#string">string</a>   <span class="comment">// path in import comment on package statement</span>
<span id="Package.Doc"></span>    Doc           <a href="/builtin/#string">string</a>   <span class="comment">// documentation synopsis</span>
<span id="Package.ImportPath"></span>    ImportPath    <a href="/builtin/#string">string</a>   <span class="comment">// import path of package (&#34;&#34; if unknown)</span>
<span id="Package.Root"></span>    Root          <a href="/builtin/#string">string</a>   <span class="comment">// root of Go tree where this package lives</span>
<span id="Package.SrcRoot"></span>    SrcRoot       <a href="/builtin/#string">string</a>   <span class="comment">// package source root directory (&#34;&#34; if unknown)</span>
<span id="Package.PkgRoot"></span>    PkgRoot       <a href="/builtin/#string">string</a>   <span class="comment">// package install root directory (&#34;&#34; if unknown)</span>
<span id="Package.PkgTargetRoot"></span>    PkgTargetRoot <a href="/builtin/#string">string</a>   <span class="comment">// architecture dependent install root directory (&#34;&#34; if unknown)</span>
<span id="Package.BinDir"></span>    BinDir        <a href="/builtin/#string">string</a>   <span class="comment">// command install directory (&#34;&#34; if unknown)</span>
<span id="Package.Goroot"></span>    Goroot        <a href="/builtin/#bool">bool</a>     <span class="comment">// package found in Go root</span>
<span id="Package.PkgObj"></span>    PkgObj        <a href="/builtin/#string">string</a>   <span class="comment">// installed .a file</span>
<span id="Package.AllTags"></span>    AllTags       []<a href="/builtin/#string">string</a> <span class="comment">// tags that can influence file selection in this directory</span>
<span id="Package.ConflictDir"></span>    ConflictDir   <a href="/builtin/#string">string</a>   <span class="comment">// this directory shadows Dir in $GOPATH</span>
<span id="Package.BinaryOnly"></span>    BinaryOnly    <a href="/builtin/#bool">bool</a>     <span class="comment">// cannot be rebuilt from source (has //go:binary-only-package comment)</span>

    <span class="comment">// Source files</span>
<span id="Package.GoFiles"></span>    GoFiles        []<a href="/builtin/#string">string</a> <span class="comment">// .go source files (excluding CgoFiles, TestGoFiles, XTestGoFiles)</span>
<span id="Package.CgoFiles"></span>    CgoFiles       []<a href="/builtin/#string">string</a> <span class="comment">// .go source files that import &#34;C&#34;</span>
<span id="Package.IgnoredGoFiles"></span>    IgnoredGoFiles []<a href="/builtin/#string">string</a> <span class="comment">// .go source files ignored for this build</span>
<span id="Package.InvalidGoFiles"></span>    InvalidGoFiles []<a href="/builtin/#string">string</a> <span class="comment">// .go source files with detected problems (parse error, wrong package name, and so on)</span>
<span id="Package.CFiles"></span>    CFiles         []<a href="/builtin/#string">string</a> <span class="comment">// .c source files</span>
<span id="Package.CXXFiles"></span>    CXXFiles       []<a href="/builtin/#string">string</a> <span class="comment">// .cc, .cpp and .cxx source files</span>
<span id="Package.MFiles"></span>    MFiles         []<a href="/builtin/#string">string</a> <span class="comment">// .m (Objective-C) source files</span>
<span id="Package.HFiles"></span>    HFiles         []<a href="/builtin/#string">string</a> <span class="comment">// .h, .hh, .hpp and .hxx source files</span>
<span id="Package.FFiles"></span>    FFiles         []<a href="/builtin/#string">string</a> <span class="comment">// .f, .F, .for and .f90 Fortran source files</span>
<span id="Package.SFiles"></span>    SFiles         []<a href="/builtin/#string">string</a> <span class="comment">// .s source files</span>
<span id="Package.SwigFiles"></span>    SwigFiles      []<a href="/builtin/#string">string</a> <span class="comment">// .swig files</span>
<span id="Package.SwigCXXFiles"></span>    SwigCXXFiles   []<a href="/builtin/#string">string</a> <span class="comment">// .swigcxx files</span>
<span id="Package.SysoFiles"></span>    SysoFiles      []<a href="/builtin/#string">string</a> <span class="comment">// .syso system object files to add to archive</span>

    <span class="comment">// Cgo directives</span>
<span id="Package.CgoCFLAGS"></span>    CgoCFLAGS    []<a href="/builtin/#string">string</a> <span class="comment">// Cgo CFLAGS directives</span>
<span id="Package.CgoCPPFLAGS"></span>    CgoCPPFLAGS  []<a href="/builtin/#string">string</a> <span class="comment">// Cgo CPPFLAGS directives</span>
<span id="Package.CgoCXXFLAGS"></span>    CgoCXXFLAGS  []<a href="/builtin/#string">string</a> <span class="comment">// Cgo CXXFLAGS directives</span>
<span id="Package.CgoFFLAGS"></span>    CgoFFLAGS    []<a href="/builtin/#string">string</a> <span class="comment">// Cgo FFLAGS directives</span>
<span id="Package.CgoLDFLAGS"></span>    CgoLDFLAGS   []<a href="/builtin/#string">string</a> <span class="comment">// Cgo LDFLAGS directives</span>
<span id="Package.CgoPkgConfig"></span>    CgoPkgConfig []<a href="/builtin/#string">string</a> <span class="comment">// Cgo pkg-config directives</span>

    <span class="comment">// Dependency information</span>
<span id="Package.Imports"></span>    Imports   []<a href="/builtin/#string">string</a>                    <span class="comment">// import paths from GoFiles, CgoFiles</span>
<span id="Package.ImportPos"></span>    ImportPos map[<a href="/builtin/#string">string</a>][]<a href="/go/token/">token</a>.<a href="/go/token/#Position">Position</a> <span class="comment">// line information for Imports</span>

    <span class="comment">// Test information</span>
<span id="Package.TestGoFiles"></span>    TestGoFiles    []<a href="/builtin/#string">string</a>                    <span class="comment">// _test.go files in package</span>
<span id="Package.TestImports"></span>    TestImports    []<a href="/builtin/#string">string</a>                    <span class="comment">// import paths from TestGoFiles</span>
<span id="Package.TestImportPos"></span>    TestImportPos  map[<a href="/builtin/#string">string</a>][]<a href="/go/token/">token</a>.<a href="/go/token/#Position">Position</a> <span class="comment">// line information for TestImports</span>
<span id="Package.XTestGoFiles"></span>    XTestGoFiles   []<a href="/builtin/#string">string</a>                    <span class="comment">// _test.go files outside package</span>
<span id="Package.XTestImports"></span>    XTestImports   []<a href="/builtin/#string">string</a>                    <span class="comment">// import paths from XTestGoFiles</span>
<span id="Package.XTestImportPos"></span>    XTestImportPos map[<a href="/builtin/#string">string</a>][]<a href="/go/token/">token</a>.<a href="/go/token/#Position">Position</a> <span class="comment">// line information for XTestImports</span>
}</pre>

A Package describes the Go package found in a directory.

<h3 id="Import">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L1125">Import</a>
    <a href="#Import">¶</a></h3>
<pre>func Import(path, srcDir <a href="/builtin/#string">string</a>, mode <a href="#ImportMode">ImportMode</a>) (*<a href="#Package">Package</a>, <a href="/builtin/#error">error</a>)</pre>

Import is shorthand for Default.Import.

<h3 id="ImportDir">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L1130">ImportDir</a>
    <a href="#ImportDir">¶</a></h3>
<pre>func ImportDir(dir <a href="/builtin/#string">string</a>, mode <a href="#ImportMode">ImportMode</a>) (*<a href="#Package">Package</a>, <a href="/builtin/#error">error</a>)</pre>

ImportDir is shorthand for Default.ImportDir.

<h3 id="Package.IsCommand">func (*Package) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/build/build.go#L423">IsCommand</a>
    <a href="#Package.IsCommand">¶</a></h3>
<pre>func (p *<a href="#Package">Package</a>) IsCommand() <a href="/builtin/#bool">bool</a></pre>

IsCommand reports whether the package is considered a command to be installed
(not just a library). Packages named "main" are treated as commands.


