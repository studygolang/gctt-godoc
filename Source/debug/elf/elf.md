version: 1.10
## package elf

  `import "debug/elf"`

## Overview

Package elf implements access to ELF object files.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func R_INFO(sym, typ uint32) uint64](#R_INFO)
- [func R_INFO32(sym, typ uint32) uint32](#R_INFO32)
- [func R_SYM32(info uint32) uint32](#R_SYM32)
- [func R_SYM64(info uint64) uint32](#R_SYM64)
- [func R_TYPE32(info uint32) uint32](#R_TYPE32)
- [func R_TYPE64(info uint64) uint32](#R_TYPE64)
- [func ST_INFO(bind SymBind, typ SymType) uint8](#ST_INFO)
- [type Chdr32](#Chdr32)
- [type Chdr64](#Chdr64)
- [type Class](#Class)
  - [func (i Class) GoString() string](#Class.GoString)
  - [func (i Class) String() string](#Class.String)
- [type CompressionType](#CompressionType)
  - [func (i CompressionType) GoString() string](#CompressionType.GoString)
  - [func (i CompressionType) String() string](#CompressionType.String)
- [type Data](#Data)
  - [func (i Data) GoString() string](#Data.GoString)
  - [func (i Data) String() string](#Data.String)
- [type Dyn32](#Dyn32)
- [type Dyn64](#Dyn64)
- [type DynFlag](#DynFlag)
  - [func (i DynFlag) GoString() string](#DynFlag.GoString)
  - [func (i DynFlag) String() string](#DynFlag.String)
- [type DynTag](#DynTag)
  - [func (i DynTag) GoString() string](#DynTag.GoString)
  - [func (i DynTag) String() string](#DynTag.String)
- [type File](#File)
  - [func NewFile(r io.ReaderAt) (*File, error)](#NewFile)
  - [func Open(name string) (*File, error)](#Open)
  - [func (f *File) Close() error](#File.Close)
  - [func (f *File) DWARF() (*dwarf.Data, error)](#File.DWARF)
  - [func (f *File) DynString(tag DynTag) ([]string, error)](#File.DynString)
  - [func (f *File) DynamicSymbols() ([]Symbol, error)](#File.DynamicSymbols)
  - [func (f *File) ImportedLibraries() ([]string, error)](#File.ImportedLibraries)
  - [func (f *File) ImportedSymbols() ([]ImportedSymbol, error)](#File.ImportedSymbols)
  - [func (f *File) Section(name string) *Section](#File.Section)
  - [func (f *File) SectionByType(typ SectionType) *Section](#File.SectionByType)
  - [func (f *File) Symbols() ([]Symbol, error)](#File.Symbols)
- [type FileHeader](#FileHeader)
- [type FormatError](#FormatError)
  - [func (e *FormatError) Error() string](#FormatError.Error)
- [type Header32](#Header32)
- [type Header64](#Header64)
- [type ImportedSymbol](#ImportedSymbol)
- [type Machine](#Machine)
  - [func (i Machine) GoString() string](#Machine.GoString)
  - [func (i Machine) String() string](#Machine.String)
- [type NType](#NType)
  - [func (i NType) GoString() string](#NType.GoString)
  - [func (i NType) String() string](#NType.String)
- [type OSABI](#OSABI)
  - [func (i OSABI) GoString() string](#OSABI.GoString)
  - [func (i OSABI) String() string](#OSABI.String)
- [type Prog](#Prog)
  - [func (p *Prog) Open() io.ReadSeeker](#Prog.Open)
- [type Prog32](#Prog32)
- [type Prog64](#Prog64)
- [type ProgFlag](#ProgFlag)
  - [func (i ProgFlag) GoString() string](#ProgFlag.GoString)
  - [func (i ProgFlag) String() string](#ProgFlag.String)
- [type ProgHeader](#ProgHeader)
- [type ProgType](#ProgType)
  - [func (i ProgType) GoString() string](#ProgType.GoString)
  - [func (i ProgType) String() string](#ProgType.String)
- [type R_386](#R_386)
  - [func (i R_386) GoString() string](#R_386.GoString)
  - [func (i R_386) String() string](#R_386.String)
- [type R_390](#R_390)
  - [func (i R_390) GoString() string](#R_390.GoString)
  - [func (i R_390) String() string](#R_390.String)
- [type R_AARCH64](#R_AARCH64)
  - [func (i R_AARCH64) GoString() string](#R_AARCH64.GoString)
  - [func (i R_AARCH64) String() string](#R_AARCH64.String)
- [type R_ALPHA](#R_ALPHA)
  - [func (i R_ALPHA) GoString() string](#R_ALPHA.GoString)
  - [func (i R_ALPHA) String() string](#R_ALPHA.String)
- [type R_ARM](#R_ARM)
  - [func (i R_ARM) GoString() string](#R_ARM.GoString)
  - [func (i R_ARM) String() string](#R_ARM.String)
- [type R_MIPS](#R_MIPS)
  - [func (i R_MIPS) GoString() string](#R_MIPS.GoString)
  - [func (i R_MIPS) String() string](#R_MIPS.String)
- [type R_PPC](#R_PPC)
  - [func (i R_PPC) GoString() string](#R_PPC.GoString)
  - [func (i R_PPC) String() string](#R_PPC.String)
- [type R_PPC64](#R_PPC64)
  - [func (i R_PPC64) GoString() string](#R_PPC64.GoString)
  - [func (i R_PPC64) String() string](#R_PPC64.String)
- [type R_SPARC](#R_SPARC)
  - [func (i R_SPARC) GoString() string](#R_SPARC.GoString)
  - [func (i R_SPARC) String() string](#R_SPARC.String)
- [type R_X86_64](#R_X86_64)
  - [func (i R_X86_64) GoString() string](#R_X86_64.GoString)
  - [func (i R_X86_64) String() string](#R_X86_64.String)
- [type Rel32](#Rel32)
- [type Rel64](#Rel64)
- [type Rela32](#Rela32)
- [type Rela64](#Rela64)
- [type Section](#Section)
  - [func (s *Section) Data() ([]byte, error)](#Section.Data)
  - [func (s *Section) Open() io.ReadSeeker](#Section.Open)
- [type Section32](#Section32)
- [type Section64](#Section64)
- [type SectionFlag](#SectionFlag)
  - [func (i SectionFlag) GoString() string](#SectionFlag.GoString)
  - [func (i SectionFlag) String() string](#SectionFlag.String)
- [type SectionHeader](#SectionHeader)
- [type SectionIndex](#SectionIndex)
  - [func (i SectionIndex) GoString() string](#SectionIndex.GoString)
  - [func (i SectionIndex) String() string](#SectionIndex.String)
- [type SectionType](#SectionType)
  - [func (i SectionType) GoString() string](#SectionType.GoString)
  - [func (i SectionType) String() string](#SectionType.String)
- [type Sym32](#Sym32)
- [type Sym64](#Sym64)
- [type SymBind](#SymBind)
  - [func ST_BIND(info uint8) SymBind](#ST_BIND)
  - [func (i SymBind) GoString() string](#SymBind.GoString)
  - [func (i SymBind) String() string](#SymBind.String)
- [type SymType](#SymType)
  - [func ST_TYPE(info uint8) SymType](#ST_TYPE)
  - [func (i SymType) GoString() string](#SymType.GoString)
  - [func (i SymType) String() string](#SymType.String)
- [type SymVis](#SymVis)
  - [func ST_VISIBILITY(other uint8) SymVis](#ST_VISIBILITY)
  - [func (i SymVis) GoString() string](#SymVis.GoString)
  - [func (i SymVis) String() string](#SymVis.String)
- [type Symbol](#Symbol)
- [type Type](#Type)
  - [func (i Type) GoString() string](#Type.GoString)
  - [func (i Type) String() string](#Type.String)
- [type Version](#Version)
  - [func (i Version) GoString() string](#Version.GoString)
  - [func (i Version) String() string](#Version.String)

### Package files
 [elf.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go) [file.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go) [reader.go](//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/reader.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="EI_CLASS">EI_CLASS</span>      = 4  <span class="comment">/* Class of machine. */</span>
    <span id="EI_DATA">EI_DATA</span>       = 5  <span class="comment">/* Data format. */</span>
    <span id="EI_VERSION">EI_VERSION</span>    = 6  <span class="comment">/* ELF format version. */</span>
    <span id="EI_OSABI">EI_OSABI</span>      = 7  <span class="comment">/* Operating system / ABI identification */</span>
    <span id="EI_ABIVERSION">EI_ABIVERSION</span> = 8  <span class="comment">/* ABI version */</span>
    <span id="EI_PAD">EI_PAD</span>        = 9  <span class="comment">/* Start of padding (per SVR4 ABI). */</span>
    <span id="EI_NIDENT">EI_NIDENT</span>     = 16 <span class="comment">/* Size of e_ident array. */</span>
)</pre>

Indexes into the Header.Ident array.

<pre>const <span id="ARM_MAGIC_TRAMP_NUMBER">ARM_MAGIC_TRAMP_NUMBER</span> = 0x5c000003</pre>

Magic number for the elf trampoline, chosen wisely to be an immediate value.

<pre>const <span id="ELFMAG">ELFMAG</span> = &#34;\177ELF&#34;</pre>

Initial magic number for ELF files.

<pre>const <span id="Sym32Size">Sym32Size</span> = 16</pre>


<pre>const <span id="Sym64Size">Sym64Size</span> = 24</pre>


<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrNoSymbols">ErrNoSymbols</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;no symbol section&#34;)</pre>

ErrNoSymbols is returned by File.Symbols and File.DynamicSymbols if there is no
such section in the File.

<h2 id="R_INFO">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2513">R_INFO</a>
    <a href="#R_INFO">¶</a></h2>
<pre>func R_INFO(sym, typ <a href="/builtin/#uint32">uint32</a>) <a href="/builtin/#uint64">uint64</a></pre>


<h2 id="R_INFO32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2411">R_INFO32</a>
    <a href="#R_INFO32">¶</a></h2>
<pre>func R_INFO32(sym, typ <a href="/builtin/#uint32">uint32</a>) <a href="/builtin/#uint32">uint32</a></pre>


<h2 id="R_SYM32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2409">R_SYM32</a>
    <a href="#R_SYM32">¶</a></h2>
<pre>func R_SYM32(info <a href="/builtin/#uint32">uint32</a>) <a href="/builtin/#uint32">uint32</a></pre>


<h2 id="R_SYM64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2511">R_SYM64</a>
    <a href="#R_SYM64">¶</a></h2>
<pre>func R_SYM64(info <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#uint32">uint32</a></pre>


<h2 id="R_TYPE32">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2410">R_TYPE32</a>
    <a href="#R_TYPE32">¶</a></h2>
<pre>func R_TYPE32(info <a href="/builtin/#uint32">uint32</a>) <a href="/builtin/#uint32">uint32</a></pre>


<h2 id="R_TYPE64">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2512">R_TYPE64</a>
    <a href="#R_TYPE64">¶</a></h2>
<pre>func R_TYPE64(info <a href="/builtin/#uint64">uint64</a>) <a href="/builtin/#uint32">uint32</a></pre>


<h2 id="ST_INFO">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2427">ST_INFO</a>
    <a href="#ST_INFO">¶</a></h2>
<pre>func ST_INFO(bind <a href="#SymBind">SymBind</a>, typ <a href="#SymType">SymType</a>) <a href="/builtin/#uint8">uint8</a></pre>


<h2 id="Chdr32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2386">Chdr32</a>
    <a href="#Chdr32">¶</a></h2>
<pre>type Chdr32 struct {
<span id="Chdr32.Type"></span>    Type      <a href="/builtin/#uint32">uint32</a>
<span id="Chdr32.Size"></span>    Size      <a href="/builtin/#uint32">uint32</a>
<span id="Chdr32.Addralign"></span>    Addralign <a href="/builtin/#uint32">uint32</a>
}</pre>

ELF32 Compression header.

<h2 id="Chdr64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2487">Chdr64</a>
    <a href="#Chdr64">¶</a></h2>
<pre>type Chdr64 struct {
<span id="Chdr64.Type"></span>    Type <a href="/builtin/#uint32">uint32</a>

<span id="Chdr64.Size"></span>    Size      <a href="/builtin/#uint64">uint64</a>
<span id="Chdr64.Addralign"></span>    Addralign <a href="/builtin/#uint64">uint64</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ELF64 Compression header.

<h2 id="Class">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L71">Class</a>
    <a href="#Class">¶</a></h2>
<pre>type Class <a href="/builtin/#byte">byte</a></pre>

Class is found in Header.Ident[EI_CLASS] and Header.Class.

<pre>const (
    <span id="ELFCLASSNONE">ELFCLASSNONE</span> <a href="#Class">Class</a> = 0 <span class="comment">/* Unknown class. */</span>
    <span id="ELFCLASS32">ELFCLASS32</span>   <a href="#Class">Class</a> = 1 <span class="comment">/* 32-bit architecture. */</span>
    <span id="ELFCLASS64">ELFCLASS64</span>   <a href="#Class">Class</a> = 2 <span class="comment">/* 64-bit architecture. */</span>
)</pre>


<h3 id="Class.GoString">func (Class) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L86">GoString</a>
    <a href="#Class.GoString">¶</a></h3>
<pre>func (i <a href="#Class">Class</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="Class.String">func (Class) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L85">String</a>
    <a href="#Class.String">¶</a></h3>
<pre>func (i <a href="#Class">Class</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="CompressionType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L427">CompressionType</a>
    <a href="#CompressionType">¶</a></h2>
<pre>type CompressionType <a href="/builtin/#int">int</a></pre>

Section compression type.

<pre>const (
    <span id="COMPRESS_ZLIB">COMPRESS_ZLIB</span>   <a href="#CompressionType">CompressionType</a> = 1          <span class="comment">/* ZLIB compression. */</span>
    <span id="COMPRESS_LOOS">COMPRESS_LOOS</span>   <a href="#CompressionType">CompressionType</a> = 0x60000000 <span class="comment">/* First OS-specific. */</span>
    <span id="COMPRESS_HIOS">COMPRESS_HIOS</span>   <a href="#CompressionType">CompressionType</a> = 0x6fffffff <span class="comment">/* Last OS-specific. */</span>
    <span id="COMPRESS_LOPROC">COMPRESS_LOPROC</span> <a href="#CompressionType">CompressionType</a> = 0x70000000 <span class="comment">/* First processor-specific type. */</span>
    <span id="COMPRESS_HIPROC">COMPRESS_HIPROC</span> <a href="#CompressionType">CompressionType</a> = 0x7fffffff <span class="comment">/* Last processor-specific type. */</span>
)</pre>


<h3 id="CompressionType.GoString">func (CompressionType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L446">GoString</a>
    <a href="#CompressionType.GoString">¶</a></h3>
<pre>func (i <a href="#CompressionType">CompressionType</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="CompressionType.String">func (CompressionType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L445">String</a>
    <a href="#CompressionType.String">¶</a></h3>
<pre>func (i <a href="#CompressionType">CompressionType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Data">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L89">Data</a>
    <a href="#Data">¶</a></h2>
<pre>type Data <a href="/builtin/#byte">byte</a></pre>

Data is found in Header.Ident[EI_DATA] and Header.Data.

<pre>const (
    <span id="ELFDATANONE">ELFDATANONE</span> <a href="#Data">Data</a> = 0 <span class="comment">/* Unknown data format. */</span>
    <span id="ELFDATA2LSB">ELFDATA2LSB</span> <a href="#Data">Data</a> = 1 <span class="comment">/* 2&#39;s complement little-endian. */</span>
    <span id="ELFDATA2MSB">ELFDATA2MSB</span> <a href="#Data">Data</a> = 2 <span class="comment">/* 2&#39;s complement big-endian. */</span>
)</pre>


<h3 id="Data.GoString">func (Data) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L104">GoString</a>
    <a href="#Data.GoString">¶</a></h3>
<pre>func (i <a href="#Data">Data</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="Data.String">func (Data) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L103">String</a>
    <a href="#Data.String">¶</a></h3>
<pre>func (i <a href="#Data">Data</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Dyn32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2380">Dyn32</a>
    <a href="#Dyn32">¶</a></h2>
<pre>type Dyn32 struct {
<span id="Dyn32.Tag"></span>    Tag <a href="/builtin/#int32">int32</a>  <span class="comment">/* Entry type. */</span>
<span id="Dyn32.Val"></span>    Val <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Integer/Address value. */</span>
}</pre>

ELF32 Dynamic structure. The ".dynamic" section contains an array of them.

<h2 id="Dyn64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2481">Dyn64</a>
    <a href="#Dyn64">¶</a></h2>
<pre>type Dyn64 struct {
<span id="Dyn64.Tag"></span>    Tag <a href="/builtin/#int64">int64</a>  <span class="comment">/* Entry type. */</span>
<span id="Dyn64.Val"></span>    Val <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Integer/address value */</span>
}</pre>

ELF64 Dynamic structure. The ".dynamic" section contains an array of them.

<h2 id="DynFlag">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L603">DynFlag</a>
    <a href="#DynFlag">¶</a></h2>
<pre>type DynFlag <a href="/builtin/#int">int</a></pre>

DT_FLAGS values.

<pre>const (
    <span id="DF_ORIGIN">DF_ORIGIN</span> <a href="#DynFlag">DynFlag</a> = 0x0001 <span class="comment">/* Indicates that the object being loaded may
       make reference to the
       $ORIGIN substitution string */</span>
    <span id="DF_SYMBOLIC">DF_SYMBOLIC</span> <a href="#DynFlag">DynFlag</a> = 0x0002 <span class="comment">/* Indicates &#34;symbolic&#34; linking. */</span>
    <span id="DF_TEXTREL">DF_TEXTREL</span>  <a href="#DynFlag">DynFlag</a> = 0x0004 <span class="comment">/* Indicates there may be relocations in non-writable segments. */</span>
    <span id="DF_BIND_NOW">DF_BIND_NOW</span> <a href="#DynFlag">DynFlag</a> = 0x0008 <span class="comment">/* Indicates that the dynamic linker should
       process all relocations for the object
       containing this entry before transferring
       control to the program. */</span>
    <span id="DF_STATIC_TLS">DF_STATIC_TLS</span> <a href="#DynFlag">DynFlag</a> = 0x0010 <span class="comment">/* Indicates that the shared object or
       executable contains code using a static
       thread-local storage scheme. */</span>
)</pre>


<h3 id="DynFlag.GoString">func (DynFlag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L629">GoString</a>
    <a href="#DynFlag.GoString">¶</a></h3>
<pre>func (i <a href="#DynFlag">DynFlag</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="DynFlag.String">func (DynFlag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L628">String</a>
    <a href="#DynFlag.String">¶</a></h3>
<pre>func (i <a href="#DynFlag">DynFlag</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="DynTag">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L505">DynTag</a>
    <a href="#DynTag">¶</a></h2>
<pre>type DynTag <a href="/builtin/#int">int</a></pre>

Dyn.Tag

<pre>const (
    <span id="DT_NULL">DT_NULL</span>         <a href="#DynTag">DynTag</a> = 0  <span class="comment">/* Terminating entry. */</span>
    <span id="DT_NEEDED">DT_NEEDED</span>       <a href="#DynTag">DynTag</a> = 1  <span class="comment">/* String table offset of a needed shared library. */</span>
    <span id="DT_PLTRELSZ">DT_PLTRELSZ</span>     <a href="#DynTag">DynTag</a> = 2  <span class="comment">/* Total size in bytes of PLT relocations. */</span>
    <span id="DT_PLTGOT">DT_PLTGOT</span>       <a href="#DynTag">DynTag</a> = 3  <span class="comment">/* Processor-dependent address. */</span>
    <span id="DT_HASH">DT_HASH</span>         <a href="#DynTag">DynTag</a> = 4  <span class="comment">/* Address of symbol hash table. */</span>
    <span id="DT_STRTAB">DT_STRTAB</span>       <a href="#DynTag">DynTag</a> = 5  <span class="comment">/* Address of string table. */</span>
    <span id="DT_SYMTAB">DT_SYMTAB</span>       <a href="#DynTag">DynTag</a> = 6  <span class="comment">/* Address of symbol table. */</span>
    <span id="DT_RELA">DT_RELA</span>         <a href="#DynTag">DynTag</a> = 7  <span class="comment">/* Address of ElfNN_Rela relocations. */</span>
    <span id="DT_RELASZ">DT_RELASZ</span>       <a href="#DynTag">DynTag</a> = 8  <span class="comment">/* Total size of ElfNN_Rela relocations. */</span>
    <span id="DT_RELAENT">DT_RELAENT</span>      <a href="#DynTag">DynTag</a> = 9  <span class="comment">/* Size of each ElfNN_Rela relocation entry. */</span>
    <span id="DT_STRSZ">DT_STRSZ</span>        <a href="#DynTag">DynTag</a> = 10 <span class="comment">/* Size of string table. */</span>
    <span id="DT_SYMENT">DT_SYMENT</span>       <a href="#DynTag">DynTag</a> = 11 <span class="comment">/* Size of each symbol table entry. */</span>
    <span id="DT_INIT">DT_INIT</span>         <a href="#DynTag">DynTag</a> = 12 <span class="comment">/* Address of initialization function. */</span>
    <span id="DT_FINI">DT_FINI</span>         <a href="#DynTag">DynTag</a> = 13 <span class="comment">/* Address of finalization function. */</span>
    <span id="DT_SONAME">DT_SONAME</span>       <a href="#DynTag">DynTag</a> = 14 <span class="comment">/* String table offset of shared object name. */</span>
    <span id="DT_RPATH">DT_RPATH</span>        <a href="#DynTag">DynTag</a> = 15 <span class="comment">/* String table offset of library path. [sup] */</span>
    <span id="DT_SYMBOLIC">DT_SYMBOLIC</span>     <a href="#DynTag">DynTag</a> = 16 <span class="comment">/* Indicates &#34;symbolic&#34; linking. [sup] */</span>
    <span id="DT_REL">DT_REL</span>          <a href="#DynTag">DynTag</a> = 17 <span class="comment">/* Address of ElfNN_Rel relocations. */</span>
    <span id="DT_RELSZ">DT_RELSZ</span>        <a href="#DynTag">DynTag</a> = 18 <span class="comment">/* Total size of ElfNN_Rel relocations. */</span>
    <span id="DT_RELENT">DT_RELENT</span>       <a href="#DynTag">DynTag</a> = 19 <span class="comment">/* Size of each ElfNN_Rel relocation. */</span>
    <span id="DT_PLTREL">DT_PLTREL</span>       <a href="#DynTag">DynTag</a> = 20 <span class="comment">/* Type of relocation used for PLT. */</span>
    <span id="DT_DEBUG">DT_DEBUG</span>        <a href="#DynTag">DynTag</a> = 21 <span class="comment">/* Reserved (not used). */</span>
    <span id="DT_TEXTREL">DT_TEXTREL</span>      <a href="#DynTag">DynTag</a> = 22 <span class="comment">/* Indicates there may be relocations in non-writable segments. [sup] */</span>
    <span id="DT_JMPREL">DT_JMPREL</span>       <a href="#DynTag">DynTag</a> = 23 <span class="comment">/* Address of PLT relocations. */</span>
    <span id="DT_BIND_NOW">DT_BIND_NOW</span>     <a href="#DynTag">DynTag</a> = 24 <span class="comment">/* [sup] */</span>
    <span id="DT_INIT_ARRAY">DT_INIT_ARRAY</span>   <a href="#DynTag">DynTag</a> = 25 <span class="comment">/* Address of the array of pointers to initialization functions */</span>
    <span id="DT_FINI_ARRAY">DT_FINI_ARRAY</span>   <a href="#DynTag">DynTag</a> = 26 <span class="comment">/* Address of the array of pointers to termination functions */</span>
    <span id="DT_INIT_ARRAYSZ">DT_INIT_ARRAYSZ</span> <a href="#DynTag">DynTag</a> = 27 <span class="comment">/* Size in bytes of the array of initialization functions. */</span>
    <span id="DT_FINI_ARRAYSZ">DT_FINI_ARRAYSZ</span> <a href="#DynTag">DynTag</a> = 28 <span class="comment">/* Size in bytes of the array of termination functions. */</span>
    <span id="DT_RUNPATH">DT_RUNPATH</span>      <a href="#DynTag">DynTag</a> = 29 <span class="comment">/* String table offset of a null-terminated library search path string. */</span>
    <span id="DT_FLAGS">DT_FLAGS</span>        <a href="#DynTag">DynTag</a> = 30 <span class="comment">/* Object specific flag values. */</span>
    <span id="DT_ENCODING">DT_ENCODING</span>     <a href="#DynTag">DynTag</a> = 32 <span class="comment">/* Values greater than or equal to DT_ENCODING
       and less than DT_LOOS follow the rules for
       the interpretation of the d_un union
       as follows: even == &#39;d_ptr&#39;, even == &#39;d_val&#39;
       or none */</span>
    <span id="DT_PREINIT_ARRAY">DT_PREINIT_ARRAY</span>   <a href="#DynTag">DynTag</a> = 32         <span class="comment">/* Address of the array of pointers to pre-initialization functions. */</span>
    <span id="DT_PREINIT_ARRAYSZ">DT_PREINIT_ARRAYSZ</span> <a href="#DynTag">DynTag</a> = 33         <span class="comment">/* Size in bytes of the array of pre-initialization functions. */</span>
    <span id="DT_LOOS">DT_LOOS</span>            <a href="#DynTag">DynTag</a> = 0x6000000d <span class="comment">/* First OS-specific */</span>
    <span id="DT_HIOS">DT_HIOS</span>            <a href="#DynTag">DynTag</a> = 0x6ffff000 <span class="comment">/* Last OS-specific */</span>
    <span id="DT_VERSYM">DT_VERSYM</span>          <a href="#DynTag">DynTag</a> = 0x6ffffff0
    <span id="DT_VERNEED">DT_VERNEED</span>         <a href="#DynTag">DynTag</a> = 0x6ffffffe
    <span id="DT_VERNEEDNUM">DT_VERNEEDNUM</span>      <a href="#DynTag">DynTag</a> = 0x6fffffff
    <span id="DT_LOPROC">DT_LOPROC</span>          <a href="#DynTag">DynTag</a> = 0x70000000 <span class="comment">/* First processor-specific type. */</span>
    <span id="DT_HIPROC">DT_HIPROC</span>          <a href="#DynTag">DynTag</a> = 0x7fffffff <span class="comment">/* Last processor-specific type. */</span>
)</pre>


<h3 id="DynTag.GoString">func (DynTag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L600">GoString</a>
    <a href="#DynTag.GoString">¶</a></h3>
<pre>func (i <a href="#DynTag">DynTag</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="DynTag.String">func (DynTag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L599">String</a>
    <a href="#DynTag.String">¶</a></h3>
<pre>func (i <a href="#DynTag">DynTag</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="File">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L41">File</a>
    <a href="#File">¶</a></h2>
<pre>type File struct {
    <a href="#FileHeader">FileHeader</a>
<span id="File.Sections"></span>    Sections []*<a href="#Section">Section</a>
<span id="File.Progs"></span>    Progs    []*<a href="#Prog">Prog</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A File represents an open ELF file.

<h3 id="NewFile">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L225">NewFile</a>
    <a href="#NewFile">¶</a></h3>
<pre>func NewFile(r <a href="/io/">io</a>.<a href="/io/#ReaderAt">ReaderAt</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

NewFile creates a new File for accessing an ELF binary in an underlying reader.
The ELF binary is expected to start at position 0 in the ReaderAt.

<h3 id="Open">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L186">Open</a>
    <a href="#Open">¶</a></h3>
<pre>func Open(name <a href="/builtin/#string">string</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

Open opens the named file using os.Open and prepares it for use as an ELF
binary.

<h3 id="File.Close">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L203">Close</a>
    <a href="#File.Close">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the File. If the File was created using NewFile directly instead of
Open, Close has no effect.

<h3 id="File.DWARF">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L1053">DWARF</a>
    <a href="#File.DWARF">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) DWARF() (*<a href="/debug/dwarf/">dwarf</a>.<a href="/debug/dwarf/#Data">Data</a>, <a href="/builtin/#error">error</a>)</pre>


<h3 id="File.DynString">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L1295">DynString</a>
    <a href="#File.DynString">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) DynString(tag <a href="#DynTag">DynTag</a>) ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

DynString returns the strings listed for the given tag in the file's dynamic
section.

The tag must be one that takes string values: DT_NEEDED, DT_SONAME, DT_RPATH, or
DT_RUNPATH.

<h3 id="File.DynamicSymbols">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L1161">DynamicSymbols</a>
    <a href="#File.DynamicSymbols">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) DynamicSymbols() ([]<a href="#Symbol">Symbol</a>, <a href="/builtin/#error">error</a>)</pre>

DynamicSymbols returns the dynamic symbol table for f. The symbols will be
listed in the order they appear in f.

For compatibility with Symbols, DynamicSymbols omits the null symbol at index 0.
After retrieving the symbols as symtab, an externally supplied index x
corresponds to symtab[x-1], not symtab[x].

<h3 id="File.ImportedLibraries">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L1286">ImportedLibraries</a>
    <a href="#File.ImportedLibraries">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) ImportedLibraries() ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

ImportedLibraries returns the names of all libraries referred to by the binary f
that are expected to be linked with the binary at dynamic link time.

<h3 id="File.ImportedSymbols">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L1176">ImportedSymbols</a>
    <a href="#File.ImportedSymbols">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) ImportedSymbols() ([]<a href="#ImportedSymbol">ImportedSymbol</a>, <a href="/builtin/#error">error</a>)</pre>

ImportedSymbols returns the names of all symbols referred to by the binary f
that are expected to be satisfied by other libraries at dynamic load time. It
does not return weak symbols.

<h3 id="File.Section">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L573">Section</a>
    <a href="#File.Section">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Section(name <a href="/builtin/#string">string</a>) *<a href="#Section">Section</a></pre>

Section returns a section with the given name, or nil if no such section exists.

<h3 id="File.SectionByType">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L214">SectionByType</a>
    <a href="#File.SectionByType">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) SectionByType(typ <a href="#SectionType">SectionType</a>) *<a href="#Section">Section</a></pre>

SectionByType returns the first section in f with the given type, or nil if
there is no such section.

<h3 id="File.Symbols">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L1150">Symbols</a>
    <a href="#File.Symbols">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Symbols() ([]<a href="#Symbol">Symbol</a>, <a href="/builtin/#error">error</a>)</pre>

Symbols returns the symbol table for f. The symbols will be listed in the order
they appear in f.

For compatibility with Go 1.0, Symbols omits the null symbol at index 0. After
retrieving the symbols as symtab, an externally supplied index x corresponds to
symtab[x-1], not symtab[x].

<h2 id="FileHeader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L28">FileHeader</a>
    <a href="#FileHeader">¶</a></h2>
<pre>type FileHeader struct {
<span id="FileHeader.Class"></span>    Class      <a href="#Class">Class</a>
<span id="FileHeader.Data"></span>    Data       <a href="#Data">Data</a>
<span id="FileHeader.Version"></span>    Version    <a href="#Version">Version</a>
<span id="FileHeader.OSABI"></span>    OSABI      <a href="#OSABI">OSABI</a>
<span id="FileHeader.ABIVersion"></span>    ABIVersion <a href="/builtin/#uint8">uint8</a>
<span id="FileHeader.ByteOrder"></span>    ByteOrder  <a href="/encoding/binary/">binary</a>.<a href="/encoding/binary/#ByteOrder">ByteOrder</a>
<span id="FileHeader.Type"></span>    Type       <a href="#Type">Type</a>
<span id="FileHeader.Machine"></span>    Machine    <a href="#Machine">Machine</a>
<span id="FileHeader.Entry"></span>    Entry      <a href="/builtin/#uint64">uint64</a>
}</pre>

A FileHeader represents an ELF file header.

<h2 id="FormatError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L170">FormatError</a>
    <a href="#FormatError">¶</a></h2>
<pre>type FormatError struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>


<h3 id="FormatError.Error">func (*FormatError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L176">Error</a>
    <a href="#FormatError.Error">¶</a></h3>
<pre>func (e *<a href="#FormatError">FormatError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Header32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2336">Header32</a>
    <a href="#Header32">¶</a></h2>
<pre>type Header32 struct {
<span id="Header32.Ident"></span>    Ident     [<a href="#EI_NIDENT">EI_NIDENT</a>]<a href="/builtin/#byte">byte</a> <span class="comment">/* File identification. */</span>
<span id="Header32.Type"></span>    Type      <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* File type. */</span>
<span id="Header32.Machine"></span>    Machine   <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Machine architecture. */</span>
<span id="Header32.Version"></span>    Version   <a href="/builtin/#uint32">uint32</a>          <span class="comment">/* ELF format version. */</span>
<span id="Header32.Entry"></span>    Entry     <a href="/builtin/#uint32">uint32</a>          <span class="comment">/* Entry point. */</span>
<span id="Header32.Phoff"></span>    Phoff     <a href="/builtin/#uint32">uint32</a>          <span class="comment">/* Program header file offset. */</span>
<span id="Header32.Shoff"></span>    Shoff     <a href="/builtin/#uint32">uint32</a>          <span class="comment">/* Section header file offset. */</span>
<span id="Header32.Flags"></span>    Flags     <a href="/builtin/#uint32">uint32</a>          <span class="comment">/* Architecture-specific flags. */</span>
<span id="Header32.Ehsize"></span>    Ehsize    <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Size of ELF header in bytes. */</span>
<span id="Header32.Phentsize"></span>    Phentsize <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Size of program header entry. */</span>
<span id="Header32.Phnum"></span>    Phnum     <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Number of program header entries. */</span>
<span id="Header32.Shentsize"></span>    Shentsize <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Size of section header entry. */</span>
<span id="Header32.Shnum"></span>    Shnum     <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Number of section header entries. */</span>
<span id="Header32.Shstrndx"></span>    Shstrndx  <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Section name strings section. */</span>
}</pre>

ELF32 File header.

<h2 id="Header64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2437">Header64</a>
    <a href="#Header64">¶</a></h2>
<pre>type Header64 struct {
<span id="Header64.Ident"></span>    Ident     [<a href="#EI_NIDENT">EI_NIDENT</a>]<a href="/builtin/#byte">byte</a> <span class="comment">/* File identification. */</span>
<span id="Header64.Type"></span>    Type      <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* File type. */</span>
<span id="Header64.Machine"></span>    Machine   <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Machine architecture. */</span>
<span id="Header64.Version"></span>    Version   <a href="/builtin/#uint32">uint32</a>          <span class="comment">/* ELF format version. */</span>
<span id="Header64.Entry"></span>    Entry     <a href="/builtin/#uint64">uint64</a>          <span class="comment">/* Entry point. */</span>
<span id="Header64.Phoff"></span>    Phoff     <a href="/builtin/#uint64">uint64</a>          <span class="comment">/* Program header file offset. */</span>
<span id="Header64.Shoff"></span>    Shoff     <a href="/builtin/#uint64">uint64</a>          <span class="comment">/* Section header file offset. */</span>
<span id="Header64.Flags"></span>    Flags     <a href="/builtin/#uint32">uint32</a>          <span class="comment">/* Architecture-specific flags. */</span>
<span id="Header64.Ehsize"></span>    Ehsize    <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Size of ELF header in bytes. */</span>
<span id="Header64.Phentsize"></span>    Phentsize <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Size of program header entry. */</span>
<span id="Header64.Phnum"></span>    Phnum     <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Number of program header entries. */</span>
<span id="Header64.Shentsize"></span>    Shentsize <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Size of section header entry. */</span>
<span id="Header64.Shnum"></span>    Shnum     <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Number of section header entries. */</span>
<span id="Header64.Shstrndx"></span>    Shstrndx  <a href="/builtin/#uint16">uint16</a>          <span class="comment">/* Section name strings section. */</span>
}</pre>

ELF64 file header.

<h2 id="ImportedSymbol">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L1166">ImportedSymbol</a>
    <a href="#ImportedSymbol">¶</a></h2>
<pre>type ImportedSymbol struct {
<span id="ImportedSymbol.Name"></span>    Name    <a href="/builtin/#string">string</a>
<span id="ImportedSymbol.Version"></span>    Version <a href="/builtin/#string">string</a>
<span id="ImportedSymbol.Library"></span>    Library <a href="/builtin/#string">string</a>
}</pre>


<h2 id="Machine">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L183">Machine</a>
    <a href="#Machine">¶</a></h2>
<pre>type Machine <a href="/builtin/#uint16">uint16</a></pre>

Machine is found in Header.Machine.

<pre>const (
    <span id="EM_NONE">EM_NONE</span>        <a href="#Machine">Machine</a> = 0   <span class="comment">/* Unknown machine. */</span>
    <span id="EM_M32">EM_M32</span>         <a href="#Machine">Machine</a> = 1   <span class="comment">/* AT&amp;T WE32100. */</span>
    <span id="EM_SPARC">EM_SPARC</span>       <a href="#Machine">Machine</a> = 2   <span class="comment">/* Sun SPARC. */</span>
    <span id="EM_386">EM_386</span>         <a href="#Machine">Machine</a> = 3   <span class="comment">/* Intel i386. */</span>
    <span id="EM_68K">EM_68K</span>         <a href="#Machine">Machine</a> = 4   <span class="comment">/* Motorola 68000. */</span>
    <span id="EM_88K">EM_88K</span>         <a href="#Machine">Machine</a> = 5   <span class="comment">/* Motorola 88000. */</span>
    <span id="EM_860">EM_860</span>         <a href="#Machine">Machine</a> = 7   <span class="comment">/* Intel i860. */</span>
    <span id="EM_MIPS">EM_MIPS</span>        <a href="#Machine">Machine</a> = 8   <span class="comment">/* MIPS R3000 Big-Endian only. */</span>
    <span id="EM_S370">EM_S370</span>        <a href="#Machine">Machine</a> = 9   <span class="comment">/* IBM System/370. */</span>
    <span id="EM_MIPS_RS3_LE">EM_MIPS_RS3_LE</span> <a href="#Machine">Machine</a> = 10  <span class="comment">/* MIPS R3000 Little-Endian. */</span>
    <span id="EM_PARISC">EM_PARISC</span>      <a href="#Machine">Machine</a> = 15  <span class="comment">/* HP PA-RISC. */</span>
    <span id="EM_VPP500">EM_VPP500</span>      <a href="#Machine">Machine</a> = 17  <span class="comment">/* Fujitsu VPP500. */</span>
    <span id="EM_SPARC32PLUS">EM_SPARC32PLUS</span> <a href="#Machine">Machine</a> = 18  <span class="comment">/* SPARC v8plus. */</span>
    <span id="EM_960">EM_960</span>         <a href="#Machine">Machine</a> = 19  <span class="comment">/* Intel 80960. */</span>
    <span id="EM_PPC">EM_PPC</span>         <a href="#Machine">Machine</a> = 20  <span class="comment">/* PowerPC 32-bit. */</span>
    <span id="EM_PPC64">EM_PPC64</span>       <a href="#Machine">Machine</a> = 21  <span class="comment">/* PowerPC 64-bit. */</span>
    <span id="EM_S390">EM_S390</span>        <a href="#Machine">Machine</a> = 22  <span class="comment">/* IBM System/390. */</span>
    <span id="EM_V800">EM_V800</span>        <a href="#Machine">Machine</a> = 36  <span class="comment">/* NEC V800. */</span>
    <span id="EM_FR20">EM_FR20</span>        <a href="#Machine">Machine</a> = 37  <span class="comment">/* Fujitsu FR20. */</span>
    <span id="EM_RH32">EM_RH32</span>        <a href="#Machine">Machine</a> = 38  <span class="comment">/* TRW RH-32. */</span>
    <span id="EM_RCE">EM_RCE</span>         <a href="#Machine">Machine</a> = 39  <span class="comment">/* Motorola RCE. */</span>
    <span id="EM_ARM">EM_ARM</span>         <a href="#Machine">Machine</a> = 40  <span class="comment">/* ARM. */</span>
    <span id="EM_SH">EM_SH</span>          <a href="#Machine">Machine</a> = 42  <span class="comment">/* Hitachi SH. */</span>
    <span id="EM_SPARCV9">EM_SPARCV9</span>     <a href="#Machine">Machine</a> = 43  <span class="comment">/* SPARC v9 64-bit. */</span>
    <span id="EM_TRICORE">EM_TRICORE</span>     <a href="#Machine">Machine</a> = 44  <span class="comment">/* Siemens TriCore embedded processor. */</span>
    <span id="EM_ARC">EM_ARC</span>         <a href="#Machine">Machine</a> = 45  <span class="comment">/* Argonaut RISC Core. */</span>
    <span id="EM_H8_300">EM_H8_300</span>      <a href="#Machine">Machine</a> = 46  <span class="comment">/* Hitachi H8/300. */</span>
    <span id="EM_H8_300H">EM_H8_300H</span>     <a href="#Machine">Machine</a> = 47  <span class="comment">/* Hitachi H8/300H. */</span>
    <span id="EM_H8S">EM_H8S</span>         <a href="#Machine">Machine</a> = 48  <span class="comment">/* Hitachi H8S. */</span>
    <span id="EM_H8_500">EM_H8_500</span>      <a href="#Machine">Machine</a> = 49  <span class="comment">/* Hitachi H8/500. */</span>
    <span id="EM_IA_64">EM_IA_64</span>       <a href="#Machine">Machine</a> = 50  <span class="comment">/* Intel IA-64 Processor. */</span>
    <span id="EM_MIPS_X">EM_MIPS_X</span>      <a href="#Machine">Machine</a> = 51  <span class="comment">/* Stanford MIPS-X. */</span>
    <span id="EM_COLDFIRE">EM_COLDFIRE</span>    <a href="#Machine">Machine</a> = 52  <span class="comment">/* Motorola ColdFire. */</span>
    <span id="EM_68HC12">EM_68HC12</span>      <a href="#Machine">Machine</a> = 53  <span class="comment">/* Motorola M68HC12. */</span>
    <span id="EM_MMA">EM_MMA</span>         <a href="#Machine">Machine</a> = 54  <span class="comment">/* Fujitsu MMA. */</span>
    <span id="EM_PCP">EM_PCP</span>         <a href="#Machine">Machine</a> = 55  <span class="comment">/* Siemens PCP. */</span>
    <span id="EM_NCPU">EM_NCPU</span>        <a href="#Machine">Machine</a> = 56  <span class="comment">/* Sony nCPU. */</span>
    <span id="EM_NDR1">EM_NDR1</span>        <a href="#Machine">Machine</a> = 57  <span class="comment">/* Denso NDR1 microprocessor. */</span>
    <span id="EM_STARCORE">EM_STARCORE</span>    <a href="#Machine">Machine</a> = 58  <span class="comment">/* Motorola Star*Core processor. */</span>
    <span id="EM_ME16">EM_ME16</span>        <a href="#Machine">Machine</a> = 59  <span class="comment">/* Toyota ME16 processor. */</span>
    <span id="EM_ST100">EM_ST100</span>       <a href="#Machine">Machine</a> = 60  <span class="comment">/* STMicroelectronics ST100 processor. */</span>
    <span id="EM_TINYJ">EM_TINYJ</span>       <a href="#Machine">Machine</a> = 61  <span class="comment">/* Advanced Logic Corp. TinyJ processor. */</span>
    <span id="EM_X86_64">EM_X86_64</span>      <a href="#Machine">Machine</a> = 62  <span class="comment">/* Advanced Micro Devices x86-64 */</span>
    <span id="EM_AARCH64">EM_AARCH64</span>     <a href="#Machine">Machine</a> = 183 <span class="comment">/* ARM 64-bit Architecture (AArch64) */</span>

    <span class="comment">/* Non-standard or deprecated. */</span>
    <span id="EM_486">EM_486</span>         <a href="#Machine">Machine</a> = 6      <span class="comment">/* Intel i486. */</span>
    <span id="EM_MIPS_RS4_BE">EM_MIPS_RS4_BE</span> <a href="#Machine">Machine</a> = 10     <span class="comment">/* MIPS R4000 Big-Endian */</span>
    <span id="EM_ALPHA_STD">EM_ALPHA_STD</span>   <a href="#Machine">Machine</a> = 41     <span class="comment">/* Digital Alpha (standard value). */</span>
    <span id="EM_ALPHA">EM_ALPHA</span>       <a href="#Machine">Machine</a> = 0x9026 <span class="comment">/* Alpha (written in the absence of an ABI) */</span>
)</pre>


<h3 id="Machine.GoString">func (Machine) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L291">GoString</a>
    <a href="#Machine.GoString">¶</a></h3>
<pre>func (i <a href="#Machine">Machine</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="Machine.String">func (Machine) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L290">String</a>
    <a href="#Machine.String">¶</a></h3>
<pre>func (i <a href="#Machine">Machine</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="NType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L632">NType</a>
    <a href="#NType">¶</a></h2>
<pre>type NType <a href="/builtin/#int">int</a></pre>

NType values; used in core files.

<pre>const (
    <span id="NT_PRSTATUS">NT_PRSTATUS</span> <a href="#NType">NType</a> = 1 <span class="comment">/* Process status. */</span>
    <span id="NT_FPREGSET">NT_FPREGSET</span> <a href="#NType">NType</a> = 2 <span class="comment">/* Floating point registers. */</span>
    <span id="NT_PRPSINFO">NT_PRPSINFO</span> <a href="#NType">NType</a> = 3 <span class="comment">/* Process state info. */</span>
)</pre>


<h3 id="NType.GoString">func (NType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L647">GoString</a>
    <a href="#NType.GoString">¶</a></h3>
<pre>func (i <a href="#NType">NType</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="NType.String">func (NType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L646">String</a>
    <a href="#NType.String">¶</a></h3>
<pre>func (i <a href="#NType">NType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="OSABI">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L107">OSABI</a>
    <a href="#OSABI">¶</a></h2>
<pre>type OSABI <a href="/builtin/#byte">byte</a></pre>

OSABI is found in Header.Ident[EI_OSABI] and Header.OSABI.

<pre>const (
    <span id="ELFOSABI_NONE">ELFOSABI_NONE</span>       <a href="#OSABI">OSABI</a> = 0   <span class="comment">/* UNIX System V ABI */</span>
    <span id="ELFOSABI_HPUX">ELFOSABI_HPUX</span>       <a href="#OSABI">OSABI</a> = 1   <span class="comment">/* HP-UX operating system */</span>
    <span id="ELFOSABI_NETBSD">ELFOSABI_NETBSD</span>     <a href="#OSABI">OSABI</a> = 2   <span class="comment">/* NetBSD */</span>
    <span id="ELFOSABI_LINUX">ELFOSABI_LINUX</span>      <a href="#OSABI">OSABI</a> = 3   <span class="comment">/* GNU/Linux */</span>
    <span id="ELFOSABI_HURD">ELFOSABI_HURD</span>       <a href="#OSABI">OSABI</a> = 4   <span class="comment">/* GNU/Hurd */</span>
    <span id="ELFOSABI_86OPEN">ELFOSABI_86OPEN</span>     <a href="#OSABI">OSABI</a> = 5   <span class="comment">/* 86Open common IA32 ABI */</span>
    <span id="ELFOSABI_SOLARIS">ELFOSABI_SOLARIS</span>    <a href="#OSABI">OSABI</a> = 6   <span class="comment">/* Solaris */</span>
    <span id="ELFOSABI_AIX">ELFOSABI_AIX</span>        <a href="#OSABI">OSABI</a> = 7   <span class="comment">/* AIX */</span>
    <span id="ELFOSABI_IRIX">ELFOSABI_IRIX</span>       <a href="#OSABI">OSABI</a> = 8   <span class="comment">/* IRIX */</span>
    <span id="ELFOSABI_FREEBSD">ELFOSABI_FREEBSD</span>    <a href="#OSABI">OSABI</a> = 9   <span class="comment">/* FreeBSD */</span>
    <span id="ELFOSABI_TRU64">ELFOSABI_TRU64</span>      <a href="#OSABI">OSABI</a> = 10  <span class="comment">/* TRU64 UNIX */</span>
    <span id="ELFOSABI_MODESTO">ELFOSABI_MODESTO</span>    <a href="#OSABI">OSABI</a> = 11  <span class="comment">/* Novell Modesto */</span>
    <span id="ELFOSABI_OPENBSD">ELFOSABI_OPENBSD</span>    <a href="#OSABI">OSABI</a> = 12  <span class="comment">/* OpenBSD */</span>
    <span id="ELFOSABI_OPENVMS">ELFOSABI_OPENVMS</span>    <a href="#OSABI">OSABI</a> = 13  <span class="comment">/* Open VMS */</span>
    <span id="ELFOSABI_NSK">ELFOSABI_NSK</span>        <a href="#OSABI">OSABI</a> = 14  <span class="comment">/* HP Non-Stop Kernel */</span>
    <span id="ELFOSABI_ARM">ELFOSABI_ARM</span>        <a href="#OSABI">OSABI</a> = 97  <span class="comment">/* ARM */</span>
    <span id="ELFOSABI_STANDALONE">ELFOSABI_STANDALONE</span> <a href="#OSABI">OSABI</a> = 255 <span class="comment">/* Standalone (embedded) application */</span>
)</pre>


<h3 id="OSABI.GoString">func (OSABI) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L150">GoString</a>
    <a href="#OSABI.GoString">¶</a></h3>
<pre>func (i <a href="#OSABI">OSABI</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="OSABI.String">func (OSABI) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L149">String</a>
    <a href="#OSABI.String">¶</a></h3>
<pre>func (i <a href="#OSABI">OSABI</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Prog">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L142">Prog</a>
    <a href="#Prog">¶</a></h2>
<pre>type Prog struct {
    <a href="#ProgHeader">ProgHeader</a>

    <span class="comment">// Embed ReaderAt for ReadAt method.</span>
    <span class="comment">// Do not embed SectionReader directly</span>
    <span class="comment">// to avoid having Read and Seek.</span>
    <span class="comment">// If a client wants Read and Seek it must use</span>
    <span class="comment">// Open() to avoid fighting over the seek offset</span>
    <span class="comment">// with other clients.</span>
    <a href="/io/">io</a>.<a href="/io/#ReaderAt">ReaderAt</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Prog represents a single ELF program header in an ELF binary.

<h3 id="Prog.Open">func (*Prog) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L156">Open</a>
    <a href="#Prog.Open">¶</a></h3>
<pre>func (p *<a href="#Prog">Prog</a>) Open() <a href="/io/">io</a>.<a href="/io/#ReadSeeker">ReadSeeker</a></pre>

Open returns a new ReadSeeker reading the ELF program body.

<h2 id="Prog32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2368">Prog32</a>
    <a href="#Prog32">¶</a></h2>
<pre>type Prog32 struct {
<span id="Prog32.Type"></span>    Type   <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Entry type. */</span>
<span id="Prog32.Off"></span>    Off    <a href="/builtin/#uint32">uint32</a> <span class="comment">/* File offset of contents. */</span>
<span id="Prog32.Vaddr"></span>    Vaddr  <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Virtual address in memory image. */</span>
<span id="Prog32.Paddr"></span>    Paddr  <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Physical address (not used). */</span>
<span id="Prog32.Filesz"></span>    Filesz <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Size of contents in file. */</span>
<span id="Prog32.Memsz"></span>    Memsz  <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Size of contents in memory. */</span>
<span id="Prog32.Flags"></span>    Flags  <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Access permission flags. */</span>
<span id="Prog32.Align"></span>    Align  <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Alignment in memory and file. */</span>
}</pre>

ELF32 Program header.

<h2 id="Prog64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2469">Prog64</a>
    <a href="#Prog64">¶</a></h2>
<pre>type Prog64 struct {
<span id="Prog64.Type"></span>    Type   <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Entry type. */</span>
<span id="Prog64.Flags"></span>    Flags  <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Access permission flags. */</span>
<span id="Prog64.Off"></span>    Off    <a href="/builtin/#uint64">uint64</a> <span class="comment">/* File offset of contents. */</span>
<span id="Prog64.Vaddr"></span>    Vaddr  <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Virtual address in memory image. */</span>
<span id="Prog64.Paddr"></span>    Paddr  <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Physical address (not used). */</span>
<span id="Prog64.Filesz"></span>    Filesz <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Size of contents in file. */</span>
<span id="Prog64.Memsz"></span>    Memsz  <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Size of contents in memory. */</span>
<span id="Prog64.Align"></span>    Align  <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Alignment in memory and file. */</span>
}</pre>

ELF64 Program header.

<h2 id="ProgFlag">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L485">ProgFlag</a>
    <a href="#ProgFlag">¶</a></h2>
<pre>type ProgFlag <a href="/builtin/#uint32">uint32</a></pre>

Prog.Flag

<pre>const (
    <span id="PF_X">PF_X</span>        <a href="#ProgFlag">ProgFlag</a> = 0x1        <span class="comment">/* Executable. */</span>
    <span id="PF_W">PF_W</span>        <a href="#ProgFlag">ProgFlag</a> = 0x2        <span class="comment">/* Writable. */</span>
    <span id="PF_R">PF_R</span>        <a href="#ProgFlag">ProgFlag</a> = 0x4        <span class="comment">/* Readable. */</span>
    <span id="PF_MASKOS">PF_MASKOS</span>   <a href="#ProgFlag">ProgFlag</a> = 0x0ff00000 <span class="comment">/* Operating system-specific. */</span>
    <span id="PF_MASKPROC">PF_MASKPROC</span> <a href="#ProgFlag">ProgFlag</a> = 0xf0000000 <span class="comment">/* Processor-specific. */</span>
)</pre>


<h3 id="ProgFlag.GoString">func (ProgFlag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L502">GoString</a>
    <a href="#ProgFlag.GoString">¶</a></h3>
<pre>func (i <a href="#ProgFlag">ProgFlag</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="ProgFlag.String">func (ProgFlag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L501">String</a>
    <a href="#ProgFlag.String">¶</a></h3>
<pre>func (i <a href="#ProgFlag">ProgFlag</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="ProgHeader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L130">ProgHeader</a>
    <a href="#ProgHeader">¶</a></h2>
<pre>type ProgHeader struct {
<span id="ProgHeader.Type"></span>    Type   <a href="#ProgType">ProgType</a>
<span id="ProgHeader.Flags"></span>    Flags  <a href="#ProgFlag">ProgFlag</a>
<span id="ProgHeader.Off"></span>    Off    <a href="/builtin/#uint64">uint64</a>
<span id="ProgHeader.Vaddr"></span>    Vaddr  <a href="/builtin/#uint64">uint64</a>
<span id="ProgHeader.Paddr"></span>    Paddr  <a href="/builtin/#uint64">uint64</a>
<span id="ProgHeader.Filesz"></span>    Filesz <a href="/builtin/#uint64">uint64</a>
<span id="ProgHeader.Memsz"></span>    Memsz  <a href="/builtin/#uint64">uint64</a>
<span id="ProgHeader.Align"></span>    Align  <a href="/builtin/#uint64">uint64</a>
}</pre>

A ProgHeader represents a single ELF program header.

<h2 id="ProgType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L449">ProgType</a>
    <a href="#ProgType">¶</a></h2>
<pre>type ProgType <a href="/builtin/#int">int</a></pre>

Prog.Type

<pre>const (
    <span id="PT_NULL">PT_NULL</span>    <a href="#ProgType">ProgType</a> = 0          <span class="comment">/* Unused entry. */</span>
    <span id="PT_LOAD">PT_LOAD</span>    <a href="#ProgType">ProgType</a> = 1          <span class="comment">/* Loadable segment. */</span>
    <span id="PT_DYNAMIC">PT_DYNAMIC</span> <a href="#ProgType">ProgType</a> = 2          <span class="comment">/* Dynamic linking information segment. */</span>
    <span id="PT_INTERP">PT_INTERP</span>  <a href="#ProgType">ProgType</a> = 3          <span class="comment">/* Pathname of interpreter. */</span>
    <span id="PT_NOTE">PT_NOTE</span>    <a href="#ProgType">ProgType</a> = 4          <span class="comment">/* Auxiliary information. */</span>
    <span id="PT_SHLIB">PT_SHLIB</span>   <a href="#ProgType">ProgType</a> = 5          <span class="comment">/* Reserved (not used). */</span>
    <span id="PT_PHDR">PT_PHDR</span>    <a href="#ProgType">ProgType</a> = 6          <span class="comment">/* Location of program header itself. */</span>
    <span id="PT_TLS">PT_TLS</span>     <a href="#ProgType">ProgType</a> = 7          <span class="comment">/* Thread local storage segment */</span>
    <span id="PT_LOOS">PT_LOOS</span>    <a href="#ProgType">ProgType</a> = 0x60000000 <span class="comment">/* First OS-specific. */</span>
    <span id="PT_HIOS">PT_HIOS</span>    <a href="#ProgType">ProgType</a> = 0x6fffffff <span class="comment">/* Last OS-specific. */</span>
    <span id="PT_LOPROC">PT_LOPROC</span>  <a href="#ProgType">ProgType</a> = 0x70000000 <span class="comment">/* First processor-specific type. */</span>
    <span id="PT_HIPROC">PT_HIPROC</span>  <a href="#ProgType">ProgType</a> = 0x7fffffff <span class="comment">/* Last processor-specific type. */</span>
)</pre>


<h3 id="ProgType.GoString">func (ProgType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L482">GoString</a>
    <a href="#ProgType.GoString">¶</a></h3>
<pre>func (i <a href="#ProgType">ProgType</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="ProgType.String">func (ProgType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L481">String</a>
    <a href="#ProgType.String">¶</a></h3>
<pre>func (i <a href="#ProgType">ProgType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="R_386">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1472">R_386</a>
    <a href="#R_386">¶</a></h2>
<pre>type R_386 <a href="/builtin/#int">int</a></pre>

Relocation types for 386.

<pre>const (
    <span id="R_386_NONE">R_386_NONE</span>          <a href="#R_386">R_386</a> = 0  <span class="comment">/* No relocation. */</span>
    <span id="R_386_32">R_386_32</span>            <a href="#R_386">R_386</a> = 1  <span class="comment">/* Add symbol value. */</span>
    <span id="R_386_PC32">R_386_PC32</span>          <a href="#R_386">R_386</a> = 2  <span class="comment">/* Add PC-relative symbol value. */</span>
    <span id="R_386_GOT32">R_386_GOT32</span>         <a href="#R_386">R_386</a> = 3  <span class="comment">/* Add PC-relative GOT offset. */</span>
    <span id="R_386_PLT32">R_386_PLT32</span>         <a href="#R_386">R_386</a> = 4  <span class="comment">/* Add PC-relative PLT offset. */</span>
    <span id="R_386_COPY">R_386_COPY</span>          <a href="#R_386">R_386</a> = 5  <span class="comment">/* Copy data from shared object. */</span>
    <span id="R_386_GLOB_DAT">R_386_GLOB_DAT</span>      <a href="#R_386">R_386</a> = 6  <span class="comment">/* Set GOT entry to data address. */</span>
    <span id="R_386_JMP_SLOT">R_386_JMP_SLOT</span>      <a href="#R_386">R_386</a> = 7  <span class="comment">/* Set GOT entry to code address. */</span>
    <span id="R_386_RELATIVE">R_386_RELATIVE</span>      <a href="#R_386">R_386</a> = 8  <span class="comment">/* Add load address of shared object. */</span>
    <span id="R_386_GOTOFF">R_386_GOTOFF</span>        <a href="#R_386">R_386</a> = 9  <span class="comment">/* Add GOT-relative symbol address. */</span>
    <span id="R_386_GOTPC">R_386_GOTPC</span>         <a href="#R_386">R_386</a> = 10 <span class="comment">/* Add PC-relative GOT table address. */</span>
    <span id="R_386_32PLT">R_386_32PLT</span>         <a href="#R_386">R_386</a> = 11
    <span id="R_386_TLS_TPOFF">R_386_TLS_TPOFF</span>     <a href="#R_386">R_386</a> = 14 <span class="comment">/* Negative offset in static TLS block */</span>
    <span id="R_386_TLS_IE">R_386_TLS_IE</span>        <a href="#R_386">R_386</a> = 15 <span class="comment">/* Absolute address of GOT for -ve static TLS */</span>
    <span id="R_386_TLS_GOTIE">R_386_TLS_GOTIE</span>     <a href="#R_386">R_386</a> = 16 <span class="comment">/* GOT entry for negative static TLS block */</span>
    <span id="R_386_TLS_LE">R_386_TLS_LE</span>        <a href="#R_386">R_386</a> = 17 <span class="comment">/* Negative offset relative to static TLS */</span>
    <span id="R_386_TLS_GD">R_386_TLS_GD</span>        <a href="#R_386">R_386</a> = 18 <span class="comment">/* 32 bit offset to GOT (index,off) pair */</span>
    <span id="R_386_TLS_LDM">R_386_TLS_LDM</span>       <a href="#R_386">R_386</a> = 19 <span class="comment">/* 32 bit offset to GOT (index,zero) pair */</span>
    <span id="R_386_16">R_386_16</span>            <a href="#R_386">R_386</a> = 20
    <span id="R_386_PC16">R_386_PC16</span>          <a href="#R_386">R_386</a> = 21
    <span id="R_386_8">R_386_8</span>             <a href="#R_386">R_386</a> = 22
    <span id="R_386_PC8">R_386_PC8</span>           <a href="#R_386">R_386</a> = 23
    <span id="R_386_TLS_GD_32">R_386_TLS_GD_32</span>     <a href="#R_386">R_386</a> = 24 <span class="comment">/* 32 bit offset to GOT (index,off) pair */</span>
    <span id="R_386_TLS_GD_PUSH">R_386_TLS_GD_PUSH</span>   <a href="#R_386">R_386</a> = 25 <span class="comment">/* pushl instruction for Sun ABI GD sequence */</span>
    <span id="R_386_TLS_GD_CALL">R_386_TLS_GD_CALL</span>   <a href="#R_386">R_386</a> = 26 <span class="comment">/* call instruction for Sun ABI GD sequence */</span>
    <span id="R_386_TLS_GD_POP">R_386_TLS_GD_POP</span>    <a href="#R_386">R_386</a> = 27 <span class="comment">/* popl instruction for Sun ABI GD sequence */</span>
    <span id="R_386_TLS_LDM_32">R_386_TLS_LDM_32</span>    <a href="#R_386">R_386</a> = 28 <span class="comment">/* 32 bit offset to GOT (index,zero) pair */</span>
    <span id="R_386_TLS_LDM_PUSH">R_386_TLS_LDM_PUSH</span>  <a href="#R_386">R_386</a> = 29 <span class="comment">/* pushl instruction for Sun ABI LD sequence */</span>
    <span id="R_386_TLS_LDM_CALL">R_386_TLS_LDM_CALL</span>  <a href="#R_386">R_386</a> = 30 <span class="comment">/* call instruction for Sun ABI LD sequence */</span>
    <span id="R_386_TLS_LDM_POP">R_386_TLS_LDM_POP</span>   <a href="#R_386">R_386</a> = 31 <span class="comment">/* popl instruction for Sun ABI LD sequence */</span>
    <span id="R_386_TLS_LDO_32">R_386_TLS_LDO_32</span>    <a href="#R_386">R_386</a> = 32 <span class="comment">/* 32 bit offset from start of TLS block */</span>
    <span id="R_386_TLS_IE_32">R_386_TLS_IE_32</span>     <a href="#R_386">R_386</a> = 33 <span class="comment">/* 32 bit offset to GOT static TLS offset entry */</span>
    <span id="R_386_TLS_LE_32">R_386_TLS_LE_32</span>     <a href="#R_386">R_386</a> = 34 <span class="comment">/* 32 bit offset within static TLS block */</span>
    <span id="R_386_TLS_DTPMOD32">R_386_TLS_DTPMOD32</span>  <a href="#R_386">R_386</a> = 35 <span class="comment">/* GOT entry containing TLS index */</span>
    <span id="R_386_TLS_DTPOFF32">R_386_TLS_DTPOFF32</span>  <a href="#R_386">R_386</a> = 36 <span class="comment">/* GOT entry containing TLS offset */</span>
    <span id="R_386_TLS_TPOFF32">R_386_TLS_TPOFF32</span>   <a href="#R_386">R_386</a> = 37 <span class="comment">/* GOT entry of -ve static TLS offset */</span>
    <span id="R_386_SIZE32">R_386_SIZE32</span>        <a href="#R_386">R_386</a> = 38
    <span id="R_386_TLS_GOTDESC">R_386_TLS_GOTDESC</span>   <a href="#R_386">R_386</a> = 39
    <span id="R_386_TLS_DESC_CALL">R_386_TLS_DESC_CALL</span> <a href="#R_386">R_386</a> = 40
    <span id="R_386_TLS_DESC">R_386_TLS_DESC</span>      <a href="#R_386">R_386</a> = 41
    <span id="R_386_IRELATIVE">R_386_IRELATIVE</span>     <a href="#R_386">R_386</a> = 42
    <span id="R_386_GOT32X">R_386_GOT32X</span>        <a href="#R_386">R_386</a> = 43
)</pre>


<h3 id="R_386.GoString">func (R_386) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1565">GoString</a>
    <a href="#R_386.GoString">¶</a></h3>
<pre>func (i <a href="#R_386">R_386</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="R_386.String">func (R_386) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1564">String</a>
    <a href="#R_386.String">¶</a></h3>
<pre>func (i <a href="#R_386">R_386</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="R_390">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2075">R_390</a>
    <a href="#R_390">¶</a></h2>
<pre>type R_390 <a href="/builtin/#int">int</a></pre>

Relocation types for s390x processors.

<pre>const (
    <span id="R_390_NONE">R_390_NONE</span>        <a href="#R_390">R_390</a> = 0
    <span id="R_390_8">R_390_8</span>           <a href="#R_390">R_390</a> = 1
    <span id="R_390_12">R_390_12</span>          <a href="#R_390">R_390</a> = 2
    <span id="R_390_16">R_390_16</span>          <a href="#R_390">R_390</a> = 3
    <span id="R_390_32">R_390_32</span>          <a href="#R_390">R_390</a> = 4
    <span id="R_390_PC32">R_390_PC32</span>        <a href="#R_390">R_390</a> = 5
    <span id="R_390_GOT12">R_390_GOT12</span>       <a href="#R_390">R_390</a> = 6
    <span id="R_390_GOT32">R_390_GOT32</span>       <a href="#R_390">R_390</a> = 7
    <span id="R_390_PLT32">R_390_PLT32</span>       <a href="#R_390">R_390</a> = 8
    <span id="R_390_COPY">R_390_COPY</span>        <a href="#R_390">R_390</a> = 9
    <span id="R_390_GLOB_DAT">R_390_GLOB_DAT</span>    <a href="#R_390">R_390</a> = 10
    <span id="R_390_JMP_SLOT">R_390_JMP_SLOT</span>    <a href="#R_390">R_390</a> = 11
    <span id="R_390_RELATIVE">R_390_RELATIVE</span>    <a href="#R_390">R_390</a> = 12
    <span id="R_390_GOTOFF">R_390_GOTOFF</span>      <a href="#R_390">R_390</a> = 13
    <span id="R_390_GOTPC">R_390_GOTPC</span>       <a href="#R_390">R_390</a> = 14
    <span id="R_390_GOT16">R_390_GOT16</span>       <a href="#R_390">R_390</a> = 15
    <span id="R_390_PC16">R_390_PC16</span>        <a href="#R_390">R_390</a> = 16
    <span id="R_390_PC16DBL">R_390_PC16DBL</span>     <a href="#R_390">R_390</a> = 17
    <span id="R_390_PLT16DBL">R_390_PLT16DBL</span>    <a href="#R_390">R_390</a> = 18
    <span id="R_390_PC32DBL">R_390_PC32DBL</span>     <a href="#R_390">R_390</a> = 19
    <span id="R_390_PLT32DBL">R_390_PLT32DBL</span>    <a href="#R_390">R_390</a> = 20
    <span id="R_390_GOTPCDBL">R_390_GOTPCDBL</span>    <a href="#R_390">R_390</a> = 21
    <span id="R_390_64">R_390_64</span>          <a href="#R_390">R_390</a> = 22
    <span id="R_390_PC64">R_390_PC64</span>        <a href="#R_390">R_390</a> = 23
    <span id="R_390_GOT64">R_390_GOT64</span>       <a href="#R_390">R_390</a> = 24
    <span id="R_390_PLT64">R_390_PLT64</span>       <a href="#R_390">R_390</a> = 25
    <span id="R_390_GOTENT">R_390_GOTENT</span>      <a href="#R_390">R_390</a> = 26
    <span id="R_390_GOTOFF16">R_390_GOTOFF16</span>    <a href="#R_390">R_390</a> = 27
    <span id="R_390_GOTOFF64">R_390_GOTOFF64</span>    <a href="#R_390">R_390</a> = 28
    <span id="R_390_GOTPLT12">R_390_GOTPLT12</span>    <a href="#R_390">R_390</a> = 29
    <span id="R_390_GOTPLT16">R_390_GOTPLT16</span>    <a href="#R_390">R_390</a> = 30
    <span id="R_390_GOTPLT32">R_390_GOTPLT32</span>    <a href="#R_390">R_390</a> = 31
    <span id="R_390_GOTPLT64">R_390_GOTPLT64</span>    <a href="#R_390">R_390</a> = 32
    <span id="R_390_GOTPLTENT">R_390_GOTPLTENT</span>   <a href="#R_390">R_390</a> = 33
    <span id="R_390_GOTPLTOFF16">R_390_GOTPLTOFF16</span> <a href="#R_390">R_390</a> = 34
    <span id="R_390_GOTPLTOFF32">R_390_GOTPLTOFF32</span> <a href="#R_390">R_390</a> = 35
    <span id="R_390_GOTPLTOFF64">R_390_GOTPLTOFF64</span> <a href="#R_390">R_390</a> = 36
    <span id="R_390_TLS_LOAD">R_390_TLS_LOAD</span>    <a href="#R_390">R_390</a> = 37
    <span id="R_390_TLS_GDCALL">R_390_TLS_GDCALL</span>  <a href="#R_390">R_390</a> = 38
    <span id="R_390_TLS_LDCALL">R_390_TLS_LDCALL</span>  <a href="#R_390">R_390</a> = 39
    <span id="R_390_TLS_GD32">R_390_TLS_GD32</span>    <a href="#R_390">R_390</a> = 40
    <span id="R_390_TLS_GD64">R_390_TLS_GD64</span>    <a href="#R_390">R_390</a> = 41
    <span id="R_390_TLS_GOTIE12">R_390_TLS_GOTIE12</span> <a href="#R_390">R_390</a> = 42
    <span id="R_390_TLS_GOTIE32">R_390_TLS_GOTIE32</span> <a href="#R_390">R_390</a> = 43
    <span id="R_390_TLS_GOTIE64">R_390_TLS_GOTIE64</span> <a href="#R_390">R_390</a> = 44
    <span id="R_390_TLS_LDM32">R_390_TLS_LDM32</span>   <a href="#R_390">R_390</a> = 45
    <span id="R_390_TLS_LDM64">R_390_TLS_LDM64</span>   <a href="#R_390">R_390</a> = 46
    <span id="R_390_TLS_IE32">R_390_TLS_IE32</span>    <a href="#R_390">R_390</a> = 47
    <span id="R_390_TLS_IE64">R_390_TLS_IE64</span>    <a href="#R_390">R_390</a> = 48
    <span id="R_390_TLS_IEENT">R_390_TLS_IEENT</span>   <a href="#R_390">R_390</a> = 49
    <span id="R_390_TLS_LE32">R_390_TLS_LE32</span>    <a href="#R_390">R_390</a> = 50
    <span id="R_390_TLS_LE64">R_390_TLS_LE64</span>    <a href="#R_390">R_390</a> = 51
    <span id="R_390_TLS_LDO32">R_390_TLS_LDO32</span>   <a href="#R_390">R_390</a> = 52
    <span id="R_390_TLS_LDO64">R_390_TLS_LDO64</span>   <a href="#R_390">R_390</a> = 53
    <span id="R_390_TLS_DTPMOD">R_390_TLS_DTPMOD</span>  <a href="#R_390">R_390</a> = 54
    <span id="R_390_TLS_DTPOFF">R_390_TLS_DTPOFF</span>  <a href="#R_390">R_390</a> = 55
    <span id="R_390_TLS_TPOFF">R_390_TLS_TPOFF</span>   <a href="#R_390">R_390</a> = 56
    <span id="R_390_20">R_390_20</span>          <a href="#R_390">R_390</a> = 57
    <span id="R_390_GOT20">R_390_GOT20</span>       <a href="#R_390">R_390</a> = 58
    <span id="R_390_GOTPLT20">R_390_GOTPLT20</span>    <a href="#R_390">R_390</a> = 59
    <span id="R_390_TLS_GOTIE20">R_390_TLS_GOTIE20</span> <a href="#R_390">R_390</a> = 60
)</pre>


<h3 id="R_390.GoString">func (R_390) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2206">GoString</a>
    <a href="#R_390.GoString">¶</a></h3>
<pre>func (i <a href="#R_390">R_390</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="R_390.String">func (R_390) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2205">String</a>
    <a href="#R_390.String">¶</a></h3>
<pre>func (i <a href="#R_390">R_390</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="R_AARCH64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L832">R_AARCH64</a>
    <a href="#R_AARCH64">¶</a></h2>
<pre>type R_AARCH64 <a href="/builtin/#int">int</a></pre>

Relocation types for AArch64 (aka arm64)

<pre>const (
    <span id="R_AARCH64_NONE">R_AARCH64_NONE</span>                            <a href="#R_AARCH64">R_AARCH64</a> = 0
    <span id="R_AARCH64_P32_ABS32">R_AARCH64_P32_ABS32</span>                       <a href="#R_AARCH64">R_AARCH64</a> = 1
    <span id="R_AARCH64_P32_ABS16">R_AARCH64_P32_ABS16</span>                       <a href="#R_AARCH64">R_AARCH64</a> = 2
    <span id="R_AARCH64_P32_PREL32">R_AARCH64_P32_PREL32</span>                      <a href="#R_AARCH64">R_AARCH64</a> = 3
    <span id="R_AARCH64_P32_PREL16">R_AARCH64_P32_PREL16</span>                      <a href="#R_AARCH64">R_AARCH64</a> = 4
    <span id="R_AARCH64_P32_MOVW_UABS_G0">R_AARCH64_P32_MOVW_UABS_G0</span>                <a href="#R_AARCH64">R_AARCH64</a> = 5
    <span id="R_AARCH64_P32_MOVW_UABS_G0_NC">R_AARCH64_P32_MOVW_UABS_G0_NC</span>             <a href="#R_AARCH64">R_AARCH64</a> = 6
    <span id="R_AARCH64_P32_MOVW_UABS_G1">R_AARCH64_P32_MOVW_UABS_G1</span>                <a href="#R_AARCH64">R_AARCH64</a> = 7
    <span id="R_AARCH64_P32_MOVW_SABS_G0">R_AARCH64_P32_MOVW_SABS_G0</span>                <a href="#R_AARCH64">R_AARCH64</a> = 8
    <span id="R_AARCH64_P32_LD_PREL_LO19">R_AARCH64_P32_LD_PREL_LO19</span>                <a href="#R_AARCH64">R_AARCH64</a> = 9
    <span id="R_AARCH64_P32_ADR_PREL_LO21">R_AARCH64_P32_ADR_PREL_LO21</span>               <a href="#R_AARCH64">R_AARCH64</a> = 10
    <span id="R_AARCH64_P32_ADR_PREL_PG_HI21">R_AARCH64_P32_ADR_PREL_PG_HI21</span>            <a href="#R_AARCH64">R_AARCH64</a> = 11
    <span id="R_AARCH64_P32_ADD_ABS_LO12_NC">R_AARCH64_P32_ADD_ABS_LO12_NC</span>             <a href="#R_AARCH64">R_AARCH64</a> = 12
    <span id="R_AARCH64_P32_LDST8_ABS_LO12_NC">R_AARCH64_P32_LDST8_ABS_LO12_NC</span>           <a href="#R_AARCH64">R_AARCH64</a> = 13
    <span id="R_AARCH64_P32_LDST16_ABS_LO12_NC">R_AARCH64_P32_LDST16_ABS_LO12_NC</span>          <a href="#R_AARCH64">R_AARCH64</a> = 14
    <span id="R_AARCH64_P32_LDST32_ABS_LO12_NC">R_AARCH64_P32_LDST32_ABS_LO12_NC</span>          <a href="#R_AARCH64">R_AARCH64</a> = 15
    <span id="R_AARCH64_P32_LDST64_ABS_LO12_NC">R_AARCH64_P32_LDST64_ABS_LO12_NC</span>          <a href="#R_AARCH64">R_AARCH64</a> = 16
    <span id="R_AARCH64_P32_LDST128_ABS_LO12_NC">R_AARCH64_P32_LDST128_ABS_LO12_NC</span>         <a href="#R_AARCH64">R_AARCH64</a> = 17
    <span id="R_AARCH64_P32_TSTBR14">R_AARCH64_P32_TSTBR14</span>                     <a href="#R_AARCH64">R_AARCH64</a> = 18
    <span id="R_AARCH64_P32_CONDBR19">R_AARCH64_P32_CONDBR19</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 19
    <span id="R_AARCH64_P32_JUMP26">R_AARCH64_P32_JUMP26</span>                      <a href="#R_AARCH64">R_AARCH64</a> = 20
    <span id="R_AARCH64_P32_CALL26">R_AARCH64_P32_CALL26</span>                      <a href="#R_AARCH64">R_AARCH64</a> = 21
    <span id="R_AARCH64_P32_GOT_LD_PREL19">R_AARCH64_P32_GOT_LD_PREL19</span>               <a href="#R_AARCH64">R_AARCH64</a> = 25
    <span id="R_AARCH64_P32_ADR_GOT_PAGE">R_AARCH64_P32_ADR_GOT_PAGE</span>                <a href="#R_AARCH64">R_AARCH64</a> = 26
    <span id="R_AARCH64_P32_LD32_GOT_LO12_NC">R_AARCH64_P32_LD32_GOT_LO12_NC</span>            <a href="#R_AARCH64">R_AARCH64</a> = 27
    <span id="R_AARCH64_P32_TLSGD_ADR_PAGE21">R_AARCH64_P32_TLSGD_ADR_PAGE21</span>            <a href="#R_AARCH64">R_AARCH64</a> = 81
    <span id="R_AARCH64_P32_TLSGD_ADD_LO12_NC">R_AARCH64_P32_TLSGD_ADD_LO12_NC</span>           <a href="#R_AARCH64">R_AARCH64</a> = 82
    <span id="R_AARCH64_P32_TLSIE_ADR_GOTTPREL_PAGE21">R_AARCH64_P32_TLSIE_ADR_GOTTPREL_PAGE21</span>   <a href="#R_AARCH64">R_AARCH64</a> = 103
    <span id="R_AARCH64_P32_TLSIE_LD32_GOTTPREL_LO12_NC">R_AARCH64_P32_TLSIE_LD32_GOTTPREL_LO12_NC</span> <a href="#R_AARCH64">R_AARCH64</a> = 104
    <span id="R_AARCH64_P32_TLSIE_LD_GOTTPREL_PREL19">R_AARCH64_P32_TLSIE_LD_GOTTPREL_PREL19</span>    <a href="#R_AARCH64">R_AARCH64</a> = 105
    <span id="R_AARCH64_P32_TLSLE_MOVW_TPREL_G1">R_AARCH64_P32_TLSLE_MOVW_TPREL_G1</span>         <a href="#R_AARCH64">R_AARCH64</a> = 106
    <span id="R_AARCH64_P32_TLSLE_MOVW_TPREL_G0">R_AARCH64_P32_TLSLE_MOVW_TPREL_G0</span>         <a href="#R_AARCH64">R_AARCH64</a> = 107
    <span id="R_AARCH64_P32_TLSLE_MOVW_TPREL_G0_NC">R_AARCH64_P32_TLSLE_MOVW_TPREL_G0_NC</span>      <a href="#R_AARCH64">R_AARCH64</a> = 108
    <span id="R_AARCH64_P32_TLSLE_ADD_TPREL_HI12">R_AARCH64_P32_TLSLE_ADD_TPREL_HI12</span>        <a href="#R_AARCH64">R_AARCH64</a> = 109
    <span id="R_AARCH64_P32_TLSLE_ADD_TPREL_LO12">R_AARCH64_P32_TLSLE_ADD_TPREL_LO12</span>        <a href="#R_AARCH64">R_AARCH64</a> = 110
    <span id="R_AARCH64_P32_TLSLE_ADD_TPREL_LO12_NC">R_AARCH64_P32_TLSLE_ADD_TPREL_LO12_NC</span>     <a href="#R_AARCH64">R_AARCH64</a> = 111
    <span id="R_AARCH64_P32_TLSDESC_LD_PREL19">R_AARCH64_P32_TLSDESC_LD_PREL19</span>           <a href="#R_AARCH64">R_AARCH64</a> = 122
    <span id="R_AARCH64_P32_TLSDESC_ADR_PREL21">R_AARCH64_P32_TLSDESC_ADR_PREL21</span>          <a href="#R_AARCH64">R_AARCH64</a> = 123
    <span id="R_AARCH64_P32_TLSDESC_ADR_PAGE21">R_AARCH64_P32_TLSDESC_ADR_PAGE21</span>          <a href="#R_AARCH64">R_AARCH64</a> = 124
    <span id="R_AARCH64_P32_TLSDESC_LD32_LO12_NC">R_AARCH64_P32_TLSDESC_LD32_LO12_NC</span>        <a href="#R_AARCH64">R_AARCH64</a> = 125
    <span id="R_AARCH64_P32_TLSDESC_ADD_LO12_NC">R_AARCH64_P32_TLSDESC_ADD_LO12_NC</span>         <a href="#R_AARCH64">R_AARCH64</a> = 126
    <span id="R_AARCH64_P32_TLSDESC_CALL">R_AARCH64_P32_TLSDESC_CALL</span>                <a href="#R_AARCH64">R_AARCH64</a> = 127
    <span id="R_AARCH64_P32_COPY">R_AARCH64_P32_COPY</span>                        <a href="#R_AARCH64">R_AARCH64</a> = 180
    <span id="R_AARCH64_P32_GLOB_DAT">R_AARCH64_P32_GLOB_DAT</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 181
    <span id="R_AARCH64_P32_JUMP_SLOT">R_AARCH64_P32_JUMP_SLOT</span>                   <a href="#R_AARCH64">R_AARCH64</a> = 182
    <span id="R_AARCH64_P32_RELATIVE">R_AARCH64_P32_RELATIVE</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 183
    <span id="R_AARCH64_P32_TLS_DTPMOD">R_AARCH64_P32_TLS_DTPMOD</span>                  <a href="#R_AARCH64">R_AARCH64</a> = 184
    <span id="R_AARCH64_P32_TLS_DTPREL">R_AARCH64_P32_TLS_DTPREL</span>                  <a href="#R_AARCH64">R_AARCH64</a> = 185
    <span id="R_AARCH64_P32_TLS_TPREL">R_AARCH64_P32_TLS_TPREL</span>                   <a href="#R_AARCH64">R_AARCH64</a> = 186
    <span id="R_AARCH64_P32_TLSDESC">R_AARCH64_P32_TLSDESC</span>                     <a href="#R_AARCH64">R_AARCH64</a> = 187
    <span id="R_AARCH64_P32_IRELATIVE">R_AARCH64_P32_IRELATIVE</span>                   <a href="#R_AARCH64">R_AARCH64</a> = 188
    <span id="R_AARCH64_NULL">R_AARCH64_NULL</span>                            <a href="#R_AARCH64">R_AARCH64</a> = 256
    <span id="R_AARCH64_ABS64">R_AARCH64_ABS64</span>                           <a href="#R_AARCH64">R_AARCH64</a> = 257
    <span id="R_AARCH64_ABS32">R_AARCH64_ABS32</span>                           <a href="#R_AARCH64">R_AARCH64</a> = 258
    <span id="R_AARCH64_ABS16">R_AARCH64_ABS16</span>                           <a href="#R_AARCH64">R_AARCH64</a> = 259
    <span id="R_AARCH64_PREL64">R_AARCH64_PREL64</span>                          <a href="#R_AARCH64">R_AARCH64</a> = 260
    <span id="R_AARCH64_PREL32">R_AARCH64_PREL32</span>                          <a href="#R_AARCH64">R_AARCH64</a> = 261
    <span id="R_AARCH64_PREL16">R_AARCH64_PREL16</span>                          <a href="#R_AARCH64">R_AARCH64</a> = 262
    <span id="R_AARCH64_MOVW_UABS_G0">R_AARCH64_MOVW_UABS_G0</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 263
    <span id="R_AARCH64_MOVW_UABS_G0_NC">R_AARCH64_MOVW_UABS_G0_NC</span>                 <a href="#R_AARCH64">R_AARCH64</a> = 264
    <span id="R_AARCH64_MOVW_UABS_G1">R_AARCH64_MOVW_UABS_G1</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 265
    <span id="R_AARCH64_MOVW_UABS_G1_NC">R_AARCH64_MOVW_UABS_G1_NC</span>                 <a href="#R_AARCH64">R_AARCH64</a> = 266
    <span id="R_AARCH64_MOVW_UABS_G2">R_AARCH64_MOVW_UABS_G2</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 267
    <span id="R_AARCH64_MOVW_UABS_G2_NC">R_AARCH64_MOVW_UABS_G2_NC</span>                 <a href="#R_AARCH64">R_AARCH64</a> = 268
    <span id="R_AARCH64_MOVW_UABS_G3">R_AARCH64_MOVW_UABS_G3</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 269
    <span id="R_AARCH64_MOVW_SABS_G0">R_AARCH64_MOVW_SABS_G0</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 270
    <span id="R_AARCH64_MOVW_SABS_G1">R_AARCH64_MOVW_SABS_G1</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 271
    <span id="R_AARCH64_MOVW_SABS_G2">R_AARCH64_MOVW_SABS_G2</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 272
    <span id="R_AARCH64_LD_PREL_LO19">R_AARCH64_LD_PREL_LO19</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 273
    <span id="R_AARCH64_ADR_PREL_LO21">R_AARCH64_ADR_PREL_LO21</span>                   <a href="#R_AARCH64">R_AARCH64</a> = 274
    <span id="R_AARCH64_ADR_PREL_PG_HI21">R_AARCH64_ADR_PREL_PG_HI21</span>                <a href="#R_AARCH64">R_AARCH64</a> = 275
    <span id="R_AARCH64_ADR_PREL_PG_HI21_NC">R_AARCH64_ADR_PREL_PG_HI21_NC</span>             <a href="#R_AARCH64">R_AARCH64</a> = 276
    <span id="R_AARCH64_ADD_ABS_LO12_NC">R_AARCH64_ADD_ABS_LO12_NC</span>                 <a href="#R_AARCH64">R_AARCH64</a> = 277
    <span id="R_AARCH64_LDST8_ABS_LO12_NC">R_AARCH64_LDST8_ABS_LO12_NC</span>               <a href="#R_AARCH64">R_AARCH64</a> = 278
    <span id="R_AARCH64_TSTBR14">R_AARCH64_TSTBR14</span>                         <a href="#R_AARCH64">R_AARCH64</a> = 279
    <span id="R_AARCH64_CONDBR19">R_AARCH64_CONDBR19</span>                        <a href="#R_AARCH64">R_AARCH64</a> = 280
    <span id="R_AARCH64_JUMP26">R_AARCH64_JUMP26</span>                          <a href="#R_AARCH64">R_AARCH64</a> = 282
    <span id="R_AARCH64_CALL26">R_AARCH64_CALL26</span>                          <a href="#R_AARCH64">R_AARCH64</a> = 283
    <span id="R_AARCH64_LDST16_ABS_LO12_NC">R_AARCH64_LDST16_ABS_LO12_NC</span>              <a href="#R_AARCH64">R_AARCH64</a> = 284
    <span id="R_AARCH64_LDST32_ABS_LO12_NC">R_AARCH64_LDST32_ABS_LO12_NC</span>              <a href="#R_AARCH64">R_AARCH64</a> = 285
    <span id="R_AARCH64_LDST64_ABS_LO12_NC">R_AARCH64_LDST64_ABS_LO12_NC</span>              <a href="#R_AARCH64">R_AARCH64</a> = 286
    <span id="R_AARCH64_LDST128_ABS_LO12_NC">R_AARCH64_LDST128_ABS_LO12_NC</span>             <a href="#R_AARCH64">R_AARCH64</a> = 299
    <span id="R_AARCH64_GOT_LD_PREL19">R_AARCH64_GOT_LD_PREL19</span>                   <a href="#R_AARCH64">R_AARCH64</a> = 309
    <span id="R_AARCH64_LD64_GOTOFF_LO15">R_AARCH64_LD64_GOTOFF_LO15</span>                <a href="#R_AARCH64">R_AARCH64</a> = 310
    <span id="R_AARCH64_ADR_GOT_PAGE">R_AARCH64_ADR_GOT_PAGE</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 311
    <span id="R_AARCH64_LD64_GOT_LO12_NC">R_AARCH64_LD64_GOT_LO12_NC</span>                <a href="#R_AARCH64">R_AARCH64</a> = 312
    <span id="R_AARCH64_LD64_GOTPAGE_LO15">R_AARCH64_LD64_GOTPAGE_LO15</span>               <a href="#R_AARCH64">R_AARCH64</a> = 313
    <span id="R_AARCH64_TLSGD_ADR_PREL21">R_AARCH64_TLSGD_ADR_PREL21</span>                <a href="#R_AARCH64">R_AARCH64</a> = 512
    <span id="R_AARCH64_TLSGD_ADR_PAGE21">R_AARCH64_TLSGD_ADR_PAGE21</span>                <a href="#R_AARCH64">R_AARCH64</a> = 513
    <span id="R_AARCH64_TLSGD_ADD_LO12_NC">R_AARCH64_TLSGD_ADD_LO12_NC</span>               <a href="#R_AARCH64">R_AARCH64</a> = 514
    <span id="R_AARCH64_TLSGD_MOVW_G1">R_AARCH64_TLSGD_MOVW_G1</span>                   <a href="#R_AARCH64">R_AARCH64</a> = 515
    <span id="R_AARCH64_TLSGD_MOVW_G0_NC">R_AARCH64_TLSGD_MOVW_G0_NC</span>                <a href="#R_AARCH64">R_AARCH64</a> = 516
    <span id="R_AARCH64_TLSLD_ADR_PREL21">R_AARCH64_TLSLD_ADR_PREL21</span>                <a href="#R_AARCH64">R_AARCH64</a> = 517
    <span id="R_AARCH64_TLSLD_ADR_PAGE21">R_AARCH64_TLSLD_ADR_PAGE21</span>                <a href="#R_AARCH64">R_AARCH64</a> = 518
    <span id="R_AARCH64_TLSIE_MOVW_GOTTPREL_G1">R_AARCH64_TLSIE_MOVW_GOTTPREL_G1</span>          <a href="#R_AARCH64">R_AARCH64</a> = 539
    <span id="R_AARCH64_TLSIE_MOVW_GOTTPREL_G0_NC">R_AARCH64_TLSIE_MOVW_GOTTPREL_G0_NC</span>       <a href="#R_AARCH64">R_AARCH64</a> = 540
    <span id="R_AARCH64_TLSIE_ADR_GOTTPREL_PAGE21">R_AARCH64_TLSIE_ADR_GOTTPREL_PAGE21</span>       <a href="#R_AARCH64">R_AARCH64</a> = 541
    <span id="R_AARCH64_TLSIE_LD64_GOTTPREL_LO12_NC">R_AARCH64_TLSIE_LD64_GOTTPREL_LO12_NC</span>     <a href="#R_AARCH64">R_AARCH64</a> = 542
    <span id="R_AARCH64_TLSIE_LD_GOTTPREL_PREL19">R_AARCH64_TLSIE_LD_GOTTPREL_PREL19</span>        <a href="#R_AARCH64">R_AARCH64</a> = 543
    <span id="R_AARCH64_TLSLE_MOVW_TPREL_G2">R_AARCH64_TLSLE_MOVW_TPREL_G2</span>             <a href="#R_AARCH64">R_AARCH64</a> = 544
    <span id="R_AARCH64_TLSLE_MOVW_TPREL_G1">R_AARCH64_TLSLE_MOVW_TPREL_G1</span>             <a href="#R_AARCH64">R_AARCH64</a> = 545
    <span id="R_AARCH64_TLSLE_MOVW_TPREL_G1_NC">R_AARCH64_TLSLE_MOVW_TPREL_G1_NC</span>          <a href="#R_AARCH64">R_AARCH64</a> = 546
    <span id="R_AARCH64_TLSLE_MOVW_TPREL_G0">R_AARCH64_TLSLE_MOVW_TPREL_G0</span>             <a href="#R_AARCH64">R_AARCH64</a> = 547
    <span id="R_AARCH64_TLSLE_MOVW_TPREL_G0_NC">R_AARCH64_TLSLE_MOVW_TPREL_G0_NC</span>          <a href="#R_AARCH64">R_AARCH64</a> = 548
    <span id="R_AARCH64_TLSLE_ADD_TPREL_HI12">R_AARCH64_TLSLE_ADD_TPREL_HI12</span>            <a href="#R_AARCH64">R_AARCH64</a> = 549
    <span id="R_AARCH64_TLSLE_ADD_TPREL_LO12">R_AARCH64_TLSLE_ADD_TPREL_LO12</span>            <a href="#R_AARCH64">R_AARCH64</a> = 550
    <span id="R_AARCH64_TLSLE_ADD_TPREL_LO12_NC">R_AARCH64_TLSLE_ADD_TPREL_LO12_NC</span>         <a href="#R_AARCH64">R_AARCH64</a> = 551
    <span id="R_AARCH64_TLSDESC_LD_PREL19">R_AARCH64_TLSDESC_LD_PREL19</span>               <a href="#R_AARCH64">R_AARCH64</a> = 560
    <span id="R_AARCH64_TLSDESC_ADR_PREL21">R_AARCH64_TLSDESC_ADR_PREL21</span>              <a href="#R_AARCH64">R_AARCH64</a> = 561
    <span id="R_AARCH64_TLSDESC_ADR_PAGE21">R_AARCH64_TLSDESC_ADR_PAGE21</span>              <a href="#R_AARCH64">R_AARCH64</a> = 562
    <span id="R_AARCH64_TLSDESC_LD64_LO12_NC">R_AARCH64_TLSDESC_LD64_LO12_NC</span>            <a href="#R_AARCH64">R_AARCH64</a> = 563
    <span id="R_AARCH64_TLSDESC_ADD_LO12_NC">R_AARCH64_TLSDESC_ADD_LO12_NC</span>             <a href="#R_AARCH64">R_AARCH64</a> = 564
    <span id="R_AARCH64_TLSDESC_OFF_G1">R_AARCH64_TLSDESC_OFF_G1</span>                  <a href="#R_AARCH64">R_AARCH64</a> = 565
    <span id="R_AARCH64_TLSDESC_OFF_G0_NC">R_AARCH64_TLSDESC_OFF_G0_NC</span>               <a href="#R_AARCH64">R_AARCH64</a> = 566
    <span id="R_AARCH64_TLSDESC_LDR">R_AARCH64_TLSDESC_LDR</span>                     <a href="#R_AARCH64">R_AARCH64</a> = 567
    <span id="R_AARCH64_TLSDESC_ADD">R_AARCH64_TLSDESC_ADD</span>                     <a href="#R_AARCH64">R_AARCH64</a> = 568
    <span id="R_AARCH64_TLSDESC_CALL">R_AARCH64_TLSDESC_CALL</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 569
    <span id="R_AARCH64_TLSLE_LDST128_TPREL_LO12">R_AARCH64_TLSLE_LDST128_TPREL_LO12</span>        <a href="#R_AARCH64">R_AARCH64</a> = 570
    <span id="R_AARCH64_TLSLE_LDST128_TPREL_LO12_NC">R_AARCH64_TLSLE_LDST128_TPREL_LO12_NC</span>     <a href="#R_AARCH64">R_AARCH64</a> = 571
    <span id="R_AARCH64_TLSLD_LDST128_DTPREL_LO12">R_AARCH64_TLSLD_LDST128_DTPREL_LO12</span>       <a href="#R_AARCH64">R_AARCH64</a> = 572
    <span id="R_AARCH64_TLSLD_LDST128_DTPREL_LO12_NC">R_AARCH64_TLSLD_LDST128_DTPREL_LO12_NC</span>    <a href="#R_AARCH64">R_AARCH64</a> = 573
    <span id="R_AARCH64_COPY">R_AARCH64_COPY</span>                            <a href="#R_AARCH64">R_AARCH64</a> = 1024
    <span id="R_AARCH64_GLOB_DAT">R_AARCH64_GLOB_DAT</span>                        <a href="#R_AARCH64">R_AARCH64</a> = 1025
    <span id="R_AARCH64_JUMP_SLOT">R_AARCH64_JUMP_SLOT</span>                       <a href="#R_AARCH64">R_AARCH64</a> = 1026
    <span id="R_AARCH64_RELATIVE">R_AARCH64_RELATIVE</span>                        <a href="#R_AARCH64">R_AARCH64</a> = 1027
    <span id="R_AARCH64_TLS_DTPMOD64">R_AARCH64_TLS_DTPMOD64</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 1028
    <span id="R_AARCH64_TLS_DTPREL64">R_AARCH64_TLS_DTPREL64</span>                    <a href="#R_AARCH64">R_AARCH64</a> = 1029
    <span id="R_AARCH64_TLS_TPREL64">R_AARCH64_TLS_TPREL64</span>                     <a href="#R_AARCH64">R_AARCH64</a> = 1030
    <span id="R_AARCH64_TLSDESC">R_AARCH64_TLSDESC</span>                         <a href="#R_AARCH64">R_AARCH64</a> = 1031
    <span id="R_AARCH64_IRELATIVE">R_AARCH64_IRELATIVE</span>                       <a href="#R_AARCH64">R_AARCH64</a> = 1032
)</pre>


<h3 id="R_AARCH64.GoString">func (R_AARCH64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1101">GoString</a>
    <a href="#R_AARCH64.GoString">¶</a></h3>
<pre>func (i <a href="#R_AARCH64">R_AARCH64</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="R_AARCH64.String">func (R_AARCH64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1100">String</a>
    <a href="#R_AARCH64.String">¶</a></h3>
<pre>func (i <a href="#R_AARCH64">R_AARCH64</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="R_ALPHA">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1104">R_ALPHA</a>
    <a href="#R_ALPHA">¶</a></h2>
<pre>type R_ALPHA <a href="/builtin/#int">int</a></pre>

Relocation types for Alpha.

<pre>const (
    <span id="R_ALPHA_NONE">R_ALPHA_NONE</span>           <a href="#R_ALPHA">R_ALPHA</a> = 0  <span class="comment">/* No reloc */</span>
    <span id="R_ALPHA_REFLONG">R_ALPHA_REFLONG</span>        <a href="#R_ALPHA">R_ALPHA</a> = 1  <span class="comment">/* Direct 32 bit */</span>
    <span id="R_ALPHA_REFQUAD">R_ALPHA_REFQUAD</span>        <a href="#R_ALPHA">R_ALPHA</a> = 2  <span class="comment">/* Direct 64 bit */</span>
    <span id="R_ALPHA_GPREL32">R_ALPHA_GPREL32</span>        <a href="#R_ALPHA">R_ALPHA</a> = 3  <span class="comment">/* GP relative 32 bit */</span>
    <span id="R_ALPHA_LITERAL">R_ALPHA_LITERAL</span>        <a href="#R_ALPHA">R_ALPHA</a> = 4  <span class="comment">/* GP relative 16 bit w/optimization */</span>
    <span id="R_ALPHA_LITUSE">R_ALPHA_LITUSE</span>         <a href="#R_ALPHA">R_ALPHA</a> = 5  <span class="comment">/* Optimization hint for LITERAL */</span>
    <span id="R_ALPHA_GPDISP">R_ALPHA_GPDISP</span>         <a href="#R_ALPHA">R_ALPHA</a> = 6  <span class="comment">/* Add displacement to GP */</span>
    <span id="R_ALPHA_BRADDR">R_ALPHA_BRADDR</span>         <a href="#R_ALPHA">R_ALPHA</a> = 7  <span class="comment">/* PC+4 relative 23 bit shifted */</span>
    <span id="R_ALPHA_HINT">R_ALPHA_HINT</span>           <a href="#R_ALPHA">R_ALPHA</a> = 8  <span class="comment">/* PC+4 relative 16 bit shifted */</span>
    <span id="R_ALPHA_SREL16">R_ALPHA_SREL16</span>         <a href="#R_ALPHA">R_ALPHA</a> = 9  <span class="comment">/* PC relative 16 bit */</span>
    <span id="R_ALPHA_SREL32">R_ALPHA_SREL32</span>         <a href="#R_ALPHA">R_ALPHA</a> = 10 <span class="comment">/* PC relative 32 bit */</span>
    <span id="R_ALPHA_SREL64">R_ALPHA_SREL64</span>         <a href="#R_ALPHA">R_ALPHA</a> = 11 <span class="comment">/* PC relative 64 bit */</span>
    <span id="R_ALPHA_OP_PUSH">R_ALPHA_OP_PUSH</span>        <a href="#R_ALPHA">R_ALPHA</a> = 12 <span class="comment">/* OP stack push */</span>
    <span id="R_ALPHA_OP_STORE">R_ALPHA_OP_STORE</span>       <a href="#R_ALPHA">R_ALPHA</a> = 13 <span class="comment">/* OP stack pop and store */</span>
    <span id="R_ALPHA_OP_PSUB">R_ALPHA_OP_PSUB</span>        <a href="#R_ALPHA">R_ALPHA</a> = 14 <span class="comment">/* OP stack subtract */</span>
    <span id="R_ALPHA_OP_PRSHIFT">R_ALPHA_OP_PRSHIFT</span>     <a href="#R_ALPHA">R_ALPHA</a> = 15 <span class="comment">/* OP stack right shift */</span>
    <span id="R_ALPHA_GPVALUE">R_ALPHA_GPVALUE</span>        <a href="#R_ALPHA">R_ALPHA</a> = 16
    <span id="R_ALPHA_GPRELHIGH">R_ALPHA_GPRELHIGH</span>      <a href="#R_ALPHA">R_ALPHA</a> = 17
    <span id="R_ALPHA_GPRELLOW">R_ALPHA_GPRELLOW</span>       <a href="#R_ALPHA">R_ALPHA</a> = 18
    <span id="R_ALPHA_IMMED_GP_16">R_ALPHA_IMMED_GP_16</span>    <a href="#R_ALPHA">R_ALPHA</a> = 19
    <span id="R_ALPHA_IMMED_GP_HI32">R_ALPHA_IMMED_GP_HI32</span>  <a href="#R_ALPHA">R_ALPHA</a> = 20
    <span id="R_ALPHA_IMMED_SCN_HI32">R_ALPHA_IMMED_SCN_HI32</span> <a href="#R_ALPHA">R_ALPHA</a> = 21
    <span id="R_ALPHA_IMMED_BR_HI32">R_ALPHA_IMMED_BR_HI32</span>  <a href="#R_ALPHA">R_ALPHA</a> = 22
    <span id="R_ALPHA_IMMED_LO32">R_ALPHA_IMMED_LO32</span>     <a href="#R_ALPHA">R_ALPHA</a> = 23
    <span id="R_ALPHA_COPY">R_ALPHA_COPY</span>           <a href="#R_ALPHA">R_ALPHA</a> = 24 <span class="comment">/* Copy symbol at runtime */</span>
    <span id="R_ALPHA_GLOB_DAT">R_ALPHA_GLOB_DAT</span>       <a href="#R_ALPHA">R_ALPHA</a> = 25 <span class="comment">/* Create GOT entry */</span>
    <span id="R_ALPHA_JMP_SLOT">R_ALPHA_JMP_SLOT</span>       <a href="#R_ALPHA">R_ALPHA</a> = 26 <span class="comment">/* Create PLT entry */</span>
    <span id="R_ALPHA_RELATIVE">R_ALPHA_RELATIVE</span>       <a href="#R_ALPHA">R_ALPHA</a> = 27 <span class="comment">/* Adjust by program base */</span>
)</pre>


<h3 id="R_ALPHA.GoString">func (R_ALPHA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1169">GoString</a>
    <a href="#R_ALPHA.GoString">¶</a></h3>
<pre>func (i <a href="#R_ALPHA">R_ALPHA</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="R_ALPHA.String">func (R_ALPHA) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1168">String</a>
    <a href="#R_ALPHA.String">¶</a></h3>
<pre>func (i <a href="#R_ALPHA">R_ALPHA</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="R_ARM">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1172">R_ARM</a>
    <a href="#R_ARM">¶</a></h2>
<pre>type R_ARM <a href="/builtin/#int">int</a></pre>

Relocation types for ARM.

<pre>const (
    <span id="R_ARM_NONE">R_ARM_NONE</span>               <a href="#R_ARM">R_ARM</a> = 0 <span class="comment">/* No relocation. */</span>
    <span id="R_ARM_PC24">R_ARM_PC24</span>               <a href="#R_ARM">R_ARM</a> = 1
    <span id="R_ARM_ABS32">R_ARM_ABS32</span>              <a href="#R_ARM">R_ARM</a> = 2
    <span id="R_ARM_REL32">R_ARM_REL32</span>              <a href="#R_ARM">R_ARM</a> = 3
    <span id="R_ARM_PC13">R_ARM_PC13</span>               <a href="#R_ARM">R_ARM</a> = 4
    <span id="R_ARM_ABS16">R_ARM_ABS16</span>              <a href="#R_ARM">R_ARM</a> = 5
    <span id="R_ARM_ABS12">R_ARM_ABS12</span>              <a href="#R_ARM">R_ARM</a> = 6
    <span id="R_ARM_THM_ABS5">R_ARM_THM_ABS5</span>           <a href="#R_ARM">R_ARM</a> = 7
    <span id="R_ARM_ABS8">R_ARM_ABS8</span>               <a href="#R_ARM">R_ARM</a> = 8
    <span id="R_ARM_SBREL32">R_ARM_SBREL32</span>            <a href="#R_ARM">R_ARM</a> = 9
    <span id="R_ARM_THM_PC22">R_ARM_THM_PC22</span>           <a href="#R_ARM">R_ARM</a> = 10
    <span id="R_ARM_THM_PC8">R_ARM_THM_PC8</span>            <a href="#R_ARM">R_ARM</a> = 11
    <span id="R_ARM_AMP_VCALL9">R_ARM_AMP_VCALL9</span>         <a href="#R_ARM">R_ARM</a> = 12
    <span id="R_ARM_SWI24">R_ARM_SWI24</span>              <a href="#R_ARM">R_ARM</a> = 13
    <span id="R_ARM_THM_SWI8">R_ARM_THM_SWI8</span>           <a href="#R_ARM">R_ARM</a> = 14
    <span id="R_ARM_XPC25">R_ARM_XPC25</span>              <a href="#R_ARM">R_ARM</a> = 15
    <span id="R_ARM_THM_XPC22">R_ARM_THM_XPC22</span>          <a href="#R_ARM">R_ARM</a> = 16
    <span id="R_ARM_TLS_DTPMOD32">R_ARM_TLS_DTPMOD32</span>       <a href="#R_ARM">R_ARM</a> = 17
    <span id="R_ARM_TLS_DTPOFF32">R_ARM_TLS_DTPOFF32</span>       <a href="#R_ARM">R_ARM</a> = 18
    <span id="R_ARM_TLS_TPOFF32">R_ARM_TLS_TPOFF32</span>        <a href="#R_ARM">R_ARM</a> = 19
    <span id="R_ARM_COPY">R_ARM_COPY</span>               <a href="#R_ARM">R_ARM</a> = 20 <span class="comment">/* Copy data from shared object. */</span>
    <span id="R_ARM_GLOB_DAT">R_ARM_GLOB_DAT</span>           <a href="#R_ARM">R_ARM</a> = 21 <span class="comment">/* Set GOT entry to data address. */</span>
    <span id="R_ARM_JUMP_SLOT">R_ARM_JUMP_SLOT</span>          <a href="#R_ARM">R_ARM</a> = 22 <span class="comment">/* Set GOT entry to code address. */</span>
    <span id="R_ARM_RELATIVE">R_ARM_RELATIVE</span>           <a href="#R_ARM">R_ARM</a> = 23 <span class="comment">/* Add load address of shared object. */</span>
    <span id="R_ARM_GOTOFF">R_ARM_GOTOFF</span>             <a href="#R_ARM">R_ARM</a> = 24 <span class="comment">/* Add GOT-relative symbol address. */</span>
    <span id="R_ARM_GOTPC">R_ARM_GOTPC</span>              <a href="#R_ARM">R_ARM</a> = 25 <span class="comment">/* Add PC-relative GOT table address. */</span>
    <span id="R_ARM_GOT32">R_ARM_GOT32</span>              <a href="#R_ARM">R_ARM</a> = 26 <span class="comment">/* Add PC-relative GOT offset. */</span>
    <span id="R_ARM_PLT32">R_ARM_PLT32</span>              <a href="#R_ARM">R_ARM</a> = 27 <span class="comment">/* Add PC-relative PLT offset. */</span>
    <span id="R_ARM_CALL">R_ARM_CALL</span>               <a href="#R_ARM">R_ARM</a> = 28
    <span id="R_ARM_JUMP24">R_ARM_JUMP24</span>             <a href="#R_ARM">R_ARM</a> = 29
    <span id="R_ARM_THM_JUMP24">R_ARM_THM_JUMP24</span>         <a href="#R_ARM">R_ARM</a> = 30
    <span id="R_ARM_BASE_ABS">R_ARM_BASE_ABS</span>           <a href="#R_ARM">R_ARM</a> = 31
    <span id="R_ARM_ALU_PCREL_7_0">R_ARM_ALU_PCREL_7_0</span>      <a href="#R_ARM">R_ARM</a> = 32
    <span id="R_ARM_ALU_PCREL_15_8">R_ARM_ALU_PCREL_15_8</span>     <a href="#R_ARM">R_ARM</a> = 33
    <span id="R_ARM_ALU_PCREL_23_15">R_ARM_ALU_PCREL_23_15</span>    <a href="#R_ARM">R_ARM</a> = 34
    <span id="R_ARM_LDR_SBREL_11_10_NC">R_ARM_LDR_SBREL_11_10_NC</span> <a href="#R_ARM">R_ARM</a> = 35
    <span id="R_ARM_ALU_SBREL_19_12_NC">R_ARM_ALU_SBREL_19_12_NC</span> <a href="#R_ARM">R_ARM</a> = 36
    <span id="R_ARM_ALU_SBREL_27_20_CK">R_ARM_ALU_SBREL_27_20_CK</span> <a href="#R_ARM">R_ARM</a> = 37
    <span id="R_ARM_TARGET1">R_ARM_TARGET1</span>            <a href="#R_ARM">R_ARM</a> = 38
    <span id="R_ARM_SBREL31">R_ARM_SBREL31</span>            <a href="#R_ARM">R_ARM</a> = 39
    <span id="R_ARM_V4BX">R_ARM_V4BX</span>               <a href="#R_ARM">R_ARM</a> = 40
    <span id="R_ARM_TARGET2">R_ARM_TARGET2</span>            <a href="#R_ARM">R_ARM</a> = 41
    <span id="R_ARM_PREL31">R_ARM_PREL31</span>             <a href="#R_ARM">R_ARM</a> = 42
    <span id="R_ARM_MOVW_ABS_NC">R_ARM_MOVW_ABS_NC</span>        <a href="#R_ARM">R_ARM</a> = 43
    <span id="R_ARM_MOVT_ABS">R_ARM_MOVT_ABS</span>           <a href="#R_ARM">R_ARM</a> = 44
    <span id="R_ARM_MOVW_PREL_NC">R_ARM_MOVW_PREL_NC</span>       <a href="#R_ARM">R_ARM</a> = 45
    <span id="R_ARM_MOVT_PREL">R_ARM_MOVT_PREL</span>          <a href="#R_ARM">R_ARM</a> = 46
    <span id="R_ARM_THM_MOVW_ABS_NC">R_ARM_THM_MOVW_ABS_NC</span>    <a href="#R_ARM">R_ARM</a> = 47
    <span id="R_ARM_THM_MOVT_ABS">R_ARM_THM_MOVT_ABS</span>       <a href="#R_ARM">R_ARM</a> = 48
    <span id="R_ARM_THM_MOVW_PREL_NC">R_ARM_THM_MOVW_PREL_NC</span>   <a href="#R_ARM">R_ARM</a> = 49
    <span id="R_ARM_THM_MOVT_PREL">R_ARM_THM_MOVT_PREL</span>      <a href="#R_ARM">R_ARM</a> = 50
    <span id="R_ARM_THM_JUMP19">R_ARM_THM_JUMP19</span>         <a href="#R_ARM">R_ARM</a> = 51
    <span id="R_ARM_THM_JUMP6">R_ARM_THM_JUMP6</span>          <a href="#R_ARM">R_ARM</a> = 52
    <span id="R_ARM_THM_ALU_PREL_11_0">R_ARM_THM_ALU_PREL_11_0</span>  <a href="#R_ARM">R_ARM</a> = 53
    <span id="R_ARM_THM_PC12">R_ARM_THM_PC12</span>           <a href="#R_ARM">R_ARM</a> = 54
    <span id="R_ARM_ABS32_NOI">R_ARM_ABS32_NOI</span>          <a href="#R_ARM">R_ARM</a> = 55
    <span id="R_ARM_REL32_NOI">R_ARM_REL32_NOI</span>          <a href="#R_ARM">R_ARM</a> = 56
    <span id="R_ARM_ALU_PC_G0_NC">R_ARM_ALU_PC_G0_NC</span>       <a href="#R_ARM">R_ARM</a> = 57
    <span id="R_ARM_ALU_PC_G0">R_ARM_ALU_PC_G0</span>          <a href="#R_ARM">R_ARM</a> = 58
    <span id="R_ARM_ALU_PC_G1_NC">R_ARM_ALU_PC_G1_NC</span>       <a href="#R_ARM">R_ARM</a> = 59
    <span id="R_ARM_ALU_PC_G1">R_ARM_ALU_PC_G1</span>          <a href="#R_ARM">R_ARM</a> = 60
    <span id="R_ARM_ALU_PC_G2">R_ARM_ALU_PC_G2</span>          <a href="#R_ARM">R_ARM</a> = 61
    <span id="R_ARM_LDR_PC_G1">R_ARM_LDR_PC_G1</span>          <a href="#R_ARM">R_ARM</a> = 62
    <span id="R_ARM_LDR_PC_G2">R_ARM_LDR_PC_G2</span>          <a href="#R_ARM">R_ARM</a> = 63
    <span id="R_ARM_LDRS_PC_G0">R_ARM_LDRS_PC_G0</span>         <a href="#R_ARM">R_ARM</a> = 64
    <span id="R_ARM_LDRS_PC_G1">R_ARM_LDRS_PC_G1</span>         <a href="#R_ARM">R_ARM</a> = 65
    <span id="R_ARM_LDRS_PC_G2">R_ARM_LDRS_PC_G2</span>         <a href="#R_ARM">R_ARM</a> = 66
    <span id="R_ARM_LDC_PC_G0">R_ARM_LDC_PC_G0</span>          <a href="#R_ARM">R_ARM</a> = 67
    <span id="R_ARM_LDC_PC_G1">R_ARM_LDC_PC_G1</span>          <a href="#R_ARM">R_ARM</a> = 68
    <span id="R_ARM_LDC_PC_G2">R_ARM_LDC_PC_G2</span>          <a href="#R_ARM">R_ARM</a> = 69
    <span id="R_ARM_ALU_SB_G0_NC">R_ARM_ALU_SB_G0_NC</span>       <a href="#R_ARM">R_ARM</a> = 70
    <span id="R_ARM_ALU_SB_G0">R_ARM_ALU_SB_G0</span>          <a href="#R_ARM">R_ARM</a> = 71
    <span id="R_ARM_ALU_SB_G1_NC">R_ARM_ALU_SB_G1_NC</span>       <a href="#R_ARM">R_ARM</a> = 72
    <span id="R_ARM_ALU_SB_G1">R_ARM_ALU_SB_G1</span>          <a href="#R_ARM">R_ARM</a> = 73
    <span id="R_ARM_ALU_SB_G2">R_ARM_ALU_SB_G2</span>          <a href="#R_ARM">R_ARM</a> = 74
    <span id="R_ARM_LDR_SB_G0">R_ARM_LDR_SB_G0</span>          <a href="#R_ARM">R_ARM</a> = 75
    <span id="R_ARM_LDR_SB_G1">R_ARM_LDR_SB_G1</span>          <a href="#R_ARM">R_ARM</a> = 76
    <span id="R_ARM_LDR_SB_G2">R_ARM_LDR_SB_G2</span>          <a href="#R_ARM">R_ARM</a> = 77
    <span id="R_ARM_LDRS_SB_G0">R_ARM_LDRS_SB_G0</span>         <a href="#R_ARM">R_ARM</a> = 78
    <span id="R_ARM_LDRS_SB_G1">R_ARM_LDRS_SB_G1</span>         <a href="#R_ARM">R_ARM</a> = 79
    <span id="R_ARM_LDRS_SB_G2">R_ARM_LDRS_SB_G2</span>         <a href="#R_ARM">R_ARM</a> = 80
    <span id="R_ARM_LDC_SB_G0">R_ARM_LDC_SB_G0</span>          <a href="#R_ARM">R_ARM</a> = 81
    <span id="R_ARM_LDC_SB_G1">R_ARM_LDC_SB_G1</span>          <a href="#R_ARM">R_ARM</a> = 82
    <span id="R_ARM_LDC_SB_G2">R_ARM_LDC_SB_G2</span>          <a href="#R_ARM">R_ARM</a> = 83
    <span id="R_ARM_MOVW_BREL_NC">R_ARM_MOVW_BREL_NC</span>       <a href="#R_ARM">R_ARM</a> = 84
    <span id="R_ARM_MOVT_BREL">R_ARM_MOVT_BREL</span>          <a href="#R_ARM">R_ARM</a> = 85
    <span id="R_ARM_MOVW_BREL">R_ARM_MOVW_BREL</span>          <a href="#R_ARM">R_ARM</a> = 86
    <span id="R_ARM_THM_MOVW_BREL_NC">R_ARM_THM_MOVW_BREL_NC</span>   <a href="#R_ARM">R_ARM</a> = 87
    <span id="R_ARM_THM_MOVT_BREL">R_ARM_THM_MOVT_BREL</span>      <a href="#R_ARM">R_ARM</a> = 88
    <span id="R_ARM_THM_MOVW_BREL">R_ARM_THM_MOVW_BREL</span>      <a href="#R_ARM">R_ARM</a> = 89
    <span id="R_ARM_TLS_GOTDESC">R_ARM_TLS_GOTDESC</span>        <a href="#R_ARM">R_ARM</a> = 90
    <span id="R_ARM_TLS_CALL">R_ARM_TLS_CALL</span>           <a href="#R_ARM">R_ARM</a> = 91
    <span id="R_ARM_TLS_DESCSEQ">R_ARM_TLS_DESCSEQ</span>        <a href="#R_ARM">R_ARM</a> = 92
    <span id="R_ARM_THM_TLS_CALL">R_ARM_THM_TLS_CALL</span>       <a href="#R_ARM">R_ARM</a> = 93
    <span id="R_ARM_PLT32_ABS">R_ARM_PLT32_ABS</span>          <a href="#R_ARM">R_ARM</a> = 94
    <span id="R_ARM_GOT_ABS">R_ARM_GOT_ABS</span>            <a href="#R_ARM">R_ARM</a> = 95
    <span id="R_ARM_GOT_PREL">R_ARM_GOT_PREL</span>           <a href="#R_ARM">R_ARM</a> = 96
    <span id="R_ARM_GOT_BREL12">R_ARM_GOT_BREL12</span>         <a href="#R_ARM">R_ARM</a> = 97
    <span id="R_ARM_GOTOFF12">R_ARM_GOTOFF12</span>           <a href="#R_ARM">R_ARM</a> = 98
    <span id="R_ARM_GOTRELAX">R_ARM_GOTRELAX</span>           <a href="#R_ARM">R_ARM</a> = 99
    <span id="R_ARM_GNU_VTENTRY">R_ARM_GNU_VTENTRY</span>        <a href="#R_ARM">R_ARM</a> = 100
    <span id="R_ARM_GNU_VTINHERIT">R_ARM_GNU_VTINHERIT</span>      <a href="#R_ARM">R_ARM</a> = 101
    <span id="R_ARM_THM_JUMP11">R_ARM_THM_JUMP11</span>         <a href="#R_ARM">R_ARM</a> = 102
    <span id="R_ARM_THM_JUMP8">R_ARM_THM_JUMP8</span>          <a href="#R_ARM">R_ARM</a> = 103
    <span id="R_ARM_TLS_GD32">R_ARM_TLS_GD32</span>           <a href="#R_ARM">R_ARM</a> = 104
    <span id="R_ARM_TLS_LDM32">R_ARM_TLS_LDM32</span>          <a href="#R_ARM">R_ARM</a> = 105
    <span id="R_ARM_TLS_LDO32">R_ARM_TLS_LDO32</span>          <a href="#R_ARM">R_ARM</a> = 106
    <span id="R_ARM_TLS_IE32">R_ARM_TLS_IE32</span>           <a href="#R_ARM">R_ARM</a> = 107
    <span id="R_ARM_TLS_LE32">R_ARM_TLS_LE32</span>           <a href="#R_ARM">R_ARM</a> = 108
    <span id="R_ARM_TLS_LDO12">R_ARM_TLS_LDO12</span>          <a href="#R_ARM">R_ARM</a> = 109
    <span id="R_ARM_TLS_LE12">R_ARM_TLS_LE12</span>           <a href="#R_ARM">R_ARM</a> = 110
    <span id="R_ARM_TLS_IE12GP">R_ARM_TLS_IE12GP</span>         <a href="#R_ARM">R_ARM</a> = 111
    <span id="R_ARM_PRIVATE_0">R_ARM_PRIVATE_0</span>          <a href="#R_ARM">R_ARM</a> = 112
    <span id="R_ARM_PRIVATE_1">R_ARM_PRIVATE_1</span>          <a href="#R_ARM">R_ARM</a> = 113
    <span id="R_ARM_PRIVATE_2">R_ARM_PRIVATE_2</span>          <a href="#R_ARM">R_ARM</a> = 114
    <span id="R_ARM_PRIVATE_3">R_ARM_PRIVATE_3</span>          <a href="#R_ARM">R_ARM</a> = 115
    <span id="R_ARM_PRIVATE_4">R_ARM_PRIVATE_4</span>          <a href="#R_ARM">R_ARM</a> = 116
    <span id="R_ARM_PRIVATE_5">R_ARM_PRIVATE_5</span>          <a href="#R_ARM">R_ARM</a> = 117
    <span id="R_ARM_PRIVATE_6">R_ARM_PRIVATE_6</span>          <a href="#R_ARM">R_ARM</a> = 118
    <span id="R_ARM_PRIVATE_7">R_ARM_PRIVATE_7</span>          <a href="#R_ARM">R_ARM</a> = 119
    <span id="R_ARM_PRIVATE_8">R_ARM_PRIVATE_8</span>          <a href="#R_ARM">R_ARM</a> = 120
    <span id="R_ARM_PRIVATE_9">R_ARM_PRIVATE_9</span>          <a href="#R_ARM">R_ARM</a> = 121
    <span id="R_ARM_PRIVATE_10">R_ARM_PRIVATE_10</span>         <a href="#R_ARM">R_ARM</a> = 122
    <span id="R_ARM_PRIVATE_11">R_ARM_PRIVATE_11</span>         <a href="#R_ARM">R_ARM</a> = 123
    <span id="R_ARM_PRIVATE_12">R_ARM_PRIVATE_12</span>         <a href="#R_ARM">R_ARM</a> = 124
    <span id="R_ARM_PRIVATE_13">R_ARM_PRIVATE_13</span>         <a href="#R_ARM">R_ARM</a> = 125
    <span id="R_ARM_PRIVATE_14">R_ARM_PRIVATE_14</span>         <a href="#R_ARM">R_ARM</a> = 126
    <span id="R_ARM_PRIVATE_15">R_ARM_PRIVATE_15</span>         <a href="#R_ARM">R_ARM</a> = 127
    <span id="R_ARM_ME_TOO">R_ARM_ME_TOO</span>             <a href="#R_ARM">R_ARM</a> = 128
    <span id="R_ARM_THM_TLS_DESCSEQ16">R_ARM_THM_TLS_DESCSEQ16</span>  <a href="#R_ARM">R_ARM</a> = 129
    <span id="R_ARM_THM_TLS_DESCSEQ32">R_ARM_THM_TLS_DESCSEQ32</span>  <a href="#R_ARM">R_ARM</a> = 130
    <span id="R_ARM_THM_GOT_BREL12">R_ARM_THM_GOT_BREL12</span>     <a href="#R_ARM">R_ARM</a> = 131
    <span id="R_ARM_THM_ALU_ABS_G0_NC">R_ARM_THM_ALU_ABS_G0_NC</span>  <a href="#R_ARM">R_ARM</a> = 132
    <span id="R_ARM_THM_ALU_ABS_G1_NC">R_ARM_THM_ALU_ABS_G1_NC</span>  <a href="#R_ARM">R_ARM</a> = 133
    <span id="R_ARM_THM_ALU_ABS_G2_NC">R_ARM_THM_ALU_ABS_G2_NC</span>  <a href="#R_ARM">R_ARM</a> = 134
    <span id="R_ARM_THM_ALU_ABS_G3">R_ARM_THM_ALU_ABS_G3</span>     <a href="#R_ARM">R_ARM</a> = 135
    <span id="R_ARM_IRELATIVE">R_ARM_IRELATIVE</span>          <a href="#R_ARM">R_ARM</a> = 160
    <span id="R_ARM_RXPC25">R_ARM_RXPC25</span>             <a href="#R_ARM">R_ARM</a> = 249
    <span id="R_ARM_RSBREL32">R_ARM_RSBREL32</span>           <a href="#R_ARM">R_ARM</a> = 250
    <span id="R_ARM_THM_RPC22">R_ARM_THM_RPC22</span>          <a href="#R_ARM">R_ARM</a> = 251
    <span id="R_ARM_RREL32">R_ARM_RREL32</span>             <a href="#R_ARM">R_ARM</a> = 252
    <span id="R_ARM_RABS32">R_ARM_RABS32</span>             <a href="#R_ARM">R_ARM</a> = 253
    <span id="R_ARM_RPC24">R_ARM_RPC24</span>              <a href="#R_ARM">R_ARM</a> = 254
    <span id="R_ARM_RBASE">R_ARM_RBASE</span>              <a href="#R_ARM">R_ARM</a> = 255
)</pre>


<h3 id="R_ARM.GoString">func (R_ARM) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1469">GoString</a>
    <a href="#R_ARM.GoString">¶</a></h3>
<pre>func (i <a href="#R_ARM">R_ARM</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="R_ARM.String">func (R_ARM) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1468">String</a>
    <a href="#R_ARM.String">¶</a></h3>
<pre>func (i <a href="#R_ARM">R_ARM</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="R_MIPS">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1568">R_MIPS</a>
    <a href="#R_MIPS">¶</a></h2>
<pre>type R_MIPS <a href="/builtin/#int">int</a></pre>

Relocation types for MIPS.

<pre>const (
    <span id="R_MIPS_NONE">R_MIPS_NONE</span>          <a href="#R_MIPS">R_MIPS</a> = 0
    <span id="R_MIPS_16">R_MIPS_16</span>            <a href="#R_MIPS">R_MIPS</a> = 1
    <span id="R_MIPS_32">R_MIPS_32</span>            <a href="#R_MIPS">R_MIPS</a> = 2
    <span id="R_MIPS_REL32">R_MIPS_REL32</span>         <a href="#R_MIPS">R_MIPS</a> = 3
    <span id="R_MIPS_26">R_MIPS_26</span>            <a href="#R_MIPS">R_MIPS</a> = 4
    <span id="R_MIPS_HI16">R_MIPS_HI16</span>          <a href="#R_MIPS">R_MIPS</a> = 5  <span class="comment">/* high 16 bits of symbol value */</span>
    <span id="R_MIPS_LO16">R_MIPS_LO16</span>          <a href="#R_MIPS">R_MIPS</a> = 6  <span class="comment">/* low 16 bits of symbol value */</span>
    <span id="R_MIPS_GPREL16">R_MIPS_GPREL16</span>       <a href="#R_MIPS">R_MIPS</a> = 7  <span class="comment">/* GP-relative reference  */</span>
    <span id="R_MIPS_LITERAL">R_MIPS_LITERAL</span>       <a href="#R_MIPS">R_MIPS</a> = 8  <span class="comment">/* Reference to literal section  */</span>
    <span id="R_MIPS_GOT16">R_MIPS_GOT16</span>         <a href="#R_MIPS">R_MIPS</a> = 9  <span class="comment">/* Reference to global offset table */</span>
    <span id="R_MIPS_PC16">R_MIPS_PC16</span>          <a href="#R_MIPS">R_MIPS</a> = 10 <span class="comment">/* 16 bit PC relative reference */</span>
    <span id="R_MIPS_CALL16">R_MIPS_CALL16</span>        <a href="#R_MIPS">R_MIPS</a> = 11 <span class="comment">/* 16 bit call through glbl offset tbl */</span>
    <span id="R_MIPS_GPREL32">R_MIPS_GPREL32</span>       <a href="#R_MIPS">R_MIPS</a> = 12
    <span id="R_MIPS_SHIFT5">R_MIPS_SHIFT5</span>        <a href="#R_MIPS">R_MIPS</a> = 16
    <span id="R_MIPS_SHIFT6">R_MIPS_SHIFT6</span>        <a href="#R_MIPS">R_MIPS</a> = 17
    <span id="R_MIPS_64">R_MIPS_64</span>            <a href="#R_MIPS">R_MIPS</a> = 18
    <span id="R_MIPS_GOT_DISP">R_MIPS_GOT_DISP</span>      <a href="#R_MIPS">R_MIPS</a> = 19
    <span id="R_MIPS_GOT_PAGE">R_MIPS_GOT_PAGE</span>      <a href="#R_MIPS">R_MIPS</a> = 20
    <span id="R_MIPS_GOT_OFST">R_MIPS_GOT_OFST</span>      <a href="#R_MIPS">R_MIPS</a> = 21
    <span id="R_MIPS_GOT_HI16">R_MIPS_GOT_HI16</span>      <a href="#R_MIPS">R_MIPS</a> = 22
    <span id="R_MIPS_GOT_LO16">R_MIPS_GOT_LO16</span>      <a href="#R_MIPS">R_MIPS</a> = 23
    <span id="R_MIPS_SUB">R_MIPS_SUB</span>           <a href="#R_MIPS">R_MIPS</a> = 24
    <span id="R_MIPS_INSERT_A">R_MIPS_INSERT_A</span>      <a href="#R_MIPS">R_MIPS</a> = 25
    <span id="R_MIPS_INSERT_B">R_MIPS_INSERT_B</span>      <a href="#R_MIPS">R_MIPS</a> = 26
    <span id="R_MIPS_DELETE">R_MIPS_DELETE</span>        <a href="#R_MIPS">R_MIPS</a> = 27
    <span id="R_MIPS_HIGHER">R_MIPS_HIGHER</span>        <a href="#R_MIPS">R_MIPS</a> = 28
    <span id="R_MIPS_HIGHEST">R_MIPS_HIGHEST</span>       <a href="#R_MIPS">R_MIPS</a> = 29
    <span id="R_MIPS_CALL_HI16">R_MIPS_CALL_HI16</span>     <a href="#R_MIPS">R_MIPS</a> = 30
    <span id="R_MIPS_CALL_LO16">R_MIPS_CALL_LO16</span>     <a href="#R_MIPS">R_MIPS</a> = 31
    <span id="R_MIPS_SCN_DISP">R_MIPS_SCN_DISP</span>      <a href="#R_MIPS">R_MIPS</a> = 32
    <span id="R_MIPS_REL16">R_MIPS_REL16</span>         <a href="#R_MIPS">R_MIPS</a> = 33
    <span id="R_MIPS_ADD_IMMEDIATE">R_MIPS_ADD_IMMEDIATE</span> <a href="#R_MIPS">R_MIPS</a> = 34
    <span id="R_MIPS_PJUMP">R_MIPS_PJUMP</span>         <a href="#R_MIPS">R_MIPS</a> = 35
    <span id="R_MIPS_RELGOT">R_MIPS_RELGOT</span>        <a href="#R_MIPS">R_MIPS</a> = 36
    <span id="R_MIPS_JALR">R_MIPS_JALR</span>          <a href="#R_MIPS">R_MIPS</a> = 37

    <span id="R_MIPS_TLS_DTPMOD32">R_MIPS_TLS_DTPMOD32</span>    <a href="#R_MIPS">R_MIPS</a> = 38 <span class="comment">/* Module number 32 bit */</span>
    <span id="R_MIPS_TLS_DTPREL32">R_MIPS_TLS_DTPREL32</span>    <a href="#R_MIPS">R_MIPS</a> = 39 <span class="comment">/* Module-relative offset 32 bit */</span>
    <span id="R_MIPS_TLS_DTPMOD64">R_MIPS_TLS_DTPMOD64</span>    <a href="#R_MIPS">R_MIPS</a> = 40 <span class="comment">/* Module number 64 bit */</span>
    <span id="R_MIPS_TLS_DTPREL64">R_MIPS_TLS_DTPREL64</span>    <a href="#R_MIPS">R_MIPS</a> = 41 <span class="comment">/* Module-relative offset 64 bit */</span>
    <span id="R_MIPS_TLS_GD">R_MIPS_TLS_GD</span>          <a href="#R_MIPS">R_MIPS</a> = 42 <span class="comment">/* 16 bit GOT offset for GD */</span>
    <span id="R_MIPS_TLS_LDM">R_MIPS_TLS_LDM</span>         <a href="#R_MIPS">R_MIPS</a> = 43 <span class="comment">/* 16 bit GOT offset for LDM */</span>
    <span id="R_MIPS_TLS_DTPREL_HI16">R_MIPS_TLS_DTPREL_HI16</span> <a href="#R_MIPS">R_MIPS</a> = 44 <span class="comment">/* Module-relative offset, high 16 bits */</span>
    <span id="R_MIPS_TLS_DTPREL_LO16">R_MIPS_TLS_DTPREL_LO16</span> <a href="#R_MIPS">R_MIPS</a> = 45 <span class="comment">/* Module-relative offset, low 16 bits */</span>
    <span id="R_MIPS_TLS_GOTTPREL">R_MIPS_TLS_GOTTPREL</span>    <a href="#R_MIPS">R_MIPS</a> = 46 <span class="comment">/* 16 bit GOT offset for IE */</span>
    <span id="R_MIPS_TLS_TPREL32">R_MIPS_TLS_TPREL32</span>     <a href="#R_MIPS">R_MIPS</a> = 47 <span class="comment">/* TP-relative offset, 32 bit */</span>
    <span id="R_MIPS_TLS_TPREL64">R_MIPS_TLS_TPREL64</span>     <a href="#R_MIPS">R_MIPS</a> = 48 <span class="comment">/* TP-relative offset, 64 bit */</span>
    <span id="R_MIPS_TLS_TPREL_HI16">R_MIPS_TLS_TPREL_HI16</span>  <a href="#R_MIPS">R_MIPS</a> = 49 <span class="comment">/* TP-relative offset, high 16 bits */</span>
    <span id="R_MIPS_TLS_TPREL_LO16">R_MIPS_TLS_TPREL_LO16</span>  <a href="#R_MIPS">R_MIPS</a> = 50 <span class="comment">/* TP-relative offset, low 16 bits */</span>
)</pre>


<h3 id="R_MIPS.GoString">func (R_MIPS) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1674">GoString</a>
    <a href="#R_MIPS.GoString">¶</a></h3>
<pre>func (i <a href="#R_MIPS">R_MIPS</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="R_MIPS.String">func (R_MIPS) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1673">String</a>
    <a href="#R_MIPS.String">¶</a></h3>
<pre>func (i <a href="#R_MIPS">R_MIPS</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="R_PPC">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1682">R_PPC</a>
    <a href="#R_PPC">¶</a></h2>
<pre>type R_PPC <a href="/builtin/#int">int</a></pre>

Relocation types for PowerPC.

Values that are shared by both R_PPC and R_PPC64 are prefixed with R_POWERPC_ in
the ELF standard. For the R_PPC type, the relevant shared relocations have been
renamed with the prefix R_PPC_. The original name follows the value in a
comment.

<pre>const (
    <span id="R_PPC_NONE">R_PPC_NONE</span>            <a href="#R_PPC">R_PPC</a> = 0  <span class="comment">// R_POWERPC_NONE</span>
    <span id="R_PPC_ADDR32">R_PPC_ADDR32</span>          <a href="#R_PPC">R_PPC</a> = 1  <span class="comment">// R_POWERPC_ADDR32</span>
    <span id="R_PPC_ADDR24">R_PPC_ADDR24</span>          <a href="#R_PPC">R_PPC</a> = 2  <span class="comment">// R_POWERPC_ADDR24</span>
    <span id="R_PPC_ADDR16">R_PPC_ADDR16</span>          <a href="#R_PPC">R_PPC</a> = 3  <span class="comment">// R_POWERPC_ADDR16</span>
    <span id="R_PPC_ADDR16_LO">R_PPC_ADDR16_LO</span>       <a href="#R_PPC">R_PPC</a> = 4  <span class="comment">// R_POWERPC_ADDR16_LO</span>
    <span id="R_PPC_ADDR16_HI">R_PPC_ADDR16_HI</span>       <a href="#R_PPC">R_PPC</a> = 5  <span class="comment">// R_POWERPC_ADDR16_HI</span>
    <span id="R_PPC_ADDR16_HA">R_PPC_ADDR16_HA</span>       <a href="#R_PPC">R_PPC</a> = 6  <span class="comment">// R_POWERPC_ADDR16_HA</span>
    <span id="R_PPC_ADDR14">R_PPC_ADDR14</span>          <a href="#R_PPC">R_PPC</a> = 7  <span class="comment">// R_POWERPC_ADDR14</span>
    <span id="R_PPC_ADDR14_BRTAKEN">R_PPC_ADDR14_BRTAKEN</span>  <a href="#R_PPC">R_PPC</a> = 8  <span class="comment">// R_POWERPC_ADDR14_BRTAKEN</span>
    <span id="R_PPC_ADDR14_BRNTAKEN">R_PPC_ADDR14_BRNTAKEN</span> <a href="#R_PPC">R_PPC</a> = 9  <span class="comment">// R_POWERPC_ADDR14_BRNTAKEN</span>
    <span id="R_PPC_REL24">R_PPC_REL24</span>           <a href="#R_PPC">R_PPC</a> = 10 <span class="comment">// R_POWERPC_REL24</span>
    <span id="R_PPC_REL14">R_PPC_REL14</span>           <a href="#R_PPC">R_PPC</a> = 11 <span class="comment">// R_POWERPC_REL14</span>
    <span id="R_PPC_REL14_BRTAKEN">R_PPC_REL14_BRTAKEN</span>   <a href="#R_PPC">R_PPC</a> = 12 <span class="comment">// R_POWERPC_REL14_BRTAKEN</span>
    <span id="R_PPC_REL14_BRNTAKEN">R_PPC_REL14_BRNTAKEN</span>  <a href="#R_PPC">R_PPC</a> = 13 <span class="comment">// R_POWERPC_REL14_BRNTAKEN</span>
    <span id="R_PPC_GOT16">R_PPC_GOT16</span>           <a href="#R_PPC">R_PPC</a> = 14 <span class="comment">// R_POWERPC_GOT16</span>
    <span id="R_PPC_GOT16_LO">R_PPC_GOT16_LO</span>        <a href="#R_PPC">R_PPC</a> = 15 <span class="comment">// R_POWERPC_GOT16_LO</span>
    <span id="R_PPC_GOT16_HI">R_PPC_GOT16_HI</span>        <a href="#R_PPC">R_PPC</a> = 16 <span class="comment">// R_POWERPC_GOT16_HI</span>
    <span id="R_PPC_GOT16_HA">R_PPC_GOT16_HA</span>        <a href="#R_PPC">R_PPC</a> = 17 <span class="comment">// R_POWERPC_GOT16_HA</span>
    <span id="R_PPC_PLTREL24">R_PPC_PLTREL24</span>        <a href="#R_PPC">R_PPC</a> = 18
    <span id="R_PPC_COPY">R_PPC_COPY</span>            <a href="#R_PPC">R_PPC</a> = 19 <span class="comment">// R_POWERPC_COPY</span>
    <span id="R_PPC_GLOB_DAT">R_PPC_GLOB_DAT</span>        <a href="#R_PPC">R_PPC</a> = 20 <span class="comment">// R_POWERPC_GLOB_DAT</span>
    <span id="R_PPC_JMP_SLOT">R_PPC_JMP_SLOT</span>        <a href="#R_PPC">R_PPC</a> = 21 <span class="comment">// R_POWERPC_JMP_SLOT</span>
    <span id="R_PPC_RELATIVE">R_PPC_RELATIVE</span>        <a href="#R_PPC">R_PPC</a> = 22 <span class="comment">// R_POWERPC_RELATIVE</span>
    <span id="R_PPC_LOCAL24PC">R_PPC_LOCAL24PC</span>       <a href="#R_PPC">R_PPC</a> = 23
    <span id="R_PPC_UADDR32">R_PPC_UADDR32</span>         <a href="#R_PPC">R_PPC</a> = 24 <span class="comment">// R_POWERPC_UADDR32</span>
    <span id="R_PPC_UADDR16">R_PPC_UADDR16</span>         <a href="#R_PPC">R_PPC</a> = 25 <span class="comment">// R_POWERPC_UADDR16</span>
    <span id="R_PPC_REL32">R_PPC_REL32</span>           <a href="#R_PPC">R_PPC</a> = 26 <span class="comment">// R_POWERPC_REL32</span>
    <span id="R_PPC_PLT32">R_PPC_PLT32</span>           <a href="#R_PPC">R_PPC</a> = 27 <span class="comment">// R_POWERPC_PLT32</span>
    <span id="R_PPC_PLTREL32">R_PPC_PLTREL32</span>        <a href="#R_PPC">R_PPC</a> = 28 <span class="comment">// R_POWERPC_PLTREL32</span>
    <span id="R_PPC_PLT16_LO">R_PPC_PLT16_LO</span>        <a href="#R_PPC">R_PPC</a> = 29 <span class="comment">// R_POWERPC_PLT16_LO</span>
    <span id="R_PPC_PLT16_HI">R_PPC_PLT16_HI</span>        <a href="#R_PPC">R_PPC</a> = 30 <span class="comment">// R_POWERPC_PLT16_HI</span>
    <span id="R_PPC_PLT16_HA">R_PPC_PLT16_HA</span>        <a href="#R_PPC">R_PPC</a> = 31 <span class="comment">// R_POWERPC_PLT16_HA</span>
    <span id="R_PPC_SDAREL16">R_PPC_SDAREL16</span>        <a href="#R_PPC">R_PPC</a> = 32
    <span id="R_PPC_SECTOFF">R_PPC_SECTOFF</span>         <a href="#R_PPC">R_PPC</a> = 33 <span class="comment">// R_POWERPC_SECTOFF</span>
    <span id="R_PPC_SECTOFF_LO">R_PPC_SECTOFF_LO</span>      <a href="#R_PPC">R_PPC</a> = 34 <span class="comment">// R_POWERPC_SECTOFF_LO</span>
    <span id="R_PPC_SECTOFF_HI">R_PPC_SECTOFF_HI</span>      <a href="#R_PPC">R_PPC</a> = 35 <span class="comment">// R_POWERPC_SECTOFF_HI</span>
    <span id="R_PPC_SECTOFF_HA">R_PPC_SECTOFF_HA</span>      <a href="#R_PPC">R_PPC</a> = 36 <span class="comment">// R_POWERPC_SECTOFF_HA</span>
    <span id="R_PPC_TLS">R_PPC_TLS</span>             <a href="#R_PPC">R_PPC</a> = 67 <span class="comment">// R_POWERPC_TLS</span>
    <span id="R_PPC_DTPMOD32">R_PPC_DTPMOD32</span>        <a href="#R_PPC">R_PPC</a> = 68 <span class="comment">// R_POWERPC_DTPMOD32</span>
    <span id="R_PPC_TPREL16">R_PPC_TPREL16</span>         <a href="#R_PPC">R_PPC</a> = 69 <span class="comment">// R_POWERPC_TPREL16</span>
    <span id="R_PPC_TPREL16_LO">R_PPC_TPREL16_LO</span>      <a href="#R_PPC">R_PPC</a> = 70 <span class="comment">// R_POWERPC_TPREL16_LO</span>
    <span id="R_PPC_TPREL16_HI">R_PPC_TPREL16_HI</span>      <a href="#R_PPC">R_PPC</a> = 71 <span class="comment">// R_POWERPC_TPREL16_HI</span>
    <span id="R_PPC_TPREL16_HA">R_PPC_TPREL16_HA</span>      <a href="#R_PPC">R_PPC</a> = 72 <span class="comment">// R_POWERPC_TPREL16_HA</span>
    <span id="R_PPC_TPREL32">R_PPC_TPREL32</span>         <a href="#R_PPC">R_PPC</a> = 73 <span class="comment">// R_POWERPC_TPREL32</span>
    <span id="R_PPC_DTPREL16">R_PPC_DTPREL16</span>        <a href="#R_PPC">R_PPC</a> = 74 <span class="comment">// R_POWERPC_DTPREL16</span>
    <span id="R_PPC_DTPREL16_LO">R_PPC_DTPREL16_LO</span>     <a href="#R_PPC">R_PPC</a> = 75 <span class="comment">// R_POWERPC_DTPREL16_LO</span>
    <span id="R_PPC_DTPREL16_HI">R_PPC_DTPREL16_HI</span>     <a href="#R_PPC">R_PPC</a> = 76 <span class="comment">// R_POWERPC_DTPREL16_HI</span>
    <span id="R_PPC_DTPREL16_HA">R_PPC_DTPREL16_HA</span>     <a href="#R_PPC">R_PPC</a> = 77 <span class="comment">// R_POWERPC_DTPREL16_HA</span>
    <span id="R_PPC_DTPREL32">R_PPC_DTPREL32</span>        <a href="#R_PPC">R_PPC</a> = 78 <span class="comment">// R_POWERPC_DTPREL32</span>
    <span id="R_PPC_GOT_TLSGD16">R_PPC_GOT_TLSGD16</span>     <a href="#R_PPC">R_PPC</a> = 79 <span class="comment">// R_POWERPC_GOT_TLSGD16</span>
    <span id="R_PPC_GOT_TLSGD16_LO">R_PPC_GOT_TLSGD16_LO</span>  <a href="#R_PPC">R_PPC</a> = 80 <span class="comment">// R_POWERPC_GOT_TLSGD16_LO</span>
    <span id="R_PPC_GOT_TLSGD16_HI">R_PPC_GOT_TLSGD16_HI</span>  <a href="#R_PPC">R_PPC</a> = 81 <span class="comment">// R_POWERPC_GOT_TLSGD16_HI</span>
    <span id="R_PPC_GOT_TLSGD16_HA">R_PPC_GOT_TLSGD16_HA</span>  <a href="#R_PPC">R_PPC</a> = 82 <span class="comment">// R_POWERPC_GOT_TLSGD16_HA</span>
    <span id="R_PPC_GOT_TLSLD16">R_PPC_GOT_TLSLD16</span>     <a href="#R_PPC">R_PPC</a> = 83 <span class="comment">// R_POWERPC_GOT_TLSLD16</span>
    <span id="R_PPC_GOT_TLSLD16_LO">R_PPC_GOT_TLSLD16_LO</span>  <a href="#R_PPC">R_PPC</a> = 84 <span class="comment">// R_POWERPC_GOT_TLSLD16_LO</span>
    <span id="R_PPC_GOT_TLSLD16_HI">R_PPC_GOT_TLSLD16_HI</span>  <a href="#R_PPC">R_PPC</a> = 85 <span class="comment">// R_POWERPC_GOT_TLSLD16_HI</span>
    <span id="R_PPC_GOT_TLSLD16_HA">R_PPC_GOT_TLSLD16_HA</span>  <a href="#R_PPC">R_PPC</a> = 86 <span class="comment">// R_POWERPC_GOT_TLSLD16_HA</span>
    <span id="R_PPC_GOT_TPREL16">R_PPC_GOT_TPREL16</span>     <a href="#R_PPC">R_PPC</a> = 87 <span class="comment">// R_POWERPC_GOT_TPREL16</span>
    <span id="R_PPC_GOT_TPREL16_LO">R_PPC_GOT_TPREL16_LO</span>  <a href="#R_PPC">R_PPC</a> = 88 <span class="comment">// R_POWERPC_GOT_TPREL16_LO</span>
    <span id="R_PPC_GOT_TPREL16_HI">R_PPC_GOT_TPREL16_HI</span>  <a href="#R_PPC">R_PPC</a> = 89 <span class="comment">// R_POWERPC_GOT_TPREL16_HI</span>
    <span id="R_PPC_GOT_TPREL16_HA">R_PPC_GOT_TPREL16_HA</span>  <a href="#R_PPC">R_PPC</a> = 90 <span class="comment">// R_POWERPC_GOT_TPREL16_HA</span>
    <span id="R_PPC_EMB_NADDR32">R_PPC_EMB_NADDR32</span>     <a href="#R_PPC">R_PPC</a> = 101
    <span id="R_PPC_EMB_NADDR16">R_PPC_EMB_NADDR16</span>     <a href="#R_PPC">R_PPC</a> = 102
    <span id="R_PPC_EMB_NADDR16_LO">R_PPC_EMB_NADDR16_LO</span>  <a href="#R_PPC">R_PPC</a> = 103
    <span id="R_PPC_EMB_NADDR16_HI">R_PPC_EMB_NADDR16_HI</span>  <a href="#R_PPC">R_PPC</a> = 104
    <span id="R_PPC_EMB_NADDR16_HA">R_PPC_EMB_NADDR16_HA</span>  <a href="#R_PPC">R_PPC</a> = 105
    <span id="R_PPC_EMB_SDAI16">R_PPC_EMB_SDAI16</span>      <a href="#R_PPC">R_PPC</a> = 106
    <span id="R_PPC_EMB_SDA2I16">R_PPC_EMB_SDA2I16</span>     <a href="#R_PPC">R_PPC</a> = 107
    <span id="R_PPC_EMB_SDA2REL">R_PPC_EMB_SDA2REL</span>     <a href="#R_PPC">R_PPC</a> = 108
    <span id="R_PPC_EMB_SDA21">R_PPC_EMB_SDA21</span>       <a href="#R_PPC">R_PPC</a> = 109
    <span id="R_PPC_EMB_MRKREF">R_PPC_EMB_MRKREF</span>      <a href="#R_PPC">R_PPC</a> = 110
    <span id="R_PPC_EMB_RELSEC16">R_PPC_EMB_RELSEC16</span>    <a href="#R_PPC">R_PPC</a> = 111
    <span id="R_PPC_EMB_RELST_LO">R_PPC_EMB_RELST_LO</span>    <a href="#R_PPC">R_PPC</a> = 112
    <span id="R_PPC_EMB_RELST_HI">R_PPC_EMB_RELST_HI</span>    <a href="#R_PPC">R_PPC</a> = 113
    <span id="R_PPC_EMB_RELST_HA">R_PPC_EMB_RELST_HA</span>    <a href="#R_PPC">R_PPC</a> = 114
    <span id="R_PPC_EMB_BIT_FLD">R_PPC_EMB_BIT_FLD</span>     <a href="#R_PPC">R_PPC</a> = 115
    <span id="R_PPC_EMB_RELSDA">R_PPC_EMB_RELSDA</span>      <a href="#R_PPC">R_PPC</a> = 116
)</pre>


<h3 id="R_PPC.GoString">func (R_PPC) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1845">GoString</a>
    <a href="#R_PPC.GoString">¶</a></h3>
<pre>func (i <a href="#R_PPC">R_PPC</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="R_PPC.String">func (R_PPC) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1844">String</a>
    <a href="#R_PPC.String">¶</a></h3>
<pre>func (i <a href="#R_PPC">R_PPC</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="R_PPC64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L1853">R_PPC64</a>
    <a href="#R_PPC64">¶</a></h2>
<pre>type R_PPC64 <a href="/builtin/#int">int</a></pre>

Relocation types for 64-bit PowerPC or Power Architecture processors.

Values that are shared by both R_PPC and R_PPC64 are prefixed with R_POWERPC_ in
the ELF standard. For the R_PPC64 type, the relevant shared relocations have
been renamed with the prefix R_PPC64_. The original name follows the value in a
comment.

<pre>const (
    <span id="R_PPC64_NONE">R_PPC64_NONE</span>               <a href="#R_PPC64">R_PPC64</a> = 0  <span class="comment">// R_POWERPC_NONE</span>
    <span id="R_PPC64_ADDR32">R_PPC64_ADDR32</span>             <a href="#R_PPC64">R_PPC64</a> = 1  <span class="comment">// R_POWERPC_ADDR32</span>
    <span id="R_PPC64_ADDR24">R_PPC64_ADDR24</span>             <a href="#R_PPC64">R_PPC64</a> = 2  <span class="comment">// R_POWERPC_ADDR24</span>
    <span id="R_PPC64_ADDR16">R_PPC64_ADDR16</span>             <a href="#R_PPC64">R_PPC64</a> = 3  <span class="comment">// R_POWERPC_ADDR16</span>
    <span id="R_PPC64_ADDR16_LO">R_PPC64_ADDR16_LO</span>          <a href="#R_PPC64">R_PPC64</a> = 4  <span class="comment">// R_POWERPC_ADDR16_LO</span>
    <span id="R_PPC64_ADDR16_HI">R_PPC64_ADDR16_HI</span>          <a href="#R_PPC64">R_PPC64</a> = 5  <span class="comment">// R_POWERPC_ADDR16_HI</span>
    <span id="R_PPC64_ADDR16_HA">R_PPC64_ADDR16_HA</span>          <a href="#R_PPC64">R_PPC64</a> = 6  <span class="comment">// R_POWERPC_ADDR16_HA</span>
    <span id="R_PPC64_ADDR14">R_PPC64_ADDR14</span>             <a href="#R_PPC64">R_PPC64</a> = 7  <span class="comment">// R_POWERPC_ADDR14</span>
    <span id="R_PPC64_ADDR14_BRTAKEN">R_PPC64_ADDR14_BRTAKEN</span>     <a href="#R_PPC64">R_PPC64</a> = 8  <span class="comment">// R_POWERPC_ADDR14_BRTAKEN</span>
    <span id="R_PPC64_ADDR14_BRNTAKEN">R_PPC64_ADDR14_BRNTAKEN</span>    <a href="#R_PPC64">R_PPC64</a> = 9  <span class="comment">// R_POWERPC_ADDR14_BRNTAKEN</span>
    <span id="R_PPC64_REL24">R_PPC64_REL24</span>              <a href="#R_PPC64">R_PPC64</a> = 10 <span class="comment">// R_POWERPC_REL24</span>
    <span id="R_PPC64_REL14">R_PPC64_REL14</span>              <a href="#R_PPC64">R_PPC64</a> = 11 <span class="comment">// R_POWERPC_REL14</span>
    <span id="R_PPC64_REL14_BRTAKEN">R_PPC64_REL14_BRTAKEN</span>      <a href="#R_PPC64">R_PPC64</a> = 12 <span class="comment">// R_POWERPC_REL14_BRTAKEN</span>
    <span id="R_PPC64_REL14_BRNTAKEN">R_PPC64_REL14_BRNTAKEN</span>     <a href="#R_PPC64">R_PPC64</a> = 13 <span class="comment">// R_POWERPC_REL14_BRNTAKEN</span>
    <span id="R_PPC64_GOT16">R_PPC64_GOT16</span>              <a href="#R_PPC64">R_PPC64</a> = 14 <span class="comment">// R_POWERPC_GOT16</span>
    <span id="R_PPC64_GOT16_LO">R_PPC64_GOT16_LO</span>           <a href="#R_PPC64">R_PPC64</a> = 15 <span class="comment">// R_POWERPC_GOT16_LO</span>
    <span id="R_PPC64_GOT16_HI">R_PPC64_GOT16_HI</span>           <a href="#R_PPC64">R_PPC64</a> = 16 <span class="comment">// R_POWERPC_GOT16_HI</span>
    <span id="R_PPC64_GOT16_HA">R_PPC64_GOT16_HA</span>           <a href="#R_PPC64">R_PPC64</a> = 17 <span class="comment">// R_POWERPC_GOT16_HA</span>
    <span id="R_PPC64_JMP_SLOT">R_PPC64_JMP_SLOT</span>           <a href="#R_PPC64">R_PPC64</a> = 21 <span class="comment">// R_POWERPC_JMP_SLOT</span>
    <span id="R_PPC64_REL32">R_PPC64_REL32</span>              <a href="#R_PPC64">R_PPC64</a> = 26 <span class="comment">// R_POWERPC_REL32</span>
    <span id="R_PPC64_ADDR64">R_PPC64_ADDR64</span>             <a href="#R_PPC64">R_PPC64</a> = 38
    <span id="R_PPC64_ADDR16_HIGHER">R_PPC64_ADDR16_HIGHER</span>      <a href="#R_PPC64">R_PPC64</a> = 39
    <span id="R_PPC64_ADDR16_HIGHERA">R_PPC64_ADDR16_HIGHERA</span>     <a href="#R_PPC64">R_PPC64</a> = 40
    <span id="R_PPC64_ADDR16_HIGHEST">R_PPC64_ADDR16_HIGHEST</span>     <a href="#R_PPC64">R_PPC64</a> = 41
    <span id="R_PPC64_ADDR16_HIGHESTA">R_PPC64_ADDR16_HIGHESTA</span>    <a href="#R_PPC64">R_PPC64</a> = 42
    <span id="R_PPC64_REL64">R_PPC64_REL64</span>              <a href="#R_PPC64">R_PPC64</a> = 44
    <span id="R_PPC64_TOC16">R_PPC64_TOC16</span>              <a href="#R_PPC64">R_PPC64</a> = 47
    <span id="R_PPC64_TOC16_LO">R_PPC64_TOC16_LO</span>           <a href="#R_PPC64">R_PPC64</a> = 48
    <span id="R_PPC64_TOC16_HI">R_PPC64_TOC16_HI</span>           <a href="#R_PPC64">R_PPC64</a> = 49
    <span id="R_PPC64_TOC16_HA">R_PPC64_TOC16_HA</span>           <a href="#R_PPC64">R_PPC64</a> = 50
    <span id="R_PPC64_TOC">R_PPC64_TOC</span>                <a href="#R_PPC64">R_PPC64</a> = 51
    <span id="R_PPC64_PLTGOT16">R_PPC64_PLTGOT16</span>           <a href="#R_PPC64">R_PPC64</a> = 52
    <span id="R_PPC64_PLTGOT16_LO">R_PPC64_PLTGOT16_LO</span>        <a href="#R_PPC64">R_PPC64</a> = 53
    <span id="R_PPC64_PLTGOT16_HI">R_PPC64_PLTGOT16_HI</span>        <a href="#R_PPC64">R_PPC64</a> = 54
    <span id="R_PPC64_PLTGOT16_HA">R_PPC64_PLTGOT16_HA</span>        <a href="#R_PPC64">R_PPC64</a> = 55
    <span id="R_PPC64_ADDR16_DS">R_PPC64_ADDR16_DS</span>          <a href="#R_PPC64">R_PPC64</a> = 56
    <span id="R_PPC64_ADDR16_LO_DS">R_PPC64_ADDR16_LO_DS</span>       <a href="#R_PPC64">R_PPC64</a> = 57
    <span id="R_PPC64_GOT16_DS">R_PPC64_GOT16_DS</span>           <a href="#R_PPC64">R_PPC64</a> = 58
    <span id="R_PPC64_GOT16_LO_DS">R_PPC64_GOT16_LO_DS</span>        <a href="#R_PPC64">R_PPC64</a> = 59
    <span id="R_PPC64_PLT16_LO_DS">R_PPC64_PLT16_LO_DS</span>        <a href="#R_PPC64">R_PPC64</a> = 60
    <span id="R_PPC64_SECTOFF_DS">R_PPC64_SECTOFF_DS</span>         <a href="#R_PPC64">R_PPC64</a> = 61
    <span id="R_PPC64_SECTOFF_LO_DS">R_PPC64_SECTOFF_LO_DS</span>      <a href="#R_PPC64">R_PPC64</a> = 61
    <span id="R_PPC64_TOC16_DS">R_PPC64_TOC16_DS</span>           <a href="#R_PPC64">R_PPC64</a> = 63
    <span id="R_PPC64_TOC16_LO_DS">R_PPC64_TOC16_LO_DS</span>        <a href="#R_PPC64">R_PPC64</a> = 64
    <span id="R_PPC64_PLTGOT16_DS">R_PPC64_PLTGOT16_DS</span>        <a href="#R_PPC64">R_PPC64</a> = 65
    <span id="R_PPC64_PLTGOT_LO_DS">R_PPC64_PLTGOT_LO_DS</span>       <a href="#R_PPC64">R_PPC64</a> = 66
    <span id="R_PPC64_TLS">R_PPC64_TLS</span>                <a href="#R_PPC64">R_PPC64</a> = 67 <span class="comment">// R_POWERPC_TLS</span>
    <span id="R_PPC64_DTPMOD64">R_PPC64_DTPMOD64</span>           <a href="#R_PPC64">R_PPC64</a> = 68 <span class="comment">// R_POWERPC_DTPMOD64</span>
    <span id="R_PPC64_TPREL16">R_PPC64_TPREL16</span>            <a href="#R_PPC64">R_PPC64</a> = 69 <span class="comment">// R_POWERPC_TPREL16</span>
    <span id="R_PPC64_TPREL16_LO">R_PPC64_TPREL16_LO</span>         <a href="#R_PPC64">R_PPC64</a> = 70 <span class="comment">// R_POWERPC_TPREL16_LO</span>
    <span id="R_PPC64_TPREL16_HI">R_PPC64_TPREL16_HI</span>         <a href="#R_PPC64">R_PPC64</a> = 71 <span class="comment">// R_POWERPC_TPREL16_HI</span>
    <span id="R_PPC64_TPREL16_HA">R_PPC64_TPREL16_HA</span>         <a href="#R_PPC64">R_PPC64</a> = 72 <span class="comment">// R_POWERPC_TPREL16_HA</span>
    <span id="R_PPC64_TPREL64">R_PPC64_TPREL64</span>            <a href="#R_PPC64">R_PPC64</a> = 73 <span class="comment">// R_POWERPC_TPREL64</span>
    <span id="R_PPC64_DTPREL16">R_PPC64_DTPREL16</span>           <a href="#R_PPC64">R_PPC64</a> = 74 <span class="comment">// R_POWERPC_DTPREL16</span>
    <span id="R_PPC64_DTPREL16_LO">R_PPC64_DTPREL16_LO</span>        <a href="#R_PPC64">R_PPC64</a> = 75 <span class="comment">// R_POWERPC_DTPREL16_LO</span>
    <span id="R_PPC64_DTPREL16_HI">R_PPC64_DTPREL16_HI</span>        <a href="#R_PPC64">R_PPC64</a> = 76 <span class="comment">// R_POWERPC_DTPREL16_HI</span>
    <span id="R_PPC64_DTPREL16_HA">R_PPC64_DTPREL16_HA</span>        <a href="#R_PPC64">R_PPC64</a> = 77 <span class="comment">// R_POWERPC_DTPREL16_HA</span>
    <span id="R_PPC64_DTPREL64">R_PPC64_DTPREL64</span>           <a href="#R_PPC64">R_PPC64</a> = 78 <span class="comment">// R_POWERPC_DTPREL64</span>
    <span id="R_PPC64_GOT_TLSGD16">R_PPC64_GOT_TLSGD16</span>        <a href="#R_PPC64">R_PPC64</a> = 79 <span class="comment">// R_POWERPC_GOT_TLSGD16</span>
    <span id="R_PPC64_GOT_TLSGD16_LO">R_PPC64_GOT_TLSGD16_LO</span>     <a href="#R_PPC64">R_PPC64</a> = 80 <span class="comment">// R_POWERPC_GOT_TLSGD16_LO</span>
    <span id="R_PPC64_GOT_TLSGD16_HI">R_PPC64_GOT_TLSGD16_HI</span>     <a href="#R_PPC64">R_PPC64</a> = 81 <span class="comment">// R_POWERPC_GOT_TLSGD16_HI</span>
    <span id="R_PPC64_GOT_TLSGD16_HA">R_PPC64_GOT_TLSGD16_HA</span>     <a href="#R_PPC64">R_PPC64</a> = 82 <span class="comment">// R_POWERPC_GOT_TLSGD16_HA</span>
    <span id="R_PPC64_GOT_TLSLD16">R_PPC64_GOT_TLSLD16</span>        <a href="#R_PPC64">R_PPC64</a> = 83 <span class="comment">// R_POWERPC_GOT_TLSLD16</span>
    <span id="R_PPC64_GOT_TLSLD16_LO">R_PPC64_GOT_TLSLD16_LO</span>     <a href="#R_PPC64">R_PPC64</a> = 84 <span class="comment">// R_POWERPC_GOT_TLSLD16_LO</span>
    <span id="R_PPC64_GOT_TLSLD16_HI">R_PPC64_GOT_TLSLD16_HI</span>     <a href="#R_PPC64">R_PPC64</a> = 85 <span class="comment">// R_POWERPC_GOT_TLSLD16_HI</span>
    <span id="R_PPC64_GOT_TLSLD16_HA">R_PPC64_GOT_TLSLD16_HA</span>     <a href="#R_PPC64">R_PPC64</a> = 86 <span class="comment">// R_POWERPC_GOT_TLSLD16_HA</span>
    <span id="R_PPC64_GOT_TPREL16_DS">R_PPC64_GOT_TPREL16_DS</span>     <a href="#R_PPC64">R_PPC64</a> = 87 <span class="comment">// R_POWERPC_GOT_TPREL16_DS</span>
    <span id="R_PPC64_GOT_TPREL16_LO_DS">R_PPC64_GOT_TPREL16_LO_DS</span>  <a href="#R_PPC64">R_PPC64</a> = 88 <span class="comment">// R_POWERPC_GOT_TPREL16_LO_DS</span>
    <span id="R_PPC64_GOT_TPREL16_HI">R_PPC64_GOT_TPREL16_HI</span>     <a href="#R_PPC64">R_PPC64</a> = 89 <span class="comment">// R_POWERPC_GOT_TPREL16_HI</span>
    <span id="R_PPC64_GOT_TPREL16_HA">R_PPC64_GOT_TPREL16_HA</span>     <a href="#R_PPC64">R_PPC64</a> = 90 <span class="comment">// R_POWERPC_GOT_TPREL16_HA</span>
    <span id="R_PPC64_GOT_DTPREL16_DS">R_PPC64_GOT_DTPREL16_DS</span>    <a href="#R_PPC64">R_PPC64</a> = 91 <span class="comment">// R_POWERPC_GOT_DTPREL16_DS</span>
    <span id="R_PPC64_GOT_DTPREL16_LO_DS">R_PPC64_GOT_DTPREL16_LO_DS</span> <a href="#R_PPC64">R_PPC64</a> = 92 <span class="comment">// R_POWERPC_GOT_DTPREL16_LO_DS</span>
    <span id="R_PPC64_GOT_DTPREL16_HI">R_PPC64_GOT_DTPREL16_HI</span>    <a href="#R_PPC64">R_PPC64</a> = 93 <span class="comment">// R_POWERPC_GOT_DTPREL16_HI</span>
    <span id="R_PPC64_GOT_DTPREL16_HA">R_PPC64_GOT_DTPREL16_HA</span>    <a href="#R_PPC64">R_PPC64</a> = 94 <span class="comment">// R_POWERPC_GOT_DTPREL16_HA</span>
    <span id="R_PPC64_TPREL16_DS">R_PPC64_TPREL16_DS</span>         <a href="#R_PPC64">R_PPC64</a> = 95
    <span id="R_PPC64_TPREL16_LO_DS">R_PPC64_TPREL16_LO_DS</span>      <a href="#R_PPC64">R_PPC64</a> = 96
    <span id="R_PPC64_TPREL16_HIGHER">R_PPC64_TPREL16_HIGHER</span>     <a href="#R_PPC64">R_PPC64</a> = 97
    <span id="R_PPC64_TPREL16_HIGHERA">R_PPC64_TPREL16_HIGHERA</span>    <a href="#R_PPC64">R_PPC64</a> = 98
    <span id="R_PPC64_TPREL16_HIGHEST">R_PPC64_TPREL16_HIGHEST</span>    <a href="#R_PPC64">R_PPC64</a> = 99
    <span id="R_PPC64_TPREL16_HIGHESTA">R_PPC64_TPREL16_HIGHESTA</span>   <a href="#R_PPC64">R_PPC64</a> = 100
    <span id="R_PPC64_DTPREL16_DS">R_PPC64_DTPREL16_DS</span>        <a href="#R_PPC64">R_PPC64</a> = 101
    <span id="R_PPC64_DTPREL16_LO_DS">R_PPC64_DTPREL16_LO_DS</span>     <a href="#R_PPC64">R_PPC64</a> = 102
    <span id="R_PPC64_DTPREL16_HIGHER">R_PPC64_DTPREL16_HIGHER</span>    <a href="#R_PPC64">R_PPC64</a> = 103
    <span id="R_PPC64_DTPREL16_HIGHERA">R_PPC64_DTPREL16_HIGHERA</span>   <a href="#R_PPC64">R_PPC64</a> = 104
    <span id="R_PPC64_DTPREL16_HIGHEST">R_PPC64_DTPREL16_HIGHEST</span>   <a href="#R_PPC64">R_PPC64</a> = 105
    <span id="R_PPC64_DTPREL16_HIGHESTA">R_PPC64_DTPREL16_HIGHESTA</span>  <a href="#R_PPC64">R_PPC64</a> = 106
    <span id="R_PPC64_TLSGD">R_PPC64_TLSGD</span>              <a href="#R_PPC64">R_PPC64</a> = 107
    <span id="R_PPC64_TLSLD">R_PPC64_TLSLD</span>              <a href="#R_PPC64">R_PPC64</a> = 108
    <span id="R_PPC64_TOCSAVE">R_PPC64_TOCSAVE</span>            <a href="#R_PPC64">R_PPC64</a> = 109
    <span id="R_PPC64_ADDR16_HIGH">R_PPC64_ADDR16_HIGH</span>        <a href="#R_PPC64">R_PPC64</a> = 110
    <span id="R_PPC64_ADDR16_HIGHA">R_PPC64_ADDR16_HIGHA</span>       <a href="#R_PPC64">R_PPC64</a> = 111
    <span id="R_PPC64_TPREL16_HIGH">R_PPC64_TPREL16_HIGH</span>       <a href="#R_PPC64">R_PPC64</a> = 112
    <span id="R_PPC64_TPREL16_HIGHA">R_PPC64_TPREL16_HIGHA</span>      <a href="#R_PPC64">R_PPC64</a> = 113
    <span id="R_PPC64_DTPREL16_HIGH">R_PPC64_DTPREL16_HIGH</span>      <a href="#R_PPC64">R_PPC64</a> = 114
    <span id="R_PPC64_DTPREL16_HIGHA">R_PPC64_DTPREL16_HIGHA</span>     <a href="#R_PPC64">R_PPC64</a> = 115
    <span id="R_PPC64_REL24_NOTOC">R_PPC64_REL24_NOTOC</span>        <a href="#R_PPC64">R_PPC64</a> = 116
    <span id="R_PPC64_ADDR64_LOCAL">R_PPC64_ADDR64_LOCAL</span>       <a href="#R_PPC64">R_PPC64</a> = 117
    <span id="R_PPC64_ENTRY">R_PPC64_ENTRY</span>              <a href="#R_PPC64">R_PPC64</a> = 118
    <span id="R_PPC64_REL16DX_HA">R_PPC64_REL16DX_HA</span>         <a href="#R_PPC64">R_PPC64</a> = 246 <span class="comment">// R_POWERPC_REL16DX_HA</span>
    <span id="R_PPC64_JMP_IREL">R_PPC64_JMP_IREL</span>           <a href="#R_PPC64">R_PPC64</a> = 247
    <span id="R_PPC64_IRELATIVE">R_PPC64_IRELATIVE</span>          <a href="#R_PPC64">R_PPC64</a> = 248 <span class="comment">// R_POWERPC_IRELATIVE</span>
    <span id="R_PPC64_REL16">R_PPC64_REL16</span>              <a href="#R_PPC64">R_PPC64</a> = 249 <span class="comment">// R_POWERPC_REL16</span>
    <span id="R_PPC64_REL16_LO">R_PPC64_REL16_LO</span>           <a href="#R_PPC64">R_PPC64</a> = 250 <span class="comment">// R_POWERPC_REL16_LO</span>
    <span id="R_PPC64_REL16_HI">R_PPC64_REL16_HI</span>           <a href="#R_PPC64">R_PPC64</a> = 251 <span class="comment">// R_POWERPC_REL16_HI</span>
    <span id="R_PPC64_REL16_HA">R_PPC64_REL16_HA</span>           <a href="#R_PPC64">R_PPC64</a> = 252 <span class="comment">// R_POWERPC_REL16_HA</span>
)</pre>


<h3 id="R_PPC64.GoString">func (R_PPC64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2072">GoString</a>
    <a href="#R_PPC64.GoString">¶</a></h3>
<pre>func (i <a href="#R_PPC64">R_PPC64</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="R_PPC64.String">func (R_PPC64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2071">String</a>
    <a href="#R_PPC64.String">¶</a></h3>
<pre>func (i <a href="#R_PPC64">R_PPC64</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="R_SPARC">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2209">R_SPARC</a>
    <a href="#R_SPARC">¶</a></h2>
<pre>type R_SPARC <a href="/builtin/#int">int</a></pre>

Relocation types for SPARC.

<pre>const (
    <span id="R_SPARC_NONE">R_SPARC_NONE</span>     <a href="#R_SPARC">R_SPARC</a> = 0
    <span id="R_SPARC_8">R_SPARC_8</span>        <a href="#R_SPARC">R_SPARC</a> = 1
    <span id="R_SPARC_16">R_SPARC_16</span>       <a href="#R_SPARC">R_SPARC</a> = 2
    <span id="R_SPARC_32">R_SPARC_32</span>       <a href="#R_SPARC">R_SPARC</a> = 3
    <span id="R_SPARC_DISP8">R_SPARC_DISP8</span>    <a href="#R_SPARC">R_SPARC</a> = 4
    <span id="R_SPARC_DISP16">R_SPARC_DISP16</span>   <a href="#R_SPARC">R_SPARC</a> = 5
    <span id="R_SPARC_DISP32">R_SPARC_DISP32</span>   <a href="#R_SPARC">R_SPARC</a> = 6
    <span id="R_SPARC_WDISP30">R_SPARC_WDISP30</span>  <a href="#R_SPARC">R_SPARC</a> = 7
    <span id="R_SPARC_WDISP22">R_SPARC_WDISP22</span>  <a href="#R_SPARC">R_SPARC</a> = 8
    <span id="R_SPARC_HI22">R_SPARC_HI22</span>     <a href="#R_SPARC">R_SPARC</a> = 9
    <span id="R_SPARC_22">R_SPARC_22</span>       <a href="#R_SPARC">R_SPARC</a> = 10
    <span id="R_SPARC_13">R_SPARC_13</span>       <a href="#R_SPARC">R_SPARC</a> = 11
    <span id="R_SPARC_LO10">R_SPARC_LO10</span>     <a href="#R_SPARC">R_SPARC</a> = 12
    <span id="R_SPARC_GOT10">R_SPARC_GOT10</span>    <a href="#R_SPARC">R_SPARC</a> = 13
    <span id="R_SPARC_GOT13">R_SPARC_GOT13</span>    <a href="#R_SPARC">R_SPARC</a> = 14
    <span id="R_SPARC_GOT22">R_SPARC_GOT22</span>    <a href="#R_SPARC">R_SPARC</a> = 15
    <span id="R_SPARC_PC10">R_SPARC_PC10</span>     <a href="#R_SPARC">R_SPARC</a> = 16
    <span id="R_SPARC_PC22">R_SPARC_PC22</span>     <a href="#R_SPARC">R_SPARC</a> = 17
    <span id="R_SPARC_WPLT30">R_SPARC_WPLT30</span>   <a href="#R_SPARC">R_SPARC</a> = 18
    <span id="R_SPARC_COPY">R_SPARC_COPY</span>     <a href="#R_SPARC">R_SPARC</a> = 19
    <span id="R_SPARC_GLOB_DAT">R_SPARC_GLOB_DAT</span> <a href="#R_SPARC">R_SPARC</a> = 20
    <span id="R_SPARC_JMP_SLOT">R_SPARC_JMP_SLOT</span> <a href="#R_SPARC">R_SPARC</a> = 21
    <span id="R_SPARC_RELATIVE">R_SPARC_RELATIVE</span> <a href="#R_SPARC">R_SPARC</a> = 22
    <span id="R_SPARC_UA32">R_SPARC_UA32</span>     <a href="#R_SPARC">R_SPARC</a> = 23
    <span id="R_SPARC_PLT32">R_SPARC_PLT32</span>    <a href="#R_SPARC">R_SPARC</a> = 24
    <span id="R_SPARC_HIPLT22">R_SPARC_HIPLT22</span>  <a href="#R_SPARC">R_SPARC</a> = 25
    <span id="R_SPARC_LOPLT10">R_SPARC_LOPLT10</span>  <a href="#R_SPARC">R_SPARC</a> = 26
    <span id="R_SPARC_PCPLT32">R_SPARC_PCPLT32</span>  <a href="#R_SPARC">R_SPARC</a> = 27
    <span id="R_SPARC_PCPLT22">R_SPARC_PCPLT22</span>  <a href="#R_SPARC">R_SPARC</a> = 28
    <span id="R_SPARC_PCPLT10">R_SPARC_PCPLT10</span>  <a href="#R_SPARC">R_SPARC</a> = 29
    <span id="R_SPARC_10">R_SPARC_10</span>       <a href="#R_SPARC">R_SPARC</a> = 30
    <span id="R_SPARC_11">R_SPARC_11</span>       <a href="#R_SPARC">R_SPARC</a> = 31
    <span id="R_SPARC_64">R_SPARC_64</span>       <a href="#R_SPARC">R_SPARC</a> = 32
    <span id="R_SPARC_OLO10">R_SPARC_OLO10</span>    <a href="#R_SPARC">R_SPARC</a> = 33
    <span id="R_SPARC_HH22">R_SPARC_HH22</span>     <a href="#R_SPARC">R_SPARC</a> = 34
    <span id="R_SPARC_HM10">R_SPARC_HM10</span>     <a href="#R_SPARC">R_SPARC</a> = 35
    <span id="R_SPARC_LM22">R_SPARC_LM22</span>     <a href="#R_SPARC">R_SPARC</a> = 36
    <span id="R_SPARC_PC_HH22">R_SPARC_PC_HH22</span>  <a href="#R_SPARC">R_SPARC</a> = 37
    <span id="R_SPARC_PC_HM10">R_SPARC_PC_HM10</span>  <a href="#R_SPARC">R_SPARC</a> = 38
    <span id="R_SPARC_PC_LM22">R_SPARC_PC_LM22</span>  <a href="#R_SPARC">R_SPARC</a> = 39
    <span id="R_SPARC_WDISP16">R_SPARC_WDISP16</span>  <a href="#R_SPARC">R_SPARC</a> = 40
    <span id="R_SPARC_WDISP19">R_SPARC_WDISP19</span>  <a href="#R_SPARC">R_SPARC</a> = 41
    <span id="R_SPARC_GLOB_JMP">R_SPARC_GLOB_JMP</span> <a href="#R_SPARC">R_SPARC</a> = 42
    <span id="R_SPARC_7">R_SPARC_7</span>        <a href="#R_SPARC">R_SPARC</a> = 43
    <span id="R_SPARC_5">R_SPARC_5</span>        <a href="#R_SPARC">R_SPARC</a> = 44
    <span id="R_SPARC_6">R_SPARC_6</span>        <a href="#R_SPARC">R_SPARC</a> = 45
    <span id="R_SPARC_DISP64">R_SPARC_DISP64</span>   <a href="#R_SPARC">R_SPARC</a> = 46
    <span id="R_SPARC_PLT64">R_SPARC_PLT64</span>    <a href="#R_SPARC">R_SPARC</a> = 47
    <span id="R_SPARC_HIX22">R_SPARC_HIX22</span>    <a href="#R_SPARC">R_SPARC</a> = 48
    <span id="R_SPARC_LOX10">R_SPARC_LOX10</span>    <a href="#R_SPARC">R_SPARC</a> = 49
    <span id="R_SPARC_H44">R_SPARC_H44</span>      <a href="#R_SPARC">R_SPARC</a> = 50
    <span id="R_SPARC_M44">R_SPARC_M44</span>      <a href="#R_SPARC">R_SPARC</a> = 51
    <span id="R_SPARC_L44">R_SPARC_L44</span>      <a href="#R_SPARC">R_SPARC</a> = 52
    <span id="R_SPARC_REGISTER">R_SPARC_REGISTER</span> <a href="#R_SPARC">R_SPARC</a> = 53
    <span id="R_SPARC_UA64">R_SPARC_UA64</span>     <a href="#R_SPARC">R_SPARC</a> = 54
    <span id="R_SPARC_UA16">R_SPARC_UA16</span>     <a href="#R_SPARC">R_SPARC</a> = 55
)</pre>


<h3 id="R_SPARC.GoString">func (R_SPARC) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2330">GoString</a>
    <a href="#R_SPARC.GoString">¶</a></h3>
<pre>func (i <a href="#R_SPARC">R_SPARC</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="R_SPARC.String">func (R_SPARC) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2329">String</a>
    <a href="#R_SPARC.String">¶</a></h3>
<pre>func (i <a href="#R_SPARC">R_SPARC</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="R_X86_64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L734">R_X86_64</a>
    <a href="#R_X86_64">¶</a></h2>
<pre>type R_X86_64 <a href="/builtin/#int">int</a></pre>

Relocation types for x86-64.

<pre>const (
    <span id="R_X86_64_NONE">R_X86_64_NONE</span>            <a href="#R_X86_64">R_X86_64</a> = 0  <span class="comment">/* No relocation. */</span>
    <span id="R_X86_64_64">R_X86_64_64</span>              <a href="#R_X86_64">R_X86_64</a> = 1  <span class="comment">/* Add 64 bit symbol value. */</span>
    <span id="R_X86_64_PC32">R_X86_64_PC32</span>            <a href="#R_X86_64">R_X86_64</a> = 2  <span class="comment">/* PC-relative 32 bit signed sym value. */</span>
    <span id="R_X86_64_GOT32">R_X86_64_GOT32</span>           <a href="#R_X86_64">R_X86_64</a> = 3  <span class="comment">/* PC-relative 32 bit GOT offset. */</span>
    <span id="R_X86_64_PLT32">R_X86_64_PLT32</span>           <a href="#R_X86_64">R_X86_64</a> = 4  <span class="comment">/* PC-relative 32 bit PLT offset. */</span>
    <span id="R_X86_64_COPY">R_X86_64_COPY</span>            <a href="#R_X86_64">R_X86_64</a> = 5  <span class="comment">/* Copy data from shared object. */</span>
    <span id="R_X86_64_GLOB_DAT">R_X86_64_GLOB_DAT</span>        <a href="#R_X86_64">R_X86_64</a> = 6  <span class="comment">/* Set GOT entry to data address. */</span>
    <span id="R_X86_64_JMP_SLOT">R_X86_64_JMP_SLOT</span>        <a href="#R_X86_64">R_X86_64</a> = 7  <span class="comment">/* Set GOT entry to code address. */</span>
    <span id="R_X86_64_RELATIVE">R_X86_64_RELATIVE</span>        <a href="#R_X86_64">R_X86_64</a> = 8  <span class="comment">/* Add load address of shared object. */</span>
    <span id="R_X86_64_GOTPCREL">R_X86_64_GOTPCREL</span>        <a href="#R_X86_64">R_X86_64</a> = 9  <span class="comment">/* Add 32 bit signed pcrel offset to GOT. */</span>
    <span id="R_X86_64_32">R_X86_64_32</span>              <a href="#R_X86_64">R_X86_64</a> = 10 <span class="comment">/* Add 32 bit zero extended symbol value */</span>
    <span id="R_X86_64_32S">R_X86_64_32S</span>             <a href="#R_X86_64">R_X86_64</a> = 11 <span class="comment">/* Add 32 bit sign extended symbol value */</span>
    <span id="R_X86_64_16">R_X86_64_16</span>              <a href="#R_X86_64">R_X86_64</a> = 12 <span class="comment">/* Add 16 bit zero extended symbol value */</span>
    <span id="R_X86_64_PC16">R_X86_64_PC16</span>            <a href="#R_X86_64">R_X86_64</a> = 13 <span class="comment">/* Add 16 bit signed extended pc relative symbol value */</span>
    <span id="R_X86_64_8">R_X86_64_8</span>               <a href="#R_X86_64">R_X86_64</a> = 14 <span class="comment">/* Add 8 bit zero extended symbol value */</span>
    <span id="R_X86_64_PC8">R_X86_64_PC8</span>             <a href="#R_X86_64">R_X86_64</a> = 15 <span class="comment">/* Add 8 bit signed extended pc relative symbol value */</span>
    <span id="R_X86_64_DTPMOD64">R_X86_64_DTPMOD64</span>        <a href="#R_X86_64">R_X86_64</a> = 16 <span class="comment">/* ID of module containing symbol */</span>
    <span id="R_X86_64_DTPOFF64">R_X86_64_DTPOFF64</span>        <a href="#R_X86_64">R_X86_64</a> = 17 <span class="comment">/* Offset in TLS block */</span>
    <span id="R_X86_64_TPOFF64">R_X86_64_TPOFF64</span>         <a href="#R_X86_64">R_X86_64</a> = 18 <span class="comment">/* Offset in static TLS block */</span>
    <span id="R_X86_64_TLSGD">R_X86_64_TLSGD</span>           <a href="#R_X86_64">R_X86_64</a> = 19 <span class="comment">/* PC relative offset to GD GOT entry */</span>
    <span id="R_X86_64_TLSLD">R_X86_64_TLSLD</span>           <a href="#R_X86_64">R_X86_64</a> = 20 <span class="comment">/* PC relative offset to LD GOT entry */</span>
    <span id="R_X86_64_DTPOFF32">R_X86_64_DTPOFF32</span>        <a href="#R_X86_64">R_X86_64</a> = 21 <span class="comment">/* Offset in TLS block */</span>
    <span id="R_X86_64_GOTTPOFF">R_X86_64_GOTTPOFF</span>        <a href="#R_X86_64">R_X86_64</a> = 22 <span class="comment">/* PC relative offset to IE GOT entry */</span>
    <span id="R_X86_64_TPOFF32">R_X86_64_TPOFF32</span>         <a href="#R_X86_64">R_X86_64</a> = 23 <span class="comment">/* Offset in static TLS block */</span>
    <span id="R_X86_64_PC64">R_X86_64_PC64</span>            <a href="#R_X86_64">R_X86_64</a> = 24 <span class="comment">/* PC relative 64-bit sign extended symbol value. */</span>
    <span id="R_X86_64_GOTOFF64">R_X86_64_GOTOFF64</span>        <a href="#R_X86_64">R_X86_64</a> = 25
    <span id="R_X86_64_GOTPC32">R_X86_64_GOTPC32</span>         <a href="#R_X86_64">R_X86_64</a> = 26
    <span id="R_X86_64_GOT64">R_X86_64_GOT64</span>           <a href="#R_X86_64">R_X86_64</a> = 27
    <span id="R_X86_64_GOTPCREL64">R_X86_64_GOTPCREL64</span>      <a href="#R_X86_64">R_X86_64</a> = 28
    <span id="R_X86_64_GOTPC64">R_X86_64_GOTPC64</span>         <a href="#R_X86_64">R_X86_64</a> = 29
    <span id="R_X86_64_GOTPLT64">R_X86_64_GOTPLT64</span>        <a href="#R_X86_64">R_X86_64</a> = 30
    <span id="R_X86_64_PLTOFF64">R_X86_64_PLTOFF64</span>        <a href="#R_X86_64">R_X86_64</a> = 31
    <span id="R_X86_64_SIZE32">R_X86_64_SIZE32</span>          <a href="#R_X86_64">R_X86_64</a> = 32
    <span id="R_X86_64_SIZE64">R_X86_64_SIZE64</span>          <a href="#R_X86_64">R_X86_64</a> = 33
    <span id="R_X86_64_GOTPC32_TLSDESC">R_X86_64_GOTPC32_TLSDESC</span> <a href="#R_X86_64">R_X86_64</a> = 34
    <span id="R_X86_64_TLSDESC_CALL">R_X86_64_TLSDESC_CALL</span>    <a href="#R_X86_64">R_X86_64</a> = 35
    <span id="R_X86_64_TLSDESC">R_X86_64_TLSDESC</span>         <a href="#R_X86_64">R_X86_64</a> = 36
    <span id="R_X86_64_IRELATIVE">R_X86_64_IRELATIVE</span>       <a href="#R_X86_64">R_X86_64</a> = 37
    <span id="R_X86_64_RELATIVE64">R_X86_64_RELATIVE64</span>      <a href="#R_X86_64">R_X86_64</a> = 38
    <span id="R_X86_64_PC32_BND">R_X86_64_PC32_BND</span>        <a href="#R_X86_64">R_X86_64</a> = 39
    <span id="R_X86_64_PLT32_BND">R_X86_64_PLT32_BND</span>       <a href="#R_X86_64">R_X86_64</a> = 40
    <span id="R_X86_64_GOTPCRELX">R_X86_64_GOTPCRELX</span>       <a href="#R_X86_64">R_X86_64</a> = 41
    <span id="R_X86_64_REX_GOTPCRELX">R_X86_64_REX_GOTPCRELX</span>   <a href="#R_X86_64">R_X86_64</a> = 42
)</pre>


<h3 id="R_X86_64.GoString">func (R_X86_64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L829">GoString</a>
    <a href="#R_X86_64.GoString">¶</a></h3>
<pre>func (i <a href="#R_X86_64">R_X86_64</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="R_X86_64.String">func (R_X86_64) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L828">String</a>
    <a href="#R_X86_64.String">¶</a></h3>
<pre>func (i <a href="#R_X86_64">R_X86_64</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Rel32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2397">Rel32</a>
    <a href="#Rel32">¶</a></h2>
<pre>type Rel32 struct {
<span id="Rel32.Off"></span>    Off  <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Location to be relocated. */</span>
<span id="Rel32.Info"></span>    Info <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Relocation type and symbol index. */</span>
}</pre>

ELF32 Relocations that don't need an addend field.

<h2 id="Rel64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2499">Rel64</a>
    <a href="#Rel64">¶</a></h2>
<pre>type Rel64 struct {
<span id="Rel64.Off"></span>    Off  <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Location to be relocated. */</span>
<span id="Rel64.Info"></span>    Info <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Relocation type and symbol index. */</span>
}</pre>

ELF64 relocations that don't need an addend field.

<h2 id="Rela32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2403">Rela32</a>
    <a href="#Rela32">¶</a></h2>
<pre>type Rela32 struct {
<span id="Rela32.Off"></span>    Off    <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Location to be relocated. */</span>
<span id="Rela32.Info"></span>    Info   <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Relocation type and symbol index. */</span>
<span id="Rela32.Addend"></span>    Addend <a href="/builtin/#int32">int32</a>  <span class="comment">/* Addend. */</span>
}</pre>

ELF32 Relocations that need an addend field.

<h2 id="Rela64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2505">Rela64</a>
    <a href="#Rela64">¶</a></h2>
<pre>type Rela64 struct {
<span id="Rela64.Off"></span>    Off    <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Location to be relocated. */</span>
<span id="Rela64.Info"></span>    Info   <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Relocation type and symbol index. */</span>
<span id="Rela64.Addend"></span>    Addend <a href="/builtin/#int64">int64</a>  <span class="comment">/* Addend. */</span>
}</pre>

ELF64 relocations that need an addend field.

<h2 id="Section">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L71">Section</a>
    <a href="#Section">¶</a></h2>
<pre>type Section struct {
    <a href="#SectionHeader">SectionHeader</a>

    <span class="comment">// Embed ReaderAt for ReadAt method.</span>
    <span class="comment">// Do not embed SectionReader directly</span>
    <span class="comment">// to avoid having Read and Seek.</span>
    <span class="comment">// If a client wants Read and Seek it must use</span>
    <span class="comment">// Open() to avoid fighting over the seek offset</span>
    <span class="comment">// with other clients.</span>
    <span class="comment">//</span>
    <span class="comment">// ReaderAt may be nil if the section is not easily available</span>
    <span class="comment">// in a random-access form. For example, a compressed section</span>
    <span class="comment">// may have a nil ReaderAt.</span>
    <a href="/io/">io</a>.<a href="/io/#ReaderAt">ReaderAt</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Section represents a single section in an ELF file.

<h3 id="Section.Data">func (*Section) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L94">Data</a>
    <a href="#Section.Data">¶</a></h3>
<pre>func (s *<a href="#Section">Section</a>) Data() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Data reads and returns the contents of the ELF section. Even if the section is
stored compressed in the ELF file, Data returns uncompressed data.

<h3 id="Section.Open">func (*Section) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L112">Open</a>
    <a href="#Section.Open">¶</a></h3>
<pre>func (s *<a href="#Section">Section</a>) Open() <a href="/io/">io</a>.<a href="/io/#ReadSeeker">ReadSeeker</a></pre>

Open returns a new ReadSeeker reading the ELF section. Even if the section is
stored compressed in the ELF file, the ReadSeeker reads uncompressed data.

<h2 id="Section32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2354">Section32</a>
    <a href="#Section32">¶</a></h2>
<pre>type Section32 struct {
<span id="Section32.Name"></span>    Name      <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Section name (index into the section header string table). */</span>
<span id="Section32.Type"></span>    Type      <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Section type. */</span>
<span id="Section32.Flags"></span>    Flags     <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Section flags. */</span>
<span id="Section32.Addr"></span>    Addr      <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Address in memory image. */</span>
<span id="Section32.Off"></span>    Off       <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Offset in file. */</span>
<span id="Section32.Size"></span>    Size      <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Size in bytes. */</span>
<span id="Section32.Link"></span>    Link      <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Index of a related section. */</span>
<span id="Section32.Info"></span>    Info      <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Depends on section type. */</span>
<span id="Section32.Addralign"></span>    Addralign <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Alignment in bytes. */</span>
<span id="Section32.Entsize"></span>    Entsize   <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Size of each entry in section. */</span>
}</pre>

ELF32 Section header.

<h2 id="Section64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2455">Section64</a>
    <a href="#Section64">¶</a></h2>
<pre>type Section64 struct {
<span id="Section64.Name"></span>    Name      <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Section name (index into the section header string table). */</span>
<span id="Section64.Type"></span>    Type      <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Section type. */</span>
<span id="Section64.Flags"></span>    Flags     <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Section flags. */</span>
<span id="Section64.Addr"></span>    Addr      <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Address in memory image. */</span>
<span id="Section64.Off"></span>    Off       <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Offset in file. */</span>
<span id="Section64.Size"></span>    Size      <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Size in bytes. */</span>
<span id="Section64.Link"></span>    Link      <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Index of a related section. */</span>
<span id="Section64.Info"></span>    Info      <a href="/builtin/#uint32">uint32</a> <span class="comment">/* Depends on section type. */</span>
<span id="Section64.Addralign"></span>    Addralign <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Alignment in bytes. */</span>
<span id="Section64.Entsize"></span>    Entsize   <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Size of each entry in section. */</span>
}</pre>

ELF64 Section header.

<h2 id="SectionFlag">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L391">SectionFlag</a>
    <a href="#SectionFlag">¶</a></h2>
<pre>type SectionFlag <a href="/builtin/#uint32">uint32</a></pre>

Section flags.

<pre>const (
    <span id="SHF_WRITE">SHF_WRITE</span>            <a href="#SectionFlag">SectionFlag</a> = 0x1        <span class="comment">/* Section contains writable data. */</span>
    <span id="SHF_ALLOC">SHF_ALLOC</span>            <a href="#SectionFlag">SectionFlag</a> = 0x2        <span class="comment">/* Section occupies memory. */</span>
    <span id="SHF_EXECINSTR">SHF_EXECINSTR</span>        <a href="#SectionFlag">SectionFlag</a> = 0x4        <span class="comment">/* Section contains instructions. */</span>
    <span id="SHF_MERGE">SHF_MERGE</span>            <a href="#SectionFlag">SectionFlag</a> = 0x10       <span class="comment">/* Section may be merged. */</span>
    <span id="SHF_STRINGS">SHF_STRINGS</span>          <a href="#SectionFlag">SectionFlag</a> = 0x20       <span class="comment">/* Section contains strings. */</span>
    <span id="SHF_INFO_LINK">SHF_INFO_LINK</span>        <a href="#SectionFlag">SectionFlag</a> = 0x40       <span class="comment">/* sh_info holds section index. */</span>
    <span id="SHF_LINK_ORDER">SHF_LINK_ORDER</span>       <a href="#SectionFlag">SectionFlag</a> = 0x80       <span class="comment">/* Special ordering requirements. */</span>
    <span id="SHF_OS_NONCONFORMING">SHF_OS_NONCONFORMING</span> <a href="#SectionFlag">SectionFlag</a> = 0x100      <span class="comment">/* OS-specific processing required. */</span>
    <span id="SHF_GROUP">SHF_GROUP</span>            <a href="#SectionFlag">SectionFlag</a> = 0x200      <span class="comment">/* Member of section group. */</span>
    <span id="SHF_TLS">SHF_TLS</span>              <a href="#SectionFlag">SectionFlag</a> = 0x400      <span class="comment">/* Section contains TLS data. */</span>
    <span id="SHF_COMPRESSED">SHF_COMPRESSED</span>       <a href="#SectionFlag">SectionFlag</a> = 0x800      <span class="comment">/* Section is compressed. */</span>
    <span id="SHF_MASKOS">SHF_MASKOS</span>           <a href="#SectionFlag">SectionFlag</a> = 0x0ff00000 <span class="comment">/* OS-specific semantics. */</span>
    <span id="SHF_MASKPROC">SHF_MASKPROC</span>         <a href="#SectionFlag">SectionFlag</a> = 0xf0000000 <span class="comment">/* Processor-specific semantics. */</span>
)</pre>


<h3 id="SectionFlag.GoString">func (SectionFlag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L424">GoString</a>
    <a href="#SectionFlag.GoString">¶</a></h3>
<pre>func (i <a href="#SectionFlag">SectionFlag</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="SectionFlag.String">func (SectionFlag) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L423">String</a>
    <a href="#SectionFlag.String">¶</a></h3>
<pre>func (i <a href="#SectionFlag">SectionFlag</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="SectionHeader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L51">SectionHeader</a>
    <a href="#SectionHeader">¶</a></h2>
<pre>type SectionHeader struct {
<span id="SectionHeader.Name"></span>    Name      <a href="/builtin/#string">string</a>
<span id="SectionHeader.Type"></span>    Type      <a href="#SectionType">SectionType</a>
<span id="SectionHeader.Flags"></span>    Flags     <a href="#SectionFlag">SectionFlag</a>
<span id="SectionHeader.Addr"></span>    Addr      <a href="/builtin/#uint64">uint64</a>
<span id="SectionHeader.Offset"></span>    Offset    <a href="/builtin/#uint64">uint64</a>
<span id="SectionHeader.Size"></span>    Size      <a href="/builtin/#uint64">uint64</a>
<span id="SectionHeader.Link"></span>    Link      <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.Info"></span>    Info      <a href="/builtin/#uint32">uint32</a>
<span id="SectionHeader.Addralign"></span>    Addralign <a href="/builtin/#uint64">uint64</a>
<span id="SectionHeader.Entsize"></span>    Entsize   <a href="/builtin/#uint64">uint64</a>

<span id="SectionHeader.FileSize"></span>    <span class="comment">// FileSize is the size of this section in the file in bytes.</span>
    <span class="comment">// If a section is compressed, FileSize is the size of the</span>
    <span class="comment">// compressed data, while Size (above) is the size of the</span>
    <span class="comment">// uncompressed data.</span>
    FileSize <a href="/builtin/#uint64">uint64</a>
}</pre>

A SectionHeader represents a single ELF section header.

<h2 id="SectionIndex">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L294">SectionIndex</a>
    <a href="#SectionIndex">¶</a></h2>
<pre>type SectionIndex <a href="/builtin/#int">int</a></pre>

Special section indices.

<pre>const (
    <span id="SHN_UNDEF">SHN_UNDEF</span>     <a href="#SectionIndex">SectionIndex</a> = 0      <span class="comment">/* Undefined, missing, irrelevant. */</span>
    <span id="SHN_LORESERVE">SHN_LORESERVE</span> <a href="#SectionIndex">SectionIndex</a> = 0xff00 <span class="comment">/* First of reserved range. */</span>
    <span id="SHN_LOPROC">SHN_LOPROC</span>    <a href="#SectionIndex">SectionIndex</a> = 0xff00 <span class="comment">/* First processor-specific. */</span>
    <span id="SHN_HIPROC">SHN_HIPROC</span>    <a href="#SectionIndex">SectionIndex</a> = 0xff1f <span class="comment">/* Last processor-specific. */</span>
    <span id="SHN_LOOS">SHN_LOOS</span>      <a href="#SectionIndex">SectionIndex</a> = 0xff20 <span class="comment">/* First operating system-specific. */</span>
    <span id="SHN_HIOS">SHN_HIOS</span>      <a href="#SectionIndex">SectionIndex</a> = 0xff3f <span class="comment">/* Last operating system-specific. */</span>
    <span id="SHN_ABS">SHN_ABS</span>       <a href="#SectionIndex">SectionIndex</a> = 0xfff1 <span class="comment">/* Absolute values. */</span>
    <span id="SHN_COMMON">SHN_COMMON</span>    <a href="#SectionIndex">SectionIndex</a> = 0xfff2 <span class="comment">/* Common data. */</span>
    <span id="SHN_XINDEX">SHN_XINDEX</span>    <a href="#SectionIndex">SectionIndex</a> = 0xffff <span class="comment">/* Escape; index stored elsewhere. */</span>
    <span id="SHN_HIRESERVE">SHN_HIRESERVE</span> <a href="#SectionIndex">SectionIndex</a> = 0xffff <span class="comment">/* Last of reserved range. */</span>
)</pre>


<h3 id="SectionIndex.GoString">func (SectionIndex) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L319">GoString</a>
    <a href="#SectionIndex.GoString">¶</a></h3>
<pre>func (i <a href="#SectionIndex">SectionIndex</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="SectionIndex.String">func (SectionIndex) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L318">String</a>
    <a href="#SectionIndex.String">¶</a></h3>
<pre>func (i <a href="#SectionIndex">SectionIndex</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="SectionType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L322">SectionType</a>
    <a href="#SectionType">¶</a></h2>
<pre>type SectionType <a href="/builtin/#uint32">uint32</a></pre>

Section type.

<pre>const (
    <span id="SHT_NULL">SHT_NULL</span>           <a href="#SectionType">SectionType</a> = 0          <span class="comment">/* inactive */</span>
    <span id="SHT_PROGBITS">SHT_PROGBITS</span>       <a href="#SectionType">SectionType</a> = 1          <span class="comment">/* program defined information */</span>
    <span id="SHT_SYMTAB">SHT_SYMTAB</span>         <a href="#SectionType">SectionType</a> = 2          <span class="comment">/* symbol table section */</span>
    <span id="SHT_STRTAB">SHT_STRTAB</span>         <a href="#SectionType">SectionType</a> = 3          <span class="comment">/* string table section */</span>
    <span id="SHT_RELA">SHT_RELA</span>           <a href="#SectionType">SectionType</a> = 4          <span class="comment">/* relocation section with addends */</span>
    <span id="SHT_HASH">SHT_HASH</span>           <a href="#SectionType">SectionType</a> = 5          <span class="comment">/* symbol hash table section */</span>
    <span id="SHT_DYNAMIC">SHT_DYNAMIC</span>        <a href="#SectionType">SectionType</a> = 6          <span class="comment">/* dynamic section */</span>
    <span id="SHT_NOTE">SHT_NOTE</span>           <a href="#SectionType">SectionType</a> = 7          <span class="comment">/* note section */</span>
    <span id="SHT_NOBITS">SHT_NOBITS</span>         <a href="#SectionType">SectionType</a> = 8          <span class="comment">/* no space section */</span>
    <span id="SHT_REL">SHT_REL</span>            <a href="#SectionType">SectionType</a> = 9          <span class="comment">/* relocation section - no addends */</span>
    <span id="SHT_SHLIB">SHT_SHLIB</span>          <a href="#SectionType">SectionType</a> = 10         <span class="comment">/* reserved - purpose unknown */</span>
    <span id="SHT_DYNSYM">SHT_DYNSYM</span>         <a href="#SectionType">SectionType</a> = 11         <span class="comment">/* dynamic symbol table section */</span>
    <span id="SHT_INIT_ARRAY">SHT_INIT_ARRAY</span>     <a href="#SectionType">SectionType</a> = 14         <span class="comment">/* Initialization function pointers. */</span>
    <span id="SHT_FINI_ARRAY">SHT_FINI_ARRAY</span>     <a href="#SectionType">SectionType</a> = 15         <span class="comment">/* Termination function pointers. */</span>
    <span id="SHT_PREINIT_ARRAY">SHT_PREINIT_ARRAY</span>  <a href="#SectionType">SectionType</a> = 16         <span class="comment">/* Pre-initialization function ptrs. */</span>
    <span id="SHT_GROUP">SHT_GROUP</span>          <a href="#SectionType">SectionType</a> = 17         <span class="comment">/* Section group. */</span>
    <span id="SHT_SYMTAB_SHNDX">SHT_SYMTAB_SHNDX</span>   <a href="#SectionType">SectionType</a> = 18         <span class="comment">/* Section indexes (see SHN_XINDEX). */</span>
    <span id="SHT_LOOS">SHT_LOOS</span>           <a href="#SectionType">SectionType</a> = 0x60000000 <span class="comment">/* First of OS specific semantics */</span>
    <span id="SHT_GNU_ATTRIBUTES">SHT_GNU_ATTRIBUTES</span> <a href="#SectionType">SectionType</a> = 0x6ffffff5 <span class="comment">/* GNU object attributes */</span>
    <span id="SHT_GNU_HASH">SHT_GNU_HASH</span>       <a href="#SectionType">SectionType</a> = 0x6ffffff6 <span class="comment">/* GNU hash table */</span>
    <span id="SHT_GNU_LIBLIST">SHT_GNU_LIBLIST</span>    <a href="#SectionType">SectionType</a> = 0x6ffffff7 <span class="comment">/* GNU prelink library list */</span>
    <span id="SHT_GNU_VERDEF">SHT_GNU_VERDEF</span>     <a href="#SectionType">SectionType</a> = 0x6ffffffd <span class="comment">/* GNU version definition section */</span>
    <span id="SHT_GNU_VERNEED">SHT_GNU_VERNEED</span>    <a href="#SectionType">SectionType</a> = 0x6ffffffe <span class="comment">/* GNU version needs section */</span>
    <span id="SHT_GNU_VERSYM">SHT_GNU_VERSYM</span>     <a href="#SectionType">SectionType</a> = 0x6fffffff <span class="comment">/* GNU version symbol table */</span>
    <span id="SHT_HIOS">SHT_HIOS</span>           <a href="#SectionType">SectionType</a> = 0x6fffffff <span class="comment">/* Last of OS specific semantics */</span>
    <span id="SHT_LOPROC">SHT_LOPROC</span>         <a href="#SectionType">SectionType</a> = 0x70000000 <span class="comment">/* reserved range for processor */</span>
    <span id="SHT_HIPROC">SHT_HIPROC</span>         <a href="#SectionType">SectionType</a> = 0x7fffffff <span class="comment">/* specific section header types */</span>
    <span id="SHT_LOUSER">SHT_LOUSER</span>         <a href="#SectionType">SectionType</a> = 0x80000000 <span class="comment">/* reserved range for application */</span>
    <span id="SHT_HIUSER">SHT_HIUSER</span>         <a href="#SectionType">SectionType</a> = 0xffffffff <span class="comment">/* specific indexes */</span>
)</pre>


<h3 id="SectionType.GoString">func (SectionType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L388">GoString</a>
    <a href="#SectionType.GoString">¶</a></h3>
<pre>func (i <a href="#SectionType">SectionType</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="SectionType.String">func (SectionType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L387">String</a>
    <a href="#SectionType.String">¶</a></h3>
<pre>func (i <a href="#SectionType">SectionType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Sym32">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2414">Sym32</a>
    <a href="#Sym32">¶</a></h2>
<pre>type Sym32 struct {
<span id="Sym32.Name"></span>    Name  <a href="/builtin/#uint32">uint32</a>
<span id="Sym32.Value"></span>    Value <a href="/builtin/#uint32">uint32</a>
<span id="Sym32.Size"></span>    Size  <a href="/builtin/#uint32">uint32</a>
<span id="Sym32.Info"></span>    Info  <a href="/builtin/#uint8">uint8</a>
<span id="Sym32.Other"></span>    Other <a href="/builtin/#uint8">uint8</a>
<span id="Sym32.Shndx"></span>    Shndx <a href="/builtin/#uint16">uint16</a>
}</pre>

ELF32 Symbol.

<h2 id="Sym64">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2516">Sym64</a>
    <a href="#Sym64">¶</a></h2>
<pre>type Sym64 struct {
<span id="Sym64.Name"></span>    Name  <a href="/builtin/#uint32">uint32</a> <span class="comment">/* String table index of name. */</span>
<span id="Sym64.Info"></span>    Info  <a href="/builtin/#uint8">uint8</a>  <span class="comment">/* Type and binding information. */</span>
<span id="Sym64.Other"></span>    Other <a href="/builtin/#uint8">uint8</a>  <span class="comment">/* Reserved (not used). */</span>
<span id="Sym64.Shndx"></span>    Shndx <a href="/builtin/#uint16">uint16</a> <span class="comment">/* Section index of symbol. */</span>
<span id="Sym64.Value"></span>    Value <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Symbol value. */</span>
<span id="Sym64.Size"></span>    Size  <a href="/builtin/#uint64">uint64</a> <span class="comment">/* Size of associated object. */</span>
}</pre>

ELF64 symbol table entries.

<h2 id="SymBind">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L650">SymBind</a>
    <a href="#SymBind">¶</a></h2>
<pre>type SymBind <a href="/builtin/#int">int</a></pre>

Symbol Binding - ELFNN_ST_BIND - st_info

<pre>const (
    <span id="STB_LOCAL">STB_LOCAL</span>  <a href="#SymBind">SymBind</a> = 0  <span class="comment">/* Local symbol */</span>
    <span id="STB_GLOBAL">STB_GLOBAL</span> <a href="#SymBind">SymBind</a> = 1  <span class="comment">/* Global symbol */</span>
    <span id="STB_WEAK">STB_WEAK</span>   <a href="#SymBind">SymBind</a> = 2  <span class="comment">/* like global - lower precedence */</span>
    <span id="STB_LOOS">STB_LOOS</span>   <a href="#SymBind">SymBind</a> = 10 <span class="comment">/* Reserved range for operating system */</span>
    <span id="STB_HIOS">STB_HIOS</span>   <a href="#SymBind">SymBind</a> = 12 <span class="comment">/*   specific semantics. */</span>
    <span id="STB_LOPROC">STB_LOPROC</span> <a href="#SymBind">SymBind</a> = 13 <span class="comment">/* reserved range for processor */</span>
    <span id="STB_HIPROC">STB_HIPROC</span> <a href="#SymBind">SymBind</a> = 15 <span class="comment">/*   specific semantics. */</span>
)</pre>


<h3 id="ST_BIND">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2425">ST_BIND</a>
    <a href="#ST_BIND">¶</a></h3>
<pre>func ST_BIND(info <a href="/builtin/#uint8">uint8</a>) <a href="#SymBind">SymBind</a></pre>


<h3 id="SymBind.GoString">func (SymBind) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L673">GoString</a>
    <a href="#SymBind.GoString">¶</a></h3>
<pre>func (i <a href="#SymBind">SymBind</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="SymBind.String">func (SymBind) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L672">String</a>
    <a href="#SymBind.String">¶</a></h3>
<pre>func (i <a href="#SymBind">SymBind</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="SymType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L676">SymType</a>
    <a href="#SymType">¶</a></h2>
<pre>type SymType <a href="/builtin/#int">int</a></pre>

Symbol type - ELFNN_ST_TYPE - st_info

<pre>const (
    <span id="STT_NOTYPE">STT_NOTYPE</span>  <a href="#SymType">SymType</a> = 0  <span class="comment">/* Unspecified type. */</span>
    <span id="STT_OBJECT">STT_OBJECT</span>  <a href="#SymType">SymType</a> = 1  <span class="comment">/* Data object. */</span>
    <span id="STT_FUNC">STT_FUNC</span>    <a href="#SymType">SymType</a> = 2  <span class="comment">/* Function. */</span>
    <span id="STT_SECTION">STT_SECTION</span> <a href="#SymType">SymType</a> = 3  <span class="comment">/* Section. */</span>
    <span id="STT_FILE">STT_FILE</span>    <a href="#SymType">SymType</a> = 4  <span class="comment">/* Source file. */</span>
    <span id="STT_COMMON">STT_COMMON</span>  <a href="#SymType">SymType</a> = 5  <span class="comment">/* Uninitialized common block. */</span>
    <span id="STT_TLS">STT_TLS</span>     <a href="#SymType">SymType</a> = 6  <span class="comment">/* TLS object. */</span>
    <span id="STT_LOOS">STT_LOOS</span>    <a href="#SymType">SymType</a> = 10 <span class="comment">/* Reserved range for operating system */</span>
    <span id="STT_HIOS">STT_HIOS</span>    <a href="#SymType">SymType</a> = 12 <span class="comment">/*   specific semantics. */</span>
    <span id="STT_LOPROC">STT_LOPROC</span>  <a href="#SymType">SymType</a> = 13 <span class="comment">/* reserved range for processor */</span>
    <span id="STT_HIPROC">STT_HIPROC</span>  <a href="#SymType">SymType</a> = 15 <span class="comment">/*   specific semantics. */</span>
)</pre>


<h3 id="ST_TYPE">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2426">ST_TYPE</a>
    <a href="#ST_TYPE">¶</a></h3>
<pre>func ST_TYPE(info <a href="/builtin/#uint8">uint8</a>) <a href="#SymType">SymType</a></pre>


<h3 id="SymType.GoString">func (SymType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L707">GoString</a>
    <a href="#SymType.GoString">¶</a></h3>
<pre>func (i <a href="#SymType">SymType</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="SymType.String">func (SymType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L706">String</a>
    <a href="#SymType.String">¶</a></h3>
<pre>func (i <a href="#SymType">SymType</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="SymVis">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L710">SymVis</a>
    <a href="#SymVis">¶</a></h2>
<pre>type SymVis <a href="/builtin/#int">int</a></pre>

Symbol visibility - ELFNN_ST_VISIBILITY - st_other

<pre>const (
    <span id="STV_DEFAULT">STV_DEFAULT</span>   <a href="#SymVis">SymVis</a> = 0x0 <span class="comment">/* Default visibility (see binding). */</span>
    <span id="STV_INTERNAL">STV_INTERNAL</span>  <a href="#SymVis">SymVis</a> = 0x1 <span class="comment">/* Special meaning in relocatable objects. */</span>
    <span id="STV_HIDDEN">STV_HIDDEN</span>    <a href="#SymVis">SymVis</a> = 0x2 <span class="comment">/* Not visible. */</span>
    <span id="STV_PROTECTED">STV_PROTECTED</span> <a href="#SymVis">SymVis</a> = 0x3 <span class="comment">/* Visible but not preemptible. */</span>
)</pre>


<h3 id="ST_VISIBILITY">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L2430">ST_VISIBILITY</a>
    <a href="#ST_VISIBILITY">¶</a></h3>
<pre>func ST_VISIBILITY(other <a href="/builtin/#uint8">uint8</a>) <a href="#SymVis">SymVis</a></pre>


<h3 id="SymVis.GoString">func (SymVis) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L727">GoString</a>
    <a href="#SymVis.GoString">¶</a></h3>
<pre>func (i <a href="#SymVis">SymVis</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="SymVis.String">func (SymVis) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L726">String</a>
    <a href="#SymVis.String">¶</a></h3>
<pre>func (i <a href="#SymVis">SymVis</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Symbol">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/file.go#L159">Symbol</a>
    <a href="#Symbol">¶</a></h2>
<pre>type Symbol struct {
<span id="Symbol.Name"></span>    Name        <a href="/builtin/#string">string</a>
<span id="Symbol.Info"></span>    Info, Other <a href="/builtin/#byte">byte</a>
<span id="Symbol.Section"></span>    Section     <a href="#SectionIndex">SectionIndex</a>
<span id="Symbol.Value"></span>    Value, Size <a href="/builtin/#uint64">uint64</a>
}</pre>

A Symbol represents an entry in an ELF symbol table section.

<h2 id="Type">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L153">Type</a>
    <a href="#Type">¶</a></h2>
<pre>type Type <a href="/builtin/#uint16">uint16</a></pre>

Type is found in Header.Type.

<pre>const (
    <span id="ET_NONE">ET_NONE</span>   <a href="#Type">Type</a> = 0      <span class="comment">/* Unknown type. */</span>
    <span id="ET_REL">ET_REL</span>    <a href="#Type">Type</a> = 1      <span class="comment">/* Relocatable. */</span>
    <span id="ET_EXEC">ET_EXEC</span>   <a href="#Type">Type</a> = 2      <span class="comment">/* Executable. */</span>
    <span id="ET_DYN">ET_DYN</span>    <a href="#Type">Type</a> = 3      <span class="comment">/* Shared object. */</span>
    <span id="ET_CORE">ET_CORE</span>   <a href="#Type">Type</a> = 4      <span class="comment">/* Core file. */</span>
    <span id="ET_LOOS">ET_LOOS</span>   <a href="#Type">Type</a> = 0xfe00 <span class="comment">/* First operating system specific. */</span>
    <span id="ET_HIOS">ET_HIOS</span>   <a href="#Type">Type</a> = 0xfeff <span class="comment">/* Last operating system-specific. */</span>
    <span id="ET_LOPROC">ET_LOPROC</span> <a href="#Type">Type</a> = 0xff00 <span class="comment">/* First processor-specific. */</span>
    <span id="ET_HIPROC">ET_HIPROC</span> <a href="#Type">Type</a> = 0xffff <span class="comment">/* Last processor-specific. */</span>
)</pre>


<h3 id="Type.GoString">func (Type) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L180">GoString</a>
    <a href="#Type.GoString">¶</a></h3>
<pre>func (i <a href="#Type">Type</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="Type.String">func (Type) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L179">String</a>
    <a href="#Type.String">¶</a></h3>
<pre>func (i <a href="#Type">Type</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Version">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L55">Version</a>
    <a href="#Version">¶</a></h2>
<pre>type Version <a href="/builtin/#byte">byte</a></pre>

Version is found in Header.Ident[EI_VERSION] and Header.Version.

<pre>const (
    <span id="EV_NONE">EV_NONE</span>    <a href="#Version">Version</a> = 0
    <span id="EV_CURRENT">EV_CURRENT</span> <a href="#Version">Version</a> = 1
)</pre>


<h3 id="Version.GoString">func (Version) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L68">GoString</a>
    <a href="#Version.GoString">¶</a></h3>
<pre>func (i <a href="#Version">Version</a>) GoString() <a href="/builtin/#string">string</a></pre>


<h3 id="Version.String">func (Version) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/debug/elf/elf.go#L67">String</a>
    <a href="#Version.String">¶</a></h3>
<pre>func (i <a href="#Version">Version</a>) String() <a href="/builtin/#string">string</a></pre>



