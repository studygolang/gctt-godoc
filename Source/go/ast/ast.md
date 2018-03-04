version: 1.10
## package ast

  `import "go/ast"`

## Overview

Package ast declares the types used to represent syntax trees for Go packages.

## Index

- [func FileExports(src *File) bool](#FileExports)
- [func FilterDecl(decl Decl, f Filter) bool](#FilterDecl)
- [func FilterFile(src *File, f Filter) bool](#FilterFile)
- [func FilterPackage(pkg *Package, f Filter) bool](#FilterPackage)
- [func Fprint(w io.Writer, fset *token.FileSet, x interface{}, f FieldFilter) error](#Fprint)
- [func Inspect(node Node, f func(Node) bool)](#Inspect)
- [func IsExported(name string) bool](#IsExported)
- [func NotNilFilter(_ string, v reflect.Value) bool](#NotNilFilter)
- [func PackageExports(pkg *Package) bool](#PackageExports)
- [func Print(fset *token.FileSet, x interface{}) error](#Print)
- [func SortImports(fset *token.FileSet, f *File)](#SortImports)
- [func Walk(v Visitor, node Node)](#Walk)
- [type ArrayType](#ArrayType)
  - [func (x *ArrayType) End() token.Pos](#ArrayType.End)
  - [func (x *ArrayType) Pos() token.Pos](#ArrayType.Pos)
- [type AssignStmt](#AssignStmt)
  - [func (s *AssignStmt) End() token.Pos](#AssignStmt.End)
  - [func (s *AssignStmt) Pos() token.Pos](#AssignStmt.Pos)
- [type BadDecl](#BadDecl)
  - [func (d *BadDecl) End() token.Pos](#BadDecl.End)
  - [func (d *BadDecl) Pos() token.Pos](#BadDecl.Pos)
- [type BadExpr](#BadExpr)
  - [func (x *BadExpr) End() token.Pos](#BadExpr.End)
  - [func (x *BadExpr) Pos() token.Pos](#BadExpr.Pos)
- [type BadStmt](#BadStmt)
  - [func (s *BadStmt) End() token.Pos](#BadStmt.End)
  - [func (s *BadStmt) Pos() token.Pos](#BadStmt.Pos)
- [type BasicLit](#BasicLit)
  - [func (x *BasicLit) End() token.Pos](#BasicLit.End)
  - [func (x *BasicLit) Pos() token.Pos](#BasicLit.Pos)
- [type BinaryExpr](#BinaryExpr)
  - [func (x *BinaryExpr) End() token.Pos](#BinaryExpr.End)
  - [func (x *BinaryExpr) Pos() token.Pos](#BinaryExpr.Pos)
- [type BlockStmt](#BlockStmt)
  - [func (s *BlockStmt) End() token.Pos](#BlockStmt.End)
  - [func (s *BlockStmt) Pos() token.Pos](#BlockStmt.Pos)
- [type BranchStmt](#BranchStmt)
  - [func (s *BranchStmt) End() token.Pos](#BranchStmt.End)
  - [func (s *BranchStmt) Pos() token.Pos](#BranchStmt.Pos)
- [type CallExpr](#CallExpr)
  - [func (x *CallExpr) End() token.Pos](#CallExpr.End)
  - [func (x *CallExpr) Pos() token.Pos](#CallExpr.Pos)
- [type CaseClause](#CaseClause)
  - [func (s *CaseClause) End() token.Pos](#CaseClause.End)
  - [func (s *CaseClause) Pos() token.Pos](#CaseClause.Pos)
- [type ChanDir](#ChanDir)
- [type ChanType](#ChanType)
  - [func (x *ChanType) End() token.Pos](#ChanType.End)
  - [func (x *ChanType) Pos() token.Pos](#ChanType.Pos)
- [type CommClause](#CommClause)
  - [func (s *CommClause) End() token.Pos](#CommClause.End)
  - [func (s *CommClause) Pos() token.Pos](#CommClause.Pos)
- [type Comment](#Comment)
  - [func (c *Comment) End() token.Pos](#Comment.End)
  - [func (c *Comment) Pos() token.Pos](#Comment.Pos)
- [type CommentGroup](#CommentGroup)
  - [func (g *CommentGroup) End() token.Pos](#CommentGroup.End)
  - [func (g *CommentGroup) Pos() token.Pos](#CommentGroup.Pos)
  - [func (g *CommentGroup) Text() string](#CommentGroup.Text)
- [type CommentMap](#CommentMap)
  - [func NewCommentMap(fset *token.FileSet, node Node, comments []*CommentGroup) CommentMap](#NewCommentMap)
  - [func (cmap CommentMap) Comments() []*CommentGroup](#CommentMap.Comments)
  - [func (cmap CommentMap) Filter(node Node) CommentMap](#CommentMap.Filter)
  - [func (cmap CommentMap) String() string](#CommentMap.String)
  - [func (cmap CommentMap) Update(old, new Node) Node](#CommentMap.Update)
- [type CompositeLit](#CompositeLit)
  - [func (x *CompositeLit) End() token.Pos](#CompositeLit.End)
  - [func (x *CompositeLit) Pos() token.Pos](#CompositeLit.Pos)
- [type Decl](#Decl)
- [type DeclStmt](#DeclStmt)
  - [func (s *DeclStmt) End() token.Pos](#DeclStmt.End)
  - [func (s *DeclStmt) Pos() token.Pos](#DeclStmt.Pos)
- [type DeferStmt](#DeferStmt)
  - [func (s *DeferStmt) End() token.Pos](#DeferStmt.End)
  - [func (s *DeferStmt) Pos() token.Pos](#DeferStmt.Pos)
- [type Ellipsis](#Ellipsis)
  - [func (x *Ellipsis) End() token.Pos](#Ellipsis.End)
  - [func (x *Ellipsis) Pos() token.Pos](#Ellipsis.Pos)
- [type EmptyStmt](#EmptyStmt)
  - [func (s *EmptyStmt) End() token.Pos](#EmptyStmt.End)
  - [func (s *EmptyStmt) Pos() token.Pos](#EmptyStmt.Pos)
- [type Expr](#Expr)
- [type ExprStmt](#ExprStmt)
  - [func (s *ExprStmt) End() token.Pos](#ExprStmt.End)
  - [func (s *ExprStmt) Pos() token.Pos](#ExprStmt.Pos)
- [type Field](#Field)
  - [func (f *Field) End() token.Pos](#Field.End)
  - [func (f *Field) Pos() token.Pos](#Field.Pos)
- [type FieldFilter](#FieldFilter)
- [type FieldList](#FieldList)
  - [func (f *FieldList) End() token.Pos](#FieldList.End)
  - [func (f *FieldList) NumFields() int](#FieldList.NumFields)
  - [func (f *FieldList) Pos() token.Pos](#FieldList.Pos)
- [type File](#File)
  - [func MergePackageFiles(pkg *Package, mode MergeMode) *File](#MergePackageFiles)
  - [func (f *File) End() token.Pos](#File.End)
  - [func (f *File) Pos() token.Pos](#File.Pos)
- [type Filter](#Filter)
- [type ForStmt](#ForStmt)
  - [func (s *ForStmt) End() token.Pos](#ForStmt.End)
  - [func (s *ForStmt) Pos() token.Pos](#ForStmt.Pos)
- [type FuncDecl](#FuncDecl)
  - [func (d *FuncDecl) End() token.Pos](#FuncDecl.End)
  - [func (d *FuncDecl) Pos() token.Pos](#FuncDecl.Pos)
- [type FuncLit](#FuncLit)
  - [func (x *FuncLit) End() token.Pos](#FuncLit.End)
  - [func (x *FuncLit) Pos() token.Pos](#FuncLit.Pos)
- [type FuncType](#FuncType)
  - [func (x *FuncType) End() token.Pos](#FuncType.End)
  - [func (x *FuncType) Pos() token.Pos](#FuncType.Pos)
- [type GenDecl](#GenDecl)
  - [func (d *GenDecl) End() token.Pos](#GenDecl.End)
  - [func (d *GenDecl) Pos() token.Pos](#GenDecl.Pos)
- [type GoStmt](#GoStmt)
  - [func (s *GoStmt) End() token.Pos](#GoStmt.End)
  - [func (s *GoStmt) Pos() token.Pos](#GoStmt.Pos)
- [type Ident](#Ident)
  - [func NewIdent(name string) *Ident](#NewIdent)
  - [func (x *Ident) End() token.Pos](#Ident.End)
  - [func (id *Ident) IsExported() bool](#Ident.IsExported)
  - [func (x *Ident) Pos() token.Pos](#Ident.Pos)
  - [func (id *Ident) String() string](#Ident.String)
- [type IfStmt](#IfStmt)
  - [func (s *IfStmt) End() token.Pos](#IfStmt.End)
  - [func (s *IfStmt) Pos() token.Pos](#IfStmt.Pos)
- [type ImportSpec](#ImportSpec)
  - [func (s *ImportSpec) End() token.Pos](#ImportSpec.End)
  - [func (s *ImportSpec) Pos() token.Pos](#ImportSpec.Pos)
- [type Importer](#Importer)
- [type IncDecStmt](#IncDecStmt)
  - [func (s *IncDecStmt) End() token.Pos](#IncDecStmt.End)
  - [func (s *IncDecStmt) Pos() token.Pos](#IncDecStmt.Pos)
- [type IndexExpr](#IndexExpr)
  - [func (x *IndexExpr) End() token.Pos](#IndexExpr.End)
  - [func (x *IndexExpr) Pos() token.Pos](#IndexExpr.Pos)
- [type InterfaceType](#InterfaceType)
  - [func (x *InterfaceType) End() token.Pos](#InterfaceType.End)
  - [func (x *InterfaceType) Pos() token.Pos](#InterfaceType.Pos)
- [type KeyValueExpr](#KeyValueExpr)
  - [func (x *KeyValueExpr) End() token.Pos](#KeyValueExpr.End)
  - [func (x *KeyValueExpr) Pos() token.Pos](#KeyValueExpr.Pos)
- [type LabeledStmt](#LabeledStmt)
  - [func (s *LabeledStmt) End() token.Pos](#LabeledStmt.End)
  - [func (s *LabeledStmt) Pos() token.Pos](#LabeledStmt.Pos)
- [type MapType](#MapType)
  - [func (x *MapType) End() token.Pos](#MapType.End)
  - [func (x *MapType) Pos() token.Pos](#MapType.Pos)
- [type MergeMode](#MergeMode)
- [type Node](#Node)
- [type ObjKind](#ObjKind)
  - [func (kind ObjKind) String() string](#ObjKind.String)
- [type Object](#Object)
  - [func NewObj(kind ObjKind, name string) *Object](#NewObj)
  - [func (obj *Object) Pos() token.Pos](#Object.Pos)
- [type Package](#Package)
  - [func NewPackage(fset *token.FileSet, files map[string]*File, importer Importer, universe *Scope) (*Package, error)](#NewPackage)
  - [func (p *Package) End() token.Pos](#Package.End)
  - [func (p *Package) Pos() token.Pos](#Package.Pos)
- [type ParenExpr](#ParenExpr)
  - [func (x *ParenExpr) End() token.Pos](#ParenExpr.End)
  - [func (x *ParenExpr) Pos() token.Pos](#ParenExpr.Pos)
- [type RangeStmt](#RangeStmt)
  - [func (s *RangeStmt) End() token.Pos](#RangeStmt.End)
  - [func (s *RangeStmt) Pos() token.Pos](#RangeStmt.Pos)
- [type ReturnStmt](#ReturnStmt)
  - [func (s *ReturnStmt) End() token.Pos](#ReturnStmt.End)
  - [func (s *ReturnStmt) Pos() token.Pos](#ReturnStmt.Pos)
- [type Scope](#Scope)
  - [func NewScope(outer *Scope) *Scope](#NewScope)
  - [func (s *Scope) Insert(obj *Object) (alt *Object)](#Scope.Insert)
  - [func (s *Scope) Lookup(name string) *Object](#Scope.Lookup)
  - [func (s *Scope) String() string](#Scope.String)
- [type SelectStmt](#SelectStmt)
  - [func (s *SelectStmt) End() token.Pos](#SelectStmt.End)
  - [func (s *SelectStmt) Pos() token.Pos](#SelectStmt.Pos)
- [type SelectorExpr](#SelectorExpr)
  - [func (x *SelectorExpr) End() token.Pos](#SelectorExpr.End)
  - [func (x *SelectorExpr) Pos() token.Pos](#SelectorExpr.Pos)
- [type SendStmt](#SendStmt)
  - [func (s *SendStmt) End() token.Pos](#SendStmt.End)
  - [func (s *SendStmt) Pos() token.Pos](#SendStmt.Pos)
- [type SliceExpr](#SliceExpr)
  - [func (x *SliceExpr) End() token.Pos](#SliceExpr.End)
  - [func (x *SliceExpr) Pos() token.Pos](#SliceExpr.Pos)
- [type Spec](#Spec)
- [type StarExpr](#StarExpr)
  - [func (x *StarExpr) End() token.Pos](#StarExpr.End)
  - [func (x *StarExpr) Pos() token.Pos](#StarExpr.Pos)
- [type Stmt](#Stmt)
- [type StructType](#StructType)
  - [func (x *StructType) End() token.Pos](#StructType.End)
  - [func (x *StructType) Pos() token.Pos](#StructType.Pos)
- [type SwitchStmt](#SwitchStmt)
  - [func (s *SwitchStmt) End() token.Pos](#SwitchStmt.End)
  - [func (s *SwitchStmt) Pos() token.Pos](#SwitchStmt.Pos)
- [type TypeAssertExpr](#TypeAssertExpr)
  - [func (x *TypeAssertExpr) End() token.Pos](#TypeAssertExpr.End)
  - [func (x *TypeAssertExpr) Pos() token.Pos](#TypeAssertExpr.Pos)
- [type TypeSpec](#TypeSpec)
  - [func (s *TypeSpec) End() token.Pos](#TypeSpec.End)
  - [func (s *TypeSpec) Pos() token.Pos](#TypeSpec.Pos)
- [type TypeSwitchStmt](#TypeSwitchStmt)
  - [func (s *TypeSwitchStmt) End() token.Pos](#TypeSwitchStmt.End)
  - [func (s *TypeSwitchStmt) Pos() token.Pos](#TypeSwitchStmt.Pos)
- [type UnaryExpr](#UnaryExpr)
  - [func (x *UnaryExpr) End() token.Pos](#UnaryExpr.End)
  - [func (x *UnaryExpr) Pos() token.Pos](#UnaryExpr.Pos)
- [type ValueSpec](#ValueSpec)
  - [func (s *ValueSpec) End() token.Pos](#ValueSpec.End)
  - [func (s *ValueSpec) Pos() token.Pos](#ValueSpec.Pos)
- [type Visitor](#Visitor)

### Examples

- [CommentMap](#exampleCommentMap)
- [Inspect](#exampleInspect)
- [Print](#examplePrint)

### Package files
 [ast.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go) [commentmap.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/commentmap.go) [filter.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/filter.go) [import.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/import.go) [print.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/print.go) [resolve.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/resolve.go) [scope.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go) [walk.go](//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/walk.go)

<h2 id="FileExports">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/filter.go#L18">FileExports</a>
    <a href="#FileExports">¶</a></h2>
<pre>func FileExports(src *<a href="#File">File</a>) <a href="/builtin/#bool">bool</a></pre>

FileExports trims the AST for a Go source file in place such that only exported
nodes remain: all top-level identifiers which are not exported and their
associated information (such as type, initial value, or function body) are
removed. Non-exported fields and methods of exported types are stripped. The
File.Comments list is not changed.

FileExports reports whether there are exported declarations.

<h2 id="FilterDecl">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/filter.go#L194">FilterDecl</a>
    <a href="#FilterDecl">¶</a></h2>
<pre>func FilterDecl(decl <a href="#Decl">Decl</a>, f <a href="#Filter">Filter</a>) <a href="/builtin/#bool">bool</a></pre>

FilterDecl trims the AST for a Go declaration in place by removing all names
(including struct field and interface method names, but not from parameter
lists) that don't pass through the filter f.

FilterDecl reports whether there are any declared names left after filtering.

<h2 id="FilterFile">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/filter.go#L219">FilterFile</a>
    <a href="#FilterFile">¶</a></h2>
<pre>func FilterFile(src *<a href="#File">File</a>, f <a href="#Filter">Filter</a>) <a href="/builtin/#bool">bool</a></pre>

FilterFile trims the AST for a Go file in place by removing all names from
top-level declarations (including struct field and interface method names, but
not from parameter lists) that don't pass through the filter f. If the
declaration is empty afterwards, the declaration is removed from the AST. Import
declarations are always removed. The File.Comments list is not changed.

FilterFile reports whether there are any top-level declarations left after
filtering.

<h2 id="FilterPackage">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/filter.go#L246">FilterPackage</a>
    <a href="#FilterPackage">¶</a></h2>
<pre>func FilterPackage(pkg *<a href="#Package">Package</a>, f <a href="#Filter">Filter</a>) <a href="/builtin/#bool">bool</a></pre>

FilterPackage trims the AST for a Go package in place by removing all names from
top-level declarations (including struct field and interface method names, but
not from parameter lists) that don't pass through the filter f. If the
declaration is empty afterwards, the declaration is removed from the AST. The
pkg.Files list is not changed, so that file names and top-level package comments
don't get lost.

FilterPackage reports whether there are any top-level declarations left after
filtering.

<h2 id="Fprint">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/print.go#L29">Fprint</a>
    <a href="#Fprint">¶</a></h2>
<pre>func Fprint(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, x interface{}, f <a href="#FieldFilter">FieldFilter</a>) <a href="/builtin/#error">error</a></pre>

Fprint prints the (sub-)tree starting at AST node x to w. If fset != nil,
position information is interpreted relative to that file set. Otherwise
positions are printed as integer values (file set specific offsets).

A non-nil FieldFilter f may be provided to control the output: struct fields for
which f(fieldname, fieldvalue) is true are printed; all others are filtered from
the output. Unexported struct fields are never printed.

<h2 id="Inspect">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/walk.go#L374">Inspect</a>
    <a href="#Inspect">¶</a></h2>
<pre>func Inspect(node <a href="#Node">Node</a>, f func(<a href="#Node">Node</a>) <a href="/builtin/#bool">bool</a>)</pre>

Inspect traverses an AST in depth-first order: It starts by calling f(node);
node must not be nil. If f returns true, Inspect invokes f recursively for each
of the non-nil children of node, followed by a call of f(nil).

<a id="exampleInspect"></a>
Example:

    // src is the input for which we want to inspect the AST.
    src := `
    package p
    const c = 1.0
    var X = f(3.14)*2 + c
    `

    // Create the AST by parsing src.
    fset := token.NewFileSet() // positions are relative to fset
    f, err := parser.ParseFile(fset, "src.go", src, 0)
    if err != nil {
        panic(err)
    }

    // Inspect the AST and print all identifiers and literals.
    ast.Inspect(f, func(n ast.Node) bool {
        var s string
        switch x := n.(type) {
        case *ast.BasicLit:
            s = x.Value
        case *ast.Ident:
            s = x.Name
        }
        if s != "" {
            fmt.Printf("%s:\t%s\n", fset.Position(n.Pos()), s)
        }
        return true
    })

    // Output:
    // src.go:2:9:	p
    // src.go:3:7:	c
    // src.go:3:11:	1.0
    // src.go:4:5:	X
    // src.go:4:9:	f
    // src.go:4:11:	3.14
    // src.go:4:17:	2
    // src.go:4:21:	c

<h2 id="IsExported">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L516">IsExported</a>
    <a href="#IsExported">¶</a></h2>
<pre>func IsExported(name <a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>

IsExported reports whether name is an exported Go symbol (that is, whether it
begins with an upper-case letter).

<h2 id="NotNilFilter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/print.go#L12">NotNilFilter</a>
    <a href="#NotNilFilter">¶</a></h2>
<pre>func NotNilFilter(_ <a href="/builtin/#string">string</a>, v <a href="/reflect/">reflect</a>.<a href="/reflect/#Value">Value</a>) <a href="/builtin/#bool">bool</a></pre>

NotNilFilter returns true for field values that are not nil; it returns false
otherwise.

<h2 id="PackageExports">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/filter.go#L29">PackageExports</a>
    <a href="#PackageExports">¶</a></h2>
<pre>func PackageExports(pkg *<a href="#Package">Package</a>) <a href="/builtin/#bool">bool</a></pre>

PackageExports trims the AST for a Go package in place such that only exported
nodes remain. The pkg.Files list is not changed, so that file names and
top-level package comments don't get lost.

PackageExports reports whether there are exported declarations; it returns false
otherwise.

<h2 id="Print">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/print.go#L63">Print</a>
    <a href="#Print">¶</a></h2>
<pre>func Print(fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, x interface{}) <a href="/builtin/#error">error</a></pre>

Print prints x to standard output, skipping nil fields. Print(fset, x) is the
same as Fprint(os.Stdout, fset, x, NotNilFilter).

<a id="examplePrint"></a>
Example:

    // src is the input for which we want to print the AST.
    src := `
    package main
    func main() {
    	println("Hello, World!")
    }
    `

    // Create the AST by parsing src.
    fset := token.NewFileSet() // positions are relative to fset
    f, err := parser.ParseFile(fset, "", src, 0)
    if err != nil {
        panic(err)
    }

    // Print the AST.
    ast.Print(fset, f)

    // Output:
    //      0  *ast.File {
    //      1  .  Package: 2:1
    //      2  .  Name: *ast.Ident {
    //      3  .  .  NamePos: 2:9
    //      4  .  .  Name: "main"
    //      5  .  }
    //      6  .  Decls: []ast.Decl (len = 1) {
    //      7  .  .  0: *ast.FuncDecl {
    //      8  .  .  .  Name: *ast.Ident {
    //      9  .  .  .  .  NamePos: 3:6
    //     10  .  .  .  .  Name: "main"
    //     11  .  .  .  .  Obj: *ast.Object {
    //     12  .  .  .  .  .  Kind: func
    //     13  .  .  .  .  .  Name: "main"
    //     14  .  .  .  .  .  Decl: *(obj @ 7)
    //     15  .  .  .  .  }
    //     16  .  .  .  }
    //     17  .  .  .  Type: *ast.FuncType {
    //     18  .  .  .  .  Func: 3:1
    //     19  .  .  .  .  Params: *ast.FieldList {
    //     20  .  .  .  .  .  Opening: 3:10
    //     21  .  .  .  .  .  Closing: 3:11
    //     22  .  .  .  .  }
    //     23  .  .  .  }
    //     24  .  .  .  Body: *ast.BlockStmt {
    //     25  .  .  .  .  Lbrace: 3:13
    //     26  .  .  .  .  List: []ast.Stmt (len = 1) {
    //     27  .  .  .  .  .  0: *ast.ExprStmt {
    //     28  .  .  .  .  .  .  X: *ast.CallExpr {
    //     29  .  .  .  .  .  .  .  Fun: *ast.Ident {
    //     30  .  .  .  .  .  .  .  .  NamePos: 4:2
    //     31  .  .  .  .  .  .  .  .  Name: "println"
    //     32  .  .  .  .  .  .  .  }
    //     33  .  .  .  .  .  .  .  Lparen: 4:9
    //     34  .  .  .  .  .  .  .  Args: []ast.Expr (len = 1) {
    //     35  .  .  .  .  .  .  .  .  0: *ast.BasicLit {
    //     36  .  .  .  .  .  .  .  .  .  ValuePos: 4:10
    //     37  .  .  .  .  .  .  .  .  .  Kind: STRING
    //     38  .  .  .  .  .  .  .  .  .  Value: "\"Hello, World!\""
    //     39  .  .  .  .  .  .  .  .  }
    //     40  .  .  .  .  .  .  .  }
    //     41  .  .  .  .  .  .  .  Ellipsis: -
    //     42  .  .  .  .  .  .  .  Rparen: 4:25
    //     43  .  .  .  .  .  .  }
    //     44  .  .  .  .  .  }
    //     45  .  .  .  .  }
    //     46  .  .  .  .  Rbrace: 5:1
    //     47  .  .  .  }
    //     48  .  .  }
    //     49  .  }
    //     50  .  Scope: *ast.Scope {
    //     51  .  .  Objects: map[string]*ast.Object (len = 1) {
    //     52  .  .  .  "main": *(obj @ 11)
    //     53  .  .  }
    //     54  .  }
    //     55  .  Unresolved: []*ast.Ident (len = 1) {
    //     56  .  .  0: *(obj @ 29)
    //     57  .  }
    //     58  }

<h2 id="SortImports">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/import.go#L5">SortImports</a>
    <a href="#SortImports">¶</a></h2>
<pre>func SortImports(fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, f *<a href="#File">File</a>)</pre>

SortImports sorts runs of consecutive import lines in import blocks in f. It
also removes duplicate imports when it is possible to do so without data loss.

<h2 id="Walk">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/walk.go#L41">Walk</a>
    <a href="#Walk">¶</a></h2>
<pre>func Walk(v <a href="#Visitor">Visitor</a>, node <a href="#Node">Node</a>)</pre>

Walk traverses an AST in depth-first order: It starts by calling v.Visit(node);
node must not be nil. If the visitor w returned by v.Visit(node) is not nil,
Walk is invoked recursively with visitor w for each of the non-nil children of
node, followed by a call of w.Visit(nil).

<h2 id="ArrayType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L365">ArrayType</a>
    <a href="#ArrayType">¶</a></h2>
<pre>type ArrayType struct {
<span id="ArrayType.Lbrack"></span>    Lbrack <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;[&#34;</span>
<span id="ArrayType.Len"></span>    Len    <a href="#Expr">Expr</a>      <span class="comment">// Ellipsis node for [...]T array types, nil for slice types</span>
<span id="ArrayType.Elt"></span>    Elt    <a href="#Expr">Expr</a>      <span class="comment">// element type</span>
}</pre>

An ArrayType node represents an array or slice type.

<h3 id="ArrayType.End">func (*ArrayType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L466">End</a>
    <a href="#ArrayType.End">¶</a></h3>
<pre>func (x *<a href="#ArrayType">ArrayType</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="ArrayType.Pos">func (*ArrayType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L433">Pos</a>
    <a href="#ArrayType.Pos">¶</a></h3>
<pre>func (x *<a href="#ArrayType">ArrayType</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="AssignStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L593">AssignStmt</a>
    <a href="#AssignStmt">¶</a></h2>
<pre>type AssignStmt struct {
<span id="AssignStmt.Lhs"></span>    Lhs    []<a href="#Expr">Expr</a>
<span id="AssignStmt.TokPos"></span>    TokPos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>   <span class="comment">// position of Tok</span>
<span id="AssignStmt.Tok"></span>    Tok    <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a> <span class="comment">// assignment token, DEFINE</span>
<span id="AssignStmt.Rhs"></span>    Rhs    []<a href="#Expr">Expr</a>
}</pre>

An AssignStmt node represents an assignment or a short variable declaration.

<h3 id="AssignStmt.End">func (*AssignStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L739">End</a>
    <a href="#AssignStmt.End">¶</a></h3>
<pre>func (s *<a href="#AssignStmt">AssignStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="AssignStmt.Pos">func (*AssignStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L710">Pos</a>
    <a href="#AssignStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#AssignStmt">AssignStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="BadDecl">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L890">BadDecl</a>
    <a href="#BadDecl">¶</a></h2>
<pre>type BadDecl struct {
<span id="BadDecl.From"></span>    From, To <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position range of bad declaration</span>
}</pre>

A BadDecl node is a placeholder for declarations containing syntax errors for
which no correct declaration nodes can be created.

<h3 id="BadDecl.End">func (*BadDecl) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L930">End</a>
    <a href="#BadDecl.End">¶</a></h3>
<pre>func (d *<a href="#BadDecl">BadDecl</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="BadDecl.Pos">func (*BadDecl) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L926">Pos</a>
    <a href="#BadDecl.Pos">¶</a></h3>
<pre>func (d *<a href="#BadDecl">BadDecl</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="BadExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L223">BadExpr</a>
    <a href="#BadExpr">¶</a></h2>
<pre>type BadExpr struct {
<span id="BadExpr.From"></span>    From, To <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position range of bad expression</span>
}</pre>

A BadExpr node is a placeholder for expressions containing syntax errors for
which no correct expression nodes can be created.

<h3 id="BadExpr.End">func (*BadExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L445">End</a>
    <a href="#BadExpr.End">¶</a></h3>
<pre>func (x *<a href="#BadExpr">BadExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="BadExpr.Pos">func (*BadExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L412">Pos</a>
    <a href="#BadExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#BadExpr">BadExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="BadStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L544">BadStmt</a>
    <a href="#BadStmt">¶</a></h2>
<pre>type BadStmt struct {
<span id="BadStmt.From"></span>    From, To <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position range of bad statement</span>
}</pre>

A BadStmt node is a placeholder for statements containing syntax errors for
which no correct statement nodes can be created.

<h3 id="BadStmt.End">func (*BadStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L725">End</a>
    <a href="#BadStmt.End">¶</a></h3>
<pre>func (s *<a href="#BadStmt">BadStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="BadStmt.Pos">func (*BadStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L703">Pos</a>
    <a href="#BadStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#BadStmt">BadStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="BasicLit">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L243">BasicLit</a>
    <a href="#BasicLit">¶</a></h2>
<pre>type BasicLit struct {
<span id="BasicLit.ValuePos"></span>    ValuePos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>   <span class="comment">// literal position</span>
<span id="BasicLit.Kind"></span>    Kind     <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a> <span class="comment">// token.INT, token.FLOAT, token.IMAG, token.CHAR, or token.STRING</span>
<span id="BasicLit.Value"></span>    Value    <a href="/builtin/#string">string</a>      <span class="comment">// literal string; e.g. 42, 0x7f, 3.14, 1e-9, 2.4i, &#39;a&#39;, &#39;\x7f&#39;, &#34;foo&#34; or `\m\n\o`</span>
}</pre>

A BasicLit node represents a literal of basic type.

<h3 id="BasicLit.End">func (*BasicLit) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L453">End</a>
    <a href="#BasicLit.End">¶</a></h3>
<pre>func (x *<a href="#BasicLit">BasicLit</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="BasicLit.Pos">func (*BasicLit) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L415">Pos</a>
    <a href="#BasicLit.Pos">¶</a></h3>
<pre>func (x *<a href="#BasicLit">BasicLit</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="BinaryExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L332">BinaryExpr</a>
    <a href="#BinaryExpr">¶</a></h2>
<pre>type BinaryExpr struct {
<span id="BinaryExpr.X"></span>    X     <a href="#Expr">Expr</a>        <span class="comment">// left operand</span>
<span id="BinaryExpr.OpPos"></span>    OpPos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>   <span class="comment">// position of Op</span>
<span id="BinaryExpr.Op"></span>    Op    <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a> <span class="comment">// operator</span>
<span id="BinaryExpr.Y"></span>    Y     <a href="#Expr">Expr</a>        <span class="comment">// right operand</span>
}</pre>

A BinaryExpr node represents a binary expression.

<h3 id="BinaryExpr.End">func (*BinaryExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L464">End</a>
    <a href="#BinaryExpr.End">¶</a></h3>
<pre>func (x *<a href="#BinaryExpr">BinaryExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="BinaryExpr.Pos">func (*BinaryExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L431">Pos</a>
    <a href="#BinaryExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#BinaryExpr">BinaryExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="BlockStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L628">BlockStmt</a>
    <a href="#BlockStmt">¶</a></h2>
<pre>type BlockStmt struct {
<span id="BlockStmt.Lbrace"></span>    Lbrace <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;{&#34;</span>
<span id="BlockStmt.List"></span>    List   []<a href="#Stmt">Stmt</a>
<span id="BlockStmt.Rbrace"></span>    Rbrace <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;}&#34;</span>
}</pre>

A BlockStmt node represents a braced statement list.

<h3 id="BlockStmt.End">func (*BlockStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L754">End</a>
    <a href="#BlockStmt.End">¶</a></h3>
<pre>func (s *<a href="#BlockStmt">BlockStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="BlockStmt.Pos">func (*BlockStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L715">Pos</a>
    <a href="#BlockStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#BlockStmt">BlockStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="BranchStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L621">BranchStmt</a>
    <a href="#BranchStmt">¶</a></h2>
<pre>type BranchStmt struct {
<span id="BranchStmt.TokPos"></span>    TokPos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>   <span class="comment">// position of Tok</span>
<span id="BranchStmt.Tok"></span>    Tok    <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a> <span class="comment">// keyword token (BREAK, CONTINUE, GOTO, FALLTHROUGH)</span>
<span id="BranchStmt.Label"></span>    Label  *<a href="#Ident">Ident</a>      <span class="comment">// label name; or nil</span>
}</pre>

A BranchStmt node represents a break, continue, goto, or fallthrough statement.

<h3 id="BranchStmt.End">func (*BranchStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L748">End</a>
    <a href="#BranchStmt.End">¶</a></h3>
<pre>func (s *<a href="#BranchStmt">BranchStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="BranchStmt.Pos">func (*BranchStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L714">Pos</a>
    <a href="#BranchStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#BranchStmt">BranchStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="CallExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L306">CallExpr</a>
    <a href="#CallExpr">¶</a></h2>
<pre>type CallExpr struct {
<span id="CallExpr.Fun"></span>    Fun      <a href="#Expr">Expr</a>      <span class="comment">// function expression</span>
<span id="CallExpr.Lparen"></span>    Lparen   <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;(&#34;</span>
<span id="CallExpr.Args"></span>    Args     []<a href="#Expr">Expr</a>    <span class="comment">// function arguments; or nil</span>
<span id="CallExpr.Ellipsis"></span>    Ellipsis <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;...&#34; (token.NoPos if there is no &#34;...&#34;)</span>
<span id="CallExpr.Rparen"></span>    Rparen   <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;)&#34;</span>
}</pre>

A CallExpr node represents an expression followed by an argument list.

<h3 id="CallExpr.End">func (*CallExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L461">End</a>
    <a href="#CallExpr.End">¶</a></h3>
<pre>func (x *<a href="#CallExpr">CallExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="CallExpr.Pos">func (*CallExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L428">Pos</a>
    <a href="#CallExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#CallExpr">CallExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="CaseClause">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L644">CaseClause</a>
    <a href="#CaseClause">¶</a></h2>
<pre>type CaseClause struct {
<span id="CaseClause.Case"></span>    Case  <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;case&#34; or &#34;default&#34; keyword</span>
<span id="CaseClause.List"></span>    List  []<a href="#Expr">Expr</a>    <span class="comment">// list of expressions or types; nil means default case</span>
<span id="CaseClause.Colon"></span>    Colon <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;:&#34;</span>
<span id="CaseClause.Body"></span>    Body  []<a href="#Stmt">Stmt</a>    <span class="comment">// statement list; or nil</span>
}</pre>

A CaseClause represents a case of an expression or type switch statement.

<h3 id="CaseClause.End">func (*CaseClause) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L761">End</a>
    <a href="#CaseClause.End">¶</a></h3>
<pre>func (s *<a href="#CaseClause">CaseClause</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="CaseClause.Pos">func (*CaseClause) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L717">Pos</a>
    <a href="#CaseClause.Pos">¶</a></h3>
<pre>func (s *<a href="#CaseClause">CaseClause</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="ChanDir">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L352">ChanDir</a>
    <a href="#ChanDir">¶</a></h2>
<pre>type ChanDir <a href="/builtin/#int">int</a></pre>

The direction of a channel type is indicated by a bit mask including one or both
of the following constants.

<pre>const (
    <span id="SEND">SEND</span> <a href="#ChanDir">ChanDir</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a>
    <span id="RECV">RECV</span>
)</pre>


<h2 id="ChanType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L402">ChanType</a>
    <a href="#ChanType">¶</a></h2>
<pre>type ChanType struct {
<span id="ChanType.Begin"></span>    Begin <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;chan&#34; keyword or &#34;&lt;-&#34; (whichever comes first)</span>
<span id="ChanType.Arrow"></span>    Arrow <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;&lt;-&#34; (token.NoPos if there is no &#34;&lt;-&#34;)</span>
<span id="ChanType.Dir"></span>    Dir   <a href="#ChanDir">ChanDir</a>   <span class="comment">// channel direction</span>
<span id="ChanType.Value"></span>    Value <a href="#Expr">Expr</a>      <span class="comment">// value type</span>
}</pre>

A ChanType node represents a channel type.

<h3 id="ChanType.End">func (*ChanType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L476">End</a>
    <a href="#ChanType.End">¶</a></h3>
<pre>func (x *<a href="#ChanType">ChanType</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="ChanType.Pos">func (*ChanType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L443">Pos</a>
    <a href="#ChanType.Pos">¶</a></h3>
<pre>func (x *<a href="#ChanType">ChanType</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="CommClause">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L668">CommClause</a>
    <a href="#CommClause">¶</a></h2>
<pre>type CommClause struct {
<span id="CommClause.Case"></span>    Case  <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;case&#34; or &#34;default&#34; keyword</span>
<span id="CommClause.Comm"></span>    Comm  <a href="#Stmt">Stmt</a>      <span class="comment">// send or receive statement; nil means default case</span>
<span id="CommClause.Colon"></span>    Colon <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;:&#34;</span>
<span id="CommClause.Body"></span>    Body  []<a href="#Stmt">Stmt</a>    <span class="comment">// statement list; or nil</span>
}</pre>

A CommClause node represents a case of a select statement.

<h3 id="CommClause.End">func (*CommClause) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L769">End</a>
    <a href="#CommClause.End">¶</a></h3>
<pre>func (s *<a href="#CommClause">CommClause</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="CommClause.Pos">func (*CommClause) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L720">Pos</a>
    <a href="#CommClause.Pos">¶</a></h3>
<pre>func (s *<a href="#CommClause">CommClause</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Comment">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L52">Comment</a>
    <a href="#Comment">¶</a></h2>
<pre>type Comment struct {
<span id="Comment.Slash"></span>    Slash <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;/&#34; starting the comment</span>
<span id="Comment.Text"></span>    Text  <a href="/builtin/#string">string</a>    <span class="comment">// comment text (excluding &#39;\n&#39; for //-style comments)</span>
}</pre>

A Comment node represents a single //-style or /*-style comment.

<h3 id="Comment.End">func (*Comment) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L58">End</a>
    <a href="#Comment.End">¶</a></h3>
<pre>func (c *<a href="#Comment">Comment</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Comment.Pos">func (*Comment) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L57">Pos</a>
    <a href="#Comment.Pos">¶</a></h3>
<pre>func (c *<a href="#Comment">Comment</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="CommentGroup">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L63">CommentGroup</a>
    <a href="#CommentGroup">¶</a></h2>
<pre>type CommentGroup struct {
<span id="CommentGroup.List"></span>    List []*<a href="#Comment">Comment</a> <span class="comment">// len(List) &gt; 0</span>
}</pre>

A CommentGroup represents a sequence of comments with no other tokens and no
empty lines between.

<h3 id="CommentGroup.End">func (*CommentGroup) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L68">End</a>
    <a href="#CommentGroup.End">¶</a></h3>
<pre>func (g *<a href="#CommentGroup">CommentGroup</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="CommentGroup.Pos">func (*CommentGroup) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L67">Pos</a>
    <a href="#CommentGroup.Pos">¶</a></h3>
<pre>func (g *<a href="#CommentGroup">CommentGroup</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="CommentGroup.Text">func (*CommentGroup) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L86">Text</a>
    <a href="#CommentGroup.Text">¶</a></h3>
<pre>func (g *<a href="#CommentGroup">CommentGroup</a>) Text() <a href="/builtin/#string">string</a></pre>

Text returns the text of the comment. Comment markers (//, /*, and */), the
first space of a line comment, and leading and trailing empty lines are removed.
Multiple empty lines are reduced to one, and trailing space on lines is trimmed.
Unless the result is empty, it is newline-terminated.

<h2 id="CommentMap">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/commentmap.go#L25">CommentMap</a>
    <a href="#CommentMap">¶</a></h2>
<pre>type CommentMap map[<a href="#Node">Node</a>][]*<a href="#CommentGroup">CommentGroup</a></pre>

A CommentMap maps an AST node to a list of comment groups associated with it.
See NewCommentMap for a description of the association.

<a id="exampleCommentMap"></a>
Example:

    // src is the input for which we create the AST that we
    // are going to manipulate.
    src := `
    // This is the package comment.
    package main
    
    // This comment is associated with the hello constant.
    const hello = "Hello, World!" // line comment 1
    
    // This comment is associated with the foo variable.
    var foo = hello // line comment 2 
    
    // This comment is associated with the main function.
    func main() {
    	fmt.Println(hello) // line comment 3
    }
    `

    // Create the AST by parsing src.
    fset := token.NewFileSet() // positions are relative to fset
    f, err := parser.ParseFile(fset, "src.go", src, parser.ParseComments)
    if err != nil {
        panic(err)
    }

    // Create an ast.CommentMap from the ast.File's comments.
    // This helps keeping the association between comments
    // and AST nodes.
    cmap := ast.NewCommentMap(fset, f, f.Comments)

    // Remove the first variable declaration from the list of declarations.
    for i, decl := range f.Decls {
        if gen, ok := decl.(*ast.GenDecl); ok && gen.Tok == token.VAR {
            copy(f.Decls[i:], f.Decls[i+1:])
            f.Decls = f.Decls[:len(f.Decls)-1]
        }
    }

    // Use the comment map to filter comments that don't belong anymore
    // (the comments associated with the variable declaration), and create
    // the new comments list.
    f.Comments = cmap.Filter(f).Comments()

    // Print the modified AST.
    var buf bytes.Buffer
    if err := format.Node(&buf, fset, f); err != nil {
        panic(err)
    }
    fmt.Printf("%s", buf.Bytes())

    // Output:
    // // This is the package comment.
    // package main
    //
    // // This comment is associated with the hello constant.
    // const hello = "Hello, World!" // line comment 1
    //
    // // This comment is associated with the main function.
    // func main() {
    // 	fmt.Println(hello) // line comment 3
    // }

<h3 id="NewCommentMap">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/commentmap.go#L133">NewCommentMap</a>
    <a href="#NewCommentMap">¶</a></h3>
<pre>func NewCommentMap(fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, node <a href="#Node">Node</a>, comments []*<a href="#CommentGroup">CommentGroup</a>) <a href="#CommentMap">CommentMap</a></pre>

NewCommentMap creates a new comment map by associating comment groups of the
comments list with the nodes of the AST specified by node.

A comment group g is associated with a node n if:

    - g starts on the same line as n ends
    - g starts on the line immediately following n, and there is
      at least one empty line after g and before the next node
    - g starts before n and is not associated to the node before n
      via the previous rules

NewCommentMap tries to associate a comment group to the "largest" node possible:
For instance, if the comment is a line comment trailing an assignment, the
comment is associated with the entire assignment rather than just the last
operand in the assignment.

<h3 id="CommentMap.Comments">func (CommentMap) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/commentmap.go#L262">Comments</a>
    <a href="#CommentMap.Comments">¶</a></h3>
<pre>func (cmap <a href="#CommentMap">CommentMap</a>) Comments() []*<a href="#CommentGroup">CommentGroup</a></pre>

Comments returns the list of comment groups in the comment map. The result is
sorted in source order.

<h3 id="CommentMap.Filter">func (CommentMap) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/commentmap.go#L248">Filter</a>
    <a href="#CommentMap.Filter">¶</a></h3>
<pre>func (cmap <a href="#CommentMap">CommentMap</a>) Filter(node <a href="#Node">Node</a>) <a href="#CommentMap">CommentMap</a></pre>

Filter returns a new comment map consisting of only those entries of cmap for
which a corresponding node exists in the AST specified by node.

<h3 id="CommentMap.String">func (CommentMap) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/commentmap.go#L307">String</a>
    <a href="#CommentMap.String">¶</a></h3>
<pre>func (cmap <a href="#CommentMap">CommentMap</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="CommentMap.Update">func (CommentMap) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/commentmap.go#L236">Update</a>
    <a href="#CommentMap.Update">¶</a></h3>
<pre>func (cmap <a href="#CommentMap">CommentMap</a>) Update(old, new <a href="#Node">Node</a>) <a href="#Node">Node</a></pre>

Update replaces an old node in the comment map with the new node and returns the
new node. Comments that were associated with the old node are associated with
the new node.

<h2 id="CompositeLit">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L256">CompositeLit</a>
    <a href="#CompositeLit">¶</a></h2>
<pre>type CompositeLit struct {
<span id="CompositeLit.Type"></span>    Type   <a href="#Expr">Expr</a>      <span class="comment">// literal type; or nil</span>
<span id="CompositeLit.Lbrace"></span>    Lbrace <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;{&#34;</span>
<span id="CompositeLit.Elts"></span>    Elts   []<a href="#Expr">Expr</a>    <span class="comment">// list of composite elements; or nil</span>
<span id="CompositeLit.Rbrace"></span>    Rbrace <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;}&#34;</span>
}</pre>

A CompositeLit node represents a composite literal.

<h3 id="CompositeLit.End">func (*CompositeLit) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L455">End</a>
    <a href="#CompositeLit.End">¶</a></h3>
<pre>func (x *<a href="#CompositeLit">CompositeLit</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="CompositeLit.Pos">func (*CompositeLit) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L417">Pos</a>
    <a href="#CompositeLit.Pos">¶</a></h3>
<pre>func (x *<a href="#CompositeLit">CompositeLit</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Decl">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L43">Decl</a>
    <a href="#Decl">¶</a></h2>
<pre>type Decl interface {
    <a href="#Node">Node</a>
    <span class="comment">// contains filtered or unexported methods</span>
}</pre>

All declaration nodes implement the Decl interface.

<h2 id="DeclStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L549">DeclStmt</a>
    <a href="#DeclStmt">¶</a></h2>
<pre>type DeclStmt struct {
<span id="DeclStmt.Decl"></span>    Decl <a href="#Decl">Decl</a> <span class="comment">// *GenDecl with CONST, TYPE, or VAR token</span>
}</pre>

A DeclStmt node represents a declaration in a statement list.

<h3 id="DeclStmt.End">func (*DeclStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L726">End</a>
    <a href="#DeclStmt.End">¶</a></h3>
<pre>func (s *<a href="#DeclStmt">DeclStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="DeclStmt.Pos">func (*DeclStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L704">Pos</a>
    <a href="#DeclStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#DeclStmt">DeclStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="DeferStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L607">DeferStmt</a>
    <a href="#DeferStmt">¶</a></h2>
<pre>type DeferStmt struct {
<span id="DeferStmt.Defer"></span>    Defer <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;defer&#34; keyword</span>
<span id="DeferStmt.Call"></span>    Call  *<a href="#CallExpr">CallExpr</a>
}</pre>

A DeferStmt node represents a defer statement.

<h3 id="DeferStmt.End">func (*DeferStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L741">End</a>
    <a href="#DeferStmt.End">¶</a></h3>
<pre>func (s *<a href="#DeferStmt">DeferStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="DeferStmt.Pos">func (*DeferStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L712">Pos</a>
    <a href="#DeferStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#DeferStmt">DeferStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Ellipsis">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L237">Ellipsis</a>
    <a href="#Ellipsis">¶</a></h2>
<pre>type Ellipsis struct {
<span id="Ellipsis.Ellipsis"></span>    Ellipsis <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;...&#34;</span>
<span id="Ellipsis.Elt"></span>    Elt      <a href="#Expr">Expr</a>      <span class="comment">// ellipsis element type (parameter lists only); or nil</span>
}</pre>

An Ellipsis node stands for the "..." type in a parameter list or the "..."
length in an array type.

<h3 id="Ellipsis.End">func (*Ellipsis) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L447">End</a>
    <a href="#Ellipsis.End">¶</a></h3>
<pre>func (x *<a href="#Ellipsis">Ellipsis</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Ellipsis.Pos">func (*Ellipsis) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L414">Pos</a>
    <a href="#Ellipsis.Pos">¶</a></h3>
<pre>func (x *<a href="#Ellipsis">Ellipsis</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="EmptyStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L557">EmptyStmt</a>
    <a href="#EmptyStmt">¶</a></h2>
<pre>type EmptyStmt struct {
<span id="EmptyStmt.Semicolon"></span>    Semicolon <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of following &#34;;&#34;</span>
<span id="EmptyStmt.Implicit"></span>    Implicit  <a href="/builtin/#bool">bool</a>      <span class="comment">// if set, &#34;;&#34; was omitted in the source</span>
}</pre>

An EmptyStmt node represents an empty statement. The "position" of the empty
statement is the position of the immediately following (explicit or implicit)
semicolon.

<h3 id="EmptyStmt.End">func (*EmptyStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L727">End</a>
    <a href="#EmptyStmt.End">¶</a></h3>
<pre>func (s *<a href="#EmptyStmt">EmptyStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="EmptyStmt.Pos">func (*EmptyStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L705">Pos</a>
    <a href="#EmptyStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#EmptyStmt">EmptyStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Expr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L31">Expr</a>
    <a href="#Expr">¶</a></h2>
<pre>type Expr interface {
    <a href="#Node">Node</a>
    <span class="comment">// contains filtered or unexported methods</span>
}</pre>

All expression nodes implement the Expr interface.

<h2 id="ExprStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L572">ExprStmt</a>
    <a href="#ExprStmt">¶</a></h2>
<pre>type ExprStmt struct {
<span id="ExprStmt.X"></span>    X <a href="#Expr">Expr</a> <span class="comment">// expression</span>
}</pre>

An ExprStmt node represents a (stand-alone) expression in a statement list.

<h3 id="ExprStmt.End">func (*ExprStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L734">End</a>
    <a href="#ExprStmt.End">¶</a></h3>
<pre>func (s *<a href="#ExprStmt">ExprStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="ExprStmt.Pos">func (*ExprStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L707">Pos</a>
    <a href="#ExprStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#ExprStmt">ExprStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Field">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L147">Field</a>
    <a href="#Field">¶</a></h2>
<pre>type Field struct {
<span id="Field.Doc"></span>    Doc     *<a href="#CommentGroup">CommentGroup</a> <span class="comment">// associated documentation; or nil</span>
<span id="Field.Names"></span>    Names   []*<a href="#Ident">Ident</a>      <span class="comment">// field/method/parameter names; or nil if anonymous field</span>
<span id="Field.Type"></span>    Type    <a href="#Expr">Expr</a>          <span class="comment">// field/method/parameter type</span>
<span id="Field.Tag"></span>    Tag     *<a href="#BasicLit">BasicLit</a>     <span class="comment">// field tag; or nil</span>
<span id="Field.Comment"></span>    Comment *<a href="#CommentGroup">CommentGroup</a> <span class="comment">// line comments; or nil</span>
}</pre>

A Field represents a Field declaration list in a struct type, a method list in
an interface type, or a parameter/result declaration in a signature.

<h3 id="Field.End">func (*Field) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L162">End</a>
    <a href="#Field.End">¶</a></h3>
<pre>func (f *<a href="#Field">Field</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Field.Pos">func (*Field) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L155">Pos</a>
    <a href="#Field.Pos">¶</a></h3>
<pre>func (f *<a href="#Field">Field</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="FieldFilter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/print.go#L8">FieldFilter</a>
    <a href="#FieldFilter">¶</a></h2>
<pre>type FieldFilter func(name <a href="/builtin/#string">string</a>, value <a href="/reflect/">reflect</a>.<a href="/reflect/#Value">Value</a>) <a href="/builtin/#bool">bool</a></pre>

A FieldFilter may be provided to Fprint to control the output.

<h2 id="FieldList">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L170">FieldList</a>
    <a href="#FieldList">¶</a></h2>
<pre>type FieldList struct {
<span id="FieldList.Opening"></span>    Opening <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of opening parenthesis/brace, if any</span>
<span id="FieldList.List"></span>    List    []*<a href="#Field">Field</a>  <span class="comment">// field list; or nil</span>
<span id="FieldList.Closing"></span>    Closing <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of closing parenthesis/brace, if any</span>
}</pre>

A FieldList represents a list of Fields, enclosed by parentheses or braces.

<h3 id="FieldList.End">func (*FieldList) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L188">End</a>
    <a href="#FieldList.End">¶</a></h3>
<pre>func (f *<a href="#FieldList">FieldList</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="FieldList.NumFields">func (*FieldList) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L201">NumFields</a>
    <a href="#FieldList.NumFields">¶</a></h3>
<pre>func (f *<a href="#FieldList">FieldList</a>) NumFields() <a href="/builtin/#int">int</a></pre>

NumFields returns the number of (named and anonymous fields) in a FieldList.

<h3 id="FieldList.Pos">func (*FieldList) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L176">Pos</a>
    <a href="#FieldList.Pos">¶</a></h3>
<pre>func (f *<a href="#FieldList">FieldList</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="File">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L973">File</a>
    <a href="#File">¶</a></h2>
<pre>type File struct {
<span id="File.Doc"></span>    Doc        *<a href="#CommentGroup">CommentGroup</a>   <span class="comment">// associated documentation; or nil</span>
<span id="File.Package"></span>    Package    <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>       <span class="comment">// position of &#34;package&#34; keyword</span>
<span id="File.Name"></span>    Name       *<a href="#Ident">Ident</a>          <span class="comment">// package name</span>
<span id="File.Decls"></span>    Decls      []<a href="#Decl">Decl</a>          <span class="comment">// top-level declarations; or nil</span>
<span id="File.Scope"></span>    Scope      *<a href="#Scope">Scope</a>          <span class="comment">// package scope (this file only)</span>
<span id="File.Imports"></span>    Imports    []*<a href="#ImportSpec">ImportSpec</a>   <span class="comment">// imports in this file</span>
<span id="File.Unresolved"></span>    Unresolved []*<a href="#Ident">Ident</a>        <span class="comment">// unresolved identifiers in this file</span>
<span id="File.Comments"></span>    Comments   []*<a href="#CommentGroup">CommentGroup</a> <span class="comment">// list of all comments in the source file</span>
}</pre>

A File node represents a Go source file.

The Comments list contains all comments in the source file in order of
appearance, including the comments that are pointed to from other nodes via Doc
and Comment fields.

For correct printing of source code containing comments (using packages
go/format and go/printer), special care must be taken to update comments when a
File's syntax tree is modified: For printing, comments are interspersed between
tokens based on their position. If syntax tree nodes are removed or moved,
relevant comments in their vicinity must also be removed (from the File.Comments
list) or moved accordingly (by updating their positions). A CommentMap may be
used to facilitate some of these operations.

Whether and how a comment is associated with a node depends on the
interpretation of the syntax tree by the manipulating program: Except for Doc
and Comment comments directly associated with nodes, the remaining comments are
"free-floating" (see also issues #18593, #20744).

<h3 id="MergePackageFiles">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/filter.go#L305">MergePackageFiles</a>
    <a href="#MergePackageFiles">¶</a></h3>
<pre>func MergePackageFiles(pkg *<a href="#Package">Package</a>, mode <a href="#MergeMode">MergeMode</a>) *<a href="#File">File</a></pre>

MergePackageFiles creates a file AST by merging the ASTs of the files belonging
to a package. The mode flags control merging behavior.

<h3 id="File.End">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L985">End</a>
    <a href="#File.End">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="File.Pos">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L984">Pos</a>
    <a href="#File.Pos">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Filter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/filter.go#L36">Filter</a>
    <a href="#Filter">¶</a></h2>
<pre>type Filter func(<a href="/builtin/#string">string</a>) <a href="/builtin/#bool">bool</a></pre>


<h2 id="ForStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L682">ForStmt</a>
    <a href="#ForStmt">¶</a></h2>
<pre>type ForStmt struct {
<span id="ForStmt.For"></span>    For  <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;for&#34; keyword</span>
<span id="ForStmt.Init"></span>    Init <a href="#Stmt">Stmt</a>      <span class="comment">// initialization statement; or nil</span>
<span id="ForStmt.Cond"></span>    Cond <a href="#Expr">Expr</a>      <span class="comment">// condition; or nil</span>
<span id="ForStmt.Post"></span>    Post <a href="#Stmt">Stmt</a>      <span class="comment">// post iteration statement; or nil</span>
<span id="ForStmt.Body"></span>    Body *<a href="#BlockStmt">BlockStmt</a>
}</pre>

A ForStmt represents a for statement.

<h3 id="ForStmt.End">func (*ForStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L776">End</a>
    <a href="#ForStmt.End">¶</a></h3>
<pre>func (s *<a href="#ForStmt">ForStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="ForStmt.Pos">func (*ForStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L722">Pos</a>
    <a href="#ForStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#ForStmt">ForStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="FuncDecl">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L915">FuncDecl</a>
    <a href="#FuncDecl">¶</a></h2>
<pre>type FuncDecl struct {
<span id="FuncDecl.Doc"></span>    Doc  *<a href="#CommentGroup">CommentGroup</a> <span class="comment">// associated documentation; or nil</span>
<span id="FuncDecl.Recv"></span>    Recv *<a href="#FieldList">FieldList</a>    <span class="comment">// receiver (methods); or nil (functions)</span>
<span id="FuncDecl.Name"></span>    Name *<a href="#Ident">Ident</a>        <span class="comment">// function/method name</span>
<span id="FuncDecl.Type"></span>    Type *<a href="#FuncType">FuncType</a>     <span class="comment">// function signature: parameters, results, and position of &#34;func&#34; keyword</span>
<span id="FuncDecl.Body"></span>    Body *<a href="#BlockStmt">BlockStmt</a>    <span class="comment">// function body; or nil for external (non-Go) function</span>
}</pre>

A FuncDecl node represents a function declaration.

<h3 id="FuncDecl.End">func (*FuncDecl) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L937">End</a>
    <a href="#FuncDecl.End">¶</a></h3>
<pre>func (d *<a href="#FuncDecl">FuncDecl</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="FuncDecl.Pos">func (*FuncDecl) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L928">Pos</a>
    <a href="#FuncDecl.Pos">¶</a></h3>
<pre>func (d *<a href="#FuncDecl">FuncDecl</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="FuncLit">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L250">FuncLit</a>
    <a href="#FuncLit">¶</a></h2>
<pre>type FuncLit struct {
<span id="FuncLit.Type"></span>    Type *<a href="#FuncType">FuncType</a>  <span class="comment">// function type</span>
<span id="FuncLit.Body"></span>    Body *<a href="#BlockStmt">BlockStmt</a> <span class="comment">// function body</span>
}</pre>

A FuncLit node represents a function literal.

<h3 id="FuncLit.End">func (*FuncLit) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L454">End</a>
    <a href="#FuncLit.End">¶</a></h3>
<pre>func (x *<a href="#FuncLit">FuncLit</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="FuncLit.Pos">func (*FuncLit) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L416">Pos</a>
    <a href="#FuncLit.Pos">¶</a></h3>
<pre>func (x *<a href="#FuncLit">FuncLit</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="FuncType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L381">FuncType</a>
    <a href="#FuncType">¶</a></h2>
<pre>type FuncType struct {
<span id="FuncType.Func"></span>    Func    <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>  <span class="comment">// position of &#34;func&#34; keyword (token.NoPos if there is no &#34;func&#34;)</span>
<span id="FuncType.Params"></span>    Params  *<a href="#FieldList">FieldList</a> <span class="comment">// (incoming) parameters; non-nil</span>
<span id="FuncType.Results"></span>    Results *<a href="#FieldList">FieldList</a> <span class="comment">// (outgoing) results; or nil</span>
}</pre>

A FuncType node represents a function type.

<h3 id="FuncType.End">func (*FuncType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L468">End</a>
    <a href="#FuncType.End">¶</a></h3>
<pre>func (x *<a href="#FuncType">FuncType</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="FuncType.Pos">func (*FuncType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L435">Pos</a>
    <a href="#FuncType.Pos">¶</a></h3>
<pre>func (x *<a href="#FuncType">FuncType</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="GenDecl">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L905">GenDecl</a>
    <a href="#GenDecl">¶</a></h2>
<pre>type GenDecl struct {
<span id="GenDecl.Doc"></span>    Doc    *<a href="#CommentGroup">CommentGroup</a> <span class="comment">// associated documentation; or nil</span>
<span id="GenDecl.TokPos"></span>    TokPos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>     <span class="comment">// position of Tok</span>
<span id="GenDecl.Tok"></span>    Tok    <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a>   <span class="comment">// IMPORT, CONST, TYPE, VAR</span>
<span id="GenDecl.Lparen"></span>    Lparen <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>     <span class="comment">// position of &#39;(&#39;, if any</span>
<span id="GenDecl.Specs"></span>    Specs  []<a href="#Spec">Spec</a>
<span id="GenDecl.Rparen"></span>    Rparen <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#39;)&#39;, if any</span>
}</pre>

A GenDecl node (generic declaration node) represents an import, constant, type
or variable declaration. A valid Lparen position (Lparen.IsValid()) indicates a
parenthesized declaration.

Relationship between Tok value and Specs element type:

    token.IMPORT  *ImportSpec
    token.CONST   *ValueSpec
    token.TYPE    *TypeSpec
    token.VAR     *ValueSpec

<h3 id="GenDecl.End">func (*GenDecl) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L931">End</a>
    <a href="#GenDecl.End">¶</a></h3>
<pre>func (d *<a href="#GenDecl">GenDecl</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="GenDecl.Pos">func (*GenDecl) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L927">Pos</a>
    <a href="#GenDecl.Pos">¶</a></h3>
<pre>func (d *<a href="#GenDecl">GenDecl</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="GoStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L601">GoStmt</a>
    <a href="#GoStmt">¶</a></h2>
<pre>type GoStmt struct {
<span id="GoStmt.Go"></span>    Go   <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;go&#34; keyword</span>
<span id="GoStmt.Call"></span>    Call *<a href="#CallExpr">CallExpr</a>
}</pre>

A GoStmt node represents a go statement.

<h3 id="GoStmt.End">func (*GoStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L740">End</a>
    <a href="#GoStmt.End">¶</a></h3>
<pre>func (s *<a href="#GoStmt">GoStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="GoStmt.Pos">func (*GoStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L711">Pos</a>
    <a href="#GoStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#GoStmt">GoStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Ident">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L228">Ident</a>
    <a href="#Ident">¶</a></h2>
<pre>type Ident struct {
<span id="Ident.NamePos"></span>    NamePos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// identifier position</span>
<span id="Ident.Name"></span>    Name    <a href="/builtin/#string">string</a>    <span class="comment">// identifier name</span>
<span id="Ident.Obj"></span>    Obj     *<a href="#Object">Object</a>   <span class="comment">// denoted object; or nil</span>
}</pre>

An Ident node represents an identifier.

<h3 id="NewIdent">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L511">NewIdent</a>
    <a href="#NewIdent">¶</a></h3>
<pre>func NewIdent(name <a href="/builtin/#string">string</a>) *<a href="#Ident">Ident</a></pre>

NewIdent creates a new Ident without position. Useful for ASTs generated by code
other than the Go parser.

<h3 id="Ident.End">func (*Ident) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L446">End</a>
    <a href="#Ident.End">¶</a></h3>
<pre>func (x *<a href="#Ident">Ident</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Ident.IsExported">func (*Ident) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L524">IsExported</a>
    <a href="#Ident.IsExported">¶</a></h3>
<pre>func (id *<a href="#Ident">Ident</a>) IsExported() <a href="/builtin/#bool">bool</a></pre>

IsExported reports whether id is an exported Go symbol (that is, whether it
begins with an uppercase letter).

<h3 id="Ident.Pos">func (*Ident) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L413">Pos</a>
    <a href="#Ident.Pos">¶</a></h3>
<pre>func (x *<a href="#Ident">Ident</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Ident.String">func (*Ident) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L526">String</a>
    <a href="#Ident.String">¶</a></h3>
<pre>func (id *<a href="#Ident">Ident</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="IfStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L635">IfStmt</a>
    <a href="#IfStmt">¶</a></h2>
<pre>type IfStmt struct {
<span id="IfStmt.If"></span>    If   <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;if&#34; keyword</span>
<span id="IfStmt.Init"></span>    Init <a href="#Stmt">Stmt</a>      <span class="comment">// initialization statement; or nil</span>
<span id="IfStmt.Cond"></span>    Cond <a href="#Expr">Expr</a>      <span class="comment">// condition</span>
<span id="IfStmt.Body"></span>    Body *<a href="#BlockStmt">BlockStmt</a>
<span id="IfStmt.Else"></span>    Else <a href="#Stmt">Stmt</a> <span class="comment">// else branch; or nil</span>
}</pre>

An IfStmt node represents an if statement.

<h3 id="IfStmt.End">func (*IfStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L755">End</a>
    <a href="#IfStmt.End">¶</a></h3>
<pre>func (s *<a href="#IfStmt">IfStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="IfStmt.Pos">func (*IfStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L716">Pos</a>
    <a href="#IfStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#IfStmt">IfStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="ImportSpec">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L818">ImportSpec</a>
    <a href="#ImportSpec">¶</a></h2>
<pre>type ImportSpec struct {
<span id="ImportSpec.Doc"></span>    Doc     *<a href="#CommentGroup">CommentGroup</a> <span class="comment">// associated documentation; or nil</span>
<span id="ImportSpec.Name"></span>    Name    *<a href="#Ident">Ident</a>        <span class="comment">// local package name (including &#34;.&#34;); or nil</span>
<span id="ImportSpec.Path"></span>    Path    *<a href="#BasicLit">BasicLit</a>     <span class="comment">// import path</span>
<span id="ImportSpec.Comment"></span>    Comment *<a href="#CommentGroup">CommentGroup</a> <span class="comment">// line comments; or nil</span>
<span id="ImportSpec.EndPos"></span>    EndPos  <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>     <span class="comment">// end of spec (overrides Path.Pos if nonzero)</span>
}</pre>

An ImportSpec node represents a single package import.

<h3 id="ImportSpec.End">func (*ImportSpec) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L858">End</a>
    <a href="#ImportSpec.End">¶</a></h3>
<pre>func (s *<a href="#ImportSpec">ImportSpec</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="ImportSpec.Pos">func (*ImportSpec) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L849">Pos</a>
    <a href="#ImportSpec.Pos">¶</a></h3>
<pre>func (s *<a href="#ImportSpec">ImportSpec</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Importer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/resolve.go#L53">Importer</a>
    <a href="#Importer">¶</a></h2>
<pre>type Importer func(imports map[<a href="/builtin/#string">string</a>]*<a href="#Object">Object</a>, path <a href="/builtin/#string">string</a>) (pkg *<a href="#Object">Object</a>, err <a href="/builtin/#error">error</a>)</pre>

An Importer resolves import paths to package Objects. The imports map records
the packages already imported, indexed by package id (canonical import path). An
Importer must determine the canonical import path and check the map to see if it
is already present in the imports map. If so, the Importer can return the map
entry. Otherwise, the Importer should load the package data for the given path
into a new *Object (pkg), record pkg in the imports map, and then return pkg.

<h2 id="IncDecStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L584">IncDecStmt</a>
    <a href="#IncDecStmt">¶</a></h2>
<pre>type IncDecStmt struct {
<span id="IncDecStmt.X"></span>    X      <a href="#Expr">Expr</a>
<span id="IncDecStmt.TokPos"></span>    TokPos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>   <span class="comment">// position of Tok</span>
<span id="IncDecStmt.Tok"></span>    Tok    <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a> <span class="comment">// INC or DEC</span>
}</pre>

An IncDecStmt node represents an increment or decrement statement.

<h3 id="IncDecStmt.End">func (*IncDecStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L736">End</a>
    <a href="#IncDecStmt.End">¶</a></h3>
<pre>func (s *<a href="#IncDecStmt">IncDecStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="IncDecStmt.Pos">func (*IncDecStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L709">Pos</a>
    <a href="#IncDecStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#IncDecStmt">IncDecStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="IndexExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L277">IndexExpr</a>
    <a href="#IndexExpr">¶</a></h2>
<pre>type IndexExpr struct {
<span id="IndexExpr.X"></span>    X      <a href="#Expr">Expr</a>      <span class="comment">// expression</span>
<span id="IndexExpr.Lbrack"></span>    Lbrack <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;[&#34;</span>
<span id="IndexExpr.Index"></span>    Index  <a href="#Expr">Expr</a>      <span class="comment">// index expression</span>
<span id="IndexExpr.Rbrack"></span>    Rbrack <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;]&#34;</span>
}</pre>

An IndexExpr node represents an expression followed by an index.

<h3 id="IndexExpr.End">func (*IndexExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L458">End</a>
    <a href="#IndexExpr.End">¶</a></h3>
<pre>func (x *<a href="#IndexExpr">IndexExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="IndexExpr.Pos">func (*IndexExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L425">Pos</a>
    <a href="#IndexExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#IndexExpr">IndexExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="InterfaceType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L388">InterfaceType</a>
    <a href="#InterfaceType">¶</a></h2>
<pre>type InterfaceType struct {
<span id="InterfaceType.Interface"></span>    Interface  <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>  <span class="comment">// position of &#34;interface&#34; keyword</span>
<span id="InterfaceType.Methods"></span>    Methods    *<a href="#FieldList">FieldList</a> <span class="comment">// list of methods</span>
<span id="InterfaceType.Incomplete"></span>    Incomplete <a href="/builtin/#bool">bool</a>       <span class="comment">// true if (source) methods are missing in the Methods list</span>
}</pre>

An InterfaceType node represents an interface type.

<h3 id="InterfaceType.End">func (*InterfaceType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L474">End</a>
    <a href="#InterfaceType.End">¶</a></h3>
<pre>func (x *<a href="#InterfaceType">InterfaceType</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="InterfaceType.Pos">func (*InterfaceType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L441">Pos</a>
    <a href="#InterfaceType.Pos">¶</a></h3>
<pre>func (x *<a href="#InterfaceType">InterfaceType</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="KeyValueExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L342">KeyValueExpr</a>
    <a href="#KeyValueExpr">¶</a></h2>
<pre>type KeyValueExpr struct {
<span id="KeyValueExpr.Key"></span>    Key   <a href="#Expr">Expr</a>
<span id="KeyValueExpr.Colon"></span>    Colon <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;:&#34;</span>
<span id="KeyValueExpr.Value"></span>    Value <a href="#Expr">Expr</a>
}</pre>

A KeyValueExpr node represents (key : value) pairs in composite literals.

<h3 id="KeyValueExpr.End">func (*KeyValueExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L465">End</a>
    <a href="#KeyValueExpr.End">¶</a></h3>
<pre>func (x *<a href="#KeyValueExpr">KeyValueExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="KeyValueExpr.Pos">func (*KeyValueExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L432">Pos</a>
    <a href="#KeyValueExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#KeyValueExpr">KeyValueExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="LabeledStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L563">LabeledStmt</a>
    <a href="#LabeledStmt">¶</a></h2>
<pre>type LabeledStmt struct {
<span id="LabeledStmt.Label"></span>    Label *<a href="#Ident">Ident</a>
<span id="LabeledStmt.Colon"></span>    Colon <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;:&#34;</span>
<span id="LabeledStmt.Stmt"></span>    Stmt  <a href="#Stmt">Stmt</a>
}</pre>

A LabeledStmt node represents a labeled statement.

<h3 id="LabeledStmt.End">func (*LabeledStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L733">End</a>
    <a href="#LabeledStmt.End">¶</a></h3>
<pre>func (s *<a href="#LabeledStmt">LabeledStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="LabeledStmt.Pos">func (*LabeledStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L706">Pos</a>
    <a href="#LabeledStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#LabeledStmt">LabeledStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="MapType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L395">MapType</a>
    <a href="#MapType">¶</a></h2>
<pre>type MapType struct {
<span id="MapType.Map"></span>    Map   <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;map&#34; keyword</span>
<span id="MapType.Key"></span>    Key   <a href="#Expr">Expr</a>
<span id="MapType.Value"></span>    Value <a href="#Expr">Expr</a>
}</pre>

A MapType node represents a map type.

<h3 id="MapType.End">func (*MapType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L475">End</a>
    <a href="#MapType.End">¶</a></h3>
<pre>func (x *<a href="#MapType">MapType</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="MapType.Pos">func (*MapType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L442">Pos</a>
    <a href="#MapType.Pos">¶</a></h3>
<pre>func (x *<a href="#MapType">MapType</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="MergeMode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/filter.go#L264">MergeMode</a>
    <a href="#MergeMode">¶</a></h2>
<pre>type MergeMode <a href="/builtin/#uint">uint</a></pre>

The MergeMode flags control the behavior of MergePackageFiles.

<pre>const (
    <span class="comment">// If set, duplicate function declarations are excluded.</span>
    <span id="FilterFuncDuplicates">FilterFuncDuplicates</span> <a href="#MergeMode">MergeMode</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a>
    <span class="comment">// If set, comments that are not associated with a specific</span>
    <span class="comment">// AST node (as Doc or Comment) are excluded.</span>
    <span id="FilterUnassociatedComments">FilterUnassociatedComments</span>
    <span class="comment">// If set, duplicate import declarations are excluded.</span>
    <span id="FilterImportDuplicates">FilterImportDuplicates</span>
)</pre>


<h2 id="Node">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L25">Node</a>
    <a href="#Node">¶</a></h2>
<pre>type Node interface {
    Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of first character belonging to the node</span>
    End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of first character immediately after the node</span>
}</pre>

All node types implement the Node interface.

<h2 id="ObjKind">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go#L127">ObjKind</a>
    <a href="#ObjKind">¶</a></h2>
<pre>type ObjKind <a href="/builtin/#int">int</a></pre>

ObjKind describes what an object represents.

<pre>const (
    <span id="Bad">Bad</span> <a href="#ObjKind">ObjKind</a> = <a href="/builtin/#iota">iota</a> <span class="comment">// for error handling</span>
    <span id="Pkg">Pkg</span>                <span class="comment">// package</span>
    <span id="Con">Con</span>                <span class="comment">// constant</span>
    <span id="Typ">Typ</span>                <span class="comment">// type</span>
    <span id="Var">Var</span>                <span class="comment">// variable</span>
    <span id="Fun">Fun</span>                <span class="comment">// function or method</span>
    <span id="Lbl">Lbl</span>                <span class="comment">// label</span>
)</pre>

The list of possible Object kinds.

<h3 id="ObjKind.String">func (ObjKind) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go#L150">String</a>
    <a href="#ObjKind.String">¶</a></h3>
<pre>func (kind <a href="#ObjKind">ObjKind</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Object">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go#L66">Object</a>
    <a href="#Object">¶</a></h2>
<pre>type Object struct {
<span id="Object.Kind"></span>    Kind <a href="#ObjKind">ObjKind</a>
<span id="Object.Name"></span>    Name <a href="/builtin/#string">string</a>      <span class="comment">// declared name</span>
<span id="Object.Decl"></span>    Decl interface{} <span class="comment">// corresponding Field, XxxSpec, FuncDecl, LabeledStmt, AssignStmt, Scope; or nil</span>
<span id="Object.Data"></span>    Data interface{} <span class="comment">// object-specific data; or nil</span>
<span id="Object.Type"></span>    Type interface{} <span class="comment">// placeholder for type information; may be nil</span>
}</pre>

An Object describes a named language entity such as a package, constant, type,
variable, function (incl. methods), or label.

The Data fields contains object-specific data:

    Kind    Data type         Data value
    Pkg     *Scope            package scope
    Con     int               iota for the respective declaration

<h3 id="NewObj">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go#L75">NewObj</a>
    <a href="#NewObj">¶</a></h3>
<pre>func NewObj(kind <a href="#ObjKind">ObjKind</a>, name <a href="/builtin/#string">string</a>) *<a href="#Object">Object</a></pre>

NewObj creates a new object of a given kind and name.

<h3 id="Object.Pos">func (*Object) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go#L82">Pos</a>
    <a href="#Object.Pos">¶</a></h3>
<pre>func (obj *<a href="#Object">Object</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>

Pos computes the source position of the declaration of an object name. The
result may be an invalid position if it cannot be computed (obj.Decl may be nil
or not correct).

<h2 id="Package">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L995">Package</a>
    <a href="#Package">¶</a></h2>
<pre>type Package struct {
<span id="Package.Name"></span>    Name    <a href="/builtin/#string">string</a>             <span class="comment">// package name</span>
<span id="Package.Scope"></span>    Scope   *<a href="#Scope">Scope</a>             <span class="comment">// package scope across all files</span>
<span id="Package.Imports"></span>    Imports map[<a href="/builtin/#string">string</a>]*<a href="#Object">Object</a> <span class="comment">// map of package id -&gt; package object</span>
<span id="Package.Files"></span>    Files   map[<a href="/builtin/#string">string</a>]*<a href="#File">File</a>   <span class="comment">// Go source files by filename</span>
}</pre>

A Package node represents a set of source files collectively building a Go
package.

<h3 id="NewPackage">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/resolve.go#L64">NewPackage</a>
    <a href="#NewPackage">¶</a></h3>
<pre>func NewPackage(fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, files map[<a href="/builtin/#string">string</a>]*<a href="#File">File</a>, importer <a href="#Importer">Importer</a>, universe *<a href="#Scope">Scope</a>) (*<a href="#Package">Package</a>, <a href="/builtin/#error">error</a>)</pre>

NewPackage creates a new Package node from a set of File nodes. It resolves
unresolved identifiers across files and updates each file's Unresolved list
accordingly. If a non-nil importer and universe scope are provided, they are
used to resolve identifiers not declared in any of the package files. Any
remaining unresolved identifiers are reported as undeclared. If the files belong
to different packages, one package name is selected and files with different
package names are reported and then ignored. The result is a package node and a
scanner.ErrorList if there were errors.

<h3 id="Package.End">func (*Package) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L1003">End</a>
    <a href="#Package.End">¶</a></h3>
<pre>func (p *<a href="#Package">Package</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Package.Pos">func (*Package) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L1002">Pos</a>
    <a href="#Package.Pos">¶</a></h3>
<pre>func (p *<a href="#Package">Package</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="ParenExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L264">ParenExpr</a>
    <a href="#ParenExpr">¶</a></h2>
<pre>type ParenExpr struct {
<span id="ParenExpr.Lparen"></span>    Lparen <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;(&#34;</span>
<span id="ParenExpr.X"></span>    X      <a href="#Expr">Expr</a>      <span class="comment">// parenthesized expression</span>
<span id="ParenExpr.Rparen"></span>    Rparen <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;)&#34;</span>
}</pre>

A ParenExpr node represents a parenthesized expression.

<h3 id="ParenExpr.End">func (*ParenExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L456">End</a>
    <a href="#ParenExpr.End">¶</a></h3>
<pre>func (x *<a href="#ParenExpr">ParenExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="ParenExpr.Pos">func (*ParenExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L423">Pos</a>
    <a href="#ParenExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#ParenExpr">ParenExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="RangeStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L691">RangeStmt</a>
    <a href="#RangeStmt">¶</a></h2>
<pre>type RangeStmt struct {
<span id="RangeStmt.For"></span>    For        <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>   <span class="comment">// position of &#34;for&#34; keyword</span>
<span id="RangeStmt.Key"></span>    Key, Value <a href="#Expr">Expr</a>        <span class="comment">// Key, Value may be nil</span>
<span id="RangeStmt.TokPos"></span>    TokPos     <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>   <span class="comment">// position of Tok; invalid if Key == nil</span>
<span id="RangeStmt.Tok"></span>    Tok        <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a> <span class="comment">// ILLEGAL if Key == nil, ASSIGN, DEFINE</span>
<span id="RangeStmt.X"></span>    X          <a href="#Expr">Expr</a>        <span class="comment">// value to range over</span>
<span id="RangeStmt.Body"></span>    Body       *<a href="#BlockStmt">BlockStmt</a>
}</pre>

A RangeStmt represents a for statement with a range clause.

<h3 id="RangeStmt.End">func (*RangeStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L777">End</a>
    <a href="#RangeStmt.End">¶</a></h3>
<pre>func (s *<a href="#RangeStmt">RangeStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="RangeStmt.Pos">func (*RangeStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L723">Pos</a>
    <a href="#RangeStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#RangeStmt">RangeStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="ReturnStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L613">ReturnStmt</a>
    <a href="#ReturnStmt">¶</a></h2>
<pre>type ReturnStmt struct {
<span id="ReturnStmt.Return"></span>    Return  <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;return&#34; keyword</span>
<span id="ReturnStmt.Results"></span>    Results []<a href="#Expr">Expr</a>    <span class="comment">// result expressions; or nil</span>
}</pre>

A ReturnStmt node represents a return statement.

<h3 id="ReturnStmt.End">func (*ReturnStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L742">End</a>
    <a href="#ReturnStmt.End">¶</a></h3>
<pre>func (s *<a href="#ReturnStmt">ReturnStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="ReturnStmt.Pos">func (*ReturnStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L713">Pos</a>
    <a href="#ReturnStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#ReturnStmt">ReturnStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Scope">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go#L9">Scope</a>
    <a href="#Scope">¶</a></h2>
<pre>type Scope struct {
<span id="Scope.Outer"></span>    Outer   *<a href="#Scope">Scope</a>
<span id="Scope.Objects"></span>    Objects map[<a href="/builtin/#string">string</a>]*<a href="#Object">Object</a>
}</pre>

A Scope maintains the set of named language entities declared in the scope and a
link to the immediately surrounding (outer) scope.

<h3 id="NewScope">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go#L15">NewScope</a>
    <a href="#NewScope">¶</a></h3>
<pre>func NewScope(outer *<a href="#Scope">Scope</a>) *<a href="#Scope">Scope</a></pre>

NewScope creates a new scope nested in the outer scope.

<h3 id="Scope.Insert">func (*Scope) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go#L33">Insert</a>
    <a href="#Scope.Insert">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Insert(obj *<a href="#Object">Object</a>) (alt *<a href="#Object">Object</a>)</pre>

Insert attempts to insert a named object obj into the scope s. If the scope
already contains an object alt with the same name, Insert leaves the scope
unchanged and returns alt. Otherwise it inserts obj and returns nil.

<h3 id="Scope.Lookup">func (*Scope) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go#L24">Lookup</a>
    <a href="#Scope.Lookup">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Lookup(name <a href="/builtin/#string">string</a>) *<a href="#Object">Object</a></pre>

Lookup returns the object with the given name if it is found in scope s,
otherwise it returns nil. Outer scopes are ignored.

<h3 id="Scope.String">func (*Scope) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/scope.go#L41">String</a>
    <a href="#Scope.String">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) String() <a href="/builtin/#string">string</a></pre>

Debugging support

<h2 id="SelectStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L676">SelectStmt</a>
    <a href="#SelectStmt">¶</a></h2>
<pre>type SelectStmt struct {
<span id="SelectStmt.Select"></span>    Select <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>  <span class="comment">// position of &#34;select&#34; keyword</span>
<span id="SelectStmt.Body"></span>    Body   *<a href="#BlockStmt">BlockStmt</a> <span class="comment">// CommClauses only</span>
}</pre>

An SelectStmt node represents a select statement.

<h3 id="SelectStmt.End">func (*SelectStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L775">End</a>
    <a href="#SelectStmt.End">¶</a></h3>
<pre>func (s *<a href="#SelectStmt">SelectStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="SelectStmt.Pos">func (*SelectStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L721">Pos</a>
    <a href="#SelectStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#SelectStmt">SelectStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="SelectorExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L271">SelectorExpr</a>
    <a href="#SelectorExpr">¶</a></h2>
<pre>type SelectorExpr struct {
<span id="SelectorExpr.X"></span>    X   <a href="#Expr">Expr</a>   <span class="comment">// expression</span>
<span id="SelectorExpr.Sel"></span>    Sel *<a href="#Ident">Ident</a> <span class="comment">// field selector</span>
}</pre>

A SelectorExpr node represents an expression followed by a selector.

<h3 id="SelectorExpr.End">func (*SelectorExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L457">End</a>
    <a href="#SelectorExpr.End">¶</a></h3>
<pre>func (x *<a href="#SelectorExpr">SelectorExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="SelectorExpr.Pos">func (*SelectorExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L424">Pos</a>
    <a href="#SelectorExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#SelectorExpr">SelectorExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="SendStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L577">SendStmt</a>
    <a href="#SendStmt">¶</a></h2>
<pre>type SendStmt struct {
<span id="SendStmt.Chan"></span>    Chan  <a href="#Expr">Expr</a>
<span id="SendStmt.Arrow"></span>    Arrow <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;&lt;-&#34;</span>
<span id="SendStmt.Value"></span>    Value <a href="#Expr">Expr</a>
}</pre>

A SendStmt node represents a send statement.

<h3 id="SendStmt.End">func (*SendStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L735">End</a>
    <a href="#SendStmt.End">¶</a></h3>
<pre>func (s *<a href="#SendStmt">SendStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="SendStmt.Pos">func (*SendStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L708">Pos</a>
    <a href="#SendStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#SendStmt">SendStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="SliceExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L285">SliceExpr</a>
    <a href="#SliceExpr">¶</a></h2>
<pre>type SliceExpr struct {
<span id="SliceExpr.X"></span>    X      <a href="#Expr">Expr</a>      <span class="comment">// expression</span>
<span id="SliceExpr.Lbrack"></span>    Lbrack <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;[&#34;</span>
<span id="SliceExpr.Low"></span>    Low    <a href="#Expr">Expr</a>      <span class="comment">// begin of slice range; or nil</span>
<span id="SliceExpr.High"></span>    High   <a href="#Expr">Expr</a>      <span class="comment">// end of slice range; or nil</span>
<span id="SliceExpr.Max"></span>    Max    <a href="#Expr">Expr</a>      <span class="comment">// maximum capacity of slice; or nil</span>
<span id="SliceExpr.Slice3"></span>    Slice3 <a href="/builtin/#bool">bool</a>      <span class="comment">// true if 3-index slice (2 colons present)</span>
<span id="SliceExpr.Rbrack"></span>    Rbrack <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;]&#34;</span>
}</pre>

An SliceExpr node represents an expression followed by slice indices.

<h3 id="SliceExpr.End">func (*SliceExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L459">End</a>
    <a href="#SliceExpr.End">¶</a></h3>
<pre>func (x *<a href="#SliceExpr">SliceExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="SliceExpr.Pos">func (*SliceExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L426">Pos</a>
    <a href="#SliceExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#SliceExpr">SliceExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Spec">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L812">Spec</a>
    <a href="#Spec">¶</a></h2>
<pre>type Spec interface {
    <a href="#Node">Node</a>
    <span class="comment">// contains filtered or unexported methods</span>
}</pre>

The Spec type stands for any of *ImportSpec, *ValueSpec, and *TypeSpec.

<h2 id="StarExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L317">StarExpr</a>
    <a href="#StarExpr">¶</a></h2>
<pre>type StarExpr struct {
<span id="StarExpr.Star"></span>    Star <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;*&#34;</span>
<span id="StarExpr.X"></span>    X    <a href="#Expr">Expr</a>      <span class="comment">// operand</span>
}</pre>

A StarExpr node represents an expression of the form "*" Expression.
Semantically it could be a unary "*" expression, or a pointer type.

<h3 id="StarExpr.End">func (*StarExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L462">End</a>
    <a href="#StarExpr.End">¶</a></h3>
<pre>func (x *<a href="#StarExpr">StarExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="StarExpr.Pos">func (*StarExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L429">Pos</a>
    <a href="#StarExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#StarExpr">StarExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Stmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L37">Stmt</a>
    <a href="#Stmt">¶</a></h2>
<pre>type Stmt interface {
    <a href="#Node">Node</a>
    <span class="comment">// contains filtered or unexported methods</span>
}</pre>

All statement nodes implement the Stmt interface.

<h2 id="StructType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L372">StructType</a>
    <a href="#StructType">¶</a></h2>
<pre>type StructType struct {
<span id="StructType.Struct"></span>    Struct     <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>  <span class="comment">// position of &#34;struct&#34; keyword</span>
<span id="StructType.Fields"></span>    Fields     *<a href="#FieldList">FieldList</a> <span class="comment">// list of field declarations</span>
<span id="StructType.Incomplete"></span>    Incomplete <a href="/builtin/#bool">bool</a>       <span class="comment">// true if (source) fields are missing in the Fields list</span>
}</pre>

A StructType node represents a struct type.

<h3 id="StructType.End">func (*StructType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L467">End</a>
    <a href="#StructType.End">¶</a></h3>
<pre>func (x *<a href="#StructType">StructType</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="StructType.Pos">func (*StructType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L434">Pos</a>
    <a href="#StructType.Pos">¶</a></h3>
<pre>func (x *<a href="#StructType">StructType</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="SwitchStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L652">SwitchStmt</a>
    <a href="#SwitchStmt">¶</a></h2>
<pre>type SwitchStmt struct {
<span id="SwitchStmt.Switch"></span>    Switch <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>  <span class="comment">// position of &#34;switch&#34; keyword</span>
<span id="SwitchStmt.Init"></span>    Init   <a href="#Stmt">Stmt</a>       <span class="comment">// initialization statement; or nil</span>
<span id="SwitchStmt.Tag"></span>    Tag    <a href="#Expr">Expr</a>       <span class="comment">// tag expression; or nil</span>
<span id="SwitchStmt.Body"></span>    Body   *<a href="#BlockStmt">BlockStmt</a> <span class="comment">// CaseClauses only</span>
}</pre>

A SwitchStmt node represents an expression switch statement.

<h3 id="SwitchStmt.End">func (*SwitchStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L767">End</a>
    <a href="#SwitchStmt.End">¶</a></h3>
<pre>func (s *<a href="#SwitchStmt">SwitchStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="SwitchStmt.Pos">func (*SwitchStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L718">Pos</a>
    <a href="#SwitchStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#SwitchStmt">SwitchStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="TypeAssertExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L298">TypeAssertExpr</a>
    <a href="#TypeAssertExpr">¶</a></h2>
<pre>type TypeAssertExpr struct {
<span id="TypeAssertExpr.X"></span>    X      <a href="#Expr">Expr</a>      <span class="comment">// expression</span>
<span id="TypeAssertExpr.Lparen"></span>    Lparen <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;(&#34;</span>
<span id="TypeAssertExpr.Type"></span>    Type   <a href="#Expr">Expr</a>      <span class="comment">// asserted type; nil means type switch X.(type)</span>
<span id="TypeAssertExpr.Rparen"></span>    Rparen <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of &#34;)&#34;</span>
}</pre>

A TypeAssertExpr node represents an expression followed by a type assertion.

<h3 id="TypeAssertExpr.End">func (*TypeAssertExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L460">End</a>
    <a href="#TypeAssertExpr.End">¶</a></h3>
<pre>func (x *<a href="#TypeAssertExpr">TypeAssertExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="TypeAssertExpr.Pos">func (*TypeAssertExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L427">Pos</a>
    <a href="#TypeAssertExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#TypeAssertExpr">TypeAssertExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="TypeSpec">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L838">TypeSpec</a>
    <a href="#TypeSpec">¶</a></h2>
<pre>type TypeSpec struct {
<span id="TypeSpec.Doc"></span>    Doc     *<a href="#CommentGroup">CommentGroup</a> <span class="comment">// associated documentation; or nil</span>
<span id="TypeSpec.Name"></span>    Name    *<a href="#Ident">Ident</a>        <span class="comment">// type name</span>
<span id="TypeSpec.Assign"></span>    Assign  <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>     <span class="comment">// position of &#39;=&#39;, if any</span>
<span id="TypeSpec.Type"></span>    Type    <a href="#Expr">Expr</a>          <span class="comment">// *Ident, *ParenExpr, *SelectorExpr, *StarExpr, or any of the *XxxTypes</span>
<span id="TypeSpec.Comment"></span>    Comment *<a href="#CommentGroup">CommentGroup</a> <span class="comment">// line comments; or nil</span>
}</pre>

A TypeSpec node represents a type declaration (TypeSpec production).

<h3 id="TypeSpec.End">func (*TypeSpec) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L874">End</a>
    <a href="#TypeSpec.End">¶</a></h3>
<pre>func (s *<a href="#TypeSpec">TypeSpec</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="TypeSpec.Pos">func (*TypeSpec) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L856">Pos</a>
    <a href="#TypeSpec.Pos">¶</a></h3>
<pre>func (s *<a href="#TypeSpec">TypeSpec</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="TypeSwitchStmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L660">TypeSwitchStmt</a>
    <a href="#TypeSwitchStmt">¶</a></h2>
<pre>type TypeSwitchStmt struct {
<span id="TypeSwitchStmt.Switch"></span>    Switch <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>  <span class="comment">// position of &#34;switch&#34; keyword</span>
<span id="TypeSwitchStmt.Init"></span>    Init   <a href="#Stmt">Stmt</a>       <span class="comment">// initialization statement; or nil</span>
<span id="TypeSwitchStmt.Assign"></span>    Assign <a href="#Stmt">Stmt</a>       <span class="comment">// x := y.(type) or y.(type)</span>
<span id="TypeSwitchStmt.Body"></span>    Body   *<a href="#BlockStmt">BlockStmt</a> <span class="comment">// CaseClauses only</span>
}</pre>

An TypeSwitchStmt node represents a type switch statement.

<h3 id="TypeSwitchStmt.End">func (*TypeSwitchStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L768">End</a>
    <a href="#TypeSwitchStmt.End">¶</a></h3>
<pre>func (s *<a href="#TypeSwitchStmt">TypeSwitchStmt</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="TypeSwitchStmt.Pos">func (*TypeSwitchStmt) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L719">Pos</a>
    <a href="#TypeSwitchStmt.Pos">¶</a></h3>
<pre>func (s *<a href="#TypeSwitchStmt">TypeSwitchStmt</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="UnaryExpr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L325">UnaryExpr</a>
    <a href="#UnaryExpr">¶</a></h2>
<pre>type UnaryExpr struct {
<span id="UnaryExpr.OpPos"></span>    OpPos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>   <span class="comment">// position of Op</span>
<span id="UnaryExpr.Op"></span>    Op    <a href="/go/token/">token</a>.<a href="/go/token/#Token">Token</a> <span class="comment">// operator</span>
<span id="UnaryExpr.X"></span>    X     <a href="#Expr">Expr</a>        <span class="comment">// operand</span>
}</pre>

A UnaryExpr node represents a unary expression. Unary "*" expressions are
represented via StarExpr nodes.

<h3 id="UnaryExpr.End">func (*UnaryExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L463">End</a>
    <a href="#UnaryExpr.End">¶</a></h3>
<pre>func (x *<a href="#UnaryExpr">UnaryExpr</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="UnaryExpr.Pos">func (*UnaryExpr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L430">Pos</a>
    <a href="#UnaryExpr.Pos">¶</a></h3>
<pre>func (x *<a href="#UnaryExpr">UnaryExpr</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="ValueSpec">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L829">ValueSpec</a>
    <a href="#ValueSpec">¶</a></h2>
<pre>type ValueSpec struct {
<span id="ValueSpec.Doc"></span>    Doc     *<a href="#CommentGroup">CommentGroup</a> <span class="comment">// associated documentation; or nil</span>
<span id="ValueSpec.Names"></span>    Names   []*<a href="#Ident">Ident</a>      <span class="comment">// value names (len(Names) &gt; 0)</span>
<span id="ValueSpec.Type"></span>    Type    <a href="#Expr">Expr</a>          <span class="comment">// value type; or nil</span>
<span id="ValueSpec.Values"></span>    Values  []<a href="#Expr">Expr</a>        <span class="comment">// initial values; or nil</span>
<span id="ValueSpec.Comment"></span>    Comment *<a href="#CommentGroup">CommentGroup</a> <span class="comment">// line comments; or nil</span>
}</pre>

A ValueSpec node represents a constant or variable declaration (ConstSpec or
VarSpec production).

<h3 id="ValueSpec.End">func (*ValueSpec) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L865">End</a>
    <a href="#ValueSpec.End">¶</a></h3>
<pre>func (s *<a href="#ValueSpec">ValueSpec</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="ValueSpec.Pos">func (*ValueSpec) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/ast.go#L855">Pos</a>
    <a href="#ValueSpec.Pos">¶</a></h3>
<pre>func (s *<a href="#ValueSpec">ValueSpec</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h2 id="Visitor">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/go/ast/walk.go#L2">Visitor</a>
    <a href="#Visitor">¶</a></h2>
<pre>type Visitor interface {
    Visit(node <a href="#Node">Node</a>) (w <a href="#Visitor">Visitor</a>)
}</pre>

A Visitor's Visit method is invoked for each node encountered by Walk. If the
result visitor w is not nil, Walk visits each of the children of node with the
visitor w, followed by a call of w.Visit(nil).


