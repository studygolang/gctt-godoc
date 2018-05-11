version: 1.10
## package syslog

  `import "log/syslog"`

## 概述

syslog包提供了一个简单的系统日志服务接口。它能通过UNIX域套接字、UDP或TCP，向syslog守护进程发送消息。

Dial函数只需要被调用一次。如果写失败，syslog客户端会尝试重新连接服务器并再次写。

syslog包处于冻结状态，不会接受新的功能。一些其它包提供更多的功能。参考：

    https://godoc.org/?q=syslog

## 索引

- [func NewLogger(p Priority, logFlag int) (*log.Logger, error)](#NewLogger)
- [type Priority](#Priority)
- [type Writer](#Writer)
  - [func Dial(network, raddr string, priority Priority, tag string) (*Writer, error)](#Dial)
  - [func New(priority Priority, tag string) (*Writer, error)](#New)
  - [func (w *Writer) Alert(m string) error](#Writer.Alert)
  - [func (w *Writer) Close() error](#Writer.Close)
  - [func (w *Writer) Crit(m string) error](#Writer.Crit)
  - [func (w *Writer) Debug(m string) error](#Writer.Debug)
  - [func (w *Writer) Emerg(m string) error](#Writer.Emerg)
  - [func (w *Writer) Err(m string) error](#Writer.Err)
  - [func (w *Writer) Info(m string) error](#Writer.Info)
  - [func (w *Writer) Notice(m string) error](#Writer.Notice)
  - [func (w *Writer) Warning(m string) error](#Writer.Warning)
  - [func (w *Writer) Write(b []byte) (int, error)](#Writer.Write)
- [Bugs](#pkg-note-BUG)

### 例子

- [Dial](#exampleDial)

### 文件
 [doc.go](//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/doc.go) [syslog.go](//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go) [syslog_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog_unix.go)

<h2 id="NewLogger">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L299">NewLogger</a>
    <a href="#NewLogger">¶</a></h2>
<pre>func NewLogger(p <a href="#Priority">Priority</a>, logFlag <a href="/builtin/#int">int</a>) (*<a href="/log/">log</a>.<a href="/log/#Logger">Logger</a>, <a href="/builtin/#error">error</a>)</pre>

NewLogger函数创建一个log.Logger对象，它会把携带优先级的消息写到系统日志服务，优先级是syslog设备和严重程度的结合。参数logFlag和log.New函数的flag参数作用一样。

<h2 id="Priority">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L14">Priority</a>
    <a href="#Priority">¶</a></h2>
<pre>type Priority <a href="/builtin/#int">int</a></pre>

Priority是syslog设备和严重程度的结合。比如，LOG_ALERT | LOG_FTP发送一个警告消息到FTP设备。默认的严重程度是LOG_EMERG，默认的设备是LOG_KERN。

<pre>const (

    <span class="comment">// From /usr/include/sys/syslog.h.</span>
    <span class="comment">// These are the same on Linux, BSD, and OS X.</span>
    <span id="LOG_EMERG">LOG_EMERG</span> <a href="#Priority">Priority</a> = <a href="/builtin/#iota">iota</a>
    <span id="LOG_ALERT">LOG_ALERT</span>
    <span id="LOG_CRIT">LOG_CRIT</span>
    <span id="LOG_ERR">LOG_ERR</span>
    <span id="LOG_WARNING">LOG_WARNING</span>
    <span id="LOG_NOTICE">LOG_NOTICE</span>
    <span id="LOG_INFO">LOG_INFO</span>
    <span id="LOG_DEBUG">LOG_DEBUG</span>
)</pre>


<pre>const (

    <span class="comment">// From /usr/include/sys/syslog.h.</span>
    <span class="comment">// These are the same up to LOG_FTP on Linux, BSD, and OS X.</span>
    <span id="LOG_KERN">LOG_KERN</span> <a href="#Priority">Priority</a> = <a href="/builtin/#iota">iota</a> &lt;&lt; 3
    <span id="LOG_USER">LOG_USER</span>
    <span id="LOG_MAIL">LOG_MAIL</span>
    <span id="LOG_DAEMON">LOG_DAEMON</span>
    <span id="LOG_AUTH">LOG_AUTH</span>
    <span id="LOG_SYSLOG">LOG_SYSLOG</span>
    <span id="LOG_LPR">LOG_LPR</span>
    <span id="LOG_NEWS">LOG_NEWS</span>
    <span id="LOG_UUCP">LOG_UUCP</span>
    <span id="LOG_CRON">LOG_CRON</span>
    <span id="LOG_AUTHPRIV">LOG_AUTHPRIV</span>
    <span id="LOG_FTP">LOG_FTP</span>

    <span id="LOG_LOCAL0">LOG_LOCAL0</span>
    <span id="LOG_LOCAL1">LOG_LOCAL1</span>
    <span id="LOG_LOCAL2">LOG_LOCAL2</span>
    <span id="LOG_LOCAL3">LOG_LOCAL3</span>
    <span id="LOG_LOCAL4">LOG_LOCAL4</span>
    <span id="LOG_LOCAL5">LOG_LOCAL5</span>
    <span id="LOG_LOCAL6">LOG_LOCAL6</span>
    <span id="LOG_LOCAL7">LOG_LOCAL7</span>
)</pre>


<h2 id="Writer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L66">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
    <span class="comment">// 包含过滤的或未导出的字段</span>
}</pre>

Writer是一个syslog服务器的连接。

<h3 id="Dial">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L108">Dial</a>
    <a href="#Dial">¶</a></h3>
<pre>func Dial(network, raddr <a href="/builtin/#string">string</a>, priority <a href="#Priority">Priority</a>, tag <a href="/builtin/#string">string</a>) (*<a href="#Writer">Writer</a>, <a href="/builtin/#error">error</a>)</pre>

Dial函数与由参数raddr和network指定的日志守护进程建立一个连接。每次写都会发送一个携带设备和严重程度（由参数priority指定），标签的日志消息到该函数返回的writer对象。如果参数tag为空，os.Args[0]会被使用。如果参数network为空，Dial函数将连接本地syslog服务。否则，请参考文档来了解net.Dial函数的network参数和raddr参数的有效的取值范围。

<a id="exampleDial"></a>
Example:

    sysLog, err := syslog.Dial("tcp", "localhost:1234",
        syslog.LOG_WARNING|syslog.LOG_DAEMON, "demotag")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Fprintf(sysLog, "This is a daemon warning with demotag.")
    sysLog.Emerg("And this is a daemon emergency with demotag.")

<h3 id="New">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L97">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(priority <a href="#Priority">Priority</a>, tag <a href="/builtin/#string">string</a>) (*<a href="#Writer">Writer</a>, <a href="/builtin/#error">error</a>)</pre>

New函数与系统日志守护进程建立一个新的连接。每次写都会发送一个携带指定优先级（syslog设备和严重程度的结合）和前缀标签的日志消息到该函数返回的writer对象。如果参数tag为空，os.Args[0]会被使用。

<h3 id="Writer.Alert">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L190">Alert</a>
    <a href="#Writer.Alert">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Alert(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Alert方法记录一个严重程度是LOG_ALERT的消息，忽略New函数里的severity参数。

<h3 id="Writer.Close">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L169">Close</a>
    <a href="#Writer.Close">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Close() <a href="/builtin/#error">error</a></pre>

Close方法关闭与syslog守护进程的连接。

<h3 id="Writer.Crit">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L197">Crit</a>
    <a href="#Writer.Crit">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Crit(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Crit方法记录一个严重程度是LOG_CRIT的消息，忽略New函数里的severity参数。

<h3 id="Writer.Debug">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L232">Debug</a>
    <a href="#Writer.Debug">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Debug(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Debug方法记录一个严重程度是LOG_DEBUG的消息，忽略New函数里的severity参数。

<h3 id="Writer.Emerg">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L183">Emerg</a>
    <a href="#Writer.Emerg">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Emerg(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Emerg方法记录一个严重程度是LOG_EMERG的消息，忽略New函数里的severity参数。

<h3 id="Writer.Err">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L204">Err</a>
    <a href="#Writer.Err">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Err(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Err方法记录一个严重程度是LOG_ERR的消息，忽略New函数里的severity参数。

<h3 id="Writer.Info">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L225">Info</a>
    <a href="#Writer.Info">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Info(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Info方法记录一个严重程度是LOG_INFO的消息，忽略New函数里的severity参数。

<h3 id="Writer.Notice">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L218">Notice</a>
    <a href="#Writer.Notice">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Notice(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Notice方法记录一个严重程度是LOG_NOTICE的消息，忽略New函数里的severity参数。

<h3 id="Writer.Warning">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L211">Warning</a>
    <a href="#Writer.Warning">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Warning(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Warning方法记录一个严重程度是LOG_WARNING的消息，忽略New函数里的severity参数。

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/syslog.go#L164">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write方法发送一个日志消息到syslog守护进程。

<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/doc.go#L9)  这个包不支持Windows。鉴于syslog包处于冻结状态，
  我们鼓励Windows用户使用标准库外的包。想了解更多背景，请参考 https://golang.org/issue/1108。
- [☞](//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/doc.go#L14)  这个包不支持Plan 9。
- [☞](//github.com/golang/go/blob/release-branch.go1.10/src/log/syslog/doc.go#L16)  这个包不支持NaCl (Native Client)。


