version: 1.10
## package os

  `import "os"`

## Overview

Package os provides a platform-independent interface to operating system
functionality. The design is Unix-like, although the error handling is Go-like;
failing calls return values of type error rather than error numbers. Often, more
information is available within the error. For example, if a call that takes a
file name fails, such as Open or Stat, the error will include the failing file
name when printed and will be of type *PathError, which may be unpacked for more
information.

The os interface is intended to be uniform across all operating systems.
Features not generally available appear in the system-specific package syscall.

Here is a simple example, opening a file and reading some of it.

    file, err := os.Open("file.go") // For read access.
    if err != nil {
    	log.Fatal(err)
    }

If the open fails, the error string will be self-explanatory, like

    open file.go: no such file or directory

The file's data can then be read into a slice of bytes. Read and Write take
their byte counts from the length of the argument slice.

    data := make([]byte, 100)
    count, err := file.Read(data)
    if err != nil {
    	log.Fatal(err)
    }
    fmt.Printf("read %d bytes: %q\n", count, data[:count])

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func Chdir(dir string) error](#Chdir)
- [func Chmod(name string, mode FileMode) error](#Chmod)
- [func Chown(name string, uid, gid int) error](#Chown)
- [func Chtimes(name string, atime time.Time, mtime time.Time) error](#Chtimes)
- [func Clearenv()](#Clearenv)
- [func Environ() []string](#Environ)
- [func Executable() (string, error)](#Executable)
- [func Exit(code int)](#Exit)
- [func Expand(s string, mapping func(string) string) string](#Expand)
- [func ExpandEnv(s string) string](#ExpandEnv)
- [func Getegid() int](#Getegid)
- [func Getenv(key string) string](#Getenv)
- [func Geteuid() int](#Geteuid)
- [func Getgid() int](#Getgid)
- [func Getgroups() ([]int, error)](#Getgroups)
- [func Getpagesize() int](#Getpagesize)
- [func Getpid() int](#Getpid)
- [func Getppid() int](#Getppid)
- [func Getuid() int](#Getuid)
- [func Getwd() (dir string, err error)](#Getwd)
- [func Hostname() (name string, err error)](#Hostname)
- [func IsExist(err error) bool](#IsExist)
- [func IsNotExist(err error) bool](#IsNotExist)
- [func IsPathSeparator(c uint8) bool](#IsPathSeparator)
- [func IsPermission(err error) bool](#IsPermission)
- [func IsTimeout(err error) bool](#IsTimeout)
- [func Lchown(name string, uid, gid int) error](#Lchown)
- [func Link(oldname, newname string) error](#Link)
- [func LookupEnv(key string) (string, bool)](#LookupEnv)
- [func Mkdir(name string, perm FileMode) error](#Mkdir)
- [func MkdirAll(path string, perm FileMode) error](#MkdirAll)
- [func NewSyscallError(syscall string, err error) error](#NewSyscallError)
- [func Readlink(name string) (string, error)](#Readlink)
- [func Remove(name string) error](#Remove)
- [func RemoveAll(path string) error](#RemoveAll)
- [func Rename(oldpath, newpath string) error](#Rename)
- [func SameFile(fi1, fi2 FileInfo) bool](#SameFile)
- [func Setenv(key, value string) error](#Setenv)
- [func Symlink(oldname, newname string) error](#Symlink)
- [func TempDir() string](#TempDir)
- [func Truncate(name string, size int64) error](#Truncate)
- [func Unsetenv(key string) error](#Unsetenv)
- [type File](#File)
  - [func Create(name string) (*File, error)](#Create)
  - [func NewFile(fd uintptr, name string) *File](#NewFile)
  - [func Open(name string) (*File, error)](#Open)
  - [func OpenFile(name string, flag int, perm FileMode) (*File, error)](#OpenFile)
  - [func Pipe() (r *File, w *File, err error)](#Pipe)
  - [func (f *File) Chdir() error](#File.Chdir)
  - [func (f *File) Chmod(mode FileMode) error](#File.Chmod)
  - [func (f *File) Chown(uid, gid int) error](#File.Chown)
  - [func (f *File) Close() error](#File.Close)
  - [func (f *File) Fd() uintptr](#File.Fd)
  - [func (f *File) Name() string](#File.Name)
  - [func (f *File) Read(b []byte) (n int, err error)](#File.Read)
  - [func (f *File) ReadAt(b []byte, off int64) (n int, err error)](#File.ReadAt)
  - [func (f *File) Readdir(n int) ([]FileInfo, error)](#File.Readdir)
  - [func (f *File) Readdirnames(n int) (names []string, err error)](#File.Readdirnames)
  - [func (f *File) Seek(offset int64, whence int) (ret int64, err error)](#File.Seek)
  - [func (f *File) SetDeadline(t time.Time) error](#File.SetDeadline)
  - [func (f *File) SetReadDeadline(t time.Time) error](#File.SetReadDeadline)
  - [func (f *File) SetWriteDeadline(t time.Time) error](#File.SetWriteDeadline)
  - [func (f *File) Stat() (FileInfo, error)](#File.Stat)
  - [func (f *File) Sync() error](#File.Sync)
  - [func (f *File) Truncate(size int64) error](#File.Truncate)
  - [func (f *File) Write(b []byte) (n int, err error)](#File.Write)
  - [func (f *File) WriteAt(b []byte, off int64) (n int, err error)](#File.WriteAt)
  - [func (f *File) WriteString(s string) (n int, err error)](#File.WriteString)
- [type FileInfo](#FileInfo)
  - [func Lstat(name string) (FileInfo, error)](#Lstat)
  - [func Stat(name string) (FileInfo, error)](#Stat)
- [type FileMode](#FileMode)
  - [func (m FileMode) IsDir() bool](#FileMode.IsDir)
  - [func (m FileMode) IsRegular() bool](#FileMode.IsRegular)
  - [func (m FileMode) Perm() FileMode](#FileMode.Perm)
  - [func (m FileMode) String() string](#FileMode.String)
- [type LinkError](#LinkError)
  - [func (e *LinkError) Error() string](#LinkError.Error)
- [type PathError](#PathError)
  - [func (e *PathError) Error() string](#PathError.Error)
  - [func (e *PathError) Timeout() bool](#PathError.Timeout)
- [type ProcAttr](#ProcAttr)
- [type Process](#Process)
  - [func FindProcess(pid int) (*Process, error)](#FindProcess)
  - [func StartProcess(name string, argv []string, attr *ProcAttr) (*Process, error)](#StartProcess)
  - [func (p *Process) Kill() error](#Process.Kill)
  - [func (p *Process) Release() error](#Process.Release)
  - [func (p *Process) Signal(sig Signal) error](#Process.Signal)
  - [func (p *Process) Wait() (*ProcessState, error)](#Process.Wait)
- [type ProcessState](#ProcessState)
  - [func (p *ProcessState) Exited() bool](#ProcessState.Exited)
  - [func (p *ProcessState) Pid() int](#ProcessState.Pid)
  - [func (p *ProcessState) String() string](#ProcessState.String)
  - [func (p *ProcessState) Success() bool](#ProcessState.Success)
  - [func (p *ProcessState) Sys() interface{}](#ProcessState.Sys)
  - [func (p *ProcessState) SysUsage() interface{}](#ProcessState.SysUsage)
  - [func (p *ProcessState) SystemTime() time.Duration](#ProcessState.SystemTime)
  - [func (p *ProcessState) UserTime() time.Duration](#ProcessState.UserTime)
- [type Signal](#Signal)
- [type SyscallError](#SyscallError)
  - [func (e *SyscallError) Error() string](#SyscallError.Error)
  - [func (e *SyscallError) Timeout() bool](#SyscallError.Timeout)

### Examples

- [Chmod](#exampleChmod)
- [Chtimes](#exampleChtimes)
- [ExpandEnv](#exampleExpandEnv)
- [FileMode](#exampleFileMode)
- [Getenv](#exampleGetenv)
- [IsNotExist](#exampleIsNotExist)
- [LookupEnv](#exampleLookupEnv)
- [OpenFile](#exampleOpenFile)
- [OpenFile (Append)](#exampleOpenFile_append)
- [Unsetenv](#exampleUnsetenv)

### Package files
 [dir.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/dir.go) [dir_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/dir_unix.go) [env.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/env.go) [error.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go) [error_posix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/error_posix.go) [error_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/error_unix.go) [exec.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go) [exec_posix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/exec_posix.go) [exec_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/exec_unix.go) [executable.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/executable.go) [executable_procfs.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/executable_procfs.go) [file.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go) [file_posix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/file_posix.go) [file_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/file_unix.go) [getwd.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/getwd.go) [path.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/path.go) [path_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/path_unix.go) [pipe_linux.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/pipe_linux.go) [proc.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/proc.go) [stat.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/stat.go) [stat_linux.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/stat_linux.go) [stat_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/stat_unix.go) [sticky_notbsd.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/sticky_notbsd.go) [str.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/str.go) [sys.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/sys.go) [sys_linux.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/sys_linux.go) [sys_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/sys_unix.go) [types.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/types.go) [types_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/types_unix.go) [wait_waitid.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/wait_waitid.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span class="comment">// Exactly one of O_RDONLY, O_WRONLY, or O_RDWR must be specified.</span>
    <span id="O_RDONLY">O_RDONLY</span> <a href="/builtin/#int">int</a> = <a href="/syscall/">syscall</a>.<a href="/syscall/#O_RDONLY">O_RDONLY</a> <span class="comment">// open the file read-only.</span>
    <span id="O_WRONLY">O_WRONLY</span> <a href="/builtin/#int">int</a> = <a href="/syscall/">syscall</a>.<a href="/syscall/#O_WRONLY">O_WRONLY</a> <span class="comment">// open the file write-only.</span>
    <span id="O_RDWR">O_RDWR</span>   <a href="/builtin/#int">int</a> = <a href="/syscall/">syscall</a>.<a href="/syscall/#O_RDWR">O_RDWR</a>   <span class="comment">// open the file read-write.</span>
    <span class="comment">// The remaining values may be or&#39;ed in to control behavior.</span>
    <span id="O_APPEND">O_APPEND</span> <a href="/builtin/#int">int</a> = <a href="/syscall/">syscall</a>.<a href="/syscall/#O_APPEND">O_APPEND</a> <span class="comment">// append data to the file when writing.</span>
    <span id="O_CREATE">O_CREATE</span> <a href="/builtin/#int">int</a> = <a href="/syscall/">syscall</a>.<a href="/syscall/#O_CREAT">O_CREAT</a>  <span class="comment">// create a new file if none exists.</span>
    <span id="O_EXCL">O_EXCL</span>   <a href="/builtin/#int">int</a> = <a href="/syscall/">syscall</a>.<a href="/syscall/#O_EXCL">O_EXCL</a>   <span class="comment">// used with O_CREATE, file must not exist.</span>
    <span id="O_SYNC">O_SYNC</span>   <a href="/builtin/#int">int</a> = <a href="/syscall/">syscall</a>.<a href="/syscall/#O_SYNC">O_SYNC</a>   <span class="comment">// open for synchronous I/O.</span>
    <span id="O_TRUNC">O_TRUNC</span>  <a href="/builtin/#int">int</a> = <a href="/syscall/">syscall</a>.<a href="/syscall/#O_TRUNC">O_TRUNC</a>  <span class="comment">// if possible, truncate file when opened.</span>
)</pre>

Flags to OpenFile wrapping those of the underlying system. Not all flags may be
implemented on a given system.

<pre>const (
    <span id="SEEK_SET">SEEK_SET</span> <a href="/builtin/#int">int</a> = 0 <span class="comment">// seek relative to the origin of the file</span>
    <span id="SEEK_CUR">SEEK_CUR</span> <a href="/builtin/#int">int</a> = 1 <span class="comment">// seek relative to the current offset</span>
    <span id="SEEK_END">SEEK_END</span> <a href="/builtin/#int">int</a> = 2 <span class="comment">// seek relative to the end</span>
)</pre>

Seek whence values.

Deprecated: Use io.SeekStart, io.SeekCurrent, and io.SeekEnd.

<pre>const (
    <span id="PathSeparator">PathSeparator</span>     = &#39;/&#39; <span class="comment">// OS-specific path separator</span>
    <span id="PathListSeparator">PathListSeparator</span> = &#39;:&#39; <span class="comment">// OS-specific path list separator</span>
)</pre>


<pre>const <span id="DevNull">DevNull</span> = &#34;/dev/null&#34;</pre>

DevNull is the name of the operating system's ``null device.'' On Unix-like
systems, it is "/dev/null"; on Windows, "NUL".

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="ErrInvalid">ErrInvalid</span>    = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;invalid argument&#34;) <span class="comment">// methods on File will return this error when the receiver is nil</span>
    <span id="ErrPermission">ErrPermission</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;permission denied&#34;)
    <span id="ErrExist">ErrExist</span>      = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;file already exists&#34;)
    <span id="ErrNotExist">ErrNotExist</span>   = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;file does not exist&#34;)
    <span id="ErrClosed">ErrClosed</span>     = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;file already closed&#34;)
    <span id="ErrNoDeadline">ErrNoDeadline</span> = <a href="/internal/poll/">poll</a>.<a href="/internal/poll/#ErrNoDeadline">ErrNoDeadline</a>
)</pre>

Portable analogs of some common system call errors.

<pre>var (
    <span id="Stdin">Stdin</span>  = <a href="#NewFile">NewFile</a>(<a href="/builtin/#uintptr">uintptr</a>(<a href="/syscall/">syscall</a>.<a href="/syscall/#Stdin">Stdin</a>), &#34;/dev/stdin&#34;)
    <span id="Stdout">Stdout</span> = <a href="#NewFile">NewFile</a>(<a href="/builtin/#uintptr">uintptr</a>(<a href="/syscall/">syscall</a>.<a href="/syscall/#Stdout">Stdout</a>), &#34;/dev/stdout&#34;)
    <span id="Stderr">Stderr</span> = <a href="#NewFile">NewFile</a>(<a href="/builtin/#uintptr">uintptr</a>(<a href="/syscall/">syscall</a>.<a href="/syscall/#Stderr">Stderr</a>), &#34;/dev/stderr&#34;)
)</pre>

Stdin, Stdout, and Stderr are open Files pointing to the standard input,
standard output, and standard error file descriptors.

Note that the Go runtime writes to standard error for panics and crashes;
closing Stderr may cause those messages to go elsewhere, perhaps to a file
opened later.

<pre>var <span id="Args">Args</span> []<a href="/builtin/#string">string</a></pre>

Args hold the command-line arguments, starting with the program name.

<h2 id="Chdir">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L221">Chdir</a>
    <a href="#Chdir">¶</a></h2>
<pre>func Chdir(dir <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Chdir changes the current working directory to the named directory. If there is
an error, it will be of type *PathError.

<h2 id="Chmod">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L326">Chmod</a>
    <a href="#Chmod">¶</a></h2>
<pre>func Chmod(name <a href="/builtin/#string">string</a>, mode <a href="#FileMode">FileMode</a>) <a href="/builtin/#error">error</a></pre>

Chmod changes the mode of the named file to mode. If the file is a symbolic
link, it changes the mode of the link's target. If there is an error, it will be
of type *PathError.

A different subset of the mode bits are used, depending on the operating system.

On Unix, the mode's permission bits, ModeSetuid, ModeSetgid, and ModeSticky are
used.

On Windows, the mode must be non-zero but otherwise only the 0200 bit (owner
writable) of mode is used; it controls whether the file's read-only attribute is
set or cleared. attribute. The other bits are currently unused. Use mode 0400
for a read-only file and 0600 for a readable+writable file.

On Plan 9, the mode's permission bits, ModeAppend, ModeExclusive, and
ModeTemporary are used.

<a id="exampleChmod"></a>
Example:

    if err := os.Chmod("some-filename", 0644); err != nil {
        log.Fatal(err)
    }

<h2 id="Chown">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_posix.go#L62">Chown</a>
    <a href="#Chown">¶</a></h2>
<pre>func Chown(name <a href="/builtin/#string">string</a>, uid, gid <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

Chown changes the numeric uid and gid of the named file. If the file is a
symbolic link, it changes the uid and gid of the link's target. If there is an
error, it will be of type *PathError.

On Windows, it always returns the syscall.EWINDOWS error, wrapped in *PathError.

<h2 id="Chtimes">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_posix.go#L129">Chtimes</a>
    <a href="#Chtimes">¶</a></h2>
<pre>func Chtimes(name <a href="/builtin/#string">string</a>, atime <a href="/time/">time</a>.<a href="/time/#Time">Time</a>, mtime <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

Chtimes changes the access and modification times of the named file, similar to
the Unix utime() or utimes() functions.

The underlying filesystem may truncate or round the values to a less precise
time unit. If there is an error, it will be of type *PathError.

<a id="exampleChtimes"></a>
Example:

    mtime := time.Date(2006, time.February, 1, 3, 4, 5, 0, time.UTC)
    atime := time.Date(2007, time.March, 2, 4, 5, 6, 0, time.UTC)
    if err := os.Chtimes("some-filename", atime, mtime); err != nil {
        log.Fatal(err)
    }

<h2 id="Clearenv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/env.go#L105">Clearenv</a>
    <a href="#Clearenv">¶</a></h2>
<pre>func Clearenv()</pre>

Clearenv deletes all environment variables.

<h2 id="Environ">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/env.go#L111">Environ</a>
    <a href="#Environ">¶</a></h2>
<pre>func Environ() []<a href="/builtin/#string">string</a></pre>

Environ returns a copy of strings representing the environment, in the form
"key=value".

<h2 id="Executable">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/executable.go#L10">Executable</a>
    <a href="#Executable">¶</a></h2>
<pre>func Executable() (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

Executable returns the path name for the executable that started the current
process. There is no guarantee that the path is still pointing to the correct
executable. If a symlink was used to start the process, depending on the
operating system, the result might be the symlink or the path it pointed to. If
a stable result is needed, path/filepath.EvalSymlinks might help.

Executable returns an absolute path unless an error occurred.

The main use case is finding resources located relative to an executable.

Executable is not supported on nacl.

<h2 id="Exit">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/proc.go#L49">Exit</a>
    <a href="#Exit">¶</a></h2>
<pre>func Exit(code <a href="/builtin/#int">int</a>)</pre>

Exit causes the current program to exit with the given status code.
Conventionally, code zero indicates success, non-zero an error. The program
terminates immediately; deferred functions are not run.

<h2 id="Expand">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/env.go#L6">Expand</a>
    <a href="#Expand">¶</a></h2>
<pre>func Expand(s <a href="/builtin/#string">string</a>, mapping func(<a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Expand replaces ${var} or $var in the string based on the mapping function. For
example, os.ExpandEnv(s) is equivalent to os.Expand(s, os.Getenv).

<h2 id="ExpandEnv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/env.go#L25">ExpandEnv</a>
    <a href="#ExpandEnv">¶</a></h2>
<pre>func ExpandEnv(s <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

ExpandEnv replaces ${var} or $var in the string according to the values of the
current environment variables. References to undefined variables are replaced by
the empty string.

<a id="exampleExpandEnv"></a>
Example:

    fmt.Println(os.ExpandEnv("$USER lives in ${HOME}."))

    // Output:
    // gopher lives in /usr/gopher.

<h2 id="Getegid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/proc.go#L35">Getegid</a>
    <a href="#Getegid">¶</a></h2>
<pre>func Getegid() <a href="/builtin/#int">int</a></pre>

Getegid returns the numeric effective group id of the caller.

On Windows, it returns -1.

<h2 id="Getenv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/env.go#L73">Getenv</a>
    <a href="#Getenv">¶</a></h2>
<pre>func Getenv(key <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

Getenv retrieves the value of the environment variable named by the key. It
returns the value, which will be empty if the variable is not present. To
distinguish between an empty value and an unset value, use LookupEnv.

<a id="exampleGetenv"></a>
Example:

    fmt.Printf("%s lives in %s.\n", os.Getenv("USER"), os.Getenv("HOME"))

    // Output:
    // gopher lives in /usr/gopher.

<h2 id="Geteuid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/proc.go#L25">Geteuid</a>
    <a href="#Geteuid">¶</a></h2>
<pre>func Geteuid() <a href="/builtin/#int">int</a></pre>

Geteuid returns the numeric effective user id of the caller.

On Windows, it returns -1.

<h2 id="Getgid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/proc.go#L30">Getgid</a>
    <a href="#Getgid">¶</a></h2>
<pre>func Getgid() <a href="/builtin/#int">int</a></pre>

Getgid returns the numeric group id of the caller.

On Windows, it returns -1.

<h2 id="Getgroups">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/proc.go#L41">Getgroups</a>
    <a href="#Getgroups">¶</a></h2>
<pre>func Getgroups() ([]<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Getgroups returns a list of the numeric ids of groups that the caller belongs
to.

On Windows, it returns syscall.EWINDOWS. See the os/user package for a possible
alternative.

<h2 id="Getpagesize">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/types.go#L3">Getpagesize</a>
    <a href="#Getpagesize">¶</a></h2>
<pre>func Getpagesize() <a href="/builtin/#int">int</a></pre>

Getpagesize returns the underlying system's memory page size.

<h2 id="Getpid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L61">Getpid</a>
    <a href="#Getpid">¶</a></h2>
<pre>func Getpid() <a href="/builtin/#int">int</a></pre>

Getpid returns the process id of the caller.

<h2 id="Getppid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L64">Getppid</a>
    <a href="#Getppid">¶</a></h2>
<pre>func Getppid() <a href="/builtin/#int">int</a></pre>

Getppid returns the process id of the caller's parent.

<h2 id="Getuid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/proc.go#L20">Getuid</a>
    <a href="#Getuid">¶</a></h2>
<pre>func Getuid() <a href="/builtin/#int">int</a></pre>

Getuid returns the numeric user id of the caller.

On Windows, it returns -1.

<h2 id="Getwd">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/getwd.go#L16">Getwd</a>
    <a href="#Getwd">¶</a></h2>
<pre>func Getwd() (dir <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

Getwd returns a rooted path name corresponding to the current directory. If the
current directory can be reached via multiple paths (due to symbolic links),
Getwd may return any one of them.

<h2 id="Hostname">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/sys.go#L1">Hostname</a>
    <a href="#Hostname">¶</a></h2>
<pre>func Hostname() (name <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

Hostname returns the host name reported by the kernel.

<h2 id="IsExist">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L58">IsExist</a>
    <a href="#IsExist">¶</a></h2>
<pre>func IsExist(err <a href="/builtin/#error">error</a>) <a href="/builtin/#bool">bool</a></pre>

IsExist returns a boolean indicating whether the error is known to report that a
file or directory already exists. It is satisfied by ErrExist as well as some
syscall errors.

<h2 id="IsNotExist">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L65">IsNotExist</a>
    <a href="#IsNotExist">¶</a></h2>
<pre>func IsNotExist(err <a href="/builtin/#error">error</a>) <a href="/builtin/#bool">bool</a></pre>

IsNotExist returns a boolean indicating whether the error is known to report
that a file or directory does not exist. It is satisfied by ErrNotExist as well
as some syscall errors.

<a id="exampleIsNotExist"></a>
Example:

    filename := "a-nonexistent-file"
    if _, err := os.Stat(filename); os.IsNotExist(err) {
        fmt.Printf("file does not exist")
    }
    // Output:
    // file does not exist

<h2 id="IsPathSeparator">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/path_unix.go#L5">IsPathSeparator</a>
    <a href="#IsPathSeparator">¶</a></h2>
<pre>func IsPathSeparator(c <a href="/builtin/#uint8">uint8</a>) <a href="/builtin/#bool">bool</a></pre>

IsPathSeparator reports whether c is a directory separator character.

<h2 id="IsPermission">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L72">IsPermission</a>
    <a href="#IsPermission">¶</a></h2>
<pre>func IsPermission(err <a href="/builtin/#error">error</a>) <a href="/builtin/#bool">bool</a></pre>

IsPermission returns a boolean indicating whether the error is known to report
that permission is denied. It is satisfied by ErrPermission as well as some
syscall errors.

<h2 id="IsTimeout">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L78">IsTimeout</a>
    <a href="#IsTimeout">¶</a></h2>
<pre>func IsTimeout(err <a href="/builtin/#error">error</a>) <a href="/builtin/#bool">bool</a></pre>

IsTimeout returns a boolean indicating whether the error is known to report that
a timeout occurred.

<h2 id="Lchown">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_posix.go#L75">Lchown</a>
    <a href="#Lchown">¶</a></h2>
<pre>func Lchown(name <a href="/builtin/#string">string</a>, uid, gid <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

Lchown changes the numeric uid and gid of the named file. If the file is a
symbolic link, it changes the uid and gid of the link itself. If there is an
error, it will be of type *PathError.

On Windows, it always returns the syscall.EWINDOWS error, wrapped in *PathError.

<h2 id="Link">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_unix.go#L311">Link</a>
    <a href="#Link">¶</a></h2>
<pre>func Link(oldname, newname <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Link creates newname as a hard link to the oldname file. If there is an error,
it will be of type *LinkError.

<h2 id="LookupEnv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/env.go#L84">LookupEnv</a>
    <a href="#LookupEnv">¶</a></h2>
<pre>func LookupEnv(key <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#bool">bool</a>)</pre>

LookupEnv retrieves the value of the environment variable named by the key. If
the variable is present in the environment the value (which may be empty) is
returned and the boolean is true. Otherwise the returned value will be empty and
the boolean will be false.

<a id="exampleLookupEnv"></a>
Example:

    show := func(key string) {
        val, ok := os.LookupEnv(key)
        if !ok {
            fmt.Printf("%s not set\n", key)
        } else {
            fmt.Printf("%s=%s\n", key, val)
        }
    }

    show("USER")
    show("GOPATH")

    // Output:
    // USER=gopher
    // GOPATH not set

<h2 id="Mkdir">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L204">Mkdir</a>
    <a href="#Mkdir">¶</a></h2>
<pre>func Mkdir(name <a href="/builtin/#string">string</a>, perm <a href="#FileMode">FileMode</a>) <a href="/builtin/#error">error</a></pre>

Mkdir creates a new directory with the specified name and permission bits
(before umask). If there is an error, it will be of type *PathError.

<h2 id="MkdirAll">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/path.go#L10">MkdirAll</a>
    <a href="#MkdirAll">¶</a></h2>
<pre>func MkdirAll(path <a href="/builtin/#string">string</a>, perm <a href="#FileMode">FileMode</a>) <a href="/builtin/#error">error</a></pre>

MkdirAll creates a directory named path, along with any necessary parents, and
returns nil, or else returns an error. The permission bits perm (before umask)
are used for all directories that MkdirAll creates. If path is already a
directory, MkdirAll does nothing and returns nil.

<h2 id="NewSyscallError">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L48">NewSyscallError</a>
    <a href="#NewSyscallError">¶</a></h2>
<pre>func NewSyscallError(syscall <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>) <a href="/builtin/#error">error</a></pre>

NewSyscallError returns, as an error, a new SyscallError with the given system
call name and error details. As a convenience, if err is nil, NewSyscallError
returns nil.

<h2 id="Readlink">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_posix.go#L8">Readlink</a>
    <a href="#Readlink">¶</a></h2>
<pre>func Readlink(name <a href="/builtin/#string">string</a>) (<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

Readlink returns the destination of the named symbolic link. If there is an
error, it will be of type *PathError.

<h2 id="Remove">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_unix.go#L268">Remove</a>
    <a href="#Remove">¶</a></h2>
<pre>func Remove(name <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Remove removes the named file or directory. If there is an error, it will be of
type *PathError.

<h2 id="RemoveAll">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/path.go#L57">RemoveAll</a>
    <a href="#RemoveAll">¶</a></h2>
<pre>func RemoveAll(path <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

RemoveAll removes path and any children it contains. It removes everything it
can but returns the first error it encounters. If the path does not exist,
RemoveAll returns nil (no error).

<h2 id="Rename">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L269">Rename</a>
    <a href="#Rename">¶</a></h2>
<pre>func Rename(oldpath, newpath <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Rename renames (moves) oldpath to newpath. If newpath already exists and is not
a directory, Rename replaces it. OS-specific restrictions may apply when oldpath
and newpath are in different directories. If there is an error, it will be of
type *LinkError.

<h2 id="SameFile">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/types.go#L106">SameFile</a>
    <a href="#SameFile">¶</a></h2>
<pre>func SameFile(fi1, fi2 <a href="#FileInfo">FileInfo</a>) <a href="/builtin/#bool">bool</a></pre>

SameFile reports whether fi1 and fi2 describe the same file. For example, on
Unix this means that the device and inode fields of the two underlying
structures are identical; on other systems the decision may be based on the path
names. SameFile only applies to results returned by this package's Stat. It
returns false in other cases.

<h2 id="Setenv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/env.go#L91">Setenv</a>
    <a href="#Setenv">¶</a></h2>
<pre>func Setenv(key, value <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Setenv sets the value of the environment variable named by the key. It returns
an error, if any.

<h2 id="Symlink">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_unix.go#L321">Symlink</a>
    <a href="#Symlink">¶</a></h2>
<pre>func Symlink(oldname, newname <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Symlink creates newname as a symbolic link to oldname. If there is an error, it
will be of type *LinkError.

<h2 id="TempDir">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L304">TempDir</a>
    <a href="#TempDir">¶</a></h2>
<pre>func TempDir() <a href="/builtin/#string">string</a></pre>

TempDir returns the default directory to use for temporary files.

On Unix systems, it returns $TMPDIR if non-empty, else /tmp. On Windows, it uses
GetTempPath, returning the first non-empty value from %TMP%, %TEMP%,
%USERPROFILE%, or the Windows directory. On Plan 9, it returns /tmp.

The directory is neither guaranteed to exist nor have accessible permissions.

<h2 id="Truncate">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_unix.go#L259">Truncate</a>
    <a href="#Truncate">¶</a></h2>
<pre>func Truncate(name <a href="/builtin/#string">string</a>, size <a href="/builtin/#int64">int64</a>) <a href="/builtin/#error">error</a></pre>

Truncate changes the size of the named file. If the file is a symbolic link, it
changes the size of the link's target. If there is an error, it will be of type
*PathError.

<h2 id="Unsetenv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/env.go#L100">Unsetenv</a>
    <a href="#Unsetenv">¶</a></h2>
<pre>func Unsetenv(key <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Unsetenv unsets a single environment variable.

<a id="exampleUnsetenv"></a>
Example:

    os.Setenv("TMPDIR", "/my/tmp")
    defer os.Unsetenv("TMPDIR")

<h2 id="File">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/types.go#L6">File</a>
    <a href="#File">¶</a></h2>
<pre>type File struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

File represents an open file descriptor.

<h3 id="Create">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L248">Create</a>
    <a href="#Create">¶</a></h3>
<pre>func Create(name <a href="/builtin/#string">string</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

Create creates the named file with mode 0666 (before umask), truncating it if it
already exists. If successful, methods on the returned File can be used for I/O;
the associated file descriptor has mode O_RDWR. If there is an error, it will be
of type *PathError.

<h3 id="NewFile">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_unix.go#L68">NewFile</a>
    <a href="#NewFile">¶</a></h3>
<pre>func NewFile(fd <a href="/builtin/#uintptr">uintptr</a>, name <a href="/builtin/#string">string</a>) *<a href="#File">File</a></pre>

NewFile returns a new File with the given file descriptor and name. The returned
value will be nil if fd is not a valid file descriptor.

<h3 id="Open">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L239">Open</a>
    <a href="#Open">¶</a></h3>
<pre>func Open(name <a href="/builtin/#string">string</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

Open opens the named file for reading. If successful, methods on the returned
file can be used for reading; the associated file descriptor has mode O_RDONLY.
If there is an error, it will be of type *PathError.

<h3 id="OpenFile">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L257">OpenFile</a>
    <a href="#OpenFile">¶</a></h3>
<pre>func OpenFile(name <a href="/builtin/#string">string</a>, flag <a href="/builtin/#int">int</a>, perm <a href="#FileMode">FileMode</a>) (*<a href="#File">File</a>, <a href="/builtin/#error">error</a>)</pre>

OpenFile is the generalized open call; most users will use Open or Create
instead. It opens the named file with specified flag (O_RDONLY etc.) and perm
(before umask), if applicable. If successful, methods on the returned File can
be used for I/O. If there is an error, it will be of type *PathError.

<a id="exampleOpenFile"></a>
Example:

    f, err := os.OpenFile("notes.txt", os.O_RDWR|os.O_CREATE, 0755)
    if err != nil {
        log.Fatal(err)
    }
    if err := f.Close(); err != nil {
        log.Fatal(err)
    }


<a id="exampleOpenFile_append"></a>
Example:

    // If the file doesn't exist, create it, or append to the file
    f, err := os.OpenFile("access.log", os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0644)
    if err != nil {
        log.Fatal(err)
    }
    if _, err := f.Write([]byte("appended some data\n")); err != nil {
        log.Fatal(err)
    }
    if err := f.Close(); err != nil {
        log.Fatal(err)
    }

<h3 id="Pipe">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/pipe_linux.go#L1">Pipe</a>
    <a href="#Pipe">¶</a></h3>
<pre>func Pipe() (r *<a href="#File">File</a>, w *<a href="#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

Pipe returns a connected pair of Files; reads from r return bytes written to w.
It returns the files and an error, if any.

<h3 id="File.Chdir">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_posix.go#L142">Chdir</a>
    <a href="#File.Chdir">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Chdir() <a href="/builtin/#error">error</a></pre>

Chdir changes the current working directory to the file, which must be a
directory. If there is an error, it will be of type *PathError.

<h3 id="File.Chmod">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L330">Chmod</a>
    <a href="#File.Chmod">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Chmod(mode <a href="#FileMode">FileMode</a>) <a href="/builtin/#error">error</a></pre>

Chmod changes the mode of the file to mode. If there is an error, it will be of
type *PathError.

<h3 id="File.Chown">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_posix.go#L87">Chown</a>
    <a href="#File.Chown">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Chown(uid, gid <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

Chown changes the numeric uid and gid of the named file. If there is an error,
it will be of type *PathError.

On Windows, it always returns the syscall.EWINDOWS error, wrapped in *PathError.

<h3 id="File.Close">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_unix.go#L189">Close</a>
    <a href="#File.Close">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the File, rendering it unusable for I/O. It returns an error, if
any.

<h3 id="File.Fd">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_unix.go#L48">Fd</a>
    <a href="#File.Fd">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Fd() <a href="/builtin/#uintptr">uintptr</a></pre>

Fd returns the integer Unix file descriptor referencing the open file. The file
descriptor is valid only until f.Close is called or f is garbage collected. On
Unix systems this will cause the SetDeadline methods to stop working.

<h3 id="File.Name">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L39">Name</a>
    <a href="#File.Name">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Name() <a href="/builtin/#string">string</a></pre>

Name returns the name of the file as presented to Open.

<h3 id="File.Read">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L93">Read</a>
    <a href="#File.Read">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Read(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Read reads up to len(b) bytes from the File. It returns the number of bytes read
and any error encountered. At end of file, Read returns 0, io.EOF.

<h3 id="File.ReadAt">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L105">ReadAt</a>
    <a href="#File.ReadAt">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) ReadAt(b []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadAt reads len(b) bytes from the File starting at byte offset off. It returns
the number of bytes read and the error, if any. ReadAt always returns a non-nil
error when n < len(b). At end of file, that error is io.EOF.

<h3 id="File.Readdir">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/dir.go#L12">Readdir</a>
    <a href="#File.Readdir">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Readdir(n <a href="/builtin/#int">int</a>) ([]<a href="#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)</pre>

Readdir reads the contents of the directory associated with file and returns a
slice of up to n FileInfo values, as would be returned by Lstat, in directory
order. Subsequent calls on the same file will yield further FileInfos.

If n > 0, Readdir returns at most n FileInfo structures. In this case, if
Readdir returns an empty slice, it will return a non-nil error explaining why.
At the end of a directory, the error is io.EOF.

If n <= 0, Readdir returns all the FileInfo from the directory in a single
slice. In this case, if Readdir succeeds (reads all the way to the end of the
directory), it returns the slice and a nil error. If it encounters an error
before the end of the directory, Readdir returns the FileInfo read until that
point and a non-nil error.

<h3 id="File.Readdirnames">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/dir.go#L31">Readdirnames</a>
    <a href="#File.Readdirnames">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Readdirnames(n <a href="/builtin/#int">int</a>) (names []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

Readdirnames reads and returns a slice of names from the directory f.

If n > 0, Readdirnames returns at most n names. In this case, if Readdirnames
returns an empty slice, it will return a non-nil error explaining why. At the
end of a directory, the error is io.EOF.

If n <= 0, Readdirnames returns all the names from the directory in a single
slice. In this case, if Readdirnames succeeds (reads all the way to the end of
the directory), it returns the slice and a nil error. If it encounters an error
before the end of the directory, Readdirnames returns the names read until that
point and a non-nil error.

<h3 id="File.Seek">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L181">Seek</a>
    <a href="#File.Seek">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Seek(offset <a href="/builtin/#int64">int64</a>, whence <a href="/builtin/#int">int</a>) (ret <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>

Seek sets the offset for the next Read or Write on file to offset, interpreted
according to whence: 0 means relative to the origin of the file, 1 means
relative to the current offset, and 2 means relative to the end. It returns the
new offset and an error, if any. The behavior of Seek on a file opened with
O_APPEND is not specified.

<h3 id="File.SetDeadline">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L354">SetDeadline</a>
    <a href="#File.SetDeadline">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline sets the read and write deadlines for a File. It is equivalent to
calling both SetReadDeadline and SetWriteDeadline.

Only some kinds of files support setting a deadline. Calls to SetDeadline for
files that do not support deadlines will return ErrNoDeadline. On most systems
ordinary files do not support deadlines, but pipes do.

A deadline is an absolute time after which I/O operations fail with an error
instead of blocking. The deadline applies to all future and pending I/O, not
just the immediately following call to Read or Write. After a deadline has been
exceeded, the connection can be refreshed by setting a deadline in the future.

An error returned after a timeout fails will implement the Timeout method, and
calling the Timeout method will return true. The PathError and SyscallError
types implement the Timeout method. In general, call IsTimeout to test whether
an error indicates a timeout.

An idle timeout can be implemented by repeatedly extending the deadline after
successful Read or Write calls.

A zero value for t means I/O operations will not time out.

<h3 id="File.SetReadDeadline">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L362">SetReadDeadline</a>
    <a href="#File.SetReadDeadline">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetReadDeadline sets the deadline for future Read calls and any
currently-blocked Read call. A zero value for t means Read will not time out.
Not all files support setting deadlines; see SetDeadline.

<h3 id="File.SetWriteDeadline">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L372">SetWriteDeadline</a>
    <a href="#File.SetWriteDeadline">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetWriteDeadline sets the deadline for any future Write calls and any
currently-blocked Write call. Even if Write times out, it may return n > 0,
indicating that some of the data was successfully written. A zero value for t
means Write will not time out. Not all files support setting deadlines; see
SetDeadline.

<h3 id="File.Stat">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/stat_unix.go#L5">Stat</a>
    <a href="#File.Stat">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Stat() (<a href="#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)</pre>

Stat returns the FileInfo structure describing file. If there is an error, it
will be of type *PathError.

<h3 id="File.Sync">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_posix.go#L113">Sync</a>
    <a href="#File.Sync">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Sync() <a href="/builtin/#error">error</a></pre>

Sync commits the current contents of the file to stable storage. Typically, this
means flushing the file system's in-memory copy of recently written data to
disk.

<h3 id="File.Truncate">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file_posix.go#L100">Truncate</a>
    <a href="#File.Truncate">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Truncate(size <a href="/builtin/#int64">int64</a>) <a href="/builtin/#error">error</a></pre>

Truncate changes the size of the file. It does not change the I/O offset. If
there is an error, it will be of type *PathError.

<h3 id="File.Write">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L130">Write</a>
    <a href="#File.Write">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) Write(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write writes len(b) bytes to the File. It returns the number of bytes written
and an error, if any. Write returns a non-nil error when n != len(b).

<h3 id="File.WriteAt">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L154">WriteAt</a>
    <a href="#File.WriteAt">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) WriteAt(b []<a href="/builtin/#byte">byte</a>, off <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteAt writes len(b) bytes to the File starting at byte offset off. It returns
the number of bytes written and an error, if any. WriteAt returns a non-nil
error when n != len(b).

<h3 id="File.WriteString">func (*File) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L197">WriteString</a>
    <a href="#File.WriteString">¶</a></h3>
<pre>func (f *<a href="#File">File</a>) WriteString(s <a href="/builtin/#string">string</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteString is like Write, but writes the contents of string s rather than a
slice of bytes.

<h2 id="FileInfo">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/types.go#L11">FileInfo</a>
    <a href="#FileInfo">¶</a></h2>
<pre>type FileInfo interface {
    Name() <a href="/builtin/#string">string</a>       <span class="comment">// base name of the file</span>
    Size() <a href="/builtin/#int64">int64</a>        <span class="comment">// length in bytes for regular files; system-dependent for others</span>
    Mode() <a href="#FileMode">FileMode</a>     <span class="comment">// file mode bits</span>
    ModTime() <a href="/time/">time</a>.<a href="/time/#Time">Time</a> <span class="comment">// modification time</span>
    IsDir() <a href="/builtin/#bool">bool</a>        <span class="comment">// abbreviation for Mode().IsDir()</span>
    Sys() interface{}   <span class="comment">// underlying data source (can return nil)</span>
}</pre>

A FileInfo describes a file and is returned by Stat and Lstat.

<h3 id="Lstat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/stat.go#L10">Lstat</a>
    <a href="#Lstat">¶</a></h3>
<pre>func Lstat(name <a href="/builtin/#string">string</a>) (<a href="#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)</pre>

Lstat returns a FileInfo describing the named file. If the file is a symbolic
link, the returned FileInfo describes the symbolic link. Lstat makes no attempt
to follow the link. If there is an error, it will be of type *PathError.

<h3 id="Stat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/stat.go#L1">Stat</a>
    <a href="#Stat">¶</a></h3>
<pre>func Stat(name <a href="/builtin/#string">string</a>) (<a href="#FileInfo">FileInfo</a>, <a href="/builtin/#error">error</a>)</pre>

Stat returns a FileInfo describing the named file. If there is an error, it will
be of type *PathError.

<h2 id="FileMode">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/types.go#L25">FileMode</a>
    <a href="#FileMode">¶</a></h2>
<pre>type FileMode <a href="/builtin/#uint32">uint32</a></pre>

A FileMode represents a file's mode and permission bits. The bits have the same
definition on all systems, so that information about files can be moved from one
system to another portably. Not all bits apply to all systems. The only required
bit is ModeDir for directories.

<pre>const (
    <span class="comment">// The single letters are the abbreviations</span>
    <span class="comment">// used by the String method&#39;s formatting.</span>
    <span id="ModeDir">ModeDir</span>        <a href="#FileMode">FileMode</a> = 1 &lt;&lt; (32 - 1 - <a href="/builtin/#iota">iota</a>) <span class="comment">// d: is a directory</span>
    <span id="ModeAppend">ModeAppend</span>                                     <span class="comment">// a: append-only</span>
    <span id="ModeExclusive">ModeExclusive</span>                                  <span class="comment">// l: exclusive use</span>
    <span id="ModeTemporary">ModeTemporary</span>                                  <span class="comment">// T: temporary file; Plan 9 only</span>
    <span id="ModeSymlink">ModeSymlink</span>                                    <span class="comment">// L: symbolic link</span>
    <span id="ModeDevice">ModeDevice</span>                                     <span class="comment">// D: device file</span>
    <span id="ModeNamedPipe">ModeNamedPipe</span>                                  <span class="comment">// p: named pipe (FIFO)</span>
    <span id="ModeSocket">ModeSocket</span>                                     <span class="comment">// S: Unix domain socket</span>
    <span id="ModeSetuid">ModeSetuid</span>                                     <span class="comment">// u: setuid</span>
    <span id="ModeSetgid">ModeSetgid</span>                                     <span class="comment">// g: setgid</span>
    <span id="ModeCharDevice">ModeCharDevice</span>                                 <span class="comment">// c: Unix character device, when ModeDevice is set</span>
    <span id="ModeSticky">ModeSticky</span>                                     <span class="comment">// t: sticky</span>

    <span class="comment">// Mask for the type bits. For regular files, none will be set.</span>
    <span id="ModeType">ModeType</span> = <a href="#ModeDir">ModeDir</a> | <a href="#ModeSymlink">ModeSymlink</a> | <a href="#ModeNamedPipe">ModeNamedPipe</a> | <a href="#ModeSocket">ModeSocket</a> | <a href="#ModeDevice">ModeDevice</a>

    <span id="ModePerm">ModePerm</span> <a href="#FileMode">FileMode</a> = 0777 <span class="comment">// Unix permission bits</span>
)</pre>

The defined file mode bits are the most significant bits of the FileMode. The
nine least-significant bits are the standard Unix rwxrwxrwx permissions. The
values of these bits should be considered part of the public API and may be used
in wire protocols or disk representations: they must not be changed, although
new bits might be added.

<a id="exampleFileMode"></a>
Example:

    fi, err := os.Lstat("some-filename")
    if err != nil {
        log.Fatal(err)
    }

    switch mode := fi.Mode(); {
    case mode.IsRegular():
        fmt.Println("regular file")
    case mode.IsDir():
        fmt.Println("directory")
    case mode&os.ModeSymlink != 0:
        fmt.Println("symbolic link")
    case mode&os.ModeNamedPipe != 0:
        fmt.Println("named pipe")
    }

<h3 id="FileMode.IsDir">func (FileMode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/types.go#L82">IsDir</a>
    <a href="#FileMode.IsDir">¶</a></h3>
<pre>func (m <a href="#FileMode">FileMode</a>) IsDir() <a href="/builtin/#bool">bool</a></pre>

IsDir reports whether m describes a directory. That is, it tests for the ModeDir
bit being set in m.

<h3 id="FileMode.IsRegular">func (FileMode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/types.go#L88">IsRegular</a>
    <a href="#FileMode.IsRegular">¶</a></h3>
<pre>func (m <a href="#FileMode">FileMode</a>) IsRegular() <a href="/builtin/#bool">bool</a></pre>

IsRegular reports whether m describes a regular file. That is, it tests that no
mode type bits are set.

<h3 id="FileMode.Perm">func (FileMode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/types.go#L93">Perm</a>
    <a href="#FileMode.Perm">¶</a></h3>
<pre>func (m <a href="#FileMode">FileMode</a>) Perm() <a href="#FileMode">FileMode</a></pre>

Perm returns the Unix permission bits in m.

<h3 id="FileMode.String">func (FileMode) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/types.go#L54">String</a>
    <a href="#FileMode.String">¶</a></h3>
<pre>func (m <a href="#FileMode">FileMode</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="LinkError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L79">LinkError</a>
    <a href="#LinkError">¶</a></h2>
<pre>type LinkError struct {
<span id="LinkError.Op"></span>    Op  <a href="/builtin/#string">string</a>
<span id="LinkError.Old"></span>    Old <a href="/builtin/#string">string</a>
<span id="LinkError.New"></span>    New <a href="/builtin/#string">string</a>
<span id="LinkError.Err"></span>    Err <a href="/builtin/#error">error</a>
}</pre>

LinkError records an error during a link or symlink or rename system call and
the paths that caused it.

<h3 id="LinkError.Error">func (*LinkError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/file.go#L86">Error</a>
    <a href="#LinkError.Error">¶</a></h3>
<pre>func (e *<a href="#LinkError">LinkError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="PathError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L17">PathError</a>
    <a href="#PathError">¶</a></h2>
<pre>type PathError struct {
<span id="PathError.Op"></span>    Op   <a href="/builtin/#string">string</a>
<span id="PathError.Path"></span>    Path <a href="/builtin/#string">string</a>
<span id="PathError.Err"></span>    Err  <a href="/builtin/#error">error</a>
}</pre>

PathError records an error and the operation and file path that caused it.

<h3 id="PathError.Error">func (*PathError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L23">Error</a>
    <a href="#PathError.Error">¶</a></h3>
<pre>func (e *<a href="#PathError">PathError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="PathError.Timeout">func (*PathError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L26">Timeout</a>
    <a href="#PathError.Timeout">¶</a></h3>
<pre>func (e *<a href="#PathError">PathError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>

Timeout reports whether this error represents a timeout.

<h2 id="ProcAttr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L30">ProcAttr</a>
    <a href="#ProcAttr">¶</a></h2>
<pre>type ProcAttr struct {
    <span class="comment">// If Dir is non-empty, the child changes into the directory before</span>
    <span class="comment">// creating the process.</span>
<span id="ProcAttr.Dir"></span>    Dir <a href="/builtin/#string">string</a>
    <span class="comment">// If Env is non-nil, it gives the environment variables for the</span>
    <span class="comment">// new process in the form returned by Environ.</span>
    <span class="comment">// If it is nil, the result of Environ will be used.</span>
<span id="ProcAttr.Env"></span>    Env []<a href="/builtin/#string">string</a>
<span id="ProcAttr.Files"></span>    <span class="comment">// Files specifies the open files inherited by the new process. The</span>
    <span class="comment">// first three entries correspond to standard input, standard output, and</span>
    <span class="comment">// standard error. An implementation may support additional entries,</span>
    <span class="comment">// depending on the underlying operating system. A nil entry corresponds</span>
    <span class="comment">// to that file being closed when the process starts.</span>
    Files []*<a href="#File">File</a>

    <span class="comment">// Operating system-specific process creation attributes.</span>
    <span class="comment">// Note that setting this field means that your program</span>
    <span class="comment">// may not execute properly or even compile on some</span>
    <span class="comment">// operating systems.</span>
<span id="ProcAttr.Sys"></span>    Sys *<a href="/syscall/">syscall</a>.<a href="/syscall/#SysProcAttr">SysProcAttr</a>
}</pre>

ProcAttr holds the attributes that will be applied to a new process started by
StartProcess.

<h2 id="Process">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L7">Process</a>
    <a href="#Process">¶</a></h2>
<pre>type Process struct {
<span id="Process.Pid"></span>    Pid <a href="/builtin/#int">int</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Process stores the information about a process created by StartProcess.

<h3 id="FindProcess">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L73">FindProcess</a>
    <a href="#FindProcess">¶</a></h3>
<pre>func FindProcess(pid <a href="/builtin/#int">int</a>) (*<a href="#Process">Process</a>, <a href="/builtin/#error">error</a>)</pre>

FindProcess looks for a running process by its pid.

The Process it returns can be used to obtain information about the underlying
operating system process.

On Unix systems, FindProcess always succeeds and returns a Process for the given
pid, regardless of whether the process exists.

<h3 id="StartProcess">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L90">StartProcess</a>
    <a href="#StartProcess">¶</a></h3>
<pre>func StartProcess(name <a href="/builtin/#string">string</a>, argv []<a href="/builtin/#string">string</a>, attr *<a href="#ProcAttr">ProcAttr</a>) (*<a href="#Process">Process</a>, <a href="/builtin/#error">error</a>)</pre>

StartProcess starts a new process with the program, arguments and attributes
specified by name, argv and attr. The argv slice will become os.Args in the new
process, so it normally starts with the program name.

If the calling goroutine has locked the operating system thread with
runtime.LockOSThread and modified any inheritable OS-level thread state (for
example, Linux or Plan 9 name spaces), the new process will inherit the caller's
thread state.

StartProcess is a low-level interface. The os/exec package provides higher-level
interfaces.

If there is an error, it will be of type *PathError.

<h3 id="Process.Kill">func (*Process) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L103">Kill</a>
    <a href="#Process.Kill">¶</a></h3>
<pre>func (p *<a href="#Process">Process</a>) Kill() <a href="/builtin/#error">error</a></pre>

Kill causes the Process to exit immediately.

<h3 id="Process.Release">func (*Process) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L98">Release</a>
    <a href="#Process.Release">¶</a></h3>
<pre>func (p *<a href="#Process">Process</a>) Release() <a href="/builtin/#error">error</a></pre>

Release releases any resources associated with the Process p, rendering it
unusable in the future. Release only needs to be called if Wait is not.

<h3 id="Process.Signal">func (*Process) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L118">Signal</a>
    <a href="#Process.Signal">¶</a></h3>
<pre>func (p *<a href="#Process">Process</a>) Signal(sig <a href="#Signal">Signal</a>) <a href="/builtin/#error">error</a></pre>

Signal sends a signal to the Process. Sending Interrupt on Windows is not
implemented.

<h3 id="Process.Wait">func (*Process) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L112">Wait</a>
    <a href="#Process.Wait">¶</a></h3>
<pre>func (p *<a href="#Process">Process</a>) Wait() (*<a href="#ProcessState">ProcessState</a>, <a href="/builtin/#error">error</a>)</pre>

Wait waits for the Process to exit, and then returns a ProcessState describing
its status and an error, if any. Wait releases any resources associated with the
Process. On most operating systems, the Process must be a child of the current
process or an error will be returned.

<h2 id="ProcessState">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec_posix.go#L48">ProcessState</a>
    <a href="#ProcessState">¶</a></h2>
<pre>type ProcessState struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ProcessState stores information about a process, as reported by Wait.

<h3 id="ProcessState.Exited">func (*ProcessState) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L133">Exited</a>
    <a href="#ProcessState.Exited">¶</a></h3>
<pre>func (p *<a href="#ProcessState">ProcessState</a>) Exited() <a href="/builtin/#bool">bool</a></pre>

Exited reports whether the program has exited.

<h3 id="ProcessState.Pid">func (*ProcessState) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec_posix.go#L55">Pid</a>
    <a href="#ProcessState.Pid">¶</a></h3>
<pre>func (p *<a href="#ProcessState">ProcessState</a>) Pid() <a href="/builtin/#int">int</a></pre>

Pid returns the process id of the exited process.

<h3 id="ProcessState.String">func (*ProcessState) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec_posix.go#L75">String</a>
    <a href="#ProcessState.String">¶</a></h3>
<pre>func (p *<a href="#ProcessState">ProcessState</a>) String() <a href="/builtin/#string">string</a></pre>


<h3 id="ProcessState.Success">func (*ProcessState) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L139">Success</a>
    <a href="#ProcessState.Success">¶</a></h3>
<pre>func (p *<a href="#ProcessState">ProcessState</a>) Success() <a href="/builtin/#bool">bool</a></pre>

Success reports whether the program exited successfully, such as with exit
status 0 on Unix.

<h3 id="ProcessState.Sys">func (*ProcessState) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L146">Sys</a>
    <a href="#ProcessState.Sys">¶</a></h3>
<pre>func (p *<a href="#ProcessState">ProcessState</a>) Sys() interface{}</pre>

Sys returns system-dependent exit information about the process. Convert it to
the appropriate underlying type, such as syscall.WaitStatus on Unix, to access
its contents.

<h3 id="ProcessState.SysUsage">func (*ProcessState) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L155">SysUsage</a>
    <a href="#ProcessState.SysUsage">¶</a></h3>
<pre>func (p *<a href="#ProcessState">ProcessState</a>) SysUsage() interface{}</pre>

SysUsage returns system-dependent resource usage information about the exited
process. Convert it to the appropriate underlying type, such as *syscall.Rusage
on Unix, to access its contents. (On Unix, *syscall.Rusage matches struct rusage
as defined in the getrusage(2) manual page.)

<h3 id="ProcessState.SystemTime">func (*ProcessState) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L128">SystemTime</a>
    <a href="#ProcessState.SystemTime">¶</a></h3>
<pre>func (p *<a href="#ProcessState">ProcessState</a>) SystemTime() <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a></pre>

SystemTime returns the system CPU time of the exited process and its children.

<h3 id="ProcessState.UserTime">func (*ProcessState) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L123">UserTime</a>
    <a href="#ProcessState.UserTime">¶</a></h3>
<pre>func (p *<a href="#ProcessState">ProcessState</a>) UserTime() <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a></pre>

UserTime returns the user CPU time of the exited process and its children.

<h2 id="Signal">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/exec.go#L55">Signal</a>
    <a href="#Signal">¶</a></h2>
<pre>type Signal interface {
    String() <a href="/builtin/#string">string</a>
    Signal() <span class="comment">// to distinguish from other Stringers</span>
}</pre>

A Signal represents an operating system signal. The usual underlying
implementation is operating system-dependent: on Unix it is syscall.Signal.

<pre>var (
    <span id="Interrupt">Interrupt</span> <a href="#Signal">Signal</a> = <a href="/syscall/">syscall</a>.<a href="/syscall/#SIGINT">SIGINT</a>
    <span id="Kill">Kill</span>      <a href="#Signal">Signal</a> = <a href="/syscall/">syscall</a>.<a href="/syscall/#SIGKILL">SIGKILL</a>
)</pre>

The only signal values guaranteed to be present in the os package on all systems
are Interrupt (send the process an interrupt) and Kill (force the process to
exit). Interrupt is not implemented on Windows; using it with os.Process.Signal
will return an error.

<h2 id="SyscallError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L32">SyscallError</a>
    <a href="#SyscallError">¶</a></h2>
<pre>type SyscallError struct {
<span id="SyscallError.Syscall"></span>    Syscall <a href="/builtin/#string">string</a>
<span id="SyscallError.Err"></span>    Err     <a href="/builtin/#error">error</a>
}</pre>

SyscallError records an error from a specific system call.

<h3 id="SyscallError.Error">func (*SyscallError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L37">Error</a>
    <a href="#SyscallError.Error">¶</a></h3>
<pre>func (e *<a href="#SyscallError">SyscallError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="SyscallError.Timeout">func (*SyscallError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/error.go#L40">Timeout</a>
    <a href="#SyscallError.Timeout">¶</a></h3>
<pre>func (e *<a href="#SyscallError">SyscallError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>

Timeout reports whether this error represents a timeout.

## Subdirectories
- [..](..)
- [exec](exec/)
- [signal](signal/)
- [user](user/)
