version: 1.10
## package plan9obj

  `import "debug/plan9obj"`

## Overview

Package plan9obj implements access to Plan 9 a.out object files.

## Index

- [Constants](#pkg-constants)
- [type File](#File)
  - [func NewFile(r io.ReaderAt) (*File, error)](#NewFile)
  - [func Open(name string) (*File, error)](#Open)
  - [func (f *File) Close() error](#File.Close)
  - [func (f *File) Section(name string) *Section](#File.Section)
  - [func (f *File) Symbols() ([]Sym, error)](#File.Symbols)
- [type FileHeader](#FileHeader)
- [type Section](#Section)
  - [func (s *Section) Data() ([]byte, error)](#Section.Data)
  - [func (s *Section) Open() io.ReadSeeker](#Section.Open)
- [type SectionHeader](#SectionHeader)
- [type Sym](#Sym)

### Package files
 [file.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go) [plan9obj.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/plan9obj.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="Magic64">Magic64</span> = 0x8000 <span class="comment">// 64-bit expanded header</span>

    <span id="Magic386">Magic386</span>   = (4*11+0)*11 + 7
    <span id="MagicAMD64">MagicAMD64</span> = (4*26+0)*26 + 7 + <a href="#Magic64">Magic64</a>
    <span id="MagicARM">MagicARM</span>   = (4*20+0)*20 + 7
)</pre>


<h2 id="File">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L17">File</a>
    <a href="#File">¶</a></h2>
<pre>type File struct {
    <a href="#FileHeader">FileHeader</a>
<span id="File.Sections"></span>    Sections []*<a href="#Section">Section</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A File represents an open Plan 9 a.out file.

<h3 id="NewFile">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L125">NewFile</a>
    <a href="#NewFile">¶</a></h3>
<pre>func NewFile(r <a href="/io/">io</a>.<a href="/io/#ReaderAt">ReaderAt</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

NewFile creates a new File for accessing a Plan 9 binary in an underlying
reader. The Plan 9 binary is expected to start at position 0 in the ReaderAt.

<h3 id="Open">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L88">Open</a>
    <a href="#Open">¶</a></h3>
<pre>func Open(name <a href="/builtin/#string">string</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

Open opens the named file using os.Open and prepares it for use as a Plan 9
a.out binary.

<h3 id="File.Close">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L105">Close</a>
    <a href="#File.Close">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the File. If the File was created using NewFile directly instead of
Open, Close has no effect.

<h3 id="File.Section">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L311">Section</a>
    <a href="#File.Section">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Section(name <a href="/builtin/#string">string</a>) *<a href="#Section">Section</a></pre>

Section returns a section with the given name, or nil if no such section exists.

<h3 id="File.Symbols">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L295">Symbols</a>
    <a href="#File.Symbols">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Symbols() ([]<a href="#Sym">Sym</a>, <a href="/builtin/#error">error</a>)</pre>

Symbols returns the symbol table for f.

<h2 id="FileHeader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L7">FileHeader</a>
    <a href="#FileHeader">¶</a></h2>
<pre>type FileHeader struct {
<span id="FileHeader.Magic"></span>    Magic       <a href="/builtin/#uint32">uint32</a>
<span id="FileHeader.Bss"></span>    Bss         <a href="/builtin/#uint32">uint32</a>
<span id="FileHeader.Entry"></span>    Entry       <a href="/builtin/#uint64">uint64</a>
<span id="FileHeader.PtrSize"></span>    PtrSize     <a href="/builtin/#int">int</a>
<span id="FileHeader.LoadAddress"></span>    LoadAddress <a href="/builtin/#uint64">uint64</a>
<span id="FileHeader.HdrSize"></span>    HdrSize     <a href="/builtin/#uint64">uint64</a>
}</pre>

A FileHeader represents a Plan 9 a.out file header.

<h2 id="Section">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L33">Section</a>
    <a href="#Section">¶</a></h2>
<pre>type Section struct {
    <a href="#SectionHeader">SectionHeader</a>

    <span class="comment">// Embed ReaderAt for ReadAt method.</span>
    <span class="comment">// Do not embed SectionReader directly</span>
    <span class="comment">// to avoid having Read and Seek.</span>
    <span class="comment">// If a client wants Read and Seek it must use</span>
    <span class="comment">// Open() to avoid fighting over the seek offset</span>
    <span class="comment">// with other clients.</span>
    <a href="/io/">io</a>.<a href="/io/#ReaderAt">ReaderAt</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Section represents a single section in a Plan 9 a.out file.

<h3 id="Section.Data">func (*Section) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L47">Data</a>
    <a href="#Section.Data">¶</a></h3>
<pre>func (s *<a href="#Section">Section</a>) Data() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Data reads and returns the contents of the Plan 9 a.out section.

<h3 id="Section.Open">func (*Section) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L57">Open</a>
    <a href="#Section.Open">¶</a></h3>
<pre>func (s *<a href="#Section">Section</a>) Open() <a href="/io/">io</a>.<a href="/io/#ReadSeeker">ReadSeeker</a></pre>

Open returns a new ReadSeeker reading the Plan 9 a.out section.

<h2 id="SectionHeader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L26">SectionHeader</a>
    <a href="#SectionHeader">¶</a></h2>
<pre>type SectionHeader struct {
<span id="SectionHeader.Name"></span>    Name   <a href="/builtin/#string">string</a>
<span id="SectionHeader.Size"></span>    Size   <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.Offset"></span>    Offset <a href="/builtin/#uint32">uint32</a>
}</pre>

A SectionHeader represents a single Plan 9 a.out section header. This structure
doesn't exist on-disk, but eases navigation through the object file.

<h2 id="Sym">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/plan9obj/file.go#L60">Sym</a>
    <a href="#Sym">¶</a></h2>
<pre>type Sym struct {
<span id="Sym.Value"></span>    Value <a href="/builtin/#uint64">uint64</a>
<span id="Sym.Type"></span>    Type  <a href="/builtin/#rune">rune</a>
<span id="Sym.Name"></span>    Name  <a href="/builtin/#string">string</a>
}</pre>

A Symbol represents an entry in a Plan 9 a.out symbol table section.


