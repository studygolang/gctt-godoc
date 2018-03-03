version: 1.10
## package syslog

  `import "log/syslog"`

## Overview

Package syslog provides a simple interface to the system log service. It can
send messages to the syslog daemon using UNIX domain sockets, UDP or TCP.

Only one call to Dial is necessary. On write failures, the syslog client will
attempt to reconnect to the server and write again.

The syslog package is frozen and is not accepting new features. Some external
packages provide more functionality. See:

    https://godoc.org/?q=syslog

## Index

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

### Examples

- [Dial](#exampleDial)

### Package files
 [doc.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/doc.go) [syslog.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go) [syslog_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog_unix.go)

<h2 id="NewLogger">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L299">NewLogger</a>
    <a href="#NewLogger">¶</a></h2>
<pre>func NewLogger(p <a href="#Priority">Priority</a>, logFlag <a href="/builtin/#int">int</a>) (*<a href="/log/">log</a>.<a href="/log/#Logger">Logger</a>, <a href="/builtin/#error">error</a>)</pre>

NewLogger creates a log.Logger whose output is written to the system log service
with the specified priority, a combination of the syslog facility and severity.
The logFlag argument is the flag set passed through to log.New to create the
Logger.

<h2 id="Priority">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L14">Priority</a>
    <a href="#Priority">¶</a></h2>
<pre>type Priority <a href="/builtin/#int">int</a></pre>

The Priority is a combination of the syslog facility and severity. For example,
LOG_ALERT | LOG_FTP sends an alert severity message from the FTP facility. The
default severity is LOG_EMERG; the default facility is LOG_KERN.

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


<h2 id="Writer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L66">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Writer is a connection to a syslog server.

<h3 id="Dial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L108">Dial</a>
    <a href="#Dial">¶</a></h3>
<pre>func Dial(network, raddr <a href="/builtin/#string">string</a>, priority <a href="#Priority">Priority</a>, tag <a href="/builtin/#string">string</a>) (*<a href="#Writer">Writer</a>, <a href="/builtin/#error">error</a>)</pre>

Dial establishes a connection to a log daemon by connecting to address raddr on
the specified network. Each write to the returned writer sends a log message
with the facility and severity (from priority) and tag. If tag is empty, the
os.Args[0] is used. If network is empty, Dial will connect to the local syslog
server. Otherwise, see the documentation for net.Dial for valid values of
network and raddr.

<a id="exampleDial"></a>
Example:

    sysLog, err := syslog.Dial("tcp", "localhost:1234",
        syslog.LOG_WARNING|syslog.LOG_DAEMON, "demotag")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Fprintf(sysLog, "This is a daemon warning with demotag.")
    sysLog.Emerg("And this is a daemon emergency with demotag.")

<h3 id="New">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L97">New</a>
    <a href="#New">¶</a></h3>
<pre>func New(priority <a href="#Priority">Priority</a>, tag <a href="/builtin/#string">string</a>) (*<a href="#Writer">Writer</a>, <a href="/builtin/#error">error</a>)</pre>

New establishes a new connection to the system log daemon. Each write to the
returned writer sends a log message with the given priority (a combination of
the syslog facility and severity) and prefix tag. If tag is empty, the
os.Args[0] is used.

<h3 id="Writer.Alert">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L190">Alert</a>
    <a href="#Writer.Alert">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Alert(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Alert logs a message with severity LOG_ALERT, ignoring the severity passed to
New.

<h3 id="Writer.Close">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L169">Close</a>
    <a href="#Writer.Close">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes a connection to the syslog daemon.

<h3 id="Writer.Crit">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L197">Crit</a>
    <a href="#Writer.Crit">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Crit(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Crit logs a message with severity LOG_CRIT, ignoring the severity passed to New.

<h3 id="Writer.Debug">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L232">Debug</a>
    <a href="#Writer.Debug">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Debug(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Debug logs a message with severity LOG_DEBUG, ignoring the severity passed to
New.

<h3 id="Writer.Emerg">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L183">Emerg</a>
    <a href="#Writer.Emerg">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Emerg(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Emerg logs a message with severity LOG_EMERG, ignoring the severity passed to
New.

<h3 id="Writer.Err">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L204">Err</a>
    <a href="#Writer.Err">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Err(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Err logs a message with severity LOG_ERR, ignoring the severity passed to New.

<h3 id="Writer.Info">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L225">Info</a>
    <a href="#Writer.Info">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Info(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Info logs a message with severity LOG_INFO, ignoring the severity passed to New.

<h3 id="Writer.Notice">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L218">Notice</a>
    <a href="#Writer.Notice">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Notice(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Notice logs a message with severity LOG_NOTICE, ignoring the severity passed to
New.

<h3 id="Writer.Warning">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L211">Warning</a>
    <a href="#Writer.Warning">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Warning(m <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Warning logs a message with severity LOG_WARNING, ignoring the severity passed
to New.

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/syslog.go#L164">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write sends a log message to the syslog daemon.

<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/doc.go#L9)  This package is not implemented on Windows. As the syslog package is frozen,
  Windows users are encouraged to use a package outside of the standard
  library. For background, see https://golang.org/issue/1108.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/doc.go#L14)  This package is not implemented on Plan 9.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/log/syslog/doc.go#L16)  This package is not implemented on NaCl (Native Client).


