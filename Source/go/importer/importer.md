version: 1.10
## package importer

  `import "go/importer"`

## Overview

Package importer provides access to export data importers.

## Index

- [func Default() types.Importer](#Default)
- [func For(compiler string, lookup Lookup) types.Importer](#For)
- [type Lookup](#Lookup)

### Package files
 [importer.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/importer/importer.go)

<h2 id="Default">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/importer/importer.go#L65">Default</a>
    <a href="#Default">¶</a></h2>
<pre>func Default() <a href="/go/types/">types</a>.<a href="/go/types/#Importer">Importer</a></pre>

Default returns an Importer for the compiler that built the running binary. If
available, the result implements types.ImporterFrom.

<h2 id="For">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/importer/importer.go#L32">For</a>
    <a href="#For">¶</a></h2>
<pre>func For(compiler <a href="/builtin/#string">string</a>, lookup <a href="#Lookup">Lookup</a>) <a href="/go/types/">types</a>.<a href="/go/types/#Importer">Importer</a></pre>

For returns an Importer for importing from installed packages for the compilers
"gc" and "gccgo", or for importing directly from the source if the compiler
argument is "source". In this latter case, importing may fail under
circumstances where the exported API is not entirely defined in pure Go source
code (if the package API depends on cgo-defined entities, the type checker won't
have access to those).

If lookup is nil, the default package lookup mechanism for the given compiler is
used, and the resulting importer attempts to resolve relative and absolute
import paths to canonical import path IDs before finding the imported file.

If lookup is non-nil, then the returned importer calls lookup each time it needs
to resolve an import path. In this mode the importer can only be invoked with
canonical import paths (not relative or absolute ones); it is assumed that the
translation to canonical import paths is being done by the client of the
importer.

<h2 id="Lookup">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/importer/importer.go#L11">Lookup</a>
    <a href="#Lookup">¶</a></h2>
<pre>type Lookup func(path <a href="/builtin/#string">string</a>) (<a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>, <a href="/builtin/#error">error</a>)</pre>

A Lookup function returns a reader to access package data for a given import
path, or an error if no matching package is found.


