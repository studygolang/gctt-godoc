version: 1.9.2
## package macho

  `import "debug/macho"`

## Overview

Package macho implements access to Mach-O object files.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [type Cpu](#Cpu)
  - [func (i Cpu) GoString() string](#Cpu.GoString)
  - [func (i Cpu) String() string](#Cpu.String)
- [type Dylib](#Dylib)
- [type DylibCmd](#DylibCmd)
- [type Dysymtab](#Dysymtab)
- [type DysymtabCmd](#DysymtabCmd)
- [type FatArch](#FatArch)
- [type FatArchHeader](#FatArchHeader)
- [type FatFile](#FatFile)
  - [func NewFatFile(r io.ReaderAt) (*FatFile, error)](#NewFatFile)
  - [func OpenFat(name string) (*FatFile, error)](#OpenFat)
  - [func (ff *FatFile) Close() error](#FatFile.Close)
- [type File](#File)
  - [func NewFile(r io.ReaderAt) (*File, error)](#NewFile)
  - [func Open(name string) (*File, error)](#Open)
  - [func (f *File) Close() error](#File.Close)
  - [func (f *File) DWARF() (*dwarf.Data, error)](#File.DWARF)
  - [func (f *File) ImportedLibraries() ([]string, error)](#File.ImportedLibraries)
  - [func (f *File) ImportedSymbols() ([]string, error)](#File.ImportedSymbols)
  - [func (f *File) Section(name string) *Section](#File.Section)
  - [func (f *File) Segment(name string) *Segment](#File.Segment)
- [type FileHeader](#FileHeader)
- [type FormatError](#FormatError)
  - [func (e *FormatError) Error() string](#FormatError.Error)
- [type Load](#Load)
- [type LoadBytes](#LoadBytes)
  - [func (b LoadBytes) Raw() []byte](#LoadBytes.Raw)
- [type LoadCmd](#LoadCmd)
  - [func (i LoadCmd) GoString() string](#LoadCmd.GoString)
  - [func (i LoadCmd) String() string](#LoadCmd.String)
- [type Nlist32](#Nlist32)
- [type Nlist64](#Nlist64)
- [type Regs386](#Regs386)
- [type RegsAMD64](#RegsAMD64)
- [type Section](#Section)
  - [func (s *Section) Data() ([]byte, error)](#Section.Data)
  - [func (s *Section) Open() io.ReadSeeker](#Section.Open)
- [type Section32](#Section32)
- [type Section64](#Section64)
- [type SectionHeader](#SectionHeader)
- [type Segment](#Segment)
  - [func (s *Segment) Data() ([]byte, error)](#Segment.Data)
  - [func (s *Segment) Open() io.ReadSeeker](#Segment.Open)
- [type Segment32](#Segment32)
- [type Segment64](#Segment64)
- [type SegmentHeader](#SegmentHeader)
- [type Symbol](#Symbol)
- [type Symtab](#Symtab)
- [type SymtabCmd](#SymtabCmd)
- [type Thread](#Thread)
- [type Type](#Type)

### Package files
 [fat.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/fat.go) [file.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go) [macho.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="Magic32">Magic32</span>  <a href="/builtin/#uint32">uint32</a> = 0xfeedface
    <span id="Magic64">Magic64</span>  <a href="/builtin/#uint32">uint32</a> = 0xfeedfacf
    <span id="MagicFat">MagicFat</span> <a href="/builtin/#uint32">uint32</a> = 0xcafebabe
)</pre>


<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrNotFat">ErrNotFat</span> = &amp;<a href="#FormatError">FormatError</a>{0, &#34;not a fat Mach-O file&#34;, <a href="/builtin/#nil">nil</a>}</pre>

ErrNotFat is returned from NewFatFile or OpenFat when the file is not a
universal binary but may be a thin binary, based on its magic number.

<h2 id="Cpu">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L35">Cpu</a>
    <a href="#Cpu">¶</a></h2>
<pre>type Cpu <a href="/builtin/#uint32">uint32</a></pre>

A Cpu is a Mach-O cpu type.

<pre>const (
    <span id="Cpu386">Cpu386</span>   <a href="#Cpu">Cpu</a> = 7
    <span id="CpuAmd64">CpuAmd64</span> <a href="#Cpu">Cpu</a> = <a href="#Cpu386">Cpu386</a> | cpuArch64
    <span id="CpuArm">CpuArm</span>   <a href="#Cpu">Cpu</a> = 12
    <span id="CpuPpc">CpuPpc</span>   <a href="#Cpu">Cpu</a> = 18
    <span id="CpuPpc64">CpuPpc64</span> <a href="#Cpu">Cpu</a> = <a href="#CpuPpc">CpuPpc</a> | cpuArch64
)</pre>


<h3 id="Cpu.GoString">func (Cpu) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L56">GoString</a>
    <a href="#Cpu.GoString">¶</a></h3>
<pre>func (i <a href="#Cpu">Cpu</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="Cpu.String">func (Cpu) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L55">String</a>
    <a href="#Cpu.String">¶</a></h3>
<pre>func (i <a href="#Cpu">Cpu</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Dylib">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L114">Dylib</a>
    <a href="#Dylib">¶</a></h2>
<pre>type Dylib struct {
    <a href="#LoadBytes">LoadBytes</a>
<span id="Dylib.Name"></span>    Name           <a href="/builtin/#string">string</a>
<span id="Dylib.Time"></span>    Time           <a href="/builtin/#uint32">uint32</a>
<span id="Dylib.CurrentVersion"></span>    CurrentVersion <a href="/builtin/#uint32">uint32</a>
<span id="Dylib.CompatVersion"></span>    CompatVersion  <a href="/builtin/#uint32">uint32</a>
}</pre>

A Dylib represents a Mach-O load dynamic library command.

<h2 id="DylibCmd">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L114">DylibCmd</a>
    <a href="#DylibCmd">¶</a></h2>
<pre>type DylibCmd struct {
<span id="DylibCmd.Cmd"></span>    Cmd            <a href="#LoadCmd">LoadCmd</a>
<span id="DylibCmd.Len"></span>    Len            <a href="/builtin/#uint32">uint32</a>
<span id="DylibCmd.Name"></span>    Name           <a href="/builtin/#uint32">uint32</a>
<span id="DylibCmd.Time"></span>    Time           <a href="/builtin/#uint32">uint32</a>
<span id="DylibCmd.CurrentVersion"></span>    CurrentVersion <a href="/builtin/#uint32">uint32</a>
<span id="DylibCmd.CompatVersion"></span>    CompatVersion  <a href="/builtin/#uint32">uint32</a>
}</pre>

A DylibCmd is a Mach-O load dynamic library command.

<h2 id="Dysymtab">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L130">Dysymtab</a>
    <a href="#Dysymtab">¶</a></h2>
<pre>type Dysymtab struct {
    <a href="#LoadBytes">LoadBytes</a>
    <a href="#DysymtabCmd">DysymtabCmd</a>
<span id="Dysymtab.IndirectSyms"></span>    IndirectSyms []<a href="/builtin/#uint32">uint32</a> <span class="comment">// indices into Symtab.Syms</span>
}</pre>

A Dysymtab represents a Mach-O dynamic symbol table command.

<h2 id="DysymtabCmd">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L165">DysymtabCmd</a>
    <a href="#DysymtabCmd">¶</a></h2>
<pre>type DysymtabCmd struct {
<span id="DysymtabCmd.Cmd"></span>    Cmd            <a href="#LoadCmd">LoadCmd</a>
<span id="DysymtabCmd.Len"></span>    Len            <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Ilocalsym"></span>    Ilocalsym      <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Nlocalsym"></span>    Nlocalsym      <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Iextdefsym"></span>    Iextdefsym     <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Nextdefsym"></span>    Nextdefsym     <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Iundefsym"></span>    Iundefsym      <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Nundefsym"></span>    Nundefsym      <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Tocoffset"></span>    Tocoffset      <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Ntoc"></span>    Ntoc           <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Modtaboff"></span>    Modtaboff      <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Nmodtab"></span>    Nmodtab        <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Extrefsymoff"></span>    Extrefsymoff   <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Nextrefsyms"></span>    Nextrefsyms    <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Indirectsymoff"></span>    Indirectsymoff <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Nindirectsyms"></span>    Nindirectsyms  <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Extreloff"></span>    Extreloff      <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Nextrel"></span>    Nextrel        <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Locreloff"></span>    Locreloff      <a href="/builtin/#uint32">uint32</a>
<span id="DysymtabCmd.Nlocrel"></span>    Nlocrel        <a href="/builtin/#uint32">uint32</a>
}</pre>

A DysymtabCmd is a Mach-O dynamic symbol table command.

<h2 id="FatArch">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/fat.go#L23">FatArch</a>
    <a href="#FatArch">¶</a></h2>
<pre>type FatArch struct {
    <a href="#FatArchHeader">FatArchHeader</a>
    *<a href="#File">File</a>
}</pre>

A FatArch is a Mach-O File inside a FatFile.

<h2 id="FatArchHeader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/fat.go#L12">FatArchHeader</a>
    <a href="#FatArchHeader">¶</a></h2>
<pre>type FatArchHeader struct {
<span id="FatArchHeader.Cpu"></span>    Cpu    <a href="#Cpu">Cpu</a>
<span id="FatArchHeader.SubCpu"></span>    SubCpu <a href="/builtin/#uint32">uint32</a>
<span id="FatArchHeader.Offset"></span>    Offset <a href="/builtin/#uint32">uint32</a>
<span id="FatArchHeader.Size"></span>    Size   <a href="/builtin/#uint32">uint32</a>
<span id="FatArchHeader.Align"></span>    Align  <a href="/builtin/#uint32">uint32</a>
}</pre>

A FatArchHeader represents a fat header for a specific image architecture.

<h2 id="FatFile">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/fat.go#L5">FatFile</a>
    <a href="#FatFile">¶</a></h2>
<pre>type FatFile struct {
<span id="FatFile.Magic"></span>    Magic  <a href="/builtin/#uint32">uint32</a>
<span id="FatFile.Arches"></span>    Arches []<a href="#FatArch">FatArch</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A FatFile is a Mach-O universal binary that contains at least one architecture.

<h3 id="NewFatFile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/fat.go#L35">NewFatFile</a>
    <a href="#NewFatFile">¶</a></h3>
<pre>func NewFatFile(r <a href="/io/">io</a>.<a href="/io/#ReaderAt">ReaderAt</a>) (*<a href="#FatFile">FatFile</a>, <a href="/builtin/#error">error</a>)</pre>

NewFatFile creates a new FatFile for accessing all the Mach-O images in a
universal binary. The Mach-O binary is expected to start at position 0 in the
ReaderAt.

<h3 id="OpenFat">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/fat.go#L115">OpenFat</a>
    <a href="#OpenFat">¶</a></h3>
<pre>func OpenFat(name <a href="/builtin/#string">string</a>) (*<a href="#FatFile">FatFile</a>, <a href="/builtin/#error">error</a>)</pre>

OpenFat opens the named file using os.Open and prepares it for use as a Mach-O
universal binary.

<h3 id="FatFile.Close">func (*FatFile) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/fat.go#L129">Close</a>
    <a href="#FatFile.Close">¶</a></h3>
<pre>func (ff *<a href="#FatFile">FatFile</a>) Close() <a href="/builtin/#error">error</a></pre>


<h2 id="File">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L10">File</a>
    <a href="#File">¶</a></h2>
<pre>type File struct {
    <a href="#FileHeader">FileHeader</a>
<span id="File.ByteOrder"></span>    ByteOrder <a href="/encoding/binary/">binary</a>.<a href="/encoding/binary/#ByteOrder">ByteOrder</a>
<span id="File.Loads"></span>    Loads     []<a href="#Load">Load</a>
<span id="File.Sections"></span>    Sections  []*<a href="#Section">Section</a>

<span id="File.Symtab"></span>    Symtab   *<a href="#Symtab">Symtab</a>
<span id="File.Dysymtab"></span>    Dysymtab *<a href="#Dysymtab">Dysymtab</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A File represents an open Mach-O file.

<h3 id="NewFile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L186">NewFile</a>
    <a href="#NewFile">¶</a></h3>
<pre>func NewFile(r <a href="/io/">io</a>.<a href="/io/#ReaderAt">ReaderAt</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

NewFile creates a new File for accessing a Mach-O binary in an underlying
reader. The Mach-O binary is expected to start at position 0 in the ReaderAt.

<h3 id="Open">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L158">Open</a>
    <a href="#Open">¶</a></h3>
<pre>func Open(name <a href="/builtin/#string">string</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

Open opens the named file using os.Open and prepares it for use as a Mach-O
binary.

<h3 id="File.Close">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L175">Close</a>
    <a href="#File.Close">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the File. If the File was created using NewFile directly instead of
Open, Close has no effect.

<h3 id="File.DWARF">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L463">DWARF</a>
    <a href="#File.DWARF">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) DWARF() (*<a href="/debug/dwarf/">dwarf</a>.<a href="/debug/dwarf/#Data">Data</a>, <a href="/builtin/#error">error</a>)</pre>

DWARF returns the DWARF debug information for the Mach-O file.

<h3 id="File.ImportedLibraries">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L506">ImportedLibraries</a>
    <a href="#File.ImportedLibraries">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) ImportedLibraries() ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ImportedLibraries returns the paths of all libraries referred to by the binary f
that are expected to be linked with the binary at dynamic link time.

<h3 id="File.ImportedSymbols">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L489">ImportedSymbols</a>
    <a href="#File.ImportedSymbols">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) ImportedSymbols() ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ImportedSymbols returns the names of all symbols referred to by the binary f
that are expected to be satisfied by other libraries at dynamic load time.

<h3 id="File.Section">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L453">Section</a>
    <a href="#File.Section">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Section(name <a href="/builtin/#string">string</a>) *<a href="#Section">Section</a></pre>

Section returns the first section with the given name, or nil if no such section
exists.

<h3 id="File.Segment">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L442">Segment</a>
    <a href="#File.Segment">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Segment(name <a href="/builtin/#string">string</a>) *<a href="#Segment">Segment</a></pre>

Segment returns the first Segment with the given name, or nil if no such segment
exists.

<h2 id="FileHeader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L3">FileHeader</a>
    <a href="#FileHeader">¶</a></h2>
<pre>type FileHeader struct {
<span id="FileHeader.Magic"></span>    Magic  <a href="/builtin/#uint32">uint32</a>
<span id="FileHeader.Cpu"></span>    Cpu    <a href="#Cpu">Cpu</a>
<span id="FileHeader.SubCpu"></span>    SubCpu <a href="/builtin/#uint32">uint32</a>
<span id="FileHeader.Type"></span>    Type   <a href="#Type">Type</a>
<span id="FileHeader.Ncmd"></span>    Ncmd   <a href="/builtin/#uint32">uint32</a>
<span id="FileHeader.Cmdsz"></span>    Cmdsz  <a href="/builtin/#uint32">uint32</a>
<span id="FileHeader.Flags"></span>    Flags  <a href="/builtin/#uint32">uint32</a>
}</pre>

A FileHeader represents a Mach-O file header.

<h2 id="FormatError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L142">FormatError</a>
    <a href="#FormatError">¶</a></h2>
<pre>type FormatError struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

FormatError is returned by some operations if the data does not have the correct
format for an object file.

<h3 id="FormatError.Error">func (*FormatError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L148">Error</a>
    <a href="#FormatError.Error">¶</a></h3>
<pre>func (e *<a href="#FormatError">FormatError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Load">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L23">Load</a>
    <a href="#Load">¶</a></h2>
<pre>type Load interface {
    Raw() []<a href="/builtin/#byte">byte</a>
}</pre>

A Load represents any Mach-O load command.

<h2 id="LoadBytes">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L28">LoadBytes</a>
    <a href="#LoadBytes">¶</a></h2>
<pre>type LoadBytes []<a href="/builtin/#byte">byte</a></pre>

A LoadBytes is the uninterpreted bytes of a Mach-O load command.

<h3 id="LoadBytes.Raw">func (LoadBytes) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L30">Raw</a>
    <a href="#LoadBytes.Raw">¶</a></h3>
<pre>func (b <a href="#LoadBytes">LoadBytes</a>) Raw() []<a href="/builtin/#byte">byte</a></pre>


<h2 id="LoadCmd">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L59">LoadCmd</a>
    <a href="#LoadCmd">¶</a></h2>
<pre>type LoadCmd <a href="/builtin/#uint32">uint32</a></pre>

A LoadCmd is a Mach-O load command.

<pre>const (
    <span id="LoadCmdSegment">LoadCmdSegment</span>    <a href="#LoadCmd">LoadCmd</a> = 1
    <span id="LoadCmdSymtab">LoadCmdSymtab</span>     <a href="#LoadCmd">LoadCmd</a> = 2
    <span id="LoadCmdThread">LoadCmdThread</span>     <a href="#LoadCmd">LoadCmd</a> = 4
    <span id="LoadCmdUnixThread">LoadCmdUnixThread</span> <a href="#LoadCmd">LoadCmd</a> = 5 <span class="comment">// thread+stack</span>
    <span id="LoadCmdDysymtab">LoadCmdDysymtab</span>   <a href="#LoadCmd">LoadCmd</a> = 11
    <span id="LoadCmdDylib">LoadCmdDylib</span>      <a href="#LoadCmd">LoadCmd</a> = 12
    <span id="LoadCmdDylinker">LoadCmdDylinker</span>   <a href="#LoadCmd">LoadCmd</a> = 15
    <span id="LoadCmdSegment64">LoadCmdSegment64</span>  <a href="#LoadCmd">LoadCmd</a> = 25
)</pre>


<h3 id="LoadCmd.GoString">func (LoadCmd) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L81">GoString</a>
    <a href="#LoadCmd.GoString">¶</a></h3>
<pre>func (i <a href="#LoadCmd">LoadCmd</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="LoadCmd.String">func (LoadCmd) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L80">String</a>
    <a href="#LoadCmd.String">¶</a></h3>
<pre>func (i <a href="#LoadCmd">LoadCmd</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Nlist32">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L189">Nlist32</a>
    <a href="#Nlist32">¶</a></h2>
<pre>type Nlist32 struct {
<span id="Nlist32.Name"></span>    Name  <a href="/builtin/#uint32">uint32</a>
<span id="Nlist32.Type"></span>    Type  <a href="/builtin/#uint8">uint8</a>
<span id="Nlist32.Sect"></span>    Sect  <a href="/builtin/#uint8">uint8</a>
<span id="Nlist32.Desc"></span>    Desc  <a href="/builtin/#uint16">uint16</a>
<span id="Nlist32.Value"></span>    Value <a href="/builtin/#uint32">uint32</a>
}</pre>

An Nlist32 is a Mach-O 32-bit symbol table entry.

<h2 id="Nlist64">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L198">Nlist64</a>
    <a href="#Nlist64">¶</a></h2>
<pre>type Nlist64 struct {
<span id="Nlist64.Name"></span>    Name  <a href="/builtin/#uint32">uint32</a>
<span id="Nlist64.Type"></span>    Type  <a href="/builtin/#uint8">uint8</a>
<span id="Nlist64.Sect"></span>    Sect  <a href="/builtin/#uint8">uint8</a>
<span id="Nlist64.Desc"></span>    Desc  <a href="/builtin/#uint16">uint16</a>
<span id="Nlist64.Value"></span>    Value <a href="/builtin/#uint64">uint64</a>
}</pre>

An Nlist64 is a Mach-O 64-bit symbol table entry.

<h2 id="Regs386">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L224">Regs386</a>
    <a href="#Regs386">¶</a></h2>
<pre>type Regs386 struct {
<span id="Regs386.AX"></span>    AX    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.BX"></span>    BX    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.CX"></span>    CX    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.DX"></span>    DX    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.DI"></span>    DI    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.SI"></span>    SI    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.BP"></span>    BP    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.SP"></span>    SP    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.SS"></span>    SS    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.FLAGS"></span>    FLAGS <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.IP"></span>    IP    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.CS"></span>    CS    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.DS"></span>    DS    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.ES"></span>    ES    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.FS"></span>    FS    <a href="/builtin/#uint32">uint32</a>
<span id="Regs386.GS"></span>    GS    <a href="/builtin/#uint32">uint32</a>
}</pre>

Regs386 is the Mach-O 386 register structure.

<h2 id="RegsAMD64">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L244">RegsAMD64</a>
    <a href="#RegsAMD64">¶</a></h2>
<pre>type RegsAMD64 struct {
<span id="RegsAMD64.AX"></span>    AX    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.BX"></span>    BX    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.CX"></span>    CX    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.DX"></span>    DX    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.DI"></span>    DI    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.SI"></span>    SI    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.BP"></span>    BP    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.SP"></span>    SP    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.R8"></span>    R8    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.R9"></span>    R9    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.R10"></span>    R10   <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.R11"></span>    R11   <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.R12"></span>    R12   <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.R13"></span>    R13   <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.R14"></span>    R14   <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.R15"></span>    R15   <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.IP"></span>    IP    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.FLAGS"></span>    FLAGS <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.CS"></span>    CS    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.FS"></span>    FS    <a href="/builtin/#uint64">uint64</a>
<span id="RegsAMD64.GS"></span>    GS    <a href="/builtin/#uint64">uint64</a>
}</pre>

RegsAMD64 is the Mach-O AMD64 register structure.

<h2 id="Section">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L87">Section</a>
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


<h3 id="Section.Data">func (*Section) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L101">Data</a>
    <a href="#Section.Data">¶</a></h3>
<pre>func (s *<a href="#Section">Section</a>) Data() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Data reads and returns the contents of the Mach-O section.

<h3 id="Section.Open">func (*Section) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L111">Open</a>
    <a href="#Section.Open">¶</a></h3>
<pre>func (s *<a href="#Section">Section</a>) Open() <a href="/io/">io</a>.<a href="/io/#ReadSeeker">ReadSeeker</a></pre>

Open returns a new ReadSeeker reading the Mach-O section.

<h2 id="Section32">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L124">Section32</a>
    <a href="#Section32">¶</a></h2>
<pre>type Section32 struct {
<span id="Section32.Name"></span>    Name     [16]<a href="/builtin/#byte">byte</a>
<span id="Section32.Seg"></span>    Seg      [16]<a href="/builtin/#byte">byte</a>
<span id="Section32.Addr"></span>    Addr     <a href="/builtin/#uint32">uint32</a>
<span id="Section32.Size"></span>    Size     <a href="/builtin/#uint32">uint32</a>
<span id="Section32.Offset"></span>    Offset   <a href="/builtin/#uint32">uint32</a>
<span id="Section32.Align"></span>    Align    <a href="/builtin/#uint32">uint32</a>
<span id="Section32.Reloff"></span>    Reloff   <a href="/builtin/#uint32">uint32</a>
<span id="Section32.Nreloc"></span>    Nreloc   <a href="/builtin/#uint32">uint32</a>
<span id="Section32.Flags"></span>    Flags    <a href="/builtin/#uint32">uint32</a>
<span id="Section32.Reserve1"></span>    Reserve1 <a href="/builtin/#uint32">uint32</a>
<span id="Section32.Reserve2"></span>    Reserve2 <a href="/builtin/#uint32">uint32</a>
}</pre>

A Section32 is a 32-bit Mach-O section header.

<h2 id="Section64">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L139">Section64</a>
    <a href="#Section64">¶</a></h2>
<pre>type Section64 struct {
<span id="Section64.Name"></span>    Name     [16]<a href="/builtin/#byte">byte</a>
<span id="Section64.Seg"></span>    Seg      [16]<a href="/builtin/#byte">byte</a>
<span id="Section64.Addr"></span>    Addr     <a href="/builtin/#uint64">uint64</a>
<span id="Section64.Size"></span>    Size     <a href="/builtin/#uint64">uint64</a>
<span id="Section64.Offset"></span>    Offset   <a href="/builtin/#uint32">uint32</a>
<span id="Section64.Align"></span>    Align    <a href="/builtin/#uint32">uint32</a>
<span id="Section64.Reloff"></span>    Reloff   <a href="/builtin/#uint32">uint32</a>
<span id="Section64.Nreloc"></span>    Nreloc   <a href="/builtin/#uint32">uint32</a>
<span id="Section64.Flags"></span>    Flags    <a href="/builtin/#uint32">uint32</a>
<span id="Section64.Reserve1"></span>    Reserve1 <a href="/builtin/#uint32">uint32</a>
<span id="Section64.Reserve2"></span>    Reserve2 <a href="/builtin/#uint32">uint32</a>
<span id="Section64.Reserve3"></span>    Reserve3 <a href="/builtin/#uint32">uint32</a>
}</pre>

A Section64 is a 64-bit Mach-O section header.

<h2 id="SectionHeader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L75">SectionHeader</a>
    <a href="#SectionHeader">¶</a></h2>
<pre>type SectionHeader struct {
<span id="SectionHeader.Name"></span>    Name   <a href="/builtin/#string">string</a>
<span id="SectionHeader.Seg"></span>    Seg    <a href="/builtin/#string">string</a>
<span id="SectionHeader.Addr"></span>    Addr   <a href="/builtin/#uint64">uint64</a>
<span id="SectionHeader.Size"></span>    Size   <a href="/builtin/#uint64">uint64</a>
<span id="SectionHeader.Offset"></span>    Offset <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.Align"></span>    Align  <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.Reloff"></span>    Reloff <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.Nreloc"></span>    Nreloc <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.Flags"></span>    Flags  <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="Segment">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L48">Segment</a>
    <a href="#Segment">¶</a></h2>
<pre>type Segment struct {
    <a href="#LoadBytes">LoadBytes</a>
    <a href="#SegmentHeader">SegmentHeader</a>

    <span class="comment">// Embed ReaderAt for ReadAt method.</span>
    <span class="comment">// Do not embed SectionReader directly</span>
    <span class="comment">// to avoid having Read and Seek.</span>
    <span class="comment">// If a client wants Read and Seek it must use</span>
    <span class="comment">// Open() to avoid fighting over the seek offset</span>
    <span class="comment">// with other clients.</span>
    <a href="/io/">io</a>.<a href="/io/#ReaderAt">ReaderAt</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Segment represents a Mach-O 32-bit or 64-bit load segment command.

<h3 id="Segment.Data">func (*Segment) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L63">Data</a>
    <a href="#Segment.Data">¶</a></h3>
<pre>func (s *<a href="#Segment">Segment</a>) Data() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Data reads and returns the contents of the segment.

<h3 id="Segment.Open">func (*Segment) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L73">Open</a>
    <a href="#Segment.Open">¶</a></h3>
<pre>func (s *<a href="#Segment">Segment</a>) Open() <a href="/io/">io</a>.<a href="/io/#ReadSeeker">ReadSeeker</a></pre>

Open returns a new ReadSeeker reading the segment.

<h2 id="Segment32">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L99">Segment32</a>
    <a href="#Segment32">¶</a></h2>
<pre>type Segment32 struct {
<span id="Segment32.Cmd"></span>    Cmd     <a href="#LoadCmd">LoadCmd</a>
<span id="Segment32.Len"></span>    Len     <a href="/builtin/#uint32">uint32</a>
<span id="Segment32.Name"></span>    Name    [16]<a href="/builtin/#byte">byte</a>
<span id="Segment32.Addr"></span>    Addr    <a href="/builtin/#uint32">uint32</a>
<span id="Segment32.Memsz"></span>    Memsz   <a href="/builtin/#uint32">uint32</a>
<span id="Segment32.Offset"></span>    Offset  <a href="/builtin/#uint32">uint32</a>
<span id="Segment32.Filesz"></span>    Filesz  <a href="/builtin/#uint32">uint32</a>
<span id="Segment32.Maxprot"></span>    Maxprot <a href="/builtin/#uint32">uint32</a>
<span id="Segment32.Prot"></span>    Prot    <a href="/builtin/#uint32">uint32</a>
<span id="Segment32.Nsect"></span>    Nsect   <a href="/builtin/#uint32">uint32</a>
<span id="Segment32.Flag"></span>    Flag    <a href="/builtin/#uint32">uint32</a>
}</pre>

A Segment32 is a 32-bit Mach-O segment load command.

<h2 id="Segment64">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L84">Segment64</a>
    <a href="#Segment64">¶</a></h2>
<pre>type Segment64 struct {
<span id="Segment64.Cmd"></span>    Cmd     <a href="#LoadCmd">LoadCmd</a>
<span id="Segment64.Len"></span>    Len     <a href="/builtin/#uint32">uint32</a>
<span id="Segment64.Name"></span>    Name    [16]<a href="/builtin/#byte">byte</a>
<span id="Segment64.Addr"></span>    Addr    <a href="/builtin/#uint64">uint64</a>
<span id="Segment64.Memsz"></span>    Memsz   <a href="/builtin/#uint64">uint64</a>
<span id="Segment64.Offset"></span>    Offset  <a href="/builtin/#uint64">uint64</a>
<span id="Segment64.Filesz"></span>    Filesz  <a href="/builtin/#uint64">uint64</a>
<span id="Segment64.Maxprot"></span>    Maxprot <a href="/builtin/#uint32">uint32</a>
<span id="Segment64.Prot"></span>    Prot    <a href="/builtin/#uint32">uint32</a>
<span id="Segment64.Nsect"></span>    Nsect   <a href="/builtin/#uint32">uint32</a>
<span id="Segment64.Flag"></span>    Flag    <a href="/builtin/#uint32">uint32</a>
}</pre>

A Segment64 is a 64-bit Mach-O segment load command.

<h2 id="SegmentHeader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L33">SegmentHeader</a>
    <a href="#SegmentHeader">¶</a></h2>
<pre>type SegmentHeader struct {
<span id="SegmentHeader.Cmd"></span>    Cmd     <a href="#LoadCmd">LoadCmd</a>
<span id="SegmentHeader.Len"></span>    Len     <a href="/builtin/#uint32">uint32</a>
<span id="SegmentHeader.Name"></span>    Name    <a href="/builtin/#string">string</a>
<span id="SegmentHeader.Addr"></span>    Addr    <a href="/builtin/#uint64">uint64</a>
<span id="SegmentHeader.Memsz"></span>    Memsz   <a href="/builtin/#uint64">uint64</a>
<span id="SegmentHeader.Offset"></span>    Offset  <a href="/builtin/#uint64">uint64</a>
<span id="SegmentHeader.Filesz"></span>    Filesz  <a href="/builtin/#uint64">uint64</a>
<span id="SegmentHeader.Maxprot"></span>    Maxprot <a href="/builtin/#uint32">uint32</a>
<span id="SegmentHeader.Prot"></span>    Prot    <a href="/builtin/#uint32">uint32</a>
<span id="SegmentHeader.Nsect"></span>    Nsect   <a href="/builtin/#uint32">uint32</a>
<span id="SegmentHeader.Flag"></span>    Flag    <a href="/builtin/#uint32">uint32</a>
}</pre>

A SegmentHeader is the header for a Mach-O 32-bit or 64-bit load segment
command.

<h2 id="Symbol">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L207">Symbol</a>
    <a href="#Symbol">¶</a></h2>
<pre>type Symbol struct {
<span id="Symbol.Name"></span>    Name  <a href="/builtin/#string">string</a>
<span id="Symbol.Type"></span>    Type  <a href="/builtin/#uint8">uint8</a>
<span id="Symbol.Sect"></span>    Sect  <a href="/builtin/#uint8">uint8</a>
<span id="Symbol.Desc"></span>    Desc  <a href="/builtin/#uint16">uint16</a>
<span id="Symbol.Value"></span>    Value <a href="/builtin/#uint64">uint64</a>
}</pre>

A Symbol is a Mach-O 32-bit or 64-bit symbol table entry.

<h2 id="Symtab">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/file.go#L123">Symtab</a>
    <a href="#Symtab">¶</a></h2>
<pre>type Symtab struct {
    <a href="#LoadBytes">LoadBytes</a>
    <a href="#SymtabCmd">SymtabCmd</a>
<span id="Symtab.Syms"></span>    Syms []<a href="#Symbol">Symbol</a>
}</pre>

A Symtab represents a Mach-O symbol table command.

<h2 id="SymtabCmd">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L155">SymtabCmd</a>
    <a href="#SymtabCmd">¶</a></h2>
<pre>type SymtabCmd struct {
<span id="SymtabCmd.Cmd"></span>    Cmd     <a href="#LoadCmd">LoadCmd</a>
<span id="SymtabCmd.Len"></span>    Len     <a href="/builtin/#uint32">uint32</a>
<span id="SymtabCmd.Symoff"></span>    Symoff  <a href="/builtin/#uint32">uint32</a>
<span id="SymtabCmd.Nsyms"></span>    Nsyms   <a href="/builtin/#uint32">uint32</a>
<span id="SymtabCmd.Stroff"></span>    Stroff  <a href="/builtin/#uint32">uint32</a>
<span id="SymtabCmd.Strsize"></span>    Strsize <a href="/builtin/#uint32">uint32</a>
}</pre>

A SymtabCmd is a Mach-O symbol table command.

<h2 id="Thread">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L216">Thread</a>
    <a href="#Thread">¶</a></h2>
<pre>type Thread struct {
<span id="Thread.Cmd"></span>    Cmd  <a href="#LoadCmd">LoadCmd</a>
<span id="Thread.Len"></span>    Len  <a href="/builtin/#uint32">uint32</a>
<span id="Thread.Type"></span>    Type <a href="/builtin/#uint32">uint32</a>
<span id="Thread.Data"></span>    Data []<a href="/builtin/#uint32">uint32</a>
}</pre>

A Thread is a Mach-O thread state command.

<h2 id="Type">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/macho/macho.go#L25">Type</a>
    <a href="#Type">¶</a></h2>
<pre>type Type <a href="/builtin/#uint32">uint32</a></pre>

A Type is the Mach-O file type, e.g. an object file, executable, or dynamic
library.

<pre>const (
    <span id="TypeObj">TypeObj</span>    <a href="#Type">Type</a> = 1
    <span id="TypeExec">TypeExec</span>   <a href="#Type">Type</a> = 2
    <span id="TypeDylib">TypeDylib</span>  <a href="#Type">Type</a> = 6
    <span id="TypeBundle">TypeBundle</span> <a href="#Type">Type</a> = 8
)</pre>



