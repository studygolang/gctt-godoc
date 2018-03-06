version: 1.10
## package exec

  `import "os/exec"`

## Overview

Package exec runs external commands. It wraps os.StartProcess to make it easier
to remap stdin and stdout, connect I/O with pipes, and do other adjustments.

Unlike the "system" library call from C and other languages, the os/exec package
intentionally does not invoke the system shell and does not expand any glob
patterns or handle other expansions, pipelines, or redirections typically done
by shells. The package behaves more like C's "exec" family of functions. To
expand glob patterns, either call the shell directly, taking care to escape any
dangerous input, or use the path/filepath package's Glob function. To expand
environment variables, use package os's ExpandEnv.

Note that the examples in this package assume a Unix system. They may not run on
Windows, and they do not run in the Go Playground used by golang.org and
godoc.org.

## Index

- [Variables](#pkg-variables)
- [func LookPath(file string) (string, error)](#LookPath)
- [type Cmd](#Cmd)
  - [func Command(name string, arg ...string) *Cmd](#Command)
  - [func CommandContext(ctx context.Context, name string, arg ...string) *Cmd](#CommandContext)
  - [func (c *Cmd) CombinedOutput() ([]byte, error)](#Cmd.CombinedOutput)
  - [func (c *Cmd) Output() ([]byte, error)](#Cmd.Output)
  - [func (c *Cmd) Run() error](#Cmd.Run)
  - [func (c *Cmd) Start() error](#Cmd.Start)
  - [func (c *Cmd) StderrPipe() (io.ReadCloser, error)](#Cmd.StderrPipe)
  - [func (c *Cmd) StdinPipe() (io.WriteCloser, error)](#Cmd.StdinPipe)
  - [func (c *Cmd) StdoutPipe() (io.ReadCloser, error)](#Cmd.StdoutPipe)
  - [func (c *Cmd) Wait() error](#Cmd.Wait)
- [type Error](#Error)
  - [func (e *Error) Error() string](#Error.Error)
- [type ExitError](#ExitError)
  - [func (e *ExitError) Error() string](#ExitError.Error)

### Examples

- [Cmd.CombinedOutput](#exampleCmd_CombinedOutput)
- [Cmd.Output](#exampleCmd_Output)
- [Cmd.Run](#exampleCmd_Run)
- [Cmd.Start](#exampleCmd_Start)
- [Cmd.StderrPipe](#exampleCmd_StderrPipe)
- [Cmd.StdinPipe](#exampleCmd_StdinPipe)
- [Cmd.StdoutPipe](#exampleCmd_StdoutPipe)
- [Command](#exampleCommand)
- [CommandContext](#exampleCommandContext)
- [Command (Environment)](#exampleCommand_environment)
- [LookPath](#exampleLookPath)

### Package files
 [exec.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go) [exec_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec_unix.go) [lp_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/lp_unix.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrNotFound">ErrNotFound</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;executable file not found in $PATH&#34;)</pre>

ErrNotFound is the error resulting if a path search failed to find an executable
file.

<h2 id="LookPath">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/lp_unix.go#L24">LookPath</a>
    <a href="#LookPath">¶</a></h2>
<pre>func LookPath(file <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

LookPath searches for an executable binary named file in the directories named
by the PATH environment variable. If file contains a slash, it is tried directly
and the PATH is not consulted. The result may be an absolute path or a path
relative to the current directory.

<a id="exampleLookPath"></a>
Example:

    path, err := exec.LookPath("fortune")
    if err != nil {
        log.Fatal("installing fortune is in your future")
    }
    fmt.Printf("fortune is available at %s\n", path)

<h2 id="Cmd">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L42">Cmd</a>
    <a href="#Cmd">¶</a></h2>
<pre>type Cmd struct {
<span id="Cmd.Path"></span>    <span class="comment">// Path is the path of the command to run.</span>
    <span class="comment">//</span>
    <span class="comment">// This is the only field that must be set to a non-zero</span>
    <span class="comment">// value. If Path is relative, it is evaluated relative</span>
    <span class="comment">// to Dir.</span>
    Path <a href="/builtin/#string">string</a>

<span id="Cmd.Args"></span>    <span class="comment">// Args holds command line arguments, including the command as Args[0].</span>
    <span class="comment">// If the Args field is empty or nil, Run uses {Path}.</span>
    <span class="comment">//</span>
    <span class="comment">// In typical use, both Path and Args are set by calling Command.</span>
    Args []<a href="/builtin/#string">string</a>

<span id="Cmd.Env"></span>    <span class="comment">// Env specifies the environment of the process.</span>
    <span class="comment">// Each entry is of the form &#34;key=value&#34;.</span>
    <span class="comment">// If Env is nil, the new process uses the current process&#39;s</span>
    <span class="comment">// environment.</span>
    <span class="comment">// If Env contains duplicate environment keys, only the last</span>
    <span class="comment">// value in the slice for each duplicate key is used.</span>
    Env []<a href="/builtin/#string">string</a>

<span id="Cmd.Dir"></span>    <span class="comment">// Dir specifies the working directory of the command.</span>
    <span class="comment">// If Dir is the empty string, Run runs the command in the</span>
    <span class="comment">// calling process&#39;s current directory.</span>
    Dir <a href="/builtin/#string">string</a>

<span id="Cmd.Stdin"></span>    <span class="comment">// Stdin specifies the process&#39;s standard input.</span>
    <span class="comment">//</span>
    <span class="comment">// If Stdin is nil, the process reads from the null device (os.DevNull).</span>
    <span class="comment">//</span>
    <span class="comment">// If Stdin is an *os.File, the process&#39;s standard input is connected</span>
    <span class="comment">// directly to that file.</span>
    <span class="comment">//</span>
    <span class="comment">// Otherwise, during the execution of the command a separate</span>
    <span class="comment">// goroutine reads from Stdin and delivers that data to the command</span>
    <span class="comment">// over a pipe. In this case, Wait does not complete until the goroutine</span>
    <span class="comment">// stops copying, either because it has reached the end of Stdin</span>
    <span class="comment">// (EOF or a read error) or because writing to the pipe returned an error.</span>
    Stdin <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>

<span id="Cmd.Stdout"></span>    <span class="comment">// Stdout and Stderr specify the process&#39;s standard output and error.</span>
    <span class="comment">//</span>
    <span class="comment">// If either is nil, Run connects the corresponding file descriptor</span>
    <span class="comment">// to the null device (os.DevNull).</span>
    <span class="comment">//</span>
    <span class="comment">// If either is an *os.File, the corresponding output from the process</span>
    <span class="comment">// is connected directly to that file.</span>
    <span class="comment">//</span>
    <span class="comment">// Otherwise, during the execution of the command a separate goroutine</span>
    <span class="comment">// reads from the process over a pipe and delivers that data to the</span>
    <span class="comment">// corresponding Writer. In this case, Wait does not complete until the</span>
    <span class="comment">// goroutine reaches EOF or encounters an error.</span>
    <span class="comment">//</span>
    <span class="comment">// If Stdout and Stderr are the same writer, and have a type that can</span>
    <span class="comment">// be compared with ==, at most one goroutine at a time will call Write.</span>
    Stdout <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>
<span id="Cmd.Stderr"></span>    Stderr <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>

<span id="Cmd.ExtraFiles"></span>    <span class="comment">// ExtraFiles specifies additional open files to be inherited by the</span>
    <span class="comment">// new process. It does not include standard input, standard output, or</span>
    <span class="comment">// standard error. If non-nil, entry i becomes file descriptor 3+i.</span>
    ExtraFiles []*<a href="/os/">os</a>.<a href="/os/#File">File</a>

<span id="Cmd.SysProcAttr"></span>    <span class="comment">// SysProcAttr holds optional, operating system-specific attributes.</span>
    <span class="comment">// Run passes it to os.StartProcess as the os.ProcAttr&#39;s Sys field.</span>
    SysProcAttr *<a href="/syscall/">syscall</a>.<a href="/syscall/#SysProcAttr">SysProcAttr</a>

<span id="Cmd.Process"></span>    <span class="comment">// Process is the underlying process, once started.</span>
    Process *<a href="/os/">os</a>.<a href="/os/#Process">Process</a>

<span id="Cmd.ProcessState"></span>    <span class="comment">// ProcessState contains information about an exited process,</span>
    <span class="comment">// available after a call to Wait or Run.</span>
    ProcessState *<a href="/os/">os</a>.<a href="/os/#ProcessState">ProcessState</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Cmd represents an external command being prepared or run.

A Cmd cannot be reused after calling its Run, Output or CombinedOutput methods.

<h3 id="Command">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L141">Command</a>
    <a href="#Command">¶</a></h3>
<pre>func Command(name <a href="/builtin/#string">string</a>, arg ...<a href="/builtin/#string">string</a>) *<a href="#Cmd">Cmd</a></pre>

Command returns the Cmd struct to execute the named program with the given
arguments.

It sets only the Path and Args in the returned structure.

If name contains no path separators, Command uses LookPath to resolve name to a
complete path if possible. Otherwise it uses name directly as Path.

The returned Cmd's Args field is constructed from the command name followed by
the elements of arg, so arg should not include the command name itself. For
example, Command("echo", "hello"). Args[0] is always name, not the possibly
resolved Path.

<a id="exampleCommand"></a>
Example:

    cmd := exec.Command("tr", "a-z", "A-Z")
    cmd.Stdin = strings.NewReader("some input")
    var out bytes.Buffer
    cmd.Stdout = &out
    err := cmd.Run()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("in all caps: %q\n", out.String())


<a id="exampleCommand_environment"></a>
Example:

    cmd := exec.Command("prog")
    cmd.Env = append(os.Environ(),
        "FOO=duplicate_value", // ignored
        "FOO=actual_value",    // this value is used
    )
    if err := cmd.Run(); err != nil {
        log.Fatal(err)
    }

<h3 id="CommandContext">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L161">CommandContext</a>
    <a href="#CommandContext">¶</a></h3>
<pre>func CommandContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, name <a href="/builtin/#string">string</a>, arg ...<a href="/builtin/#string">string</a>) *<a href="#Cmd">Cmd</a></pre>

CommandContext is like Command but includes a context.

The provided context is used to kill the process (by calling os.Process.Kill) if
the context becomes done before the command completes on its own.

<a id="exampleCommandContext"></a>
Example:

    ctx, cancel := context.WithTimeout(context.Background(), 100*time.Millisecond)
    defer cancel()

    if err := exec.CommandContext(ctx, "sleep", "5").Run(); err != nil {
        // This will fail after 100 milliseconds. The 5 second sleep
        // will be interrupted.
    }

<h3 id="Cmd.CombinedOutput">func (*Cmd) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L501">CombinedOutput</a>
    <a href="#Cmd.CombinedOutput">¶</a></h3>
<pre>func (c *<a href="#Cmd">Cmd</a>) CombinedOutput() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

CombinedOutput runs the command and returns its combined standard output and
standard error.

<a id="exampleCmd_CombinedOutput"></a>
Example:

    cmd := exec.Command("sh", "-c", "echo stdout; echo 1>&2 stderr")
    stdoutStderr, err := cmd.CombinedOutput()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%s\n", stdoutStderr)

<h3 id="Cmd.Output">func (*Cmd) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L478">Output</a>
    <a href="#Cmd.Output">¶</a></h3>
<pre>func (c *<a href="#Cmd">Cmd</a>) Output() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

Output runs the command and returns its standard output. Any returned error will
usually be of type *ExitError. If c.Stderr was nil, Output populates
ExitError.Stderr.

<a id="exampleCmd_Output"></a>
Example:

    out, err := exec.Command("date").Output()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("The date is %s\n", out)

<h3 id="Cmd.Run">func (*Cmd) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L291">Run</a>
    <a href="#Cmd.Run">¶</a></h3>
<pre>func (c *<a href="#Cmd">Cmd</a>) Run() <a href="/builtin/#error">error</a></pre>

Run starts the specified command and waits for it to complete.

The returned error is nil if the command runs, has no problems copying stdin,
stdout, and stderr, and exits with a zero exit status.

If the command starts but does not complete successfully, the error is of type
*ExitError. Other error types may be returned for other situations.

If the calling goroutine has locked the operating system thread with
runtime.LockOSThread and modified any inheritable OS-level thread state (for
example, Linux or Plan 9 name spaces), the new process will inherit the caller's
thread state.

<a id="exampleCmd_Run"></a>
Example:

    cmd := exec.Command("sleep", "1")
    log.Printf("Running command and waiting for it to finish...")
    err := cmd.Run()
    log.Printf("Command finished with error: %v", err)

<h3 id="Cmd.Start">func (*Cmd) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L328">Start</a>
    <a href="#Cmd.Start">¶</a></h3>
<pre>func (c *<a href="#Cmd">Cmd</a>) Start() <a href="/builtin/#error">error</a></pre>

Start starts the specified command but does not wait for it to complete.

The Wait method will return the exit code and release associated resources once
the command exits.

<a id="exampleCmd_Start"></a>
Example:

    cmd := exec.Command("sleep", "5")
    err := cmd.Start()
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Waiting for command to finish...")
    err = cmd.Wait()
    log.Printf("Command finished with error: %v", err)

<h3 id="Cmd.StderrPipe">func (*Cmd) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L588">StderrPipe</a>
    <a href="#Cmd.StderrPipe">¶</a></h3>
<pre>func (c *<a href="#Cmd">Cmd</a>) StderrPipe() (<a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>, <a href="/builtin/#error">error</a>)</pre>

StderrPipe returns a pipe that will be connected to the command's standard error
when the command starts.

Wait will close the pipe after seeing the command exit, so most callers need not
close the pipe themselves; however, an implication is that it is incorrect to
call Wait before all reads from the pipe have completed. For the same reason, it
is incorrect to use Run when using StderrPipe. See the StdoutPipe example for
idiomatic usage.

<a id="exampleCmd_StderrPipe"></a>
Example:

    cmd := exec.Command("sh", "-c", "echo stdout; echo 1>&2 stderr")
    stderr, err := cmd.StderrPipe()
    if err != nil {
        log.Fatal(err)
    }

    if err := cmd.Start(); err != nil {
        log.Fatal(err)
    }

    slurp, _ := ioutil.ReadAll(stderr)
    fmt.Printf("%s\n", slurp)

    if err := cmd.Wait(); err != nil {
        log.Fatal(err)
    }

<h3 id="Cmd.StdinPipe">func (*Cmd) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L521">StdinPipe</a>
    <a href="#Cmd.StdinPipe">¶</a></h3>
<pre>func (c *<a href="#Cmd">Cmd</a>) StdinPipe() (<a href="/io/">io</a>.<a href="/io/#WriteCloser">WriteCloser</a>, <a href="/builtin/#error">error</a>)</pre>

StdinPipe returns a pipe that will be connected to the command's standard input
when the command starts. The pipe will be closed automatically after Wait sees
the command exit. A caller need only call Close to force the pipe to close
sooner. For example, if the command being run will not exit until standard input
is closed, the caller must close the pipe.

<a id="exampleCmd_StdinPipe"></a>
Example:

    cmd := exec.Command("cat")
    stdin, err := cmd.StdinPipe()
    if err != nil {
        log.Fatal(err)
    }

    go func() {
        defer stdin.Close()
        io.WriteString(stdin, "values written to stdin are passed to cmd's standard input")
    }()

    out, err := cmd.CombinedOutput()
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("%s\n", out)

<h3 id="Cmd.StdoutPipe">func (*Cmd) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L563">StdoutPipe</a>
    <a href="#Cmd.StdoutPipe">¶</a></h3>
<pre>func (c *<a href="#Cmd">Cmd</a>) StdoutPipe() (<a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a>, <a href="/builtin/#error">error</a>)</pre>

StdoutPipe returns a pipe that will be connected to the command's standard
output when the command starts.

Wait will close the pipe after seeing the command exit, so most callers need not
close the pipe themselves; however, an implication is that it is incorrect to
call Wait before all reads from the pipe have completed. For the same reason, it
is incorrect to call Run when using StdoutPipe. See the example for idiomatic
usage.

<a id="exampleCmd_StdoutPipe"></a>
Example:

    cmd := exec.Command("echo", "-n", `{"Name": "Bob", "Age": 32}`)
    stdout, err := cmd.StdoutPipe()
    if err != nil {
        log.Fatal(err)
    }
    if err := cmd.Start(); err != nil {
        log.Fatal(err)
    }
    var person struct {
        Name string
        Age  int
    }
    if err := json.NewDecoder(stdout).Decode(&person); err != nil {
        log.Fatal(err)
    }
    if err := cmd.Wait(); err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%s is %d years old\n", person.Name, person.Age)

<h3 id="Cmd.Wait">func (*Cmd) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L442">Wait</a>
    <a href="#Cmd.Wait">¶</a></h3>
<pre>func (c *<a href="#Cmd">Cmd</a>) Wait() <a href="/builtin/#error">error</a></pre>

Wait waits for the command to exit and waits for any copying to stdin or copying
from stdout or stderr to complete.

The command must have been started by Start.

The returned error is nil if the command runs, has no problems copying stdin,
stdout, and stderr, and exits with a zero exit status.

If the command fails to run or doesn't complete successfully, the error is of
type *ExitError. Other error types may be returned for I/O problems.

If any of c.Stdin, c.Stdout or c.Stderr are not an *os.File, Wait also waits for
the respective I/O loop copying to or from the process to complete.

Wait releases any resources associated with the Cmd.

<h2 id="Error">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L29">Error</a>
    <a href="#Error">¶</a></h2>
<pre>type Error struct {
<span id="Error.Name"></span>    Name <a href="/builtin/#string">string</a>
<span id="Error.Err"></span>    Err  <a href="/builtin/#error">error</a>
}</pre>

Error records the name of a binary that failed to be executed and the reason it
failed.

<h3 id="Error.Error">func (*Error) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L34">Error</a>
    <a href="#Error.Error">¶</a></h3>
<pre>func (e *<a href="#Error">Error</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="ExitError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L405">ExitError</a>
    <a href="#ExitError">¶</a></h2>
<pre>type ExitError struct {
    *<a href="/os/">os</a>.<a href="/os/#ProcessState">ProcessState</a>

<span id="ExitError.Stderr"></span>    <span class="comment">// Stderr holds a subset of the standard error output from the</span>
    <span class="comment">// Cmd.Output method if standard error was not otherwise being</span>
    <span class="comment">// collected.</span>
    <span class="comment">//</span>
    <span class="comment">// If the error output is long, Stderr may contain only a prefix</span>
    <span class="comment">// and suffix of the output, with the middle replaced with</span>
    <span class="comment">// text about the number of omitted bytes.</span>
    <span class="comment">//</span>
    <span class="comment">// Stderr is provided for debugging, for inclusion in error messages.</span>
    <span class="comment">// Users with other needs should redirect Cmd.Stderr as needed.</span>
    Stderr []<a href="/builtin/#byte">byte</a>
}</pre>

An ExitError reports an unsuccessful exit by a command.

<h3 id="ExitError.Error">func (*ExitError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec/exec.go#L421">Error</a>
    <a href="#ExitError.Error">¶</a></h3>
<pre>func (e *<a href="#ExitError">ExitError</a>) Error() <a href="/builtin/#string">string</a></pre>



