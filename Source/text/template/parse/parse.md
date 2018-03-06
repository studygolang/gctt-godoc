version: 1.10
## package parse

  `import "text/template/parse"`

## Overview

Package parse builds parse trees for templates as defined by text/template and
html/template. Clients should use those packages to construct templates rather
than this one, which provides shared internal data structures not intended for
general use.

## Index

- [func IsEmptyTree(n Node) bool](#IsEmptyTree)
- [func Parse(name, text, leftDelim, rightDelim string, funcs ...map[string]interface{}) (map[string]*Tree, error)](#Parse)
- [type ActionNode](#ActionNode)
  - [func (a *ActionNode) Copy() Node](#ActionNode.Copy)
  - [func (a *ActionNode) String() string](#ActionNode.String)
- [type BoolNode](#BoolNode)
  - [func (b *BoolNode) Copy() Node](#BoolNode.Copy)
  - [func (b *BoolNode) String() string](#BoolNode.String)
- [type BranchNode](#BranchNode)
  - [func (b *BranchNode) Copy() Node](#BranchNode.Copy)
  - [func (b *BranchNode) String() string](#BranchNode.String)
- [type ChainNode](#ChainNode)
  - [func (c *ChainNode) Add(field string)](#ChainNode.Add)
  - [func (c *ChainNode) Copy() Node](#ChainNode.Copy)
  - [func (c *ChainNode) String() string](#ChainNode.String)
- [type CommandNode](#CommandNode)
  - [func (c *CommandNode) Copy() Node](#CommandNode.Copy)
  - [func (c *CommandNode) String() string](#CommandNode.String)
- [type DotNode](#DotNode)
  - [func (d *DotNode) Copy() Node](#DotNode.Copy)
  - [func (d *DotNode) String() string](#DotNode.String)
  - [func (d *DotNode) Type() NodeType](#DotNode.Type)
- [type FieldNode](#FieldNode)
  - [func (f *FieldNode) Copy() Node](#FieldNode.Copy)
  - [func (f *FieldNode) String() string](#FieldNode.String)
- [type IdentifierNode](#IdentifierNode)
  - [func NewIdentifier(ident string) *IdentifierNode](#NewIdentifier)
  - [func (i *IdentifierNode) Copy() Node](#IdentifierNode.Copy)
  - [func (i *IdentifierNode) SetPos(pos Pos) *IdentifierNode](#IdentifierNode.SetPos)
  - [func (i *IdentifierNode) SetTree(t *Tree) *IdentifierNode](#IdentifierNode.SetTree)
  - [func (i *IdentifierNode) String() string](#IdentifierNode.String)
- [type IfNode](#IfNode)
  - [func (i *IfNode) Copy() Node](#IfNode.Copy)
- [type ListNode](#ListNode)
  - [func (l *ListNode) Copy() Node](#ListNode.Copy)
  - [func (l *ListNode) CopyList() *ListNode](#ListNode.CopyList)
  - [func (l *ListNode) String() string](#ListNode.String)
- [type NilNode](#NilNode)
  - [func (n *NilNode) Copy() Node](#NilNode.Copy)
  - [func (n *NilNode) String() string](#NilNode.String)
  - [func (n *NilNode) Type() NodeType](#NilNode.Type)
- [type Node](#Node)
- [type NodeType](#NodeType)
  - [func (t NodeType) Type() NodeType](#NodeType.Type)
- [type NumberNode](#NumberNode)
  - [func (n *NumberNode) Copy() Node](#NumberNode.Copy)
  - [func (n *NumberNode) String() string](#NumberNode.String)
- [type PipeNode](#PipeNode)
  - [func (p *PipeNode) Copy() Node](#PipeNode.Copy)
  - [func (p *PipeNode) CopyPipe() *PipeNode](#PipeNode.CopyPipe)
  - [func (p *PipeNode) String() string](#PipeNode.String)
- [type Pos](#Pos)
  - [func (p Pos) Position() Pos](#Pos.Position)
- [type RangeNode](#RangeNode)
  - [func (r *RangeNode) Copy() Node](#RangeNode.Copy)
- [type StringNode](#StringNode)
  - [func (s *StringNode) Copy() Node](#StringNode.Copy)
  - [func (s *StringNode) String() string](#StringNode.String)
- [type TemplateNode](#TemplateNode)
  - [func (t *TemplateNode) Copy() Node](#TemplateNode.Copy)
  - [func (t *TemplateNode) String() string](#TemplateNode.String)
- [type TextNode](#TextNode)
  - [func (t *TextNode) Copy() Node](#TextNode.Copy)
  - [func (t *TextNode) String() string](#TextNode.String)
- [type Tree](#Tree)
  - [func New(name string, funcs ...map[string]interface{}) *Tree](#New)
  - [func (t *Tree) Copy() *Tree](#Tree.Copy)
  - [func (t *Tree) ErrorContext(n Node) (location, context string)](#Tree.ErrorContext)
  - [func (t *Tree) Parse(text, leftDelim, rightDelim string, treeSet map[string]*Tree, funcs ...map[string]interface{}) (tree *Tree, err error)](#Tree.Parse)
- [type VariableNode](#VariableNode)
  - [func (v *VariableNode) Copy() Node](#VariableNode.Copy)
  - [func (v *VariableNode) String() string](#VariableNode.String)
- [type WithNode](#WithNode)
  - [func (w *WithNode) Copy() Node](#WithNode.Copy)

### Package files
 [lex.go](//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/lex.go) [node.go](//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go) [parse.go](//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/parse.go)

<h2 id="IsEmptyTree">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/parse.go#L242">IsEmptyTree</a>
    <a href="#IsEmptyTree">¶</a></h2>
<pre>func IsEmptyTree(n <a href="#Node">Node</a>) <a href="/builtin/#bool">bool</a></pre>

IsEmptyTree reports whether this tree (node) is empty of everything but space.

<h2 id="Parse">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/parse.go#L41">Parse</a>
    <a href="#Parse">¶</a></h2>
<pre>func Parse(name, text, leftDelim, rightDelim <a href="/builtin/#string">string</a>, funcs ...map[<a href="/builtin/#string">string</a>]interface{}) (map[<a href="/builtin/#string">string</a>]*<a href="#Tree">Tree</a>, <a href="/builtin/#error">error</a>)</pre>

Parse returns a map from template name to parse.Tree, created by parsing the
templates described in the argument string. The top-level template will be given
the specified name. If an error is encountered, parsing stops and an empty map
is returned with the error.

<h2 id="ActionNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L197">ActionNode</a>
    <a href="#ActionNode">¶</a></h2>
<pre>type ActionNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="ActionNode.Line"></span>    Line <a href="/builtin/#int">int</a>       <span class="comment">// The line number in the input. Deprecated: Kept for compatibility.</span>
<span id="ActionNode.Pipe"></span>    Pipe *<a href="#PipeNode">PipeNode</a> <span class="comment">// The pipeline in the action.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ActionNode holds an action (something bounded by delimiters). Control actions
have their own nodes; ActionNode represents simple ones such as field
evaluations and parenthesized pipelines.

<h3 id="ActionNode.Copy">func (*ActionNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L218">Copy</a>
    <a href="#ActionNode.Copy">¶</a></h3>
<pre>func (a *<a href="#ActionNode">ActionNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="ActionNode.String">func (*ActionNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L209">String</a>
    <a href="#ActionNode.String">¶</a></h3>
<pre>func (a *<a href="#ActionNode">ActionNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="BoolNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L479">BoolNode</a>
    <a href="#BoolNode">¶</a></h2>
<pre>type BoolNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="BoolNode.True"></span>    True <a href="/builtin/#bool">bool</a> <span class="comment">// The value of the boolean constant.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

BoolNode holds a boolean constant.

<h3 id="BoolNode.Copy">func (*BoolNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L501">Copy</a>
    <a href="#BoolNode.Copy">¶</a></h3>
<pre>func (b *<a href="#BoolNode">BoolNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="BoolNode.String">func (*BoolNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L490">String</a>
    <a href="#BoolNode.String">¶</a></h3>
<pre>func (b *<a href="#BoolNode">BoolNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="BranchNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L718">BranchNode</a>
    <a href="#BranchNode">¶</a></h2>
<pre>type BranchNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="BranchNode.Line"></span>    Line     <a href="/builtin/#int">int</a>       <span class="comment">// The line number in the input. Deprecated: Kept for compatibility.</span>
<span id="BranchNode.Pipe"></span>    Pipe     *<a href="#PipeNode">PipeNode</a> <span class="comment">// The pipeline to be evaluated.</span>
<span id="BranchNode.List"></span>    List     *<a href="#ListNode">ListNode</a> <span class="comment">// What to execute if the value is non-empty.</span>
<span id="BranchNode.ElseList"></span>    ElseList *<a href="#ListNode">ListNode</a> <span class="comment">// What to execute if the value is empty (nil if absent).</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

BranchNode is the common representation of if, range, and with.

<h3 id="BranchNode.Copy">func (*BranchNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L750">Copy</a>
    <a href="#BranchNode.Copy">¶</a></h3>
<pre>func (b *<a href="#BranchNode">BranchNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="BranchNode.String">func (*BranchNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L728">String</a>
    <a href="#BranchNode.String">¶</a></h3>
<pre>func (b *<a href="#BranchNode">BranchNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="ChainNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L435">ChainNode</a>
    <a href="#ChainNode">¶</a></h2>
<pre>type ChainNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="ChainNode.Node"></span>    Node  <a href="#Node">Node</a>
<span id="ChainNode.Field"></span>    Field []<a href="/builtin/#string">string</a> <span class="comment">// The identifiers in lexical order.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ChainNode holds a term followed by a chain of field accesses (identifier
starting with '.'). The names may be chained ('.x.y'). The periods are dropped
from each ident.

<h3 id="ChainNode.Add">func (*ChainNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L448">Add</a>
    <a href="#ChainNode.Add">¶</a></h3>
<pre>func (c *<a href="#ChainNode">ChainNode</a>) Add(field <a href="/builtin/#string">string</a>)</pre>

Add adds the named field (which should start with a period) to the end of the
chain.

<h3 id="ChainNode.Copy">func (*ChainNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L474">Copy</a>
    <a href="#ChainNode.Copy">¶</a></h3>
<pre>func (c *<a href="#ChainNode">ChainNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="ChainNode.String">func (*ChainNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L459">String</a>
    <a href="#ChainNode.String">¶</a></h3>
<pre>func (c *<a href="#ChainNode">ChainNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="CommandNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L224">CommandNode</a>
    <a href="#CommandNode">¶</a></h2>
<pre>type CommandNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="CommandNode.Args"></span>    Args []<a href="#Node">Node</a> <span class="comment">// Arguments in lexical order: Identifier, field, or constant.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

CommandNode holds a command (a pipeline inside an evaluating action).

<h3 id="CommandNode.Copy">func (*CommandNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L258">Copy</a>
    <a href="#CommandNode.Copy">¶</a></h3>
<pre>func (c *<a href="#CommandNode">CommandNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="CommandNode.String">func (*CommandNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L239">String</a>
    <a href="#CommandNode.String">¶</a></h3>
<pre>func (c *<a href="#CommandNode">CommandNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="DotNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L343">DotNode</a>
    <a href="#DotNode">¶</a></h2>
<pre>type DotNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

DotNode holds the special identifier '.'.

<h3 id="DotNode.Copy">func (*DotNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L368">Copy</a>
    <a href="#DotNode.Copy">¶</a></h3>
<pre>func (d *<a href="#DotNode">DotNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="DotNode.String">func (*DotNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L360">String</a>
    <a href="#DotNode.String">¶</a></h3>
<pre>func (d *<a href="#DotNode">DotNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="DotNode.Type">func (*DotNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L353">Type</a>
    <a href="#DotNode.Type">¶</a></h3>
<pre>func (d *<a href="#DotNode">DotNode</a>) Type() <a href="#NodeType">NodeType</a></pre>


<h2 id="FieldNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L405">FieldNode</a>
    <a href="#FieldNode">¶</a></h2>
<pre>type FieldNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="FieldNode.Ident"></span>    Ident []<a href="/builtin/#string">string</a> <span class="comment">// The identifiers in lexical order.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

FieldNode holds a field (identifier starting with '.'). The names may be chained
('.x.y'). The period is dropped from each ident.

<h3 id="FieldNode.Copy">func (*FieldNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L428">Copy</a>
    <a href="#FieldNode.Copy">¶</a></h3>
<pre>func (f *<a href="#FieldNode">FieldNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="FieldNode.String">func (*FieldNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L416">String</a>
    <a href="#FieldNode.String">¶</a></h3>
<pre>func (f *<a href="#FieldNode">FieldNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="IdentifierNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L270">IdentifierNode</a>
    <a href="#IdentifierNode">¶</a></h2>
<pre>type IdentifierNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="IdentifierNode.Ident"></span>    Ident <a href="/builtin/#string">string</a> <span class="comment">// The identifier&#39;s name.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

IdentifierNode holds an identifier.

<h3 id="NewIdentifier">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L278">NewIdentifier</a>
    <a href="#NewIdentifier">¶</a></h3>
<pre>func NewIdentifier(ident <a href="/builtin/#string">string</a>) *<a href="#IdentifierNode">IdentifierNode</a></pre>

NewIdentifier returns a new IdentifierNode with the given identifier name.

<h3 id="IdentifierNode.Copy">func (*IdentifierNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L306">Copy</a>
    <a href="#IdentifierNode.Copy">¶</a></h3>
<pre>func (i *<a href="#IdentifierNode">IdentifierNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="IdentifierNode.SetPos">func (*IdentifierNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L285">SetPos</a>
    <a href="#IdentifierNode.SetPos">¶</a></h3>
<pre>func (i *<a href="#IdentifierNode">IdentifierNode</a>) SetPos(pos <a href="#Pos">Pos</a>) *<a href="#IdentifierNode">IdentifierNode</a></pre>

SetPos sets the position. NewIdentifier is a public method so we can't modify
its signature. Chained for convenience. TODO: fix one day?

<h3 id="IdentifierNode.SetTree">func (*IdentifierNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L293">SetTree</a>
    <a href="#IdentifierNode.SetTree">¶</a></h3>
<pre>func (i *<a href="#IdentifierNode">IdentifierNode</a>) SetTree(t *<a href="#Tree">Tree</a>) *<a href="#IdentifierNode">IdentifierNode</a></pre>

SetTree sets the parent tree for the node. NewIdentifier is a public method so
we can't modify its signature. Chained for convenience. TODO: fix one day?

<h3 id="IdentifierNode.String">func (*IdentifierNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L298">String</a>
    <a href="#IdentifierNode.String">¶</a></h3>
<pre>func (i *<a href="#IdentifierNode">IdentifierNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="IfNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L764">IfNode</a>
    <a href="#IfNode">¶</a></h2>
<pre>type IfNode struct {
    <a href="#BranchNode">BranchNode</a>
}</pre>

IfNode represents an {{if}} action and its commands.

<h3 id="IfNode.Copy">func (*IfNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L772">Copy</a>
    <a href="#IfNode.Copy">¶</a></h3>
<pre>func (i *<a href="#IfNode">IfNode</a>) Copy() <a href="#Node">Node</a></pre>


<h2 id="ListNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L67">ListNode</a>
    <a href="#ListNode">¶</a></h2>
<pre>type ListNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="ListNode.Nodes"></span>    Nodes []<a href="#Node">Node</a> <span class="comment">// The element nodes in lexical order.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ListNode holds a sequence of nodes.

<h3 id="ListNode.Copy">func (*ListNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L105">Copy</a>
    <a href="#ListNode.Copy">¶</a></h3>
<pre>func (l *<a href="#ListNode">ListNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="ListNode.CopyList">func (*ListNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L94">CopyList</a>
    <a href="#ListNode.CopyList">¶</a></h3>
<pre>func (l *<a href="#ListNode">ListNode</a>) CopyList() *<a href="#ListNode">ListNode</a></pre>


<h3 id="ListNode.String">func (*ListNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L86">String</a>
    <a href="#ListNode.String">¶</a></h3>
<pre>func (l *<a href="#ListNode">ListNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="NilNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L373">NilNode</a>
    <a href="#NilNode">¶</a></h2>
<pre>type NilNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

NilNode holds the special identifier 'nil' representing an untyped nil constant.

<h3 id="NilNode.Copy">func (*NilNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L398">Copy</a>
    <a href="#NilNode.Copy">¶</a></h3>
<pre>func (n *<a href="#NilNode">NilNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="NilNode.String">func (*NilNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L390">String</a>
    <a href="#NilNode.String">¶</a></h3>
<pre>func (n *<a href="#NilNode">NilNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="NilNode.Type">func (*NilNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L383">Type</a>
    <a href="#NilNode.Type">¶</a></h3>
<pre>func (n *<a href="#NilNode">NilNode</a>) Type() <a href="#NodeType">NodeType</a></pre>


<h2 id="Node">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L11">Node</a>
    <a href="#Node">¶</a></h2>
<pre>type Node interface {
    Type() <a href="#NodeType">NodeType</a>
    String() <a href="/builtin/#string">string</a>
    <span class="comment">// Copy does a deep copy of the Node and all its components.</span>
    <span class="comment">// To avoid type assertions, some XxxNodes also have specialized</span>
    <span class="comment">// CopyXxx methods that return *XxxNode.</span>
    Copy() <a href="#Node">Node</a>
    Position() <a href="#Pos">Pos</a> <span class="comment">// byte position of start of node in full original input string</span>
    <span class="comment">// contains filtered or unexported methods</span>
}</pre>

A Node is an element in the parse tree. The interface is trivial. The interface
contains an unexported method so that only types local to this package can
satisfy it.

<h2 id="NodeType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L25">NodeType</a>
    <a href="#NodeType">¶</a></h2>
<pre>type NodeType <a href="/builtin/#int">int</a></pre>

NodeType identifies the type of a parse tree node.

<pre>const (
    <span id="NodeText">NodeText</span>    <a href="#NodeType">NodeType</a> = <a href="/builtin/#iota">iota</a> <span class="comment">// Plain text.</span>
    <span id="NodeAction">NodeAction</span>                  <span class="comment">// A non-control action such as a field evaluation.</span>
    <span id="NodeBool">NodeBool</span>                    <span class="comment">// A boolean constant.</span>
    <span id="NodeChain">NodeChain</span>                   <span class="comment">// A sequence of field accesses.</span>
    <span id="NodeCommand">NodeCommand</span>                 <span class="comment">// An element of a pipeline.</span>
    <span id="NodeDot">NodeDot</span>                     <span class="comment">// The cursor, dot.</span>

    <span id="NodeField">NodeField</span>      <span class="comment">// A field or method name.</span>
    <span id="NodeIdentifier">NodeIdentifier</span> <span class="comment">// An identifier; always a function name.</span>
    <span id="NodeIf">NodeIf</span>         <span class="comment">// An if action.</span>
    <span id="NodeList">NodeList</span>       <span class="comment">// A list of Nodes.</span>
    <span id="NodeNil">NodeNil</span>        <span class="comment">// An untyped nil constant.</span>
    <span id="NodeNumber">NodeNumber</span>     <span class="comment">// A numerical constant.</span>
    <span id="NodePipe">NodePipe</span>       <span class="comment">// A pipeline of commands.</span>
    <span id="NodeRange">NodeRange</span>      <span class="comment">// A range action.</span>
    <span id="NodeString">NodeString</span>     <span class="comment">// A string constant.</span>
    <span id="NodeTemplate">NodeTemplate</span>   <span class="comment">// A template invocation action.</span>
    <span id="NodeVariable">NodeVariable</span>   <span class="comment">// A $ variable.</span>
    <span id="NodeWith">NodeWith</span>       <span class="comment">// A with action.</span>
)</pre>


<h3 id="NodeType.Type">func (NodeType) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L37">Type</a>
    <a href="#NodeType.Type">¶</a></h3>
<pre>func (t <a href="#NodeType">NodeType</a>) Type() <a href="#NodeType">NodeType</a></pre>

Type returns itself and provides an easy default implementation for embedding in
a Node. Embedded in all non-trivial Nodes.

<h2 id="NumberNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L508">NumberNode</a>
    <a href="#NumberNode">¶</a></h2>
<pre>type NumberNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="NumberNode.IsInt"></span>    IsInt      <a href="/builtin/#bool">bool</a>       <span class="comment">// Number has an integral value.</span>
<span id="NumberNode.IsUint"></span>    IsUint     <a href="/builtin/#bool">bool</a>       <span class="comment">// Number has an unsigned integral value.</span>
<span id="NumberNode.IsFloat"></span>    IsFloat    <a href="/builtin/#bool">bool</a>       <span class="comment">// Number has a floating-point value.</span>
<span id="NumberNode.IsComplex"></span>    IsComplex  <a href="/builtin/#bool">bool</a>       <span class="comment">// Number is complex.</span>
<span id="NumberNode.Int64"></span>    Int64      <a href="/builtin/#int64">int64</a>      <span class="comment">// The signed integer value.</span>
<span id="NumberNode.Uint64"></span>    Uint64     <a href="/builtin/#uint64">uint64</a>     <span class="comment">// The unsigned integer value.</span>
<span id="NumberNode.Float64"></span>    Float64    <a href="/builtin/#float64">float64</a>    <span class="comment">// The floating-point value.</span>
<span id="NumberNode.Complex128"></span>    Complex128 <a href="/builtin/#complex128">complex128</a> <span class="comment">// The complex value.</span>
<span id="NumberNode.Text"></span>    Text       <a href="/builtin/#string">string</a>     <span class="comment">// The original textual representation from the input.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

NumberNode holds a number: signed or unsigned integer, float, or complex. The
value is parsed and stored under all the types that can represent the value.
This simulates in a small amount of code the behavior of Go's ideal constants.

<h3 id="NumberNode.Copy">func (*NumberNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L634">Copy</a>
    <a href="#NumberNode.Copy">¶</a></h3>
<pre>func (n *<a href="#NumberNode">NumberNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="NumberNode.String">func (*NumberNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L626">String</a>
    <a href="#NumberNode.String">¶</a></h3>
<pre>func (n *<a href="#NumberNode">NumberNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="PipeNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L134">PipeNode</a>
    <a href="#PipeNode">¶</a></h2>
<pre>type PipeNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="PipeNode.Line"></span>    Line <a href="/builtin/#int">int</a>             <span class="comment">// The line number in the input. Deprecated: Kept for compatibility.</span>
<span id="PipeNode.Decl"></span>    Decl []*<a href="#VariableNode">VariableNode</a> <span class="comment">// Variable declarations in lexical order.</span>
<span id="PipeNode.Cmds"></span>    Cmds []*<a href="#CommandNode">CommandNode</a>  <span class="comment">// The commands in lexical order.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

PipeNode holds a pipeline with optional declaration

<h3 id="PipeNode.Copy">func (*PipeNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L190">Copy</a>
    <a href="#PipeNode.Copy">¶</a></h3>
<pre>func (p *<a href="#PipeNode">PipeNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="PipeNode.CopyPipe">func (*PipeNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L175">CopyPipe</a>
    <a href="#PipeNode.CopyPipe">¶</a></h3>
<pre>func (p *<a href="#PipeNode">PipeNode</a>) CopyPipe() *<a href="#PipeNode">PipeNode</a></pre>


<h3 id="PipeNode.String">func (*PipeNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L151">String</a>
    <a href="#PipeNode.String">¶</a></h3>
<pre>func (p *<a href="#PipeNode">PipeNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Pos">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L29">Pos</a>
    <a href="#Pos">¶</a></h2>
<pre>type Pos <a href="/builtin/#int">int</a></pre>

Pos represents a byte position in the original input text from which this
template was parsed.

<h3 id="Pos.Position">func (Pos) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L31">Position</a>
    <a href="#Pos.Position">¶</a></h3>
<pre>func (p <a href="#Pos">Pos</a>) Position() <a href="#Pos">Pos</a></pre>


<h2 id="RangeNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L777">RangeNode</a>
    <a href="#RangeNode">¶</a></h2>
<pre>type RangeNode struct {
    <a href="#BranchNode">BranchNode</a>
}</pre>

RangeNode represents a {{range}} action and its commands.

<h3 id="RangeNode.Copy">func (*RangeNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L785">Copy</a>
    <a href="#RangeNode.Copy">¶</a></h3>
<pre>func (r *<a href="#RangeNode">RangeNode</a>) Copy() <a href="#Node">Node</a></pre>


<h2 id="StringNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L641">StringNode</a>
    <a href="#StringNode">¶</a></h2>
<pre>type StringNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="StringNode.Quoted"></span>    Quoted <a href="/builtin/#string">string</a> <span class="comment">// The original text of the string, with quotes.</span>
<span id="StringNode.Text"></span>    Text   <a href="/builtin/#string">string</a> <span class="comment">// The string, after quote processing.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

StringNode holds a string constant. The value has been "unquoted".

<h3 id="StringNode.Copy">func (*StringNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L661">Copy</a>
    <a href="#StringNode.Copy">¶</a></h3>
<pre>func (s *<a href="#StringNode">StringNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="StringNode.String">func (*StringNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L653">String</a>
    <a href="#StringNode.String">¶</a></h3>
<pre>func (s *<a href="#StringNode">StringNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="TemplateNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L803">TemplateNode</a>
    <a href="#TemplateNode">¶</a></h2>
<pre>type TemplateNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="TemplateNode.Line"></span>    Line <a href="/builtin/#int">int</a>       <span class="comment">// The line number in the input. Deprecated: Kept for compatibility.</span>
<span id="TemplateNode.Name"></span>    Name <a href="/builtin/#string">string</a>    <span class="comment">// The name of the template (unquoted).</span>
<span id="TemplateNode.Pipe"></span>    Pipe *<a href="#PipeNode">PipeNode</a> <span class="comment">// The command to evaluate as dot for the template.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

TemplateNode represents a {{template}} action.

<h3 id="TemplateNode.Copy">func (*TemplateNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L827">Copy</a>
    <a href="#TemplateNode.Copy">¶</a></h3>
<pre>func (t *<a href="#TemplateNode">TemplateNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="TemplateNode.String">func (*TemplateNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L816">String</a>
    <a href="#TemplateNode.String">¶</a></h3>
<pre>func (t *<a href="#TemplateNode">TemplateNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="TextNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L110">TextNode</a>
    <a href="#TextNode">¶</a></h2>
<pre>type TextNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="TextNode.Text"></span>    Text []<a href="/builtin/#byte">byte</a> <span class="comment">// The text; may span newlines.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

TextNode holds plain text.

<h3 id="TextNode.Copy">func (*TextNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L129">Copy</a>
    <a href="#TextNode.Copy">¶</a></h3>
<pre>func (t *<a href="#TextNode">TextNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="TextNode.String">func (*TextNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L121">String</a>
    <a href="#TextNode.String">¶</a></h3>
<pre>func (t *<a href="#TextNode">TextNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="Tree">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/parse.go#L10">Tree</a>
    <a href="#Tree">¶</a></h2>
<pre>type Tree struct {
<span id="Tree.Name"></span>    Name      <a href="/builtin/#string">string</a>    <span class="comment">// name of the template represented by the tree.</span>
<span id="Tree.ParseName"></span>    ParseName <a href="/builtin/#string">string</a>    <span class="comment">// name of the top-level template during parsing, for error messages.</span>
<span id="Tree.Root"></span>    Root      *<a href="#ListNode">ListNode</a> <span class="comment">// top-level root of the tree.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Tree is the representation of a single parsed template.

<h3 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/parse.go#L115">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(name <a href="/builtin/#string">string</a>, funcs ...map[<a href="/builtin/#string">string</a>]interface{}) *<a href="#Tree">Tree</a></pre>

New allocates a new parse tree with the given name.

<h3 id="Tree.Copy">func (*Tree) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/parse.go#L25">Copy</a>
    <a href="#Tree.Copy">¶</a></h3>
<pre>func (t *<a href="#Tree">Tree</a>) Copy() *<a href="#Tree">Tree</a></pre>

Copy returns a copy of the Tree. Any parsing state is discarded.

<h3 id="Tree.ErrorContext">func (*Tree) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/parse.go#L125">ErrorContext</a>
    <a href="#Tree.ErrorContext">¶</a></h3>
<pre>func (t *<a href="#Tree">Tree</a>) ErrorContext(n <a href="#Node">Node</a>) (location, context <a href="/builtin/#string">string</a>)</pre>

ErrorContext returns a textual representation of the location of the node in the
input text. The receiver is only used when the node does not have a pointer to
the tree inside, which can occur in old code.

<h3 id="Tree.Parse">func (*Tree) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/parse.go#L218">Parse</a>
    <a href="#Tree.Parse">¶</a></h3>
<pre>func (t *<a href="#Tree">Tree</a>) Parse(text, leftDelim, rightDelim <a href="/builtin/#string">string</a>, treeSet map[<a href="/builtin/#string">string</a>]*<a href="#Tree">Tree</a>, funcs ...map[<a href="/builtin/#string">string</a>]interface{}) (tree *<a href="#Tree">Tree</a>, err <a href="/builtin/#error">error</a>)</pre>

Parse parses the template definition string to construct a representation of the
template for execution. If either action delimiter string is empty, the default
("{{" or "}}") is used. Embedded template definitions are added to the treeSet
map.

<h2 id="VariableNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L312">VariableNode</a>
    <a href="#VariableNode">¶</a></h2>
<pre>type VariableNode struct {
    <a href="#NodeType">NodeType</a>
    <a href="#Pos">Pos</a>

<span id="VariableNode.Ident"></span>    Ident []<a href="/builtin/#string">string</a> <span class="comment">// Variable name and fields in lexical order.</span>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

VariableNode holds a list of variable names, possibly with chained field
accesses. The dollar sign is part of the (first) name.

<h3 id="VariableNode.Copy">func (*VariableNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L338">Copy</a>
    <a href="#VariableNode.Copy">¶</a></h3>
<pre>func (v *<a href="#VariableNode">VariableNode</a>) Copy() <a href="#Node">Node</a></pre>


<h3 id="VariableNode.String">func (*VariableNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L323">String</a>
    <a href="#VariableNode.String">¶</a></h3>
<pre>func (v *<a href="#VariableNode">VariableNode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="WithNode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L790">WithNode</a>
    <a href="#WithNode">¶</a></h2>
<pre>type WithNode struct {
    <a href="#BranchNode">BranchNode</a>
}</pre>

WithNode represents a {{with}} action and its commands.

<h3 id="WithNode.Copy">func (*WithNode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/text/template/parse/node.go#L798">Copy</a>
    <a href="#WithNode.Copy">¶</a></h3>
<pre>func (w *<a href="#WithNode">WithNode</a>) Copy() <a href="#Node">Node</a></pre>



