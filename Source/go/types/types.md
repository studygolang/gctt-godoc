version: 1.9.2
## package types

  `import "go/types"`

## Overview

Package types declares the data types and implements the algorithms for
type-checking of Go packages. Use Config.Check to invoke the type checker for a
package. Alternatively, create a new type checker with NewChecker and invoke it
incrementally by calling Checker.Files.

Type-checking consists of several interdependent phases:

Name resolution maps each identifier (ast.Ident) in the program to the language
object (Object) it denotes. Use Info.{Defs,Uses,Implicits} for the results of
name resolution.

Constant folding computes the exact constant value (constant.Value) for every
expression (ast.Expr) that is a compile-time constant. Use
Info.Types[expr].Value for the results of constant folding.

Type inference computes the type (Type) of every expression (ast.Expr) and
checks for compliance with the language specification. Use Info.Types[expr].Type
for the results of type inference.

For a tutorial, see https://golang.org/s/types-tutorial.

## Index

- [Variables](#pkg-variables)
- [func AssertableTo(V *Interface, T Type) bool](#AssertableTo)
- [func AssignableTo(V, T Type) bool](#AssignableTo)
- [func Comparable(T Type) bool](#Comparable)
- [func ConvertibleTo(V, T Type) bool](#ConvertibleTo)
- [func DefPredeclaredTestFuncs()](#DefPredeclaredTestFuncs)
- [func ExprString(x ast.Expr) string](#ExprString)
- [func Id(pkg *Package, name string) string](#Id)
- [func Identical(x, y Type) bool](#Identical)
- [func IdenticalIgnoreTags(x, y Type) bool](#IdenticalIgnoreTags)
- [func Implements(V Type, T *Interface) bool](#Implements)
- [func IsInterface(typ Type) bool](#IsInterface)
- [func ObjectString(obj Object, qf Qualifier) string](#ObjectString)
- [func SelectionString(s *Selection, qf Qualifier) string](#SelectionString)
- [func TypeString(typ Type, qf Qualifier) string](#TypeString)
- [func WriteExpr(buf *bytes.Buffer, x ast.Expr)](#WriteExpr)
- [func WriteSignature(buf *bytes.Buffer, sig *Signature, qf Qualifier)](#WriteSignature)
- [func WriteType(buf *bytes.Buffer, typ Type, qf Qualifier)](#WriteType)
- [type Array](#Array)
  - [func NewArray(elem Type, len int64) *Array](#NewArray)
  - [func (a *Array) Elem() Type](#Array.Elem)
  - [func (a *Array) Len() int64](#Array.Len)
  - [func (t *Array) String() string](#Array.String)
  - [func (t *Array) Underlying() Type](#Array.Underlying)
- [type Basic](#Basic)
  - [func (b *Basic) Info() BasicInfo](#Basic.Info)
  - [func (b *Basic) Kind() BasicKind](#Basic.Kind)
  - [func (b *Basic) Name() string](#Basic.Name)
  - [func (t *Basic) String() string](#Basic.String)
  - [func (t *Basic) Underlying() Type](#Basic.Underlying)
- [type BasicInfo](#BasicInfo)
- [type BasicKind](#BasicKind)
- [type Builtin](#Builtin)
  - [func (obj *Builtin) Exported() bool](#Builtin.Exported)
  - [func (obj *Builtin) Id() string](#Builtin.Id)
  - [func (obj *Builtin) Name() string](#Builtin.Name)
  - [func (obj *Builtin) Parent() *Scope](#Builtin.Parent)
  - [func (obj *Builtin) Pkg() *Package](#Builtin.Pkg)
  - [func (obj *Builtin) Pos() token.Pos](#Builtin.Pos)
  - [func (obj *Builtin) String() string](#Builtin.String)
  - [func (obj *Builtin) Type() Type](#Builtin.Type)
- [type Chan](#Chan)
  - [func NewChan(dir ChanDir, elem Type) *Chan](#NewChan)
  - [func (c *Chan) Dir() ChanDir](#Chan.Dir)
  - [func (c *Chan) Elem() Type](#Chan.Elem)
  - [func (t *Chan) String() string](#Chan.String)
  - [func (t *Chan) Underlying() Type](#Chan.Underlying)
- [type ChanDir](#ChanDir)
- [type Checker](#Checker)
  - [func NewChecker(conf *Config, fset *token.FileSet, pkg *Package, info *Info) *Checker](#NewChecker)
  - [func (check *Checker) Files(files []*ast.File) error](#Checker.Files)
- [type Config](#Config)
  - [func (conf *Config) Check(path string, fset *token.FileSet, files []*ast.File, info *Info) (*Package, error)](#Config.Check)
- [type Const](#Const)
  - [func NewConst(pos token.Pos, pkg *Package, name string, typ Type, val constant.Value) *Const](#NewConst)
  - [func (obj *Const) Exported() bool](#Const.Exported)
  - [func (obj *Const) Id() string](#Const.Id)
  - [func (obj *Const) Name() string](#Const.Name)
  - [func (obj *Const) Parent() *Scope](#Const.Parent)
  - [func (obj *Const) Pkg() *Package](#Const.Pkg)
  - [func (obj *Const) Pos() token.Pos](#Const.Pos)
  - [func (obj *Const) String() string](#Const.String)
  - [func (obj *Const) Type() Type](#Const.Type)
  - [func (obj *Const) Val() constant.Value](#Const.Val)
- [type Error](#Error)
  - [func (err Error) Error() string](#Error.Error)
- [type Func](#Func)
  - [func MissingMethod(V Type, T *Interface, static bool) (method *Func, wrongType bool)](#MissingMethod)
  - [func NewFunc(pos token.Pos, pkg *Package, name string, sig *Signature) *Func](#NewFunc)
  - [func (obj *Func) Exported() bool](#Func.Exported)
  - [func (obj *Func) FullName() string](#Func.FullName)
  - [func (obj *Func) Id() string](#Func.Id)
  - [func (obj *Func) Name() string](#Func.Name)
  - [func (obj *Func) Parent() *Scope](#Func.Parent)
  - [func (obj *Func) Pkg() *Package](#Func.Pkg)
  - [func (obj *Func) Pos() token.Pos](#Func.Pos)
  - [func (obj *Func) Scope() *Scope](#Func.Scope)
  - [func (obj *Func) String() string](#Func.String)
  - [func (obj *Func) Type() Type](#Func.Type)
- [type ImportMode](#ImportMode)
- [type Importer](#Importer)
- [type ImporterFrom](#ImporterFrom)
- [type Info](#Info)
  - [func (info *Info) ObjectOf(id *ast.Ident) Object](#Info.ObjectOf)
  - [func (info *Info) TypeOf(e ast.Expr) Type](#Info.TypeOf)
- [type Initializer](#Initializer)
  - [func (init *Initializer) String() string](#Initializer.String)
- [type Interface](#Interface)
  - [func NewInterface(methods []*Func, embeddeds []*Named) *Interface](#NewInterface)
  - [func (t *Interface) Complete() *Interface](#Interface.Complete)
  - [func (t *Interface) Embedded(i int) *Named](#Interface.Embedded)
  - [func (t *Interface) Empty() bool](#Interface.Empty)
  - [func (t *Interface) ExplicitMethod(i int) *Func](#Interface.ExplicitMethod)
  - [func (t *Interface) Method(i int) *Func](#Interface.Method)
  - [func (t *Interface) NumEmbeddeds() int](#Interface.NumEmbeddeds)
  - [func (t *Interface) NumExplicitMethods() int](#Interface.NumExplicitMethods)
  - [func (t *Interface) NumMethods() int](#Interface.NumMethods)
  - [func (t *Interface) String() string](#Interface.String)
  - [func (t *Interface) Underlying() Type](#Interface.Underlying)
- [type Label](#Label)
  - [func NewLabel(pos token.Pos, pkg *Package, name string) *Label](#NewLabel)
  - [func (obj *Label) Exported() bool](#Label.Exported)
  - [func (obj *Label) Id() string](#Label.Id)
  - [func (obj *Label) Name() string](#Label.Name)
  - [func (obj *Label) Parent() *Scope](#Label.Parent)
  - [func (obj *Label) Pkg() *Package](#Label.Pkg)
  - [func (obj *Label) Pos() token.Pos](#Label.Pos)
  - [func (obj *Label) String() string](#Label.String)
  - [func (obj *Label) Type() Type](#Label.Type)
- [type Map](#Map)
  - [func NewMap(key, elem Type) *Map](#NewMap)
  - [func (m *Map) Elem() Type](#Map.Elem)
  - [func (m *Map) Key() Type](#Map.Key)
  - [func (t *Map) String() string](#Map.String)
  - [func (t *Map) Underlying() Type](#Map.Underlying)
- [type MethodSet](#MethodSet)
  - [func NewMethodSet(T Type) *MethodSet](#NewMethodSet)
  - [func (s *MethodSet) At(i int) *Selection](#MethodSet.At)
  - [func (s *MethodSet) Len() int](#MethodSet.Len)
  - [func (s *MethodSet) Lookup(pkg *Package, name string) *Selection](#MethodSet.Lookup)
  - [func (s *MethodSet) String() string](#MethodSet.String)
- [type Named](#Named)
  - [func NewNamed(obj *TypeName, underlying Type, methods []*Func) *Named](#NewNamed)
  - [func (t *Named) AddMethod(m *Func)](#Named.AddMethod)
  - [func (t *Named) Method(i int) *Func](#Named.Method)
  - [func (t *Named) NumMethods() int](#Named.NumMethods)
  - [func (t *Named) Obj() *TypeName](#Named.Obj)
  - [func (t *Named) SetUnderlying(underlying Type)](#Named.SetUnderlying)
  - [func (t *Named) String() string](#Named.String)
  - [func (t *Named) Underlying() Type](#Named.Underlying)
- [type Nil](#Nil)
  - [func (obj *Nil) Exported() bool](#Nil.Exported)
  - [func (obj *Nil) Id() string](#Nil.Id)
  - [func (obj *Nil) Name() string](#Nil.Name)
  - [func (obj *Nil) Parent() *Scope](#Nil.Parent)
  - [func (obj *Nil) Pkg() *Package](#Nil.Pkg)
  - [func (obj *Nil) Pos() token.Pos](#Nil.Pos)
  - [func (obj *Nil) String() string](#Nil.String)
  - [func (obj *Nil) Type() Type](#Nil.Type)
- [type Object](#Object)
  - [func LookupFieldOrMethod(T Type, addressable bool, pkg *Package, name string) (obj Object, index []int, indirect bool)](#LookupFieldOrMethod)
- [type Package](#Package)
  - [func NewPackage(path, name string) *Package](#NewPackage)
  - [func (pkg *Package) Complete() bool](#Package.Complete)
  - [func (pkg *Package) Imports() []*Package](#Package.Imports)
  - [func (pkg *Package) MarkComplete()](#Package.MarkComplete)
  - [func (pkg *Package) Name() string](#Package.Name)
  - [func (pkg *Package) Path() string](#Package.Path)
  - [func (pkg *Package) Scope() *Scope](#Package.Scope)
  - [func (pkg *Package) SetImports(list []*Package)](#Package.SetImports)
  - [func (pkg *Package) SetName(name string)](#Package.SetName)
  - [func (pkg *Package) String() string](#Package.String)
- [type PkgName](#PkgName)
  - [func NewPkgName(pos token.Pos, pkg *Package, name string, imported *Package) *PkgName](#NewPkgName)
  - [func (obj *PkgName) Exported() bool](#PkgName.Exported)
  - [func (obj *PkgName) Id() string](#PkgName.Id)
  - [func (obj *PkgName) Imported() *Package](#PkgName.Imported)
  - [func (obj *PkgName) Name() string](#PkgName.Name)
  - [func (obj *PkgName) Parent() *Scope](#PkgName.Parent)
  - [func (obj *PkgName) Pkg() *Package](#PkgName.Pkg)
  - [func (obj *PkgName) Pos() token.Pos](#PkgName.Pos)
  - [func (obj *PkgName) String() string](#PkgName.String)
  - [func (obj *PkgName) Type() Type](#PkgName.Type)
- [type Pointer](#Pointer)
  - [func NewPointer(elem Type) *Pointer](#NewPointer)
  - [func (p *Pointer) Elem() Type](#Pointer.Elem)
  - [func (t *Pointer) String() string](#Pointer.String)
  - [func (t *Pointer) Underlying() Type](#Pointer.Underlying)
- [type Qualifier](#Qualifier)
  - [func RelativeTo(pkg *Package) Qualifier](#RelativeTo)
- [type Scope](#Scope)
  - [func NewScope(parent *Scope, pos, end token.Pos, comment string) *Scope](#NewScope)
  - [func (s *Scope) Child(i int) *Scope](#Scope.Child)
  - [func (s *Scope) Contains(pos token.Pos) bool](#Scope.Contains)
  - [func (s *Scope) End() token.Pos](#Scope.End)
  - [func (s *Scope) Innermost(pos token.Pos) *Scope](#Scope.Innermost)
  - [func (s *Scope) Insert(obj Object) Object](#Scope.Insert)
  - [func (s *Scope) Len() int](#Scope.Len)
  - [func (s *Scope) Lookup(name string) Object](#Scope.Lookup)
  - [func (s *Scope) LookupParent(name string, pos token.Pos) (*Scope, Object)](#Scope.LookupParent)
  - [func (s *Scope) Names() []string](#Scope.Names)
  - [func (s *Scope) NumChildren() int](#Scope.NumChildren)
  - [func (s *Scope) Parent() *Scope](#Scope.Parent)
  - [func (s *Scope) Pos() token.Pos](#Scope.Pos)
  - [func (s *Scope) String() string](#Scope.String)
  - [func (s *Scope) WriteTo(w io.Writer, n int, recurse bool)](#Scope.WriteTo)
- [type Selection](#Selection)
  - [func (s *Selection) Index() []int](#Selection.Index)
  - [func (s *Selection) Indirect() bool](#Selection.Indirect)
  - [func (s *Selection) Kind() SelectionKind](#Selection.Kind)
  - [func (s *Selection) Obj() Object](#Selection.Obj)
  - [func (s *Selection) Recv() Type](#Selection.Recv)
  - [func (s *Selection) String() string](#Selection.String)
  - [func (s *Selection) Type() Type](#Selection.Type)
- [type SelectionKind](#SelectionKind)
- [type Signature](#Signature)
  - [func NewSignature(recv *Var, params, results *Tuple, variadic bool) *Signature](#NewSignature)
  - [func (s *Signature) Params() *Tuple](#Signature.Params)
  - [func (s *Signature) Recv() *Var](#Signature.Recv)
  - [func (s *Signature) Results() *Tuple](#Signature.Results)
  - [func (t *Signature) String() string](#Signature.String)
  - [func (t *Signature) Underlying() Type](#Signature.Underlying)
  - [func (s *Signature) Variadic() bool](#Signature.Variadic)
- [type Sizes](#Sizes)
  - [func SizesFor(compiler, arch string) Sizes](#SizesFor)
- [type Slice](#Slice)
  - [func NewSlice(elem Type) *Slice](#NewSlice)
  - [func (s *Slice) Elem() Type](#Slice.Elem)
  - [func (t *Slice) String() string](#Slice.String)
  - [func (t *Slice) Underlying() Type](#Slice.Underlying)
- [type StdSizes](#StdSizes)
  - [func (s *StdSizes) Alignof(T Type) int64](#StdSizes.Alignof)
  - [func (s *StdSizes) Offsetsof(fields []*Var) []int64](#StdSizes.Offsetsof)
  - [func (s *StdSizes) Sizeof(T Type) int64](#StdSizes.Sizeof)
- [type Struct](#Struct)
  - [func NewStruct(fields []*Var, tags []string) *Struct](#NewStruct)
  - [func (s *Struct) Field(i int) *Var](#Struct.Field)
  - [func (s *Struct) NumFields() int](#Struct.NumFields)
  - [func (t *Struct) String() string](#Struct.String)
  - [func (s *Struct) Tag(i int) string](#Struct.Tag)
  - [func (t *Struct) Underlying() Type](#Struct.Underlying)
- [type Tuple](#Tuple)
  - [func NewTuple(x ...*Var) *Tuple](#NewTuple)
  - [func (t *Tuple) At(i int) *Var](#Tuple.At)
  - [func (t *Tuple) Len() int](#Tuple.Len)
  - [func (t *Tuple) String() string](#Tuple.String)
  - [func (t *Tuple) Underlying() Type](#Tuple.Underlying)
- [type Type](#Type)
  - [func Default(typ Type) Type](#Default)
- [type TypeAndValue](#TypeAndValue)
  - [func Eval(fset *token.FileSet, pkg *Package, pos token.Pos, expr string) (TypeAndValue, error)](#Eval)
  - [func (tv TypeAndValue) Addressable() bool](#TypeAndValue.Addressable)
  - [func (tv TypeAndValue) Assignable() bool](#TypeAndValue.Assignable)
  - [func (tv TypeAndValue) HasOk() bool](#TypeAndValue.HasOk)
  - [func (tv TypeAndValue) IsBuiltin() bool](#TypeAndValue.IsBuiltin)
  - [func (tv TypeAndValue) IsNil() bool](#TypeAndValue.IsNil)
  - [func (tv TypeAndValue) IsType() bool](#TypeAndValue.IsType)
  - [func (tv TypeAndValue) IsValue() bool](#TypeAndValue.IsValue)
  - [func (tv TypeAndValue) IsVoid() bool](#TypeAndValue.IsVoid)
- [type TypeName](#TypeName)
  - [func NewTypeName(pos token.Pos, pkg *Package, name string, typ Type) *TypeName](#NewTypeName)
  - [func (obj *TypeName) Exported() bool](#TypeName.Exported)
  - [func (obj *TypeName) Id() string](#TypeName.Id)
  - [func (obj *TypeName) IsAlias() bool](#TypeName.IsAlias)
  - [func (obj *TypeName) Name() string](#TypeName.Name)
  - [func (obj *TypeName) Parent() *Scope](#TypeName.Parent)
  - [func (obj *TypeName) Pkg() *Package](#TypeName.Pkg)
  - [func (obj *TypeName) Pos() token.Pos](#TypeName.Pos)
  - [func (obj *TypeName) String() string](#TypeName.String)
  - [func (obj *TypeName) Type() Type](#TypeName.Type)
- [type Var](#Var)
  - [func NewField(pos token.Pos, pkg *Package, name string, typ Type, anonymous bool) *Var](#NewField)
  - [func NewParam(pos token.Pos, pkg *Package, name string, typ Type) *Var](#NewParam)
  - [func NewVar(pos token.Pos, pkg *Package, name string, typ Type) *Var](#NewVar)
  - [func (obj *Var) Anonymous() bool](#Var.Anonymous)
  - [func (obj *Var) Exported() bool](#Var.Exported)
  - [func (obj *Var) Id() string](#Var.Id)
  - [func (obj *Var) IsField() bool](#Var.IsField)
  - [func (obj *Var) Name() string](#Var.Name)
  - [func (obj *Var) Parent() *Scope](#Var.Parent)
  - [func (obj *Var) Pkg() *Package](#Var.Pkg)
  - [func (obj *Var) Pos() token.Pos](#Var.Pos)
  - [func (obj *Var) String() string](#Var.String)
  - [func (obj *Var) Type() Type](#Var.Type)

### Examples

- [Info](#exampleInfo)
- [MethodSet](#exampleMethodSet)
- [Scope](#exampleScope)

### Package files
 [api.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go) [assignments.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/assignments.go) [builtins.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/builtins.go) [call.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/call.go) [check.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/check.go) [conversions.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/conversions.go) [decl.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/decl.go) [errors.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/errors.go) [eval.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/eval.go) [expr.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/expr.go) [exprstring.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/exprstring.go) [initorder.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/initorder.go) [labels.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/labels.go) [lookup.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/lookup.go) [methodset.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/methodset.go) [object.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go) [objset.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/objset.go) [operand.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/operand.go) [ordering.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/ordering.go) [package.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go) [predicates.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/predicates.go) [resolver.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/resolver.go) [return.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/return.go) [scope.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go) [selection.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go) [sizes.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/sizes.go) [stmt.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/stmt.go) [type.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go) [typestring.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/typestring.go) [typexpr.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/typexpr.go) [universe.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/universe.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="Universe">Universe</span> *<a href="#Scope">Scope</a>
    <span id="Unsafe">Unsafe</span>   *<a href="#Package">Package</a>
)</pre>


<pre>var <span id="Typ">Typ</span> = []*<a href="#Basic">Basic</a>{
    <a href="#Invalid">Invalid</a>: {<a href="#Invalid">Invalid</a>, 0, &#34;invalid type&#34;},

    <a href="#Bool">Bool</a>:          {<a href="#Bool">Bool</a>, <a href="#IsBoolean">IsBoolean</a>, &#34;bool&#34;},
    <a href="#Int">Int</a>:           {<a href="#Int">Int</a>, <a href="#IsInteger">IsInteger</a>, &#34;int&#34;},
    <a href="#Int8">Int8</a>:          {<a href="#Int8">Int8</a>, <a href="#IsInteger">IsInteger</a>, &#34;int8&#34;},
    <a href="#Int16">Int16</a>:         {<a href="#Int16">Int16</a>, <a href="#IsInteger">IsInteger</a>, &#34;int16&#34;},
    <a href="#Int32">Int32</a>:         {<a href="#Int32">Int32</a>, <a href="#IsInteger">IsInteger</a>, &#34;int32&#34;},
    <a href="#Int64">Int64</a>:         {<a href="#Int64">Int64</a>, <a href="#IsInteger">IsInteger</a>, &#34;int64&#34;},
    <a href="#Uint">Uint</a>:          {<a href="#Uint">Uint</a>, <a href="#IsInteger">IsInteger</a> | <a href="#IsUnsigned">IsUnsigned</a>, &#34;uint&#34;},
    <a href="#Uint8">Uint8</a>:         {<a href="#Uint8">Uint8</a>, <a href="#IsInteger">IsInteger</a> | <a href="#IsUnsigned">IsUnsigned</a>, &#34;uint8&#34;},
    <a href="#Uint16">Uint16</a>:        {<a href="#Uint16">Uint16</a>, <a href="#IsInteger">IsInteger</a> | <a href="#IsUnsigned">IsUnsigned</a>, &#34;uint16&#34;},
    <a href="#Uint32">Uint32</a>:        {<a href="#Uint32">Uint32</a>, <a href="#IsInteger">IsInteger</a> | <a href="#IsUnsigned">IsUnsigned</a>, &#34;uint32&#34;},
    <a href="#Uint64">Uint64</a>:        {<a href="#Uint64">Uint64</a>, <a href="#IsInteger">IsInteger</a> | <a href="#IsUnsigned">IsUnsigned</a>, &#34;uint64&#34;},
    <a href="#Uintptr">Uintptr</a>:       {<a href="#Uintptr">Uintptr</a>, <a href="#IsInteger">IsInteger</a> | <a href="#IsUnsigned">IsUnsigned</a>, &#34;uintptr&#34;},
    <a href="#Float32">Float32</a>:       {<a href="#Float32">Float32</a>, <a href="#IsFloat">IsFloat</a>, &#34;float32&#34;},
    <a href="#Float64">Float64</a>:       {<a href="#Float64">Float64</a>, <a href="#IsFloat">IsFloat</a>, &#34;float64&#34;},
    <a href="#Complex64">Complex64</a>:     {<a href="#Complex64">Complex64</a>, <a href="#IsComplex">IsComplex</a>, &#34;complex64&#34;},
    <a href="#Complex128">Complex128</a>:    {<a href="#Complex128">Complex128</a>, <a href="#IsComplex">IsComplex</a>, &#34;complex128&#34;},
    <a href="#String">String</a>:        {<a href="#String">String</a>, <a href="#IsString">IsString</a>, &#34;string&#34;},
    <a href="#UnsafePointer">UnsafePointer</a>: {<a href="#UnsafePointer">UnsafePointer</a>, 0, &#34;Pointer&#34;},

    <a href="#UntypedBool">UntypedBool</a>:    {<a href="#UntypedBool">UntypedBool</a>, <a href="#IsBoolean">IsBoolean</a> | <a href="#IsUntyped">IsUntyped</a>, &#34;untyped bool&#34;},
    <a href="#UntypedInt">UntypedInt</a>:     {<a href="#UntypedInt">UntypedInt</a>, <a href="#IsInteger">IsInteger</a> | <a href="#IsUntyped">IsUntyped</a>, &#34;untyped int&#34;},
    <a href="#UntypedRune">UntypedRune</a>:    {<a href="#UntypedRune">UntypedRune</a>, <a href="#IsInteger">IsInteger</a> | <a href="#IsUntyped">IsUntyped</a>, &#34;untyped rune&#34;},
    <a href="#UntypedFloat">UntypedFloat</a>:   {<a href="#UntypedFloat">UntypedFloat</a>, <a href="#IsFloat">IsFloat</a> | <a href="#IsUntyped">IsUntyped</a>, &#34;untyped float&#34;},
    <a href="#UntypedComplex">UntypedComplex</a>: {<a href="#UntypedComplex">UntypedComplex</a>, <a href="#IsComplex">IsComplex</a> | <a href="#IsUntyped">IsUntyped</a>, &#34;untyped complex&#34;},
    <a href="#UntypedString">UntypedString</a>:  {<a href="#UntypedString">UntypedString</a>, <a href="#IsString">IsString</a> | <a href="#IsUntyped">IsUntyped</a>, &#34;untyped string&#34;},
    <a href="#UntypedNil">UntypedNil</a>:     {<a href="#UntypedNil">UntypedNil</a>, <a href="#IsUntyped">IsUntyped</a>, &#34;untyped nil&#34;},
}</pre>


<h2 id="AssertableTo">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L345">AssertableTo</a>
    <a href="#AssertableTo">¶</a></h2>
<pre>func AssertableTo(V *<a href="#Interface">Interface</a>, T <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a></pre>

AssertableTo reports whether a value of type V can be asserted to have type T.

<h2 id="AssignableTo">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L351">AssignableTo</a>
    <a href="#AssignableTo">¶</a></h2>
<pre>func AssignableTo(V, T <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a></pre>

AssignableTo reports whether a value of type V is assignable to a variable of
type T.

<h2 id="Comparable">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/predicates.go#L71">Comparable</a>
    <a href="#Comparable">¶</a></h2>
<pre>func Comparable(T <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a></pre>

Comparable reports whether values of type T are comparable.

<h2 id="ConvertibleTo">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L357">ConvertibleTo</a>
    <a href="#ConvertibleTo">¶</a></h2>
<pre>func ConvertibleTo(V, T <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a></pre>

ConvertibleTo reports whether a value of type V is convertible to a value of
type T.

<h2 id="DefPredeclaredTestFuncs">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/universe.go#L160">DefPredeclaredTestFuncs</a>
    <a href="#DefPredeclaredTestFuncs">¶</a></h2>
<pre>func DefPredeclaredTestFuncs()</pre>

DefPredeclaredTestFuncs defines the assert and trace built-ins. These built-ins
are intended for debugging and testing of this package only.

<h2 id="ExprString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/exprstring.go#L5">ExprString</a>
    <a href="#ExprString">¶</a></h2>
<pre>func ExprString(x <a href="/go/ast/">ast</a>.<a href="/go/ast/#Expr">Expr</a>) <a href="/builtin/#string">string</a></pre>

ExprString returns the (possibly simplified) string representation for x.

<h2 id="Id">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L47">Id</a>
    <a href="#Id">¶</a></h2>
<pre>func Id(pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Id returns name if it is exported, otherwise it returns the name qualified with
the package path.

<h2 id="Identical">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/predicates.go#L104">Identical</a>
    <a href="#Identical">¶</a></h2>
<pre>func Identical(x, y <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a></pre>

Identical reports whether x and y are identical.

<h2 id="IdenticalIgnoreTags">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/predicates.go#L109">IdenticalIgnoreTags</a>
    <a href="#IdenticalIgnoreTags">¶</a></h2>
<pre>func IdenticalIgnoreTags(x, y <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a></pre>

IdenticalIgnoreTags reports whether x and y are identical if tags are ignored.

<h2 id="Implements">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L363">Implements</a>
    <a href="#Implements">¶</a></h2>
<pre>func Implements(V <a href="#Type">Type</a>, T *<a href="#Interface">Interface</a>) <a href="/builtin/#bool">bool</a></pre>

Implements reports whether type V implements interface T.

<h2 id="IsInterface">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/predicates.go#L65">IsInterface</a>
    <a href="#IsInterface">¶</a></h2>
<pre>func IsInterface(typ <a href="#Type">Type</a>) <a href="/builtin/#bool">bool</a></pre>

IsInterface reports whether typ is an interface type.

<h2 id="ObjectString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L352">ObjectString</a>
    <a href="#ObjectString">¶</a></h2>
<pre>func ObjectString(obj <a href="#Object">Object</a>, qf <a href="#Qualifier">Qualifier</a>) <a href="/builtin/#string">string</a></pre>

ObjectString returns the string form of obj. The Qualifier controls the printing
of package-level objects, and may be nil.

<h2 id="SelectionString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go#L109">SelectionString</a>
    <a href="#SelectionString">¶</a></h2>
<pre>func SelectionString(s *<a href="#Selection">Selection</a>, qf <a href="#Qualifier">Qualifier</a>) <a href="/builtin/#string">string</a></pre>

SelectionString returns the string form of s. The Qualifier controls the
printing of package-level objects, and may be nil.

Examples:

    "field (T) f int"
    "method (T) f(X) Y"
    "method expr (T) f(X) Y"

<h2 id="TypeString">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/typestring.go#L55">TypeString</a>
    <a href="#TypeString">¶</a></h2>
<pre>func TypeString(typ <a href="#Type">Type</a>, qf <a href="#Qualifier">Qualifier</a>) <a href="/builtin/#string">string</a></pre>

TypeString returns the string representation of typ. The Qualifier controls the
printing of package-level objects, and may be nil.

<h2 id="WriteExpr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/exprstring.go#L12">WriteExpr</a>
    <a href="#WriteExpr">¶</a></h2>
<pre>func WriteExpr(buf *<a href="/bytes/">bytes</a>.<a href="/bytes/#Buffer">Buffer</a>, x <a href="/go/ast/">ast</a>.<a href="/go/ast/#Expr">Expr</a>)</pre>

WriteExpr writes the (possibly simplified) string representation for x to buf.

<h2 id="WriteSignature">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/typestring.go#L265">WriteSignature</a>
    <a href="#WriteSignature">¶</a></h2>
<pre>func WriteSignature(buf *<a href="/bytes/">bytes</a>.<a href="/bytes/#Buffer">Buffer</a>, sig *<a href="#Signature">Signature</a>, qf <a href="#Qualifier">Qualifier</a>)</pre>

WriteSignature writes the representation of the signature sig to buf, without a
leading "func" keyword. The Qualifier controls the printing of package-level
objects, and may be nil.

<h2 id="WriteType">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/typestring.go#L64">WriteType</a>
    <a href="#WriteType">¶</a></h2>
<pre>func WriteType(buf *<a href="/bytes/">bytes</a>.<a href="/bytes/#Buffer">Buffer</a>, typ <a href="#Type">Type</a>, qf <a href="#Qualifier">Qualifier</a>)</pre>

WriteType writes the string representation of typ to buf. The Qualifier controls
the printing of package-level objects, and may be nil.

<h2 id="Array">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L84">Array</a>
    <a href="#Array">¶</a></h2>
<pre>type Array struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

An Array represents an array type.

<h3 id="NewArray">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L90">NewArray</a>
    <a href="#NewArray">¶</a></h3>
<pre>func NewArray(elem <a href="#Type">Type</a>, len <a href="/builtin/#int64">int64</a>) *<a href="#Array">Array</a></pre>

NewArray returns a new array type for the given element type and length.

<h3 id="Array.Elem">func (*Array) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L96">Elem</a>
    <a href="#Array.Elem">¶</a></h3>
<pre>func (a *<a href="#Array">Array</a>) Elem() <a href="#Type">Type</a></pre>

Elem returns element type of array a.

<h3 id="Array.Len">func (*Array) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L93">Len</a>
    <a href="#Array.Len">¶</a></h3>
<pre>func (a *<a href="#Array">Array</a>) Len() <a href="/builtin/#int64">int64</a></pre>

Len returns the length of array a.

<h3 id="Array.String">func (*Array) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L431">String</a>
    <a href="#Array.String">¶</a></h3>
<pre>func (t *<a href="#Array">Array</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Array.Underlying">func (*Array) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L419">Underlying</a>
    <a href="#Array.Underlying">¶</a></h3>
<pre>func (t *<a href="#Array">Array</a>) Underlying() <a href="#Type">Type</a></pre>


<h2 id="Basic">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L68">Basic</a>
    <a href="#Basic">¶</a></h2>
<pre>type Basic struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Basic represents a basic type.

<h3 id="Basic.Info">func (*Basic) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L78">Info</a>
    <a href="#Basic.Info">¶</a></h3>
<pre>func (b *<a href="#Basic">Basic</a>) Info() <a href="#BasicInfo">BasicInfo</a></pre>

Info returns information about properties of basic type b.

<h3 id="Basic.Kind">func (*Basic) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L75">Kind</a>
    <a href="#Basic.Kind">¶</a></h3>
<pre>func (b *<a href="#Basic">Basic</a>) Kind() <a href="#BasicKind">BasicKind</a></pre>

Kind returns the kind of basic type b.

<h3 id="Basic.Name">func (*Basic) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L81">Name</a>
    <a href="#Basic.Name">¶</a></h3>
<pre>func (b *<a href="#Basic">Basic</a>) Name() <a href="/builtin/#string">string</a></pre>

Name returns the name of basic type b.

<h3 id="Basic.String">func (*Basic) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L430">String</a>
    <a href="#Basic.String">¶</a></h3>
<pre>func (t *<a href="#Basic">Basic</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Basic.Underlying">func (*Basic) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L418">Underlying</a>
    <a href="#Basic.Underlying">¶</a></h3>
<pre>func (t *<a href="#Basic">Basic</a>) Underlying() <a href="#Type">Type</a></pre>


<h2 id="BasicInfo">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L50">BasicInfo</a>
    <a href="#BasicInfo">¶</a></h2>
<pre>type BasicInfo <a href="/builtin/#int">int</a></pre>

BasicInfo is a set of flags describing properties of a basic type.

<pre>const (
    <span id="IsBoolean">IsBoolean</span> <a href="#BasicInfo">BasicInfo</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a>
    <span id="IsInteger">IsInteger</span>
    <span id="IsUnsigned">IsUnsigned</span>
    <span id="IsFloat">IsFloat</span>
    <span id="IsComplex">IsComplex</span>
    <span id="IsString">IsString</span>
    <span id="IsUntyped">IsUntyped</span>

    <span id="IsOrdered">IsOrdered</span>   = <a href="#IsInteger">IsInteger</a> | <a href="#IsFloat">IsFloat</a> | <a href="#IsString">IsString</a>
    <span id="IsNumeric">IsNumeric</span>   = <a href="#IsInteger">IsInteger</a> | <a href="#IsFloat">IsFloat</a> | <a href="#IsComplex">IsComplex</a>
    <span id="IsConstType">IsConstType</span> = <a href="#IsBoolean">IsBoolean</a> | <a href="#IsNumeric">IsNumeric</a> | <a href="#IsString">IsString</a>
)</pre>

Properties of basic types.

<h2 id="BasicKind">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L10">BasicKind</a>
    <a href="#BasicKind">¶</a></h2>
<pre>type BasicKind <a href="/builtin/#int">int</a></pre>

BasicKind describes the kind of basic type.

<pre>const (
    <span id="Invalid">Invalid</span> <a href="#BasicKind">BasicKind</a> = <a href="/builtin/#iota">iota</a> <span class="comment">// type is invalid</span>

    <span class="comment">// predeclared types</span>
    <span id="Bool">Bool</span>
    <span id="Int">Int</span>
    <span id="Int8">Int8</span>
    <span id="Int16">Int16</span>
    <span id="Int32">Int32</span>
    <span id="Int64">Int64</span>
    <span id="Uint">Uint</span>
    <span id="Uint8">Uint8</span>
    <span id="Uint16">Uint16</span>
    <span id="Uint32">Uint32</span>
    <span id="Uint64">Uint64</span>
    <span id="Uintptr">Uintptr</span>
    <span id="Float32">Float32</span>
    <span id="Float64">Float64</span>
    <span id="Complex64">Complex64</span>
    <span id="Complex128">Complex128</span>
    <span id="String">String</span>
    <span id="UnsafePointer">UnsafePointer</span>

    <span class="comment">// types for untyped values</span>
    <span id="UntypedBool">UntypedBool</span>
    <span id="UntypedInt">UntypedInt</span>
    <span id="UntypedRune">UntypedRune</span>
    <span id="UntypedFloat">UntypedFloat</span>
    <span id="UntypedComplex">UntypedComplex</span>
    <span id="UntypedString">UntypedString</span>
    <span id="UntypedNil">UntypedNil</span>

    <span class="comment">// aliases</span>
    <span id="Byte">Byte</span> = <a href="#Uint8">Uint8</a>
    <span id="Rune">Rune</span> = <a href="#Int32">Int32</a>
)</pre>


<h2 id="Builtin">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L239">Builtin</a>
    <a href="#Builtin">¶</a></h2>
<pre>type Builtin struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Builtin represents a built-in function. Builtins don't have a valid type.

<h3 id="Builtin.Exported">func (*Builtin) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L81">Exported</a>
    <a href="#Builtin.Exported">¶</a></h3>
<pre>func (obj *<a href="#Builtin">Builtin</a>) Exported() <a href="/builtin/#bool">bool</a></pre>


<h3 id="Builtin.Id">func (*Builtin) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L82">Id</a>
    <a href="#Builtin.Id">¶</a></h3>
<pre>func (obj *<a href="#Builtin">Builtin</a>) Id() <a href="/builtin/#string">string</a></pre>


<h3 id="Builtin.Name">func (*Builtin) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L79">Name</a>
    <a href="#Builtin.Name">¶</a></h3>
<pre>func (obj *<a href="#Builtin">Builtin</a>) Name() <a href="/builtin/#string">string</a></pre>


<h3 id="Builtin.Parent">func (*Builtin) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L76">Parent</a>
    <a href="#Builtin.Parent">¶</a></h3>
<pre>func (obj *<a href="#Builtin">Builtin</a>) Parent() *<a href="#Scope">Scope</a></pre>


<h3 id="Builtin.Pkg">func (*Builtin) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L78">Pkg</a>
    <a href="#Builtin.Pkg">¶</a></h3>
<pre>func (obj *<a href="#Builtin">Builtin</a>) Pkg() *<a href="#Package">Package</a></pre>


<h3 id="Builtin.Pos">func (*Builtin) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L77">Pos</a>
    <a href="#Builtin.Pos">¶</a></h3>
<pre>func (obj *<a href="#Builtin">Builtin</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Builtin.String">func (*Builtin) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L364">String</a>
    <a href="#Builtin.String">¶</a></h3>
<pre>func (obj *<a href="#Builtin">Builtin</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Builtin.Type">func (*Builtin) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L80">Type</a>
    <a href="#Builtin.Type">¶</a></h3>
<pre>func (obj *<a href="#Builtin">Builtin</a>) Type() <a href="#Type">Type</a></pre>


<h2 id="Chan">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L341">Chan</a>
    <a href="#Chan">¶</a></h2>
<pre>type Chan struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Chan represents a channel type.

<h3 id="NewChan">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L357">NewChan</a>
    <a href="#NewChan">¶</a></h3>
<pre>func NewChan(dir <a href="#ChanDir">ChanDir</a>, elem <a href="#Type">Type</a>) *<a href="#Chan">Chan</a></pre>

NewChan returns a new channel type for the given direction and element type.

<h3 id="Chan.Dir">func (*Chan) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L362">Dir</a>
    <a href="#Chan.Dir">¶</a></h3>
<pre>func (c *<a href="#Chan">Chan</a>) Dir() <a href="#ChanDir">ChanDir</a></pre>

Dir returns the direction of channel c.

<h3 id="Chan.Elem">func (*Chan) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L365">Elem</a>
    <a href="#Chan.Elem">¶</a></h3>
<pre>func (c *<a href="#Chan">Chan</a>) Elem() <a href="#Type">Type</a></pre>

Elem returns the element type of channel c.

<h3 id="Chan.String">func (*Chan) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L439">String</a>
    <a href="#Chan.String">¶</a></h3>
<pre>func (t *<a href="#Chan">Chan</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Chan.Underlying">func (*Chan) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L427">Underlying</a>
    <a href="#Chan.Underlying">¶</a></h3>
<pre>func (t *<a href="#Chan">Chan</a>) Underlying() <a href="#Type">Type</a></pre>


<h2 id="ChanDir">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L347">ChanDir</a>
    <a href="#ChanDir">¶</a></h2>
<pre>type ChanDir <a href="/builtin/#int">int</a></pre>

A ChanDir value indicates a channel direction.

<pre>const (
    <span id="SendRecv">SendRecv</span> <a href="#ChanDir">ChanDir</a> = <a href="/builtin/#iota">iota</a>
    <span id="SendOnly">SendOnly</span>
    <span id="RecvOnly">RecvOnly</span>
)</pre>

The direction of a channel is indicated by one of these constants.

<h2 id="Checker">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/check.go#L62">Checker</a>
    <a href="#Checker">¶</a></h2>
<pre>type Checker struct {
    *<a href="#Info">Info</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Checker maintains the state of the type checker. It must be created with
NewChecker.

<h3 id="NewChecker">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/check.go#L149">NewChecker</a>
    <a href="#NewChecker">¶</a></h3>
<pre>func NewChecker(conf *<a href="#Config">Config</a>, fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, pkg *<a href="#Package">Package</a>, info *<a href="#Info">Info</a>) *<a href="#Checker">Checker</a></pre>

NewChecker returns a new Checker instance for a given package. Package files may
be added incrementally via checker.Files.

<h3 id="Checker.Files">func (*Checker) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/check.go#L220">Files</a>
    <a href="#Checker.Files">¶</a></h3>
<pre>func (check *<a href="#Checker">Checker</a>) Files(files []*<a href="/go/ast/">ast</a>.<a href="/go/ast/#File">File</a>) <a href="/builtin/#error">error</a></pre>

Files checks the provided files as part of the checker's package.

<h2 id="Config">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L84">Config</a>
    <a href="#Config">¶</a></h2>
<pre>type Config struct {
    <span class="comment">// If IgnoreFuncBodies is set, function bodies are not</span>
    <span class="comment">// type-checked.</span>
<span id="Config.IgnoreFuncBodies"></span>    IgnoreFuncBodies <a href="/builtin/#bool">bool</a>

    <span class="comment">// If FakeImportC is set, `import &#34;C&#34;` (for packages requiring Cgo)</span>
    <span class="comment">// declares an empty &#34;C&#34; package and errors are omitted for qualified</span>
    <span class="comment">// identifiers referring to package C (which won&#39;t find an object).</span>
    <span class="comment">// This feature is intended for the standard library cmd/api tool.</span>
    <span class="comment">//</span>
    <span class="comment">// Caution: Effects may be unpredictable due to follow-on errors.</span>
    <span class="comment">//          Do not use casually!</span>
<span id="Config.FakeImportC"></span>    FakeImportC <a href="/builtin/#bool">bool</a>

    <span class="comment">// If Error != nil, it is called with each error found</span>
    <span class="comment">// during type checking; err has dynamic type Error.</span>
    <span class="comment">// Secondary errors (for instance, to enumerate all types</span>
    <span class="comment">// involved in an invalid recursive type declaration) have</span>
    <span class="comment">// error strings that start with a &#39;\t&#39; character.</span>
    <span class="comment">// If Error == nil, type-checking stops with the first</span>
    <span class="comment">// error found.</span>
<span id="Config.Error"></span>    Error func(err <a href="/builtin/#error">error</a>)

    <span class="comment">// An importer is used to import packages referred to from</span>
    <span class="comment">// import declarations.</span>
    <span class="comment">// If the installed importer implements ImporterFrom, the type</span>
    <span class="comment">// checker calls ImportFrom instead of Import.</span>
    <span class="comment">// The type checker reports an error if an importer is needed</span>
    <span class="comment">// but none was installed.</span>
<span id="Config.Importer"></span>    Importer <a href="#Importer">Importer</a>

    <span class="comment">// If Sizes != nil, it provides the sizing functions for package unsafe.</span>
    <span class="comment">// Otherwise SizesFor(&#34;gc&#34;, &#34;amd64&#34;) is used instead.</span>
<span id="Config.Sizes"></span>    Sizes <a href="#Sizes">Sizes</a>

    <span class="comment">// If DisableUnusedImportCheck is set, packages are not checked</span>
    <span class="comment">// for unused imports.</span>
<span id="Config.DisableUnusedImportCheck"></span>    DisableUnusedImportCheck <a href="/builtin/#bool">bool</a>
}</pre>

A Config specifies the configuration for type checking. The zero value for
Config is a ready-to-use default configuration.

<h3 id="Config.Check">func (*Config) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L339">Check</a>
    <a href="#Config.Check">¶</a></h3>
<pre>func (conf *<a href="#Config">Config</a>) Check(path <a href="/builtin/#string">string</a>, fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, files []*<a href="/go/ast/">ast</a>.<a href="/go/ast/#File">File</a>, info *<a href="#Info">Info</a>) (*<a href="#Package">Package</a>, <a href="/builtin/#error">error</a>)</pre>

Check type-checks a package and returns the resulting package object and the
first error if any. Additionally, if info != nil, Check populates each of the
non-nil maps in the Info struct.

The package is marked as complete if no errors occurred, otherwise it is
incomplete. See Config.Error for controlling behavior in the presence of errors.

The package is specified by a list of *ast.Files and corresponding file set, and
the package path the package is identified with. The clean path must not be
empty or dot (".").

<h2 id="Const">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L129">Const</a>
    <a href="#Const">¶</a></h2>
<pre>type Const struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Const represents a declared constant.

<h3 id="NewConst">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L135">NewConst</a>
    <a href="#NewConst">¶</a></h3>
<pre>func NewConst(pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>, typ <a href="#Type">Type</a>, val <a href="/go/constant/">constant</a>.<a href="/go/constant/#Value">Value</a>) *<a href="#Const">Const</a></pre>


<h3 id="Const.Exported">func (*Const) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L81">Exported</a>
    <a href="#Const.Exported">¶</a></h3>
<pre>func (obj *<a href="#Const">Const</a>) Exported() <a href="/builtin/#bool">bool</a></pre>


<h3 id="Const.Id">func (*Const) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L82">Id</a>
    <a href="#Const.Id">¶</a></h3>
<pre>func (obj *<a href="#Const">Const</a>) Id() <a href="/builtin/#string">string</a></pre>


<h3 id="Const.Name">func (*Const) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L79">Name</a>
    <a href="#Const.Name">¶</a></h3>
<pre>func (obj *<a href="#Const">Const</a>) Name() <a href="/builtin/#string">string</a></pre>


<h3 id="Const.Parent">func (*Const) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L76">Parent</a>
    <a href="#Const.Parent">¶</a></h3>
<pre>func (obj *<a href="#Const">Const</a>) Parent() *<a href="#Scope">Scope</a></pre>


<h3 id="Const.Pkg">func (*Const) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L78">Pkg</a>
    <a href="#Const.Pkg">¶</a></h3>
<pre>func (obj *<a href="#Const">Const</a>) Pkg() *<a href="#Package">Package</a></pre>


<h3 id="Const.Pos">func (*Const) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L77">Pos</a>
    <a href="#Const.Pos">¶</a></h3>
<pre>func (obj *<a href="#Const">Const</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Const.String">func (*Const) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L359">String</a>
    <a href="#Const.String">¶</a></h3>
<pre>func (obj *<a href="#Const">Const</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Const.Type">func (*Const) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L80">Type</a>
    <a href="#Const.Type">¶</a></h3>
<pre>func (obj *<a href="#Const">Const</a>) Type() <a href="#Type">Type</a></pre>


<h3 id="Const.Val">func (*Const) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L139">Val</a>
    <a href="#Const.Val">¶</a></h3>
<pre>func (obj *<a href="#Const">Const</a>) Val() <a href="/go/constant/">constant</a>.<a href="/go/constant/#Value">Value</a></pre>


<h2 id="Error">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L31">Error</a>
    <a href="#Error">¶</a></h2>
<pre>type Error struct {
<span id="Error.Fset"></span>    Fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a> <span class="comment">// file set for interpretation of Pos</span>
<span id="Error.Pos"></span>    Pos  <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>      <span class="comment">// error position</span>
<span id="Error.Msg"></span>    Msg  <a href="/builtin/#string">string</a>         <span class="comment">// error message</span>
<span id="Error.Soft"></span>    Soft <a href="/builtin/#bool">bool</a>           <span class="comment">// if set, error is &#34;soft&#34;</span>
}</pre>

An Error describes a type-checking error; it implements the error interface. A
"soft" error is an error that still permits a valid interpretation of a package
(such as "unused variable"); "hard" errors may lead to unpredictable behavior if
ignored.

<h3 id="Error.Error">func (Error) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L40">Error</a>
    <a href="#Error.Error">¶</a></h3>
<pre>func (err <a href="#Error">Error</a>) Error() <a href="/builtin/#string">string</a></pre>

Error returns an error string formatted as follows: filename:line:column:
message

<h2 id="Func">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L203">Func</a>
    <a href="#Func">¶</a></h2>
<pre>type Func struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Func represents a declared function, concrete method, or abstract (interface)
method. Its Type() is always a *Signature. An abstract method may belong to many
interfaces due to embedding.

<h3 id="MissingMethod">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/lookup.go#L240">MissingMethod</a>
    <a href="#MissingMethod">¶</a></h3>
<pre>func MissingMethod(V <a href="#Type">Type</a>, T *<a href="#Interface">Interface</a>, static <a href="/builtin/#bool">bool</a>) (method *<a href="#Func">Func</a>, wrongType <a href="/builtin/#bool">bool</a>)</pre>

MissingMethod returns (nil, false) if V implements T, otherwise it returns a
missing method required by T and whether it is missing or just has the wrong
type.

For non-interface types V, or if static is set, V implements T if all methods of
T are present in V. Otherwise (V is an interface and static is not set),
MissingMethod only checks that methods of T which are also present in V have
matching types (e.g., for a type assertion x.(T) where x is of interface type
V).

<h3 id="NewFunc">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L207">NewFunc</a>
    <a href="#NewFunc">¶</a></h3>
<pre>func NewFunc(pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>, sig *<a href="#Signature">Signature</a>) *<a href="#Func">Func</a></pre>


<h3 id="Func.Exported">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L81">Exported</a>
    <a href="#Func.Exported">¶</a></h3>
<pre>func (obj *<a href="#Func">Func</a>) Exported() <a href="/builtin/#bool">bool</a></pre>


<h3 id="Func.FullName">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L218">FullName</a>
    <a href="#Func.FullName">¶</a></h3>
<pre>func (obj *<a href="#Func">Func</a>) FullName() <a href="/builtin/#string">string</a></pre>

FullName returns the package- or receiver-type-qualified name of function or
method obj.

<h3 id="Func.Id">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L82">Id</a>
    <a href="#Func.Id">¶</a></h3>
<pre>func (obj *<a href="#Func">Func</a>) Id() <a href="/builtin/#string">string</a></pre>


<h3 id="Func.Name">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L79">Name</a>
    <a href="#Func.Name">¶</a></h3>
<pre>func (obj *<a href="#Func">Func</a>) Name() <a href="/builtin/#string">string</a></pre>


<h3 id="Func.Parent">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L76">Parent</a>
    <a href="#Func.Parent">¶</a></h3>
<pre>func (obj *<a href="#Func">Func</a>) Parent() *<a href="#Scope">Scope</a></pre>


<h3 id="Func.Pkg">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L78">Pkg</a>
    <a href="#Func.Pkg">¶</a></h3>
<pre>func (obj *<a href="#Func">Func</a>) Pkg() *<a href="#Package">Package</a></pre>


<h3 id="Func.Pos">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L77">Pos</a>
    <a href="#Func.Pos">¶</a></h3>
<pre>func (obj *<a href="#Func">Func</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Func.Scope">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L224">Scope</a>
    <a href="#Func.Scope">¶</a></h3>
<pre>func (obj *<a href="#Func">Func</a>) Scope() *<a href="#Scope">Scope</a></pre>


<h3 id="Func.String">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L362">String</a>
    <a href="#Func.String">¶</a></h3>
<pre>func (obj *<a href="#Func">Func</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Func.Type">func (*Func) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L80">Type</a>
    <a href="#Func.Type">¶</a></h3>
<pre>func (obj *<a href="#Func">Func</a>) Type() <a href="#Type">Type</a></pre>


<h2 id="ImportMode">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L57">ImportMode</a>
    <a href="#ImportMode">¶</a></h2>
<pre>type ImportMode <a href="/builtin/#int">int</a></pre>

ImportMode is reserved for future use.

<h2 id="Importer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L49">Importer</a>
    <a href="#Importer">¶</a></h2>
<pre>type Importer interface {
    <span class="comment">// Import returns the imported package for the given import path.</span>
    <span class="comment">// The semantics is like for ImporterFrom.ImportFrom except that</span>
    <span class="comment">// dir and mode are ignored (since they are not present).</span>
    Import(path <a href="/builtin/#string">string</a>) (*<a href="#Package">Package</a>, <a href="/builtin/#error">error</a>)
}</pre>

An Importer resolves import paths to Packages.

CAUTION: This interface does not support the import of locally vendored
packages. See https://golang.org/s/go15vendor. If possible, external
implementations should implement ImporterFrom.

<h2 id="ImporterFrom">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L62">ImporterFrom</a>
    <a href="#ImporterFrom">¶</a></h2>
<pre>type ImporterFrom interface {
    <span class="comment">// Importer is present for backward-compatibility. Calling</span>
    <span class="comment">// Import(path) is the same as calling ImportFrom(path, &#34;&#34;, 0);</span>
    <span class="comment">// i.e., locally vendored packages may not be found.</span>
    <span class="comment">// The types package does not call Import if an ImporterFrom</span>
    <span class="comment">// is present.</span>
    <a href="#Importer">Importer</a>

    <span class="comment">// ImportFrom returns the imported package for the given import</span>
    <span class="comment">// path when imported by a package file located in dir.</span>
    <span class="comment">// If the import failed, besides returning an error, ImportFrom</span>
    <span class="comment">// is encouraged to cache and return a package anyway, if one</span>
    <span class="comment">// was created. This will reduce package inconsistencies and</span>
    <span class="comment">// follow-on type checker errors due to the missing package.</span>
    <span class="comment">// The mode value must be 0; it is reserved for future use.</span>
    <span class="comment">// Two calls to ImportFrom with the same path and dir must</span>
    <span class="comment">// return the same package.</span>
    ImportFrom(path, dir <a href="/builtin/#string">string</a>, mode <a href="#ImportMode">ImportMode</a>) (*<a href="#Package">Package</a>, <a href="/builtin/#error">error</a>)
}</pre>

An ImporterFrom resolves import paths to packages; it supports vendoring per
https://golang.org/s/go15vendor. Use go/importer to obtain an ImporterFrom
implementation.

<h2 id="Info">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L128">Info</a>
    <a href="#Info">¶</a></h2>
<pre>type Info struct {
<span id="Info.Types"></span>    <span class="comment">// Types maps expressions to their types, and for constant</span>
    <span class="comment">// expressions, also their values. Invalid expressions are</span>
    <span class="comment">// omitted.</span>
    <span class="comment">//</span>
    <span class="comment">// For (possibly parenthesized) identifiers denoting built-in</span>
    <span class="comment">// functions, the recorded signatures are call-site specific:</span>
    <span class="comment">// if the call result is not a constant, the recorded type is</span>
    <span class="comment">// an argument-specific signature. Otherwise, the recorded type</span>
    <span class="comment">// is invalid.</span>
    <span class="comment">//</span>
    <span class="comment">// The Types map does not record the type of every identifier,</span>
    <span class="comment">// only those that appear where an arbitrary expression is</span>
    <span class="comment">// permitted. For instance, the identifier f in a selector</span>
    <span class="comment">// expression x.f is found only in the Selections map, the</span>
    <span class="comment">// identifier z in a variable declaration &#39;var z int&#39; is found</span>
    <span class="comment">// only in the Defs map, and identifiers denoting packages in</span>
    <span class="comment">// qualified identifiers are collected in the Uses map.</span>
    Types map[<a href="/go/ast/">ast</a>.<a href="/go/ast/#Expr">Expr</a>]<a href="#TypeAndValue">TypeAndValue</a>

<span id="Info.Defs"></span>    <span class="comment">// Defs maps identifiers to the objects they define (including</span>
    <span class="comment">// package names, dots &#34;.&#34; of dot-imports, and blank &#34;_&#34; identifiers).</span>
    <span class="comment">// For identifiers that do not denote objects (e.g., the package name</span>
    <span class="comment">// in package clauses, or symbolic variables t in t := x.(type) of</span>
    <span class="comment">// type switch headers), the corresponding objects are nil.</span>
    <span class="comment">//</span>
    <span class="comment">// For an anonymous field, Defs returns the field *Var it defines.</span>
    <span class="comment">//</span>
    <span class="comment">// Invariant: Defs[id] == nil || Defs[id].Pos() == id.Pos()</span>
    Defs map[*<a href="/go/ast/">ast</a>.<a href="/go/ast/#Ident">Ident</a>]<a href="#Object">Object</a>

<span id="Info.Uses"></span>    <span class="comment">// Uses maps identifiers to the objects they denote.</span>
    <span class="comment">//</span>
    <span class="comment">// For an anonymous field, Uses returns the *TypeName it denotes.</span>
    <span class="comment">//</span>
    <span class="comment">// Invariant: Uses[id].Pos() != id.Pos()</span>
    Uses map[*<a href="/go/ast/">ast</a>.<a href="/go/ast/#Ident">Ident</a>]<a href="#Object">Object</a>

<span id="Info.Implicits"></span>    <span class="comment">// Implicits maps nodes to their implicitly declared objects, if any.</span>
    <span class="comment">// The following node and object types may appear:</span>
    <span class="comment">//</span>
    <span class="comment">//	node               declared object</span>
    <span class="comment">//</span>
    <span class="comment">//	*ast.ImportSpec    *PkgName for dot-imports and imports without renames</span>
    <span class="comment">//	*ast.CaseClause    type-specific *Var for each type switch case clause (incl. default)</span>
    <span class="comment">//      *ast.Field         anonymous parameter *Var</span>
    <span class="comment">//</span>
    Implicits map[<a href="/go/ast/">ast</a>.<a href="/go/ast/#Node">Node</a>]<a href="#Object">Object</a>

<span id="Info.Selections"></span>    <span class="comment">// Selections maps selector expressions (excluding qualified identifiers)</span>
    <span class="comment">// to their corresponding selections.</span>
    Selections map[*<a href="/go/ast/">ast</a>.<a href="/go/ast/#SelectorExpr">SelectorExpr</a>]*<a href="#Selection">Selection</a>

<span id="Info.Scopes"></span>    <span class="comment">// Scopes maps ast.Nodes to the scopes they define. Package scopes are not</span>
    <span class="comment">// associated with a specific node but with all files belonging to a package.</span>
    <span class="comment">// Thus, the package scope can be found in the type-checked Package object.</span>
    <span class="comment">// Scopes nest, with the Universe scope being the outermost scope, enclosing</span>
    <span class="comment">// the package scope, which contains (one or more) files scopes, which enclose</span>
    <span class="comment">// function scopes which in turn enclose statement and function literal scopes.</span>
    <span class="comment">// Note that even though package-level functions are declared in the package</span>
    <span class="comment">// scope, the function scopes are embedded in the file scope of the file</span>
    <span class="comment">// containing the function declaration.</span>
    <span class="comment">//</span>
    <span class="comment">// The following node types may appear in Scopes:</span>
    <span class="comment">//</span>
    <span class="comment">//	*ast.File</span>
    <span class="comment">//	*ast.FuncType</span>
    <span class="comment">//	*ast.BlockStmt</span>
    <span class="comment">//	*ast.IfStmt</span>
    <span class="comment">//	*ast.SwitchStmt</span>
    <span class="comment">//	*ast.TypeSwitchStmt</span>
    <span class="comment">//	*ast.CaseClause</span>
    <span class="comment">//	*ast.CommClause</span>
    <span class="comment">//	*ast.ForStmt</span>
    <span class="comment">//	*ast.RangeStmt</span>
    <span class="comment">//</span>
    Scopes map[<a href="/go/ast/">ast</a>.<a href="/go/ast/#Node">Node</a>]*<a href="#Scope">Scope</a>

<span id="Info.InitOrder"></span>    <span class="comment">// InitOrder is the list of package-level initializers in the order in which</span>
    <span class="comment">// they must be executed. Initializers referring to variables related by an</span>
    <span class="comment">// initialization dependency appear in topological order, the others appear</span>
    <span class="comment">// in source order. Variables without an initialization expression do not</span>
    <span class="comment">// appear in this list.</span>
    InitOrder []*<a href="#Initializer">Initializer</a>
}</pre>

Info holds result type information for a type-checked package. Only the
information for which a map is provided is collected. If the package has type
errors, the collected information may be incomplete.

<a id="exampleInfo"></a>
Example:

    // Parse a single source file.
    const input = `
    package fib
    
    type S string
    
    var a, b, c = len(b), S(c), "hello"
    
    func fib(x int) int {
    	if x < 2 {
    		return x
    	}
    	return fib(x-1) - fib(x-2)
    }`
    fset := token.NewFileSet()
    f, err := parser.ParseFile(fset, "fib.go", input, 0)
    if err != nil {
        log.Fatal(err)
    }

    // Type-check the package.
    // We create an empty map for each kind of input
    // we're interested in, and Check populates them.
    info := types.Info{
        Types: make(map[ast.Expr]types.TypeAndValue),
        Defs:  make(map[*ast.Ident]types.Object),
        Uses:  make(map[*ast.Ident]types.Object),
    }
    var conf types.Config
    pkg, err := conf.Check("fib", fset, []*ast.File{f}, &info)
    if err != nil {
        log.Fatal(err)
    }

    // Print package-level variables in initialization order.
    fmt.Printf("InitOrder: %v\n\n", info.InitOrder)

    // For each named object, print the line and
    // column of its definition and each of its uses.
    fmt.Println("Defs and Uses of each named object:")
    usesByObj := make(map[types.Object][]string)
    for id, obj := range info.Uses {
        posn := fset.Position(id.Pos())
        lineCol := fmt.Sprintf("%d:%d", posn.Line, posn.Column)
        usesByObj[obj] = append(usesByObj[obj], lineCol)
    }
    var items []string
    for obj, uses := range usesByObj {
        sort.Strings(uses)
        item := fmt.Sprintf("%s:\n  defined at %s\n  used at %s",
            types.ObjectString(obj, types.RelativeTo(pkg)),
            fset.Position(obj.Pos()),
            strings.Join(uses, ", "))
        items = append(items, item)
    }
    sort.Strings(items) // sort by line:col, in effect
    fmt.Println(strings.Join(items, "\n"))
    fmt.Println()

    fmt.Println("Types and Values of each expression:")
    items = nil
    for expr, tv := range info.Types {
        var buf bytes.Buffer
        posn := fset.Position(expr.Pos())
        tvstr := tv.Type.String()
        if tv.Value != nil {
            tvstr += " = " + tv.Value.String()
        }
        // line:col | expr | mode : type = value
        fmt.Fprintf(&buf, "%2d:%2d | %-19s | %-7s : %s",
            posn.Line, posn.Column, exprString(fset, expr),
            mode(tv), tvstr)
        items = append(items, buf.String())
    }
    sort.Strings(items)
    fmt.Println(strings.Join(items, "\n"))

    // Output:
    // InitOrder: [c = "hello" b = S(c) a = len(b)]
    //
    // Defs and Uses of each named object:
    // builtin len:
    //   defined at -
    //   used at 6:15
    // func fib(x int) int:
    //   defined at fib.go:8:6
    //   used at 12:20, 12:9
    // type S string:
    //   defined at fib.go:4:6
    //   used at 6:23
    // type int:
    //   defined at -
    //   used at 8:12, 8:17
    // type string:
    //   defined at -
    //   used at 4:8
    // var b S:
    //   defined at fib.go:6:8
    //   used at 6:19
    // var c string:
    //   defined at fib.go:6:11
    //   used at 6:25
    // var x int:
    //   defined at fib.go:8:10
    //   used at 10:10, 12:13, 12:24, 9:5
    //
    // Types and Values of each expression:
    //  4: 8 | string              | type    : string
    //  6:15 | len                 | builtin : func(string) int
    //  6:15 | len(b)              | value   : int
    //  6:19 | b                   | var     : fib.S
    //  6:23 | S                   | type    : fib.S
    //  6:23 | S(c)                | value   : fib.S
    //  6:25 | c                   | var     : string
    //  6:29 | "hello"             | value   : string = "hello"
    //  8:12 | int                 | type    : int
    //  8:17 | int                 | type    : int
    //  9: 5 | x                   | var     : int
    //  9: 5 | x < 2               | value   : untyped bool
    //  9: 9 | 2                   | value   : int = 2
    // 10:10 | x                   | var     : int
    // 12: 9 | fib                 | value   : func(x int) int
    // 12: 9 | fib(x - 1)          | value   : int
    // 12: 9 | fib(x-1) - fib(x-2) | value   : int
    // 12:13 | x                   | var     : int
    // 12:13 | x - 1               | value   : int
    // 12:15 | 1                   | value   : int = 1
    // 12:20 | fib                 | value   : func(x int) int
    // 12:20 | fib(x - 2)          | value   : int
    // 12:24 | x                   | var     : int
    // 12:24 | x - 2               | value   : int
    // 12:26 | 2                   | value   : int = 2

<h3 id="Info.ObjectOf">func (*Info) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L237">ObjectOf</a>
    <a href="#Info.ObjectOf">¶</a></h3>
<pre>func (info *<a href="#Info">Info</a>) ObjectOf(id *<a href="/go/ast/">ast</a>.<a href="/go/ast/#Ident">Ident</a>) <a href="#Object">Object</a></pre>

ObjectOf returns the object denoted by the specified id, or nil if not found.

If id is an anonymous struct field, ObjectOf returns the field (*Var) it uses,
not the type (*TypeName) it defines.

Precondition: the Uses and Defs maps are populated.

<h3 id="Info.TypeOf">func (*Info) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L217">TypeOf</a>
    <a href="#Info.TypeOf">¶</a></h3>
<pre>func (info *<a href="#Info">Info</a>) TypeOf(e <a href="/go/ast/">ast</a>.<a href="/go/ast/#Expr">Expr</a>) <a href="#Type">Type</a></pre>

TypeOf returns the type of expression e, or nil if not found. Precondition: the
Types, Uses and Defs maps are populated.

<h2 id="Initializer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L310">Initializer</a>
    <a href="#Initializer">¶</a></h2>
<pre>type Initializer struct {
<span id="Initializer.Lhs"></span>    Lhs []*<a href="#Var">Var</a> <span class="comment">// var Lhs = Rhs</span>
<span id="Initializer.Rhs"></span>    Rhs <a href="/go/ast/">ast</a>.<a href="/go/ast/#Expr">Expr</a>
}</pre>

An Initializer describes a package-level variable, or a list of variables in
case of a multi-valued initialization expression, and the corresponding
initialization expression.

<h3 id="Initializer.String">func (*Initializer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L315">String</a>
    <a href="#Initializer.String">¶</a></h3>
<pre>func (init *<a href="#Initializer">Initializer</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Interface">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L231">Interface</a>
    <a href="#Interface">¶</a></h2>
<pre>type Interface struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

An Interface represents an interface type.

<h3 id="NewInterface">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L239">NewInterface</a>
    <a href="#NewInterface">¶</a></h3>
<pre>func NewInterface(methods []*<a href="#Func">Func</a>, embeddeds []*<a href="#Named">Named</a>) *<a href="#Interface">Interface</a></pre>

NewInterface returns a new interface for the given methods and embedded types.

<h3 id="Interface.Complete">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L291">Complete</a>
    <a href="#Interface.Complete">¶</a></h3>
<pre>func (t *<a href="#Interface">Interface</a>) Complete() *<a href="#Interface">Interface</a></pre>

Complete computes the interface's method set. It must be called by users of
NewInterface after the interface's embedded types are fully defined and before
using the interface type in any way other than to form other types. Complete
returns the receiver.

<h3 id="Interface.Embedded">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L275">Embedded</a>
    <a href="#Interface.Embedded">¶</a></h3>
<pre>func (t *<a href="#Interface">Interface</a>) Embedded(i <a href="/builtin/#int">int</a>) *<a href="#Named">Named</a></pre>

Embedded returns the i'th embedded type of interface t for 0 <= i <
t.NumEmbeddeds(). The types are ordered by the corresponding TypeName's unique
Id.

<h3 id="Interface.Empty">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L285">Empty</a>
    <a href="#Interface.Empty">¶</a></h3>
<pre>func (t *<a href="#Interface">Interface</a>) Empty() <a href="/builtin/#bool">bool</a></pre>

Empty returns true if t is the empty interface.

<h3 id="Interface.ExplicitMethod">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L268">ExplicitMethod</a>
    <a href="#Interface.ExplicitMethod">¶</a></h3>
<pre>func (t *<a href="#Interface">Interface</a>) ExplicitMethod(i <a href="/builtin/#int">int</a>) *<a href="#Func">Func</a></pre>

ExplicitMethod returns the i'th explicitly declared method of interface t for 0
<= i < t.NumExplicitMethods(). The methods are ordered by their unique Id.

<h3 id="Interface.Method">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L282">Method</a>
    <a href="#Interface.Method">¶</a></h3>
<pre>func (t *<a href="#Interface">Interface</a>) Method(i <a href="/builtin/#int">int</a>) *<a href="#Func">Func</a></pre>

Method returns the i'th method of interface t for 0 <= i < t.NumMethods(). The
methods are ordered by their unique Id.

<h3 id="Interface.NumEmbeddeds">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L271">NumEmbeddeds</a>
    <a href="#Interface.NumEmbeddeds">¶</a></h3>
<pre>func (t *<a href="#Interface">Interface</a>) NumEmbeddeds() <a href="/builtin/#int">int</a></pre>

NumEmbeddeds returns the number of embedded types in interface t.

<h3 id="Interface.NumExplicitMethods">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L264">NumExplicitMethods</a>
    <a href="#Interface.NumExplicitMethods">¶</a></h3>
<pre>func (t *<a href="#Interface">Interface</a>) NumExplicitMethods() <a href="/builtin/#int">int</a></pre>

NumExplicitMethods returns the number of explicitly declared methods of
interface t.

<h3 id="Interface.NumMethods">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L278">NumMethods</a>
    <a href="#Interface.NumMethods">¶</a></h3>
<pre>func (t *<a href="#Interface">Interface</a>) NumMethods() <a href="/builtin/#int">int</a></pre>

NumMethods returns the total number of methods of interface t.

<h3 id="Interface.String">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L437">String</a>
    <a href="#Interface.String">¶</a></h3>
<pre>func (t *<a href="#Interface">Interface</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Interface.Underlying">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L425">Underlying</a>
    <a href="#Interface.Underlying">¶</a></h3>
<pre>func (t *<a href="#Interface">Interface</a>) Underlying() <a href="#Type">Type</a></pre>


<h2 id="Label">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L228">Label</a>
    <a href="#Label">¶</a></h2>
<pre>type Label struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Label represents a declared label.

<h3 id="NewLabel">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L233">NewLabel</a>
    <a href="#NewLabel">¶</a></h3>
<pre>func NewLabel(pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>) *<a href="#Label">Label</a></pre>


<h3 id="Label.Exported">func (*Label) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L81">Exported</a>
    <a href="#Label.Exported">¶</a></h3>
<pre>func (obj *<a href="#Label">Label</a>) Exported() <a href="/builtin/#bool">bool</a></pre>


<h3 id="Label.Id">func (*Label) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L82">Id</a>
    <a href="#Label.Id">¶</a></h3>
<pre>func (obj *<a href="#Label">Label</a>) Id() <a href="/builtin/#string">string</a></pre>


<h3 id="Label.Name">func (*Label) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L79">Name</a>
    <a href="#Label.Name">¶</a></h3>
<pre>func (obj *<a href="#Label">Label</a>) Name() <a href="/builtin/#string">string</a></pre>


<h3 id="Label.Parent">func (*Label) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L76">Parent</a>
    <a href="#Label.Parent">¶</a></h3>
<pre>func (obj *<a href="#Label">Label</a>) Parent() *<a href="#Scope">Scope</a></pre>


<h3 id="Label.Pkg">func (*Label) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L78">Pkg</a>
    <a href="#Label.Pkg">¶</a></h3>
<pre>func (obj *<a href="#Label">Label</a>) Pkg() *<a href="#Package">Package</a></pre>


<h3 id="Label.Pos">func (*Label) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L77">Pos</a>
    <a href="#Label.Pos">¶</a></h3>
<pre>func (obj *<a href="#Label">Label</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Label.String">func (*Label) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L363">String</a>
    <a href="#Label.String">¶</a></h3>
<pre>func (obj *<a href="#Label">Label</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Label.Type">func (*Label) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L80">Type</a>
    <a href="#Label.Type">¶</a></h3>
<pre>func (obj *<a href="#Label">Label</a>) Type() <a href="#Type">Type</a></pre>


<h2 id="Map">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L325">Map</a>
    <a href="#Map">¶</a></h2>
<pre>type Map struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Map represents a map type.

<h3 id="NewMap">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L330">NewMap</a>
    <a href="#NewMap">¶</a></h3>
<pre>func NewMap(key, elem <a href="#Type">Type</a>) *<a href="#Map">Map</a></pre>

NewMap returns a new map for the given key and element types.

<h3 id="Map.Elem">func (*Map) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L338">Elem</a>
    <a href="#Map.Elem">¶</a></h3>
<pre>func (m *<a href="#Map">Map</a>) Elem() <a href="#Type">Type</a></pre>

Elem returns the element type of map m.

<h3 id="Map.Key">func (*Map) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L335">Key</a>
    <a href="#Map.Key">¶</a></h3>
<pre>func (m *<a href="#Map">Map</a>) Key() <a href="#Type">Type</a></pre>

Key returns the key type of map m.

<h3 id="Map.String">func (*Map) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L438">String</a>
    <a href="#Map.String">¶</a></h3>
<pre>func (t *<a href="#Map">Map</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Map.Underlying">func (*Map) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L426">Underlying</a>
    <a href="#Map.Underlying">¶</a></h3>
<pre>func (t *<a href="#Map">Map</a>) Underlying() <a href="#Type">Type</a></pre>


<h2 id="MethodSet">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/methodset.go#L8">MethodSet</a>
    <a href="#MethodSet">¶</a></h2>
<pre>type MethodSet struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A MethodSet is an ordered set of concrete or abstract (interface) methods; a
method is a MethodVal selection, and they are ordered by ascending m.Obj().Id().
The zero value for a MethodSet is a ready-to-use empty method set.

<a id="exampleMethodSet"></a>
Example:

    // Parse a single source file.
    const input = `
    package temperature
    import "fmt"
    type Celsius float64
    func (c Celsius) String() string  { return fmt.Sprintf("%g°C", c) }
    func (c *Celsius) SetF(f float64) { *c = Celsius(f - 32 / 9 * 5) }
    `
    fset := token.NewFileSet()
    f, err := parser.ParseFile(fset, "celsius.go", input, 0)
    if err != nil {
        log.Fatal(err)
    }

    // Type-check a package consisting of this file.
    // Type information for the imported packages
    // comes from $GOROOT/pkg/$GOOS_$GOOARCH/fmt.a.
    conf := types.Config{Importer: importer.Default()}
    pkg, err := conf.Check("temperature", fset, []*ast.File{f}, nil)
    if err != nil {
        log.Fatal(err)
    }

    // Print the method sets of Celsius and *Celsius.
    celsius := pkg.Scope().Lookup("Celsius").Type()
    for _, t := range []types.Type{celsius, types.NewPointer(celsius)} {
        fmt.Printf("Method set of %s:\n", t)
        mset := types.NewMethodSet(t)
        for i := 0; i < mset.Len(); i++ {
            fmt.Println(mset.At(i))
        }
        fmt.Println()
    }

    // Output:
    // Method set of temperature.Celsius:
    // method (temperature.Celsius) String() string
    //
    // Method set of *temperature.Celsius:
    // method (*temperature.Celsius) SetF(f float64)
    // method (*temperature.Celsius) String() string

<h3 id="NewMethodSet">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/methodset.go#L57">NewMethodSet</a>
    <a href="#NewMethodSet">¶</a></h3>
<pre>func NewMethodSet(T <a href="#Type">Type</a>) *<a href="#MethodSet">MethodSet</a></pre>

NewMethodSet returns the method set for the given type T. It always returns a
non-nil method set, even if it is empty.

<h3 id="MethodSet.At">func (*MethodSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/methodset.go#L30">At</a>
    <a href="#MethodSet.At">¶</a></h3>
<pre>func (s *<a href="#MethodSet">MethodSet</a>) At(i <a href="/builtin/#int">int</a>) *<a href="#Selection">Selection</a></pre>

At returns the i'th method in s for 0 <= i < s.Len().

<h3 id="MethodSet.Len">func (*MethodSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/methodset.go#L27">Len</a>
    <a href="#MethodSet.Len">¶</a></h3>
<pre>func (s *<a href="#MethodSet">MethodSet</a>) Len() <a href="/builtin/#int">int</a></pre>

Len returns the number of methods in s.

<h3 id="MethodSet.Lookup">func (*MethodSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/methodset.go#L33">Lookup</a>
    <a href="#MethodSet.Lookup">¶</a></h3>
<pre>func (s *<a href="#MethodSet">MethodSet</a>) Lookup(pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>) *<a href="#Selection">Selection</a></pre>

Lookup returns the method with matching package and name, or nil if not found.

<h3 id="MethodSet.String">func (*MethodSet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/methodset.go#L12">String</a>
    <a href="#MethodSet.String">¶</a></h3>
<pre>func (s *<a href="#MethodSet">MethodSet</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Named">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L368">Named</a>
    <a href="#Named">¶</a></h2>
<pre>type Named struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Named represents a named type.

<h3 id="NewNamed">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L376">NewNamed</a>
    <a href="#NewNamed">¶</a></h3>
<pre>func NewNamed(obj *<a href="#TypeName">TypeName</a>, underlying <a href="#Type">Type</a>, methods []*<a href="#Func">Func</a>) *<a href="#Named">Named</a></pre>

NewNamed returns a new named type for the given type name, underlying type, and
associated methods. The underlying type must not be a *Named.

<h3 id="Named.AddMethod">func (*Named) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L410">AddMethod</a>
    <a href="#Named.AddMethod">¶</a></h3>
<pre>func (t *<a href="#Named">Named</a>) AddMethod(m *<a href="#Func">Func</a>)</pre>

AddMethod adds method m unless it is already in the method list. TODO(gri) find
a better solution instead of providing this function

<h3 id="Named.Method">func (*Named) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L394">Method</a>
    <a href="#Named.Method">¶</a></h3>
<pre>func (t *<a href="#Named">Named</a>) Method(i <a href="/builtin/#int">int</a>) *<a href="#Func">Func</a></pre>

Method returns the i'th method of named type t for 0 <= i < t.NumMethods().

<h3 id="Named.NumMethods">func (*Named) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L391">NumMethods</a>
    <a href="#Named.NumMethods">¶</a></h3>
<pre>func (t *<a href="#Named">Named</a>) NumMethods() <a href="/builtin/#int">int</a></pre>

NumMethods returns the number of explicit methods whose receiver is named type
t.

<h3 id="Named.Obj">func (*Named) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L388">Obj</a>
    <a href="#Named.Obj">¶</a></h3>
<pre>func (t *<a href="#Named">Named</a>) Obj() *<a href="#TypeName">TypeName</a></pre>

Obj returns the type name for the named type t.

<h3 id="Named.SetUnderlying">func (*Named) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L398">SetUnderlying</a>
    <a href="#Named.SetUnderlying">¶</a></h3>
<pre>func (t *<a href="#Named">Named</a>) SetUnderlying(underlying <a href="#Type">Type</a>)</pre>

SetUnderlying sets the underlying type and marks t as complete. TODO(gri)
determine if there's a better solution rather than providing this function

<h3 id="Named.String">func (*Named) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L440">String</a>
    <a href="#Named.String">¶</a></h3>
<pre>func (t *<a href="#Named">Named</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Named.Underlying">func (*Named) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L428">Underlying</a>
    <a href="#Named.Underlying">¶</a></h3>
<pre>func (t *<a href="#Named">Named</a>) Underlying() <a href="#Type">Type</a></pre>


<h2 id="Nil">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L249">Nil</a>
    <a href="#Nil">¶</a></h2>
<pre>type Nil struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Nil represents the predeclared value nil.

<h3 id="Nil.Exported">func (*Nil) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L81">Exported</a>
    <a href="#Nil.Exported">¶</a></h3>
<pre>func (obj *<a href="#Nil">Nil</a>) Exported() <a href="/builtin/#bool">bool</a></pre>


<h3 id="Nil.Id">func (*Nil) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L82">Id</a>
    <a href="#Nil.Id">¶</a></h3>
<pre>func (obj *<a href="#Nil">Nil</a>) Id() <a href="/builtin/#string">string</a></pre>


<h3 id="Nil.Name">func (*Nil) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L79">Name</a>
    <a href="#Nil.Name">¶</a></h3>
<pre>func (obj *<a href="#Nil">Nil</a>) Name() <a href="/builtin/#string">string</a></pre>


<h3 id="Nil.Parent">func (*Nil) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L76">Parent</a>
    <a href="#Nil.Parent">¶</a></h3>
<pre>func (obj *<a href="#Nil">Nil</a>) Parent() *<a href="#Scope">Scope</a></pre>


<h3 id="Nil.Pkg">func (*Nil) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L78">Pkg</a>
    <a href="#Nil.Pkg">¶</a></h3>
<pre>func (obj *<a href="#Nil">Nil</a>) Pkg() *<a href="#Package">Package</a></pre>


<h3 id="Nil.Pos">func (*Nil) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L77">Pos</a>
    <a href="#Nil.Pos">¶</a></h3>
<pre>func (obj *<a href="#Nil">Nil</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Nil.String">func (*Nil) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L365">String</a>
    <a href="#Nil.String">¶</a></h3>
<pre>func (obj *<a href="#Nil">Nil</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Nil.Type">func (*Nil) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L80">Type</a>
    <a href="#Nil.Type">¶</a></h3>
<pre>func (obj *<a href="#Nil">Nil</a>) Type() <a href="#Type">Type</a></pre>


<h2 id="Object">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L11">Object</a>
    <a href="#Object">¶</a></h2>
<pre>type Object interface {
    Parent() *<a href="#Scope">Scope</a> <span class="comment">// scope in which this object is declared; nil for methods and struct fields</span>
    Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a> <span class="comment">// position of object identifier in declaration</span>
    Pkg() *<a href="#Package">Package</a>  <span class="comment">// nil for objects in the Universe scope and labels</span>
    Name() <a href="/builtin/#string">string</a>   <span class="comment">// package local object name</span>
    Type() <a href="#Type">Type</a>     <span class="comment">// object type</span>
    Exported() <a href="/builtin/#bool">bool</a> <span class="comment">// reports whether the name starts with a capital letter</span>
    Id() <a href="/builtin/#string">string</a>     <span class="comment">// object name if exported, qualified name if not exported (see func Id)</span>

    <span class="comment">// String returns a human-readable string of the object.</span>
    String() <a href="/builtin/#string">string</a>
    <span class="comment">// contains filtered or unexported methods</span>
}</pre>

An Object describes a named language entity such as a package, constant, type,
variable, function (incl. methods), or label. All objects implement the Object
interface.

<h3 id="LookupFieldOrMethod">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/lookup.go#L25">LookupFieldOrMethod</a>
    <a href="#LookupFieldOrMethod">¶</a></h3>
<pre>func LookupFieldOrMethod(T <a href="#Type">Type</a>, addressable <a href="/builtin/#bool">bool</a>, pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>) (obj <a href="#Object">Object</a>, index []<a href="/builtin/#int">int</a>, indirect <a href="/builtin/#bool">bool</a>)</pre>

LookupFieldOrMethod looks up a field or method with given package and name in T
and returns the corresponding *Var or *Func, an index sequence, and a bool
indicating if there were any pointer indirections on the path to the field or
method. If addressable is set, T is the type of an addressable variable (only
matters for method lookups).

The last index entry is the field or method index in the (possibly embedded)
type where the entry was found, either:

    1) the list of declared methods of a named type; or
    2) the list of all methods (method set) of an interface type; or
    3) the list of fields of a struct type.

The earlier index entries are the indices of the anonymous struct fields
traversed to get to the found entry, starting at depth 0.

If no entry is found, a nil object is returned. In this case, the returned index
and indirect values have the following meaning:

    	- If index != nil, the index sequence points to an ambiguous entry
    	(the same name appeared more than once at the same embedding level).

    	- If indirect is set, a method with a pointer receiver type was found
         but there was no pointer on the path from the actual receiver type to
    	the method's formal receiver base type, nor was the receiver addressable.

<h2 id="Package">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L3">Package</a>
    <a href="#Package">¶</a></h2>
<pre>type Package struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Package describes a Go package.

<h3 id="NewPackage">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L14">NewPackage</a>
    <a href="#NewPackage">¶</a></h3>
<pre>func NewPackage(path, name <a href="/builtin/#string">string</a>) *<a href="#Package">Package</a></pre>

NewPackage returns a new Package for the given package path and name. The
package is not complete and contains no explicit imports.

<h3 id="Package.Complete">func (*Package) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L35">Complete</a>
    <a href="#Package.Complete">¶</a></h3>
<pre>func (pkg *<a href="#Package">Package</a>) Complete() <a href="/builtin/#bool">bool</a></pre>

A package is complete if its scope contains (at least) all exported objects;
otherwise it is incomplete.

<h3 id="Package.Imports">func (*Package) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L46">Imports</a>
    <a href="#Package.Imports">¶</a></h3>
<pre>func (pkg *<a href="#Package">Package</a>) Imports() []*<a href="#Package">Package</a></pre>

Imports returns the list of packages directly imported by pkg; the list is in
source order.

If pkg was loaded from export data, Imports includes packages that provide
package-level objects referenced by pkg. This may be more or less than the set
of packages directly imported by pkg's source code.

<h3 id="Package.MarkComplete">func (*Package) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L38">MarkComplete</a>
    <a href="#Package.MarkComplete">¶</a></h3>
<pre>func (pkg *<a href="#Package">Package</a>) MarkComplete()</pre>

MarkComplete marks a package as complete.

<h3 id="Package.Name">func (*Package) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L23">Name</a>
    <a href="#Package.Name">¶</a></h3>
<pre>func (pkg *<a href="#Package">Package</a>) Name() <a href="/builtin/#string">string</a></pre>

Name returns the package name.

<h3 id="Package.Path">func (*Package) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L20">Path</a>
    <a href="#Package.Path">¶</a></h3>
<pre>func (pkg *<a href="#Package">Package</a>) Path() <a href="/builtin/#string">string</a></pre>

Path returns the package path.

<h3 id="Package.Scope">func (*Package) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L31">Scope</a>
    <a href="#Package.Scope">¶</a></h3>
<pre>func (pkg *<a href="#Package">Package</a>) Scope() *<a href="#Scope">Scope</a></pre>

Scope returns the (complete or incomplete) package scope holding the objects
declared at package level (TypeNames, Consts, Vars, and Funcs).

<h3 id="Package.SetImports">func (*Package) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L50">SetImports</a>
    <a href="#Package.SetImports">¶</a></h3>
<pre>func (pkg *<a href="#Package">Package</a>) SetImports(list []*<a href="#Package">Package</a>)</pre>

SetImports sets the list of explicitly imported packages to list. It is the
caller's responsibility to make sure list elements are unique.

<h3 id="Package.SetName">func (*Package) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L26">SetName</a>
    <a href="#Package.SetName">¶</a></h3>
<pre>func (pkg *<a href="#Package">Package</a>) SetName(name <a href="/builtin/#string">string</a>)</pre>

SetName sets the package name.

<h3 id="Package.String">func (*Package) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/package.go#L52">String</a>
    <a href="#Package.String">¶</a></h3>
<pre>func (pkg *<a href="#Package">Package</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="PkgName">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L114">PkgName</a>
    <a href="#PkgName">¶</a></h2>
<pre>type PkgName struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A PkgName represents an imported Go package.

<h3 id="NewPkgName">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L120">NewPkgName</a>
    <a href="#NewPkgName">¶</a></h3>
<pre>func NewPkgName(pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>, imported *<a href="#Package">Package</a>) *<a href="#PkgName">PkgName</a></pre>


<h3 id="PkgName.Exported">func (*PkgName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L81">Exported</a>
    <a href="#PkgName.Exported">¶</a></h3>
<pre>func (obj *<a href="#PkgName">PkgName</a>) Exported() <a href="/builtin/#bool">bool</a></pre>


<h3 id="PkgName.Id">func (*PkgName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L82">Id</a>
    <a href="#PkgName.Id">¶</a></h3>
<pre>func (obj *<a href="#PkgName">PkgName</a>) Id() <a href="/builtin/#string">string</a></pre>


<h3 id="PkgName.Imported">func (*PkgName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L126">Imported</a>
    <a href="#PkgName.Imported">¶</a></h3>
<pre>func (obj *<a href="#PkgName">PkgName</a>) Imported() *<a href="#Package">Package</a></pre>

Imported returns the package that was imported. It is distinct from Pkg(), which
is the package containing the import statement.

<h3 id="PkgName.Name">func (*PkgName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L79">Name</a>
    <a href="#PkgName.Name">¶</a></h3>
<pre>func (obj *<a href="#PkgName">PkgName</a>) Name() <a href="/builtin/#string">string</a></pre>


<h3 id="PkgName.Parent">func (*PkgName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L76">Parent</a>
    <a href="#PkgName.Parent">¶</a></h3>
<pre>func (obj *<a href="#PkgName">PkgName</a>) Parent() *<a href="#Scope">Scope</a></pre>


<h3 id="PkgName.Pkg">func (*PkgName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L78">Pkg</a>
    <a href="#PkgName.Pkg">¶</a></h3>
<pre>func (obj *<a href="#PkgName">PkgName</a>) Pkg() *<a href="#Package">Package</a></pre>


<h3 id="PkgName.Pos">func (*PkgName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L77">Pos</a>
    <a href="#PkgName.Pos">¶</a></h3>
<pre>func (obj *<a href="#PkgName">PkgName</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="PkgName.String">func (*PkgName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L358">String</a>
    <a href="#PkgName.String">¶</a></h3>
<pre>func (obj *<a href="#PkgName">PkgName</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="PkgName.Type">func (*PkgName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L80">Type</a>
    <a href="#PkgName.Type">¶</a></h3>
<pre>func (obj *<a href="#PkgName">PkgName</a>) Type() <a href="#Type">Type</a></pre>


<h2 id="Pointer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L147">Pointer</a>
    <a href="#Pointer">¶</a></h2>
<pre>type Pointer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Pointer represents a pointer type.

<h3 id="NewPointer">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L152">NewPointer</a>
    <a href="#NewPointer">¶</a></h3>
<pre>func NewPointer(elem <a href="#Type">Type</a>) *<a href="#Pointer">Pointer</a></pre>

NewPointer returns a new pointer type for the given element (base) type.

<h3 id="Pointer.Elem">func (*Pointer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L155">Elem</a>
    <a href="#Pointer.Elem">¶</a></h3>
<pre>func (p *<a href="#Pointer">Pointer</a>) Elem() <a href="#Type">Type</a></pre>

Elem returns the element type for the given pointer p.

<h3 id="Pointer.String">func (*Pointer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L434">String</a>
    <a href="#Pointer.String">¶</a></h3>
<pre>func (t *<a href="#Pointer">Pointer</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Pointer.Underlying">func (*Pointer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L422">Underlying</a>
    <a href="#Pointer.Underlying">¶</a></h3>
<pre>func (t *<a href="#Pointer">Pointer</a>) Underlying() <a href="#Type">Type</a></pre>


<h2 id="Qualifier">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/typestring.go#L15">Qualifier</a>
    <a href="#Qualifier">¶</a></h2>
<pre>type Qualifier func(*<a href="#Package">Package</a>) <a href="/builtin/#string">string</a></pre>

A Qualifier controls how named package-level objects are printed in calls to
TypeString, ObjectString, and SelectionString.

These three formatting routines call the Qualifier for each package-level object
O, and if the Qualifier returns a non-empty string p, the object is printed in
the form p.O. If it returns an empty string, only the object name O is printed.

Using a nil Qualifier is equivalent to using (*Package).Path: the object is
qualified by the import path, e.g., "encoding/json.Marshal".

<h3 id="RelativeTo">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/typestring.go#L19">RelativeTo</a>
    <a href="#RelativeTo">¶</a></h3>
<pre>func RelativeTo(pkg *<a href="#Package">Package</a>) <a href="#Qualifier">Qualifier</a></pre>

RelativeTo(pkg) returns a Qualifier that fully qualifies members of all packages
other than pkg.

<h2 id="Scope">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L15">Scope</a>
    <a href="#Scope">¶</a></h2>
<pre>type Scope struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Scope maintains a set of objects and links to its containing (parent) and
contained (children) scopes. Objects may be inserted and looked up by name. The
zero value for Scope is a ready-to-use empty scope.

<a id="exampleScope"></a>
Example:

    // Parse the source files for a package.
    fset := token.NewFileSet()
    var files []*ast.File
    for _, file := range []struct{ name, input string }{
        {"main.go", `
    package main
    import "fmt"
    func main() {
    	freezing := FToC(-18)
    	fmt.Println(freezing, Boiling) }
    `},
        {"celsius.go", `
    package main
    import "fmt"
    type Celsius float64
    func (c Celsius) String() string { return fmt.Sprintf("%g°C", c) }
    func FToC(f float64) Celsius { return Celsius(f - 32 / 9 * 5) }
    const Boiling Celsius = 100
    `},
    } {
        f, err := parser.ParseFile(fset, file.name, file.input, 0)
        if err != nil {
            log.Fatal(err)
        }
        files = append(files, f)
    }

    // Type-check a package consisting of these files.
    // Type information for the imported "fmt" package
    // comes from $GOROOT/pkg/$GOOS_$GOOARCH/fmt.a.
    conf := types.Config{Importer: importer.Default()}
    pkg, err := conf.Check("temperature", fset, files, nil)
    if err != nil {
        log.Fatal(err)
    }

    // Print the tree of scopes.
    // For determinism, we redact addresses.
    var buf bytes.Buffer
    pkg.Scope().WriteTo(&buf, 0, true)
    rx := regexp.MustCompile(` 0x[a-fA-F0-9]*`)
    fmt.Println(rx.ReplaceAllString(buf.String(), ""))

    // Output:
    // package "temperature" scope {
    // .  const temperature.Boiling temperature.Celsius
    // .  type temperature.Celsius float64
    // .  func temperature.FToC(f float64) temperature.Celsius
    // .  func temperature.main()
    //
    // .  main.go scope {
    // .  .  package fmt
    //
    // .  .  function scope {
    // .  .  .  var freezing temperature.Celsius
    // .  .  }.  }
    // .  celsius.go scope {
    // .  .  package fmt
    //
    // .  .  function scope {
    // .  .  .  var c temperature.Celsius
    // .  .  }
    // .  .  function scope {
    // .  .  .  var f float64
    // .  .  }.  }}

<h3 id="NewScope">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L25">NewScope</a>
    <a href="#NewScope">¶</a></h3>
<pre>func NewScope(parent *<a href="#Scope">Scope</a>, pos, end <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, comment <a href="/builtin/#string">string</a>) *<a href="#Scope">Scope</a></pre>

NewScope returns a new, empty scope contained in the given parent scope, if any.
The comment is for debugging only.

<h3 id="Scope.Child">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L56">Child</a>
    <a href="#Scope.Child">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Child(i <a href="/builtin/#int">int</a>) *<a href="#Scope">Scope</a></pre>

Child returns the i'th child scope for 0 <= i < NumChildren().

<h3 id="Scope.Contains">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L113">Contains</a>
    <a href="#Scope.Contains">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Contains(pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>) <a href="/builtin/#bool">bool</a></pre>

Contains returns true if pos is within the scope's extent. The result is
guaranteed to be valid only if the type-checked AST has complete position
information.

<h3 id="Scope.End">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L108">End</a>
    <a href="#Scope.End">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) End() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Scope.Innermost">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L122">Innermost</a>
    <a href="#Scope.Innermost">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Innermost(pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>) *<a href="#Scope">Scope</a></pre>

Innermost returns the innermost (child) scope containing pos. If pos is not
within any scope, the result is nil. The result is also nil for the Universe
scope. The result is guaranteed to be valid only if the type-checked AST has
complete position information.

<h3 id="Scope.Insert">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L88">Insert</a>
    <a href="#Scope.Insert">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Insert(obj <a href="#Object">Object</a>) <a href="#Object">Object</a></pre>

Insert attempts to insert an object obj into scope s. If s already contains an
alternative object alt with the same name, Insert leaves s unchanged and returns
alt. Otherwise it inserts obj, sets the object's parent scope if not already
set, and returns nil.

<h3 id="Scope.Len">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L38">Len</a>
    <a href="#Scope.Len">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Len() <a href="/builtin/#int">int</a></pre>

Len() returns the number of scope elements.

<h3 id="Scope.Lookup">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L60">Lookup</a>
    <a href="#Scope.Lookup">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Lookup(name <a href="/builtin/#string">string</a>) <a href="#Object">Object</a></pre>

Lookup returns the object in scope s with the given name if such an object
exists; otherwise the result is nil.

<h3 id="Scope.LookupParent">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L74">LookupParent</a>
    <a href="#Scope.LookupParent">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) LookupParent(name <a href="/builtin/#string">string</a>, pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>) (*<a href="#Scope">Scope</a>, <a href="#Object">Object</a>)</pre>

LookupParent follows the parent chain of scopes starting with s until it finds a
scope where Lookup(name) returns a non-nil object, and then returns that scope
and object. If a valid position pos is provided, only objects that were declared
at or before pos are considered. If no such scope and object exists, the result
is (nil, nil).

Note that obj.Parent() may be different from the returned scope if the object
was inserted into the scope and already had a parent at that time (see Insert,
below). This can only happen for dot-imported objects whose scope is the scope
of the package that exported them.

<h3 id="Scope.Names">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L41">Names</a>
    <a href="#Scope.Names">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Names() []<a href="/builtin/#string">string</a></pre>

Names returns the scope's element names in sorted order.

<h3 id="Scope.NumChildren">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L53">NumChildren</a>
    <a href="#Scope.NumChildren">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) NumChildren() <a href="/builtin/#int">int</a></pre>

NumChildren() returns the number of scopes nested in s.

<h3 id="Scope.Parent">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L35">Parent</a>
    <a href="#Scope.Parent">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Parent() *<a href="#Scope">Scope</a></pre>

Parent returns the scope's containing (parent) scope.

<h3 id="Scope.Pos">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L107">Pos</a>
    <a href="#Scope.Pos">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>

Pos and End describe the scope's source code extent [pos, end). The results are
guaranteed to be valid only if the type-checked AST has complete position
information. The extent is undefined for Universe and package scopes.

<h3 id="Scope.String">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L176">String</a>
    <a href="#Scope.String">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) String() <a href="/builtin/#string">string</a></pre>

String returns a string representation of the scope, for debugging.

<h3 id="Scope.WriteTo">func (*Scope) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/scope.go#L149">WriteTo</a>
    <a href="#Scope.WriteTo">¶</a></h3>
<pre>func (s *<a href="#Scope">Scope</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, n <a href="/builtin/#int">int</a>, recurse <a href="/builtin/#bool">bool</a>)</pre>

WriteTo writes a string representation of the scope to w, with the scope
elements sorted by name. The level of indentation is controlled by n >= 0, with
n == 0 for no indentation. If recurse is set, it also writes nested (children)
scopes.

<h2 id="Selection">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go#L30">Selection</a>
    <a href="#Selection">¶</a></h2>
<pre>type Selection struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Selection describes a selector expression x.f. For the declarations:

    type T struct{ x int; E }
    type E struct{}
    func (e E) m() {}
    var p *T

the following relations exist:

    Selector    Kind          Recv    Obj    Type               Index     Indirect

    p.x         FieldVal      T       x      int                {0}       true
    p.m         MethodVal     *T      m      func (e *T) m()    {1, 0}    true
    T.m         MethodExpr    T       m      func m(_ T)        {1, 0}    false

<h3 id="Selection.Index">func (*Selection) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go#L92">Index</a>
    <a href="#Selection.Index">¶</a></h3>
<pre>func (s *<a href="#Selection">Selection</a>) Index() []<a href="/builtin/#int">int</a></pre>

Index describes the path from x to f in x.f. The last index entry is the field
or method index of the type declaring f; either:

    1) the list of declared methods of a named type; or
    2) the list of methods of an interface type; or
    3) the list of fields of a struct type.

The earlier index entries are the indices of the embedded fields implicitly
traversed to get from (the type of) x to f, starting at embedding depth 0.

<h3 id="Selection.Indirect">func (*Selection) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go#L96">Indirect</a>
    <a href="#Selection.Indirect">¶</a></h3>
<pre>func (s *<a href="#Selection">Selection</a>) Indirect() <a href="/builtin/#bool">bool</a></pre>

Indirect reports whether any pointer indirection was required to get from x to f
in x.f.

<h3 id="Selection.Kind">func (*Selection) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go#L39">Kind</a>
    <a href="#Selection.Kind">¶</a></h3>
<pre>func (s *<a href="#Selection">Selection</a>) Kind() <a href="#SelectionKind">SelectionKind</a></pre>

Kind returns the selection kind.

<h3 id="Selection.Obj">func (*Selection) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go#L46">Obj</a>
    <a href="#Selection.Obj">¶</a></h3>
<pre>func (s *<a href="#Selection">Selection</a>) Obj() <a href="#Object">Object</a></pre>

Obj returns the object denoted by x.f; a *Var for a field selection, and a *Func
in all other cases.

<h3 id="Selection.Recv">func (*Selection) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go#L42">Recv</a>
    <a href="#Selection.Recv">¶</a></h3>
<pre>func (s *<a href="#Selection">Selection</a>) Recv() <a href="#Type">Type</a></pre>

Recv returns the type of x in x.f.

<h3 id="Selection.String">func (*Selection) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go#L98">String</a>
    <a href="#Selection.String">¶</a></h3>
<pre>func (s *<a href="#Selection">Selection</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Selection.Type">func (*Selection) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go#L50">Type</a>
    <a href="#Selection.Type">¶</a></h3>
<pre>func (s *<a href="#Selection">Selection</a>) Type() <a href="#Type">Type</a></pre>

Type returns the type of x.f, which may be different from the type of f. See
Selection for more information.

<h2 id="SelectionKind">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/selection.go#L6">SelectionKind</a>
    <a href="#SelectionKind">¶</a></h2>
<pre>type SelectionKind <a href="/builtin/#int">int</a></pre>

SelectionKind describes the kind of a selector expression x.f (excluding
qualified identifiers).

<pre>const (
    <span id="FieldVal">FieldVal</span>   <a href="#SelectionKind">SelectionKind</a> = <a href="/builtin/#iota">iota</a> <span class="comment">// x.f is a struct field selector</span>
    <span id="MethodVal">MethodVal</span>                       <span class="comment">// x.f is a method selector</span>
    <span id="MethodExpr">MethodExpr</span>                      <span class="comment">// x.f is a method expression</span>
)</pre>


<h2 id="Signature">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L184">Signature</a>
    <a href="#Signature">¶</a></h2>
<pre>type Signature struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Signature represents a (non-builtin) function or method type.

<h3 id="NewSignature">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L200">NewSignature</a>
    <a href="#NewSignature">¶</a></h3>
<pre>func NewSignature(recv *<a href="#Var">Var</a>, params, results *<a href="#Tuple">Tuple</a>, variadic <a href="/builtin/#bool">bool</a>) *<a href="#Signature">Signature</a></pre>

NewSignature returns a new function type for the given receiver, parameters, and
results, either of which may be nil. If variadic is set, the function is
variadic, it must have at least one parameter, and the last parameter must be of
unnamed slice type.

<h3 id="Signature.Params">func (*Signature) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L222">Params</a>
    <a href="#Signature.Params">¶</a></h3>
<pre>func (s *<a href="#Signature">Signature</a>) Params() *<a href="#Tuple">Tuple</a></pre>

Params returns the parameters of signature s, or nil.

<h3 id="Signature.Recv">func (*Signature) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L219">Recv</a>
    <a href="#Signature.Recv">¶</a></h3>
<pre>func (s *<a href="#Signature">Signature</a>) Recv() *<a href="#Var">Var</a></pre>

Recv returns the receiver of signature s (if a method), or nil if a function.

For an abstract method, Recv returns the enclosing interface either as a *Named
or an *Interface. Due to embedding, an interface may contain methods whose
receiver type is a different interface.

<h3 id="Signature.Results">func (*Signature) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L225">Results</a>
    <a href="#Signature.Results">¶</a></h3>
<pre>func (s *<a href="#Signature">Signature</a>) Results() *<a href="#Tuple">Tuple</a></pre>

Results returns the results of signature s, or nil.

<h3 id="Signature.String">func (*Signature) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L436">String</a>
    <a href="#Signature.String">¶</a></h3>
<pre>func (t *<a href="#Signature">Signature</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Signature.Underlying">func (*Signature) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L424">Underlying</a>
    <a href="#Signature.Underlying">¶</a></h3>
<pre>func (t *<a href="#Signature">Signature</a>) Underlying() <a href="#Type">Type</a></pre>


<h3 id="Signature.Variadic">func (*Signature) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L228">Variadic</a>
    <a href="#Signature.Variadic">¶</a></h3>
<pre>func (s *<a href="#Signature">Signature</a>) Variadic() <a href="/builtin/#bool">bool</a></pre>

Variadic reports whether the signature s is variadic.

<h2 id="Sizes">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/sizes.go#L1">Sizes</a>
    <a href="#Sizes">¶</a></h2>
<pre>type Sizes interface {
    <span class="comment">// Alignof returns the alignment of a variable of type T.</span>
    <span class="comment">// Alignof must implement the alignment guarantees required by the spec.</span>
    Alignof(T <a href="#Type">Type</a>) <a href="/builtin/#int64">int64</a>

    <span class="comment">// Offsetsof returns the offsets of the given struct fields, in bytes.</span>
    <span class="comment">// Offsetsof must implement the offset guarantees required by the spec.</span>
    Offsetsof(fields []*<a href="#Var">Var</a>) []<a href="/builtin/#int64">int64</a>

    <span class="comment">// Sizeof returns the size of a variable of type T.</span>
    <span class="comment">// Sizeof must implement the size guarantees required by the spec.</span>
    Sizeof(T <a href="#Type">Type</a>) <a href="/builtin/#int64">int64</a>
}</pre>

Sizes defines the sizing functions for package unsafe.

<h3 id="SizesFor">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/sizes.go#L170">SizesFor</a>
    <a href="#SizesFor">¶</a></h3>
<pre>func SizesFor(compiler, arch <a href="/builtin/#string">string</a>) <a href="#Sizes">Sizes</a></pre>

SizesFor returns the Sizes used by a compiler for an architecture. The result is
nil if a compiler/architecture pair is not known.

Supported architectures for compiler "gc": "386", "arm", "arm64", "amd64",
"amd64p32", "mips", "mipsle", "mips64", "mips64le", "ppc64", "ppc64le", "s390x".

<h2 id="Slice">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L99">Slice</a>
    <a href="#Slice">¶</a></h2>
<pre>type Slice struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Slice represents a slice type.

<h3 id="NewSlice">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L104">NewSlice</a>
    <a href="#NewSlice">¶</a></h3>
<pre>func NewSlice(elem <a href="#Type">Type</a>) *<a href="#Slice">Slice</a></pre>

NewSlice returns a new slice type for the given element type.

<h3 id="Slice.Elem">func (*Slice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L107">Elem</a>
    <a href="#Slice.Elem">¶</a></h3>
<pre>func (s *<a href="#Slice">Slice</a>) Elem() <a href="#Type">Type</a></pre>

Elem returns the element type of slice s.

<h3 id="Slice.String">func (*Slice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L432">String</a>
    <a href="#Slice.String">¶</a></h3>
<pre>func (t *<a href="#Slice">Slice</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Slice.Underlying">func (*Slice) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L420">Underlying</a>
    <a href="#Slice.Underlying">¶</a></h3>
<pre>func (t *<a href="#Slice">Slice</a>) Underlying() <a href="#Type">Type</a></pre>


<h2 id="StdSizes">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/sizes.go#L33">StdSizes</a>
    <a href="#StdSizes">¶</a></h2>
<pre>type StdSizes struct {
<span id="StdSizes.WordSize"></span>    WordSize <a href="/builtin/#int64">int64</a> <span class="comment">// word size in bytes - must be &gt;= 4 (32bits)</span>
<span id="StdSizes.MaxAlign"></span>    MaxAlign <a href="/builtin/#int64">int64</a> <span class="comment">// maximum alignment in bytes - must be &gt;= 1</span>
}</pre>

StdSizes is a convenience type for creating commonly used Sizes. It makes the
following simplifying assumptions:

    	- The size of explicitly sized basic types (int16, etc.) is the
    	  specified size.
    	- The size of strings and interfaces is 2*WordSize.
    	- The size of slices is 3*WordSize.
    	- The size of an array of n elements corresponds to the size of
    	  a struct of n consecutive fields of the array's element type.
         - The size of a struct is the offset of the last field plus that
    	  field's size. As with all element types, if the struct is used
    	  in an array its size must first be aligned to a multiple of the
    	  struct's alignment.
    	- All other types have size WordSize.
    	- Arrays and structs are aligned per spec definition; all other
    	  types are naturally aligned with a maximum alignment MaxAlign.

*StdSizes implements Sizes.

<h3 id="StdSizes.Alignof">func (*StdSizes) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/sizes.go#L38">Alignof</a>
    <a href="#StdSizes.Alignof">¶</a></h3>
<pre>func (s *<a href="#StdSizes">StdSizes</a>) Alignof(T <a href="#Type">Type</a>) <a href="/builtin/#int64">int64</a></pre>


<h3 id="StdSizes.Offsetsof">func (*StdSizes) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/sizes.go#L82">Offsetsof</a>
    <a href="#StdSizes.Offsetsof">¶</a></h3>
<pre>func (s *<a href="#StdSizes">StdSizes</a>) Offsetsof(fields []*<a href="#Var">Var</a>) []<a href="/builtin/#int64">int64</a></pre>


<h3 id="StdSizes.Sizeof">func (*StdSizes) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/sizes.go#L110">Sizeof</a>
    <a href="#StdSizes.Sizeof">¶</a></h3>
<pre>func (s *<a href="#StdSizes">StdSizes</a>) Sizeof(T <a href="#Type">Type</a>) <a href="/builtin/#int64">int64</a></pre>


<h2 id="Struct">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L110">Struct</a>
    <a href="#Struct">¶</a></h2>
<pre>type Struct struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Struct represents a struct type.

<h3 id="NewStruct">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L119">NewStruct</a>
    <a href="#NewStruct">¶</a></h3>
<pre>func NewStruct(fields []*<a href="#Var">Var</a>, tags []<a href="/builtin/#string">string</a>) *<a href="#Struct">Struct</a></pre>

NewStruct returns a new struct with the given fields and corresponding field
tags. If a field with index i has a tag, tags[i] must be that tag, but len(tags)
may be only as long as required to hold the tag with the largest index i.
Consequently, if no field has a tag, tags may be nil.

<h3 id="Struct.Field">func (*Struct) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L136">Field</a>
    <a href="#Struct.Field">¶</a></h3>
<pre>func (s *<a href="#Struct">Struct</a>) Field(i <a href="/builtin/#int">int</a>) *<a href="#Var">Var</a></pre>

Field returns the i'th field for 0 <= i < NumFields().

<h3 id="Struct.NumFields">func (*Struct) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L133">NumFields</a>
    <a href="#Struct.NumFields">¶</a></h3>
<pre>func (s *<a href="#Struct">Struct</a>) NumFields() <a href="/builtin/#int">int</a></pre>

NumFields returns the number of fields in the struct (including blank and
anonymous fields).

<h3 id="Struct.String">func (*Struct) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L433">String</a>
    <a href="#Struct.String">¶</a></h3>
<pre>func (t *<a href="#Struct">Struct</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Struct.Tag">func (*Struct) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L139">Tag</a>
    <a href="#Struct.Tag">¶</a></h3>
<pre>func (s *<a href="#Struct">Struct</a>) Tag(i <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a></pre>

Tag returns the i'th field tag for 0 <= i < NumFields().

<h3 id="Struct.Underlying">func (*Struct) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L421">Underlying</a>
    <a href="#Struct.Underlying">¶</a></h3>
<pre>func (t *<a href="#Struct">Struct</a>) Underlying() <a href="#Type">Type</a></pre>


<h2 id="Tuple">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L160">Tuple</a>
    <a href="#Tuple">¶</a></h2>
<pre>type Tuple struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Tuple represents an ordered list of variables; a nil *Tuple is a valid (empty)
tuple. Tuples are used as components of signatures and to represent the type of
multiple assignments; they are not first class types of Go.

<h3 id="NewTuple">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L165">NewTuple</a>
    <a href="#NewTuple">¶</a></h3>
<pre>func NewTuple(x ...*<a href="#Var">Var</a>) *<a href="#Tuple">Tuple</a></pre>

NewTuple returns a new tuple for the given variables.

<h3 id="Tuple.At">func (*Tuple) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L181">At</a>
    <a href="#Tuple.At">¶</a></h3>
<pre>func (t *<a href="#Tuple">Tuple</a>) At(i <a href="/builtin/#int">int</a>) *<a href="#Var">Var</a></pre>

At returns the i'th variable of tuple t.

<h3 id="Tuple.Len">func (*Tuple) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L173">Len</a>
    <a href="#Tuple.Len">¶</a></h3>
<pre>func (t *<a href="#Tuple">Tuple</a>) Len() <a href="/builtin/#int">int</a></pre>

Len returns the number variables of tuple t.

<h3 id="Tuple.String">func (*Tuple) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L435">String</a>
    <a href="#Tuple.String">¶</a></h3>
<pre>func (t *<a href="#Tuple">Tuple</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Tuple.Underlying">func (*Tuple) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L423">Underlying</a>
    <a href="#Tuple.Underlying">¶</a></h3>
<pre>func (t *<a href="#Tuple">Tuple</a>) Underlying() <a href="#Type">Type</a></pre>


<h2 id="Type">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/type.go#L1">Type</a>
    <a href="#Type">¶</a></h2>
<pre>type Type interface {
    <span class="comment">// Underlying returns the underlying type of a type.</span>
    Underlying() <a href="#Type">Type</a>

    <span class="comment">// String returns a string representation of a type.</span>
    String() <a href="/builtin/#string">string</a>
}</pre>

A Type represents a type of Go. All types implement the Type interface.

<h3 id="Default">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/predicates.go#L288">Default</a>
    <a href="#Default">¶</a></h3>
<pre>func Default(typ <a href="#Type">Type</a>) <a href="#Type">Type</a></pre>

Default returns the default "typed" type for an "untyped" type; it returns the
incoming type for all other types. The default type for untyped nil is untyped
nil.

<h2 id="TypeAndValue">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L246">TypeAndValue</a>
    <a href="#TypeAndValue">¶</a></h2>
<pre>type TypeAndValue struct {
<span id="TypeAndValue.Type"></span>    Type  <a href="#Type">Type</a>
<span id="TypeAndValue.Value"></span>    Value <a href="/go/constant/">constant</a>.<a href="/go/constant/#Value">Value</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

TypeAndValue reports the type and value (for constants) of the corresponding
expression.

<h3 id="Eval">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/eval.go#L27">Eval</a>
    <a href="#Eval">¶</a></h3>
<pre>func Eval(fset *<a href="/go/token/">token</a>.<a href="/go/token/#FileSet">FileSet</a>, pkg *<a href="#Package">Package</a>, pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, expr <a href="/builtin/#string">string</a>) (<a href="#TypeAndValue">TypeAndValue</a>, <a href="/builtin/#error">error</a>)</pre>

Eval returns the type and, if constant, the value for the expression expr,
evaluated at position pos of package pkg, which must have been derived from
type-checking an AST with complete position information relative to the provided
file set.

If the expression contains function literals, their bodies are ignored (i.e.,
the bodies are not type-checked).

If pkg == nil, the Universe scope is used and the provided position pos is
ignored. If pkg != nil, and pos is invalid, the package scope is used.
Otherwise, pos must belong to the package.

An error is returned if pos is not within the package or if the node cannot be
evaluated.

Note: Eval should not be used instead of running Check to compute types and
values, but in addition to Check. Eval will re-evaluate its argument each time,
and it also does not know about the context in which an expression is used
(e.g., an assignment). Thus, top- level untyped constants will return an untyped
type rather then the respective context-specific type.

<h3 id="TypeAndValue.Addressable">func (TypeAndValue) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L291">Addressable</a>
    <a href="#TypeAndValue.Addressable">¶</a></h3>
<pre>func (tv <a href="#TypeAndValue">TypeAndValue</a>) Addressable() <a href="/builtin/#bool">bool</a></pre>

Addressable reports whether the corresponding expression is addressable
(https://golang.org/ref/spec#Address_operators).

<h3 id="TypeAndValue.Assignable">func (TypeAndValue) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L297">Assignable</a>
    <a href="#TypeAndValue.Assignable">¶</a></h3>
<pre>func (tv <a href="#TypeAndValue">TypeAndValue</a>) Assignable() <a href="/builtin/#bool">bool</a></pre>

Assignable reports whether the corresponding expression is assignable to
(provided a value of the right type).

<h3 id="TypeAndValue.HasOk">func (TypeAndValue) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L303">HasOk</a>
    <a href="#TypeAndValue.HasOk">¶</a></h3>
<pre>func (tv <a href="#TypeAndValue">TypeAndValue</a>) HasOk() <a href="/builtin/#bool">bool</a></pre>

HasOk reports whether the corresponding expression may be used on the lhs of a
comma-ok assignment.

<h3 id="TypeAndValue.IsBuiltin">func (TypeAndValue) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L268">IsBuiltin</a>
    <a href="#TypeAndValue.IsBuiltin">¶</a></h3>
<pre>func (tv <a href="#TypeAndValue">TypeAndValue</a>) IsBuiltin() <a href="/builtin/#bool">bool</a></pre>

IsBuiltin reports whether the corresponding expression denotes a (possibly
parenthesized) built-in function.

<h3 id="TypeAndValue.IsNil">func (TypeAndValue) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L285">IsNil</a>
    <a href="#TypeAndValue.IsNil">¶</a></h3>
<pre>func (tv <a href="#TypeAndValue">TypeAndValue</a>) IsNil() <a href="/builtin/#bool">bool</a></pre>

IsNil reports whether the corresponding expression denotes the predeclared value
nil.

<h3 id="TypeAndValue.IsType">func (TypeAndValue) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L262">IsType</a>
    <a href="#TypeAndValue.IsType">¶</a></h3>
<pre>func (tv <a href="#TypeAndValue">TypeAndValue</a>) IsType() <a href="/builtin/#bool">bool</a></pre>

IsType reports whether the corresponding expression specifies a type.

<h3 id="TypeAndValue.IsValue">func (TypeAndValue) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L275">IsValue</a>
    <a href="#TypeAndValue.IsValue">¶</a></h3>
<pre>func (tv <a href="#TypeAndValue">TypeAndValue</a>) IsValue() <a href="/builtin/#bool">bool</a></pre>

IsValue reports whether the corresponding expression is a value. Builtins are
not considered values. Constant values have a non- nil Value.

<h3 id="TypeAndValue.IsVoid">func (TypeAndValue) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/api.go#L257">IsVoid</a>
    <a href="#TypeAndValue.IsVoid">¶</a></h3>
<pre>func (tv <a href="#TypeAndValue">TypeAndValue</a>) IsVoid() <a href="/builtin/#bool">bool</a></pre>

IsVoid reports whether the corresponding expression is a function call without
results.

<h2 id="TypeName">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L143">TypeName</a>
    <a href="#TypeName">¶</a></h2>
<pre>type TypeName struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A TypeName represents a name for a (named or alias) type.

<h3 id="NewTypeName">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L147">NewTypeName</a>
    <a href="#NewTypeName">¶</a></h3>
<pre>func NewTypeName(pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>, typ <a href="#Type">Type</a>) *<a href="#TypeName">TypeName</a></pre>


<h3 id="TypeName.Exported">func (*TypeName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L81">Exported</a>
    <a href="#TypeName.Exported">¶</a></h3>
<pre>func (obj *<a href="#TypeName">TypeName</a>) Exported() <a href="/builtin/#bool">bool</a></pre>


<h3 id="TypeName.Id">func (*TypeName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L82">Id</a>
    <a href="#TypeName.Id">¶</a></h3>
<pre>func (obj *<a href="#TypeName">TypeName</a>) Id() <a href="/builtin/#string">string</a></pre>


<h3 id="TypeName.IsAlias">func (*TypeName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L152">IsAlias</a>
    <a href="#TypeName.IsAlias">¶</a></h3>
<pre>func (obj *<a href="#TypeName">TypeName</a>) IsAlias() <a href="/builtin/#bool">bool</a></pre>

IsAlias reports whether obj is an alias name for a type.

<h3 id="TypeName.Name">func (*TypeName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L79">Name</a>
    <a href="#TypeName.Name">¶</a></h3>
<pre>func (obj *<a href="#TypeName">TypeName</a>) Name() <a href="/builtin/#string">string</a></pre>


<h3 id="TypeName.Parent">func (*TypeName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L76">Parent</a>
    <a href="#TypeName.Parent">¶</a></h3>
<pre>func (obj *<a href="#TypeName">TypeName</a>) Parent() *<a href="#Scope">Scope</a></pre>


<h3 id="TypeName.Pkg">func (*TypeName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L78">Pkg</a>
    <a href="#TypeName.Pkg">¶</a></h3>
<pre>func (obj *<a href="#TypeName">TypeName</a>) Pkg() *<a href="#Package">Package</a></pre>


<h3 id="TypeName.Pos">func (*TypeName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L77">Pos</a>
    <a href="#TypeName.Pos">¶</a></h3>
<pre>func (obj *<a href="#TypeName">TypeName</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="TypeName.String">func (*TypeName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L360">String</a>
    <a href="#TypeName.String">¶</a></h3>
<pre>func (obj *<a href="#TypeName">TypeName</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="TypeName.Type">func (*TypeName) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L80">Type</a>
    <a href="#TypeName.Type">¶</a></h3>
<pre>func (obj *<a href="#TypeName">TypeName</a>) Type() <a href="#Type">Type</a></pre>


<h2 id="Var">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L176">Var</a>
    <a href="#Var">¶</a></h2>
<pre>type Var struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Variable represents a declared variable (including function parameters and
results, and struct fields).

<h3 id="NewField">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L192">NewField</a>
    <a href="#NewField">¶</a></h3>
<pre>func NewField(pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>, typ <a href="#Type">Type</a>, anonymous <a href="/builtin/#bool">bool</a>) *<a href="#Var">Var</a></pre>


<h3 id="NewParam">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L188">NewParam</a>
    <a href="#NewParam">¶</a></h3>
<pre>func NewParam(pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>, typ <a href="#Type">Type</a>) *<a href="#Var">Var</a></pre>


<h3 id="NewVar">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L184">NewVar</a>
    <a href="#NewVar">¶</a></h3>
<pre>func NewVar(pos <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a>, pkg *<a href="#Package">Package</a>, name <a href="/builtin/#string">string</a>, typ <a href="#Type">Type</a>) *<a href="#Var">Var</a></pre>


<h3 id="Var.Anonymous">func (*Var) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L196">Anonymous</a>
    <a href="#Var.Anonymous">¶</a></h3>
<pre>func (obj *<a href="#Var">Var</a>) Anonymous() <a href="/builtin/#bool">bool</a></pre>


<h3 id="Var.Exported">func (*Var) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L81">Exported</a>
    <a href="#Var.Exported">¶</a></h3>
<pre>func (obj *<a href="#Var">Var</a>) Exported() <a href="/builtin/#bool">bool</a></pre>


<h3 id="Var.Id">func (*Var) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L82">Id</a>
    <a href="#Var.Id">¶</a></h3>
<pre>func (obj *<a href="#Var">Var</a>) Id() <a href="/builtin/#string">string</a></pre>


<h3 id="Var.IsField">func (*Var) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L197">IsField</a>
    <a href="#Var.IsField">¶</a></h3>
<pre>func (obj *<a href="#Var">Var</a>) IsField() <a href="/builtin/#bool">bool</a></pre>


<h3 id="Var.Name">func (*Var) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L79">Name</a>
    <a href="#Var.Name">¶</a></h3>
<pre>func (obj *<a href="#Var">Var</a>) Name() <a href="/builtin/#string">string</a></pre>


<h3 id="Var.Parent">func (*Var) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L76">Parent</a>
    <a href="#Var.Parent">¶</a></h3>
<pre>func (obj *<a href="#Var">Var</a>) Parent() *<a href="#Scope">Scope</a></pre>


<h3 id="Var.Pkg">func (*Var) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L78">Pkg</a>
    <a href="#Var.Pkg">¶</a></h3>
<pre>func (obj *<a href="#Var">Var</a>) Pkg() *<a href="#Package">Package</a></pre>


<h3 id="Var.Pos">func (*Var) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L77">Pos</a>
    <a href="#Var.Pos">¶</a></h3>
<pre>func (obj *<a href="#Var">Var</a>) Pos() <a href="/go/token/">token</a>.<a href="/go/token/#Pos">Pos</a></pre>


<h3 id="Var.String">func (*Var) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L361">String</a>
    <a href="#Var.String">¶</a></h3>
<pre>func (obj *<a href="#Var">Var</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="Var.Type">func (*Var) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/go/types/object.go#L80">Type</a>
    <a href="#Var.Type">¶</a></h3>
<pre>func (obj *<a href="#Var">Var</a>) Type() <a href="#Type">Type</a></pre>



