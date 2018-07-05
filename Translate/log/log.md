version: 1.10
## package log

  `import "log"`

## 概述

log 包实现了一个简单的日志记录包。它定义了一个支持格式化输出的 Logger 类型。它还有一个预定义的“标准” Logger ，可以通过 Print[f|ln] ， Fatal[f|ln] ， Panic[f|ln] 等函数来访问，与手动创建一个 Logger 相比，“标准” Logger 更易于使用。 logger 会把日志打印到标准错误（屏幕），每条日志都带有日期和时间信息。每条日志占据独立的一行：如果被打印的日志没有换行符， logger 会自动添加。 Fatal 函数在打印日志后会调用 os.Exit(1) 。 Panic 函数在打印日志后会调用 panic 函数。

## 索引

- [Constants](#pkg-constants)
- [func Fatal(v ...interface{})](#Fatal)
- [func Fatalf(format string, v ...interface{})](#Fatalf)
- [func Fatalln(v ...interface{})](#Fatalln)
- [func Flags() int](#Flags)
- [func Output(calldepth int, s string) error](#Output)
- [func Panic(v ...interface{})](#Panic)
- [func Panicf(format string, v ...interface{})](#Panicf)
- [func Panicln(v ...interface{})](#Panicln)
- [func Prefix() string](#Prefix)
- [func Print(v ...interface{})](#Print)
- [func Printf(format string, v ...interface{})](#Printf)
- [func Println(v ...interface{})](#Println)
- [func SetFlags(flag int)](#SetFlags)
- [func SetOutput(w io.Writer)](#SetOutput)
- [func SetPrefix(prefix string)](#SetPrefix)
- [type Logger](#Logger)
  - [func New(out io.Writer, prefix string, flag int) *Logger](#New)
  - [func (l *Logger) Fatal(v ...interface{})](#Logger.Fatal)
  - [func (l *Logger) Fatalf(format string, v ...interface{})](#Logger.Fatalf)
  - [func (l *Logger) Fatalln(v ...interface{})](#Logger.Fatalln)
  - [func (l *Logger) Flags() int](#Logger.Flags)
  - [func (l *Logger) Output(calldepth int, s string) error](#Logger.Output)
  - [func (l *Logger) Panic(v ...interface{})](#Logger.Panic)
  - [func (l *Logger) Panicf(format string, v ...interface{})](#Logger.Panicf)
  - [func (l *Logger) Panicln(v ...interface{})](#Logger.Panicln)
  - [func (l *Logger) Prefix() string](#Logger.Prefix)
  - [func (l *Logger) Print(v ...interface{})](#Logger.Print)
  - [func (l *Logger) Printf(format string, v ...interface{})](#Logger.Printf)
  - [func (l *Logger) Println(v ...interface{})](#Logger.Println)
  - [func (l *Logger) SetFlags(flag int)](#Logger.SetFlags)
  - [func (l *Logger) SetOutput(w io.Writer)](#Logger.SetOutput)
  - [func (l *Logger) SetPrefix(prefix string)](#Logger.SetPrefix)

### 例子

- [Logger](#exampleLogger)
- [Logger.Output](#exampleLogger_Output)

### 文件
 [log.go](//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go)

<h2 id="pkg-constants">常量</h2>

<pre>const (
    <span id="Ldate">Ldate</span>         = 1 &lt;&lt; <a href="/builtin/#iota">iota</a>     <span class="comment">// 本地时区日期：2009/01/23</span>
    <span id="Ltime">Ltime</span>                         <span class="comment">// 本地时区时间： 01:23:23</span>
    <span id="Lmicroseconds">Lmicroseconds</span>                 <span class="comment">// 毫秒解析格式： 01:23:23.123123.  假设当前值是 Ltime</span>
    <span id="Llongfile">Llongfile</span>                     <span class="comment">// 带路径的文件名和行号： /a/b/c/d.go:23</span>
    <span id="Lshortfile">Lshortfile</span>                    <span class="comment">// 文件名和行号： d.go:23. overrides Llongfile</span>
    <span id="LUTC">LUTC</span>                          <span class="comment">// 如果 Ldate 或 Ltime 被赋值，使用 UTC 时区而不是本地时区</span>
    <span id="LstdFlags">LstdFlags</span>     = <a href="#Ldate">Ldate</a> | <a href="#Ltime">Ltime</a> <span class="comment">// 标准 logger 的初始值</span>
)</pre>

这些标志定义了 Logger 打印的每个日志的前缀文本。这些标志通过或操作组合来控制打印格式。标志组合的先后顺序没有影响。当 Llongfile 或 Lshortfile 被指定时，前缀文本会以冒号结尾。举个例子， Ldate | Ltime (或
 LstdFlags ) 标志组合会产生如下格式的日志：

    2009/01/23 01:23:23 message
而 Ldate | Ltime | Lmicroseconds | Llongfile 标志组合会产生如下格式的日志：

    2009/01/23 01:23:23.123123 /a/b/c/d.go:23: message

<h2 id="Fatal">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L295">Fatal</a>
    <a href="#Fatal">¶</a></h2>
<pre>func Fatal(v ...interface{})</pre>

Fatal 函数等价于调用 Print() 后调用 os.Exit(1) 。

<h2 id="Fatalf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L301">Fatalf</a>
    <a href="#Fatalf">¶</a></h2>
<pre>func Fatalf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Fatalf 函数等价于调用 Printf() 后调用 os.Exit(1) 。

<h2 id="Fatalln">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L307">Fatalln</a>
    <a href="#Fatalln">¶</a></h2>
<pre>func Fatalln(v ...interface{})</pre>

Fatalln 函数等价于调用 Println() 后调用 os.Exit(1) 。

<h2 id="Flags">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L255">Flags</a>
    <a href="#Flags">¶</a></h2>
<pre>func Flags() <a href="/builtin/#int">int</a></pre>

Flags 函数返回标准 logger 的输出参数。

<h2 id="Output">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L340">Output</a>
    <a href="#Output">¶</a></h2>
<pre>func Output(calldepth <a href="/builtin/#int">int</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Output 函数打印一个 logging 事件的信息。打印的内容包括日志内容以及被 Logger 的标志指定的前缀文本。如果参数 s 的最后一个字符不是换行符，会自动补上换行符。如果 Llongfile 或 Lshortfile 被指定，参数 calldepth 表示计算文件名和行数时，忽略的函数调用的次数。如果值为1会打印函数调用者的详细信息。

<h2 id="Panic">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L313">Panic</a>
    <a href="#Panic">¶</a></h2>
<pre>func Panic(v ...interface{})</pre>

Panic 函数等价于调用 Print() 后调用 panic() 。

<h2 id="Panicf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L320">Panicf</a>
    <a href="#Panicf">¶</a></h2>
<pre>func Panicf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Panicf 函数等价于调用 Printf() 后调用 panic() 。

<h2 id="Panicln">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L327">Panicln</a>
    <a href="#Panicln">¶</a></h2>
<pre>func Panicln(v ...interface{})</pre>

Panicln 函数等价于调用 Println() 后调用 panic() 。

<h2 id="Prefix">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L265">Prefix</a>
    <a href="#Prefix">¶</a></h2>
<pre>func Prefix() <a href="/builtin/#string">string</a></pre>

Prefix 函数返回标准 logger 的输出前缀。

<h2 id="Print">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L278">Print</a>
    <a href="#Print">¶</a></h2>
<pre>func Print(v ...interface{})</pre>

Print 函数调用 Output 函数来使用标准 logger 打印日志。参数按照 fmt.Print 函数的方式处理。

<h2 id="Printf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L284">Printf</a>
    <a href="#Printf">¶</a></h2>
<pre>func Printf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Printf 函数调用 Output 函数来使用标准 logger 打印日志。参数按照 fmt.Printf 函数的格式处理。

<h2 id="Println">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L290">Println</a>
    <a href="#Println">¶</a></h2>
<pre>func Println(v ...interface{})</pre>

Println 函数调用 Output 函数来使用标准 logger 打印日志。参数按照 fmt.Println 函数的格式处理。

<h2 id="SetFlags">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L260">SetFlags</a>
    <a href="#SetFlags">¶</a></h2>
<pre>func SetFlags(flag <a href="/builtin/#int">int</a>)</pre>

SetFlags 函数设置标准 logger 的打印标志。

<h2 id="SetOutput">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L248">SetOutput</a>
    <a href="#SetOutput">¶</a></h2>
<pre>func SetOutput(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>)</pre>

SetOutput 函数设置标准 logger 的输出目标。

<h2 id="SetPrefix">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L270">SetPrefix</a>
    <a href="#SetPrefix">¶</a></h2>
<pre>func SetPrefix(prefix <a href="/builtin/#string">string</a>)</pre>

SetPrefix 函数设置标准 logger 的打印前缀文本。

<h2 id="Logger">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L40">Logger</a>
    <a href="#Logger">¶</a></h2>
<pre>type Logger struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

一个 Logger 表示一个激活的 logging 对象，它可以向 io.Writer 写入多条记录。每次 logging 操作调用一次 Writer 的 Write 方法。一个 Logger 可以被多个 goroutines 并发使用，它保证按顺序地调用 Writer 。

<a id="exampleLogger"></a>
Example:

    var (
        buf    bytes.Buffer
        logger = log.New(&buf, "logger: ", log.Lshortfile)
    )

    logger.Print("Hello, log file!")

    fmt.Print(&buf)
    // Output:
    // logger: example_test.go:19: Hello, log file!

<h3 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L52">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(out <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, prefix <a href="/builtin/#string">string</a>, flag <a href="/builtin/#int">int</a>) *<a href="#Logger">Logger</a></pre>

New 函数创建一个新的 Logger 。参数 out 设置日志信息的输出位置。参数 prefix 出现在每条日志的开始。参数 flag 定义 logging 的属性。

<h3 id="Logger.Fatal">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L181">Fatal</a>
    <a href="#Logger.Fatal">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Fatal(v ...interface{})</pre>

Fatal 方法等价于调用 l.Print() 后调用 os.Exit(1) 。

<h3 id="Logger.Fatalf">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L187">Fatalf</a>
    <a href="#Logger.Fatalf">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Fatalf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Fatalf 方法等价于调用 l.Printf() 后调用 os.Exit(1) 。

<h3 id="Logger.Fatalln">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L193">Fatalln</a>
    <a href="#Logger.Fatalln">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Fatalln(v ...interface{})</pre>

Fatalln 方法等价于调用 l.Println() 后调用 os.Exit(1) 。

<h3 id="Logger.Flags">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L220">Flags</a>
    <a href="#Logger.Flags">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Flags() <a href="/builtin/#int">int</a></pre>

Flags 方法返回 logger 的输出标志。

<h3 id="Logger.Output">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L139">Output</a>
    <a href="#Logger.Output">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Output(calldepth <a href="/builtin/#int">int</a>, s <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Output 方法打印一个 logging 事件的信息。打印的内容包括日志内容以及被 Logger 的标志指定的前缀文本。如果参数 s 的最后一个字符不是换行符，会自动补上换行符。参数 calldepth 一般用来恢复 PC ，尽管当前所有预定义的路径都是2。

<a id="exampleLogger_Output"></a>
Example:

    var (
        buf    bytes.Buffer
        logger = log.New(&buf, "INFO: ", log.Lshortfile)

        infof = func(info string) {
            logger.Output(2, info)
        }
    )

    infof("Hello world")

    fmt.Print(&buf)
    // Output:
    // INFO: example_test.go:36: Hello world

<h3 id="Logger.Panic">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L199">Panic</a>
    <a href="#Logger.Panic">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Panic(v ...interface{})</pre>

Panic 方法等价于调用 l.Print() 后调用 panic() 。

<h3 id="Logger.Panicf">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L206">Panicf</a>
    <a href="#Logger.Panicf">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Panicf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Panicf 方法等价于调用 l.Printf() 后调用 panic() 。

<h3 id="Logger.Panicln">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L213">Panicln</a>
    <a href="#Logger.Panicln">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Panicln(v ...interface{})</pre>

Panicln 方法等价于调用 l.Println() 后调用 panic() 。

<h3 id="Logger.Prefix">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L234">Prefix</a>
    <a href="#Logger.Prefix">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Prefix() <a href="/builtin/#string">string</a></pre>

Prefix 方法返回 logger 的输出前缀文本。

<h3 id="Logger.Print">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L174">Print</a>
    <a href="#Logger.Print">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Print(v ...interface{})</pre>

Print 方法调用 l.Output 函数来使用 logger 打印日志。参数按照 fmt.Print 函数的格式处理。

<h3 id="Logger.Printf">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L168">Printf</a>
    <a href="#Logger.Printf">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Printf(format <a href="/builtin/#string">string</a>, v ...interface{})</pre>

Printf 方法调用 l.Output 函数来使用 logger 打印日志。参数按照 fmt.Printf 函数的格式处理。

<h3 id="Logger.Println">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L178">Println</a>
    <a href="#Logger.Println">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) Println(v ...interface{})</pre>

Println 方法调用 l.Output 函数来使用 logger 打印日志。参数按照 fmt.Println 函数的格式处理。

<h3 id="Logger.SetFlags">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L227">SetFlags</a>
    <a href="#Logger.SetFlags">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) SetFlags(flag <a href="/builtin/#int">int</a>)</pre>

SetFlags 方法设置 logger 的打印标志。

<h3 id="Logger.SetOutput">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L57">SetOutput</a>
    <a href="#Logger.SetOutput">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) SetOutput(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>)</pre>

SetOutput 方法设置 logger 的输出目标。

<h3 id="Logger.SetPrefix">func (*Logger) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/log.go#L241">SetPrefix</a>
    <a href="#Logger.SetPrefix">¶</a></h3>
<pre>func (l *<a href="#Logger">Logger</a>) SetPrefix(prefix <a href="/builtin/#string">string</a>)</pre>

SetPrefix 方法设置 logger 的打印前缀文本。

## Subdirectories
- [..](..)
- [syslog](syslog/)
