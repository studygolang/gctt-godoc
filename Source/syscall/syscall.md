version: 1.10
## package syscall

  `import "syscall"`

## Overview

Package syscall contains an interface to the low-level operating system
primitives. The details vary depending on the underlying system, and by default,
godoc will display the syscall documentation for the current system. If you want
godoc to display syscall documentation for another system, set $GOOS and $GOARCH
to the desired system. For example, if you want to view documentation for
freebsd/arm on linux/amd64, set $GOOS to freebsd and $GOARCH to arm. The primary
use of syscall is inside other packages that provide a more portable interface
to the system, such as "os", "time" and "net". Use those packages rather than
this one if you can. For details of the functions and data types in this package
consult the manuals for the appropriate operating system. These calls return err
== nil to indicate success; otherwise err is an operating system error
describing the failure. On most systems, that error has type syscall.Errno.

NOTE: This package is locked down. Code outside the standard Go repository
should be migrated to use the corresponding package in the golang.org/x/sys
repository. That is also where updates required by new systems or versions
should be applied. Signal, Errno and SysProcAttr are not yet available in
golang.org/x/sys and must still be referenced from the syscall package. See
https://golang.org/s/go1.4-syscall for more information.

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func Accept(fd int) (nfd int, sa Sockaddr, err error)](#Accept)
- [func Accept4(fd int, flags int) (nfd int, sa Sockaddr, err error)](#Accept4)
- [func Access(path string, mode uint32) (err error)](#Access)
- [func Acct(path string) (err error)](#Acct)
- [func Adjtimex(buf *Timex) (state int, err error)](#Adjtimex)
- [func AttachLsf(fd int, i []SockFilter) error](#AttachLsf)
- [func Bind(fd int, sa Sockaddr) (err error)](#Bind)
- [func BindToDevice(fd int, device string) (err error)](#BindToDevice)
- [func BytePtrFromString(s string) (*byte, error)](#BytePtrFromString)
- [func ByteSliceFromString(s string) ([]byte, error)](#ByteSliceFromString)
- [func Chdir(path string) (err error)](#Chdir)
- [func Chmod(path string, mode uint32) (err error)](#Chmod)
- [func Chown(path string, uid int, gid int) (err error)](#Chown)
- [func Chroot(path string) (err error)](#Chroot)
- [func Clearenv()](#Clearenv)
- [func Close(fd int) (err error)](#Close)
- [func CloseOnExec(fd int)](#CloseOnExec)
- [func CmsgLen(datalen int) int](#CmsgLen)
- [func CmsgSpace(datalen int) int](#CmsgSpace)
- [func Connect(fd int, sa Sockaddr) (err error)](#Connect)
- [func Creat(path string, mode uint32) (fd int, err error)](#Creat)
- [func DetachLsf(fd int) error](#DetachLsf)
- [func Dup(oldfd int) (fd int, err error)](#Dup)
- [func Dup2(oldfd int, newfd int) (err error)](#Dup2)
- [func Dup3(oldfd int, newfd int, flags int) (err error)](#Dup3)
- [func Environ() []string](#Environ)
- [func EpollCreate(size int) (fd int, err error)](#EpollCreate)
- [func EpollCreate1(flag int) (fd int, err error)](#EpollCreate1)
- [func EpollCtl(epfd int, op int, fd int, event *EpollEvent) (err error)](#EpollCtl)
- [func EpollWait(epfd int, events []EpollEvent, msec int) (n int, err error)](#EpollWait)
- [func Exec(argv0 string, argv []string, envv []string) (err error)](#Exec)
- [func Exit(code int)](#Exit)
- [func Faccessat(dirfd int, path string, mode uint32, flags int) (err error)](#Faccessat)
- [func Fallocate(fd int, mode uint32, off int64, len int64) (err error)](#Fallocate)
- [func Fchdir(fd int) (err error)](#Fchdir)
- [func Fchmod(fd int, mode uint32) (err error)](#Fchmod)
- [func Fchmodat(dirfd int, path string, mode uint32, flags int) (err error)](#Fchmodat)
- [func Fchown(fd int, uid int, gid int) (err error)](#Fchown)
- [func Fchownat(dirfd int, path string, uid int, gid int, flags int) (err error)](#Fchownat)
- [func FcntlFlock(fd uintptr, cmd int, lk *Flock_t) error](#FcntlFlock)
- [func Fdatasync(fd int) (err error)](#Fdatasync)
- [func Flock(fd int, how int) (err error)](#Flock)
- [func ForkExec(argv0 string, argv []string, attr *ProcAttr) (pid int, err error)](#ForkExec)
- [func Fstat(fd int, stat *Stat_t) (err error)](#Fstat)
- [func Fstatfs(fd int, buf *Statfs_t) (err error)](#Fstatfs)
- [func Fsync(fd int) (err error)](#Fsync)
- [func Ftruncate(fd int, length int64) (err error)](#Ftruncate)
- [func Futimes(fd int, tv []Timeval) (err error)](#Futimes)
- [func Futimesat(dirfd int, path string, tv []Timeval) (err error)](#Futimesat)
- [func Getcwd(buf []byte) (n int, err error)](#Getcwd)
- [func Getdents(fd int, buf []byte) (n int, err error)](#Getdents)
- [func Getegid() (egid int)](#Getegid)
- [func Getenv(key string) (value string, found bool)](#Getenv)
- [func Geteuid() (euid int)](#Geteuid)
- [func Getgid() (gid int)](#Getgid)
- [func Getgroups() (gids []int, err error)](#Getgroups)
- [func Getpagesize() int](#Getpagesize)
- [func Getpgid(pid int) (pgid int, err error)](#Getpgid)
- [func Getpgrp() (pid int)](#Getpgrp)
- [func Getpid() (pid int)](#Getpid)
- [func Getppid() (ppid int)](#Getppid)
- [func Getpriority(which int, who int) (prio int, err error)](#Getpriority)
- [func Getrlimit(resource int, rlim *Rlimit) (err error)](#Getrlimit)
- [func Getrusage(who int, rusage *Rusage) (err error)](#Getrusage)
- [func GetsockoptInet4Addr(fd, level, opt int) (value [4]byte, err error)](#GetsockoptInet4Addr)
- [func GetsockoptInt(fd, level, opt int) (value int, err error)](#GetsockoptInt)
- [func Gettid() (tid int)](#Gettid)
- [func Gettimeofday(tv *Timeval) (err error)](#Gettimeofday)
- [func Getuid() (uid int)](#Getuid)
- [func Getwd() (wd string, err error)](#Getwd)
- [func Getxattr(path string, attr string, dest []byte) (sz int, err error)](#Getxattr)
- [func InotifyAddWatch(fd int, pathname string, mask uint32) (watchdesc int, err error)](#InotifyAddWatch)
- [func InotifyInit() (fd int, err error)](#InotifyInit)
- [func InotifyInit1(flags int) (fd int, err error)](#InotifyInit1)
- [func InotifyRmWatch(fd int, watchdesc uint32) (success int, err error)](#InotifyRmWatch)
- [func Ioperm(from int, num int, on int) (err error)](#Ioperm)
- [func Iopl(level int) (err error)](#Iopl)
- [func Kill(pid int, sig Signal) (err error)](#Kill)
- [func Klogctl(typ int, buf []byte) (n int, err error)](#Klogctl)
- [func Lchown(path string, uid int, gid int) (err error)](#Lchown)
- [func Link(oldpath string, newpath string) (err error)](#Link)
- [func Listen(s int, n int) (err error)](#Listen)
- [func Listxattr(path string, dest []byte) (sz int, err error)](#Listxattr)
- [func LsfSocket(ifindex, proto int) (int, error)](#LsfSocket)
- [func Lstat(path string, stat *Stat_t) (err error)](#Lstat)
- [func Madvise(b []byte, advice int) (err error)](#Madvise)
- [func Mkdir(path string, mode uint32) (err error)](#Mkdir)
- [func Mkdirat(dirfd int, path string, mode uint32) (err error)](#Mkdirat)
- [func Mkfifo(path string, mode uint32) (err error)](#Mkfifo)
- [func Mknod(path string, mode uint32, dev int) (err error)](#Mknod)
- [func Mknodat(dirfd int, path string, mode uint32, dev int) (err error)](#Mknodat)
- [func Mlock(b []byte) (err error)](#Mlock)
- [func Mlockall(flags int) (err error)](#Mlockall)
- [func Mmap(fd int, offset int64, length int, prot int, flags int) (data []byte, err error)](#Mmap)
- [func Mount(source string, target string, fstype string, flags uintptr, data string) (err error)](#Mount)
- [func Mprotect(b []byte, prot int) (err error)](#Mprotect)
- [func Munlock(b []byte) (err error)](#Munlock)
- [func Munlockall() (err error)](#Munlockall)
- [func Munmap(b []byte) (err error)](#Munmap)
- [func Nanosleep(time *Timespec, leftover *Timespec) (err error)](#Nanosleep)
- [func NetlinkRIB(proto, family int) ([]byte, error)](#NetlinkRIB)
- [func Open(path string, mode int, perm uint32) (fd int, err error)](#Open)
- [func Openat(dirfd int, path string, flags int, mode uint32) (fd int, err error)](#Openat)
- [func ParseDirent(buf []byte, max int, names []string) (consumed int, count int, newnames []string)](#ParseDirent)
- [func ParseNetlinkMessage(b []byte) ([]NetlinkMessage, error)](#ParseNetlinkMessage)
- [func ParseNetlinkRouteAttr(m *NetlinkMessage) ([]NetlinkRouteAttr, error)](#ParseNetlinkRouteAttr)
- [func ParseSocketControlMessage(b []byte) ([]SocketControlMessage, error)](#ParseSocketControlMessage)
- [func ParseUnixRights(m *SocketControlMessage) ([]int, error)](#ParseUnixRights)
- [func Pause() (err error)](#Pause)
- [func Pipe(p []int) (err error)](#Pipe)
- [func Pipe2(p []int, flags int) (err error)](#Pipe2)
- [func PivotRoot(newroot string, putold string) (err error)](#PivotRoot)
- [func Pread(fd int, p []byte, offset int64) (n int, err error)](#Pread)
- [func PtraceAttach(pid int) (err error)](#PtraceAttach)
- [func PtraceCont(pid int, signal int) (err error)](#PtraceCont)
- [func PtraceDetach(pid int) (err error)](#PtraceDetach)
- [func PtraceGetEventMsg(pid int) (msg uint, err error)](#PtraceGetEventMsg)
- [func PtraceGetRegs(pid int, regsout *PtraceRegs) (err error)](#PtraceGetRegs)
- [func PtracePeekData(pid int, addr uintptr, out []byte) (count int, err error)](#PtracePeekData)
- [func PtracePeekText(pid int, addr uintptr, out []byte) (count int, err error)](#PtracePeekText)
- [func PtracePokeData(pid int, addr uintptr, data []byte) (count int, err error)](#PtracePokeData)
- [func PtracePokeText(pid int, addr uintptr, data []byte) (count int, err error)](#PtracePokeText)
- [func PtraceSetOptions(pid int, options int) (err error)](#PtraceSetOptions)
- [func PtraceSetRegs(pid int, regs *PtraceRegs) (err error)](#PtraceSetRegs)
- [func PtraceSingleStep(pid int) (err error)](#PtraceSingleStep)
- [func PtraceSyscall(pid int, signal int) (err error)](#PtraceSyscall)
- [func Pwrite(fd int, p []byte, offset int64) (n int, err error)](#Pwrite)
- [func RawSyscall(trap, a1, a2, a3 uintptr) (r1, r2 uintptr, err Errno)](#RawSyscall)
- [func RawSyscall6(trap, a1, a2, a3, a4, a5, a6 uintptr) (r1, r2 uintptr, err Errno)](#RawSyscall6)
- [func Read(fd int, p []byte) (n int, err error)](#Read)
- [func ReadDirent(fd int, buf []byte) (n int, err error)](#ReadDirent)
- [func Readlink(path string, buf []byte) (n int, err error)](#Readlink)
- [func Reboot(cmd int) (err error)](#Reboot)
- [func Recvfrom(fd int, p []byte, flags int) (n int, from Sockaddr, err error)](#Recvfrom)
- [func Recvmsg(fd int, p, oob []byte, flags int) (n, oobn int, recvflags int, from Sockaddr, err error)](#Recvmsg)
- [func Removexattr(path string, attr string) (err error)](#Removexattr)
- [func Rename(oldpath string, newpath string) (err error)](#Rename)
- [func Renameat(olddirfd int, oldpath string, newdirfd int, newpath string) (err error)](#Renameat)
- [func Rmdir(path string) error](#Rmdir)
- [func Seek(fd int, offset int64, whence int) (off int64, err error)](#Seek)
- [func Select(nfd int, r *FdSet, w *FdSet, e *FdSet, timeout *Timeval) (n int, err error)](#Select)
- [func Sendfile(outfd int, infd int, offset *int64, count int) (written int, err error)](#Sendfile)
- [func Sendmsg(fd int, p, oob []byte, to Sockaddr, flags int) (err error)](#Sendmsg)
- [func SendmsgN(fd int, p, oob []byte, to Sockaddr, flags int) (n int, err error)](#SendmsgN)
- [func Sendto(fd int, p []byte, flags int, to Sockaddr) (err error)](#Sendto)
- [func SetLsfPromisc(name string, m bool) error](#SetLsfPromisc)
- [func SetNonblock(fd int, nonblocking bool) (err error)](#SetNonblock)
- [func Setdomainname(p []byte) (err error)](#Setdomainname)
- [func Setenv(key, value string) error](#Setenv)
- [func Setfsgid(gid int) (err error)](#Setfsgid)
- [func Setfsuid(uid int) (err error)](#Setfsuid)
- [func Setgid(gid int) (err error)](#Setgid)
- [func Setgroups(gids []int) (err error)](#Setgroups)
- [func Sethostname(p []byte) (err error)](#Sethostname)
- [func Setpgid(pid int, pgid int) (err error)](#Setpgid)
- [func Setpriority(which int, who int, prio int) (err error)](#Setpriority)
- [func Setregid(rgid int, egid int) (err error)](#Setregid)
- [func Setresgid(rgid int, egid int, sgid int) (err error)](#Setresgid)
- [func Setresuid(ruid int, euid int, suid int) (err error)](#Setresuid)
- [func Setreuid(ruid int, euid int) (err error)](#Setreuid)
- [func Setrlimit(resource int, rlim *Rlimit) (err error)](#Setrlimit)
- [func Setsid() (pid int, err error)](#Setsid)
- [func SetsockoptByte(fd, level, opt int, value byte) (err error)](#SetsockoptByte)
- [func SetsockoptICMPv6Filter(fd, level, opt int, filter *ICMPv6Filter) error](#SetsockoptICMPv6Filter)
- [func SetsockoptIPMreq(fd, level, opt int, mreq *IPMreq) (err error)](#SetsockoptIPMreq)
- [func SetsockoptIPMreqn(fd, level, opt int, mreq *IPMreqn) (err error)](#SetsockoptIPMreqn)
- [func SetsockoptIPv6Mreq(fd, level, opt int, mreq *IPv6Mreq) (err error)](#SetsockoptIPv6Mreq)
- [func SetsockoptInet4Addr(fd, level, opt int, value [4]byte) (err error)](#SetsockoptInet4Addr)
- [func SetsockoptInt(fd, level, opt int, value int) (err error)](#SetsockoptInt)
- [func SetsockoptLinger(fd, level, opt int, l *Linger) (err error)](#SetsockoptLinger)
- [func SetsockoptString(fd, level, opt int, s string) (err error)](#SetsockoptString)
- [func SetsockoptTimeval(fd, level, opt int, tv *Timeval) (err error)](#SetsockoptTimeval)
- [func Settimeofday(tv *Timeval) (err error)](#Settimeofday)
- [func Setuid(uid int) (err error)](#Setuid)
- [func Setxattr(path string, attr string, data []byte, flags int) (err error)](#Setxattr)
- [func Shutdown(fd int, how int) (err error)](#Shutdown)
- [func SlicePtrFromStrings(ss []string) ([]*byte, error)](#SlicePtrFromStrings)
- [func Socket(domain, typ, proto int) (fd int, err error)](#Socket)
- [func Socketpair(domain, typ, proto int) (fd [2]int, err error)](#Socketpair)
- [func Splice(rfd int, roff *int64, wfd int, woff *int64, len int, flags int) (n int64, err error)](#Splice)
- [func StartProcess(argv0 string, argv []string, attr *ProcAttr) (pid int, handle uintptr, err error)](#StartProcess)
- [func Stat(path string, stat *Stat_t) (err error)](#Stat)
- [func Statfs(path string, buf *Statfs_t) (err error)](#Statfs)
- [func StringBytePtr(s string) *byte](#StringBytePtr)
- [func StringByteSlice(s string) []byte](#StringByteSlice)
- [func StringSlicePtr(ss []string) []*byte](#StringSlicePtr)
- [func Symlink(oldpath string, newpath string) (err error)](#Symlink)
- [func Sync()](#Sync)
- [func SyncFileRange(fd int, off int64, n int64, flags int) (err error)](#SyncFileRange)
- [func Syscall(trap, a1, a2, a3 uintptr) (r1, r2 uintptr, err Errno)](#Syscall)
- [func Syscall6(trap, a1, a2, a3, a4, a5, a6 uintptr) (r1, r2 uintptr, err Errno)](#Syscall6)
- [func Sysinfo(info *Sysinfo_t) (err error)](#Sysinfo)
- [func Tee(rfd int, wfd int, len int, flags int) (n int64, err error)](#Tee)
- [func Tgkill(tgid int, tid int, sig Signal) (err error)](#Tgkill)
- [func Times(tms *Tms) (ticks uintptr, err error)](#Times)
- [func TimespecToNsec(ts Timespec) int64](#TimespecToNsec)
- [func TimevalToNsec(tv Timeval) int64](#TimevalToNsec)
- [func Truncate(path string, length int64) (err error)](#Truncate)
- [func Umask(mask int) (oldmask int)](#Umask)
- [func Uname(buf *Utsname) (err error)](#Uname)
- [func UnixCredentials(ucred *Ucred) []byte](#UnixCredentials)
- [func UnixRights(fds ...int) []byte](#UnixRights)
- [func Unlink(path string) error](#Unlink)
- [func Unlinkat(dirfd int, path string) error](#Unlinkat)
- [func Unmount(target string, flags int) (err error)](#Unmount)
- [func Unsetenv(key string) error](#Unsetenv)
- [func Unshare(flags int) (err error)](#Unshare)
- [func Ustat(dev int, ubuf *Ustat_t) (err error)](#Ustat)
- [func Utime(path string, buf *Utimbuf) (err error)](#Utime)
- [func Utimes(path string, tv []Timeval) (err error)](#Utimes)
- [func UtimesNano(path string, ts []Timespec) (err error)](#UtimesNano)
- [func Wait4(pid int, wstatus *WaitStatus, options int, rusage *Rusage) (wpid int, err error)](#Wait4)
- [func Write(fd int, p []byte) (n int, err error)](#Write)
- [type Cmsghdr](#Cmsghdr)
  - [func (cmsg *Cmsghdr) SetLen(length int)](#Cmsghdr.SetLen)
- [type Conn](#Conn)
- [type Credential](#Credential)
- [type Dirent](#Dirent)
- [type EpollEvent](#EpollEvent)
- [type Errno](#Errno)
  - [func (e Errno) Error() string](#Errno.Error)
  - [func (e Errno) Temporary() bool](#Errno.Temporary)
  - [func (e Errno) Timeout() bool](#Errno.Timeout)
- [type FdSet](#FdSet)
- [type Flock_t](#Flock_t)
- [type Fsid](#Fsid)
- [type ICMPv6Filter](#ICMPv6Filter)
  - [func GetsockoptICMPv6Filter(fd, level, opt int) (*ICMPv6Filter, error)](#GetsockoptICMPv6Filter)
- [type IPMreq](#IPMreq)
  - [func GetsockoptIPMreq(fd, level, opt int) (*IPMreq, error)](#GetsockoptIPMreq)
- [type IPMreqn](#IPMreqn)
  - [func GetsockoptIPMreqn(fd, level, opt int) (*IPMreqn, error)](#GetsockoptIPMreqn)
- [type IPv6MTUInfo](#IPv6MTUInfo)
  - [func GetsockoptIPv6MTUInfo(fd, level, opt int) (*IPv6MTUInfo, error)](#GetsockoptIPv6MTUInfo)
- [type IPv6Mreq](#IPv6Mreq)
  - [func GetsockoptIPv6Mreq(fd, level, opt int) (*IPv6Mreq, error)](#GetsockoptIPv6Mreq)
- [type IfAddrmsg](#IfAddrmsg)
- [type IfInfomsg](#IfInfomsg)
- [type Inet4Pktinfo](#Inet4Pktinfo)
- [type Inet6Pktinfo](#Inet6Pktinfo)
- [type InotifyEvent](#InotifyEvent)
- [type Iovec](#Iovec)
  - [func (iov *Iovec) SetLen(length int)](#Iovec.SetLen)
- [type Linger](#Linger)
- [type Msghdr](#Msghdr)
  - [func (msghdr *Msghdr) SetControllen(length int)](#Msghdr.SetControllen)
- [type NetlinkMessage](#NetlinkMessage)
- [type NetlinkRouteAttr](#NetlinkRouteAttr)
- [type NetlinkRouteRequest](#NetlinkRouteRequest)
- [type NlAttr](#NlAttr)
- [type NlMsgerr](#NlMsgerr)
- [type NlMsghdr](#NlMsghdr)
- [type ProcAttr](#ProcAttr)
- [type PtraceRegs](#PtraceRegs)
  - [func (r *PtraceRegs) PC() uint64](#PtraceRegs.PC)
  - [func (r *PtraceRegs) SetPC(pc uint64)](#PtraceRegs.SetPC)
- [type RawConn](#RawConn)
- [type RawSockaddr](#RawSockaddr)
- [type RawSockaddrAny](#RawSockaddrAny)
- [type RawSockaddrInet4](#RawSockaddrInet4)
- [type RawSockaddrInet6](#RawSockaddrInet6)
- [type RawSockaddrLinklayer](#RawSockaddrLinklayer)
- [type RawSockaddrNetlink](#RawSockaddrNetlink)
- [type RawSockaddrUnix](#RawSockaddrUnix)
- [type Rlimit](#Rlimit)
- [type RtAttr](#RtAttr)
- [type RtGenmsg](#RtGenmsg)
- [type RtMsg](#RtMsg)
- [type RtNexthop](#RtNexthop)
- [type Rusage](#Rusage)
- [type Signal](#Signal)
  - [func (s Signal) Signal()](#Signal.Signal)
  - [func (s Signal) String() string](#Signal.String)
- [type SockFilter](#SockFilter)
  - [func LsfJump(code, k, jt, jf int) *SockFilter](#LsfJump)
  - [func LsfStmt(code, k int) *SockFilter](#LsfStmt)
- [type SockFprog](#SockFprog)
- [type Sockaddr](#Sockaddr)
  - [func Getpeername(fd int) (sa Sockaddr, err error)](#Getpeername)
  - [func Getsockname(fd int) (sa Sockaddr, err error)](#Getsockname)
- [type SockaddrInet4](#SockaddrInet4)
- [type SockaddrInet6](#SockaddrInet6)
- [type SockaddrLinklayer](#SockaddrLinklayer)
- [type SockaddrNetlink](#SockaddrNetlink)
- [type SockaddrUnix](#SockaddrUnix)
- [type SocketControlMessage](#SocketControlMessage)
- [type Stat_t](#Stat_t)
- [type Statfs_t](#Statfs_t)
- [type SysProcAttr](#SysProcAttr)
- [type SysProcIDMap](#SysProcIDMap)
- [type Sysinfo_t](#Sysinfo_t)
- [type TCPInfo](#TCPInfo)
- [type Termios](#Termios)
- [type Time_t](#Time_t)
  - [func Time(t *Time_t) (tt Time_t, err error)](#Time)
- [type Timespec](#Timespec)
  - [func NsecToTimespec(nsec int64) Timespec](#NsecToTimespec)
  - [func (ts *Timespec) Nano() int64](#Timespec.Nano)
  - [func (ts *Timespec) Unix() (sec int64, nsec int64)](#Timespec.Unix)
- [type Timeval](#Timeval)
  - [func NsecToTimeval(nsec int64) Timeval](#NsecToTimeval)
  - [func (tv *Timeval) Nano() int64](#Timeval.Nano)
  - [func (tv *Timeval) Unix() (sec int64, nsec int64)](#Timeval.Unix)
- [type Timex](#Timex)
- [type Tms](#Tms)
- [type Ucred](#Ucred)
  - [func GetsockoptUcred(fd, level, opt int) (*Ucred, error)](#GetsockoptUcred)
  - [func ParseUnixCredentials(m *SocketControlMessage) (*Ucred, error)](#ParseUnixCredentials)
- [type Ustat_t](#Ustat_t)
- [type Utimbuf](#Utimbuf)
- [type Utsname](#Utsname)
- [type WaitStatus](#WaitStatus)
  - [func (w WaitStatus) Continued() bool](#WaitStatus.Continued)
  - [func (w WaitStatus) CoreDump() bool](#WaitStatus.CoreDump)
  - [func (w WaitStatus) ExitStatus() int](#WaitStatus.ExitStatus)
  - [func (w WaitStatus) Exited() bool](#WaitStatus.Exited)
  - [func (w WaitStatus) Signal() Signal](#WaitStatus.Signal)
  - [func (w WaitStatus) Signaled() bool](#WaitStatus.Signaled)
  - [func (w WaitStatus) StopSignal() Signal](#WaitStatus.StopSignal)
  - [func (w WaitStatus) Stopped() bool](#WaitStatus.Stopped)
  - [func (w WaitStatus) TrapCause() int](#WaitStatus.TrapCause)

### Package files
 [dirent.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/dirent.go) [endian_little.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/endian_little.go) [env_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/env_unix.go) [exec_linux.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_linux.go) [exec_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_unix.go) [flock.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/flock.go) [lsf_linux.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/lsf_linux.go) [msan0.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/msan0.go) [net.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/net.go) [netlink_linux.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/netlink_linux.go) [setuidgid_linux.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/setuidgid_linux.go) [sockcmsg_linux.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/sockcmsg_linux.go) [sockcmsg_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/sockcmsg_unix.go) [str.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/str.go) [syscall.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go) [syscall_linux.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go) [syscall_linux_amd64.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux_amd64.go) [syscall_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go) [timestruct.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/timestruct.go) [zerrors_linux_amd64.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zerrors_linux_amd64.go) [zsyscall_linux_amd64.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go) [zsysnum_linux_amd64.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsysnum_linux_amd64.go) [ztypes_linux_amd64.go](//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="AF_ALG">AF_ALG</span>                           = 0x26
    <span id="AF_APPLETALK">AF_APPLETALK</span>                     = 0x5
    <span id="AF_ASH">AF_ASH</span>                           = 0x12
    <span id="AF_ATMPVC">AF_ATMPVC</span>                        = 0x8
    <span id="AF_ATMSVC">AF_ATMSVC</span>                        = 0x14
    <span id="AF_AX25">AF_AX25</span>                          = 0x3
    <span id="AF_BLUETOOTH">AF_BLUETOOTH</span>                     = 0x1f
    <span id="AF_BRIDGE">AF_BRIDGE</span>                        = 0x7
    <span id="AF_CAIF">AF_CAIF</span>                          = 0x25
    <span id="AF_CAN">AF_CAN</span>                           = 0x1d
    <span id="AF_DECnet">AF_DECnet</span>                        = 0xc
    <span id="AF_ECONET">AF_ECONET</span>                        = 0x13
    <span id="AF_FILE">AF_FILE</span>                          = 0x1
    <span id="AF_IEEE802154">AF_IEEE802154</span>                    = 0x24
    <span id="AF_INET">AF_INET</span>                          = 0x2
    <span id="AF_INET6">AF_INET6</span>                         = 0xa
    <span id="AF_IPX">AF_IPX</span>                           = 0x4
    <span id="AF_IRDA">AF_IRDA</span>                          = 0x17
    <span id="AF_ISDN">AF_ISDN</span>                          = 0x22
    <span id="AF_IUCV">AF_IUCV</span>                          = 0x20
    <span id="AF_KEY">AF_KEY</span>                           = 0xf
    <span id="AF_LLC">AF_LLC</span>                           = 0x1a
    <span id="AF_LOCAL">AF_LOCAL</span>                         = 0x1
    <span id="AF_MAX">AF_MAX</span>                           = 0x27
    <span id="AF_NETBEUI">AF_NETBEUI</span>                       = 0xd
    <span id="AF_NETLINK">AF_NETLINK</span>                       = 0x10
    <span id="AF_NETROM">AF_NETROM</span>                        = 0x6
    <span id="AF_PACKET">AF_PACKET</span>                        = 0x11
    <span id="AF_PHONET">AF_PHONET</span>                        = 0x23
    <span id="AF_PPPOX">AF_PPPOX</span>                         = 0x18
    <span id="AF_RDS">AF_RDS</span>                           = 0x15
    <span id="AF_ROSE">AF_ROSE</span>                          = 0xb
    <span id="AF_ROUTE">AF_ROUTE</span>                         = 0x10
    <span id="AF_RXRPC">AF_RXRPC</span>                         = 0x21
    <span id="AF_SECURITY">AF_SECURITY</span>                      = 0xe
    <span id="AF_SNA">AF_SNA</span>                           = 0x16
    <span id="AF_TIPC">AF_TIPC</span>                          = 0x1e
    <span id="AF_UNIX">AF_UNIX</span>                          = 0x1
    <span id="AF_UNSPEC">AF_UNSPEC</span>                        = 0x0
    <span id="AF_WANPIPE">AF_WANPIPE</span>                       = 0x19
    <span id="AF_X25">AF_X25</span>                           = 0x9
    <span id="ARPHRD_ADAPT">ARPHRD_ADAPT</span>                     = 0x108
    <span id="ARPHRD_APPLETLK">ARPHRD_APPLETLK</span>                  = 0x8
    <span id="ARPHRD_ARCNET">ARPHRD_ARCNET</span>                    = 0x7
    <span id="ARPHRD_ASH">ARPHRD_ASH</span>                       = 0x30d
    <span id="ARPHRD_ATM">ARPHRD_ATM</span>                       = 0x13
    <span id="ARPHRD_AX25">ARPHRD_AX25</span>                      = 0x3
    <span id="ARPHRD_BIF">ARPHRD_BIF</span>                       = 0x307
    <span id="ARPHRD_CHAOS">ARPHRD_CHAOS</span>                     = 0x5
    <span id="ARPHRD_CISCO">ARPHRD_CISCO</span>                     = 0x201
    <span id="ARPHRD_CSLIP">ARPHRD_CSLIP</span>                     = 0x101
    <span id="ARPHRD_CSLIP6">ARPHRD_CSLIP6</span>                    = 0x103
    <span id="ARPHRD_DDCMP">ARPHRD_DDCMP</span>                     = 0x205
    <span id="ARPHRD_DLCI">ARPHRD_DLCI</span>                      = 0xf
    <span id="ARPHRD_ECONET">ARPHRD_ECONET</span>                    = 0x30e
    <span id="ARPHRD_EETHER">ARPHRD_EETHER</span>                    = 0x2
    <span id="ARPHRD_ETHER">ARPHRD_ETHER</span>                     = 0x1
    <span id="ARPHRD_EUI64">ARPHRD_EUI64</span>                     = 0x1b
    <span id="ARPHRD_FCAL">ARPHRD_FCAL</span>                      = 0x311
    <span id="ARPHRD_FCFABRIC">ARPHRD_FCFABRIC</span>                  = 0x313
    <span id="ARPHRD_FCPL">ARPHRD_FCPL</span>                      = 0x312
    <span id="ARPHRD_FCPP">ARPHRD_FCPP</span>                      = 0x310
    <span id="ARPHRD_FDDI">ARPHRD_FDDI</span>                      = 0x306
    <span id="ARPHRD_FRAD">ARPHRD_FRAD</span>                      = 0x302
    <span id="ARPHRD_HDLC">ARPHRD_HDLC</span>                      = 0x201
    <span id="ARPHRD_HIPPI">ARPHRD_HIPPI</span>                     = 0x30c
    <span id="ARPHRD_HWX25">ARPHRD_HWX25</span>                     = 0x110
    <span id="ARPHRD_IEEE1394">ARPHRD_IEEE1394</span>                  = 0x18
    <span id="ARPHRD_IEEE802">ARPHRD_IEEE802</span>                   = 0x6
    <span id="ARPHRD_IEEE80211">ARPHRD_IEEE80211</span>                 = 0x321
    <span id="ARPHRD_IEEE80211_PRISM">ARPHRD_IEEE80211_PRISM</span>           = 0x322
    <span id="ARPHRD_IEEE80211_RADIOTAP">ARPHRD_IEEE80211_RADIOTAP</span>        = 0x323
    <span id="ARPHRD_IEEE802154">ARPHRD_IEEE802154</span>                = 0x324
    <span id="ARPHRD_IEEE802154_PHY">ARPHRD_IEEE802154_PHY</span>            = 0x325
    <span id="ARPHRD_IEEE802_TR">ARPHRD_IEEE802_TR</span>                = 0x320
    <span id="ARPHRD_INFINIBAND">ARPHRD_INFINIBAND</span>                = 0x20
    <span id="ARPHRD_IPDDP">ARPHRD_IPDDP</span>                     = 0x309
    <span id="ARPHRD_IPGRE">ARPHRD_IPGRE</span>                     = 0x30a
    <span id="ARPHRD_IRDA">ARPHRD_IRDA</span>                      = 0x30f
    <span id="ARPHRD_LAPB">ARPHRD_LAPB</span>                      = 0x204
    <span id="ARPHRD_LOCALTLK">ARPHRD_LOCALTLK</span>                  = 0x305
    <span id="ARPHRD_LOOPBACK">ARPHRD_LOOPBACK</span>                  = 0x304
    <span id="ARPHRD_METRICOM">ARPHRD_METRICOM</span>                  = 0x17
    <span id="ARPHRD_NETROM">ARPHRD_NETROM</span>                    = 0x0
    <span id="ARPHRD_NONE">ARPHRD_NONE</span>                      = 0xfffe
    <span id="ARPHRD_PIMREG">ARPHRD_PIMREG</span>                    = 0x30b
    <span id="ARPHRD_PPP">ARPHRD_PPP</span>                       = 0x200
    <span id="ARPHRD_PRONET">ARPHRD_PRONET</span>                    = 0x4
    <span id="ARPHRD_RAWHDLC">ARPHRD_RAWHDLC</span>                   = 0x206
    <span id="ARPHRD_ROSE">ARPHRD_ROSE</span>                      = 0x10e
    <span id="ARPHRD_RSRVD">ARPHRD_RSRVD</span>                     = 0x104
    <span id="ARPHRD_SIT">ARPHRD_SIT</span>                       = 0x308
    <span id="ARPHRD_SKIP">ARPHRD_SKIP</span>                      = 0x303
    <span id="ARPHRD_SLIP">ARPHRD_SLIP</span>                      = 0x100
    <span id="ARPHRD_SLIP6">ARPHRD_SLIP6</span>                     = 0x102
    <span id="ARPHRD_TUNNEL">ARPHRD_TUNNEL</span>                    = 0x300
    <span id="ARPHRD_TUNNEL6">ARPHRD_TUNNEL6</span>                   = 0x301
    <span id="ARPHRD_VOID">ARPHRD_VOID</span>                      = 0xffff
    <span id="ARPHRD_X25">ARPHRD_X25</span>                       = 0x10f
    <span id="BPF_A">BPF_A</span>                            = 0x10
    <span id="BPF_ABS">BPF_ABS</span>                          = 0x20
    <span id="BPF_ADD">BPF_ADD</span>                          = 0x0
    <span id="BPF_ALU">BPF_ALU</span>                          = 0x4
    <span id="BPF_AND">BPF_AND</span>                          = 0x50
    <span id="BPF_B">BPF_B</span>                            = 0x10
    <span id="BPF_DIV">BPF_DIV</span>                          = 0x30
    <span id="BPF_H">BPF_H</span>                            = 0x8
    <span id="BPF_IMM">BPF_IMM</span>                          = 0x0
    <span id="BPF_IND">BPF_IND</span>                          = 0x40
    <span id="BPF_JA">BPF_JA</span>                           = 0x0
    <span id="BPF_JEQ">BPF_JEQ</span>                          = 0x10
    <span id="BPF_JGE">BPF_JGE</span>                          = 0x30
    <span id="BPF_JGT">BPF_JGT</span>                          = 0x20
    <span id="BPF_JMP">BPF_JMP</span>                          = 0x5
    <span id="BPF_JSET">BPF_JSET</span>                         = 0x40
    <span id="BPF_K">BPF_K</span>                            = 0x0
    <span id="BPF_LD">BPF_LD</span>                           = 0x0
    <span id="BPF_LDX">BPF_LDX</span>                          = 0x1
    <span id="BPF_LEN">BPF_LEN</span>                          = 0x80
    <span id="BPF_LSH">BPF_LSH</span>                          = 0x60
    <span id="BPF_MAJOR_VERSION">BPF_MAJOR_VERSION</span>                = 0x1
    <span id="BPF_MAXINSNS">BPF_MAXINSNS</span>                     = 0x1000
    <span id="BPF_MEM">BPF_MEM</span>                          = 0x60
    <span id="BPF_MEMWORDS">BPF_MEMWORDS</span>                     = 0x10
    <span id="BPF_MINOR_VERSION">BPF_MINOR_VERSION</span>                = 0x1
    <span id="BPF_MISC">BPF_MISC</span>                         = 0x7
    <span id="BPF_MSH">BPF_MSH</span>                          = 0xa0
    <span id="BPF_MUL">BPF_MUL</span>                          = 0x20
    <span id="BPF_NEG">BPF_NEG</span>                          = 0x80
    <span id="BPF_OR">BPF_OR</span>                           = 0x40
    <span id="BPF_RET">BPF_RET</span>                          = 0x6
    <span id="BPF_RSH">BPF_RSH</span>                          = 0x70
    <span id="BPF_ST">BPF_ST</span>                           = 0x2
    <span id="BPF_STX">BPF_STX</span>                          = 0x3
    <span id="BPF_SUB">BPF_SUB</span>                          = 0x10
    <span id="BPF_TAX">BPF_TAX</span>                          = 0x0
    <span id="BPF_TXA">BPF_TXA</span>                          = 0x80
    <span id="BPF_W">BPF_W</span>                            = 0x0
    <span id="BPF_X">BPF_X</span>                            = 0x8
    <span id="CLONE_CHILD_CLEARTID">CLONE_CHILD_CLEARTID</span>             = 0x200000
    <span id="CLONE_CHILD_SETTID">CLONE_CHILD_SETTID</span>               = 0x1000000
    <span id="CLONE_DETACHED">CLONE_DETACHED</span>                   = 0x400000
    <span id="CLONE_FILES">CLONE_FILES</span>                      = 0x400
    <span id="CLONE_FS">CLONE_FS</span>                         = 0x200
    <span id="CLONE_IO">CLONE_IO</span>                         = 0x80000000
    <span id="CLONE_NEWIPC">CLONE_NEWIPC</span>                     = 0x8000000
    <span id="CLONE_NEWNET">CLONE_NEWNET</span>                     = 0x40000000
    <span id="CLONE_NEWNS">CLONE_NEWNS</span>                      = 0x20000
    <span id="CLONE_NEWPID">CLONE_NEWPID</span>                     = 0x20000000
    <span id="CLONE_NEWUSER">CLONE_NEWUSER</span>                    = 0x10000000
    <span id="CLONE_NEWUTS">CLONE_NEWUTS</span>                     = 0x4000000
    <span id="CLONE_PARENT">CLONE_PARENT</span>                     = 0x8000
    <span id="CLONE_PARENT_SETTID">CLONE_PARENT_SETTID</span>              = 0x100000
    <span id="CLONE_PTRACE">CLONE_PTRACE</span>                     = 0x2000
    <span id="CLONE_SETTLS">CLONE_SETTLS</span>                     = 0x80000
    <span id="CLONE_SIGHAND">CLONE_SIGHAND</span>                    = 0x800
    <span id="CLONE_SYSVSEM">CLONE_SYSVSEM</span>                    = 0x40000
    <span id="CLONE_THREAD">CLONE_THREAD</span>                     = 0x10000
    <span id="CLONE_UNTRACED">CLONE_UNTRACED</span>                   = 0x800000
    <span id="CLONE_VFORK">CLONE_VFORK</span>                      = 0x4000
    <span id="CLONE_VM">CLONE_VM</span>                         = 0x100
    <span id="DT_BLK">DT_BLK</span>                           = 0x6
    <span id="DT_CHR">DT_CHR</span>                           = 0x2
    <span id="DT_DIR">DT_DIR</span>                           = 0x4
    <span id="DT_FIFO">DT_FIFO</span>                          = 0x1
    <span id="DT_LNK">DT_LNK</span>                           = 0xa
    <span id="DT_REG">DT_REG</span>                           = 0x8
    <span id="DT_SOCK">DT_SOCK</span>                          = 0xc
    <span id="DT_UNKNOWN">DT_UNKNOWN</span>                       = 0x0
    <span id="DT_WHT">DT_WHT</span>                           = 0xe
    <span id="EPOLLERR">EPOLLERR</span>                         = 0x8
    <span id="EPOLLET">EPOLLET</span>                          = -0x80000000
    <span id="EPOLLHUP">EPOLLHUP</span>                         = 0x10
    <span id="EPOLLIN">EPOLLIN</span>                          = 0x1
    <span id="EPOLLMSG">EPOLLMSG</span>                         = 0x400
    <span id="EPOLLONESHOT">EPOLLONESHOT</span>                     = 0x40000000
    <span id="EPOLLOUT">EPOLLOUT</span>                         = 0x4
    <span id="EPOLLPRI">EPOLLPRI</span>                         = 0x2
    <span id="EPOLLRDBAND">EPOLLRDBAND</span>                      = 0x80
    <span id="EPOLLRDHUP">EPOLLRDHUP</span>                       = 0x2000
    <span id="EPOLLRDNORM">EPOLLRDNORM</span>                      = 0x40
    <span id="EPOLLWRBAND">EPOLLWRBAND</span>                      = 0x200
    <span id="EPOLLWRNORM">EPOLLWRNORM</span>                      = 0x100
    <span id="EPOLL_CLOEXEC">EPOLL_CLOEXEC</span>                    = 0x80000
    <span id="EPOLL_CTL_ADD">EPOLL_CTL_ADD</span>                    = 0x1
    <span id="EPOLL_CTL_DEL">EPOLL_CTL_DEL</span>                    = 0x2
    <span id="EPOLL_CTL_MOD">EPOLL_CTL_MOD</span>                    = 0x3
    <span id="EPOLL_NONBLOCK">EPOLL_NONBLOCK</span>                   = 0x800
    <span id="ETH_P_1588">ETH_P_1588</span>                       = 0x88f7
    <span id="ETH_P_8021Q">ETH_P_8021Q</span>                      = 0x8100
    <span id="ETH_P_802_2">ETH_P_802_2</span>                      = 0x4
    <span id="ETH_P_802_3">ETH_P_802_3</span>                      = 0x1
    <span id="ETH_P_AARP">ETH_P_AARP</span>                       = 0x80f3
    <span id="ETH_P_ALL">ETH_P_ALL</span>                        = 0x3
    <span id="ETH_P_AOE">ETH_P_AOE</span>                        = 0x88a2
    <span id="ETH_P_ARCNET">ETH_P_ARCNET</span>                     = 0x1a
    <span id="ETH_P_ARP">ETH_P_ARP</span>                        = 0x806
    <span id="ETH_P_ATALK">ETH_P_ATALK</span>                      = 0x809b
    <span id="ETH_P_ATMFATE">ETH_P_ATMFATE</span>                    = 0x8884
    <span id="ETH_P_ATMMPOA">ETH_P_ATMMPOA</span>                    = 0x884c
    <span id="ETH_P_AX25">ETH_P_AX25</span>                       = 0x2
    <span id="ETH_P_BPQ">ETH_P_BPQ</span>                        = 0x8ff
    <span id="ETH_P_CAIF">ETH_P_CAIF</span>                       = 0xf7
    <span id="ETH_P_CAN">ETH_P_CAN</span>                        = 0xc
    <span id="ETH_P_CONTROL">ETH_P_CONTROL</span>                    = 0x16
    <span id="ETH_P_CUST">ETH_P_CUST</span>                       = 0x6006
    <span id="ETH_P_DDCMP">ETH_P_DDCMP</span>                      = 0x6
    <span id="ETH_P_DEC">ETH_P_DEC</span>                        = 0x6000
    <span id="ETH_P_DIAG">ETH_P_DIAG</span>                       = 0x6005
    <span id="ETH_P_DNA_DL">ETH_P_DNA_DL</span>                     = 0x6001
    <span id="ETH_P_DNA_RC">ETH_P_DNA_RC</span>                     = 0x6002
    <span id="ETH_P_DNA_RT">ETH_P_DNA_RT</span>                     = 0x6003
    <span id="ETH_P_DSA">ETH_P_DSA</span>                        = 0x1b
    <span id="ETH_P_ECONET">ETH_P_ECONET</span>                     = 0x18
    <span id="ETH_P_EDSA">ETH_P_EDSA</span>                       = 0xdada
    <span id="ETH_P_FCOE">ETH_P_FCOE</span>                       = 0x8906
    <span id="ETH_P_FIP">ETH_P_FIP</span>                        = 0x8914
    <span id="ETH_P_HDLC">ETH_P_HDLC</span>                       = 0x19
    <span id="ETH_P_IEEE802154">ETH_P_IEEE802154</span>                 = 0xf6
    <span id="ETH_P_IEEEPUP">ETH_P_IEEEPUP</span>                    = 0xa00
    <span id="ETH_P_IEEEPUPAT">ETH_P_IEEEPUPAT</span>                  = 0xa01
    <span id="ETH_P_IP">ETH_P_IP</span>                         = 0x800
    <span id="ETH_P_IPV6">ETH_P_IPV6</span>                       = 0x86dd
    <span id="ETH_P_IPX">ETH_P_IPX</span>                        = 0x8137
    <span id="ETH_P_IRDA">ETH_P_IRDA</span>                       = 0x17
    <span id="ETH_P_LAT">ETH_P_LAT</span>                        = 0x6004
    <span id="ETH_P_LINK_CTL">ETH_P_LINK_CTL</span>                   = 0x886c
    <span id="ETH_P_LOCALTALK">ETH_P_LOCALTALK</span>                  = 0x9
    <span id="ETH_P_LOOP">ETH_P_LOOP</span>                       = 0x60
    <span id="ETH_P_MOBITEX">ETH_P_MOBITEX</span>                    = 0x15
    <span id="ETH_P_MPLS_MC">ETH_P_MPLS_MC</span>                    = 0x8848
    <span id="ETH_P_MPLS_UC">ETH_P_MPLS_UC</span>                    = 0x8847
    <span id="ETH_P_PAE">ETH_P_PAE</span>                        = 0x888e
    <span id="ETH_P_PAUSE">ETH_P_PAUSE</span>                      = 0x8808
    <span id="ETH_P_PHONET">ETH_P_PHONET</span>                     = 0xf5
    <span id="ETH_P_PPPTALK">ETH_P_PPPTALK</span>                    = 0x10
    <span id="ETH_P_PPP_DISC">ETH_P_PPP_DISC</span>                   = 0x8863
    <span id="ETH_P_PPP_MP">ETH_P_PPP_MP</span>                     = 0x8
    <span id="ETH_P_PPP_SES">ETH_P_PPP_SES</span>                    = 0x8864
    <span id="ETH_P_PUP">ETH_P_PUP</span>                        = 0x200
    <span id="ETH_P_PUPAT">ETH_P_PUPAT</span>                      = 0x201
    <span id="ETH_P_RARP">ETH_P_RARP</span>                       = 0x8035
    <span id="ETH_P_SCA">ETH_P_SCA</span>                        = 0x6007
    <span id="ETH_P_SLOW">ETH_P_SLOW</span>                       = 0x8809
    <span id="ETH_P_SNAP">ETH_P_SNAP</span>                       = 0x5
    <span id="ETH_P_TEB">ETH_P_TEB</span>                        = 0x6558
    <span id="ETH_P_TIPC">ETH_P_TIPC</span>                       = 0x88ca
    <span id="ETH_P_TRAILER">ETH_P_TRAILER</span>                    = 0x1c
    <span id="ETH_P_TR_802_2">ETH_P_TR_802_2</span>                   = 0x11
    <span id="ETH_P_WAN_PPP">ETH_P_WAN_PPP</span>                    = 0x7
    <span id="ETH_P_WCCP">ETH_P_WCCP</span>                       = 0x883e
    <span id="ETH_P_X25">ETH_P_X25</span>                        = 0x805
    <span id="FD_CLOEXEC">FD_CLOEXEC</span>                       = 0x1
    <span id="FD_SETSIZE">FD_SETSIZE</span>                       = 0x400
    <span id="F_DUPFD">F_DUPFD</span>                          = 0x0
    <span id="F_DUPFD_CLOEXEC">F_DUPFD_CLOEXEC</span>                  = 0x406
    <span id="F_EXLCK">F_EXLCK</span>                          = 0x4
    <span id="F_GETFD">F_GETFD</span>                          = 0x1
    <span id="F_GETFL">F_GETFL</span>                          = 0x3
    <span id="F_GETLEASE">F_GETLEASE</span>                       = 0x401
    <span id="F_GETLK">F_GETLK</span>                          = 0x5
    <span id="F_GETLK64">F_GETLK64</span>                        = 0x5
    <span id="F_GETOWN">F_GETOWN</span>                         = 0x9
    <span id="F_GETOWN_EX">F_GETOWN_EX</span>                      = 0x10
    <span id="F_GETPIPE_SZ">F_GETPIPE_SZ</span>                     = 0x408
    <span id="F_GETSIG">F_GETSIG</span>                         = 0xb
    <span id="F_LOCK">F_LOCK</span>                           = 0x1
    <span id="F_NOTIFY">F_NOTIFY</span>                         = 0x402
    <span id="F_OK">F_OK</span>                             = 0x0
    <span id="F_RDLCK">F_RDLCK</span>                          = 0x0
    <span id="F_SETFD">F_SETFD</span>                          = 0x2
    <span id="F_SETFL">F_SETFL</span>                          = 0x4
    <span id="F_SETLEASE">F_SETLEASE</span>                       = 0x400
    <span id="F_SETLK">F_SETLK</span>                          = 0x6
    <span id="F_SETLK64">F_SETLK64</span>                        = 0x6
    <span id="F_SETLKW">F_SETLKW</span>                         = 0x7
    <span id="F_SETLKW64">F_SETLKW64</span>                       = 0x7
    <span id="F_SETOWN">F_SETOWN</span>                         = 0x8
    <span id="F_SETOWN_EX">F_SETOWN_EX</span>                      = 0xf
    <span id="F_SETPIPE_SZ">F_SETPIPE_SZ</span>                     = 0x407
    <span id="F_SETSIG">F_SETSIG</span>                         = 0xa
    <span id="F_SHLCK">F_SHLCK</span>                          = 0x8
    <span id="F_TEST">F_TEST</span>                           = 0x3
    <span id="F_TLOCK">F_TLOCK</span>                          = 0x2
    <span id="F_ULOCK">F_ULOCK</span>                          = 0x0
    <span id="F_UNLCK">F_UNLCK</span>                          = 0x2
    <span id="F_WRLCK">F_WRLCK</span>                          = 0x1
    <span id="ICMPV6_FILTER">ICMPV6_FILTER</span>                    = 0x1
    <span id="IFA_F_DADFAILED">IFA_F_DADFAILED</span>                  = 0x8
    <span id="IFA_F_DEPRECATED">IFA_F_DEPRECATED</span>                 = 0x20
    <span id="IFA_F_HOMEADDRESS">IFA_F_HOMEADDRESS</span>                = 0x10
    <span id="IFA_F_NODAD">IFA_F_NODAD</span>                      = 0x2
    <span id="IFA_F_OPTIMISTIC">IFA_F_OPTIMISTIC</span>                 = 0x4
    <span id="IFA_F_PERMANENT">IFA_F_PERMANENT</span>                  = 0x80
    <span id="IFA_F_SECONDARY">IFA_F_SECONDARY</span>                  = 0x1
    <span id="IFA_F_TEMPORARY">IFA_F_TEMPORARY</span>                  = 0x1
    <span id="IFA_F_TENTATIVE">IFA_F_TENTATIVE</span>                  = 0x40
    <span id="IFA_MAX">IFA_MAX</span>                          = 0x7
    <span id="IFF_ALLMULTI">IFF_ALLMULTI</span>                     = 0x200
    <span id="IFF_AUTOMEDIA">IFF_AUTOMEDIA</span>                    = 0x4000
    <span id="IFF_BROADCAST">IFF_BROADCAST</span>                    = 0x2
    <span id="IFF_DEBUG">IFF_DEBUG</span>                        = 0x4
    <span id="IFF_DYNAMIC">IFF_DYNAMIC</span>                      = 0x8000
    <span id="IFF_LOOPBACK">IFF_LOOPBACK</span>                     = 0x8
    <span id="IFF_MASTER">IFF_MASTER</span>                       = 0x400
    <span id="IFF_MULTICAST">IFF_MULTICAST</span>                    = 0x1000
    <span id="IFF_NOARP">IFF_NOARP</span>                        = 0x80
    <span id="IFF_NOTRAILERS">IFF_NOTRAILERS</span>                   = 0x20
    <span id="IFF_NO_PI">IFF_NO_PI</span>                        = 0x1000
    <span id="IFF_ONE_QUEUE">IFF_ONE_QUEUE</span>                    = 0x2000
    <span id="IFF_POINTOPOINT">IFF_POINTOPOINT</span>                  = 0x10
    <span id="IFF_PORTSEL">IFF_PORTSEL</span>                      = 0x2000
    <span id="IFF_PROMISC">IFF_PROMISC</span>                      = 0x100
    <span id="IFF_RUNNING">IFF_RUNNING</span>                      = 0x40
    <span id="IFF_SLAVE">IFF_SLAVE</span>                        = 0x800
    <span id="IFF_TAP">IFF_TAP</span>                          = 0x2
    <span id="IFF_TUN">IFF_TUN</span>                          = 0x1
    <span id="IFF_TUN_EXCL">IFF_TUN_EXCL</span>                     = 0x8000
    <span id="IFF_UP">IFF_UP</span>                           = 0x1
    <span id="IFF_VNET_HDR">IFF_VNET_HDR</span>                     = 0x4000
    <span id="IFNAMSIZ">IFNAMSIZ</span>                         = 0x10
    <span id="IN_ACCESS">IN_ACCESS</span>                        = 0x1
    <span id="IN_ALL_EVENTS">IN_ALL_EVENTS</span>                    = 0xfff
    <span id="IN_ATTRIB">IN_ATTRIB</span>                        = 0x4
    <span id="IN_CLASSA_HOST">IN_CLASSA_HOST</span>                   = 0xffffff
    <span id="IN_CLASSA_MAX">IN_CLASSA_MAX</span>                    = 0x80
    <span id="IN_CLASSA_NET">IN_CLASSA_NET</span>                    = 0xff000000
    <span id="IN_CLASSA_NSHIFT">IN_CLASSA_NSHIFT</span>                 = 0x18
    <span id="IN_CLASSB_HOST">IN_CLASSB_HOST</span>                   = 0xffff
    <span id="IN_CLASSB_MAX">IN_CLASSB_MAX</span>                    = 0x10000
    <span id="IN_CLASSB_NET">IN_CLASSB_NET</span>                    = 0xffff0000
    <span id="IN_CLASSB_NSHIFT">IN_CLASSB_NSHIFT</span>                 = 0x10
    <span id="IN_CLASSC_HOST">IN_CLASSC_HOST</span>                   = 0xff
    <span id="IN_CLASSC_NET">IN_CLASSC_NET</span>                    = 0xffffff00
    <span id="IN_CLASSC_NSHIFT">IN_CLASSC_NSHIFT</span>                 = 0x8
    <span id="IN_CLOEXEC">IN_CLOEXEC</span>                       = 0x80000
    <span id="IN_CLOSE">IN_CLOSE</span>                         = 0x18
    <span id="IN_CLOSE_NOWRITE">IN_CLOSE_NOWRITE</span>                 = 0x10
    <span id="IN_CLOSE_WRITE">IN_CLOSE_WRITE</span>                   = 0x8
    <span id="IN_CREATE">IN_CREATE</span>                        = 0x100
    <span id="IN_DELETE">IN_DELETE</span>                        = 0x200
    <span id="IN_DELETE_SELF">IN_DELETE_SELF</span>                   = 0x400
    <span id="IN_DONT_FOLLOW">IN_DONT_FOLLOW</span>                   = 0x2000000
    <span id="IN_EXCL_UNLINK">IN_EXCL_UNLINK</span>                   = 0x4000000
    <span id="IN_IGNORED">IN_IGNORED</span>                       = 0x8000
    <span id="IN_ISDIR">IN_ISDIR</span>                         = 0x40000000
    <span id="IN_LOOPBACKNET">IN_LOOPBACKNET</span>                   = 0x7f
    <span id="IN_MASK_ADD">IN_MASK_ADD</span>                      = 0x20000000
    <span id="IN_MODIFY">IN_MODIFY</span>                        = 0x2
    <span id="IN_MOVE">IN_MOVE</span>                          = 0xc0
    <span id="IN_MOVED_FROM">IN_MOVED_FROM</span>                    = 0x40
    <span id="IN_MOVED_TO">IN_MOVED_TO</span>                      = 0x80
    <span id="IN_MOVE_SELF">IN_MOVE_SELF</span>                     = 0x800
    <span id="IN_NONBLOCK">IN_NONBLOCK</span>                      = 0x800
    <span id="IN_ONESHOT">IN_ONESHOT</span>                       = 0x80000000
    <span id="IN_ONLYDIR">IN_ONLYDIR</span>                       = 0x1000000
    <span id="IN_OPEN">IN_OPEN</span>                          = 0x20
    <span id="IN_Q_OVERFLOW">IN_Q_OVERFLOW</span>                    = 0x4000
    <span id="IN_UNMOUNT">IN_UNMOUNT</span>                       = 0x2000
    <span id="IPPROTO_AH">IPPROTO_AH</span>                       = 0x33
    <span id="IPPROTO_COMP">IPPROTO_COMP</span>                     = 0x6c
    <span id="IPPROTO_DCCP">IPPROTO_DCCP</span>                     = 0x21
    <span id="IPPROTO_DSTOPTS">IPPROTO_DSTOPTS</span>                  = 0x3c
    <span id="IPPROTO_EGP">IPPROTO_EGP</span>                      = 0x8
    <span id="IPPROTO_ENCAP">IPPROTO_ENCAP</span>                    = 0x62
    <span id="IPPROTO_ESP">IPPROTO_ESP</span>                      = 0x32
    <span id="IPPROTO_FRAGMENT">IPPROTO_FRAGMENT</span>                 = 0x2c
    <span id="IPPROTO_GRE">IPPROTO_GRE</span>                      = 0x2f
    <span id="IPPROTO_HOPOPTS">IPPROTO_HOPOPTS</span>                  = 0x0
    <span id="IPPROTO_ICMP">IPPROTO_ICMP</span>                     = 0x1
    <span id="IPPROTO_ICMPV6">IPPROTO_ICMPV6</span>                   = 0x3a
    <span id="IPPROTO_IDP">IPPROTO_IDP</span>                      = 0x16
    <span id="IPPROTO_IGMP">IPPROTO_IGMP</span>                     = 0x2
    <span id="IPPROTO_IP">IPPROTO_IP</span>                       = 0x0
    <span id="IPPROTO_IPIP">IPPROTO_IPIP</span>                     = 0x4
    <span id="IPPROTO_IPV6">IPPROTO_IPV6</span>                     = 0x29
    <span id="IPPROTO_MTP">IPPROTO_MTP</span>                      = 0x5c
    <span id="IPPROTO_NONE">IPPROTO_NONE</span>                     = 0x3b
    <span id="IPPROTO_PIM">IPPROTO_PIM</span>                      = 0x67
    <span id="IPPROTO_PUP">IPPROTO_PUP</span>                      = 0xc
    <span id="IPPROTO_RAW">IPPROTO_RAW</span>                      = 0xff
    <span id="IPPROTO_ROUTING">IPPROTO_ROUTING</span>                  = 0x2b
    <span id="IPPROTO_RSVP">IPPROTO_RSVP</span>                     = 0x2e
    <span id="IPPROTO_SCTP">IPPROTO_SCTP</span>                     = 0x84
    <span id="IPPROTO_TCP">IPPROTO_TCP</span>                      = 0x6
    <span id="IPPROTO_TP">IPPROTO_TP</span>                       = 0x1d
    <span id="IPPROTO_UDP">IPPROTO_UDP</span>                      = 0x11
    <span id="IPPROTO_UDPLITE">IPPROTO_UDPLITE</span>                  = 0x88
    <span id="IPV6_2292DSTOPTS">IPV6_2292DSTOPTS</span>                 = 0x4
    <span id="IPV6_2292HOPLIMIT">IPV6_2292HOPLIMIT</span>                = 0x8
    <span id="IPV6_2292HOPOPTS">IPV6_2292HOPOPTS</span>                 = 0x3
    <span id="IPV6_2292PKTINFO">IPV6_2292PKTINFO</span>                 = 0x2
    <span id="IPV6_2292PKTOPTIONS">IPV6_2292PKTOPTIONS</span>              = 0x6
    <span id="IPV6_2292RTHDR">IPV6_2292RTHDR</span>                   = 0x5
    <span id="IPV6_ADDRFORM">IPV6_ADDRFORM</span>                    = 0x1
    <span id="IPV6_ADD_MEMBERSHIP">IPV6_ADD_MEMBERSHIP</span>              = 0x14
    <span id="IPV6_AUTHHDR">IPV6_AUTHHDR</span>                     = 0xa
    <span id="IPV6_CHECKSUM">IPV6_CHECKSUM</span>                    = 0x7
    <span id="IPV6_DROP_MEMBERSHIP">IPV6_DROP_MEMBERSHIP</span>             = 0x15
    <span id="IPV6_DSTOPTS">IPV6_DSTOPTS</span>                     = 0x3b
    <span id="IPV6_HOPLIMIT">IPV6_HOPLIMIT</span>                    = 0x34
    <span id="IPV6_HOPOPTS">IPV6_HOPOPTS</span>                     = 0x36
    <span id="IPV6_IPSEC_POLICY">IPV6_IPSEC_POLICY</span>                = 0x22
    <span id="IPV6_JOIN_ANYCAST">IPV6_JOIN_ANYCAST</span>                = 0x1b
    <span id="IPV6_JOIN_GROUP">IPV6_JOIN_GROUP</span>                  = 0x14
    <span id="IPV6_LEAVE_ANYCAST">IPV6_LEAVE_ANYCAST</span>               = 0x1c
    <span id="IPV6_LEAVE_GROUP">IPV6_LEAVE_GROUP</span>                 = 0x15
    <span id="IPV6_MTU">IPV6_MTU</span>                         = 0x18
    <span id="IPV6_MTU_DISCOVER">IPV6_MTU_DISCOVER</span>                = 0x17
    <span id="IPV6_MULTICAST_HOPS">IPV6_MULTICAST_HOPS</span>              = 0x12
    <span id="IPV6_MULTICAST_IF">IPV6_MULTICAST_IF</span>                = 0x11
    <span id="IPV6_MULTICAST_LOOP">IPV6_MULTICAST_LOOP</span>              = 0x13
    <span id="IPV6_NEXTHOP">IPV6_NEXTHOP</span>                     = 0x9
    <span id="IPV6_PKTINFO">IPV6_PKTINFO</span>                     = 0x32
    <span id="IPV6_PMTUDISC_DO">IPV6_PMTUDISC_DO</span>                 = 0x2
    <span id="IPV6_PMTUDISC_DONT">IPV6_PMTUDISC_DONT</span>               = 0x0
    <span id="IPV6_PMTUDISC_PROBE">IPV6_PMTUDISC_PROBE</span>              = 0x3
    <span id="IPV6_PMTUDISC_WANT">IPV6_PMTUDISC_WANT</span>               = 0x1
    <span id="IPV6_RECVDSTOPTS">IPV6_RECVDSTOPTS</span>                 = 0x3a
    <span id="IPV6_RECVERR">IPV6_RECVERR</span>                     = 0x19
    <span id="IPV6_RECVHOPLIMIT">IPV6_RECVHOPLIMIT</span>                = 0x33
    <span id="IPV6_RECVHOPOPTS">IPV6_RECVHOPOPTS</span>                 = 0x35
    <span id="IPV6_RECVPKTINFO">IPV6_RECVPKTINFO</span>                 = 0x31
    <span id="IPV6_RECVRTHDR">IPV6_RECVRTHDR</span>                   = 0x38
    <span id="IPV6_RECVTCLASS">IPV6_RECVTCLASS</span>                  = 0x42
    <span id="IPV6_ROUTER_ALERT">IPV6_ROUTER_ALERT</span>                = 0x16
    <span id="IPV6_RTHDR">IPV6_RTHDR</span>                       = 0x39
    <span id="IPV6_RTHDRDSTOPTS">IPV6_RTHDRDSTOPTS</span>                = 0x37
    <span id="IPV6_RTHDR_LOOSE">IPV6_RTHDR_LOOSE</span>                 = 0x0
    <span id="IPV6_RTHDR_STRICT">IPV6_RTHDR_STRICT</span>                = 0x1
    <span id="IPV6_RTHDR_TYPE_0">IPV6_RTHDR_TYPE_0</span>                = 0x0
    <span id="IPV6_RXDSTOPTS">IPV6_RXDSTOPTS</span>                   = 0x3b
    <span id="IPV6_RXHOPOPTS">IPV6_RXHOPOPTS</span>                   = 0x36
    <span id="IPV6_TCLASS">IPV6_TCLASS</span>                      = 0x43
    <span id="IPV6_UNICAST_HOPS">IPV6_UNICAST_HOPS</span>                = 0x10
    <span id="IPV6_V6ONLY">IPV6_V6ONLY</span>                      = 0x1a
    <span id="IPV6_XFRM_POLICY">IPV6_XFRM_POLICY</span>                 = 0x23
    <span id="IP_ADD_MEMBERSHIP">IP_ADD_MEMBERSHIP</span>                = 0x23
    <span id="IP_ADD_SOURCE_MEMBERSHIP">IP_ADD_SOURCE_MEMBERSHIP</span>         = 0x27
    <span id="IP_BLOCK_SOURCE">IP_BLOCK_SOURCE</span>                  = 0x26
    <span id="IP_DEFAULT_MULTICAST_LOOP">IP_DEFAULT_MULTICAST_LOOP</span>        = 0x1
    <span id="IP_DEFAULT_MULTICAST_TTL">IP_DEFAULT_MULTICAST_TTL</span>         = 0x1
    <span id="IP_DF">IP_DF</span>                            = 0x4000
    <span id="IP_DROP_MEMBERSHIP">IP_DROP_MEMBERSHIP</span>               = 0x24
    <span id="IP_DROP_SOURCE_MEMBERSHIP">IP_DROP_SOURCE_MEMBERSHIP</span>        = 0x28
    <span id="IP_FREEBIND">IP_FREEBIND</span>                      = 0xf
    <span id="IP_HDRINCL">IP_HDRINCL</span>                       = 0x3
    <span id="IP_IPSEC_POLICY">IP_IPSEC_POLICY</span>                  = 0x10
    <span id="IP_MAXPACKET">IP_MAXPACKET</span>                     = 0xffff
    <span id="IP_MAX_MEMBERSHIPS">IP_MAX_MEMBERSHIPS</span>               = 0x14
    <span id="IP_MF">IP_MF</span>                            = 0x2000
    <span id="IP_MINTTL">IP_MINTTL</span>                        = 0x15
    <span id="IP_MSFILTER">IP_MSFILTER</span>                      = 0x29
    <span id="IP_MSS">IP_MSS</span>                           = 0x240
    <span id="IP_MTU">IP_MTU</span>                           = 0xe
    <span id="IP_MTU_DISCOVER">IP_MTU_DISCOVER</span>                  = 0xa
    <span id="IP_MULTICAST_IF">IP_MULTICAST_IF</span>                  = 0x20
    <span id="IP_MULTICAST_LOOP">IP_MULTICAST_LOOP</span>                = 0x22
    <span id="IP_MULTICAST_TTL">IP_MULTICAST_TTL</span>                 = 0x21
    <span id="IP_OFFMASK">IP_OFFMASK</span>                       = 0x1fff
    <span id="IP_OPTIONS">IP_OPTIONS</span>                       = 0x4
    <span id="IP_ORIGDSTADDR">IP_ORIGDSTADDR</span>                   = 0x14
    <span id="IP_PASSSEC">IP_PASSSEC</span>                       = 0x12
    <span id="IP_PKTINFO">IP_PKTINFO</span>                       = 0x8
    <span id="IP_PKTOPTIONS">IP_PKTOPTIONS</span>                    = 0x9
    <span id="IP_PMTUDISC">IP_PMTUDISC</span>                      = 0xa
    <span id="IP_PMTUDISC_DO">IP_PMTUDISC_DO</span>                   = 0x2
    <span id="IP_PMTUDISC_DONT">IP_PMTUDISC_DONT</span>                 = 0x0
    <span id="IP_PMTUDISC_PROBE">IP_PMTUDISC_PROBE</span>                = 0x3
    <span id="IP_PMTUDISC_WANT">IP_PMTUDISC_WANT</span>                 = 0x1
    <span id="IP_RECVERR">IP_RECVERR</span>                       = 0xb
    <span id="IP_RECVOPTS">IP_RECVOPTS</span>                      = 0x6
    <span id="IP_RECVORIGDSTADDR">IP_RECVORIGDSTADDR</span>               = 0x14
    <span id="IP_RECVRETOPTS">IP_RECVRETOPTS</span>                   = 0x7
    <span id="IP_RECVTOS">IP_RECVTOS</span>                       = 0xd
    <span id="IP_RECVTTL">IP_RECVTTL</span>                       = 0xc
    <span id="IP_RETOPTS">IP_RETOPTS</span>                       = 0x7
    <span id="IP_RF">IP_RF</span>                            = 0x8000
    <span id="IP_ROUTER_ALERT">IP_ROUTER_ALERT</span>                  = 0x5
    <span id="IP_TOS">IP_TOS</span>                           = 0x1
    <span id="IP_TRANSPARENT">IP_TRANSPARENT</span>                   = 0x13
    <span id="IP_TTL">IP_TTL</span>                           = 0x2
    <span id="IP_UNBLOCK_SOURCE">IP_UNBLOCK_SOURCE</span>                = 0x25
    <span id="IP_XFRM_POLICY">IP_XFRM_POLICY</span>                   = 0x11
    <span id="LINUX_REBOOT_CMD_CAD_OFF">LINUX_REBOOT_CMD_CAD_OFF</span>         = 0x0
    <span id="LINUX_REBOOT_CMD_CAD_ON">LINUX_REBOOT_CMD_CAD_ON</span>          = 0x89abcdef
    <span id="LINUX_REBOOT_CMD_HALT">LINUX_REBOOT_CMD_HALT</span>            = 0xcdef0123
    <span id="LINUX_REBOOT_CMD_KEXEC">LINUX_REBOOT_CMD_KEXEC</span>           = 0x45584543
    <span id="LINUX_REBOOT_CMD_POWER_OFF">LINUX_REBOOT_CMD_POWER_OFF</span>       = 0x4321fedc
    <span id="LINUX_REBOOT_CMD_RESTART">LINUX_REBOOT_CMD_RESTART</span>         = 0x1234567
    <span id="LINUX_REBOOT_CMD_RESTART2">LINUX_REBOOT_CMD_RESTART2</span>        = 0xa1b2c3d4
    <span id="LINUX_REBOOT_CMD_SW_SUSPEND">LINUX_REBOOT_CMD_SW_SUSPEND</span>      = 0xd000fce2
    <span id="LINUX_REBOOT_MAGIC1">LINUX_REBOOT_MAGIC1</span>              = 0xfee1dead
    <span id="LINUX_REBOOT_MAGIC2">LINUX_REBOOT_MAGIC2</span>              = 0x28121969
    <span id="LOCK_EX">LOCK_EX</span>                          = 0x2
    <span id="LOCK_NB">LOCK_NB</span>                          = 0x4
    <span id="LOCK_SH">LOCK_SH</span>                          = 0x1
    <span id="LOCK_UN">LOCK_UN</span>                          = 0x8
    <span id="MADV_DOFORK">MADV_DOFORK</span>                      = 0xb
    <span id="MADV_DONTFORK">MADV_DONTFORK</span>                    = 0xa
    <span id="MADV_DONTNEED">MADV_DONTNEED</span>                    = 0x4
    <span id="MADV_HUGEPAGE">MADV_HUGEPAGE</span>                    = 0xe
    <span id="MADV_HWPOISON">MADV_HWPOISON</span>                    = 0x64
    <span id="MADV_MERGEABLE">MADV_MERGEABLE</span>                   = 0xc
    <span id="MADV_NOHUGEPAGE">MADV_NOHUGEPAGE</span>                  = 0xf
    <span id="MADV_NORMAL">MADV_NORMAL</span>                      = 0x0
    <span id="MADV_RANDOM">MADV_RANDOM</span>                      = 0x1
    <span id="MADV_REMOVE">MADV_REMOVE</span>                      = 0x9
    <span id="MADV_SEQUENTIAL">MADV_SEQUENTIAL</span>                  = 0x2
    <span id="MADV_UNMERGEABLE">MADV_UNMERGEABLE</span>                 = 0xd
    <span id="MADV_WILLNEED">MADV_WILLNEED</span>                    = 0x3
    <span id="MAP_32BIT">MAP_32BIT</span>                        = 0x40
    <span id="MAP_ANON">MAP_ANON</span>                         = 0x20
    <span id="MAP_ANONYMOUS">MAP_ANONYMOUS</span>                    = 0x20
    <span id="MAP_DENYWRITE">MAP_DENYWRITE</span>                    = 0x800
    <span id="MAP_EXECUTABLE">MAP_EXECUTABLE</span>                   = 0x1000
    <span id="MAP_FILE">MAP_FILE</span>                         = 0x0
    <span id="MAP_FIXED">MAP_FIXED</span>                        = 0x10
    <span id="MAP_GROWSDOWN">MAP_GROWSDOWN</span>                    = 0x100
    <span id="MAP_HUGETLB">MAP_HUGETLB</span>                      = 0x40000
    <span id="MAP_LOCKED">MAP_LOCKED</span>                       = 0x2000
    <span id="MAP_NONBLOCK">MAP_NONBLOCK</span>                     = 0x10000
    <span id="MAP_NORESERVE">MAP_NORESERVE</span>                    = 0x4000
    <span id="MAP_POPULATE">MAP_POPULATE</span>                     = 0x8000
    <span id="MAP_PRIVATE">MAP_PRIVATE</span>                      = 0x2
    <span id="MAP_SHARED">MAP_SHARED</span>                       = 0x1
    <span id="MAP_STACK">MAP_STACK</span>                        = 0x20000
    <span id="MAP_TYPE">MAP_TYPE</span>                         = 0xf
    <span id="MCL_CURRENT">MCL_CURRENT</span>                      = 0x1
    <span id="MCL_FUTURE">MCL_FUTURE</span>                       = 0x2
    <span id="MNT_DETACH">MNT_DETACH</span>                       = 0x2
    <span id="MNT_EXPIRE">MNT_EXPIRE</span>                       = 0x4
    <span id="MNT_FORCE">MNT_FORCE</span>                        = 0x1
    <span id="MSG_CMSG_CLOEXEC">MSG_CMSG_CLOEXEC</span>                 = 0x40000000
    <span id="MSG_CONFIRM">MSG_CONFIRM</span>                      = 0x800
    <span id="MSG_CTRUNC">MSG_CTRUNC</span>                       = 0x8
    <span id="MSG_DONTROUTE">MSG_DONTROUTE</span>                    = 0x4
    <span id="MSG_DONTWAIT">MSG_DONTWAIT</span>                     = 0x40
    <span id="MSG_EOR">MSG_EOR</span>                          = 0x80
    <span id="MSG_ERRQUEUE">MSG_ERRQUEUE</span>                     = 0x2000
    <span id="MSG_FASTOPEN">MSG_FASTOPEN</span>                     = 0x20000000
    <span id="MSG_FIN">MSG_FIN</span>                          = 0x200
    <span id="MSG_MORE">MSG_MORE</span>                         = 0x8000
    <span id="MSG_NOSIGNAL">MSG_NOSIGNAL</span>                     = 0x4000
    <span id="MSG_OOB">MSG_OOB</span>                          = 0x1
    <span id="MSG_PEEK">MSG_PEEK</span>                         = 0x2
    <span id="MSG_PROXY">MSG_PROXY</span>                        = 0x10
    <span id="MSG_RST">MSG_RST</span>                          = 0x1000
    <span id="MSG_SYN">MSG_SYN</span>                          = 0x400
    <span id="MSG_TRUNC">MSG_TRUNC</span>                        = 0x20
    <span id="MSG_TRYHARD">MSG_TRYHARD</span>                      = 0x4
    <span id="MSG_WAITALL">MSG_WAITALL</span>                      = 0x100
    <span id="MSG_WAITFORONE">MSG_WAITFORONE</span>                   = 0x10000
    <span id="MS_ACTIVE">MS_ACTIVE</span>                        = 0x40000000
    <span id="MS_ASYNC">MS_ASYNC</span>                         = 0x1
    <span id="MS_BIND">MS_BIND</span>                          = 0x1000
    <span id="MS_DIRSYNC">MS_DIRSYNC</span>                       = 0x80
    <span id="MS_INVALIDATE">MS_INVALIDATE</span>                    = 0x2
    <span id="MS_I_VERSION">MS_I_VERSION</span>                     = 0x800000
    <span id="MS_KERNMOUNT">MS_KERNMOUNT</span>                     = 0x400000
    <span id="MS_MANDLOCK">MS_MANDLOCK</span>                      = 0x40
    <span id="MS_MGC_MSK">MS_MGC_MSK</span>                       = 0xffff0000
    <span id="MS_MGC_VAL">MS_MGC_VAL</span>                       = 0xc0ed0000
    <span id="MS_MOVE">MS_MOVE</span>                          = 0x2000
    <span id="MS_NOATIME">MS_NOATIME</span>                       = 0x400
    <span id="MS_NODEV">MS_NODEV</span>                         = 0x4
    <span id="MS_NODIRATIME">MS_NODIRATIME</span>                    = 0x800
    <span id="MS_NOEXEC">MS_NOEXEC</span>                        = 0x8
    <span id="MS_NOSUID">MS_NOSUID</span>                        = 0x2
    <span id="MS_NOUSER">MS_NOUSER</span>                        = -0x80000000
    <span id="MS_POSIXACL">MS_POSIXACL</span>                      = 0x10000
    <span id="MS_PRIVATE">MS_PRIVATE</span>                       = 0x40000
    <span id="MS_RDONLY">MS_RDONLY</span>                        = 0x1
    <span id="MS_REC">MS_REC</span>                           = 0x4000
    <span id="MS_RELATIME">MS_RELATIME</span>                      = 0x200000
    <span id="MS_REMOUNT">MS_REMOUNT</span>                       = 0x20
    <span id="MS_RMT_MASK">MS_RMT_MASK</span>                      = 0x800051
    <span id="MS_SHARED">MS_SHARED</span>                        = 0x100000
    <span id="MS_SILENT">MS_SILENT</span>                        = 0x8000
    <span id="MS_SLAVE">MS_SLAVE</span>                         = 0x80000
    <span id="MS_STRICTATIME">MS_STRICTATIME</span>                   = 0x1000000
    <span id="MS_SYNC">MS_SYNC</span>                          = 0x4
    <span id="MS_SYNCHRONOUS">MS_SYNCHRONOUS</span>                   = 0x10
    <span id="MS_UNBINDABLE">MS_UNBINDABLE</span>                    = 0x20000
    <span id="NAME_MAX">NAME_MAX</span>                         = 0xff
    <span id="NETLINK_ADD_MEMBERSHIP">NETLINK_ADD_MEMBERSHIP</span>           = 0x1
    <span id="NETLINK_AUDIT">NETLINK_AUDIT</span>                    = 0x9
    <span id="NETLINK_BROADCAST_ERROR">NETLINK_BROADCAST_ERROR</span>          = 0x4
    <span id="NETLINK_CONNECTOR">NETLINK_CONNECTOR</span>                = 0xb
    <span id="NETLINK_DNRTMSG">NETLINK_DNRTMSG</span>                  = 0xe
    <span id="NETLINK_DROP_MEMBERSHIP">NETLINK_DROP_MEMBERSHIP</span>          = 0x2
    <span id="NETLINK_ECRYPTFS">NETLINK_ECRYPTFS</span>                 = 0x13
    <span id="NETLINK_FIB_LOOKUP">NETLINK_FIB_LOOKUP</span>               = 0xa
    <span id="NETLINK_FIREWALL">NETLINK_FIREWALL</span>                 = 0x3
    <span id="NETLINK_GENERIC">NETLINK_GENERIC</span>                  = 0x10
    <span id="NETLINK_INET_DIAG">NETLINK_INET_DIAG</span>                = 0x4
    <span id="NETLINK_IP6_FW">NETLINK_IP6_FW</span>                   = 0xd
    <span id="NETLINK_ISCSI">NETLINK_ISCSI</span>                    = 0x8
    <span id="NETLINK_KOBJECT_UEVENT">NETLINK_KOBJECT_UEVENT</span>           = 0xf
    <span id="NETLINK_NETFILTER">NETLINK_NETFILTER</span>                = 0xc
    <span id="NETLINK_NFLOG">NETLINK_NFLOG</span>                    = 0x5
    <span id="NETLINK_NO_ENOBUFS">NETLINK_NO_ENOBUFS</span>               = 0x5
    <span id="NETLINK_PKTINFO">NETLINK_PKTINFO</span>                  = 0x3
    <span id="NETLINK_ROUTE">NETLINK_ROUTE</span>                    = 0x0
    <span id="NETLINK_SCSITRANSPORT">NETLINK_SCSITRANSPORT</span>            = 0x12
    <span id="NETLINK_SELINUX">NETLINK_SELINUX</span>                  = 0x7
    <span id="NETLINK_UNUSED">NETLINK_UNUSED</span>                   = 0x1
    <span id="NETLINK_USERSOCK">NETLINK_USERSOCK</span>                 = 0x2
    <span id="NETLINK_XFRM">NETLINK_XFRM</span>                     = 0x6
    <span id="NLA_ALIGNTO">NLA_ALIGNTO</span>                      = 0x4
    <span id="NLA_F_NESTED">NLA_F_NESTED</span>                     = 0x8000
    <span id="NLA_F_NET_BYTEORDER">NLA_F_NET_BYTEORDER</span>              = 0x4000
    <span id="NLA_HDRLEN">NLA_HDRLEN</span>                       = 0x4
    <span id="NLMSG_ALIGNTO">NLMSG_ALIGNTO</span>                    = 0x4
    <span id="NLMSG_DONE">NLMSG_DONE</span>                       = 0x3
    <span id="NLMSG_ERROR">NLMSG_ERROR</span>                      = 0x2
    <span id="NLMSG_HDRLEN">NLMSG_HDRLEN</span>                     = 0x10
    <span id="NLMSG_MIN_TYPE">NLMSG_MIN_TYPE</span>                   = 0x10
    <span id="NLMSG_NOOP">NLMSG_NOOP</span>                       = 0x1
    <span id="NLMSG_OVERRUN">NLMSG_OVERRUN</span>                    = 0x4
    <span id="NLM_F_ACK">NLM_F_ACK</span>                        = 0x4
    <span id="NLM_F_APPEND">NLM_F_APPEND</span>                     = 0x800
    <span id="NLM_F_ATOMIC">NLM_F_ATOMIC</span>                     = 0x400
    <span id="NLM_F_CREATE">NLM_F_CREATE</span>                     = 0x400
    <span id="NLM_F_DUMP">NLM_F_DUMP</span>                       = 0x300
    <span id="NLM_F_ECHO">NLM_F_ECHO</span>                       = 0x8
    <span id="NLM_F_EXCL">NLM_F_EXCL</span>                       = 0x200
    <span id="NLM_F_MATCH">NLM_F_MATCH</span>                      = 0x200
    <span id="NLM_F_MULTI">NLM_F_MULTI</span>                      = 0x2
    <span id="NLM_F_REPLACE">NLM_F_REPLACE</span>                    = 0x100
    <span id="NLM_F_REQUEST">NLM_F_REQUEST</span>                    = 0x1
    <span id="NLM_F_ROOT">NLM_F_ROOT</span>                       = 0x100
    <span id="O_ACCMODE">O_ACCMODE</span>                        = 0x3
    <span id="O_APPEND">O_APPEND</span>                         = 0x400
    <span id="O_ASYNC">O_ASYNC</span>                          = 0x2000
    <span id="O_CLOEXEC">O_CLOEXEC</span>                        = 0x80000
    <span id="O_CREAT">O_CREAT</span>                          = 0x40
    <span id="O_DIRECT">O_DIRECT</span>                         = 0x4000
    <span id="O_DIRECTORY">O_DIRECTORY</span>                      = 0x10000
    <span id="O_DSYNC">O_DSYNC</span>                          = 0x1000
    <span id="O_EXCL">O_EXCL</span>                           = 0x80
    <span id="O_FSYNC">O_FSYNC</span>                          = 0x101000
    <span id="O_LARGEFILE">O_LARGEFILE</span>                      = 0x0
    <span id="O_NDELAY">O_NDELAY</span>                         = 0x800
    <span id="O_NOATIME">O_NOATIME</span>                        = 0x40000
    <span id="O_NOCTTY">O_NOCTTY</span>                         = 0x100
    <span id="O_NOFOLLOW">O_NOFOLLOW</span>                       = 0x20000
    <span id="O_NONBLOCK">O_NONBLOCK</span>                       = 0x800
    <span id="O_RDONLY">O_RDONLY</span>                         = 0x0
    <span id="O_RDWR">O_RDWR</span>                           = 0x2
    <span id="O_RSYNC">O_RSYNC</span>                          = 0x101000
    <span id="O_SYNC">O_SYNC</span>                           = 0x101000
    <span id="O_TRUNC">O_TRUNC</span>                          = 0x200
    <span id="O_WRONLY">O_WRONLY</span>                         = 0x1
    <span id="PACKET_ADD_MEMBERSHIP">PACKET_ADD_MEMBERSHIP</span>            = 0x1
    <span id="PACKET_BROADCAST">PACKET_BROADCAST</span>                 = 0x1
    <span id="PACKET_DROP_MEMBERSHIP">PACKET_DROP_MEMBERSHIP</span>           = 0x2
    <span id="PACKET_FASTROUTE">PACKET_FASTROUTE</span>                 = 0x6
    <span id="PACKET_HOST">PACKET_HOST</span>                      = 0x0
    <span id="PACKET_LOOPBACK">PACKET_LOOPBACK</span>                  = 0x5
    <span id="PACKET_MR_ALLMULTI">PACKET_MR_ALLMULTI</span>               = 0x2
    <span id="PACKET_MR_MULTICAST">PACKET_MR_MULTICAST</span>              = 0x0
    <span id="PACKET_MR_PROMISC">PACKET_MR_PROMISC</span>                = 0x1
    <span id="PACKET_MULTICAST">PACKET_MULTICAST</span>                 = 0x2
    <span id="PACKET_OTHERHOST">PACKET_OTHERHOST</span>                 = 0x3
    <span id="PACKET_OUTGOING">PACKET_OUTGOING</span>                  = 0x4
    <span id="PACKET_RECV_OUTPUT">PACKET_RECV_OUTPUT</span>               = 0x3
    <span id="PACKET_RX_RING">PACKET_RX_RING</span>                   = 0x5
    <span id="PACKET_STATISTICS">PACKET_STATISTICS</span>                = 0x6
    <span id="PRIO_PGRP">PRIO_PGRP</span>                        = 0x1
    <span id="PRIO_PROCESS">PRIO_PROCESS</span>                     = 0x0
    <span id="PRIO_USER">PRIO_USER</span>                        = 0x2
    <span id="PROT_EXEC">PROT_EXEC</span>                        = 0x4
    <span id="PROT_GROWSDOWN">PROT_GROWSDOWN</span>                   = 0x1000000
    <span id="PROT_GROWSUP">PROT_GROWSUP</span>                     = 0x2000000
    <span id="PROT_NONE">PROT_NONE</span>                        = 0x0
    <span id="PROT_READ">PROT_READ</span>                        = 0x1
    <span id="PROT_WRITE">PROT_WRITE</span>                       = 0x2
    <span id="PR_CAPBSET_DROP">PR_CAPBSET_DROP</span>                  = 0x18
    <span id="PR_CAPBSET_READ">PR_CAPBSET_READ</span>                  = 0x17
    <span id="PR_ENDIAN_BIG">PR_ENDIAN_BIG</span>                    = 0x0
    <span id="PR_ENDIAN_LITTLE">PR_ENDIAN_LITTLE</span>                 = 0x1
    <span id="PR_ENDIAN_PPC_LITTLE">PR_ENDIAN_PPC_LITTLE</span>             = 0x2
    <span id="PR_FPEMU_NOPRINT">PR_FPEMU_NOPRINT</span>                 = 0x1
    <span id="PR_FPEMU_SIGFPE">PR_FPEMU_SIGFPE</span>                  = 0x2
    <span id="PR_FP_EXC_ASYNC">PR_FP_EXC_ASYNC</span>                  = 0x2
    <span id="PR_FP_EXC_DISABLED">PR_FP_EXC_DISABLED</span>               = 0x0
    <span id="PR_FP_EXC_DIV">PR_FP_EXC_DIV</span>                    = 0x10000
    <span id="PR_FP_EXC_INV">PR_FP_EXC_INV</span>                    = 0x100000
    <span id="PR_FP_EXC_NONRECOV">PR_FP_EXC_NONRECOV</span>               = 0x1
    <span id="PR_FP_EXC_OVF">PR_FP_EXC_OVF</span>                    = 0x20000
    <span id="PR_FP_EXC_PRECISE">PR_FP_EXC_PRECISE</span>                = 0x3
    <span id="PR_FP_EXC_RES">PR_FP_EXC_RES</span>                    = 0x80000
    <span id="PR_FP_EXC_SW_ENABLE">PR_FP_EXC_SW_ENABLE</span>              = 0x80
    <span id="PR_FP_EXC_UND">PR_FP_EXC_UND</span>                    = 0x40000
    <span id="PR_GET_DUMPABLE">PR_GET_DUMPABLE</span>                  = 0x3
    <span id="PR_GET_ENDIAN">PR_GET_ENDIAN</span>                    = 0x13
    <span id="PR_GET_FPEMU">PR_GET_FPEMU</span>                     = 0x9
    <span id="PR_GET_FPEXC">PR_GET_FPEXC</span>                     = 0xb
    <span id="PR_GET_KEEPCAPS">PR_GET_KEEPCAPS</span>                  = 0x7
    <span id="PR_GET_NAME">PR_GET_NAME</span>                      = 0x10
    <span id="PR_GET_PDEATHSIG">PR_GET_PDEATHSIG</span>                 = 0x2
    <span id="PR_GET_SECCOMP">PR_GET_SECCOMP</span>                   = 0x15
    <span id="PR_GET_SECUREBITS">PR_GET_SECUREBITS</span>                = 0x1b
    <span id="PR_GET_TIMERSLACK">PR_GET_TIMERSLACK</span>                = 0x1e
    <span id="PR_GET_TIMING">PR_GET_TIMING</span>                    = 0xd
    <span id="PR_GET_TSC">PR_GET_TSC</span>                       = 0x19
    <span id="PR_GET_UNALIGN">PR_GET_UNALIGN</span>                   = 0x5
    <span id="PR_MCE_KILL">PR_MCE_KILL</span>                      = 0x21
    <span id="PR_MCE_KILL_CLEAR">PR_MCE_KILL_CLEAR</span>                = 0x0
    <span id="PR_MCE_KILL_DEFAULT">PR_MCE_KILL_DEFAULT</span>              = 0x2
    <span id="PR_MCE_KILL_EARLY">PR_MCE_KILL_EARLY</span>                = 0x1
    <span id="PR_MCE_KILL_GET">PR_MCE_KILL_GET</span>                  = 0x22
    <span id="PR_MCE_KILL_LATE">PR_MCE_KILL_LATE</span>                 = 0x0
    <span id="PR_MCE_KILL_SET">PR_MCE_KILL_SET</span>                  = 0x1
    <span id="PR_SET_DUMPABLE">PR_SET_DUMPABLE</span>                  = 0x4
    <span id="PR_SET_ENDIAN">PR_SET_ENDIAN</span>                    = 0x14
    <span id="PR_SET_FPEMU">PR_SET_FPEMU</span>                     = 0xa
    <span id="PR_SET_FPEXC">PR_SET_FPEXC</span>                     = 0xc
    <span id="PR_SET_KEEPCAPS">PR_SET_KEEPCAPS</span>                  = 0x8
    <span id="PR_SET_NAME">PR_SET_NAME</span>                      = 0xf
    <span id="PR_SET_PDEATHSIG">PR_SET_PDEATHSIG</span>                 = 0x1
    <span id="PR_SET_PTRACER">PR_SET_PTRACER</span>                   = 0x59616d61
    <span id="PR_SET_SECCOMP">PR_SET_SECCOMP</span>                   = 0x16
    <span id="PR_SET_SECUREBITS">PR_SET_SECUREBITS</span>                = 0x1c
    <span id="PR_SET_TIMERSLACK">PR_SET_TIMERSLACK</span>                = 0x1d
    <span id="PR_SET_TIMING">PR_SET_TIMING</span>                    = 0xe
    <span id="PR_SET_TSC">PR_SET_TSC</span>                       = 0x1a
    <span id="PR_SET_UNALIGN">PR_SET_UNALIGN</span>                   = 0x6
    <span id="PR_TASK_PERF_EVENTS_DISABLE">PR_TASK_PERF_EVENTS_DISABLE</span>      = 0x1f
    <span id="PR_TASK_PERF_EVENTS_ENABLE">PR_TASK_PERF_EVENTS_ENABLE</span>       = 0x20
    <span id="PR_TIMING_STATISTICAL">PR_TIMING_STATISTICAL</span>            = 0x0
    <span id="PR_TIMING_TIMESTAMP">PR_TIMING_TIMESTAMP</span>              = 0x1
    <span id="PR_TSC_ENABLE">PR_TSC_ENABLE</span>                    = 0x1
    <span id="PR_TSC_SIGSEGV">PR_TSC_SIGSEGV</span>                   = 0x2
    <span id="PR_UNALIGN_NOPRINT">PR_UNALIGN_NOPRINT</span>               = 0x1
    <span id="PR_UNALIGN_SIGBUS">PR_UNALIGN_SIGBUS</span>                = 0x2
    <span id="PTRACE_ARCH_PRCTL">PTRACE_ARCH_PRCTL</span>                = 0x1e
    <span id="PTRACE_ATTACH">PTRACE_ATTACH</span>                    = 0x10
    <span id="PTRACE_CONT">PTRACE_CONT</span>                      = 0x7
    <span id="PTRACE_DETACH">PTRACE_DETACH</span>                    = 0x11
    <span id="PTRACE_EVENT_CLONE">PTRACE_EVENT_CLONE</span>               = 0x3
    <span id="PTRACE_EVENT_EXEC">PTRACE_EVENT_EXEC</span>                = 0x4
    <span id="PTRACE_EVENT_EXIT">PTRACE_EVENT_EXIT</span>                = 0x6
    <span id="PTRACE_EVENT_FORK">PTRACE_EVENT_FORK</span>                = 0x1
    <span id="PTRACE_EVENT_VFORK">PTRACE_EVENT_VFORK</span>               = 0x2
    <span id="PTRACE_EVENT_VFORK_DONE">PTRACE_EVENT_VFORK_DONE</span>          = 0x5
    <span id="PTRACE_GETEVENTMSG">PTRACE_GETEVENTMSG</span>               = 0x4201
    <span id="PTRACE_GETFPREGS">PTRACE_GETFPREGS</span>                 = 0xe
    <span id="PTRACE_GETFPXREGS">PTRACE_GETFPXREGS</span>                = 0x12
    <span id="PTRACE_GETREGS">PTRACE_GETREGS</span>                   = 0xc
    <span id="PTRACE_GETREGSET">PTRACE_GETREGSET</span>                 = 0x4204
    <span id="PTRACE_GETSIGINFO">PTRACE_GETSIGINFO</span>                = 0x4202
    <span id="PTRACE_GET_THREAD_AREA">PTRACE_GET_THREAD_AREA</span>           = 0x19
    <span id="PTRACE_KILL">PTRACE_KILL</span>                      = 0x8
    <span id="PTRACE_OLDSETOPTIONS">PTRACE_OLDSETOPTIONS</span>             = 0x15
    <span id="PTRACE_O_MASK">PTRACE_O_MASK</span>                    = 0x7f
    <span id="PTRACE_O_TRACECLONE">PTRACE_O_TRACECLONE</span>              = 0x8
    <span id="PTRACE_O_TRACEEXEC">PTRACE_O_TRACEEXEC</span>               = 0x10
    <span id="PTRACE_O_TRACEEXIT">PTRACE_O_TRACEEXIT</span>               = 0x40
    <span id="PTRACE_O_TRACEFORK">PTRACE_O_TRACEFORK</span>               = 0x2
    <span id="PTRACE_O_TRACESYSGOOD">PTRACE_O_TRACESYSGOOD</span>            = 0x1
    <span id="PTRACE_O_TRACEVFORK">PTRACE_O_TRACEVFORK</span>              = 0x4
    <span id="PTRACE_O_TRACEVFORKDONE">PTRACE_O_TRACEVFORKDONE</span>          = 0x20
    <span id="PTRACE_PEEKDATA">PTRACE_PEEKDATA</span>                  = 0x2
    <span id="PTRACE_PEEKTEXT">PTRACE_PEEKTEXT</span>                  = 0x1
    <span id="PTRACE_PEEKUSR">PTRACE_PEEKUSR</span>                   = 0x3
    <span id="PTRACE_POKEDATA">PTRACE_POKEDATA</span>                  = 0x5
    <span id="PTRACE_POKETEXT">PTRACE_POKETEXT</span>                  = 0x4
    <span id="PTRACE_POKEUSR">PTRACE_POKEUSR</span>                   = 0x6
    <span id="PTRACE_SETFPREGS">PTRACE_SETFPREGS</span>                 = 0xf
    <span id="PTRACE_SETFPXREGS">PTRACE_SETFPXREGS</span>                = 0x13
    <span id="PTRACE_SETOPTIONS">PTRACE_SETOPTIONS</span>                = 0x4200
    <span id="PTRACE_SETREGS">PTRACE_SETREGS</span>                   = 0xd
    <span id="PTRACE_SETREGSET">PTRACE_SETREGSET</span>                 = 0x4205
    <span id="PTRACE_SETSIGINFO">PTRACE_SETSIGINFO</span>                = 0x4203
    <span id="PTRACE_SET_THREAD_AREA">PTRACE_SET_THREAD_AREA</span>           = 0x1a
    <span id="PTRACE_SINGLEBLOCK">PTRACE_SINGLEBLOCK</span>               = 0x21
    <span id="PTRACE_SINGLESTEP">PTRACE_SINGLESTEP</span>                = 0x9
    <span id="PTRACE_SYSCALL">PTRACE_SYSCALL</span>                   = 0x18
    <span id="PTRACE_SYSEMU">PTRACE_SYSEMU</span>                    = 0x1f
    <span id="PTRACE_SYSEMU_SINGLESTEP">PTRACE_SYSEMU_SINGLESTEP</span>         = 0x20
    <span id="PTRACE_TRACEME">PTRACE_TRACEME</span>                   = 0x0
    <span id="RLIMIT_AS">RLIMIT_AS</span>                        = 0x9
    <span id="RLIMIT_CORE">RLIMIT_CORE</span>                      = 0x4
    <span id="RLIMIT_CPU">RLIMIT_CPU</span>                       = 0x0
    <span id="RLIMIT_DATA">RLIMIT_DATA</span>                      = 0x2
    <span id="RLIMIT_FSIZE">RLIMIT_FSIZE</span>                     = 0x1
    <span id="RLIMIT_NOFILE">RLIMIT_NOFILE</span>                    = 0x7
    <span id="RLIMIT_STACK">RLIMIT_STACK</span>                     = 0x3
    <span id="RLIM_INFINITY">RLIM_INFINITY</span>                    = -0x1
    <span id="RTAX_ADVMSS">RTAX_ADVMSS</span>                      = 0x8
    <span id="RTAX_CWND">RTAX_CWND</span>                        = 0x7
    <span id="RTAX_FEATURES">RTAX_FEATURES</span>                    = 0xc
    <span id="RTAX_FEATURE_ALLFRAG">RTAX_FEATURE_ALLFRAG</span>             = 0x8
    <span id="RTAX_FEATURE_ECN">RTAX_FEATURE_ECN</span>                 = 0x1
    <span id="RTAX_FEATURE_SACK">RTAX_FEATURE_SACK</span>                = 0x2
    <span id="RTAX_FEATURE_TIMESTAMP">RTAX_FEATURE_TIMESTAMP</span>           = 0x4
    <span id="RTAX_HOPLIMIT">RTAX_HOPLIMIT</span>                    = 0xa
    <span id="RTAX_INITCWND">RTAX_INITCWND</span>                    = 0xb
    <span id="RTAX_INITRWND">RTAX_INITRWND</span>                    = 0xe
    <span id="RTAX_LOCK">RTAX_LOCK</span>                        = 0x1
    <span id="RTAX_MAX">RTAX_MAX</span>                         = 0xe
    <span id="RTAX_MTU">RTAX_MTU</span>                         = 0x2
    <span id="RTAX_REORDERING">RTAX_REORDERING</span>                  = 0x9
    <span id="RTAX_RTO_MIN">RTAX_RTO_MIN</span>                     = 0xd
    <span id="RTAX_RTT">RTAX_RTT</span>                         = 0x4
    <span id="RTAX_RTTVAR">RTAX_RTTVAR</span>                      = 0x5
    <span id="RTAX_SSTHRESH">RTAX_SSTHRESH</span>                    = 0x6
    <span id="RTAX_UNSPEC">RTAX_UNSPEC</span>                      = 0x0
    <span id="RTAX_WINDOW">RTAX_WINDOW</span>                      = 0x3
    <span id="RTA_ALIGNTO">RTA_ALIGNTO</span>                      = 0x4
    <span id="RTA_MAX">RTA_MAX</span>                          = 0x10
    <span id="RTCF_DIRECTSRC">RTCF_DIRECTSRC</span>                   = 0x4000000
    <span id="RTCF_DOREDIRECT">RTCF_DOREDIRECT</span>                  = 0x1000000
    <span id="RTCF_LOG">RTCF_LOG</span>                         = 0x2000000
    <span id="RTCF_MASQ">RTCF_MASQ</span>                        = 0x400000
    <span id="RTCF_NAT">RTCF_NAT</span>                         = 0x800000
    <span id="RTCF_VALVE">RTCF_VALVE</span>                       = 0x200000
    <span id="RTF_ADDRCLASSMASK">RTF_ADDRCLASSMASK</span>                = 0xf8000000
    <span id="RTF_ADDRCONF">RTF_ADDRCONF</span>                     = 0x40000
    <span id="RTF_ALLONLINK">RTF_ALLONLINK</span>                    = 0x20000
    <span id="RTF_BROADCAST">RTF_BROADCAST</span>                    = 0x10000000
    <span id="RTF_CACHE">RTF_CACHE</span>                        = 0x1000000
    <span id="RTF_DEFAULT">RTF_DEFAULT</span>                      = 0x10000
    <span id="RTF_DYNAMIC">RTF_DYNAMIC</span>                      = 0x10
    <span id="RTF_FLOW">RTF_FLOW</span>                         = 0x2000000
    <span id="RTF_GATEWAY">RTF_GATEWAY</span>                      = 0x2
    <span id="RTF_HOST">RTF_HOST</span>                         = 0x4
    <span id="RTF_INTERFACE">RTF_INTERFACE</span>                    = 0x40000000
    <span id="RTF_IRTT">RTF_IRTT</span>                         = 0x100
    <span id="RTF_LINKRT">RTF_LINKRT</span>                       = 0x100000
    <span id="RTF_LOCAL">RTF_LOCAL</span>                        = 0x80000000
    <span id="RTF_MODIFIED">RTF_MODIFIED</span>                     = 0x20
    <span id="RTF_MSS">RTF_MSS</span>                          = 0x40
    <span id="RTF_MTU">RTF_MTU</span>                          = 0x40
    <span id="RTF_MULTICAST">RTF_MULTICAST</span>                    = 0x20000000
    <span id="RTF_NAT">RTF_NAT</span>                          = 0x8000000
    <span id="RTF_NOFORWARD">RTF_NOFORWARD</span>                    = 0x1000
    <span id="RTF_NONEXTHOP">RTF_NONEXTHOP</span>                    = 0x200000
    <span id="RTF_NOPMTUDISC">RTF_NOPMTUDISC</span>                   = 0x4000
    <span id="RTF_POLICY">RTF_POLICY</span>                       = 0x4000000
    <span id="RTF_REINSTATE">RTF_REINSTATE</span>                    = 0x8
    <span id="RTF_REJECT">RTF_REJECT</span>                       = 0x200
    <span id="RTF_STATIC">RTF_STATIC</span>                       = 0x400
    <span id="RTF_THROW">RTF_THROW</span>                        = 0x2000
    <span id="RTF_UP">RTF_UP</span>                           = 0x1
    <span id="RTF_WINDOW">RTF_WINDOW</span>                       = 0x80
    <span id="RTF_XRESOLVE">RTF_XRESOLVE</span>                     = 0x800
    <span id="RTM_BASE">RTM_BASE</span>                         = 0x10
    <span id="RTM_DELACTION">RTM_DELACTION</span>                    = 0x31
    <span id="RTM_DELADDR">RTM_DELADDR</span>                      = 0x15
    <span id="RTM_DELADDRLABEL">RTM_DELADDRLABEL</span>                 = 0x49
    <span id="RTM_DELLINK">RTM_DELLINK</span>                      = 0x11
    <span id="RTM_DELNEIGH">RTM_DELNEIGH</span>                     = 0x1d
    <span id="RTM_DELQDISC">RTM_DELQDISC</span>                     = 0x25
    <span id="RTM_DELROUTE">RTM_DELROUTE</span>                     = 0x19
    <span id="RTM_DELRULE">RTM_DELRULE</span>                      = 0x21
    <span id="RTM_DELTCLASS">RTM_DELTCLASS</span>                    = 0x29
    <span id="RTM_DELTFILTER">RTM_DELTFILTER</span>                   = 0x2d
    <span id="RTM_F_CLONED">RTM_F_CLONED</span>                     = 0x200
    <span id="RTM_F_EQUALIZE">RTM_F_EQUALIZE</span>                   = 0x400
    <span id="RTM_F_NOTIFY">RTM_F_NOTIFY</span>                     = 0x100
    <span id="RTM_F_PREFIX">RTM_F_PREFIX</span>                     = 0x800
    <span id="RTM_GETACTION">RTM_GETACTION</span>                    = 0x32
    <span id="RTM_GETADDR">RTM_GETADDR</span>                      = 0x16
    <span id="RTM_GETADDRLABEL">RTM_GETADDRLABEL</span>                 = 0x4a
    <span id="RTM_GETANYCAST">RTM_GETANYCAST</span>                   = 0x3e
    <span id="RTM_GETDCB">RTM_GETDCB</span>                       = 0x4e
    <span id="RTM_GETLINK">RTM_GETLINK</span>                      = 0x12
    <span id="RTM_GETMULTICAST">RTM_GETMULTICAST</span>                 = 0x3a
    <span id="RTM_GETNEIGH">RTM_GETNEIGH</span>                     = 0x1e
    <span id="RTM_GETNEIGHTBL">RTM_GETNEIGHTBL</span>                  = 0x42
    <span id="RTM_GETQDISC">RTM_GETQDISC</span>                     = 0x26
    <span id="RTM_GETROUTE">RTM_GETROUTE</span>                     = 0x1a
    <span id="RTM_GETRULE">RTM_GETRULE</span>                      = 0x22
    <span id="RTM_GETTCLASS">RTM_GETTCLASS</span>                    = 0x2a
    <span id="RTM_GETTFILTER">RTM_GETTFILTER</span>                   = 0x2e
    <span id="RTM_MAX">RTM_MAX</span>                          = 0x4f
    <span id="RTM_NEWACTION">RTM_NEWACTION</span>                    = 0x30
    <span id="RTM_NEWADDR">RTM_NEWADDR</span>                      = 0x14
    <span id="RTM_NEWADDRLABEL">RTM_NEWADDRLABEL</span>                 = 0x48
    <span id="RTM_NEWLINK">RTM_NEWLINK</span>                      = 0x10
    <span id="RTM_NEWNDUSEROPT">RTM_NEWNDUSEROPT</span>                 = 0x44
    <span id="RTM_NEWNEIGH">RTM_NEWNEIGH</span>                     = 0x1c
    <span id="RTM_NEWNEIGHTBL">RTM_NEWNEIGHTBL</span>                  = 0x40
    <span id="RTM_NEWPREFIX">RTM_NEWPREFIX</span>                    = 0x34
    <span id="RTM_NEWQDISC">RTM_NEWQDISC</span>                     = 0x24
    <span id="RTM_NEWROUTE">RTM_NEWROUTE</span>                     = 0x18
    <span id="RTM_NEWRULE">RTM_NEWRULE</span>                      = 0x20
    <span id="RTM_NEWTCLASS">RTM_NEWTCLASS</span>                    = 0x28
    <span id="RTM_NEWTFILTER">RTM_NEWTFILTER</span>                   = 0x2c
    <span id="RTM_NR_FAMILIES">RTM_NR_FAMILIES</span>                  = 0x10
    <span id="RTM_NR_MSGTYPES">RTM_NR_MSGTYPES</span>                  = 0x40
    <span id="RTM_SETDCB">RTM_SETDCB</span>                       = 0x4f
    <span id="RTM_SETLINK">RTM_SETLINK</span>                      = 0x13
    <span id="RTM_SETNEIGHTBL">RTM_SETNEIGHTBL</span>                  = 0x43
    <span id="RTNH_ALIGNTO">RTNH_ALIGNTO</span>                     = 0x4
    <span id="RTNH_F_DEAD">RTNH_F_DEAD</span>                      = 0x1
    <span id="RTNH_F_ONLINK">RTNH_F_ONLINK</span>                    = 0x4
    <span id="RTNH_F_PERVASIVE">RTNH_F_PERVASIVE</span>                 = 0x2
    <span id="RTN_MAX">RTN_MAX</span>                          = 0xb
    <span id="RTPROT_BIRD">RTPROT_BIRD</span>                      = 0xc
    <span id="RTPROT_BOOT">RTPROT_BOOT</span>                      = 0x3
    <span id="RTPROT_DHCP">RTPROT_DHCP</span>                      = 0x10
    <span id="RTPROT_DNROUTED">RTPROT_DNROUTED</span>                  = 0xd
    <span id="RTPROT_GATED">RTPROT_GATED</span>                     = 0x8
    <span id="RTPROT_KERNEL">RTPROT_KERNEL</span>                    = 0x2
    <span id="RTPROT_MRT">RTPROT_MRT</span>                       = 0xa
    <span id="RTPROT_NTK">RTPROT_NTK</span>                       = 0xf
    <span id="RTPROT_RA">RTPROT_RA</span>                        = 0x9
    <span id="RTPROT_REDIRECT">RTPROT_REDIRECT</span>                  = 0x1
    <span id="RTPROT_STATIC">RTPROT_STATIC</span>                    = 0x4
    <span id="RTPROT_UNSPEC">RTPROT_UNSPEC</span>                    = 0x0
    <span id="RTPROT_XORP">RTPROT_XORP</span>                      = 0xe
    <span id="RTPROT_ZEBRA">RTPROT_ZEBRA</span>                     = 0xb
    <span id="RT_CLASS_DEFAULT">RT_CLASS_DEFAULT</span>                 = 0xfd
    <span id="RT_CLASS_LOCAL">RT_CLASS_LOCAL</span>                   = 0xff
    <span id="RT_CLASS_MAIN">RT_CLASS_MAIN</span>                    = 0xfe
    <span id="RT_CLASS_MAX">RT_CLASS_MAX</span>                     = 0xff
    <span id="RT_CLASS_UNSPEC">RT_CLASS_UNSPEC</span>                  = 0x0
    <span id="RUSAGE_CHILDREN">RUSAGE_CHILDREN</span>                  = -0x1
    <span id="RUSAGE_SELF">RUSAGE_SELF</span>                      = 0x0
    <span id="RUSAGE_THREAD">RUSAGE_THREAD</span>                    = 0x1
    <span id="SCM_CREDENTIALS">SCM_CREDENTIALS</span>                  = 0x2
    <span id="SCM_RIGHTS">SCM_RIGHTS</span>                       = 0x1
    <span id="SCM_TIMESTAMP">SCM_TIMESTAMP</span>                    = 0x1d
    <span id="SCM_TIMESTAMPING">SCM_TIMESTAMPING</span>                 = 0x25
    <span id="SCM_TIMESTAMPNS">SCM_TIMESTAMPNS</span>                  = 0x23
    <span id="SHUT_RD">SHUT_RD</span>                          = 0x0
    <span id="SHUT_RDWR">SHUT_RDWR</span>                        = 0x2
    <span id="SHUT_WR">SHUT_WR</span>                          = 0x1
    <span id="SIOCADDDLCI">SIOCADDDLCI</span>                      = 0x8980
    <span id="SIOCADDMULTI">SIOCADDMULTI</span>                     = 0x8931
    <span id="SIOCADDRT">SIOCADDRT</span>                        = 0x890b
    <span id="SIOCATMARK">SIOCATMARK</span>                       = 0x8905
    <span id="SIOCDARP">SIOCDARP</span>                         = 0x8953
    <span id="SIOCDELDLCI">SIOCDELDLCI</span>                      = 0x8981
    <span id="SIOCDELMULTI">SIOCDELMULTI</span>                     = 0x8932
    <span id="SIOCDELRT">SIOCDELRT</span>                        = 0x890c
    <span id="SIOCDEVPRIVATE">SIOCDEVPRIVATE</span>                   = 0x89f0
    <span id="SIOCDIFADDR">SIOCDIFADDR</span>                      = 0x8936
    <span id="SIOCDRARP">SIOCDRARP</span>                        = 0x8960
    <span id="SIOCGARP">SIOCGARP</span>                         = 0x8954
    <span id="SIOCGIFADDR">SIOCGIFADDR</span>                      = 0x8915
    <span id="SIOCGIFBR">SIOCGIFBR</span>                        = 0x8940
    <span id="SIOCGIFBRDADDR">SIOCGIFBRDADDR</span>                   = 0x8919
    <span id="SIOCGIFCONF">SIOCGIFCONF</span>                      = 0x8912
    <span id="SIOCGIFCOUNT">SIOCGIFCOUNT</span>                     = 0x8938
    <span id="SIOCGIFDSTADDR">SIOCGIFDSTADDR</span>                   = 0x8917
    <span id="SIOCGIFENCAP">SIOCGIFENCAP</span>                     = 0x8925
    <span id="SIOCGIFFLAGS">SIOCGIFFLAGS</span>                     = 0x8913
    <span id="SIOCGIFHWADDR">SIOCGIFHWADDR</span>                    = 0x8927
    <span id="SIOCGIFINDEX">SIOCGIFINDEX</span>                     = 0x8933
    <span id="SIOCGIFMAP">SIOCGIFMAP</span>                       = 0x8970
    <span id="SIOCGIFMEM">SIOCGIFMEM</span>                       = 0x891f
    <span id="SIOCGIFMETRIC">SIOCGIFMETRIC</span>                    = 0x891d
    <span id="SIOCGIFMTU">SIOCGIFMTU</span>                       = 0x8921
    <span id="SIOCGIFNAME">SIOCGIFNAME</span>                      = 0x8910
    <span id="SIOCGIFNETMASK">SIOCGIFNETMASK</span>                   = 0x891b
    <span id="SIOCGIFPFLAGS">SIOCGIFPFLAGS</span>                    = 0x8935
    <span id="SIOCGIFSLAVE">SIOCGIFSLAVE</span>                     = 0x8929
    <span id="SIOCGIFTXQLEN">SIOCGIFTXQLEN</span>                    = 0x8942
    <span id="SIOCGPGRP">SIOCGPGRP</span>                        = 0x8904
    <span id="SIOCGRARP">SIOCGRARP</span>                        = 0x8961
    <span id="SIOCGSTAMP">SIOCGSTAMP</span>                       = 0x8906
    <span id="SIOCGSTAMPNS">SIOCGSTAMPNS</span>                     = 0x8907
    <span id="SIOCPROTOPRIVATE">SIOCPROTOPRIVATE</span>                 = 0x89e0
    <span id="SIOCRTMSG">SIOCRTMSG</span>                        = 0x890d
    <span id="SIOCSARP">SIOCSARP</span>                         = 0x8955
    <span id="SIOCSIFADDR">SIOCSIFADDR</span>                      = 0x8916
    <span id="SIOCSIFBR">SIOCSIFBR</span>                        = 0x8941
    <span id="SIOCSIFBRDADDR">SIOCSIFBRDADDR</span>                   = 0x891a
    <span id="SIOCSIFDSTADDR">SIOCSIFDSTADDR</span>                   = 0x8918
    <span id="SIOCSIFENCAP">SIOCSIFENCAP</span>                     = 0x8926
    <span id="SIOCSIFFLAGS">SIOCSIFFLAGS</span>                     = 0x8914
    <span id="SIOCSIFHWADDR">SIOCSIFHWADDR</span>                    = 0x8924
    <span id="SIOCSIFHWBROADCAST">SIOCSIFHWBROADCAST</span>               = 0x8937
    <span id="SIOCSIFLINK">SIOCSIFLINK</span>                      = 0x8911
    <span id="SIOCSIFMAP">SIOCSIFMAP</span>                       = 0x8971
    <span id="SIOCSIFMEM">SIOCSIFMEM</span>                       = 0x8920
    <span id="SIOCSIFMETRIC">SIOCSIFMETRIC</span>                    = 0x891e
    <span id="SIOCSIFMTU">SIOCSIFMTU</span>                       = 0x8922
    <span id="SIOCSIFNAME">SIOCSIFNAME</span>                      = 0x8923
    <span id="SIOCSIFNETMASK">SIOCSIFNETMASK</span>                   = 0x891c
    <span id="SIOCSIFPFLAGS">SIOCSIFPFLAGS</span>                    = 0x8934
    <span id="SIOCSIFSLAVE">SIOCSIFSLAVE</span>                     = 0x8930
    <span id="SIOCSIFTXQLEN">SIOCSIFTXQLEN</span>                    = 0x8943
    <span id="SIOCSPGRP">SIOCSPGRP</span>                        = 0x8902
    <span id="SIOCSRARP">SIOCSRARP</span>                        = 0x8962
    <span id="SOCK_CLOEXEC">SOCK_CLOEXEC</span>                     = 0x80000
    <span id="SOCK_DCCP">SOCK_DCCP</span>                        = 0x6
    <span id="SOCK_DGRAM">SOCK_DGRAM</span>                       = 0x2
    <span id="SOCK_NONBLOCK">SOCK_NONBLOCK</span>                    = 0x800
    <span id="SOCK_PACKET">SOCK_PACKET</span>                      = 0xa
    <span id="SOCK_RAW">SOCK_RAW</span>                         = 0x3
    <span id="SOCK_RDM">SOCK_RDM</span>                         = 0x4
    <span id="SOCK_SEQPACKET">SOCK_SEQPACKET</span>                   = 0x5
    <span id="SOCK_STREAM">SOCK_STREAM</span>                      = 0x1
    <span id="SOL_AAL">SOL_AAL</span>                          = 0x109
    <span id="SOL_ATM">SOL_ATM</span>                          = 0x108
    <span id="SOL_DECNET">SOL_DECNET</span>                       = 0x105
    <span id="SOL_ICMPV6">SOL_ICMPV6</span>                       = 0x3a
    <span id="SOL_IP">SOL_IP</span>                           = 0x0
    <span id="SOL_IPV6">SOL_IPV6</span>                         = 0x29
    <span id="SOL_IRDA">SOL_IRDA</span>                         = 0x10a
    <span id="SOL_PACKET">SOL_PACKET</span>                       = 0x107
    <span id="SOL_RAW">SOL_RAW</span>                          = 0xff
    <span id="SOL_SOCKET">SOL_SOCKET</span>                       = 0x1
    <span id="SOL_TCP">SOL_TCP</span>                          = 0x6
    <span id="SOL_X25">SOL_X25</span>                          = 0x106
    <span id="SOMAXCONN">SOMAXCONN</span>                        = 0x80
    <span id="SO_ACCEPTCONN">SO_ACCEPTCONN</span>                    = 0x1e
    <span id="SO_ATTACH_FILTER">SO_ATTACH_FILTER</span>                 = 0x1a
    <span id="SO_BINDTODEVICE">SO_BINDTODEVICE</span>                  = 0x19
    <span id="SO_BROADCAST">SO_BROADCAST</span>                     = 0x6
    <span id="SO_BSDCOMPAT">SO_BSDCOMPAT</span>                     = 0xe
    <span id="SO_DEBUG">SO_DEBUG</span>                         = 0x1
    <span id="SO_DETACH_FILTER">SO_DETACH_FILTER</span>                 = 0x1b
    <span id="SO_DOMAIN">SO_DOMAIN</span>                        = 0x27
    <span id="SO_DONTROUTE">SO_DONTROUTE</span>                     = 0x5
    <span id="SO_ERROR">SO_ERROR</span>                         = 0x4
    <span id="SO_KEEPALIVE">SO_KEEPALIVE</span>                     = 0x9
    <span id="SO_LINGER">SO_LINGER</span>                        = 0xd
    <span id="SO_MARK">SO_MARK</span>                          = 0x24
    <span id="SO_NO_CHECK">SO_NO_CHECK</span>                      = 0xb
    <span id="SO_OOBINLINE">SO_OOBINLINE</span>                     = 0xa
    <span id="SO_PASSCRED">SO_PASSCRED</span>                      = 0x10
    <span id="SO_PASSSEC">SO_PASSSEC</span>                       = 0x22
    <span id="SO_PEERCRED">SO_PEERCRED</span>                      = 0x11
    <span id="SO_PEERNAME">SO_PEERNAME</span>                      = 0x1c
    <span id="SO_PEERSEC">SO_PEERSEC</span>                       = 0x1f
    <span id="SO_PRIORITY">SO_PRIORITY</span>                      = 0xc
    <span id="SO_PROTOCOL">SO_PROTOCOL</span>                      = 0x26
    <span id="SO_RCVBUF">SO_RCVBUF</span>                        = 0x8
    <span id="SO_RCVBUFFORCE">SO_RCVBUFFORCE</span>                   = 0x21
    <span id="SO_RCVLOWAT">SO_RCVLOWAT</span>                      = 0x12
    <span id="SO_RCVTIMEO">SO_RCVTIMEO</span>                      = 0x14
    <span id="SO_REUSEADDR">SO_REUSEADDR</span>                     = 0x2
    <span id="SO_RXQ_OVFL">SO_RXQ_OVFL</span>                      = 0x28
    <span id="SO_SECURITY_AUTHENTICATION">SO_SECURITY_AUTHENTICATION</span>       = 0x16
    <span id="SO_SECURITY_ENCRYPTION_NETWORK">SO_SECURITY_ENCRYPTION_NETWORK</span>   = 0x18
    <span id="SO_SECURITY_ENCRYPTION_TRANSPORT">SO_SECURITY_ENCRYPTION_TRANSPORT</span> = 0x17
    <span id="SO_SNDBUF">SO_SNDBUF</span>                        = 0x7
    <span id="SO_SNDBUFFORCE">SO_SNDBUFFORCE</span>                   = 0x20
    <span id="SO_SNDLOWAT">SO_SNDLOWAT</span>                      = 0x13
    <span id="SO_SNDTIMEO">SO_SNDTIMEO</span>                      = 0x15
    <span id="SO_TIMESTAMP">SO_TIMESTAMP</span>                     = 0x1d
    <span id="SO_TIMESTAMPING">SO_TIMESTAMPING</span>                  = 0x25
    <span id="SO_TIMESTAMPNS">SO_TIMESTAMPNS</span>                   = 0x23
    <span id="SO_TYPE">SO_TYPE</span>                          = 0x3
    <span id="S_BLKSIZE">S_BLKSIZE</span>                        = 0x200
    <span id="S_IEXEC">S_IEXEC</span>                          = 0x40
    <span id="S_IFBLK">S_IFBLK</span>                          = 0x6000
    <span id="S_IFCHR">S_IFCHR</span>                          = 0x2000
    <span id="S_IFDIR">S_IFDIR</span>                          = 0x4000
    <span id="S_IFIFO">S_IFIFO</span>                          = 0x1000
    <span id="S_IFLNK">S_IFLNK</span>                          = 0xa000
    <span id="S_IFMT">S_IFMT</span>                           = 0xf000
    <span id="S_IFREG">S_IFREG</span>                          = 0x8000
    <span id="S_IFSOCK">S_IFSOCK</span>                         = 0xc000
    <span id="S_IREAD">S_IREAD</span>                          = 0x100
    <span id="S_IRGRP">S_IRGRP</span>                          = 0x20
    <span id="S_IROTH">S_IROTH</span>                          = 0x4
    <span id="S_IRUSR">S_IRUSR</span>                          = 0x100
    <span id="S_IRWXG">S_IRWXG</span>                          = 0x38
    <span id="S_IRWXO">S_IRWXO</span>                          = 0x7
    <span id="S_IRWXU">S_IRWXU</span>                          = 0x1c0
    <span id="S_ISGID">S_ISGID</span>                          = 0x400
    <span id="S_ISUID">S_ISUID</span>                          = 0x800
    <span id="S_ISVTX">S_ISVTX</span>                          = 0x200
    <span id="S_IWGRP">S_IWGRP</span>                          = 0x10
    <span id="S_IWOTH">S_IWOTH</span>                          = 0x2
    <span id="S_IWRITE">S_IWRITE</span>                         = 0x80
    <span id="S_IWUSR">S_IWUSR</span>                          = 0x80
    <span id="S_IXGRP">S_IXGRP</span>                          = 0x8
    <span id="S_IXOTH">S_IXOTH</span>                          = 0x1
    <span id="S_IXUSR">S_IXUSR</span>                          = 0x40
    <span id="TCIFLUSH">TCIFLUSH</span>                         = 0x0
    <span id="TCIOFLUSH">TCIOFLUSH</span>                        = 0x2
    <span id="TCOFLUSH">TCOFLUSH</span>                         = 0x1
    <span id="TCP_CONGESTION">TCP_CONGESTION</span>                   = 0xd
    <span id="TCP_CORK">TCP_CORK</span>                         = 0x3
    <span id="TCP_DEFER_ACCEPT">TCP_DEFER_ACCEPT</span>                 = 0x9
    <span id="TCP_INFO">TCP_INFO</span>                         = 0xb
    <span id="TCP_KEEPCNT">TCP_KEEPCNT</span>                      = 0x6
    <span id="TCP_KEEPIDLE">TCP_KEEPIDLE</span>                     = 0x4
    <span id="TCP_KEEPINTVL">TCP_KEEPINTVL</span>                    = 0x5
    <span id="TCP_LINGER2">TCP_LINGER2</span>                      = 0x8
    <span id="TCP_MAXSEG">TCP_MAXSEG</span>                       = 0x2
    <span id="TCP_MAXWIN">TCP_MAXWIN</span>                       = 0xffff
    <span id="TCP_MAX_WINSHIFT">TCP_MAX_WINSHIFT</span>                 = 0xe
    <span id="TCP_MD5SIG">TCP_MD5SIG</span>                       = 0xe
    <span id="TCP_MD5SIG_MAXKEYLEN">TCP_MD5SIG_MAXKEYLEN</span>             = 0x50
    <span id="TCP_MSS">TCP_MSS</span>                          = 0x200
    <span id="TCP_NODELAY">TCP_NODELAY</span>                      = 0x1
    <span id="TCP_QUICKACK">TCP_QUICKACK</span>                     = 0xc
    <span id="TCP_SYNCNT">TCP_SYNCNT</span>                       = 0x7
    <span id="TCP_WINDOW_CLAMP">TCP_WINDOW_CLAMP</span>                 = 0xa
    <span id="TIOCCBRK">TIOCCBRK</span>                         = 0x5428
    <span id="TIOCCONS">TIOCCONS</span>                         = 0x541d
    <span id="TIOCEXCL">TIOCEXCL</span>                         = 0x540c
    <span id="TIOCGDEV">TIOCGDEV</span>                         = 0x80045432
    <span id="TIOCGETD">TIOCGETD</span>                         = 0x5424
    <span id="TIOCGICOUNT">TIOCGICOUNT</span>                      = 0x545d
    <span id="TIOCGLCKTRMIOS">TIOCGLCKTRMIOS</span>                   = 0x5456
    <span id="TIOCGPGRP">TIOCGPGRP</span>                        = 0x540f
    <span id="TIOCGPTN">TIOCGPTN</span>                         = 0x80045430
    <span id="TIOCGRS485">TIOCGRS485</span>                       = 0x542e
    <span id="TIOCGSERIAL">TIOCGSERIAL</span>                      = 0x541e
    <span id="TIOCGSID">TIOCGSID</span>                         = 0x5429
    <span id="TIOCGSOFTCAR">TIOCGSOFTCAR</span>                     = 0x5419
    <span id="TIOCGWINSZ">TIOCGWINSZ</span>                       = 0x5413
    <span id="TIOCINQ">TIOCINQ</span>                          = 0x541b
    <span id="TIOCLINUX">TIOCLINUX</span>                        = 0x541c
    <span id="TIOCMBIC">TIOCMBIC</span>                         = 0x5417
    <span id="TIOCMBIS">TIOCMBIS</span>                         = 0x5416
    <span id="TIOCMGET">TIOCMGET</span>                         = 0x5415
    <span id="TIOCMIWAIT">TIOCMIWAIT</span>                       = 0x545c
    <span id="TIOCMSET">TIOCMSET</span>                         = 0x5418
    <span id="TIOCM_CAR">TIOCM_CAR</span>                        = 0x40
    <span id="TIOCM_CD">TIOCM_CD</span>                         = 0x40
    <span id="TIOCM_CTS">TIOCM_CTS</span>                        = 0x20
    <span id="TIOCM_DSR">TIOCM_DSR</span>                        = 0x100
    <span id="TIOCM_DTR">TIOCM_DTR</span>                        = 0x2
    <span id="TIOCM_LE">TIOCM_LE</span>                         = 0x1
    <span id="TIOCM_RI">TIOCM_RI</span>                         = 0x80
    <span id="TIOCM_RNG">TIOCM_RNG</span>                        = 0x80
    <span id="TIOCM_RTS">TIOCM_RTS</span>                        = 0x4
    <span id="TIOCM_SR">TIOCM_SR</span>                         = 0x10
    <span id="TIOCM_ST">TIOCM_ST</span>                         = 0x8
    <span id="TIOCNOTTY">TIOCNOTTY</span>                        = 0x5422
    <span id="TIOCNXCL">TIOCNXCL</span>                         = 0x540d
    <span id="TIOCOUTQ">TIOCOUTQ</span>                         = 0x5411
    <span id="TIOCPKT">TIOCPKT</span>                          = 0x5420
    <span id="TIOCPKT_DATA">TIOCPKT_DATA</span>                     = 0x0
    <span id="TIOCPKT_DOSTOP">TIOCPKT_DOSTOP</span>                   = 0x20
    <span id="TIOCPKT_FLUSHREAD">TIOCPKT_FLUSHREAD</span>                = 0x1
    <span id="TIOCPKT_FLUSHWRITE">TIOCPKT_FLUSHWRITE</span>               = 0x2
    <span id="TIOCPKT_IOCTL">TIOCPKT_IOCTL</span>                    = 0x40
    <span id="TIOCPKT_NOSTOP">TIOCPKT_NOSTOP</span>                   = 0x10
    <span id="TIOCPKT_START">TIOCPKT_START</span>                    = 0x8
    <span id="TIOCPKT_STOP">TIOCPKT_STOP</span>                     = 0x4
    <span id="TIOCSBRK">TIOCSBRK</span>                         = 0x5427
    <span id="TIOCSCTTY">TIOCSCTTY</span>                        = 0x540e
    <span id="TIOCSERCONFIG">TIOCSERCONFIG</span>                    = 0x5453
    <span id="TIOCSERGETLSR">TIOCSERGETLSR</span>                    = 0x5459
    <span id="TIOCSERGETMULTI">TIOCSERGETMULTI</span>                  = 0x545a
    <span id="TIOCSERGSTRUCT">TIOCSERGSTRUCT</span>                   = 0x5458
    <span id="TIOCSERGWILD">TIOCSERGWILD</span>                     = 0x5454
    <span id="TIOCSERSETMULTI">TIOCSERSETMULTI</span>                  = 0x545b
    <span id="TIOCSERSWILD">TIOCSERSWILD</span>                     = 0x5455
    <span id="TIOCSER_TEMT">TIOCSER_TEMT</span>                     = 0x1
    <span id="TIOCSETD">TIOCSETD</span>                         = 0x5423
    <span id="TIOCSIG">TIOCSIG</span>                          = 0x40045436
    <span id="TIOCSLCKTRMIOS">TIOCSLCKTRMIOS</span>                   = 0x5457
    <span id="TIOCSPGRP">TIOCSPGRP</span>                        = 0x5410
    <span id="TIOCSPTLCK">TIOCSPTLCK</span>                       = 0x40045431
    <span id="TIOCSRS485">TIOCSRS485</span>                       = 0x542f
    <span id="TIOCSSERIAL">TIOCSSERIAL</span>                      = 0x541f
    <span id="TIOCSSOFTCAR">TIOCSSOFTCAR</span>                     = 0x541a
    <span id="TIOCSTI">TIOCSTI</span>                          = 0x5412
    <span id="TIOCSWINSZ">TIOCSWINSZ</span>                       = 0x5414
    <span id="TUNATTACHFILTER">TUNATTACHFILTER</span>                  = 0x401054d5
    <span id="TUNDETACHFILTER">TUNDETACHFILTER</span>                  = 0x401054d6
    <span id="TUNGETFEATURES">TUNGETFEATURES</span>                   = 0x800454cf
    <span id="TUNGETIFF">TUNGETIFF</span>                        = 0x800454d2
    <span id="TUNGETSNDBUF">TUNGETSNDBUF</span>                     = 0x800454d3
    <span id="TUNGETVNETHDRSZ">TUNGETVNETHDRSZ</span>                  = 0x800454d7
    <span id="TUNSETDEBUG">TUNSETDEBUG</span>                      = 0x400454c9
    <span id="TUNSETGROUP">TUNSETGROUP</span>                      = 0x400454ce
    <span id="TUNSETIFF">TUNSETIFF</span>                        = 0x400454ca
    <span id="TUNSETLINK">TUNSETLINK</span>                       = 0x400454cd
    <span id="TUNSETNOCSUM">TUNSETNOCSUM</span>                     = 0x400454c8
    <span id="TUNSETOFFLOAD">TUNSETOFFLOAD</span>                    = 0x400454d0
    <span id="TUNSETOWNER">TUNSETOWNER</span>                      = 0x400454cc
    <span id="TUNSETPERSIST">TUNSETPERSIST</span>                    = 0x400454cb
    <span id="TUNSETSNDBUF">TUNSETSNDBUF</span>                     = 0x400454d4
    <span id="TUNSETTXFILTER">TUNSETTXFILTER</span>                   = 0x400454d1
    <span id="TUNSETVNETHDRSZ">TUNSETVNETHDRSZ</span>                  = 0x400454d8
    <span id="WALL">WALL</span>                             = 0x40000000
    <span id="WCLONE">WCLONE</span>                           = 0x80000000
    <span id="WCONTINUED">WCONTINUED</span>                       = 0x8
    <span id="WEXITED">WEXITED</span>                          = 0x4
    <span id="WNOHANG">WNOHANG</span>                          = 0x1
    <span id="WNOTHREAD">WNOTHREAD</span>                        = 0x20000000
    <span id="WNOWAIT">WNOWAIT</span>                          = 0x1000000
    <span id="WORDSIZE">WORDSIZE</span>                         = 0x40
    <span id="WSTOPPED">WSTOPPED</span>                         = 0x2
    <span id="WUNTRACED">WUNTRACED</span>                        = 0x2
)</pre>


<pre>const (
    <span id="E2BIG">E2BIG</span>           = <a href="#Errno">Errno</a>(0x7)
    <span id="EACCES">EACCES</span>          = <a href="#Errno">Errno</a>(0xd)
    <span id="EADDRINUSE">EADDRINUSE</span>      = <a href="#Errno">Errno</a>(0x62)
    <span id="EADDRNOTAVAIL">EADDRNOTAVAIL</span>   = <a href="#Errno">Errno</a>(0x63)
    <span id="EADV">EADV</span>            = <a href="#Errno">Errno</a>(0x44)
    <span id="EAFNOSUPPORT">EAFNOSUPPORT</span>    = <a href="#Errno">Errno</a>(0x61)
    <span id="EAGAIN">EAGAIN</span>          = <a href="#Errno">Errno</a>(0xb)
    <span id="EALREADY">EALREADY</span>        = <a href="#Errno">Errno</a>(0x72)
    <span id="EBADE">EBADE</span>           = <a href="#Errno">Errno</a>(0x34)
    <span id="EBADF">EBADF</span>           = <a href="#Errno">Errno</a>(0x9)
    <span id="EBADFD">EBADFD</span>          = <a href="#Errno">Errno</a>(0x4d)
    <span id="EBADMSG">EBADMSG</span>         = <a href="#Errno">Errno</a>(0x4a)
    <span id="EBADR">EBADR</span>           = <a href="#Errno">Errno</a>(0x35)
    <span id="EBADRQC">EBADRQC</span>         = <a href="#Errno">Errno</a>(0x38)
    <span id="EBADSLT">EBADSLT</span>         = <a href="#Errno">Errno</a>(0x39)
    <span id="EBFONT">EBFONT</span>          = <a href="#Errno">Errno</a>(0x3b)
    <span id="EBUSY">EBUSY</span>           = <a href="#Errno">Errno</a>(0x10)
    <span id="ECANCELED">ECANCELED</span>       = <a href="#Errno">Errno</a>(0x7d)
    <span id="ECHILD">ECHILD</span>          = <a href="#Errno">Errno</a>(0xa)
    <span id="ECHRNG">ECHRNG</span>          = <a href="#Errno">Errno</a>(0x2c)
    <span id="ECOMM">ECOMM</span>           = <a href="#Errno">Errno</a>(0x46)
    <span id="ECONNABORTED">ECONNABORTED</span>    = <a href="#Errno">Errno</a>(0x67)
    <span id="ECONNREFUSED">ECONNREFUSED</span>    = <a href="#Errno">Errno</a>(0x6f)
    <span id="ECONNRESET">ECONNRESET</span>      = <a href="#Errno">Errno</a>(0x68)
    <span id="EDEADLK">EDEADLK</span>         = <a href="#Errno">Errno</a>(0x23)
    <span id="EDEADLOCK">EDEADLOCK</span>       = <a href="#Errno">Errno</a>(0x23)
    <span id="EDESTADDRREQ">EDESTADDRREQ</span>    = <a href="#Errno">Errno</a>(0x59)
    <span id="EDOM">EDOM</span>            = <a href="#Errno">Errno</a>(0x21)
    <span id="EDOTDOT">EDOTDOT</span>         = <a href="#Errno">Errno</a>(0x49)
    <span id="EDQUOT">EDQUOT</span>          = <a href="#Errno">Errno</a>(0x7a)
    <span id="EEXIST">EEXIST</span>          = <a href="#Errno">Errno</a>(0x11)
    <span id="EFAULT">EFAULT</span>          = <a href="#Errno">Errno</a>(0xe)
    <span id="EFBIG">EFBIG</span>           = <a href="#Errno">Errno</a>(0x1b)
    <span id="EHOSTDOWN">EHOSTDOWN</span>       = <a href="#Errno">Errno</a>(0x70)
    <span id="EHOSTUNREACH">EHOSTUNREACH</span>    = <a href="#Errno">Errno</a>(0x71)
    <span id="EIDRM">EIDRM</span>           = <a href="#Errno">Errno</a>(0x2b)
    <span id="EILSEQ">EILSEQ</span>          = <a href="#Errno">Errno</a>(0x54)
    <span id="EINPROGRESS">EINPROGRESS</span>     = <a href="#Errno">Errno</a>(0x73)
    <span id="EINTR">EINTR</span>           = <a href="#Errno">Errno</a>(0x4)
    <span id="EINVAL">EINVAL</span>          = <a href="#Errno">Errno</a>(0x16)
    <span id="EIO">EIO</span>             = <a href="#Errno">Errno</a>(0x5)
    <span id="EISCONN">EISCONN</span>         = <a href="#Errno">Errno</a>(0x6a)
    <span id="EISDIR">EISDIR</span>          = <a href="#Errno">Errno</a>(0x15)
    <span id="EISNAM">EISNAM</span>          = <a href="#Errno">Errno</a>(0x78)
    <span id="EKEYEXPIRED">EKEYEXPIRED</span>     = <a href="#Errno">Errno</a>(0x7f)
    <span id="EKEYREJECTED">EKEYREJECTED</span>    = <a href="#Errno">Errno</a>(0x81)
    <span id="EKEYREVOKED">EKEYREVOKED</span>     = <a href="#Errno">Errno</a>(0x80)
    <span id="EL2HLT">EL2HLT</span>          = <a href="#Errno">Errno</a>(0x33)
    <span id="EL2NSYNC">EL2NSYNC</span>        = <a href="#Errno">Errno</a>(0x2d)
    <span id="EL3HLT">EL3HLT</span>          = <a href="#Errno">Errno</a>(0x2e)
    <span id="EL3RST">EL3RST</span>          = <a href="#Errno">Errno</a>(0x2f)
    <span id="ELIBACC">ELIBACC</span>         = <a href="#Errno">Errno</a>(0x4f)
    <span id="ELIBBAD">ELIBBAD</span>         = <a href="#Errno">Errno</a>(0x50)
    <span id="ELIBEXEC">ELIBEXEC</span>        = <a href="#Errno">Errno</a>(0x53)
    <span id="ELIBMAX">ELIBMAX</span>         = <a href="#Errno">Errno</a>(0x52)
    <span id="ELIBSCN">ELIBSCN</span>         = <a href="#Errno">Errno</a>(0x51)
    <span id="ELNRNG">ELNRNG</span>          = <a href="#Errno">Errno</a>(0x30)
    <span id="ELOOP">ELOOP</span>           = <a href="#Errno">Errno</a>(0x28)
    <span id="EMEDIUMTYPE">EMEDIUMTYPE</span>     = <a href="#Errno">Errno</a>(0x7c)
    <span id="EMFILE">EMFILE</span>          = <a href="#Errno">Errno</a>(0x18)
    <span id="EMLINK">EMLINK</span>          = <a href="#Errno">Errno</a>(0x1f)
    <span id="EMSGSIZE">EMSGSIZE</span>        = <a href="#Errno">Errno</a>(0x5a)
    <span id="EMULTIHOP">EMULTIHOP</span>       = <a href="#Errno">Errno</a>(0x48)
    <span id="ENAMETOOLONG">ENAMETOOLONG</span>    = <a href="#Errno">Errno</a>(0x24)
    <span id="ENAVAIL">ENAVAIL</span>         = <a href="#Errno">Errno</a>(0x77)
    <span id="ENETDOWN">ENETDOWN</span>        = <a href="#Errno">Errno</a>(0x64)
    <span id="ENETRESET">ENETRESET</span>       = <a href="#Errno">Errno</a>(0x66)
    <span id="ENETUNREACH">ENETUNREACH</span>     = <a href="#Errno">Errno</a>(0x65)
    <span id="ENFILE">ENFILE</span>          = <a href="#Errno">Errno</a>(0x17)
    <span id="ENOANO">ENOANO</span>          = <a href="#Errno">Errno</a>(0x37)
    <span id="ENOBUFS">ENOBUFS</span>         = <a href="#Errno">Errno</a>(0x69)
    <span id="ENOCSI">ENOCSI</span>          = <a href="#Errno">Errno</a>(0x32)
    <span id="ENODATA">ENODATA</span>         = <a href="#Errno">Errno</a>(0x3d)
    <span id="ENODEV">ENODEV</span>          = <a href="#Errno">Errno</a>(0x13)
    <span id="ENOENT">ENOENT</span>          = <a href="#Errno">Errno</a>(0x2)
    <span id="ENOEXEC">ENOEXEC</span>         = <a href="#Errno">Errno</a>(0x8)
    <span id="ENOKEY">ENOKEY</span>          = <a href="#Errno">Errno</a>(0x7e)
    <span id="ENOLCK">ENOLCK</span>          = <a href="#Errno">Errno</a>(0x25)
    <span id="ENOLINK">ENOLINK</span>         = <a href="#Errno">Errno</a>(0x43)
    <span id="ENOMEDIUM">ENOMEDIUM</span>       = <a href="#Errno">Errno</a>(0x7b)
    <span id="ENOMEM">ENOMEM</span>          = <a href="#Errno">Errno</a>(0xc)
    <span id="ENOMSG">ENOMSG</span>          = <a href="#Errno">Errno</a>(0x2a)
    <span id="ENONET">ENONET</span>          = <a href="#Errno">Errno</a>(0x40)
    <span id="ENOPKG">ENOPKG</span>          = <a href="#Errno">Errno</a>(0x41)
    <span id="ENOPROTOOPT">ENOPROTOOPT</span>     = <a href="#Errno">Errno</a>(0x5c)
    <span id="ENOSPC">ENOSPC</span>          = <a href="#Errno">Errno</a>(0x1c)
    <span id="ENOSR">ENOSR</span>           = <a href="#Errno">Errno</a>(0x3f)
    <span id="ENOSTR">ENOSTR</span>          = <a href="#Errno">Errno</a>(0x3c)
    <span id="ENOSYS">ENOSYS</span>          = <a href="#Errno">Errno</a>(0x26)
    <span id="ENOTBLK">ENOTBLK</span>         = <a href="#Errno">Errno</a>(0xf)
    <span id="ENOTCONN">ENOTCONN</span>        = <a href="#Errno">Errno</a>(0x6b)
    <span id="ENOTDIR">ENOTDIR</span>         = <a href="#Errno">Errno</a>(0x14)
    <span id="ENOTEMPTY">ENOTEMPTY</span>       = <a href="#Errno">Errno</a>(0x27)
    <span id="ENOTNAM">ENOTNAM</span>         = <a href="#Errno">Errno</a>(0x76)
    <span id="ENOTRECOVERABLE">ENOTRECOVERABLE</span> = <a href="#Errno">Errno</a>(0x83)
    <span id="ENOTSOCK">ENOTSOCK</span>        = <a href="#Errno">Errno</a>(0x58)
    <span id="ENOTSUP">ENOTSUP</span>         = <a href="#Errno">Errno</a>(0x5f)
    <span id="ENOTTY">ENOTTY</span>          = <a href="#Errno">Errno</a>(0x19)
    <span id="ENOTUNIQ">ENOTUNIQ</span>        = <a href="#Errno">Errno</a>(0x4c)
    <span id="ENXIO">ENXIO</span>           = <a href="#Errno">Errno</a>(0x6)
    <span id="EOPNOTSUPP">EOPNOTSUPP</span>      = <a href="#Errno">Errno</a>(0x5f)
    <span id="EOVERFLOW">EOVERFLOW</span>       = <a href="#Errno">Errno</a>(0x4b)
    <span id="EOWNERDEAD">EOWNERDEAD</span>      = <a href="#Errno">Errno</a>(0x82)
    <span id="EPERM">EPERM</span>           = <a href="#Errno">Errno</a>(0x1)
    <span id="EPFNOSUPPORT">EPFNOSUPPORT</span>    = <a href="#Errno">Errno</a>(0x60)
    <span id="EPIPE">EPIPE</span>           = <a href="#Errno">Errno</a>(0x20)
    <span id="EPROTO">EPROTO</span>          = <a href="#Errno">Errno</a>(0x47)
    <span id="EPROTONOSUPPORT">EPROTONOSUPPORT</span> = <a href="#Errno">Errno</a>(0x5d)
    <span id="EPROTOTYPE">EPROTOTYPE</span>      = <a href="#Errno">Errno</a>(0x5b)
    <span id="ERANGE">ERANGE</span>          = <a href="#Errno">Errno</a>(0x22)
    <span id="EREMCHG">EREMCHG</span>         = <a href="#Errno">Errno</a>(0x4e)
    <span id="EREMOTE">EREMOTE</span>         = <a href="#Errno">Errno</a>(0x42)
    <span id="EREMOTEIO">EREMOTEIO</span>       = <a href="#Errno">Errno</a>(0x79)
    <span id="ERESTART">ERESTART</span>        = <a href="#Errno">Errno</a>(0x55)
    <span id="ERFKILL">ERFKILL</span>         = <a href="#Errno">Errno</a>(0x84)
    <span id="EROFS">EROFS</span>           = <a href="#Errno">Errno</a>(0x1e)
    <span id="ESHUTDOWN">ESHUTDOWN</span>       = <a href="#Errno">Errno</a>(0x6c)
    <span id="ESOCKTNOSUPPORT">ESOCKTNOSUPPORT</span> = <a href="#Errno">Errno</a>(0x5e)
    <span id="ESPIPE">ESPIPE</span>          = <a href="#Errno">Errno</a>(0x1d)
    <span id="ESRCH">ESRCH</span>           = <a href="#Errno">Errno</a>(0x3)
    <span id="ESRMNT">ESRMNT</span>          = <a href="#Errno">Errno</a>(0x45)
    <span id="ESTALE">ESTALE</span>          = <a href="#Errno">Errno</a>(0x74)
    <span id="ESTRPIPE">ESTRPIPE</span>        = <a href="#Errno">Errno</a>(0x56)
    <span id="ETIME">ETIME</span>           = <a href="#Errno">Errno</a>(0x3e)
    <span id="ETIMEDOUT">ETIMEDOUT</span>       = <a href="#Errno">Errno</a>(0x6e)
    <span id="ETOOMANYREFS">ETOOMANYREFS</span>    = <a href="#Errno">Errno</a>(0x6d)
    <span id="ETXTBSY">ETXTBSY</span>         = <a href="#Errno">Errno</a>(0x1a)
    <span id="EUCLEAN">EUCLEAN</span>         = <a href="#Errno">Errno</a>(0x75)
    <span id="EUNATCH">EUNATCH</span>         = <a href="#Errno">Errno</a>(0x31)
    <span id="EUSERS">EUSERS</span>          = <a href="#Errno">Errno</a>(0x57)
    <span id="EWOULDBLOCK">EWOULDBLOCK</span>     = <a href="#Errno">Errno</a>(0xb)
    <span id="EXDEV">EXDEV</span>           = <a href="#Errno">Errno</a>(0x12)
    <span id="EXFULL">EXFULL</span>          = <a href="#Errno">Errno</a>(0x36)
)</pre>

Errors

<pre>const (
    <span id="SIGABRT">SIGABRT</span>   = <a href="#Signal">Signal</a>(0x6)
    <span id="SIGALRM">SIGALRM</span>   = <a href="#Signal">Signal</a>(0xe)
    <span id="SIGBUS">SIGBUS</span>    = <a href="#Signal">Signal</a>(0x7)
    <span id="SIGCHLD">SIGCHLD</span>   = <a href="#Signal">Signal</a>(0x11)
    <span id="SIGCLD">SIGCLD</span>    = <a href="#Signal">Signal</a>(0x11)
    <span id="SIGCONT">SIGCONT</span>   = <a href="#Signal">Signal</a>(0x12)
    <span id="SIGFPE">SIGFPE</span>    = <a href="#Signal">Signal</a>(0x8)
    <span id="SIGHUP">SIGHUP</span>    = <a href="#Signal">Signal</a>(0x1)
    <span id="SIGILL">SIGILL</span>    = <a href="#Signal">Signal</a>(0x4)
    <span id="SIGINT">SIGINT</span>    = <a href="#Signal">Signal</a>(0x2)
    <span id="SIGIO">SIGIO</span>     = <a href="#Signal">Signal</a>(0x1d)
    <span id="SIGIOT">SIGIOT</span>    = <a href="#Signal">Signal</a>(0x6)
    <span id="SIGKILL">SIGKILL</span>   = <a href="#Signal">Signal</a>(0x9)
    <span id="SIGPIPE">SIGPIPE</span>   = <a href="#Signal">Signal</a>(0xd)
    <span id="SIGPOLL">SIGPOLL</span>   = <a href="#Signal">Signal</a>(0x1d)
    <span id="SIGPROF">SIGPROF</span>   = <a href="#Signal">Signal</a>(0x1b)
    <span id="SIGPWR">SIGPWR</span>    = <a href="#Signal">Signal</a>(0x1e)
    <span id="SIGQUIT">SIGQUIT</span>   = <a href="#Signal">Signal</a>(0x3)
    <span id="SIGSEGV">SIGSEGV</span>   = <a href="#Signal">Signal</a>(0xb)
    <span id="SIGSTKFLT">SIGSTKFLT</span> = <a href="#Signal">Signal</a>(0x10)
    <span id="SIGSTOP">SIGSTOP</span>   = <a href="#Signal">Signal</a>(0x13)
    <span id="SIGSYS">SIGSYS</span>    = <a href="#Signal">Signal</a>(0x1f)
    <span id="SIGTERM">SIGTERM</span>   = <a href="#Signal">Signal</a>(0xf)
    <span id="SIGTRAP">SIGTRAP</span>   = <a href="#Signal">Signal</a>(0x5)
    <span id="SIGTSTP">SIGTSTP</span>   = <a href="#Signal">Signal</a>(0x14)
    <span id="SIGTTIN">SIGTTIN</span>   = <a href="#Signal">Signal</a>(0x15)
    <span id="SIGTTOU">SIGTTOU</span>   = <a href="#Signal">Signal</a>(0x16)
    <span id="SIGUNUSED">SIGUNUSED</span> = <a href="#Signal">Signal</a>(0x1f)
    <span id="SIGURG">SIGURG</span>    = <a href="#Signal">Signal</a>(0x17)
    <span id="SIGUSR1">SIGUSR1</span>   = <a href="#Signal">Signal</a>(0xa)
    <span id="SIGUSR2">SIGUSR2</span>   = <a href="#Signal">Signal</a>(0xc)
    <span id="SIGVTALRM">SIGVTALRM</span> = <a href="#Signal">Signal</a>(0x1a)
    <span id="SIGWINCH">SIGWINCH</span>  = <a href="#Signal">Signal</a>(0x1c)
    <span id="SIGXCPU">SIGXCPU</span>   = <a href="#Signal">Signal</a>(0x18)
    <span id="SIGXFSZ">SIGXFSZ</span>   = <a href="#Signal">Signal</a>(0x19)
)</pre>

Signals

<pre>const (
    <span id="SYS_READ">SYS_READ</span>                   = 0
    <span id="SYS_WRITE">SYS_WRITE</span>                  = 1
    <span id="SYS_OPEN">SYS_OPEN</span>                   = 2
    <span id="SYS_CLOSE">SYS_CLOSE</span>                  = 3
    <span id="SYS_STAT">SYS_STAT</span>                   = 4
    <span id="SYS_FSTAT">SYS_FSTAT</span>                  = 5
    <span id="SYS_LSTAT">SYS_LSTAT</span>                  = 6
    <span id="SYS_POLL">SYS_POLL</span>                   = 7
    <span id="SYS_LSEEK">SYS_LSEEK</span>                  = 8
    <span id="SYS_MMAP">SYS_MMAP</span>                   = 9
    <span id="SYS_MPROTECT">SYS_MPROTECT</span>               = 10
    <span id="SYS_MUNMAP">SYS_MUNMAP</span>                 = 11
    <span id="SYS_BRK">SYS_BRK</span>                    = 12
    <span id="SYS_RT_SIGACTION">SYS_RT_SIGACTION</span>           = 13
    <span id="SYS_RT_SIGPROCMASK">SYS_RT_SIGPROCMASK</span>         = 14
    <span id="SYS_RT_SIGRETURN">SYS_RT_SIGRETURN</span>           = 15
    <span id="SYS_IOCTL">SYS_IOCTL</span>                  = 16
    <span id="SYS_PREAD64">SYS_PREAD64</span>                = 17
    <span id="SYS_PWRITE64">SYS_PWRITE64</span>               = 18
    <span id="SYS_READV">SYS_READV</span>                  = 19
    <span id="SYS_WRITEV">SYS_WRITEV</span>                 = 20
    <span id="SYS_ACCESS">SYS_ACCESS</span>                 = 21
    <span id="SYS_PIPE">SYS_PIPE</span>                   = 22
    <span id="SYS_SELECT">SYS_SELECT</span>                 = 23
    <span id="SYS_SCHED_YIELD">SYS_SCHED_YIELD</span>            = 24
    <span id="SYS_MREMAP">SYS_MREMAP</span>                 = 25
    <span id="SYS_MSYNC">SYS_MSYNC</span>                  = 26
    <span id="SYS_MINCORE">SYS_MINCORE</span>                = 27
    <span id="SYS_MADVISE">SYS_MADVISE</span>                = 28
    <span id="SYS_SHMGET">SYS_SHMGET</span>                 = 29
    <span id="SYS_SHMAT">SYS_SHMAT</span>                  = 30
    <span id="SYS_SHMCTL">SYS_SHMCTL</span>                 = 31
    <span id="SYS_DUP">SYS_DUP</span>                    = 32
    <span id="SYS_DUP2">SYS_DUP2</span>                   = 33
    <span id="SYS_PAUSE">SYS_PAUSE</span>                  = 34
    <span id="SYS_NANOSLEEP">SYS_NANOSLEEP</span>              = 35
    <span id="SYS_GETITIMER">SYS_GETITIMER</span>              = 36
    <span id="SYS_ALARM">SYS_ALARM</span>                  = 37
    <span id="SYS_SETITIMER">SYS_SETITIMER</span>              = 38
    <span id="SYS_GETPID">SYS_GETPID</span>                 = 39
    <span id="SYS_SENDFILE">SYS_SENDFILE</span>               = 40
    <span id="SYS_SOCKET">SYS_SOCKET</span>                 = 41
    <span id="SYS_CONNECT">SYS_CONNECT</span>                = 42
    <span id="SYS_ACCEPT">SYS_ACCEPT</span>                 = 43
    <span id="SYS_SENDTO">SYS_SENDTO</span>                 = 44
    <span id="SYS_RECVFROM">SYS_RECVFROM</span>               = 45
    <span id="SYS_SENDMSG">SYS_SENDMSG</span>                = 46
    <span id="SYS_RECVMSG">SYS_RECVMSG</span>                = 47
    <span id="SYS_SHUTDOWN">SYS_SHUTDOWN</span>               = 48
    <span id="SYS_BIND">SYS_BIND</span>                   = 49
    <span id="SYS_LISTEN">SYS_LISTEN</span>                 = 50
    <span id="SYS_GETSOCKNAME">SYS_GETSOCKNAME</span>            = 51
    <span id="SYS_GETPEERNAME">SYS_GETPEERNAME</span>            = 52
    <span id="SYS_SOCKETPAIR">SYS_SOCKETPAIR</span>             = 53
    <span id="SYS_SETSOCKOPT">SYS_SETSOCKOPT</span>             = 54
    <span id="SYS_GETSOCKOPT">SYS_GETSOCKOPT</span>             = 55
    <span id="SYS_CLONE">SYS_CLONE</span>                  = 56
    <span id="SYS_FORK">SYS_FORK</span>                   = 57
    <span id="SYS_VFORK">SYS_VFORK</span>                  = 58
    <span id="SYS_EXECVE">SYS_EXECVE</span>                 = 59
    <span id="SYS_EXIT">SYS_EXIT</span>                   = 60
    <span id="SYS_WAIT4">SYS_WAIT4</span>                  = 61
    <span id="SYS_KILL">SYS_KILL</span>                   = 62
    <span id="SYS_UNAME">SYS_UNAME</span>                  = 63
    <span id="SYS_SEMGET">SYS_SEMGET</span>                 = 64
    <span id="SYS_SEMOP">SYS_SEMOP</span>                  = 65
    <span id="SYS_SEMCTL">SYS_SEMCTL</span>                 = 66
    <span id="SYS_SHMDT">SYS_SHMDT</span>                  = 67
    <span id="SYS_MSGGET">SYS_MSGGET</span>                 = 68
    <span id="SYS_MSGSND">SYS_MSGSND</span>                 = 69
    <span id="SYS_MSGRCV">SYS_MSGRCV</span>                 = 70
    <span id="SYS_MSGCTL">SYS_MSGCTL</span>                 = 71
    <span id="SYS_FCNTL">SYS_FCNTL</span>                  = 72
    <span id="SYS_FLOCK">SYS_FLOCK</span>                  = 73
    <span id="SYS_FSYNC">SYS_FSYNC</span>                  = 74
    <span id="SYS_FDATASYNC">SYS_FDATASYNC</span>              = 75
    <span id="SYS_TRUNCATE">SYS_TRUNCATE</span>               = 76
    <span id="SYS_FTRUNCATE">SYS_FTRUNCATE</span>              = 77
    <span id="SYS_GETDENTS">SYS_GETDENTS</span>               = 78
    <span id="SYS_GETCWD">SYS_GETCWD</span>                 = 79
    <span id="SYS_CHDIR">SYS_CHDIR</span>                  = 80
    <span id="SYS_FCHDIR">SYS_FCHDIR</span>                 = 81
    <span id="SYS_RENAME">SYS_RENAME</span>                 = 82
    <span id="SYS_MKDIR">SYS_MKDIR</span>                  = 83
    <span id="SYS_RMDIR">SYS_RMDIR</span>                  = 84
    <span id="SYS_CREAT">SYS_CREAT</span>                  = 85
    <span id="SYS_LINK">SYS_LINK</span>                   = 86
    <span id="SYS_UNLINK">SYS_UNLINK</span>                 = 87
    <span id="SYS_SYMLINK">SYS_SYMLINK</span>                = 88
    <span id="SYS_READLINK">SYS_READLINK</span>               = 89
    <span id="SYS_CHMOD">SYS_CHMOD</span>                  = 90
    <span id="SYS_FCHMOD">SYS_FCHMOD</span>                 = 91
    <span id="SYS_CHOWN">SYS_CHOWN</span>                  = 92
    <span id="SYS_FCHOWN">SYS_FCHOWN</span>                 = 93
    <span id="SYS_LCHOWN">SYS_LCHOWN</span>                 = 94
    <span id="SYS_UMASK">SYS_UMASK</span>                  = 95
    <span id="SYS_GETTIMEOFDAY">SYS_GETTIMEOFDAY</span>           = 96
    <span id="SYS_GETRLIMIT">SYS_GETRLIMIT</span>              = 97
    <span id="SYS_GETRUSAGE">SYS_GETRUSAGE</span>              = 98
    <span id="SYS_SYSINFO">SYS_SYSINFO</span>                = 99
    <span id="SYS_TIMES">SYS_TIMES</span>                  = 100
    <span id="SYS_PTRACE">SYS_PTRACE</span>                 = 101
    <span id="SYS_GETUID">SYS_GETUID</span>                 = 102
    <span id="SYS_SYSLOG">SYS_SYSLOG</span>                 = 103
    <span id="SYS_GETGID">SYS_GETGID</span>                 = 104
    <span id="SYS_SETUID">SYS_SETUID</span>                 = 105
    <span id="SYS_SETGID">SYS_SETGID</span>                 = 106
    <span id="SYS_GETEUID">SYS_GETEUID</span>                = 107
    <span id="SYS_GETEGID">SYS_GETEGID</span>                = 108
    <span id="SYS_SETPGID">SYS_SETPGID</span>                = 109
    <span id="SYS_GETPPID">SYS_GETPPID</span>                = 110
    <span id="SYS_GETPGRP">SYS_GETPGRP</span>                = 111
    <span id="SYS_SETSID">SYS_SETSID</span>                 = 112
    <span id="SYS_SETREUID">SYS_SETREUID</span>               = 113
    <span id="SYS_SETREGID">SYS_SETREGID</span>               = 114
    <span id="SYS_GETGROUPS">SYS_GETGROUPS</span>              = 115
    <span id="SYS_SETGROUPS">SYS_SETGROUPS</span>              = 116
    <span id="SYS_SETRESUID">SYS_SETRESUID</span>              = 117
    <span id="SYS_GETRESUID">SYS_GETRESUID</span>              = 118
    <span id="SYS_SETRESGID">SYS_SETRESGID</span>              = 119
    <span id="SYS_GETRESGID">SYS_GETRESGID</span>              = 120
    <span id="SYS_GETPGID">SYS_GETPGID</span>                = 121
    <span id="SYS_SETFSUID">SYS_SETFSUID</span>               = 122
    <span id="SYS_SETFSGID">SYS_SETFSGID</span>               = 123
    <span id="SYS_GETSID">SYS_GETSID</span>                 = 124
    <span id="SYS_CAPGET">SYS_CAPGET</span>                 = 125
    <span id="SYS_CAPSET">SYS_CAPSET</span>                 = 126
    <span id="SYS_RT_SIGPENDING">SYS_RT_SIGPENDING</span>          = 127
    <span id="SYS_RT_SIGTIMEDWAIT">SYS_RT_SIGTIMEDWAIT</span>        = 128
    <span id="SYS_RT_SIGQUEUEINFO">SYS_RT_SIGQUEUEINFO</span>        = 129
    <span id="SYS_RT_SIGSUSPEND">SYS_RT_SIGSUSPEND</span>          = 130
    <span id="SYS_SIGALTSTACK">SYS_SIGALTSTACK</span>            = 131
    <span id="SYS_UTIME">SYS_UTIME</span>                  = 132
    <span id="SYS_MKNOD">SYS_MKNOD</span>                  = 133
    <span id="SYS_USELIB">SYS_USELIB</span>                 = 134
    <span id="SYS_PERSONALITY">SYS_PERSONALITY</span>            = 135
    <span id="SYS_USTAT">SYS_USTAT</span>                  = 136
    <span id="SYS_STATFS">SYS_STATFS</span>                 = 137
    <span id="SYS_FSTATFS">SYS_FSTATFS</span>                = 138
    <span id="SYS_SYSFS">SYS_SYSFS</span>                  = 139
    <span id="SYS_GETPRIORITY">SYS_GETPRIORITY</span>            = 140
    <span id="SYS_SETPRIORITY">SYS_SETPRIORITY</span>            = 141
    <span id="SYS_SCHED_SETPARAM">SYS_SCHED_SETPARAM</span>         = 142
    <span id="SYS_SCHED_GETPARAM">SYS_SCHED_GETPARAM</span>         = 143
    <span id="SYS_SCHED_SETSCHEDULER">SYS_SCHED_SETSCHEDULER</span>     = 144
    <span id="SYS_SCHED_GETSCHEDULER">SYS_SCHED_GETSCHEDULER</span>     = 145
    <span id="SYS_SCHED_GET_PRIORITY_MAX">SYS_SCHED_GET_PRIORITY_MAX</span> = 146
    <span id="SYS_SCHED_GET_PRIORITY_MIN">SYS_SCHED_GET_PRIORITY_MIN</span> = 147
    <span id="SYS_SCHED_RR_GET_INTERVAL">SYS_SCHED_RR_GET_INTERVAL</span>  = 148
    <span id="SYS_MLOCK">SYS_MLOCK</span>                  = 149
    <span id="SYS_MUNLOCK">SYS_MUNLOCK</span>                = 150
    <span id="SYS_MLOCKALL">SYS_MLOCKALL</span>               = 151
    <span id="SYS_MUNLOCKALL">SYS_MUNLOCKALL</span>             = 152
    <span id="SYS_VHANGUP">SYS_VHANGUP</span>                = 153
    <span id="SYS_MODIFY_LDT">SYS_MODIFY_LDT</span>             = 154
    <span id="SYS_PIVOT_ROOT">SYS_PIVOT_ROOT</span>             = 155
    <span id="SYS__SYSCTL">SYS__SYSCTL</span>                = 156
    <span id="SYS_PRCTL">SYS_PRCTL</span>                  = 157
    <span id="SYS_ARCH_PRCTL">SYS_ARCH_PRCTL</span>             = 158
    <span id="SYS_ADJTIMEX">SYS_ADJTIMEX</span>               = 159
    <span id="SYS_SETRLIMIT">SYS_SETRLIMIT</span>              = 160
    <span id="SYS_CHROOT">SYS_CHROOT</span>                 = 161
    <span id="SYS_SYNC">SYS_SYNC</span>                   = 162
    <span id="SYS_ACCT">SYS_ACCT</span>                   = 163
    <span id="SYS_SETTIMEOFDAY">SYS_SETTIMEOFDAY</span>           = 164
    <span id="SYS_MOUNT">SYS_MOUNT</span>                  = 165
    <span id="SYS_UMOUNT2">SYS_UMOUNT2</span>                = 166
    <span id="SYS_SWAPON">SYS_SWAPON</span>                 = 167
    <span id="SYS_SWAPOFF">SYS_SWAPOFF</span>                = 168
    <span id="SYS_REBOOT">SYS_REBOOT</span>                 = 169
    <span id="SYS_SETHOSTNAME">SYS_SETHOSTNAME</span>            = 170
    <span id="SYS_SETDOMAINNAME">SYS_SETDOMAINNAME</span>          = 171
    <span id="SYS_IOPL">SYS_IOPL</span>                   = 172
    <span id="SYS_IOPERM">SYS_IOPERM</span>                 = 173
    <span id="SYS_CREATE_MODULE">SYS_CREATE_MODULE</span>          = 174
    <span id="SYS_INIT_MODULE">SYS_INIT_MODULE</span>            = 175
    <span id="SYS_DELETE_MODULE">SYS_DELETE_MODULE</span>          = 176
    <span id="SYS_GET_KERNEL_SYMS">SYS_GET_KERNEL_SYMS</span>        = 177
    <span id="SYS_QUERY_MODULE">SYS_QUERY_MODULE</span>           = 178
    <span id="SYS_QUOTACTL">SYS_QUOTACTL</span>               = 179
    <span id="SYS_NFSSERVCTL">SYS_NFSSERVCTL</span>             = 180
    <span id="SYS_GETPMSG">SYS_GETPMSG</span>                = 181
    <span id="SYS_PUTPMSG">SYS_PUTPMSG</span>                = 182
    <span id="SYS_AFS_SYSCALL">SYS_AFS_SYSCALL</span>            = 183
    <span id="SYS_TUXCALL">SYS_TUXCALL</span>                = 184
    <span id="SYS_SECURITY">SYS_SECURITY</span>               = 185
    <span id="SYS_GETTID">SYS_GETTID</span>                 = 186
    <span id="SYS_READAHEAD">SYS_READAHEAD</span>              = 187
    <span id="SYS_SETXATTR">SYS_SETXATTR</span>               = 188
    <span id="SYS_LSETXATTR">SYS_LSETXATTR</span>              = 189
    <span id="SYS_FSETXATTR">SYS_FSETXATTR</span>              = 190
    <span id="SYS_GETXATTR">SYS_GETXATTR</span>               = 191
    <span id="SYS_LGETXATTR">SYS_LGETXATTR</span>              = 192
    <span id="SYS_FGETXATTR">SYS_FGETXATTR</span>              = 193
    <span id="SYS_LISTXATTR">SYS_LISTXATTR</span>              = 194
    <span id="SYS_LLISTXATTR">SYS_LLISTXATTR</span>             = 195
    <span id="SYS_FLISTXATTR">SYS_FLISTXATTR</span>             = 196
    <span id="SYS_REMOVEXATTR">SYS_REMOVEXATTR</span>            = 197
    <span id="SYS_LREMOVEXATTR">SYS_LREMOVEXATTR</span>           = 198
    <span id="SYS_FREMOVEXATTR">SYS_FREMOVEXATTR</span>           = 199
    <span id="SYS_TKILL">SYS_TKILL</span>                  = 200
    <span id="SYS_TIME">SYS_TIME</span>                   = 201
    <span id="SYS_FUTEX">SYS_FUTEX</span>                  = 202
    <span id="SYS_SCHED_SETAFFINITY">SYS_SCHED_SETAFFINITY</span>      = 203
    <span id="SYS_SCHED_GETAFFINITY">SYS_SCHED_GETAFFINITY</span>      = 204
    <span id="SYS_SET_THREAD_AREA">SYS_SET_THREAD_AREA</span>        = 205
    <span id="SYS_IO_SETUP">SYS_IO_SETUP</span>               = 206
    <span id="SYS_IO_DESTROY">SYS_IO_DESTROY</span>             = 207
    <span id="SYS_IO_GETEVENTS">SYS_IO_GETEVENTS</span>           = 208
    <span id="SYS_IO_SUBMIT">SYS_IO_SUBMIT</span>              = 209
    <span id="SYS_IO_CANCEL">SYS_IO_CANCEL</span>              = 210
    <span id="SYS_GET_THREAD_AREA">SYS_GET_THREAD_AREA</span>        = 211
    <span id="SYS_LOOKUP_DCOOKIE">SYS_LOOKUP_DCOOKIE</span>         = 212
    <span id="SYS_EPOLL_CREATE">SYS_EPOLL_CREATE</span>           = 213
    <span id="SYS_EPOLL_CTL_OLD">SYS_EPOLL_CTL_OLD</span>          = 214
    <span id="SYS_EPOLL_WAIT_OLD">SYS_EPOLL_WAIT_OLD</span>         = 215
    <span id="SYS_REMAP_FILE_PAGES">SYS_REMAP_FILE_PAGES</span>       = 216
    <span id="SYS_GETDENTS64">SYS_GETDENTS64</span>             = 217
    <span id="SYS_SET_TID_ADDRESS">SYS_SET_TID_ADDRESS</span>        = 218
    <span id="SYS_RESTART_SYSCALL">SYS_RESTART_SYSCALL</span>        = 219
    <span id="SYS_SEMTIMEDOP">SYS_SEMTIMEDOP</span>             = 220
    <span id="SYS_FADVISE64">SYS_FADVISE64</span>              = 221
    <span id="SYS_TIMER_CREATE">SYS_TIMER_CREATE</span>           = 222
    <span id="SYS_TIMER_SETTIME">SYS_TIMER_SETTIME</span>          = 223
    <span id="SYS_TIMER_GETTIME">SYS_TIMER_GETTIME</span>          = 224
    <span id="SYS_TIMER_GETOVERRUN">SYS_TIMER_GETOVERRUN</span>       = 225
    <span id="SYS_TIMER_DELETE">SYS_TIMER_DELETE</span>           = 226
    <span id="SYS_CLOCK_SETTIME">SYS_CLOCK_SETTIME</span>          = 227
    <span id="SYS_CLOCK_GETTIME">SYS_CLOCK_GETTIME</span>          = 228
    <span id="SYS_CLOCK_GETRES">SYS_CLOCK_GETRES</span>           = 229
    <span id="SYS_CLOCK_NANOSLEEP">SYS_CLOCK_NANOSLEEP</span>        = 230
    <span id="SYS_EXIT_GROUP">SYS_EXIT_GROUP</span>             = 231
    <span id="SYS_EPOLL_WAIT">SYS_EPOLL_WAIT</span>             = 232
    <span id="SYS_EPOLL_CTL">SYS_EPOLL_CTL</span>              = 233
    <span id="SYS_TGKILL">SYS_TGKILL</span>                 = 234
    <span id="SYS_UTIMES">SYS_UTIMES</span>                 = 235
    <span id="SYS_VSERVER">SYS_VSERVER</span>                = 236
    <span id="SYS_MBIND">SYS_MBIND</span>                  = 237
    <span id="SYS_SET_MEMPOLICY">SYS_SET_MEMPOLICY</span>          = 238
    <span id="SYS_GET_MEMPOLICY">SYS_GET_MEMPOLICY</span>          = 239
    <span id="SYS_MQ_OPEN">SYS_MQ_OPEN</span>                = 240
    <span id="SYS_MQ_UNLINK">SYS_MQ_UNLINK</span>              = 241
    <span id="SYS_MQ_TIMEDSEND">SYS_MQ_TIMEDSEND</span>           = 242
    <span id="SYS_MQ_TIMEDRECEIVE">SYS_MQ_TIMEDRECEIVE</span>        = 243
    <span id="SYS_MQ_NOTIFY">SYS_MQ_NOTIFY</span>              = 244
    <span id="SYS_MQ_GETSETATTR">SYS_MQ_GETSETATTR</span>          = 245
    <span id="SYS_KEXEC_LOAD">SYS_KEXEC_LOAD</span>             = 246
    <span id="SYS_WAITID">SYS_WAITID</span>                 = 247
    <span id="SYS_ADD_KEY">SYS_ADD_KEY</span>                = 248
    <span id="SYS_REQUEST_KEY">SYS_REQUEST_KEY</span>            = 249
    <span id="SYS_KEYCTL">SYS_KEYCTL</span>                 = 250
    <span id="SYS_IOPRIO_SET">SYS_IOPRIO_SET</span>             = 251
    <span id="SYS_IOPRIO_GET">SYS_IOPRIO_GET</span>             = 252
    <span id="SYS_INOTIFY_INIT">SYS_INOTIFY_INIT</span>           = 253
    <span id="SYS_INOTIFY_ADD_WATCH">SYS_INOTIFY_ADD_WATCH</span>      = 254
    <span id="SYS_INOTIFY_RM_WATCH">SYS_INOTIFY_RM_WATCH</span>       = 255
    <span id="SYS_MIGRATE_PAGES">SYS_MIGRATE_PAGES</span>          = 256
    <span id="SYS_OPENAT">SYS_OPENAT</span>                 = 257
    <span id="SYS_MKDIRAT">SYS_MKDIRAT</span>                = 258
    <span id="SYS_MKNODAT">SYS_MKNODAT</span>                = 259
    <span id="SYS_FCHOWNAT">SYS_FCHOWNAT</span>               = 260
    <span id="SYS_FUTIMESAT">SYS_FUTIMESAT</span>              = 261
    <span id="SYS_NEWFSTATAT">SYS_NEWFSTATAT</span>             = 262
    <span id="SYS_UNLINKAT">SYS_UNLINKAT</span>               = 263
    <span id="SYS_RENAMEAT">SYS_RENAMEAT</span>               = 264
    <span id="SYS_LINKAT">SYS_LINKAT</span>                 = 265
    <span id="SYS_SYMLINKAT">SYS_SYMLINKAT</span>              = 266
    <span id="SYS_READLINKAT">SYS_READLINKAT</span>             = 267
    <span id="SYS_FCHMODAT">SYS_FCHMODAT</span>               = 268
    <span id="SYS_FACCESSAT">SYS_FACCESSAT</span>              = 269
    <span id="SYS_PSELECT6">SYS_PSELECT6</span>               = 270
    <span id="SYS_PPOLL">SYS_PPOLL</span>                  = 271
    <span id="SYS_UNSHARE">SYS_UNSHARE</span>                = 272
    <span id="SYS_SET_ROBUST_LIST">SYS_SET_ROBUST_LIST</span>        = 273
    <span id="SYS_GET_ROBUST_LIST">SYS_GET_ROBUST_LIST</span>        = 274
    <span id="SYS_SPLICE">SYS_SPLICE</span>                 = 275
    <span id="SYS_TEE">SYS_TEE</span>                    = 276
    <span id="SYS_SYNC_FILE_RANGE">SYS_SYNC_FILE_RANGE</span>        = 277
    <span id="SYS_VMSPLICE">SYS_VMSPLICE</span>               = 278
    <span id="SYS_MOVE_PAGES">SYS_MOVE_PAGES</span>             = 279
    <span id="SYS_UTIMENSAT">SYS_UTIMENSAT</span>              = 280
    <span id="SYS_EPOLL_PWAIT">SYS_EPOLL_PWAIT</span>            = 281
    <span id="SYS_SIGNALFD">SYS_SIGNALFD</span>               = 282
    <span id="SYS_TIMERFD_CREATE">SYS_TIMERFD_CREATE</span>         = 283
    <span id="SYS_EVENTFD">SYS_EVENTFD</span>                = 284
    <span id="SYS_FALLOCATE">SYS_FALLOCATE</span>              = 285
    <span id="SYS_TIMERFD_SETTIME">SYS_TIMERFD_SETTIME</span>        = 286
    <span id="SYS_TIMERFD_GETTIME">SYS_TIMERFD_GETTIME</span>        = 287
    <span id="SYS_ACCEPT4">SYS_ACCEPT4</span>                = 288
    <span id="SYS_SIGNALFD4">SYS_SIGNALFD4</span>              = 289
    <span id="SYS_EVENTFD2">SYS_EVENTFD2</span>               = 290
    <span id="SYS_EPOLL_CREATE1">SYS_EPOLL_CREATE1</span>          = 291
    <span id="SYS_DUP3">SYS_DUP3</span>                   = 292
    <span id="SYS_PIPE2">SYS_PIPE2</span>                  = 293
    <span id="SYS_INOTIFY_INIT1">SYS_INOTIFY_INIT1</span>          = 294
    <span id="SYS_PREADV">SYS_PREADV</span>                 = 295
    <span id="SYS_PWRITEV">SYS_PWRITEV</span>                = 296
    <span id="SYS_RT_TGSIGQUEUEINFO">SYS_RT_TGSIGQUEUEINFO</span>      = 297
    <span id="SYS_PERF_EVENT_OPEN">SYS_PERF_EVENT_OPEN</span>        = 298
    <span id="SYS_RECVMMSG">SYS_RECVMMSG</span>               = 299
    <span id="SYS_FANOTIFY_INIT">SYS_FANOTIFY_INIT</span>          = 300
    <span id="SYS_FANOTIFY_MARK">SYS_FANOTIFY_MARK</span>          = 301
    <span id="SYS_PRLIMIT64">SYS_PRLIMIT64</span>              = 302
)</pre>


<pre>const (
    <span id="SizeofSockaddrInet4">SizeofSockaddrInet4</span>     = 0x10
    <span id="SizeofSockaddrInet6">SizeofSockaddrInet6</span>     = 0x1c
    <span id="SizeofSockaddrAny">SizeofSockaddrAny</span>       = 0x70
    <span id="SizeofSockaddrUnix">SizeofSockaddrUnix</span>      = 0x6e
    <span id="SizeofSockaddrLinklayer">SizeofSockaddrLinklayer</span> = 0x14
    <span id="SizeofSockaddrNetlink">SizeofSockaddrNetlink</span>   = 0xc
    <span id="SizeofLinger">SizeofLinger</span>            = 0x8
    <span id="SizeofIPMreq">SizeofIPMreq</span>            = 0x8
    <span id="SizeofIPMreqn">SizeofIPMreqn</span>           = 0xc
    <span id="SizeofIPv6Mreq">SizeofIPv6Mreq</span>          = 0x14
    <span id="SizeofMsghdr">SizeofMsghdr</span>            = 0x38
    <span id="SizeofCmsghdr">SizeofCmsghdr</span>           = 0x10
    <span id="SizeofInet4Pktinfo">SizeofInet4Pktinfo</span>      = 0xc
    <span id="SizeofInet6Pktinfo">SizeofInet6Pktinfo</span>      = 0x14
    <span id="SizeofIPv6MTUInfo">SizeofIPv6MTUInfo</span>       = 0x20
    <span id="SizeofICMPv6Filter">SizeofICMPv6Filter</span>      = 0x20
    <span id="SizeofUcred">SizeofUcred</span>             = 0xc
    <span id="SizeofTCPInfo">SizeofTCPInfo</span>           = 0x68
)</pre>


<pre>const (
    <span id="IFA_UNSPEC">IFA_UNSPEC</span>          = 0x0
    <span id="IFA_ADDRESS">IFA_ADDRESS</span>         = 0x1
    <span id="IFA_LOCAL">IFA_LOCAL</span>           = 0x2
    <span id="IFA_LABEL">IFA_LABEL</span>           = 0x3
    <span id="IFA_BROADCAST">IFA_BROADCAST</span>       = 0x4
    <span id="IFA_ANYCAST">IFA_ANYCAST</span>         = 0x5
    <span id="IFA_CACHEINFO">IFA_CACHEINFO</span>       = 0x6
    <span id="IFA_MULTICAST">IFA_MULTICAST</span>       = 0x7
    <span id="IFLA_UNSPEC">IFLA_UNSPEC</span>         = 0x0
    <span id="IFLA_ADDRESS">IFLA_ADDRESS</span>        = 0x1
    <span id="IFLA_BROADCAST">IFLA_BROADCAST</span>      = 0x2
    <span id="IFLA_IFNAME">IFLA_IFNAME</span>         = 0x3
    <span id="IFLA_MTU">IFLA_MTU</span>            = 0x4
    <span id="IFLA_LINK">IFLA_LINK</span>           = 0x5
    <span id="IFLA_QDISC">IFLA_QDISC</span>          = 0x6
    <span id="IFLA_STATS">IFLA_STATS</span>          = 0x7
    <span id="IFLA_COST">IFLA_COST</span>           = 0x8
    <span id="IFLA_PRIORITY">IFLA_PRIORITY</span>       = 0x9
    <span id="IFLA_MASTER">IFLA_MASTER</span>         = 0xa
    <span id="IFLA_WIRELESS">IFLA_WIRELESS</span>       = 0xb
    <span id="IFLA_PROTINFO">IFLA_PROTINFO</span>       = 0xc
    <span id="IFLA_TXQLEN">IFLA_TXQLEN</span>         = 0xd
    <span id="IFLA_MAP">IFLA_MAP</span>            = 0xe
    <span id="IFLA_WEIGHT">IFLA_WEIGHT</span>         = 0xf
    <span id="IFLA_OPERSTATE">IFLA_OPERSTATE</span>      = 0x10
    <span id="IFLA_LINKMODE">IFLA_LINKMODE</span>       = 0x11
    <span id="IFLA_LINKINFO">IFLA_LINKINFO</span>       = 0x12
    <span id="IFLA_NET_NS_PID">IFLA_NET_NS_PID</span>     = 0x13
    <span id="IFLA_IFALIAS">IFLA_IFALIAS</span>        = 0x14
    <span id="IFLA_MAX">IFLA_MAX</span>            = 0x1d
    <span id="RT_SCOPE_UNIVERSE">RT_SCOPE_UNIVERSE</span>   = 0x0
    <span id="RT_SCOPE_SITE">RT_SCOPE_SITE</span>       = 0xc8
    <span id="RT_SCOPE_LINK">RT_SCOPE_LINK</span>       = 0xfd
    <span id="RT_SCOPE_HOST">RT_SCOPE_HOST</span>       = 0xfe
    <span id="RT_SCOPE_NOWHERE">RT_SCOPE_NOWHERE</span>    = 0xff
    <span id="RT_TABLE_UNSPEC">RT_TABLE_UNSPEC</span>     = 0x0
    <span id="RT_TABLE_COMPAT">RT_TABLE_COMPAT</span>     = 0xfc
    <span id="RT_TABLE_DEFAULT">RT_TABLE_DEFAULT</span>    = 0xfd
    <span id="RT_TABLE_MAIN">RT_TABLE_MAIN</span>       = 0xfe
    <span id="RT_TABLE_LOCAL">RT_TABLE_LOCAL</span>      = 0xff
    <span id="RT_TABLE_MAX">RT_TABLE_MAX</span>        = 0xffffffff
    <span id="RTA_UNSPEC">RTA_UNSPEC</span>          = 0x0
    <span id="RTA_DST">RTA_DST</span>             = 0x1
    <span id="RTA_SRC">RTA_SRC</span>             = 0x2
    <span id="RTA_IIF">RTA_IIF</span>             = 0x3
    <span id="RTA_OIF">RTA_OIF</span>             = 0x4
    <span id="RTA_GATEWAY">RTA_GATEWAY</span>         = 0x5
    <span id="RTA_PRIORITY">RTA_PRIORITY</span>        = 0x6
    <span id="RTA_PREFSRC">RTA_PREFSRC</span>         = 0x7
    <span id="RTA_METRICS">RTA_METRICS</span>         = 0x8
    <span id="RTA_MULTIPATH">RTA_MULTIPATH</span>       = 0x9
    <span id="RTA_FLOW">RTA_FLOW</span>            = 0xb
    <span id="RTA_CACHEINFO">RTA_CACHEINFO</span>       = 0xc
    <span id="RTA_TABLE">RTA_TABLE</span>           = 0xf
    <span id="RTN_UNSPEC">RTN_UNSPEC</span>          = 0x0
    <span id="RTN_UNICAST">RTN_UNICAST</span>         = 0x1
    <span id="RTN_LOCAL">RTN_LOCAL</span>           = 0x2
    <span id="RTN_BROADCAST">RTN_BROADCAST</span>       = 0x3
    <span id="RTN_ANYCAST">RTN_ANYCAST</span>         = 0x4
    <span id="RTN_MULTICAST">RTN_MULTICAST</span>       = 0x5
    <span id="RTN_BLACKHOLE">RTN_BLACKHOLE</span>       = 0x6
    <span id="RTN_UNREACHABLE">RTN_UNREACHABLE</span>     = 0x7
    <span id="RTN_PROHIBIT">RTN_PROHIBIT</span>        = 0x8
    <span id="RTN_THROW">RTN_THROW</span>           = 0x9
    <span id="RTN_NAT">RTN_NAT</span>             = 0xa
    <span id="RTN_XRESOLVE">RTN_XRESOLVE</span>        = 0xb
    <span id="RTNLGRP_NONE">RTNLGRP_NONE</span>        = 0x0
    <span id="RTNLGRP_LINK">RTNLGRP_LINK</span>        = 0x1
    <span id="RTNLGRP_NOTIFY">RTNLGRP_NOTIFY</span>      = 0x2
    <span id="RTNLGRP_NEIGH">RTNLGRP_NEIGH</span>       = 0x3
    <span id="RTNLGRP_TC">RTNLGRP_TC</span>          = 0x4
    <span id="RTNLGRP_IPV4_IFADDR">RTNLGRP_IPV4_IFADDR</span> = 0x5
    <span id="RTNLGRP_IPV4_MROUTE">RTNLGRP_IPV4_MROUTE</span> = 0x6
    <span id="RTNLGRP_IPV4_ROUTE">RTNLGRP_IPV4_ROUTE</span>  = 0x7
    <span id="RTNLGRP_IPV4_RULE">RTNLGRP_IPV4_RULE</span>   = 0x8
    <span id="RTNLGRP_IPV6_IFADDR">RTNLGRP_IPV6_IFADDR</span> = 0x9
    <span id="RTNLGRP_IPV6_MROUTE">RTNLGRP_IPV6_MROUTE</span> = 0xa
    <span id="RTNLGRP_IPV6_ROUTE">RTNLGRP_IPV6_ROUTE</span>  = 0xb
    <span id="RTNLGRP_IPV6_IFINFO">RTNLGRP_IPV6_IFINFO</span> = 0xc
    <span id="RTNLGRP_IPV6_PREFIX">RTNLGRP_IPV6_PREFIX</span> = 0x12
    <span id="RTNLGRP_IPV6_RULE">RTNLGRP_IPV6_RULE</span>   = 0x13
    <span id="RTNLGRP_ND_USEROPT">RTNLGRP_ND_USEROPT</span>  = 0x14
    <span id="SizeofNlMsghdr">SizeofNlMsghdr</span>      = 0x10
    <span id="SizeofNlMsgerr">SizeofNlMsgerr</span>      = 0x14
    <span id="SizeofRtGenmsg">SizeofRtGenmsg</span>      = 0x1
    <span id="SizeofNlAttr">SizeofNlAttr</span>        = 0x4
    <span id="SizeofRtAttr">SizeofRtAttr</span>        = 0x4
    <span id="SizeofIfInfomsg">SizeofIfInfomsg</span>     = 0x10
    <span id="SizeofIfAddrmsg">SizeofIfAddrmsg</span>     = 0x8
    <span id="SizeofRtMsg">SizeofRtMsg</span>         = 0xc
    <span id="SizeofRtNexthop">SizeofRtNexthop</span>     = 0x8
)</pre>


<pre>const (
    <span id="SizeofSockFilter">SizeofSockFilter</span> = 0x8
    <span id="SizeofSockFprog">SizeofSockFprog</span>  = 0x10
)</pre>


<pre>const (
    <span id="VINTR">VINTR</span>    = 0x0
    <span id="VQUIT">VQUIT</span>    = 0x1
    <span id="VERASE">VERASE</span>   = 0x2
    <span id="VKILL">VKILL</span>    = 0x3
    <span id="VEOF">VEOF</span>     = 0x4
    <span id="VTIME">VTIME</span>    = 0x5
    <span id="VMIN">VMIN</span>     = 0x6
    <span id="VSWTC">VSWTC</span>    = 0x7
    <span id="VSTART">VSTART</span>   = 0x8
    <span id="VSTOP">VSTOP</span>    = 0x9
    <span id="VSUSP">VSUSP</span>    = 0xa
    <span id="VEOL">VEOL</span>     = 0xb
    <span id="VREPRINT">VREPRINT</span> = 0xc
    <span id="VDISCARD">VDISCARD</span> = 0xd
    <span id="VWERASE">VWERASE</span>  = 0xe
    <span id="VLNEXT">VLNEXT</span>   = 0xf
    <span id="VEOL2">VEOL2</span>    = 0x10
    <span id="IGNBRK">IGNBRK</span>   = 0x1
    <span id="BRKINT">BRKINT</span>   = 0x2
    <span id="IGNPAR">IGNPAR</span>   = 0x4
    <span id="PARMRK">PARMRK</span>   = 0x8
    <span id="INPCK">INPCK</span>    = 0x10
    <span id="ISTRIP">ISTRIP</span>   = 0x20
    <span id="INLCR">INLCR</span>    = 0x40
    <span id="IGNCR">IGNCR</span>    = 0x80
    <span id="ICRNL">ICRNL</span>    = 0x100
    <span id="IUCLC">IUCLC</span>    = 0x200
    <span id="IXON">IXON</span>     = 0x400
    <span id="IXANY">IXANY</span>    = 0x800
    <span id="IXOFF">IXOFF</span>    = 0x1000
    <span id="IMAXBEL">IMAXBEL</span>  = 0x2000
    <span id="IUTF8">IUTF8</span>    = 0x4000
    <span id="OPOST">OPOST</span>    = 0x1
    <span id="OLCUC">OLCUC</span>    = 0x2
    <span id="ONLCR">ONLCR</span>    = 0x4
    <span id="OCRNL">OCRNL</span>    = 0x8
    <span id="ONOCR">ONOCR</span>    = 0x10
    <span id="ONLRET">ONLRET</span>   = 0x20
    <span id="OFILL">OFILL</span>    = 0x40
    <span id="OFDEL">OFDEL</span>    = 0x80
    <span id="B0">B0</span>       = 0x0
    <span id="B50">B50</span>      = 0x1
    <span id="B75">B75</span>      = 0x2
    <span id="B110">B110</span>     = 0x3
    <span id="B134">B134</span>     = 0x4
    <span id="B150">B150</span>     = 0x5
    <span id="B200">B200</span>     = 0x6
    <span id="B300">B300</span>     = 0x7
    <span id="B600">B600</span>     = 0x8
    <span id="B1200">B1200</span>    = 0x9
    <span id="B1800">B1800</span>    = 0xa
    <span id="B2400">B2400</span>    = 0xb
    <span id="B4800">B4800</span>    = 0xc
    <span id="B9600">B9600</span>    = 0xd
    <span id="B19200">B19200</span>   = 0xe
    <span id="B38400">B38400</span>   = 0xf
    <span id="CSIZE">CSIZE</span>    = 0x30
    <span id="CS5">CS5</span>      = 0x0
    <span id="CS6">CS6</span>      = 0x10
    <span id="CS7">CS7</span>      = 0x20
    <span id="CS8">CS8</span>      = 0x30
    <span id="CSTOPB">CSTOPB</span>   = 0x40
    <span id="CREAD">CREAD</span>    = 0x80
    <span id="PARENB">PARENB</span>   = 0x100
    <span id="PARODD">PARODD</span>   = 0x200
    <span id="HUPCL">HUPCL</span>    = 0x400
    <span id="CLOCAL">CLOCAL</span>   = 0x800
    <span id="B57600">B57600</span>   = 0x1001
    <span id="B115200">B115200</span>  = 0x1002
    <span id="B230400">B230400</span>  = 0x1003
    <span id="B460800">B460800</span>  = 0x1004
    <span id="B500000">B500000</span>  = 0x1005
    <span id="B576000">B576000</span>  = 0x1006
    <span id="B921600">B921600</span>  = 0x1007
    <span id="B1000000">B1000000</span> = 0x1008
    <span id="B1152000">B1152000</span> = 0x1009
    <span id="B1500000">B1500000</span> = 0x100a
    <span id="B2000000">B2000000</span> = 0x100b
    <span id="B2500000">B2500000</span> = 0x100c
    <span id="B3000000">B3000000</span> = 0x100d
    <span id="B3500000">B3500000</span> = 0x100e
    <span id="B4000000">B4000000</span> = 0x100f
    <span id="ISIG">ISIG</span>     = 0x1
    <span id="ICANON">ICANON</span>   = 0x2
    <span id="XCASE">XCASE</span>    = 0x4
    <span id="ECHO">ECHO</span>     = 0x8
    <span id="ECHOE">ECHOE</span>    = 0x10
    <span id="ECHOK">ECHOK</span>    = 0x20
    <span id="ECHONL">ECHONL</span>   = 0x40
    <span id="NOFLSH">NOFLSH</span>   = 0x80
    <span id="TOSTOP">TOSTOP</span>   = 0x100
    <span id="ECHOCTL">ECHOCTL</span>  = 0x200
    <span id="ECHOPRT">ECHOPRT</span>  = 0x400
    <span id="ECHOKE">ECHOKE</span>   = 0x800
    <span id="FLUSHO">FLUSHO</span>   = 0x1000
    <span id="PENDIN">PENDIN</span>   = 0x4000
    <span id="IEXTEN">IEXTEN</span>   = 0x8000
    <span id="TCGETS">TCGETS</span>   = 0x5401
    <span id="TCSETS">TCSETS</span>   = 0x5402
)</pre>


<pre>const <span id="ImplementsGetwd">ImplementsGetwd</span> = <a href="/builtin/#true">true</a></pre>


<pre>const (
    <span id="PathMax">PathMax</span> = 0x1000
)</pre>


<pre>const <span id="SizeofInotifyEvent">SizeofInotifyEvent</span> = 0x10</pre>


<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="Stdin">Stdin</span>  = 0
    <span id="Stdout">Stdout</span> = 1
    <span id="Stderr">Stderr</span> = 2
)</pre>


<pre>var <span id="ForkLock">ForkLock</span> <a href="/sync/">sync</a>.<a href="/sync/#RWMutex">RWMutex</a></pre>


<pre>var <span id="SocketDisableIPv6">SocketDisableIPv6</span> <a href="/builtin/#bool">bool</a></pre>

For testing: clients can set this flag to force creation of IPv6 sockets to
return EAFNOSUPPORT.

<h2 id="Accept">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L426">Accept</a>
    <a href="#Accept">¶</a></h2>
<pre>func Accept(fd <a href="/builtin/#int">int</a>) (nfd <a href="/builtin/#int">int</a>, sa <a href="#Sockaddr">Sockaddr</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Accept4">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L441">Accept4</a>
    <a href="#Accept4">¶</a></h2>
<pre>func Accept4(fd <a href="/builtin/#int">int</a>, flags <a href="/builtin/#int">int</a>) (nfd <a href="/builtin/#int">int</a>, sa <a href="#Sockaddr">Sockaddr</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Access">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L10">Access</a>
    <a href="#Access">¶</a></h2>
<pre>func Access(path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#uint32">uint32</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Acct">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L213">Acct</a>
    <a href="#Acct">¶</a></h2>
<pre>func Acct(path <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Adjtimex">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L228">Adjtimex</a>
    <a href="#Adjtimex">¶</a></h2>
<pre>func Adjtimex(buf *<a href="#Timex">Timex</a>) (state <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="AttachLsf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/lsf_linux.go#L63">AttachLsf</a>
    <a href="#AttachLsf">¶</a></h2>
<pre>func AttachLsf(fd <a href="/builtin/#int">int</a>, i []<a href="#SockFilter">SockFilter</a>) <a href="/builtin/#error">error</a></pre>

Deprecated: Use golang.org/x/net/bpf instead.

<h2 id="Bind">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L207">Bind</a>
    <a href="#Bind">¶</a></h2>
<pre>func Bind(fd <a href="/builtin/#int">int</a>, sa <a href="#Sockaddr">Sockaddr</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="BindToDevice">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L609">BindToDevice</a>
    <a href="#BindToDevice">¶</a></h2>
<pre>func BindToDevice(fd <a href="/builtin/#int">int</a>, device <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>

BindToDevice binds the socket associated with fd to device.

<h2 id="BytePtrFromString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go#L61">BytePtrFromString</a>
    <a href="#BytePtrFromString">¶</a></h2>
<pre>func BytePtrFromString(s <a href="/builtin/#string">string</a>) (*<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

BytePtrFromString returns a pointer to a NUL-terminated array of bytes
containing the text of s. If s contains a NUL byte at any location, it returns
(nil, EINVAL).

<h2 id="ByteSliceFromString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go#L40">ByteSliceFromString</a>
    <a href="#ByteSliceFromString">¶</a></h2>
<pre>func ByteSliceFromString(s <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

ByteSliceFromString returns a NUL-terminated slice of bytes containing the text
of s. If s contains a NUL byte at any location, it returns (nil, EINVAL).

<h2 id="Chdir">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L239">Chdir</a>
    <a href="#Chdir">¶</a></h2>
<pre>func Chdir(path <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Chmod">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L14">Chmod</a>
    <a href="#Chmod">¶</a></h2>
<pre>func Chmod(path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#uint32">uint32</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Chown">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L18">Chown</a>
    <a href="#Chown">¶</a></h2>
<pre>func Chown(path <a href="/builtin/#string">string</a>, uid <a href="/builtin/#int">int</a>, gid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Chroot">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L254">Chroot</a>
    <a href="#Chroot">¶</a></h2>
<pre>func Chroot(path <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Clearenv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/env_unix.go#L115">Clearenv</a>
    <a href="#Clearenv">¶</a></h2>
<pre>func Clearenv()</pre>


<h2 id="Close">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L269">Close</a>
    <a href="#Close">¶</a></h2>
<pre>func Close(fd <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="CloseOnExec">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_unix.go#L86">CloseOnExec</a>
    <a href="#CloseOnExec">¶</a></h2>
<pre>func CloseOnExec(fd <a href="/builtin/#int">int</a>)</pre>


<h2 id="CmsgLen">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/sockcmsg_unix.go#L17">CmsgLen</a>
    <a href="#CmsgLen">¶</a></h2>
<pre>func CmsgLen(datalen <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

CmsgLen returns the value to store in the Len field of the Cmsghdr structure,
taking into account any necessary alignment.

<h2 id="CmsgSpace">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/sockcmsg_unix.go#L23">CmsgSpace</a>
    <a href="#CmsgSpace">¶</a></h2>
<pre>func CmsgSpace(datalen <a href="/builtin/#int">int</a>) <a href="/builtin/#int">int</a></pre>

CmsgSpace returns the number of bytes an ancillary element with payload of the
passed data length occupies.

<h2 id="Connect">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L215">Connect</a>
    <a href="#Connect">¶</a></h2>
<pre>func Connect(fd <a href="/builtin/#int">int</a>, sa <a href="#Sockaddr">Sockaddr</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Creat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L22">Creat</a>
    <a href="#Creat">¶</a></h2>
<pre>func Creat(path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#uint32">uint32</a>) (fd <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="DetachLsf">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/lsf_linux.go#L71">DetachLsf</a>
    <a href="#DetachLsf">¶</a></h2>
<pre>func DetachLsf(fd <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

Deprecated: Use golang.org/x/net/bpf instead.

<h2 id="Dup">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L279">Dup</a>
    <a href="#Dup">¶</a></h2>
<pre>func Dup(oldfd <a href="/builtin/#int">int</a>) (fd <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Dup2">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1148">Dup2</a>
    <a href="#Dup2">¶</a></h2>
<pre>func Dup2(oldfd <a href="/builtin/#int">int</a>, newfd <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Dup3">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L290">Dup3</a>
    <a href="#Dup3">¶</a></h2>
<pre>func Dup3(oldfd <a href="/builtin/#int">int</a>, newfd <a href="/builtin/#int">int</a>, flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Environ">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/env_unix.go#L128">Environ</a>
    <a href="#Environ">¶</a></h2>
<pre>func Environ() []<a href="/builtin/#string">string</a></pre>


<h2 id="EpollCreate">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L300">EpollCreate</a>
    <a href="#EpollCreate">¶</a></h2>
<pre>func EpollCreate(size <a href="/builtin/#int">int</a>) (fd <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="EpollCreate1">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L311">EpollCreate1</a>
    <a href="#EpollCreate1">¶</a></h2>
<pre>func EpollCreate1(flag <a href="/builtin/#int">int</a>) (fd <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="EpollCtl">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L322">EpollCtl</a>
    <a href="#EpollCtl">¶</a></h2>
<pre>func EpollCtl(epfd <a href="/builtin/#int">int</a>, op <a href="/builtin/#int">int</a>, fd <a href="/builtin/#int">int</a>, event *<a href="#EpollEvent">EpollEvent</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="EpollWait">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L332">EpollWait</a>
    <a href="#EpollWait">¶</a></h2>
<pre>func EpollWait(epfd <a href="/builtin/#int">int</a>, events []<a href="#EpollEvent">EpollEvent</a>, msec <a href="/builtin/#int">int</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Exec">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_unix.go#L244">Exec</a>
    <a href="#Exec">¶</a></h2>
<pre>func Exec(argv0 <a href="/builtin/#string">string</a>, argv []<a href="/builtin/#string">string</a>, envv []<a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>

Exec invokes the execve(2) system call.

<h2 id="Exit">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go#L98">Exit</a>
    <a href="#Exit">¶</a></h2>
<pre>func Exit(code <a href="/builtin/#int">int</a>)</pre>


<h2 id="Faccessat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L349">Faccessat</a>
    <a href="#Faccessat">¶</a></h2>
<pre>func Faccessat(dirfd <a href="/builtin/#int">int</a>, path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#uint32">uint32</a>, flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Fallocate">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L364">Fallocate</a>
    <a href="#Fallocate">¶</a></h2>
<pre>func Fallocate(fd <a href="/builtin/#int">int</a>, mode <a href="/builtin/#uint32">uint32</a>, off <a href="/builtin/#int64">int64</a>, len <a href="/builtin/#int64">int64</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Fchdir">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L374">Fchdir</a>
    <a href="#Fchdir">¶</a></h2>
<pre>func Fchdir(fd <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Fchmod">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L384">Fchmod</a>
    <a href="#Fchmod">¶</a></h2>
<pre>func Fchmod(fd <a href="/builtin/#int">int</a>, mode <a href="/builtin/#uint32">uint32</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Fchmodat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L394">Fchmodat</a>
    <a href="#Fchmodat">¶</a></h2>
<pre>func Fchmodat(dirfd <a href="/builtin/#int">int</a>, path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#uint32">uint32</a>, flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Fchown">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1158">Fchown</a>
    <a href="#Fchown">¶</a></h2>
<pre>func Fchown(fd <a href="/builtin/#int">int</a>, uid <a href="/builtin/#int">int</a>, gid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Fchownat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L409">Fchownat</a>
    <a href="#Fchownat">¶</a></h2>
<pre>func Fchownat(dirfd <a href="/builtin/#int">int</a>, path <a href="/builtin/#string">string</a>, uid <a href="/builtin/#int">int</a>, gid <a href="/builtin/#int">int</a>, flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="FcntlFlock">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/flock.go#L6">FcntlFlock</a>
    <a href="#FcntlFlock">¶</a></h2>
<pre>func FcntlFlock(fd <a href="/builtin/#uintptr">uintptr</a>, cmd <a href="/builtin/#int">int</a>, lk *<a href="#Flock_t">Flock_t</a>) <a href="/builtin/#error">error</a></pre>

FcntlFlock performs a fcntl syscall for the F_GETLK, F_SETLK or F_SETLKW
command.

<h2 id="Fdatasync">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L435">Fdatasync</a>
    <a href="#Fdatasync">¶</a></h2>
<pre>func Fdatasync(fd <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Flock">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L445">Flock</a>
    <a href="#Flock">¶</a></h2>
<pre>func Flock(fd <a href="/builtin/#int">int</a>, how <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="ForkExec">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_unix.go#L225">ForkExec</a>
    <a href="#ForkExec">¶</a></h2>
<pre>func ForkExec(argv0 <a href="/builtin/#string">string</a>, argv []<a href="/builtin/#string">string</a>, attr *<a href="#ProcAttr">ProcAttr</a>) (pid <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Combination of fork and exec, careful to be thread safe.

<h2 id="Fstat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1168">Fstat</a>
    <a href="#Fstat">¶</a></h2>
<pre>func Fstat(fd <a href="/builtin/#int">int</a>, stat *<a href="#Stat_t">Stat_t</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Fstatfs">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1178">Fstatfs</a>
    <a href="#Fstatfs">¶</a></h2>
<pre>func Fstatfs(fd <a href="/builtin/#int">int</a>, buf *<a href="#Statfs_t">Statfs_t</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Fsync">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L455">Fsync</a>
    <a href="#Fsync">¶</a></h2>
<pre>func Fsync(fd <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Ftruncate">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1188">Ftruncate</a>
    <a href="#Ftruncate">¶</a></h2>
<pre>func Ftruncate(fd <a href="/builtin/#int">int</a>, length <a href="/builtin/#int64">int64</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Futimes">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L122">Futimes</a>
    <a href="#Futimes">¶</a></h2>
<pre>func Futimes(fd <a href="/builtin/#int">int</a>, tv []<a href="#Timeval">Timeval</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Futimesat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L111">Futimesat</a>
    <a href="#Futimesat">¶</a></h2>
<pre>func Futimesat(dirfd <a href="/builtin/#int">int</a>, path <a href="/builtin/#string">string</a>, tv []<a href="#Timeval">Timeval</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Getcwd">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L135">Getcwd</a>
    <a href="#Getcwd">¶</a></h2>
<pre>func Getcwd(buf []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Getdents">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L465">Getdents</a>
    <a href="#Getdents">¶</a></h2>
<pre>func Getdents(fd <a href="/builtin/#int">int</a>, buf []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Getegid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1198">Getegid</a>
    <a href="#Getegid">¶</a></h2>
<pre>func Getegid() (egid <a href="/builtin/#int">int</a>)</pre>


<h2 id="Getenv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/env_unix.go#L61">Getenv</a>
    <a href="#Getenv">¶</a></h2>
<pre>func Getenv(key <a href="/builtin/#string">string</a>) (value <a href="/builtin/#string">string</a>, found <a href="/builtin/#bool">bool</a>)</pre>


<h2 id="Geteuid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1206">Geteuid</a>
    <a href="#Geteuid">¶</a></h2>
<pre>func Geteuid() (euid <a href="/builtin/#int">int</a>)</pre>


<h2 id="Getgid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1214">Getgid</a>
    <a href="#Getgid">¶</a></h2>
<pre>func Getgid() (gid <a href="/builtin/#int">int</a>)</pre>


<h2 id="Getgroups">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L145">Getgroups</a>
    <a href="#Getgroups">¶</a></h2>
<pre>func Getgroups() (gids []<a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Getpagesize">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go#L97">Getpagesize</a>
    <a href="#Getpagesize">¶</a></h2>
<pre>func Getpagesize() <a href="/builtin/#int">int</a></pre>


<h2 id="Getpgid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L482">Getpgid</a>
    <a href="#Getpgid">¶</a></h2>
<pre>func Getpgid(pid <a href="/builtin/#int">int</a>) (pgid <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Getpgrp">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L827">Getpgrp</a>
    <a href="#Getpgrp">¶</a></h2>
<pre>func Getpgrp() (pid <a href="/builtin/#int">int</a>)</pre>


<h2 id="Getpid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L493">Getpid</a>
    <a href="#Getpid">¶</a></h2>
<pre>func Getpid() (pid <a href="/builtin/#int">int</a>)</pre>


<h2 id="Getppid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L501">Getppid</a>
    <a href="#Getppid">¶</a></h2>
<pre>func Getppid() (ppid <a href="/builtin/#int">int</a>)</pre>


<h2 id="Getpriority">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L509">Getpriority</a>
    <a href="#Getpriority">¶</a></h2>
<pre>func Getpriority(which <a href="/builtin/#int">int</a>, who <a href="/builtin/#int">int</a>) (prio <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Getrlimit">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1222">Getrlimit</a>
    <a href="#Getrlimit">¶</a></h2>
<pre>func Getrlimit(resource <a href="/builtin/#int">int</a>, rlim *<a href="#Rlimit">Rlimit</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Getrusage">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L520">Getrusage</a>
    <a href="#Getrusage">¶</a></h2>
<pre>func Getrusage(who <a href="/builtin/#int">int</a>, rusage *<a href="#Rusage">Rusage</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="GetsockoptInet4Addr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L468">GetsockoptInet4Addr</a>
    <a href="#GetsockoptInet4Addr">¶</a></h2>
<pre>func GetsockoptInet4Addr(fd, level, opt <a href="/builtin/#int">int</a>) (value [4]<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="GetsockoptInt">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L232">GetsockoptInt</a>
    <a href="#GetsockoptInt">¶</a></h2>
<pre>func GetsockoptInt(fd, level, opt <a href="/builtin/#int">int</a>) (value <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Gettid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L530">Gettid</a>
    <a href="#Gettid">¶</a></h2>
<pre>func Gettid() (tid <a href="/builtin/#int">int</a>)</pre>


<h2 id="Gettimeofday">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux_amd64.go#L57">Gettimeofday</a>
    <a href="#Gettimeofday">¶</a></h2>
<pre>func Gettimeofday(tv *<a href="#Timeval">Timeval</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Getuid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1232">Getuid</a>
    <a href="#Getuid">¶</a></h2>
<pre>func Getuid() (uid <a href="/builtin/#int">int</a>)</pre>


<h2 id="Getwd">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L132">Getwd</a>
    <a href="#Getwd">¶</a></h2>
<pre>func Getwd() (wd <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Getxattr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L538">Getxattr</a>
    <a href="#Getxattr">¶</a></h2>
<pre>func Getxattr(path <a href="/builtin/#string">string</a>, attr <a href="/builtin/#string">string</a>, dest []<a href="/builtin/#byte">byte</a>) (sz <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="InotifyAddWatch">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L565">InotifyAddWatch</a>
    <a href="#InotifyAddWatch">¶</a></h2>
<pre>func InotifyAddWatch(fd <a href="/builtin/#int">int</a>, pathname <a href="/builtin/#string">string</a>, mask <a href="/builtin/#uint32">uint32</a>) (watchdesc <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="InotifyInit">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1240">InotifyInit</a>
    <a href="#InotifyInit">¶</a></h2>
<pre>func InotifyInit() (fd <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="InotifyInit1">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L581">InotifyInit1</a>
    <a href="#InotifyInit1">¶</a></h2>
<pre>func InotifyInit1(flags <a href="/builtin/#int">int</a>) (fd <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="InotifyRmWatch">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L592">InotifyRmWatch</a>
    <a href="#InotifyRmWatch">¶</a></h2>
<pre>func InotifyRmWatch(fd <a href="/builtin/#int">int</a>, watchdesc <a href="/builtin/#uint32">uint32</a>) (success <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Ioperm">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1251">Ioperm</a>
    <a href="#Ioperm">¶</a></h2>
<pre>func Ioperm(from <a href="/builtin/#int">int</a>, num <a href="/builtin/#int">int</a>, on <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Iopl">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1261">Iopl</a>
    <a href="#Iopl">¶</a></h2>
<pre>func Iopl(level <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Kill">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L603">Kill</a>
    <a href="#Kill">¶</a></h2>
<pre>func Kill(pid <a href="/builtin/#int">int</a>, sig <a href="#Signal">Signal</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Klogctl">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L613">Klogctl</a>
    <a href="#Klogctl">¶</a></h2>
<pre>func Klogctl(typ <a href="/builtin/#int">int</a>, buf []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Lchown">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1271">Lchown</a>
    <a href="#Lchown">¶</a></h2>
<pre>func Lchown(path <a href="/builtin/#string">string</a>, uid <a href="/builtin/#int">int</a>, gid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Link">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L28">Link</a>
    <a href="#Link">¶</a></h2>
<pre>func Link(oldpath <a href="/builtin/#string">string</a>, newpath <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Listen">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1286">Listen</a>
    <a href="#Listen">¶</a></h2>
<pre>func Listen(s <a href="/builtin/#int">int</a>, n <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Listxattr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L630">Listxattr</a>
    <a href="#Listxattr">¶</a></h2>
<pre>func Listxattr(path <a href="/builtin/#string">string</a>, dest []<a href="/builtin/#byte">byte</a>) (sz <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="LsfSocket">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/lsf_linux.go#L14">LsfSocket</a>
    <a href="#LsfSocket">¶</a></h2>
<pre>func LsfSocket(ifindex, proto <a href="/builtin/#int">int</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Deprecated: Use golang.org/x/net/bpf instead.

<h2 id="Lstat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1296">Lstat</a>
    <a href="#Lstat">¶</a></h2>
<pre>func Lstat(path <a href="/builtin/#string">string</a>, stat *<a href="#Stat_t">Stat_t</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Madvise">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1064">Madvise</a>
    <a href="#Madvise">¶</a></h2>
<pre>func Madvise(b []<a href="/builtin/#byte">byte</a>, advice <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Mkdir">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L32">Mkdir</a>
    <a href="#Mkdir">¶</a></h2>
<pre>func Mkdir(path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#uint32">uint32</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Mkdirat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L652">Mkdirat</a>
    <a href="#Mkdirat">¶</a></h2>
<pre>func Mkdirat(dirfd <a href="/builtin/#int">int</a>, path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#uint32">uint32</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Mkfifo">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L251">Mkfifo</a>
    <a href="#Mkfifo">¶</a></h2>
<pre>func Mkfifo(path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#uint32">uint32</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Mknod">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L36">Mknod</a>
    <a href="#Mknod">¶</a></h2>
<pre>func Mknod(path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#uint32">uint32</a>, dev <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Mknodat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L667">Mknodat</a>
    <a href="#Mknodat">¶</a></h2>
<pre>func Mknodat(dirfd <a href="/builtin/#int">int</a>, path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#uint32">uint32</a>, dev <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Mlock">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1096">Mlock</a>
    <a href="#Mlock">¶</a></h2>
<pre>func Mlock(b []<a href="/builtin/#byte">byte</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Mlockall">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1128">Mlockall</a>
    <a href="#Mlockall">¶</a></h2>
<pre>func Mlockall(flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Mmap">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L899">Mmap</a>
    <a href="#Mmap">¶</a></h2>
<pre>func Mmap(fd <a href="/builtin/#int">int</a>, offset <a href="/builtin/#int64">int64</a>, length <a href="/builtin/#int">int</a>, prot <a href="/builtin/#int">int</a>, flags <a href="/builtin/#int">int</a>) (data []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Mount">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L783">Mount</a>
    <a href="#Mount">¶</a></h2>
<pre>func Mount(source <a href="/builtin/#string">string</a>, target <a href="/builtin/#string">string</a>, fstype <a href="/builtin/#string">string</a>, flags <a href="/builtin/#uintptr">uintptr</a>, data <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Mprotect">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1080">Mprotect</a>
    <a href="#Mprotect">¶</a></h2>
<pre>func Mprotect(b []<a href="/builtin/#byte">byte</a>, prot <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Munlock">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1112">Munlock</a>
    <a href="#Munlock">¶</a></h2>
<pre>func Munlock(b []<a href="/builtin/#byte">byte</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Munlockall">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1138">Munlockall</a>
    <a href="#Munlockall">¶</a></h2>
<pre>func Munlockall() (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Munmap">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L903">Munmap</a>
    <a href="#Munmap">¶</a></h2>
<pre>func Munmap(b []<a href="/builtin/#byte">byte</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Nanosleep">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L682">Nanosleep</a>
    <a href="#Nanosleep">¶</a></h2>
<pre>func Nanosleep(time *<a href="#Timespec">Timespec</a>, leftover *<a href="#Timespec">Timespec</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="NetlinkRIB">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/netlink_linux.go#L42">NetlinkRIB</a>
    <a href="#NetlinkRIB">¶</a></h2>
<pre>func NetlinkRIB(proto, family <a href="/builtin/#int">int</a>) ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

NetlinkRIB returns routing information base, as known as RIB, which consists of
network facility information, states and parameters.

<h2 id="Open">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L40">Open</a>
    <a href="#Open">¶</a></h2>
<pre>func Open(path <a href="/builtin/#string">string</a>, mode <a href="/builtin/#int">int</a>, perm <a href="/builtin/#uint32">uint32</a>) (fd <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Openat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L46">Openat</a>
    <a href="#Openat">¶</a></h2>
<pre>func Openat(dirfd <a href="/builtin/#int">int</a>, path <a href="/builtin/#string">string</a>, flags <a href="/builtin/#int">int</a>, mode <a href="/builtin/#uint32">uint32</a>) (fd <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="ParseDirent">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/dirent.go#L54">ParseDirent</a>
    <a href="#ParseDirent">¶</a></h2>
<pre>func ParseDirent(buf []<a href="/builtin/#byte">byte</a>, max <a href="/builtin/#int">int</a>, names []<a href="/builtin/#string">string</a>) (consumed <a href="/builtin/#int">int</a>, count <a href="/builtin/#int">int</a>, newnames []<a href="/builtin/#string">string</a>)</pre>

ParseDirent parses up to max directory entries in buf, appending the names to
names. It returns the number of bytes consumed from buf, the number of entries
added to names, and the new names slice.

<h2 id="ParseNetlinkMessage">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/netlink_linux.go#L106">ParseNetlinkMessage</a>
    <a href="#ParseNetlinkMessage">¶</a></h2>
<pre>func ParseNetlinkMessage(b []<a href="/builtin/#byte">byte</a>) ([]<a href="#NetlinkMessage">NetlinkMessage</a>, <a href="/builtin/#error">error</a>)</pre>

ParseNetlinkMessage parses b as an array of netlink messages and returns the
slice containing the NetlinkMessage structures.

<h2 id="ParseNetlinkRouteAttr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/netlink_linux.go#L138">ParseNetlinkRouteAttr</a>
    <a href="#ParseNetlinkRouteAttr">¶</a></h2>
<pre>func ParseNetlinkRouteAttr(m *<a href="#NetlinkMessage">NetlinkMessage</a>) ([]<a href="#NetlinkRouteAttr">NetlinkRouteAttr</a>, <a href="/builtin/#error">error</a>)</pre>

ParseNetlinkRouteAttr parses m's payload as an array of netlink route attributes
and returns the slice containing the NetlinkRouteAttr structures.

<h2 id="ParseSocketControlMessage">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/sockcmsg_unix.go#L39">ParseSocketControlMessage</a>
    <a href="#ParseSocketControlMessage">¶</a></h2>
<pre>func ParseSocketControlMessage(b []<a href="/builtin/#byte">byte</a>) ([]<a href="#SocketControlMessage">SocketControlMessage</a>, <a href="/builtin/#error">error</a>)</pre>

ParseSocketControlMessage parses b as an array of socket control messages.

<h2 id="ParseUnixRights">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/sockcmsg_unix.go#L81">ParseUnixRights</a>
    <a href="#ParseUnixRights">¶</a></h2>
<pre>func ParseUnixRights(m *<a href="#SocketControlMessage">SocketControlMessage</a>) ([]<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

ParseUnixRights decodes a socket control message that contains an integer array
of open file descriptors from another process.

<h2 id="Pause">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L692">Pause</a>
    <a href="#Pause">¶</a></h2>
<pre>func Pause() (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Pipe">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux_amd64.go#L87">Pipe</a>
    <a href="#Pipe">¶</a></h2>
<pre>func Pipe(p []<a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Pipe2">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux_amd64.go#L100">Pipe2</a>
    <a href="#Pipe2">¶</a></h2>
<pre>func Pipe2(p []<a href="/builtin/#int">int</a>, flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PivotRoot">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L702">PivotRoot</a>
    <a href="#PivotRoot">¶</a></h2>
<pre>func PivotRoot(newroot <a href="/builtin/#string">string</a>, putold <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Pread">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1311">Pread</a>
    <a href="#Pread">¶</a></h2>
<pre>func Pread(fd <a href="/builtin/#int">int</a>, p []<a href="/builtin/#byte">byte</a>, offset <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtraceAttach">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L751">PtraceAttach</a>
    <a href="#PtraceAttach">¶</a></h2>
<pre>func PtraceAttach(pid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtraceCont">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L741">PtraceCont</a>
    <a href="#PtraceCont">¶</a></h2>
<pre>func PtraceCont(pid <a href="/builtin/#int">int</a>, signal <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtraceDetach">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L753">PtraceDetach</a>
    <a href="#PtraceDetach">¶</a></h2>
<pre>func PtraceDetach(pid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtraceGetEventMsg">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L734">PtraceGetEventMsg</a>
    <a href="#PtraceGetEventMsg">¶</a></h2>
<pre>func PtraceGetEventMsg(pid <a href="/builtin/#int">int</a>) (msg <a href="/builtin/#uint">uint</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtraceGetRegs">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L722">PtraceGetRegs</a>
    <a href="#PtraceGetRegs">¶</a></h2>
<pre>func PtraceGetRegs(pid <a href="/builtin/#int">int</a>, regsout *<a href="#PtraceRegs">PtraceRegs</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtracePeekData">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L659">PtracePeekData</a>
    <a href="#PtracePeekData">¶</a></h2>
<pre>func PtracePeekData(pid <a href="/builtin/#int">int</a>, addr <a href="/builtin/#uintptr">uintptr</a>, out []<a href="/builtin/#byte">byte</a>) (count <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtracePeekText">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L655">PtracePeekText</a>
    <a href="#PtracePeekText">¶</a></h2>
<pre>func PtracePeekText(pid <a href="/builtin/#int">int</a>, addr <a href="/builtin/#uintptr">uintptr</a>, out []<a href="/builtin/#byte">byte</a>) (count <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtracePokeData">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L718">PtracePokeData</a>
    <a href="#PtracePokeData">¶</a></h2>
<pre>func PtracePokeData(pid <a href="/builtin/#int">int</a>, addr <a href="/builtin/#uintptr">uintptr</a>, data []<a href="/builtin/#byte">byte</a>) (count <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtracePokeText">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L714">PtracePokeText</a>
    <a href="#PtracePokeText">¶</a></h2>
<pre>func PtracePokeText(pid <a href="/builtin/#int">int</a>, addr <a href="/builtin/#uintptr">uintptr</a>, data []<a href="/builtin/#byte">byte</a>) (count <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtraceSetOptions">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L730">PtraceSetOptions</a>
    <a href="#PtraceSetOptions">¶</a></h2>
<pre>func PtraceSetOptions(pid <a href="/builtin/#int">int</a>, options <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtraceSetRegs">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L726">PtraceSetRegs</a>
    <a href="#PtraceSetRegs">¶</a></h2>
<pre>func PtraceSetRegs(pid <a href="/builtin/#int">int</a>, regs *<a href="#PtraceRegs">PtraceRegs</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtraceSingleStep">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L749">PtraceSingleStep</a>
    <a href="#PtraceSingleStep">¶</a></h2>
<pre>func PtraceSingleStep(pid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="PtraceSyscall">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L745">PtraceSyscall</a>
    <a href="#PtraceSyscall">¶</a></h2>
<pre>func PtraceSyscall(pid <a href="/builtin/#int">int</a>, signal <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Pwrite">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1328">Pwrite</a>
    <a href="#Pwrite">¶</a></h2>
<pre>func Pwrite(fd <a href="/builtin/#int">int</a>, p []<a href="/builtin/#byte">byte</a>, offset <a href="/builtin/#int64">int64</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="RawSyscall">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L21">RawSyscall</a>
    <a href="#RawSyscall">¶</a></h2>
<pre>func RawSyscall(trap, a1, a2, a3 <a href="/builtin/#uintptr">uintptr</a>) (r1, r2 <a href="/builtin/#uintptr">uintptr</a>, err <a href="#Errno">Errno</a>)</pre>


<h2 id="RawSyscall6">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L22">RawSyscall6</a>
    <a href="#RawSyscall6">¶</a></h2>
<pre>func RawSyscall6(trap, a1, a2, a3, a4, a5, a6 <a href="/builtin/#uintptr">uintptr</a>) (r1, r2 <a href="/builtin/#uintptr">uintptr</a>, err <a href="#Errno">Errno</a>)</pre>


<h2 id="Read">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L151">Read</a>
    <a href="#Read">¶</a></h2>
<pre>func Read(fd <a href="/builtin/#int">int</a>, p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="ReadDirent">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L761">ReadDirent</a>
    <a href="#ReadDirent">¶</a></h2>
<pre>func ReadDirent(fd <a href="/builtin/#int">int</a>, buf []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Readlink">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L52">Readlink</a>
    <a href="#Readlink">¶</a></h2>
<pre>func Readlink(path <a href="/builtin/#string">string</a>, buf []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Reboot">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L757">Reboot</a>
    <a href="#Reboot">¶</a></h2>
<pre>func Reboot(cmd <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Recvfrom">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L239">Recvfrom</a>
    <a href="#Recvfrom">¶</a></h2>
<pre>func Recvfrom(fd <a href="/builtin/#int">int</a>, p []<a href="/builtin/#byte">byte</a>, flags <a href="/builtin/#int">int</a>) (n <a href="/builtin/#int">int</a>, from <a href="#Sockaddr">Sockaddr</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Recvmsg">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L520">Recvmsg</a>
    <a href="#Recvmsg">¶</a></h2>
<pre>func Recvmsg(fd <a href="/builtin/#int">int</a>, p, oob []<a href="/builtin/#byte">byte</a>, flags <a href="/builtin/#int">int</a>) (n, oobn <a href="/builtin/#int">int</a>, recvflags <a href="/builtin/#int">int</a>, from <a href="#Sockaddr">Sockaddr</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Removexattr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L749">Removexattr</a>
    <a href="#Removexattr">¶</a></h2>
<pre>func Removexattr(path <a href="/builtin/#string">string</a>, attr <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Rename">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L56">Rename</a>
    <a href="#Rename">¶</a></h2>
<pre>func Rename(oldpath <a href="/builtin/#string">string</a>, newpath <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Renameat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L769">Renameat</a>
    <a href="#Renameat">¶</a></h2>
<pre>func Renameat(olddirfd <a href="/builtin/#int">int</a>, oldpath <a href="/builtin/#string">string</a>, newdirfd <a href="/builtin/#int">int</a>, newpath <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Rmdir">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L60">Rmdir</a>
    <a href="#Rmdir">¶</a></h2>
<pre>func Rmdir(path <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>


<h2 id="Seek">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1345">Seek</a>
    <a href="#Seek">¶</a></h2>
<pre>func Seek(fd <a href="/builtin/#int">int</a>, offset <a href="/builtin/#int64">int64</a>, whence <a href="/builtin/#int">int</a>) (off <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Select">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1356">Select</a>
    <a href="#Select">¶</a></h2>
<pre>func Select(nfd <a href="/builtin/#int">int</a>, r *<a href="#FdSet">FdSet</a>, w *<a href="#FdSet">FdSet</a>, e *<a href="#FdSet">FdSet</a>, timeout *<a href="#Timeval">Timeval</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Sendfile">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L314">Sendfile</a>
    <a href="#Sendfile">¶</a></h2>
<pre>func Sendfile(outfd <a href="/builtin/#int">int</a>, infd <a href="/builtin/#int">int</a>, offset *<a href="/builtin/#int64">int64</a>, count <a href="/builtin/#int">int</a>) (written <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Sendmsg">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L559">Sendmsg</a>
    <a href="#Sendmsg">¶</a></h2>
<pre>func Sendmsg(fd <a href="/builtin/#int">int</a>, p, oob []<a href="/builtin/#byte">byte</a>, to <a href="#Sockaddr">Sockaddr</a>, flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SendmsgN">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L564">SendmsgN</a>
    <a href="#SendmsgN">¶</a></h2>
<pre>func SendmsgN(fd <a href="/builtin/#int">int</a>, p, oob []<a href="/builtin/#byte">byte</a>, to <a href="#Sockaddr">Sockaddr</a>, flags <a href="/builtin/#int">int</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Sendto">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L251">Sendto</a>
    <a href="#Sendto">¶</a></h2>
<pre>func Sendto(fd <a href="/builtin/#int">int</a>, p []<a href="/builtin/#byte">byte</a>, flags <a href="/builtin/#int">int</a>, to <a href="#Sockaddr">Sockaddr</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SetLsfPromisc">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/lsf_linux.go#L38">SetLsfPromisc</a>
    <a href="#SetLsfPromisc">¶</a></h2>
<pre>func SetLsfPromisc(name <a href="/builtin/#string">string</a>, m <a href="/builtin/#bool">bool</a>) <a href="/builtin/#error">error</a></pre>

Deprecated: Use golang.org/x/net/bpf instead.

<h2 id="SetNonblock">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_unix.go#L88">SetNonblock</a>
    <a href="#SetNonblock">¶</a></h2>
<pre>func SetNonblock(fd <a href="/builtin/#int">int</a>, nonblocking <a href="/builtin/#bool">bool</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setdomainname">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L789">Setdomainname</a>
    <a href="#Setdomainname">¶</a></h2>
<pre>func Setdomainname(p []<a href="/builtin/#byte">byte</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setenv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/env_unix.go#L83">Setenv</a>
    <a href="#Setenv">¶</a></h2>
<pre>func Setenv(key, value <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>


<h2 id="Setfsgid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1378">Setfsgid</a>
    <a href="#Setfsgid">¶</a></h2>
<pre>func Setfsgid(gid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setfsuid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1388">Setfsuid</a>
    <a href="#Setfsuid">¶</a></h2>
<pre>func Setfsuid(uid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setgid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L868">Setgid</a>
    <a href="#Setgid">¶</a></h2>
<pre>func Setgid(gid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setgroups">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L171">Setgroups</a>
    <a href="#Setgroups">¶</a></h2>
<pre>func Setgroups(gids []<a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Sethostname">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L805">Sethostname</a>
    <a href="#Sethostname">¶</a></h2>
<pre>func Sethostname(p []<a href="/builtin/#byte">byte</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setpgid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L821">Setpgid</a>
    <a href="#Setpgid">¶</a></h2>
<pre>func Setpgid(pid <a href="/builtin/#int">int</a>, pgid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setpriority">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L852">Setpriority</a>
    <a href="#Setpriority">¶</a></h2>
<pre>func Setpriority(which <a href="/builtin/#int">int</a>, who <a href="/builtin/#int">int</a>, prio <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setregid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1398">Setregid</a>
    <a href="#Setregid">¶</a></h2>
<pre>func Setregid(rgid <a href="/builtin/#int">int</a>, egid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setresgid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1408">Setresgid</a>
    <a href="#Setresgid">¶</a></h2>
<pre>func Setresgid(rgid <a href="/builtin/#int">int</a>, egid <a href="/builtin/#int">int</a>, sgid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setresuid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1418">Setresuid</a>
    <a href="#Setresuid">¶</a></h2>
<pre>func Setresuid(ruid <a href="/builtin/#int">int</a>, euid <a href="/builtin/#int">int</a>, suid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setreuid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1438">Setreuid</a>
    <a href="#Setreuid">¶</a></h2>
<pre>func Setreuid(ruid <a href="/builtin/#int">int</a>, euid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setrlimit">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1428">Setrlimit</a>
    <a href="#Setrlimit">¶</a></h2>
<pre>func Setrlimit(resource <a href="/builtin/#int">int</a>, rlim *<a href="#Rlimit">Rlimit</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setsid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L831">Setsid</a>
    <a href="#Setsid">¶</a></h2>
<pre>func Setsid() (pid <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SetsockoptByte">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L259">SetsockoptByte</a>
    <a href="#SetsockoptByte">¶</a></h2>
<pre>func SetsockoptByte(fd, level, opt <a href="/builtin/#int">int</a>, value <a href="/builtin/#byte">byte</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SetsockoptICMPv6Filter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L280">SetsockoptICMPv6Filter</a>
    <a href="#SetsockoptICMPv6Filter">¶</a></h2>
<pre>func SetsockoptICMPv6Filter(fd, level, opt <a href="/builtin/#int">int</a>, filter *<a href="#ICMPv6Filter">ICMPv6Filter</a>) <a href="/builtin/#error">error</a></pre>


<h2 id="SetsockoptIPMreq">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L272">SetsockoptIPMreq</a>
    <a href="#SetsockoptIPMreq">¶</a></h2>
<pre>func SetsockoptIPMreq(fd, level, opt <a href="/builtin/#int">int</a>, mreq *<a href="#IPMreq">IPMreq</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SetsockoptIPMreqn">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L516">SetsockoptIPMreqn</a>
    <a href="#SetsockoptIPMreqn">¶</a></h2>
<pre>func SetsockoptIPMreqn(fd, level, opt <a href="/builtin/#int">int</a>, mreq *<a href="#IPMreqn">IPMreqn</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SetsockoptIPv6Mreq">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L276">SetsockoptIPv6Mreq</a>
    <a href="#SetsockoptIPv6Mreq">¶</a></h2>
<pre>func SetsockoptIPv6Mreq(fd, level, opt <a href="/builtin/#int">int</a>, mreq *<a href="#IPv6Mreq">IPv6Mreq</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SetsockoptInet4Addr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L268">SetsockoptInet4Addr</a>
    <a href="#SetsockoptInet4Addr">¶</a></h2>
<pre>func SetsockoptInet4Addr(fd, level, opt <a href="/builtin/#int">int</a>, value [4]<a href="/builtin/#byte">byte</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SetsockoptInt">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L263">SetsockoptInt</a>
    <a href="#SetsockoptInt">¶</a></h2>
<pre>func SetsockoptInt(fd, level, opt <a href="/builtin/#int">int</a>, value <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SetsockoptLinger">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L284">SetsockoptLinger</a>
    <a href="#SetsockoptLinger">¶</a></h2>
<pre>func SetsockoptLinger(fd, level, opt <a href="/builtin/#int">int</a>, l *<a href="#Linger">Linger</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SetsockoptString">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L288">SetsockoptString</a>
    <a href="#SetsockoptString">¶</a></h2>
<pre>func SetsockoptString(fd, level, opt <a href="/builtin/#int">int</a>, s <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SetsockoptTimeval">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L292">SetsockoptTimeval</a>
    <a href="#SetsockoptTimeval">¶</a></h2>
<pre>func SetsockoptTimeval(fd, level, opt <a href="/builtin/#int">int</a>, tv *<a href="#Timeval">Timeval</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Settimeofday">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L842">Settimeofday</a>
    <a href="#Settimeofday">¶</a></h2>
<pre>func Settimeofday(tv *<a href="#Timeval">Timeval</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setuid">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L864">Setuid</a>
    <a href="#Setuid">¶</a></h2>
<pre>func Setuid(uid <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Setxattr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L862">Setxattr</a>
    <a href="#Setxattr">¶</a></h2>
<pre>func Setxattr(path <a href="/builtin/#string">string</a>, attr <a href="/builtin/#string">string</a>, data []<a href="/builtin/#byte">byte</a>, flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Shutdown">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1448">Shutdown</a>
    <a href="#Shutdown">¶</a></h2>
<pre>func Shutdown(fd <a href="/builtin/#int">int</a>, how <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SlicePtrFromStrings">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_unix.go#L73">SlicePtrFromStrings</a>
    <a href="#SlicePtrFromStrings">¶</a></h2>
<pre>func SlicePtrFromStrings(ss []<a href="/builtin/#string">string</a>) ([]*<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

SlicePtrFromStrings converts a slice of strings to a slice of pointers to
NUL-terminated byte arrays. If any string contains a NUL byte, it returns (nil,
EINVAL).

<h2 id="Socket">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L296">Socket</a>
    <a href="#Socket">¶</a></h2>
<pre>func Socket(domain, typ, proto <a href="/builtin/#int">int</a>) (fd <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Socketpair">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L304">Socketpair</a>
    <a href="#Socketpair">¶</a></h2>
<pre>func Socketpair(domain, typ, proto <a href="/builtin/#int">int</a>) (fd [2]<a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Splice">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1458">Splice</a>
    <a href="#Splice">¶</a></h2>
<pre>func Splice(rfd <a href="/builtin/#int">int</a>, roff *<a href="/builtin/#int64">int64</a>, wfd <a href="/builtin/#int">int</a>, woff *<a href="/builtin/#int64">int64</a>, len <a href="/builtin/#int">int</a>, flags <a href="/builtin/#int">int</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="StartProcess">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_unix.go#L230">StartProcess</a>
    <a href="#StartProcess">¶</a></h2>
<pre>func StartProcess(argv0 <a href="/builtin/#string">string</a>, argv []<a href="/builtin/#string">string</a>, attr *<a href="#ProcAttr">ProcAttr</a>) (pid <a href="/builtin/#int">int</a>, handle <a href="/builtin/#uintptr">uintptr</a>, err <a href="/builtin/#error">error</a>)</pre>

StartProcess wraps ForkExec for package os.

<h2 id="Stat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1469">Stat</a>
    <a href="#Stat">¶</a></h2>
<pre>func Stat(path <a href="/builtin/#string">string</a>, stat *<a href="#Stat_t">Stat_t</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Statfs">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1484">Statfs</a>
    <a href="#Statfs">¶</a></h2>
<pre>func Statfs(path <a href="/builtin/#string">string</a>, buf *<a href="#Statfs_t">Statfs_t</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="StringBytePtr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go#L56">StringBytePtr</a>
    <a href="#StringBytePtr">¶</a></h2>
<pre>func StringBytePtr(s <a href="/builtin/#string">string</a>) *<a href="/builtin/#byte">byte</a></pre>

StringBytePtr returns a pointer to a NUL-terminated array of bytes. If s
contains a NUL byte this function panics instead of returning an error.

Deprecated: Use BytePtrFromString instead.

<h2 id="StringByteSlice">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go#L29">StringByteSlice</a>
    <a href="#StringByteSlice">¶</a></h2>
<pre>func StringByteSlice(s <a href="/builtin/#string">string</a>) []<a href="/builtin/#byte">byte</a></pre>

StringByteSlice converts a string to a NUL-terminated []byte, If s contains a
NUL byte this function panics instead of returning an error.

Deprecated: Use ByteSliceFromString instead.

<h2 id="StringSlicePtr">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_unix.go#L61">StringSlicePtr</a>
    <a href="#StringSlicePtr">¶</a></h2>
<pre>func StringSlicePtr(ss []<a href="/builtin/#string">string</a>) []*<a href="/builtin/#byte">byte</a></pre>

StringSlicePtr converts a slice of strings to a slice of pointers to
NUL-terminated byte arrays. If any string contains a NUL byte this function
panics instead of returning an error.

Deprecated: Use SlicePtrFromStrings instead.

<h2 id="Symlink">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L66">Symlink</a>
    <a href="#Symlink">¶</a></h2>
<pre>func Symlink(oldpath <a href="/builtin/#string">string</a>, newpath <a href="/builtin/#string">string</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Sync">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L888">Sync</a>
    <a href="#Sync">¶</a></h2>
<pre>func Sync()</pre>


<h2 id="SyncFileRange">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1499">SyncFileRange</a>
    <a href="#SyncFileRange">¶</a></h2>
<pre>func SyncFileRange(fd <a href="/builtin/#int">int</a>, off <a href="/builtin/#int64">int64</a>, n <a href="/builtin/#int64">int64</a>, flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Syscall">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L19">Syscall</a>
    <a href="#Syscall">¶</a></h2>
<pre>func Syscall(trap, a1, a2, a3 <a href="/builtin/#uintptr">uintptr</a>) (r1, r2 <a href="/builtin/#uintptr">uintptr</a>, err <a href="#Errno">Errno</a>)</pre>


<h2 id="Syscall6">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L20">Syscall6</a>
    <a href="#Syscall6">¶</a></h2>
<pre>func Syscall6(trap, a1, a2, a3, a4, a5, a6 <a href="/builtin/#uintptr">uintptr</a>) (r1, r2 <a href="/builtin/#uintptr">uintptr</a>, err <a href="#Errno">Errno</a>)</pre>


<h2 id="Sysinfo">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L895">Sysinfo</a>
    <a href="#Sysinfo">¶</a></h2>
<pre>func Sysinfo(info *<a href="#Sysinfo_t">Sysinfo_t</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Tee">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L905">Tee</a>
    <a href="#Tee">¶</a></h2>
<pre>func Tee(rfd <a href="/builtin/#int">int</a>, wfd <a href="/builtin/#int">int</a>, len <a href="/builtin/#int">int</a>, flags <a href="/builtin/#int">int</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Tgkill">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L916">Tgkill</a>
    <a href="#Tgkill">¶</a></h2>
<pre>func Tgkill(tgid <a href="/builtin/#int">int</a>, tid <a href="/builtin/#int">int</a>, sig <a href="#Signal">Signal</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Times">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L926">Times</a>
    <a href="#Times">¶</a></h2>
<pre>func Times(tms *<a href="#Tms">Tms</a>) (ticks <a href="/builtin/#uintptr">uintptr</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="TimespecToNsec">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/timestruct.go#L1">TimespecToNsec</a>
    <a href="#TimespecToNsec">¶</a></h2>
<pre>func TimespecToNsec(ts <a href="#Timespec">Timespec</a>) <a href="/builtin/#int64">int64</a></pre>

TimespecToNsec converts a Timespec value into a number of nanoseconds since the
Unix epoch.

<h2 id="TimevalToNsec">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/timestruct.go#L17">TimevalToNsec</a>
    <a href="#TimevalToNsec">¶</a></h2>
<pre>func TimevalToNsec(tv <a href="#Timeval">Timeval</a>) <a href="/builtin/#int64">int64</a></pre>

TimevalToNsec converts a Timeval value into a number of nanoseconds since the
Unix epoch.

<h2 id="Truncate">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L1509">Truncate</a>
    <a href="#Truncate">¶</a></h2>
<pre>func Truncate(path <a href="/builtin/#string">string</a>, length <a href="/builtin/#int64">int64</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Umask">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L937">Umask</a>
    <a href="#Umask">¶</a></h2>
<pre>func Umask(mask <a href="/builtin/#int">int</a>) (oldmask <a href="/builtin/#int">int</a>)</pre>


<h2 id="Uname">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L945">Uname</a>
    <a href="#Uname">¶</a></h2>
<pre>func Uname(buf *<a href="#Utsname">Utsname</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="UnixCredentials">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/sockcmsg_linux.go#L4">UnixCredentials</a>
    <a href="#UnixCredentials">¶</a></h2>
<pre>func UnixCredentials(ucred *<a href="#Ucred">Ucred</a>) []<a href="/builtin/#byte">byte</a></pre>

UnixCredentials encodes credentials into a socket control message for sending to
another process. This can be used for authentication.

<h2 id="UnixRights">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/sockcmsg_unix.go#L64">UnixRights</a>
    <a href="#UnixRights">¶</a></h2>
<pre>func UnixRights(fds ...<a href="/builtin/#int">int</a>) []<a href="/builtin/#byte">byte</a></pre>

UnixRights encodes a set of open file descriptors into a socket control message
for sending to another process.

<h2 id="Unlink">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L70">Unlink</a>
    <a href="#Unlink">¶</a></h2>
<pre>func Unlink(path <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>


<h2 id="Unlinkat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L76">Unlinkat</a>
    <a href="#Unlinkat">¶</a></h2>
<pre>func Unlinkat(dirfd <a href="/builtin/#int">int</a>, path <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>


<h2 id="Unmount">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L955">Unmount</a>
    <a href="#Unmount">¶</a></h2>
<pre>func Unmount(target <a href="/builtin/#string">string</a>, flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Unsetenv">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/env_unix.go#L47">Unsetenv</a>
    <a href="#Unsetenv">¶</a></h2>
<pre>func Unsetenv(key <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>


<h2 id="Unshare">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L970">Unshare</a>
    <a href="#Unshare">¶</a></h2>
<pre>func Unshare(flags <a href="/builtin/#int">int</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Ustat">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L980">Ustat</a>
    <a href="#Ustat">¶</a></h2>
<pre>func Ustat(dev <a href="/builtin/#int">int</a>, ubuf *<a href="#Ustat_t">Ustat_t</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Utime">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/zsyscall_linux_amd64.go#L990">Utime</a>
    <a href="#Utime">¶</a></h2>
<pre>func Utime(path <a href="/builtin/#string">string</a>, buf *<a href="#Utimbuf">Utimbuf</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Utimes">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L82">Utimes</a>
    <a href="#Utimes">¶</a></h2>
<pre>func Utimes(path <a href="/builtin/#string">string</a>, tv []<a href="#Timeval">Timeval</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="UtimesNano">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L91">UtimesNano</a>
    <a href="#UtimesNano">¶</a></h2>
<pre>func UtimesNano(path <a href="/builtin/#string">string</a>, ts []<a href="#Timespec">Timespec</a>) (err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Wait4">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L242">Wait4</a>
    <a href="#Wait4">¶</a></h2>
<pre>func Wait4(pid <a href="/builtin/#int">int</a>, wstatus *<a href="#WaitStatus">WaitStatus</a>, options <a href="/builtin/#int">int</a>, rusage *<a href="#Rusage">Rusage</a>) (wpid <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Write">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L167">Write</a>
    <a href="#Write">¶</a></h2>
<pre>func Write(fd <a href="/builtin/#int">int</a>, p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Cmsghdr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L234">Cmsghdr</a>
    <a href="#Cmsghdr">¶</a></h2>
<pre>type Cmsghdr struct {
<span id="Cmsghdr.Len"></span>    Len   <a href="/builtin/#uint64">uint64</a>
<span id="Cmsghdr.Level"></span>    Level <a href="/builtin/#int32">int32</a>
<span id="Cmsghdr.Type"></span>    Type  <a href="/builtin/#int32">int32</a>
}</pre>


<h3 id="Cmsghdr.SetLen">func (*Cmsghdr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux_amd64.go#L123">SetLen</a>
    <a href="#Cmsghdr.SetLen">¶</a></h3>
<pre>func (cmsg *<a href="#Cmsghdr">Cmsghdr</a>) SetLen(length <a href="/builtin/#int">int</a>)</pre>


<h2 id="Conn">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/net.go#L21">Conn</a>
    <a href="#Conn">¶</a></h2>
<pre>type Conn interface {
    <span class="comment">// SyscallConn returns a raw network connection.</span>
    SyscallConn() (<a href="#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)
}</pre>

Conn is implemented by some types in the net package to provide access to the
underlying file descriptor or handle.

<h2 id="Credential">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_unix.go#L104">Credential</a>
    <a href="#Credential">¶</a></h2>
<pre>type Credential struct {
<span id="Credential.Uid"></span>    Uid         <a href="/builtin/#uint32">uint32</a>   <span class="comment">// User ID.</span>
<span id="Credential.Gid"></span>    Gid         <a href="/builtin/#uint32">uint32</a>   <span class="comment">// Group ID.</span>
<span id="Credential.Groups"></span>    Groups      []<a href="/builtin/#uint32">uint32</a> <span class="comment">// Supplementary group IDs.</span>
<span id="Credential.NoSetGroups"></span>    NoSetGroups <a href="/builtin/#bool">bool</a>     <span class="comment">// If true, don&#39;t set supplementary groups</span>
}</pre>

Credential holds user and group identities to be assumed by a child process
started by StartProcess.

<h2 id="Dirent">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L124">Dirent</a>
    <a href="#Dirent">¶</a></h2>
<pre>type Dirent struct {
<span id="Dirent.Ino"></span>    Ino       <a href="/builtin/#uint64">uint64</a>
<span id="Dirent.Off"></span>    Off       <a href="/builtin/#int64">int64</a>
<span id="Dirent.Reclen"></span>    Reclen    <a href="/builtin/#uint16">uint16</a>
<span id="Dirent.Type"></span>    Type      <a href="/builtin/#uint8">uint8</a>
<span id="Dirent.Name"></span>    Name      [256]<a href="/builtin/#int8">int8</a>
<span id="Dirent.Pad_cgo_0"></span>    Pad_cgo_0 [5]<a href="/builtin/#byte">byte</a>
}</pre>


<h2 id="EpollEvent">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L577">EpollEvent</a>
    <a href="#EpollEvent">¶</a></h2>
<pre>type EpollEvent struct {
<span id="EpollEvent.Events"></span>    Events <a href="/builtin/#uint32">uint32</a>
<span id="EpollEvent.Fd"></span>    Fd     <a href="/builtin/#int32">int32</a>
<span id="EpollEvent.Pad"></span>    Pad    <a href="/builtin/#int32">int32</a>
}</pre>


<h2 id="Errno">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L91">Errno</a>
    <a href="#Errno">¶</a></h2>
<pre>type Errno <a href="/builtin/#uintptr">uintptr</a></pre>

An Errno is an unsigned number describing an error condition. It implements the
error interface. The zero Errno is by convention a non-error, so code to convert
from Errno to error should use:

    err = nil
    if errno != 0 {
    	err = errno
    }

<h3 id="Errno.Error">func (Errno) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L93">Error</a>
    <a href="#Errno.Error">¶</a></h3>
<pre>func (e <a href="#Errno">Errno</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="Errno.Temporary">func (Errno) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L103">Temporary</a>
    <a href="#Errno.Temporary">¶</a></h3>
<pre>func (e <a href="#Errno">Errno</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="Errno.Timeout">func (Errno) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L107">Timeout</a>
    <a href="#Errno.Timeout">¶</a></h3>
<pre>func (e <a href="#Errno">Errno</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="FdSet">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L536">FdSet</a>
    <a href="#FdSet">¶</a></h2>
<pre>type FdSet struct {
<span id="FdSet.Bits"></span>    Bits [16]<a href="/builtin/#int64">int64</a>
}</pre>


<h2 id="Flock_t">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L137">Flock_t</a>
    <a href="#Flock_t">¶</a></h2>
<pre>type Flock_t struct {
<span id="Flock_t.Type"></span>    Type      <a href="/builtin/#int16">int16</a>
<span id="Flock_t.Whence"></span>    Whence    <a href="/builtin/#int16">int16</a>
<span id="Flock_t.Pad_cgo_0"></span>    Pad_cgo_0 [4]<a href="/builtin/#byte">byte</a>
<span id="Flock_t.Start"></span>    Start     <a href="/builtin/#int64">int64</a>
<span id="Flock_t.Len"></span>    Len       <a href="/builtin/#int64">int64</a>
<span id="Flock_t.Pid"></span>    Pid       <a href="/builtin/#int32">int32</a>
<span id="Flock_t.Pad_cgo_1"></span>    Pad_cgo_1 [4]<a href="/builtin/#byte">byte</a>
}</pre>


<h2 id="Fsid">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L133">Fsid</a>
    <a href="#Fsid">¶</a></h2>
<pre>type Fsid struct {
<span id="Fsid.X__val"></span>    X__val [2]<a href="/builtin/#int32">int32</a>
}</pre>


<h2 id="ICMPv6Filter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L256">ICMPv6Filter</a>
    <a href="#ICMPv6Filter">¶</a></h2>
<pre>type ICMPv6Filter struct {
<span id="ICMPv6Filter.Data"></span>    Data [8]<a href="/builtin/#uint32">uint32</a>
}</pre>


<h3 id="GetsockoptICMPv6Filter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L502">GetsockoptICMPv6Filter</a>
    <a href="#GetsockoptICMPv6Filter">¶</a></h3>
<pre>func GetsockoptICMPv6Filter(fd, level, opt <a href="/builtin/#int">int</a>) (*<a href="#ICMPv6Filter">ICMPv6Filter</a>, <a href="/builtin/#error">error</a>)</pre>


<h2 id="IPMreq">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L206">IPMreq</a>
    <a href="#IPMreq">¶</a></h2>
<pre>type IPMreq struct {
<span id="IPMreq.Multiaddr"></span>    Multiaddr [4]<a href="/builtin/#byte">byte</a> <span class="comment">/* in_addr */</span>
<span id="IPMreq.Interface"></span>    Interface [4]<a href="/builtin/#byte">byte</a> <span class="comment">/* in_addr */</span>
}</pre>


<h3 id="GetsockoptIPMreq">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L474">GetsockoptIPMreq</a>
    <a href="#GetsockoptIPMreq">¶</a></h3>
<pre>func GetsockoptIPMreq(fd, level, opt <a href="/builtin/#int">int</a>) (*<a href="#IPMreq">IPMreq</a>, <a href="/builtin/#error">error</a>)</pre>


<h2 id="IPMreqn">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L211">IPMreqn</a>
    <a href="#IPMreqn">¶</a></h2>
<pre>type IPMreqn struct {
<span id="IPMreqn.Multiaddr"></span>    Multiaddr [4]<a href="/builtin/#byte">byte</a> <span class="comment">/* in_addr */</span>
<span id="IPMreqn.Address"></span>    Address   [4]<a href="/builtin/#byte">byte</a> <span class="comment">/* in_addr */</span>
<span id="IPMreqn.Ifindex"></span>    Ifindex   <a href="/builtin/#int32">int32</a>
}</pre>


<h3 id="GetsockoptIPMreqn">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L481">GetsockoptIPMreqn</a>
    <a href="#GetsockoptIPMreqn">¶</a></h3>
<pre>func GetsockoptIPMreqn(fd, level, opt <a href="/builtin/#int">int</a>) (*<a href="#IPMreqn">IPMreqn</a>, <a href="/builtin/#error">error</a>)</pre>


<h2 id="IPv6MTUInfo">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L251">IPv6MTUInfo</a>
    <a href="#IPv6MTUInfo">¶</a></h2>
<pre>type IPv6MTUInfo struct {
<span id="IPv6MTUInfo.Addr"></span>    Addr <a href="#RawSockaddrInet6">RawSockaddrInet6</a>
<span id="IPv6MTUInfo.Mtu"></span>    Mtu  <a href="/builtin/#uint32">uint32</a>
}</pre>


<h3 id="GetsockoptIPv6MTUInfo">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L495">GetsockoptIPv6MTUInfo</a>
    <a href="#GetsockoptIPv6MTUInfo">¶</a></h3>
<pre>func GetsockoptIPv6MTUInfo(fd, level, opt <a href="/builtin/#int">int</a>) (*<a href="#IPv6MTUInfo">IPv6MTUInfo</a>, <a href="/builtin/#error">error</a>)</pre>


<h2 id="IPv6Mreq">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L217">IPv6Mreq</a>
    <a href="#IPv6Mreq">¶</a></h2>
<pre>type IPv6Mreq struct {
<span id="IPv6Mreq.Multiaddr"></span>    Multiaddr [16]<a href="/builtin/#byte">byte</a> <span class="comment">/* in6_addr */</span>
<span id="IPv6Mreq.Interface"></span>    Interface <a href="/builtin/#uint32">uint32</a>
}</pre>


<h3 id="GetsockoptIPv6Mreq">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L488">GetsockoptIPv6Mreq</a>
    <a href="#GetsockoptIPv6Mreq">¶</a></h3>
<pre>func GetsockoptIPv6Mreq(fd, level, opt <a href="/builtin/#int">int</a>) (*<a href="#IPv6Mreq">IPv6Mreq</a>, <a href="/builtin/#error">error</a>)</pre>


<h2 id="IfAddrmsg">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L451">IfAddrmsg</a>
    <a href="#IfAddrmsg">¶</a></h2>
<pre>type IfAddrmsg struct {
<span id="IfAddrmsg.Family"></span>    Family    <a href="/builtin/#uint8">uint8</a>
<span id="IfAddrmsg.Prefixlen"></span>    Prefixlen <a href="/builtin/#uint8">uint8</a>
<span id="IfAddrmsg.Flags"></span>    Flags     <a href="/builtin/#uint8">uint8</a>
<span id="IfAddrmsg.Scope"></span>    Scope     <a href="/builtin/#uint8">uint8</a>
<span id="IfAddrmsg.Index"></span>    Index     <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="IfInfomsg">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L442">IfInfomsg</a>
    <a href="#IfInfomsg">¶</a></h2>
<pre>type IfInfomsg struct {
<span id="IfInfomsg.Family"></span>    Family     <a href="/builtin/#uint8">uint8</a>
<span id="IfInfomsg.X__ifi_pad"></span>    X__ifi_pad <a href="/builtin/#uint8">uint8</a>
<span id="IfInfomsg.Type"></span>    Type       <a href="/builtin/#uint16">uint16</a>
<span id="IfInfomsg.Index"></span>    Index      <a href="/builtin/#int32">int32</a>
<span id="IfInfomsg.Flags"></span>    Flags      <a href="/builtin/#uint32">uint32</a>
<span id="IfInfomsg.Change"></span>    Change     <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="Inet4Pktinfo">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L240">Inet4Pktinfo</a>
    <a href="#Inet4Pktinfo">¶</a></h2>
<pre>type Inet4Pktinfo struct {
<span id="Inet4Pktinfo.Ifindex"></span>    Ifindex  <a href="/builtin/#int32">int32</a>
<span id="Inet4Pktinfo.Spec_dst"></span>    Spec_dst [4]<a href="/builtin/#byte">byte</a> <span class="comment">/* in_addr */</span>
<span id="Inet4Pktinfo.Addr"></span>    Addr     [4]<a href="/builtin/#byte">byte</a> <span class="comment">/* in_addr */</span>
}</pre>


<h2 id="Inet6Pktinfo">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L246">Inet6Pktinfo</a>
    <a href="#Inet6Pktinfo">¶</a></h2>
<pre>type Inet6Pktinfo struct {
<span id="Inet6Pktinfo.Addr"></span>    Addr    [16]<a href="/builtin/#byte">byte</a> <span class="comment">/* in6_addr */</span>
<span id="Inet6Pktinfo.Ifindex"></span>    Ifindex <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="InotifyEvent">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L496">InotifyEvent</a>
    <a href="#InotifyEvent">¶</a></h2>
<pre>type InotifyEvent struct {
<span id="InotifyEvent.Wd"></span>    Wd     <a href="/builtin/#int32">int32</a>
<span id="InotifyEvent.Mask"></span>    Mask   <a href="/builtin/#uint32">uint32</a>
<span id="InotifyEvent.Cookie"></span>    Cookie <a href="/builtin/#uint32">uint32</a>
<span id="InotifyEvent.Len"></span>    Len    <a href="/builtin/#uint32">uint32</a>
<span id="InotifyEvent.Name"></span>    Name   [0]<a href="/builtin/#uint8">uint8</a>
}</pre>


<h2 id="Iovec">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L201">Iovec</a>
    <a href="#Iovec">¶</a></h2>
<pre>type Iovec struct {
<span id="Iovec.Base"></span>    Base *<a href="/builtin/#byte">byte</a>
<span id="Iovec.Len"></span>    Len  <a href="/builtin/#uint64">uint64</a>
}</pre>


<h3 id="Iovec.SetLen">func (*Iovec) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux_amd64.go#L115">SetLen</a>
    <a href="#Iovec.SetLen">¶</a></h3>
<pre>func (iov *<a href="#Iovec">Iovec</a>) SetLen(length <a href="/builtin/#int">int</a>)</pre>


<h2 id="Linger">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L196">Linger</a>
    <a href="#Linger">¶</a></h2>
<pre>type Linger struct {
<span id="Linger.Onoff"></span>    Onoff  <a href="/builtin/#int32">int32</a>
<span id="Linger.Linger"></span>    Linger <a href="/builtin/#int32">int32</a>
}</pre>


<h2 id="Msghdr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L222">Msghdr</a>
    <a href="#Msghdr">¶</a></h2>
<pre>type Msghdr struct {
<span id="Msghdr.Name"></span>    Name       *<a href="/builtin/#byte">byte</a>
<span id="Msghdr.Namelen"></span>    Namelen    <a href="/builtin/#uint32">uint32</a>
<span id="Msghdr.Pad_cgo_0"></span>    Pad_cgo_0  [4]<a href="/builtin/#byte">byte</a>
<span id="Msghdr.Iov"></span>    Iov        *<a href="#Iovec">Iovec</a>
<span id="Msghdr.Iovlen"></span>    Iovlen     <a href="/builtin/#uint64">uint64</a>
<span id="Msghdr.Control"></span>    Control    *<a href="/builtin/#byte">byte</a>
<span id="Msghdr.Controllen"></span>    Controllen <a href="/builtin/#uint64">uint64</a>
<span id="Msghdr.Flags"></span>    Flags      <a href="/builtin/#int32">int32</a>
<span id="Msghdr.Pad_cgo_1"></span>    Pad_cgo_1  [4]<a href="/builtin/#byte">byte</a>
}</pre>


<h3 id="Msghdr.SetControllen">func (*Msghdr) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux_amd64.go#L119">SetControllen</a>
    <a href="#Msghdr.SetControllen">¶</a></h3>
<pre>func (msghdr *<a href="#Msghdr">Msghdr</a>) SetControllen(length <a href="/builtin/#int">int</a>)</pre>


<h2 id="NetlinkMessage">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/netlink_linux.go#L99">NetlinkMessage</a>
    <a href="#NetlinkMessage">¶</a></h2>
<pre>type NetlinkMessage struct {
<span id="NetlinkMessage.Header"></span>    Header <a href="#NlMsghdr">NlMsghdr</a>
<span id="NetlinkMessage.Data"></span>    Data   []<a href="/builtin/#byte">byte</a>
}</pre>

NetlinkMessage represents a netlink message.

<h2 id="NetlinkRouteAttr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/netlink_linux.go#L130">NetlinkRouteAttr</a>
    <a href="#NetlinkRouteAttr">¶</a></h2>
<pre>type NetlinkRouteAttr struct {
<span id="NetlinkRouteAttr.Attr"></span>    Attr  <a href="#RtAttr">RtAttr</a>
<span id="NetlinkRouteAttr.Value"></span>    Value []<a href="/builtin/#byte">byte</a>
}</pre>

NetlinkRouteAttr represents a netlink route attribute.

<h2 id="NetlinkRouteRequest">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/netlink_linux.go#L14">NetlinkRouteRequest</a>
    <a href="#NetlinkRouteRequest">¶</a></h2>
<pre>type NetlinkRouteRequest struct {
<span id="NetlinkRouteRequest.Header"></span>    Header <a href="#NlMsghdr">NlMsghdr</a>
<span id="NetlinkRouteRequest.Data"></span>    Data   <a href="#RtGenmsg">RtGenmsg</a>
}</pre>

NetlinkRouteRequest represents a request message to receive routing and link
states from the kernel.

<h2 id="NlAttr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L432">NlAttr</a>
    <a href="#NlAttr">¶</a></h2>
<pre>type NlAttr struct {
<span id="NlAttr.Len"></span>    Len  <a href="/builtin/#uint16">uint16</a>
<span id="NlAttr.Type"></span>    Type <a href="/builtin/#uint16">uint16</a>
}</pre>


<h2 id="NlMsgerr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L423">NlMsgerr</a>
    <a href="#NlMsgerr">¶</a></h2>
<pre>type NlMsgerr struct {
<span id="NlMsgerr.Error"></span>    Error <a href="/builtin/#int32">int32</a>
<span id="NlMsgerr.Msg"></span>    Msg   <a href="#NlMsghdr">NlMsghdr</a>
}</pre>


<h2 id="NlMsghdr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L415">NlMsghdr</a>
    <a href="#NlMsghdr">¶</a></h2>
<pre>type NlMsghdr struct {
<span id="NlMsghdr.Len"></span>    Len   <a href="/builtin/#uint32">uint32</a>
<span id="NlMsghdr.Type"></span>    Type  <a href="/builtin/#uint16">uint16</a>
<span id="NlMsghdr.Flags"></span>    Flags <a href="/builtin/#uint16">uint16</a>
<span id="NlMsghdr.Seq"></span>    Seq   <a href="/builtin/#uint32">uint32</a>
<span id="NlMsghdr.Pid"></span>    Pid   <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="ProcAttr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_unix.go#L113">ProcAttr</a>
    <a href="#ProcAttr">¶</a></h2>
<pre>type ProcAttr struct {
<span id="ProcAttr.Dir"></span>    Dir   <a href="/builtin/#string">string</a>    <span class="comment">// Current working directory.</span>
<span id="ProcAttr.Env"></span>    Env   []<a href="/builtin/#string">string</a>  <span class="comment">// Environment.</span>
<span id="ProcAttr.Files"></span>    Files []<a href="/builtin/#uintptr">uintptr</a> <span class="comment">// File descriptors.</span>
<span id="ProcAttr.Sys"></span>    Sys   *<a href="#SysProcAttr">SysProcAttr</a>
}</pre>

ProcAttr holds attributes that will be applied to a new process started by
StartProcess.

<h2 id="PtraceRegs">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L506">PtraceRegs</a>
    <a href="#PtraceRegs">¶</a></h2>
<pre>type PtraceRegs struct {
<span id="PtraceRegs.R15"></span>    R15      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.R14"></span>    R14      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.R13"></span>    R13      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.R12"></span>    R12      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Rbp"></span>    Rbp      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Rbx"></span>    Rbx      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.R11"></span>    R11      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.R10"></span>    R10      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.R9"></span>    R9       <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.R8"></span>    R8       <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Rax"></span>    Rax      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Rcx"></span>    Rcx      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Rdx"></span>    Rdx      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Rsi"></span>    Rsi      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Rdi"></span>    Rdi      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Orig_rax"></span>    Orig_rax <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Rip"></span>    Rip      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Cs"></span>    Cs       <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Eflags"></span>    Eflags   <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Rsp"></span>    Rsp      <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Ss"></span>    Ss       <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Fs_base"></span>    Fs_base  <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Gs_base"></span>    Gs_base  <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Ds"></span>    Ds       <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Es"></span>    Es       <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Fs"></span>    Fs       <a href="/builtin/#uint64">uint64</a>
<span id="PtraceRegs.Gs"></span>    Gs       <a href="/builtin/#uint64">uint64</a>
}</pre>


<h3 id="PtraceRegs.PC">func (*PtraceRegs) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux_amd64.go#L111">PC</a>
    <a href="#PtraceRegs.PC">¶</a></h3>
<pre>func (r *<a href="#PtraceRegs">PtraceRegs</a>) PC() <a href="/builtin/#uint64">uint64</a></pre>


<h3 id="PtraceRegs.SetPC">func (*PtraceRegs) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux_amd64.go#L113">SetPC</a>
    <a href="#PtraceRegs.SetPC">¶</a></h3>
<pre>func (r *<a href="#PtraceRegs">PtraceRegs</a>) SetPC(pc <a href="/builtin/#uint64">uint64</a>)</pre>


<h2 id="RawConn">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/net.go#L1">RawConn</a>
    <a href="#RawConn">¶</a></h2>
<pre>type RawConn interface {
    <span class="comment">// Control invokes f on the underlying connection&#39;s file</span>
    <span class="comment">// descriptor or handle.</span>
    <span class="comment">// The file descriptor fd is guaranteed to remain valid while</span>
    <span class="comment">// f executes but not after f returns.</span>
    Control(f func(fd <a href="/builtin/#uintptr">uintptr</a>)) <a href="/builtin/#error">error</a>

    <span class="comment">// Read invokes f on the underlying connection&#39;s file</span>
    <span class="comment">// descriptor or handle; f is expected to try to read from the</span>
    <span class="comment">// file descriptor.</span>
    <span class="comment">// If f returns true, Read returns. Otherwise Read blocks</span>
    <span class="comment">// waiting for the connection to be ready for reading and</span>
    <span class="comment">// tries again repeatedly.</span>
    <span class="comment">// The file descriptor is guaranteed to remain valid while f</span>
    <span class="comment">// executes but not after f returns.</span>
    Read(f func(fd <a href="/builtin/#uintptr">uintptr</a>) (done <a href="/builtin/#bool">bool</a>)) <a href="/builtin/#error">error</a>

    <span class="comment">// Write is like Read but for writing.</span>
    Write(f func(fd <a href="/builtin/#uintptr">uintptr</a>) (done <a href="/builtin/#bool">bool</a>)) <a href="/builtin/#error">error</a>
}</pre>

A RawConn is a raw network connection.

<h2 id="RawSockaddr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L184">RawSockaddr</a>
    <a href="#RawSockaddr">¶</a></h2>
<pre>type RawSockaddr struct {
<span id="RawSockaddr.Family"></span>    Family <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddr.Data"></span>    Data   [14]<a href="/builtin/#int8">int8</a>
}</pre>


<h2 id="RawSockaddrAny">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L189">RawSockaddrAny</a>
    <a href="#RawSockaddrAny">¶</a></h2>
<pre>type RawSockaddrAny struct {
<span id="RawSockaddrAny.Addr"></span>    Addr <a href="#RawSockaddr">RawSockaddr</a>
<span id="RawSockaddrAny.Pad"></span>    Pad  [96]<a href="/builtin/#int8">int8</a>
}</pre>


<h2 id="RawSockaddrInet4">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L147">RawSockaddrInet4</a>
    <a href="#RawSockaddrInet4">¶</a></h2>
<pre>type RawSockaddrInet4 struct {
<span id="RawSockaddrInet4.Family"></span>    Family <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddrInet4.Port"></span>    Port   <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddrInet4.Addr"></span>    Addr   [4]<a href="/builtin/#byte">byte</a> <span class="comment">/* in_addr */</span>
<span id="RawSockaddrInet4.Zero"></span>    Zero   [8]<a href="/builtin/#uint8">uint8</a>
}</pre>


<h2 id="RawSockaddrInet6">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L154">RawSockaddrInet6</a>
    <a href="#RawSockaddrInet6">¶</a></h2>
<pre>type RawSockaddrInet6 struct {
<span id="RawSockaddrInet6.Family"></span>    Family   <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddrInet6.Port"></span>    Port     <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddrInet6.Flowinfo"></span>    Flowinfo <a href="/builtin/#uint32">uint32</a>
<span id="RawSockaddrInet6.Addr"></span>    Addr     [16]<a href="/builtin/#byte">byte</a> <span class="comment">/* in6_addr */</span>
<span id="RawSockaddrInet6.Scope_id"></span>    Scope_id <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="RawSockaddrLinklayer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L167">RawSockaddrLinklayer</a>
    <a href="#RawSockaddrLinklayer">¶</a></h2>
<pre>type RawSockaddrLinklayer struct {
<span id="RawSockaddrLinklayer.Family"></span>    Family   <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddrLinklayer.Protocol"></span>    Protocol <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddrLinklayer.Ifindex"></span>    Ifindex  <a href="/builtin/#int32">int32</a>
<span id="RawSockaddrLinklayer.Hatype"></span>    Hatype   <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddrLinklayer.Pkttype"></span>    Pkttype  <a href="/builtin/#uint8">uint8</a>
<span id="RawSockaddrLinklayer.Halen"></span>    Halen    <a href="/builtin/#uint8">uint8</a>
<span id="RawSockaddrLinklayer.Addr"></span>    Addr     [8]<a href="/builtin/#uint8">uint8</a>
}</pre>


<h2 id="RawSockaddrNetlink">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L177">RawSockaddrNetlink</a>
    <a href="#RawSockaddrNetlink">¶</a></h2>
<pre>type RawSockaddrNetlink struct {
<span id="RawSockaddrNetlink.Family"></span>    Family <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddrNetlink.Pad"></span>    Pad    <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddrNetlink.Pid"></span>    Pid    <a href="/builtin/#uint32">uint32</a>
<span id="RawSockaddrNetlink.Groups"></span>    Groups <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="RawSockaddrUnix">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L162">RawSockaddrUnix</a>
    <a href="#RawSockaddrUnix">¶</a></h2>
<pre>type RawSockaddrUnix struct {
<span id="RawSockaddrUnix.Family"></span>    Family <a href="/builtin/#uint16">uint16</a>
<span id="RawSockaddrUnix.Path"></span>    Path   [108]<a href="/builtin/#int8">int8</a>
}</pre>


<h2 id="Rlimit">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L84">Rlimit</a>
    <a href="#Rlimit">¶</a></h2>
<pre>type Rlimit struct {
<span id="Rlimit.Cur"></span>    Cur <a href="/builtin/#uint64">uint64</a>
<span id="Rlimit.Max"></span>    Max <a href="/builtin/#uint64">uint64</a>
}</pre>


<h2 id="RtAttr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L437">RtAttr</a>
    <a href="#RtAttr">¶</a></h2>
<pre>type RtAttr struct {
<span id="RtAttr.Len"></span>    Len  <a href="/builtin/#uint16">uint16</a>
<span id="RtAttr.Type"></span>    Type <a href="/builtin/#uint16">uint16</a>
}</pre>


<h2 id="RtGenmsg">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L428">RtGenmsg</a>
    <a href="#RtGenmsg">¶</a></h2>
<pre>type RtGenmsg struct {
<span id="RtGenmsg.Family"></span>    Family <a href="/builtin/#uint8">uint8</a>
}</pre>


<h2 id="RtMsg">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L459">RtMsg</a>
    <a href="#RtMsg">¶</a></h2>
<pre>type RtMsg struct {
<span id="RtMsg.Family"></span>    Family   <a href="/builtin/#uint8">uint8</a>
<span id="RtMsg.Dst_len"></span>    Dst_len  <a href="/builtin/#uint8">uint8</a>
<span id="RtMsg.Src_len"></span>    Src_len  <a href="/builtin/#uint8">uint8</a>
<span id="RtMsg.Tos"></span>    Tos      <a href="/builtin/#uint8">uint8</a>
<span id="RtMsg.Table"></span>    Table    <a href="/builtin/#uint8">uint8</a>
<span id="RtMsg.Protocol"></span>    Protocol <a href="/builtin/#uint8">uint8</a>
<span id="RtMsg.Scope"></span>    Scope    <a href="/builtin/#uint8">uint8</a>
<span id="RtMsg.Type"></span>    Type     <a href="/builtin/#uint8">uint8</a>
<span id="RtMsg.Flags"></span>    Flags    <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="RtNexthop">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L471">RtNexthop</a>
    <a href="#RtNexthop">¶</a></h2>
<pre>type RtNexthop struct {
<span id="RtNexthop.Len"></span>    Len     <a href="/builtin/#uint16">uint16</a>
<span id="RtNexthop.Flags"></span>    Flags   <a href="/builtin/#uint8">uint8</a>
<span id="RtNexthop.Hops"></span>    Hops    <a href="/builtin/#uint8">uint8</a>
<span id="RtNexthop.Ifindex"></span>    Ifindex <a href="/builtin/#int32">int32</a>
}</pre>


<h2 id="Rusage">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L65">Rusage</a>
    <a href="#Rusage">¶</a></h2>
<pre>type Rusage struct {
<span id="Rusage.Utime"></span>    Utime    <a href="#Timeval">Timeval</a>
<span id="Rusage.Stime"></span>    Stime    <a href="#Timeval">Timeval</a>
<span id="Rusage.Maxrss"></span>    Maxrss   <a href="/builtin/#int64">int64</a>
<span id="Rusage.Ixrss"></span>    Ixrss    <a href="/builtin/#int64">int64</a>
<span id="Rusage.Idrss"></span>    Idrss    <a href="/builtin/#int64">int64</a>
<span id="Rusage.Isrss"></span>    Isrss    <a href="/builtin/#int64">int64</a>
<span id="Rusage.Minflt"></span>    Minflt   <a href="/builtin/#int64">int64</a>
<span id="Rusage.Majflt"></span>    Majflt   <a href="/builtin/#int64">int64</a>
<span id="Rusage.Nswap"></span>    Nswap    <a href="/builtin/#int64">int64</a>
<span id="Rusage.Inblock"></span>    Inblock  <a href="/builtin/#int64">int64</a>
<span id="Rusage.Oublock"></span>    Oublock  <a href="/builtin/#int64">int64</a>
<span id="Rusage.Msgsnd"></span>    Msgsnd   <a href="/builtin/#int64">int64</a>
<span id="Rusage.Msgrcv"></span>    Msgrcv   <a href="/builtin/#int64">int64</a>
<span id="Rusage.Nsignals"></span>    Nsignals <a href="/builtin/#int64">int64</a>
<span id="Rusage.Nvcsw"></span>    Nvcsw    <a href="/builtin/#int64">int64</a>
<span id="Rusage.Nivcsw"></span>    Nivcsw   <a href="/builtin/#int64">int64</a>
}</pre>


<h2 id="Signal">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L137">Signal</a>
    <a href="#Signal">¶</a></h2>
<pre>type Signal <a href="/builtin/#int">int</a></pre>

A Signal is a number describing a process signal. It implements the os.Signal
interface.

<h3 id="Signal.Signal">func (Signal) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L139">Signal</a>
    <a href="#Signal.Signal">¶</a></h3>
<pre>func (s <a href="#Signal">Signal</a>) Signal()</pre>


<h3 id="Signal.String">func (Signal) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L141">String</a>
    <a href="#Signal.String">¶</a></h3>
<pre>func (s <a href="#Signal">Signal</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="SockFilter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L483">SockFilter</a>
    <a href="#SockFilter">¶</a></h2>
<pre>type SockFilter struct {
<span id="SockFilter.Code"></span>    Code <a href="/builtin/#uint16">uint16</a>
<span id="SockFilter.Jt"></span>    Jt   <a href="/builtin/#uint8">uint8</a>
<span id="SockFilter.Jf"></span>    Jf   <a href="/builtin/#uint8">uint8</a>
<span id="SockFilter.K"></span>    K    <a href="/builtin/#uint32">uint32</a>
}</pre>


<h3 id="LsfJump">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/lsf_linux.go#L9">LsfJump</a>
    <a href="#LsfJump">¶</a></h3>
<pre>func LsfJump(code, k, jt, jf <a href="/builtin/#int">int</a>) *<a href="#SockFilter">SockFilter</a></pre>

Deprecated: Use golang.org/x/net/bpf instead.

<h3 id="LsfStmt">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/lsf_linux.go#L4">LsfStmt</a>
    <a href="#LsfStmt">¶</a></h3>
<pre>func LsfStmt(code, k <a href="/builtin/#int">int</a>) *<a href="#SockFilter">SockFilter</a></pre>

Deprecated: Use golang.org/x/net/bpf instead.

<h2 id="SockFprog">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L490">SockFprog</a>
    <a href="#SockFprog">¶</a></h2>
<pre>type SockFprog struct {
<span id="SockFprog.Len"></span>    Len       <a href="/builtin/#uint16">uint16</a>
<span id="SockFprog.Pad_cgo_0"></span>    Pad_cgo_0 [6]<a href="/builtin/#byte">byte</a>
<span id="SockFprog.Filter"></span>    Filter    *<a href="#SockFilter">SockFilter</a>
}</pre>


<h2 id="Sockaddr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L185">Sockaddr</a>
    <a href="#Sockaddr">¶</a></h2>
<pre>type Sockaddr interface {
    <span class="comment">// contains filtered or unexported methods</span>
}</pre>


<h3 id="Getpeername">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L223">Getpeername</a>
    <a href="#Getpeername">¶</a></h3>
<pre>func Getpeername(fd <a href="/builtin/#int">int</a>) (sa <a href="#Sockaddr">Sockaddr</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="Getsockname">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L459">Getsockname</a>
    <a href="#Getsockname">¶</a></h3>
<pre>func Getsockname(fd <a href="/builtin/#int">int</a>) (sa <a href="#Sockaddr">Sockaddr</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="SockaddrInet4">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L189">SockaddrInet4</a>
    <a href="#SockaddrInet4">¶</a></h2>
<pre>type SockaddrInet4 struct {
<span id="SockaddrInet4.Port"></span>    Port <a href="/builtin/#int">int</a>
<span id="SockaddrInet4.Addr"></span>    Addr [4]<a href="/builtin/#byte">byte</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>


<h2 id="SockaddrInet6">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L195">SockaddrInet6</a>
    <a href="#SockaddrInet6">¶</a></h2>
<pre>type SockaddrInet6 struct {
<span id="SockaddrInet6.Port"></span>    Port   <a href="/builtin/#int">int</a>
<span id="SockaddrInet6.ZoneId"></span>    ZoneId <a href="/builtin/#uint32">uint32</a>
<span id="SockaddrInet6.Addr"></span>    Addr   [16]<a href="/builtin/#byte">byte</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>


<h2 id="SockaddrLinklayer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L311">SockaddrLinklayer</a>
    <a href="#SockaddrLinklayer">¶</a></h2>
<pre>type SockaddrLinklayer struct {
<span id="SockaddrLinklayer.Protocol"></span>    Protocol <a href="/builtin/#uint16">uint16</a>
<span id="SockaddrLinklayer.Ifindex"></span>    Ifindex  <a href="/builtin/#int">int</a>
<span id="SockaddrLinklayer.Hatype"></span>    Hatype   <a href="/builtin/#uint16">uint16</a>
<span id="SockaddrLinklayer.Pkttype"></span>    Pkttype  <a href="/builtin/#uint8">uint8</a>
<span id="SockaddrLinklayer.Halen"></span>    Halen    <a href="/builtin/#uint8">uint8</a>
<span id="SockaddrLinklayer.Addr"></span>    Addr     [8]<a href="/builtin/#byte">byte</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>


<h2 id="SockaddrNetlink">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L337">SockaddrNetlink</a>
    <a href="#SockaddrNetlink">¶</a></h2>
<pre>type SockaddrNetlink struct {
<span id="SockaddrNetlink.Family"></span>    Family <a href="/builtin/#uint16">uint16</a>
<span id="SockaddrNetlink.Pad"></span>    Pad    <a href="/builtin/#uint16">uint16</a>
<span id="SockaddrNetlink.Pid"></span>    Pid    <a href="/builtin/#uint32">uint32</a>
<span id="SockaddrNetlink.Groups"></span>    Groups <a href="/builtin/#uint32">uint32</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>


<h2 id="SockaddrUnix">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_unix.go#L202">SockaddrUnix</a>
    <a href="#SockaddrUnix">¶</a></h2>
<pre>type SockaddrUnix struct {
<span id="SockaddrUnix.Name"></span>    Name <a href="/builtin/#string">string</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>


<h2 id="SocketControlMessage">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/sockcmsg_unix.go#L32">SocketControlMessage</a>
    <a href="#SocketControlMessage">¶</a></h2>
<pre>type SocketControlMessage struct {
<span id="SocketControlMessage.Header"></span>    Header <a href="#Cmsghdr">Cmsghdr</a>
<span id="SocketControlMessage.Data"></span>    Data   []<a href="/builtin/#byte">byte</a>
}</pre>

SocketControlMessage represents a socket control message.

<h2 id="Stat_t">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L91">Stat_t</a>
    <a href="#Stat_t">¶</a></h2>
<pre>type Stat_t struct {
<span id="Stat_t.Dev"></span>    Dev       <a href="/builtin/#uint64">uint64</a>
<span id="Stat_t.Ino"></span>    Ino       <a href="/builtin/#uint64">uint64</a>
<span id="Stat_t.Nlink"></span>    Nlink     <a href="/builtin/#uint64">uint64</a>
<span id="Stat_t.Mode"></span>    Mode      <a href="/builtin/#uint32">uint32</a>
<span id="Stat_t.Uid"></span>    Uid       <a href="/builtin/#uint32">uint32</a>
<span id="Stat_t.Gid"></span>    Gid       <a href="/builtin/#uint32">uint32</a>
<span id="Stat_t.X__pad0"></span>    X__pad0   <a href="/builtin/#int32">int32</a>
<span id="Stat_t.Rdev"></span>    Rdev      <a href="/builtin/#uint64">uint64</a>
<span id="Stat_t.Size"></span>    Size      <a href="/builtin/#int64">int64</a>
<span id="Stat_t.Blksize"></span>    Blksize   <a href="/builtin/#int64">int64</a>
<span id="Stat_t.Blocks"></span>    Blocks    <a href="/builtin/#int64">int64</a>
<span id="Stat_t.Atim"></span>    Atim      <a href="#Timespec">Timespec</a>
<span id="Stat_t.Mtim"></span>    Mtim      <a href="#Timespec">Timespec</a>
<span id="Stat_t.Ctim"></span>    Ctim      <a href="#Timespec">Timespec</a>
<span id="Stat_t.X__unused"></span>    X__unused [3]<a href="/builtin/#int64">int64</a>
}</pre>


<h2 id="Statfs_t">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L109">Statfs_t</a>
    <a href="#Statfs_t">¶</a></h2>
<pre>type Statfs_t struct {
<span id="Statfs_t.Type"></span>    Type    <a href="/builtin/#int64">int64</a>
<span id="Statfs_t.Bsize"></span>    Bsize   <a href="/builtin/#int64">int64</a>
<span id="Statfs_t.Blocks"></span>    Blocks  <a href="/builtin/#uint64">uint64</a>
<span id="Statfs_t.Bfree"></span>    Bfree   <a href="/builtin/#uint64">uint64</a>
<span id="Statfs_t.Bavail"></span>    Bavail  <a href="/builtin/#uint64">uint64</a>
<span id="Statfs_t.Files"></span>    Files   <a href="/builtin/#uint64">uint64</a>
<span id="Statfs_t.Ffree"></span>    Ffree   <a href="/builtin/#uint64">uint64</a>
<span id="Statfs_t.Fsid"></span>    Fsid    <a href="#Fsid">Fsid</a>
<span id="Statfs_t.Namelen"></span>    Namelen <a href="/builtin/#int64">int64</a>
<span id="Statfs_t.Frsize"></span>    Frsize  <a href="/builtin/#int64">int64</a>
<span id="Statfs_t.Flags"></span>    Flags   <a href="/builtin/#int64">int64</a>
<span id="Statfs_t.Spare"></span>    Spare   [4]<a href="/builtin/#int64">int64</a>
}</pre>


<h2 id="SysProcAttr">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_linux.go#L12">SysProcAttr</a>
    <a href="#SysProcAttr">¶</a></h2>
<pre>type SysProcAttr struct {
<span id="SysProcAttr.Chroot"></span>    Chroot       <a href="/builtin/#string">string</a>         <span class="comment">// Chroot.</span>
<span id="SysProcAttr.Credential"></span>    Credential   *<a href="#Credential">Credential</a>    <span class="comment">// Credential.</span>
<span id="SysProcAttr.Ptrace"></span>    Ptrace       <a href="/builtin/#bool">bool</a>           <span class="comment">// Enable tracing.</span>
<span id="SysProcAttr.Setsid"></span>    Setsid       <a href="/builtin/#bool">bool</a>           <span class="comment">// Create session.</span>
<span id="SysProcAttr.Setpgid"></span>    Setpgid      <a href="/builtin/#bool">bool</a>           <span class="comment">// Set process group ID to Pgid, or, if Pgid == 0, to new pid.</span>
<span id="SysProcAttr.Setctty"></span>    Setctty      <a href="/builtin/#bool">bool</a>           <span class="comment">// Set controlling terminal to fd Ctty (only meaningful if Setsid is set)</span>
<span id="SysProcAttr.Noctty"></span>    Noctty       <a href="/builtin/#bool">bool</a>           <span class="comment">// Detach fd 0 from controlling terminal</span>
<span id="SysProcAttr.Ctty"></span>    Ctty         <a href="/builtin/#int">int</a>            <span class="comment">// Controlling TTY fd</span>
<span id="SysProcAttr.Foreground"></span>    Foreground   <a href="/builtin/#bool">bool</a>           <span class="comment">// Place child&#39;s process group in foreground. (Implies Setpgid. Uses Ctty as fd of controlling TTY)</span>
<span id="SysProcAttr.Pgid"></span>    Pgid         <a href="/builtin/#int">int</a>            <span class="comment">// Child&#39;s process group ID if Setpgid.</span>
<span id="SysProcAttr.Pdeathsig"></span>    Pdeathsig    <a href="#Signal">Signal</a>         <span class="comment">// Signal that the process will get when its parent dies (Linux only)</span>
<span id="SysProcAttr.Cloneflags"></span>    Cloneflags   <a href="/builtin/#uintptr">uintptr</a>        <span class="comment">// Flags for clone calls (Linux only)</span>
<span id="SysProcAttr.Unshareflags"></span>    Unshareflags <a href="/builtin/#uintptr">uintptr</a>        <span class="comment">// Flags for unshare calls (Linux only)</span>
<span id="SysProcAttr.UidMappings"></span>    UidMappings  []<a href="#SysProcIDMap">SysProcIDMap</a> <span class="comment">// User ID mappings for user namespaces.</span>
<span id="SysProcAttr.GidMappings"></span>    GidMappings  []<a href="#SysProcIDMap">SysProcIDMap</a> <span class="comment">// Group ID mappings for user namespaces.</span>
<span id="SysProcAttr.GidMappingsEnableSetgroups"></span>    <span class="comment">// GidMappingsEnableSetgroups enabling setgroups syscall.</span>
    <span class="comment">// If false, then setgroups syscall will be disabled for the child process.</span>
    <span class="comment">// This parameter is no-op if GidMappings == nil. Otherwise for unprivileged</span>
    <span class="comment">// users this should be set to false for mappings work.</span>
    GidMappingsEnableSetgroups <a href="/builtin/#bool">bool</a>
<span id="SysProcAttr.AmbientCaps"></span>    AmbientCaps                []<a href="/builtin/#uintptr">uintptr</a> <span class="comment">// Ambient capabilities (Linux only)</span>
}</pre>


<h2 id="SysProcIDMap">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/exec_linux.go#L6">SysProcIDMap</a>
    <a href="#SysProcIDMap">¶</a></h2>
<pre>type SysProcIDMap struct {
<span id="SysProcIDMap.ContainerID"></span>    ContainerID <a href="/builtin/#int">int</a> <span class="comment">// Container ID.</span>
<span id="SysProcIDMap.HostID"></span>    HostID      <a href="/builtin/#int">int</a> <span class="comment">// Host ID.</span>
<span id="SysProcIDMap.Size"></span>    Size        <a href="/builtin/#int">int</a> <span class="comment">// Size.</span>
}</pre>

SysProcIDMap holds Container ID to Host ID mappings used for User Namespaces in
Linux. See user_namespaces(7).

<h2 id="Sysinfo_t">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L540">Sysinfo_t</a>
    <a href="#Sysinfo_t">¶</a></h2>
<pre>type Sysinfo_t struct {
<span id="Sysinfo_t.Uptime"></span>    Uptime    <a href="/builtin/#int64">int64</a>
<span id="Sysinfo_t.Loads"></span>    Loads     [3]<a href="/builtin/#uint64">uint64</a>
<span id="Sysinfo_t.Totalram"></span>    Totalram  <a href="/builtin/#uint64">uint64</a>
<span id="Sysinfo_t.Freeram"></span>    Freeram   <a href="/builtin/#uint64">uint64</a>
<span id="Sysinfo_t.Sharedram"></span>    Sharedram <a href="/builtin/#uint64">uint64</a>
<span id="Sysinfo_t.Bufferram"></span>    Bufferram <a href="/builtin/#uint64">uint64</a>
<span id="Sysinfo_t.Totalswap"></span>    Totalswap <a href="/builtin/#uint64">uint64</a>
<span id="Sysinfo_t.Freeswap"></span>    Freeswap  <a href="/builtin/#uint64">uint64</a>
<span id="Sysinfo_t.Procs"></span>    Procs     <a href="/builtin/#uint16">uint16</a>
<span id="Sysinfo_t.Pad"></span>    Pad       <a href="/builtin/#uint16">uint16</a>
<span id="Sysinfo_t.Pad_cgo_0"></span>    Pad_cgo_0 [4]<a href="/builtin/#byte">byte</a>
<span id="Sysinfo_t.Totalhigh"></span>    Totalhigh <a href="/builtin/#uint64">uint64</a>
<span id="Sysinfo_t.Freehigh"></span>    Freehigh  <a href="/builtin/#uint64">uint64</a>
<span id="Sysinfo_t.Unit"></span>    Unit      <a href="/builtin/#uint32">uint32</a>
<span id="Sysinfo_t.X_f"></span>    X_f       [0]<a href="/builtin/#byte">byte</a>
<span id="Sysinfo_t.Pad_cgo_1"></span>    Pad_cgo_1 [4]<a href="/builtin/#byte">byte</a>
}</pre>


<h2 id="TCPInfo">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L266">TCPInfo</a>
    <a href="#TCPInfo">¶</a></h2>
<pre>type TCPInfo struct {
<span id="TCPInfo.State"></span>    State          <a href="/builtin/#uint8">uint8</a>
<span id="TCPInfo.Ca_state"></span>    Ca_state       <a href="/builtin/#uint8">uint8</a>
<span id="TCPInfo.Retransmits"></span>    Retransmits    <a href="/builtin/#uint8">uint8</a>
<span id="TCPInfo.Probes"></span>    Probes         <a href="/builtin/#uint8">uint8</a>
<span id="TCPInfo.Backoff"></span>    Backoff        <a href="/builtin/#uint8">uint8</a>
<span id="TCPInfo.Options"></span>    Options        <a href="/builtin/#uint8">uint8</a>
<span id="TCPInfo.Pad_cgo_0"></span>    Pad_cgo_0      [2]<a href="/builtin/#byte">byte</a>
<span id="TCPInfo.Rto"></span>    Rto            <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Ato"></span>    Ato            <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Snd_mss"></span>    Snd_mss        <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Rcv_mss"></span>    Rcv_mss        <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Unacked"></span>    Unacked        <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Sacked"></span>    Sacked         <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Lost"></span>    Lost           <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Retrans"></span>    Retrans        <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Fackets"></span>    Fackets        <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Last_data_sent"></span>    Last_data_sent <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Last_ack_sent"></span>    Last_ack_sent  <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Last_data_recv"></span>    Last_data_recv <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Last_ack_recv"></span>    Last_ack_recv  <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Pmtu"></span>    Pmtu           <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Rcv_ssthresh"></span>    Rcv_ssthresh   <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Rtt"></span>    Rtt            <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Rttvar"></span>    Rttvar         <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Snd_ssthresh"></span>    Snd_ssthresh   <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Snd_cwnd"></span>    Snd_cwnd       <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Advmss"></span>    Advmss         <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Reordering"></span>    Reordering     <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Rcv_rtt"></span>    Rcv_rtt        <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Rcv_space"></span>    Rcv_space      <a href="/builtin/#uint32">uint32</a>
<span id="TCPInfo.Total_retrans"></span>    Total_retrans  <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="Termios">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L589">Termios</a>
    <a href="#Termios">¶</a></h2>
<pre>type Termios struct {
<span id="Termios.Iflag"></span>    Iflag     <a href="/builtin/#uint32">uint32</a>
<span id="Termios.Oflag"></span>    Oflag     <a href="/builtin/#uint32">uint32</a>
<span id="Termios.Cflag"></span>    Cflag     <a href="/builtin/#uint32">uint32</a>
<span id="Termios.Lflag"></span>    Lflag     <a href="/builtin/#uint32">uint32</a>
<span id="Termios.Line"></span>    Line      <a href="/builtin/#uint8">uint8</a>
<span id="Termios.Cc"></span>    Cc        [32]<a href="/builtin/#uint8">uint8</a>
<span id="Termios.Pad_cgo_0"></span>    Pad_cgo_0 [3]<a href="/builtin/#byte">byte</a>
<span id="Termios.Ispeed"></span>    Ispeed    <a href="/builtin/#uint32">uint32</a>
<span id="Termios.Ospeed"></span>    Ospeed    <a href="/builtin/#uint32">uint32</a>
}</pre>


<h2 id="Time_t">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L51">Time_t</a>
    <a href="#Time_t">¶</a></h2>
<pre>type Time_t <a href="/builtin/#int64">int64</a></pre>


<h3 id="Time">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux_amd64.go#L65">Time</a>
    <a href="#Time">¶</a></h3>
<pre>func Time(t *<a href="#Time_t">Time_t</a>) (tt <a href="#Time_t">Time_t</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Timespec">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L14">Timespec</a>
    <a href="#Timespec">¶</a></h2>
<pre>type Timespec struct {
<span id="Timespec.Sec"></span>    Sec  <a href="/builtin/#int64">int64</a>
<span id="Timespec.Nsec"></span>    Nsec <a href="/builtin/#int64">int64</a>
}</pre>


<h3 id="NsecToTimespec">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/timestruct.go#L5">NsecToTimespec</a>
    <a href="#NsecToTimespec">¶</a></h3>
<pre>func NsecToTimespec(nsec <a href="/builtin/#int64">int64</a>) <a href="#Timespec">Timespec</a></pre>

NsecToTimespec takes a number of nanoseconds since the Unix epoch and returns
the corresponding Timespec value.

<h3 id="Timespec.Nano">func (*Timespec) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go#L86">Nano</a>
    <a href="#Timespec.Nano">¶</a></h3>
<pre>func (ts *<a href="#Timespec">Timespec</a>) Nano() <a href="/builtin/#int64">int64</a></pre>

Nano returns ts as the number of nanoseconds elapsed since the Unix epoch.

<h3 id="Timespec.Unix">func (*Timespec) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go#L75">Unix</a>
    <a href="#Timespec.Unix">¶</a></h3>
<pre>func (ts *<a href="#Timespec">Timespec</a>) Unix() (sec <a href="/builtin/#int64">int64</a>, nsec <a href="/builtin/#int64">int64</a>)</pre>

Unix returns ts as the number of seconds and nanoseconds elapsed since the Unix
epoch.

<h2 id="Timeval">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L19">Timeval</a>
    <a href="#Timeval">¶</a></h2>
<pre>type Timeval struct {
<span id="Timeval.Sec"></span>    Sec  <a href="/builtin/#int64">int64</a>
<span id="Timeval.Usec"></span>    Usec <a href="/builtin/#int64">int64</a>
}</pre>


<h3 id="NsecToTimeval">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/timestruct.go#L21">NsecToTimeval</a>
    <a href="#NsecToTimeval">¶</a></h3>
<pre>func NsecToTimeval(nsec <a href="/builtin/#int64">int64</a>) <a href="#Timeval">Timeval</a></pre>

NsecToTimeval takes a number of nanoseconds since the Unix epoch and returns the
corresponding Timeval value.

<h3 id="Timeval.Nano">func (*Timeval) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go#L91">Nano</a>
    <a href="#Timeval.Nano">¶</a></h3>
<pre>func (tv *<a href="#Timeval">Timeval</a>) Nano() <a href="/builtin/#int64">int64</a></pre>

Nano returns tv as the number of nanoseconds elapsed since the Unix epoch.

<h3 id="Timeval.Unix">func (*Timeval) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall.go#L81">Unix</a>
    <a href="#Timeval.Unix">¶</a></h3>
<pre>func (tv *<a href="#Timeval">Timeval</a>) Unix() (sec <a href="/builtin/#int64">int64</a>, nsec <a href="/builtin/#int64">int64</a>)</pre>

Unix returns tv as the number of seconds and nanoseconds elapsed since the Unix
epoch.

<h2 id="Timex">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L24">Timex</a>
    <a href="#Timex">¶</a></h2>
<pre>type Timex struct {
<span id="Timex.Modes"></span>    Modes     <a href="/builtin/#uint32">uint32</a>
<span id="Timex.Pad_cgo_0"></span>    Pad_cgo_0 [4]<a href="/builtin/#byte">byte</a>
<span id="Timex.Offset"></span>    Offset    <a href="/builtin/#int64">int64</a>
<span id="Timex.Freq"></span>    Freq      <a href="/builtin/#int64">int64</a>
<span id="Timex.Maxerror"></span>    Maxerror  <a href="/builtin/#int64">int64</a>
<span id="Timex.Esterror"></span>    Esterror  <a href="/builtin/#int64">int64</a>
<span id="Timex.Status"></span>    Status    <a href="/builtin/#int32">int32</a>
<span id="Timex.Pad_cgo_1"></span>    Pad_cgo_1 [4]<a href="/builtin/#byte">byte</a>
<span id="Timex.Constant"></span>    Constant  <a href="/builtin/#int64">int64</a>
<span id="Timex.Precision"></span>    Precision <a href="/builtin/#int64">int64</a>
<span id="Timex.Tolerance"></span>    Tolerance <a href="/builtin/#int64">int64</a>
<span id="Timex.Time"></span>    Time      <a href="#Timeval">Timeval</a>
<span id="Timex.Tick"></span>    Tick      <a href="/builtin/#int64">int64</a>
<span id="Timex.Ppsfreq"></span>    Ppsfreq   <a href="/builtin/#int64">int64</a>
<span id="Timex.Jitter"></span>    Jitter    <a href="/builtin/#int64">int64</a>
<span id="Timex.Shift"></span>    Shift     <a href="/builtin/#int32">int32</a>
<span id="Timex.Pad_cgo_2"></span>    Pad_cgo_2 [4]<a href="/builtin/#byte">byte</a>
<span id="Timex.Stabil"></span>    Stabil    <a href="/builtin/#int64">int64</a>
<span id="Timex.Jitcnt"></span>    Jitcnt    <a href="/builtin/#int64">int64</a>
<span id="Timex.Calcnt"></span>    Calcnt    <a href="/builtin/#int64">int64</a>
<span id="Timex.Errcnt"></span>    Errcnt    <a href="/builtin/#int64">int64</a>
<span id="Timex.Stbcnt"></span>    Stbcnt    <a href="/builtin/#int64">int64</a>
<span id="Timex.Tai"></span>    Tai       <a href="/builtin/#int32">int32</a>
<span id="Timex.Pad_cgo_3"></span>    Pad_cgo_3 [44]<a href="/builtin/#byte">byte</a>
}</pre>


<h2 id="Tms">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L53">Tms</a>
    <a href="#Tms">¶</a></h2>
<pre>type Tms struct {
<span id="Tms.Utime"></span>    Utime  <a href="/builtin/#int64">int64</a>
<span id="Tms.Stime"></span>    Stime  <a href="/builtin/#int64">int64</a>
<span id="Tms.Cutime"></span>    Cutime <a href="/builtin/#int64">int64</a>
<span id="Tms.Cstime"></span>    Cstime <a href="/builtin/#int64">int64</a>
}</pre>


<h2 id="Ucred">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L260">Ucred</a>
    <a href="#Ucred">¶</a></h2>
<pre>type Ucred struct {
<span id="Ucred.Pid"></span>    Pid <a href="/builtin/#int32">int32</a>
<span id="Ucred.Uid"></span>    Uid <a href="/builtin/#uint32">uint32</a>
<span id="Ucred.Gid"></span>    Gid <a href="/builtin/#uint32">uint32</a>
}</pre>


<h3 id="GetsockoptUcred">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L509">GetsockoptUcred</a>
    <a href="#GetsockoptUcred">¶</a></h3>
<pre>func GetsockoptUcred(fd, level, opt <a href="/builtin/#int">int</a>) (*<a href="#Ucred">Ucred</a>, <a href="/builtin/#error">error</a>)</pre>


<h3 id="ParseUnixCredentials">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/sockcmsg_linux.go#L17">ParseUnixCredentials</a>
    <a href="#ParseUnixCredentials">¶</a></h3>
<pre>func ParseUnixCredentials(m *<a href="#SocketControlMessage">SocketControlMessage</a>) (*<a href="#Ucred">Ucred</a>, <a href="/builtin/#error">error</a>)</pre>

ParseUnixCredentials decodes a socket control message that contains credentials
in a Ucred structure. To receive such a message, the SO_PASSCRED option must be
enabled on the socket.

<h2 id="Ustat_t">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L568">Ustat_t</a>
    <a href="#Ustat_t">¶</a></h2>
<pre>type Ustat_t struct {
<span id="Ustat_t.Tfree"></span>    Tfree     <a href="/builtin/#int32">int32</a>
<span id="Ustat_t.Pad_cgo_0"></span>    Pad_cgo_0 [4]<a href="/builtin/#byte">byte</a>
<span id="Ustat_t.Tinode"></span>    Tinode    <a href="/builtin/#uint64">uint64</a>
<span id="Ustat_t.Fname"></span>    Fname     [6]<a href="/builtin/#int8">int8</a>
<span id="Ustat_t.Fpack"></span>    Fpack     [6]<a href="/builtin/#int8">int8</a>
<span id="Ustat_t.Pad_cgo_1"></span>    Pad_cgo_1 [4]<a href="/builtin/#byte">byte</a>
}</pre>


<h2 id="Utimbuf">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L60">Utimbuf</a>
    <a href="#Utimbuf">¶</a></h2>
<pre>type Utimbuf struct {
<span id="Utimbuf.Actime"></span>    Actime  <a href="/builtin/#int64">int64</a>
<span id="Utimbuf.Modtime"></span>    Modtime <a href="/builtin/#int64">int64</a>
}</pre>


<h2 id="Utsname">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/ztypes_linux_amd64.go#L559">Utsname</a>
    <a href="#Utsname">¶</a></h2>
<pre>type Utsname struct {
<span id="Utsname.Sysname"></span>    Sysname    [65]<a href="/builtin/#int8">int8</a>
<span id="Utsname.Nodename"></span>    Nodename   [65]<a href="/builtin/#int8">int8</a>
<span id="Utsname.Release"></span>    Release    [65]<a href="/builtin/#int8">int8</a>
<span id="Utsname.Version"></span>    Version    [65]<a href="/builtin/#int8">int8</a>
<span id="Utsname.Machine"></span>    Machine    [65]<a href="/builtin/#int8">int8</a>
<span id="Utsname.Domainname"></span>    Domainname [65]<a href="/builtin/#int8">int8</a>
}</pre>


<h2 id="WaitStatus">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L183">WaitStatus</a>
    <a href="#WaitStatus">¶</a></h2>
<pre>type WaitStatus <a href="/builtin/#uint32">uint32</a></pre>


<h3 id="WaitStatus.Continued">func (WaitStatus) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L208">Continued</a>
    <a href="#WaitStatus.Continued">¶</a></h3>
<pre>func (w <a href="#WaitStatus">WaitStatus</a>) Continued() <a href="/builtin/#bool">bool</a></pre>


<h3 id="WaitStatus.CoreDump">func (WaitStatus) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L210">CoreDump</a>
    <a href="#WaitStatus.CoreDump">¶</a></h3>
<pre>func (w <a href="#WaitStatus">WaitStatus</a>) CoreDump() <a href="/builtin/#bool">bool</a></pre>


<h3 id="WaitStatus.ExitStatus">func (WaitStatus) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L212">ExitStatus</a>
    <a href="#WaitStatus.ExitStatus">¶</a></h3>
<pre>func (w <a href="#WaitStatus">WaitStatus</a>) ExitStatus() <a href="/builtin/#int">int</a></pre>


<h3 id="WaitStatus.Exited">func (WaitStatus) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L202">Exited</a>
    <a href="#WaitStatus.Exited">¶</a></h3>
<pre>func (w <a href="#WaitStatus">WaitStatus</a>) Exited() <a href="/builtin/#bool">bool</a></pre>


<h3 id="WaitStatus.Signal">func (WaitStatus) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L219">Signal</a>
    <a href="#WaitStatus.Signal">¶</a></h3>
<pre>func (w <a href="#WaitStatus">WaitStatus</a>) Signal() <a href="#Signal">Signal</a></pre>


<h3 id="WaitStatus.Signaled">func (WaitStatus) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L204">Signaled</a>
    <a href="#WaitStatus.Signaled">¶</a></h3>
<pre>func (w <a href="#WaitStatus">WaitStatus</a>) Signaled() <a href="/builtin/#bool">bool</a></pre>


<h3 id="WaitStatus.StopSignal">func (WaitStatus) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L226">StopSignal</a>
    <a href="#WaitStatus.StopSignal">¶</a></h3>
<pre>func (w <a href="#WaitStatus">WaitStatus</a>) StopSignal() <a href="#Signal">Signal</a></pre>


<h3 id="WaitStatus.Stopped">func (WaitStatus) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L206">Stopped</a>
    <a href="#WaitStatus.Stopped">¶</a></h3>
<pre>func (w <a href="#WaitStatus">WaitStatus</a>) Stopped() <a href="/builtin/#bool">bool</a></pre>


<h3 id="WaitStatus.TrapCause">func (WaitStatus) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/syscall/syscall_linux.go#L233">TrapCause</a>
    <a href="#WaitStatus.TrapCause">¶</a></h3>
<pre>func (w <a href="#WaitStatus">WaitStatus</a>) TrapCause() <a href="/builtin/#int">int</a></pre>



