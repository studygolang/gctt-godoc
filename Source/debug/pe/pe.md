version: 1.10
## package pe

  `import "debug/pe"`

## Overview

Package pe implements access to PE (Microsoft Windows Portable Executable)
files.

## Index

- [Constants](#pkg-constants)
- [type COFFSymbol](#COFFSymbol)
  - [func (sym *COFFSymbol) FullName(st StringTable) (string, error)](#COFFSymbol.FullName)
- [type DataDirectory](#DataDirectory)
- [type File](#File)
  - [func NewFile(r io.ReaderAt) (*File, error)](#NewFile)
  - [func Open(name string) (*File, error)](#Open)
  - [func (f *File) Close() error](#File.Close)
  - [func (f *File) DWARF() (*dwarf.Data, error)](#File.DWARF)
  - [func (f *File) ImportedLibraries() ([]string, error)](#File.ImportedLibraries)
  - [func (f *File) ImportedSymbols() ([]string, error)](#File.ImportedSymbols)
  - [func (f *File) Section(name string) *Section](#File.Section)
- [type FileHeader](#FileHeader)
- [type FormatError](#FormatError)
  - [func (e *FormatError) Error() string](#FormatError.Error)
- [type ImportDirectory](#ImportDirectory)
- [type OptionalHeader32](#OptionalHeader32)
- [type OptionalHeader64](#OptionalHeader64)
- [type Reloc](#Reloc)
- [type Section](#Section)
  - [func (s *Section) Data() ([]byte, error)](#Section.Data)
  - [func (s *Section) Open() io.ReadSeeker](#Section.Open)
- [type SectionHeader](#SectionHeader)
- [type SectionHeader32](#SectionHeader32)
- [type StringTable](#StringTable)
  - [func (st StringTable) String(start uint32) (string, error)](#StringTable.String)
- [type Symbol](#Symbol)

### Package files
 [file.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go) [pe.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/pe.go) [section.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/section.go) [string.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/string.go) [symbol.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/symbol.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="IMAGE_FILE_MACHINE_UNKNOWN">IMAGE_FILE_MACHINE_UNKNOWN</span>   = 0x0
    <span id="IMAGE_FILE_MACHINE_AM33">IMAGE_FILE_MACHINE_AM33</span>      = 0x1d3
    <span id="IMAGE_FILE_MACHINE_AMD64">IMAGE_FILE_MACHINE_AMD64</span>     = 0x8664
    <span id="IMAGE_FILE_MACHINE_ARM">IMAGE_FILE_MACHINE_ARM</span>       = 0x1c0
    <span id="IMAGE_FILE_MACHINE_EBC">IMAGE_FILE_MACHINE_EBC</span>       = 0xebc
    <span id="IMAGE_FILE_MACHINE_I386">IMAGE_FILE_MACHINE_I386</span>      = 0x14c
    <span id="IMAGE_FILE_MACHINE_IA64">IMAGE_FILE_MACHINE_IA64</span>      = 0x200
    <span id="IMAGE_FILE_MACHINE_M32R">IMAGE_FILE_MACHINE_M32R</span>      = 0x9041
    <span id="IMAGE_FILE_MACHINE_MIPS16">IMAGE_FILE_MACHINE_MIPS16</span>    = 0x266
    <span id="IMAGE_FILE_MACHINE_MIPSFPU">IMAGE_FILE_MACHINE_MIPSFPU</span>   = 0x366
    <span id="IMAGE_FILE_MACHINE_MIPSFPU16">IMAGE_FILE_MACHINE_MIPSFPU16</span> = 0x466
    <span id="IMAGE_FILE_MACHINE_POWERPC">IMAGE_FILE_MACHINE_POWERPC</span>   = 0x1f0
    <span id="IMAGE_FILE_MACHINE_POWERPCFP">IMAGE_FILE_MACHINE_POWERPCFP</span> = 0x1f1
    <span id="IMAGE_FILE_MACHINE_R4000">IMAGE_FILE_MACHINE_R4000</span>     = 0x166
    <span id="IMAGE_FILE_MACHINE_SH3">IMAGE_FILE_MACHINE_SH3</span>       = 0x1a2
    <span id="IMAGE_FILE_MACHINE_SH3DSP">IMAGE_FILE_MACHINE_SH3DSP</span>    = 0x1a3
    <span id="IMAGE_FILE_MACHINE_SH4">IMAGE_FILE_MACHINE_SH4</span>       = 0x1a6
    <span id="IMAGE_FILE_MACHINE_SH5">IMAGE_FILE_MACHINE_SH5</span>       = 0x1a8
    <span id="IMAGE_FILE_MACHINE_THUMB">IMAGE_FILE_MACHINE_THUMB</span>     = 0x1c2
    <span id="IMAGE_FILE_MACHINE_WCEMIPSV2">IMAGE_FILE_MACHINE_WCEMIPSV2</span> = 0x169
)</pre>


<pre>const <span id="COFFSymbolSize">COFFSymbolSize</span> = 18</pre>


<h2 id="COFFSymbol">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/symbol.go#L6">COFFSymbol</a>
    <a href="#COFFSymbol">¶</a></h2>
<pre>type COFFSymbol struct {
<span id="COFFSymbol.Name"></span>    Name               [8]<a href="/builtin/#uint8">uint8</a>
<span id="COFFSymbol.Value"></span>    Value              <a href="/builtin/#uint32">uint32</a>
<span id="COFFSymbol.SectionNumber"></span>    SectionNumber      <a href="/builtin/#int16">int16</a>
<span id="COFFSymbol.Type"></span>    Type               <a href="/builtin/#uint16">uint16</a>
<span id="COFFSymbol.StorageClass"></span>    StorageClass       <a href="/builtin/#uint8">uint8</a>
<span id="COFFSymbol.NumberOfAuxSymbols"></span>    NumberOfAuxSymbols <a href="/builtin/#uint8">uint8</a>
}</pre>

COFFSymbol represents single COFF symbol table record.

<h3 id="COFFSymbol.FullName">func (*COFFSymbol) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/symbol.go#L45">FullName</a>
    <a href="#COFFSymbol.FullName">¶</a></h3>
<pre>func (sym *<a href="#COFFSymbol">COFFSymbol</a>) FullName(st <a href="#StringTable">StringTable</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

FullName finds real name of symbol sym. Normally name is stored in sym.Name, but
if it is longer then 8 characters, it is stored in COFF string table st instead.

<h2 id="DataDirectory">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/pe.go#L7">DataDirectory</a>
    <a href="#DataDirectory">¶</a></h2>
<pre>type DataDirectory struct {
<span id="DataDirectory.VirtualAddress"></span>    VirtualAddress <a href="/builtin/#uint32">uint32</a>
<span id="DataDirectory.Size"></span>    Size           <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="File">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L10">File</a>
    <a href="#File">¶</a></h2>
<pre>type File struct {
    <a href="#FileHeader">FileHeader</a>
<span id="File.OptionalHeader"></span>    OptionalHeader interface{} <span class="comment">// of type *OptionalHeader32 or *OptionalHeader64</span>
<span id="File.Sections"></span>    Sections       []*<a href="#Section">Section</a>
<span id="File.Symbols"></span>    Symbols        []*<a href="#Symbol">Symbol</a>    <span class="comment">// COFF symbols with auxiliary symbol records removed</span>
<span id="File.COFFSymbols"></span>    COFFSymbols    []<a href="#COFFSymbol">COFFSymbol</a> <span class="comment">// all COFF symbols (including auxiliary symbol records)</span>
<span id="File.StringTable"></span>    StringTable    <a href="#StringTable">StringTable</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A File represents an open PE file.

<h3 id="NewFile">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L56">NewFile</a>
    <a href="#NewFile">¶</a></h3>
<pre>func NewFile(r <a href="/io/">io</a>.<a href="/io/#ReaderAt">ReaderAt</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

NewFile creates a new File for accessing a PE binary in an underlying reader.

<h3 id="Open">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L22">Open</a>
    <a href="#Open">¶</a></h3>
<pre>func Open(name <a href="/builtin/#string">string</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

Open opens the named file using os.Open and prepares it for use as a PE binary.

<h3 id="File.Close">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L39">Close</a>
    <a href="#File.Close">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the File. If the File was created using NewFile directly instead of
Open, Close has no effect.

<h3 id="File.DWARF">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L209">DWARF</a>
    <a href="#File.DWARF">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) DWARF() (*<a href="/debug/dwarf/">dwarf</a>.<a href="/debug/dwarf/#Data">Data</a>, <a href="/builtin/#error">error</a>)</pre>


<h3 id="File.ImportedLibraries">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L323">ImportedLibraries</a>
    <a href="#File.ImportedLibraries">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) ImportedLibraries() ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ImportedLibraries returns the names of all libraries referred to by the binary f
that are expected to be linked with the binary at dynamic link time.

<h3 id="File.ImportedSymbols">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L251">ImportedSymbols</a>
    <a href="#File.ImportedSymbols">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) ImportedSymbols() ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ImportedSymbols returns the names of all symbols referred to by the binary f
that are expected to be satisfied by other libraries at dynamic load time. It
does not return weak symbols.

<h3 id="File.Section">func (*File) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L200">Section</a>
    <a href="#File.Section">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Section(name <a href="/builtin/#string">string</a>) *<a href="#Section">Section</a></pre>

Section returns the first section with the given name, or nil if no such section
exists.

<h2 id="FileHeader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/pe.go#L1">FileHeader</a>
    <a href="#FileHeader">¶</a></h2>
<pre>type FileHeader struct {
<span id="FileHeader.Machine"></span>    Machine              <a href="/builtin/#uint16">uint16</a>
<span id="FileHeader.NumberOfSections"></span>    NumberOfSections     <a href="/builtin/#uint16">uint16</a>
<span id="FileHeader.TimeDateStamp"></span>    TimeDateStamp        <a href="/builtin/#uint32">uint32</a>
<span id="FileHeader.PointerToSymbolTable"></span>    PointerToSymbolTable <a href="/builtin/#uint32">uint32</a>
<span id="FileHeader.NumberOfSymbols"></span>    NumberOfSymbols      <a href="/builtin/#uint32">uint32</a>
<span id="FileHeader.SizeOfOptionalHeader"></span>    SizeOfOptionalHeader <a href="/builtin/#uint16">uint16</a>
<span id="FileHeader.Characteristics"></span>    Characteristics      <a href="/builtin/#uint16">uint16</a>
}</pre>


<h2 id="FormatError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L331">FormatError</a>
    <a href="#FormatError">¶</a></h2>
<pre>type FormatError struct {
}</pre>

FormatError is unused. The type is retained for compatibility.

<h3 id="FormatError.Error">func (*FormatError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L334">Error</a>
    <a href="#FormatError.Error">¶</a></h3>
<pre>func (e *<a href="#FormatError">FormatError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="ImportDirectory">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/file.go#L237">ImportDirectory</a>
    <a href="#ImportDirectory">¶</a></h2>
<pre>type ImportDirectory struct {
<span id="ImportDirectory.OriginalFirstThunk"></span>    OriginalFirstThunk <a href="/builtin/#uint32">uint32</a>
<span id="ImportDirectory.TimeDateStamp"></span>    TimeDateStamp      <a href="/builtin/#uint32">uint32</a>
<span id="ImportDirectory.ForwarderChain"></span>    ForwarderChain     <a href="/builtin/#uint32">uint32</a>
<span id="ImportDirectory.Name"></span>    Name               <a href="/builtin/#uint32">uint32</a>
<span id="ImportDirectory.FirstThunk"></span>    FirstThunk         <a href="/builtin/#uint32">uint32</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>


<h2 id="OptionalHeader32">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/pe.go#L12">OptionalHeader32</a>
    <a href="#OptionalHeader32">¶</a></h2>
<pre>type OptionalHeader32 struct {
<span id="OptionalHeader32.Magic"></span>    Magic                       <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader32.MajorLinkerVersion"></span>    MajorLinkerVersion          <a href="/builtin/#uint8">uint8</a>
<span id="OptionalHeader32.MinorLinkerVersion"></span>    MinorLinkerVersion          <a href="/builtin/#uint8">uint8</a>
<span id="OptionalHeader32.SizeOfCode"></span>    SizeOfCode                  <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.SizeOfInitializedData"></span>    SizeOfInitializedData       <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.SizeOfUninitializedData"></span>    SizeOfUninitializedData     <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.AddressOfEntryPoint"></span>    AddressOfEntryPoint         <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.BaseOfCode"></span>    BaseOfCode                  <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.BaseOfData"></span>    BaseOfData                  <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.ImageBase"></span>    ImageBase                   <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.SectionAlignment"></span>    SectionAlignment            <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.FileAlignment"></span>    FileAlignment               <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.MajorOperatingSystemVersion"></span>    MajorOperatingSystemVersion <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader32.MinorOperatingSystemVersion"></span>    MinorOperatingSystemVersion <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader32.MajorImageVersion"></span>    MajorImageVersion           <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader32.MinorImageVersion"></span>    MinorImageVersion           <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader32.MajorSubsystemVersion"></span>    MajorSubsystemVersion       <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader32.MinorSubsystemVersion"></span>    MinorSubsystemVersion       <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader32.Win32VersionValue"></span>    Win32VersionValue           <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.SizeOfImage"></span>    SizeOfImage                 <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.SizeOfHeaders"></span>    SizeOfHeaders               <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.CheckSum"></span>    CheckSum                    <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.Subsystem"></span>    Subsystem                   <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader32.DllCharacteristics"></span>    DllCharacteristics          <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader32.SizeOfStackReserve"></span>    SizeOfStackReserve          <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.SizeOfStackCommit"></span>    SizeOfStackCommit           <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.SizeOfHeapReserve"></span>    SizeOfHeapReserve           <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.SizeOfHeapCommit"></span>    SizeOfHeapCommit            <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.LoaderFlags"></span>    LoaderFlags                 <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.NumberOfRvaAndSizes"></span>    NumberOfRvaAndSizes         <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader32.DataDirectory"></span>    DataDirectory               [16]<a href="#DataDirectory">DataDirectory</a>
}</pre>


<h2 id="OptionalHeader64">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/pe.go#L46">OptionalHeader64</a>
    <a href="#OptionalHeader64">¶</a></h2>
<pre>type OptionalHeader64 struct {
<span id="OptionalHeader64.Magic"></span>    Magic                       <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader64.MajorLinkerVersion"></span>    MajorLinkerVersion          <a href="/builtin/#uint8">uint8</a>
<span id="OptionalHeader64.MinorLinkerVersion"></span>    MinorLinkerVersion          <a href="/builtin/#uint8">uint8</a>
<span id="OptionalHeader64.SizeOfCode"></span>    SizeOfCode                  <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.SizeOfInitializedData"></span>    SizeOfInitializedData       <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.SizeOfUninitializedData"></span>    SizeOfUninitializedData     <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.AddressOfEntryPoint"></span>    AddressOfEntryPoint         <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.BaseOfCode"></span>    BaseOfCode                  <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.ImageBase"></span>    ImageBase                   <a href="/builtin/#uint64">uint64</a>
<span id="OptionalHeader64.SectionAlignment"></span>    SectionAlignment            <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.FileAlignment"></span>    FileAlignment               <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.MajorOperatingSystemVersion"></span>    MajorOperatingSystemVersion <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader64.MinorOperatingSystemVersion"></span>    MinorOperatingSystemVersion <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader64.MajorImageVersion"></span>    MajorImageVersion           <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader64.MinorImageVersion"></span>    MinorImageVersion           <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader64.MajorSubsystemVersion"></span>    MajorSubsystemVersion       <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader64.MinorSubsystemVersion"></span>    MinorSubsystemVersion       <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader64.Win32VersionValue"></span>    Win32VersionValue           <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.SizeOfImage"></span>    SizeOfImage                 <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.SizeOfHeaders"></span>    SizeOfHeaders               <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.CheckSum"></span>    CheckSum                    <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.Subsystem"></span>    Subsystem                   <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader64.DllCharacteristics"></span>    DllCharacteristics          <a href="/builtin/#uint16">uint16</a>
<span id="OptionalHeader64.SizeOfStackReserve"></span>    SizeOfStackReserve          <a href="/builtin/#uint64">uint64</a>
<span id="OptionalHeader64.SizeOfStackCommit"></span>    SizeOfStackCommit           <a href="/builtin/#uint64">uint64</a>
<span id="OptionalHeader64.SizeOfHeapReserve"></span>    SizeOfHeapReserve           <a href="/builtin/#uint64">uint64</a>
<span id="OptionalHeader64.SizeOfHeapCommit"></span>    SizeOfHeapCommit            <a href="/builtin/#uint64">uint64</a>
<span id="OptionalHeader64.LoaderFlags"></span>    LoaderFlags                 <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.NumberOfRvaAndSizes"></span>    NumberOfRvaAndSizes         <a href="/builtin/#uint32">uint32</a>
<span id="OptionalHeader64.DataDirectory"></span>    DataDirectory               [16]<a href="#DataDirectory">DataDirectory</a>
}</pre>


<h2 id="Reloc">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/section.go#L36">Reloc</a>
    <a href="#Reloc">¶</a></h2>
<pre>type Reloc struct {
<span id="Reloc.VirtualAddress"></span>    VirtualAddress   <a href="/builtin/#uint32">uint32</a>
<span id="Reloc.SymbolTableIndex"></span>    SymbolTableIndex <a href="/builtin/#uint32">uint32</a>
<span id="Reloc.Type"></span>    Type             <a href="/builtin/#uint16">uint16</a>
}</pre>

Reloc represents a PE COFF relocation. Each section contains its own relocation
list.

<h2 id="Section">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/section.go#L74">Section</a>
    <a href="#Section">¶</a></h2>
<pre>type Section struct {
    <a href="#SectionHeader">SectionHeader</a>
<span id="Section.Relocs"></span>    Relocs []<a href="#Reloc">Reloc</a>

    <span class="comment">// Embed ReaderAt for ReadAt method.</span>
    <span class="comment">// Do not embed SectionReader directly</span>
    <span class="comment">// to avoid having Read and Seek.</span>
    <span class="comment">// If a client wants Read and Seek it must use</span>
    <span class="comment">// Open() to avoid fighting over the seek offset</span>
    <span class="comment">// with other clients.</span>
    <a href="/io/">io</a>.<a href="/io/#ReaderAt">ReaderAt</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Section provides access to PE COFF section.

<h3 id="Section.Data">func (*Section) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/section.go#L89">Data</a>
    <a href="#Section.Data">¶</a></h3>
<pre>func (s *<a href="#Section">Section</a>) Data() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Data reads and returns the contents of the PE section s.

<h3 id="Section.Open">func (*Section) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/section.go#L99">Open</a>
    <a href="#Section.Open">¶</a></h3>
<pre>func (s *<a href="#Section">Section</a>) Open() <a href="/io/">io</a>.<a href="/io/#ReadSeeker">ReadSeeker</a></pre>

Open returns a new ReadSeeker reading the PE section s.

<h2 id="SectionHeader">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/section.go#L60">SectionHeader</a>
    <a href="#SectionHeader">¶</a></h2>
<pre>type SectionHeader struct {
<span id="SectionHeader.Name"></span>    Name                 <a href="/builtin/#string">string</a>
<span id="SectionHeader.VirtualSize"></span>    VirtualSize          <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.VirtualAddress"></span>    VirtualAddress       <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.Size"></span>    Size                 <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.Offset"></span>    Offset               <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.PointerToRelocations"></span>    PointerToRelocations <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.PointerToLineNumbers"></span>    PointerToLineNumbers <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.NumberOfRelocations"></span>    NumberOfRelocations  <a href="/builtin/#uint16">uint16</a>
<span id="SectionHeader.NumberOfLineNumbers"></span>    NumberOfLineNumbers  <a href="/builtin/#uint16">uint16</a>
<span id="SectionHeader.Characteristics"></span>    Characteristics      <a href="/builtin/#uint32">uint32</a>
}</pre>

SectionHeader is similar to SectionHeader32 with Name field replaced by Go
string.

<h2 id="SectionHeader32">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/section.go#L5">SectionHeader32</a>
    <a href="#SectionHeader32">¶</a></h2>
<pre>type SectionHeader32 struct {
<span id="SectionHeader32.Name"></span>    Name                 [8]<a href="/builtin/#uint8">uint8</a>
<span id="SectionHeader32.VirtualSize"></span>    VirtualSize          <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader32.VirtualAddress"></span>    VirtualAddress       <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader32.SizeOfRawData"></span>    SizeOfRawData        <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader32.PointerToRawData"></span>    PointerToRawData     <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader32.PointerToRelocations"></span>    PointerToRelocations <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader32.PointerToLineNumbers"></span>    PointerToLineNumbers <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader32.NumberOfRelocations"></span>    NumberOfRelocations  <a href="/builtin/#uint16">uint16</a>
<span id="SectionHeader32.NumberOfLineNumbers"></span>    NumberOfLineNumbers  <a href="/builtin/#uint16">uint16</a>
<span id="SectionHeader32.Characteristics"></span>    Characteristics      <a href="/builtin/#uint32">uint32</a>
}</pre>

SectionHeader32 represents real PE COFF section header.

<h2 id="StringTable">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/string.go#L13">StringTable</a>
    <a href="#StringTable">¶</a></h2>
<pre>type StringTable []<a href="/builtin/#byte">byte</a></pre>

StringTable is a COFF string table.

<h3 id="StringTable.String">func (StringTable) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/string.go#L46">String</a>
    <a href="#StringTable.String">¶</a></h3>
<pre>func (st <a href="#StringTable">StringTable</a>) String(start <a href="/builtin/#uint32">uint32</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

String extracts string from COFF string table st at offset start.

<h2 id="Symbol">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/debug/pe/symbol.go#L82">Symbol</a>
    <a href="#Symbol">¶</a></h2>
<pre>type Symbol struct {
<span id="Symbol.Name"></span>    Name          <a href="/builtin/#string">string</a>
<span id="Symbol.Value"></span>    Value         <a href="/builtin/#uint32">uint32</a>
<span id="Symbol.SectionNumber"></span>    SectionNumber <a href="/builtin/#int16">int16</a>
<span id="Symbol.Type"></span>    Type          <a href="/builtin/#uint16">uint16</a>
<span id="Symbol.StorageClass"></span>    StorageClass  <a href="/builtin/#uint8">uint8</a>
}</pre>

Symbol is similar to COFFSymbol with Name field replaced by Go string. Symbol
also does not have NumberOfAuxSymbols.


